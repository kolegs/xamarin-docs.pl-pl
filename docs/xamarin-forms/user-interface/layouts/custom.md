---
title: Tworzenie niestandardowego układu
description: W tym artykule wyjaśniono, jak napisać klasę układu niestandardowego, a ilustruje klasę WrapLayout zależne od orientacji rozmieszcza jego elementy podrzędne w poziomie na stronie, a następnie opakowuje wyświetlanie kolejnych elementów podrzędnych do dodatkowych wierszy.
ms.prod: xamarin
ms.assetid: B0CFDB59-14E5-49E9-965A-3DCCEDAC2E31
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/29/2017
ms.openlocfilehash: 0c16fd3930926a05ed7796391962d0fc8996dc96
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995378"
---
# <a name="creating-a-custom-layout"></a>Tworzenie niestandardowego układu

_Zestaw narzędzi Xamarin.Forms definiuje cztery układ klasy — StackLayout, AbsoluteLayout, RelativeLayout i siatki, i każdy rozmieszcza jego elementy podrzędne w inny sposób. Jednak czasami jest konieczne do organizowania zawartości strony przy użyciu układu nie został dostarczony przez zestaw narzędzi Xamarin.Forms. W tym artykule wyjaśniono, jak napisać klasę układu niestandardowego, a ilustruje klasę WrapLayout zależne od orientacji rozmieszcza jego elementy podrzędne w poziomie na stronie, a następnie opakowuje wyświetlanie kolejnych elementów podrzędnych do dodatkowych wierszy._

## <a name="overview"></a>Omówienie

W interfejsie Xamarin.Forms, dziedziczyć wszystkie klasy układ [ `Layout<T>` ](xref:Xamarin.Forms.Layout`1) klasy i ograniczenia typu ogólnego, aby [ `View` ](xref:Xamarin.Forms.View) i jego typów pochodnych. Z kolei `Layout<T>` klasa pochodzi od [ `Layout` ](xref:Xamarin.Forms.Layout) klasy, która udostępnia mechanizm dla położenia i rozmiaru podrzędnych elementów.

Każdy element wizualny jest odpowiedzialny za sprawdzenie własnego preferowany rozmiar, który jest znany jako *żądane* rozmiar. [`Page`](xref:Xamarin.Forms.Page), [ `Layout` ](xref:Xamarin.Forms.Layout), i [ `Layout<View>` ](xref:Xamarin.Forms.Layout`1) typy pochodne są zobowiązani do sprawdzenia lokalizacji i rozmiaru ich podrzędne i elementy podrzędne względem siebie. W związku z tym, układ obejmuje relacji nadrzędny podrzędny, gdzie nadrzędnego okaże się, jakie powinny być rozmiar jego elementy podrzędne, ale będzie próbował dopasować żądany rozmiar elementu podrzędnego.

Dogłębnej wiedzy w zakresie platformy Xamarin.Forms układ i unieważniania cyklu jest wymagana do utworzenia układu niestandardowego. Teraz będzie to dokładniej omówione tych cykli.

## <a name="layout"></a>Układ

Układ, który rozpoczyna się w górnej części drzewa wizualnego ze stroną, a następnie przechodzi przez wszystkie gałęzie drzewa wizualnego w celu objęcia każdy element wizualny na stronie. Elementy, które są elementy nadrzędne do innych elementów są odpowiedzialne za rozmiary i rozmieszczenia ich elementy podrzędne względem siebie.

[ `VisualElement` ](xref:Xamarin.Forms.VisualElement) Klasa definiuje [ `Measure` ](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags)) metodę, która mierzy element dla operacji układu i [ `Layout` ](xref:Xamarin.Forms.VisualElement.Layout(Xamarin.Forms.Rectangle)) metody, która określa prostokątny obszar elementu będą renderowane w ciągu. Po uruchomieniu aplikacji, pierwsza strona jest wyświetlana, *cyklu układ* składający się pierwszego dnia `Measure` wywołań, a następnie `Layout` wywołuje, rozpoczyna się na [ `Page` ](xref:Xamarin.Forms.Page) obiektu:

1. Podczas cyklu układ co element nadrzędny jest odpowiedzialny za wywołanie `Measure` metody na jego elementy podrzędne.
1. Po mierzonych elementy podrzędne elementu nadrzędnego, co jest odpowiedzialny za wywołania `Layout` metody na jego elementy podrzędne.

Ten cykl gwarantuje, że każdy element wizualny na stronie otrzyma wywołania `Measure` i `Layout` metody. Na poniższym diagramie przedstawiono proces:

![](custom-images/layout-cycle.png "Cykl układ zestawu narzędzi Xamarin.Forms")

> [!NOTE]
> Należy pamiętać, że cykle układu może również wystąpić na podzestaw drzewa wizualnego, jeśli zmieni się coś, co wpływa na układ. Obejmuje to elementy dodawany lub usuwany z kolekcji, takim jak [ `StackLayout` ](xref:Xamarin.Forms.StackLayout), zmiana [ `IsVisible` ](xref:Xamarin.Forms.VisualElement.IsVisible) właściwość elementu lub zmianę w rozmiarze elementu.

Każda klasa zestawu narzędzi Xamarin.Forms, który ma `Content` lub `Children` właściwość ma możliwym do zastąpienia [ `LayoutChildren` ](xref:Xamarin.Forms.Layout.LayoutChildren(System.Double,System.Double,System.Double,System.Double)) metody. Klasy niestandardowe układy, które wynikają z [ `Layout<View>` ](xref:Xamarin.Forms.Layout`1) należy przesłonić tę metodę i upewnij się, że [ `Measure` ](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags)) i [ `Layout` ](xref:Xamarin.Forms.VisualElement.Layout(Xamarin.Forms.Rectangle)) są metody wywoływana na wszystkie elementy podrzędne, aby zapewnić żądany układ niestandardowy.

Ponadto każdej klasy, która jest pochodną [ `Layout` ](xref:Xamarin.Forms.Layout) lub [ `Layout<View>` ](xref:Xamarin.Forms.Layout`1) przesłonięcie [ `OnMeasure` ](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double)) metoda, która jest w przypadku, gdy klasa układu Określa rozmiar który musi być przez wykonywanie wywołań do [ `Measure` ](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags)) metody jego elementów podrzędnych.

> [!NOTE]
> Elementy określić ich rozmiar na podstawie *ograniczenia*, które wskazują, ile miejsca jest dostępna dla elementu z elementu nadrzędnego. Ograniczenia są przekazywane do [ `Measure` ](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags)) i [ `OnMeasure` ](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double)) metody mogą należeć do zakresu od 0 do `Double.PositiveInfinity`. Element jest *ograniczone*, lub *pełni ograniczone*, po odebraniu wywołanie w celu jego [ `Measure` ](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags)) metody z argumentami nie nieskończone — element jest ograniczony do określonego rozmiaru. Element jest *nieograniczonego*, lub *częściowo ograniczony*, po odebraniu wywołanie w celu jego `Measure` metody z co najmniej jednego argumentu równa `Double.PositiveInfinity` — mogą być nieskończone ograniczenia traktować jako wskazujący automatyczne określanie rozmiaru.

## <a name="invalidation"></a>Unieważnieniu

Unieważnieniu polega na za pomocą którego zmiana elementu na stronie wyzwala nowy cykl układu. Elementy są traktowane jako nieprawidłowy, gdy nie będzie ona miała właściwego rozmiaru lub położenia. Na przykład jeśli [ `FontSize` ](xref:Xamarin.Forms.Button.FontSize) właściwość [ `Button` ](xref:Xamarin.Forms.Button) zmian `Button` jest nazywany jest nieprawidłowy, ponieważ nie będzie miało właściwy rozmiar. Zmiana rozmiaru `Button` następnie może mieć wpływ zmian w układzie przez pozostałą część strony.

