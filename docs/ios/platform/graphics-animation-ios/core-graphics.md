---
title: Grafika Core w Xamarin.iOS
description: W tym artykule omówiono podstawowe grafiki struktury systemu iOS. Widoczny jest sposób umożliwia grafiki Core Rysuj geometrię, obrazów i plików PDF.
ms.prod: xamarin
ms.assetid: 4A30F480-0723-4B8A-9049-7CEB6211304A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 7d7124c7d09ca4e36ce22d60f578ea4a75d4a05b
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34786759"
---
# <a name="core-graphics-in-xamarinios"></a>Grafika Core w Xamarin.iOS

_W tym artykule omówiono podstawowe grafiki struktury systemu iOS. Widoczny jest sposób umożliwia grafiki Core Rysuj geometrię, obrazów i plików PDF._

obejmuje iOS [ *grafiki Core* ](https://developer.apple.com/library/prerelease/ios/documentation/CoreGraphics/Reference/CoreGraphics_Framework/index.html) framework, aby zapewnić obsługę rysowania niskiego poziomu. Te struktury są co Włącz bogate możliwości graficznego w UIKit. 

Grafika Core jest struktury grafiki 2D niskiego poziomu, która umożliwia rysowania grafiki niezależnie od urządzenia. Wszystkie 2D Rysowanie w UIKit jest używana wewnętrznie grafiki Core.

Grafika Core obsługuje Rysowanie w wielu scenariuszach, w tym:

-  [Rysowanie na ekranie za pośrednictwem `UIView` ](#Drawing_in_a_UIView_Subclass) .
-  [Rysowanie obrazów w pamięci lub na ekranie](#Drawing_Images_and_Text).
-  Tworzenie i rysowanie w pliku PDF.
-  Odczytywanie i rysowanie istniejącego pliku PDF.


## <a name="geometric-space"></a>Geometrycznych miejsca

Niezależnie od tego, w tym scenariuszu wszystkie rysunku odbywa się za pomocą grafiki Core odbywa się przestrzeni geometryczne, co oznacza, że działa on w abstrakcyjny punktów, a nie w pikselach. Opisz, co ma narysowanego pod względem geometrii i rysowanie stanu, takich jak kolory, style linii, itp., i grafiki Core obsługuje tłumaczenia wszystko w pikselach. Taki stan jest dodawany do kontekstu grafiki, które możesz traktować jak Malarz kanwy.

Istnieje kilka zalety tego podejścia:

-  Kod rysowania staje się dynamicznej, a następnie zmodyfikować grafiki w czasie wykonywania.
-  Zmniejsza potrzebę statycznych obrazów w pakiecie aplikacji można zmniejszyć rozmiar aplikacji.
-  Grafika staną się bardziej odporne na zmiany rozpoznawania na urządzeniach.

<a name="Drawing_in_a_UIView_Subclass"/>

## <a name="drawing-in-a-uiview-subclass"></a>Rysowanie w podklasy UIView

Każdy `UIView` ma `Draw` metodę, która jest wywoływana przez system, kiedy zachodzi potrzeba narysowania. Aby dodać kod rysowania widoku podklasy `UIView` i zastąpić `Draw`:

```csharp
public class TriangleView : UIView
{
    public override void Draw (CGRect rect)
    {
        base.Draw (rect);
    }
}
```

Rysuj nigdy nie powinna być wywoływana bezpośrednio. Jest ona wywoływana przez system podczas przetwarzania wykonywania pętli. Po raz pierwszy przez wykonywania pętli po dodaniu widoku do hierarchii widoku jego `Draw` metoda jest wywoływana. Kolejne wywołania `Draw` wystąpić, gdy widok jest oznaczony jako wymagające narysowania przez wywołanie albo `SetNeedsDisplay` lub `SetNeedsDisplayInRect` w widoku.

### <a name="pattern-for-graphics-code"></a>Wzorzec kodu grafiki

Kod w `Draw` implementacji powinien opisano, co chce narysowanego. Kod rysowania jest zgodna ze wzorcem, w którym ustawia stan niektórych i wywołuje metodę, aby zażądać go narysowania. Ten wzorzec może być uogólniony w następujący sposób:

1. Pobiera kontekst grafiki.

2. Skonfiguruj atrybuty rysowania.

3. Utwórz niektórych geometrii rysowanie elementów podstawowych.

4. Wywołanie metody rysunku lub Stroke.

### <a name="basic-drawing-example"></a>Przykład podstawowego rysowania

Rozważmy na przykład poniższy fragment kodu:

```csharp
//get graphics context
using (CGContext g = UIGraphics.GetCurrentContext ()) {
            
    //set up drawing attributes
    g.SetLineWidth (10);
    UIColor.Blue.SetFill ();
    UIColor.Red.SetStroke ();

    //create geometry
    var path = new CGPath ();

    path.AddLines (new CGPoint[]{
    new CGPoint (100, 200),
    new CGPoint (160, 100), 
    new CGPoint (220, 200)});

    path.CloseSubpath ();

    //add geometry to graphics context and draw it
    g.AddPath (path);       
    g.DrawPath (CGPathDrawingMode.FillStroke);
}
```

Umożliwia podział ten kod:

```csharp
using (CGContext g = UIGraphics.GetCurrentContext ()) {
...
}
```
Z tego wiersza najpierw pobiera bieżący kontekst grafiki do użycia na rysunku. Można traktować kontekstu grafiki jako obszaru roboczego czy rysowanie sytuacji, zawierający wszystkie stanu o rysunku, takich jak obrysu i wypełnienia kolorów, a także geometrii do rysowania.

```csharp
g.SetLineWidth (10);
UIColor.Blue.SetFill ();
UIColor.Red.SetStroke ();
``` 

Po pierwsze kontekstu grafiki kod konfiguruje niektóre atrybuty do użycia podczas rysowania przedstawionych powyżej. W takim przypadku kolorów linii szerokości, obrysu i wypełnienia są ustawione. Ponieważ są one obsługiwane w kontekście grafiki stan dowolnego kolejnych rysunku będzie używać tych atrybutów.

Aby utworzyć geometrii kod używa `CGPath`, dzięki czemu ścieżki grafiki do opisania z linii i krzywych. W takim przypadku ścieżka dodaje linie łączące tablicy punktów tworzą trójkąt. Wyświetlone poniżej używa grafiki Core współrzędnych Rysowanie w widoku, gdzie źródła znajduje się w lewym górnym rogu, dodatnią x-bezpośrednio w prawo i w kierunku y dodatnie w dół:

```csharp
var path = new CGPath ();

path.AddLines (new CGPoint[]{
new CGPoint (100, 200),
new CGPoint (160, 100), 
new CGPoint (220, 200)});

path.CloseSubpath ();
``` 

Po utworzeniu ścieżki, jest ona dodawana do kontekstu grafiki, tak, aby podczas wywoływania `AddPath` i `DrawPath` odpowiednio można rysowania.

Poniżej przedstawiono uzyskany widok:

 ![](core-graphics-images/00-bluetriangle.png "Trójkąt przykładowe dane wyjściowe")

## <a name="creating-gradient-fills"></a>Tworzenie gradientu wypełnienia

Dostępne są także bardziej rozbudowane formularze rysunku. Na przykład grafiki Core umożliwia tworzenie gradientem i stosowanie ścieżki przycinania. Rysowanie wypełnienia gradientowego wewnątrz ścieżki z poprzedniego przykładu, najpierw ścieżka musi być ustawiona jako ścieżka wycinka:

```csharp
// add the path back to the graphics context so that it is the current path
g.AddPath (path);
// set the current path to be the clipping path
g.Clip ();
```

Ustawienia bieżącej ścieżki, ponieważ ścieżka wycinka ogranicza wszystkie kolejne rysunku w obrębie geometrii ścieżki, na przykład następujący kod, który pobiera gradientu liniowego:

```csharp
// the color space determines how Core Graphics interprets color information
    using (CGColorSpace rgb = CGColorSpace.CreateDeviceRGB()) {
        CGGradient gradient = new CGGradient (rgb, new CGColor[] {
        UIColor.Blue.CGColor,
        UIColor.Yellow.CGColor
    });

// draw a linear gradient
    g.DrawLinearGradient (
        gradient, 
        new CGPoint (path.BoundingBox.Left, path.BoundingBox.Top), 
        new CGPoint (path.BoundingBox.Right, path.BoundingBox.Bottom), 
        CGGradientDrawingOptions.DrawsBeforeStartLocation);
    }
```

Te zmiany przedstawienia wypełnienia gradientowego, jak pokazano poniżej:

 ![](core-graphics-images/01-gradient-fill.png "Przykład z wypełnieniem gradientowym")

## <a name="modifying-line-patterns"></a>Modyfikowanie wzorce wiersza

Atrybuty rysowania linii może również zostać zmieniony z grafiką Core. Obejmuje to zmianę szerokości i obrysu kolor linii, a także wzorzec wierszy, jak pokazano w poniższym kodzie:

```csharp
//use a dashed line
g.SetLineDash (0, new nfloat[] { 10, 4 * (nfloat)Math.PI });
```

Dodawanie ten kod przed rysowania wyników operacji w jednostkach kreskowane pociągnięć 10 długi, 4 jednostki odstępów między łączniki, jak pokazano poniżej:

 ![](core-graphics-images/02-dashed-stroke.png "Dodawanie ten kod przed rysowania wyników operacji w pociągnięć kreskowanej")
 
Należy pamiętać, że korzystając z interfejsu API Unified w Xamarin.iOS typu tablicy musi być `nfloat`i musi być jawnie rzutowany Math.pi —.

<a name="Drawing_Images_and_Text"/>

## <a name="drawing-images-and-text"></a>Rysowanie tekstu i grafiki

Oprócz Rysowanie ścieżek w kontekście grafiki widoku, grafiki Core obsługuje również rysowania obrazy i tekst. Rysowanie obrazów, po prostu utworzyć `CGImage` i przekaż go do `DrawImage` wywołania:

```csharp
public override void Draw (CGRect rect)
{
    base.Draw (rect);

    using(CGContext g = UIGraphics.GetCurrentContext ()){
        g.DrawImage (rect, UIImage.FromFile ("MyImage.png").CGImage);
    }
}
```

Jednak to tworzy obraz rysowane odwrócony, jak pokazano poniżej:

 ![](core-graphics-images/03-upside-down-monkey.png "Obraz rysowane wokół osi poziomej")

Przyczyną tego jest punkt początkowy grafiki Core do rysowania obrazu podczas znajduje się w lewym dolnym widoku ma swoją witryną źródłową w lewym górnym rogu. W związku z tym do poprawnego wyświetlania obrazu źródła ma zostać zmodyfikowana, co można wykonać przez zmodyfikowanie *bieżącego macierzy transformacji* *(CTM)*. CTM definiuje, gdzie punktów na żywo, znanej także jako *przestrzeni*. Odwracanie CTM w kierunku y i przesuwając go przez granice wysokość w kierunku y ujemna można Przerzucanie obrazu.

Kontekst grafiki ma metody pomocnicze do przekształcania CTM. W takim przypadku `ScaleCTM` "odwraca" Rysowanie i `TranslateCTM` przenosi do lewego górnego, jak pokazano poniżej:

```csharp
public override void Draw (CGRect rect)
{
    base.Draw (rect);
    
    using (CGContext g = UIGraphics.GetCurrentContext ()) {

        // scale and translate the CTM so the image appears upright
        g.ScaleCTM (1, -1);
        g.TranslateCTM (0, -Bounds.Height);
        g.DrawImage (rect, UIImage.FromFile ("MyImage.png").CGImage);
}   
```

Następnie jest wyświetlany obraz wynikowy pionowo:

 ![](core-graphics-images/04-upright-monkey.png "Obraz wyświetlany słupka próbki")

> [!IMPORTANT]
> Zmiany w kontekście grafiki zastosowane do wszystkich kolejnych operacji rysowania. W związku z tym gdy jest on przekształcany CTM, ma wpływ na wszystkie dodatkowe rysunku. Na przykład jeśli został narysowany trójkąt po przekształceniu CTM, wydaje się odwrócony.

### <a name="adding-text-to-the-image"></a>Dodawanie tekstu do obrazu

Jako przy użyciu ścieżek i obrazów, rysowanie tekstu z grafiką Core wymaga tego samego wzorca podstawowe ustawienia niektórych stanu grafiki i wywołanie metody do rysowania. W przypadku tekstu, metoda do wyświetlania tekstu jest `ShowText`. Po dodaniu do rysowania przykład obrazu, poniższy kod rysuje tekst przy użyciu grafiki rdzeni:

```csharp
public override void Draw (RectangleF rect)
{
    base.Draw (rect);
    
    // image drawing code omitted for brevity ...

    // translate the CTM by the font size so it displays on screen
    float fontSize = 35f;
    g.TranslateCTM (0, fontSize);

    // set general-purpose graphics state
    g.SetLineWidth (1.0f);
    g.SetStrokeColor (UIColor.Yellow.CGColor);
    g.SetFillColor (UIColor.Red.CGColor);
    g.SetShadow (new CGSize (5, 5), 0, UIColor.Blue.CGColor);

    // set text specific graphics state
    g.SetTextDrawingMode (CGTextDrawingMode.FillStroke);
    g.SelectFont ("Helvetica", fontSize, CGTextEncoding.MacRoman);

    // show the text
    g.ShowText ("Hello Core Graphics");
}
```

Jak widać, ustawiania stanu grafiki Rysowanie tekstu jest podobny do rysowania geometrii. Tekst rysowania jednak, również są stosowane rysowania trybu i czcionkę tekstu. W takim przypadku cienia jest również zastosować, mimo że zastosowanie shadows działa tak samo dla ścieżki rysowania.

Wynikowa tekst jest wyświetlany w obrazie, jak pokazano poniżej:

 ![](core-graphics-images/05-text-on-image.png "Tekst wynikowy jest wyświetlany w obrazie")

## <a name="memory-backed-images"></a>Obrazy z kopią zapasową w pamięci

Oprócz rysowania kontekst grafiki widoku, obsługuje grafiki Core rysowania pamięci kopii obrazów, nazywane również rysowania ekranem. Wymaga:

-  Tworzenie kontekstu grafiki, który nie jest obsługiwana przez pamięci mapy bitowej
-  Ustawianie stanu rysowania i wydawania polecenia rysowania
-  Pobieranie obrazu z kontekstu
-  Usuwanie kontekstu


W odróżnieniu od `Draw` metody, w przypadku, gdy kontekst jest dostarczana przez widok, w tym przypadku tworzenia kontekstu w jeden z dwóch sposobów:

1. Wywołując `UIGraphics.BeginImageContext` (lub `BeginImageContextWithOptions`)

2. Tworząc nowy `CGBitmapContextInstance`

 `CGBitmapContextInstance` jest przydatne podczas pracy bezpośrednio z bitami obrazu, takie jak w przypadku gdy używasz algorytmu manipulowania niestandardowego obrazu. We wszystkich innych przypadkach należy używać `BeginImageContext` lub `BeginImageContextWithOptions`.

Po utworzeniu kontekstu obrazu, dodawanie rysowania kodu jest tak jak w `UIView` podklasy. Na przykład przykładowy kod wcześniej używany do rysowania trójkąt może służyć do rysowania do obrazu w pamięci, a nie w `UIView`, jak pokazano poniżej:

```csharp
UIImage DrawTriangle ()
{
    UIImage triangleImage;

    //push a memory backed bitmap context on the context stack
    UIGraphics.BeginImageContext (new CGSize (200.0f, 200.0f));

    //get graphics context
    using(CGContext g = UIGraphics.GetCurrentContext ()){

        //set up drawing attributes
        g.SetLineWidth(4);
        UIColor.Purple.SetFill ();
        UIColor.Black.SetStroke ();

        //create geometry
        path = new CGPath ();

        path.AddLines(new CGPoint[]{
            new CGPoint(100,200),
            new CGPoint(160,100), 
            new CGPoint(220,200)});

        path.CloseSubpath();

        //add geometry to graphics context and draw it
        g.AddPath(path);
        g.DrawPath(CGPathDrawingMode.FillStroke);

        //get a UIImage from the context
        triangleImage = UIGraphics.GetImageFromCurrentImageContext ();
    }
    
    return triangleImage;
}
```

Zazwyczaj rysunku do mapy bitowej kopii pamięci jest używane do przechwycenia obrazu za pomocą dowolnego `UIView`. Na przykład następujący kod renderuje widok warstwy do kontekstu mapy bitowej i tworzy `UIImage` z niej:

```csharp
UIGraphics.BeginImageContext (cellView.Frame.Size);

//render the view's layer in the current context
anyView.Layer.RenderInContext (UIGraphics.GetCurrentContext ());

//get a UIImage from the context
UIImage anyViewImage = UIGraphics.GetImageFromCurrentImageContext ();
UIGraphics.EndImageContext ();
```

## <a name="drawing-pdfs"></a>Rysowanie plików PDF

Oprócz obrazów grafiki Core obsługuje rysunku PDF. Podobnie jak obrazy, można renderowanie plików PDF w pamięci oraz jak odczytu dokumentu PDF do renderowania w `UIView`.

### <a name="pdf-in-a-uiview"></a>Plik PDF w UIView

Grafika Core obsługuje również odczyt dokumentu PDF z pliku i renderowaniem go w widoku przy użyciu `CGPDFDocument` klasy. `CGPDFDocument` Klasa reprezentuje PDF w kodzie i może służyć do odczytu i rysowanie stron.

Na przykład poniższy kod w `UIView` podklasy odczytuje plik PDF z pliku do `CGPDFDocument`:

```csharp
public class PDFView : UIView
{
    CGPDFDocument pdfDoc;

    public PDFView ()
    {
        //create a CGPDFDocument from file.pdf included in the main bundle
        pdfDoc = CGPDFDocument.FromFile ("file.pdf");
    }
  
     public override void Draw (Rectangle rect)
    {
        ...
    }
}
```

`Draw` Metoda może następnie używać `CGPDFDocument` odczytać strony do `CGPDFPage` i renderować ją przez wywołanie metody `DrawPDFPage`, jak pokazano poniżej:

```csharp
public override void Draw (CGRect rect)
{
    base.Draw (rect);
        
    //flip the CTM so the PDF will be drawn upright
    using (CGContext g = UIGraphics.GetCurrentContext ()) {
        g.TranslateCTM (0, Bounds.Height);
        g.ScaleCTM (1, -1);
        
        // render the first page of the PDF
        using (CGPDFPage pdfPage = pdfDoc.GetPage (1)) {
            
        //get the affine transform that defines where the PDF is drawn
        CGAffineTransform t = pdfPage.GetDrawingTransform (CGPDFBox.Crop, rect, 0, true);
        
        //concatenate the pdf transform with the CTM for display in the view
        g.ConcatCTM (t);
        
        //draw the pdf page
        g.DrawPDFPage (pdfPage);
        }
    }
}
```

### <a name="memory-backed-pdf"></a>Kopia pamięci PDF

PDF w pamięci, należy utworzyć w kontekście PDF przez wywołanie metody `BeginPDFContext`. Rysunek PDF jest szczegółowego do stron. Każdej z nich jest uruchomiona przez wywołanie metody `BeginPDFPage` oraz zostać zakończona, wywołując `EndPDFContent`, z grafiki między kodem. Ponadto z rysunkiem obrazu pamięci wykonania kopii PDF rysowania używa początek w lewym dolnym można wyjaśnić, zmieniając po prostu CTM, takich jak z obrazami.

Poniższy kod przedstawia sposób rysowania tekstu do pliku PDF:

```csharp
//data buffer to hold the PDF
NSMutableData data = new NSMutableData ();

//create a PDF with empty rectangle, which will configure it for 8.5x11 inches
UIGraphics.BeginPDFContext (data, CGRect.Empty, null);

//start a PDF page
UIGraphics.BeginPDFPage ();
       
using (CGContext g = UIGraphics.GetCurrentContext ()) {
    g.ScaleCTM (1, -1);
    g.TranslateCTM (0, -25);      
    g.SelectFont ("Helvetica", 25, CGTextEncoding.MacRoman);
    g.ShowText ("Hello Core Graphics");
    }
    
//complete a PDF page
UIGraphics.EndPDFContent ();
```

Tekstu jest rysowana na PDF, które następnie są zawarte w `NSData` który może zostać zapisany, przekazanego, przesłanego itp.


## <a name="summary"></a>Podsumowanie

W tym artykule analizujemy dostępnych za pośrednictwem możliwości grafiki *grafiki Core* framework. Widzieliśmy Rysuj geometrię, obrazów i plików PDF w kontekście przy użyciu grafiki Core `UIView,` oraz jak do kontekstów kopii pamięci grafiki.

## <a name="related-links"></a>Linki pokrewne

- [Przykład grafiki Core](https://developer.xamarin.com/samples/monotouch/GraphicsAndAnimation/)
- [Grafika i wskazówki animacji](~/ios/platform/graphics-animation-ios/graphics-animation-walkthrough.md)
- [Podstawowe funkcje animacji](~/ios/platform/graphics-animation-ios/core-animation.md)
- [Podstawowe przepisami animacji](https://developer.xamarin.com/recipes/ios/animation/coreanimation)
