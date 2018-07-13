---
title: Kontrolki niestandardowe transportu filmu wideo
description: W tym artykule wyjaśniono, jak zaimplementować transport w niestandardowych formantów w aplikacji odtwarzacza wideo przy użyciu zestawu narzędzi Xamarin.Forms.
ms.prod: xamarin
ms.assetid: CE9E955D-A9AC-4019-A5D7-6390D80DECA1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: 84870de28ffd30b2d29fb5d8fbea815e1fd0d9c4
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996441"
---
# <a name="custom-video-transport-controls"></a>Kontrolki niestandardowe transportu filmu wideo

Kontrolki transportu odtwarzacza wideo obejmują przyciski, które wykonują funkcje **Odtwórz**, **Wstrzymaj**, i **zatrzymać**. Przyciski te zazwyczaj są identyfikowane za pomocą dobrze znanych ikony zamiast tekstu i **odtwarzania** i **Wstrzymaj** funkcje zwykle są połączone w jeden z przycisków.

Domyślnie `VideoPlayer` Wyświetla transportu kontrolek obsługiwanych przez każdej z platform. Po ustawieniu `AreTransportControlsEnabled` właściwości `false`, te kontrolki są pomijane. Pozwala to sterować `VideoPlayer` programowo lub podać własne kontrolki transportu.

## <a name="the-play-pause-and-stop-methods"></a>Odtwórz, Wstrzymaj i Zatrzymaj metody

`VideoPlayer` Klasa definiuje trzy metody o nazwie `Play`, `Pause`, i `Stop` implementowane przez wyzwalanie zdarzeń:

```csharp
namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
        public event EventHandler PlayRequested;

        public void Play()
        {
            PlayRequested?.Invoke(this, EventArgs.Empty);
        }

        public event EventHandler PauseRequested;

        public void Pause()
        {
            PauseRequested?.Invoke(this, EventArgs.Empty);
        }

        public event EventHandler StopRequested;

        public void Stop()
        {
            StopRequested?.Invoke(this, EventArgs.Empty);
        }
    }
}
```

Programy obsługi zdarzeń dla zdarzenia te są ustawiane przez `VideoPlayerRenderer` klasy w każdej z platform, jak pokazano poniżej:

### <a name="ios-transport-implementations"></a>iOS transportu implementacji

Wersja systemu iOS `VideoPlayerRenderer` używa `OnElementChanged` metodę, aby ustawić programy obsługi dla tych trzech zdarzeń podczas `NewElement` właściwość nie jest `null` oraz odłącza procedury obsługi zdarzeń podczas `OldElement` nie jest `null`:

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        AVPlayer player;
        ···
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                ···
                args.NewElement.PlayRequested += OnPlayRequested;
                args.NewElement.PauseRequested += OnPauseRequested;
                args.NewElement.StopRequested += OnStopRequested;
            }

            if (args.OldElement != null)
            {
                ···
                args.OldElement.PlayRequested -= OnPlayRequested;
                args.OldElement.PauseRequested -= OnPauseRequested;
                args.OldElement.StopRequested -= OnStopRequested;
            }
        }
        ···
        // Event handlers to implement methods
        void OnPlayRequested(object sender, EventArgs args)
        {
            player.Play();
        }

        void OnPauseRequested(object sender, EventArgs args)
        {
            player.Pause();
        }

        void OnStopRequested(object sender, EventArgs args)
        {
            player.Pause();
            player.Seek(new CMTime(0, 1));
        }
    }
}
```

Programy obsługi zdarzeń są implementowane przez wywołanie metody w `AVPlayer` obiektu. Istnieje nie `Stop` metodę `AVPlayer`, więc jest symulowane wstrzymywanie wideo i przenosząc pozycji na początku.

### <a name="android-transport-implementations"></a>Implementacje transportu dla systemu android

Implementacja systemu Android jest podobny do wykonania dla systemu iOS. Programy obsługi dla trzech funkcji są ustawiane podczas `NewElement` nie `null` i odłączyć, kiedy `OldElement` nie `null`:

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        VideoView videoView;
        ···
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                ···
                args.NewElement.PlayRequested += OnPlayRequested;
                args.NewElement.PauseRequested += OnPauseRequested;
                args.NewElement.StopRequested += OnStopRequested;
            }

            if (args.OldElement != null)
            {
                ···
                args.OldElement.PlayRequested -= OnPlayRequested;
                args.OldElement.PauseRequested -= OnPauseRequested;
                args.OldElement.StopRequested -= OnStopRequested;
            }
        }
        ···
        void OnPlayRequested(object sender, EventArgs args)
        {
            videoView.Start();
        }

        void OnPauseRequested(object sender, EventArgs args)
        {
            videoView.Pause();
        }

        void OnStopRequested(object sender, EventArgs args)
        {
            videoView.StopPlayback();
        }
    }
}
```

