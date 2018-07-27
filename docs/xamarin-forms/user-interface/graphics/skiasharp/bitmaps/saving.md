---
title: Zapisywanie map bitowych skiasharp — pliki
description: Zapoznaj się w różnych formatach plików, które są obsługiwane przez skiasharp — Zapisywanie map bitowych w bibliotece zdjęć użytkownika.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 2D696CB6-B31B-42BC-8D3B-11D63B1E7D9C
author: charlespetzold
ms.author: chape
ms.date: 07/10/2018
ms.openlocfilehash: 5ef18728bbf417750575bad88b3498f66fa585c4
ms.sourcegitcommit: ffb0f3dbf77b5f244b195618316bbd8964541e42
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/26/2018
ms.locfileid: "39276018"
---
# <a name="saving-skiasharp-bitmaps-to-files"></a>Zapisywanie map bitowych skiasharp — pliki

Po aplikacji SkiaSharp utworzone lub zmodyfikowane mapy bitowej, aplikacja może ma zostać zapisany mapy bitowej na bibliotekę zdjęć użytkownika:

![Zapisywanie map bitowych](saving-images/SavingSample.png "Zapisywanie map bitowych")

To zadanie obejmuje dwa kroki:

- Konwertowanie map bitowych SkiaSharp dane w formacie do określonego pliku, np. JPEG lub PNG.
- Zapisywanie wyniku biblioteki zdjęć przy użyciu kodu specyficznego dla platformy.

## <a name="file-formats-and-codecs"></a>Formaty i kodery-dekodery

Najczęściej plik bitmapy popularnych współczesnych formatuje Użyj kompresji w celu zmniejszenia miejsca do magazynowania. Dwie szerokie kategorie metod kompresji są nazywane _stratnej_ i _Bezstratne_. Niniejsze postanowienia wskazują, czy algorytmu kompresji powoduje utratę danych.

Najbardziej popularne stratnej format został opracowany przez JPEG i nosi nazwę JPEG. Algorytm kompresji JPEG analizuje obrazu przy użyciu narzędzia matematyczne _przekształcenie dyskretnych cosinus_i podejmie próbę usunięcia danych, która nie ma kluczowe znaczenie dla zachowania wizualnej obrazu. Stopień kompresji może być kontrolowana za pomocą ustawień, ogólnie nazywane _jakości_. Ustawienia wyższej jakości powodować większe pliki.

Z kolei jest algorytm kompresji bezstratne analizuje obraz dla powtórzeń i wzorców pikseli, które mogą być zakodowane w sposób, który ogranicza dane, ale nie powoduje utraty żadnych informacji. Można przywrócić oryginalnych danych mapy bitowej całkowicie z skompresowanego pliku. Podstawowy bezstratne skompresowany plik formatu używanego obecnie jest przenośny Network Graphics (PNG).

Ogólnie rzecz biorąc JPEG jest używana do zdjęć, podczas gdy PNG jest używany dla obrazów, które zostały wygenerowane ręcznie lub algorithmically. Dowolnego algorytmu bezstratne kompresji, który powoduje zmniejszenie rozmiaru niektórych plików zawsze należy zwiększyć rozmiar innych. Na szczęście to zwiększenie rozmiaru zazwyczaj występuje tylko dla danych, które zawiera wiele informacji losowy (lub pozornie losowe).

Kompresja algorytmy są dostatecznie złożone i gwarantuje, że dwa warunki, które opisują procesy kompresja i Dekompresja:

- _dekodowanie_ &mdash; odczytu format pliku mapy bitowej i zdekompresować go
- _kodowanie_ &mdash; zmniejszyć mapę bitową i zapis do pliku mapy bitowej w formacie

