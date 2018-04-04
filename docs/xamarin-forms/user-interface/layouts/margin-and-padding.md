---
title: Margines i wypełnienie
description: Margines i wypełnienie właściwości kontrolowania zachowania układu, gdy element jest renderowany w interfejsie użytkownika. W tym artykule przedstawiono różnicę między dwoma właściwości i sposobu ich ustawiania.
ms.prod: xamarin
ms.assetid: BEB096BB-51DF-410F-B0F1-D235287B0F4A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/27/2016
ms.openlocfilehash: 545468d3b02f9651c45fcaebe159351aafea6432
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="margin-and-padding"></a>Margines i wypełnienie

_Margines i wypełnienie właściwości kontrolowania zachowania układu, gdy element jest renderowany w interfejsie użytkownika. W tym artykule przedstawiono różnicę między dwoma właściwości i sposobu ich ustawiania._

## <a name="overview"></a>Omówienie

Margines i wypełnienie są powiązane układu pojęcia:

- [ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) Właściwość reprezentuje odległość między elementu i jego elementy sąsiednie i jest używana do sterowania miejsce renderowania elementu i miejsce renderowania jej sąsiadami. `Margin` można określić wartości [układu](~/xamarin-forms/user-interface/controls/layouts.md) i [widoku](~/xamarin-forms/user-interface/controls/views.md) klasy.
- [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/) Właściwość reprezentuje odległość między elementu i jego elementów podrzędnych i jest używany do rozdzielania formantu z własną zawartość. `Padding` można określić wartości [układu](~/xamarin-forms/user-interface/controls/layouts.md) klasy.

Na poniższym diagramie przedstawiono dwa pojęcia:

[![](margin-and-padding-images/margins-and-padding-sml.png "Marginesy i dopełnienia pojęcia")](margin-and-padding-images/margins-and-padding.png#lightbox "marginesy i dopełnienia pojęcia")

Należy pamiętać, że [ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) wartości są dodatku. W związku z tym jeśli dwa elementy sąsiednie określić margines 20 pikseli, odległość między elementami będzie 40 pikseli. Ponadto, czy margines i wypełnienie dodatku, gdy oba są stosowane, w tym odległość między elementem i zawartość będzie marginesu oraz dopełnienie.

## <a name="specifying-a-thickness"></a>Określanie grubości

[ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) i [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/) właściwości są typu [ `Thickness` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/). Istnieją trzy możliwości podczas tworzenia `Thickness` struktury:

- Utwórz [ `Thickness` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/) struktury zdefiniowane przez pojedynczą wartość uniform. Pojedynczą wartość jest stosowany do lewej, górnej, prawej i dolnej krawędzi elementu.
- Utwórz [ `Thickness` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/) struktury zdefiniowane przez wartości w poziomie i w pionie. Poziomy wartość symetrycznie jest stosowany do lewej i prawej stronie elementu z wartością pionowy symetrycznie są stosowane do górnej i dolnej krawędzi elementu.
- Utwórz [ `Thickness` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/) struktury zdefiniowane przez cztery różne wartości, które są stosowane do lewej, górnej, prawej i dolnej krawędzi elementu.

Poniższy przykładowy kod XAML zawiera wszystkie trzy możliwości:

```xaml
<StackLayout Padding="0,20,0,0">
  <Label Text="Xamarin.Forms" Margin="20" />
  <Label Text="Xamarin.iOS" Margin="10, 15" />
  <Label Text="Xamarin.Android" Margin="0, 20, 15, 5" />
</StackLayout>
```

W poniższym przykładzie kodu pokazano równoważne kodu C#:

```csharp
var stackLayout = new StackLayout {
  Padding = new Thickness(0,20,0,0),
  Children = {
    new Label { Text = "Xamarin.Forms", Margin = new Thickness (20) },
    new Label { Text = "Xamarin.iOS", Margin = new Thickness (10, 25) },
    new Label { Text = "Xamarin.Android", Margin = new Thickness (0, 20, 15, 5) }
  }
};
```

> [!NOTE]
> `Thickness` wartości może być ujemna, które zwykle klipy lub overdraws zawartości.

## <a name="summary"></a>Podsumowanie

W tym artykule przedstawiono różnice między [ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) i [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/) właściwości i sposobu ich ustawiania. Właściwości kontrolować zachowanie układu elementu jest renderowany w interfejsie użytkownika.


## <a name="related-links"></a>Linki pokrewne

- [Margin](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/)
- [Dopełnienie](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/)
- [Grubość](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/)
