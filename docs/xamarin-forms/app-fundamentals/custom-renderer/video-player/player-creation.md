---
title: Tworzenie odtwarzaczy wideo platformy
ms.prod: xamarin
ms.assetid: EEE2FB9B-EB73-4A3F-A859-7A1D4808E149
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: 9efdc8376d9970cb429654e3d3aa2eef75ac2996
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/10/2018
---
# <a name="creating-the-platform-video-players"></a>Tworzenie odtwarzaczy wideo platformy

[ **VideoPlayerDemos** ](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/) rozwiązanie zawiera cały kod do implementacji odtwarzacza wideo dla platformy Xamarin.Forms. Obejmuje on też serii strony, który demonstruje sposób używania odtwarzacza wideo w aplikacji. Wszystkie `VideoPlayer` kodu i jego renderowania platformy znajdują się w folderach projektu o nazwie `FormsVideoLibrary`, a także korzystać z przestrzeni nazw `FormsVideoLibrary`. To powinno ułatwiają skopiuj pliki do swojej aplikacji i referencyjne klasy.

## <a name="the-video-player"></a>Odtwarzacza wideo

[ `VideoPlayer` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/CustomRenderers/VideoPlayerDemos/VideoPlayerDemos/VideoPlayerDemos/VideoPlayer.cs) Klasy jest częścią **VideoPlayerDemos** biblioteki standardowej .NET, współużytkowany platformy. Dziedziczy `View`:

```csharp
using System;
using Xamarin.Forms;

namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
    }
}
```

Elementy członkowskie tej klasy (i `IVideoPlayerController` interfejsu) są opisane w artykułach, które należy wykonać.

Każdy z trzech platform zawiera klasę o nazwie `VideoPlayerRenderer` zawierający kod specyficzne dla platformy, aby zaimplementować odtwarzacza wideo. Głównym zadaniem tego modułu renderowania ma utworzyć odtwarzacza wideo dla tej platformy.

### <a name="the-ios-player-view-controller"></a>Kontroler widoku player systemu iOS

