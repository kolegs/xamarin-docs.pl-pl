---
title: Mapa bitowa — podstawy w SkiaSharp
description: W tym artykule wyjaśniono, jak załadować mapy bitowe w SkiaSharp z różnych źródeł i wyświetlaj je w aplikacjach Xamarin.Forms i przedstawia to z przykładowym kodem.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 32C95DFF-9065-42D7-966C-D3DBD16906B3
author: charlespetzold
ms.author: chape
ms.date: 07/17/2018
ms.openlocfilehash: 92863ff9e843cabc26c568e95aab52c6d199c35e
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615213"
---
# <a name="bitmap-basics-in-skiasharp"></a>Mapa bitowa — podstawy w SkiaSharp

_Ładowanie map bitowych z różnych źródeł i ich wyświetlenie._

Obsługa mapy bitowej w SkiaSharp jest dość rozbudowana. W tym artykule opisano tylko podstawowe informacje o &mdash; ładowanie mapy bitowe i sposób ich wyświetlania:

![](bitmaps-images/bitmapssample.png "Wyświetlanie dwóch map bitowych")

Znacznie bardziej zaawansowanej eksploracji mapy bitowe znajdują się w sekcji [mapy bitowe SkiaSharp](../bitmaps/index.md).

Skiasharp — mapa bitowa jest obiektem typu [ `SKBitmap` ](https://developer.xamarin.com/api/type/SkiaSharp.SKBitmap/). Istnieje wiele sposobów, aby utworzyć mapę bitową, ale w tym artykule ogranicza się do [ `SKBitmap.Decode` ](https://developer.xamarin.com/api/member/SkiaSharp.SKBitmap.Decode/p/System.IO.Stream/) metody, która ładuje mapę bitową z .NET `Stream` obiektu.

**Podstawowe mapy bitowe** strony w **SkiaSharpFormsDemos** program pokazuje, jak można załadować mapy bitowe z trzech różnych źródeł:

- Ponad Internet
- Z zasobu osadzonego w pliku wykonywalnym
- Z biblioteki zdjęć użytkownika

Trzy `SKBitmap` obiektów dla tych trzech źródeł są zdefiniowane jako pola w [ `BasicBitmapsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/BasicBitmapsPage.cs) klasy:

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

## <a name="loading-a-bitmap-from-the-web"></a>Trwa ładowanie mapy bitowej z sieci Web

Aby załadować mapy bitowej na podstawie adresu URL, możesz użyć [ `HttpClient` ](/dotnet/api/system.net.http.httpclient?view=netstandard-2.0) klasy. Należy utworzyć wystąpienie tylko jedno wystąpienie `HttpClient` i użyć go ponownie, więc Zapisz go jako pola:

```csharp
HttpClient httpClient = new HttpClient();
```

Korzystając z `HttpClient` z systemem iOS i aplikacji dla systemu Android, będziesz chciał ustawiania właściwości projektu, zgodnie z opisem w dokumentach na  **[zabezpieczeń TLS (Transport Layer) 1.2](~/cross-platform/app-fundamentals/transport-layer-security.md)**.

Ponieważ jest najbardziej łatwa w użyciu `await` operatora z `HttpClient`, kod nie można wykonać w `BasicBitmapsPage` konstruktora. Zamiast tego jest on częścią `OnAppearing` zastąpienia. Tutaj jej adres URL wskazuje obszar w witrynie sieci web platformy Xamarin przy użyciu map bitowych niektóre przykładowe. Pakiet w witrynie sieci web umożliwia dołączanie Specyfikacja rozmiaru mapy bitowej do określonej szerokości:


```csharp
protected override async void OnAppearing()
{
    base.OnAppearing();

    // Load web bitmap.
    string url = "https://developer.xamarin.com/demo/IMG_3256.JPG?width=480";

    try
    {
        using (Stream stream = await httpClient.GetStreamAsync(url))
        using (MemoryStream memStream = new MemoryStream())
        {
            await stream.CopyToAsync(memStream);
            memStream.Seek(0, SeekOrigin.Begin);

            webBitmap = SKBitmap.Decode(stream);
            canvasView.InvalidateSurface();
        };
    }
    catch
    {
    }
}
```

Android zgłosi wyjątek, korzystając z `Stream` zwróciło `GetStreamAsync` w `SKBitmap.Decode` metody ponieważ działa ona długotrwałej operacji w wątku głównym. Z tego powodu zawartość pliku mapy bitowej są kopiowane do `MemoryStream` przy użyciu `CopyToAsync`.

Statyczne `SKBitmap.Decode` metoda jest odpowiedzialna dekodowanie pliki map bitowych. W programach JPEG, PNG, GIF i kilka innych formatów popularnych mapy bitowej, a wyniki są zapisywane w wewnętrznym formacie SkiaSharp. W tym momencie `SKCanvasView` musi być unieważnione, aby umożliwić `PaintSurface` program obsługi, aby zaktualizować wyświetlane dane. 

## <a name="loading-a-bitmap-resource"></a>Trwa ładowanie zasób mapy bitowej

Pod względem kodu Najprostszym sposobem ładowanie map bitowych obejmują zasób mapy bitowej bezpośrednio w aplikacji. **SkiaSharpFormsDemos** program obejmuje folder o nazwie **Media** zawierający plik mapy bitowej o nazwie **monkey.png**. W **właściwości** okno dialogowe dla tego pliku, musisz udzielić takiego pliku **Build Action** z **zasób osadzony**!

Każdy zasób osadzony ma *identyfikator zasobu* składający się z nazwy projektu, folder i nazwę pliku, wszystkie połączone w czasie: **SkiaSharpFormsDemos.Media.monkey.png**. Możesz uzyskać dostęp do tego zasobu, określając zasobu identyfikator jako argument do [ `GetManifestResourceStream` ](xref:System.Reflection.Assembly.GetManifestResourceStream(System.String)) metody [ `Assembly` ](xref:System.Reflection.Assembly) klasy:

```csharp
string resourceID = "SkiaSharpFormsDemos.Media.monkey.png";
Assembly assembly = GetType().GetTypeInfo().Assembly;

using (Stream stream = assembly.GetManifestResourceStream(resourceID))
{
    resourceBitmap = SKBitmap.Decode(stream);
}
```

To `Stream` obiektów, które mogą być przekazywane bezpośrednio do `SKBitmap.Decode` metody.

## <a name="loading-a-bitmap-from-the-photo-library"></a>Trwa ładowanie mapy bitowej z biblioteki zdjęć

Użytkownik może również dla użytkownika do załadowania zdjęć z biblioteki obrazów urządzenia. Ten obiekt nie zostanie podany przez samego zestawu narzędzi Xamarin.Forms. Zadanie nie wymaga usługi zależności, takiego jak opisano w artykule [pobrania zdjęcia z biblioteki obrazów](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md).

**IPhotoLibrary.cs** w pliku **SkiaSharpFormsDemos** projektu i trzy **PhotoLibrary.cs** plikami w projektach platformy zostało zmienione tak, z tego artykułu. Ponadto Android **MainActivity.cs** plik został zmodyfikowany, zgodnie z opisem w artykule, a projekt dla systemu iOS zostały przypisane uprawnienie do dostępu do biblioteki zdjęć z dwoma wierszami w dolnej części **info.plist**  pliku.

`BasicBitmapsPage` Dodaje Konstruktor `TapGestureRecognizer` do `SKCanvasView` Aby otrzymywać powiadomienia o podsłuchu. W przypadku otrzymania naciśnij `Tapped` obsługi uzyskuje dostęp do wywołań i obraz selektora zależności usługi `GetImageStreamAsync`. Jeśli `Stream` obiekt jest zwracany, a następnie zawartość jest kopiowana do `MemoryStream`, co jest wymagane przez niektóre z platform. Pozostała część kodu jest podobny do dwóch innych technik:

```csharp
// Add tap gesture recognizer
TapGestureRecognizer tapRecognizer = new TapGestureRecognizer();
tapRecognizer.Tapped += async (sender, args) =>
{
    // Load bitmap from photo library
    IPhotoLibrary photoLibrary = DependencyService.Get<IPhotoLibrary>();

    using (Stream stream = await photoLibrary.PickPhotoAsync())
    {
        if (stream != null)
        {
            libraryBitmap = SKBitmap.Decode(stream);
            canvasView.InvalidateSurface();
        }
    }
};
canvasView.GestureRecognizers.Add(tapRecognizer);
```

Należy zauważyć, że `Tapped` obsługi zdarzeń wywołuje `InvalidateSurface` metody `SKCanvasView` obiektu. Spowoduje to wygenerowanie nowe wywołanie w celu `PaintSurface` programu obsługi.

## <a name="displaying-the-bitmaps"></a>Wyświetlanie map bitowych

`PaintSurface` Obsługi wymaganych, aby wyświetlić trzy map bitowych. Program obsługi założono, że telefon jest w trybie pionowym i dzieli kanwy w pionie na trzy części równe.

Pierwszy mapy bitowej jest wyświetlany z najprostszych [ `DrawBitmap` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmap/p/SkiaSharp.SKBitmap/System.Single/System.Single/SkiaSharp.SKPaint/) metody. Wszystko, czego potrzebujesz, aby określić są współrzędne X i Y, w którym ma zostać umieszczony w lewym górnym rogu mapy bitowej:

```csharp
public void DrawBitmap (SKBitmap bitmap, Single x, Single y, SKPaint paint = null)
```

Mimo że `SKPaint` parametr jest zdefiniowany, jego wartość domyślną `null` i można je zignorować. Piksele lustrzany mapy bitowej, po prostu są przenoszone do piksele wyświetlanej powierzchni z mapowanie jeden do jednego.

Program można uzyskać wymiarów z mapy bitowej w pikselach [ `Width` ](https://developer.xamarin.com/api/property/SkiaSharp.SKBitmap.Width/) i [ `Height` ](https://developer.xamarin.com/api/property/SkiaSharp.SKBitmap.Height/) właściwości. Te właściwości Zezwalaj na program do obliczania współrzędne do pozycji mapy bitowej w środkowej części trzeciej górnego obszaru roboczego:

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

Pozostałe dwie mapy bitowe są wyświetlane przy użyciu wersji [ `DrawBitmap` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmap/p/SkiaSharp.SKBitmap/SkiaSharp.SKRect/SkiaSharp.SKPaint/) z `SKRect` parametru:

```csharp
public void DrawBitmap (SKBitmap bitmap, SKRect dest, SKPaint paint = null)
```

Trzecia wersja [ `DrawBitmap` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmap/p/SkiaSharp.SKBitmap/SkiaSharp.SKRect/SkiaSharp.SKRect/SkiaSharp.SKPaint/) ma dwa `SKRect` argumenty do określania podzbiór prostokątny mapy bitowej do wyświetlania, ale ta wersja nie jest używany w tym artykule.

Poniżej przedstawiono kod, aby wyświetlić mapę bitową ładowane z mapy bitowej zasób osadzony:

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

Mapa bitowa jest rozciągnięty do wymiarów prostokąta, dlatego małp poziomo jest rozciągany tak, następujące zrzuty ekranu:

[![](bitmaps-images/basicbitmaps-small.png "Potrójna zrzut ekranu przedstawiający stronę podstawowe mapy bitowe")](bitmaps-images/basicbitmaps-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę podstawowe map bitowych")

Trzeci obrazu &mdash; który można wyświetlić tylko jeśli Uruchom program i załaduj zdjęcie z biblioteki obrazów &mdash; również jest wyświetlany w obrębie prostokąta, ale stopień jego położenie i rozmiar, które są dostosowywane do mapy bitowej współczynnik proporcji. To obliczenie jest nieco więcej wysiłku, ponieważ wymaga obliczenia na podstawie rozmiaru mapy bitowej i prostokąta docelowego współczynnik skalowania i wyśrodkowania prostokąt, w tym obszarze:

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

Jeśli żadne bitmapy nie został jeszcze załadowany z biblioteki obrazów, a następnie `else` bloku Wyświetla tekst, który zostanie wyświetlony monit naciśnij ekran.


## <a name="related-links"></a>Linki pokrewne

- [Skiasharp — interfejsy API](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (przykład)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
- [Pobrania zdjęcia z biblioteki obrazów](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md)
