---
title: Układy
description: Układ widoków na ekranie.
ms.prod: xamarin
ms.assetid: 65030DA3-C7C1-4A02-B478-811073C39139
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 10/26/2017
ms.openlocfilehash: 864e81b6955fd5138c4055a3f202695803139ac6
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2018
---
# <a name="layouts"></a>Układy

Platformy Xamarin.Forms ma kilka układów i funkcji do organizowania zawartości ekranu. 

> [!VIDEO https://youtube.com/embed/4HlLjTZQzjM]

**Układy platformy Xamarin.Forms, przez [Xamarin University](https://university.xamarin.com/)**

Każdego formantu układu jest opisane poniżej, a także szczegółowe informacje na temat obsługi zmiany orientacji ekranu:

* **[StackLayout](stack-layout.md)**  &ndash; służy do rozmieszczania liniowo, widoki poziomo czy pionowo. Widoki StackLayout może być wyrównany do Centrum lewego lub prawego układu.
* **[AbsoluteLayout](absolute-layout.md)**  &ndash; używany do rozmiaru w postaci wartości bezwzględne lub stosunek & Rozmieść widoków ustawiając współrzędnych. AbsoluteLayout może służyć do warstwy widoków, a także zakotwiczyć je w lewo, prawo lub center.
* **[RelativeLayout](relative-layout.md)**  &ndash; służy do rozmieszczania widoki przez ustawienia ograniczenia względem ich nadrzędnego wymiary i położenie.
* **[Siatka](grid.md)**  &ndash; służy do rozmieszczania widoków w siatce. Wartości bezwzględne lub stosunek można określić wierszy i kolumn.
* **[ScrollView](scroll-view.md)**  &ndash; używane w celu dostarczania przewijania, jeśli widok nie mieści się całkowicie w zakresie ekranu.
* **[LayoutOptions](layout-options.md)**  &ndash; zdefiniuj wyrównanie i rozszerzenia dla widoku, względem jego elementu nadrzędnego.
* **[Dane wejściowe przezroczystość](#input_transparency)**  &ndash; Określa, czy element otrzymuje dane wejściowe.
* **[Margines i wypełnienie](margin-and-padding.md)**  &ndash; demonstruje sposób kontrolowania zachowania układu, gdy element jest renderowany w interfejsie użytkownika.
* **[Orientacja urządzenia](device-orientation.md)**  &ndash; wyjaśniono, jak obsługiwać zmiany orientacji urządzenia.
* **[Układ na urządzeniach z tabletu i pulpitu](tablet.md)**  &ndash; pokazano, jak Optymalizuj pod kątem większych ekrany na każdej z platform.
* **[Tworzenie układu niestandardowego](custom.md)**  &ndash; wyjaśnia sposób tworzenia niestandardowego układu klasy.
* **[Kompresja układu](layout-compression.md)**  &ndash; usuwa określony układu z drzewa wizualnego w celu zwiększenia wydajności renderowania strony.

Formanty platformy można również bezpośrednio w układów platformy Xamarin.Forms z [ **natywnego osadzanie** ](~/xamarin-forms/platform/native-views/index.md) (Nowa funkcja w wersji 2.2 platformy Xamarin.Forms), i będzie możliwe [ **Tworzenie niestandardowych układów** ](custom.md) aby spełniały określone wymagania.

Poniższa ilustracja wizualizuje formantów układu:

[![](images/layouts-sml.png "Układy platformy Xamarin.Forms")](images/layouts.png#lightbox "układów platformy Xamarin.Forms")

## <a name="choosing-the-right-layout"></a>Wybieranie układu prawo

Układy, w aplikacji można pomocy lub pogarszać możesz podczas tworzenia aplikacji platformy Xamarin.Forms atrakcyjne i można go użyć. Biorąc trochę czasu, należy wziąć pod uwagę, jak działa każdego układu można napisać kod interfejsu użytkownika czyszczący i skalowalność. Ekran może mieć kombinację układów do osiągnięcia określonego projektu.

### <a name="stacklayoutstack-layoutmd"></a>[StackLayout](stack-layout.md)

`StackLayout` Służy do wyświetlania widoków wzdłuż linii pionowej lub poziomej. Położenie i rozmiar w układzie, jest określany na podstawie widoku `HeightRequest`, `WidthRequest`, `HorizontalOptions` i `VerticalOptions`. `StackLayout` jest często używana jako podstawowa układu, rozmieszczanie innych układów na ekranie.

Na przykład w sytuacji, w których `StackLayout` może być dobrym rozwiązaniem, należy wziąć pod uwagę aplikacji, które mają być wyświetlone przycisk i etykietę z etykiety wyrównane do lewej i przycisk wyrównany do prawej.

```xaml
<StackLayout Orientation="Horizontal">
  <Label HorizontalOptions="StartAndExpand" Text="Label" />
  <Button HorizontalOptions="End" Text="Button" />
</StackLayout>
```

### <a name="absolutelayoutabsolute-layoutmd"></a>[AbsoluteLayout](absolute-layout.md)

`AbsoluteLayout` Służy do wyświetlania widoków, rozmiar i pozycja jest określona, albo jako jawne wartości lub wartości rozmiaru układu. W odróżnieniu od `StackLayout` i `Grid`, `AbsoluteLayout` umożliwia podrzędnych widoków nakładanie się. W odróżnieniu od `RelativeLayout`, `AbsoluteLayout` nie umożliwia umieszczanie elementów na ekranie.

Na przykład w sytuacji, w których `AbsoluteLayout` może być dobrym rozwiązaniem, należy wziąć pod uwagę aplikację, która należy do prezentowania kolekcji obiektów jako stosów. Jest to często postrzegane, prezentując albumów zdjęć lub utworów. Poniższy kod zapewnia wygląd stosu, z elementami obrócone o wskazówki na zawartość stosu:

W języku XAML:

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

Należy uwzględnić następujące aspekty powyżej kodu:

- Każdy `Image` jest wyświetlany w tym samym miejscu (na środku znak w poziomie)
- `Padding` Jest uznawany za przez `AbsoluteLayout`, w przeciwieństwie do `RelativeLayout`, która powoduje ignorowanie.
- `AbsoluteLayout.LayoutFlags` Określa interpretacji granice układu. W takim przypadku `PositionProportional`, oznacza współrzędne będzie stosunku rozmiaru układ, gdy rozmiar zostanie potraktowany jako jawne rozmiar.
- `AbsoluteLayout.Layoutbounds` Określa położenie, pozycji w pionie, szerokość i wysokość w podanej kolejności.

### <a name="relativelayoutrelative-layoutmd"></a>[RelativeLayout](relative-layout.md)

`RelativeLayout` Służy do wyświetlania widoków, rozmiar i pozycja określona jako wartości względem wartości układ lub innego widoku. Względne wartości musi być zgodny z on odpowiednie wartości w widoku powiązanych. Na przykład możliwość ustawienia widoku jest `Width` właściwości być proporcjonalny do innego widoku `X` właściwości.

RelativeLayout może służyć do tworzenia UI, które Skala proporcjonalnie na rozmiary urządzenia. Następujące XAML implementuje projektu z polami w narożnikach znajdujące się najwyżej z flagpole przy użyciu flagi w Centrum:

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

Należy uwzględnić następujące aspekty powyżej kodu:

- Zarówno położenia i rozmiary są określone jako ograniczenia.
- Flagpole nosi nazwę, aby (zielony pola) flagi względem flagpole można ustawić pozycji.
- Wyrażenia ograniczenia mają `Factor` i `Constant` właściwości, które mogą być używane do definiowania pozycji i rozmiary jako wielokrotności (lub ułamków) właściwości innych obiektów, a także stałą. Stałe może być ujemna.

### <a name="gridgridmd"></a>[Siatka](grid.md)

`Grid` Służy do wyświetlania elementów w wiersze i kolumny. Należy pamiętać, że siatki nie jest tabelą, więc nie ma koncepcji komórek, wierszy nagłówki i stopki lub granicach między wiersze i kolumny. Ogólnie rzecz biorąc siatki nie jest odpowiedni do wyświetlania danych tabelarycznych. W przypadku używające, rozważ [ListView](~/xamarin-forms/user-interface/listview/index.md) lub [TableView](~/xamarin-forms/user-interface/tableview.md).

Na przykład w sytuacji, w których `Grid` jest układ prawa do używania, należy wziąć pod uwagę liczbowa na wejściu Kalkulator. Liczbowych danych wejściowych dla Kalkulator może składać się z czterech wierszy i kolumn trzy, każdy z przyciskiem. Poniższy kod implementuje ten projekt:

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

Należy uwzględnić następujące aspekty powyżej kodu:

- Siatki i kolumny są jawnie określone, nie wywnioskować na podstawie zawartości.
- `Height` i `Width` wartości można ustawić w formie gwiazdek, co oznacza, że siatki ustawi tych wartości w celu wypełnienia dostępnego miejsca.
- Każdy przycisk pozycja jest określona przez `Grid.Row`  &  `Grid.Column` właściwości.

### <a name="layoutoptionslayout-optionsmd"></a>[LayoutOptions](layout-options.md)

[ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/) Struktury może służyć do definiowania wyrównanie i rozszerzenia dla widoku, względem jego elementu nadrzędnego.

### <a name="margin-and-paddingmargin-and-paddingmd"></a>[Margines i wypełnienie](margin-and-padding.md)

[ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) i [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/) właściwości kontrolowania zachowania układu, gdy element jest renderowany w interfejsie użytkownika.

<a name="input_transparency" />

### <a name="input-transparency"></a>Przezroczystość wejściowych

Każdy element ma [ `InputTransparent` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.InputTransparent/) właściwość, która służy do definiowania, czy element otrzymuje dane wejściowe. Jego wartość domyślna to `false`, zapewniając, że element odbiera dane wejściowe.

Jeśli ta właściwość jest skonfigurowana na klasę kontenera, takich jak klasy układu, jego wartość transferów na elementy podrzędne. W związku z tym ustawienie [ `InputTransparent` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.InputTransparent/) właściwości `true` układ klasy spowoduje elementów w obrębie układu nie odbiera danych wejściowych.

### <a name="device-orientationdevice-orientationmd"></a>[Orientacja urządzenia](device-orientation.md)

Platformy Xamarin.Forms i swoją wbudowanych układów są w stanie obsługiwać zmiany orientacji urządzenia. Należy wziąć pod uwagę orientacji, których aplikacja będzie obsługiwać, jak również jak należy podjąć wykorzystanie miejsca w orientacji poziomej i pionowej trybów.

### <a name="layout-for-tablet-and-desktop-appstabletmd"></a>[Układ aplikacji typu Tablet i pulpitu](tablet.md)

iOS, Android i platformy uniwersalnej systemu Windows na wszystkie obsługują większych rozmiarów ekranu tablet urządzeń (a także komputerów przenośnych i stacjonarnych dla systemu Windows). Platformy Xamarin.Forms umożliwia optymalizowanie aplikacji na ekranach większych wykrywania typu urządzenia i dostosowywania układ strony lub przy użyciu całkiem strony całkowicie dla większych ekranów.

### <a name="creating-a-custom-layoutcustommd"></a>[Tworzenie niestandardowego układu](custom.md)

Cztery układ klasy — definiuje platformy Xamarin.Forms [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/), [ `AbsoluteLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/), [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/), i [ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/), i każdego Rozmieszcza elementy podrzędne w inny sposób. Jednak czasami jego niezbędne do obsługi zawartości strony przy użyciu układu nie udostępniane przez platformy Xamarin.Forms. W tym artykule opisano sposób tworzenia niestandardowego układu klasy i prezentuje działanie zależne orientacji `WrapLayout` klasy, która Rozmieszcza elementy podrzędne w poziomie na stronie, a następnie opakowuje wyświetlanie kolejnych elementów podrzędnych do dodatkowych wierszy.

### <a name="layout-compressionlayout-compressionmd"></a>[Kompresja układu](layout-compression.md)

Kompresja układu usuwa określony układów z drzewa wizualnego w celu zwiększenia wydajności renderowania strony. Korzyści wydajności, który zapewnia to różni się w zależności od złożoności strony, wersja używanego systemu operacyjnego i urządzenia, na którym jest uruchomiona aplikacja. Jednakże wydajność będzie widoczny na starszych urządzeń.

## <a name="making-your-choice"></a>Dokonywania wyboru

Należy pamiętać, że w większości przypadków więcej niż jeden wybór układu może służyć do wykonania żądanego projektu. Gdy istnieje wiele wyborów prawidłowe, należy wziąć pod uwagę rozwiązania jest najprostsza w danej sytuacji.
Większości projektów nie może istnieć tylko jeden układ tak układami zagnieżdżania jako potrzebne do utworzenia bardziej złożonych wzorów.


## <a name="related-links"></a>Linki pokrewne

- [Firmy Apple Human Interface Guidelines](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/MobileHIG)
- [Android projekt witryny sieci Web](https://developer.android.com/design/index.html)
- [Układ (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [Przykład BusinessTumble (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
