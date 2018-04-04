---
title: AbsoluteLayout
description: AbsoluteLayout umożliwia utworzenie UI idealnych pikseli.
ms.prod: xamarin
ms.assetid: 01A5CCE0-AD45-4806-84FD-72C007005B38
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/25/2015
ms.openlocfilehash: d07026fbcc43a43a9f26d85ad15d5a4e3165e2ef
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="absolutelayout"></a>AbsoluteLayout

[`AbsoluteLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/) Określa położenie i rozmiar proporcjonalny do rozmiaru i pozycji lub wartości bezwzględne elementy podrzędne. Widoki podrzędnych może być pozycjonowane i rozmiarze przy użyciu proporcjonalnych wartości lub wartości statyczne i proporcjonalne i wartości statycznych mogą być wymieszane.

[![](absolute-layout-images/layouts-sml.png "Układy platformy Xamarin.Forms")](absolute-layout-images/layouts.png#lightbox "układów platformy Xamarin.Forms")

W tym artykule opisano:

- **[Cel](#Purpose)**  &ndash; typowe zastosowania dla `AbsoluteLayout`.
- **[Użycie](#Usage)**  &ndash; sposób użycia `AbsoluteLayout` do osiągnięcia żądanej projektu.
  - **[Układy proporcjonalne](#Proportional_Layouts)**  &ndash; zrozumieć, jak proporcjonalnych wartości pracy w `AbsoluteLayout`.
  - **[Określanie wartości](#Specifying_Values)**  &ndash; zrozumieć, jak określona proporcjonalne i wartości bezwzględne.
  - **[Wartości proporcjonalne](#Proportional_Values)**  &ndash; zrozumieć, jak proporcjonalnych wartości pracy.
    - **[Wartości bezwzględne](#Absolute_Values)**  &ndash; zrozumieć, jak działają wartości bezwzględne.

<a name="Purpose" />

## <a name="purpose"></a>Cel

Z powodu pozycjonowania model `AbsoluteLayout`, układ sprawia, że stosunkowo prosta położenie elementów, dzięki czemu są one opróżniania z dowolnej strony układu i wyśrodkowany. Z proporcjonalne rozmiary i pozycje, elementy `AbsoluteLayout` może automatycznie skalować do dowolnego rozmiaru widoku. Dla elementów, w której należy skalować tylko pozycji, ale nie do rozmiaru można mieszać wartości bezwzględne i proporcjonalna.

`AbsoluteLayout` może być używane wszędzie, gdzie elementy muszą się znajdować w widoku i jest szczególnie przydatne w przypadku wyrównywanie elementów do krawędzi.

<a name="Usage" />

## <a name="usage"></a>Użycie

<a name="Proportional_Layouts" />

### <a name="proportional-layouts"></a>Układy proporcjonalne

`AbsoluteLayout` ma zgodnie z którymi zakotwiczenia elementu znajduje się względem jego elementu, ponieważ element znajduje się względem układ podczas rozmieszczania proporcjonalne jest używany model unikatowy zakotwiczenia. W przypadku bezwzględny zakotwiczenia znajduje się w pozycji (0,0) w widoku. Ma to dwie ważne konsekwencje:

- Elementy nie może znajdować się poza ekranu przy użyciu wartości proporcjonalnych.
- Elementy może być niezawodnie umieszczony dowolnej strony układu lub w Centrum, niezależnie od rozmiaru układ lub urządzenia.

`AbsoluteLayout`, takich jak `RelativeLayout`, jest w stanie umieść elementy tak, aby zachodziły na siebie.

Uwaga na poniższym zrzucie ekranu zakotwiczenia pola jest białe kropki. Zwróć uwagę relacji między kotwicą a pole przesyłane za pośrednictwem układ:

![](absolute-layout-images/anchor-start.png "Zakotwiczenia na początku")
![](absolute-layout-images/anchor-center.png "zakotwiczenia w Centrum")
![](absolute-layout-images/anchor-end.png "zakotwiczenia na końcu")

<a name="Specifying_Values" />

### <a name="specifying-values"></a>Określanie wartości

Widoki w `AbsoluteLayout` będą umieszczane przy użyciu cztery wartości:

- **X** &ndash; x (poziomy) pozycja kotwicy widoku
- **Y** &ndash; położenie y (pionowe) zakotwiczenia widoku
- **Szerokość** &ndash; szerokość widoku
- **Wysokość** &ndash; wysokości widoku

Każdy z tych wartości można ustawić jako [proporcjonalne](#Proportional_Values) wartość lub [bezwzględną](#Absolute_Values) wartość.

Wartości są określane jako połączenie granice i flagę. `LayoutBounds` jest [ `Rectangle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Rectangle/) składające się z czterech wartości: `x`, `y`, `width`, `height`.

