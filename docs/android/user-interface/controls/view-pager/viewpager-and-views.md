---
title: ViewPager z widokami
description: ViewPager jest Menedżer układu, umożliwiające wdrożenie gestural nawigacji. Gestural nawigacji umożliwia użytkownikowi Przejdź lewy i prawy kroków strony danych. Ten przewodnik opisuje sposób nadawania swipeable interfejsu użytkownika z ViewPager i PagerTabStrip, korzystanie z widoków jako strony danych (kolejnych przewodniku opisano sposób użycia fragmenty dla stron).
ms.prod: xamarin
ms.assetid: 42E5379F-B0F4-4B87-A314-BF3DE405B0C8
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 39251f7cf6bc287b76b7921278853158bdb14d66
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="viewpager-with-views"></a>ViewPager z widokami

_ViewPager jest Menedżer układu, umożliwiające wdrożenie gestural nawigacji. Gestural nawigacji umożliwia użytkownikowi Przejdź lewy i prawy kroków strony danych. Ten przewodnik opisuje sposób nadawania swipeable interfejsu użytkownika z ViewPager i PagerTabStrip, korzystanie z widoków jako strony danych (kolejnych przewodniku opisano sposób użycia fragmenty dla stron)._

 
## <a name="overview"></a>Omówienie

Ten przewodnik jest wskazówki, który udostępnia pokaz krok po kroku dotyczące używania `ViewPager` do zaimplementowania Galeria obrazów drzew liściaste i wiecznie zielone. W tej aplikacji, użytkownik swipes lewy i prawy za pomocą "drzewa wykazu" Aby wyświetlić obrazy drzewa. Na początku każdej stronie katalogu, nazwa drzewa ma na liście`PagerTabStrip`, i obraz drzewa jest wyświetlany w `ImageView`. Karta jest używana do interfejsu `ViewPager` do właściwego modelu danych. Adapter pochodną implementuje tę aplikację `PagerAdapter`. 

Mimo że `ViewPager`-aplikacji są często stosowane z `Fragment`s, istnieją przypadki użycia stosunkowo proste gdzie dodatkowe złożoność `Fragment`s nie jest konieczne. Na przykład aplikacja galerii podstawowy obraz przedstawione w tym przewodniku nie wymaga stosowania `Fragment`s. Ponieważ zawartość jest statyczna i swipes tylko użytkownika i z powrotem między różnymi obrazami wdrożenia można przechowywać prostsze przy użyciu standardowych widoków dla systemu Android i układów. 



## <a name="start-an-app-project"></a>Projekt aplikacji

Utwórz nowy projekt dla systemu Android o nazwie **TreePager** (zobacz [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md) Aby uzyskać więcej informacji o tworzeniu nowych projektów dla systemu Android). Następnie uruchom Menedżera pakietów NuGet. (Aby uzyskać więcej informacji na temat instalowania pakietów NuGet, zobacz [wskazówki: w tym NuGet w projekcie](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)). Znajdź i zainstaluj **biblioteki obsługi systemu Android w wersji 4**: 

