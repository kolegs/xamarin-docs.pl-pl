---
title: Podsumowanie rozdział 2. Anatomia aplikacji
description: 'Tworzenie aplikacji mobilnych za pomocą zestawu narzędzi Xamarin.Forms: Podsumowanie rozdział 2. Anatomia aplikacji'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 8764EB7D-8331-4CF7-9BE1-26D0DEE9E0BB
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: d1daceba29e45adf64947c89555cc4e75a850d32
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995280"
---
# <a name="summary-of-chapter-2-anatomy-of-an-app"></a>Podsumowanie rozdział 2. Anatomia aplikacji

W aplikacji platformy Xamarin.Forms, obiekty, które zajmują miejsce na ekranie są znane jako *elementy wizualne*, zhermetyzowany przez [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) klasy. Elementy wizualne, może zostać podzielony na trzy kategorie odpowiadający tych klas:

- [Strona](xref:Xamarin.Forms.Page)
- [Układ](xref:Xamarin.Forms.Layout)
- [Widok](xref:Xamarin.Forms.View)

A `Page` utworów zależnych zajmuje cały ekran lub prawie cały ekran. Często jest elementem podrzędnym strony `Layout` utworów zależnych do organizowania elementów wizualnych podrzędnych. Elementy podrzędne `Layout` mogą być inne `Layout` klasy lub `View` pochodnych (często nazywany *elementy*), które są znane obiekty, takie jak tekst, mapy bitowe, suwaki, przyciski, pola listy i tak dalej.

W tym rozdziale pokazano, jak utworzyć aplikację, skupiając się na [ `Label` ](xref:Xamarin.Forms.Label), czyli `View` utworów zależnych, który wyświetla tekst.

## <a name="say-hello"></a>Zacznij korzystać

Za pomocą platformy Xamarin, zainstalowane można utworzyć nowe rozwiązanie Xamarin.Forms w programie Visual Studio lub Visual Studio dla komputerów Mac. [ **Hello** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello) rozwiązanie używa Portable Class Library dla wspólnego kodu. Pokazuje rozwiązanie Xamarin.Forms tworzone w programie Visual Studio bez żadnych modyfikacji. To rozwiązanie składa się z sześciu projektów (ostatnie dwie z nich nie są tworzone przy użyciu szablonów bieżącego rozwiązania Xamarin.Forms):

- [**Witaj**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello), biblioteki klas przenośnych (PCL) udostępnionych przez inne projekty
- [**Hello.Droid**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.Droid), projekt aplikacji dla systemu Android
- [**Hello.iOS**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.iOS), projekt aplikacji dla systemu iOS
- [**Hello.UWP**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.UWP), projekt aplikacji uniwersalnej platformy Windows (Windows 10 i Windows 10 Mobile)
- [**Hello.Windows**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.Windows), projekt aplikacji dla Windows 8.1
- [**Hello.WinPhone**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.WinPhone), projekt aplikacji dla systemu Windows Phone 8.1

Można wprowadzić dowolne z tych projektów aplikacji projektu startowego, a następnie tworzyć i uruchom program na urządzenie lub symulator.

W wielu programach Xamarin.Forms nie modyfikowanie projektów aplikacji. Pozostają one często niewielki wycinków tylko do uruchamiania programu. Większość zespół będzie wspólne dla wszystkich aplikacji biblioteki klas przenośnych.

## <a name="inside-the-files"></a>Wewnątrz plików

Wizualizacje wyświetlane przez **Hello** programu są zdefiniowane w Konstruktorze typu [ `App` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello/App.cs) klasy. `App` pochodzi od klasy zestawu narzędzi Xamarin.Forms [ `Application` ](xref:Xamarin.Forms.Application).

**Odwołania** części **Hello** projektu PCL obejmuje następujące zestawy Xamarin.Forms:

- **Xamarin.Forms.Core**
- **Xamarin.Forms.Xaml**
- **Xamarin.Forms.Platform**

**Odwołania** sekcje projektów pięć aplikacji zawierają dodatkowe zestawy, które są stosowane do poszczególnych platform:

- **Xamarin.Forms.Platform.Android**
- **Xamarin.Forms.Platform.iOS**
- **Xamarin.Forms.Platform.UWP**
- **Xamarin.Forms.Platform.WinRT**
- **Xamarin.Forms.Platform.WinRT.Tablet**
- **Xamarin.Forms.Platform.WinRT.Phone**

Każdy z projektów aplikacji zawiera wywołanie statycznej `Forms.Init` method in Class metoda `Xamarin.Forms` przestrzeni nazw. Inicjuje to biblioteki rozwiązania Xamarin.Forms. Inna wersja `Forms.Init` jest zdefiniowana dla każdej platformy. Wywołania tej metody można znaleźć w następujących klas:

- iOS: [`AppDelegate`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.iOS/AppDelegate.cs)
- Android: [`MainActivity`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs)
- Platformy uniwersalnej systemu Windows: [ `App` klasy `OnLaunched` — metoda](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs)
- Windows 8.1: [ `App` klasy `OnLaunched` — metoda](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Windows/App.xaml.cs#L65)
- Windows Phone 8.1: [ `App` klasy `OnLaunched` — metoda](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.WinPhone/App.xaml.cs#L67)

Ponadto, trzeba utworzyć wystąpienie każdej z platform `App` klasy lokalizacji w aplikacji PCL. Dzieje się tak w wywołaniu `LoadApplication` w następujące kategorie:

- iOS: [`AppDelegate`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.iOS/AppDelegate.cs)
- Android: [`MainActivity`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs)
- PLATFORMY UNIWERSALNEJ SYSTEMU WINDOWS: [`MainPage`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.UWP/MainPage.xaml.cs)
- Windows 8.1: [`MainPage`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Windows/MainPage.xaml.cs)
- Windows Phone 8.1: [`MainPage`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.WindowsPhone/MainPage.xaml.cs)

W przeciwnym wypadku te projekty aplikacji są normalne programy "nic".

## <a name="pcl-or-sap"></a>PCL lub SAP?

Istnieje możliwość utworzyć rozwiązanie Xamarin.Forms za pomocą wspólnego kodu w bibliotece klas przenośnych (PCL) lub udostępnionych zasobów projektu (SAP). Aby utworzyć rozwiązanie SAP, wybierz opcję udostępnione w programie Visual Studio. [ **HelloSap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/HelloSap) rozwiązanie przedstawia szablon SAP bez żadnych modyfikacji.

Pakiety podejście PCL wszystkie typowe kodu w projekcie biblioteki odwołują się projekty aplikacji platformy. Dzięki podejściu SAP wspólny kod skutecznie istnieje we wszystkich projektach aplikacji platformy i jest współużytkowana przez je.

Większość programistów platformy Xamarin.Forms preferowane podejście PCL. W tym podręczniku większość rozwiązań to PCL. Te, które używają SAP obejmują **Sap** sufiksu w nazwie projektu.

Aby zapewnić obsługę wszystkich platform Xamarin.Forms, wersję platformy .NET, używane przez PCL musi sobie radzić z następujących platform:

- .NET Framework 4.5
- Windows 8
- Windows Phone 8,1
- Xamarin.Android
- Xamarin.iOS
- Xamarin.IOS (wersja klasyczna)

Jest to nazywane 111 profilu komputera.

Dzięki podejściu SAP kodu w projekcie udostępnionym wykonywać inny kod dla różnych platform za pomocą dyrektywy preprocesora C# (`#if`, #`elif`, i `#endif`) za pomocą tych wstępnie zdefiniowane identyfikatory:

- iOS: `__IOS__`
- Android: `__ANDROID__`
- UWP: `WINDOWS_UWP`
- Windows 8.1: `WINDOWS_APP`
- Windows Phone 8.1: `WINDOWS_PHONE_APP`

W aplikacji PCL platformy korzystania w czasie wykonywania, można określić, jak zobaczysz później w tym rozdziale.

## <a name="labels-for-text"></a>Etykiety tekstowe

[ **Greetings** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Greetings) rozwiązanie pokazuje, jak dodać nowy plik C# do **Greetings** projektu. Ten plik definiuje klasę o nazwie `GreetingsPage` który pochodzi od klasy `ContentPage`. W tym podręczniku, większość projekty zawierają jeden `ContentPage` utworów zależnych, którego nazwa jest nazwą projektu z sufiksem `Page` dołączane.

`GreetingsPage` Konstruktor tworzy [ `Label` ](xref:Xamarin.Forms.Label) widoku, który jest widok zestawu narzędzi Xamarin.Forms, który służy do wyświetlania tekstu. [ `Text` ](xref:Xamarin.Forms.Label.Text) Właściwość jest ustawiona na tekst wyświetlany przez `Label`. Ten program ustawia `Label` do `Content` właściwość `ContentPage`. Konstruktor obiektu `App` następnie tworzy wystąpienie klasy `GreetingsPage` i ustawia ją na jego `MainPage` właściwości.

Tekst jest wyświetlany w lewym górnym rogu strony. W systemach iOS oznacza to, że nakłada się pasek stanu strony. Istnieje kilka rozwiązań tego problemu:

### <a name="solution-1-include-padding-on-the-page"></a>Rozwiązanie 1. Potrójna zrzut ekranu przedstawiający greetings programpoziomo i pionowo wyśrodkowane etykietypoziomo i pionowo wyśrodkowane etykiety

Ustaw właściwość [`Padding`](xref:Xamarin.Forms.Page.Padding) na stronie. `Padding` jest typu [`Thickness`](xref:Xamarin.Forms.Thickness), struktura o czterech właściwościach:

- [`Left`](xref:Xamarin.Forms.Thickness.Left)
- [`Top`](xref:Xamarin.Forms.Thickness.Top)
- [`Right`](xref:Xamarin.Forms.Thickness.Right)
- [`Bottom`](xref:Xamarin.Forms.Thickness.Bottom)

`Padding` Można również Wyśrodkuj tekst (lub umieścić go w ośmiu innych lokalizacjach na stronie), ustawiając `Padding`   i    właściwości  do elementu członkowskiego    wyliczenia: Pozwala to `Label` na uniknięcie nadpisania paska stanu iOS.

### <a name="solution-2-include-padding-just-for-ios-sap-only"></a>, co oznacza, po prawej stronie lub u dołu (w zależności od orientacja) Te dwie właściwości są zdefiniowane tylko przez , podczas gdy  i  właściwości są definiowane przez  i dziedziczone przez wszystkie  pochodnych.

Efekty wizualne wydawałyby się podobne, ale są one bardzo różne, tak jak pokazano w następnym rozdziale. Zostało to wykazane w rozwiązaniu [**GreetingsSap**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/GreetingsSap).

### <a name="solution-3-include-padding-just-for-ios-pcl-or-sap"></a>Przykłady rozdział 2 Przykłady rozdział 2 F #

W wersji Xamarin.Forms użytej w książce właściwość `Padding` specyficzną dla iOS w PCL lub SAP można wybrać za pomocą [`Device.OnPlatform`](xref:Xamarin.Forms.Device.OnPlatform(System.Action,System.Action,System.Action,System.Action)) lub [`Device.OnPlatform<T>`](xref:Xamarin.Forms.Device.OnPlatform*) statycznej metody. Te metody są one przestarzałe

`Device.OnPlatform` Metody są używane do uruchomienia kodu specyficznego dla platformy, lub wybierz wartości specyficzne dla platformy. Wewnętrznie, należy używać [ `Device.OS` ](xref:Xamarin.Forms.Device.OS) statycznych właściwości tylko do odczytu, która zwraca element członkowski [ `TargetPlatform` ](xref:Xamarin.Forms.TargetPlatform) wyliczenia:

- [`iOS`](xref:Xamarin.Forms.TargetPlatform.iOS)
- [`Android`](xref:Xamarin.Forms.TargetPlatform.Android)
- [`Windows`](xref:Xamarin.Forms.TargetPlatform.Windows) Windows 8.1, Windows Phone 8.1 i wszystkie urządzenia platformy uniwersalnej systemu Windows.
- [`WinPhone`](xref:Xamarin.Forms.TargetPlatform.WinPhone), wcześniej używany do identyfikowania Windows Phone 8.0, ale jest obecnie nieużywane
- [`Other`](xref:Xamarin.Forms.TargetPlatform.Other) jest używana

`Device.OnPlatform` Metod `Device.OS` właściwości i `TargetPlatform` wyliczenia są wszystkie obecnie przestarzałe. Zamiast tego należy użyć [ `Device.RuntimePlatform` ](xref:Xamarin.Forms.Device.RuntimePlatform) właściwości i porównaj `string` zwracają wartość z pola statyczne:

- [`iOS`](xref:Xamarin.Forms.Device.iOS), ciąg "iOS"
- [`Android`](xref:Xamarin.Forms.Device.Android), ciąg "Android"
- [`UWP`](xref:Xamarin.Forms.Device.UWP), ciąg "UWP" odwołujące się do platformy Windows Runtime
- `Windows`, ciąg "Windows" dla środowiska uruchomieniowego Windows (Windows 8.1 i Windows Phone 8.1 przestarzałe)
- `WinPhone`, ciąg "WinPhone" dla Windows Phone 8.0 (przestarzałe)

[ `Device.Idiom` ](xref:Xamarin.Forms.Device.Idiom) Dotyczy statycznych właściwości tylko do odczytu. Spowoduje to zwrócenie członkiem [ `TargetIdiom` ](xref:Xamarin.Forms.TargetIdiom), która zawiera te elementy członkowskie:

- [`Desktop`](xref:Xamarin.Forms.TargetIdiom.Desktop)
- [`Tablet`](xref:Xamarin.Forms.TargetIdiom.Tablet)
- [`Phone`](xref:Xamarin.Forms.TargetIdiom.Phone)
- [`Unsupported`](xref:Xamarin.Forms.TargetIdiom.Unsupported) jest używana

Dla systemów iOS i Android, odcięcia między `Tablet` i `Phone` szerokość pionowa, 600 jednostek. Dla platformy Windows `Desktop` wskazuje aplikacji platformy uniwersalnej systemu Windows, działających w ramach systemu Windows 10 `Tablet` jest programem Windows 8.1 i `Phone` wskazuje aplikacji platformy uniwersalnej systemu Windows, działających w ramach systemu Windows 10 lub aplikację Windows Phone 8.1.

## <a name="solution-3a-set-margin-on-the-label"></a>3a rozwiązania. Ustaw marginesu na etykiecie

[ `Margin` ](xref:Xamarin.Forms.View.Margin) Właściwość wprowadzono za późno, mają zostać uwzględnione w książce, ale jest również typu `Thickness` i ustaw go na `Label` do definiowania obszar poza widoku, który znajduje się w obliczeniach Układ widoku.

`Padding` Właściwość jest dostępna tylko na [ `Layout` ](xref:Xamarin.Forms.Layout) i [ `Page` ](xref:Xamarin.Forms.Page) pochodnych. `Margin` Właściwość jest dostępna we wszystkich [ `View` ](xref:Xamarin.Forms.View) pochodnych.

## <a name="solution-4-center-the-label-within-the-page"></a>Rozwiązanie 4. Wyśrodkuj etykietę na stronie

Można wyśrodkować `Label` w ramach `Page` (lub umieścić go w jednym z ośmiu innych miejscach), ustawiając [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) i [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) właściwości `Label` do wartości typu [ `LayoutOptions` ](xref:Xamarin.Forms.LayoutOptions). `LayoutOptions` Struktury definiuje dwie właściwości:

- [ `Alignment` ](xref:Xamarin.Forms.LayoutOptions.Alignment) Właściwości typu [ `LayoutAlignment` ](xref:Xamarin.Forms.LayoutAlignment), wyliczenie z cztery elementy członkowskie: [ `Start` ](xref:Xamarin.Forms.LayoutAlignment.Start), co oznacza, że po lewej stronie lub górnej w zależności od Orientacja [ `Center` ](xref:Xamarin.Forms.LayoutAlignment.Center), [ `End` ](xref:Xamarin.Forms.LayoutAlignment.End), oznacza to po prawej stronie lub u dołu, w zależności od orientacji, i [ `Fill` ](xref:Xamarin.Forms.LayoutAlignment.Fill).

- [ `Expands` ](xref:Xamarin.Forms.LayoutOptions.Expands) Właściwości typu `bool`.

Zwykle te właściwości nie są używane bezpośrednio. Zamiast tego kombinacji te dwie właściwości są dostarczane przez osiem statycznych właściwości tylko do odczytu typu `LayoutOptions`:

- [`LayoutOptions.Start`](xref:Xamarin.Forms.LayoutOptions.Start)
- [`LayoutOptions.Center`](xref:Xamarin.Forms.LayoutOptions.Center)
- [`LayoutOptions.End`](xref:Xamarin.Forms.LayoutOptions.End)
- [`LayoutOptions.Fill`](xref:Xamarin.Forms.LayoutOptions.Fill)
- [`LayoutOptions.StartAndExpand`](xref:Xamarin.Forms.LayoutOptions.StartAndExpand)
- [`LayoutOptions.CenterAndExpand`](xref:Xamarin.Forms.LayoutOptions.CenterAndExpand)
- [`LayoutOptions.EndAndExpand`](xref:Xamarin.Forms.LayoutOptions.EndAndExpand)
- [`LayoutOptions.FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand)

`HorizontalOptions` i `VerticalOptions` są najważniejsze właściwości w układzie platformy Xamarin.Forms i zostały omówione bardziej szczegółowo w [ **rozdział 4. Przewijanie stosu**](chapter04.md).

Poniżej przedstawiono wynik z `HorizontalOptions` i `VerticalOptions` właściwości `Label` jednocześnie ustawionych na `LayoutOptions.Center`:

[![Potrójna zrzut ekranu przedstawiający greetings program](images/ch02fg05-small.png "poziomo i pionowo wyśrodkowane etykiety")](images/ch02fg05-large.png#lightbox "poziomo i pionowo wyśrodkowane etykiety")

## <a name="solution-5-center-the-text-within-the-label"></a>Rozwiązanie 5. Wyśrodkowanie tekstu w etykiecie

Można również Wyśrodkuj tekst (lub umieścić go w ośmiu innych lokalizacjach na stronie), ustawiając [ `HorizontalTextAlignment` ](xref:Xamarin.Forms.Label.HorizontalTextAlignment) i [ `VerticalTextAlignment` ](xref:Xamarin.Forms.Label.VerticalTextAlignment) właściwości `Label` do elementu członkowskiego [ `TextAlignment` ](xref:Xamarin.Forms.TextAlignment) wyliczenia:

- [`Start`](xref:Xamarin.Forms.TextAlignment.Start), znaczenie po lewej stronie lub górnej (w zależności od orientacja)
- [`Center`](xref:Xamarin.Forms.TextAlignment.Center)
- [`End`](xref:Xamarin.Forms.TextAlignment.End), co oznacza, po prawej stronie lub u dołu (w zależności od orientacja)

Te dwie właściwości są zdefiniowane tylko przez `Label`, podczas gdy `HorizontalAlignment` i `VerticalAlignment` właściwości są definiowane przez `View` i dziedziczone przez wszystkie `View` pochodnych. Efekty wizualne wydawałyby się podobne, ale są one bardzo różne, tak jak pokazano w następnym rozdziale.



## <a name="related-links"></a>Linki pokrewne

- [Pełny tekst rozdział 2 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch02-Apr2016.pdf)
- [Przykłady rozdział 2](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02)
- [Przykłady rozdział 2 F #](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/FS)
- [Wprowadzenie do zestawu narzędzi Xamarin.Forms](~/xamarin-forms/get-started/index.md)