### <a name="absolutelayoutflags"></a>AbsoluteLayoutFlags

[`AbsoluteLayoutFlags`](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayoutFlags/) Określa, jak będą interpretowane wartości i zawiera następujące wstępnie zdefiniowane opcje:

- **Brak** &ndash; interpretuje wszystkie wartości jako bezwzględną. Jest to wartość domyślna, jeśli nie określono żadnych flag układu.
- **Wszystkie** &ndash; interpretuje wszystkie wartości jako proporcjonalnych.
- **WidthProportional** &ndash; interpretuje `Width` wartość jako proporcjonalne i wszystkie inne wartości jako bezwzględną.
- **HeightProportional** &ndash; interpretuje tylko wartość wysokości jako proporcjonalne z wszystkich innych wartości bezwzględnej.
- **XProportional** &ndash; interpretuje `X` wartość jako proporcjonalne, podczas traktowanie wszystkich pozostałych wartości jako bezwzględną.
- **YProportional** &ndash; interpretuje `Y` wartość jako proporcjonalne, podczas traktowanie wszystkich pozostałych wartości jako bezwzględną.
- **PositionProportional** &ndash; interpretuje `X` i `Y` wartości jako proporcjonalne, gdy wartości rozmiaru są interpretowane jako bezwzględną.
- **SizeProportional** &ndash; interpretuje `Width` i `Height` wartości jako proporcjonalne wartości pozycji są bezwzględne.

W języku XAML, granice i flagi są ustawiane jako część definicji widoków w układzie, przy użyciu `AbsoluteLayout.LayoutBounds` właściwości. Granice są ustawione jako listę wartości rozdzielaną przecinkami `X`, `Y`, `Width`, i `Height`, w tej kolejności. Flagi są również określony w deklaracji widoków przy użyciu układu `AbsoluteLayout.LayoutFlags` właściwości. Należy pamiętać, że flagi można łączyć w języku XAML, używając listy rozdzielanej przecinkami. Rozważmy następujący przykład:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="LayoutSamples.AbsoluteLayoutExploration"
Title="Absolute Layout Exploration">
    <ContentPage.Content>
        <AbsoluteLayout>
            <Label Text="I'm centered on iPhone 4 but no other device"
                AbsoluteLayout.LayoutBounds="115,150,100,100" LineBreakMode="WordWrap"  />
            <Label Text="I'm bottom center on every device."
                AbsoluteLayout.LayoutBounds=".5,1,.5,.1" AbsoluteLayout.LayoutFlags="All"
                LineBreakMode="WordWrap"  />
            <BoxView Color="Olive"  AbsoluteLayout.LayoutBounds="1,.5, 25, 100"
                AbsoluteLayout.LayoutFlags="PositionProportional" />
            <BoxView Color="Red" AbsoluteLayout.LayoutBounds="0,.5,25,100"
                AbsoluteLayout.LayoutFlags="PositionProportional" />
            <BoxView Color="Blue" AbsoluteLayout.LayoutBounds=".5,0,100,25"
                AbsoluteLayout.LayoutFlags="PositionProportional" />
        </AbsoluteLayout>
    </ContentPage.Content>
