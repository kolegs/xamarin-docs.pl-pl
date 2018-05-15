---
title: Ładowanie aplikacji zasobów wideo
ms.prod: xamarin
ms.assetid: F75BD540-9354-4C17-A119-57F3DEC66D54
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: 89c424ee80a4ebf6363a836e752b72ee9bc5cd5a
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/10/2018
---
# <a name="loading-application-resource-videos"></a>Ładowanie aplikacji zasobów wideo

Niestandardowe moduły renderowania dla `VideoPlayer` widoku są w stanie odtwarzania plików wideo, które zostały osadzone w projektach poszczególnych platform jako zasoby aplikacji. Jednak w bieżącej wersji programu `VideoPlayer` nie może uzyskać dostęp do zasobów osadzonych w bibliotece .NET Standard.

Aby załadować te zasoby, Utwórz wystąpienie `ResourceVideoSource` przez ustawienie `Path` właściwości do pliku (lub folderu i nazwa pliku) zasobu. Alternatywnie można wywołać statycznych `VideoSource.FromResource` metodę, aby odwoływać się do zasobu. Następnie ustaw `ResourceVideoSource` do obiektu `Source` właściwość `VideoPlayer`. 

## <a name="storing-the-video-files"></a>Przechowywanie plików wideo

Zapisanie pliku wideo w projekcie platformy różni się w trzech platformy:

### <a name="ios-video-resources"></a>Zasoby wideo z systemem iOS

W projekcie systemu iOS można przechowywać wideo w **zasobów** folder lub podfolder **zasobów** folderu. Plik musi mieć `Build Action` z `BundleResource`. Ustaw `Path` właściwość `ResourceVideoSource` do nazwy pliku, na przykład **MyFile.mp4** dla pliku w **zasobów** folderze, lub **MyFolder/MyFile.mp4**, gdzie **MójFolder** jest podfolderem **zasobów**.

W **VideoPlayerDemos** rozwiązania, **VideoPlayerDemos.iOS** projekt zawiera podfolder **zasobów** o nazwie **wideo** zawierającego plik o nazwie **iOSApiVideo.mp4**. To jest krótki film wideo przedstawiający sposób używania witryny sieci web program Xamarin, aby znaleźć dokumentację dla systemu iOS `AVPlayerViewController` klasy.

### <a name="android-video-resources"></a>Android zasobów wideo

W projekcie systemu Android wideo musi być przechowywany w podfolderze **zasobów** o nazwie **raw**. **Raw** folderu nie może zawierać podfolderów. Nadaj pliku wideo `Build Action` z `AndroidResource`. Ustaw `Path` właściwość `ResourceVideoSource` do nazwy pliku, na przykład **MyFile.mp4**. 

**VideoPlayerDemos.Android** projekt zawiera podfolder **zasobów** o nazwie **raw**, który zawiera plik o nazwie **AndroidApiVideo.mp4**. 

### <a name="uwp-video-resources"></a>Zasoby wideo platformy uniwersalnej systemu Windows

W projekcie platformy uniwersalnej systemu Windows można przechowywać pliki wideo w dowolnym folderze w projekcie. Nadaj plikowi `Build Action` z `Content`. Ustaw `Path` właściwość `ResourceVideoSource` do folderu i nazwę pliku, na przykład **MyFolder/MyVideo.mp4**. 

**VideoPlayerDemos.UWP** projekt zawiera folder o nazwie **wideo** z plikiem **UWPApiVideo.mp4**.

## <a name="loading-the-video-files"></a>Ładowanie plików wideo

Platforma klas renderowania zawiera kod w jego `SetSource` metody do ładowania plików wideo, przechowywane jako zasoby.

### <a name="ios-resource-loading"></a>ładowanie zasobów systemu iOS