Kilka klas są wykorzystywane podczas implementowania odtwarzacza wideo w systemie iOS. Aplikacja najpierw tworzy [ `AVPlayerViewController` ](https://developer.xamarin.com/api/type/AVKit.AVPlayerViewController/) , a następnie ustawia [ `Player` ](https://developer.xamarin.com/api/property/AVKit.AVPlayerViewController.Player/) dla właściwości typu obiektu [ `AVPlayer` ](https://developer.xamarin.com/api/type/AVFoundation.AVPlayer/). Odtwarzacz przypisany źródła wideo, wymagane są dodatkowe klasy.

Wszystkie moduły renderowania, iOS, takich jak [ `VideoPlayerRenderer` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/CustomRenderers/VideoPlayerDemos/VideoPlayerDemos/VideoPlayerDemos.iOS/VideoPlayerRenderer.cs) zawiera `ExportRenderer` atrybut, który identyfikuje `VideoPlayer` widok z renderującego:

```csharp
using System;
using System.ComponentModel;
using System.IO;

using AVFoundation;
using AVKit;
using CoreMedia;
using Foundation;
using UIKit;

using Xamarin.Forms;
using Xamarin.Forms.Platform.iOS;

[assembly: ExportRenderer(typeof(FormsVideoLibrary.VideoPlayer),
                          typeof(FormsVideoLibrary.iOS.VideoPlayerRenderer))]

namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
    }
}
```

Zazwyczaj renderowania, która ustawia kontrolkę platformy pochodzi z [ `ViewRenderer<View, NativeView>` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.iOS/ViewRenderer.cs) klasy, gdzie `View` jest platformy Xamarin.Forms `View` pochodną (w tym przypadku `VideoPlayer`) i `NativeView` jest systemu iOS `UIView` pochodnych dla klasy renderowania. Moduł renderowania, że argument rodzajowy po prostu ustawiono `UIView`, ze względów pojawi się wkrótce.

Podczas renderowania opiera się na `UIViewController` zależnych (tą jest), a następnie klasy powinny zastępować `ViewController` właściwości i przywracać kontroler widoku, w tym przypadku `AVPlayerViewController`. Będący celem `_playerViewController` pola:

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        AVPlayer player;
        AVPlayerItem playerItem;
        AVPlayerViewController _playerViewController;       // solely for ViewController property

        public override UIViewController ViewController => _playerViewController;

        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            base.OnElementChanged(args);

            if (args.NewElement != null)
            {
                if (Control == null)
                {
                    // Create AVPlayerViewController
                    _playerViewController = new AVPlayerViewController();

                    // Set Player property to AVPlayer
                    player = new AVPlayer();
                    _playerViewController.Player = player;

                    // Use the View from the controller as the native control
                    SetNativeControl(_playerViewController.View);
                }
                ···
            }
        }
        ···
    }
}
```

Podstawowym obowiązkiem `OnElementChanged` jest zastąpienie, aby sprawdzić, czy `Control` właściwość jest `null` i jeśli tak, Utwórz formant platformy i przekaż go do `SetNativeControl` metody. W takim przypadku tego obiektu jest dostępna tylko z `View` właściwość `AVPlayerViewController`. Czy `UIView` zależnych stanie się być Klasa prywatna o nazwie `AVPlayerView`, ale ponieważ jest to element prywatny, nie można jawnie określony jako drugi argument rodzajowy `ViewRenderer`.

Zazwyczaj `Control` właściwość klasy renderowania później odwołuje się do `UIView` używaną do zaimplementowania renderowania, w tym przypadku `Control` właściwość nie jest używana w innym miejscu.

### <a name="the-android-video-view"></a>Android widoku wideo

Android renderowania dla `VideoPlayer` opiera się na Android [ `VideoView` ](https://developer.xamarin.com/api/type/Android.Widget.VideoView/) klasy. Jednak jeśli `VideoView` jest używany przez samego siebie do odtwarzania wideo w aplikacji platformy Xamarin.Forms wideo wypełnienia przydzielonym obszar dla `VideoPlayer` bez zachowania prawidłowy współczynnik proporcji. Dla tego przyczyny (jak pojawi się wkrótce), `VideoView` staje się elementem podrzędnym Android `RelativeLayout`. A `using` dyrektywa definiuje `ARelativeLayout` odróżniający go od platformy Xamarin.Forms `RelativeLayout`, którym jest drugi argument rodzajowy `ViewRenderer`:

```csharp
using System;
using System.ComponentModel;
using System.IO;

using Android.Content;
using Android.Media;
using Android.Widget;
using ARelativeLayout = Android.Widget.RelativeLayout;

using Xamarin.Forms;
using Xamarin.Forms.Platform.Android;

[assembly: ExportRenderer(typeof(FormsVideoLibrary.VideoPlayer),
                          typeof(FormsVideoLibrary.Droid.VideoPlayerRenderer))]

namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        ···
        public VideoPlayerRenderer(Context context) : base(context)
        {
        }
        ···
    }
}
```

Począwszy od platformy Xamarin.Forms 2.5 Android renderowania powinna zawierać konstruktora z `Context` argumentu.

`OnElementChanged` Zastąpienie tworzy zarówno `VideoView` i `RelativeLayout` i ustawia parametry układu `VideoView` do niego w Centrum `RelativeLayout`.


```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        VideoView videoView;
        MediaController mediaController;    // Used to display transport controls
        bool isPrepared;
        ···
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            base.OnElementChanged(args);

            if (args.NewElement != null)
            {
                if (Control == null)
                {
                    // Save the VideoView for future reference
                    videoView = new VideoView(Context);

                    // Put the VideoView in a RelativeLayout
                    ARelativeLayout relativeLayout = new ARelativeLayout(Context);
                    relativeLayout.AddView(videoView);

                    // Center the VideoView in the RelativeLayout
                    ARelativeLayout.LayoutParams layoutParams =
                        new ARelativeLayout.LayoutParams(LayoutParams.MatchParent, LayoutParams.MatchParent);
                    layoutParams.AddRule(LayoutRules.CenterInParent);
                    videoView.LayoutParameters = layoutParams;

                    // Handle a VideoView event
                    videoView.Prepared += OnVideoViewPrepared;

                    // Use the RelativeLayout as the native control
                    SetNativeControl(relativeLayout);
                }
                ···
            }
            ···
        }

        protected override void Dispose(bool disposing)
        {
            if (Control != null && videoView != null)
            {
                videoView.Prepared -= OnVideoViewPrepared;
            }
            base.Dispose(disposing);
        }

        void OnVideoViewPrepared(object sender, EventArgs args)
        {
            isPrepared = true;
            ···
        }
        ···
    }
}
```

Program obsługi `Prepared` zdarzeń jest podłączony w tej metodzie i odłączyć w `Dispose` metody. To zdarzenie jest wywoływane, gdy `VideoView` ma wystarczającej ilości informacji, aby rozpocząć odtwarzanie pliku wideo.

### <a name="the-uwp-media-element"></a>Element multimediów platformy uniwersalnej systemu Windows

W systemie Windows platformy Uniwersalnej, najczęściej odtwarzacza wideo jest [ `MediaElement` ](/uwp/api/Windows.UI.Xaml.Controls.MediaElement/). Tej dokumentacji `MediaElement` oznacza to, że [ `MediaPlayerElement` ](/uwp/api/windows.ui.xaml.controls.mediaplayerelement/) należy użyć, gdy jest tylko do obsługi wersji systemu Windows 10, począwszy od 1607 kompilacji.

`OnElementChanged` Zastąpienie musi utworzyć `MediaElement`, ustaw kilka programów obsługi zdarzeń i przekazać `MediaElement` do obiektu `SetNativeControl`:

```csharp
using System;
using System.ComponentModel;

