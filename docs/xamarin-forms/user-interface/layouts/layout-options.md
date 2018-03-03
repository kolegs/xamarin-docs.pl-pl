---
title: LayoutOptions
description: "Każdy widok platformy Xamarin.Forms ma właściwości HorizontalOptions i VerticalOptions typu LayoutOptions. W tym artykule opisano wpływ każdej wartości LayoutOptions na wyrównanie i rozszerzenia widoku."
ms.topic: article
ms.prod: xamarin
ms.assetid: 7CAB5631-5153-4DEF-8AD7-C6011CE44307
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/10/2017
ms.openlocfilehash: 978985c4e9803fad33760e4b40ab73d57f3ec420
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="layoutoptions"></a>LayoutOptions

_Każdy widok platformy Xamarin.Forms ma właściwości HorizontalOptions i VerticalOptions typu LayoutOptions. W tym artykule opisano wpływ każdej wartości LayoutOptions na wyrównanie i rozszerzenia widoku._

## <a name="overview"></a>Omówienie

[ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/) Struktury hermetyzuje dwóch preferencje układu:

- **Wyrównanie** — widok preferowana przez wyrównania, który określa jej położenie i rozmiar w jego układ nadrzędnej.
- **Rozszerzenia** — używane tylko przez [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/)oraz wskazuje, jeśli widok należy użyć dodatkowe miejsce, jeśli jest dostępna.

Te preferencje układ można zastosować do [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/), względem jego elementu nadrzędnego, ustawiając [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) lub [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) właściwości `View` do jednego pola publiczne z [ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/) struktury. Pola publiczne są następujące:

- [`Start`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Start/)
- [`Center`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Center/)
- [`End`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.End/)
- [`Fill`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Fill/)
- [`StartAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.StartAndExpand/)
- [`CenterAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.CenterAndExpand/)
- [`EndAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.EndAndExpand/)
- [`FillAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.FillAndExpand/)

`Start`, `Center`, `End`, I `Fill` pola są używane do definiowania widoku wyrównania w układzie nadrzędnej:

- Wyrównania poziomego [ `Start` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Start/) pozycji [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) na lewą stronę układu nadrzędny i wyrównanie w pionie, umieszcza `View` u góry Układ nadrzędnej.
- Wyrównanie w poziomie i w pionie, aby uzyskać [ `Center` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Center/) koncentruje się poziomo czy pionowo [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/).
- Wyrównania poziomego [ `End` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.End/) pozycji [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) na po prawej stronie układu nadrzędny i wyrównanie w pionie, umieszcza `View` na dole układu nadrzędnej.
- Wyrównania poziomego [ `Fill` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Fill/) upewnia się, że [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) wypełnia szerokość układu nadrzędny i wyrównanie w pionie, zapewnia, że `View` wypełnia Wysokość układu nadrzędnej.

`StartAndExpand`, `CenterAndExpand`, `EndAndExpand`, I `FillAndExpand` wartości są używane do definiowania preferencji wyrównanie i określa, czy widok zajmują więcej miejsca Jeśli jest dostępny w obrębie nadrzędnego [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/).

> [!NOTE]
> Wartość domyślna w widoku [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) i [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) właściwości [ `LayoutOptions.Fill` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Fill/).

<a name="alignment" />

## <a name="alignment"></a>Wyrównanie

Określa wyrównanie jak widok przy znajduje się w jego układ nadrzędnego układ nadrzędny zawiera nieużywane miejsce (układu nadrzędny jest większy niż łączny rozmiar wszystkich jego obiektów podrzędnych).

A [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) tylko szanuje `Start`, `Center`, `End`, i `Fill` [ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/) pola podrzędnego widoki, które są w odwrotnym kierunku Aby `StackLayout` orientacji. W związku z tym podrzędnych widoków w orientacji pionowej `StackLayout` można ustawić ich [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) właściwości do jednego z `Start`, `Center`, `End`, lub `Fill` pola. Podobnie, widoki podrzędnych w orientacji poziomej `StackLayout` można ustawić ich [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) właściwości do jednego z `Start`, `Center`, `End`, lub `Fill` pola.

A [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) nie przestrzega `Start`, `Center`, `End`, i `Fill` [ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/) pola podrzędnego widoków, które znajdują się w tym samym kierunku co `StackLayout` orientacji. W związku z tym orientacji pionowej `StackLayout` ignoruje `Start`, `Center`, `End`, lub `Fill` pola zostaną ustawione na [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) właściwości widoków podrzędnych. Podobnie, orientacji poziomej `StackLayout` ignoruje `Start`, `Center`, `End`, lub `Fill` pola zostaną ustawione na [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) właściwości widoków podrzędnych.