Elementy unieważnienie samodzielnie za pomocą wywołania [ `InvalidateMeasure` ](xref:Xamarin.Forms.VisualElement.InvalidateMeasure) metody, zazwyczaj gdy właściwość elementu zmieni nowy rozmiar elementu, który może spowodować. Ta metoda uruchamia [ `MeasureInvalidated` ](xref:Xamarin.Forms.VisualElement.MeasureInvalidated) zdarzenie, które obsługuje jego elementu nadrzędnego, wyzwolenie nowego cyklu układu.

[ `Layout` ](xref:Xamarin.Forms.Layout) Klasy Ustawia program obsługi [ `MeasureInvalidated` ](xref:Xamarin.Forms.VisualElement.MeasureInvalidated) zdarzenia dla każdego podrzędnego dodawane do jej `Content` właściwości lub `Children` kolekcji oraz odłącza obsługi podczas podrzędne zostaną usunięte. W związku z tym każdego elementu w drzewie wizualnym, który ma elementy podrzędne są alerty zawsze wtedy, gdy jeden z jego elementów podrzędnych zmienia rozmiar. Na poniższym diagramie przedstawiono, jak zmianę w rozmiarze elementu w drzewie wizualnym może spowodować zmiany, które ripple w górę drzewa:

![](custom-images/invalidation.png "Unieważnieniu w drzewie wizualnym")

Jednak `Layout` klasy próbuje ograniczyć wpływ zmianę w rozmiarze elementu podrzędnego na układ strony. Układ jest ograniczony rozmiar, następnie zmiany rozmiaru podrzędnych nie wpływa na nic wyższa niż układ nadrzędnego w drzewie wizualnym. Jednak zazwyczaj zmianę w rozmiarze układu ma wpływ na sposób układ rozmieszcza jego elementów podrzędnych. W związku z tym, wszelkie zmiany rozmiaru układu i rozpocznie się układ cyklu w układzie układ będzie otrzymywać połączeń do jego [ `OnMeasure` ](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double)) i [ `LayoutChildren` ](xref:Xamarin.Forms.Layout.LayoutChildren(System.Double,System.Double,System.Double,System.Double)) metody.

[ `Layout` ](xref:Xamarin.Forms.Layout) Klasa definiuje również [ `InvalidateLayout` ](xref:Xamarin.Forms.Layout.InvalidateLayout) metody, która ma podobny cel do [ `InvalidateMeasure` ](xref:Xamarin.Forms.VisualElement.InvalidateMeasure) metody. `InvalidateLayout` Wywołać metody, gdy wprowadzona zmiana wpływa na sposób układ umieszcza i jego elementów podrzędnych o rozmiarach. Na przykład `Layout` wywołuje klasę `InvalidateLayout` zawsze, gdy jest elementem podrzędnym jest dodane lub usunięte z układu.

