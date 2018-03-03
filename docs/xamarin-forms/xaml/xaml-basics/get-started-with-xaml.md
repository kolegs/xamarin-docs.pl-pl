---
title: "Część 1. Wprowadzenie do języka XAML"
description: "W aplikacji platformy Xamarin.Forms XAML, przede wszystkim służy do definiowania visual zawartości strony. Plik XAML zawsze jest skojarzony z plikiem kodu C#, który obsługuje kod znaczników. Razem tych dwóch plików współtworzyć nową definicję klasy widoków podrzędnych i właściwości inicjowania. W pliku XAML klasy i właściwości, które są przywoływane z elementów XML oraz atrybuty i ustanowiono łącza między znaczników i kodu."
ms.topic: article
ms.prod: xamarin
ms.assetid: 1D6164F9-4ECE-43A6-B583-1F5D5EFC1DDF
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 10/25/2017
ms.openlocfilehash: 8e02dbd8687fc10582874710db7ca6848f546751
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="part-1-getting-started-with-xaml"></a>Część 1. Wprowadzenie do języka XAML

_W aplikacji platformy Xamarin.Forms XAML, przede wszystkim służy do definiowania visual zawartości strony. Plik XAML zawsze jest skojarzony z plikiem kodu C#, który obsługuje kod znaczników. Razem tych dwóch plików współtworzyć nową definicję klasy widoków podrzędnych i właściwości inicjowania. W pliku XAML klasy i właściwości, które są przywoływane z elementów XML oraz atrybuty i ustanowiono łącza między znaczników i kodu._

## <a name="creating-the-solution"></a>Tworzenie rozwiązania

Aby rozpocząć edytowanie pierwszego pliku XAML, użyj programu Visual Studio lub Visual Studio dla komputerów Mac, aby utworzyć nowe rozwiązanie platformy Xamarin.Forms. (Wybierz kartę w górnej części strony odpowiadający środowisku).

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

W systemie Windows, użyj programu Visual Studio, aby wybrać **Plik > Nowy > Projekt** z menu. W **nowy projekt** okno dialogowe, wybierz opcję **Visual C# > Cross Platform** po lewej stronie, a następnie **dla wielu aplikacji dla platformy (platformy Xamarin.Forms lub natywne)** z listy w Centrum. 

![](get-started-with-xaml-images/win/newprojectdialog.png "Okno dialogowe nowego projektu")

Wybierz lokalizację dla rozwiązania, nadaj mu nazwę **XamlSamples** (lub użytkownik woli) i naciśnij klawisz **OK**.

Na następnym ekranie Wybierz **pusta aplikacja** szablonu, **platformy Xamarin.Forms** technologii interfejsu użytkownika i **przenośnej biblioteki klasy (PCL)** strategii udostępniania kodu:

![](get-started-with-xaml-images/win/newcrossplatformapp.png "Okno dialogowe nowego aplikacji")

Press **OK**. 

Cztery projekty są tworzone w rozwiązaniu: **XamlSamples** przenośnej biblioteki klas (PCL) **XamlSamples.Android**, **XamlSamples.iOS**i uniwersalnych systemu Windows Rozwiązanie platformy **XamlSamples.UWP**.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

W programie Visual Studio dla komputerów Mac, wybierz **Plik > nowe rozwiązanie** z menu. W **nowy projekt** okno dialogowe, wybierz opcję **Multiplatform > aplikacji** po lewej stronie, a **pusta aplikacja formularzy** (*nie* **formularzy aplikacji** ) z listy szablonów:

![](get-started-with-xaml-images/mac/newprojectdialog1.png "Okno dialogowe nowego projektu 1")

Naciśnij klawisz **dalej**.

W oknie dialogowym dalej nazwę projektu z **XamlSamples** (lub użytkownik woli). Upewnij się, że **Użyj przenośnej biblioteki klas** przycisk radiowy zostanie wybrany oraz że **XAML Użyj plików interfejs użytkownika** zaznaczono:

![](get-started-with-xaml-images/mac/newprojectdialog2.png "Okno dialogowe nowego projektu 2")

Naciśnij klawisz **dalej**. 

W oknie dialogowym następujące można wybrać lokalizację dla projektu:

![](get-started-with-xaml-images/mac/newprojectdialog3.png "Okno dialogowe nowego projektu 3")

Naciśnij klawisz **tworzenie**

W rozwiązaniu są tworzone trzy projekty: **XamlSamples** przenośnej biblioteki klas (PCL) **XamlSamples.Android**, i **XamlSamples.iOS**. 

-----

Po utworzeniu **XamlSamples** rozwiązanie, warto przetestować środowiska deweloperskiego, wybierając różne projekty platformy jako projekt startowy rozwiązania i tworzenie i wdrażanie prostej aplikacji utworzone przez szablon projektu na emulatory telefonu lub rzeczywistego urządzenia.

O ile nie trzeba napisać kod specyficzne dla platformy udostępnionego **XamlSamples** PCL projekt jest, gdzie można będzie spędzać praktycznie cały czas programowania. Te artykuły nie będzie ryzyka poza tego projektu.

### <a name="anatomy-of-a-xaml-file"></a>Anatomia pliku XAML

W ramach **XamlSamples** przenośnej biblioteki klas są pary plików z następujących nazw:

- **App.XAML**, plik XAML; i
- **App.XAML.cs**, C# *CodeBehind* plik skojarzony z pliku XAML.

Musisz kliknij strzałkę obok pozycji **App.xaml** do znajduje się w pliku CodeBehind. 

Zarówno **App.xaml** i **App.xaml.cs** przyczyniają się do klasy o nazwie `App` która pochodzi z `Application`. Większość klas z plików XAML przyczyniają się do klasy, która jest pochodną `ContentPage`; te pliki umożliwia definiowanie visual zawartość całej strony XAML. Ta zasada dotyczy dwóch plików w **XamlSamples** projektu:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

- **MainPage.xaml**, plik XAML; i
- **MainPage.xaml.cs**, plik CodeBehind C#.

**MainPage.xaml** pliku wygląda następująco:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples"
             x:Class="XamlSamples.MainPage">

    <Label Text="Welcome to Xamarin Forms!" 
           VerticalOptions="Center" 
           HorizontalOptions="Center" />

</ContentPage>
```

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

- **XamlSamplesPage.xaml**, plik XAML; i
- **XamlSamplesPage.xaml.cs**, plik CodeBehind C#.

**XamlSamplesPage.xaml** pliku wygląda następująco:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" 
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" 
             xmlns:local="clr-namespace:XamlSamples" 
             x:Class="XamlSamples.XamlSamplesPage">

    <Label Text="Welcome to Xamarin Forms!" 
           VerticalOptions="Center" 
           HorizontalOptions="Center" />

</ContentPage>
```

-----

Dwa przestrzeni nazw XML ( `xmlns`) deklaracje odwoływać się do identyfikatorów URI, pierwszy pozornie w witrynie sieci web na platformie Xamarin i drugi na firmy Microsoft. Nie odblokowane, jakie tych identyfikatorów URI wskaż sprawdzanie. Nie ma nic nie. Są po prostu identyfikatorów URI należących do firmy Microsoft i Xamarin i funkcjonują zasadniczo identyfikatorami wersji.

Pierwszej deklaracji przestrzeni nazw XML oznacza, że znaczniki zdefiniowane w pliku XAML z prefiksem nie odwołują się do klasy w platformy Xamarin.Forms, na przykład `ContentPage`. Druga deklaracja przestrzeni nazw definiuje prefiksem `x`. Ten element jest używany dla wielu elementów i atrybutów, które są wewnętrzne w języku XAML sobą i które są obsługiwane przez inne implementacje XAML. Jednak te elementy i atrybuty są nieco inne w zależności od roku osadzone w identyfikatorze URI. Platformy Xamarin.Forms obsługuje specyfikację XAML 2009, ale nie wszystkie z nich.

`local` Deklaracji przestrzeni nazw umożliwia dostęp do innych klas z projektu PCL.

Na końcu pierwszego tagu `x` prefiks jest używany w postaci atrybutu o nazwie `Class`. Ponieważ do stosowania `x` takie jak prefiks jest praktycznie uniwersalnych dla przestrzeni nazw XAML atrybuty XAML `Class` prawie zawsze są określane jako `x:Class`.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

