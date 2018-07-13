---
title: Odtwarzanie filmu wideo z Internetu
description: W tym artykule opisano sposób odtwarzania filmów wideo w sieci web w aplikacji odtwarzacza wideo przy użyciu zestawu narzędzi Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 75781A10-865D-4BA8-8D6B-E3DA012922BC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: 566f056bd616c918ce274b9c7406d94fdc265ea2
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994562"
---
# <a name="playing-a-web-video"></a>Odtwarzanie filmu wideo z Internetu

`VideoPlayer` Klasa definiuje `Source` właściwość używaną do określania źródła pliku wideo, a także `AutoPlay` właściwości. `AutoPlay` ma domyślne ustawienie `true`, co oznacza, że wideo ma się zacząć odtwarzanie automatycznie po `Source` został ustawiony:

```csharp
using System;
using Xamarin.Forms;

namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
        // Source property
        public static readonly BindableProperty SourceProperty =
            BindableProperty.Create(nameof(Source), typeof(VideoSource), typeof(VideoPlayer), null);

        [TypeConverter(typeof(VideoSourceConverter))]
        public VideoSource Source
        {
            set { SetValue(SourceProperty, value); }
            get { return (VideoSource)GetValue(SourceProperty); }
        }

        // AutoPlay property
        public static readonly BindableProperty AutoPlayProperty =
            BindableProperty.Create(nameof(AutoPlay), typeof(bool), typeof(VideoPlayer), true);

        public bool AutoPlay
        {
            set { SetValue(AutoPlayProperty, value); }
            get { return (bool)GetValue(AutoPlayProperty); }
        }
        ···
    }
}
```

`Source` Właściwość jest typu `VideoSource`, który jest deseniem po Xamarin.Forms [ `ImageSource` ](xref:Xamarin.Forms.ImageSource) abstrakcyjnej klasy i jej trzech pochodne [ `UriImageSource` ](xref:Xamarin.Forms.UriImageSource), [ `FileImageSource` ](xref:Xamarin.Forms.FileImageSource), i [ `StreamImageSource` ](xref:Xamarin.Forms.StreamImageSource). Nie strumienia opcja jest dostępna dla `VideoPlayer` jednak ponieważ systemów iOS i Android obsługuje odtwarzanie wideo ze strumienia.

## <a name="video-sources"></a>Filmów wideo

Abstrakcyjna `VideoSource` klasy składa się wyłącznie z trzech metod statycznych, które wystąpienia trzech klas, które wynikają z `VideoSource`:

```csharp
namespace FormsVideoLibrary
{
    [TypeConverter(typeof(VideoSourceConverter))]
    public abstract class VideoSource : Element
    {
        public static VideoSource FromUri(string uri)
        {
            return new UriVideoSource() { Uri = uri };
        }

        public static VideoSource FromFile(string file)
        {
            return new FileVideoSource() { File = file };
        }

        public static VideoSource FromResource(string path)
        {
            return new ResourceVideoSource() { Path = path };
        }
    }
}
```

`UriVideoSource` Klasa jest używana do określania plików wideo do pobrania z identyfikatorem URI. Definiuje jedną właściwość typu `string`:

```csharp
namespace FormsVideoLibrary
{
    public class UriVideoSource : VideoSource
    {
        public static readonly BindableProperty UriProperty =
            BindableProperty.Create(nameof(Uri), typeof(string), typeof(UriVideoSource));

        public string Uri
        {
            set { SetValue(UriProperty, value); }
            get { return (string)GetValue(UriProperty); }
        }
    }
}
```

Obsługa obiektów typu `UriVideoSource` opisano poniżej.

`ResourceVideoSource` Umożliwia dostęp do plików wideo, które są przechowywane jako zasoby osadzone w aplikacji platformy, również określony za pomocą klasy `string` właściwości:

```csharp
namespace FormsVideoLibrary
{
    public class ResourceVideoSource : VideoSource
    {
        public static readonly BindableProperty PathProperty =
            BindableProperty.Create(nameof(Path), typeof(string), typeof(ResourceVideoSource));

        public string Path
        {
            set { SetValue(PathProperty, value); }
            get { return (string)GetValue(PathProperty); }
        }
    }
}
```

Obsługa obiektów typu `ResourceVideoSource` jest opisany w artykule [ładowanie filmów wideo zasobów aplikacji](loading-resources.md). `VideoPlayer` Klasa ma możliwość załadowanie pliku wideo, która jest przechowywana jako zasób w bibliotece programu .NET Standard.