[ `InvalidateLayout` ](xref:Xamarin.Forms.Layout.InvalidateLayout) Może zostać zastąpiona w celu implementowania pamięci podręcznej, aby zminimalizować powtarzających się wywołań [ `Measure` ](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags)) metody układu elementów podrzędnych. Zastępowanie `InvalidateLayout` metoda zapewnia powiadomienie z informacją o po elementy podrzędne są dodawane do lub usunięte z układu. Podobnie [ `OnChildMeasureInvalidated` ](xref:Xamarin.Forms.Layout.OnChildMeasureInvalidated) metodę można przesłonić zapewnienie powiadomienie, gdy jeden z elementów podrzędnych układ zmienia rozmiar. Dla obu zastąpienia metody układu niestandardowego powinien odpowiadać przez wyczyszczenie pamięci podręcznej. Aby uzyskać więcej informacji, zobacz [Obliczanie i buforowanie danych](#caching).

## <a name="creating-a-custom-layout"></a>Tworzenie niestandardowego układu

Proces tworzenia układu niestandardowego, jest następująca:

1. Utwórz klasę pochodną od klasy `Layout<View>`. Aby uzyskać więcej informacji, zobacz [tworzenia WrapLayout](#creating).
1. [*opcjonalne*] Dodaj właściwości, wspierane przez właściwości możliwej do wiązania parametrów, które należy ustawić na klasie układu. Aby uzyskać więcej informacji, zobacz [dodanie właściwości kopii, które można powiązać właściwości](#adding_properties).
1. Zastąp [ `OnMeasure` ](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double)) metody do wywołania [ `Measure` ](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags)) metody w układzie wszystkie elementy podrzędne i wróć żądany rozmiar dla układu. Aby uzyskać więcej informacji, zobacz [zastąpienie metody OnMeasure](#onmeasure).
1. Zastąp [ `LayoutChildren` ](xref:Xamarin.Forms.Layout.LayoutChildren(System.Double,System.Double,System.Double,System.Double)) metody do wywołania [ `Layout` ](xref:Xamarin.Forms.VisualElement.Layout(Xamarin.Forms.Rectangle)) metody w układzie wszystkie elementy podrzędne. Nie można wywołać [ `Layout` ](xref:Xamarin.Forms.VisualElement.Layout(Xamarin.Forms.Rectangle)) metoda dla każdego elementu podrzędnego w układzie spowoduje elementu podrzędnego, nigdy nie odbiera prawidłowy rozmiar lub położenie i dlatego podrzędne nie będą widoczne na stronie. Aby uzyskać więcej informacji, zobacz [zastąpienie metody LayoutChildren](#layoutchildren).

  > [!NOTE]
>  Podczas wyliczania elementów podrzędnych w [ `OnMeasure` ](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double)) i [ `LayoutChildren` ](xref:Xamarin.Forms.Layout.LayoutChildren(System.Double,System.Double,System.Double,System.Double)) zastąpień, Pomiń wszystkie podrzędne którego [ `IsVisible` ](xref:Xamarin.Forms.VisualElement.IsVisible) właściwość jest ustawiona na `false`. Pozwoli to zagwarantować, układ niestandardowy nie zostawić miejsce dla dzieci niewidoczne.

1. [*opcjonalne*] Zastąp [ `InvalidateLayout` ](xref:Xamarin.Forms.Layout.InvalidateLayout) metodę, aby otrzymywać powiadomienia, gdy elementy podrzędne są dodawane do lub usunięte z układu. Aby uzyskać więcej informacji, zobacz [zastąpienie metody InvalidateLayout](#invalidatelayout).
1. [*opcjonalne*] Zastąp [ `OnChildMeasureInvalidated` ](xref:Xamarin.Forms.Layout.OnChildMeasureInvalidated) metodę, aby otrzymywać powiadomienia, gdy jeden z elementów podrzędnych układ zmienia rozmiar. Aby uzyskać więcej informacji, zobacz [zastąpienie metody OnChildMeasureInvalidated](#onchildmeasureinvalidated).

> [!NOTE]
> Należy pamiętać, że [ `OnMeasure` ](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double)) zastąpienie nie wywołany, jeśli rozmiar układ jest regulowane przez jego element nadrzędny, a nie jego elementów podrzędnych. Jednak zostanie wywołany przesłonięcie, czy co najmniej jeden z ograniczeniami są nieskończone, czy klasa układ ma niedomyślny [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) lub [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) wartości właściwości. Z tego powodu [ `LayoutChildren` ](xref:Xamarin.Forms.Layout.LayoutChildren(System.Double,System.Double,System.Double,System.Double)) override nie można polegać na rozmiary podrzędnych uzyskanego podczas [ `OnMeasure` ](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double)) wywołania metody. Zamiast tego `LayoutChildren` musi zostać wywołany [ `Measure` ](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags)) układu elementów podrzędnych, przed wywołaniem metody [ `Layout` ](xref:Xamarin.Forms.VisualElement.Layout(Xamarin.Forms.Rectangle)) metody. Alternatywnie rozmiar elementu podrzędnego uzyskane w `OnMeasure` zastąpienie mogą być buforowane, aby uniknąć później `Measure` wywołań w `LayoutChildren` zastąpienie, ale klasa układ muszą dowiedzieć się, gdy rozmiary muszą pochodzić ponownie. Aby uzyskać więcej informacji, zobacz [Obliczanie i buforowanie danych układu](#caching).

Klasa układ może następnie być używany przez dodanie jej do [ `Page` ](xref:Xamarin.Forms.Page)i dodając elementy podrzędne w układzie. Aby uzyskać więcej informacji, zobacz [wykorzystywanie WrapLayout](#consuming).

<a name="creating" />

### <a name="creating-a-wraplayout"></a>Tworzenie WrapLayout

Przykładowa aplikacja demonstruje wrażliwych orientacji `WrapLayout` klasę, która organizuje jego elementy podrzędne w poziomie na stronie, a następnie opakowuje wyświetlanie kolejnych elementów podrzędnych do dodatkowych wierszy.

`WrapLayout` Klasy przydziela tyle samo miejsca dla każdego elementu podrzędnego, znane jako *komórka rozmiaru*, na podstawie maksymalnego rozmiaru elementy podrzędne. Mniejszy niż rozmiar komórki, które można umieścić w komórce elementy podrzędne na podstawie ich [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) i [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) wartości właściwości.

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

`LayoutData` Struktury przechowuje dane o kolekcji elementów podrzędnych w liczbę właściwości:

- `VisibleChildCount` — Liczba elementów podrzędnych, które są widoczne w układzie.
- `CellSize` – Maksymalny rozmiar wszystkich obiektów podrzędnych, dostosowane do rozmiaru układu.
- `Rows` — Liczba wierszy.
- `Columns` — Liczba kolumn.

`layoutDataCache` Pole jest używane do przechowywania wielu `LayoutData` wartości. Podczas uruchamiania aplikacji, dwa `LayoutData` obiekty będą buforowane do `layoutDataCache` słownik na potrzeby bieżącej orientacji — jedno dla ograniczenia argumenty `OnMeasure` przesłonięć i jeden dla `width` i `height` argumentów Aby `LayoutChildren` zastąpienia. W przypadku rotacji urządzenia w orientacji poziomej `OnMeasure` zastąpienia i `LayoutChildren` zastąpienie zostanie ponownie wywołany, co spowoduje dwa inne `LayoutData` obiektów z pamięci podręcznej do słownika. Jednak przy powrocie urządzenia do orientacji pionowej, nie dalsze obliczenia są wymagane, ponieważ `layoutDataCache` już zawierającą wymagane dane.

Poniższy kod przedstawia przykład `GetLayoutData` metody, która oblicza właściwości `LayoutData` ze strukturą w oparciu o określonym rozmiarze:

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

- Określa, czy obliczeniowego `LayoutData` wartości jest już w pamięci podręcznej i zwraca go, jeśli jest ona dostępna.
- W przeciwnym razie wylicza przy użyciu wszystkich elementów podrzędnych, wywołując `Measure` metoda dla każdego elementu podrzędnego z nieskończoną szerokość i wysokość i określa rozmiar maksymalny podrzędnych.
- Pod warunkiem, że istnieje co najmniej jeden element podrzędny widoczne, ją oblicza liczbę wierszy i kolumn, wymagane, a następnie oblicza rozmiar komórki dla dzieci, w oparciu o wymiary `WrapLayout`. Uwaga rozmiar komórki jest zazwyczaj nieco większa niż rozmiar maksymalny podrzędnych, ale czy mogą być również mniejszych Jeśli `WrapLayout` nie jest dostatecznie szerokie, dla najszerszego podrzędnej lub wystarczająco wysoka najwyższego podrzędnego.
- Przechowuje nowe `LayoutData` wartość w pamięci podręcznej.

<a name="adding_properties" />

#### <a name="adding-properties-backed-by-bindable-properties"></a>Dodawanie właściwości kopii, które można powiązać właściwości

`WrapLayout` Klasa definiuje `ColumnSpacing` i `RowSpacing` właściwości, których wartości są używane do oddzielania wierszy i kolumn w układzie i które są wspierane przez właściwości możliwej do wiązania. Właściwości możliwe do wiązania przedstawiono w poniższym przykładzie kodu:

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

Wywołuje program obsługi zmiany właściwości każdej możliwej do wiązania właściwości `InvalidateLayout` przekazać zastąpienie metody, aby wyzwolić nowego układu `WrapLayout`. Aby uzyskać więcej informacji, zobacz [zastąpienie metody InvalidateLayout](#invalidatelayout) i [zastąpienie metody OnChildMeasureInvalidated](#onchildmeasureinvalidated).

<a name="onmeasure" />

#### <a name="overriding-the-onmeasure-method"></a>Zastępowanie metody OnMeasure

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

Zastąpienie wywołuje `GetLayoutData` metody i konstrukcje `SizeRequest` obiektu na podstawie zwróconych danych, uwzględniając również `RowSpacing` i `ColumnSpacing` wartości właściwości. Aby uzyskać więcej informacji na temat `GetLayoutData` metody, zobacz [Obliczanie i buforowanie danych](#caching).

> [!IMPORTANT]
> [ `Measure` ](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags)) i [ `OnMeasure` ](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double)) metody nigdy nie może zażądać nieskończonej wymiaru, zwracając [ `SizeRequest` ](xref:Xamarin.Forms.SizeRequest) ustawioną na wartość `Double.PositiveInfinity`. Jednak co najmniej jeden z argumentów ograniczenie `OnMeasure` może być `Double.PositiveInfinity`.

<a name="layoutchildren" />

#### <a name="overriding-the-layoutchildren-method"></a>Zastępowanie metody LayoutChildren

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

Zastąpienie zaczyna się od wywołania `GetLayoutData` metody, a następnie wylicza wszystkie elementy podrzędne, rozmiar i umieść je w obrębie każdego elementu podrzędnego komórki. Jest to osiągane przez wywołanie [ `LayoutChildIntoBoundingRegion` ](xref:Xamarin.Forms.Layout.LayoutChildIntoBoundingRegion(Xamarin.Forms.VisualElement,Xamarin.Forms.Rectangle)) metody, która jest używana do pozycji elementu podrzędnego w obrębie prostokąta na podstawie jego [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) i [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions)wartości właściwości. Jest to odpowiednik wywołania do elementu podrzędnego [ `Layout` ](xref:Xamarin.Forms.VisualElement.Layout(Xamarin.Forms.Rectangle)) metody.

> [!NOTE]
> Należy zauważyć, że prostokąt przekazany do `LayoutChildIntoBoundingRegion` metoda obejmuje cały obszar, w którym mogą znajdować się elementem podrzędnym.

Aby uzyskać więcej informacji na temat `GetLayoutData` metody, zobacz [Obliczanie i buforowanie danych](#caching).

<a name="invalidatelayout" />

#### <a name="overriding-the-invalidatelayout-method"></a>Zastępowanie metody InvalidateLayout

[ `InvalidateLayout` ](xref:Xamarin.Forms.Layout.InvalidateLayout) Zastąpienie jest wywoływane, gdy elementy podrzędne są dodawane do lub usunięte z układu lub gdy dla jednego z `WrapLayout` wartość zmiany właściwości, jak pokazano w poniższym przykładzie kodu:

```csharp
protected override void InvalidateLayout()
{
  base.InvalidateLayout();
  layoutInfoCache.Clear();
}
```

Zastąpienie unieważnia układ i odrzuca wszystkie informacje w pamięci podręcznej układu.

> [!NOTE]
> Aby zatrzymać [ `Layout` ](xref:Xamarin.Forms.Layout) wywołanie klasy [ `InvalidateLayout` ](xref:Xamarin.Forms.Layout.InvalidateLayout) przesłonić metodę zawsze wtedy, gdy element podrzędny jest dodane lub usunięte z układu [ `ShouldInvalidateOnChildAdded` ](xref:Xamarin.Forms.Layout.ShouldInvalidateOnChildAdded(Xamarin.Forms.View)) i [ `ShouldInvalidateOnChildRemoved` ](xref:Xamarin.Forms.Layout.ShouldInvalidateOnChildRemoved(Xamarin.Forms.View)) metody i zwrócenia `false`. Klasa układ następnie można zaimplementować niestandardowy proces, gdy elementy podrzędne są dodawane lub usuwane.

<a name="onchildmeasureinvalidated" />

#### <a name="overriding-the-onchildmeasureinvalidated-method"></a>Zastępowanie metody OnChildMeasureInvalidated

[ `OnChildMeasureInvalidated` ](xref:Xamarin.Forms.Layout.OnChildMeasureInvalidated) Zastąpienie jest wywoływane, gdy jeden z elementów podrzędnych układ zmienia rozmiar i przedstawiono w poniższym przykładzie kodu:

```csharp
protected override void OnChildMeasureInvalidated()
{
  base.OnChildMeasureInvalidated();
  layoutInfoCache.Clear();
}
```

Zastąpienie unieważnia układ podrzędnych i odrzuca wszystkie informacje o układzie pamięci podręcznej.

<a name="consuming" />

### <a name="consuming-the-wraplayout"></a>Korzystanie z WrapLayout

`WrapLayout` Klasy mogą być używane, umieszczając je na [ `Page` ](xref:Xamarin.Forms.Page) pochodne typu, jak pokazano w poniższym przykładzie kodu XAML:

```xaml
<ContentPage ... xmlns:local="clr-namespace:ImageWrapLayout">
    <ScrollView Margin="0,20,0,20">
        <local:WrapLayout x:Name="wrapLayout" />
    </ScrollView>
</ContentPage>
```

Równoważny kod C# jest pokazany poniżej:

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

Następnie można dodać elementów podrzędnych do `WrapLayout` stosownie do potrzeb. Poniższy kod przedstawia przykład [ `Image` ](xref:Xamarin.Forms.Image) elementy są dodane do `WrapLayout`:

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

Gdy strony `WrapLayout` jest wyświetlana Przykładowa aplikacja uzyskuje dostęp do zdalnego pliku JSON zawierającego listę zdjęcia asynchronicznie, tworzy [ `Image` ](xref:Xamarin.Forms.Image) elementu dla każdego zdjęć i dodaje go do `WrapLayout`. Skutkuje to wygląd pokazano na poniższych zrzutach ekranu:

![](custom-images/portait-screenshots.png "Zrzuty ekranu pionowa aplikacji przykładowej")

Poniższe zrzuty ekranu Pokaż `WrapLayout` po jest zostały obrócone w orientacji poziomej:

![](custom-images/landscape-ios.png "Przykładowy zrzut ekranu pozioma aplikacji systemu iOS")
![](custom-images/landscape-android.png "zrzut ekranu przykładu dla systemu Android aplikacji pozioma")
![](custom-images/landscape-uwp.png " Przykładowy zrzut ekranu pozioma aplikacji dla platformy uniwersalnej systemu Windows")

Liczba kolumn w każdym wierszu zależy od tego, rozmiar zdjęcia, szerokość ekranu i liczbę pikseli na jednostka miary niezależna od urządzenia. [ `Image` ](xref:Xamarin.Forms.Image) Elementy asynchronicznie Załaduj zdjęcia i w związku z tym `WrapLayout` klasy będą otrzymywać częste wywołania do jego [ `LayoutChildren` ](xref:Xamarin.Forms.Layout.LayoutChildren(System.Double,System.Double,System.Double,System.Double)) metody, ponieważ do każdego `Image` element otrzymuje nowy rozmiar zdjęcia załadowane w oparciu.

## <a name="summary"></a>Podsumowanie

W tym artykule wyjaśniono, jak napisać klasę układu niestandardowego i pokazano wrażliwych orientacji `WrapLayout` klasę, która organizuje jego elementy podrzędne w poziomie na stronie, a następnie opakowuje wyświetlanie kolejnych elementów podrzędnych do dodatkowych wierszy.


## <a name="related-links"></a>Linki pokrewne

- [WrapLayout (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/CustomLayout/WrapLayout/)
- [Układy niestandardowe](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter26.md)
- [Tworzenie niestandardowych układów w interfejsie Xamarin.Forms (wideo)](https://evolve.xamarin.com/session/56e20f83bad314273ca4d81c)
- [Układ<T>](xref:Xamarin.Forms.Layout`1)
- [Układ](xref:Xamarin.Forms.Layout)
- [VisualElement](xref:Xamarin.Forms.VisualElement)
