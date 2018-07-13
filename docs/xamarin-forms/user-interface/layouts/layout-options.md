---
title: Opcje układu w interfejsie Xamarin.Forms
description: Każdego widoku interfejsu Xamarin.Forms ma właściwości HorizontalOptions i VerticalOptions typu LayoutOptions. W tym artykule opisano wpływ każdej wartości LayoutOptions wyrównania i rozszerzanie widoku.
ms.prod: xamarin
ms.assetid: 7CAB5631-5153-4DEF-8AD7-C6011CE44307
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/10/2017
ms.openlocfilehash: 1ede5f75925a3dafa93062d147fa349ff91f07d2
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995313"
---
# <a name="layout-options-in-xamarinforms"></a>Opcje układu w interfejsie Xamarin.Forms

_Każdego widoku interfejsu Xamarin.Forms ma właściwości HorizontalOptions i VerticalOptions typu LayoutOptions. W tym artykule opisano wpływ każdej wartości LayoutOptions wyrównania i rozszerzanie widoku._

## <a name="overview"></a>Omówienie

[ `LayoutOptions` ](xref:Xamarin.Forms.LayoutOptions) Struktury hermetyzuje dwóch preferencje układu:

- **Wyrównanie** — widok preferowanego wyrównanie, który określa jej położenie i rozmiar, w ramach jego układ nadrzędnej.
- **Rozszerzenie** — używane tylko przez [ `StackLayout` ](xref:Xamarin.Forms.StackLayout)oraz wskazuje, w przypadku widoku należy używać dodatkowe miejsce, jeśli jest ona dostępna.

Te preferencje układu można zastosować do [ `View` ](xref:Xamarin.Forms.View), względem jego elementu nadrzędnego, ustawiając [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) lub [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) właściwość `View` do jednego z pola publiczne z [ `LayoutOptions` ](xref:Xamarin.Forms.LayoutOptions) struktury. Pola publiczne są następujące:

- [`Start`](xref:Xamarin.Forms.LayoutOptions.Start)
- [`Center`](xref:Xamarin.Forms.LayoutOptions.Center)
- [`End`](xref:Xamarin.Forms.LayoutOptions.End)
- [`Fill`](xref:Xamarin.Forms.LayoutOptions.Fill)
- [`StartAndExpand`](xref:Xamarin.Forms.LayoutOptions.StartAndExpand)
- [`CenterAndExpand`](xref:Xamarin.Forms.LayoutOptions.CenterAndExpand)
- [`EndAndExpand`](xref:Xamarin.Forms.LayoutOptions.EndAndExpand)
- [`FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand)

`Start`, `Center`, `End`, I `Fill` pola są używane do definiowania wyrównania tego widoku w układzie nadrzędnego:

- Wyrównanie w poziomie, aby uzyskać [ `Start` ](xref:Xamarin.Forms.LayoutOptions.Start) pozycji [ `View` ](xref:Xamarin.Forms.View) po stronie lewej układu nadrzędnej oraz wyrównanie w pionie, umieszcza `View` u góry Układ nadrzędnej.
- Wyrównanie w poziomie i pionie, aby uzyskać [ `Center` ](xref:Xamarin.Forms.LayoutOptions.Center) poziomo lub pionowo centra [ `View` ](xref:Xamarin.Forms.View).
- Wyrównanie w poziomie, aby uzyskać [ `End` ](xref:Xamarin.Forms.LayoutOptions.End) pozycji [ `View` ](xref:Xamarin.Forms.View) na po prawej stronie układu nadrzędnej oraz wyrównanie w pionie, umieszcza `View` u dołu układu nadrzędnej.
- Wyrównanie w poziomie, aby uzyskać [ `Fill` ](xref:Xamarin.Forms.LayoutOptions.Fill) zapewnia, że [ `View` ](xref:Xamarin.Forms.View) wypełnia szerokość układu nadrzędnego i wyrównanie w pionie, zapewnia, że `View` wypełnia Wysokość układu nadrzędnej.

`StartAndExpand`, `CenterAndExpand`, `EndAndExpand`, I `FillAndExpand` wartości są używane do definiowania preferencje wyrównania i tego, czy widok zajmują więcej miejsca Jeśli jest dostępna w ramach nadrzędnej [ `StackLayout` ](xref:Xamarin.Forms.StackLayout).

> [!NOTE]
> Wartość domyślna widoku [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) i [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) właściwości jest [ `LayoutOptions.Fill` ](xref:Xamarin.Forms.LayoutOptions.Fill).

<a name="alignment" />

## <a name="alignment"></a>Wyrównanie

Wyrównanie Określa, jak widok przy znajduje się w obrębie swojego nadrzędnego układu układ nadrzędny zawiera nieużywane miejsce (układ nadrzędnego jest większy niż łącznego rozmiaru wszystkich elementów podrzędnych).

A [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) tylko szanuje `Start`, `Center`, `End`, i `Fill` [ `LayoutOptions` ](xref:Xamarin.Forms.LayoutOptions) pól w widokach podrzędne, które znajdują się w odwrotnym kierunku Aby `StackLayout` orientacji. W związku z tym, podrzędne widoków w orientacji pionowej `StackLayout` można ustawić ich [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) właściwości do jednego z `Start`, `Center`, `End`, lub `Fill` pola. Podobnie, podrzędne widoków w orientacji poziomej `StackLayout` można ustawić ich [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) właściwości do jednego z `Start`, `Center`, `End`, lub `Fill` pola.

A [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) respektują `Start`, `Center`, `End`, i `Fill` [ `LayoutOptions` ](xref:Xamarin.Forms.LayoutOptions) pól w widokach podrzędne, które znajdują się w tym samym kierunku co `StackLayout` orientacji. W związku z tym, orientacji pionowej `StackLayout` ignoruje `Start`, `Center`, `End`, lub `Fill` pola zostaną ustawione na [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) właściwości podrzędnej widoków. Podobnie, orientacji poziomej `StackLayout` ignoruje `Start`, `Center`, `End`, lub `Fill` pola zostaną ustawione na [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) właściwości podrzędnej widoków.

> [!NOTE]
> [`LayoutOptions.Fill`](xref:Xamarin.Forms.LayoutOptions.Fill) ogólnie zastąpienia rozmiar określony za pomocą żądań [ `HeightRequest` ](xref:Xamarin.Forms.VisualElement.HeightRequest) i [ `WidthRequest` ](xref:Xamarin.Forms.VisualElement.WidthRequest) właściwości.

Pokazano w poniższym przykładzie kodu XAML, orientacji pionowej [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) gdzie każdego elementu podrzędnego [ `Label` ](xref:Xamarin.Forms.Label) ustawia jego [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) właściwości jedno z pól wyrównanie czterech z [ `LayoutOptions` ](xref:Xamarin.Forms.LayoutOptions) strukturę:

```xaml
<StackLayout Margin="0,20,0,0">
  ...
  <Label Text="Start" BackgroundColor="Gray" HorizontalOptions="Start" />
  <Label Text="Center" BackgroundColor="Gray" HorizontalOptions="Center" />
  <Label Text="End" BackgroundColor="Gray" HorizontalOptions="End" />
  <Label Text="Fill" BackgroundColor="Gray" HorizontalOptions="Fill" />
</StackLayout>
```

Równoważny kod C# jest pokazany poniżej:

```csharp
Content = new StackLayout
{
  Margin = new Thickness(0, 20, 0, 0),
  Children = {
    ...
    new Label { Text = "Start", BackgroundColor = Color.Gray, HorizontalOptions = LayoutOptions.Start },
    new Label { Text = "Center", BackgroundColor = Color.Gray, HorizontalOptions = LayoutOptions.Center },
    new Label { Text = "End", BackgroundColor = Color.Gray, HorizontalOptions = LayoutOptions.End },
    new Label { Text = "Fill", BackgroundColor = Color.Gray, HorizontalOptions = LayoutOptions.Fill }
  }
};
```

Ten kod powoduje w układzie pokazano na poniższych zrzutach ekranu:

[![](layout-options-images/alignment.png "Opcje układu wyrównanie")](layout-options-images/alignment-large.png#lightbox "opcji układu wyrównania")

<a name="expansion" />

## <a name="expansion"></a>Rozszerzenia

Rozszerzenie Określa, czy widok, zajmują więcej miejsca, jeśli to możliwe, w ramach [ `StackLayout` ](xref:Xamarin.Forms.StackLayout). Jeśli `StackLayout` zawiera nieużywane miejsce (czyli `StackLayout` przekracza łączny rozmiar wszystkich jego elementów podrzędnych), nieużywane miejsce jest dzielone równomiernie wszystkich widoków podrzędne, które żądają rozszerzenia, ustawiając ich [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions)lub [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) właściwości [ `LayoutOptions` ](xref:Xamarin.Forms.LayoutOptions) pole, które używa `AndExpand` sufiks. Należy pamiętać, że w przypadku wszystkich miejsca w `StackLayout` jest używany, opcje rozszerzenia nie mają wpływu.

A [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) rozwinąć można tylko widoki podrzędnych w kierunku orientacji. W związku z tym, orientacji pionowej `StackLayout` można rozwinąć widoki podrzędne, które ustaw ich [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) właściwości do jednego z `StartAndExpand`, `CenterAndExpand`, `EndAndExpand`, lub `FillAndExpand` pola, jeśli `StackLayout` zawiera nieużywane miejsce. Podobnie, orientacji poziomej `StackLayout` można rozwinąć widoki podrzędne, które ustaw ich [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) właściwości do jednego z `StartAndExpand`, `CenterAndExpand`, `EndAndExpand`, lub `FillAndExpand` pola, jeśli `StackLayout` zawiera nieużywane miejsce.

A [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) nie można rozwinąć widoków podrzędnych w kierunku przeciwnym orientacji. W związku z tym, w orientacji pionowej `StackLayout`, ustawiając [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) właściwość widok podrzędny [ `StartAndExpand` ](xref:Xamarin.Forms.LayoutOptions.StartAndExpand) ma taki sam efekt jak ustawienie właściwości [ `Start`](xref:Xamarin.Forms.LayoutOptions.Start).

> [!NOTE]
> Należy pamiętać, że włączenie rozszerzenia nie zmienia rozmiar widoku, chyba że używa [ `LayoutOptions.FillAndExpand` ](xref:Xamarin.Forms.LayoutOptions.FillAndExpand).

Pokazano w poniższym przykładzie kodu XAML, orientacji pionowej [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) gdzie każdego elementu podrzędnego [ `Label` ](xref:Xamarin.Forms.Label) ustawia jego [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) właściwości jedno z pól cztery rozszerzenia z [ `LayoutOptions` ](xref:Xamarin.Forms.LayoutOptions) strukturę:

```xaml
<StackLayout Margin="0,20,0,0">
  ...
  <BoxView BackgroundColor="Red" HeightRequest="1" />
  <Label Text="Start" BackgroundColor="Gray" VerticalOptions="StartAndExpand" />
  <BoxView BackgroundColor="Red" HeightRequest="1" />
  <Label Text="Center" BackgroundColor="Gray" VerticalOptions="CenterAndExpand" />
  <BoxView BackgroundColor="Red" HeightRequest="1" />
  <Label Text="End" BackgroundColor="Gray" VerticalOptions="EndAndExpand" />
  <BoxView BackgroundColor="Red" HeightRequest="1" />
  <Label Text="Fill" BackgroundColor="Gray" VerticalOptions="FillAndExpand" />
  <BoxView BackgroundColor="Red" HeightRequest="1" />
</StackLayout>
```

Równoważny kod C# jest pokazany poniżej:

```csharp
Content = new StackLayout
{
  Margin = new Thickness(0, 20, 0, 0),
  Children = {
    ...
    new BoxView { BackgroundColor = Color.Red, HeightRequest = 1 },
    new Label { Text = "StartAndExpand", BackgroundColor = Color.Gray, VerticalOptions = LayoutOptions.StartAndExpand },
    new BoxView { BackgroundColor = Color.Red, HeightRequest = 1 },
    new Label { Text = "CenterAndExpand", BackgroundColor = Color.Gray, VerticalOptions = LayoutOptions.CenterAndExpand },
    new BoxView { BackgroundColor = Color.Red, HeightRequest = 1 },
    new Label { Text = "EndAndExpand", BackgroundColor = Color.Gray, VerticalOptions = LayoutOptions.EndAndExpand },
    new BoxView { BackgroundColor = Color.Red, HeightRequest = 1 },
    new Label { Text = "FillAndExpand", BackgroundColor = Color.Gray, VerticalOptions = LayoutOptions.FillAndExpand },
    new BoxView { BackgroundColor = Color.Red, HeightRequest = 1 }
  }
};
```

Ten kod powoduje w układzie pokazano na poniższych zrzutach ekranu:

[![](layout-options-images/expansion.png "Rozszerzenie Opcje układu")](layout-options-images/expansion-large.png#lightbox "rozszerzenie opcje układu")

Każdy [ `Label` ](xref:Xamarin.Forms.Label) zajmuje tyle samo miejsca, w ramach [ `StackLayout` ](xref:Xamarin.Forms.StackLayout). Jednak tylko ostatni `Label`, który konfiguruje jego [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) właściwości [ `FillAndExpand` ](xref:Xamarin.Forms.LayoutOptions.FillAndExpand) ma inny rozmiar. Ponadto każdy `Label` jest oddzielona małych red [ `BoxView` ](xref:Xamarin.Forms.BoxView), umożliwiająca miejsce `Label` zajmuje się łatwo można wyświetlić.

## <a name="summary"></a>Podsumowanie

W tym artykule opisano wpływ każdy [ `LayoutOptions` ](xref:Xamarin.Forms.LayoutOptions) wartość struktura ma wyrównanie i rozszerzanie widoku, względem jego elementu nadrzędnego. `Start`, `Center`, `End`, I `Fill` pola są używane do definiowania wyrównania tego widoku w układzie nadrzędnego i `StartAndExpand`, `CenterAndExpand`, `EndAndExpand`, i `FillAndExpand` pola są używane do definiowania Preferencje wyrównanie oraz określić, czy widok, zajmują więcej miejsca, jeśli to możliwe, w ramach [ `StackLayout` ](xref:Xamarin.Forms.StackLayout).



## <a name="related-links"></a>Linki pokrewne

- [LayoutOptions (przykład)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/layoutoptions/)
- [LayoutOptions](xref:Xamarin.Forms.LayoutOptions)
