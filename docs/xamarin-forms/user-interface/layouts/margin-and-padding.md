---
title: Margines i wypełnienie
description: Margines i wypełnienie właściwości Sterowanie zachowaniem układ, gdy element jest wyświetlana w interfejsie użytkownika. W tym artykule przedstawiono różnią się dwie właściwości i sposobu ich ustawiania.
ms.prod: xamarin
ms.assetid: BEB096BB-51DF-410F-B0F1-D235287B0F4A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/27/2016
ms.openlocfilehash: 595e673c59d23a45cbaf923a0d58faff2000c296
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996619"
---
# <a name="margin-and-padding"></a>Margines i wypełnienie

_Margines i wypełnienie właściwości Sterowanie zachowaniem układ, gdy element jest wyświetlana w interfejsie użytkownika. W tym artykule przedstawiono różnią się dwie właściwości i sposobu ich ustawiania._

## <a name="overview"></a>Omówienie

Margines i wypełnienie to pojęcia pokrewne układu:

- [ `Margin` ](xref:Xamarin.Forms.View.Margin) Właściwość reprezentuje odległość między elementem i jego sąsiadujące elementy i jest używane do kontrolowania miejsce renderowania elementu i jego sąsiadami miejsce renderowania. `Margin` można określić wartości na [układ](~/xamarin-forms/user-interface/controls/layouts.md) i [widoku](~/xamarin-forms/user-interface/controls/views.md) klasy.
- [ `Padding` ](xref:Xamarin.Forms.Layout.Padding) Właściwość reprezentuje odległość między elementem i jego elementy podrzędne i jest używany do oddzielania kontrolki z własnej zawartości. `Padding` można określić wartości na [układ](~/xamarin-forms/user-interface/controls/layouts.md) klasy.

Na poniższym diagramie przedstawiono dwa pojęcia:

[![](margin-and-padding-images/margins-and-padding-sml.png "Marginesy i dopełnienie pojęcia")](margin-and-padding-images/margins-and-padding.png#lightbox "marginesy i dopełnienie pojęcia")

Należy pamiętać, że [ `Margin` ](xref:Xamarin.Forms.View.Margin) wartości są dodatku. W związku z tym jeśli dwa sąsiadujące elementy określić margines 20 pikseli, odległości między elementami będzie 40 pikseli. Ponadto margines i wypełnienie są addytywne, gdy jednocześnie są stosowane, w tym odległość między element zawartości będzie margines i wypełnienie.

## <a name="specifying-a-thickness"></a>Określanie grubości

[ `Margin` ](xref:Xamarin.Forms.View.Margin) i [ `Padding` ](xref:Xamarin.Forms.Layout.Padding) właściwości są oba typu [ `Thickness` ](xref:Xamarin.Forms.Thickness). Istnieją trzy możliwości, tworząc `Thickness` strukturę:

- Tworzenie [ `Thickness` ](xref:Xamarin.Forms.Thickness) strukturę zdefiniowaną przez pojedynczą wartość jednolite. Pojedynczą wartość jest stosowane po lewej stronie, u góry, prawej strony i dolne krawędzie elementu.
- Tworzenie [ `Thickness` ](xref:Xamarin.Forms.Thickness) strukturę zdefiniowaną przez poziome i pionowe wartości. Poziomy wartość symetrycznie jest stosowany do lewej i prawej stronie elementu, wartością pionowy symetrycznie stosowane do górnej i dolnej krawędzi elementu.
- Tworzenie [ `Thickness` ](xref:Xamarin.Forms.Thickness) strukturę zdefiniowaną przez cztery różne wartości, które są stosowane po lewej stronie, u góry, prawej strony i dolne krawędzie elementu.

Poniższy kod XAML zawiera wszystkie trzy możliwości:

```xaml
<StackLayout Padding="0,20,0,0">
  <Label Text="Xamarin.Forms" Margin="20" />
  <Label Text="Xamarin.iOS" Margin="10, 15" />
  <Label Text="Xamarin.Android" Margin="0, 20, 15, 5" />
</StackLayout>
```

Równoważny kod C# pokazano w poniższym przykładzie kodu:

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
> `Thickness` wartości mogą być ujemne, które zazwyczaj Przycina lub overdraws zawartości.

## <a name="summary"></a>Podsumowanie

W tym artykule pokazano różnicę między [ `Margin` ](xref:Xamarin.Forms.View.Margin) i [ `Padding` ](xref:Xamarin.Forms.Layout.Padding) właściwości i sposobu ich ustawiania. Właściwości kontrolować układ zachowanie, gdy element jest wyświetlana w interfejsie użytkownika.


## <a name="related-links"></a>Linki pokrewne

- [Margines](xref:Xamarin.Forms.View.Margin)
- [Dopełnienie](xref:Xamarin.Forms.Layout.Padding)
- [Grubość](xref:Xamarin.Forms.Thickness)