[ `SKBitmap` ](https://developer.xamarin.com/api/type/SkiaSharp.SKBitmap/) Klasa zawiera wiele metod o nazwie `Decode` które tworzą `SKBitmap` ze skompresowanym źródła. Wszystkie opcje, które są wymagane jest podać nazwę pliku, strumienia lub tablicę bajtów. Dekoder można określić format pliku i przekazują do odpowiednich funkcji dekodowania wewnętrznego.

Ponadto [ `SKCodec` ](https://developer.xamarin.com/api/type/SkiaSharp.SKCodec/) klasa ma dwie metody o nazwie `Create` , można utworzyć `SKCodec` obiektu ze skompresowanym źródła i zezwolić na aplikację uzyskać bardziej zaangażowane w proces dekodowania. ( `SKCodec` Klasy jest wyświetlany w artykule [ **animowanie mapy bitowe SkiaSharp** ](animating.md#gif-animation) związku dekodowanie animowany plik GIF.)

Podczas kodowania mapy bitowej, jest wymaganych więcej informacji: kodera musi znać format określonego pliku, aplikacja chce (JPEG lub PNG lub inny element). W razie potrzeby stratnej format Koduj musisz także wiedzieć żądany poziom jakości. 

`SKBitmap` Klasa definiuje jedną [ `Encode` ](https://developer.xamarin.com/api/member/SkiaSharp.SKBitmap.Encode/p/SkiaSharp.SKWStream/SkiaSharp.SKEncodedImageFormat/System.Int32/) metody za pomocą następującej składni:

```csharp
public Boolean Encode (SKWStream dst, SKEncodedImageFormat format, Int32 quality)
```

Ta metoda jest opisany bardziej szczegółowo wkrótce. Zakodowany mapy bitowej są zapisywane do strumienia zapisu. ("W" w `SKWStream` oznacza "z możliwością zapisu".) Drugi i trzeci argument służy do określania plików oraz (w formatach stratnej) odpowiednią jakość, od 0 do 100.

Ponadto [ `SKImage` ](https://developer.xamarin.com/api/type/SkiaSharp.SKImage/) i [ `SKPixmap` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPixmap/) również definiować klasy `Encode` metod, które są nieco bardziej wszechstronna i które można wybrać. Możesz łatwo tworzyć `SKImage` obiektu z `SKBitmap` przy użyciu statycznych [ `SKImage.FromBitmap` ](https://developer.xamarin.com/api/member/SkiaSharp.SKImage.FromBitmap/p/SkiaSharp.SKBitmap/) metody. Możesz uzyskać `SKPixmap` obiektu z `SKBitmp` przy użyciu [ `PeekPixels` ](https://developer.xamarin.com/api/member/SkiaSharp.SKBitmap.PeekPixels()/) metody.

Jedną z [ `Encode` ](https://developer.xamarin.com/api/member/SkiaSharp.SKImage.Encode()/) metody zdefiniowane przez `SKImage` nie ma parametrów i automatycznie zapisuje na PNG format. Tej metody bez parametrów jest bardzo łatwa w użyciu.

## <a name="platform-specific-code-for-saving-bitmap-files"></a>Kodu specyficznego dla platformy do zapisywania plików map bitowych

Jeśli kodowanie `SKBitmap` format obiektu do danego pliku, zwykle będzie pozostać przy użyciu obiektu strumienia, pewne rodzaje lub tablicę danych. Niektóre z `Encode` metody (łącznie z parametrów zdefiniowanych przez `SKImage`) zwracają [ `SKData` ](https://developer.xamarin.com/api/type/SkiaSharp.SKData/) obiektu, który może zostać przekonwertowany na tablicę bajtów przy użyciu [ `ToArray` ](https://developer.xamarin.com/api/member/SkiaSharp.SKData.ToArray()/) metody. Te dane, następnie musi zostać zapisany do pliku. 

Zapisywanie do pliku w magazynie lokalnym w aplikacji jest całkiem proste, ponieważ używasz standardowych `System.IO` klasy i metody dla tego zadania. Ta technika jest przedstawiona w artykule [ **animowanie mapy bitowe SkiaSharp** ](animating.md#bitmap-animation) związku animowanie szereg mapy bitowe zestawu Mandelbrot.

Jeśli chcesz, aby go do udostępnienia przez inne aplikacje, musi zostać zapisany do biblioteki zdjęć użytkownika. To zadanie wymaga kodu specyficznego dla platformy, a także korzystanie z zestawu narzędzi Xamarin.Forms [ `DependencyService` ](xref:Xamarin.Forms.DependencyService).

**SkiaSharpFormsDemo** projektu w [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) aplikacji zdefiniowano `IPhotoLibrary` interfejs używany przy użyciu `DependencyService` klasy. Definiuje składnia `SavePhotoAsync` metody:

```csharp
public interface IPhotoLibrary
{
    Task<Stream> PickPhotoAsync();

    Task<bool> SavePhotoAsync(byte[] data, string folder, string filename);
}
```

Ten interfejs definiuje również `PickPhotoAsync` metody, która służy do otwierania selektora plików specyficznych dla platformy dla urządzenia biblioteki zdjęć.

Aby uzyskać `SavePhotoAsync`, pierwszy argument jest tablicę bajtów, która zawiera mapę bitową już zakodowany w formacie do określonego pliku, na przykład JPEG lub PNG. Jest to możliwe, aplikacja może chcesz wyizolować wszystkich map bitowych, utworzonego do określonego folderu, który jest określony w parametrze dalej, a następnie według nazwy pliku. Metoda zwraca wartość Boolean wskazującą sukces, czy nie.

Oto jak `SavePhotoAsync` jest wdrażana w trzech platformach:

### <a name="the-ios-implementation"></a>Implementacja systemu iOS

Implementacja systemu iOS `SavePhotoAsync` używa [ `SaveToPhotosAlbum` ](https://developer.xamarin.com/api/member/UIKit.UIImage.SaveToPhotosAlbum/) metody `UIImage`:

```csharp
public class PhotoLibrary : IPhotoLibrary
{
    ···
    public Task<bool> SavePhotoAsync(byte[] data, string folder, string filename)
    {
        NSData nsData = NSData.FromArray(data);
        UIImage image = new UIImage(nsData);
        TaskCompletionSource<bool> taskCompletionSource = new TaskCompletionSource<bool>();

        image.SaveToPhotosAlbum((UIImage img, NSError error) =>
        {
            taskCompletionSource.SetResult(error == null);
        });

        return taskCompletionSource.Task;
    }
}
```

Niestety nie istnieje żaden sposób, aby określić nazwę pliku lub folderu dla obrazu. 

**Info.plist** pliku w projekcie dla systemu iOS wymaga klucza wskazujący, że dodaje obrazy do biblioteki zdjęć:

```xml
<key>NSPhotoLibraryAddUsageDescription</key>
<string>SkiaSharp Forms Demos adds images to your photo library</string>
```

Należy uważać! Uprawnienia klucza po prostu uzyskiwania dostępu do biblioteki zdjęć jest bardzo podobne, ale nie sam:

```xml
<key>NSPhotoLibraryUsageDescription</key>
<string>SkiaSharp Forms Demos accesses your photo library</string>
```

### <a name="the-android-implementation"></a>Implementacja systemu Android

Implementacja systemu Android `SavePhotoAsync` najpierw sprawdza, czy `folder` argument jest `null` ani być pustym ciągiem. Jeśli tak, mapa bitowa jest zapisywany w katalogu głównym bibliotekę zdjęć. W przeciwnym razie uzyskuje się folder, a jeśli on nie istnieje, zostanie utworzony:

```csharp
public class PhotoLibrary : IPhotoLibrary
{
    ···
    public async Task<bool> SavePhotoAsync(byte[] data, string folder, string filename)
    {
        try
        {
            File picturesDirectory = Environment.GetExternalStoragePublicDirectory(Environment.DirectoryPictures);
            File folderDirectory = picturesDirectory;

            if (!string.IsNullOrEmpty(folder))
            {
                folderDirectory = new File(picturesDirectory, folder);
                folderDirectory.Mkdirs();
            }

            using (File bitmapFile = new File(folderDirectory, filename))
            {
                bitmapFile.CreateNewFile();

                using (FileOutputStream outputStream = new FileOutputStream(bitmapFile))
                {
                    await outputStream.WriteAsync(data);
                }

                // Make sure it shows up in the Photos gallery promptly.
                MediaScannerConnection.ScanFile(MainActivity.Instance,
                                                new string[] { bitmapFile.Path },
                                                new string[] { "image/png", "image/jpeg" }, null);
            }
        }
        catch
        {
            return false;
        }

        return true;
    }
}
```

Wywołanie `MediaScannerConnection.ScanFile` nie jest ściśle wymagane, ale jeśli testujesz program natychmiast, sprawdzając biblioteki zdjęć, znacznie ułatwia, aktualizując Widok galerii biblioteki.

**AndroidManifest.xml** plik wymaga następującego tagu uprawnień:

```xml
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```

### <a name="the-uwp-implementation"></a>Implementacja platformy uniwersalnej systemu Windows

Implementacja platformy uniwersalnej systemu Windows `SavePhotoAsync` jest bardzo podobne w strukturze do wykonania dla systemu Android:

```csharp
public class PhotoLibrary : IPhotoLibrary
{
    ···
    public async Task<bool> SavePhotoAsync(byte[] data, string folder, string filename)
    {
        StorageFolder picturesDirectory = KnownFolders.PicturesLibrary;
        StorageFolder folderDirectory = picturesDirectory;

        // Get the folder or create it if necessary
        if (!string.IsNullOrEmpty(folder))
        {
            try
            {
                folderDirectory = await picturesDirectory.GetFolderAsync(folder);
            }
            catch
            { }

            if (folderDirectory == null)
            {
                try
                {
                    folderDirectory = await picturesDirectory.CreateFolderAsync(folder);
                }
                catch
                {
                    return false;
                }
            }
        }

        try
        {
            // Create the file.
            StorageFile storageFile = await folderDirectory.CreateFileAsync(filename,
                                                CreationCollisionOption.GenerateUniqueName);

            // Convert byte[] to Windows buffer and write it out.
            IBuffer buffer = WindowsRuntimeBuffer.Create(data, 0, data.Length, data.Length);
            await FileIO.WriteBufferAsync(storageFile, buffer);
        }
        catch
        {
            return false;
        }

        return true;
    }
}
```

**Możliwości** części **Package.appxmanifest** wymaga pliku **biblioteki obrazów**.

## <a name="exploring-the-image-formats"></a>Eksplorowanie formatów obrazu

Oto [ `Encode` ](https://developer.xamarin.com/api/member/SkiaSharp.SKBitmap.Encode/p/SkiaSharp.SKWStream/SkiaSharp.SKEncodedImageFormat/System.Int32/) metody `SKImage` ponownie:

```csharp
public Boolean Encode (SKWStream dst, SKEncodedImageFormat format, Int32 quality)
```

[`SKEncodedImageFormat`](https://developer.xamarin.com/api/type/SkiaSharp.SKEncodedImageFormat/) to wyliczenie z elementów członkowskich, które odwołują się do jedenaście mapy bitowej formatów plików, z których niektóre są raczej zasłoniętej:

- `Astc` &mdash; Kompresja adaptacyjne skalowalne tekstury
- `Bmp` &mdash; Mapa bitowa Windows
- `Dng` &mdash; Ujemna cyfrowego Adobe
- `Gif` &mdash; Format GIF
- `Ico` &mdash; Obrazy ikon Windows
- `Jpeg` &mdash; JPEG
- `Ktx` &mdash; Format tekstury Khronos OpenGL
- `Pkm` &mdash; Pokémon Zapisz plik
- `Png` &mdash; Przenośne grafiki sieci
- `Wbmp` &mdash; Mapa bitowa bezprzewodowej protokołu aplikacji (1 bit na piksel)
- `Webp` &mdash; Format Google WebP

Jak można zauważyć wkrótce, tylko trzy te formaty plików (`Jpeg`, `Png`, i `Webp`) są faktycznie objęte SkiaSharp.

Aby zapisać `SKBitmap` obiektu o nazwie `bitmap` do biblioteki zdjęć użytkownika, należy również członkiem `SKEncodedImageFormat` wyliczeń o nazwie `imageFormat` i (w formatach stratnej) całkowitą `quality` zmiennej. Poniższy kod umożliwia zapisanie tej mapy bitowej w pliku o nazwie `filename` w `folder` folderu:

```csharp
using (MemoryStream memStream = new MemoryStream())
using (SKManagedWStream wstream = new SKManagedWStream(memStream))
{
    bitmap.Encode(wstream, imageFormat, quality);
    byte[] data = memStream.ToArray();

    // Check the data array for content!

    bool success = await DependencyService.Get<IPhotoLibrary>().SavePhotoAsync(data, folder, filename);

    // Check return value for success!
}
```

`SKManagedWStream` Klasa pochodzi od `SKWStream` (który oznacza "strumień możliwy do zapisania"). `Encode` Metoda zapisuje plik zakodowany mapy bitowej w strumieniu. Komentarze w tym kodzie odnoszą się do niektórych sprawdzania błędów, może być konieczne wykonanie. 

**Zapisz formatów plików** strony w [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) aplikacja używa podobny kod, aby umożliwić wypróbowanie Zapisywanie mapy bitowej w różnych formatach.

Plik XAML zawiera `SKCanvasView` wyświetlającą mapy bitowej, a pozostałej części strony zawiera wszystko, co aplikacja musi wywoływać `Encode` metody `SKBitmap`. Ma ona `Picker` członka `SKEncodedImageFormat` wyliczenia, `Slider` dla argumentu jakości formatów stratnej mapy bitowej dwóch `Entry` widoków dla nazwy pliku i folderu i `Button` przy zapisywaniu pliku.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp;assembly=SkiaSharp"
             xmlns:skiaforms="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Bitmaps.SaveFileFormatsPage"
             Title="Save Bitmap Formats">

    <StackLayout Margin="10">
        <skiaforms:SKCanvasView PaintSurface="OnCanvasViewPaintSurface"
                                VerticalOptions="FillAndExpand" />

        <Picker x:Name="formatPicker"
                Title="image format"
                SelectedIndexChanged="OnFormatPickerChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type skia:SKEncodedImageFormat}">
                    <x:Static Member="skia:SKEncodedImageFormat.Astc" />
                    <x:Static Member="skia:SKEncodedImageFormat.Bmp" />
                    <x:Static Member="skia:SKEncodedImageFormat.Dng" />
                    <x:Static Member="skia:SKEncodedImageFormat.Gif" />
                    <x:Static Member="skia:SKEncodedImageFormat.Ico" />
                    <x:Static Member="skia:SKEncodedImageFormat.Jpeg" />
                    <x:Static Member="skia:SKEncodedImageFormat.Ktx" />
                    <x:Static Member="skia:SKEncodedImageFormat.Pkm" />
                    <x:Static Member="skia:SKEncodedImageFormat.Png" />
                    <x:Static Member="skia:SKEncodedImageFormat.Wbmp" />
                    <x:Static Member="skia:SKEncodedImageFormat.Webp" />
                </x:Array>
            </Picker.ItemsSource>
        </Picker>

        <Slider x:Name="qualitySlider"
                Maximum="100"
                Value="50" />

        <Label Text="{Binding Source={x:Reference qualitySlider},
                              Path=Value,
                              StringFormat='Quality = {0:F0}'}"
               HorizontalTextAlignment="Center" />

        <StackLayout Orientation="Horizontal">
            <Label Text="Folder Name: "
                   VerticalOptions="Center" />

            <Entry x:Name="folderNameEntry"
                   Text="SaveFileFormats"
                   HorizontalOptions="FillAndExpand" />
        </StackLayout>

        <StackLayout Orientation="Horizontal">
            <Label Text="File Name: "
                   VerticalOptions="Center" />

            <Entry x:Name="fileNameEntry"
                   Text="Sample.xxx"
                   HorizontalOptions="FillAndExpand" />
        </StackLayout>

        <Button Text="Save" 
                Clicked="OnButtonClicked">
            <Button.Triggers>
                <DataTrigger TargetType="Button"
                             Binding="{Binding Source={x:Reference formatPicker},
                                               Path=SelectedIndex}"
                             Value="-1">
                    <Setter Property="IsEnabled" Value="False" />
                </DataTrigger>

                <DataTrigger TargetType="Button"
                             Binding="{Binding Source={x:Reference fileNameEntry},
                                               Path=Text.Length}"
                             Value="0">
                    <Setter Property="IsEnabled" Value="False" />
                </DataTrigger>
            </Button.Triggers>
        </Button>

        <Label x:Name="statusLabel"
               Text="OK"
               Margin="10, 0" />
    </StackLayout>
</ContentPage>
```

Plik związany z kodem ładuje zasób mapy bitowej i używa `SKCanvasView` aby go wyświetlić. Bitmap, nigdy nie zmiany. `SelectedIndexChanged` Obsługa `Picker` zmienia nazwę pliku z rozszerzeniem, który jest taki sam jak element członkowski wyliczenia:

```csharp
public partial class SaveFileFormatsPage : ContentPage
{
    SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(typeof(SaveFileFormatsPage),
        "SkiaSharpFormsDemos.Media.MonkeyFace.png");

    public SaveFileFormatsPage ()
    {
        InitializeComponent ();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        args.Surface.Canvas.DrawBitmap(bitmap, args.Info.Rect, BitmapStretch.Uniform);
    }

    void OnFormatPickerChanged(object sender, EventArgs args)
    {
        if (formatPicker.SelectedIndex != -1)
        {
            SKEncodedImageFormat imageFormat = (SKEncodedImageFormat)formatPicker.SelectedItem;
            fileNameEntry.Text = Path.ChangeExtension(fileNameEntry.Text, imageFormat.ToString());
            statusLabel.Text = "OK";
        }
    }

    async void OnButtonClicked(object sender, EventArgs args)
    {
        SKEncodedImageFormat imageFormat = (SKEncodedImageFormat)formatPicker.SelectedItem;
        int quality = (int)qualitySlider.Value;

        using (MemoryStream memStream = new MemoryStream())
        using (SKManagedWStream wstream = new SKManagedWStream(memStream))
        {
            bitmap.Encode(wstream, imageFormat, quality);
            byte[] data = memStream.ToArray();

            if (data == null)
            {
                statusLabel.Text = "Encode returned null";
            }
            else if (data.Length == 0)
            {
                statusLabel.Text = "Encode returned empty array";
            }
            else
            {
                bool success = await DependencyService.Get<IPhotoLibrary>().
                    SavePhotoAsync(data, folderNameEntry.Text, fileNameEntry.Text);

                if (!success)
                {
                    statusLabel.Text = "SavePhotoAsync return false";
                }
                else
                {
                    statusLabel.Text = "Success!";
                }
            }
        }
    }
}
```

`Clicked` Obsługa `Button` działa wszystkie rzeczywiste. Uzyskuje dwóch argumentów dla `Encode` z `Picker` i `Slider`, a następnie używa kodu pokazanego wcześniej do utworzenia `SKManagedWStream` dla `Encode` metody. Dwa `Entry` widoków przedstawić nazwy plików i folderów dla `SavePhotoAsync` metody.

Większość tej metody jest przeznaczone do obsługi problemów lub błędów. Jeśli `Encode` tworzy pustą tablicę, oznacza to, że format określonego pliku nie jest obsługiwany. Jeśli `SavePhotoAsync` zwraca `false`, a następnie plik nie został pomyślnie zapisany. 

W tym miejscu jest uruchomiony na trzech platformach program:

[![Zapisz formatów plików](saving-images/SaveFileFormats.png "zapisywać formaty plików")](saving-images/SaveFileFormats-Large.png#lightbox)

Ten zrzut ekranu przedstawia tylko dla trzech formatach, które są obsługiwane na tych platformach:

- JPEG
- PNG
- WebP

Dla wszystkich pozostałych formatów `Encode` metoda zapisuje nic w strumieniu i tablicy bajtowej wynikowy jest pusty.

Mapa bitowa, **Zapisz formaty plików** zapisuje strony jest kwadratowy 600 pikseli. O 4 bajty na piksel to suma bajtów 1,440,000 w pamięci. W poniższej tabeli przedstawiono rozmiaru pliku dla różnych kombinacji format pliku i jakości:

|Format|Jakość|Rozmiar|
|------|------:|---:|
| PNG | Brak | 492K |
| JPEG | 0 | 2,95 K |
|      | 50 | 22.1 K |
|      | 100 | 206K |
| WebP | 0 | 2.71K |
|      | 50 | 11,9 K |
|      | 100 | 101K |

Możesz eksperymentować z różnymi ustawieniami jakości i sprawdź wyniki.

## <a name="saving-finger-paint-art"></a>Zapisywanie finger-paint clipart

Typowym zastosowaniem jednego mapy bitowej znajduje się w rysowania programów, w której działa zgodnie z coś o nazwie _mapy bitowej w tle_. Wszystkie rysowania są przechowywane na mapę bitową, która jest następnie wyświetlana przez program. Mapa bitowa również przydatność zapisywania na rysunku.

[ **Malowanie palcami w SkiaSharp** ](../paths/finger-paint.md) artykule przedstawiono sposób użycia śledzenia do zaimplementowania pierwotnych program kolorowa touch. Program obsługuje tylko jeden kolor i szerokość pociągnięcia tylko jeden, ale zachowane całe Rysowanie w zbiorze `SKPath` obiektów.

**Paint palcem z Zapisz** strony w [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) przykładowe zachowuje, całe Rysowanie w zbiorze `SKPath` obiektów, ale ona również renderuje rysunku na mapę bitową, który można zapisać do biblioteki zdjęć.

Większość tego programu jest podobny do oryginalnego **Finger Paint** program. Ulepszeniem jest, że plik XAML teraz tworzy przyciski **wyczyść** i **Zapisz**:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             xmlns:tt="clr-namespace:TouchTracking"
             x:Class="SkiaSharpFormsDemos.Bitmaps.FingerPaintSavePage"
             Title="Finger Paint Save">

    <StackLayout>
        <Grid BackgroundColor="White"
              VerticalOptions="FillAndExpand">
            <skia:SKCanvasView x:Name="canvasView"
                               PaintSurface="OnCanvasViewPaintSurface" />
            <Grid.Effects>
                <tt:TouchEffect Capture="True"
                                TouchAction="OnTouchEffectAction" />
            </Grid.Effects>
        </Grid>

        <Grid>
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="*" />
                <ColumnDefinition Width="*" />
            </Grid.ColumnDefinitions>
        </Grid>

        <Button Text="Clear"
                Grid.Row="0"
                Margin="50, 5"
                Clicked="OnClearButtonClicked" />

        <Button Text="Save"
                Grid.Row="1"
                Margin="50, 5"
                Clicked="OnSaveButtonClicked" />

    </StackLayout>
</ContentPage>
```

Plik związany z kodem przechowuje pola typu `SKBitmap` o nazwie `saveBitmap`. Ta mapa bitowa jest tworzony lub odtworzone w `PaintSurface` obsługi zawsze wtedy, gdy rozmiar wyświetlania powierzchni zmiany. Jeśli mapa bitowa musi zostać ponownie utworzone, zawartości istniejącej mapy bitowej są kopiowane do nowej mapy bitowej, tak aby wszystko, co jest zachowana, niezależnie od tego, jak wyświetlanej powierzchni zmienia rozmiar:

```csharp
public partial class FingerPaintSavePage : ContentPage
{
    ···
    SKBitmap saveBitmap;

    public FingerPaintSavePage ()
    {
        InitializeComponent ();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        // Create bitmap the size of the display surface
        if (saveBitmap == null)
        {
            saveBitmap = new SKBitmap(info.Width, info.Height);
        }
        // Or create new bitmap for a new size of display surface
        else if (saveBitmap.Width < info.Width || saveBitmap.Height < info.Height)
        {
            SKBitmap newBitmap = new SKBitmap(Math.Max(saveBitmap.Width, info.Width),
                                              Math.Max(saveBitmap.Height, info.Height));

            using (SKCanvas newCanvas = new SKCanvas(newBitmap))
            {
                newCanvas.Clear();
                newCanvas.DrawBitmap(saveBitmap, 0, 0);
            }

            saveBitmap = newBitmap;
        }

        // Render the bitmap
        canvas.Clear();
        canvas.DrawBitmap(saveBitmap, 0, 0);
    }
    ···
}
```

Rysowanie, wykonywane przez `PaintSurface` obsługi występuje na końcu i składa się wyłącznie z renderowaniem mapy bitowej.

Przetwarzanie touch jest podobny do starszych program. Program obsługuje dwie kolekcje `inProgressPaths` i `completedPaths`, które zawierają wszystko, co użytkownik ma rysowany od czasu ostatniego wyświetlanie został wyczyszczony. Dla każdego zdarzenia dotykowe `OnTouchEffectAction` obsługi zdarzeń wywołuje `UpdateBitmap`:

```csharp
public partial class FingerPaintSavePage : ContentPage
{
    Dictionary<long, SKPath> inProgressPaths = new Dictionary<long, SKPath>();
    List<SKPath> completedPaths = new List<SKPath>();

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Blue,
        StrokeWidth = 10,
        StrokeCap = SKStrokeCap.Round,
        StrokeJoin = SKStrokeJoin.Round
    };
    ···
    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        switch (args.Type)
        {
            case TouchActionType.Pressed:
                if (!inProgressPaths.ContainsKey(args.Id))
                {
                    SKPath path = new SKPath();
                    path.MoveTo(ConvertToPixel(args.Location));
                    inProgressPaths.Add(args.Id, path);
                    UpdateBitmap();
                }
                break;

            case TouchActionType.Moved:
                if (inProgressPaths.ContainsKey(args.Id))
                {
                    SKPath path = inProgressPaths[args.Id];
                    path.LineTo(ConvertToPixel(args.Location));
                    UpdateBitmap();
                }
                break;

            case TouchActionType.Released:
                if (inProgressPaths.ContainsKey(args.Id))
                {
                    completedPaths.Add(inProgressPaths[args.Id]);
                    inProgressPaths.Remove(args.Id);
                    UpdateBitmap();
                }
                break;

            case TouchActionType.Cancelled:
                if (inProgressPaths.ContainsKey(args.Id))
                {
                    inProgressPaths.Remove(args.Id);
                    UpdateBitmap();
                }
                break;
        }
    }

    SKPoint ConvertToPixel(Point pt)
    {
        return new SKPoint((float)(canvasView.CanvasSize.Width * pt.X / canvasView.Width),
                            (float)(canvasView.CanvasSize.Height * pt.Y / canvasView.Height));
    }

    void UpdateBitmap()
    {
        using (SKCanvas saveBitmapCanvas = new SKCanvas(saveBitmap))
        {
            saveBitmapCanvas.Clear();

            foreach (SKPath path in completedPaths)
            {
                saveBitmapCanvas.DrawPath(path, paint);
            }

            foreach (SKPath path in inProgressPaths.Values)
            {
                saveBitmapCanvas.DrawPath(path, paint);
            }
        }

        canvasView.InvalidateSurface();
    }
    ···
}
```

`UpdateBitmap` Ponownych rysowaniach metoda `saveBitmap` przez utworzenie nowego `SKCanvas`, czyszczenie i następnie renderowania wszystkich ścieżek w mapie bitowej. Kontroler uzna samego unieważniając `canvasView` , dzięki czemu mogą być wystawiane mapy bitowej na ekranie.

Poniżej przedstawiono procedury obsługi dla dwóch przycisków. **Wyczyść** przycisk czyści obie kolekcje ścieżki aktualizacji `saveBitmap` (które powoduje wyczyszczenie mapę bitową), a unieważnia `SKCanvasView`:

```csharp
public partial class FingerPaintSavePage : ContentPage
{
    ···
    void OnClearButtonClicked(object sender, EventArgs args)
    {
        completedPaths.Clear();
        inProgressPaths.Clear();
        UpdateBitmap();
        canvasView.InvalidateSurface();
    }

    async void OnSaveButtonClicked(object sender, EventArgs args)
    {
        using (SKImage image = SKImage.FromBitmap(saveBitmap))
        {
            SKData data = image.Encode();
            DateTime dt = DateTime.Now;
            string filename = String.Format("FingerPaint-{0:D4}{1:D2}{2:D2}-{3:D2}{4:D2}{5:D2}{6:D3}.png",
                                            dt.Year, dt.Month, dt.Day, dt.Hour, dt.Minute, dt.Second, dt.Millisecond);

            IPhotoLibrary photoLibrary = DependencyService.Get<IPhotoLibrary>();
            bool result = await photoLibrary.SavePhotoAsync(data.ToArray(), "FingerPaint", filename);

            if (!result)
            {
                await DisplayAlert("FingerPaint", "Artwork could not be saved. Sorry!", "OK");
            }
        }
    }
}
```

**Zapisz** obsługi przycisk korzysta z uproszczonego [ `Encode` ](https://developer.xamarin.com/api/member/SkiaSharp.SKImage.Encode()/) metody z `SKImage`. Ta metoda koduje w formacie PNG. `SKImage` Obiekt jest tworzony na podstawie `saveBitmap`i `SKData` obiekt zawiera zakodowany plik PNG. 

`ToArray` Metody `SKData` uzyskuje tablicę bajtów. Jest to, co jest przekazywany do `SavePhotoAsync` metoda wraz z nazwą folderu stały i unikatową nazwę pliku skonstruowany na podstawie aktualnej daty i godziny.

Poniżej przedstawiono program w akcji:

[![Finger Paint, Zapisz](saving-images/FingerPaintSave.png "Finger Paint, Zapisz")](saving-images/FingerPaintSave-Large.png#lightbox)

Bardzo podobne techniki jest używany w [ **aplikacja SpinPaint** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SpinPaint/) próbki. Jest to również kolorowa program, chyba że użytkownik maluje na dysku obrotowych, który następnie odtwarza projekty na jego cztery raporty. Kręci się kolor zmiany paint finger jako dysk:

[![Uruchamiaj Paint](saving-images/SpinPaint.png "pokrętła malowania")](saving-images/SpinPaint-Large.png#lightbox)

**Zapisz** przycisku `SpinPaint` klasa jest podobna do **Finger Paint** , zapisuje obraz nazwą folderu stałych (**SpainPaint**) i nazwa pliku jest zbudowany z Data i godzina.

## <a name="related-links"></a>Linki pokrewne

- [Skiasharp — interfejsy API](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (przykład)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
- [Aplikacja SpinPaint (przykład)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SpinPaint/)