using Windows.Storage;
using Windows.Storage.Streams;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Media;

using Xamarin.Forms;
using Xamarin.Forms.Platform.UWP;

[assembly: ExportRenderer(typeof(FormsVideoLibrary.VideoPlayer),
                          typeof(FormsVideoLibrary.UWP.VideoPlayerRenderer))]

namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            base.OnElementChanged(args);

            if (args.NewElement != null)
            {
                if (Control == null)
                {
                    MediaElement mediaElement = new MediaElement();
                    SetNativeControl(mediaElement);

                    mediaElement.MediaOpened += OnMediaElementMediaOpened;
                    mediaElement.CurrentStateChanged += OnMediaElementCurrentStateChanged;
                }
                ···
            }
            ···
        }

        protected override void Dispose(bool disposing)
        {
            if (Control != null)
            {
                Control.MediaOpened -= OnMediaElementMediaOpened;
                Control.CurrentStateChanged -= OnMediaElementCurrentStateChanged;
            }

            base.Dispose(disposing);
        }        
        ···
    }
}
```

Programy obsługi dwóch zdarzeń są odłączone w `Dispose` zdarzenia dla renderującego.

## <a name="showing-the-transport-controls"></a>Wyświetlanie kontroli transportu

Odtwarzaczy wideo zawarte w trzech platformy obsługuje domyślny zestaw transportu formantów, które obejmują przycisków odtwarzanie wstrzymywanie i pasek, aby wskazać bieżącą pozycję w wideo i przenieść do nowej pozycji.

`VideoPlayer` Klasa definiuje właściwość o nazwie `AreTransportControlsEnabled` i ustawia wartość domyślną `true`:


```csharp
namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
        // AreTransportControlsEnabled property
        public static readonly BindableProperty AreTransportControlsEnabledProperty =
            BindableProperty.Create(nameof(AreTransportControlsEnabled), typeof(bool), typeof(VideoPlayer), true);

        public bool AreTransportControlsEnabled
        {
            set { SetValue(AreTransportControlsEnabledProperty, value); }
            get { return (bool)GetValue(AreTransportControlsEnabledProperty); }
        }
        ···
    }
}
```

Mimo że ta właściwość ma zarówno atrybut `set` i `get` metody dostępu, renderującego musi obsługiwać przypadków tylko wtedy, gdy właściwość jest ustawiona. `get` Akcesor po prostu zwraca bieżącą wartość właściwości.

Właściwości, takie jak `AreTransportControlsEnabled` będą obsługiwane platformy renderowania na dwa sposoby:

- Po raz pierwszy jest podczas tworzenia platformy Xamarin.Forms `VideoPlayer` elementu. Jest to wskazane `OnElementChanged` zastąpienie renderującego podczas `NewElement` właściwość nie jest `null`. W tej chwili można ustawić renderującego jest własnych platformy odtwarzacza wideo z początkowa wartość właściwości zgodnie z definicją w `VideoPlayer`.

- Jeśli właściwość w `VideoPlayer` zmieni się później, a następnie `OnElementPropertyChanged` wywołanie metody w module renderowania. Umożliwia renderowanie zaktualizować odtwarzacza wideo platformy na podstawie nowe ustawienia właściwości.

Oto jak `AreTransportControlsEnabled` właściwości jest obsługiwany na platformach trzy:

### <a name="ios-playback-controls"></a>formanty odtwarzania z systemem iOS

Właściwości systemu IOS `AVPlayerViewController` który kontroluje wyświetlanie transportu jest formanty [ `ShowsPlaybackControls` ](https://developer.xamarin.com/api/property/AVKit.AVPlayerViewController.ShowsPlaybackControls/). Oto jak tej właściwości jest ustawiana w sklepie iOS `VideoViewRenderer`:

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
        AVPlayerViewController _playerViewController;       // solely for ViewController property

        public override UIViewController ViewController => _playerViewController;

        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                ···
                SetAreTransportControlsEnabled();
                ···
            }
        }

        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged(sender, args);

            if (args.PropertyName == VideoPlayer.AreTransportControlsEnabledProperty.PropertyName)
            {
                SetAreTransportControlsEnabled();
            }
            ···
        }

        void SetAreTransportControlsEnabled()
        {
            ((AVPlayerViewController)ViewController).ShowsPlaybackControls = Element.AreTransportControlsEnabled;
        }
        ···
    }
}
```

