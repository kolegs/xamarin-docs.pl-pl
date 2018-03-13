---
title: Odtwarzanie wideo w sieci Web
ms.topic: article
ms.prod: xamarin
ms.assetid: 75781A10-865D-4BA8-8D6B-E3DA012922BC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: a5a98df4346c8720ae25fae4f27b5294993111c4
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/12/2018
---
# <a name="playing-a-web-video"></a>Odtwarzanie wideo w sieci Web

`VideoPlayer` Klasa definiuje `Source` właściwość używana do określania źródła pliku wideo, a także `AutoPlay` właściwości. `AutoPlay` ma domyślne ustawienie `true`, co oznacza, że wideo należy rozpocząć odtwarzanie automatycznie po `Source` ustawiono:

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

`Source` Właściwość jest typu `VideoSource`, który jest deseniem po platformy Xamarin.Forms [ `ImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageSource/) abstrakcyjnej klasy, a jego trzy pochodne [ `UriImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.UriImageSource/), [ `FileImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FileImageSource/), i [ `StreamImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StreamImageSource/). Opcja strumienia, nie jest dostępna dla `VideoPlayer` jednak ponieważ systemów iOS i Android nie obsługuje odtwarzania wideo ze strumienia.

## <a name="video-sources"></a>Źródła wideo

Abstract `VideoSource` klasy składa się wyłącznie z trzech metod statycznych, które wystąpienia trzech klas, które pochodzą z `VideoSource`:

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

`UriVideoSource` Klasa jest używana do określenia plik wideo do pobrania z identyfikatora URI. Definiuje właściwości jednego typu `string`:

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

`ResourceVideoSource` Klasa jest używana do dostępu do plików wideo, są przechowywane jako zasoby osadzone w aplikacji platformy także określona z `string` właściwości:

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

Obsługa obiektów typu `ResourceVideoSource` jest opisana w artykule [ładowania aplikacji zasobów wideo](loading-resources.md). `VideoPlayer` Klasa ma możliwość załadowania pliku wideo przechowywane jako zasobu w bibliotece klas przenośnych.

`FileVideoSource` Klasa jest używana do dostępu do plików wideo z biblioteki wideo urządzenia. Jedną właściwość jest również typu `string`:

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

Obsługa obiektów typu `FileVideoSource` jest opisana w artykule [podczas uzyskiwania dostępu do biblioteki wideo urządzenia](accessing-library.md).

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

Ten konwerter typów jest wywoływane, gdy `Source` właściwość jest ustawiona na ciąg w języku XAML. Oto `VideoSourceConverter` klasy:

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

`ConvertFromInvariantString` Metoda próbuje przekonwertować ciągu do `Uri` obiektu. Jeśli który zakończy się pomyślnie, a nie `file:`, a następnie metoda zwraca `UriVideoSource`. W przeciwnym razie zwraca `ResourceVideoSource`.

## <a name="setting-the-video-source"></a>Ustawienie źródła wideo

Wszystkie inne logiki obejmujących źródła wideo jest zaimplementowana w renderowania poszczególnych platform. W poniższych sekcjach przedstawiono sposób renderowania platformy odtwarzania wideo podczas `Source` ma ustawioną właściwość `UriVideoSource` obiektu.

### <a name="the-ios-video-source"></a>Źródła wideo z systemem iOS

Dwie sekcje `VideoPlayerRenderer` obejmuje ustawienia źródła wideo odtwarzacza wideo. Gdy platformy Xamarin.Forms najpierw tworzy obiekt typu `VideoPlayer`, `OnElementChanged` metoda jest wywoływana z `NewElement` właściwość obiekt arguments wartość która `VideoPlayer`. `OnElementChanged` Wywołania metody `SetSource`:

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

Później na, kiedy `Source` właściwości zostanie zmieniona, `OnElementPropertyChanged` metoda jest wywoływana z `PropertyName` właściwości "Source", a `SetSource` nie zostanie ponownie wywołany.

