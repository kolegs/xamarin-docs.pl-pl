---
title: Podsumowanie rozdział 2. Struktura aplikacji
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 8764EB7D-8331-4CF7-9BE1-26D0DEE9E0BB
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: c206b124349614db7249609707bd22e8a4efe6d8
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="summary-of-chapter-2-anatomy-of-an-app"></a>Podsumowanie rozdział 2. Struktura aplikacji

W aplikacji platformy Xamarin.Forms obiektów, które zajmują miejsce na ekranie są określane jako *elementy wizualne*, hermetyzowany przez [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/) klasy. Elementy wizualne może zostać podzielony na trzy kategorie odpowiadający te klasy:

- [Strona](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)
- [Układ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/)
- [Widok](https://developer.xamarin.com/api/type/Xamarin.Forms.View/)

A `Page` zależnych zajmuje cały ekran lub prawie cały ekran. Często jest elementem podrzędnym strony `Layout` zależnych, aby zorganizować podrzędnego elementów wizualnych. Elementy podrzędne `Layout` mogą być inne `Layout` klasy lub `View` pochodnych (często nazywane *elementy*), które są znane obiekty, takie jak tekstu, mapy bitowe, suwaki, przyciski, pola listy i tak dalej.

W tym rozdziale pokazano, jak utworzyć aplikację koncentrujące się na [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/), która jest `View` pochodnej, która służy do wyświetlania tekstu.

## <a name="say-hello"></a>Zacznij korzystać

Z platformą Xamarin zainstalowany możesz utworzyć nowe rozwiązanie platformy Xamarin.Forms w Visual Studio lub Visual Studio dla komputerów Mac. [ **Hello** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello) rozwiązanie używa przenośnej biblioteki klas dla typowych kodu. Jednak przedstawia rozwiązanie platformy Xamarin.Forms utworzone w programie Visual Studio bez żadnych modyfikacji. Rozwiązania składa się z sześciu projektów (ostatnie dwa z nich nie są tworzone z bieżącym szablony rozwiązań platformy Xamarin.Forms):

- [**Witaj**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello), przenośnej biblioteki klas (PCL) współużytkowany przez innych projektów
- [**Hello.Droid**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.Droid), projekt aplikacji dla systemu Android
- [**Hello.iOS**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.iOS), projekt aplikacji dla systemu iOS
- [**Hello.UWP**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.UWP), projekt aplikacji platformy uniwersalnej systemu Windows (Windows 10 i Windows 10 Mobile)
- [**Hello.Windows**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.Windows), projekt aplikacji dla Windows 8.1
- [**Hello.WinPhone**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.WinPhone), projekt aplikacji dla systemu Windows Phone 8.1

Można wprowadzić dowolne z tych projektów aplikacji projekt startowy, a następnie utworzyć i uruchomić program na urządzenie lub symulator.

W wielu programach platformy Xamarin.Forms nie modyfikuje projektów aplikacji. Pozostają one często niewielki rozmiar klas zastępczych tylko w celu uruchomienia programu. Większość zespół będzie przenośnej biblioteki klas wspólne dla wszystkich aplikacji.

## <a name="inside-the-files"></a>Wewnątrz plików

Elementy wizualne, wyświetlane przez **Hello** programu są zdefiniowane w Konstruktorze [ `App` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello/App.cs) klasy. `App` pochodzi od klasy platformy Xamarin.Forms [ `Application` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Application/).

**Odwołania** sekcji **Hello** PCL projekt zawiera następujące zestawy platformy Xamarin.Forms:

- **Xamarin.Forms.Core**
- **Xamarin.Forms.Xaml**
- **Xamarin.Forms.Platform**

**Odwołania** sekcje projektów pięć aplikacji obejmują dodatkowe zestawy, które dotyczą poszczególnych platform:

- **Xamarin.Forms.Platform.Android**
- **Xamarin.Forms.Platform.iOS**
- **Xamarin.Forms.Platform.UWP**
- **Xamarin.Forms.Platform.WinRT**
- **Xamarin.Forms.Platform.WinRT.Tablet**
- **Xamarin.Forms.Platform.WinRT.Phone**

Każdy z projektów aplikacji zawiera wywołanie statycznych `Forms.Init` metody w `Xamarin.Forms` przestrzeni nazw. Powoduje to inicjowanie biblioteki platformy Xamarin.Forms. Inna wersja `Forms.Init` jest zdefiniowany dla każdej platformy. Wywołania do tej metody można znaleźć w następujących klas:

- iOS: [`AppDelegate`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.iOS/AppDelegate.cs)
- System android: [`MainActivity`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs)
- Platformy uniwersalnej systemu Windows: [ `App` klasy `OnLaunched` — metoda](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs)
- Windows 8.1: [ `App` klasy `OnLaunched` — metoda](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Windows/App.xaml.cs#L65)
- Windows Phone 8.1: [ `App` klasy `OnLaunched` — metoda](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.WinPhone/App.xaml.cs#L67)

Ponadto należy utworzyć wystąpienia każdej z platform `App` klasy lokalizacji w PCL. Dzieje się tak w wywołaniu `LoadApplication` w następujących klas:

- iOS: [`AppDelegate`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.iOS/AppDelegate.cs)
- System android: [`MainActivity`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs)
- UWP: [`MainPage`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.UWP/MainPage.xaml.cs)
- Windows 8.1: [`MainPage`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Windows/MainPage.xaml.cs)
- Windows Phone 8.1: [`MainPage`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.WindowsPhone/MainPage.xaml.cs)

W przeciwnym wypadku te projekty aplikacji są normalne programy "nic nie rób".

## <a name="pcl-or-sap"></a>PCL lub SAP?

Istnieje możliwość utworzenia rozwiązania platformy Xamarin.Forms wspólnej kodu z przenośnej biblioteki klas (PCL) lub udostępnionego projektu zasobów (SAP). Tworzenie rozwiązań programu SAP, wybierz opcję udostępnione w programie Visual Studio. [ **HelloSap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/HelloSap) rozwiązanie przedstawia szablonu SAP bez żadnych modyfikacji.

Pakiety podejście PCL wszystkie typowe kodu w projektach biblioteki odwołuje się projekty aplikacji platformy. Z podejścia SAP typowy kod skutecznie istnieje we wszystkich projektach aplikacji platformy i udostępniony między nimi.

Większość deweloperów platformy Xamarin.Forms preferowane podejście PCL. W tym książki większość rozwiązań jest PCL. Używające SAP obejmują **Sap** sufiksu w nazwie projektu.

Aby zapewnić obsługę wszystkich platform platformy Xamarin.Forms, wersji platformy .NET używane przez PCL należy uwzględnić następujące platformy:

- .NET Framework 4.5
- Windows 8
- Windows Phone 8,1
- Xamarin.Android
- Xamarin.iOS
- Xamarin.IOS (Classic)

Jest to nazywane 111 profil komputera.

Z podejścia SAP kodu w projekcie udostępnionym wykonywać różnego kodu dla różnych platform za pomocą dyrektywy preprocesora C# (`#if`, #`elif`, i `#endif`) z tych wstępnie zdefiniowane identyfikatory:

- iOS: `__IOS__`
- Android: `__ANDROID__`
- UWP: `WINDOWS_UWP`
- Windows 8.1: `WINDOWS_APP`
- Windows Phone 8.1: `WINDOWS_PHONE_APP`

W PCL platformy pracujesz w czasie wykonywania, można określić, jak można zauważyć dalej w tym rozdziale.

## <a name="labels-for-text"></a>Tekst etykiety

[ **Pozdrowienia** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Greetings) rozwiązanie przedstawia sposób dodawania nowego pliku C# do **pozdrowienia** projektu. Ten plik definiuje klasę o nazwie `GreetingsPage` która pochodzi z `ContentPage`. Tego podręcznika większości projektów zawierać pojedynczy `ContentPage` zależnych, którego nazwa jest nazwą projektu z sufiksem `Page` dołączane.

`GreetingsPage` Tworzy konstruktora [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) widoku, który jest widok platformy Xamarin.Forms, który służy do wyświetlania tekstu. [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) Właściwość jest ustawiona na tekst wyświetlany przez `Label`. Ten program ustawia `Label` do `Content` właściwość `ContentPage`. Konstruktor obiektu `App` następnie tworzy wystąpienie klasy `GreetingsPage` i ustawia ją na jego `MainPage` właściwości.

Tekst jest wyświetlany w lewym górnym rogu strony. W systemach iOS oznacza to, że nakłada na pasku stanu strony. Istnieje kilka rozwiązania tego problemu:

### <a name="solution-1-include-padding-on-the-page"></a>Rozwiązanie 1. Obejmują dopełnienie na stronie

Ustaw [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Padding/) właściwości na stronie. `Padding` Typ jest [ `Thickness` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/), struktura z czterech właściwości:

- [`Left`](https://developer.xamarin.com/api/property/Xamarin.Forms.Thickness.Left/)
- [`Top`](https://developer.xamarin.com/api/property/Xamarin.Forms.Thickness.Top/)
- [`Right`](https://developer.xamarin.com/api/property/Xamarin.Forms.Thickness.Right/)
- [`Bottom`](https://developer.xamarin.com/api/property/Xamarin.Forms.Thickness.Bottom/)

`Padding` definiuje obszar wewnątrz strony, gdy zawartość jest wyłączone. Dzięki temu `Label` Aby uniknąć zastępowania pasek stanu systemu iOS.

### <a name="solution-2-include-padding-just-for-ios-sap-only"></a>Rozwiązanie 2. Obejmują dopełnienie tylko dla systemu iOS (tylko SAP)

Ustaw właściwość "Padding" tylko w systemie iOS przy użyciu SAP z dyrektywy preprocesora C#. To jest przedstawiona w [ **GreetingsSap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/GreetingsSap) rozwiązania.

### <a name="solution-3-include-padding-just-for-ios-pcl-or-sap"></a>Rozwiązanie 3. Obejmują dopełnienie tylko dla systemu iOS (PCL lub SAP)

W wersji platformy Xamarin.Forms używane książki `Padding` można wybrać właściwości specyficzne dla systemu iOS w PCL lub SAP przy użyciu [ `Device.OnPlatform` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.OnPlatform/p/System.Action/System.Action/System.Action/System.Action/) lub [ `Device.OnPlatform<T>` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.OnPlatform%7BT%7D/p/T/T/T/) metody statycznej. Te metody są obecnie przestarzałe.

`Device.OnPlatform` Metody są używane do uruchomienia kodu specyficzne dla platformy, lub wybierz wartości, specyficzne dla platformy. Wewnętrznie, należy korzystać z [ `Device.OS` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Device.OS/) statycznej właściwości tylko do odczytu, która zwraca element członkowski [ `TargetPlatform` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TargetPlatform/) wyliczenie:

- [`iOS`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.iOS/)
- [`Android`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.Android/)
- [`Windows`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.Windows/) Windows 8.1, Windows Phone 8.1 i wszystkie urządzenia platformy uniwersalnej systemu Windows.
- [`WinPhone`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.WinPhone/), wcześniej używany do identyfikowania Windows Phone 8.0, ale jest teraz nieużywane
- [`Other`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.Other/) nie jest używana

`Device.OnPlatform` Metod, `Device.OS` właściwości oraz `TargetPlatform` wyliczenia są wszystkie obecnie przestarzałe. Zamiast tego należy użyć [ `Device.RuntimePlatform` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Device.RuntimePlatform/) właściwości i porównaj `string` zwrócić wartość z pola statycznego:

- [`iOS`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.iOS/), ciąg "iOS" 
- [`Android`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.Android/), ciąg "Android"
- [`UWP`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.UWP/), ciąg "Platformy UWP" odwołujący się do platformy środowiska wykonawczego systemu Windows
- [`Windows`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.Windows/), ciąg "Windows" dla środowiska uruchomieniowego systemu Windows (Windows 8.1 i Windows Phone 8.1)
- [`WinPhone`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.WinPhone/), ciąg "Systemu WinPhone" dla systemu Windows Phone 8.0 

[ `Device.Idiom` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Device.Idiom/) Dotyczy statycznej właściwości tylko do odczytu. Zwraca ten element członkowski [ `TargetIdiom` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TargetIdiom/), która zawiera te elementy członkowskie:

- [`Desktop`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetIdiom.Desktop/)
- [`Tablet`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetIdiom.Tablet/)
- [`Phone`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetIdiom.Phone/)
- [`Unsupported`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetIdiom.Unsupported/) nie jest używana

Systemy iOS i Android, odcięcia między `Tablet` i `Phone` jest pionowy szerokości 600 jednostki. Na platformie Windows `Desktop` wskazuje aplikacji platformy uniwersalnej systemu Windows, Windows 10 do uruchamiania `Tablet` jest programem Windows 8.1 i `Phone` wskazuje aplikacji platformy uniwersalnej systemu Windows, do uruchamiania systemu Windows 10 lub aplikacji Windows Phone 8.1.

## <a name="solution-3a-set-margin-on-the-label"></a>3a rozwiązania. Ustaw margines na etykiecie

[ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) Właściwość wprowadzono za późno do uwzględnienia w książce, ale jest również typu `Thickness` i można ją ustawić na `Label` do definiowania obszar poza widoku, który znajduje się w obliczeniach Układ widoku.

`Padding` Właściwość jest dostępna tylko na [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/) i [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) pochodnych. `Margin` Właściwość jest dostępna we wszystkich [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) pochodnych.

## <a name="solution-4-center-the-label-within-the-page"></a>Rozwiązanie 4. Etykieta strony Centrum

Można wyśrodkować `Label` w `Page` (lub umieść ją w jednym z innych osiem miejsc) przez ustawienie [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) i [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) właściwości `Label` wartość typu [ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/). `LayoutOptions` Struktury definiuje dwie właściwości:

- [ `Alignment` ](https://developer.xamarin.com/api/property/Xamarin.Forms.LayoutOptions.Alignment/) Właściwości typu [ `LayoutAlignment` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutAlignment/), wyliczenie z czterech członków: [ `Start` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutAlignment.Start/), co oznacza, że lewej lub górnej w zależności od Orientacja [ `Center` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutAlignment.Center/), [ `End` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutAlignment.End/), co oznacza right lub bottom w zależności od orientacji, i [ `Fill` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutAlignment.Fill/).

- [ `Expands` ](https://developer.xamarin.com/api/property/Xamarin.Forms.LayoutOptions.Expands/) Właściwości typu `bool`.

Zwykle te właściwości nie są używane bezpośrednio. Zamiast tego kombinacji te dwie właściwości są dostarczane przez osiem statycznej właściwości tylko do odczytu typu `LayoutOptions`:

- [`LayoutOptions.Start`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Start/)
- [`LayoutOptions.Center`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Center/)
- [`LayoutOptions.End`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.End/)
- [`LayoutOptions.Fill`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Fill/)
- [`LayoutOptions.StartAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.StartAndExpand/)
- [`LayoutOptions.CenterAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.CenterAndExpand/)
- [`LayoutOptions.EndAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.EndAndExpand/)
- [`LayoutOptions.FillAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.FillAndExpand/)

`HorizontalOptions` i `VerticalOptions` są najważniejsze właściwości w układzie platformy Xamarin.Forms i omówiono bardziej szczegółowo w [ **rozdział 4. Przewijanie stosu**](chapter04.md).

Poniżej przedstawiono wyniki `HorizontalOptions` i `VerticalOptions` właściwości `Label` jednocześnie ustawionych na `LayoutOptions.Center`:

[![Potrójna zrzut ekranu przedstawiający program pozdrowienia](images/ch02fg05-small.png "poziomie i w pionie etykiet wyśrodkowane")](images/ch02fg05-large.png#lightbox "poziomie i w pionie etykiet wyśrodkowane")

## <a name="solution-5-center-the-text-within-the-label"></a>Rozwiązanie 5. Centrum tekstu w etykiecie

Możesz też Centrum tekst (i umieścić go w ośmiu innych lokalizacji na stronie) przez ustawienie [ `HorizontalTextAlignment` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.HorizontalTextAlignment/) i [ `VerticalTextAlignment` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.VerticalTextAlignment/) właściwości `Label` do elementu członkowskiego [ `TextAlignment` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TextAlignment/) wyliczenie:

- [`Start`](https://developer.xamarin.com/api/field/Xamarin.Forms.TextAlignment.Start/), znaczenie left lub top (w zależności od orientacji)
- [`Center`](https://developer.xamarin.com/api/field/Xamarin.Forms.TextAlignment.Center/)
- [`End`](https://developer.xamarin.com/api/field/Xamarin.Forms.TextAlignment.End/), co oznacza prawej lub dolnej (w zależności od orientacji)

Te dwie właściwości są definiowane tylko przez `Label`, podczas gdy `HorizontalAlignment` i `VerticalAlignment` właściwości są definiowane przez `View` i dziedziczone przez wszystkie `View` pochodnych. Efekty wizualne mogą wydawać się podobnie, ale są one bardzo różnią się, jak dalej rozdziale przedstawiono.



## <a name="related-links"></a>Linki pokrewne

- [Pełny tekst rozdział 2 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch03-Apr2016.pdf)
- [Przykłady rozdział 2](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03)
- [Przykłady rozdział 2 F #](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/FS)
- [Wprowadzenie do platformy Xamarin.Forms](~/xamarin-forms/get-started/index.md)