</ContentPage>
```

![](absolute-layout-images/exploration.png "Przykłady AbsoluteLayout")

Należy pamiętać o następujących kwestiach:

- Etykieta w Centrum znajduje się za pomocą bezwzględnej wartości rozmiaru i pozycji. Z tego powodu prawdopodobnie jest wyśrodkowane na iPhone 4S i niższe, ale nie jest wyśrodkowane na większych urządzeń.
- Tekst u dołu układ znajduje się za pomocą proporcjonalne wartości rozmiaru i pozycji. Zawsze pojawią się na środku w dolnej układ, ale jego rozmiar będzie rosnąć o dużym rozmiarze układu.
- Trzy kolorze `BoxView`s znajdują się w górnym, lewej i prawej krawędzi ekranu przy użyciu proporcjonalne położenie i rozmiar bezwzględny.

Poniżej osiąga ten sam układ w języku C#:

```csharp
public class AbsoluteLayoutExplorationCode : ContentPage
{
    public AbsoluteLayoutExplorationCode ()
    {
        Title = "Absolute Layout Exploration - Code";
        var layout = new AbsoluteLayout();

        var centerLabel = new Label {
        Text = "I'm centered on iPhone 4 but no other device.",
        LineBreakMode = LineBreakMode.WordWrap};

        AbsoluteLayout.SetLayoutBounds (centerLabel, new Rectangle (115, 159, 100, 100));
        // No need to set layout flags, absolute positioning is the default

        var bottomLabel = new Label { Text = "I'm bottom center on every device.", LineBreakMode = LineBreakMode.WordWrap };
        AbsoluteLayout.SetLayoutBounds (bottomLabel, new Rectangle (.5, 1, .5, .1));
        AbsoluteLayout.SetLayoutFlags (bottomLabel, AbsoluteLayoutFlags.All);

        var rightBox = new BoxView{ Color = Color.Olive };
        AbsoluteLayout.SetLayoutBounds (rightBox, new Rectangle (1, .5, 25, 100));
        AbsoluteLayout.SetLayoutFlags (rightBox, AbsoluteLayoutFlags.PositionProportional);

        var leftBox = new BoxView{ Color = Color.Red };
        AbsoluteLayout.SetLayoutBounds (leftBox, new Rectangle (0, .5, 25, 100));
        AbsoluteLayout.SetLayoutFlags (leftBox, AbsoluteLayoutFlags.PositionProportional);

        var topBox = new BoxView{ Color = Color.Blue };
        AbsoluteLayout.SetLayoutBounds (topBox, new Rectangle (.5, 0, 100, 25));
        AbsoluteLayout.SetLayoutFlags (topBox, AbsoluteLayoutFlags.PositionProportional);

        layout.Children.Add (bottomLabel);
        layout.Children.Add (centerLabel);
        layout.Children.Add (rightBox);
        layout.Children.Add (leftBox);
        layout.Children.Add (topBox);

        Content = layout;
    }
}
```
<a name="Proportional_Values" />

### <a name="proportional-values"></a>Proporcjonalnych wartości

Wartości proporcjonalne zdefiniować relację między układ i widokiem. Ta relacja definiuje pozycji widok podrzędny lub wartość skali w stosunku do odpowiedniej wartości układu nadrzędnej. Te wartości są wyrażane jako `double`s o wartości od 0 do 1.

Proporcjonalnych wartości są używane do położenie i rozmiar widokach w ramach układu. Tak, gdy szerokość widoku jest ustawiona jako część, wartość szerokości wynikowe jest udział pomnożona przez `AbsoluteLayout`jego szerokości. Na przykład z `AbsoluteLayout` szerokości `500` i widoku ustawioną proporcjonalne szerokość.5, szerokość renderowany widok będzie 250 (500 x.5).

Aby użyć wartości proporcjonalne, ustaw `LayoutBounds` przy użyciu (x, y) proporcje i proporcjonalne rozmiary, następnie ustaw `LayoutFlags` do `All`.

W języku XAML:

```xaml
<Label Text="I'm bottom center on every device."
  AbsoluteLayout.LayoutBounds=".5,1,.5,.1" AbsoluteLayout.LayoutFlags="All"  />
