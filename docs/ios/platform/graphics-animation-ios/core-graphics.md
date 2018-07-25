---
title: Podstawowe funkcje grafiki w rozszerzeniu Xamarin.iOS
description: W tym artykule omówiono podstawowe funkcje grafiki struktury systemu iOS. Widoczny jest sposób użycia podstawowe funkcje grafiki do rysowania geometrii, obrazów i plików PDF.
ms.prod: xamarin
ms.assetid: 4A30F480-0723-4B8A-9049-7CEB6211304A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 82c54074db722824c56ae3ae86620c804b8d109e
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241952"
---
# <a name="core-graphics-in-xamarinios"></a>Podstawowe funkcje grafiki w rozszerzeniu Xamarin.iOS

_W tym artykule omówiono podstawowe funkcje grafiki struktury systemu iOS. Widoczny jest sposób użycia podstawowe funkcje grafiki do rysowania geometrii, obrazów i plików PDF._

dla systemu iOS zawiera [ *podstawowe funkcje grafiki* ](https://developer.apple.com/library/prerelease/ios/documentation/CoreGraphics/Reference/CoreGraphics_Framework/index.html) framework w celu zapewnienia obsługi rysowania niskiego poziomu. Te struktury są co Włączanie zaawansowanych funkcji graficznych w ramach UIKit. 

Podstawowe funkcje grafiki jest struktury grafika 2D niskiego poziomu, która umożliwia rysowanie grafiki niezależnie od urządzenia. Wszystkie 2D rysowania w strukturze UIKit jest używane wewnętrznie podstawowe funkcje grafiki.

Podstawowe funkcje grafiki obsługuje Rysowanie w wielu scenariuszach, w tym:

-  [Rysowanie na ekranie za pomocą `UIView` ](#Drawing_in_a_UIView_Subclass) .
-  [Rysowanie obrazów w pamięci lub na ekranie](#Drawing_Images_and_Text).
-  Tworzenie i rysowanie w pliku PDF.
-  Odczytywanie i rysowanie istniejącego pliku PDF.


## <a name="geometric-space"></a>Miejsce geometryczne

Niezależnie od tego, w tym scenariuszu wszystkie rysunku zrobić za pomocą podstawowe funkcje grafiki odbywa się w przestrzeni adresowej geometryczne, co oznacza, że działa on w abstrakcyjny punktów, a nie pikseli. Opisz, co ma rysowane pod względem geometry i rysowanie stan, takich jak kolory, style linii, itp., a podstawowe funkcje grafiki zajmuje się tłumaczenia wszystko w pikselach. Taki stan jest dodawany do kontekście grafiki można traktować jak Malarz kanwy.

Istnieje kilka zalet tego podejścia:

-  Kodu rysowania staje się dynamiczne, a następnie zmodyfikować grafiki w czasie wykonywania.
-  Zmniejszając konieczność stosowania obrazów statycznych do zbioru aplikacji może zmniejszyć rozmiar aplikacji.
-  Grafiki stają się bardziej odporne na zmiany rozwiązania na urządzeniach.

<a name="Drawing_in_a_UIView_Subclass"/>

## <a name="drawing-in-a-uiview-subclass"></a>Rysowanie w podklasę UIView

Każdy `UIView` ma `Draw` metodę, która jest wywoływana przez system, gdy musi zostać narysowany. Aby dodać kod rysowania do widoku podklasy `UIView` i zastąpić `Draw`:

```csharp
public class TriangleView : UIView
{
    public override void Draw (CGRect rect)
    {
        base.Draw (rect);
    }
}
```

Rysowanie nigdy nie powinna być wywoływana bezpośrednio. Jest ona wywoływana przez system podczas przetwarzania wykonywania pętli. Po raz pierwszy przy użyciu wykonywania pętli po dodaniu do hierarchii widoków, widok jego `Draw` metoda jest wywoływana. Kolejne wywołania `Draw` występują, gdy widok jest oznaczony jako wymagające do rysowania, wywołując jedną `SetNeedsDisplay` lub `SetNeedsDisplayInRect` w widoku.

### <a name="pattern-for-graphics-code"></a>Wzorzec dla kodu grafiki

Kod w `Draw` implementacji powinna opisywać, co chce rysowane. Kod rysowania jest zgodna ze wzorcem, w którym ustawia niektóre stan rysowania i wywołuje metodę, aby zażądać jej rysowania. Ten wzorzec może być uogólniony w następujący sposób:

1. Pobierz kontekst grafiki.

2. Skonfiguruj atrybuty rysowania.

3. Tworzenie geometrii niektóre z Rysowanie w nim elementów podstawowych.

4. Wywołaj metodę obrysu i rysowania.

### <a name="basic-drawing-example"></a>Przykład podstawowe Rysowanie

Na przykład rozważmy następujący fragment kodu:

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

Możemy podzielić ten kod:

```csharp
using (CGContext g = UIGraphics.GetCurrentContext ()) {
...
}
```
Z tego wiersza najpierw pobiera bieżący kontekst grafiki do użycia do rysowania. Można traktować kontekstu grafiki jako obszar roboczy, rysowania się dzieje w, zawierający wszystkie stany o rysunku, takich jak obrysu i koloru wypełnienia, a także typy geometryczne, aby narysować.

```csharp
g.SetLineWidth (10);
UIColor.Blue.SetFill ();
UIColor.Red.SetStroke ();
``` 

Po otrzymaniu kontekstu grafiki kod konfiguruje niektóre atrybuty do użycia podczas rysowania, jak pokazano powyżej. W tym przypadku są ustawione kolory linii szerokości, obrysu i wypełnienia. Wszystkie kolejne rysowania będzie używać tych atrybutów, ponieważ są one obsługiwane w stanie kontekst grafiki.

Aby utworzyć geometrii kod używa `CGPath`, co pozwala ścieżki grafiki do opisania z linii i krzywych. W takim przypadku ścieżka dodaje łączącymi tablica punktów tworzą trójkąt. Wyświetlone poniżej podstawowe funkcje grafiki używa współrzędnych dla widoku rysowania, której źródłem jest w lewym górnym rogu dodatnie x-bezpośrednio po prawej stronie i kierunku y dodatnie w dół:

```csharp
var path = new CGPath ();

path.AddLines (new CGPoint[]{
new CGPoint (100, 200),
new CGPoint (160, 100), 
new CGPoint (220, 200)});

path.CloseSubpath ();
``` 

Po utworzeniu ścieżki jest dodawany do kontekstu grafiki, aby podczas wywoływania `AddPath` i `DrawPath` odpowiednio ją rysować.

Poniżej przedstawiono wynikowym widoku:

 ![](core-graphics-images/00-bluetriangle.png "Przykładowe dane wyjściowe trójkąt")

## <a name="creating-gradient-fills"></a>Tworzenie gradientu wypełnienia

Dostępne są również bardziej rozbudowane rodzaje rysowania. Podstawowe funkcje grafiki umożliwia na przykład tworząc gradientem i stosując ścieżki przycinania. Aby narysować wypełnienia gradientowego wewnątrz ścieżki z poprzedniego przykładu, najpierw ścieżka musi być ustawiona jako ścieżki przycięcia:

```csharp
// add the path back to the graphics context so that it is the current path
g.AddPath (path);
// set the current path to be the clipping path
g.Clip ();
```

Ustawienia bieżącej ścieżki, ponieważ przycinania ogranicza wszystkie kolejne rysowania w obrębie geometrii ścieżkę, taką jak poniższy kod, który rysuje gradientu liniowego:

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

Te zmiany powodują utworzenie wypełniacza gradientu, jak pokazano poniżej:

 ![](core-graphics-images/01-gradient-fill.png "Przykład z wypełnieniem gradientowym")

## <a name="modifying-line-patterns"></a>Modyfikowanie wzorców wiersza

Atrybuty rysowania linii można zmodyfikować w taki sposób, przy użyciu podstawowe funkcje grafiki. Obejmuje to zmianę koloru linii szerokości i obrysu, a także wzorzec wiersza, jak pokazano w poniższym kodzie:

```csharp
//use a dashed line
g.SetLineDash (0, new nfloat[] { 10, 4 * (nfloat)Math.PI });
```

Dodawanie ten kod przed żadnych rysowania wyników operacji w jednostkach pociągnięć przerywaną, co 10 długa, za pomocą 4 jednostek odstępy między kresek, jak pokazano poniżej:

 ![](core-graphics-images/02-dashed-stroke.png "Dodawanie ten kod przed żadnych rysowania wyników operacji w pociągnięć kreskowanej")
 
Należy pamiętać, że podczas korzystania z interfejsu API Unified w rozszerzeniu Xamarin.iOS, typ tablicy musi być `nfloat`, a także musi być jawnie rzutowane na Math.PI.

<a name="Drawing_Images_and_Text"/>

## <a name="drawing-images-and-text"></a>Rysowanie obrazów i tekstu

Oprócz rysowania ścieżek w kontekście grafiki widoku, podstawowe funkcje grafiki obsługuje również rysowanie obrazów i tekstu. Aby narysować obrazu, po prostu Utwórz `CGImage` i przekazać ją do `DrawImage` wywołania:

```csharp
public override void Draw (CGRect rect)
{
    base.Draw (rect);

    using(CGContext g = UIGraphics.GetCurrentContext ()){
        g.DrawImage (rect, UIImage.FromFile ("MyImage.png").CGImage);
    }
}
```

Jednak to tworzy obraz rysowane nogami, jak pokazano poniżej:

 ![](core-graphics-images/03-upside-down-monkey.png "Obraz rysowane wokół osi poziomej")

Przyczyną jest podstawowe funkcje grafiki do rysowania obraz pochodzi z lewym dolnym rogu, natomiast widok pochodzenia w lewym górnym rogu. W związku z tym, aby poprawnie wyświetlać obraz, pochodzenie ma zostać zmodyfikowana, który można osiągnąć, modyfikując *bieżącego macierzy transformacji* *(CTM)*. CTM Określa, gdzie punkty na żywo, znany także jako *przestrzeni*. Odwracanie CTM w kierunku y i przesuwając go przez granice wysokość w kierunku ujemne y można przerzucić obraz.

Kontekst grafiki zawiera metody pomocnicze do przekształcania CTM. W tym przypadku `ScaleCTM` "odwraca" rysunku i `TranslateCTM` przenosi do lewego górnego rogu, jak pokazano poniżej:

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

Obraz wynikowy zostanie wyświetlona pionowo:

 ![](core-graphics-images/04-upright-monkey.png "Przykładowy obraz wyświetlany słupka")

> [!IMPORTANT]
> Zmiany do kontekstu grafiki stosowane do wszystkich kolejnych operacji rysowania. W związku z tym gdy jest on przekształcany CTM, ma wpływ na wszelkie dodatkowe rysowania. Na przykład jeśli trójkąt narysowany po przekształceniu CTM, wydaje się odwrócona.

### <a name="adding-text-to-the-image"></a>Dodawanie tekstu do obrazu

Jako przy użyciu ścieżek i obrazów, rysowanie tekstu za pomocą podstawowe funkcje grafiki polega na tym samym podstawowy wzorzec pewnego stanu grafiki i wywołanie metody do rysowania. W przypadku tekstu metodą do wyświetlania tekstu jest `ShowText`. Po dodaniu do obrazów, rysowania przykład, poniższy kod rysuje tekst przy użyciu podstawowe funkcje grafiki:

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

Jak widać, ustawiania stanu graficznych do rysowania tekstu jest podobny do rysowania geometrii. Do celów rysowania jednak tekstu, również są stosowane rysowania, tryb i czcionkę tekstu. W takim przypadku w tle jest również zastosowane, mimo że zastosowanie cieni działa tak samo, dla ścieżki rysowania.

Wynikowy tekst jest wyświetlany z obrazem, jak pokazano poniżej:

 ![](core-graphics-images/05-text-on-image.png "Wynikowy tekst jest wyświetlany z obrazem")

## <a name="memory-backed-images"></a>Obrazy z kopią zapasową w pamięci

Oprócz rysowania do kontekstu grafiki widoku, obsługuje podstawowe funkcje grafiki rysowania pamięci kopii obrazów, znany także jako rysowania ekranem. Wymaga:

-  Tworzenie kontekstu grafiki, który jest zabezpieczony za pomocą w pamięci mapy bitowej
-  Ustawianie stanu rysowania i wydawanie polecenia rysowania
-  Pobieranie obrazu z kontekstu
-  Usuwanie kontekstu


W odróżnieniu od `Draw` metody, jeśli kontekst jest dostarczana przez widok, w tym przypadku tworzenia kontekstu w jeden z dwóch sposobów:

1. Przez wywołanie metody `UIGraphics.BeginImageContext` (lub `BeginImageContextWithOptions`)

2. Tworząc nową `CGBitmapContextInstance`

 `CGBitmapContextInstance` jest przydatne podczas pracy bezpośrednio z bitów obrazu, takie jak w przypadkach, gdy używasz algorytm manipulowania obraz niestandardowy. We wszystkich innych przypadkach należy używać `BeginImageContext` lub `BeginImageContextWithOptions`.

Po utworzeniu kontekst obrazu, dodawanie kodu rysowania jest tak samo, jak jest `UIView` podklasę. Na przykład, w przykładzie kodu używane wcześniej, aby narysować trójkąt może służyć do rysowania obrazu w pamięci, a nie w `UIView`, jak pokazano poniżej:

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

Typowym zastosowaniem rysunku do mapy bitowej opartych na pamięci jest przechwytywania obrazu za pomocą dowolnego `UIView`. Na przykład, poniższy kod renderuje widok warstwy do kontekstu mapy bitowej i tworzy `UIImage` z niej:

```csharp
UIGraphics.BeginImageContext (cellView.Frame.Size);

//render the view's layer in the current context
anyView.Layer.RenderInContext (UIGraphics.GetCurrentContext ());

//get a UIImage from the context
UIImage anyViewImage = UIGraphics.GetImageFromCurrentImageContext ();
UIGraphics.EndImageContext ();
```

## <a name="drawing-pdfs"></a>Rysowanie plików PDF

Oprócz obrazy podstawowe funkcje grafiki obsługuje rysowania PDF. Takich jak obrazy, można renderować PDF w pamięci oraz jak odczyt dokumentu PDF do renderowania w `UIView`.

### <a name="pdf-in-a-uiview"></a>Plik PDF w UIView

Podstawowe funkcje grafiki obsługuje również podczas odczytu pliku PDF z pliku i renderowaniem go w widoku przy użyciu `CGPDFDocument` klasy. `CGPDFDocument` Klasa reprezentuje PDF w kodzie i może służyć do odczytu, a następnie narysuj stron.

Na przykład, poniższy kod w `UIView` podklasy odczytuje plik PDF z pliku do `CGPDFDocument`:

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

`Draw` Metoda może następnie używać `CGPDFDocument` można odczytać strony do `CGPDFPage` renderowany przez wywołanie metody `DrawPDFPage`, jak pokazano poniżej:

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

### <a name="memory-backed-pdf"></a>Pliki PDF z kopią zapasową w pamięci

Dla pliku PDF w pamięci, musisz utworzyć kontekst PDF, wywołując `BeginPDFContext`. Rysowanie do formatu PDF jest szczegółową do stron. Każda strona jest uruchomiona przez wywołanie metody `BeginPDFPage` oraz zostać zakończona, wywołując `EndPDFContent`, za pomocą grafiki między kodem. Ponadto za pomocą rysowania obrazu pamięci wykonania kopii PDF rysowania używa pochodzenia w lewym dolnym rogu, które mogą uwzględniać, po prostu modyfikując CTM podobnie jak przy użyciu obrazów.

Poniższy kod pokazuje, jak rysować tekst do pliku PDF:

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

Wynikowy tekstu jest rysowana na format PDF, który następnie jest zawarty w `NSData` , mogą być zapisywane, przekazanych, pocztą e-mail, itp.


## <a name="summary"></a>Podsumowanie

W tym artykule przyjrzeliśmy się funkcje grafiki zapewnianą w ramach *podstawowe funkcje grafiki* framework. Widzieliśmy, jak za pomocą podstawowe funkcje grafiki Rysuj geometrię, obrazów i plików PDF w kontekście `UIView,` oraz jak do opartych na pamięci grafiki kontekstów.

## <a name="related-links"></a>Linki pokrewne

- [Przykład grafiki Core](https://developer.xamarin.com/samples/monotouch/GraphicsAndAnimation/)
- [Grafika i animacje wskazówki](~/ios/platform/graphics-animation-ios/graphics-animation-walkthrough.md)
- [Podstawowe funkcje animacji](~/ios/platform/graphics-animation-ios/core-animation.md)
- [Podstawowe przepisy animacji](https://github.com/xamarin/recipes/tree/master/Recipes/ios/animation/coreanimation)