Trzy funkcje wywołań metody zdefiniowane przez `VideoView`.

### <a name="uwp-transport-implementations"></a>Implementacje transportu platformy uniwersalnej systemu Windows

Implementacja platformy uniwersalnej systemu Windows funkcji transportu trzy jest bardzo podobny do systemów iOS i Android implementacji:

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
                args.NewElement.PlayRequested += OnPlayRequested;
                args.NewElement.PauseRequested += OnPauseRequested;
                args.NewElement.StopRequested += OnStopRequested;
            }

            if (args.OldElement != null)
            {
                ···
                args.OldElement.PlayRequested -= OnPlayRequested;
                args.OldElement.PauseRequested -= OnPauseRequested;
                args.OldElement.StopRequested -= OnStopRequested;
            }
        }
        ···
        // Event handlers to implement methods
        void OnPlayRequested(object sender, EventArgs args)
        {
            Control.Play();
        }

        void OnPauseRequested(object sender, EventArgs args)
        {
            Control.Pause();
        }

        void OnStopRequested(object sender, EventArgs args)
        {
            Control.Stop();
        }
    }
}
```

## <a name="the-video-player-status"></a>Stan odtwarzacza wideo

Implementowanie **Odtwórz**, **Wstrzymaj**, i **zatrzymać** funkcje nie są wystarczające do obsługi formantów transportu. Często **Odtwórz** i **Wstrzymaj** polecenia są implementowane za pomocą tego samego przycisku, który zmienia jego wygląd, aby wskazać, czy wideo jest obecnie wstrzymana lub odtwarzania. Ponadto z przycisku nie powinien włączone nawet wideo nie została jeszcze załadowana.

Te wymagania oznacza, że odtwarzacza wideo musi udostępnić wskazująca bieżący stan przypadku jego odtwarzanie lub wstrzymana, lub jeśli nie jest jeszcze gotowy do odtwarzania wideo. (Trzech platformach obsługują także właściwości, które wskazują, jeśli wideo może być wstrzymana lub mogą zostać przeniesione do nowej pozycji, ale te właściwości są odpowiednie dla przesyłanie strumieniowe filmów wideo, a nie plików wideo, dzięki czemu nie są obsługiwane w `VideoPlayer` opisane w tym miejscu.)

**VideoPlayerDemos** projekt zawiera `VideoStatus` wyliczenie z trzema elementami członkowskimi:

```csharp
namespace FormsVideoLibrary
{
    public enum VideoStatus
    {
        NotReady,
        Playing,
        Paused
    }
}
```

`VideoPlayer` Klasy definiuje tylko do rzeczywistego powiązać właściwości o nazwie `Status` typu `VideoStatus`. Ta właściwość jest zdefiniowana jako tylko do odczytu, ponieważ może zostać ustawiona tylko od programu renderującego platformy:

```csharp
using System;
using Xamarin.Forms;

namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
        // Status read-only property
        private static readonly BindablePropertyKey StatusPropertyKey =
            BindableProperty.CreateReadOnly(nameof(Status), typeof(VideoStatus), typeof(VideoPlayer), VideoStatus.NotReady);

        public static readonly BindableProperty StatusProperty = StatusPropertyKey.BindableProperty;

        public VideoStatus Status
        {
            get { return (VideoStatus)GetValue(StatusProperty); }
        }

        VideoStatus IVideoPlayerController.Status
        {
            set { SetValue(StatusPropertyKey, value); }
            get { return Status; }
        }
        ···
    }
}
```

Zwykle, tylko do odczytu właściwości możliwej do wiązania miałby prywatnej `set` dostępu na `Status` właściwości, aby zezwalała na nelze nastavit od tej klasy. Aby uzyskać `View` utworów zależnych, obsługiwane przez programy renderujące, jednak musi być ustawiona właściwość z poza klasy, ale tylko przez moduł renderowania platformy.

Z tego powodu zdefiniowano innej właściwości o nazwie `IVideoPlayerController.Status`. To jest jawną implementacją interfejsu i jest możliwe, `IVideoPlayerController` interfejs, który `VideoPlayer` klasy implementuje:

```csharp
namespace FormsVideoLibrary
{
    public interface IVideoPlayerController
    {
        VideoStatus Status { set; get; }

        TimeSpan Duration { set; get; }
    }
}
```

Jest to podobne jak [ `WebView` ](xref:Xamarin.Forms.WebView) kontrolować używa [ `IWebViewController` ](xref:Xamarin.Forms.IWebViewController) interfejs do implementacji `CanGoBack` i `CanGoForward` właściwości. (Zobacz kod źródłowy [ `WebView` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Core/WebView.cs) i jego programy renderujące, aby uzyskać szczegółowe informacje.)

Dzięki temu możliwe dla klasy zewnętrzne w stosunku do `VideoPlayer` można ustawić `Status` właściwość przez odwołanie się do `IVideoPlayerController` interfejsu. (W wkrótce będzie wyświetlany jest kod.) Można ustawić właściwości, z również inne klasy, ale jest mało prawdopodobne, należy ustawić przypadkowo. Co najważniejsze `Status` właściwości nie można ustawić za pomocą powiązania danych.

Ułatwiają renderowania w ochronie to `Status` właściwość zaktualizowana, `VideoPlayer` klasa definiuje `UpdateStatus` zdarzeń, który jest wyzwalany co dziesiątej sekundy:

```csharp
namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        public event EventHandler UpdateStatus;

        public VideoPlayer()
        {
            Device.StartTimer(TimeSpan.FromMilliseconds(100), () =>
            {
                UpdateStatus?.Invoke(this, EventArgs.Empty);
                return true;
            });
        }
        ···
    }
}
```

### <a name="the-ios-status-setting"></a>Ustawienia stanu dla systemu iOS

Systemu iOS `VideoPlayerRenderer` Ustawia program obsługi `UpdateStatus` zdarzeń (oraz odłącza programu obsługi podczas bazowego `VideoPlayer` element nie istnieje) i używa program obsługi, aby ustawić `Status` właściwości:

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
                args.NewElement.UpdateStatus += OnUpdateStatus;
                ···
            }

            if (args.OldElement != null)
            {
                args.OldElement.UpdateStatus -= OnUpdateStatus;
                ···
            }
        }
        ···
        void OnUpdateStatus(object sender, EventArgs args)
        {
            VideoStatus videoStatus = VideoStatus.NotReady;

            switch (player.Status)
            {
                case AVPlayerStatus.ReadyToPlay:
                    switch (player.TimeControlStatus)
                    {
                        case AVPlayerTimeControlStatus.Playing:
                            videoStatus = VideoStatus.Playing;
                            break;

                        case AVPlayerTimeControlStatus.Paused:
                            videoStatus = VideoStatus.Paused;
                            break;
                    }
                    break;
                }
            }

            ((IVideoPlayerController)Element).Status = videoStatus;
            ···
        }
        ···
    }
}
```

