---
title: Klasa urządzenia zestawu narzędzi Xamarin.Forms
description: W tym artykule opisano sposób użycia klasy urządzenia platformy Xamarin.Forms, aby uzyskać szczegółową kontrolę nad tym funkcje i układy, w poszczególnych platform.
ms.prod: xamarin
ms.assetid: 2F304AEC-8612-4833-81E5-B2F3F469B2DF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2017
ms.openlocfilehash: ff707cdf73665ae07881d2d17ec837a4cfacaca0
ms.sourcegitcommit: 3e980fbf92c69c3dd737554e8c6d5b94cf69ee3a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37935374"
---
# <a name="xamarinforms-device-class"></a>Klasa urządzenia zestawu narzędzi Xamarin.Forms

[ `Device` ](xref:Xamarin.Forms.Device) Klasy zawiera szereg właściwości i metod, które pomogą dostosować układ i funkcje na poszczególnych platform.

Oprócz metod i właściwości cel wybierając kod w konkretnych typów sprzętu i rozmiarach `Device` klasa zawiera [BeginInvokeOnMainThread](#Device_BeginInvokeOnMainThread) metody, który powinien być używany podczas interakcji z interfejsem użytkownika kontrolki z wątków w tle.

<a name="providing-platform-values" />

## <a name="providing-platform-specific-values"></a>Podając wartości specyficzne dla platformy

Przed Xamarin.Forms 2.3.4 platformy aplikacja była uruchomiona na może uzyskać, sprawdzając [ `Device.OS` ](xref:Xamarin.Forms.Device.OS) właściwość i porównując ją do [ `TargetPlatform.iOS` ](xref:Xamarin.Forms.TargetPlatform.iOS), [ `TargetPlatform.Android` ](xref:Xamarin.Forms.TargetPlatform.Android), [ `TargetPlatform.WinPhone` ](xref:Xamarin.Forms.TargetPlatform.WinPhone), i [ `TargetPlatform.Windows` ](xref:Xamarin.Forms.TargetPlatform.Windows) wartości wyliczenia. Analogicznie, jeden z [ `Device.OnPlatform` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.OnPlatform/p/System.Action/System.Action/System.Action/System.Action/) przeciążenia może służyć do zapewnienia wartości specyficznych dla platformy do formantu.

Jednak od zestawu narzędzi Xamarin.Forms 2.3.4 te interfejsy API zostały przestarzały i zastąpiony. [ `Device` ](xref:Xamarin.Forms.Device) Klasy zawiera teraz stałych ciągów publicznych, które identyfikują platform — [ `Device.iOS` ](xref:Xamarin.Forms.Device.iOS), [ `Device.Android` ](xref:Xamarin.Forms.Device.Android), `Device.WinPhone`() przestarzałe) `Device.WinRT` (przestarzałe) [ `Device.UWP` ](xref:Xamarin.Forms.Device.UWP), i [ `Device.macOS` ](xref:Xamarin.Forms.Device.macOS). Podobnie [ `Device.OnPlatform` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.OnPlatform/p/System.Action/System.Action/System.Action/System.Action/) przeciążenia została zastąpiona [ `OnPlatform` ](https://developer.xamarin.com/api/type/Xamarin.Forms.OnPlatform%3CT%3E/) i [ `On` ](xref:Xamarin.Forms.On) interfejsów API.

W języku C#, można podać wartości specyficznych dla platformy, tworząc `switch` instrukcji na [ `Device.RuntimePlatform` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Device.RuntimePlatform/) właściwości, a następnie podając `case` instrukcji dla platform wymagane:

```csharp
double top;
switch (Device.RuntimePlatform)
{
  case Device.iOS:
    top = 20;
    break;
  case Device.Android:
  case Device.UWP:
  default:
    top = 0;
    break;
}
layout.Margin = new Thickness(5, top, 5, 0);
```

[ `OnPlatform` ](https://developer.xamarin.com/api/type/Xamarin.Forms.OnPlatform%3CT%3E/) i [ `On` ](xref:Xamarin.Forms.On) klasy zapewniają taką samą funkcjonalność w XAML:

```xaml
<StackLayout>
  <StackLayout.Margin>
    <OnPlatform x:TypeArguments="Thickness">
      <On Platform="iOS" Value="0,20,0,0" />
      <On Platform="Android, UWP" Value="0,0,0,0" />
    </OnPlatform>
  </StackLayout.Margin>
  ...
</StackLayout>
```

[ `OnPlatform` ](https://developer.xamarin.com/api/type/Xamarin.Forms.OnPlatform%3CT%3E/) Klasa jest klasa generyczna i dlatego musi być utworzone przy użyciu `x:TypeArguments` atrybut, który jest zgodny z typem docelowym. W [ `On` ](xref:Xamarin.Forms.On) klasy [ `Platform` ](xref:Xamarin.Forms.On.Platform) atrybut może zaakceptować pojedynczy `string` wartość lub wielu rozdzielonych przecinkami `string` wartości.

> [!IMPORTANT]
> Podając niepoprawne `Platform` wartość atrybutu `On` klasy nie powoduje wystąpienie błędu. Zamiast tego kod zostanie wykonany bez wartości specyficzne dla platformy, są stosowane.

<a name="Device_Idiom" />

## <a name="deviceidiom"></a>Device.Idiom

`Device.Idiom` Może służyć do zmiany układy lub funkcji, w zależności od urządzenia, na której działa aplikacja na. [ `TargetIdiom` ](xref:Xamarin.Forms.TargetIdiom) Wyliczenie zawiera następujące wartości:

-  **Telefon** — iPhone, iPod touch oraz urządzenia z systemem Android węższe niż 600 spadku ^
-  **Tablet** — iPad, urządzenia Windows i urządzeń z systemem Android szerszy niż 600 spadku ^
-  **Pulpit** — tylko zwracanych w [aplikacji platformy UWP](~/xamarin-forms/platform/windows/installation/index.md) na komputerze stacjonarnym z systemem Windows 10 (zwraca `Phone` na urządzeniach przenośnych Windows, w tym w scenariuszach firmy Continuum)
-  **TV** — Tizen telewizyjne urządzenia
-  **Nieobsługiwana** — nieużywane

*^ spadku niekoniecznie jest liczba fizycznych pikseli*

`Idiom` jest szczególnie przydatne w przypadku tworzenia układów, wykorzystujące większych ekrany, takich jak to:

```csharp
if (Device.Idiom == TargetIdiom.Phone) {
    // layout views vertically
} else {
    // layout views horizontally for a larger display (tablet or desktop)
}
```

## <a name="deviceflowdirection"></a>Device.FlowDirection

[ `Device.FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) Wartość pobiera [ `FlowDirection` ](xref:Xamarin.Forms.FlowDirection) wartości wyliczenia, który reprezentuje bieżący kierunek przepływu przez urządzenie. Kierunek przepływu jest kierunek, w którym elementy interfejsu użytkownika na tej stronie są skanowane okiem. Wartości wyliczenia są następujące:

- [`LeftToRight`](xref:Xamarin.Forms.FlowDirection.LeftToRight)
- [`RightToRight`](xref:Xamarin.Forms.FlowDirection.RightToLeft)
- [`MatchParent`](xref:Xamarin.Forms.FlowDirection.MatchParent)

W XAML [ `Device.FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) wartość można pobrać przy użyciu `x:Static` — rozszerzenie znaczników:

```xaml
<ContentPage ... FlowDirection="{x:Static Device.FlowDirection}"> />
```

Jest równoważny kod w języku C#:

```csharp
this.FlowDirection = Device.FlowDirection;
```

Aby uzyskać więcej informacji na temat kierunek przepływu, zobacz [lokalizacji od prawej do lewej](~/xamarin-forms/app-fundamentals/localization/right-to-left.md).

<a name="Device_Styles" />

## <a name="devicestyles"></a>Device.Styles

[ `Styles` Właściwość](~/xamarin-forms/user-interface/styles/index.md) zawiera definicje stylów wbudowanych, które mogą być stosowane do niektóre kontrolki (takie jak `Label`) `Style` właściwości. Są dostępne style:

* BodyStyle
* CaptionStyle
* ListItemDetailTextStyle
* ListItemTextStyle
* SubtitleStyle
* TitleStyle

<a name="Device_GetNamedSize" />

## <a name="devicegetnamedsize"></a>Device.GetNamedSize

`GetNamedSize` można używać podczas ustawiania [ `FontSize` ](~/xamarin-forms/user-interface/text/fonts.md) w kodzie języka C#:

```csharp
myLabel.FontSize = Device.GetNamedSize (NamedSize.Small, myLabel);
someLabel.FontSize = Device.OnPlatform (
      24,         // hardcoded size
      Device.GetNamedSize (NamedSize.Medium, someLabel),
      Device.GetNamedSize (NamedSize.Large, someLabel)
);
```

<a name="Device_OpenUri" />

## <a name="deviceopenuri"></a>Device.OpenUri

`OpenUri` — Metoda może służyć do wyzwolenia operacje na podstawowej platformy, takich jak Otwórz adres URL w przeglądarce internetowej natywnego (**Safari** w systemie iOS lub **Internet** w systemie Android).

```csharp
Device.OpenUri(new Uri("https://evolve.xamarin.com/"));
```

[Przykładowe WebView](https://github.com/xamarin/xamarin-forms-samples/blob/master/WorkingWithWebview/WorkingWithWebview/WebAppPage.cs) zawiera przykłady dotyczące używania `OpenUri` otwieranie adresów URL i również wyzwalać połączeń telefonicznych.

[Próbki mapy](https://github.com/xamarin/xamarin-forms-samples/blob/master/WorkingWithMaps/WorkingWithMaps/MapAppPage.cs) używa również `Device.OpenUri` do wyświetlania mapy i Wskazówki dojazdu przy użyciu natywnych **mapuje** aplikacje w systemach iOS i Android.

<a name="Device_StartTimer" />

## <a name="devicestarttimer"></a>Device.StartTimer

`Device` Ma również klasy `StartTimer` metodę, która zapewnia prostą metodę do wyzwalania zadania zależne od czasu, który współdziała z wspólnego kodu zestawu narzędzi Xamarin.Forms, łącznie z biblioteki .NET Standard. Przekaż `TimeSpan` Ustaw interwał i zwracać `true` zapewnienie Czasomierz został uruchomiony lub `false` ją zatrzymać po bieżącego wywołania.

```csharp
Device.StartTimer (new TimeSpan (0, 0, 60), () => {
    // do something every 60 seconds
    return true; // runs again, or false to stop
});
```

Jeśli kod wewnątrz czasomierza wchodzi w interakcję z interfejsem użytkownika (taką jak konfigurowanie tekst `Label` lub wyświetlanie alertu) powinna być podejmowana wewnątrz `BeginInvokeOnMainThread` wyrażenia (patrz poniżej).

<a name="Device_BeginInvokeOnMainThread" />

## <a name="devicebegininvokeonmainthread"></a>Device.BeginInvokeOnMainThread

Elementy interfejsu użytkownika nigdy nie powinni mieć dostęp wątków w tle, takie jak kod uruchomiony w czasomierz lub procedury obsługi zakończenia dla operacji asynchronicznych, takich jak żądania sieci web. Wszelki kod w tle, który musi zaktualizować interfejs użytkownika powinna być otoczona wewnątrz [ `BeginInvokeOnMainThread` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.BeginInvokeOnMainThread/p/System.Action/). Jest to równoważne z `InvokeOnMainThread` w systemach iOS, `RunOnUiThread` w systemie Android i `Dispatcher.RunAsync` na platformie Universal Windows.

Kod zestawu narzędzi Xamarin.Forms to:

```csharp
Device.BeginInvokeOnMainThread ( () => {
  // interact with UI elements
});
```

Uwaga tej metody za pomocą `async/await` nie trzeba używać `BeginInvokeOnMainThread` działające z wątku głównego interfejsu użytkownika.

## <a name="summary"></a>Podsumowanie

Xamarin.Forms `Device` klasa umożliwia precyzyjne kontrolowanie funkcji i układów w poszczególnych platform — nawet w typowych kodu (projekty biblioteki .NET Standard lub udostępnione projekty).


## <a name="related-links"></a>Linki pokrewne

- [Przykładowe urządzenia](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithDevice/)
- [Style próbki](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [Urządzenia](xref:Xamarin.Forms.Device)
