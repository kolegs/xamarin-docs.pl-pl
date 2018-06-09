---
title: Wydajność ListView
description: Mimo że element ListView jest zaawansowanym widok, aby wyświetlić dane, ma pewne ograniczenia. W tym artykule wyjaśniono, jak zapewnić doskonałą wydajność z platformy Xamarin.Forms ListView w aplikacji.
ms.prod: xamarin
ms.assetid: 1B085639-652C-4862-86EB-5D55D32B9395
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/11/2017
ms.openlocfilehash: 4803a612e2b06e458f2859dbbbd30b970f0fc8ea
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35244906"
---
# <a name="listview-performance"></a>Wydajność ListView

Podczas pisania aplikacji dla urządzeń przenośnych, ma znaczenie wydajności. Użytkownicy pochodzić oczekiwać płynnego przewijania i czasy ładowania szybkie. Nie spełniają oczekiwań użytkowników będzie koszt klasyfikacje sklepu z aplikacjami lub w przypadku aplikacji biznesowych — koszt zaoszczędzić czas i pieniądze.

Mimo że [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) jest zaawansowanym widok do wyświetlania danych, ma pewne ograniczenia. Przewijanie wydajności mogą występować podczas korzystania z niestandardowych komórek, szczególnie w przypadku, gdy zawiera widok głęboko zagnieżdżone hierarchie lub używać niektórych układów wymagające dużej miary. Na szczęście są metod, które można użyć w celu uniknięcia pogorszenie wydajności.

<a name="cachingstrategy" />

## <a name="caching-strategy"></a>Strategii buforowania

Widokach listy są często używane do wyświetlania znacznie więcej danych niż dopasowania na ekranie. Należy wziąć pod uwagę aplikacji utworów muzycznych, np. Biblioteka utworów może być tysiące wpisów. Proste podejście, które będzie można utworzyć wiersza dla każdego utworu musi pogorszenie wydajności. Takie podejście marnuje cenną pamięć i może zmniejszyć przewijanie do przeszukiwania. Innym rozwiązaniem jest utworzenie i zniszcz wierszy, zgodnie z danych jest wyświetlony w wyniku przewijania. Wymaga to stałej wystąpienia i oczyszczania widoku obiektów, które mogą być bardzo wolno.

W celu zachowywania pamięci, natywnego [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) odpowiedniki dla poszczególnych platform ma wbudowane funkcje ponownie przy użyciu wierszy. Tylko komórki widoczne na ekranie są ładowane w pamięci i **zawartości** jest ładowany do istniejących komórek. Zapobiega to aplikacji z konieczności tworzenia wystąpienia tysiące obiektów, oszczędzając czas i pamięci.

Zezwala na platformy Xamarin.Forms [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) komórki ponownego użycia za pomocą [ `ListViewCachingStrategy` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListViewCachingStrategy/) wyliczenia, który ma następujące wartości:

```csharp
public enum ListViewCachingStrategy
{
  RetainElement,   // the default value
  RecycleElement,
  RecycleElementAndDataTemplate
}
```

> [!NOTE]
> Ignoruje platformy uniwersalnej systemu Windows (UWP) [ `RetainElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RetainElement/) buforowanie strategii, ponieważ zawsze używa buforowanie w celu zwiększenia wydajności. W związku z tym domyślnie działa tak, jakby [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/) buforowanie strategii została zastosowana.

### <a name="retainelement"></a>RetainElement

[ `RetainElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RetainElement/) Buforowanie strategii Określa, że [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) wygeneruje komórki dla każdego elementu na liście, a wartość domyślna to `ListView` zachowanie. Powinien on zazwyczaj używane w następujących okolicznościach:

- Podczas każdej komórki ma dużą liczbę powiązania (20-30 +).
- Gdy szablonu komórki zmienia się często.
- Podczas testowania wykaże, że `RecycleElement` buforowanie strategii powoduje wykonanie zmniejszenie szybkości.

