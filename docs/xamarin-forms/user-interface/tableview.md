---
title: TableView
description: "Przedstawia przewijania menu, ustawienia i zwykłe formularze."
ms.topic: article
ms.prod: xamarin
ms.assetid: D1619D19-A74F-40DF-8E53-B1B7DFF7A3FB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 2dff3ca311341ffe1e30145ad436820b715973b0
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
---
# <a name="tableview"></a>TableView

[TableView](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/) jest widok, aby wyświetlić przewijanej listy danych lub opcji w przypadku, gdy istnieją wiersze, które nie mają tego samego szablonu. W odróżnieniu od [ListView](~/xamarin-forms/user-interface/listview/index.md), TableView nie ma pojęcie `ItemsSource`, więc elementy muszą zostać dodane jako elementy podrzędne ręcznie.

Ten przewodnik składa się z następujących sekcji:

- **[Przypadki użycia](#Use_Cases)**  &ndash; użycie TableView zamiast ListView lub widok niestandardowy.
- **[Struktura TableView](#TableView_Structure)**  &ndash; hierarchii widoków, które jest wymagane w ciągu TableView.
- **[Wygląd TableView](#TableView_Appearance)**  &ndash; opcji dostosowywania TableView.
- **[Wbudowane komórek](#Built-In_Cells)**  &ndash; wbudowanego opcje, w tym [EntryCell](#EntryCell) i [SwitchCell](#SwitchCell).
- **[Niestandardowe komórek](#Custom_Cells)**  &ndash; porady: tworzenie własnych niestandardowych komórek.

![](tableview-images/tableview-all-sml.png "Przykład TableView")

<a name="Use_Cases" />

## <a name="use-cases"></a>Przypadki użycia

TableView jest przydatna, gdy:

- prezentowanie ustawienia
- zbieranie danych w formularzu, lub
- Wyświetlanie danych, które są prezentowane inaczej w wierszu wiersza (np. liczb, wartości procentowe i obrazy).

TableView obsługuje przewijanie i rozmieszczania wierszy w sekcjach atrakcyjne, potrzebują powyższych scenariuszy. `TableView` Kontroli korzysta z każdej z platform podstawowy równoważnych widoku, jeśli jest dostępna, tworzenie natywnego wyglądu dla każdej platformy.

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

Każdy `TableSection` składa się z nagłówka i co najmniej jeden ViewCells. Tutaj przedstawiono `TableSection`w `Title` ustawioną właściwość *"Pierścień"* w Konstruktorze:

```csharp
var section = new TableSection ("Ring") { //TableSection constructor takes title as an optional parameter
    new SwitchCell {Text = "New Voice Mail"},
    new SwitchCell {Text = "New Mail", On = true}
};
```

Aby osiągnąć ten sam układ zgodnie z powyższym w języku XAML:

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

Przedstawia TableView `Intent` właściwość, która jest wyliczenie z następujących opcji:

- **Dane** &ndash; do użycia podczas wyświetlania danych. Należy pamiętać, że [ListView](~/xamarin-forms/user-interface/listview/index.md) może być lepszym rozwiązaniem do przewijania listę danych.
- **Formularz** &ndash; do użycia podczas TableView działa jako formularza.
- **Menu** &ndash; do użycia podczas wyświetlone menu opcji.
- **Ustawienia** &ndash; do użycia podczas wyświetlania listy ustawień konfiguracji.

`TableIntent` Wybierzesz może mieć wpływ na sposób `TableView` pojawia się na każdej z platform. Nawet jeśli usuwaj różnice, jest najlepszym rozwiązaniem, aby wybrać `TableIntent` , które najlepiej odpowiadają sposób ma być używany w tabeli.

<a name="Built-In_Cells" />

## <a name="built-in-cells"></a>Wbudowane komórek

Platformy Xamarin.Forms zawiera wbudowane komórek do zbierania i wyświetlania informacji. Mimo że ListView i TableView można korzystać ze wszystkich tych samych komórek, najbardziej odpowiednie dla scenariusza TableView są następujące:

- **SwitchCell** &ndash; prezentowania i przechwytywania stanu PRAWDA/FAŁSZ, wraz z etykietę tekstową.
- **EntryCell** &ndash; prezentowania i przechwytywanie tekstu.

Zobacz [wygląd komórek ListView](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md) szczegółowy opis [TextCell](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#TextCell) i [ImageCell](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#ImageCell).

<a name="switchcell" />

### <a name="switchcell"></a>SwitchCell
[`SwitchCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.SwitchCell/) formant do użycia na potrzeby prezentacji i przechwytywanie włączania/wyłączania lub `true` / `false` stanu.

SwitchCells ma jeden wiersz tekstu w celu edycji, a właściwość wł. / wył. Czy obu tych właściwości można powiązać.

- `Text` &ndash; tekst do wyświetlenia obok przełącznika.
- `On` &ndash; Określa, czy przełącznik jest wyświetlany jako włączone lub wyłączone.

Należy pamiętać, że `SwitchCell` przedstawia `OnChanged` zdarzeń, umożliwiając reagowania na zmiany w stanie komórki.

![](tableview-images/switch-cell.png "Przykład SwitchCell")

<a name="entrycell" />

### <a name="entrycell"></a>EntryCell
[`EntryCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.EntryCell/) jest przydatne, gdy konieczne jest wyświetlenie danych tekstowych, które użytkownik może edytować. `EntryCell`s oferują następujące właściwości, które można dostosowywać:

- `Keyboard` &ndash; Klawiatury do wyświetlenia podczas edycji. Dostępne są opcje dla elementów, jak wartości liczbowe, poczty e-mail, numerów telefonów itd. [Zobacz dokumentacja interfejsu API](http://developer.xamarin.com/api/type/Xamarin.Forms.Keyboard/).
- `Label` &ndash; Tekst etykiety do wyświetlenia z prawej strony pola wprowadzania tekstu.
- `LabelColor` &ndash; Kolor tekstu etykiety.
- `Placeholder` &ndash; Tekst wyświetlany w polu wpisu, gdy jest zerowa lub pusta. Ten tekst zniknie po rozpoczęciu wprowadzania tekstu.
- `Text` &ndash; Tekst w polu wpisu.
- `HorizontalTextAlignment` &ndash; Wyrównanie tekstu w poziomie. Centrum, lewego lub prawego wyrównania. [Zobacz dokumentacja interfejsu API](http://developer.xamarin.com/api/type/Xamarin.Forms.TextAlignment/).

Należy pamiętać, że `EntryCell` przedstawia `Completed` zdarzenie, które jest wywoływane, gdy użytkownik naciśnie "gotowe" na klawiaturze podczas edycji tekstu.

![](tableview-images/entry-cell.png "EntryCell Example")

<a name="Custom_Cells" />

## <a name="custom-cells"></a>Niestandardowe komórek
Gdy wbudowane komórki nie są wystarczająco, niestandardowe komórek można stanowią i przechwytywania danych w sposób, który ma sens dla aplikacji. Na przykład można przedstawić suwaka, aby zezwolić użytkownikowi na wybranie przezroczystość obrazu.

Wszystkie niestandardowe komórki musi pochodzić od [ `ViewCell` ](http://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/), tej samej klasy podstawowej, że wszystkie komórki wbudowane typy użycia.

To jest przykład komórki niestandardowych:

![](tableview-images/custom-cell.png "Przykład niestandardowych komórki")

### <a name="xaml"></a>XAML
Kod XAML, aby utworzyć układ powyżej znajduje się poniżej:

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

Wiele zadań XAML powyżej. Ta funkcja pozwala podzielić go:

- Element główny w obszarze `TableView` jest `TableRoot`.
- Brak `TableSection` znajdującym się poniżej `TableRoot`.
- `ViewCell` Zdefiniowano bezpośrednio pod sekcja. W odróżnieniu od `ListView`, `TableView` nie wymaga tego niestandardowe (lub któregokolwiek) komórki są zdefiniowane w `ItemTemplate`.
- StackLayout służy do zarządzania układu niestandardowego komórki. Dowolny układ można zastosować w tym miejscu.

### <a name="cnum"></a>C&num;

Ponieważ `TableView` działa z danymi statycznymi lub dane, które jest zmieniony ręcznie, nie ma pojęcie szablon elementu. Zamiast tego komórek niestandardowych można ręcznie tworzyć i umieścić w tabeli. Uwaga techniki tworzenia niestandardowego komórki, który dziedziczy z `ViewCell`, a następnie dodanie go do `TableView` jak czy wbudowanych komórki, obsługiwana jest również.
Oto kod c#, aby osiągnąć powyższe układu:

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

C# powyżej jest wykonywanie partii. Ta funkcja pozwala podzielić go:

- Element główny w obszarze `TableView` jest `TableRoot`.
- Brak `TableSection` znajdującym się poniżej `TableRoot`.
- `ViewCell` Zdefiniowano bezpośrednio pod sekcja. W odróżnieniu od `ListView`, `TableView` nie wymaga tego niestandardowe (lub któregokolwiek) komórki są zdefiniowane w `ItemTemplate`.
- StackLayout służy do zarządzania układu niestandardowego komórki. Dowolny układ można zastosować w tym miejscu.

Należy pamiętać, nigdy nie zdefiniowano klasę dla niestandardowych komórki. Zamiast tego `ViewCell`w widoku właściwość ma wartość dla konkretnego wystąpienia `ViewCell`.



## <a name="related-links"></a>Linki pokrewne

- [TableView (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/TableView)
