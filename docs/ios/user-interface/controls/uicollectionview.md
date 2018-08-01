---
title: Kolekcja widoków platformy Xamarin.iOS
description: Kolekcja widoków zezwala na zawartość mają być wyświetlane przy użyciu dowolnego układów. Umożliwiają one łatwe tworzenie układów siatki fabrycznej, również obsługuje układy niestandardowe.
ms.prod: xamarin
ms.assetid: F4B85F25-0CB5-4FEA-A3B5-D22FCDC81AE4
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: b9ba2f885364084d6bee67c460b4831c00c7ae55
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790630"
---
# <a name="collection-views-in-xamarinios"></a>Kolekcja widoków platformy Xamarin.iOS

_Kolekcja widoków zezwala na zawartość mają być wyświetlane przy użyciu dowolnego układów. Umożliwiają one łatwe tworzenie układów siatki fabrycznej, również obsługuje układy niestandardowe._

Kolekcja widoków, dostępne w `UICollectionView` klasy, są nowe pojęcie w systemie iOS 6, który wprowadzenie przedstawiające wielu elementów za pomocą układów. Wzorce udostępniania danych na `UICollectionView` do tworzenia elementów i interakcji z tych wykonaj elementów tego samego delegowania i danych źródłowych wzorce często używane w opracowywania aplikacji systemu iOS.

Jednak działać widoki kolekcji z podsystemem układu, która jest niezależna od `UICollectionView` samej siebie. W związku z tym po prostu dostarczanie inny układ można łatwo zmienić prezentacji widok kolekcji.

iOS zapewnia układ klasy o nazwie `UICollectionViewFlowLayout` , która umożliwia układów opartych na wiersz, takich jak siatki są tworzone z żadne dodatkowe czynności. Ponadto układy niestandardowe również można tworzyć zezwalających na prezentacji, który można wyobrazić sobie.

## <a name="uicollectionview-basics"></a>Podstawy UICollectionView

`UICollectionView` Klasy składa się z trzech różnych elementów:

-  **Komórki** — widoków opartych na danych dla każdego elementu
-  **Dodatkowe widoki** — widoków opartych na danych skojarzonych z sekcji.
-  **Widoki decoration** — Non-data zmiennych widoków tworzonych przez układ

## <a name="cells"></a>Komórki

