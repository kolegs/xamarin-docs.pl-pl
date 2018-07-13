---
title: TableView zestawu narzędzi Xamarin.Forms
description: W tym artykule wyjaśniono, jak korzystać z zestawu narzędzi Xamarin.Forms TableView klasy do przedstawienia menu przewijania, ustawienia i formularze w aplikacjach.
ms.prod: xamarin
ms.assetid: D1619D19-A74F-40DF-8E53-B1B7DFF7A3FB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 47cd79611cfeaf48c0422772d8f3e75eb57ba771
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996056"
---
# <a name="xamarinforms-tableview"></a>TableView zestawu narzędzi Xamarin.Forms

[TableView](xref:Xamarin.Forms.TableView) jest widok, aby wyświetlić przewijaną listę dane lub opcje w przypadku, gdy istnieją wiersze, które nie mają tego samego szablonu. W odróżnieniu od [ListView](~/xamarin-forms/user-interface/listview/index.md), TableView nie ma koncepcji `ItemsSource`, więc elementy muszą zostać dodane jako elementy podrzędne ręcznie.

Ten przewodnik składa się z następujących sekcji:

- **[Zastosowań](#Use_Cases)**  &ndash; kiedy należy używać TableView zamiast ListView lub widok niestandardowy.
- **[Struktura TableView](#TableView_Structure)**  &ndash; hierarchii widoków, które są potrzebne w ramach TableView.
- **[Wygląd TableView](#TableView_Appearance)**  &ndash; opcji dostosowywania TableView.
- **[Wbudowane komórek](#Built-In_Cells)**  &ndash; opcji wbudowanych komórki, w tym [EntryCell](#EntryCell) i [SwitchCell](#SwitchCell).
- **[Niestandardowe komórek](#Custom_Cells)**  &ndash; jak tworzyć własne niestandardowe komórek.

![](tableview-images/tableview-all-sml.png "Przykład TableView")

<a name="Use_Cases" />

## <a name="use-cases"></a>Przypadki użycia

TableView jest przydatne, gdy:

- prezentowanie dostępnych ustawień,
- zbieranie danych w formularzu, lub
- Wyświetlanie danych prezentowanych inaczej od wiersza do wiersza (np. liczby, wartości procentowych i obrazów).

TableView obsługuje przewijania i układ wierszy w sekcjach atrakcyjne, wspólnego potrzebę powyższych scenariuszy. `TableView` Kontroli korzysta z każdej z platform podstawowych równoważne widoku, jeśli są dostępne, tworzenie natywnych wyglądu dla każdej platformy.

<a name="TableView_Structure" />

## <a name="tableview-structure"></a>Struktura TableView

Elementy w `TableView` są podzielone na sekcje. W katalogu głównym `TableView` jest `TableRoot`, który jest nadrzędny do jednej lub kilku `TableSections`:

```csharp
Content = new TableView {
    Root = new TableRoot {
        new TableSection...
    },
    Intent = TableIntent.Settings
};
```

Każdy `TableSection` składa się z nagłówkiem i co najmniej jeden ViewCells. Tutaj widzimy `TableSection`firmy `Title` właściwością *"Pierścień"* w Konstruktorze:

```csharp
var section = new TableSection ("Ring") { //TableSection constructor takes title as an optional parameter
    new SwitchCell {Text = "New Voice Mail"},
    new SwitchCell {Text = "New Mail", On = true}
};
```

Aby osiągnąć ten sam układ, tak jak powyżej w XAML:

```xaml
<TableView Intent="Settings">
    <TableRoot>
        <TableSection Title="Ring">
            <SwitchCell Text="New Voice Mail" />
      <SwitchCell Text="New Mail" On="true" />
        </TableSection>
    </TableRoot>
</TableView>
```

<a name="TableView_Appearance" />

## <a name="tableview-appearance"></a>Wygląd TableView

Udostępnia TableView `Intent` właściwość, która jest wyliczenie z następujących opcji:

- **Dane** &ndash; do użycia przy wyświetlaniu pozycji danych. Należy pamiętać, że [ListView](~/xamarin-forms/user-interface/listview/index.md) może być opcją lepszą przewijając listę danych.
- **Formularz** &ndash; do użycia podczas TableView działa jako formularz.
- **Menu** &ndash; do użycia podczas wyświetlania menu opcji.
- **Ustawienia** &ndash; do użycia przy wyświetlaniu listy ustawień konfiguracji.

`TableIntent` Możesz wybrać, może mieć wpływ na sposób, w jaki `TableView` pojawia się na każdej platformie. Nawet jeśli usuwaj różnic, jest najlepszym rozwiązaniem jest, aby wybrać `TableIntent` który najlepiej pasuje do sposobu, w których zamierzasz używać w tabeli.

<a name="Built-In_Cells" />

## <a name="built-in-cells"></a>Wbudowane komórek

Zestaw narzędzi Xamarin.Forms jest powiązana z wbudowanych komórek do gromadzenia i wyświetlania informacji. Mimo że ListView i TableView można używać wszystkich tych samych komórkach, najbardziej odpowiednie do scenariusza TableView są następujące:

- **SwitchCell** &ndash; prezentowania i przechwytywania stanu PRAWDA/FAŁSZ, wraz z etykietę tekstową.
- **EntryCell** &ndash; prezentowania i przechwytywanie tekstu.

Zobacz [wygląd komórki ListView](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md) szczegółowy opis działania [TextCell](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#TextCell) i [ImageCell](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#ImageCell).

<a name="switchcell" />

### <a name="switchcell"></a>SwitchCell
[`SwitchCell`](xref:Xamarin.Forms.SwitchCell) jest formantem na potrzeby prezentacji i przechwytywanie włączenia/wyłączenia lub `true` / `false` stanu.

SwitchCells ma jeden wiersz tekstu do edycji i właściwością wł. / wył. Obie te właściwości są możliwej do wiązania.

- `Text` &ndash; tekst do wyświetlenia obok przełącznika.
- `On` &ndash; Czy przełącznik jest wyświetlany jako włączona lub wyłączona.

Należy pamiętać, że `SwitchCell` udostępnia `OnChanged` zdarzeń, umożliwiając odpowiadanie na zmiany w stanie komórki.

![](tableview-images/switch-cell.png "Przykład SwitchCell")

<a name="entrycell" />

### <a name="entrycell"></a>EntryCell
[`EntryCell`](xref:Xamarin.Forms.EntryCell) jest przydatne, gdy konieczne jest wyświetlenie danych tekstowych, które użytkownik może edytować. `EntryCell`s oferują następujące właściwości, które można dostosować:

- `Keyboard` &ndash; Klawiatury do wyświetlania podczas edycji. Dostępne są opcje dla elementów, takich jak wartości liczbowych, poczty e-mail, numerów telefonów itd. [Zobacz dokumentacja interfejsu API](xref:Xamarin.Forms.Keyboard).
- `Label` &ndash; Tekst etykiety, które mają być wyświetlane z prawej strony pola wprowadzania tekstu.
- `LabelColor` &ndash; Kolor tekstu etykiety.
- `Placeholder` &ndash; Tekst do wyświetlenia w polu wpisu po wartości null ani być pusta. Ten tekst zniknie po rozpoczęciu wprowadzania tekstu.
- `Text` &ndash; Tekst w polu wpisu.
- `HorizontalTextAlignment` &ndash; Wyrównanie tekstu w poziomie. Może być, lewego lub prawego wyrównanie do środka. [Zobacz dokumentacja interfejsu API](xref:Xamarin.Forms.TextAlignment).

Należy pamiętać, że `EntryCell` udostępnia `Completed` zdarzenie, które jest wywoływane, gdy użytkownik naciśnie "gotowe" na klawiaturze podczas edycji tekstu.

![](tableview-images/entry-cell.png "Przykład EntryCell")

<a name="Custom_Cells" />

## <a name="custom-cells"></a>Niestandardowe komórek
Gdy wbudowane komórki nie są wystarczające, niestandardowe komórek może służyć umożliwiające zaprezentowanie i przechwytywania danych w taki sposób, który ma sens dla swojej aplikacji. Na przykład można przedstawić suwak, aby umożliwić użytkownikom wybór przezroczystość obrazu.

Wszystkie komórki niestandardowe muszą pochodzić od [ `ViewCell` ](xref:Xamarin.Forms.ViewCell), tej samej klasy bazowej, że wszystkie komórki wbudowane typy użycia.

Jest to przykład niestandardowych komórki:

![](tableview-images/custom-cell.png "Przykład niestandardowych komórki")

### <a name="xaml"></a>XAML
XAML do tworzenia układu powyżej znajduje się poniżej:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="DemoTableView.TablePage" Title="TableView">
    <ContentPage.Content>
        <TableView Intent="Settings">
            <TableRoot>
                <TableSection Title="Getting Started">
                    <ViewCell>
                        <StackLayout Orientation="Horizontal">
                            <Image Source="bulb.png" />
                            <Label Text="left"
                              TextColor="#f35e20" />
                            <Label Text="right"
                              HorizontalOptions="EndAndExpand"
                              TextColor="#503026" />
                        </StackLayout>
                    </ViewCell>
                </TableSection>
            </TableRoot>
        </TableView>
    </ContentPage.Content>
</ContentPage>

```

Powyższe XAML znacznie robi. Możemy ją rozbić:

- Element główny, w obszarze `TableView` jest `TableRoot`.
- Brak `TableSection` znajdującym się poniżej `TableRoot`.
- `ViewCell` Jest zdefiniowany bezpośrednio pod sekcja. W odróżnieniu od `ListView`, `TableView` nie wymaga tego niestandardowego (lub jakichkolwiek) komórek są zdefiniowane w `ItemTemplate`.
- StackLayout służy do zarządzania układu niestandardowego komórki. Dowolny układ może być używana w tym miejscu.

### <a name="cnum"></a>C&num;

Ponieważ `TableView` działa przy użyciu statycznych danych lub dane, które jest zmieniony ręcznie, nie ma koncepcji szablon elementu. Zamiast tego niestandardowego komórek można ręcznie tworzyć i umieścić w tabeli. Należy pamiętać, że techniki tworzenia niestandardowego komórki dziedziczy `ViewCell`, a następnie dodanie go do `TableView` takich jak Ty, czy wbudowane komórki, jest również obsługiwany.
Poniżej przedstawiono kod c#, aby osiągnąć powyższe układu:

```csharp
var table = new TableView();
table.Intent = TableIntent.Settings;
var layout = new StackLayout() { Orientation = StackOrientation.Horizontal };
layout.Children.Add (new Image() {Source = "bulb.png"});
layout.Children.Add (new Label() {
    Text = "left",
    TextColor = Color.FromHex("#f35e20"),
    VerticalOptions = LayoutOptions.Center
});
layout.Children.Add (new Label () {
    Text = "right",
    TextColor = Color.FromHex ("#503026"),
    VerticalOptions = LayoutOptions.Center,
    HorizontalOptions = LayoutOptions.EndAndExpand
});
table.Root = new TableRoot () {
    new TableSection("Getting Started") {
        new ViewCell() {View = layout}
    }
};

Content = table;
```

C# powyżej robi partii. Możemy ją rozbić:

- Element główny, w obszarze `TableView` jest `TableRoot`.
- Brak `TableSection` znajdującym się poniżej `TableRoot`.
- `ViewCell` Jest zdefiniowany bezpośrednio pod sekcja. W odróżnieniu od `ListView`, `TableView` nie wymaga tego niestandardowego (lub jakichkolwiek) komórek są zdefiniowane w `ItemTemplate`.
- StackLayout służy do zarządzania układu niestandardowego komórki. Dowolny układ może być używana w tym miejscu.

Należy pamiętać, nigdy nie zdefiniowano klasy dla niestandardowych komórki. Zamiast tego `ViewCell`w widoku właściwość jest ustawiona dla konkretnego wystąpienia `ViewCell`.



## <a name="related-links"></a>Linki pokrewne

- [TableView (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/TableView)