`FileVideoSource` Klasa jest używana do dostępu do plików wideo z urządzenia wideo biblioteki. Jedną właściwość, jest również typu `string`:

```csharp
namespace FormsVideoLibrary
{
    public class FileVideoSource : VideoSource
    {
        public static readonly BindableProperty FileProperty =
                  BindableProperty.Create(nameof(File), typeof(string), typeof(FileVideoSource));

        public string File
        {
            set { SetValue(FileProperty, value); }
            get { return (string)GetValue(FileProperty); }
        }
    }
}
```

Obsługa obiektów typu `FileVideoSource` jest opisany w artykule [uzyskiwania dostępu do biblioteki wideo urządzenia](accessing-library.md).

`VideoSource` Klasa zawiera `TypeConverter` atrybut, który odwołuje się do `VideoSourceConverter`:

```csharp
namespace FormsVideoLibrary
{
    [TypeConverter(typeof(VideoSourceConverter))]
    public abstract class VideoSource : Element
    {
        ···
    }
}
```

Ten konwerter typu jest wywoływana po `Source` właściwość jest ustawiona na ciąg w XAML. Oto `VideoSourceConverter` klasy:

```csharp
namespace FormsVideoLibrary
{
    public class VideoSourceConverter : TypeConverter
    {
        public override object ConvertFromInvariantString(string value)
        {
            if (!String.IsNullOrWhiteSpace(value))
            {
                Uri uri;
                return Uri.TryCreate(value, UriKind.Absolute, out uri) && uri.Scheme != "file" ?
                                VideoSource.FromUri(value) : VideoSource.FromResource(value);
            }

            throw new InvalidOperationException("Cannot convert null or whitespace to ImageSource");
        }
    }
}
```

`ConvertFromInvariantString` Metoda podejmuje próbę przekonwertowania ciągu na `Uri` obiektu. Jeśli się to powiedzie i nie jest to schemat `file:`, a następnie metoda zwraca `UriVideoSource`. W przeciwnym razie zwraca `ResourceVideoSource`.

## <a name="setting-the-video-source"></a>Ustawienia źródła wideo

Wszystkie inne logiki dotyczących filmów wideo jest zaimplementowana w programy renderujące poszczególnych platform. W poniższych sekcjach opisano, jak programy renderujące platformy odtwarzania wideo po `Source` właściwość jest ustawiona na `UriVideoSource` obiektu.

### <a name="the-ios-video-source"></a>Źródło wideo dla systemu iOS

Dwie sekcje `VideoPlayerRenderer` są zaangażowane w ustawienie źródła wideo z odtwarzaczem wideo. Gdy Xamarin.Forms najpierw tworzy obiekt typu `VideoPlayer`, `OnElementChanged` metoda jest wywoływana z `NewElement` obiektu w argumenty właściwością, `VideoPlayer`. `OnElementChanged` Wywołania metody `SetSource`:

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                ···
                SetSource();
                ···
            }
        }

        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            ···
            else if (args.PropertyName == VideoPlayer.SourceProperty.PropertyName)
            {
                SetSource();
            }
            ···
        }
        ···
    }
}
```

Później, w przypadku `Source` właściwość zostanie zmieniona, `OnElementPropertyChanged` metoda jest wywoływana z `PropertyName` właściwość "Źródło" i `SetSource` wywoływana jest ponownie.

Do odtwarzania pliku wideo w systemie iOS, obiekt typu [ `AVAsset` ](https://developer.xamarin.com/api/type/AVFoundation.AVAsset/) utworzenia do hermetyzacji pliku wideo, które jest używane do tworzenia [ `AVPlayerItem` ](https://developer.xamarin.com/api/type/AVFoundation.AVPlayerItem/), który jest następnie przekazane do `AVPlayer`obiektu. Poniżej przedstawiono sposób, w jaki `SetSource` metodę uchwytów `Source` właściwości, gdy jest on typu `UriVideoSource`:

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        AVPlayer player;
        AVPlayerItem playerItem;
        ···
        void SetSource()
        {
            AVAsset asset = null;

            if (Element.Source is UriVideoSource)
            {
                string uri = (Element.Source as UriVideoSource).Uri;

                if (!String.IsNullOrWhiteSpace(uri))
                {
                    asset = AVAsset.FromUrl(new NSUrl(uri));
                }
            }
            ···
            if (asset != null)
            {
                playerItem = new AVPlayerItem(asset);
            }
            else
            {
                playerItem = null;
            }

            player.ReplaceCurrentItemWithPlayerItem(playerItem);

            if (playerItem != null && Element.AutoPlay)
            {
                player.Play();
            }
        }
        ···
    }
}
```