```

W języku C#:

```csharp
var label = new Label {Text = "I'm bottom center on every device."};
AbsoluteLayout.SetLayoutBounds(label, new Rectangle(.5,1,.5,.1));
AbsoluteLayout.SetLayoutFlags(label, AbsoluteLayoutFlags.All);
```

<a name="Absolute_Values" />

### <a name="absolute-values"></a>Wartości bezwzględne

Wartości bezwzględne jawnie definiować, gdzie widoków powinien być umieszczony w układzie. W odróżnieniu od wartości proporcjonalne wartości bezwzględne są w stanie pozycjonowanie i rozmiary widoku, który nie mieści się w granicach układu.

Przy użyciu wartości bezwzględne dla pozycjonowanie może być niebezpieczne, gdy rozmiar układ jest nieznana. Używając położenia bezwzględne, elementu w środku ekranu w rozmiarze co można przesunąć o dowolnym rozmiarze. Należy koniecznie testowanie aplikacji w różnych rozmiarów ekranu obsługiwanych urządzeń.

Aby użyć wartości bezwzględnej układu, należy ustawić `LayoutBounds` przy użyciu (x, y) współrzędnych i jawnych rozmiarów, następnie ustaw `LayoutFlags` do `None`.

W języku XAML:

```xaml
<Label Text="I'm centered on iPhone 4 but no other device."
  AbsoluteLayout.LayoutBounds="115,150,100,100"  />