`x:Class` Atrybut określa w pełni kwalifikowaną nazwę klasy .NET: `MainPage` klasy w `XamlSamples` przestrzeni nazw. Oznacza to, że ten plik XAML definiuje nową klasę o nazwie `MainPage` w `XamlSamples` przestrzeni nazw, która jest pochodną `ContentPage`— znacznik, w którym `x:Class` atrybut występuje.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

`x:Class` Atrybut określa w pełni kwalifikowaną nazwę klasy .NET: `XamlSamplesPage` klasy w `XamlSamples` przestrzeni nazw. Oznacza to, że ten plik XAML definiuje nową klasę o nazwie `XamlSamplesPage` w `XamlSamples` przestrzeni nazw, która jest pochodną `ContentPage`— znacznik, w którym `x:Class` atrybut występuje.

-----

`x:Class` Atrybut może występować tylko w elemencie głównym pliku XAML do definiowania klasy pochodnej w języku C#. Jest to tylko nowa klasa zdefiniowana w pliku XAML. Wszystkie inne elementy pojawia się w pliku XAML ma zamiast tego po prostu utworzonych na podstawie istniejących klas i zainicjować.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

**MainPage.xaml.cs** pliku wygląda następująco (jako uzupełnienie nieużywane `using` dyrektywy):

```csharp
using Xamarin.Forms;

namespace XamlSamples
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
        }
    }
}
```

`MainPage` Pochodną klasy `ContentPage`, ale zauważyć `partial` definicji klasy. Sugeruje to, że powinno być innej definicji częściowej klasy dla `MainPage`, ale gdy jest on? A co jest `InitializeComponent` metody? 

Gdy program Visual Studio tworzy projekt, analizuje pliku XAML, aby wygenerować plik kodu C#. W **XamlSamples\XamlSamples\obj\Debug** katalogu, można znaleźć w pliku o nazwie **XamlSamples.MainPage.xaml.g.cs**. "g" oznacza dla wygenerowany. To inne klasy częściowej definicji `MainPage` zawierający definicję `InitializeComponent` metoda wywoływana z `MainPage` konstruktora. Te dwie częściowego `MainPage` definicje klas następnie je kompilować razem. W zależności od tego, czy XAML jest kompilowane, czy nie plik XAML lub binarnym pliku XAML jest osadzony w pliku wykonywalnego.

W czasie wykonywania kodu w wywołaniach projektu danej platformy `LoadApplication` metoda przekazywania do niej nowe wystąpienie klasy `App` klasy w PCL. `App` Tworzy konstruktora klasy `MainPage`. Wywołuje konstruktor klasy `InitializeComponent`, które następnie wywołuje `LoadFromXaml` metodę, która wyodrębnia plik XAML (lub jej skompilowanym plikiem binarnym) z PCL. `LoadFromXaml` inicjuje wszystkie obiekty, które są zdefiniowane w pliku XAML, łączy je wszystkie razem w relacji nadrzędny podrzędny, dołącza obsługi zdarzeń zdefiniowanych w kodzie na zdarzenia w pliku XAML i ustawia wynikowe drzewa obiektów jako zawartość strony.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

**XamlSamplesPage.xaml.cs** pliku wygląda następująco:

```csharp
using Xamarin.Forms;

namespace XamlSamples
{
    public partial class XamlSamplesPage : ContentPage
    {
        public XamlSamplesPage()
        {
            InitializeComponent();
        }
    }
}
```

`XamlSamplesPage` Pochodną klasy `ContentPage`, ale zauważyć `partial` definicji klasy. Brak sugeruje, że powinien być inny plik C# z innej definicji częściowej klasy dla `XamlSamplesPage`, ale gdy jest on? A co jest `InitializeComponent` metody?

