---
title: Podsumowanie rozdziałów 27. Niestandardowe programy renderujące
description: 'Tworzenie aplikacji mobilnych za pomocą zestawu narzędzi Xamarin.Forms: Podsumowanie działu 27. Niestandardowe programy renderujące'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 49961953-9336-4FD4-A42F-6D9B05FF52E7
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 0497770909b33108eaac0fa5044e98febeb61763
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996311"
---
# <a name="summary-of-chapter-27-custom-renderers"></a>Podsumowanie rozdziałów 27. Niestandardowe programy renderujące

Element zestawu narzędzi Xamarin.Forms, takie jak `Button` renderowania za pomocą przycisku specyficzne dla platformy, zhermetyzowanych w ramach klasę o nazwie `ButtonRenderer`.  Oto [wersję dla systemu iOS `ButtonRenderer` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.iOS/Renderers/ButtonRenderer.cs), [wersji dla systemu Android `ButtonRenderer` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.Android/Renderers/ButtonRenderer.cs)i [wersję środowiska uruchomieniowego Windows `ButtonRenderer` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.WinRT/ButtonRenderer.cs).

W tym rozdziale omówiono, w jaki sposób można napisać własne programy renderujące, aby utworzyć niestandardowe widoki, które mapują do obiektów specyficznych dla platformy.

## <a name="the-complete-class-hierarchy"></a>Hierarchia klas ukończone

Istnieje siedem zestawów, które zawierają kod specyficzny dla platformy Xamarin.Forms.
Źródła można wyświetlić w witrynie GitHub przy użyciu poniższych linków:

- [**Xamarin.Forms.Platform** ](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform) (bardzo małe)
- [**Xamarin.Forms.Platform.iOS**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.iOS)
- [**Xamarin.Forms.Platform.Android**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.Android)
- [**Xamarin.Forms.Platform.WinRT** ](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.WinRT) (większe niż trzy dalej)
- [**Xamarin.Forms.Platform.UAP**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.UAP)
- [**Xamarin.Forms.Platform.WinRT.Tablet**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.WinRT.Tablet)
- [**Xamarin.Forms.Platform.WinRT.Phone**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.WinRT.Phone)

[ **PlatformClassHierarchy** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/PlatformClassHierarchy) przykładzie wyświetlono hierarchii klas dla zestawów, które są prawidłowe dla wykonywanie platformy.

Można zauważyć ważne klasę o nazwie `ViewRenderer`. Jest to klasa, pochodzić od, tworząc renderowania specyficzne dla platformy. Istnieje ona w trzech różnych wersji, ponieważ jest powiązany do systemu widoku platformy docelowej:

IOS [ `ViewRenderer<TView, TNativeView>` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.iOS/ViewRenderer.cs#L26) ma argumenty ogólne:

- `TView` ograniczone do [`Xamarin.Forms.View`](xref:Xamarin.Forms.View)
- `TNativeView` ograniczone do [`UIKit.UIView`](https://developer.xamarin.com/api/type/UIKit.UIView/)

Android [ `ViewRenderer<TView, TNativeView>` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.Android/ViewRenderer.cs#L14) ma argumenty ogólne:

- `TView` ograniczone do [`Xamarin.Forms.View`](xref:Xamarin.Forms.View)
- `TNativeView` ograniczone do [`Android.Views.View`](https://developer.xamarin.com/api/type/Android.Views.View/)

Środowisko wykonawcze Windows [ `ViewRenderer<TElement, TNativeElement>` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.WinRT/ViewRenderer.cs#L12) ma inaczej nazwane argumenty ogólne:

- `TElement` ograniczone do [`Xamarin.Forms.View`](xref:Xamarin.Forms.View)
- `TNativeElement` ograniczone do [`Windows.UI.Xaml.FrameworkElement`](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.aspx)

Podczas pisania programu renderującego, użytkownik będzie wyprowadzanie klasy z `View`, a następnie ich zapisywanie wielu `ViewRenderer` klasy, jeden dla każdej z obsługiwanych platform. Każda implementacja specyficzne dla platformy będzie odwoływać się do macierzystych klasę, która pochodzi od typu, można określić jako `TNativeView` lub `TNativeElement` parametru.

## <a name="hello-custom-renderers"></a>Witaj, niestandardowe programy renderujące!

[ **HelloRenderers** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/HelloRenderers) program odwołuje się widok niestandardowy o nazwie `HelloView` w jego [ `App` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers/App.cs) klasy.

[ `HelloView` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers/HelloView.cs) Klasa znajduje się w **HelloRenderers** projektu, a po prostu pochodzi od klasy `View`.

[ `HelloViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers.iOS/HelloViewRenderer.cs) Klasy w **HelloRenderers.iOS** projektu pochodzi od klasy `ViewRenderer<HelloView, UILabel>`. W `OnElementChanged` zastąpienia, tworzy natywnych dla systemów iOS `UILabel` i wywołania `SetNativeControl`.

[ `HelloViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers.Droid/HelloViewRenderer.cs) Klasy w **HelloRenderers.Droid** projektu pochodzi od klasy `ViewRenderer<HelloView, TextView>`. W `OnElementChanged` zastąpienia, tworzy aplikację Android `TextView` i wywołania `SetNativeControl`.

[ `HelloViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers.UWP/HelloViewRenderer.cs) Klasy w **HelloRenderers.UWP** i innych projektów Windows pochodzi od klasy `ViewRenderer<HelloView, TextBlock>`. W `OnElementChanged` zastąpienia, tworzy on Windows `TextBlock` i wywołania `SetNativeControl`.

Wszystkie `ViewRenderer` zawierają pochodne `ExportRenderer` atrybut na poziomie zestawu, który kojarzy `HelloView` klasy przy użyciu określonych `HelloViewRenderer` klasy. Jest to, jak Xamarin.Forms lokalizuje renderowania w poszczególnych platform projektów:

[![Potrójna zrzut ekranu przedstawiający widok Hello](images/ch27fg02-small.png "niestandardowe programy renderujące")](images/ch27fg02-large.png#lightbox "niestandardowe programy renderujące")

## <a name="renderers-and-properties"></a>Właściwości i programy renderujące

Kolejny zbiór programy renderujące implementuje Rysowanie elipsy i znajduje się w różnych projektów z [ **Xamarin.FormsBook.Platform** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform) rozwiązania.

[ `EllipseView` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/EllipseView.cs) Klasa się zebrała **Xamarin.FormsBook.Platform** platformy. Klasa jest podobna do `BoxView` i definiuje jedną właściwość: `Color` typu `Color`.

Programy renderujące, mogą przesyłać wartości właściwości ustawione na `View` do natywnego obiektu przez zastąpienie `OnElementPropertyChanged` metody w module renderowania. W ramach tej metody (i w większości programu renderującego) dostępne są dwie właściwości:

- `Element`, element zestawu narzędzi Xamarin.Forms
- `Control`, natywne widoku lub obiekt elementu widget lub kontrolki

Typy te właściwości są określane przez parametrów ogólnych do `ViewRenderer`. W tym przykładzie `Element` typu `EllipseView`.

`OnElementPropertyChanged` Zastąpienie w związku z tym można przetransferować `Color` wartość `Element` do natywnych `Control` obiektu, prawdopodobnie z pewnego rodzaju konwersji. Trzy programy renderujące są następujące:

- dla systemu iOS: [ `EllipseViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/EllipseViewRenderer.cs), który używa [ `EllipseUIView` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/EllipseUIView.cs) klasy elipsy.
- Android: [ `EllipseViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/EllipseViewRenderer.cs), który używa [ `EllipseDrawableView` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/EllipseDrawableView.cs) klasy elipsy.
- Środowisko wykonawcze Windows: [ `EllipseViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/EllipseViewRenderer.cs), którego można użyć natywny Windows [ `Ellipse` ](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.ellipse.aspx) klasy.

[ **EllipseDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/EllipseDemo) klasa wyświetla kilka z nich `EllipseView` obiektów:

[![Potrójna zrzut ekranu przedstawiający pokaz elipsy](images/ch27fg03-small.png "EllipseView niestandardowe programy renderujące")](images/ch27fg03-large.png#lightbox "EllipseView niestandardowe programy renderujące")

[ **BouncingBall** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/BouncingBall) odbija `EllipseView` wyłączone stronach ekranu.

## <a name="renderers-and-events"></a>Zdarzenia i programy renderujące

Istnieje również możliwość renderowania pośrednio generowanie zdarzeń. [ `StepSlider` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/StepSlider.cs) Klasa jest podobna do normalnego zestawu narzędzi Xamarin.Forms `Slider` , ale umożliwia określenie liczby kroków dyskretnych `Minimum` i `Maximum` wartości.

Trzy programy renderujące są następujące:

- iOS: [`StepSliderRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/StepSliderRenderer.cs)
- Android: [`StepSliderRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/StepSliderRenderer.cs)
- Środowisko wykonawcze Windows: [`StepSliderRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/StepSliderRenderer.cs)

Programy renderujące wykrywał zmiany natywne kontrolki, a następnie wywołaj `SetValueFromRenderer`, które odwołują się do właściwości możliwej do wiązania, zdefiniowane w `StepSlider`, zmiana co powoduje, że `StepSlider` ognia `ValueChanged` zdarzeń.

[ **StepSliderDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/StepSliderDemo) przykład demonstruje ten nowy suwak.



## <a name="related-links"></a>Linki pokrewne

- [Pełny tekst działu 27 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch27-Apr2016.pdf)
- [Przykłady działu 27](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27)
- [Niestandardowe programy renderujące](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
