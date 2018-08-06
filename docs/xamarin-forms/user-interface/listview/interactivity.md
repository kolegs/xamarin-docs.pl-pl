---
title: Interakcyjność ListView
description: W tym artykule opisano sposób interakcyjności ListView Xamarin.Forms, implementując wybory, kontekstu akcji i ściągania do odświeżania.
ms.prod: xamarin
ms.assetid: CD14EB90-B08C-4E8F-A314-DA0EEC76E647
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/13/2018
ms.openlocfilehash: 77a48e36f33fc690306f5e590f9f4f3064fe1ddf
ms.sourcegitcommit: 4c0093ee5d4aeb16c0e6f0c740c4796736971651
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/23/2018
ms.locfileid: "39202945"
---
# <a name="listview-interactivity"></a>Interakcyjność ListView

ListView obsługuje interakcji z danymi, które stanowi za pomocą następujących metod:

- [**Wybór & podsłuchu** ](#selectiontaps) &ndash; reagować na podsłuchu lub opcji/usuwanie elementów. Włącz lub wyłącz wybór wiersza (domyślnie włączone).
- [**Kontekst akcji** ](#Context_Actions) &ndash; udostępniają funkcje każdego elementu, na przykład, przesuń palcem do usunięcia.
- [**Ściągnięcia do odświeżania** ](#Pull_to_Refresh) &ndash; zaimplementować idiom ściągnięcia do odświeżenia, nadchodzące użytkownicy mają można oczekiwać od natywnych środowisk.

<a name="selectiontaps" />

## <a name="selection--taps"></a>Wybór & podsłuchu

[ `ListView` ](xref:Xamarin.Forms.ListView) Tryb wyboru zależy od ustawienia [ `ListView.SelectionMode` ](xref:Xamarin.Forms.ListView.SelectionMode) właściwości na wartość [ `ListViewSelectionMode` ](xref:Xamarin.Forms.ListViewSelectionMode) wyliczenia:

- [`Single`](xref:Xamarin.Forms.ListViewSelectionMode.Single) Wskazuje, że pojedynczy element można wybrany, za pomocą wybranego elementu, jest wyróżniona. Jest to wartość domyślna.
- [`None`](xref:Xamarin.Forms.ListViewSelectionMode.None) Wskazuje, że nie można wybrać elementy.

Gdy użytkownik wybiera element, są uruchamiane dwa zdarzenia:

- [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) generowane, gdy nowy element jest wybrany.
- [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) generowane, gdy element jest wybrany.

> [!NOTE]
> Dwukrotne naciśnięcie tego samego elementu nastąpi dwa [ `ItemTapped` ](xref:Xamarin.Forms.ListView.ItemTapped) zdarzenia, ale będzie fire tylko jeden [ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected) zdarzeń.

Gdy [ `SelectionMode` ](xref:Xamarin.Forms.ListView.SelectionMode) właściwość jest ustawiona na [ `Single` ](xref:Xamarin.Forms.ListViewSelectionMode.Single), elementy w [ `ListView` ](xref:Xamarin.Forms.ListView) można wybrać, [ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected) i [ `ItemTapped` ](xref:Xamarin.Forms.ListView.ItemTapped) zdarzenia będą uruchamiane, a [ `SelectedItem` ](xref:Xamarin.Forms.ListView.SelectedItem) właściwość zostanie ustawiona na wartość wybranego elementu.

Gdy [ `SelectionMode` ](xref:Xamarin.Forms.ListView.SelectionMode) właściwość jest ustawiona na [ `None` ](xref:Xamarin.Forms.ListViewSelectionMode.None), elementy w [ `ListView` ](xref:Xamarin.Forms.ListView) nie można wybrać, [ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected) zdarzenie nie zostanie wyzwolone, a [ `SelectedItem` ](xref:Xamarin.Forms.ListView.SelectedItem) pozostanie właściwość `null`. Jednak [ `ItemTapped` ](xref:Xamarin.Forms.ListView.ItemTapped) zdarzenia będą nadal uruchamiane i element naciśnięty zostaną pokrótce wyróżnione podczas wzorcu tap.

Jeśli został wybrany element i [ `SelectionMode` ](xref:Xamarin.Forms.ListView.SelectionMode) właściwości została zmieniona z [ `Single` ](xref:Xamarin.Forms.ListViewSelectionMode.Single) do [ `None` ](xref:Xamarin.Forms.ListViewSelectionMode.None), [ `SelectedItem` ](xref:Xamarin.Forms.ListView.SelectedItem) będzie można ustawić właściwości `null` i [ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected) zdarzenie zostanie wyzwolone przy użyciu `null` elementu.

Poniższe zrzuty ekranu Pokaż [ `ListView` ](xref:Xamarin.Forms.ListView) przy użyciu domyślnego trybu zaznaczania:

![](interactivity-images/selection-default.png "ListView z wyborem włączone")

### <a name="disabling-selection"></a>Wyłączanie zaznaczenia

Aby wyłączyć [ `ListView` ](xref:Xamarin.Forms.ListView) zbiór [ `SelectionMode` ](xref:Xamarin.Forms.ListView.SelectionMode) właściwości [ `None` ](xref:Xamarin.Forms.ListViewSelectionMode.None):

```xaml
<ListView ... SelectionMode="None" />
```

```csharp
var listView = new ListView { ... SelectionMode = ListViewSelectionMode.None };
```

<a name="Context_Actions" />

## <a name="context-actions"></a>Kontekst akcji
Często użytkownicy będą chcieli podejmowanie akcji na element `ListView`. Rozważmy na przykład lista adresów e-mail w aplikacji Mail. W systemach iOS, można szybko przesuń należy usunąć komunikat::

![](interactivity-images/context-default.png "ListView przy użyciu kontekstu akcji")

Kontekst akcji można zaimplementować w języku C# i XAML. Poniżej przedstawiono przewodniki określonych w obu przypadkach jednak najpierw Przyjrzyjmy Przyjrzyj się kilka szczegółów implementacji klucza dla obu.

Akcje kontekstowe są tworzone przy użyciu `MenuItem`s. Wybierz zdarzenia vlastnost MenuItems są inicjowane przez element MenuItem, nie ListView. To różni się od obsługi zdarzeń wzorca tap komórek, gdzie ListView zgłasza zdarzenie, a nie w komórce. Ponieważ ListView podnoszenie zdarzenia swojego programu obsługi zdarzeń otrzymuje informacje o kluczu, takich jak którego wybrano lub dotknięcie elementu.

Domyślnie element MenuItem nie ma możliwości komórki, która należy do żadnej informacji o tym. `CommandParameter` jest dostępna w `MenuItem` do przechowywania obiektów, takich jak obiekt spod ViewCell MenuItem. `CommandParameter` można ustawić w językach XAML i C#.

### <a name="c"></a>C#  

Kontekst akcji można zaimplementować w dowolnym `Cell` podklasy (tak długo, jak nie jest używana jako nagłówek grupy), tworząc `MenuItem`s oraz dodać je do `ContextActions` kolekcji komórki. Masz następujące właściwości, które można skonfigurować dla kontekstu akcji:

* **Tekst** &ndash; ciąg, który pojawia się w elemencie menu.
* **Kliknięto** &ndash; zdarzenie po kliknięciu elementu.
* **IsDestructive** &ndash; (opcjonalnie) w przypadku wartości true element jest renderowany inaczej w systemie iOS.

Wiele akcji w kontekście można dodać do komórki, jednak powinien mieć tylko jeden `IsDestructive` równa `true`. Poniższy kod demonstruje, jak kontekstu akcje zostaną dodane do `ViewCell`:

```csharp
var moreAction = new MenuItem { Text = "More" };
moreAction.SetBinding (MenuItem.CommandParameterProperty, new Binding ("."));
moreAction.Clicked += async (sender, e) => {
    var mi = ((MenuItem)sender);
    Debug.WriteLine("More Context Action clicked: " + mi.CommandParameter);
};

var deleteAction = new MenuItem { Text = "Delete", IsDestructive = true }; // red background
deleteAction.SetBinding (MenuItem.CommandParameterProperty, new Binding ("."));
deleteAction.Clicked += async (sender, e) => {
    var mi = ((MenuItem)sender);
    Debug.WriteLine("Delete Context Action clicked: " + mi.CommandParameter);
};
// add to the ViewCell's ContextActions property
ContextActions.Add (moreAction);
ContextActions.Add (deleteAction);
```

### <a name="xaml"></a>XAML

`MenuItem`s można również tworzyć w kolekcji XAML deklaratywnie. XAML poniżej pokazuje niestandardowy komórki za pomocą dwóch działania w kontekście wdrożone:

```xaml
<ListView x:Name="ContextDemoList">
  <ListView.ItemTemplate>
    <DataTemplate>
      <ViewCell>
         <ViewCell.ContextActions>
            <MenuItem Clicked="OnMore" CommandParameter="{Binding .}"
               Text="More" />
            <MenuItem Clicked="OnDelete" CommandParameter="{Binding .}"
               Text="Delete" IsDestructive="True" />
         </ViewCell.ContextActions>
         <StackLayout Padding="15,0">
              <Label Text="{Binding title}" />
         </StackLayout>
      </ViewCell>
    </DataTemplate>
  </ListView.ItemTemplate>
</ListView>
```

W pliku związanym z kodem, upewnij się, `Clicked` metody są implementowane:

```csharp
public void OnMore (object sender, EventArgs e) {
    var mi = ((MenuItem)sender);
    DisplayAlert("More Context Action", mi.CommandParameter + " more context action", "OK");
}

public void OnDelete (object sender, EventArgs e) {
    var mi = ((MenuItem)sender);
    DisplayAlert("Delete Context Action", mi.CommandParameter + " delete context action", "OK");
}
```

> [!NOTE]
> `NavigationPageRenderer` Dla systemu Android zawiera możliwym do zastąpienia `UpdateMenuItemIcon` metodę, która może służyć do załadowania ikon z niestandardowego `Drawable`. To zastąpienie sprawia, że można używać obrazów SVG jako ikony na `MenuItem` wystąpień w systemie Android.

<a name="Pull_to_Refresh" />

## <a name="pull-to-refresh"></a>Przeciągnij, aby odświeżyć
Użytkownicy mają pochodzić można oczekiwać, że przeciągnij w dół w wykazie danych zostaną odświeżone tej listy. `ListView` obsługuje ten out-of--box. Aby włączyć funkcję ściągnięcia do odświeżenia, należy ustawić `IsPullToRefreshEnabled` na wartość true:

```csharp
listView.IsPullToRefreshEnabled = true;
```

Ściąganie jest ściągnięcia do odświeżania jako użytkownik:

![](interactivity-images/refresh-start.png "Przeciągnij ListView, aby odświeżyć w toku")

Ściągnięcia do odświeżania jako użytkownik wydała ściągnięcia. Jest to, co użytkownik widzi, gdy aktualizujesz listy: ![](interactivity-images/refresh-in-progress.png "przeciągnij ListView, aby Odświeżanie ukończone")

ListView udostępnia kilka zdarzeń, które pozwalają na odpowiadanie na zdarzenia ściągnięcia do odświeżania.

-  `RefreshCommand` Zostaną wywołane i `Refreshing` zdarzeń o nazwie. `IsRefreshing` zostanie ustawiony na `true`.
-  Należy wykonać, wymaga Odśwież zawartość widoku listy poleceń lub zdarzenia w niezależnie od kodu.
-  Podczas odświeżania jest ukończone, wywołaj `EndRefresh` lub ustaw `IsRefreshing` do `false` mówić widok listy wszystko będzie gotowe.

`CanExecute` Przestrzegane właściwości, które zapewnia możliwość sterowania czy polecenie ściągnięcia do odświeżania powinno być włączone.



## <a name="related-links"></a>Linki pokrewne

- [Interakcyjność ListView (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/interactivity)
- [informacje o wersji 1.4](http://forums.xamarin.com/discussion/35451/xamarin-forms-1-4-0-released/)
- [informacje o wersji 1.3](http://forums.xamarin.com/discussion/29934/xamarin-forms-1-3-0-released/)