W przypadku programu Visual Studio for Mac kompilacji projektu, analizuje pliku XAML, aby wygenerować plik kodu C#. W **XamlSamples\XamlSamples\obj\Debug** katalogu, można znaleźć w pliku o nazwie **XamlSamples.XamlSamplesPage.xaml.g.cs**. "g" oznacza dla wygenerowany. To inne klasy częściowej definicji `XamlSamplesPage` zawierający definicję `InitializeComponent` metoda wywoływana z `XamlSamplesPage` konstruktora.  Te dwie częściowego `XamlSamplesPage` definicje klas następnie je kompilować razem. W zależności od tego, czy XAML jest kompilowane, czy nie plik XAML lub binarnym pliku XAML jest osadzony w pliku wykonywalnego.

W czasie wykonywania kodu w wywołaniach projektu danej platformy `LoadApplication` metoda przekazywania do niej nowe wystąpienie klasy `App` klasy w PCL. `App` Tworzy konstruktora klasy `XamlSamplesPage`. Wywołuje konstruktor klasy `InitializeComponent`, które następnie wywołuje `LoadFromXaml` metodę, która wyodrębnia plik XAML (lub jej skompilowanym plikiem binarnym) z PCL. `LoadFromXaml` inicjuje wszystkie obiekty, które są zdefiniowane w pliku XAML, łączy je wszystkie razem w relacji nadrzędny podrzędny, dołącza obsługi zdarzeń zdefiniowanych w kodzie na zdarzenia w pliku XAML i ustawia wynikowe drzewa obiektów jako zawartość strony.

-----

Mimo że zwykle nie trzeba poświęcić czas z plikami wygenerowanego kodu, czasami środowiska uruchomieniowego wyjątki są zgłaszane na kod w wygenerowanym pliki, należy zapoznać się z nimi.

Gdy skompilować i uruchomić ten program `Label` element będzie wyświetlany na środku strony zgodnie z sugestią, XAML. Trzy platform od lewej do prawej są z systemem iOS, Android i Windows 10 Mobile:

[![](get-started-with-xaml-images/xamlsamples.png "Domyślnie wyświetlana platformy Xamarin.Forms")](get-started-with-xaml-images/xamlsamples-large.png "wyświetlana domyślna platformy Xamarin.Forms")

Aby uzyskać bardziej interesujące elementy wizualne, wystarczy więcej interesujące XAML.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

## <a name="preliminaries"></a>Wymagania wstępne

Aby wprowadzić nazwy plików w programie Visual Studio dla komputerów Mac zgodne z plików utworzonych przez Visual Studio działające w systemie Windows, zmień **XamlSamplesPage.xaml** do **MainPage.xaml**, i  **XamlSamplesPage.xaml.cs** do **MainPage.xaml.cs**. W ramach **XamlSamplesPage.xaml** Zmień `XamlSamplesPage` do `MainPage`. W ramach **XamlSamplesPage.xaml.cs** Zmień dwa wystąpienia `XamlSamplesPage` do `MainPage`. W ramach **App.xaml.cs** pliku, Zmień instrukcję

```csharp
MainPage = new XamlSamplesPage();
```

na:

```csharp
MainPage = new MainPage();
```

-----

Test, że program nadal kompiluje i wdraża przed kontynuowaniem.

## <a name="adding-new-xaml-pages"></a>Dodawanie nowych stron XAML

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Aby dodać inne opartych na języku XAML `ContentPage` klas do projektu, zaznacz **XamlSamples** PCL projektu i wywoływać **projektu > Dodaj nowy element** elementu menu. W lewej części **Dodaj nowy element** okno dialogowe, wybierz opcję **Visual C#** i **platformy Xamarin.Forms**. Wybierz z listy **strony zawartości** (nie **strony zawartości (C#)**, który tworzy stronę tylko kod lub **widok zawartości**, który nie jest stroną). Określ stronę nazwę, na przykład **HelloXamlPage.xaml**:

![](get-started-with-xaml-images/win/addnewitemdialog.png "Dodaj nowy element okna dialogowego")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Aby dodać inne opartych na języku XAML `ContentPage` klas do projektu, zaznacz **XamlSamples** PCL projektu i wywoływać **Plik > Nowy plik** elementu menu. W lewej części **nowy plik** okno dialogowe, wybierz opcję **formularze** po lewej stronie, i **Xaml wartość ContentPage formularzy** (nie **wartość formularze ContentPage**, która tworzy stronę tylko kod lub **widok zawartości**, który nie jest stroną). Określ stronę nazwę, na przykład **HelloXamlPage**:

![](get-started-with-xaml-images/mac/newfiledialog.png "Okno dialogowe nowego pliku")

-----

Dwa pliki zostaną dodane do projektu, **HelloXamlPage.xaml** i plik CodeBehind **HelloXamlPage.xaml.cs**. 

## <a name="setting-page-content"></a>Ustawienie zawartości strony

Edytuj **HelloXamlPage.xaml** pliku, tak aby były tylko tagi dla `ContentPage` i `ContentPage.Content`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.HelloXamlPage">
    <ContentPage.Content>
        
    </ContentPage.Content>
</ContentPage>
```

`ContentPage.Content` Tagi są częścią unikatowy składni języka XAML. Na początku one wydaje się nieprawidłowy kod XML, ale są one prawnych. Okres nie jest znak specjalny w kodzie XML.

`ContentPage.Content` Tagi są nazywane *elementu właściwości* tagów. `Content` jest właściwością `ContentPage`i zazwyczaj ma ustawioną wartość jednego widoku lub układu z widokami podrzędnych. Zazwyczaj właściwości stają się atrybutów w języku XAML, ale będzie trudne do ustawić `Content` atrybutu obiekt złożony. Z tego powodu właściwość jest wyrażona jako element XML składające się z nazwy klasy i nazwę właściwości oddzielone kropką. Teraz `Content` właściwość może być ustawiony między wartością `ContentPage.Content` tagi następująco:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.HelloXamlPage"
             Title="Hello XAML Page">
    <ContentPage.Content>

        <Label Text="Hello, XAML!"
               VerticalOptions="Center"
               HorizontalTextAlignment="Center"
               Rotation="-15"
               IsVisible="true"
               FontSize="Large"
               FontAttributes="Bold"
               TextColor="Blue" />

    </ContentPage.Content>
</ContentPage>
```

Ponadto należy zauważyć, że `Title` ustawiono atrybut w tagu głównym.

W tej chwili relacji między klas, właściwości i XML powinny być widoczne: klasa platformy Xamarin.Forms A (takich jak `ContentPage` lub `Label`) pojawia się w pliku XAML, jako XML element. Właściwości tej klasy — w tym `Title` na `ContentPage` i siedmiu właściwości `Label`— zwykle są wyświetlane jako atrybuty XML.

Istnieje wiele skrótów można ustawić wartości tych właściwości. Niektóre właściwości są danych podstawowych: na przykład `Title` i `Text` właściwości są typu `String`, `Rotation` jest typu `Double`, i `IsVisible` (czyli `true` domyślnie i ustawiono tutaj tylko w przypadku Ilustracja) jest typu `Boolean`.

`HorizontalTextAlignment` Właściwość jest typu `TextAlignment`, która jest wyliczenie. Dla właściwości dowolnego typu wyliczenia Podaj wystarczy nazwę elementu członkowskiego.

Dla właściwości złożonych typów jednak konwertery są używane do analizowania XAML. Są to platformy Xamarin.Forms, który pochodzi od klasy `TypeConverter`. Wiele jest klas publicznych, ale niektórych nie. Dla tego konkretnego pliku XAML niektóre z tych klas pełnić rolę w tle:

-  `LayoutOptionsConverter` Aby uzyskać `VerticalOptions` właściwości
-  `FontSizeConverter` Aby uzyskać `FontSize` właściwości
-  `ColorTypeConverter` Aby uzyskać `TextColor` właściwości

Konwerterów określają dopuszczalny składnia ustawienia właściwości.

`ThicknessTypeConverter` Może obsługiwać jedną, dwie lub cztery liczby rozdzielone przecinkami. Jeśli podano jeden numer, ma zastosowanie do wszystkich czterech krawędzi. Z dwóch liczb pierwsza to lewy i prawy dopełnienie, a drugą jest wartość górny i dolny. Są cztery liczby w kolejności od lewej, górnej, prawej i dolnej.

`LayoutOptionsConverter` Można przekonwertować nazwy pola statyczne publiczne z `LayoutOptions` struktury do wartości typu `LayoutOptions`.

`FontSizeConverter` Może obsłużyć `NamedSize` elementu członkowskiego lub rozmiar czcionki liczbowych.