`AutoPlay` Właściwość ma analogowy nie klas wideo dla systemu iOS, więc właściwość jest badany na końcu `SetSource` metodę do wywołania `Play` metody `AVPlayer` obiektu.

W niektórych przypadkach wideo nadal odtwarzany po stronie z `VideoPlayer` przejście do strony głównej. Aby zatrzymać wideo `ReplaceCurrentItemWithPlayerItem` jest również ustawiona `Dispose` zastąpienia:

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
        protected override void Dispose(bool disposing)
        {
            base.Dispose(disposing);

            if (player != null)
            {
                player.ReplaceCurrentItemWithPlayerItem(null);
            }
        }
        ···
    }
}
```

### <a name="the-android-video-source"></a>Źródło wideo dla systemu Android

Android `VideoPlayerRenderer` musi ustawić źródło wideo odtwarzacza podczas `VideoPlayer` jest najpierw utworzony i nowszych wersjach po `Source` zmiany właściwości:

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        ···
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                ···
                SetSource();
                ···
            }
        }
        ···
        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            ···
            else if (args.PropertyName == VideoPlayer.SourceProperty.PropertyName)
            {
                SetSource();
            }
            ···
        }
        ···
    }
}
```

`SetSource` Metoda obsługuje obiektów typu `UriVideoSource` przez wywołanie metody `SetVideoUri` na `VideoView` z systemem Android `Uri` obiektu utworzonego na podstawie ciągu identyfikatora URI. `Uri` Klasy jest w pełni kwalifikowana tutaj w odróżnieniu od programu .NET `Uri` klasy:

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

            if (Element.Source is UriVideoSource)
            {
                string uri = (Element.Source as UriVideoSource).Uri;

                if (!String.IsNullOrWhiteSpace(uri))
                {
                    videoView.SetVideoURI(Android.Net.Uri.Parse(uri));
                    hasSetSource = true;
                }
            }
            ···

            if (hasSetSource && Element.AutoPlay)
            {
                videoView.Start();
            }
        }
        ···
    }
}
```

Android `VideoView` nie ma odpowiadającego mu `AutoPlay` właściwości, więc `Start` metoda jest wywoływana, gdy nowy film wideo został ustawiony.

Istnieje następująca różnica między zachowanie systemu iOS i Android programy renderujące, jeśli `Source` właściwość `VideoPlayer` jest ustawiona na `null`, lub jeśli `Uri` właściwość `UriVideoSource` jest równa `null` lub ciąg pusty. Odtwarzacz wideo dla systemu iOS jest aktualnie odtwarzania wideo, a `Source` jest ustawiona na `null` (lub ciąg jest `null` lub puste), `ReplaceCurrentItemWithPlayerItem` jest wywoływana z `null` wartości. Bieżący plik wideo zostanie zastąpiony i zatrzymuje odtwarzanie.

System android nie obsługuje podobne możliwości. Jeśli `Source` właściwość jest ustawiona na `null`, `SetSource` po prostu ignoruje metody i bieżący plik wideo jest nadal odtwarzany.

### <a name="the-uwp-video-source"></a>Źródło wideo platformy uniwersalnej systemu Windows

Platformy UWP `MediaElement` definiuje `AutoPlay` właściwość, która jest obsługiwana w modułu renderowania, takich jak wszystkich innych właściwości:

```csharp
namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                ···
                SetSource();
                SetAutoPlay();
                ···    
            }
        }

        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            ···
            else if (args.PropertyName == VideoPlayer.SourceProperty.PropertyName)
            {
                SetSource();
            }
            else if (args.PropertyName == VideoPlayer.AutoPlayProperty.PropertyName)
            {
                SetAutoPlay();
            }
            ···
        }
        ···
    }
}
```

`SetSource` Obsługuje właściwość `UriVideoSource` obiektu przez ustawienie `Source` właściwość `MediaElement` do .NET `Uri` wartość lub `null` Jeśli `Source` właściwość `VideoPlayer` jest ustawiona na `null`:

```csharp
namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        ···
        async void SetSource()
        {
            bool hasSetSource = false;

            if (Element.Source is UriVideoSource)
            {
                string uri = (Element.Source as UriVideoSource).Uri;

                if (!String.IsNullOrWhiteSpace(uri))
                {
                    Control.Source = new Uri(uri);
                    hasSetSource = true;
                }
            }
            ···
            if (!hasSetSource)
            {
                Control.Source = null;
            }
        }

        void SetAutoPlay()
        {
            Control.AutoPlay = Element.AutoPlay;
        }
        ···
    }
}
```

## <a name="setting-a-url-source"></a>Ustawienie adresu URL źródła

Z implementacją tych właściwości w trzech programy renderujące jest możliwe do odtwarzania wideo ze źródła do adresu URL. **Odtwarzania filmu wideo z Internetu** strony w [ **VideoPlayDemos** ]( https://developer.xamarin.com/samples/xamarin-forms/customrenderers/videoplayerdemos/index.md) program jest definiowany przez następujący plik XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.PlayWebVideoPage"
             Title="Play Web Video">

    <video:VideoPlayer Source="https://archive.org/download/BigBuckBunny_328/BigBuckBunny_512kb.mp4" />

</ContentPage>
```

