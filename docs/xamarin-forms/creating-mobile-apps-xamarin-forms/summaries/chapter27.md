---
title: "Podsumowanie działu 27. Niestandardowe moduły renderowania"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 49961953-9336-4FD4-A42F-6D9B05FF52E7
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 6d7c2b17e9596b7d2dd26aaf77cf13f7f8086cd5
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/12/2018
---
# <a name="summary-of-chapter-27-custom-renderers"></a>Podsumowanie działu 27. Niestandardowe moduły renderowania

Element platformy Xamarin.Forms, takich jak `Button` jest odwzorowywany z przycisku specyficzne dla platformy hermetyzowany w klasie o nazwie `ButtonRenderer`.  Oto [wersję systemu iOS `ButtonRenderer` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.iOS/Renderers/ButtonRenderer.cs), [wersji Android `ButtonRenderer` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.Android/Renderers/ButtonRenderer.cs)i [wersji środowiska wykonawczego systemu Windows `ButtonRenderer` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.WinRT/ButtonRenderer.cs).

W tym rozdziale omówiono, jak pisać własne moduły renderowania, aby utworzyć niestandardowe widoki, które mapują na obiekty specyficzne dla platformy.

## <a name="the-complete-class-hierarchy"></a>Hierarchia klas ukończone

Istnieje siedem zestawy, które zawierają kod specyficzne dla platformy platformy Xamarin.Forms.
Źródła można wyświetlić w witrynie GitHub przy użyciu tych łączy:

- [**Xamarin.Forms.Platform** ](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform) (bardzo małe)
- [**Xamarin.Forms.Platform.iOS**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.iOS)
- [**Xamarin.Forms.Platform.Android**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.Android)
- [**Xamarin.Forms.Platform.WinRT** ](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.WinRT) (większe niż dalej trzech)
- [**Xamarin.Forms.Platform.UAP**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.UAP)
- [**Xamarin.Forms.Platform.WinRT.Tablet**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.WinRT.Tablet)
- [**Xamarin.Forms.Platform.WinRT.Phone**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.WinRT.Phone)

[ **PlatformClassHierarchy** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/PlatformClassHierarchy) przykładzie wyświetlono hierarchii klas dla zestawów, które są prawidłowe dla wykonywania platformy.

Można zauważyć ważne klasy o nazwie `ViewRenderer`. Jest to klasa, który pochodzi od, tworząc renderowania specyficzne dla platformy. Istnieje on w trzech różnych wersji, ponieważ jest powiązany systemu widoku platformy docelowej:

IOS [ `ViewRenderer<TView, TNativeView>` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.iOS/ViewRenderer.cs#L26) ma argumenty ogólne:

- `TView` ograniczone do [`Xamarin.Forms.View`](https://developer.xamarin.com/api/type/Xamarin.Forms.View/)
- `TNativeView` ograniczone do [`UIKit.UIView`](https://developer.xamarin.com/api/type/UIKit.UIView/)

Android [ `ViewRenderer<TView, TNativeView>` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.Android/ViewRenderer.cs#L14) ma argumenty ogólne:

- `TView` ograniczone do [`Xamarin.Forms.View`](https://developer.xamarin.com/api/type/Xamarin.Forms.View/)
- `TNativeView` ograniczone do [`Android.Views.View`](https://developer.xamarin.com/api/type/Android.Views.View/)

Środowisko wykonawcze systemu Windows [ `ViewRenderer<TElement, TNativeElement>` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.WinRT/ViewRenderer.cs#L12) inaczej o nazwie argumentów ogólnych:

- `TElement` ograniczone do [`Xamarin.Forms.View`](https://developer.xamarin.com/api/type/Xamarin.Forms.View/)
- `TNativeElement` ograniczone do [`Windows.UI.Xaml.FrameworkElement`](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.aspx)

Podczas zapisywania mechanizm renderujący, użytkownik będzie wyprowadzanie klasy z `View`, a następnie zapisywania wielu `ViewRenderer` klas, po jednej dla każdej z obsługiwanych platform. Każda implementacja specyficzna dla platformy będzie odwoływać się do natywnej klasą pochodzącą z typem określonym jako `TNativeView` lub `TNativeElement` parametru.

## <a name="hello-custom-renderers"></a>Witaj, niestandardowe moduły renderowania!

[ **HelloRenderers** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/HelloRenderers) program odwołuje się widok niestandardowy o nazwie `HelloView` w jego [ `App` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers/App.cs) klasy.

[ `HelloView` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers/HelloView.cs) Klasa znajduje się w **HelloRenderers** projektu i po prostu pochodzi od `View`.

[ `HelloViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers.iOS/HelloViewRenderer.cs) Klasy w **HelloRenderers.iOS** projektu jest pochodną `ViewRenderer<HelloView, UILabel>`. W `OnElementChanged` zastąpienia, tworzy natywnego iOS `UILabel` i wywołania `SetNativeControl`.

[ `HelloViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers.Droid/HelloViewRenderer.cs) Klasy w **HelloRenderers.Droid** projektu jest pochodną `ViewRenderer<HelloView, TextView>`. W `OnElementChanged` zastąpienia, tworzy Android `TextView` i wywołania `SetNativeControl`.

[ `HelloViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers.UWP/HelloViewRenderer.cs) Klasy w **HelloRenderers.UWP** i innych projektów systemu Windows jest pochodną `ViewRenderer<HelloView, TextBlock>`. W `OnElementChanged` zastąpienia, tworzy Windows `TextBlock` i wywołania `SetNativeControl`.

Wszystkie `ViewRenderer` zawiera pochodne `ExportRenderer` atrybut na poziomie zestawu, który kojarzy `HelloView` klasy z danej `HelloViewRenderer` klasy. Jest to, jak platformy Xamarin.Forms lokalizuje renderowania w projektach poszczególnych platform:

[![Potrójna zrzut ekranu przedstawiający widok Hello](images/ch27fg02-small.png "niestandardowe moduły renderowania")](images/ch27fg02-large.png#lightbox "niestandardowe moduły renderowania")

## <a name="renderers-and-properties"></a>Moduły renderowania i właściwości

Następny zestaw renderowania implementuje rysowania elipsy i znajduje się w różnych projektów [ **Xamarin.FormsBook.Platform** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform) rozwiązania.

[ `EllipseView` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/EllipseView.cs) Klasa znajduje się w **Xamarin.FormsBook.Platform** platformy. Klasa jest podobny do `BoxView` i definiuje jedną właściwość: `Color` typu `Color`.

Programy renderujące mogą przekazywać wartości właściwości ustawione na `View` do natywnej obiektu przez zastąpienie `OnElementPropertyChanged` metody w module renderowania. W ramach tej metody (i w większości mechanizm renderujący) dostępne są dwie właściwości:

- `Element`, element platformy Xamarin.Forms
- `Control`, natywnego widoku lub elementu widget lub sterowania w obiekcie

Typy te właściwości są określane przez parametry ogólne do `ViewRenderer`. W tym przykładzie `Element` jest typu `EllipseView`.

`OnElementPropertyChanged` Zastąpienia w związku z tym można przetransferować `Color` wartość `Element` do natywnego `Control` obiektu, prawdopodobnie z niektórych rodzaj konwersji. Są trzy renderowania:

- iOS: [ `EllipseViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/EllipseViewRenderer.cs), który korzysta z [ `EllipseUIView` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/EllipseUIView.cs) klasy elipsy.
- System android: [ `EllipseViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/EllipseViewRenderer.cs), który korzysta z [ `EllipseDrawableView` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/EllipseDrawableView.cs) klasy elipsy.
- Środowisko wykonawcze systemu Windows: [ `EllipseViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/EllipseViewRenderer.cs), którego można użyć natywnego Windows [ `Ellipse` ](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.ellipse.aspx) klasy.

[ **EllipseDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/EllipseDemo) klasa przedstawia niektóre z tych `EllipseView` obiektów:

[![Potrójna zrzut ekranu przedstawiający pokaz elipsy](images/ch27fg03-small.png "renderowania niestandardowe EllipseView")](images/ch27fg03-large.png#lightbox "EllipseView renderowania niestandardowe")

[ **BouncingBall** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/BouncingBall) odrzuceń `EllipseView` wyłączone krawędzi ekranu.

## <a name="renderers-and-events"></a>Moduły renderowania i zdarzenia

Istnieje również możliwość renderowania pośrednio generowanie zdarzeń. [ `StepSlider` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/StepSlider.cs) Klasy jest podobna do normalnej platformy Xamarin.Forms `Slider` , ale zezwala na określanie liczby odrębny kroków pomiędzy `Minimum` i `Maximum` wartości.

Są trzy renderowania:

- iOS: [`StepSliderRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/StepSliderRenderer.cs)
- System android: [`StepSliderRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/StepSliderRenderer.cs)
- Środowisko wykonawcze systemu Windows: [`StepSliderRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/StepSliderRenderer.cs)

Programy renderujące wykrywa zmian do macierzystego formantu, a następnie wywołać `SetValueFromRenderer`, który odwołuje się do właściwości możliwej do wiązania zdefiniowane w `StepSlider`, co powoduje, że zmiana `StepSlider` uruchomienie `ValueChanged` zdarzeń.

[ **StepSliderDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/StepSliderDemo) przykładzie pokazano to nowy suwak.



## <a name="related-links"></a>Linki pokrewne

- [Pełny tekst działu 27 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch27-Apr2016.pdf)
- [Przykłady działu 27](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27)
- [Niestandardowe programy renderujące](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