Do odtwarzania pliku wideo w systemie iOS, typu obiektu [ `AVAsset` ](https://developer.xamarin.com/api/type/AVFoundation.AVAsset/) utworzenia hermetyzacji pliku wideo, i który jest używany do tworzenia [ `AVPlayerItem` ](https://developer.xamarin.com/api/type/AVFoundation.AVPlayerItem/), który jest następnie przekazany do `AVPlayer`obiektu. Oto jak `SetSource` dojścia metody `Source` właściwości, gdy jest on typu `UriVideoSource`:

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

`AutoPlay` Właściwości ma nie analogowy w klasach wideo z systemem iOS, więc właściwości się zbadana na końcu `SetSource` metodę do wywołania `Play` metoda `AVPlayer` obiektu.

W niektórych przypadkach wideo nadal odtwarzanie po stronie z `VideoPlayer` przejście na stronę główną. Aby zatrzymać wideo, `ReplaceCurrentItemWithPlayerItem` zostanie również ustawiona `Dispose` zastąpienia:

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

### <a name="the-android-video-source"></a>Android źródła wideo

Android `VideoPlayerRenderer` ustawić źródła wideo odtwarzacza podczas `VideoPlayer` jest najpierw utworzony i nowszych Jeśli `Source` zmian właściwości:

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

`SetSource` Metoda obsługuje obiektów typu `UriVideoSource` przez wywołanie metody `SetVideoUri` na `VideoView` z Android `Uri` obiektu utworzone na podstawie ciągu identyfikatora URI. `Uri` Klasy jest w pełni kwalifikowana tutaj odróżniający go od .NET `Uri` klasy:

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

Android `VideoView` nie ma odpowiedniego `AutoPlay` właściwości, więc `Start` metoda jest wywoływana, gdy ustawiono nowego pliku wideo.

Jeśli występuje różnica między zachowanie systemu IOS i Android renderowania `Source` właściwość `VideoPlayer` ma ustawioną wartość `null`, lub, jeśli `Uri` właściwość `UriVideoSource` ma ustawioną wartość `null` lub ciąg pusty. Jeśli odtwarzacza wideo z systemem iOS jest aktualnie odtwarzanie wideo, i `Source` ma ustawioną wartość `null` (lub ciąg jest `null` lub puste), `ReplaceCurrentItemWithPlayerItem` jest wywoływany z `null` wartość. Bieżący plik wideo jest zastępowany i zatrzymuje odtwarzanie.

Android nie obsługuje podobne funkcje. Jeśli `Source` właściwość jest ustawiona na `null`, `SetSource` po prostu ignoruje — metoda i bieżący plik wideo jest nadal odtwarzany.

### <a name="the-uwp-video-source"></a>Źródło obrazu platformy uniwersalnej systemu Windows

UWP `MediaElement` definiuje `AutoPlay` właściwość, która jest obsługiwane w module renderowania, podobnie jak inne właściwości:

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

`SetSource` Dojść właściwości `UriVideoSource` obiektu przez ustawienie `Source` właściwość `MediaElement` do .NET `Uri` wartość, lub do `null` Jeśli `Source` właściwość `VideoPlayer` ma ustawioną wartość `null`:

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

## <a name="setting-a-url-source"></a>Ustawienie źródła adresu URL

Wykonanie tych właściwości w trzech moduły renderowania prawdopodobnie odtwarzanie wideo z adres URL źródła. **Odtwarzania wideo w sieci Web** strony [ **VideoPlayDemos** ]( https://developer.xamarin.com/samples/xamarin-forms/customrenderers/videoplayerdemos/index.md) programu jest definiowana za pomocą następującego pliku XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.PlayWebVideoPage"
             Title="Play Web Video">

    <video:VideoPlayer Source="https://archive.org/download/BigBuckBunny_328/BigBuckBunny_512kb.mp4" />

</ContentPage>
```

`VideoSourceConverter` Klasy konwertuje ciąg na `UriVideoSource`. Po przejściu do **odtwarzania wideo w sieci Web** strony, wideo rozpoczyna się ładowania i rozpoczyna odtwarzanie po pobraniu i buforowane wystarczającej ilości danych. Plik wideo jest około 10 minut, długość:

[![Odtwarzanie wideo w sieci Web](web-videos-images/playwebvideo-small.png "odtwarzania wideo w sieci Web")](web-videos-images/playwebvideo-large.png#lightbox "odtwarzania wideo w sieci Web")

Na wszystkich platformach trzy formanty transportu zanikania się, jeśli nie są używane, ale mogą zostać przywrócone, aby wyświetlić, wybierając wideo.

Wideo uniemożliwia automatyczne uruchamianie ustawiając `AutoPlay` właściwości `false`:

```xaml
<video:VideoPlayer Source="https://archive.org/download/BigBuckBunny_328/BigBuckBunny_512kb.mp4"
                   AutoPlay="false" />
```

Musisz naciśnij **odtwarzanie** przycisk, aby uruchomić wideo.

Podobnie, można pominąć wyświetlania formantów transportu, ustawiając `AreTransportControlsEnabled` właściwości `false`:

```xaml
<video:VideoPlayer Source="https://archive.org/download/BigBuckBunny_328/BigBuckBunny_512kb.mp4"
                   AreTransportControlsEnabled="False" />
```

Jeśli ustawiono obie właściwości `false`, wideo nie Rozpocznij odtwarzanie i nie będzie można go uruchomić! Należy wywołać `Play` z pliku CodeBehind lub utworzyć własne kontrolki transportu, zgodnie z opisem w artykule [implementacja wideo transportu formanty](custom-transport.md). 

**App.xaml** plik zawiera zasoby dla dwóch dodatkowych plików wideo:

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

Odwołania do jednej z tych innych filmów, można zastąpić jawnego adresu URL na liście **PlayWebVideo.xaml** pliku z `StaticResource` — rozszerzenie znaczników, w którym to przypadku `VideoSourceConverter` nie jest wymagane do utworzenia `UriVideoSource` obiektu:

```xaml
<video:VideoPlayer Source="{StaticResource ElephantsDream}" />
```

Alternatywnie, można ustawić `Source` właściwości z pliku wideo w `ListView`, zgodnie z opisem w artykule [powiązania źródła wideo do odtwarzacza](source-bindings.md).





## <a name="related-links"></a>Linki pokrewne

- [Pokazy odtwarzacza wideo (przykład)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