Wersja systemu iOS `VideoPlayerRenderer` używa `GetUrlForResource` metody `NSBundle` ładowania zasobów. Pełna ścieżka musi podzielone na nazwę pliku, rozszerzenia i katalogu. W kodzie użyto `Path` klasy w ramach platformy .NET `System.IO` przestrzeń nazw dla dzielenia ścieżkę pliku do tych składników:

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
        void SetSource()
        {
            AVAsset asset = null;
            ···
            else if (Element.Source is ResourceVideoSource)
            {
                string path = (Element.Source as ResourceVideoSource).Path;

                if (!String.IsNullOrWhitespace(path))
                {
                    string directory = Path.GetDirectoryName(path);
                    string filename = Path.GetFileNameWithoutExtension(path);
                    string extension = Path.GetExtension(path).Substring(1);
                    NSUrl url = NSBundle.MainBundle.GetUrlForResource(filename, extension, directory);
                    asset = AVAsset.FromUrl(url);
                }
            }
            ···
        }
        ···
    }
}
```

### <a name="android-resource-loading"></a>Ładowanie zasobów systemu android

Android `VideoPlayerRenderer` używa nazwy pliku, a pakiet do skonstruowania `Uri` obiektu. Nazwa pakietu jest nazwa aplikacji, w tym przypadku **VideoPlayerDemos.Android**, którą można uzyskać z statycznych `Context.PackageName` właściwości. Wynikowe `Uri` obiektu są następnie przekazywane do `SetVideoURI` metody `VideoView`:

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        ···    
        void SetSource()
        {
            isPrepared = false;
            bool hasSetSource = false;
            ···
            else if (Element.Source is ResourceVideoSource)
            {
                string package = Context.PackageName;
                string path = (Element.Source as ResourceVideoSource).Path;

                if (!String.IsNullOrWhiteSpace(path))
                {
                    string filename = Path.GetFileNameWithoutExtension(path).ToLowerInvariant();
                    string uri = "android.resource://" + package + "/raw/" + filename;
                    videoView.SetVideoURI(Android.Net.Uri.Parse(uri));
                    hasSetSource = true;
                }
            }
            ···
        }
        ···
    }
}
```

### <a name="uwp-resource-loading"></a>Ładowanie zasobów platformy uniwersalnej systemu Windows

UWP `VideoPlayerRenderer` tworzy `Uri` obiektu dla ścieżki i ustawia ją na `Source` właściwości `MediaElement`:

```csharp
namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        ···
        async void SetSource()
        {
            bool hasSetSource = false;
            ···
            else if (Element.Source is ResourceVideoSource)
            {
                string path = "ms-appx:///" + (Element.Source as ResourceVideoSource).Path;

                if (!String.IsNullOrWhiteSpace(path))
                {
                    Control.Source = new Uri(path);
                    hasSetSource = true;
                }
            }
        }
        ···
    }
}
```

## <a name="playing-the-resource-file"></a>Odtwarzanie plików zasobów

**Odtwarzanie wideo zasobów** strony **VideoPlayerDemos** używa rozwiązania `OnPlatform` klasę, aby określić plik wideo dla każdej platformy:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.PlayVideoResourcePage"
             Title="Play Video Resource">
    <video:VideoPlayer>
        <video:VideoPlayer.Source>
            <video:ResourceVideoSource>
                <video:ResourceVideoSource.Path>
                    <OnPlatform x:TypeArguments="x:String">
                        <On Platform="iOS" Value="Videos/iOSApiVideo.mp4" />
                        <On Platform="Android" Value="AndroidApiVideo.mp4" />
                        <On Platform="UWP" Value="Videos/UWPApiVideo.mp4" />
                    </OnPlatform>
                </video:ResourceVideoSource.Path>
            </video:ResourceVideoSource>
        </video:VideoPlayer.Source>
    </video:VideoPlayer>
</ContentPage>
```

Jeśli zasób z systemem iOS jest przechowywany w **zasobów** folderu, i jeśli zasób platformy uniwersalnej systemu Windows jest przechowywany w folderze głównym projektu, możesz użyć tej samej nazwy pliku dla trzech platform. Jeśli tak jest, a następnie można ustawić bezpośrednio do tej nazwy `Source` właściwość `VideoPlayer`. 

Oto działająca na platformach trzy strony:

[![Odtwarzanie wideo zasobów](loading-resources-images/playvideoresource-small.png "odtwarzanie wideo zasobów")](loading-resources-images/playvideoresource-large.png#lightbox "odtwarzanie wideo zasobów")

Teraz przedstawiono sposób [załadować wideo z identyfikatora URI sieci Web](web-videos.md) i jak odtworzyć zasoby osadzone. Ponadto można [załadować wideo z biblioteki wideo urządzenia](accessing-library.md).


## <a name="related-links"></a>Linki pokrewne

- [Pokazy odtwarzacza wideo (przykład)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
