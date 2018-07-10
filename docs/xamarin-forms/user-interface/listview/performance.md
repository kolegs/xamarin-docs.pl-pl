---
title: Wydajność ListView
description: Mimo że ListView zaawansowany widok, aby wyświetlić dane, ma pewne ograniczenia. W tym artykule wyjaśniono, jak zapewniające wysoką wydajność dzięki ListView Xamarin.Forms, w aplikacji.
ms.prod: xamarin
ms.assetid: 1B085639-652C-4862-86EB-5D55D32B9395
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/11/2017
ms.openlocfilehash: 906fd60954b18064467e665295dba8bb75ed5a45
ms.sourcegitcommit: 3e980fbf92c69c3dd737554e8c6d5b94cf69ee3a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37935592"
---
# <a name="listview-performance"></a>Wydajność ListView

Podczas pisania aplikacji dla urządzeń przenośnych, jest wydajność. Użytkownicy cenić płynne przewijanie i krótszy czas ładowania szybkie. Kończy się niepowodzeniem spełnić oczekiwania użytkowników będzie koszt klasyfikacje ze sklepu z aplikacjami lub w przypadku aplikacji line-of-business, koszt zaoszczędzić czas i pieniądze.

Mimo że [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) to zaawansowany widok do wyświetlania danych, ma pewne ograniczenia. Przewijanie wydajności mogą występować, korzystając z niestandardowego komórek, szczególnie w przypadku, gdy zawiera widok głęboko zagnieżdżone hierarchie, lub używać niektórych układów wymagającymi wielu miar. Na szczęście są techniki, które można użyć, aby uniknąć pogorszenia wydajności.

<a name="cachingstrategy" />

## <a name="caching-strategy"></a>Strategii buforowania

ListViews są często używane do wyświetlania większej ilości danych niż Dopasuj na ekranie. Należy wziąć pod uwagę aplikację utworów muzycznych, na przykład. Biblioteka utworów może być tysiące wpisów. Proste podejście, będzie można utworzyć wiersza dla każdego utworu, mają niską wydajnością. Takie podejście marnuje cenną pamięć i może zmniejszyć przewinięcie do przeszukiwania. Innym rozwiązaniem jest utworzenie i zniszcz wierszy, zgodnie z danych jest przewijane do widoku. Wymaga to stała podczas tworzenia wystąpienia i oczyszczania widoku obiektów, które mogą być bardzo wolne.

W celu zachowywania pamięci, natywnych [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) odpowiedników dla każdej z platform ma wbudowane funkcje ponowne wykorzystanie wierszy. Tylko komórki, które są widoczne na ekranie są ładowane do pamięci i **zawartości** jest ładowany do istniejących komórek. Zapobiega to aplikacji z konieczności tworzenia wystąpienia tysiące obiektów, oszczędzając czas i pamięci.

Zezwala na platformie Xamarin.Forms [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) komórki ponownego użycia za pomocą [ `ListViewCachingStrategy` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListViewCachingStrategy/) wyliczenia, która ma następujące wartości:

```csharp
public enum ListViewCachingStrategy
{
  RetainElement,   // the default value
  RecycleElement,
  RecycleElementAndDataTemplate
}
```

> [!NOTE]
> Ignoruje uniwersalnej platformy Windows (UWP) [ `RetainElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RetainElement) buforowania strategii, ponieważ zawsze używa pamięci podręcznej do zwiększenia wydajności. W związku z tym, domyślnie działa tak, jakby [ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) strategii buforowania jest stosowany.

### <a name="retainelement"></a>RetainElement

[ `RetainElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RetainElement) Strategii buforowania określa, że [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) wygeneruje komórki dla każdego elementu na liście, a jest ustawieniem domyślnym `ListView` zachowanie. Ogólnie należy jej używać w następujących okolicznościach:

- Jeśli każda komórka ma dużą liczbę powiązania (20-30 +).
- Gdy szablon komórki zmienia się często.
- Podczas testowania wykaże, że `RecycleElement` buforowania strategii skutkuje szybkość wykonywania mniejsze.

Ważne jest, aby rozpoznać konsekwencje [ `RetainElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RetainElement) strategii buforowania, podczas pracy z niestandardowych komórek. Każdy kod inicjowania komórki będzie musiał ją uruchomić dla każdego tworzenia komórki który może być wiele razy na sekundę. W takiej sytuacji technik układu, które zostały poprawnie na stronie, takich jak przy użyciu wielu zagnieżdżonych [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) wystąpień, stają się wąskich gardeł wydajności, gdy są one skonfigurowane i niszczone w czasie rzeczywistym, gdy użytkownik przewija.

<a name="recycleelement" />

### <a name="recycleelement"></a>RecycleElement

[ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) Strategii buforowania określa, że [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) podejmie próbę zminimalizować jej zużycia i wykonywanie prędkości pamięci przez odtwarzanie komórek listy. Ten tryb nie zawsze oferuje lepszą wydajność i testowanie powinno być przeprowadzane w celu ustalenia, jakie ulepszenia. Jednak go jest ogólnie preferowanych przez i powinny być używane w następujących okolicznościach:

- Gdy każda komórka zawiera małą umiarkowana liczba powiązań.
- Gdy każda komórka [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) definiuje wszystkie dane w komórce.
- Gdy każda komórka przypomina stopniu, za pomocą szablonu komórki niezmiennych.

Podczas wirtualizacji komórki będzie zawierać kontekst powiązania, zaktualizować, a więc jeśli aplikacja korzysta z tego trybu musi zapewnić odpowiednio obsługiwany aktualizacje kontekstu powiązania. Wszystkie dane dotyczące komórki muszą pochodzić z kontekstu powiązania lub mogą wystąpić błędy spójności. Można to zrobić za pomocą powiązania danych do wyświetlania danych komórki. Alternatywnie danych komórki powinna być ustawiona `OnBindingContextChanged` zastąpić, a nie w komórce niestandardowego konstruktora, jak pokazano w poniższym przykładzie kodu:

```csharp
public class CustomCell : ViewCell
{
  Image image = null;

  public CustomCell ()
  {
    image = new Image();
    View = image;
  }

  protected override void OnBindingContextChanged ()
  {
    base.OnBindingContextChanged ();

    var item = BindingContext as ImageItem;
    if (item != null) {
      image.Source = item.ImageUrl;
    }
  }
}
```

Aby uzyskać więcej informacji, zobacz [zmiany kontekstu powiązania](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#binding-context-changes).

W systemach iOS i Android komórek użycia niestandardowe programy renderujące, muszą one zapewnić powiadomienie o zmianie właściwości jest poprawnie zaimplementowana. Gdy komórek są ponownie ich wartości właściwości ulegnie zmianie podczas aktualizowania kontekstu powiązania, dostępne komórki za pomocą `PropertyChanged` zdarzeń zgłaszanych. Aby uzyskać więcej informacji, zobacz [Dostosowywanie obiektu ViewCell](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md).

#### <a name="recycleelement-with-a-datatemplateselector"></a>RecycleElement z DataTemplateSelector

Gdy [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) używa [ `DataTemplateSelector` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/) wybrać [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/), [ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) buforowania Strategia nie będzie buforować `DataTemplate`s. Zamiast tego `DataTemplate` został wybrany do każdego elementu danych na liście.

> [!NOTE]
> [ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) Strategii buforowania ma jako warunek wstępny, wprowadzona w Xamarin.Forms 2.4, że w przypadku [ `DataTemplateSelector` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/) jest proszony o wybranie [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/)każdy `DataTemplate` musi zwracać taki sam [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) typu. Na przykład, biorąc pod uwagę [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) z `DataTemplateSelector` może zwracać albo `MyDataTemplateA` (gdzie `MyDataTemplateA` zwraca `ViewCell` typu `MyViewCellA`), lub `MyDataTemplateB` (gdzie `MyDataTemplateB`zwraca `ViewCell` typu `MyViewCellB`), gdy `MyDataTemplateA` jest zwracany, aplikacja musi zwracać `MyViewCellA` lub zostanie zgłoszony wyjątek.

### <a name="recycleelementanddatatemplate"></a>RecycleElementAndDataTemplate

[ `RecycleElementAndDataTemplate` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElementAndDataTemplate) Strategii buforowania opiera się na [ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) buforowania strategii dodatkowo zapewniając, że w przypadku [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) używa [ `DataTemplateSelector` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/) wybrać [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/), `DataTemplate`s są buforowane przez typ elementu na liście. W związku z tym `DataTemplate`s wybrano jeden raz dla typu elementu, a nie raz na każde wystąpienie elementu.

> [!NOTE]
> [ `RecycleElementAndDataTemplate` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElementAndDataTemplate) Strategii buforowania ma jako warunek wstępny, `DataTemplate`s zwrócony przez [ `DataTemplateSelector` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/) należy użyć [ `DataTemplate` ](https://developer.xamarin.com/api/constructor/Xamarin.Forms.DataTemplate.DataTemplate/p/System.Type/) Konstruktor pobierający `Type`.

### <a name="setting-the-caching-strategy"></a>Ustawianie strategii buforowania

[ `ListViewCachingStrategy` ](xref:Xamarin.Forms.ListViewCachingStrategy) Wartość wyliczenia jest określony za pomocą [ `ListView` ](xref:Xamarin.Forms.ListView) przeciążenia konstruktora, jak pokazano w poniższym przykładzie kodu:

```csharp
var listView = new ListView(ListViewCachingStrategy.RecycleElement);
```

W XAML, ustaw `CachingStrategy` atrybutu, jak pokazano w poniższym kodzie:

```xaml
<ListView CachingStrategy="RecycleElement">
    <ListView.ItemTemplate>
        <DataTemplate>
            <ViewCell>
              ...
            </ViewCell>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

Ma to taki sam efekt jak ustawienie pamięci podręcznej argumentu strategii w Konstruktorze w języku C#; należy pamiętać, że ma nie `CachingStrategy` właściwość `ListView`.

#### <a name="setting-the-caching-strategy-in-a-subclassed-listview"></a>Ustawianie strategii buforowania w ListView będące podklasami

Ustawienie `CachingStrategy` atrybut w XAML będące podklasami [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) nie przyniesie żądane zachowanie, ponieważ nie istnieje żadne `CachingStrategy` właściwość `ListView`. Ponadto jeśli [XAMLC](~/xamarin-forms/xaml/xamlc.md) jest włączona, komunikat o błędzie zostaną wygenerowane: **nie właściwości, właściwości możliwej do wiązania lub zdarzeń, znaleziono "CachingStrategy"**

Rozwiązanie tego problemu jest określenie konstruktora na będące podklasami [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) akceptujący [ `ListViewCachingStrategy` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListViewCachingStrategy/) parametr i przekazuje ją do klasy bazowej:

```csharp
public class CustomListView : ListView
{
  public CustomListView (ListViewCachingStrategy strategy) : base (strategy)
  {
  }
  ...
}
```

A następnie [ `ListViewCachingStrategy` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListViewCachingStrategy/) można określić wartość wyliczenia z XAML przy użyciu `x:Arguments` składni:

```xaml
<local:CustomListView>
  <x:Arguments>
    <ListViewCachingStrategy>RecycleElement</ListViewCachingStrategy>
  </x:Arguments>
</local:CustomListView>
```

<a name="improving-performance" />

## <a name="improving-listview-performance"></a>Poprawianie wydajności ListView

Istnieje wiele technik dla poprawy wydajności `ListView`:

-  Powiąż `ItemsSource` właściwości `IList<T>` zbiór zamiast `IEnumerable<T>` kolekcji, ponieważ `IEnumerable<T>` kolekcje nie obsługują dostępu swobodnego.
-  Użyj wbudowanych komórek (takich jak `TextCell`  /  `SwitchCell` ) zamiast `ViewCell` zawsze, gdy można.
-  Za pomocą mniejszej liczby elementów. Na przykład należy rozważyć użycie jednej `FormattedString` etykiety zamiast wielu etykiet.
-  Zastąp `ListView` z `TableView` przy wyświetlaniu niehomogenicznych danych — czyli danych różnych typów.
-  Ograniczenia używania [ `Cell.ForceUpdateSize` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Cell.ForceUpdateSize()/) metody. Nadmiernie obciążany, spowoduje obniżenia wydajności.
-  W systemie Android, należy unikać ustawienia `ListView`firmy widoczność separator wierszy lub kolor po utworzeniu wystąpienia, ponieważ powoduje to spadek wydajności.
-  Unikanie zmieniania układu komórek na podstawie [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/). Wiąże się z tym dużych kosztów układ i inicjowania.
-  Należy unikać układ głęboko zagnieżdżone hierarchie. Użyj `AbsoluteLayout` lub `Grid` z myślą o zmniejszeniu zagnieżdżania.
-  Należy unikać określonych `LayoutOptions` innych niż `Fill` (wypełnienia jest najtańsze do obliczeń).
-  Unikaj umieszczania `ListView` wewnątrz `ScrollView` z następujących powodów:
    - `ListView` Implementuje własne przewijania.
    - `ListView` Nie będzie otrzymywać wszystkie gesty, jak będzie obsługiwany przez nadrzędne `ScrollView`.
    - `ListView` z elementów listy, funkcje potencjalnie oferty, który może powodować dostosowany nagłówek i stopka, który przewija `ScrollView` był używany. Aby uzyskać więcej informacji, zobacz [nagłówków i stopek](~/xamarin-forms/user-interface/listview/customizing-list-appearance.md#Headers_and_Footers).
-  Jeśli potrzebujesz bardzo specyficzny, złożonych projektów, znajdujące się w Twojej komórek, należy wziąć pod uwagę niestandardowego modułu renderowania.

`AbsoluteLayout` ma możliwość wykonywania układów bez wywołania jednej miary. Dzięki temu ogromne wydajności. Jeśli `AbsoluteLayout` nie może być używany, należy wziąć pod uwagę [ `RelativeLayout` ](http://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/). Jeśli przy użyciu `RelativeLayout`, bezpośrednie przekazanie ograniczenia będzie znacznie szybciej niż przy użyciu wyrażenia interfejsu API. Wynika to z interfejsu API w wyrażeniu JIT, a w systemie iOS drzewa ma interpretować, który jest wolniejszy. Wyrażenie interfejsu API jest odpowiednia dla układy stron, gdzie jest tylko wymagane na wstępny układ i rotacji, ale w `ListView`, w którym jest uruchamiany stale podczas przewijania, jego dobrze jest wydajność.

Tworzenie niestandardowego modułu renderowania dla [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) lub jej komórek jest jednym z podejść do zmniejszenia wpływu obliczenia układ na przewijanie wydajności. Aby uzyskać więcej informacji, zobacz [Dostosowywanie ListView](~/xamarin-forms/app-fundamentals/custom-renderer/listview.md) i [Dostosowywanie obiektu ViewCell](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md).


## <a name="related-links"></a>Linki pokrewne

- [Widok niestandardowy moduł renderowania (przykład)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithListviewNative/)
- [Niestandardowe renderowanie ViewCell (przykład)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/viewcell/)
- [ListViewCachingStrategy](https://developer.xamarin.com/api/type/Xamarin.Forms.ListViewCachingStrategy/)