Dwie właściwości `AVPlayer` muszą być dostępne: [ `Status` ](https://developer.xamarin.com/api/property/AVFoundation.AVPlayer.Status/) właściwości typu `AVPlayerStatus` i [ `TimeControlStatus` ](https://developer.xamarin.com/api/property/AVFoundation.AVPlayer.TimeControlStatus/) właściwości typu `AVPlayerTimeControlStatus`. Należy zauważyć, że `Element` właściwości (czyli `VideoPlayer`) musi być rzutowany `IVideoPlayerController` można ustawić `Status` właściwości.

### <a name="the-android-status-setting"></a>Ustawienia stanu dla systemu Android

[ `IsPlaying` ](https://developer.xamarin.com/api/property/Android.Widget.VideoView.IsPlaying/) Właściwość Android `VideoView` jest wartością logiczną, wskazującą, tylko jeśli wideo jest wstrzymana lub odtwarzania. Aby ustalić, czy `VideoView` można ani play ani wstrzymanie odtwarzania filmu od jeszcze `Prepared` zdarzenia `VideoView` muszą być obsługiwane. Te dwie procedury obsługi są ustawiane w `OnElementChanged` metody i odłączone podczas `Dispose` zastąpienia:

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        VideoView videoView;
        ···
        bool isPrepared;

        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                if (Control == null)
                {
                    ···
                    videoView.Prepared += OnVideoViewPrepared;
                    ···
                }
                ···
                args.NewElement.UpdateStatus += OnUpdateStatus;
                ···
            }

            if (args.OldElement != null)
            {
                args.OldElement.UpdateStatus -= OnUpdateStatus;
                ···
            }

        }

        protected override void Dispose(bool disposing)
        {
            if (Control != null && videoView != null)
            {
                videoView.Prepared -= OnVideoViewPrepared;
            }
            if (Element != null)
            {
                Element.UpdateStatus -= OnUpdateStatus;
            }

            base.Dispose(disposing);
        }
        ···
    }
}
```

`UpdateStatus` Program obsługi używa `isPrepared` pola (w `Prepared` obsługi) oraz `IsPlaying` właściwość umożliwiająca ustawienie `Status` właściwości:

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        VideoView videoView;
        ···
        bool isPrepared;
        ···
        void OnVideoViewPrepared(object sender, EventArgs args)
        {
            isPrepared = true;
            ···
        }
        ···
        void OnUpdateStatus(object sender, EventArgs args)
        {
            VideoStatus status = VideoStatus.NotReady;

            if (isPrepared)
            {
                status = videoView.IsPlaying ? VideoStatus.Playing : VideoStatus.Paused;
            }
            ···
        }
        ···
    }
}
```

### <a name="the-uwp-status-setting"></a>Ustawienia stanu platformy uniwersalnej systemu Windows