```

W języku C#:

```csharp
var label = new Label {Text = "I'm centered on iPhone 4 but no other device."};
AbsoluteLayout.SetLayoutBounds(label, new Rectangle(115,150,100,100));
```

## <a name="exploring-a-complex-layout"></a>Poznawanie złożonego układu

Układy mieć najważniejsze wady i zalety tworzenia konkretnego układów. W tej serii artykułów układu Przykładowa aplikacja została utworzona z ten sam układ strony implementowane przy użyciu trzech różnych układów.

Należy wziąć pod uwagę następujące XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="TheBusinessTumble.AbsoluteLayoutPage"
Title="AbsoluteLayout">
    <ContentPage.ToolbarItems>
        <ToolbarItem Text="Save" />
    </ContentPage.ToolbarItems>
    <ContentPage.Content>
        <ScrollView>
            <AbsoluteLayout BackgroundColor="Maroon">
                <BoxView BackgroundColor="Gray" AbsoluteLayout.LayoutBounds="0
                    0,1,100" AbsoluteLayout.LayoutFlags="XProportional,YProportional,WidthProportional" />
                <Button BackgroundColor="Maroon"
                    AbsoluteLayout.LayoutBounds=".5,55,70,70" AbsoluteLayout.LayoutFlags="XProportional" BorderRadius="35" />
                <Button BackgroundColor="Red" AbsoluteLayout.LayoutBounds=".5
                    60,60,60" AbsoluteLayout.LayoutFlags="XProportional" BorderRadius="30" />
                <Label Text="User Name" FontAttributes="Bold" FontSize="26"
                    TextColor="Black" HorizontalTextAlignment="Center" AbsoluteLayout.LayoutBounds=".5,140,1,40" AbsoluteLayout.LayoutFlags="XProportional,WidthProportional" />
                <Entry Text="Bio + Hashtags" TextColor="White"
                    BackgroundColor="Maroon" AbsoluteLayout.LayoutBounds=".5,180,1,40" AbsoluteLayout.LayoutFlags="XProportional,WidthProportional" />
                <AbsoluteLayout BackgroundColor="White"
                        AbsoluteLayout.LayoutBounds="0, 220, 1, 50" AbsoluteLayout.LayoutFlags="XProportional,WidthProportional">
                    <AbsoluteLayout AbsoluteLayout.LayoutBounds="0,0,.5,1" AbsoluteLayout.LayoutFlags="WidthProportional,HeightProportional">
                        <Button BackgroundColor="Black" BorderRadius="20"
                            AbsoluteLayout.LayoutBounds="5,.5,40,40" AbsoluteLayout.LayoutFlags="YProportional" />
                        <Label Text="Accent Color" TextColor="Black"
                            AbsoluteLayout.LayoutBounds="50,.55,1,25" AbsoluteLayout.LayoutFlags="YProportional,WidthProportional" />
                    </AbsoluteLayout>
                    <AbsoluteLayout AbsoluteLayout.LayoutBounds="1,0,.5,1" AbsoluteLayout.LayoutFlags="WidthProportional,HeightProportional,XProportional">
                        <Button BackgroundColor="Maroon" BorderRadius="20"
                            AbsoluteLayout.LayoutBounds="5,.5,40,40" AbsoluteLayout.LayoutFlags="YProportional" />
                        <Label Text="Primary Color" TextColor="Black"
                            AbsoluteLayout.LayoutBounds="50,.55,1,25" AbsoluteLayout.LayoutFlags="YProportional,WidthProportional" />
                    </AbsoluteLayout>
                </AbsoluteLayout>
                <AbsoluteLayout AbsoluteLayout.LayoutBounds="0,270,1,50" AbsoluteLayout.LayoutFlags="WidthProportional" Padding="5,0,0,0">
                    <Label Text="Age:" TextColor="White"
                        AbsoluteLayout.LayoutBounds="0,25,.25,50" AbsoluteLayout.LayoutFlags="WidthProportional" />
                    <Entry Text="35" TextColor="White" BackgroundColor="Maroon"
                        AbsoluteLayout.LayoutBounds="1,10,.75,50" AbsoluteLayout.LayoutFlags="XProportional,WidthProportional" />
                </AbsoluteLayout>
                <AbsoluteLayout AbsoluteLayout.LayoutBounds="0,320,1,50" AbsoluteLayout.LayoutFlags="WidthProportional" Padding="5,0,0,0">
                    <Label Text="Interests:" TextColor="White"
                        AbsoluteLayout.LayoutBounds="0,25,.25,50" AbsoluteLayout.LayoutFlags="WidthProportional" />
                    <Entry Text="Xamarin.Forms" TextColor="White"
                        BackgroundColor="Maroon" AbsoluteLayout.LayoutBounds="1,10,.75,50" AbsoluteLayout.LayoutFlags="XProportional,WidthProportional" />
                </AbsoluteLayout>
                <AbsoluteLayout AbsoluteLayout.LayoutBounds="0,370,1,50" AbsoluteLayout.LayoutFlags="WidthProportional" Padding="5,0,0,0">
                    <Label Text="Ask me about:" TextColor="White"
                        AbsoluteLayout.LayoutBounds="0,25,.25,50" AbsoluteLayout.LayoutFlags="WidthProportional" />
                    <Entry Text="Xamarin, C#, .NET, Mono" TextColor="White"
                        BackgroundColor="Maroon" AbsoluteLayout.LayoutBounds="1,10,.75,50" AbsoluteLayout.LayoutFlags="XProportional,WidthProportional" />
                </AbsoluteLayout>
            </AbsoluteLayout>
        </ScrollView>
    </ContentPage.Content>
</ContentPage>
```

Powyższy kod wyniki w układzie następujące:

![](absolute-layout-images/abs.png "AbsoluteLayout złożonych")

Należy zauważyć, że z powodu różnic w sposób renderowania przycisków przez Windows Phone, niektóre kółka zostały zastąpione przez boxviews na zrzucie ekranu Windows Phone.

Zwróć uwagę, że `AbsoluteLayout`s są zagnieżdżone, ponieważ w niektórych przypadkach zagnieżdżania układów mogą być łatwiejsze niż przedstawienie wszystkich elementów w ten sam układ.



## <a name="related-links"></a>Linki pokrewne

- [Tworzenie aplikacji mobilnych za pomocą platformy Xamarin.Forms, rozdział 14](https://developer.xamarin.com/r/xamarin-forms/book/chapter14.pdf)
- [AbsoluteLayout](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/)
- [Układ (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [Przykład BusinessTumble (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