Ważne jest, aby rozpoznać konsekwencje [ `RetainElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RetainElement/) buforowanie strategii podczas pracy z niestandardowych komórek. Dowolny kod inicjujący komórki będzie musiał uruchomić tworzenie każdej komórki, które mogą być wiele razy w ciągu sekundy. W takim przypadku układu techniki, które zostały poprawnie na stronie, takich jak przy użyciu wielu zagnieżdżonych [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) wystąpień, stają się wąskich gardeł wydajności, gdy są one skonfigurowane i zniszczone w czasie rzeczywistym, gdy użytkownik przewija.

<a name="recycleelement" />

### <a name="recycleelement"></a>RecycleElement

[ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/) Buforowanie strategii Określa, że [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) podejmie próbę zminimalizować jego rozmiaru i wykonywania szybkość pamięci komórek listy odtwarzania. W tym trybie nie zawsze oferuje lepszą wydajność i testowania należy wykonać, aby ustalić, jakie ulepszenia. Jednak preferowanym rozwiązaniem jest ogólnie rzecz biorąc i powinien być używany w następujących okolicznościach:

- Jeśli każda komórka zawiera małą umiarkowanej liczbie powiązania.
- Podczas każdej komórki [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) definiuje wszystkie dane komórki.
- Podczas każdej komórki jest przede wszystkim podobnie, niezmiennych szablonu komórki.

Podczas wirtualizacji komórki zostanie zaktualizowane kontekst powiązania, i dlatego jeśli aplikacja korzysta z tego trybu go musi zapewnić odpowiednio obsługi aktualizacji kontekstu powiązania. Wszystkie dane dotyczące komórki muszą pochodzić z kontekstu powiązania lub mogą wystąpić błędy spójności. Można to zrobić za pomocą powiązania danych do wyświetlenia danych komórki. Alternatywnie komórki danych powinien być ustawiony `OnBindingContextChanged` zastąpienie, a nie w Konstruktorze komórki niestandardowych, jak pokazano w poniższym przykładzie:

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

W systemach iOS i Android użycie komórek niestandardowe moduły renderowania, muszą one zapewnić powiadomienia o zmianie właściwości jest poprawnie zaimplementowana. Po komórki są ponownie ich wartości właściwości zostaną zmienione po zaktualizowaniu niż dostępna komórki, kontekst powiązania z `PropertyChanged` zdarzeń zgłaszanych. Aby uzyskać więcej informacji, zobacz [Dostosowywanie ViewCell](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md).

#### <a name="recycleelement-with-a-datatemplateselector"></a>RecycleElement z DataTemplateSelector

Gdy [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) używa [ `DataTemplateSelector` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/) wybierz [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/), [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/) buforowanie Strategia nie będzie buforować `DataTemplate`s. Zamiast tego `DataTemplate` wybrano dla każdego elementu danych na liście.

> [!NOTE]
> [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/) Buforowanie strategii ma warunek wstępny, wprowadzone w 2.4 platformy Xamarin.Forms, że w przypadku [ `DataTemplateSelector` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/) jest wyświetlony monit o wybranie [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/)każdy `DataTemplate` musi zwracać taki sam [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) typu. Na przykład [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) z `DataTemplateSelector` które może zwracać `MyDataTemplateA` (gdzie `MyDataTemplateA` zwraca `ViewCell` typu `MyViewCellA`), lub `MyDataTemplateB` (gdzie `MyDataTemplateB`zwraca `ViewCell` typu `MyViewCellB`), gdy `MyDataTemplateA` jest zwracany, aplikacja musi zwracać `MyViewCellA` lub zostanie wygenerowany wyjątek.

### <a name="recycleelementanddatatemplate"></a>RecycleElementAndDataTemplate

[ `RecycleElementAndDataTemplate` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElementAndDataTemplate/) Buforowanie strategii opiera się na [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/) buforowanie strategii dodatkowo zapewniając, że w przypadku [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) używa [ `DataTemplateSelector` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/) wybierz [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/), `DataTemplate`s są buforowane przez typ elementu na liście. W związku z tym `DataTemplate`s wybrano jeden raz dla typu elementu, zamiast raz dla każdego wystąpienia elementu.

> [!NOTE]
> [ `RecycleElementAndDataTemplate` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElementAndDataTemplate/) Buforowanie strategii ma warunek wstępny który `DataTemplate`s zwrócony przez [ `DataTemplateSelector` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/) należy użyć [ `DataTemplate` ](https://developer.xamarin.com/api/constructor/Xamarin.Forms.DataTemplate.DataTemplate/p/System.Type/) Konstruktor pobierający `Type`.

### <a name="setting-the-caching-strategy"></a>Ustawienie strategii buforowania

[ `ListViewCachingStrategy` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListViewCachingStrategy/) Wartość wyliczenia jest określona z [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) przeładowania konstruktora, jak pokazano w poniższym przykładzie:

```csharp
var listView = new ListView(ListViewCachingStrategy.RecycleElement);
```

W języku XAML, należy ustawić `CachingStrategy` atrybutu, jak pokazano w poniższym kodzie:

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

Jest to ten sam efekt co ustawienie buforowania argumentu strategii w Konstruktorze w języku C#; należy pamiętać, że ma nie `CachingStrategy` właściwość `ListView`.

#### <a name="setting-the-caching-strategy-in-a-subclassed-listview"></a>Ustawienie strategii buforowania w elemencie ListView będące podklasami

Ustawienie `CachingStrategy` atrybut w XAML podklasą [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) nie przyniesie zachowanie, ponieważ istnieje nie `CachingStrategy` właściwość `ListView`. Ponadto jeśli [XAMLC](~/xamarin-forms/xaml/xamlc.md) jest włączona, komunikat o błędzie zostanie wygenerowany: **nie właściwości, można powiązać właściwości lub zdarzeń, znaleziono "CachingStrategy"**

Rozwiązanie tego problemu jest zdefiniowanie konstruktora w podklasą [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) która akceptuje [ `ListViewCachingStrategy` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListViewCachingStrategy/) parametru i przekazuje ją do klasy podstawowej:

```csharp
public class CustomListView : ListView
{
  public CustomListView (ListViewCachingStrategy strategy) : base (strategy)
  {
  }
  ...
}
```

Następnie przy użyciu [ `ListViewCachingStrategy` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListViewCachingStrategy/) można określić wartość wyliczenia z XAML przy użyciu `x:Arguments` składni:

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

-  Powiąż `ItemsSource` właściwości `IList<T>` kolekcji zamiast `IEnumerable<T>` kolekcji, ponieważ `IEnumerable<T>` kolekcje nie obsługują dostępie.
-  Użyj wbudowanego komórek (takich jak `TextCell`  /  `SwitchCell` ) zamiast `ViewCell` przy każdej można.
-  Użyj mniejszej liczby elementów. Na przykład należy rozważyć użycie pojedynczego `FormattedString` etykiety zamiast wielu etykiet.
-  Zastąp `ListView` z `TableView` podczas wyświetlania niehomogenicznych danych — to znaczy różnych typów danych.
-  Ograniczenia używania [ `Cell.ForceUpdateSize` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Cell.ForceUpdateSize()/) metody. Jeśli nadmiernego obciążenia, obniży wydajności.
-  W systemie Android, należy unikać ustawienie `ListView`na widoczność separator wierszy lub kolor po utworzeniu wystąpienia, ponieważ powoduje zmniejszenie wydajności dużych.
-  Należy unikać zmiany układu komórek, na podstawie [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/). Wiąże się z tym duże koszty układ i inicjowania.
-  Unikaj układu głęboko zagnieżdżone hierarchie. Użyj `AbsoluteLayout` lub `Grid` umożliwiają zmniejszenie zagnieżdżenia.
-  Unikaj określonych `LayoutOptions` innych niż `Fill` (Fill ma cheapest obliczeniowe).
-  Unikaj umieszczania `ListView` wewnątrz `ScrollView` z następujących powodów:
  - `ListView` Implementuje własną przewijania.
  - `ListView` Nie będzie otrzymywać wszystkie gesty, jak jest realizowany przez element nadrzędny `ScrollView`.
  - `ListView` Może ona powodować dostosowane nagłówku i stopce, który przewija elementy listy, funkcje potencjalnie oferty, które `ScrollView` użyto. Aby uzyskać więcej informacji, zobacz [nagłówków i stopek](~/xamarin-forms/user-interface/listview/customizing-list-appearance.md#Headers_and_Footers).
-  Jeśli potrzebujesz bardzo specyficznego, złożone projektu przedstawionych w Twojej komórek, należy wziąć pod uwagę niestandardowego modułu renderowania.

`AbsoluteLayout` ma możliwość wykonania układów bez wywołania jednej miary. Dzięki temu można bardzo zaawansowane wydajności. Jeśli `AbsoluteLayout` nie może być używane, należy wziąć pod uwagę [ `RelativeLayout` ](http://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/). Jeśli przy użyciu `RelativeLayout`, przekazywanie ograniczenia bezpośrednio będzie znacznie szybciej niż przy użyciu wyrażenia interfejsu API. Wynika to z interfejsu API w wyrażeniu JIT, a w systemie iOS drzewa ma być interpretowane, który jest wolniejszy. Wyrażenie interfejsu API jest odpowiedni dla układy stron, gdzie go tylko wymagane na początkowy układ i obracanie, ale w `ListView`, w którym jest uruchamiany stale podczas przewijania, powinna ona wydajności.

Tworzenie niestandardowego modułu renderowania dla [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) lub jej komórek jest jednym z podejść do zmniejszenia wpływu układu obliczenia na przewijanie wydajności. Aby uzyskać więcej informacji, zobacz [Dostosowywanie ListView](~/xamarin-forms/app-fundamentals/custom-renderer/listview.md) i [Dostosowywanie ViewCell](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md).


## <a name="related-links"></a>Linki pokrewne

- [Widok niestandardowy moduł renderowania (przykład)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithListviewNative/)
- [Niestandardowe renderowanie ViewCell (przykład)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/viewcell/)
- [ListViewCachingStrategy](https://developer.xamarin.com/api/type/Xamarin.Forms.ListViewCachingStrategy/)
