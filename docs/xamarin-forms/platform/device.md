---
title: Klasa urządzenia
ms.prod: xamarin
ms.assetid: 2F304AEC-8612-4833-81E5-B2F3F469B2DF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2017
ms.openlocfilehash: 1fc3fb17ec97ce9028abbf63cdedbfc5fec12204
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/07/2018
---
# <a name="device-class"></a>Klasa urządzenia

[ `Device` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Device/) Klasy zawiera wiele właściwości i metody, aby pomóc deweloperom dostosowywać układ i funkcje na podstawie poszczególnych platform.

Oprócz metody i właściwości do kodu docelowego w konkretnych typów sprzętu i wielkości `Device` klasa zawiera [BeginInvokeOnMainThread](#Device_BeginInvokeOnMainThread) metodę, która ma być używana podczas interakcji z interfejsu użytkownika formantów wątki w tle.

<a name="providing-platform-values" />

## <a name="providing-platform-specific-values"></a>Udostępnienie wartości specyficzne dla platformy

Przed platformy Xamarin.Forms 2.3.4, aplikacja była uruchomiona na platformie można uzyskać, sprawdzając [ `Device.OS` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Device.OS/) właściwości i porównanie do [ `TargetPlatform.iOS` ](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.iOS/), [ `TargetPlatform.Android` ](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.Android/), [ `TargetPlatform.WinPhone` ](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.WinPhone/), i [ `TargetPlatform.Windows` ](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.Windows/) wartości wyliczenia. Analogicznie, jeden z [ `Device.OnPlatform` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.OnPlatform/p/System.Action/System.Action/System.Action/System.Action/) przeciążenia może służyć do zapewnienia wartości specyficzne dla platformy do formantu.

Jednak ponieważ platformy Xamarin.Forms 2.3.4 te interfejsy API zostały przestarzałe i zastępowane. [ `Device` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Device/) Klasy zawiera teraz stałe ciąg publicznego, które identyfikują platformy — [ `Device.iOS` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.iOS/), [ `Device.Android` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.Android/), [ `Device.WinPhone` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.WinPhone/) (przestarzałe) [ `Device.WinRT` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.WinRT/) (przestarzałe) [ `Device.UWP` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.UWP/), i [ `Device.macOS` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.macOS/). Podobnie [ `Device.OnPlatform` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.OnPlatform/p/System.Action/System.Action/System.Action/System.Action/) zostały zastąpione przeciążenia [ `OnPlatform` ](https://developer.xamarin.com/api/type/Xamarin.Forms.OnPlatform%3CT%3E/) i [ `On` ](https://developer.xamarin.com/api/type/Xamarin.Forms.On/) interfejsów API.

W języku C#, można podać wartości specyficzne dla platformy, tworząc `switch` instrukcji na [ `Device.RuntimePlatform` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Device.RuntimePlatform/) właściwości i podają `case` instrukcje dla platform wymagane:

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

[ `OnPlatform` ](https://developer.xamarin.com/api/type/Xamarin.Forms.OnPlatform%3CT%3E/) i [ `On` ](https://developer.xamarin.com/api/type/Xamarin.Forms.On/) klasy mają te same funkcje w języku XAML:

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

[ `OnPlatform` ](https://developer.xamarin.com/api/type/Xamarin.Forms.OnPlatform%3CT%3E/) Klasa jest klasą szablonową i dlatego musi być utworzone z `x:TypeArguments` atrybut, który jest zgodny z typem docelowym. W [ `On` ](https://developer.xamarin.com/api/type/Xamarin.Forms.On/) klasy [ `Platform` ](https://developer.xamarin.com/api/property/Xamarin.Forms.On.Platform/) atrybut może przyjąć pojedynczy `string` wartość lub wielu rozdzielana przecinkami `string` wartości.

> [!IMPORTANT]
> Zapewnianie nieprawidłowych `Platform` wartość w atrybutu `On` klasy nie spowoduje błąd. Zamiast tego kod zostanie wykonany bez wartości specyficzne dla platformy, które są stosowane.

<a name="Device_Idiom" />

## <a name="deviceidiom"></a>Device.Idiom

`Device.Idiom` Służy do zmiany układów lub funkcji w zależności od urządzenia, aplikacja jest uruchomiona na. [ `TargetIdiom` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TargetIdiom/) Wyliczenie zawiera następujące wartości:

-  **Telefon** — iPhone i iPod touch, a urządzenia z systemem Android węższe niż 600 DIP ^
-  **Tablet** — iPad, urządzeń z systemem Windows i urządzeń z systemem Android szerszy niż 600 DIP ^
-  **Pulpitu** — tylko zwracane w [aplikacji platformy UWP](~/xamarin-forms/platform/windows/installation/index.md) na komputerach stacjonarnych z systemem Windows 10 (zwraca `Phone` na urządzeniach przenośnych systemu Windows, w tym w scenariuszach kontynuacja)
-  **TV** – telewizji Tizen urządzeń
-  **Nieobsługiwany** — nieużywane

*^ DIP niekoniecznie liczbę fizycznych pikseli*

`Idiom` jest szczególnie przydatne w przypadku tworzenia układów wykorzystujące ekrany większy, jak to:

```csharp
if (Device.Idiom == TargetIdiom.Phone) {
    // layout views vertically
} else {
    // layout views horizontally for a larger display (tablet or desktop)
}
```

## <a name="deviceflowdirection"></a>Device.FlowDirection

[ `Device.FlowDirection` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.FlowDirection/) Wartość pobiera [ `FlowDirection` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FlowDirection/) wartości wyliczenia, która reprezentuje bieżący kierunek przepływu używany przez urządzenie. Kierunek przepływu jest kierunek, w którym elementy interfejsu użytkownika na stronie są skanowane przez oczu. Wartości wyliczenia są:

- [`LeftToRight`](https://developer.xamarin.com/api/field/Xamarin.Forms.FlowDirection.LeftToRight/)
- [`RightToRight`](https://developer.xamarin.com/api/field/Xamarin.Forms.FlowDirection.RightToLeft/)
- [`MatchParent`](https://developer.xamarin.com/api/field/Xamarin.Forms.FlowDirection.MatchParent/)

W języku XAML [ `Device.FlowDirection` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.FlowDirection/) można pobrać wartości przy użyciu `x:Static` — rozszerzenie znaczników:

```xaml
<ContentPage ... FlowDirection="{x:Static Device.FlowDirection}"> />
```

Jest równoważne kodu w języku C#:

```csharp
this.FlowDirection = Device.FlowDirection;
```

Aby uzyskać więcej informacji na temat kierunek przepływu, zobacz [lokalizacja od prawej do lewej](~/xamarin-forms/app-fundamentals/localization/right-to-left.md).

<a name="Device_Styles" />

## <a name="devicestyles"></a>Device.Styles

[ `Styles` Właściwości](~/xamarin-forms/user-interface/styles/index.md) zawiera definicje wbudowane style, które można zastosować do niektórych formantów (takie jak `Label`) `Style` właściwości. Style dostępne są następujące:

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

`OpenUri` Metoda może służyć do wyzwolenia operacje na podstawowej platformy, takich jak Otwórz adres URL w przeglądarce natywnej (**Safari** w systemie iOS lub **Internet** w systemie Android).

```csharp
Device.OpenUri(new Uri("https://evolve.xamarin.com/"));
```

[Próbki WebView](https://github.com/xamarin/xamarin-forms-samples/blob/master/WorkingWithWebview/WorkingWithWebview/WebAppPage.cs) zawiera przykład za pomocą `OpenUri` na otwieranie adresów URL, a także wyzwoli połączeń telefonicznych.

[Próbki mapy](https://github.com/xamarin/xamarin-forms-samples/blob/master/WorkingWithMaps/WorkingWithMaps/MapAppPage.cs) używa również `Device.OpenUri` do wyświetlenia mapy i wskazówkami za pomocą natywnego **mapy** aplikacji w systemach iOS i Android.

<a name="Device_StartTimer" />

## <a name="devicestarttimer"></a>Device.StartTimer

`Device` Klasa ma również `StartTimer` metodę, która zapewnia prostą metodę do wyzwalania zadania zależne od czasu, który działa w typowy kod platformy Xamarin.Forms (w tym PCLs). Przekaż `TimeSpan` ustawić interwał, a następnie wróć `true` do zachowania czasomierza uruchomiony lub `false` przestanie po bieżącego wywołania.

```csharp
Device.StartTimer (new TimeSpan (0, 0, 60), () => {
    // do something every 60 seconds
    return true; // runs again, or false to stop
});
```

Jeśli kodu wewnątrz czasomierza wchodzi w interakcję z interfejsem użytkownika (np. Określanie tekstu `Label` lub wyświetlanie alertu) należy przypisywać wewnątrz `BeginInvokeOnMainThread` wyrażenia (patrz poniżej).

<a name="Device_BeginInvokeOnMainThread" />

## <a name="devicebegininvokeonmainthread"></a>Device.BeginInvokeOnMainThread

Elementy interfejsu użytkownika, nigdy nie powinni mieć dostęp przez wątki w tle, takie jak kodu uruchamianego w czasomierz lub zakończenia obsługę asynchroniczne operacje, takie jak żądania sieci web. Tło kodu, który musi zaktualizować interfejsu użytkownika musi być ujęte w [ `BeginInvokeOnMainThread` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.BeginInvokeOnMainThread/p/System.Action/). Jest to równoważne z `InvokeOnMainThread` w systemach iOS, `RunOnUiThread` w systemie Android, i `Dispatcher.RunAsync` na platformy uniwersalnej systemu Windows.

Kod platformy Xamarin.Forms jest:

```csharp
Device.BeginInvokeOnMainThread ( () => {
  // interact with UI elements
});
```

Uwaga tej metody za pomocą `async/await` nie trzeba używać `BeginInvokeOnMainThread` działające z wątku interfejsu użytkownika głównego.

## <a name="summary"></a>Podsumowanie

Platformy Xamarin.Forms `Device` klasa umożliwia precyzyjną kontrolę nad funkcjonalność i układy na podstawie na platformie — nawet wspólną kodu (PCL lub udostępnionych projektów).


## <a name="related-links"></a>Linki pokrewne

- [Przykładowe urządzenia](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithDevice/)
- [Style próbki](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [Urządzenia](https://developer.xamarin.com/api/type/Xamarin.Forms.Device/)
