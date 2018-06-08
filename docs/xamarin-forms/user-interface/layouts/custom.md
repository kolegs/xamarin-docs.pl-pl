---
title: Tworzenie układu niestandardowego
description: Platformy Xamarin.Forms definiuje cztery układ klasy — StackLayout, AbsoluteLayout RelativeLayout i siatki, i każdego Rozmieszcza elementy podrzędne w inny sposób. Jednak czasami jest niezbędne do obsługi zawartości strony przy użyciu układu nie został dostarczony przez platformy Xamarin.Forms. W tym artykule opisano sposób tworzenia niestandardowego układu klasy i przedstawia klasę WrapLayout liter orientacji Rozmieszcza elementy podrzędne w poziomie na stronie, a następnie opakowuje wyświetlanie kolejnych elementów podrzędnych do dodatkowych wierszy.
ms.prod: xamarin
ms.assetid: B0CFDB59-14E5-49E9-965A-3DCCEDAC2E31
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/29/2017
ms.openlocfilehash: 7f2a443ded0d6b41db237e9842cd80e016bb5251
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/07/2018
ms.locfileid: "34847750"
---
# <a name="creating-a-custom-layout"></a>Tworzenie układu niestandardowego

_Platformy Xamarin.Forms definiuje cztery układ klasy — StackLayout, AbsoluteLayout RelativeLayout i siatki, i każdego Rozmieszcza elementy podrzędne w inny sposób. Jednak czasami jest niezbędne do obsługi zawartości strony przy użyciu układu nie został dostarczony przez platformy Xamarin.Forms. W tym artykule opisano sposób tworzenia niestandardowego układu klasy i przedstawia klasę WrapLayout liter orientacji Rozmieszcza elementy podrzędne w poziomie na stronie, a następnie opakowuje wyświetlanie kolejnych elementów podrzędnych do dodatkowych wierszy._

## <a name="overview"></a>Omówienie

W przypadku platformy Xamarin.Forms, wszystkie klasy układu pochodzi od [ `Layout<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/) klasy i ograniczenie typu ogólnego, aby [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) i jego typów pochodnych. Z kolei `Layout<T>` pochodną klasy [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/) klasy, która udostępnia mechanizm dla położenia i rozmiaru podrzędnych elementów.

Każdy element wizualny jest odpowiedzialny za określenie własnych preferowany rozmiar, znany jako *żądany* rozmiar. [`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/), [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/), i [ `Layout<View>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/) typy pochodne są zobowiązani do określenia lokalizacji i rozmiaru podrzędnych lub elementy podrzędne względem siebie. W związku z tym układu obejmuje relacji nadrzędny podrzędny, gdzie element nadrzędny Określa, co powinna być rozmiar jego elementów podrzędnych, ale będzie podejmować próby pomieścić żądany rozmiar elementu podrzędnego.

Dokładne zrozumienie platformy Xamarin.Forms układ i unieważniania cyklu jest wymagane do utworzenia niestandardowego układu. Omówione zostaną teraz te cykle.

## <a name="layout"></a>Układ

Układ rozpoczyna się w górnej części drzewa wizualnego ze stroną, a obejmującego wszystkie gałęzie drzewa wizualnego na każdy element wizualny na stronie. Elementy nadrzędne z innymi elementami są zobowiązani do zmiany rozmiaru i pozycjonowania ich elementy podrzędne względem siebie.

[ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/) Klasa definiuje [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/) metoda element dla operacji układu i [ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/) metodę, która określa prostokątny obszar element zostanie umieszczony wewnątrz. Podczas uruchamiania aplikacji, a pierwsza strona jest wyświetlana, *cyklu układu* składające się pierwszego dnia `Measure` wywołań, a następnie `Layout` wywołuje, rozpoczyna na [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) obiektu:

1. Podczas cyklu układu, co element nadrzędny jest odpowiedzialny za wywoływanie `Measure` metody dla jego elementów podrzędnych.
1. Po mierzonych dzieci co element nadrzędny jest odpowiedzialny za wywoływanie `Layout` metody dla jego elementów podrzędnych.

Ten cykl gwarantuje, że każdy element wizualny na stronie otrzyma wywołań `Measure` i `Layout` metody. Proces przedstawiono na poniższym diagramie:

![](custom-images/layout-cycle.png "Cykl układ platformy Xamarin.Forms")

> [!NOTE]
> Należy pamiętać, że cykle układu również może wystąpić w podzestawie drzewa wizualnego, jeśli coś zmiany wpływają na układ. W tym elementów jest dodany lub usunięty z kolekcji takim jak [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/), zmiany w [ `IsVisible` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsVisible/) właściwości elementu lub zmianę w rozmiarze elementu.

Każda klasa platformy Xamarin.Forms, który ma `Content` lub `Children` właściwość ma możliwym do zastąpienia [ `LayoutChildren` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildren/p/System.Double/System.Double/System.Double/System.Double/) metody. Klasy niestandardowego układu, które pochodzą z [ `Layout<View>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/) należy przesłonić tę metodę i upewnij się, że [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/) i [ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/) metody wywołana dla wszystkich elementów podrzędnych, aby zapewnić odpowiednią niestandardowego układu.

