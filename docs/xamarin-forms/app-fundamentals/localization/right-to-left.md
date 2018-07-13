---
title: Lokalizacja od prawej do lewej
description: Lokalizacja od prawej do lewej dodaje obsługę kierunek przepływu od prawej do lewej, do aplikacji platformy Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 90E0CB16-C42A-4CC8-A70E-0C2CFB64A429
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 05/07/2018
ms.openlocfilehash: 6c0de68f974c704b5f43232a1fc98065c90ee4f7
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995726"
---
# <a name="right-to-left-localization"></a>Lokalizacja od prawej do lewej

_Lokalizacja od prawej do lewej dodaje obsługę kierunek przepływu od prawej do lewej, do aplikacji platformy Xamarin.Forms._

> [!NOTE]
> Od prawej do lewej lokalizacji wymaga użycia z systemem iOS 9 lub nowszą oraz interfejsu API 17 lub nowszego w systemie Android.

Kierunek przepływu jest kierunek, w którym elementy interfejsu użytkownika na tej stronie są skanowane okiem. Niektórych języków, takich jak arabski i hebrajski, wymagają rozmieszczony elementy interfejsu użytkownika w kierunku od prawej do lewej przepływu. Można to osiągnąć przez ustawienie [ `VisualElement.FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) właściwości. Tej właściwości pobiera lub Ustawia kierunek, w której przepływ elementów interfejsu użytkownika w ramach dowolnego elementu nadrzędnego, który kontroluje ich układ i powinna być równa jeden z [ `FlowDirection` ](xref:Xamarin.Forms.FlowDirection) wartości wyliczenia:

- [`LeftToRight`](xref:Xamarin.Forms.FlowDirection.LeftToRight)
- [`RightToLeft`](xref:Xamarin.Forms.FlowDirection.RightToLeft)
- [`MatchParent`](xref:Xamarin.Forms.FlowDirection.MatchParent)

Ustawienie [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) właściwości [ `RightToLeft` ](xref:Xamarin.Forms.FlowDirection.RightToLeft) w elemencie ogólnie Ustawia wyrównanie po prawej stronie, aby czytania od prawej do lewej, i układ formantu mogą przepływać z od prawej do lewej:

[![TodoItemPage w języku arabskim o kierunku od prawej do lewej przepływ](rtl-images/TodoItemPage-Arabic.png "TodoItemPage w języku arabskim o kierunku od prawej do lewej przepływ")](rtl-images/TodoItemPage-Arabic-Large.png#lightbox "TodoItemPage w języku arabskim o kierunku od prawej do lewej przepływu")

> [!TIP]
> Należy ustawić tylko [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) właściwość wstępny układ. Zmiana tej wartości w czasie wykonywania powoduje, że proces kosztowne układ, który będzie mieć wpływ na wydajność.

Wartość domyślna [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) wartość właściwości dla elementu bez klasy nadrzędnej jest [ `LeftToRight` ](xref:Xamarin.Forms.FlowDirection.LeftToRight), podczas gdy wartość domyślna `FlowDirection` dla elementu z elementu nadrzędnego jest [ `MatchParent`](xref:Xamarin.Forms.FlowDirection.MatchParent). W związku z tym, dziedziczy element `FlowDirection` wartość właściwości od jego elementu nadrzędnego w drzewie wizualnym i dowolnego elementu można zastąpić wartość otrzymuje od jego obiektu nadrzędnego.

> [!TIP]
> Lokalizowanie aplikacji w językach od prawej do lewej, ustawić [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) właściwości układu strony lub katalogu głównego. To powoduje, że wszystkie elementy zawarte w obrębie strony lub głównego układu reagować odpowiednio na kierunek przepływu.

## <a name="respecting-device-flow-direction"></a>Uwzględnienie kierunek przepływu urządzenia

Uwzględnienie kierunek przepływu urządzenia na podstawie wybranego języka i regionu jest wybór jawne dla deweloperów i nie jest wykonywane automatycznie. Można osiągnąć przez ustawienie [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) właściwości na stronie lub głównego układu do `static` [ `Device.FlowDirection` ](xref:Xamarin.Forms.Device.FlowDirection) wartość:

```xaml
<ContentPage ... FlowDirection="{x:Static Device.FlowDirection}"> />
```

```csharp
this.FlowDirection = Device.FlowDirection;
```

Wszystkie elementy podrzędne strony lub głównego układu, zostaną domyślnie następnie dziedziczą [ `Device.FlowDirection` ](xref:Xamarin.Forms.Device.FlowDirection) wartości.

## <a name="platform-setup"></a>Instalator platformy

Instalator określonych platform jest wymagany do włączenia ustawień regionalnych od prawej do lewej.

### <a name="ios"></a>iOS

Wpisanie od prawej do lewej powinny zostać dodane jako obsługiwanego języka do elementów tablicy `CFBundleLocalizations` w **Info.plist**. W poniższym przykładzie pokazano arabski o została dodana do tabeli dla `CFBundleLocalizations` klucza:

```xml
<key>CFBundleLocalizations</key>
<array>
    <string>en</string>
    <string>ar</string>
</array>
```

![Języki obsługiwane w pliku info.plist](rtl-images/ios-locales.png "Info.plist obsługiwane języki")

Aby uzyskać więcej informacji, zobacz [podstawowe informacje o lokalizacji, w systemie iOS](https://docs.microsoft.com/en-gb/xamarin/ios/app-fundamentals/localization/#localization-basics-in-ios).

Następnie można przetestować lokalizacji od prawej do lewej, zmieniając język i region, w urządzeniu/symulatorze od prawej do lewej ustawień regionalnych, który został określony w **Info.plist**.

> [!WARNING]
> Należy pamiętać, że po zmianie języka i regionu regionalne od prawej do lewej w systemach iOS, wszelkie [ `DatePicker` ](xref:Xamarin.Forms.DatePicker) widoków spowoduje zgłoszenie wyjątku, jeśli nie dołączysz zasoby wymagane dla ustawień regionalnych. Na przykład podczas testowania aplikacji w języku arabskim, który ma `DatePicker`, upewnij się, że **mideast** wybrano **internacjonalizacji** części **kompilacja systemu iOS** okienka.

### <a name="android"></a>Android

Aplikacji **AndroidManifest.xml** można zaktualizować pliku, aby `<uses-sdk>` zestaw węzłów `android:minSdkVersion` atrybutu 17 i `<application>` zestaw węzłów `android:supportsRtl` atrybutu `true`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest ... >
    <uses-sdk android:minSdkVersion="17" ... />
    <application ... android:supportsRtl="true">
    </application>
</manifest>
```

Następnie mogą być testowane lokalizacji od prawej do lewej, zmieniając urządzenia/emulator, aby korzystać z języka od prawej do lewej lub przez włączenie **Kierunek układu od prawej do lewej życie** w **Ustawienia > Opcje dewelopera**.

### <a name="universal-windows-platform-uwp"></a>Platforma uniwersalna systemu Windows (UWP)

Wymagane zasoby językowe powinny być określone w `<Resources>` węźle **Package.appxmanifest** pliku. W poniższym przykładzie pokazano arabski dodano do `<Resources>` węzła:

```xml
<Resources>
    <Resource Language="x-generate"/>
    <Resource Language="en" />
    <Resource Language="ar" />
</Resources>
```

Ponadto platformy uniwersalnej systemu Windows wymaga aplikacji domyślnej kultury jest jawnie zdefiniowany w bibliotece programu .NET Standard. Można to zrobić, ustawiając `NeutralResourcesLanguage` atrybutu w `AssemblyInfo.cs`, lub w innej klasy kultury domyślnej:

```csharp
using System.Resources;

[assembly: NeutralResourcesLanguage("en")]
```

Następnie można przetestować lokalizacji od prawej do lewej, zmieniając języka i regionu na urządzeniu odpowiednie ustawienia regionalne od prawej do lewej.

## <a name="limitations"></a>Ograniczenia

Lokalizacja zestawu narzędzi Xamarin.Forms od prawej do lewej jest aktualnie ma kilka ograniczeń:

- [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) Lokalizacja przycisku paska narzędzi elementu lokalizacji i przejścia animacji jest kontrolowana przez urządzenia ustawień regionalnych, zamiast [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) właściwości.
- [`CarouselPage`](xref:Xamarin.Forms.CarouselPage) Przesuń kierunek nie Przerzucanie.
- [`Image`](xref:Xamarin.Forms.Image) nie Przerzucanie zawartości wizualnej.
- [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String)) i [ `DisplayActionSheet` ](xref:Xamarin.Forms.Page.DisplayActionSheet(System.String,System.String,System.String,System.String[])) orientacji jest kontrolowana przez ustawienia regionalne urządzenia, a nie [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) właściwości.
- [`WebView`](xref:Xamarin.Forms.WebView) zawartość nie przestrzega [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) właściwości.
- A `TextDirection` właściwość musi ma zostać dodana, aby kontrolować wyrównanie tekstu.