`Element` Właściwość programu renderującego odwołuje się do `VideoPlayer` klasy.

### <a name="the-android-media-controller"></a>Kontroler nośnika systemu Android

W systemie Android, wyświetlanie elementów sterujących transport wymaga utworzenia [ `MediaController` ](https://developer.xamarin.com/api/type/Android.Widget.MediaController/) obiekt i kojarzenie go z `VideoView` obiektu. Sposobu przedstawiono w części `SetAreTransportControlsEnabled` metody:

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        VideoView videoView;
        MediaController mediaController;    // Used to display transport controls
        bool isPrepared;

        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                ···
                SetAreTransportControlsEnabled();
                ···
            }
        }

        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged(sender, args);

            if (args.PropertyName == VideoPlayer.AreTransportControlsEnabledProperty.PropertyName)
            {
                SetAreTransportControlsEnabled();
            }
            ···
        }

        void SetAreTransportControlsEnabled()
        {
            if (Element.AreTransportControlsEnabled)
            {
                mediaController = new MediaController(Context);
                mediaController.SetMediaPlayer(videoView);
                videoView.SetMediaController(mediaController);
            }
            else
            {
                videoView.SetMediaController(null);

                if (mediaController != null)
                {
                    mediaController.SetMediaPlayer(null);
                    mediaController = null;
                }
            }
        }
        ···
    }
}
```

### <a name="the-uwp-transport-controls-property"></a>Właściwości formantów transportu platformy uniwersalnej systemu Windows

UWP `MediaElement` definiuje właściwość o nazwie [ `AreTransportControlsEnabled` ](/uwp/api/windows.ui.xaml.controls.mediaelement#Windows_UI_Xaml_Controls_MediaElement_AreTransportControlsEnabled), dzięki czemu ustawiono właściwość z `VideoPlayer` właściwość o tej samej nazwie:

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
                SetAreTransportControlsEnabled();
                ···
            }
        }

        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged(sender, args);

            if (args.PropertyName == VideoPlayer.AreTransportControlsEnabledProperty.PropertyName)
            {
                SetAreTransportControlsEnabled();
            }
            ···
        }

        void SetAreTransportControlsEnabled()
        {
            Control.AreTransportControlsEnabled = Element.AreTransportControlsEnabled;
        }
        ···
    }
}
```

Co więcej właściwości jest niezbędne rozpocząć odtwarzanie wideo: jest to ważne `Source` właściwość, która odwołuje się do pliku wideo. Implementowanie `Source` właściwości jest opisana w artykule [odtwarzanie wideo w sieci Web](web-videos.md).


## <a name="related-links"></a>Linki pokrewne

- [Pokazy odtwarzacza wideo (przykład)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