Ponadto każda klasa która pochodzi z [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/) lub [ `Layout<View>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/) przesłonięcie [ `OnMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/) metodę, która jest w przypadku, gdy klasa układu Określa rozmiar, który musi on być przy wywołaniach [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/) metod jego elementów podrzędnych.

> [!NOTE]
> Elementy określić ich rozmiaru na podstawie *ograniczenia*, która wskazuje, ile miejsca jest dostępne dla elementu w ramach jego elementu nadrzędnego. Ograniczenia przekazany do [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/) i [ `OnMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/) metody mogą należeć do zakresu od 0 do `Double.PositiveInfinity`. Element *ograniczone*, lub *pełni ograniczonego*, gdy odbierze wywołanie jego [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/) — metoda z argumentami z systemem innym niż nieskończone — element jest ograniczony do określonego rozmiaru. Element *nieograniczonego*, lub *częściowo ograniczone*, gdy odbierze wywołanie jego `Measure` metody z co najmniej jednego argumentu równa `Double.PositiveInfinity` — mogą być nieskończone ograniczenia traktować jako wskazujący automatyczna zmiana rozmiaru.

## <a name="invalidation"></a>Unieważnienie

Unieważnienie to proces, za pomocą której zmiana elementu na stronie wyzwala nowy cykl układu. Elementy są uznawane za nieprawidłowe, gdy mają one już prawidłowe rozmiaru lub położenia. Na przykład jeśli [ `FontSize` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.FontSize/) właściwość [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) zmiany, `Button` , jest określany jest nieprawidłowy, ponieważ nie ma właściwego rozmiaru. Zmiana rozmiaru `Button` następnie może mieć wpływ zmian w układzie za pośrednictwem pozostałej części strony.

Elementy unieważnienie się za pomocą [ `InvalidateMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.InvalidateMeasure()/) metody, zazwyczaj podczas właściwości elementu zmiany nowy rozmiar elementu, który może spowodować. Ta metoda generowane [ `MeasureInvalidated` ](https://developer.xamarin.com/api/event/Xamarin.Forms.VisualElement.MeasureInvalidated/) zdarzenie, które obsługuje jego elementu nadrzędnego do wyzwolenia nowy cykl układu.

[ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/) Klasy Ustawia program obsługi [ `MeasureInvalidated` ](https://developer.xamarin.com/api/event/Xamarin.Forms.VisualElement.MeasureInvalidated/) zdarzenia w każdej podrzędnej dodane do jego `Content` właściwości lub `Children` kolekcji i odłącza obsługi podczas podrzędne zostaną usunięte. W związku z tym każdego elementu w drzewie wizualnym elementów podrzędnych jest alert, zawsze, gdy jeden z jego elementów podrzędnych zmienia rozmiar. Na poniższym diagramie przedstawiono, jak zmianę w rozmiarze elementu w drzewie wizualnym może spowodować zmiany, które ripple w górę drzewa:

![](custom-images/invalidation.png "Unieważnienie w drzewie wizualnym")

Jednak `Layout` klasa próbuje ograniczyć ich wpływ na zmianę w rozmiarze elementu podrzędnego na układ strony. Układ ma rozmiar ograniczone, następnie zmiany rozmiaru podrzędnych nie wpływa na niczego wyższe niż układu nadrzędnego w drzewie wizualnym. Jednak zwykle zmianę w rozmiarze układu ma wpływ na sposób układ Rozmieszcza elementy podrzędne. W związku z tym każda zmiana w rozmiarze układu i rozpocznie się cykl układu dla układu układ będzie odbierać wywołań jego [ `OnMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/) i [ `LayoutChildren` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildren/p/System.Double/System.Double/System.Double/System.Double/) metody.

[ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/) Klasy definiuje również [ `InvalidateLayout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.InvalidateLayout()/) metody, której cel podobne do [ `InvalidateMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.InvalidateMeasure()/) metody. `InvalidateLayout` Metoda powinna być wywoływana, gdy zostaną zmienione, który ma wpływ na sposób układ Określa położenie i rozmiar jego elementów podrzędnych. Na przykład `Layout` klasy wywołuje `InvalidateLayout` zawsze, gdy jest elementem podrzędnym jest dodane lub usunięte z układu.

[ `InvalidateLayout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.InvalidateLayout()/) Może zostać zastąpiona w celu wdrożenia pamięci podręcznej w celu zminimalizowania powtarzane wywołania [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/) metody układ elementów podrzędnych. Zastępowanie `InvalidateLayout` metoda zapewnia powiadomienie z informacją o gdy elementy podrzędne są dodane lub usunięte z układu. Podobnie [ `OnChildMeasureInvalidated` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.OnChildMeasureInvalidated()/) metody może zostać zastąpiona w celu Podaj powiadomienie, gdy jeden z elementów podrzędnych układu zmienia rozmiar. Dla obu zamienników metod niestandardowego układu powinien odpowiadać przez wyczyszczenie pamięci podręcznej. Aby uzyskać więcej informacji, zobacz [Obliczanie i buforowanie danych](#caching).

## <a name="creating-a-custom-layout"></a>Tworzenie układu niestandardowego

Proces tworzenia niestandardowego układu wygląda następująco:

1. Utwórz klasę pochodną od klasy `Layout<View>`. Aby uzyskać więcej informacji, zobacz [tworzenie WrapLayout](#creating).
1. [*opcjonalne*] Dodaj właściwości, obsługiwana przez właściwości, parametrów, które powinien być ustawiony na klasie układu. Aby uzyskać więcej informacji, zobacz [Dodawanie właściwości obsługiwana przez właściwości](#adding_properties).
1. Zastąpienie [ `OnMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/) metody do wywołania [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/) metody w wszystkich układ elementów podrzędnych i wróć żądany rozmiar dla układu. Aby uzyskać więcej informacji, zobacz [przesłaniania metody OnMeasure](#onmeasure).
1. Zastąpienie [ `LayoutChildren` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildren/p/System.Double/System.Double/System.Double/System.Double/) metody do wywołania [ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/) metody dla wszystkich układ elementów podrzędnych. Nie można wywołać [ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/) metody dla każdego elementu podrzędnego w układzie spowoduje podrzędnych nigdy nie odbieranie poprawne rozmiaru lub położenia i dlatego dziecko nie będzie widoczny na stronie. Aby uzyskać więcej informacji, zobacz [przesłaniania metody LayoutChildren](#layoutchildren).

  > [!NOTE]
>  Podczas wyliczania elementów podrzędnych w [ `OnMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/) i [ `LayoutChildren` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildren/p/System.Double/System.Double/System.Double/System.Double/) zastąpienia, Pomiń dziecka, którego [ `IsVisible` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsVisible/) właściwość jest ustawiona na `false`. Zapewni to niestandardowego układu nie zostawić miejsce dla dzieci niewidoczne.

1. [*opcjonalne*] Zastąp [ `InvalidateLayout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.InvalidateLayout()/) metodę, aby otrzymać powiadomienie, gdy elementy podrzędne są dodane lub usunięte z układu. Aby uzyskać więcej informacji, zobacz [przesłaniania metody InvalidateLayout](#invalidatelayout).
1. [*opcjonalne*] Zastąp [ `OnChildMeasureInvalidated` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.OnChildMeasureInvalidated()/) metodę, aby otrzymać powiadomienie, gdy jeden z elementów podrzędnych układu zmienia rozmiar. Aby uzyskać więcej informacji, zobacz [przesłaniania metody OnChildMeasureInvalidated](#onchildmeasureinvalidated).

> [!NOTE]
> Należy pamiętać, że [ `OnMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/) zastąpienie nie można wywołać, jeśli rozmiar układ podlega jego elementu nadrzędnego, a nie jej elementy podrzędne. Jednak zostanie wywołany zastąpienie, czy co najmniej jeden z warunków ograniczających jest nieograniczony, czy klasa układu ma niedomyślny [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) lub [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) wartości właściwości. Z tego powodu [ `LayoutChildren` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildren/p/System.Double/System.Double/System.Double/System.Double/) zastąpienie nie zależą od rozmiarów podrzędny uzyskany podczas [ `OnMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/) wywołania metody. Zamiast tego `LayoutChildren` należy wywołać [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/) metody na układ elementów podrzędnych, zanim wywoła [ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/) metody. Alternatywnie rozmiar elementu podrzędnego uzyskanych w `OnMeasure` zastąpienie mogą być buforowane, aby uniknąć później `Measure` wywołań w `LayoutChildren` zastąpienie, ale klasa układu muszą dowiedzieć się, gdy trzeba ponownie uzyskać rozmiary. Aby uzyskać więcej informacji, zobacz [Obliczanie i buforowanie danych układu](#caching).

Układ klasy mogą być następnie używane przez dodanie go do [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)oraz przez dodanie elementów podrzędnych do układu. Aby uzyskać więcej informacji, zobacz [zużywa WrapLayout](#consuming).

<a name="creating" />

### <a name="creating-a-wraplayout"></a>Tworzenie WrapLayout

Aplikacja przykładowa prezentuje wrażliwych orientacji `WrapLayout` klasy, która Rozmieszcza elementy podrzędne w poziomie na stronie, a następnie opakowuje wyświetlanie kolejnych elementów podrzędnych do dodatkowych wierszy.

`WrapLayout` Klasy przydziela samo miejsca dla każdego elementu podrzędnego, znany jako *komórki rozmiar*, na podstawie maksymalnego rozmiaru elementu podrzędnego. Mniejsze niż rozmiar komórki może być umieszczony wewnątrz komórki przy realizacji elementów podrzędnych na podstawie ich [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) i [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) wartości właściwości.

`WrapLayout` Definicji klasy przedstawiono w poniższym przykładzie kodu:

```csharp
public class WrapLayout : Layout<View>
{
  Dictionary<Size, LayoutData> layoutDataCache = new Dictionary<Size, LayoutData>();
  ...
}
```

<a name="caching" />

#### <a name="calculating-and-caching-layout-data"></a>Obliczanie i buforowanie danych układu

`LayoutData` Struktury przechowuje dane o kolekcji elementów podrzędnych na wiele właściwości:

- `VisibleChildCount` — liczbę elementów podrzędnych, które są widoczne w układzie.
- `CellSize` — Maksymalny rozmiar wszystkich obiektów podrzędnych, dostosowane do rozmiaru układu.
- `Rows` — Liczba wierszy.
- `Columns` — Liczba kolumn.

`layoutDataCache` Pole jest używane do przechowywania wielu `LayoutData` wartości. Po uruchomieniu aplikacji dwa `LayoutData` obiekty będą buforowane na `layoutDataCache` słownik na potrzeby bieżącej orientacji — jeden dla argumenty ograniczenia `OnMeasure` zastępowania i jeden dla `width` i `height` argumentów Aby `LayoutChildren` zastąpienia. Podczas obracania urządzenia w orientacji poziomej `OnMeasure` zastąpienia i `LayoutChildren` zastąpienie zostanie ponownie wywołany, co spowoduje dwa inne `LayoutData` obiektów w pamięci podręcznej do słownika. Niemniej jednak w przypadku powrotu urządzenia do orientacji pionowej, nie dalsze obliczenia wymagane są ponieważ `layoutDataCache` ma już wymagane dane.

Poniższy kod przedstawia przykład `GetLayoutData` metodę, która oblicza właściwości `LayoutData` strukturalnych na podstawie określonego rozmiaru:

```csharp
LayoutData GetLayoutData(double width, double height)
{
  Size size = new Size(width, height);

  // Check if cached information is available.
  if (layoutDataCache.ContainsKey(size))
  {
    return layoutDataCache[size];
  }

  int visibleChildCount = 0;
  Size maxChildSize = new Size();
  int rows = 0;
  int columns = 0;
  LayoutData layoutData = new LayoutData();

  // Enumerate through all the children.
  foreach (View child in Children)
  {
    // Skip invisible children.
    if (!child.IsVisible)
      continue;

    // Count the visible children.
    visibleChildCount++;

    // Get the child's requested size.
    SizeRequest childSizeRequest = child.Measure(Double.PositiveInfinity, Double.PositiveInfinity);

    // Accumulate the maximum child size.
    maxChildSize.Width = Math.Max(maxChildSize.Width, childSizeRequest.Request.Width);
    maxChildSize.Height = Math.Max(maxChildSize.Height, childSizeRequest.Request.Height);
  }

  if (visibleChildCount != 0)
  {
    // Calculate the number of rows and columns.
    if (Double.IsPositiveInfinity(width))
    {
      columns = visibleChildCount;
      rows = 1;
    }
    else
    {
      columns = (int)((width + ColumnSpacing) / (maxChildSize.Width + ColumnSpacing));
      columns = Math.Max(1, columns);
      rows = (visibleChildCount + columns - 1) / columns;
    }

    // Now maximize the cell size based on the layout size.
    Size cellSize = new Size();

    if (Double.IsPositiveInfinity(width))
      cellSize.Width = maxChildSize.Width;
    else
      cellSize.Width = (width - ColumnSpacing * (columns - 1)) / columns;

    if (Double.IsPositiveInfinity(height))
      cellSize.Height = maxChildSize.Height;
    else
      cellSize.Height = (height - RowSpacing * (rows - 1)) / rows;

    layoutData = new LayoutData(visibleChildCount, cellSize, rows, columns);
  }

  layoutDataCache.Add(size, layoutData);
  return layoutData;
}
```

`GetLayoutData` Metoda wykonuje następujące operacje:

- Określa, czy obliczeniowego `LayoutData` wartość jest już w pamięci podręcznej i zwraca go, jeśli jest dostępna.
- W przeciwnym razie wylicza przez wszystkie elementy podrzędne, wywoływania `Measure` metody dla każdego elementu podrzędnego z nieskończone szerokość i wysokość i określa rozmiar maksymalny podrzędnych.
- Pod warunkiem, że istnieje co najmniej jeden element podrzędny widoczne, jego oblicza liczbę wierszy i kolumn wymagane, a następnie oblicza rozmiar komórki dla elementów podrzędnych na podstawie wymiarów z `WrapLayout`. Należy pamiętać, że rozmiar komórki jest zazwyczaj nieco większa niż rozmiar maksymalny podrzędnych, ale że może również być mniejsze Jeśli `WrapLayout` nie jest dostatecznie dla najszerszego podrzędnej lub wystarczająco wysoka najwyższego podrzędnych.
- Przechowuje nowe `LayoutData` wartość w pamięci podręcznej.

<a name="adding_properties" />

#### <a name="adding-properties-backed-by-bindable-properties"></a>Dodawanie właściwości obsługiwana przez właściwości

`WrapLayout` Klasa definiuje `ColumnSpacing` i `RowSpacing` właściwości, których wartości są używane do oddzielania wierszy i kolumn w układzie i które bazują na właściwości. Można powiązać właściwości są przedstawione w poniższym przykładzie:

```csharp
public static readonly BindableProperty ColumnSpacingProperty = BindableProperty.Create(
  "ColumnSpacing",
  typeof(double),
  typeof(WrapLayout),
  5.0,
  propertyChanged: (bindable, oldvalue, newvalue) =>
  {
    ((WrapLayout)bindable).InvalidateLayout();
  });

public static readonly BindableProperty RowSpacingProperty = BindableProperty.Create(
  "RowSpacing",
  typeof(double),
  typeof(WrapLayout),
  5.0,
  propertyChanged: (bindable, oldvalue, newvalue) =>
  {
    ((WrapLayout)bindable).InvalidateLayout();
  });
```

Wywołuje program obsługi zmiany właściwości każdej można powiązać właściwości `InvalidateLayout` przekazywania zastąpienie metody, aby wyzwolić nowy układ `WrapLayout`. Aby uzyskać więcej informacji, zobacz [przesłaniania metody InvalidateLayout](#invalidatelayout) i [przesłaniania metody OnChildMeasureInvalidated](#onchildmeasureinvalidated).

<a name="onmeasure" />

#### <a name="overriding-the-onmeasure-method"></a>Zastąpienie metody OnMeasure

`OnMeasure` Zastąpienie przedstawiono w poniższym przykładzie kodu:

```csharp
protected override SizeRequest OnMeasure(double widthConstraint, double heightConstraint)
{
  LayoutData layoutData = GetLayoutData(widthConstraint, heightConstraint);
  if (layoutData.VisibleChildCount == 0)
  {
    return new SizeRequest();
  }

  Size totalSize = new Size(layoutData.CellSize.Width * layoutData.Columns + ColumnSpacing * (layoutData.Columns - 1),
                layoutData.CellSize.Height * layoutData.Rows + RowSpacing * (layoutData.Rows - 1));
  return new SizeRequest(totalSize);
}
```

Zastąpienie wywołuje `GetLayoutData` metody oraz elementów składowych `SizeRequest` obiektu na podstawie zwróconych danych podczas biorąc pod uwagę także `RowSpacing` i `ColumnSpacing` wartości właściwości. Aby uzyskać więcej informacji na temat `GetLayoutData` metody, zobacz [Obliczanie i buforowanie danych](#caching).

> [!IMPORTANT]
> [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/) i [ `OnMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/) metody nigdy nie może zażądać nieskończone wymiaru zwracając [ `SizeRequest` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SizeRequest/) wartości z ustawioną właściwością `Double.PositiveInfinity`. Jednak co najmniej jeden z argumentów ograniczenia `OnMeasure` może być `Double.PositiveInfinity`.

<a name="layoutchildren" />

#### <a name="overriding-the-layoutchildren-method"></a>Zastąpienie metody LayoutChildren

`LayoutChildren` Zastąpienie przedstawiono w poniższym przykładzie kodu:

```csharp
protected override void LayoutChildren(double x, double y, double width, double height)
{
  LayoutData layoutData = GetLayoutData(width, height);

  if (layoutData.VisibleChildCount == 0)
  {
    return;
  }

  double xChild = x;
  double yChild = y;
  int row = 0;
  int column = 0;

  foreach (View child in Children)
  {
    if (!child.IsVisible)
    {
      continue;
    }

    LayoutChildIntoBoundingRegion(child, new Rectangle(new Point(xChild, yChild), layoutData.CellSize));
    if (++column == layoutData.Columns)
    {
      column = 0;
      row++;
      xChild = x;
      yChild += RowSpacing + layoutData.CellSize.Height;
    }
    else
    {
      xChild += ColumnSpacing + layoutData.CellSize.Width;
    }
  }
}
```

Zastąpienie rozpoczyna się od wywołania `GetLayoutData` metody, a następnie wylicza wszystkie elementy podrzędne do określenia rozmiaru i umieść je w komórce tego elementu podrzędnego. Jest to osiągane przez wywoływanie [ `LayoutChildIntoBoundingRegion` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildIntoBoundingRegion/p/Xamarin.Forms.VisualElement/Xamarin.Forms.Rectangle/) metodę, która jest używana do pozycjonowania elementu podrzędnego w obrębie prostokąta na podstawie jego [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) i [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/)wartości właściwości. Jest to odpowiednik wywołania do tego elementu podrzędnego [ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/) metody.

> [!NOTE]
> Należy pamiętać, że prostokąt przekazany do `LayoutChildIntoBoundingRegion` metoda zawiera cały obszar, w którym mogą znajdować się obiekt podrzędny.

Aby uzyskać więcej informacji na temat `GetLayoutData` metody, zobacz [Obliczanie i buforowanie danych](#caching).

<a name="invalidatelayout" />

#### <a name="overriding-the-invalidatelayout-method"></a>Zastąpienie metody InvalidateLayout

[ `InvalidateLayout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.InvalidateLayout()/) Zastąpienia jest wywoływane, gdy elementy podrzędne są dodane lub usunięte z układu, lub gdy jeden z `WrapLayout` zmiany właściwości wartość, jak pokazano w poniższym przykładzie:

```csharp
protected override void InvalidateLayout()
{
  base.InvalidateLayout();
  layoutInfoCache.Clear();
}
```

Zastąpienie unieważnia układ i spowoduje odrzucenie wszystkich informacji o układzie pamięci podręcznej.

> [!NOTE]
> Aby zatrzymać [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/) klasy wywoływania [ `InvalidateLayout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.InvalidateLayout()/) przesłonić metodę zawsze, gdy element podrzędny jest dodane lub usunięte z układu, [ `ShouldInvalidateOnChildAdded` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.ShouldInvalidateOnChildAdded/p/Xamarin.Forms.View/) i [ `ShouldInvalidateOnChildRemoved` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.ShouldInvalidateOnChildRemoved/p/Xamarin.Forms.View/) metod i zwrócenie `false`. Klasa układu następnie można wdrożyć niestandardowy proces, gdy elementy podrzędne są dodawane lub usuwane.

<a name="onchildmeasureinvalidated" />

#### <a name="overriding-the-onchildmeasureinvalidated-method"></a>Zastąpienie metody OnChildMeasureInvalidated

[ `OnChildMeasureInvalidated` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.OnChildMeasureInvalidated()/) Zastąpienia jest wywoływane, gdy jeden z elementów podrzędnych układu zmienia rozmiar i przedstawiono w poniższym przykładzie:

```csharp
protected override void OnChildMeasureInvalidated()
{
  base.OnChildMeasureInvalidated();
  layoutInfoCache.Clear();
}
```

Zastąpienie unieważnia układu podrzędnych i odrzuca wszystkie informacje o układzie pamięci podręcznej.

<a name="consuming" />

### <a name="consuming-the-wraplayout"></a>Korzystanie z WrapLayout

`WrapLayout` Klasy może być zużyte przez umieszczenia go na [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) pochodzi z typu, jak pokazano w poniższym przykładzie kodu XAML:

```xaml
<ContentPage ... xmlns:local="clr-namespace:ImageWrapLayout">
    <ScrollView Margin="0,20,0,20">
        <local:WrapLayout x:Name="wrapLayout" />
    </ScrollView>
</ContentPage>
```

Poniżej przedstawiono równoważne kodu C#:

```csharp
public class ImageWrapLayoutPageCS : ContentPage
{
  WrapLayout wrapLayout;

  public ImageWrapLayoutPageCS()
  {
    wrapLayout = new WrapLayout();

    Content = new ScrollView
    {
      Margin = new Thickness(0, 20, 0, 20),
      Content = wrapLayout
    };
  }
  ...
}
```

Następnie można dodać elementów podrzędnych do `WrapLayout` zgodnie z wymaganiami. Poniższy kod przedstawia przykład [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) dodawane do elementów `WrapLayout`:

```csharp
protected override async void OnAppearing()
{
  base.OnAppearing();

  var images = await GetImageListAsync();
  foreach (var photo in images.Photos)
  {
    var image = new Image
    {
      Source = ImageSource.FromUri(new Uri(photo + string.Format("?width={0}&height={0}&mode=max", Device.RuntimePlatform == Device.UWP ? 120 : 240)))
    };
    wrapLayout.Children.Add(image);
  }
}

async Task<ImageList> GetImageListAsync()
{
  var requestUri = "https://docs.xamarin.com/demo/stock.json";
  using (var client = new HttpClient())
  {
    var result = await client.GetStringAsync(requestUri);
    return JsonConvert.DeserializeObject<ImageList>(result);
  }
}
```

Gdy strony `WrapLayout` pojawi się przykładowa aplikacja uzyskuje dostęp do zdalnego plik JSON zawierający listę zdjęć asynchronicznie, tworzy [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) elementu dla każdego zdjęć i dodaje go do `WrapLayout`. Powoduje to wygląd pokazano na poniższych zrzutach ekranu:

![](custom-images/portait-screenshots.png "Zrzuty ekranu pionowa aplikacji przykładowych")

Pokaż poniższe zrzuty ekranu `WrapLayout` po jest zostały obrócone w orientacji poziomej:

![](custom-images/landscape-ios.png "Przykładowe zrzut ekranu pozioma aplikacji systemu iOS")
![](custom-images/landscape-android.png "zrzut ekranu przykładu Android aplikacji pozioma") 
 ![ ] (custom-images/landscape-uwp.png " Zrzut ekranu pozioma aplikacji platformy uniwersalnej systemu Windows przykładu")

Liczba kolumn w każdym wierszu zależy od rozmiaru fotografii, szerokość ekranu i liczba pikseli na jednostkę niezależnych od urządzenia. [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) Elementy asynchronicznie załadować fotografii i w związku z tym `WrapLayout` klasy otrzymają częste wywołania do jego [ `LayoutChildren` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildren/p/System.Double/System.Double/System.Double/System.Double/) metody, ponieważ do każdego `Image` element odbiera nowy rozmiar oparte na załadować zdjęcie.

## <a name="summary"></a>Podsumowanie

W tym artykule wyjaśniono sposób pisania klasy niestandardowego układu i sprawdzono wrażliwych orientacji `WrapLayout` klasy, która Rozmieszcza elementy podrzędne w poziomie na stronie, a następnie opakowuje wyświetlanie kolejnych elementów podrzędnych do dodatkowych wierszy.


## <a name="related-links"></a>Linki pokrewne

- [WrapLayout (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/CustomLayout/WrapLayout/)
- [Układy niestandardowe](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter26.md)
- [Tworzenie niestandardowych układów w platformy Xamarin.Forms (klip wideo)](https://evolve.xamarin.com/session/56e20f83bad314273ca4d81c)
- [Układ<T>](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/)
- [Układ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/)
- [VisualElement](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/)