### <a name="ios"></a>iOS

- [`Stepper`](xref:Xamarin.Forms.Stepper) orientacja jest kontrolowana przez ustawienia regionalne urządzenia, a nie [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) właściwości.
- [`EntryCell`](xref:Xamarin.Forms.EntryCell) Wyrównanie tekstu jest kontrolowana przez ustawienia regionalne urządzenia, a nie [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) właściwości.
- [`ContextActions`](xref:Xamarin.Forms.Cell.ContextActions) gestów i wyrównanie nie zostały wycofane.

### <a name="android"></a>Android

- [`SearchBar`](xref:Xamarin.Forms.SearchBar) orientacja jest kontrolowana przez ustawienia regionalne urządzenia, a nie [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) właściwości.
- [`ContextActions`](xref:Xamarin.Forms.Cell.ContextActions) umieszczanie jest kontrolowana przez urządzenia ustawień regionalnych, zamiast [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) właściwości.

### <a name="uwp"></a>Platforma UWP

- [`Editor`](xref:Xamarin.Forms.Editor) Wyrównanie tekstu jest kontrolowana przez ustawienia regionalne urządzenia, a nie [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) właściwości.
- [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) Właściwość nie jest dziedziczona przez [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) elementów podrzędnych.
- [`ContextActions`](xref:Xamarin.Forms.Cell.ContextActions) Wyrównanie tekstu jest kontrolowana przez ustawienia regionalne urządzenia, a nie [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) właściwości.

## <a name="right-to-left-language-support-with-xamarinuniversity"></a>Od prawej do lewej języka obsługuje z Xamarin.University

> [!VIDEO https://youtube.com/embed/f2lQ5yw3iiU]

**Produkt Xamarin.Forms 3.0 od prawej do lewej pomocy technicznej przez [platformy Xamarin University](https://university.xamarin.com/)**

## <a name="related-links"></a>Linki pokrewne

- [TodoLocalizedRTL przykładowej aplikacji](https://developer.xamarin.com/samples/xamarin-forms/TodoLocalizedRTL/)
