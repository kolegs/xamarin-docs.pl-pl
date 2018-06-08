---
title: Lokalizacja od prawej do lewej
description: Lokalizacja od prawej do lewej dodaje obsługę kierunek przepływu od prawej do lewej dla aplikacji platformy Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 90E0CB16-C42A-4CC8-A70E-0C2CFB64A429
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 05/07/2018
ms.openlocfilehash: 94cf937a810e84ead0bdc1f69933968b42090a6f
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/07/2018
ms.locfileid: "34846435"
---
# <a name="right-to-left-localization"></a>Lokalizacja od prawej do lewej

_Lokalizacja od prawej do lewej dodaje obsługę kierunek przepływu od prawej do lewej dla aplikacji platformy Xamarin.Forms._

> [!NOTE]
> Lokalizacja od prawej do lewej wymaga użycia systemu iOS 9 lub nowszą i interfejsu API 17 lub nowszej w systemie Android.

Kierunek przepływu jest kierunek, w którym elementy interfejsu użytkownika na stronie są skanowane przez oczu. Niektóre języki, np. arabski i hebrajski, wymagają poukładany elementy interfejsu użytkownika w kierunku przepływu od prawej do lewej. Można to osiągnąć przez ustawienie [ `VisualElement.FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) właściwości. Ta właściwość pobiera lub Ustawia kierunek, w którym przepływ elementy interfejsu użytkownika w obrębie dowolnego elementu nadrzędnego, który kontroluje ich układ i powinny być ustawione na jedną z [ `FlowDirection` ](xref:Xamarin.Forms.FlowDirection) wartości wyliczenia:

- [`LeftToRight`](xref:Xamarin.Forms.FlowDirection.LeftToRight)
- [`RightToLeft`](xref:Xamarin.Forms.FlowDirection.RightToLeft)
- [`MatchParent`](xref:Xamarin.Forms.FlowDirection.MatchParent)

Ustawienie [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) właściwości [ `RightToLeft` ](xref:Xamarin.Forms.FlowDirection.RightToLeft) w elemencie zazwyczaj Ustawia wyrównanie w prawo, aby czytania od prawej do lewej i układ formantu przepływać z od prawej do lewej:

[![TodoItemPage w arabski kierunku przepływu od prawej do lewej](rtl-images/TodoItemPage-Arabic.png "TodoItemPage w arabski kierunku przepływu od prawej do lewej")](rtl-images/TodoItemPage-Arabic-Large.png#lightbox "TodoItemPage w arabski kierunku przepływu od prawej do lewej")

> [!TIP]
> Należy ustawić tylko [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) właściwości układu początkowej. Zmiana tej wartości w czasie wykonywania powoduje, że proces kosztowne układu, który będzie miało wpływ na wydajność.

Domyślny [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) wartość właściwości dla elementu bez elementu nadrzędnego jest [ `LeftToRight` ](xref:Xamarin.Forms.FlowDirection.LeftToRight), podczas gdy domyślny `FlowDirection` element z elementu nadrzędnego jest [ `MatchParent`](xref:Xamarin.Forms.FlowDirection.MatchParent). W związku z tym elementem dziedziczy `FlowDirection` wartość właściwości od jego elementu nadrzędnego w drzewie wizualnym, a każdy element można zastąpić wartość otrzymuje od jego elementu nadrzędnego.

> [!TIP]
> Lokalizowanie aplikacji dla języków od prawej do lewej, ustawić [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) właściwości układ strony lub głównego. To powoduje, że wszystkie elementy zawarte w strony lub głównego układu, odpowiednio odpowiadać na kierunek przepływu.

## <a name="respecting-device-flow-direction"></a>Przestrzeganie urządzenia kierunek przepływu

Przestrzeganie kierunek przepływu urządzenia na podstawie wybranego języka i regionu jest wybrany jawną developer i nie jest wykonywane automatycznie. Uzyskuje się przez ustawienie [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) właściwości na stronie lub głównego układu, aby `static` [ `Device.FlowDirection` ](xref:Xamarin.Forms.Device.FlowDirection) wartość:

```xaml
<ContentPage ... FlowDirection="{x:Static Device.FlowDirection}"> />
```

```csharp
this.FlowDirection = Device.FlowDirection;
```

Wszystkie elementy podrzędne strony lub głównego układu, będą domyślnie następnie dziedziczą [ `Device.FlowDirection` ](xref:Xamarin.Forms.Device.FlowDirection) wartości.

## <a name="platform-setup"></a>Instalator platformy

Instalator określonej platformy są wymagane do włączenia ustawień regionalnych od prawej do lewej.

### <a name="ios"></a>iOS

Wymagane ustawienia regionalne od prawej do lewej powinno być dodane obsługiwanych języków w elementach tablicy dla `CFBundleLocalizations` klucza w **Info.plist**. W poniższym przykładzie przedstawiono arabski, został on dodany do tablicy dla `CFBundleLocalizations` klucza:

```xml
<key>CFBundleLocalizations</key>
<array>
    <string>en</string>
    <string>ar</string>
</array>
```

![Języki obsługiwane w pliku info.plist](rtl-images/ios-locales.png "Info.plist obsługiwane języki")

Aby uzyskać więcej informacji, zobacz [podstawowe informacje o lokalizacji w systemie iOS](https://docs.microsoft.com/en-gb/xamarin/ios/app-fundamentals/localization/#localization-basics-in-ios).

Następnie można przetestować lokalizacja od prawej do lewej, zmieniając ustawienia regionalne od prawej do lewej, która została określona w języka i regionu w urządzeniu/symulatorze **Info.plist**.

> [!WARNING]
> Należy pamiętać, że w przypadku zmiany region i język ustawienia regionalne od prawej do lewej w systemach iOS, w każdym [ `DatePicker` ](xref:Xamarin.Forms.DatePicker) widoków spowoduje zgłoszenie wyjątku, jeśli nie ma zasobów wymaganych dla ustawień regionalnych. Na przykład podczas testowania aplikacji w arabski, który ma `DatePicker`, upewnij się, że **mideast** wybrano **internacjonalizacji** sekcji **kompilacji systemu iOS** okienka.

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

Następnie można przetestować lokalizacja od prawej do lewej, zmieniając urządzeniu/emulatorze do używania języka od prawej do lewej lub włączenie **Kierunek układu od prawej do lewej życie** w **Ustawienia > Opcje dewelopera**.

### <a name="universal-windows-platform-uwp"></a>Platforma uniwersalna systemu Windows (UWP)

Wymagane zasoby językowe powinny być określone w `<Resources>` węzła **Package.appxmanifest** pliku. W poniższym przykładzie przedstawiono arabski dodany do `<Resources>` węzła:

```xml
<Resources>
    <Resource Language="x-generate"/>
    <Resource Language="en" />
    <Resource Language="ar" />
</Resources>
```

Ponadto platformy uniwersalnej systemu Windows wymaga, aby kultury domyślnej aplikacji jest jawnie zdefiniowany w bibliotece programu .NET Standard. Można to zrobić przez ustawienie `NeutralResourcesLanguage` atrybutu w `AssemblyInfo.cs`, lub w innej klasy kultury domyślnej:

```csharp
using System.Resources;

[assembly: NeutralResourcesLanguage("en")]
```

Następnie można przetestować lokalizacja od prawej do lewej, zmieniając języka i regionu na urządzeniu do odpowiednich ustawień regionalnych od prawej do lewej.

## <a name="limitations"></a>Ograniczenia

Lokalizacja od prawej do lewej platformy Xamarin.Forms aktualnie ma kilka ograniczeń:

- [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) położenie przycisku paska narzędzi elementu lokalizacji i przejścia animacji jest kontrolowany przez ustawienia regionalne urządzenia, a nie [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) właściwości.
- [`CarouselPage`](xref:Xamarin.Forms.CarouselPage) Kierunek Przejdź nie przerzucania.
- [`Image`](xref:Xamarin.Forms.Image) zawartość wizualna nie przerzucania.
- [`DisplayAlert`](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayAlert/p/System.String/System.String/System.String/) i [ `DisplayActionSheet` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayActionSheet/p/System.String/System.String/System.String/System.String[]/) orientacji jest kontrolowany przez ustawienia regionalne urządzenia, a nie [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) właściwości.
- [`WebView`](xref:Xamarin.Forms.WebView) zawartość nie przestrzega [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) właściwości.
- A `TextDirection` właściwość musi ma zostać dodany, w celu sterowania wyrównaniem tekstu.

### <a name="ios"></a>iOS

- [`Stepper`](xref:Xamarin.Forms.Stepper) orientacja jest kontrolowany przez ustawienia regionalne urządzenia, a nie [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) właściwości.
- [`EntryCell`](xref:Xamarin.Forms.EntryCell) Wyrównanie tekstu jest kontrolowany przez ustawienia regionalne urządzenia, a nie [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) właściwości.
- [`ContextActions`](xref:Xamarin.Forms.Cell.ContextActions) gestów i wyrównania nie zostały wycofane.

### <a name="android"></a>Android

- [`SearchBar`](xref:Xamarin.Forms.SearchBar) orientacja jest kontrolowany przez ustawienia regionalne urządzenia, a nie [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) właściwości.
- [`ContextActions`](xref:Xamarin.Forms.Cell.ContextActions) umieszczanie jest kontrolowany przez ustawienia regionalne urządzenia, a nie [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) właściwości.

### <a name="uwp"></a>Platforma UWP

- [`Editor`](xref:Xamarin.Forms.Editor) Wyrównanie tekstu jest kontrolowany przez ustawienia regionalne urządzenia, a nie [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) właściwości.
- [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) Właściwość nie jest dziedziczona przez [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) elementów podrzędnych.
- [`ContextActions`](xref:Xamarin.Forms.Cell.ContextActions) Wyrównanie tekstu jest kontrolowany przez ustawienia regionalne urządzenia, a nie [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) właściwości.

## <a name="right-to-left-language-support-with-xamarinuniversity"></a>Od prawej do lewej języka obsługuje z Xamarin.University

> [!VIDEO https://youtube.com/embed/f2lQ5yw3iiU]

**Obsługuje platformy Xamarin.Forms 3.0 od prawej do lewej, przez [Xamarin University](https://university.xamarin.com/)**

## <a name="related-links"></a>Linki pokrewne

- [TodoLocalizedRTL przykładowej aplikacji](https://developer.xamarin.com/samples/xamarin-forms/TodoLocalizedRTL/)