Platformy UWP `VideoPlayerRenderer` sprawia, że użycie `UpdateStatus` zdarzenia, ale nie musi ona ustawienie `Status` właściwości. `MediaElement` Definiuje [ `CurrentStateChanged` ](/uwp/api/windows.ui.xaml.controls.mediaelement#Windows_UI_Xaml_Controls_MediaElement_CurrentStateChanged) zdarzeń, który umożliwia modułu renderowania Aby otrzymywać powiadomienia, gdy [ `CurrentState` ](/uwp/api/windows.ui.xaml.controls.mediaelement#Windows_UI_Xaml_Controls_MediaElement_CurrentState) właściwości została zmieniona. Właściwość jest odłączana w `Dispose` zastąpienia:

```csharp
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
                    ···
                    mediaElement.CurrentStateChanged += OnMediaElementCurrentStateChanged;
                };
                ···
            }
            ···
        }

        protected override void Dispose(bool disposing)
        {
            if (Control != null)
            {
                ···
                Control.CurrentStateChanged -= OnMediaElementCurrentStateChanged;
            }

            base.Dispose(disposing);
        }
        ···
    }
}
```

`CurrentState` Właściwość jest typu [ `MediaElementState` ](/uwp/api/windows.ui.xaml.media.mediaelementstate)i łatwo mapuje do `VideoStatus`:

```csharp
namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        ···
        void OnMediaElementCurrentStateChanged(object sender, RoutedEventArgs args)
        {
            VideoStatus videoStatus = VideoStatus.NotReady;

            switch (Control.CurrentState)
            {
                case MediaElementState.Playing:
                    videoStatus = VideoStatus.Playing;
                    break;

                case MediaElementState.Paused:
                case MediaElementState.Stopped:
                    videoStatus = VideoStatus.Paused;
                    break;
            }

            ((IVideoPlayerController)Element).Status = videoStatus;
        }
        ···
    }
}
```

## <a name="play-pause-and-stop-buttons"></a>Odtwórz, Pauza i Zatrzymaj przycisków

Przy użyciu znaków Unicode dla symboliczne **Odtwórz**, **Wstrzymaj**, i **zatrzymać** obrazów jest problematyczne. [Różne techniczne](https://unicode-table.com/en/blocks/miscellaneous-technical/) sekcji w standardzie Unicode definiuje trzy znaki symboli pozornie odpowiednie do tego celu. Są to:

- 0x23F5 (czarne średnie skierowaną w prawo trójkąt) lub &#x23F5; dla **odtwarzania**
- 0x23F8 (Podwójna kreska pionowa) lub &#x23F8; dla **wstrzymania**
- 0x23F9 (czarne kwadrat) lub &#x23F9; dla **Stop**

Niezależnie od tego jak te symbole są wyświetlane w przeglądarce i różnych przeglądarek obsługiwać je na różne sposoby, nie są wyświetlane spójnie na platformach obsługiwanych przez zestaw narzędzi Xamarin.Forms. Na urządzeniach platformy uniwersalnej systemu Windows i iOS **Wstrzymaj** i **zatrzymać** znaki mają wygląd graficzny, z niebieskim tłem 3D i białe pierwszego planu. Nie jest to przypadek, w systemie Android, w której symbol jest po prostu niebieski. Jednak punktu kodu 0x23F5 dla **Odtwórz** nie ma ona, że tego samego wygląd na platformy uniwersalnej systemu Windows, a nawet jest obsługiwana w systemach iOS i Android.

Z tego powodu nie można używać punktu kodu 0x23F5 **Odtwórz**. Jest dobrą:

- 0x25B6 (czarny trójkąt skierowaną w prawo) lub &#x25B6; dla **odtwarzania**

Jest to obsługiwane przez wszystkich trzech platformach, z tą różnicą, że to zwykły czarny trójkąt, który wygląda inaczej niż wygląd 3D **Wstrzymaj** i **zatrzymać**. Jedną z możliwości jest postępuj zgodnie z punktu kodu 0x25B6 variant kodem:

- 0x25B6 następuje 0xFE0F (wariant 16) lub &#x25B6; &#xFE0F; dla **odtwarzania**

Jest to, co jest używany w znaczniku, pokazano poniżej. W systemach iOS, zapewnia **Odtwórz** symboli tego samego wygląd 3D jako **Wstrzymaj** i **zatrzymać** przyciski, ale wariant nie działa w systemach Android i platformy uniwersalnej systemu Windows.

**Transport w niestandardowych** stronie zestawy **AreTransportControlsEnabled** właściwości **false** i zawiera `ActivityIndicator` wyświetlany podczas ładowania wideo i dwa przyciski. `DataTrigger` obiekty są używane do włączania i wyłączania `ActivityIndicator` i przyciski i aby przełączyć się pierwszy przycisk między **Odtwórz** i **Wstrzymaj**:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.CustomTransportPage"
             Title="Custom Transport">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <video:VideoPlayer x:Name="videoPlayer"
                           Grid.Row="0"
                           AutoPlay="False"
                           AreTransportControlsEnabled="False"
                           Source="{StaticResource BigBuckBunny}" />

        <ActivityIndicator Grid.Row="0"
                           Color="Gray"
                           IsVisible="False">
            <ActivityIndicator.Triggers>
                <DataTrigger TargetType="ActivityIndicator"
                             Binding="{Binding Source={x:Reference videoPlayer},
                                               Path=Status}"
                             Value="{x:Static video:VideoStatus.NotReady}">
                    <Setter Property="IsVisible" Value="True" />
                    <Setter Property="IsRunning" Value="True" />
                </DataTrigger>
            </ActivityIndicator.Triggers>
        </ActivityIndicator>

        <StackLayout Grid.Row="1"
                     Orientation="Horizontal"
                     Margin="0, 10"
                     BindingContext="{x:Reference videoPlayer}">

            <Button Text="&#x25B6;&#xFE0F; Play"
                    HorizontalOptions="CenterAndExpand"
                    Clicked="OnPlayPauseButtonClicked">
                <Button.Triggers>
                    <DataTrigger TargetType="Button"
                                 Binding="{Binding Status}"
                                 Value="{x:Static video:VideoStatus.Playing}">
                        <Setter Property="Text" Value="&#x23F8; Pause" />
                    </DataTrigger>

                    <DataTrigger TargetType="Button"
                                 Binding="{Binding Status}"
                                 Value="{x:Static video:VideoStatus.NotReady}">
                        <Setter Property="IsEnabled" Value="False" />
                    </DataTrigger>
                </Button.Triggers>
            </Button>

            <Button Text="&#x23F9; Stop"
                    HorizontalOptions="CenterAndExpand"
                    Clicked="OnStopButtonClicked">
                <Button.Triggers>
                    <DataTrigger TargetType="Button"
                                 Binding="{Binding Status}"
                                 Value="{x:Static video:VideoStatus.NotReady}">
                        <Setter Property="IsEnabled" Value="False" />
                    </DataTrigger>
                </Button.Triggers>
            </Button>
        </StackLayout>
    </Grid>
</ContentPage>
```

Wyzwalacze danych są opisane szczegółowo w artykule [wyzwalaczy danych](~/xamarin-forms/app-fundamentals/triggers.md#data).

Plik związany z kodem ma programy obsługi dla przycisku `Clicked` zdarzenia:

```csharp
namespace VideoPlayerDemos
{
    public partial class CustomTransportPage : ContentPage
    {
        public CustomTransportPage()
        {
            InitializeComponent();
        }

        void OnPlayPauseButtonClicked(object sender, EventArgs args)
        {
            if (videoPlayer.Status == VideoStatus.Playing)
            {
                videoPlayer.Pause();
            }
            else if (videoPlayer.Status == VideoStatus.Paused)
            {
                videoPlayer.Play();
            }
        }

        void OnStopButtonClicked(object sender, EventArgs args)
        {
            videoPlayer.Stop();
        }
    }
}
```

Ponieważ `AutoPlay` ustawiono `false` w **CustomTransport.xaml** pliku, należy nacisnąć klawisz **Odtwórz** przycisk włączenie staje się rozpocząć wideo. Przyciski są zdefiniowane tak, aby ich odpowiedniki tekstowe towarzyszy znaków Unicode, omówione powyżej. Przyciski mieć spójny wygląd na każdej platformie, podczas odtwarzania wideo:

[![Odtwarzanie Transport w niestandardowych](custom-transport-images/customtransportplaying-small.png "odtwarzanie Transport w niestandardowych")](custom-transport-images/customtransportplaying-large.png#lightbox "odtwarzanie Transport w niestandardowych")

Ale w systemach Android i platformy uniwersalnej systemu Windows **Odtwórz** przycisk wygląda bardzo różnią się, gdy wideo jest wstrzymane:

[![Wstrzymano Transport w niestandardowych](custom-transport-images/customtransportpaused-small.png "wstrzymana Transport w niestandardowych")](custom-transport-images/customtransportpaused-large.png#lightbox "wstrzymana Transport w niestandardowych")

W przypadku aplikacji produkcyjnej będzie prawdopodobnie chcesz użyć własne obrazy mapy bitowej dla przycisków, aby osiągnąć zgodność visual.


## <a name="related-links"></a>Linki pokrewne

- [Wideo demonstracyjne Player (przykład)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
