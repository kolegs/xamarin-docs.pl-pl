---
title: ViewPager z fragmenty
description: "ViewPager jest Menedżer układu, umożliwiające wdrożenie gestural nawigacji. Gestural nawigacji umożliwia użytkownikowi Przejdź lewy i prawy kroków strony danych. Ten przewodnik opisuje sposób nadawania swipeable interfejsu użytkownika z ViewPager przy użyciu fragmentów jako strony danych."
ms.topic: article
ms.prod: xamarin
ms.assetid: 62B6286F-3680-48F3-B91B-453692E457E5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: cd71617cce209ef0127023f69c2b503fee031e43
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="viewpager-with-fragments"></a>ViewPager z fragmenty

_ViewPager jest Menedżer układu, umożliwiające wdrożenie gestural nawigacji. Gestural nawigacji umożliwia użytkownikowi Przejdź lewy i prawy kroków strony danych. Ten przewodnik opisuje sposób nadawania swipeable interfejsu użytkownika z ViewPager przy użyciu fragmentów jako strony danych._

 
## <a name="overview"></a>Omówienie

`ViewPager` jest często używana w połączeniu z fragmentów, dzięki czemu jest łatwiejsze w zarządzaniu cyklem życia każdej strony `ViewPager`. W tym przewodniku `ViewPager` służy do tworzenia aplikacji o nazwie **FlashCardPager** przedstawiający szereg problemów matematyczne na kartach flash. Każda karta flash jest implementowany jako fragmentu. Użytkownik swipes lewy i prawy za pośrednictwem karty flash i naciska na problem matematyczne, aby wyświetlić jego odpowiedzi. Ta aplikacja tworzy `Fragment` wystąpienia dla każdej karty flash i implementuje pochodną karty `FragmentPagerAdapter`. W [Viewpager i widoki](~/android/user-interface/controls/view-pager/viewpager-and-views.md), większość pracy zostało wykonane w `MainActivity` metody cyklu życia. W **FlashCardPager**, większość pracy zostanie wykonane przez `Fragment` w jednej z metod jej cyklu życia. 

Ten przewodnik nie obejmuje podstawy fragmenty &ndash; Jeśli nie znasz jeszcze z fragmentów w Xamarin.Android, zobacz [fragmenty](~/android/platform/fragments/index.md) ułatwiające rozpoczęcie pracy z fragmentów. 



## <a name="start-an-app-project"></a>Projekt aplikacji

Utwórz nowy projekt dla systemu Android o nazwie **FlashCardPager**. Następnie uruchom Menedżera pakietów NuGet (Aby uzyskać więcej informacji na temat instalowania pakietów NuGet, zobacz [wskazówki: w tym NuGet w projekcie](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)). Znajdź i zainstaluj **Xamarin.Android.Support.v4** pakietu zgodnie z objaśnieniem w [Viewpager i widoki](~/android/user-interface/controls/view-pager/viewpager-and-views.md). 



## <a name="add-an-example-data-source"></a>Dodawanie źródła danych przykład

W **FlashCardPager**, źródło danych jest talii kart flash reprezentowany przez `FlashCardDeck` klasy; te dane źródła dostaw `ViewPager` z zawartością elementu. `FlashCardDeck` zawiera kolekcję gotowe matematyczne problemów i odpowiedzi. `FlashCardDeck` Konstruktor wymaga argumentów: 

```csharp
FlashCardDeck flashCards = new FlashCardDeck();
```

Zbiór kart flash w `FlashCardDeck` jest zorganizowana w taki sposób, że każda karta flash są dostępne dla indeksatora. Na przykład następujący wiersz kodu pobiera czwarty problem flash karty w talii: 

```csharp
string problem = flashCardDeck[3].Problem;
```

Ten wiersz kodu pobiera odpowiednie odpowiedzi do poprzedniego problemu:

```csharp
string answer = flashCardDeck[3].Answer;
```

