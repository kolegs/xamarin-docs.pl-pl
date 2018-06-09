---
title: Interakcyjności ListView
description: W tym artykule opisano sposób interakcyjności w elemencie ListView platformy Xamarin.Forms zaimplementowanie zaznaczeń, przejdź do usunięcia i pociągnij, aby odświeżyć.
ms.prod: xamarin
ms.assetid: CD14EB90-B08C-4E8F-A314-DA0EEC76E647
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/01/2018
ms.openlocfilehash: 64ffda681c51c21b7485f0865af4b740316edaaa
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245110"
---
# <a name="listview-interactivity"></a>Interakcyjności ListView

Element ListView obsługuje interakcję z danymi, które stanowi w następujący sposób:

- [**Wybór & podsłuchu** ](#selectiontaps) &ndash; odpowiadanie na podsłuchu i opcje/usuwanie elementów. Włącz lub wyłącz zaznaczenie wiersza (domyślnie włączony).
- [**Kontekst akcji** ](#Context_Actions) &ndash; Uwidacznianie funkcji dla każdego elementu, na przykład, przejdź do usunięcia.
- [**Pociągnij, aby odświeżyć** ](#Pull_to_Refresh) &ndash; zaimplementować idiom pociągnij, aby odświeżyć, które użytkownicy mogą pochodzić oczekiwać z środowiska macierzystego.

<a name="selectiontaps" />

## <a name="selection--taps"></a>Wybór & podsłuchu
`ListView` obsługuje wybór jednego elementu na raz. Wybór jest domyślnie włączone. Po naciśnięciu elementu dwa zdarzenia są uruchamiane: `ItemTapped` i `ItemSelected`. Należy pamiętać, że naciśnięcie dwa razy ten sam element nie zostanie wyzwolony wielu `ItemSelected` zdarzeń, ale będą wyzwalać wielu `ItemTapped` zdarzenia. Należy również zauważyć, że `ItemSelected` można wywołać, jeśli element nie jest zaznaczona.

Należy pamiętać, że `ItemSelected` jest wywoływana zarówno podczas elementy są wyczyszczone, jak i po jej wybraniu. Oznacza to, że należy sprawdzić null `SelectedItem` w Twojej `ItemSelected` obsługi zdarzeń przed jego użyciem:

```csharp
void OnSelection (object sender, SelectedItemChangedEventArgs e)
{
  if (e.SelectedItem == null) {
    return; //ItemSelected is called on deselection, which results in SelectedItem being set to null
  }
  DisplayAlert ("Item Selected", e.SelectedItem.ToString (), "Ok");
  //((ListView)sender).SelectedItem = null; //uncomment line if you want to disable the visual selection state.
}
```

### <a name="disabling-selection"></a>Wyłączenie zaznaczenia

Jeśli chcesz wyłączyć wybór obsługi `ItemSelected` zdarzeń i zestaw `SelectedItem` właściwości na wartość null:

```csharp
SelectionDemoList.ItemSelected += (sender, e) => {
    ((ListView)sender).SelectedItem = null;
};
```

Z włączoną opcją:

![](interactivity-images/selection-default.png "Element ListView o wybór włączane")

<a name="Context_Actions" />

## <a name="context-actions"></a>Kontekst akcji
Często użytkownicy będą do wykonania akcji na element `ListView`. Rozważmy na przykład listę adresów e-mail w aplikacji poczty. W systemach iOS, można szybko przesuń można usunąć wiadomości::

![](interactivity-images/context-default.png "Element ListView z kontekstu akcji")

Kontekst akcji można zaimplementować w języku C# i języka XAML. Poniżej przedstawiono przewodniki określonych dla obu, ale najpierw umożliwia Spójrz na niektóre szczegóły implementacji klucza dla obu.

Kontekst akcji są tworzone przy użyciu `MenuItem`s. Wybierz zdarzenia dla elementów MenuItems, zostaną podniesione przez element MenuItem, nie w elemencie ListView. Różni się to od obsługi zdarzeń naciśnij dla komórki, gdy element ListView zgłasza zdarzenie, a nie w komórce. Ponieważ element ListView jest wywołaniem zdarzenia, jej procedura obsługi zdarzeń otrzymuje kluczowe informacje, takie jak który zaznaczone lub dotknięciu elementu.

Domyślnie element MenuItem nie ma możliwości komórki, która należy do wiedzy. `CommandParameter` jest dostępna w `MenuItem` do przechowywania obiektów, takich jak obiekt za ViewCell element MenuItem. `CommandParameter` można ustawić w języku XAML i C#.

### <a name="c"></a>C#  

Kontekst akcji można zaimplementować w żadnym `Cell` podklasy (pod warunkiem, ponieważ nie jest on używany jako nagłówek grupy) przez utworzenie `MenuItem`s i dodanie ich do `ContextActions` kolekcji dla komórki. Dostępne są następujące właściwości można skonfigurować dla kontekstu akcji:

* **Tekst** &ndash; ciąg, który jest wyświetlany w elemencie menu.
* **Kliknięto** &ndash; zdarzenie, gdy element zostanie kliknięty.
* **IsDestructive** &ndash; (opcjonalnie) w przypadku wartości true elementu jest renderowany inaczej w systemie iOS.

Kilka akcji w kontekście można dodać w komórce, ale powinien mieć tylko jeden `IsDestructive` ustawioną `true`. Poniższy kod przedstawia sposób działania kontekstu zostanie dodany do `ViewCell`:

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

`MenuItem`s można również tworzyć w zbiorze XAML deklaratywnie. XAML poniżej przedstawia komórki niestandardowych z dwóch kontekstu działań:

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

W pliku CodeBehind Sprawdź `Clicked` metody są implementowane:

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
> `NavigationPageRenderer` Dla systemu Android ma możliwym do zastąpienia `UpdateMenuItemIcon` metodę, która może służyć do załadowania ikon z niestandardowego `Drawable`. To zastąpienie pozwala używać obrazów SVG jako ikony na `MenuItem` wystąpienia w systemie Android.

<a name="Pull_to_Refresh" />

## <a name="pull-to-refresh"></a>Przeciągnij odświeżania
Użytkownicy mają pochodzić można oczekiwać, że ściąganie w dół na liście danych zostanie odświeżony tej listy. `ListView` obsługuje ten out-of--box. Aby włączyć funkcję pociągnij, aby odświeżyć, ustaw `IsPullToRefreshEnabled` true:

```csharp
listView.IsPullToRefreshEnabled = true;
```

Ściąganie jest ściągnięcia, aby odświeżyć jako użytkownik:

![](interactivity-images/refresh-start.png "Przeciągnij element ListView, aby odświeżyć w toku")

Ściągnięcia, aby odświeżyć jako użytkownik wydała ściągania. Jest to, co użytkownik zobaczy podczas aktualizujesz listy: ![ ] (interactivity-images/refresh-in-progress.png "przeciągnij element ListView, aby odświeżyć pełną")

Element ListView udostępnia kilka zdarzeń, które pozwalają na odpowiadanie na zdarzenia pociągnij, aby odświeżyć.

-  `RefreshCommand` Zostanie wywołany i `Refreshing` zdarzenia o nazwie. `IsRefreshing` zostanie ustawiona do `true`.
-  Należy wykonać, niezależnie od kodu jest wymagany do Odśwież zawartość widoku listy, w poleceniu lub zdarzeń.
-  Podczas odświeżania się ukończyć, wywołaj `EndRefresh` lub ustaw `IsRefreshing` do `false` mówić widok listy wszystko gotowe.

`CanExecute` Przestrzegane właściwości, które zapewnia możliwość sterowania czy polecenia pociągnij, aby odświeżyć powinno być włączone.



## <a name="related-links"></a>Linki pokrewne

- [Element ListView interakcyjności (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/interactivity)
- [informacje o wersji 1.4](http://forums.xamarin.com/discussion/35451/xamarin-forms-1-4-0-released/)
- [informacje o wersji 1.3](http://forums.xamarin.com/discussion/29934/xamarin-forms-1-3-0-released/)