`ColorTypeConverter` Akceptuje nazwy pola statyczne publiczne z `Color` struktury lub wartości szesnastkowe RGB z lub bez kanału alfa poprzedzone znakiem numeru (#). Oto składnia bez kanału alfa:

 `TextColor="#rrggbb"`

Każdy z małego litery jest cyfrą szesnastkową. Oto, jak wchodzi kanału alfa:

 `TextColor="#aarrggbb">`

Dla kanału alfa należy pamiętać, że FF jest całkowicie nieprzezroczyste i 00 jest w pełni przezroczyste.

Dwa inne formaty umożliwiają określenie tylko jednego cyfrą szesnastkową dla każdego kanału:

 `TextColor="#rgb"``TextColor="#argb"`

W takich przypadkach program cyfra jest powtarzany do utworzenia wartość. Na przykład #CF3 jest kolor RGB 33-CC-FF.

## <a name="page-navigation"></a>Nawigacja strony

Po uruchomieniu **XamlSamples** program `MainPage` jest wyświetlany. Aby zobaczyć nowe `HelloXamlPage` można albo zestawu, który jako nowe uruchamiania w obszarze **App.xaml.cs** plików lub przejdź do nowej strony z `MainPage`.

Aby zaimplementować nawigacji, najpierw zmień kod w **App.xaml.cs** konstruktora, aby `NavigationPage` tworzony jest obiekt:

```csharp
public App()
{
    InitializeComponent();
    MainPage = new NavigationPage(new MainPage());
}
```

W **MainPage.xaml.cs** konstruktora, możesz utworzyć prostą `Button` i użyj programu obsługi zdarzeń, aby przejść do `HelloXamlPage`:

```csharp
public MainPage()
{
    InitializeComponent();

    Button button = new Button
    {
        Text = "Navigate!",
        HorizontalOptions = LayoutOptions.Center,
        VerticalOptions = LayoutOptions.Center
    };

    button.Clicked += async (sender, args) =>
    {
        await Navigation.PushAsync(new HelloXamlPage());
    };

    Content = button;
}
```

Ustawienie `Content` właściwości strony zastępuje ustawienie `Content` właściwość w pliku XAML. Podczas kompilacji i wdrażania nowej wersji tego programu, na ekranie pojawi się przycisk. Użycie jej przechodzi do `HelloXamlPage`. Oto wynikowe strony na telefonie iPhone, Android i Windows 10 Mobile urządzeń:

[ ![](get-started-with-xaml-images/helloxaml1.png "Tekst etykiety obrócony")](get-started-with-xaml-images/helloxaml1-large.png "obracany tekst etykiety")

Można przejść do `MainPage` przy użyciu **< Wstecz** przycisk w systemach iOS, w górnej części strony lub w dolnej części telefonu w systemie Android, za pomocą strzałki w lewo lub strzałki w lewo w dolnej części strony w systemie Windows 10 Mobile.

Możesz eksperymentować XAML dla różnych sposobów renderowania `Label`. Jeśli potrzebujesz osadzić dowolne znaki Unicode w tekście, można standardowej składni XML. Na przykład aby umieścić powitanie w cudzysłowy inteligentne, należy użyć:

 `<Label Text="&#x201C;Hello, XAML!&#x201D;" … />`

Oto wygląda następująco:

[ ![](get-started-with-xaml-images/helloxaml2.png "Obracany tekst etykiety ze znaków Unicode")](get-started-with-xaml-images/helloxaml2-large.png "obracany tekst etykiety ze znaków Unicode")

## <a name="xaml-and-code-interactions"></a>XAML i kodem interakcji

**HelloXamlPage** próbka zawiera tylko jeden `Label` na stronie, ale jest to bardzo rzadko. Większość `ContentPage` zestaw pochodne `Content` właściwości układ niektórych sortowania, takich jak `StackLayout`. `Children` Właściwość `StackLayout` jest zdefiniowany typ `IList<View>` , ale jest rzeczywiście typu obiektu `ElementCollection<View>`, i że kolekcji można wypełniać za pomocą wielu widoków lub innych układów. W języku XAML te relacje nadrzędny podrzędny są wyznaczane z normalnym hierarchii XML. Oto pliku XAML, aby uzyskać nową stronę o nazwie **XamlPlusCodePage**:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.XamlPlusCodePage"
             Title="XAML + Code Page">
    <StackLayout>
        <Slider VerticalOptions="CenterAndExpand" />

        <Label Text="A simple Label"
               Font="Large"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <Button Text="Click Me!"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

Ten plik XAML zakończeniu składnię, a Oto wygląda następująco:

[ ![](get-started-with-xaml-images/xamlpluscode1.png "Wiele formantów na stronie")](get-started-with-xaml-images/xamlpluscode1-large.png "wiele formantów na stronie")

Jednak użytkownik prawdopodobnie wziąć pod uwagę ten program jest funkcjonalnie niewystarczające. Być może `Slider` powinien spowodować `Label` Aby wyświetlić bieżącą wartość i `Button` prawdopodobnie jest przeznaczona do zrobienia czegoś w programie.

Jak można zauważyć w [część 4. Podstawowe informacje dotyczące powiązania danych](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md), zadania wyświetlania `Slider` wartości przy użyciu `Label` mogą być obsługiwane wyłącznie w języku XAML z powiązania danych. Jednak warto najpierw Zobacz rozwiązania kodu. Mimo tego, że obsługa `Button` kliknij wymaga ostatecznie kodu. Oznacza to, że plik CodeBehind dla `XamlPlusCodePage` musi zawierać obsługę `ValueChanged` zdarzenie `Slider` i `Clicked` zdarzenie `Button`. Dodajmy je:

```csharp
namespace XamlSamples
{
    public partial class XamlPlusCodePage
    {
        public XamlPlusCodePage()
        {
            InitializeComponent();
        }

        void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
        {

        }

        void OnButtonClicked(object sender, EventArgs args)
        {

        }
    }
}
```

Te programy obsługi zdarzeń jest konieczne jest publiczny.

W pliku XAML `Slider` i `Button` tagi konieczne zawiera atrybuty `ValueChanged` i `Clicked` zdarzenia, które odwołują się te programy obsługi zdarzeń:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.XamlPlusCodePage"
             Title="XAML + Code Page">
    <StackLayout>
        <Slider VerticalOptions="CenterAndExpand"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="A simple Label"
               Font="Large"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <Button Text="Click Me!"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                Clicked="OnButtonClicked" />
    </StackLayout>
</ContentPage>
```

Należy zauważyć, że przypisanie obsługi na zdarzenie ma takiej samej składni jak przypisywanie wartości do właściwości.

Jeśli program obsługi dla `ValueChanged` zdarzenie `Slider` będą używać `Label` Aby wyświetlić bieżącą wartość, program obsługi ma odwołuje się do tego obiektu z kodu. `Label` Musi nazwę, która zostanie określony z `x:Name` atrybutu.

```xaml
<Label x:Name="valueLabel"
       Text="A simple Label"
       Font="Large"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand" />
```

`x` Prefiks `x:Name` atrybut wskazuje, że dany atrybut jest wewnętrzna w języku XAML.

Nazwa zostanie przypisana do `x:Name` atrybut ma te same zasady stosowania jako nazwy zmiennych C#. Na przykład musi rozpoczynać się od litery lub podkreślenia i zawierać nie spacji osadzonych.

Teraz `ValueChanged` może ustawić programu obsługi zdarzeń `Label` do wyświetlenia nowego `Slider` wartość. Nowa wartość jest dostępna w argumentach zdarzenia:

```csharp
void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
{
    valueLabel.Text = args.NewValue.ToString("F3");
}
```

Lub program obsługi może uzyskać `Slider` obiekt, który generuje to zdarzenie z `sender` argumentu i uzyskiwanie `Value` właściwości niż:

```csharp
void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
{
    valueLabel.Text = ((Slider)sender).Value.ToString("F3");
}
```

Przy pierwszym uruchomieniu programu, `Label` nie są wyświetlane `Slider` wartości, ponieważ `ValueChanged` zdarzenie jeszcze nie wyzwolone. Ale manipulowania `Slider` powoduje, że wartość do wyświetlenia:

[ ![](get-started-with-xaml-images/xamlpluscode2.png "Wartość suwaka wyświetlane")](get-started-with-xaml-images/xamlpluscode2-large.png "wyświetlana wartość suwaka")

Teraz dla `Button`. Załóżmy symulować odpowiedzi na `Clicked` zdarzeń za pomocą wyświetlania alertu o `Text` przycisku. Program obsługi zdarzeń można bezpiecznie rzutowany `sender` argument `Button` i uzyskuje dostęp do jego właściwości:

```csharp
async void OnButtonClicked(object sender, EventArgs args)
{
    Button button = (Button)sender;
    await DisplayAlert("Clicked!",
        "The button labeled '" + button.Text + "' has been clicked",
        "OK");
}
```

Metoda jest zdefiniowany jako `async` ponieważ `DisplayAlert` metoda jest asynchroniczne i powinno być poprzedzone znakiem `await` operatora, który zwraca po zakończeniu metody. Ponieważ ta metoda uzyskuje `Button` wyzwoleniem zdarzenia z `sender` argumentu, można użyć tej procedury obsługi wielu przycisków.

W tym samouczku czy obiekt zdefiniowany w języku XAML mogą wyzwalać zdarzenia jest obsługiwany w pliku CodeBehind i że plik CodeBehind dostęp do obiektu zdefiniowany w języku XAML, przy użyciu nazwy przypisanej do niego z `x:Name` atrybutu. Są dwa podstawowe sposoby, współpracujące kodu i języka XAML.

Niektóre dodatkowe wgląd w sposób działania XAML może zgromadzone, sprawdzając nowo utworzonego **pliku XamlPlusCode.xaml.g.cs**, który teraz obejmuje dowolną nazwę przypisany do żadnej `x:Name` atrybut jako pole prywatne. Oto uproszczonej wersji tego pliku:

```csharp
public partial class XamlPlusCodePage : ContentPage {

    private Label valueLabel;

    private void InitializeComponent() {
        this.LoadFromXaml(typeof(XamlPlusCodePage));
        valueLabel = this.FindByName<Label>("valueLabel");
    }
}
```

Deklaracja to pole umożliwia zmienną za darmo można używać w dowolnym miejscu w `XamlPlusCodePage` pliku klasy częściowej zgodnie z właściwości. W czasie wykonywania pole jest przypisane po przeanalizowaniu XAML. Oznacza to, że `valueLabel` pole jest `null` podczas `XamlPlusCodePage` Konstruktor rozpoczyna się jednak prawidłowy po `InitializeComponent` jest wywoływana.

Po `InitializeComponent` zwraca sterowania do konstruktora, elementy wizualne strony ma skonstruowane tak jak gdyby miał zostały utworzone i zainicjowane w kodzie. Plik XAML odgrywa już żadnej roli w klasie. Te obiekty na stronie w sposób, który ma, na przykład, dodając widoków można manipulować `StackLayout`, lub ustawienie `Content` właściwości strony do innego elementu całkowicie. Możesz "zapoznać się z drzewa", sprawdzając `Content` właściwość strony i elementy `Children` kolekcje układów. Można ustawić właściwości widoków dostępne w ten sposób lub dynamicznie przypisać procedury obsługi zdarzeń do nich.

Możesz także. Jest stronę, i XAML jest wyłącznie narzędzia do tworzenia jego zawartości.

## <a name="summary"></a>Podsumowanie

Z tym wprowadzenie przedstawiono sposób pliku XAML i pliku kodu przyczyniają się do definicji klasy i interakcje między plików XAML i kodem. Ale XAML ma także własny unikatowy funkcje syntaktycznych umożliwiające do użycia w bardzo elastyczny sposób. Możesz rozpocząć eksplorowanie w [część 2. Składnia podstawowych języka XAML](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md).



## <a name="related-links"></a>Linki pokrewne

- [XamlSamples](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/)
- [Part 2. Część 2. Podstawowa składnia języka XAML](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
- [Part 3. Część 3. Rozszerzenia struktury znaczników XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Part 4. Część 4. Powiązania danych — podstawy](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
- [Part 5. Z danych powiązanie z modelem MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