Ponieważ szczegóły implementacji `FlashCardDeck` nie są istotne dla zrozumienia `ViewPager`, `FlashCardDeck` kodu nie ma na liście.
Aby kod źródłowy `FlashCardDeck` znajduje się w temacie [FlashCardDeck.cs](https://github.com/xamarin/monodroid-samples/blob/master/UserInterface/FlashCardPager/FlashCardPager/FlashCardDeck.cs).
Pobierz ten plik źródłowy (lub skopiuj i Wklej kod do nowego **FlashCardDeck.cs** plików) i dodaj go do projektu.



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
```

Plik XML definiuje `ViewPager` który zajmuje cały ekran. Należy pamiętać, że muszą używać w pełni kwalifikowaną nazwę **android.support.v4.view.ViewPager** ponieważ `ViewPager` jest spakowany w bibliotece pomocy technicznej. `ViewPager` jest dostępna wyłącznie z [biblioteki obsługi systemu Android w wersji 4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/); nie jest dostępne w zestawie SDK systemu Android.


## <a name="set-up-viewpager"></a>Konfigurowanie ViewPager

Edytuj **MainActivity.cs** i dodaj następującą `using` instrukcji:

```csharp
using Android.Support.V4.View;
using Android.Support.V4.App;
```

Zmień `MainActivity` deklaracji klasy, dzięki czemu jest pochodną `AppCompatActivity`:

```csharp
public class MainActivity : FragmentActivity
```

`MainActivity` jest pochodną`FragmentActivity` (zamiast `Activity`) ponieważ `FragmentActivity` wie, jak zarządzać obsługi fragmentów. Zastąp `OnCreate` metodę z następującym kodem: 

```csharp
protected override void OnCreate(Bundle bundle)
{
    base.OnCreate(bundle);
    SetContentView(Resource.Layout.Main);
    ViewPager viewPager = FindViewById<ViewPager>(Resource.Id.viewpager);
    FlashCardDeck flashCards = new FlashCardDeck();
}
```

Ten kod wykonuje następujące czynności:

1.  Ustawia widok z **Main.axml** zasobów układu.

2.  Pobiera odwołanie do `ViewPager` z układu.

3.  Tworzy nową `FlashCardDeck` jako źródła danych.

Podczas kompilacji, a następnie uruchomić ten kod, powinien zostać wyświetlony ekran podobny Poniższy zrzut ekranu: 

[![Zrzut ekranu FlashCardPager aplikacji za pomocą ViewPager pusty](viewpager-and-fragments-images/01-initial-screen-sml.png)](viewpager-and-fragments-images/01-initial-screen.png#lightbox)

W tym momencie `ViewPager` jest pusta, ponieważ jest brak fragmentów, które są używane wypełnić `ViewPager`, i brak mu adapter do tworzenia tych fragmentów danych w **FlashCardDeck**. 

W poniższych sekcjach `FlashCardFragment` tworzą do implementowania każdej karty flash i `FragmentPagerAdapter` służy do połączenia `ViewPager` na fragmenty utworzone na podstawie danych w `FlashCardDeck`. 



## <a name="create-the-fragment"></a>Utwórz Fragment

Każda karta flash, które będą zarządzane przez fragment interfejsu użytkownika o nazwie `FlashCardFragment`. `FlashCardFragment`w widoku będą wyświetlane informacje z jednej karty flash. Każde wystąpienie `FlashCardFragment` będzie obsługiwana przez `ViewPager`. 
`FlashCardFragment`w widoku będzie składać się z `TextView` wyświetlającym tekst problem Karta flash. Ten widok wdroży program obsługi zdarzeń, który używa `Toast` do wyświetlenia odpowiedzi po naciśnięciu pytanie Karta flash. 



### <a name="create-the-flashcardfragment-layout"></a>Utwórz układ FlashCardFragment

Przed `FlashCardFragment` może być zaimplementowany, jego układ musi być zdefiniowany. Ten układ jest jeden fragment układ kontenera fragmentu. Dodaj nowy układ Android **zasobów/układ** o nazwie **flashcard_layout.axml**. Otwórz **Resources/layout/flashcard_layout.axml** i zastąp jego zawartość następującym kodem: 

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <TextView
        android:id="@+id/flash_card_question"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:gravity="center"
            android:textAppearance="@android:style/TextAppearance.Large"
            android:textSize="100sp"
            android:layout_centerHorizontal="true"
            android:layout_centerVertical="true"
            android:text="Question goes here" />
    </RelativeLayout>
```

Definiuje fragment jedna karta flash; Każdy fragment składa się z `TextView` wyświetlający matematyczne problem przy użyciu czcionki duże (100sp). Ten tekst jest wyśrodkowywana w poziomie i w pionie, na karcie flash. 



### <a name="create-the-initial-flashcardfragment-class"></a>Tworzenie klasy FlashCardFragment wstępnej

Dodaj nowy plik o nazwie **FlashCardFragment.cs** i zastąp jego zawartość następującym kodem:

```csharp
using System;
using Android.OS;
using Android.Views;
using Android.Widget;
using Android.Support.V4.App;

namespace FlashCardPager
{
    public class FlashCardFragment : Android.Support.V4.App.Fragment
    {
        public FlashCardFragment() { }

        public static FlashCardFragment newInstance(String question, String answer)
        {
            FlashCardFragment fragment = new FlashCardFragment();
            return fragment;
        }
        public override View OnCreateView (
            LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState)
        {
            View view = inflater.Inflate (Resource.Layout.flashcard_layout, container, false);
            TextView questionBox = (TextView)view.FindViewById (Resource.Id.flash_card_question);
            return view;
        }
    }
}
```

Ten kod zastępcze limit podstawowych `Fragment` definicji, która będzie służyć do wyświetlenia Karta flash. Należy pamiętać, że `FlashCardFragment` pochodzi z wersji biblioteki obsługi `Fragment` zdefiniowane w `Android.Support.V4.App.Fragment`. Konstruktor jest pusta, aby `newInstance` metoda fabryki służy do tworzenia nowego `FlashCardFragment` zamiast konstruktora. 

`OnCreateView` Cyklu życia metoda tworzy i konfiguruje `TextView`. Go nadyma układu dla fragmentu `TextView` i zwraca nadmuchany `TextView` do obiektu wywołującego. `LayoutInflater` i `ViewGroup` są przekazywane do `OnCreateView` tak, aby go zwiększyć układu. `savedInstanceState` Pakietu zawiera dane, które `OnCreateView` używa do odtworzenia `TextView` z zapisanym stanie. 

Widok ten fragment jest jawnie zwiększony przez wywołanie `inflater.Inflate`. `container` Argument jest nadrzędny widoku i `false` flagi nakazuje inflater zaniechania Dodawanie widoku nadmuchany do widoku nadrzędnego (zostaną dodane, gdy `ViewPager` wywołanie elementu karty `GetItem` metody później w tym Przewodnik). 



### <a name="add-state-code-to-flashcardfragment"></a>Dodaj do FlashCardFragment kod stanu

Podobnie jak działanie ma fragmentu `Bundle` go używa, aby zapisać i pobrania jej stanu. W **FlashCardPager**, to `Bundle` służy do Zapisz pytanie i odpowiedź tekst skojarzony karty flash. W **FlashCardFragment.cs**, Dodaj następujący `Bundle` klucze do góry `FlashCardFragment` definicji klasy: 

```csharp
private static string FLASH_CARD_QUESTION = "card_question";
private static string FLASH_CARD_ANSWER = "card_answer";
```

Modyfikowanie `newInstance` metoda fabryki, tak że tworzy `Bundle` obiektu i używa do przechowywania przekazany pytanie i odpowiedź tekst po zostanie on uruchomiony powyżej kluczy: 

```csharp
public static FlashCardFragment newInstance(String question, String answer)
{
    FlashCardFragment fragment = new FlashCardFragment();

    Bundle args = new Bundle();
    args.PutString(FLASH_CARD_QUESTION, question);
    args.PutString(FLASH_CARD_ANSWER, answer);
    fragment.Arguments = args;

    return fragment;
}
```

Zmodyfikuj metodę cyklu życia fragmentu `OnCreateView` można pobrać informacji z przekazany w pakiecie i załadować tekstu zapytania do `TextBox`: 

```csharp
public override View OnCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState)
{
    string question = Arguments.GetString(FLASH_CARD_QUESTION, "");
    string answer = Arguments.GetString(FLASH_CARD_ANSWER, "");

    View view = inflater.Inflate(Resource.Layout.flashcard_layout, container, false);
    TextView questionBox = (TextView)view.FindViewById(Resource.Id.flash_card_question);
    questionBox.Text = question;

    return view;
}
```

`answer` Zmienna nie jest używana w tym miejscu, ale będzie służyć później po dodaniu kod obsługi zdarzeń do tego pliku. 


## <a name="create-the-adapter"></a>Utwórz kartę

`ViewPager` używa obiektu kontrolera karty, która znajduje się między `ViewPager` i źródła danych (patrz ilustracja w ViewPager [karty](~/android/user-interface/controls/view-pager/index.md#adapter) artykułu). Aby dostęp do tych danych `ViewPager` wymaga podania adapter niestandardowy pochodną `PagerAdapter`. Ponieważ w tym przykładzie użyto fragmenty, używa `FragmentPagerAdapter` &ndash; `FragmentPagerAdapter` jest pochodną `PagerAdapter`. 
`FragmentPagerAdapter` reprezentuje stronę jako `Fragment` który jest trwale przechowywana w Menedżerze fragmentów dla tak długo, jak użytkownik może wrócić do strony. Jako swipes użytkownika za pośrednictwem strony `ViewPager`, `FragmentPagerAdapter` wyodrębnia dane ze źródła danych i używa jej do utworzenia `Fragment`s dla `ViewPager` do wyświetlenia. 

Po zaimplementowaniu `FragmentPagerAdapter`, konieczne jest przesłonięcie następujących czynności:

-   **Liczba** &ndash; właściwości tylko do odczytu, która zwraca liczbę dostępnych widoków (strony).

-   **GetItem** &ndash; Zwraca fragment do wyświetlenia określonej strony.

Dodaj nowy plik o nazwie **FlashCardDeckAdapter.cs** i zastąp jego zawartość następującym kodem:

```csharp
using System;
using Android.Views;
using Android.Widget;
using Android.Support.V4.App;

namespace FlashCardPager
{
    class FlashCardDeckAdapter : FragmentPagerAdapter
    {
        public FlashCardDeckAdapter (Android.Support.V4.App.FragmentManager fm, FlashCardDeck flashCards)
            : base(fm)
        {
        }

        public override int Count
        {
            get { throw new NotImplementedException(); }
        }

        public override Android.Support.V4.App.Fragment GetItem(int position)
        {
            throw new NotImplementedException();
        }
    }
}
```

Ten kod zastępcze limit podstawowych `FragmentPagerAdapter` implementacji. W poniższych sekcjach każda z tych metod zostanie zastąpiony kodem pracy. Konstruktor ma na celu przekazania Menedżera fragmentu `FlashCardDeckAdapter`przez konstruktora klasy podstawowej. 



### <a name="implement-the-adapter-constructor"></a>Implementuje konstruktora karty

Gdy aplikacja tworzy `FlashCardDeckAdapter`, podaje odwołania do Menedżera fragmentu i wystąpień `FlashCardDeck`. Dodaj następujące zmiennej członkowskiej na początku `FlashCardDeckAdapter` klasy w **FlashCardDeckAdapter.cs**: 

```csharp
public FlashCardDeck flashCardDeck;
```

Dodaj następujący wiersz kodu w celu `FlashCardDeckAdapter` konstruktora: 

```csharp
this.flashCardDeck = flashCards;
```

Ten wiersz kodu magazynów `FlashCardDeck` wystąpienie `FlashCardDeckAdapter` będzie używana. 



### <a name="implement-count"></a>Liczba wdrożenie

`Count` Implementacji jest stosunkowo prosta: zwraca liczbę kart flash talii Karta flash. Zastąp `Count` następującym kodem: 

```csharp
public override int Count
{
    get { return flashCardDeck.NumCards; }
}
```


`NumCards` Właściwość `FlashCardDeck` zwraca liczbę kart flash (liczba fragmentów) w zestawie danych. 



### <a name="implement-getitem"></a>Implementowanie GetItem

`GetItem` Metoda zwraca fragmentu skojarzone z określonej pozycji. Gdy `GetItem` jest wywoływana dla pozycji w talii Karta flash zwraca `FlashCardFragment` skonfigurowany tak, aby wyświetlić problem Karta flash na tej pozycji. Zastąp `GetItem` metodę z następującym kodem: 

```csharp
public override Android.Support.V4.App.Fragment GetItem(int position)
{
    return (Android.Support.V4.App.Fragment)
        FlashCardFragment.newInstance (
            flashCardDeck[position].Problem, flashCardDeck[position].Answer);
}
```

Ten kod wykonuje następujące czynności:

1.  Wyszukuje ciąg problem matematyczne w `FlashCardDeck` służącego do określonej pozycji. 

2.  Wyszukuje ciąg odpowiedzi w `FlashCardDeck` służącego do określonej pozycji. 

3.  Wywołania `FlashCardFragment` metoda fabryki `newInstance`, przekazując Karta flash ciągi problem i odpowiedzi. 

4.  Tworzy i zwraca nową kartę flash `Fragment` zawierający tekst pytania i odpowiedzi dla tej pozycji. 

Gdy `ViewPager` renderuje `Fragment` w `position`, wyświetla `TextBox` zawierającą ciąg problem matematyczne znajdującej się na `position` talii Karta flash. 



## <a name="add-the-adapter-to-the-viewpager"></a>Dodaj kartę do ViewPager

Teraz, gdy `FlashCardDeckAdapter` jest zaimplementowana, jest czas, aby dodać go do `ViewPager`. W **MainActivity.cs**, Dodaj następujący wiersz kodu na końcu `OnCreate` metody:

```csharp
FlashCardDeckAdapter adapter =
    new FlashCardDeckAdapter(SupportFragmentManager, flashCards);
viewPager.Adapter = adapter;
```

Ten kod tworzy `FlashCardDeckAdapter`, przekazując `SupportFragmentManager` w pierwszym argumencie. ( `SupportFragmentManager` Można uzyskać odwołania do właściwości FragmentActivity `FragmentManager` &ndash; uzyskać więcej informacji o `FragmentManager`, zobacz [Zarządzanie fragmenty](~/android/platform/fragments/managing-fragments.md).) 

Implementacja core jest teraz ukończona &ndash; skompilować i uruchomić aplikację.
Powinny pojawić się pierwszy obraz talii Karta flash wyświetlane na ekranie, jak pokazano w lewym dalej zrzucie ekranu. Przejdź po lewej, aby zobaczyć więcej kart flash, następnie przejdź prawej strony, aby wrócić do talii Karta flash:

[![Przykład zrzuty ekranu aplikacji FlashCardPager bez pagera wskaźników](viewpager-and-fragments-images/02-example-views-sml.png)](viewpager-and-fragments-images/02-example-views.png#lightbox)



## <a name="add-a-pager-indicator"></a>Dodaj wskaźnik Pager

Tym minimalnego `ViewPager` implementacji Wyświetla każdego flash karty w talii, ale stanowi bez wskazania, gdy użytkownik znajduje się w talii. Następnym krokiem jest dodanie `PagerTabStrip`. `PagerTabStrip` Informuje użytkownika, określające, jaki problem numer jest wyświetlany i udostępnia kontekstu nawigacji, wyświetlając wskazówkę karty flash poprzedniego i następnego. 

Otwórz **Resources/layout/Main.axml** i Dodaj `PagerTabStrip` do układu:

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.v4.view.ViewPager xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/pager"
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

Podczas kompilacji i uruchom aplikację, powinny być widoczne wszystkie puste `PagerTabStrip` wyświetlany u góry każdego Karta flash: 

[![Closeup PagerTabStrip bez tekstu](viewpager-and-fragments-images/03-empty-pagetabstrip-sml.png)](viewpager-and-fragments-images/03-empty-pagetabstrip.png#lightbox)



### <a name="display-a-title"></a>Wyświetlaj tytułu

Aby dodać tytuł do każdej karty strony, zaimplementuj `GetPageTitleFormatted` metody na karcie. `ViewPager` wywołania `GetPageTitleFormatted` (jeśli zaimplementowano) można uzyskać ciąg tytułu, który opisuje stronę od określonej pozycji. Dodaj następującą metodę do `FlashCardDeckAdapter` klasy w **FlashCardDeckAdapter.cs**: 

```csharp
public override Java.Lang.ICharSequence GetPageTitleFormatted(int position)
{
    return new Java.Lang.String("Problem " + (position + 1));
}
```

Ten kod konwertuje pozycja w talii Karta flash liczbę problem. Wynikowy ciąg jest konwertowany na języku Java `String` zwróceniem do `ViewPager`. Po uruchomieniu aplikacji z tej nowej metody każdej strony zawiera numer problem w `PagerTabStrip`: 

[![Zrzuty ekranu FlashCardPager o numerze problem powyżej każdej strony wyświetlany](viewpager-and-fragments-images/04-pagetabstrip-sml.png)](viewpager-and-fragments-images/04-pagetabstrip.png#lightbox)

Szybko wyświetlić numer problem w talii Karta flash wyświetlany u góry każdego Karta flash Przesuń i z powrotem. 



## <a name="handle-user-input"></a>Obsługa danych wejściowych użytkownika

**FlashCardPager** prezentuje serię na podstawie fragmentu karty flash w `ViewPager`, ale nie ma jeszcze sposobem ujawnić odpowiedzi dla każdego problemu. W tej sekcji, program obsługi zdarzeń jest dodawany do `FlashCardFragment` do wyświetlenia odpowiedzi po naciśnięciu w tekście problem Karta flash. 

Otwórz **FlashCardFragment.cs** i Dodaj następujący kod na końcu `OnCreateView` metody tuż przed widoku jest zwracany do obiektu wywołującego: 

```csharp
questionBox.Click += delegate
{
    Toast.MakeText(Activity.ApplicationContext,
            "Answer: " + answer, ToastLength.Short).Show();
};
```

To `Click` obsługi zdarzeń wyświetla odpowiedź w wyskakującego powiadomienia, który jest wyświetlany, gdy użytkownik naciska `TextBox`. `answer` Zmiennej został zainicjowany wcześniej, gdy informacje o stanie zostały odczytane z pakietu, który został przekazany do `OnCreateView`. Kompilacji i uruchom aplikację, a następnie naciśnij pozycję tekstu na każdej karcie flash wyświetlić odpowiedzi: 

[![Zrzuty ekranu FlashCardPager aplikacji wyskakujące powiadomienia matematyczne problem jest wybrany.](viewpager-and-fragments-images/05-answer-sml.png)](viewpager-and-fragments-images/05-answer.png#lightbox)

**FlashCardPager** przedstawionych w tym przewodniku używa `MainActivity` pochodną `FragmentActivity`, ale może również pochodzić `MainActivity` z `AppCompatActivity` (który również obsługuje zarządzanie fragmenty). Aby wyświetlić `AppCompatActivity` przykład, zobacz [FlashCardPager](https://developer.xamarin.com/samples/monodroid/UserInterface%5CFlashCardPager/) w galerii próbki. 



## <a name="summary"></a>Podsumowanie

W tym przewodniku podany krok przykład sposobu tworzenia podstawowego `ViewPager`— na podstawie aplikacji przy użyciu `Fragment`s. On przedstawiony przykład źródła danych zawierającego Karta flash pytania i odpowiedzi, `ViewPager` układu, aby wyświetlić karty flash i `FragmentPagerAdapter` podklasy, który łączy `ViewPager` ze źródłem danych. Pomóc użytkownikowi, przejdź do karty flash, zostały uwzględnione instrukcje objaśniające, jak dodać `PagerTabStrip` do wyświetlenia liczba problemu na początku każdej strony. Na koniec kod obsługi zdarzeń został dodany do wyświetlenia odpowiedzi po naciśnięciu na problem Karta flash. 



## <a name="related-links"></a>Linki pokrewne

- [FlashCardPager (przykład)](https://developer.xamarin.com/samples/monodroid/UserInterface/FlashCardPager)