`VideoSourceConverter` Klasy konwertuje ciąg na `UriVideoSource`. Po przejściu do **odtwarzanie wideo w sieci Web** stronie wideo rozpocznie się ładowanie i rozpoczyna odtwarzanie po pobrane i buforowane wystarczającej ilości danych. Film wideo jest około 10 minut:

[![Odtwarzanie wideo w sieci Web](web-videos-images/playwebvideo-small.png "odtwarzanie wideo w sieci Web")](web-videos-images/playwebvideo-large.png#lightbox "odtwarzanie wideo w sieci Web")

Na wszystkich trzech platformach kontrolki transportu zanikanie Jeśli nie są używane, ale mogą zostać przywrócone, aby wyświetlić, wybierając wideo.

Możesz uniemożliwić wideo automatyczne uruchamianie, ustawiając `AutoPlay` właściwości `false`:

```xaml
<video:VideoPlayer Source="https://archive.org/download/BigBuckBunny_328/BigBuckBunny_512kb.mp4"
                   AutoPlay="false" />
```

Należy nacisnąć klawisz **Odtwórz** przycisk, aby rozpocząć film wideo.

Podobnie, można zapobiec wyświetlaniu kontrolki transportu, ustawiając `AreTransportControlsEnabled` właściwości `false`:

```xaml
<video:VideoPlayer Source="https://archive.org/download/BigBuckBunny_328/BigBuckBunny_512kb.mp4"
                   AreTransportControlsEnabled="False" />
```

Jeśli obie te właściwości jest ustawiona na `false`, a następnie wideo nie będzie rozpocząć odtwarzanie i nie będzie można uruchomić ją! Musisz wywołać `Play` z pliku związanego z kodem lub utworzyć własne kontrolki transportu, zgodnie z opisem w artykule [Implementowanie kontrolki transportu filmu wideo niestandardowe](custom-transport.md).

**App.xaml** plik zawiera zasoby dla dwóch dodatkowych filmów wideo:

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.App">
    <Application.Resources>
        <ResourceDictionary>

            <video:UriVideoSource x:Key="ElephantsDream"
                                  Uri="https://archive.org/download/ElephantsDream/ed_hd_512kb.mp4" />

            <video:UriVideoSource x:Key="BigBuckBunny"
                                  Uri="https://archive.org/download/BigBuckBunny_328/BigBuckBunny_512kb.mp4" />

            <video:UriVideoSource x:Key="Sintel"
                                  Uri="https://archive.org/download/Sintel/sintel-2048-stereo_512kb.mp4" />

        </ResourceDictionary>
    </Application.Resources>
</Application>
```

Do odwołania, jeden z tych innych filmów, można zastąpić jawne adresu URL w **PlayWebVideo.xaml** plik z `StaticResource` — rozszerzenie znaczników, w którym to przypadku `VideoSourceConverter` nie jest wymagane do utworzenia `UriVideoSource` obiektu:

```xaml
<video:VideoPlayer Source="{StaticResource ElephantsDream}" />
```

Alternatywnie, można ustawić `Source` właściwości z pliku wideo w `ListView`, zgodnie z opisem w następnym artykule [wiązanie źródeł wideo z odtwarzaczem](source-bindings.md).





## <a name="related-links"></a>Linki pokrewne

- [Wideo demonstracyjne Player (przykład)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
