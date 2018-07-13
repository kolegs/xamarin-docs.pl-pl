---
title: Układy w interfejsie Xamarin.Forms
description: Zestaw narzędzi Xamarin.Forms ma kilka układy i funkcji do organizowania zawartości ekranu, a w tym artykule opisano je.
ms.prod: xamarin
ms.assetid: 65030DA3-C7C1-4A02-B478-811073C39139
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 10/26/2017
ms.openlocfilehash: cff5f9c15f4608ecfb643d2c49dd636df8b18b5c
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995758"
---
# <a name="layouts-in-xamarinforms"></a>Układy w interfejsie Xamarin.Forms

Zestaw narzędzi Xamarin.Forms ma kilka układy i funkcji do organizowania zawartości ekranu.

> [!VIDEO https://youtube.com/embed/4HlLjTZQzjM]

**Układy platformy Xamarin.Forms, [platformy Xamarin University](https://university.xamarin.com/)**

Każdy formant układu jest opisane poniżej, a także szczegółowe informacje na temat sposobu obsługi zmiany orientacji ekranu:

* **[StackLayout](stack-layout.md)**  &ndash; służy do rozmieszczania widoków liniowo, poziomo czy pionowo. Widoki w StackLayout może być wyrównany do Centrum z lewej lub prawej układu.
* **[AbsoluteLayout](absolute-layout.md)**  &ndash; używany do rozmiaru pod względem wartości bezwzględne lub współczynniki ro & Zmieść widoków, ustawiając współrzędnych. AbsoluteLayout może służyć do warstwy widoki, a także zakotwiczyć je w lewo, prawo lub Centrum.
* **[RelativeLayout](relative-layout.md)**  &ndash; służy do rozmieszczania widoków, ustawiając ograniczenia względem ich nadrzędnych wymiarów i położenie.
* **[Siatka](grid.md)**  &ndash; służy do rozmieszczania widoków w siatce. Pod względem wartości bezwzględne lub współczynniki można określić wierszy i kolumn.
* **[FlexLayout](flex-layout.md)**  &ndash; służy do rozmieszczania poziomo lub pionowo widoków z zawijaniem.
* **[ScrollView](scroll-view.md)**  &ndash; używane do zapewnienia przewijania, gdy widok nie mieści się całkowicie w granicach ekranu.
* **[LayoutOptions](layout-options.md)**  &ndash; definiują wyrównania i rozszerzenia dla widoku, względem jego elementu nadrzędnego.
* **[Dane wejściowe przezroczystości](#input_transparency)**  &ndash; Określa, czy element odbiera dane wejściowe.
* **[Margines i wypełnienie](margin-and-padding.md)**  &ndash; pokazuje, jak kontrolować układ zachowanie, gdy element jest wyświetlana w interfejsie użytkownika.
* **[Orientacja urządzenia](device-orientation.md)**  &ndash; wyjaśnia, jak obsługiwać zmiany orientacji urządzenia.
* **[Układ na tablety i pulpitu urządzenia](tablet.md)**  &ndash; pokazuje, jak Optymalizacja pod kątem większych ekrany na każdej platformie.
* **[Tworzenie niestandardowego układu](custom.md)**  &ndash; wyjaśnia, jak utworzyć klasę układu niestandardowego.
* **[Kompresja układu](layout-compression.md)**  &ndash; usuwa określony układ z drzewa wizualnego w celu zwiększenia wydajności renderowania strony.

Kontrolek platformy można również bezpośrednio w układy platformy Xamarin.Forms przy użyciu [ **natywnych osadzania** ](~/xamarin-forms/platform/native-views/index.md) (Nowość w Xamarin.Forms 2.2), i możesz [ **Utwórz układy niestandardowe** ](custom.md) pod kątem wymagań dotyczących.

Poniższa ilustracja wizualizuje formantów układu:

[![](images/layouts-sml.png "Układy platformy Xamarin.Forms")](images/layouts.png#lightbox "układy platformy Xamarin.Forms")

## <a name="choosing-the-right-layout"></a>Wybierając odpowiedni układ

Układy, w aplikacji można pomocy lub obniżyć, podczas tworzenia aplikacji platformy Xamarin.Forms atrakcyjne i użyteczne. Trwa trochę czasu, należy wziąć pod uwagę, jak działa każdy układu można napisać bardziej przejrzyste i bardziej skalowalne kod interfejsu użytkownika. Ekran można mieć kombinację różnych układów w celu osiągnięcia określony projekt.

### <a name="stacklayoutstack-layoutmd"></a>[StackLayout](stack-layout.md)

`StackLayout` Służy do wyświetlania widoków wzdłuż linii poziomej lub pionowej. Położenie i rozmiar w układzie jest określana na podstawie widoku `HeightRequest`, `WidthRequest`, `HorizontalOptions` i `VerticalOptions`. `StackLayout` jest często używana jako podstawowy układ, rozmieszczanie inne układy na ekranie.

Na przykład kiedy `StackLayout` może być dobrym rozwiązaniem, należy wziąć pod uwagę aplikację, która wymaga, aby wyświetlić przycisk i etykietę, za pomocą etykiety wyrównane do lewej i przycisk wyrównany do prawej.

```xaml
<StackLayout Orientation="Horizontal">
  <Label HorizontalOptions="StartAndExpand" Text="Label" />
  <Button HorizontalOptions="End" Text="Button" />
</StackLayout>
```

### <a name="flexlayoutflex-layoutmd"></a>[FlexLayout](flex-layout.md)

`FlexLayout` Przypomina `StackLayout` w tym, że zawiera widoki podrzędne w poziomie lub pionie:

```xaml
<FlexLayout Direction="Column"
            AlignItems="Center"
            JustifyContent="SpaceEvenly">

    <Label Text="FlexLayout in Action" />
    <Button Text="Button" />
    <Label Text="Another Label" />
</FlexLayout>
```

Jednakże, jeśli istnieje zbyt wiele elementów podrzędnych, aby zmieścić ją w pojedynczy wiersz lub kolumnę, `FlexLayout` również jest zdolny do zawijania te widoki. `FlexLayout` jest oparta na Module układ pole elastyczne CSS i ma wiele tych samych opcji wbudowanych pozycjonowanie i wyrównywanie jego elementów podrzędnych.

### <a name="absolutelayoutabsolute-layoutmd"></a>[AbsoluteLayout](absolute-layout.md)

`AbsoluteLayout` Służy do wyświetlania widoków o rozmiarze i pozycja określana jest zarówno jako jawne wartości lub wartości rozmiaru układu. W odróżnieniu od `StackLayout` i `Grid`, `AbsoluteLayout` umożliwia widoki nakładają się na siebie podrzędne. W odróżnieniu od `RelativeLayout`, `AbsoluteLayout` nie pozwala na umieszczenie elementów poza ekranem.

Na przykład kiedy `AbsoluteLayout` może być dobrym rozwiązaniem, należy wziąć pod uwagę aplikację, która należy do prezentowania kolekcji obiektów jako stosów. Występuje to często podczas wyświetlania albumów zdjęć lub utwory. Poniższy kod zapewnia wygląd stosu, z elementami obracać lodowej zawartość stosu:

W XAML:

```xaml
<AbsoluteLayout Padding="15">
  <Image AbsoluteLayout.LayoutFlags="PositionProportional" AbsoluteLayout.LayoutBounds="0.5, 0, 100, 100" Rotation="30"
    Source="bottom.png" />
  <Image AbsoluteLayout.LayoutFlags="PositionProportional" AbsoluteLayout.LayoutBounds="0.5, 0, 100, 100" Rotation="60"
    Source="middle.png" />
  <Image AbsoluteLayout.LayoutFlags="PositionProportional" AbsoluteLayout.LayoutBounds="0.5, 0, 100, 100"
    Source="cover.png" />
</AbsoluteLayout>
```

Należy zwrócić uwagę następujących aspektów powyższy kod:

- Każdy `Image` są wyświetlane w tym samym miejscu (na środku przestrzeni w poziomie)
- `Padding` Jest uznawany za przez `AbsoluteLayout`, w przeciwieństwie do `RelativeLayout`, który je ignoruje.
- `AbsoluteLayout.LayoutFlags` Określa, jak interpretować granice układu. W tym przypadku `PositionProportional`, oznacza współrzędne będzie stosunku rozmiaru układu, gdy rozmiar, będzie interpretowany jako jawny rozmiar.
- `AbsoluteLayout.Layoutbounds` Określa położenie w poziomie, położenie w pionie, szerokość i wysokość w tej kolejności.

### <a name="relativelayoutrelative-layoutmd"></a>[RelativeLayout](relative-layout.md)

`RelativeLayout` Służy do wyświetlania widoków z rozmiar i położenie określony jako wartości względem wartości układu lub w innym widoku. Względne wartości jest konieczne pasuje on odpowiednie wartości w powiązanej widoku. Na przykład możliwość ustawienia widoku jest `Width` właściwość proporcjonalny do innego widoku `X` właściwości.

RelativeLayout może służyć do tworzenia interfejsów użytkownika, które można skalować proporcjonalnie różnych rozmiarów urządzeń. Następujące XAML implementuje projektu z polami w rogach najwyższego poziomu za pomocą flagpole przy użyciu flagi w Centrum:

```xaml
<RelativeLayout HorizontalOptions="FillAndExpand" VerticalOptions="FillAndExpand">
  <BoxView Color="Blue" HeightRequest="50" WidthRequest="50"
    RelativeLayout.XConstraint= "{ConstraintExpression Type=RelativeToParent, Property=Width, Factor = 0}"
    RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor = 0}" />
  <BoxView Color="Red" HeightRequest="50" WidthRequest="50"
    RelativeLayout.XConstraint= "{ConstraintExpression Type=RelativeToParent, Property=Width, Factor = .9}"
    RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor = 0}" />
  <BoxView Color="Gray" WidthRequest="15" x:Name="pole"
    RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=.75}"
    RelativeLayout.XConstraint= "{ConstraintExpression Type=RelativeToParent, Property=Width, Factor = .45}"
    RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor = .25}" />
  <BoxView Color="Green"
    RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=.10, Constant=10}"
    RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent,Property=Width, Factor=.2, Constant=20}"
    RelativeLayout.XConstraint= "{ConstraintExpression Type=RelativeToView, ElementName=pole, Property=X, Constant=15}"
    RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToView, ElementName=pole, Property=Y, Constant=0}" />
</RelativeLayout>
```

Należy zwrócić uwagę następujących aspektów powyższy kod:

- Rozmiary i położenie są określane jako warunki ograniczające.
- Nosi nazwę flagpole tak, aby flaga firmy (zielony pola) można ustawić położenie względem flagpole.
- Wyrażenia ograniczeń mają `Factor` i `Constant` właściwości, które mogą służyć do definiowania rozmiary i położenie jako wielokrotności (lub ułamki) właściwości innych obiektów, a także stałą. Stałe może być ujemna.

### <a name="gridgridmd"></a>[Siatka](grid.md)

`Grid` Służy do wyświetlania elementów w wiersze i kolumny. Należy pamiętać, że siatki nie jest tabelą, więc nie ma koncepcji komórek, wierszy Nagłówek i stopka lub granice pomiędzy wiersze i kolumny. Ogólnie rzecz biorąc siatka nie jest odpowiedni do wyświetlania danych tabelarycznych. Do użycia z tym należy wziąć pod uwagę [ListView](~/xamarin-forms/user-interface/listview/index.md) lub [TableView](~/xamarin-forms/user-interface/tableview.md).

Na przykład gdy `Grid` jest odpowiedni układ do użycia, należy wziąć pod uwagę wejścia numerycznego Kalkulator. Liczbowe dane wejściowe na potrzeby Kalkulator może składać się z czterech wiersze i trzy kolumny, każdy z przyciskiem. Poniższy kod implementuje w tym projekcie:

```xaml
<Grid>
  <Grid.RowDefinitions>
    <RowDefinition Height="*" />
    <RowDefinition Height="*" />
    <RowDefinition Height="*" />
    <RowDefinition Height="*" />
  </Grid.RowDefinitions>

  <Grid.ColumnDefinitions>
    <ColumnDefinition Width="*" />
    <ColumnDefinition Width="*" />
    <ColumnDefinition Width="*" />
  </Grid.ColumnDefinitions>
  <Button Text="1" Grid.Row="0" Grid.Column="0" />
  <Button Text="2" Grid.Row="0" Grid.Column="1" />
  <Button Text="3" Grid.Row="0" Grid.Column="2" />
  <Button Text="4" Grid.Row="1" Grid.Column="0" />
  <Button Text="5" Grid.Row="1" Grid.Column="1" />
  <Button Text="6" Grid.Row="1" Grid.Column="2" />
  <Button Text="7" Grid.Row="2" Grid.Column="0" />
  <Button Text="8" Grid.Row="2" Grid.Column="1" />
  <Button Text="9" Grid.Row="2" Grid.Column="2" />
  <Button Text="0" Grid.Row="3" Grid.Column="1" />
  <Button Text="&lt;-" Grid.Row="3" Grid.Column="2" />
</Grid>
```

Należy zwrócić uwagę następujących aspektów powyższy kod:

- Siatki i kolumny są jawnie określone, nie jest wnioskowany z zawartości.
- `Height` i `Width` wartości można ustawić w formie gwiazdek, co oznacza, że siatki ustawi te wartości w celu wypełnienia dostępnego miejsca.
- Każdy przycisk pozycja jest określona przez `Grid.Row`  &  `Grid.Column` właściwości.

### <a name="layoutoptionslayout-optionsmd"></a>[LayoutOptions](layout-options.md)

[ `LayoutOptions` ](xref:Xamarin.Forms.LayoutOptions) Struktura może służyć do definiowania wyrównania i rozszerzenia dla widoku, względem jego elementu nadrzędnego.

### <a name="margin-and-paddingmargin-and-paddingmd"></a>[Margines i wypełnienie](margin-and-padding.md)

[ `Margin` ](xref:Xamarin.Forms.View.Margin) i [ `Padding` ](xref:Xamarin.Forms.Layout.Padding) właściwości Sterowanie zachowaniem układ, gdy element jest wyświetlana w interfejsie użytkownika.

<a name="input_transparency" />

### <a name="input-transparency"></a>Przejrzystość danych wejściowych

Każdy element ma [ `InputTransparent` ](xref:Xamarin.Forms.VisualElement.InputTransparent) właściwość, która jest używana do definiowania, czy element uzyskuje dane wejściowe. Jego wartość domyślna to `false`, zapewniając, że element będzie otrzymywać dane wejściowe.

Gdy ta właściwość jest ustawiona na klasy kontenera, taki jak klasa układu, jej transfer wartości do elementów podrzędnych. W związku z tym, ustawienie [ `InputTransparent` ](xref:Xamarin.Forms.VisualElement.InputTransparent) właściwość `true` układ klasy spowoduje elementy w układzie, nie odbiera danych wejściowych.

### <a name="device-orientationdevice-orientationmd"></a>[Orientacja urządzenia](device-orientation.md)

Zestaw narzędzi Xamarin.Forms i jego wbudowanych układów są zdolne do obsługi zmian w orientacji urządzenia. Należy wziąć pod uwagę orientacje, których Twoja aplikacja będzie obsługiwać, a także jak wprowadzisz użytkowania w odpowiednim miejscu w trybie poziomym i pionowym.

### <a name="layout-for-tablet-and-desktop-appstabletmd"></a>[Układ aplikacji dla tabletu i pulpitu](tablet.md)

dla systemu iOS, Android i platformy uniwersalnej Windows wsparcie większych rozmiarów ekranu na tablet urządzeń (a także komputery przenośne i komputery stacjonarne dla Windows). Zestaw narzędzi Xamarin.Forms pozwala zoptymalizować aplikację dla większych ekranów wykrywania typu urządzenia i dostosowywania układu strony lub całkowicie używając zupełnie różne strony dla większych ekranów.

### <a name="creating-a-custom-layoutcustommd"></a>[Tworzenie niestandardowego układu](custom.md)

Zestaw narzędzi Xamarin.Forms definiuje cztery układ klasy — [ `StackLayout` ](xref:Xamarin.Forms.StackLayout), [ `AbsoluteLayout` ](xref:Xamarin.Forms.AbsoluteLayout), [ `RelativeLayout` ](xref:Xamarin.Forms.RelativeLayout), i [ `Grid` ](xref:Xamarin.Forms.Grid), i każdy rozmieszcza jego elementy podrzędne w inny sposób. Jednak czasami konieczne organizować zawartość strony, przy użyciu układu nie dostarczane przez zestaw narzędzi Xamarin.Forms. W tym artykule wyjaśniono, jak napisać klasę układu niestandardowego i pokazuje wrażliwych orientacji `WrapLayout` klasę, która organizuje jego elementy podrzędne w poziomie na stronie, a następnie opakowuje wyświetlanie kolejnych elementów podrzędnych do dodatkowych wierszy.

### <a name="layout-compressionlayout-compressionmd"></a>[Kompresja układu](layout-compression.md)

Kompresja układu usuwa określony układy z drzewa wizualnego w celu zwiększenia wydajności renderowania strony. Korzyści wydajności, które ten dostarcza różni się w zależności od złożoności strony, wersja używanego systemu operacyjnego i urządzenia, na którym działa aplikacja. Jednakże wydajność będzie widoczna w starszych urządzeń.

## <a name="making-your-choice"></a>Tworzenie ulubionego

Należy pamiętać, że w większości przypadków więcej niż jeden układ wybór może służyć do implementowania żądanego projektu. W przypadku wielu prawidłowe opcje należy wziąć pod uwagę podejście, które będą najłatwiejszy w danej sytuacji.
Większość projektów nie można realizować za pomocą tylko jeden układ, dlatego zagnieżdżanie układów jako potrzebne do utworzenia bardziej złożonych projektów.


## <a name="related-links"></a>Linki pokrewne

- [Wskazówki dotyczące interfejsu firmy Apple](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/MobileHIG)
- [Android projektu witryny sieci Web](https://developer.android.com/design/index.html)
- [Układ (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [Przykład BusinessTumble (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