Komórki są obiekty reprezentujące jeden element w zestawie danych, który zobaczy przez widok kolekcji. Każda komórka jest wystąpieniem `UICollectionViewCell` klasy, która składa się z trzech różnych widoków, jak pokazano na poniższej ilustracji:

 [![](uicollectionview-images/01-uicollectionviewcell.png "Każda komórka składa się z trzech różnych widoków, jak pokazano poniżej")](uicollectionview-images/01-uicollectionviewcell.png#lightbox)

`UICollectionViewCell` Klasa ma następujące właściwości dla każdego z tych widoków:

-   `ContentView` — Ten widok zawiera zawartość, która przedstawia komórki. Jest on renderowany w najwyższej kolejności na ekranie.
-   `SelectedBackgroundView` — Komórki ma wbudowaną obsługą dla zaznaczenia. Ten widok służy do wizualne oznaczenia, że jest zaznaczone komórki. Jest on renderowany poniżej `ContentView` po wybraniu komórki.
-   `BackgroundView` — Komórek można również wyświetlić tła, który jest przedstawiony przez `BackgroundView` . Ten widok jest renderowany poniżej `SelectedBackgroundView` .


Przez ustawienie `ContentView` taki sposób, że jego rozmiar jest mniejszy niż `BackgroundView` i `SelectedBackgroundView`, `BackgroundView` pozwala wizualnie ramki zawartości, w trybie `SelectedBackgroundView` pojawi się po wybraniu komórki, jak pokazano poniżej:

 [![](uicollectionview-images/02-cells.png "Elementy innej komórki")](uicollectionview-images/02-cells.png#lightbox)

Komórek na zrzucie ekranu powyżej są tworzone przez dziedziczenie z `UICollectionViewCell` i ustawienie `ContentView`, `SelectedBackgroundView` i `BackgroundView` właściwości, odpowiednio, jak pokazano w poniższym kodzie:

```csharp
public class AnimalCell : UICollectionViewCell
{
        UIImageView imageView;

        [Export ("initWithFrame:")]
        public AnimalCell (CGRect frame) : base (frame)
        {
            BackgroundView = new UIView{BackgroundColor = UIColor.Orange};

            SelectedBackgroundView = new UIView{BackgroundColor = UIColor.Green};

            ContentView.Layer.BorderColor = UIColor.LightGray.CGColor;
            ContentView.Layer.BorderWidth = 2.0f;
            ContentView.BackgroundColor = UIColor.White;
            ContentView.Transform = CGAffineTransform.MakeScale (0.8f, 0.8f);

            imageView = new UIImageView (UIImage.FromBundle ("placeholder.png"));
            imageView.Center = ContentView.Center;
            imageView.Transform = CGAffineTransform.MakeScale (0.7f, 0.7f);

            ContentView.AddSubview (imageView);
        }

        public UIImage Image {
            set {
                imageView.Image = value;
            }
        }
}
```

 <a name="Supplementary_Views" />


## <a name="supplementary-views"></a>Dodatkowe widoki

Dodatkowe widoki są widoki, które są dostępne informacje związane z każdej sekcji `UICollectionView`. Tak jak komórek dodatkowe widoki są oparte na danych. W przypadku, gdy komórek istnieje element danych ze źródła danych, dodatkowych widoków przedstawić sekcji danych, takich jak kategorii książki na półce lub genre utworów muzycznych w bibliotece utworów muzycznych.

Na przykład wyświetlanie dodatkowych mogła zostać użyta do prezentowania nagłówka dla określonej sekcji, jak pokazano na poniższej ilustracji:

 [![](uicollectionview-images/02a-supplementary-view.png "Dodatkowy widok używany do wyświetlania zawartości nagłówka dla określonej sekcji, jak pokazano poniżej")](uicollectionview-images/02a-supplementary-view.png#lightbox)

Aby użyć widoku dodatkowych, najpierw musi być zarejestrowana w `ViewDidLoad` metody:

```csharp
CollectionView.RegisterClassForSupplementaryView (typeof(Header), UICollectionElementKindSection.Header, headerId);
```

Następnie, widok musi zostać zwrócone przy użyciu `GetViewForSupplementaryElement`, utworzony za pomocą `DequeueReusableSupplementaryView`, a dziedziczy `UICollectionReusableView`. Poniższy fragment kodu utworzy SupplementaryView wyświetlany na zrzucie ekranu powyżej:

```csharp
public override UICollectionReusableView GetViewForSupplementaryElement (UICollectionView collectionView, NSString elementKind, NSIndexPath indexPath)
        {
            var headerView = (Header)collectionView.DequeueReusableSupplementaryView (elementKind, headerId, indexPath);
            headerView.Text = "Supplementary View";
            return headerView;
        }

```

Dodatkowe widoki są bardziej ogólne niż tylko nagłówki i stopki.
Może być umieszczony w dowolnym miejscu w widoku kolekcji, a może składać się z żadnych widoków, co można swobodnie dostosowywać ich wyglądu.

 <a name="Decoration_Views" />


## <a name="decoration-views"></a>Widoki decoration

Widoki decoration są wyłącznie visual widoków, które mogą być wyświetlane w `UICollectionView`. W odróżnieniu od komórek i dodatkowe widoki one są nie opartych na danych. Zawsze są tworzone w ramach podklasy układ, a następnie zmienić jako układ zawartości. Na przykład widoku Decoration może posłużyć do prezentowania Przewija widok tła z zawartością w `UICollectionView`, jak pokazano poniżej:

 [![](uicollectionview-images/02c-decoration-view.png "Widok decoration czerwonym tle")](uicollectionview-images/02c-decoration-view.png#lightbox)

 Poniższy fragment kodu zmienia się tła na czerwono przykłady `CircleLayout` klasy:

 ```csharp
 public class MyDecorationView : UICollectionReusableView
    {
        [Export ("initWithFrame:")]
        public MyDecorationView (CGRect frame) : base (frame)
        {
            BackgroundColor = UIColor.Red;
        }
    }
 ```


## <a name="data-source"></a>Źródło danych

Podobnie jak w przypadku innych części systemu IOS, takie jak `UITableView` i `MKMapView`, `UICollectionView` pobiera dane z *źródła danych*, która jest widoczna w Xamarin.iOS za pośrednictwem **`UICollectionViewDataSource`** klasy. Ta klasa jest odpowiedzialny za zapewnienie zawartości do `UICollectionView` takich jak:

-  **Komórki** — zwrócony z `GetCell` metody.
-  **Dodatkowe widoki** — zwrócony z `GetViewForSupplementaryElement` metody.
-  **Liczba sekcji** — zwrócony z `NumberOfSections` metody. Wartość domyślna to 1, jeśli nie jest zaimplementowana.
-  **Liczba elementów w każdej sekcji** — zwrócony z `GetItemsCount` metody.

### <a name="uicollectionviewcontroller"></a>UICollectionViewController
Dla wygody `UICollectionViewController` klasy jest dostępny. To jest automatycznie konfigurowany do obu delegata, która została szczegółowo opisana w następnej sekcji, można i źródło danych dla jej `UICollectionView` widoku.

Jak `UITableView`, `UICollectionView` klasy będzie wywoływać tylko źródła danych można uzyskać komórek elementów znajdujących się na ekranie.
Komórkom przewinięcie ekranu są umieszczane w kolejce do ponownego użycia, jak na poniższym obrazie przedstawiono:

 [![](uicollectionview-images/03-cell-reuse.png "Komórkom przewinięcie ekranu są umieszczane w kolejce do ponownego użycia w sposób pokazany poniżej")](uicollectionview-images/03-cell-reuse.png#lightbox)

Uproszczonej ponownemu komórki `UICollectionView` i `UITableView`. Nie trzeba utworzyć komórki bezpośrednio w źródle danych, jeśli jeden nie jest dostępna w kolejce ponownemu jako komórki są zarejestrowane w systemie. Jeśli komórka nie jest dostępna podczas wywołania do usuwania kolejki komórki z kolejki ponownemu, iOS zostanie utworzony automatycznie na podstawie typu lub nib, który został zarejestrowany.
Ta sama metoda jest również dostępny do dodatkowych widoków.

Rozważmy na przykład następujący kod, który rejestruje `AnimalCell` klasy:

```csharp
static NSString animalCellId = new NSString ("AnimalCell");
CollectionView.RegisterClassForCell (typeof(AnimalCell), animalCellId);
```

Gdy `UICollectionView` musi komórki, ponieważ element znajduje się na ekranie `UICollectionView` wywołuje źródła danych `GetCell` metody. Podobnie jak to działa z UITableView, ta metoda jest odpowiedzialna za skonfigurowanie komórki danych zapasowy, który będzie `AnimalCell` w takim przypadku klasa.

Poniższy kod przedstawia implementację `GetCell` zwracającą `AnimalCell` wystąpienie:

```csharp
public override UICollectionViewCell GetCell (UICollectionView collectionView, Foundation.NSIndexPath indexPath)
{
        var animalCell = (AnimalCell)collectionView.DequeueReusableCell (animalCellId, indexPath);

        var animal = animals [indexPath.Row];

        animalCell.Image = animal.Image;

        return animalCell;
}
```

Wywołanie `DequeReusableCell` jest w przypadku, gdy komórka zostaną albo cofnąć umieszczone w kolejce z kolejki ponownemu lub, jeśli komórka nie jest dostępna w kolejce na podstawie typu zarejestrowany w wywołaniu `CollectionView.RegisterClassForCell`.

W takim przypadku rejestrując `AnimalCell` klasy, z systemem iOS zostanie utworzony nowy `AnimalCell` wewnętrznie i przywróć jego po wywołania do usuwania kolejki komórki, po upływie którego jest skonfigurowany z obrazem zawarta w klasie zwierząt i zwracane do wyświetlenia na `UICollectionView`.

 <a name="Delegate" />


### <a name="delegate"></a>Delegate

`UICollectionView` Klasy używa delegowanego typu `UICollectionViewDelegate` do obsługi interakcji z zawartością w `UICollectionView`. To zapewnia kontrolę nad:

-  **Zaznaczanie komórek** — Określa, czy jest zaznaczona komórki.
-  **Komórki podświetlanie** — Określa, czy jest obecnie trwa dotknięciu komórki.
-  **Komórki menu** — Menu wyświetlanych dla komórki w odpowiedzi na gestu długi naciśnij.


Podobnie jak w przypadku źródła danych `UICollectionViewController` jest domyślnie skonfigurowane, aby być delegowanie dla `UICollectionView`.

 <a name="Cell_HighLighting" />


#### <a name="cell-highlighting"></a>Wyróżnianie komórki

Po naciśnięciu komórki, przejścia do komórki w zaznaczony stan i nie jest zaznaczone do użytkownika wind ich palca z komórki. Dzięki tymczasowej zmiany wyglądu komórki przed faktycznie jest zaznaczone. W przypadku zaznaczenia, komórki `SelectedBackgroundView` jest wyświetlany. Na poniższym rysunku przedstawiono zaznaczony stan tuż przed ustawieniem występuje:

 [![](uicollectionview-images/04-cell-highlight.png "Na poniższym rysunku pokazano zaznaczony stan tuż przed ustawieniem występuje")](uicollectionview-images/04-cell-highlight.png#lightbox)

Aby zaimplementować wyróżnianie, `ItemHighlighted` i `ItemUnhighlighted` metody `UICollectionViewDelegate` mogą być używane. Na przykład następujący kod zastosuje żółty tła `ContentView` podczas komórki zostanie wyróżniona i białe tło podczas odinstalowywania wyróżnione, jak pokazano na ilustracji powyżej:

```csharp
public override void ItemHighlighted (UICollectionView collectionView, NSIndexPath indexPath)
{
        var cell = collectionView.CellForItem(indexPath);
        cell.ContentView.BackgroundColor = UIColor.Yellow;
}

public override void ItemUnhighlighted (UICollectionView collectionView, NSIndexPath indexPath)
{
        var cell = collectionView.CellForItem(indexPath);
        cell.ContentView.BackgroundColor = UIColor.White;
}
```

 <a name="Disabling_Selection" />


#### <a name="disabling-selection"></a>Wyłączenie zaznaczenia

Wybór jest domyślnie włączone w `UICollectionView`. Aby wyłączyć zaznaczenia, należy zastąpić `ShouldHighlightItem` i zwróci wartość false, jak pokazano poniżej:

```csharp
public override bool ShouldHighlightItem (UICollectionView collectionView, NSIndexPath indexPath)
{
        return false;
}
```

Po wyłączeniu wyróżnianie proces wybierania komórki jest również wyłączone. Ponadto istnieje również `ShouldSelectItem` metody kontrolujące wybór bezpośrednio, ale jeśli `ShouldHighlightItem` jest zaimplementowana i zwraca wartość FAŁSZ, `ShouldSelectItem` nie jest wywoływany.

 `ShouldSelectItem` Umożliwia wybór należy włączyć lub wyłączyć na podstawie elementu — gdy `ShouldHighlightItem` nie jest zaimplementowana. Umożliwia także wyróżnianie bez wyboru, jeśli `ShouldHighlightItem` jest zaimplementowana i zwraca wartość true, podczas `ShouldSelectItem` zwraca wartość false.

 <a name="Cell_Menus" />


#### <a name="cell-menus"></a>Menu komórki

Każda komórka w `UICollectionView` Wyświetla menu, które umożliwia wycinania, kopiowania i wklejania opcjonalnie obsługiwany. Aby utworzyć menu Edycja w komórce:

1.  Zastąpienie `ShouldShowMenu` i zwraca wartość true, jeśli element powinien być wyświetlony menu.
1.  Zastąpienie `CanPerformAction` i zwracają wartość true dla każdej akcji, która może wykonać elementu, który będzie wycinania, kopiowania ani wklejania.
1.  Zastąpienie `PerformAction` przeprowadzić edycję, skopiuj operacji wklejania.


Poniższy zrzut ekranu Pokaż menu po naciśnięciu klawisza komórki długo:

 [![](uicollectionview-images/04a-menu.png "Ten zrzut ekranu Pokaż menu po naciśnięciu długo komórki")](uicollectionview-images/04a-menu.png#lightbox)

 <a name="Layout" />


## <a name="layout"></a>Układ

`UICollectionView` obsługuje system układu, umożliwiający pozycjonowanie wszystkich swoich elementów, komórek, dodatkowych widoków i dekoracji, mają być zarządzane niezależnie od `UICollectionView` samej siebie.
Przy użyciu systemu układu, aplikacja może obsługiwać układy, takie jak siatki możemy w tym artykule przedstawiono, jak również zapewniają układy niestandardowe.

 <a name="Layout_Basics" />


### <a name="layout-basics"></a>Podstawowe informacje dotyczące układu

Układy w `UICollectionView` są zdefiniowane w klasie, która dziedziczy `UICollectionViewLayout`. Implementacja układ jest odpowiedzialny za tworzenie układu atrybuty dla każdego elementu w `UICollectionView`. Istnieją dwa sposoby tworzenia układu:

-  Użyj wbudowanej `UICollectionViewFlowLayout` .
-  Podaj układu niestandardowego przez dziedziczenie z `UICollectionViewLayout` .


 <a name="Flow_Layout" />


### <a name="flow-layout"></a>Układ przepływu

`UICollectionViewFlowLayout` Klasa udostępnia liniowej układu to odpowiednie dla rozmieszczanie zawartości w siatce komórek, jak firma Microsoft w tym samouczku.

Aby użyć układu przepływu:

-  Utwórz wystąpienie `UICollectionViewFlowLayout` :


```csharp
var layout = new UICollectionViewFlowLayout ();
```

-  Przekaż wystąpienie konstruktora atrybutu `UICollectionView` :


```csharp
simpleCollectionViewController = new SimpleCollectionViewController (layout);
```

To są wystarczające do układu zawartości w siatce. Ponadto podczas orientację zmiany, `UICollectionViewFlowLayout` obsługuje zmienianie rozmieszczenia zawartości odpowiednio, jak pokazano poniżej:

 [![](uicollectionview-images/05-layout-orientation.png "Przykład zmiany orientacji")](uicollectionview-images/05-layout-orientation.png#lightbox)

 <a name="Section_Inset" />


#### <a name="section-inset"></a>Wstawki sekcji

Aby zapewnić miejsce wokół `UIContentView`, ma układów `SectionInset` właściwości typu `UIEdgeInsets`. Na przykład następujący kod udostępnia bufor 50 pikseli wokół każdej sekcji `UIContentView` gdy poukładany `UICollectionViewFlowLayout`:

```csharp
var layout = new UICollectionViewFlowLayout ();
layout.SectionInset = new UIEdgeInsets (50,50,50,50);
```

Powoduje to odstępy wokół sekcji, jak pokazano poniżej:

 [![](uicollectionview-images/06-sectioninset.png "Odstępy dookoła sekcji, jak pokazano poniżej")](uicollectionview-images/06-sectioninset.png#lightbox)

 <a name="Subclassing_UICollectionViewFlowLayout" />


#### <a name="subclassing-uicollectionviewflowlayout"></a>Tworzenie podklas UICollectionViewFlowLayout

W wersji przy użyciu `UICollectionViewFlowLayout` bezpośrednio, jego można również odziedziczenia dostosować układ zawartości wzdłuż linii. Na przykład to mogą służyć do tworzenia układu nie jest zawijany komórek w siatce, ale zamiast tego tworzy pojedynczy wiersz z efektem przewijania poziomie, jak pokazano poniżej:

 [![](uicollectionview-images/07-line-layout.png "Pojedynczy wiersz z efektem przewijania pozioma")](uicollectionview-images/07-line-layout.png#lightbox)

Aby zaimplementować to przez podklasy `UICollectionViewFlowLayout` wymaga:

-  Inicjowanie jakiekolwiek właściwości układu, które dotyczą sam układ lub wszystkie elementy w układzie w konstruktorze.
-  Zastępowanie `ShouldInvalidateLayoutForBoundsChange` , zwracająca wartość true, że w przypadku granice `UICollectionView` zostaną obliczone ponownie zmiany, układ komórek. Ten element jest używany w takim przypadku Sprawdź kod dla transformacji stosowany do komórek centermost mają zostać zastosowane podczas przewijania.
-  Zastępowanie `TargetContentOffset` dokonanie centermost komórki przyciąganie do środka `UICollectionView` jako przewijanie zatrzymuje.
-  Zastępowanie `LayoutAttributesForElementsInRect` do zwraca tablicę `UICollectionViewLayoutAttributes` . Każdy `UICollectionViewLayoutAttribute` zawiera informacje na temat sposobu układu konkretny element, w tym właściwości, takie jak jego `Center` , `Size` , `ZIndex` i `Transform3D` .


Poniższy kod przedstawia implementację takie:

```csharp
using System;
using CoreGraphics;
using Foundation;
using UIKit;
using CoreGraphics;
using CoreAnimation;

namespace SimpleCollectionView
{
    public class LineLayout : UICollectionViewFlowLayout
    {
        public const float ITEM_SIZE = 200.0f;
        public const int ACTIVE_DISTANCE = 200;
        public const float ZOOM_FACTOR = 0.3f;

        public LineLayout ()
        {
            ItemSize = new CGSize (ITEM_SIZE, ITEM_SIZE);
            ScrollDirection = UICollectionViewScrollDirection.Horizontal;
            SectionInset = new UIEdgeInsets (400,0,400,0);
            MinimumLineSpacing = 50.0f;
        }

        public override bool ShouldInvalidateLayoutForBoundsChange (CGRect newBounds)
        {
            return true;
        }

        public override UICollectionViewLayoutAttributes[] LayoutAttributesForElementsInRect (CGRect rect)
        {
            var array = base.LayoutAttributesForElementsInRect (rect);
            var visibleRect = new CGRect (CollectionView.ContentOffset, CollectionView.Bounds.Size);

            foreach (var attributes in array) {
                if (attributes.Frame.IntersectsWith (rect)) {
                    float distance = (float)(visibleRect.GetMidX () - attributes.Center.X);
                    float normalizedDistance = distance / ACTIVE_DISTANCE;
                    if (Math.Abs (distance) < ACTIVE_DISTANCE) {
                        float zoom = 1 + ZOOM_FACTOR * (1 - Math.Abs (normalizedDistance));
                        attributes.Transform3D = CATransform3D.MakeScale (zoom, zoom, 1.0f);
                        attributes.ZIndex = 1;
                    }
                }
            }
            return array;
        }

        public override CGPoint TargetContentOffset (CGPoint proposedContentOffset, CGPoint scrollingVelocity)
        {
            float offSetAdjustment = float.MaxValue;
            float horizontalCenter = (float)(proposedContentOffset.X + (this.CollectionView.Bounds.Size.Width / 2.0));
            CGRect targetRect = new CGRect (proposedContentOffset.X, 0.0f, this.CollectionView.Bounds.Size.Width, this.CollectionView.Bounds.Size.Height);
            var array = base.LayoutAttributesForElementsInRect (targetRect);
            foreach (var layoutAttributes in array) {
                float itemHorizontalCenter = (float)layoutAttributes.Center.X;
                if (Math.Abs (itemHorizontalCenter - horizontalCenter) < Math.Abs (offSetAdjustment)) {
                    offSetAdjustment = itemHorizontalCenter - horizontalCenter;
                }
            }
            return new CGPoint (proposedContentOffset.X + offSetAdjustment, proposedContentOffset.Y);
        }

    }
}
```

 <a name="Custom_Layout" />


### <a name="custom-layout"></a>Niestandardowego układu

Oprócz używania `UICollectionViewFlowLayout`, układów można również całkowicie dostosować przez dziedziczenie bezpośrednio z `UICollectionViewLayout`.

Klucza metody do przesłonięcia to:

-   `PrepareLayout` — Używany do wykonywania początkowej geometrycznych obliczeń, które będą używane w całym procesie układu.
-   `CollectionViewContentSize` — Zwraca rozmiar obszaru, używany do wyświetlania zawartości.
-   `LayoutAttributesForElementsInRect` — Podczas UICollectionViewFlowLayout przykładzie pokazano wcześniej, ta metoda służy do zapewnienia informacji do `UICollectionView` dotyczących układu poszczególnych elementów. Jednak w przeciwieństwie do `UICollectionViewFlowLayout` , podczas tworzenia niestandardowego układu, można umieszczać elementy, jednak można wybrać.


Na przykład tej samej zawartości może być przedstawiony w układzie cykliczne, jak pokazano poniżej:

 [![](uicollectionview-images/08-circle-layout.png "Cykliczne niestandardowego układu, jak pokazano poniżej")](uicollectionview-images/08-circle-layout.png#lightbox)

Zaawansowane kwestią dotyczącą układów jest to, że można zmienić z układu siatki na układ poziomy przewijania i następnie do tego układu cykliczne wymaga tylko klasy układu dostarczony do `UICollectionView` można zmienić. Nic w `UICollectionView`, jego delegata lub danych źródła zmian kodu w ogóle.


## <a name="changes-in-ios-9"></a>Zmiany w systemie iOS 9


W systemie iOS 9, widok kolekcji (`UICollectionView`) teraz obsługuje przeciągnij zmiany kolejności elementów poza pole przez dodanie nowych aparat rozpoznawania gestów domyślne i kilka nowych metod pomocniczych.

Przy użyciu tych nowych metod, można łatwo zaimplementować przeciągnij, aby zmienić kolejność, w widoku kolekcji i dostępna jest opcja Dostosowywanie wyglądu elementy w każdym etapie procesu opcję.

[![](uicollectionview-images/intro01.png "Przykład procesu opcję")](uicollectionview-images/intro01.png#lightbox)

W tym artykule firma Microsoft będzie Przyjrzyjmy się implementacja przeciągania do zmiany kolejności w aplikacji platformy Xamarin.iOS, a także niektóre zmiany wprowadzone do kontrolki widok kolekcji z systemem iOS 9:

- [Łatwe zmiany kolejności elementów](#Easy-Reordering-of-Items)
    - [Prosty przykład opcję](#Simple-Reordering-Example)
    - [Przy użyciu aparat rozpoznawania gestów niestandardowych](#Using-a-Custom-Gesture-Recognizer)
    - [Niestandardowe układy i zmianę kolejności](#Custom-Layouts-and-Reording)
- [Przeglądanie zmian w kolekcji](#Collection-View-Changes)

<a name="Easy-Reordering-of-Items" />

## <a name="reordering-of-items"></a>Ponowne porządkowanie elementów

Jak już wspomniano, jedną z najbardziej znaczących zmian do widoku kolekcji w systemie iOS 9 był łatwy funkcja przeciągania do zmiany kolejności poza pole.

W systemie iOS 9, jest użycie najszybszym sposobem na dodawanie, zmienianie kolejności do widoku kolekcji `UICollectionViewController`.
Kontroler widoku kolekcji ma teraz `InstallsStandardGestureForInteractiveMovement` właściwość, która dodaje standard *aparat rozpoznawania gestów* obsługującej przeciąganie, aby zmienić kolejność elementów w kolekcji.
Ponieważ wartość domyślna to `true`, należy zaimplementować `MoveItem` metody `UICollectionViewDataSource` klasy do obsługi przeciągania do zmiany kolejności. Na przykład:

```csharp
public override void MoveItem (UICollectionView collectionView, NSIndexPath sourceIndexPath, NSIndexPath destinationIndexPath)
{
    // Reorder our list of items
    ...
}
```
<a name="Simple-Reordering-Example" />

### <a name="simple-reordering-example"></a>Prosty przykład opcję

Przykład szybki start nowego projektu platformy Xamarin.iOS i edytować **Main.storyboard** pliku. Przeciągnij `UICollectionViewController` na powierzchnię projektu:

[![](uicollectionview-images/quick01.png "Dodawanie UICollectionViewController")](uicollectionview-images/quick01.png#lightbox)

Wybierz widok kolekcji (być może jest ona najłatwiej wykonać te czynności w konspekt dokumentu). Na karcie Układ konsoli właściwości należy ustawić następujące wymiary, jak pokazano na poniższym zrzucie ekranu:

- **Rozmiar komórki**: szerokość — 60 | Wysokość — 60
- **Rozmiar nagłówka**: szerokość — 0 | Wysokość — 0
- **Rozmiar stopki**: szerokość — 0 | Wysokość — 0
- **Odstępy min**: dla komórek — 8 | Wiersze — 8
- **Sekcja marginesy**: Top – 16 | Dolny – 16 | Po lewej – 16 | Prawo – 16

[![](uicollectionview-images/quick04.png "Ustaw rozmiary widok kolekcji")](uicollectionview-images/quick04.png#lightbox)

Następnie Edytuj domyślne komórki:
    - Zmienianie kolorów tła niebieski
    - Dodaj etykietę do działania jako tytuł w komórce
    - Ustaw identyfikator ponownemu **komórki**

[![](uicollectionview-images/quick02.png "Edytuj domyślne komórki")](uicollectionview-images/quick02.png#lightbox)

Dodawanie ograniczeń do zachowania wyśrodkowana wewnątrz komórki, ponieważ zmienia rozmiar etykiety:

W **konsoli właściwości** dla _CollectionViewCell_ i ustaw **klasy** do `TextCollectionViewCell`:

[![](uicollectionview-images/quick05.png "Ustaw klasy TextCollectionViewCell")](uicollectionview-images/quick05.png#lightbox)

Ustaw **wielokrotnego użytku widok kolekcji** do `Cell`:

[![](uicollectionview-images/quick06.png "Ustawianie widoku do ponownego użycia kolekcji do komórki")](uicollectionview-images/quick06.png#lightbox)

Na koniec wybierz etykietę i nadaj mu nazwę `TextLabel`:

[![](uicollectionview-images/quick07.png "Etykieta nazwy TextLabel")](uicollectionview-images/quick07.png#lightbox)

Edytuj `TextCollectionViewCell` klasy i dodaj następujące właściwości.:

```csharp
using System;
using Foundation;
using UIKit;

namespace CollectionView
{
    public partial class TextCollectionViewCell : UICollectionViewCell
    {
        #region Computed Properties
        public string Title {
            get { return TextLabel.Text; }
            set { TextLabel.Text = value; }
        }
        #endregion

        #region Constructors
        public TextCollectionViewCell (IntPtr handle) : base (handle)
        {
        }
        #endregion
    }
}
```

W tym miejscu `Text` właściwość etykiety jest ujawniona jako tytuł komórki, dlatego można ją ustawić z kodu.

Dodaj nową klasę C# do projektu i nadaj mu `WaterfallCollectionSource`. Przeprowadź edycję pliku i przydzielić mu wyglądać następująco:

```csharp
using System;
using Foundation;
using UIKit;
using System.Collections.Generic;

namespace CollectionView
{
    public class WaterfallCollectionSource : UICollectionViewDataSource
    {
        #region Computed Properties
        public WaterfallCollectionView CollectionView { get; set;}
        public List<int> Numbers { get; set; } = new List<int> ();
        #endregion

        #region Constructors
        public WaterfallCollectionSource (WaterfallCollectionView collectionView)
        {
            // Initialize
            CollectionView = collectionView;

            // Init numbers collection
            for (int n = 0; n < 100; ++n) {
                Numbers.Add (n);
            }
        }
        #endregion

        #region Override Methods
        public override nint NumberOfSections (UICollectionView collectionView) {
            // We only have one section
            return 1;
        }

        public override nint GetItemsCount (UICollectionView collectionView, nint section) {
            // Return the number of items
            return Numbers.Count;
        }

        public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
        {
            // Get a reusable cell and set {~~it's~>its~~} title from the item
            var cell = collectionView.DequeueReusableCell ("Cell", indexPath) as TextCollectionViewCell;
            cell.Title = Numbers [(int)indexPath.Item].ToString();

            return cell;
        }

        public override bool CanMoveItem (UICollectionView collectionView, NSIndexPath indexPath) {
            // We can always move items
            return true;
        }

        public override void MoveItem (UICollectionView collectionView, NSIndexPath sourceIndexPath, NSIndexPath destinationIndexPath)
        {
            // Reorder our list of items
            var item = Numbers [(int)sourceIndexPath.Item];
            Numbers.RemoveAt ((int)sourceIndexPath.Item);
            Numbers.Insert ((int)destinationIndexPath.Item, item);
        }
        #endregion
    }
}
```

Ta klasa zostanie źródła danych dla naszych widok kolekcji i podaj informacje dotyczące każdej komórki w kolekcji.
Zwróć uwagę, że `MoveItem` zaimplementowano metody zezwalająca przeciągnij elementy w kolekcji muszą być kolejnos'ci.

Dodaj nową C# klasę do projektu i nadaj mu `WaterfallCollectionDelegate`. Edytuj ten plik i przydzielić mu wyglądać następująco:

```csharp
using System;
using Foundation;
using UIKit;
using System.Collections.Generic;

namespace CollectionView
{
    public class WaterfallCollectionDelegate : UICollectionViewDelegate
    {
        #region Computed Properties
        public WaterfallCollectionView CollectionView { get; set;}
        #endregion

        #region Constructors
        public WaterfallCollectionDelegate (WaterfallCollectionView collectionView)
        {

            // Initialize
            CollectionView = collectionView;

        }
        #endregion

        #region Overrides Methods
        public override bool ShouldHighlightItem (UICollectionView collectionView, NSIndexPath indexPath) {
            // Always allow for highlighting
            return true;
        }

        public override void ItemHighlighted (UICollectionView collectionView, NSIndexPath indexPath)
        {
            // Get cell and change to green background
            var cell = collectionView.CellForItem(indexPath);
            cell.ContentView.BackgroundColor = UIColor.FromRGB(183,208,57);
        }

        public override void ItemUnhighlighted (UICollectionView collectionView, NSIndexPath indexPath)
        {
            // Get cell and return to blue background
            var cell = collectionView.CellForItem(indexPath);
            cell.ContentView.BackgroundColor = UIColor.FromRGB(164,205,255);
        }
        #endregion
    }
}
```

To będzie działać jak obiekt delegowany dla naszych widok kolekcji. Aby wyróżnić komórki, ponieważ użytkownik wchodzi w interakcję z nią w widoku kolekcji zostały nadpisane metody.

Dodaj jedną ostatniego C# klasę do projektu i nadaj mu `WaterfallCollectionView`. Edytuj ten plik i przydzielić mu wyglądać następująco:

```csharp
using System;
using UIKit;
using System.Collections.Generic;
using Foundation;

namespace CollectionView
{
    [Register("WaterfallCollectionView")]
    public class WaterfallCollectionView : UICollectionView
    {

        #region Constructors
        public WaterfallCollectionView (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();

            // Initialize
            DataSource = new WaterfallCollectionSource(this);
            Delegate = new WaterfallCollectionDelegate(this);

        }
        #endregion
    }
}
```

Zwróć uwagę, że `DataSource` i `Delegate` utworzony powyżej są ustawiane podczas widok kolekcji jest utworzone na podstawie jego scenorysu (lub **.xib** pliku).

Edytuj **Main.storyboard** ponownie plik i wybierz widok kolekcji i przejdź do **właściwości**. Ustaw **klasy** niestandardowe `WaterfallCollectionView` klasy zdefiniowanego powyżej:

Zapisać zmiany wprowadzone do interfejsu użytkownika i uruchomić aplikację.
Jeśli użytkownik wybierze element z listy i przeciągania go do nowej lokalizacji, innych elementów zostanie animować automatycznie, jak przenieść do końca elementu.
Gdy użytkownik porzuca elementu w nowej lokalizacji, będzie trzymaj do tej lokalizacji. Na przykład:

[![](uicollectionview-images/intro01.png "Przykład przeciągnięcie elementu do nowej lokalizacji")](uicollectionview-images/intro01.png#lightbox)

<a name="Using-a-Custom-Gesture-Recognizer" />

### <a name="using-a-custom-gesture-recognizer"></a>Przy użyciu aparat rozpoznawania gestów niestandardowych

W przypadkach, gdy nie można użyć `UICollectionViewController` i musi być zwykły `UIViewController`, lub jeśli chcesz wyłączyć mieć większą kontrolę nad gestu przeciągania i upuszczania, można utworzyć własne niestandardowe aparat rozpoznawania gestów i dodaj go do widoku kolekcji po załadowaniu widoku. Na przykład:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Create a custom gesture recognizer
    var longPressGesture = new UILongPressGestureRecognizer ((gesture) => {

        // Take action based on state
        switch(gesture.State) {
        case UIGestureRecognizerState.Began:
            var selectedIndexPath = CollectionView.IndexPathForItemAtPoint(gesture.LocationInView(View));
            if (selectedIndexPath !=null) {
                CollectionView.BeginInteractiveMovementForItem(selectedIndexPath);
            }
            break;
        case UIGestureRecognizerState.Changed:
            CollectionView.UpdateInteractiveMovementTargetPosition(gesture.LocationInView(View));
            break;
        case UIGestureRecognizerState.Ended:
            CollectionView.EndInteractiveMovement();
            break;
        default:
            CollectionView.CancelInteractiveMovement();
            break;
        }

    });

    // Add the custom recognizer to the collection view
    CollectionView.AddGestureRecognizer(longPressGesture);
}
```

W tym miejscu kilka nowych metod dodany do kolekcji widoku jest używany do wdrożenia i kontrolowania operacji przeciągania:

 - `BeginInteractiveMovementForItem` -Oznacza początek operacji przenoszenia.
 - `UpdateInteractiveMovementTargetPosition` -Są wysyłane jako lokalizacja elementu zostanie zaktualizowany.
 - `EndInteractiveMovement` -Oznacza koniec przenoszenia elementu.
 - `CancelInteractiveMovement` -Oznacza użytkownika anulowanie operacji przenoszenia.

Gdy aplikacja jest uruchamiana, operacji przeciągania będą działać dokładnie tak, domyślnie przeciągnij aparat rozpoznawania gestów, która pochodzi z widokiem kolekcji.

<a name="Custom-Layouts-and-Reording" />

### <a name="custom-layouts-and-reordering"></a>Niestandardowe układy i zmianę kolejności

W systemie iOS 9 kilka nowych metod zostały dodane do pracy z przeciągania do zmiany kolejności i niestandardowe układy w widoku kolekcji. Aby zapoznać się z tej funkcji, Dodajmy niestandardowego układu do kolekcji.

Najpierw dodaj nową klasę C# o nazwie `WaterfallCollectionLayout` do projektu. Go edytować i naprawić ona wyglądać następująco:

```csharp
using System;
using Foundation;
using UIKit;
using System.Collections.Generic;
using CoreGraphics;

namespace CollectionView
{
    [Register("WaterfallCollectionLayout")]
    public class WaterfallCollectionLayout : UICollectionViewLayout
    {
        #region Private Variables
        private int columnCount = 2;
        private nfloat minimumColumnSpacing = 10;
        private nfloat minimumInterItemSpacing = 10;
        private nfloat headerHeight = 0.0f;
        private nfloat footerHeight = 0.0f;
        private UIEdgeInsets sectionInset = new UIEdgeInsets(0, 0, 0, 0);
        private WaterfallCollectionRenderDirection itemRenderDirection = WaterfallCollectionRenderDirection.ShortestFirst;
        private Dictionary<nint,UICollectionViewLayoutAttributes> headersAttributes = new Dictionary<nint, UICollectionViewLayoutAttributes>();
        private Dictionary<nint,UICollectionViewLayoutAttributes> footersAttributes = new Dictionary<nint, UICollectionViewLayoutAttributes>();
        private List<CGRect> unionRects = new List<CGRect>();
        private List<nfloat> columnHeights = new List<nfloat>();
        private List<UICollectionViewLayoutAttributes> allItemAttributes = new List<UICollectionViewLayoutAttributes>();
        private List<List<UICollectionViewLayoutAttributes>> sectionItemAttributes = new List<List<UICollectionViewLayoutAttributes>>();
        private nfloat unionSize = 20;
        #endregion

        #region Computed Properties
        [Export("ColumnCount")]
        public int ColumnCount {
            get { return columnCount; }
            set {
                WillChangeValue ("ColumnCount");
                columnCount = value;
                DidChangeValue ("ColumnCount");

                InvalidateLayout ();
            }
        }

        [Export("MinimumColumnSpacing")]
        public nfloat MinimumColumnSpacing {
            get { return minimumColumnSpacing; }
            set {
                WillChangeValue ("MinimumColumnSpacing");
                minimumColumnSpacing = value;
                DidChangeValue ("MinimumColumnSpacing");

                InvalidateLayout ();
            }
        }

        [Export("MinimumInterItemSpacing")]
        public nfloat MinimumInterItemSpacing {
            get { return minimumInterItemSpacing; }
            set {
                WillChangeValue ("MinimumInterItemSpacing");
                minimumInterItemSpacing = value;
                DidChangeValue ("MinimumInterItemSpacing");

                InvalidateLayout ();
            }
        }

        [Export("HeaderHeight")]
        public nfloat HeaderHeight {
            get { return headerHeight; }
            set {
                WillChangeValue ("HeaderHeight");
                headerHeight = value;
                DidChangeValue ("HeaderHeight");

                InvalidateLayout ();
            }
        }

        [Export("FooterHeight")]
        public nfloat FooterHeight {
            get { return footerHeight; }
            set {
                WillChangeValue ("FooterHeight");
                footerHeight = value;
                DidChangeValue ("FooterHeight");

                InvalidateLayout ();
            }
        }

        [Export("SectionInset")]
        public UIEdgeInsets SectionInset {
            get { return sectionInset; }
            set {
                WillChangeValue ("SectionInset");
                sectionInset = value;
                DidChangeValue ("SectionInset");

                InvalidateLayout ();
            }
        }

        [Export("ItemRenderDirection")]
        public WaterfallCollectionRenderDirection ItemRenderDirection {
            get { return itemRenderDirection; }
            set {
                WillChangeValue ("ItemRenderDirection");
                itemRenderDirection = value;
                DidChangeValue ("ItemRenderDirection");

                InvalidateLayout ();
            }
        }
        #endregion

        #region Constructors
        public WaterfallCollectionLayout ()
        {
        }

        public WaterfallCollectionLayout(NSCoder coder) : base(coder) {

        }
        #endregion

        #region Public Methods
        public nfloat ItemWidthInSectionAtIndex(int section) {

            var width = CollectionView.Bounds.Width - SectionInset.Left - SectionInset.Right;
            return (nfloat)Math.Floor ((width - ((ColumnCount - 1) * MinimumColumnSpacing)) / ColumnCount);
        }
        #endregion

        #region Override Methods
        public override void PrepareLayout ()
        {
            base.PrepareLayout ();

            // Get the number of sections
            var numberofSections = CollectionView.NumberOfSections();
            if (numberofSections == 0)
                return;

            // Reset collections
            headersAttributes.Clear ();
            footersAttributes.Clear ();
            unionRects.Clear ();
            columnHeights.Clear ();
            allItemAttributes.Clear ();
            sectionItemAttributes.Clear ();

            // Initialize column heights
            for (int n = 0; n < ColumnCount; n++) {
                columnHeights.Add ((nfloat)0);
            }

            // Process all sections
            nfloat top = 0.0f;
            var attributes = new UICollectionViewLayoutAttributes ();
            var columnIndex = 0;
            for (nint section = 0; section < numberofSections; ++section) {
                // Calculate section specific metrics
                var minimumInterItemSpacing = (MinimumInterItemSpacingForSection == null) ? MinimumColumnSpacing :
                    MinimumInterItemSpacingForSection (CollectionView, this, section);

                // Calculate widths
                var width = CollectionView.Bounds.Width - SectionInset.Left - SectionInset.Right;
                var itemWidth = (nfloat)Math.Floor ((width - ((ColumnCount - 1) * MinimumColumnSpacing)) / ColumnCount);

                // Calculate section header
                var heightHeader = (HeightForHeader == null) ? HeaderHeight :
                    HeightForHeader (CollectionView, this, section);

                if (heightHeader > 0) {
                    attributes = UICollectionViewLayoutAttributes.CreateForSupplementaryView (UICollectionElementKindSection.Header, NSIndexPath.FromRowSection (0, section));
                    attributes.Frame = new CGRect (0, top, CollectionView.Bounds.Width, heightHeader);
                    headersAttributes.Add (section, attributes);
                    allItemAttributes.Add (attributes);

                    top = attributes.Frame.GetMaxY ();
                }

                top += SectionInset.Top;
                for (int n = 0; n < ColumnCount; n++) {
                    columnHeights [n] = top;
                }

                // Calculate Section Items
                var itemCount = CollectionView.NumberOfItemsInSection(section);
                List<UICollectionViewLayoutAttributes> itemAttributes = new List<UICollectionViewLayoutAttributes> ();

                for (nint n = 0; n < itemCount; n++) {
                    var indexPath = NSIndexPath.FromRowSection (n, section);
                    columnIndex = NextColumnIndexForItem (n);
                    var xOffset = SectionInset.Left + (itemWidth + MinimumColumnSpacing) * (nfloat)columnIndex;
                    var yOffset = columnHeights [columnIndex];
                    var itemSize = (SizeForItem == null) ? new CGSize (0, 0) : SizeForItem (CollectionView, this, indexPath);
                    nfloat itemHeight = 0.0f;

                    if (itemSize.Height > 0.0f && itemSize.Width > 0.0f) {
                        itemHeight = (nfloat)Math.Floor (itemSize.Height * itemWidth / itemSize.Width);
                    }

                    attributes = UICollectionViewLayoutAttributes.CreateForCell (indexPath);
                    attributes.Frame = new CGRect (xOffset, yOffset, itemWidth, itemHeight);
                    itemAttributes.Add (attributes);
                    allItemAttributes.Add (attributes);
                    columnHeights [columnIndex] = attributes.Frame.GetMaxY () + MinimumInterItemSpacing;
                }
                sectionItemAttributes.Add (itemAttributes);

                // Calculate Section Footer
                nfloat footerHeight = 0.0f;
                columnIndex = LongestColumnIndex();
                top = columnHeights [columnIndex] - MinimumInterItemSpacing + SectionInset.Bottom;
                footerHeight = (HeightForFooter == null) ? FooterHeight : HeightForFooter(CollectionView, this, section);

                if (footerHeight > 0) {
                    attributes = UICollectionViewLayoutAttributes.CreateForSupplementaryView (UICollectionElementKindSection.Footer, NSIndexPath.FromRowSection (0, section));
                    attributes.Frame = new CGRect (0, top, CollectionView.Bounds.Width, footerHeight);
                    footersAttributes.Add (section, attributes);
                    allItemAttributes.Add (attributes);
                    top = attributes.Frame.GetMaxY ();
                }

                for (int n = 0; n < ColumnCount; n++) {
                    columnHeights [n] = top;
                }
            }

            var i =0;
            var attrs = allItemAttributes.Count;
            while(i < attrs) {
                var rect1 = allItemAttributes [i].Frame;
                i = (int)Math.Min (i + unionSize, attrs) - 1;
                var rect2 = allItemAttributes [i].Frame;
                unionRects.Add (CGRect.Union (rect1, rect2));
                i++;
            }

        }

        public override CGSize CollectionViewContentSize {
            get {
                if (CollectionView.NumberOfSections () == 0) {
                    return new CGSize (0, 0);
                }

                var contentSize = CollectionView.Bounds.Size;
                contentSize.Height = columnHeights [0];
                return contentSize;
            }
        }

        public override UICollectionViewLayoutAttributes LayoutAttributesForItem (NSIndexPath indexPath)
        {
            if (indexPath.Section >= sectionItemAttributes.Count) {
                return null;
            }

            if (indexPath.Item >= sectionItemAttributes [indexPath.Section].Count) {
                return null;
            }

            var list = sectionItemAttributes [indexPath.Section];
            return list [(int)indexPath.Item];
        }

        public override UICollectionViewLayoutAttributes LayoutAttributesForSupplementaryView (NSString kind, NSIndexPath indexPath)
        {
            var attributes = new UICollectionViewLayoutAttributes ();

            switch (kind) {
            case "header":
                attributes = headersAttributes [indexPath.Section];
                break;
            case "footer":
                attributes = footersAttributes [indexPath.Section];
                break;
            }

            return attributes;
        }

        public override UICollectionViewLayoutAttributes[] LayoutAttributesForElementsInRect (CGRect rect)
        {
            var begin = 0;
            var end = unionRects.Count;
            List<UICollectionViewLayoutAttributes> attrs = new List<UICollectionViewLayoutAttributes> ();


            for (int i = 0; i < end; i++) {
                if (rect.IntersectsWith(unionRects[i])) {
                    begin = i * (int)unionSize;
                }
            }

            for (int i = end - 1; i >= 0; i--) {
                if (rect.IntersectsWith (unionRects [i])) {
                    end = (int)Math.Min ((i + 1) * (int)unionSize, allItemAttributes.Count);
                    break;
                }
            }

            for (int i = begin; i < end; i++) {
                var attr = allItemAttributes [i];
                if (rect.IntersectsWith (attr.Frame)) {
                    attrs.Add (attr);
                }
            }

            return attrs.ToArray();
        }

        public override bool ShouldInvalidateLayoutForBoundsChange (CGRect newBounds)
        {
            var oldBounds = CollectionView.Bounds;
            return (newBounds.Width != oldBounds.Width);
        }
        #endregion

        #region Private Methods
        private int ShortestColumnIndex() {
            var index = 0;
            var shortestHeight = nfloat.MaxValue;
            var n = 0;

            // Scan each column for the shortest height
            foreach (nfloat height in columnHeights) {
                if (height < shortestHeight) {
                    shortestHeight = height;
                    index = n;
                }
                ++n;
            }

            return index;
        }

        private int LongestColumnIndex() {
            var index = 0;
            var longestHeight = nfloat.MinValue;
            var n = 0;

            // Scan each column for the shortest height
            foreach (nfloat height in columnHeights) {
                if (height > longestHeight) {
                    longestHeight = height;
                    index = n;
                }
                ++n;
            }

            return index;
        }

        private int NextColumnIndexForItem(nint item) {
            var index = 0;

            switch (ItemRenderDirection) {
            case WaterfallCollectionRenderDirection.ShortestFirst:
                index = ShortestColumnIndex ();
                break;
            case WaterfallCollectionRenderDirection.LeftToRight:
                index = ColumnCount;
                break;
            case WaterfallCollectionRenderDirection.RightToLeft:
                index = (ColumnCount - 1) - ((int)item / ColumnCount);
                break;
            }

            return index;
        }
        #endregion

        #region Events
        public delegate CGSize WaterfallCollectionSizeDelegate(UICollectionView collectionView, WaterfallCollectionLayout layout, NSIndexPath indexPath);
        public delegate nfloat WaterfallCollectionFloatDelegate(UICollectionView collectionView, WaterfallCollectionLayout layout, nint section);
        public delegate UIEdgeInsets WaterfallCollectionEdgeInsetsDelegate(UICollectionView collectionView, WaterfallCollectionLayout layout, nint section);

        public event WaterfallCollectionSizeDelegate SizeForItem;
        public event WaterfallCollectionFloatDelegate HeightForHeader;
        public event WaterfallCollectionFloatDelegate HeightForFooter;
        public event WaterfallCollectionEdgeInsetsDelegate InsetForSection;
        public event WaterfallCollectionFloatDelegate MinimumInterItemSpacingForSection;
        #endregion
    }
}
```

Może to być umożliwiające klasy niestandardowej dwie kolumny, typu układu wykresu kaskadowego widok kolekcji.
W kodzie za pomocą kodowania klucz-wartość (za pośrednictwem `WillChangeValue` i `DidChangeValue` metody) ma zostać udostępnione wiązanie danych do naszej obliczone właściwości tej klasy.

Następnie Edytuj `WaterfallCollectionSource` i wprowadź następujące zmiany i uzupełnienia:

```csharp
private Random rnd = new Random();
...

public List<nfloat> Heights { get; set; } = new List<nfloat> ();
...

public WaterfallCollectionSource (WaterfallCollectionView collectionView)
{
    // Initialize
    CollectionView = collectionView;

    // Init numbers collection
    for (int n = 0; n < 100; ++n) {
        Numbers.Add (n);
        Heights.Add (rnd.Next (0, 100) + 40.0f);
    }
}
```

Spowoduje to utworzenie losowe wysokości dla poszczególnych elementów, które będą wyświetlane na liście.

Następnie Edytuj `WaterfallCollectionView` klasy i dodaj następującą właściwość pomocy:

```csharp
public WaterfallCollectionSource Source {
    get { return (WaterfallCollectionSource)DataSource; }
}
```

Ułatwi można odczytać niestandardowego układu naszych źródła danych (i wysokości elementu).

Na koniec Edytuj kontroler widoku i Dodaj następujący kod:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    var waterfallLayout = new WaterfallCollectionLayout ();

    // Wireup events
    waterfallLayout.SizeForItem += (collectionView, layout, indexPath) => {
        var collection = collectionView as WaterfallCollectionView;
        return new CGSize((View.Bounds.Width-40)/3,collection.Source.Heights[(int)indexPath.Item]);
    };

    // Attach the custom layout to the collection
    CollectionView.SetCollectionViewLayout(waterfallLayout, false);
}
```

Tworzy wystąpienie naszych niestandardowego układu, ustawia zdarzenie w celu udostępnienia rozmiar każdego elementu i dołącza nowy układ do naszej widok kolekcji.

Jeśli firma Microsoft uruchamianie aplikacji platformy Xamarin.iOS widok kolekcji będzie teraz wyglądać następująco:

[![](uicollectionview-images/custom01.png "Widok kolekcji będzie wyglądać tak jak to")](uicollectionview-images/custom01.png#lightbox)

Firma Microsoft może nadal przeciągnij do-zmiany kolejności elementów jako przed, ale elementy teraz zmieni rozmiar, aby dopasować nowej lokalizacji, gdy są one usuwane.

## <a name="collection-view-changes"></a>Przeglądanie zmian w kolekcji

W poniższych sekcjach przeniesiemy szczegółowe przyjrzeć się zmiany wprowadzone do każdej klasy w widoku kolekcji przez system iOS 9.

### <a name="uicollectionview"></a>UICollectionView

Wprowadzono następujące zmiany lub dodatki do `UICollectionView` klasy dla systemu iOS 9:

 - `BeginInteractiveMovementForItem` — Oznacza początek operacji przeciągania.
 - `CancelInteractiveMovement` — Informuje o kolekcji widoku użytkownik anulował operację przeciągania.
 - `EndInteractiveMovement` — Informuje o kolekcji widoku, że użytkownik zakończył operację przeciągania.
 - `GetIndexPathsForVisibleSupplementaryElements` — Zwraca `indexPath` nagłówka lub stopki w sekcji Widok kolekcji.
 - `GetSupplementaryView` — Zwraca danego nagłówku lub stopce strony.
 - `GetVisibleSupplementaryViews` — Zwraca listę wszystkich widoczne nagłówki i stopki.
 - `UpdateInteractiveMovementTargetPosition` — Informuje o kolekcji widoku użytkownik został przeniesiony lub jest przeniesienie elementu w czasie trwania operacji przeciągania.

### <a name="uicollectionviewcontroller"></a>UICollectionViewController

Wprowadzono następujące zmiany lub dodatki do `UICollectionViewController` klasy w systemie iOS 9:

 - `InstallsStandardGestureForInteractiveMovement` — Jeśli `true` nowy aparat rozpoznawania gestów, obsługujący automatyczne przeciągania do zmiany kolejności będą używane.
 - `CanMoveItem` — Informuje widok kolekcji, jeśli danego elementu można zmienić kolejności przeciągania.
 - `GetTargetContentOffset` — Używany do pobierania przesunięcie elementu widoku danej kolekcji.
 - `GetTargetIndexPathForMove` — Pobiera `indexPath` danego elementu dla operacji przeciągania.
 - `MoveItem` — Przenosi kolejność danego elementu na liście.


### <a name="uicollectionviewdatasource"></a>UICollectionViewDataSource

Wprowadzono następujące zmiany lub dodatki do `UICollectionViewDataSource` klasy w systemie iOS 9:

 - `CanMoveItem` — Informuje widok kolekcji, jeśli danego elementu można zmienić kolejności przeciągania.
 - `MoveItem` — Przenosi kolejność danego elementu na liście.

### <a name="uicollectionviewdelegate"></a>UICollectionViewDelegate

Wprowadzono następujące zmiany lub dodatki do `UICollectionViewDelegate` klasy w systemie iOS 9:

 - `GetTargetContentOffset` — Używany do pobierania przesunięcie elementu widoku danej kolekcji.
 - `GetTargetIndexPathForMove` — Pobiera `indexPath` danego elementu dla operacji przeciągania.

### <a name="uicollectionviewflowlayout"></a>UICollectionViewFlowLayout

Wprowadzono następujące zmiany lub dodatki do `UICollectionViewFlowLayout` klasy w systemie iOS 9:

 - `SectionFootersPinToVisibleBounds` — Systemu stopki sekcji do kolekcji widoczne granice widoku.
 - `SectionHeadersPinToVisibleBounds` — Nagłówki sekcji do kolekcji widoczne granice widoku systemu.

### <a name="uicollectionviewlayout"></a>UICollectionViewLayout

Wprowadzono następujące zmiany lub dodatki do `UICollectionViewLayout` klasy w systemie iOS 9:

 - `GetInvalidationContextForEndingInteractiveMovementOfItems` — Zwraca kontekst unieważniania pod koniec operacji przeciągania, gdy użytkownik zakończy przeciągania lub anuluje go.
 - `GetInvalidationContextForInteractivelyMovingItems` — Zwraca kontekst unieważniania w chwili rozpoczęcia operacji przeciągania.
 - `GetLayoutAttributesForInteractivelyMovingItem` — Pobiera atrybuty układu dla danego elementu podczas przeciągania elementu.
 - `GetTargetIndexPathForInteractivelyMovingItem` — Zwraca `indexPath` elementu, który jest w danym momencie, gdy przeciąganie elementu.

### <a name="uicollectionviewlayoutattributes"></a>UICollectionViewLayoutAttributes

Wprowadzono następujące zmiany lub dodatki do `UICollectionViewLayoutAttributes` klasy w systemie iOS 9:

 - `CollisionBoundingPath` — Zwraca ścieżkę kolizji dwóch elementów podczas operacji przeciągania.
 - `CollisionBoundsType` — Zwraca typ kolizji (jako `UIDynamicItemCollisionBoundsType`) podczas operacji przeciągania wystąpił.

### <a name="uicollectionviewlayoutinvalidationcontext"></a>UICollectionViewLayoutInvalidationContext

Wprowadzono następujące zmiany lub dodatki do `UICollectionViewLayoutInvalidationContext` klasy w systemie iOS 9:

 - `InteractiveMovementTarget` — Zwraca element docelowy operacji przeciągania.
 - `PreviousIndexPathsForInteractivelyMovingItems` — Zwraca `indexPaths` innych elementów objętych przeciągnij, aby zmienić kolejność operacji.
 - `TargetIndexPathsForInteractivelyMovingItems` — Zwraca `indexPaths` elementów, które będzie można zmienić kolejności, w wyniku operacji przeciągania do zmiany kolejności.

### <a name="uicollectionviewsource"></a>UICollectionViewSource

Wprowadzono następujące zmiany lub dodatki do `UICollectionViewSource` klasy w systemie iOS 9:

 - `CanMoveItem` — Informuje widok kolekcji, jeśli danego elementu można zmienić kolejności przeciągania.
 - `GetTargetContentOffset` — Zwraca przesunięcia elementów, które ma zostać przeniesiona przy użyciu operacji przeciągania do zmiany kolejności.
 - `GetTargetIndexPathForMove` — Zwraca `indexPath` elementu, który zostanie przeniesiony podczas operacji przeciągania do zmiany kolejności.
 - `MoveItem` — Przenosi kolejność danego elementu na liście.

## <a name="summary"></a>Podsumowanie

W tym artykule ma objętych zmiany kolekcji widoków dla systemu iOS 9 i opisano sposób ich wdrażania w platformy Xamarin.iOS.
Omówione, implementowanie prostego akcji przeciągania do zmiany kolejności w widoku kolekcji; Używanie niestandardowych aparat rozpoznawania gestów z przeciągnij do-zmiany kolejności; i wpływ układ widoku kolekcji niestandardowej przeciągania do zmiany kolejności.

## <a name="related-links"></a>Linki pokrewne

- [Przykłady z systemem iOS 9](https://developer.xamarin.com/samples/ios/iOS9/)
- [Przykładowy widok kolekcji](https://developer.xamarin.com/samples/monotouch/ios9/CollectionView/)
- [SimpleCollectionView (przykład)](https://developer.xamarin.com/samples/SimpleCollectionView/)
- [Zdarzenia, protokoły i delegaci](~/ios/app-fundamentals/delegates-protocols-and-events.md)
- [Praca z tabelami i komórek](~/ios/user-interface/controls/tables/index.md)
