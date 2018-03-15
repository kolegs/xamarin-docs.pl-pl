---
title: Podstawy mapy bitowej
description: "Załaduj map bitowych z różnych źródeł i wyświetlić je."
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 32C95DFF-9065-42D7-966C-D3DBD16906B3
author: charlespetzold
ms.author: chape
ms.date: 04/03/2017
ms.openlocfilehash: 921697657397662e92fb72c32e6efcc31745d7f1
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/15/2018
---
# <a name="bitmap-basics"></a>Podstawy mapy bitowej

_Załaduj map bitowych z różnych źródeł i wyświetlić je._

Obsługa map bitowych w SkiaSharp jest dość rozbudowana. W tym artykule opisano tylko podstawowe informacje o &mdash; sposób załadować mapy bitowe i można je wyświetlić:

![](bitmaps-images/bitmapssample.png "Wyświetlanie dwóch mapy bitowe")

Mapy bitowej SkiaSharp jest obiektem typu [ `SKBitmap` ](https://developer.xamarin.com/api/type/SkiaSharp.SKBitmap/). Istnieje wiele sposobów, aby utworzyć mapę bitową, ale w tym artykule ogranicza się do [ `SKBitmap.Decode` ](https://developer.xamarin.com/api/member/SkiaSharp.SKBitmap.Decode/p/SkiaSharp.SKStream/) metodę, która ładuje mapę bitową z [ `SKStream` ](https://developer.xamarin.com/api/type/SkiaSharp.SKStream/) obiektu, który odwołuje się do pliku mapy bitowej. Jest łatwe w użyciu [ `SKManagedStream` ](https://developer.xamarin.com/api/type/SkiaSharp.SKManagedStream/) klasą pochodzącą z `SKStream` ponieważ ma on konstruktora akceptującego .NET [ `Stream` ](https://developer.xamarin.com/api/type/System.IO.Stream/) obiektu.

**Podstawowe map bitowych** strony **SkiaSharpFormsDemos** program pokazano, jak można załadować mapy bitowe z trzech różnych źródeł:

- Przez Internet
- Z zasobu osadzonego w pliku wykonywalnego
- Z biblioteki zdjęcia użytkownika

Trzy `SKBitmap` obiektów dla tych źródeł jest trzech są zdefiniowane jako pola w [ `BasicBitmapsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Basics/BasicBitmapsPage.cs) klasy:

```csharp
public class BasicBitmapsPage : ContentPage
{
    SKCanvasView canvasView;
    SKBitmap webBitmap;
    SKBitmap resourceBitmap;
    SKBitmap libraryBitmap;

    public BasicBitmapsPage()
    {
        Title = "Basic Bitmaps";

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
        ...
    }
    ...
}
```

## <a name="loading-a-bitmap-from-the-web"></a>Ładuje mapę bitową z sieci Web

Aby załadować mapy bitowej na podstawie adresu URL, można użyć [ `WebRequest` ](https://developer.xamarin.com/api/type/System.Net.WebRequest/) klasy, jak pokazano w poniższym kodzie wykonywane w `BasicBitmapsPage` konstruktora. Tutaj jej adres URL wskazuje obszar w witrynie sieci web Xamarin niektóre mapy bitowe próbki. Pakiet w witrynie sieci web umożliwia dołączanie specyfikację do zmiany rozmiaru mapy bitowej do określonej szerokości:

```csharp
Uri uri = new Uri("http://developer.xamarin.com/demo/IMG_3256.JPG?width=480");
WebRequest request = WebRequest.Create(uri);
request.BeginGetResponse((IAsyncResult arg) =>
{
    try
    {
        using (Stream stream = request.EndGetResponse(arg).GetResponseStream())
        using (MemoryStream memStream = new MemoryStream())
        {
            stream.CopyTo(memStream);
            memStream.Seek(0, SeekOrigin.Begin);

            using (SKManagedStream skStream = new SKManagedStream(memStream))
            {
                webBitmap = SKBitmap.Decode(skStream);
            }
        }
    }
    catch
    {
    }

    Device.BeginInvokeOnMainThread(() => canvasView.InvalidateSurface());

}, null);
```

Gdy mapa bitowa została pomyślnie pobrana, metody wywołania zwrotnego przekazywane do `BeginGetResponse` uruchamia metody. `EndGetResponse` Wywołanie musi znajdować się w `try` blokowania w przypadku, gdy wystąpił błąd. `Stream` Uzyskanego z `GetResponseStream` nie jest odpowiednia na niektórych platformach, więc zawartość mapy bitowej są kopiowane do `MemoryStream` obiektu. W tym momencie `SKManagedStream` można utworzyć obiektu. To teraz odwołuje się do pliku mapy bitowej, który prawdopodobnie jest to plik JPEG lub PNG. `SKBitmap.Decode` Metoda Dekoduje plik mapy bitowej i przechowuje wyniki w wewnętrznym formacie SkiaSharp.

Metoda wywołania zwrotnego przekazany do `BeginGetResponse` uruchomień po zakończeniu konstruktora wykonywania, których `SKCanvasView` musi być unieważniona umożliwia `PaintSurface` obsługi, aby zaktualizować wyświetlane. Jednak `BeginGetResponse` wywołania zwrotnego jest uruchamiany w dodatkowej wątku do wykonania, dlatego należy użyć `Device.BeginInvokeOnMainThread` do uruchomienia `InvalidateSurface` metody w wątku interfejsu użytkownika.

## <a name="loading-a-bitmap-resource"></a>Ładowanie zasobu mapy bitowej

Pod względem kodu Najprostszym sposobem ładowania map bitowych jest tym zasobu mapy bitowej bezpośrednio w aplikacji. **SkiaSharpFormsDemos** program zawiera folder o nazwie **nośnika** zawierający plik mapy bitowej o nazwie **monkey.png**. W **właściwości** okna dialogowego dla tego pliku, musisz podać taki plik **Akcja kompilacji** z **osadzonego zasobu**!

Każdy osadzonego zasobu ma *identyfikator zasobu* składający się z nazwy projektu, folder i nazwę pliku, wszystkich połączonych w czasie: **SkiaSharpFormsDemos.Media.monkey.png**. Możesz uzyskać dostęp do tego zasobu, określając ten zasób identyfikator jako argument [ `GetManifestResourceStream` ](https://developer.xamarin.com/api/member/System.Reflection.Assembly.GetManifestResourceStream/p/System.String/) metody [ `Assembly` ](https://developer.xamarin.com/api/type/System.Reflection.Assembly/) klasy:

```csharp
string resourceID = "SkiaSharpFormsDemos.Media.monkey.png";
Assembly assembly = GetType().GetTypeInfo().Assembly;

using (Stream stream = assembly.GetManifestResourceStream(resourceID))
using (SKManagedStream skStream = new SKManagedStream(stream))
{
    resourceBitmap = SKBitmap.Decode(skStream);
}
```

To `Stream` można przekonwertować bezpośrednio do obiektu `SKManagedStream` obiektu.

## <a name="loading-a-bitmap-from-the-photo-library"></a>Ładowanie mapy bitowej z biblioteki zdjęcia

Istnieje również możliwość dla użytkownika można załadować zdjęcie z biblioteki obrazów urządzenia. Tej funkcji nie jest zapewniana przez platformy Xamarin.Forms samej siebie. Zadanie wymaga usługi zależności, takie jak opisano w artykule [pobrania zdjęcia z biblioteki obrazów](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md).

**IPicturePicker.cs** plików i trzech **PicturePickerImplementation.cs** pliki z tego artykułu zostały skopiowane do różnych projektów **SkiaSharpFormsDemos**rozwiązania i nowej nazwy przestrzeni nazw. Ponadto Android **MainActivity.cs** plik został zmodyfikowany, zgodnie z opisem w artykule i projektu iOS udzielono uprawnień dostępu biblioteki zdjęć z dwa wiersze w dolnej części **info.plist**  pliku.

`BasicBitmapsPage` Dodaje konstruktora `TapGestureRecognizer` do `SKCanvasView` ma być powiadamiany o podsłuchu. Po otrzymaniu naciśnij `Tapped` obsługi uzyskuje dostęp do usługi zależności selektora obrazu i wywołania `GetImageStreamAsync`. Jeśli `Stream` obiekt jest zwracany, a następnie zawartość jest kopiowana do `MemoryStream`, co jest wymagane przez niektóre platformy. Pozostała część kodu jest podobny do dwóch innych technik:

```csharp
// Add tap gesture recognizer
TapGestureRecognizer tapRecognizer = new TapGestureRecognizer();
tapRecognizer.Tapped += async (sender, args) =>
{
    // Load bitmap from photo library
    IPicturePicker picturePicker = DependencyService.Get<IPicturePicker>();

    using (Stream stream = await picturePicker.GetImageStreamAsync())
    {
        if (stream != null)
        {
            using (MemoryStream memStream = new MemoryStream())
            {
                stream.CopyTo(memStream);
                memStream.Seek(0, SeekOrigin.Begin);

                using (SKManagedStream skStream = new SKManagedStream(memStream))
                {
                    libraryBitmap = SKBitmap.Decode(skStream);
                }
            }
            canvasView.InvalidateSurface();
        }
    }
};
canvasView.GestureRecognizers.Add(tapRecognizer);
```

Zwróć uwagę, że `Tapped` wywołań obsługi `InvalidateSurface` metody `SKCanvasView` obiektu. Spowoduje to wygenerowanie nowe wywołanie w celu `PaintSurface` obsługi.

## <a name="displaying-the-bitmaps"></a>Wyświetlanie map bitowych

`PaintSurface` Obsługi potrzebne do wyświetlania trzy map bitowych. Program obsługi założono, że telefon jest w trybie portret i w pionie dzieli obszar roboczy na trzy równe części.

Pierwszy mapy bitowej jest wyświetlany z najprostszą [ `DrawBitmap` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmap/p/SkiaSharp.SKBitmap/System.Single/System.Single/SkiaSharp.SKPaint/) metody. Należy określić wszystkie są współrzędne X i Y, w którym ma być umieszczony w lewym górnym rogu mapy bitowej:

```csharp
public void DrawBitmap (SKBitmap bitmap, Single x, Single y, SKPaint paint = null)
```

Mimo że `SKPaint` parametr jest zdefiniowany, ma ona wartości domyślnej równej `null` i można je zignorować. Pikseli mapy bitowej po prostu są przenoszone do pikseli powierzchni ekranu z mapowanie jeden do jednego.

Program można uzyskać wymiary mapy bitowej z [ `Width` ](https://developer.xamarin.com/api/property/SkiaSharp.SKBitmap.Width/) i [ `Height` ](https://developer.xamarin.com/api/property/SkiaSharp.SKBitmap.Height/) właściwości. Te właściwości Zezwól na obliczanie współrzędnych w celu umieść mapy bitowej w Centrum trzeciej górny obszar roboczy programu:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    if (webBitmap != null)
    {
        float x = (info.Width - webBitmap.Width) / 2;
        float y = (info.Height / 3 - webBitmap.Height) / 2;
        canvas.DrawBitmap(webBitmap, x, y);
    }
    ...
}
```

Dwa bitmapy są wyświetlane z wersją [ `DrawBitmap` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmap/p/SkiaSharp.SKBitmap/SkiaSharp.SKRect/SkiaSharp.SKPaint/) z `SKRect` parametru:

```csharp
public void DrawBitmap (SKBitmap bitmap, SKRect dest, SKPaint paint = null)
```

Trzecia wersja [ `DrawBitmap` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmap/p/SkiaSharp.SKBitmap/SkiaSharp.SKRect/SkiaSharp.SKRect/SkiaSharp.SKPaint/) dwiema `SKRect` argumenty służący do określania podzbiór prostokątne mapy bitowej do wyświetlania, ale ta wersja nie jest używana w tym artykule.

Oto kod, aby wyświetlić mapy bitowej załadowane z mapy bitowej osadzonego zasobu:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    if (resourceBitmap != null)
    {
        canvas.DrawBitmap(resourceBitmap,
            new SKRect(0, info.Height / 3, info.Width, 2 * info.Height / 3));
    }
    ...
}
```

Mapy bitowej jest rozciągany tak, aby wymiary prostokąta, dlatego małp poziomo jest rozciągany tak, te zrzuty ekranu:

[![](bitmaps-images/basicbitmaps-small.png "Potrójna zrzut ekranu strony mapy bitowe podstawowe")](bitmaps-images/basicbitmaps-large.png#lightbox "Potrójna zrzut ekranu strony podstawowe mapy bitowe")

Trzeci obrazu &mdash; który można wyświetlić tylko jeśli uruchomienie programu i załadować zdjęcie z biblioteki obrazów &mdash; jest wyświetlany również w prostokącie, ale prostokąta położenie i rozmiar są dostosowane do mapy bitowej współczynnik proporcji. Obliczona w ten sposób jest nieco więcej wysiłku, ponieważ wymaga obliczania zależnie od rozmiaru mapy bitowej, jak i docelowy prostokąt czynnik skalowania oraz wyśrodkowanie prostokąt w tym obszarze:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    if (libraryBitmap != null)
    {
        float scale = Math.Min((float)info.Width / libraryBitmap.Width,
                               info.Height / 3f / libraryBitmap.Height);

        float left = (info.Width - scale * libraryBitmap.Width) / 2;
        float top = (info.Height / 3 - scale * libraryBitmap.Height) / 2;
        float right = left + scale * libraryBitmap.Width;
        float bottom = top + scale * libraryBitmap.Height;
        SKRect rect = new SKRect(left, top, right, bottom);
        rect.Offset(0, 2 * info.Height / 3);

        canvas.DrawBitmap(libraryBitmap, rect);
    }
    else
    {
        using (SKPaint paint = new SKPaint())
        {
            paint.Color = SKColors.Blue;
            paint.TextAlign = SKTextAlign.Center;
            paint.TextSize = 48;

            canvas.DrawText("Tap to load bitmap",
                info.Width / 2, 5 * info.Height / 6, paint);
        }
    }
}
```

Jeśli bitmapy nie został jeszcze załadowany z biblioteki obrazów, a następnie `else` bloku Wyświetla tekst w celu monitowania użytkownika o naciśnięcie przycisku ekranu.


## <a name="related-links"></a>Linki pokrewne

- [Interfejsy API SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (przykład)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
- [Pobrania zdjęcia z biblioteki obrazów](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md)
