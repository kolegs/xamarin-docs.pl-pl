---
title: AbsoluteLayout zestawu narzędzi Xamarin.Forms
description: W tym artykule wyjaśniono, jak korzystać z zestawu narzędzi Xamarin.Forms AbsoluteLayout klasy tworzyć interfejsy doskonałe rozwiązanie pikseli. Ta klasa określa położenie i rozmiar proporcjonalny do rozmiaru i położenia lub wartości bezwzględne elementów podrzędnych.
ms.prod: xamarin
ms.assetid: 01A5CCE0-AD45-4806-84FD-72C007005B38
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/25/2015
ms.openlocfilehash: 0d49b8c50db08ad07952425492591ee246de4f8b
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998351"
---
# <a name="xamarinforms-absolutelayout"></a>AbsoluteLayout zestawu narzędzi Xamarin.Forms

[`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout) Określa położenie i rozmiar proporcjonalny do rozmiaru i położenia lub wartości bezwzględne elementów podrzędnych. Widoki podrzędnych może być osiągniesz idealną pozycję i wielkości, przy użyciu proporcjonalnych wartości lub wartości statyczne i proporcjonalna i wartości statyczne mogą być mieszane.

[![](absolute-layout-images/layouts-sml.png "Układy platformy Xamarin.Forms")](absolute-layout-images/layouts.png#lightbox "układy platformy Xamarin.Forms")

W tym artykule omówiono:

- **[Cel](#Purpose)**  &ndash; typowe zastosowania dla `AbsoluteLayout`.
- **[Użycie](#Usage)**  &ndash; sposób używania `AbsoluteLayout` do osiągnięcia żądanego projektu.
  - **[Układy proporcjonalna](#Proportional_Layouts)**  &ndash; zrozumieć, jak proporcjonalnych wartości działają w `AbsoluteLayout`.
  - **[Określanie wartości](#Specifying_Values)**  &ndash; zrozumieć, jak określono proporcjonalnego i wartości bezwzględne.
  - **[Wartości proporcjonalna](#Proportional_Values)**  &ndash; zrozumieć, jak proporcjonalnych wartości pracy.
    - **[Wartości bezwzględne](#Absolute_Values)**  &ndash; zrozumieć, jak działają wartości bezwzględne.

<a name="Purpose" />

## <a name="purpose"></a>Cel

Ze względu na model pozycjonowania `AbsoluteLayout`, układ sprawia, że stosunkowo prosta do pozycji elementów, aby były one w dowolnej stronie układu, lub do środka. Za pomocą proporcjonalna rozmiary i położenie, elementy w `AbsoluteLayout` mogą automatycznie przeprowadzać skalowanie do dowolnej wielkości widoku. Dla elementów, w którym powinny być skalowane jedynie stanowiska, ale nie do rozmiaru mogą być mieszane wartości bezwzględne i proporcjonalne.

`AbsoluteLayout` może być używane wszędzie, gdzie elementy muszą się znajdować w widoku i jest szczególnie przydatna podczas wyrównywanie elementów do krawędzi.

<a name="Usage" />

## <a name="usage"></a>Użycie

<a name="Proportional_Layouts" />

### <a name="proportional-layouts"></a>Proporcjonalna układów

`AbsoluteLayout` ma unikatowe kotwica (model), zgodnie z którą jest umieszczony zakotwiczenia elementu względem jego elementu zgodnie z tego element jest pozycjonowany względem układ stosowania proporcjonalna pozycjonowanie. W przypadku bezwzględny zakotwiczenie wynosi (0,0) w widoku. To ma dwie ważne konsekwencje:

- Elementy nie może znajdować się poza ekranem przy użyciu wartości proporcjonalne.
- Elementy może być niezawodnie umieszczony dowolnej strony układu lub w Centrum, bez względu na rozmiar układu lub urządzenia.

`AbsoluteLayout`, takich jak `RelativeLayout`, jest w stanie umieść elementy nakładają się.

Uwaga na następującym zrzucie ekranu zakotwiczenia pole jest białe kropka. Zwróć uwagę relacji między zakotwiczenie i pola, kiedy przesuwa się on za pośrednictwem układu:

![](absolute-layout-images/anchor-start.png "Zakotwiczenia przy uruchamianiu")
![](absolute-layout-images/anchor-center.png "zakotwiczenia pośrodku")
![](absolute-layout-images/anchor-end.png "kotwicy na końcu")

<a name="Specifying_Values" />

### <a name="specifying-values"></a>Określanie wartości

Widoków w obrębie `AbsoluteLayout` są umieszczone za pomocą czterech wartości:

- **X** &ndash; pozycji (poziomy) x kotwicy tego widoku
- **Y** &ndash; pozycję w osi y (w pionie) kotwicy tego widoku
- **Szerokość** &ndash; szerokość widoku
- **Wysokość** &ndash; wysokości widoku

Każda z tych wartości może być ustawiona jako [proporcjonalna](#Proportional_Values) wartość lub [bezwzględne](#Absolute_Values) wartość.

Wartości są określane jako połączenie granice i flagi. `LayoutBounds` jest [ `Rectangle` ](xref:Xamarin.Forms.Rectangle) składający się z czterech wartości: `x`, `y`, `width`, `height`.

### <a name="absolutelayoutflags"></a>AbsoluteLayoutFlags

[`AbsoluteLayoutFlags`](xref:Xamarin.Forms.AbsoluteLayoutFlags) Określa, jak interpretować wartości i ma następujące wstępnie zdefiniowane opcje:

- **Brak** &ndash; interpretuje wszystkie wartości jako bezwzględne. Jest wartością domyślną, jeśli nie określono żadnych flag układu.
- **Wszystkie** &ndash; interpretuje wszystkie wartości jako proporcjonalne.
- **WidthProportional** &ndash; interpretuje `Width` wartości jako proporcjonalne i inne wartości jako bezwzględne.
- **HeightProportional** &ndash; interpretuje tylko wysokość wartości jako proporcjonalne ze wszystkich pozostałych wartości bezwzględnej.
- **XProportional** &ndash; interpretuje `X` wartości jako proporcjonalne, w czasie traktowania wszystkich pozostałych wartości jako bezwzględne.
- **YProportional** &ndash; interpretuje `Y` wartości jako proporcjonalne, w czasie traktowania wszystkich pozostałych wartości jako bezwzględne.
- **PositionProportional** &ndash; interpretuje `X` i `Y` wartości jako proporcjonalne, podczas gdy wartości rozmiarów są interpretowane jako bezwzględne.
- **SizeProportional** &ndash; interpretuje `Width` i `Height` wartości jako proporcjonalne wartości pozycji są bezwzględne.

W XAML, granice i flagi są ustawione jako część definicji widoków w układzie, za pomocą `AbsoluteLayout.LayoutBounds` właściwości. Granice są ustawione jako listę wartości rozdzielonych przecinkami `X`, `Y`, `Width`, i `Height`, w tej kolejności. Flagi również są określone w deklaracji widoków przy użyciu układu `AbsoluteLayout.LayoutFlags` właściwości. Należy pamiętać, że można łączyć flagi w XAML przy użyciu listy rozdzielonych przecinkami. Rozważmy następujący przykład:

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

- Etykieta w środku znajduje się za pomocą bezwzględnej wartości rozmiaru i położenia. Z tego powodu wygląda na to wyśrodkowane na telefonie iPhone 4S i niższy, ale nie wyśrodkowane na większych urządzeniach.
- Tekst u dołu układ znajduje się za pomocą proporcjonalnych wartości rozmiaru i położenia. Zawsze będzie wyglądać na dole na środku układu, ale jego rozmiar będzie rosnąć o dużym rozmiarze układu.
- Trzy kolorowe `BoxView`s są umieszczone na krawędzi górnego, lewego i prawego na ekranie za pomocą proporcjonalna położenie i rozmiar bezwzględne.

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

### <a name="proportional-values"></a>Proporcjonalna wartości

Wartości proporcjonalna zdefiniowanie relacji między układ i widokiem. Ta relacja definiuje położenie widok podrzędny i wartość skalowania w stosunku do odpowiedniej wartości układu nadrzędnej. Te wartości są wyrażane jako `double`s przy użyciu wartości z zakresu od 0 do 1.

Proporcjonalna wartości są używane do położenie i rozmiar widoków w układzie. Dlatego po ustawieniu szerokości widoku w stosunku do wartości wynikowe szerokość to pomnożona przez `AbsoluteLayout`jego szerokość. Na przykład za pomocą `AbsoluteLayout` szerokości `500` widoku ustawić proporcjonalna szerokość.5, szerokość renderowanym widoku. zostanie ona 250 (500 x.5).

Aby użyć wartości proporcjonalna, ustaw `LayoutBounds` przy użyciu (x, y) proporcji i rozmiarach proporcjonalna, następnie ustawić `LayoutFlags` do `All`.

W XAML:

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

Wartości bezwzględne jawne zdefiniowanie, gdzie widoków powinien znajdować się w układzie. W przeciwieństwie do wartości proporcjonalna wartości bezwzględne są w stanie Zmienianie położenia i rozmiaru widoku, który nie mieści się w granicach układu.

Przy użyciu wartości bezwzględne dla pozycjonowanie może być niebezpieczne, gdy rozmiar układ jest nieznany. Korzystając z położenia bezwzględne, element w środku ekranu w jedno rozwiązanie może przesunięcia o innym rozmiarze. Należy przetestować aplikację dla różnych rozmiarów ekranu obsługiwanych urządzeń.

Aby użyć wartości bezwzględnej układu, ustaw `LayoutBounds` przy użyciu (x, y) współrzędnych i jawnych rozmiarów, następnie ustawić `LayoutFlags` do `None`.

W XAML:

```xaml
<Label Text="I'm centered on iPhone 4 but no other device."
  AbsoluteLayout.LayoutBounds="115,150,100,100"  />
```

W języku C#:

```csharp
var label = new Label {Text = "I'm centered on iPhone 4 but no other device."};
AbsoluteLayout.SetLayoutBounds(label, new Rectangle(115,150,100,100));
```

## <a name="exploring-a-complex-layout"></a>Eksplorowanie układ złożony

Układy mają zalety i słabe strony dotyczące tworzenia układów określonej. W tej serii artykułów układu utworzono przykładową aplikację przy użyciu tego samego układu strony implementowane przy użyciu trzech różnych układów.

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

Powyższy kod powoduje w układzie następujących:

![](absolute-layout-images/abs.png "Złożone AbsoluteLayout")

Należy zauważyć, że `AbsoluteLayout`s są zagnieżdżone, ponieważ w niektórych przypadkach zagnieżdżania układy może być łatwiejsze niż prezentowanie wszystkich elementów w obrębie tego samego układu.

## <a name="related-links"></a>Linki pokrewne

- [Tworzenie aplikacji mobilnych za pomocą zestawu narzędzi Xamarin.Forms rozdział 14](https://developer.xamarin.com/r/xamarin-forms/book/chapter14.pdf)
- [AbsoluteLayout](xref:Xamarin.Forms.AbsoluteLayout)
- [Układ (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [Przykład BusinessTumble (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