> [!NOTE]
> [`LayoutOptions.Fill`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Fill/) Zazwyczaj zastąpienia rozmiar określony przy użyciu żądania [ `HeightRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.HeightRequest/) i [ `WidthRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.WidthRequest/) właściwości.

W poniższym przykładzie kodu XAML pokazano orientacji pionowej [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) gdzie każdego elementu podrzędnego [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) ustawia jego [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) właściwości jedno z pól wyrównanie cztery z [ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/) struktury:

```xaml
<StackLayout Margin="0,20,0,0">
  ...
  <Label Text="Start" BackgroundColor="Gray" HorizontalOptions="Start" />
  <Label Text="Center" BackgroundColor="Gray" HorizontalOptions="Center" />
  <Label Text="End" BackgroundColor="Gray" HorizontalOptions="End" />
  <Label Text="Fill" BackgroundColor="Gray" HorizontalOptions="Fill" />
</StackLayout>
```

Poniżej przedstawiono równoważne kodu C#:

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

Kod wyniki w układzie pokazano na poniższych zrzutach ekranu:

[![](layout-options-images/alignment.png "Opcje układu wyrównania")](layout-options-images/alignment-large.png "opcji wyrównania układu")

<a name="expansion" />

## <a name="expansion"></a>Rozszerzenia

Rozszerzenia kontroluje, czy widok zajmie więcej miejsca, jeśli jest dostępny w ramach [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/). Jeśli `StackLayout` zawiera nieużywane miejsce (to znaczy `StackLayout` jest większy niż łączny rozmiar wszystkich jego elementów podrzędnych), nieużywane miejsce jest dzielone równomiernie wszystkie widoki podrzędne, które żądania rozszerzeń przez ustawienie ich [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/)lub [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) właściwości [ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/) pola, które używa `AndExpand` sufiks. Należy pamiętać, że w przypadku wszystkich miejsca w `StackLayout` jest używana, opcje rozszerzenia nie obowiązują.

A [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) można rozszerzyć tylko widoki podrzędnych w kierunku orientacji. W związku z tym orientacji pionowej `StackLayout` rozszerzyć widoki podrzędne, które ustawić ich [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) właściwości do jednego z `StartAndExpand`, `CenterAndExpand`, `EndAndExpand`, lub `FillAndExpand` pola, jeśli `StackLayout` zawiera nieużywane miejsce. Podobnie, orientacji poziomej `StackLayout` rozszerzyć widoki podrzędne, które ustawić ich [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) właściwości do jednego z `StartAndExpand`, `CenterAndExpand`, `EndAndExpand`, lub `FillAndExpand` pola, jeśli `StackLayout` zawiera nieużywane miejsce.

A [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) nie można rozwinąć widoków podrzędnych w kierunku przeciwnym orientacji. W związku z tym w orientacji pionowej `StackLayout`, ustawienie [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) właściwość widok podrzędny do [ `StartAndExpand` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.StartAndExpand/) działa tak samo jak ustawienie właściwości [ `Start`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Start/).

> [!NOTE]
> Należy pamiętać, że włączenie rozszerzenia nie zmienia rozmiar widoku, chyba że użyto [ `LayoutOptions.FillAndExpand` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.FillAndExpand/).

W poniższym przykładzie kodu XAML pokazano orientacji pionowej [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) gdzie każdego elementu podrzędnego [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) ustawia jego [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) właściwości jedno z pól rozszerzenia cztery z [ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/) struktury:

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

Poniżej przedstawiono równoważne kodu C#:

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

Kod wyniki w układzie pokazano na poniższych zrzutach ekranu:

[![](layout-options-images/expansion.png "Opcje układu rozszerzenia")](layout-options-images/expansion-large.png "opcje układu rozszerzenia")

Każdy [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) zajmuje tego samego ilość miejsca w [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/). Jednak tylko ostatni `Label`, który określa jego [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) właściwości [ `FillAndExpand` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.FillAndExpand/) ma inny rozmiar. Ponadto każdy `Label` jest oddzielona małych czerwony [ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/), co pozwala miejsce `Label` zajmuje łatwo przeglądać.

## <a name="summary"></a>Podsumowanie

W tym artykule wyjaśniono wpływ każdy [ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/) ma wartość Struktura na wyrównanie i rozszerzanie widoku względem jego elementu nadrzędnego. `Start`, `Center`, `End`, I `Fill` pola są używane do definiowania widoku wyrównania w układzie nadrzędny i `StartAndExpand`, `CenterAndExpand`, `EndAndExpand`, i `FillAndExpand` pola są używane do definiowania preferencji wyrównanie i ustalić, czy widok zajmie więcej miejsca, jeśli jest dostępny w ramach [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/).



## <a name="related-links"></a>Linki pokrewne

- [LayoutOptions (przykład)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/layoutoptions/)
- [LayoutOptions](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/)