[![Zrzut ekranu pomocy technicznej Nuget w wersji 4 wybrane w Menedżerze pakietów NuGet](viewpager-and-views-images/01-install-support-lib-sml.png)](viewpager-and-views-images/01-install-support-lib.png#lightbox)

Spowoduje to również zainstalowanie reaquired wszelkie dodatkowe pakiety przez **biblioteki obsługi systemu Android w wersji 4**.



## <a name="add-an-example-data-source"></a>Dodawanie źródła danych przykład

W tym przykładzie drzewa źródła danych katalogu (reprezentowane przez `TreeCatalog` klasy) dostarcza `ViewPager` z zawartością elementu. 
`TreeCatalog` zawiera kolekcję gotowych obrazów drzewa i tytułów drzewa karta sieciowa będzie używać do tworzenia `View`s. `TreeCatalog` Konstruktor wymaga argumentów:

```csharp
TreeCatalog treeCatalog = new TreeCatalog();
```

Kolekcja obrazów w `TreeCatalog` jest zorganizowana w taki sposób, że każdego obrazu są dostępne dla indeksatora. Na przykład następujący wiersz kodu pobiera identyfikator zasobu obrazu trzeci obrazu w kolekcji: 

```csharp
int imageId = treeCatalog[2].imageId;
```

Ponieważ szczegóły implementacji `TreeCatalog` nie są istotne dla zrozumienia `ViewPager`, `TreeCatalog` kodu nie ma na liście. Aby kod źródłowy `TreeCatalog` znajduje się w temacie [TreeCatalog.cs](https://github.com/xamarin/monodroid-samples/blob/master/UserInterface/TreePager/TreePager/TreeCatalog.cs). Pobierz ten plik źródłowy (lub skopiuj i Wklej kod do nowego **TreeCatalog.cs** plików) i dodaj go do projektu. Ponadto Pobierz i Rozpakuj [pliki obrazów](https://github.com/xamarin/monodroid-samples/blob/master/UserInterface/TreePager/Resources/tree-images.zip?raw=true) do Twojej **obiektów drawable/zasobów** folderu i dołączać je do projektu. 



## <a name="create-a-viewpager-layout"></a>Utwórz układ ViewPager

Otwórz **Resources/layout/Main.axml** i zastąp jego zawartość XML następujące:

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.v4.view.ViewPager
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/viewpager"
    android:layout_width="match_parent"
    android:layout_height="match_parent" >

</android.support.v4.view.ViewPager>

```csharp
This XML defines a `ViewPager` that occupies the entire screen. Note that
you must use the fully-qualified name **android.support.v4.view.ViewPager**
because `ViewPager` is packaged in a support library. `ViewPager` is
available only from 
[Android Support Library v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/);
it is not available in the Android SDK. 


## Set up ViewPager

Edit **MainActivity.cs** and add the following `using` statement:

```csharp
using Android.Support.V4.View;
```

Zastąp `OnCreate` metodę z następującym kodem:

```csharp
protected override void OnCreate(Bundle bundle)
{
    base.OnCreate(bundle);
    SetContentView(Resource.Layout.Main);
    ViewPager viewPager = FindViewById<ViewPager>(Resource.Id.viewpager);
    TreeCatalog treeCatalog = new TreeCatalog();
}
```

Ten kod wykonuje następujące czynności:

1.  Ustawia widok z **Main.axml** zasobów układu.

2.  Pobiera odwołanie do `ViewPager` z układu.

3.  Tworzy nową `TreeCatalog` jako źródła danych.

Podczas kompilacji, a następnie uruchomić ten kod, powinien zostać wyświetlony ekran podobny Poniższy zrzut ekranu: 

[![Zrzut ekranu aplikacji wyświetlanie ViewPager pusty](viewpager-and-views-images/02-initial-screen-sml.png)](viewpager-and-views-images/02-initial-screen.png#lightbox)

W tym momencie `ViewPager` jest pusta, ponieważ brak karty do uzyskiwania dostępu do zawartości w **TreeCatalog**. W następnej sekcji **PagerAdapter** służy do połączenia `ViewPager` do **TreeCatalog**. 


## <a name="create-the-adapter"></a>Utwórz kartę

`ViewPager` używa obiektu kontrolera karty, która znajduje się między `ViewPager` i źródła danych (patrz ilustracja w [karty](~/android/user-interface/controls/view-pager/index.md#adapter)). Aby uzyskać dostęp do tych danych `ViewPager` wymaga podania adapter niestandardowy pochodną `PagerAdapter`. Ta karta umożliwia wypełnienie każdego `ViewPager` strony z zawartością ze źródła danych. Ponieważ to źródło danych jest specyficzny dla aplikacji, adapter niestandardowy jest kod, który zrozumienie sposobu uzyskiwania dostępu do danych. Jako swipes użytkownika za pośrednictwem strony `ViewPager`, karta wyodrębnia dane ze źródła danych i ładuje go do strony dla `ViewPager` do wyświetlenia. 

Po zaimplementowaniu `PagerAdapter`, konieczne jest przesłonięcie następujących czynności:

-   **InstantiateItem** &ndash; tworzy strony (`View`) dla określonej pozycji i dodaje go do `ViewPager`jego Kolekcja widoków. 

-   **DestroyItem** &ndash; usuwa stronę określonej pozycji.

-   **Liczba** &ndash; właściwości tylko do odczytu, która zwraca liczbę dostępnych widoków (strony). 

-   **IsViewFromObject** &ndash; Określa, czy strona jest skojarzony z określonym obiektem klucza. (Ten obiekt jest tworzony przez `InstantiateItem` metody.) W tym przykładzie jest obiektu klucza `TreeCatalog` obiektu danych.

Dodaj nowy plik o nazwie **TreePagerAdapter.cs** i zastąp jego zawartość następującym kodem: 

```csharp
using System;
using Android.App;
using Android.Runtime;
using Android.Content;
using Android.Views;
using Android.Widget;
using Android.Support.V4.View;
using Java.Lang;

namespace TreePager
{
    class TreePagerAdapter : PagerAdapter
    {
        public override int Count
        {
            get { throw new NotImplementedException(); }
        }

        public override bool IsViewFromObject(View view, Java.Lang.Object obj)
        {
            throw new NotImplementedException();
        }

        public override Java.Lang.Object InstantiateItem (View container, int position)
        {
            throw new NotImplementedException();
        }

        public override void DestroyItem(View container, int position, Java.Lang.Object view)
        {
            throw new NotImplementedException();
        }
    }
}
```

Ten kod zastępcze limit podstawowych `PagerAdapter` implementacji. W poniższych sekcjach każda z tych metod zostanie zastąpiony kodem pracy. 



### <a name="implement-the-constructor"></a>Implementuje konstruktora

Gdy aplikacja tworzy `TreePagerAdapter`, podaje kontekst ( `MainActivity`) i wystąpień `TreeCatalog`. Dodaj następujące zmienne Członkowskie i konstruktora do góry `TreePagerAdapter` klasy w **TreePagerAdapter.cs**: 

```csharp
Context context;
TreeCatalog treeCatalog;

public TreePagerAdapter (Context context, TreeCatalog treeCatalog)
{
    this.context = context;
    this.treeCatalog = treeCatalog;
}
```

Ten konstruktor służy do przechowywania kontekstu i `TreeCatalog` wystąpienie `TreePagerAdapter` będzie używana. 



### <a name="implement-count"></a>Liczba wdrożenie

`Count` Implementacji jest stosunkowo prosta: zwraca liczbę drzew w drzewie katalogu. Zastąp `Count` następującym kodem:

```csharp
public override int Count
{
    get { return treeCatalog.NumTrees; }
}
```

`NumTrees` Właściwość `TreeCatalog` zwraca liczbę drzewa (liczba stron) w zestawie danych.



### <a name="implement-instantiateitem"></a>Implementowanie InstantiateItem

`InstantiateItem` Metoda tworzy stronę do określonej pozycji. Należy również dodać nowy widok, aby `ViewPager`firmy Wyświetl kolekcję. Aby było to możliwe, `ViewPager` przekazuje się jako parametr kontenera. 

Zastąp `InstantiateItem` metodę z następującym kodem:

```csharp
public override Java.Lang.Object InstantiateItem (View container, int position)
{
    var imageView = new ImageView (context);
    imageView.SetImageResource (treeCatalog[position].imageId);
    var viewPager = container.JavaCast<ViewPager>();
    viewPager.AddView (imageView);
    return imageView;
}
```

Ten kod wykonuje następujące czynności:

1.  Tworzy nową `ImageView` do wyświetlania obrazu drzewa w określonej pozycji. Aplikacji `MainActivity` kontekstu, który zostanie przekazany do `ImageView` konstruktora.

2.  Ustawia `ImageView` zasobów do `TreeCatalog` obrazu identyfikator zasobu w określonej pozycji.

3.  Rzutuje przekazany kontenera `View` do `ViewPager` odwołania.
    Należy pamiętać, że należy użyć `JavaCast<ViewPager>()` prawidłowo przeprowadzenie tego rzutowania (jest to niezbędne, aby Android wykonuje konwersji typu zaznaczone środowiska uruchomieniowego).

4.  Dodaje skonkretyzowanym `ImageView` do `ViewPager` i zwraca `ImageView` do obiektu wywołującego.

Gdy `ViewPager` Wyświetla obraz w `position`, jego wartość to `ImageView`. Początkowo `InstantiateItem` jest wywoływana dwukrotnie, aby wypełnić najpierw dwie strony z widoków. Jako użytkownik przewija widok, jest on nazywany ponownie do obsługi widoków tuż za i wcześniejsze aktualnie wyświetlanego elementu. 



### <a name="implement-destroyitem"></a>Implementowanie DestroyItem

`DestroyItem` Metoda usuwa stronę podanego położenia. W aplikacji, w którym można zmienić widok na poszczególnych pozycji `ViewPager` musi mieć jakiś sposób usuwania starych widok na tej pozycji przed zastąpieniem nowy widok. W `TreeCatalog` przykład nie zmienia widoku w każdym miejscu, dlatego widok usunięta przez `DestroyItem` po prostu będzie ponownie dodane podczas `InstantiateItem` jest wywoływana dla tej pozycji. (Aby uzyskać lepszą wydajność, jeden zaimplementować puli do odtworzenia `View`s, który będzie ponownie wyświetlanych w tym samym miejscu.) 

Zastąp `DestroyItem` metodę z następującym kodem: 

```csharp
public override void DestroyItem(View container, int position, Java.Lang.Object view)
{
    var viewPager = container.JavaCast<ViewPager>();
    viewPager.RemoveView(view as View);
}
```

Ten kod wykonuje następujące czynności:

1.  Rzutuje przekazany kontenera `View` do `ViewPager` odwołania.

2.  Rzutuje przekazany obiekt Java (`view`) w C# `View` (`view as View`);

3.  Usuwa widok z `ViewPager`. 



### <a name="implement-isviewfromobject"></a>Implementowanie IsViewFromObject

Jako slajdów użytkownika po lewej i prawej za pośrednictwem strony zawartości `ViewPager` wywołania `IsViewFromObject` do sprawdzenia, czy obiekt podrzędny `View` na pozycji jest skojarzony z obiektem karty w tym samym miejscu (w związku z tym nosi nazwę obiektu karty *klucz obiektu*). W przypadku aplikacji stosunkowo proste skojarzenie jest jednym z tożsamości &ndash; karty klucz obiektu w tym wystąpieniu jest widok, który został wcześniej zostały zwrócone do `ViewPager` za pośrednictwem `InstantiateItem`. Jednak w przypadku innych aplikacji klucz obiektu może być niektóre inne wystąpienia klasy karty skojarzonego z (ale nie taka sama, jak) widok podrzędny `ViewPager` wyświetlane na tej pozycji. Tylko karty wie, czy widok przekazany i klucz obiektu są skojarzone. 

`IsViewFromObject` musi zostać wdrożone dla `PagerAdapter` działać prawidłowo. Jeśli `IsViewFromObject` zwraca `false` dla danej pozycji, `ViewPager` widoku nie będą wyświetlane na tej pozycji. W `TreePager` aplikacji, obiekt klucza zwróconego przez `InstantiateItem` *jest* strony `View` drzewa, tak sprawdzić tożsamość zawiera tylko kod (tj, klucz obiektu i widoku są tego samego). Zastąp `IsViewFromObject` następującym kodem: 

```csharp
public override bool IsViewFromObject(View view, Java.Lang.Object obj)
{
    return view == obj;
}
```


## <a name="add-the-adapter-to-the-viewpager"></a>Dodaj kartę do ViewPager

Teraz, gdy `TreePagerAdapter` jest zaimplementowana, jest czas, aby dodać go do `ViewPager`. W **MainActivity.cs**, Dodaj następujący wiersz kodu na końcu `OnCreate` metody:

```csharp
viewPager.Adapter = new TreePagerAdapter(this, treeCatalog);
```

Ten kod tworzy `TreePagerAdapter`, przekazując `MainActivity` jako kontekst (`this`). Skonkretyzowanym `TreeCatalog` zostanie przekazany do konstruktora drugi argument. `ViewPager`w `Adapter` ustawiono właściwość do wystąpień `TreePagerAdapter` obiekt; tej wtyczki `TreePagerAdapter` do `ViewPager`. 

Implementacja core jest teraz ukończona &ndash; skompilować i uruchomić aplikację. Powinny pojawić się pierwszy obraz katalogu drzewa wyświetlane na ekranie, jak pokazano w lewym dalej zrzucie ekranu. Przejdź po lewej, aby zobaczyć więcej widok drzewa, następnie prawo przejdź do wrócić do katalogu drzewa: 

[![Zrzuty ekranu TreePager aplikacji, szybko przesuwając za pomocą obrazów drzewa](viewpager-and-views-images/03-example-views-sml.png)](viewpager-and-views-images/03-example-views.png#lightbox)



## <a name="add-a-pager-indicator"></a>Dodaj wskaźnik Pager

Tym minimalnego `ViewPager` implementacji zawiera obrazy katalogu drzewa, ale zapewnia bez wskazania, gdy użytkownik znajduje się w katalogu. Następnym krokiem jest dodanie `PagerTabStrip`. `PagerTabStrip` Informuje użytkownika jako do strony, która jest wyświetlana i udostępnia kontekstu nawigacji, wyświetlając wskazówkę stron poprzedniego i następnego. `PagerTabStrip` jest przeznaczony do użycia jako wskaźnik dla bieżącej strony `ViewPager`; przewija i aktualizuje jako swipes użytkownika za pośrednictwem każdej strony. 

Otwórz **Resources/layout/Main.axml** i Dodaj `PagerTabStrip` do układu:

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.v4.view.ViewPager
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/viewpager"
    android:layout_width="match_parent"
    android:layout_height="match_parent" >

    <android.support.v4.view.PagerTabStrip
          android:layout_width="match_parent"
          android:layout_height="wrap_content"
          android:layout_gravity="top"
          android:paddingBottom="10dp"
          android:paddingTop="10dp"
          android:textColor="#fff" />

</android.support.v4.view.ViewPager>
```

`ViewPager` i `PagerTabStrip` działają razem. Gdy zadeklarować `PagerTabStrip` wewnątrz `ViewPager` układ `ViewPager` będą automatycznie znajdować `PagerTabStrip` i podłącz go do karty sieciowej. Podczas kompilacji i uruchom aplikację, powinny być widoczne wszystkie puste `PagerTabStrip` wyświetlany u góry każdego ekranu: 

[![Zrzut ekranu Closeup PagerTabStrip pusty](viewpager-and-views-images/04-empty-pagetabstrip-cap-sml.png)](viewpager-and-views-images/04-empty-pagetabstrip-cap.png#lightbox)



### <a name="display-a-title"></a>Wyświetlaj tytułu

Aby dodać tytuł do każdej karty strony, zaimplementuj `GetPageTitleFormatted` metoda `PagerAdapter`-klasy. `ViewPager` wywołania `GetPageTitleFormatted` (jeśli zaimplementowano) można uzyskać ciąg tytułu, który opisuje stronę od określonej pozycji. Dodaj następującą metodę do `TreePagerAdapter` klasy w **TreePagerAdapter.cs**: 

```csharp
public override Java.Lang.ICharSequence GetPageTitleFormatted(int position)
{
    return new Java.Lang.String(treeCatalog[position].caption);
}
```

Ten kod pobiera parametry podpisu drzewa z określonej strony (pozycji) w katalogu drzewa, konwertuje go na języku Java `String`i zwraca go do `ViewPager`. Po uruchomieniu aplikacji z tej nowej metody każdej stronie wyświetla podpis drzewa w `PagerTabStrip`. Powinny pojawić się w górnej części ekranu bez podkreślenie nazwa drzewa: 

[![Zrzuty ekranu stron z kartami PagerTabStrip wypełnione tekstu](viewpager-and-views-images/05-final-pagetabstrip-sml.png)](viewpager-and-views-images/05-final-pagetabstrip.png#lightbox)

Można szybko przesuń i z powrotem do wyświetlania każdego obrazu drzewa podpisami w katalogu. 



### <a name="pagertitlestrip-variation"></a>Zmiana PagerTitleStrip

`PagerTitleStrip` jest bardzo podobny do `PagerTabStrip` z tą różnicą, że `PagerTabStrip` dodaje podkreślenie dla aktualnie wybranej karty. Można zastąpić `PagerTabStrip` z `PagerTitleStrip` w powyższych układ i uruchom aplikację ponownie, aby zobaczyć, jak wygląda z `PagerTitleStrip`: 

[![PagerTitleStrip podkreśleniem usunięta z tekstu](viewpager-and-views-images/06-pagetitlestrip-example-sml.png)](viewpager-and-views-images/06-pagetitlestrip-example.png#lightbox)

Należy pamiętać, że podkreślenie zostanie usunięta po przekonwertowaniu `PagerTitleStrip`. 


 
## <a name="summary"></a>Podsumowanie

W tym przewodniku podany krok przykład sposobu tworzenia podstawowego `ViewPager`— na podstawie aplikacji bez użycia `Fragment`s. On przedstawiony przykład źródło danych zawierające obrazy i parametry podpisu `ViewPager` układu do wyświetlania obrazów, a `PagerAdapter` podklasy, który łączy `ViewPager` ze źródłem danych. Pomóc użytkownikowi Przejdź w zestawie danych, zostały zawarte instrukcje objaśniające, jak dodać `PagerTabStrip` lub `PagerTitleStrip` do wyświetlenia etykieta obrazu na początku każdej strony. 


## <a name="related-links"></a>Linki pokrewne

- [TreePager (przykład)](https://developer.xamarin.com/samples/monodroid/UserInterface/TreePager)
