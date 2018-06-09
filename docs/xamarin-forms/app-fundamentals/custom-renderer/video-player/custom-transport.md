---
title: Formanty niestandardowe transportu wideo
description: W tym artykule opisano sposób wdrożenia formantów niestandardowych transportu w aplikacji odtwarzacza wideo, za pomocą platformy Xamarin.Forms.
ms.prod: xamarin
ms.assetid: CE9E955D-A9AC-4019-A5D7-6390D80DECA1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: a20c68d5f86dad852a4425206846292c1c6c5838
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241663"
---
# <a name="custom-video-transport-controls"></a>Formanty niestandardowe transportu wideo

Formanty transportu odtwarzacza wideo obejmują przycisków wykonywania funkcji **odtwarzanie**, **Wstrzymaj**, i **zatrzymać**. Przyciski te zwykle są oznaczone symbolem znane ikony zamiast tekstu i **odtwarzanie** i **Wstrzymaj** funkcje zazwyczaj są połączone w jeden z przycisków.

Domyślnie `VideoPlayer` Wyświetla transportu formanty obsługiwane przez poszczególnych platform. Podczas ustawiania `AreTransportControlsEnabled` właściwości `false`, będą pomijane tych kontrolek. Następnie można kontrolować `VideoPlayer` programowo lub podać własne kontrolki transportu.

## <a name="the-play-pause-and-stop-methods"></a>Metody Play, Wstrzymaj i Zatrzymaj

`VideoPlayer` Klasa definiuje trzy metody o nazwie `Play`, `Pause`, i `Stop` który są implementowane przez wyzwalania zdarzenia:

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

### <a name="ios-transport-implementations"></a>implementacje transportu z systemem iOS

Wersja systemu iOS `VideoPlayerRenderer` używa `OnElementChanged` metodę, aby ustawić programy obsługi dla tych trzech zdarzeń po `NewElement` właściwość nie jest `null` i odłącza obsługi zdarzeń podczas `OldElement` nie jest `null`:

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

Programy obsługi zdarzeń są implementowane przez wywołanie metody na `AVPlayer` obiektu. Brak nie `Stop` metodę `AVPlayer`, więc jest symulowany wstrzymywania wideo i przeniesienie pozycji na początku.

### <a name="android-transport-implementations"></a>Implementacje transportu dla systemu android

Implementacja systemu Android jest podobna do wykonania dla systemu iOS. Programy obsługi trzy funkcje są ustawiane podczas `NewElement` nie jest `null` i odłączyć, kiedy `OldElement` nie jest `null`:

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

Implementacja platformy uniwersalnej systemu Windows funkcji transportu trzy jest bardzo podobny do implementacji systemu Android i iOS:

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

Implementowanie **odtwarzanie**, **Wstrzymaj**, i **zatrzymać** funkcje nie są wystarczające do obsługi kontroli transportu. Często **odtwarzanie** i **Wstrzymaj** polecenia są implementowane przy użyciu tego samego przycisku, który zmienia jego wygląd, aby wskazać, czy plik wideo jest aktualnie wstrzymana lub odtwarzania. Ponadto przycisku nawet nie powinny być włączone, jeśli wideo nie została jeszcze załadowana.

Te wymagania oznaczać odtwarzacza wideo należy udostępnić wskazująca bieżący stan, jeśli jego odtwarzanie lub wstrzymana lub nie jest jeszcze gotowa do odtwarzania wideo. (Trzy platformy obsługują także właściwości, które wskazują, czy plik wideo można wstrzymywać lub mogą zostać przeniesione do nowej pozycji, ale te właściwości są stosowane do strumieniowego przesyłania wideo, a nie plików wideo, więc nie są obsługiwane w `VideoPlayer` opisanych tutaj.)

**VideoPlayerDemos** projekt zawiera `VideoStatus` wyliczenie z trzech elementów członkowskich:

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

`VideoPlayer` Klasa definiuje właściwość można powiązać tylko do rzeczywistego o nazwie `Status` typu `VideoStatus`. Ta właściwość jest zdefiniowana jako tylko do odczytu, ponieważ może zostać ustawiona tylko z renderowania platformy:

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

Zwykle, które można powiązać właściwości tylko do odczytu musi prywatnej `set` akcesor na `Status` właściwości, aby zezwalała na można ustawić z należące do klasy. Aby uzyskać `View` zależnych obsługiwanych przez moduły renderowania, jednak należy ustawić właściwość z poza klasą, ale tylko przez moduł renderowania platformy.

Z tego powodu zdefiniowano inna właściwość o nazwie `IVideoPlayerController.Status`. To jest jawnej implementacji interfejsu i jest możliwe przez `IVideoPlayerController` interfejsu `VideoPlayer` implementuje klasy:

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

Ten jest podobny do sposobu [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) kontrolować używa [ `IWebViewController` ](https://developer.xamarin.com/api/type/Xamarin.Forms.IWebViewController/) interfejs do implementacji `CanGoBack` i `CanGoForward` właściwości. (Zobacz kod źródłowy [ `WebView` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Core/WebView.cs) i jego moduły renderowania, aby uzyskać więcej informacji.)

Dzięki temu klasy zewnętrzne `VideoPlayer` można ustawić `Status` właściwość przez odwołanie `IVideoPlayerController` interfejsu. (Zobaczysz kod wkrótce.) Można ustawić właściwości z również inne klasy, ale jest mało prawdopodobne, należy ustawić wartość przypadkowo. Przede wszystkim `Status` nie można ustawić właściwości za pośrednictwem powiązania danych.

Ułatwienie renderowania w zapewnieniu to `Status` właściwość zaktualizowana, `VideoPlayer` klasa definiuje `UpdateStatus` zdarzenie wyzwalane co dziesiątym dniu sekundy:

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

### <a name="the-ios-status-setting"></a>Ustawienie stanu systemu iOS

IOS `VideoPlayerRenderer` Ustawia program obsługi dla `UpdateStatus` zdarzenia (i odłącza programu obsługi podczas odpowiadającego `VideoPlayer` element nie istnieje) i używa programu obsługi, aby ustawić `Status` właściwości:

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

Dwie właściwości `AVPlayer` muszą być dostępne: [ `Status` ](https://developer.xamarin.com/api/property/AVFoundation.AVPlayer.Status/) właściwości typu `AVPlayerStatus` i [ `TimeControlStatus` ](https://developer.xamarin.com/api/property/AVFoundation.AVPlayer.TimeControlStatus/) właściwości typu `AVPlayerTimeControlStatus`. Zwróć uwagę, że `Element` właściwości (czyli `VideoPlayer`) musi być rzutowane na `IVideoPlayerController` można ustawić `Status` właściwości.

### <a name="the-android-status-setting"></a>Ustawienie stanu systemu Android

[ `IsPlaying` ](https://developer.xamarin.com/api/property/Android.Widget.VideoView.IsPlaying/) Właściwości Android `VideoView` jest wartością logiczną, wskazującą, tylko jeśli wideo jest wstrzymana lub odtwarzania. Aby ustalić, czy `VideoView` można ani play ani wstrzymać wideo jeszcze `Prepared` zdarzenie `VideoView` muszą być obsługiwane. Te dwa programy obsługi są ustawiane w `OnElementChanged` metody i odłączone podczas `Dispose` zastąpienia:

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

`UpdateStatus` Program obsługi używa `isPrepared` pola (w `Prepared` obsługi) i `IsPlaying` właściwości można ustawić `Status` właściwości:

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

### <a name="the-uwp-status-setting"></a>Ustawienie stanu platformy uniwersalnej systemu Windows

UWP `VideoPlayerRenderer` sprawia, że użycie `UpdateStatus` zdarzeń, ale nie musi ona ustawienia `Status` właściwości. `MediaElement` Definiuje [ `CurrentStateChanged` ](/uwp/api/windows.ui.xaml.controls.mediaelement#Windows_UI_Xaml_Controls_MediaElement_CurrentStateChanged) zdarzenie, które umożliwia renderującego zgłaszane po [ `CurrentState` ](/uwp/api/windows.ui.xaml.controls.mediaelement#Windows_UI_Xaml_Controls_MediaElement_CurrentState) właściwość zostanie zmieniona. Właściwość jest odłączana w `Dispose` zastąpienia:

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

`CurrentState` Właściwość jest typu [ `MediaElementState` ](/uwp/api/windows.ui.xaml.media.mediaelementstate)i łatwo mapuje na `VideoStatus`:

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

## <a name="play-pause-and-stop-buttons"></a>Odtwórz, Wstrzymaj i Zatrzymaj przycisków

Przy użyciu znaków Unicode dla symboliczne **odtwarzanie**, **Wstrzymaj**, i **zatrzymać** obrazów jest problematyczne. [Różne techniczne](https://unicode-table.com/en/blocks/miscellaneous-technical/) sekcji standardu Unicode określa trzy znaki symboli pozornie odpowiednie do tego celu. Są to:

- 0x23F5 (czarny średnia wskazująca w prawo trójkąt) lub &#x23F5; dla **odtwarzania**
- 0x23F8 (dwa razy pionowy pasek) lub &#x23F8; dla **Wstrzymaj**
- 0x23F9 (czarny kwadrat) lub &#x23F9; dla **zatrzymania**

Niezależnie od sposobu te symbole są wyświetlane w przeglądarce (i różnych przeglądarkach obsługi je na różne sposoby), nie są wyświetlane spójnie na platformach obsługiwanych przez platformy Xamarin.Forms. W systemach iOS i urządzenia platformy uniwersalnej systemu Windows **Wstrzymaj** i **zatrzymać** znaków ma wygląd graficzny, niebieski tła 3W i białe pierwszego planu. To nie jest w systemie Android, w którym symbol jest po prostu niebieski. Jednak 0x23F5 punktów kodowych znaków dwuskładnikowych dla **odtwarzanie** nie ma czy sam wygląd na platformy uniwersalnej systemu Windows, a nawet nie jest obsługiwana w systemach iOS i Android.

Z tego powodu nie można używać punktów 0x23F5 **odtwarzanie**. Dobrym zastępuje jest:

- 0x25B6 (czarny trójkąt wskazująca w prawo) lub &#x25B6; dla **odtwarzania**

Jest to obsługiwane przez wszystkie trzy platformy, z tą różnicą, że jest zwykły trójkąt czarny, który wygląda inaczej wygląd 3D **Wstrzymaj** i **zatrzymać**. Wykonaj 0x25B6 punktów kodowych znaków dwuskładnikowych z kodem wariantu jest jedną z możliwości:

- 0x25B6 następuje 0xFE0F (wariant 16) lub &#x25B6; &#xFE0F; dla **odtwarzania**

Jest to, co jest używany w znaczniku pokazano poniżej. W systemach iOS, daje **odtwarzanie** symbolu sam wygląd 3D jako **Wstrzymaj** i **zatrzymać** przycisków, ale wariantu nie działa w systemach Android i platformy uniwersalnej systemu Windows.

**Transportu niestandardowe** strony zestawy **AreTransportControlsEnabled** właściwości **false** i zawiera `ActivityIndicator` wyświetlany podczas ładowania wideo i dwa przyciski. `DataTrigger` obiekty służą do włączania i wyłączania `ActivityIndicator` i przyciski i aby przełączyć przycisku pierwszej między **odtwarzanie** i **Wstrzymaj**:

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

Wyzwalacze danych opisano szczegółowo w artykule [wyzwalaczy danych](~/xamarin-forms/app-fundamentals/triggers.md#data).

Programy obsługi dla przycisku ma pliku CodeBehind `Clicked` zdarzenia:

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

Ponieważ `AutoPlay` ustawiono `false` w **CustomTransport.xaml** pliku, konieczne będzie naciśnij **odtwarzania** przycisk włączenie staje się rozpocząć wideo. Przyciski są zdefiniowane tak, aby ich odpowiedniki tekstu towarzyszy znaków Unicode opisanych wyżej. Przyciski ma spójny wygląd na każdej platformie, podczas odtwarzania wideo:

[![Odtwarzanie niestandardowych transportu](custom-transport-images/customtransportplaying-small.png "odtwarzanie niestandardowych transportu")](custom-transport-images/customtransportplaying-large.png#lightbox "odtwarzanie transportu niestandardowych")

Jednak w systemach Android i platformy uniwersalnej systemu Windows **odtwarzanie** przycisk wygląda bardzo różnią się, gdy wideo jest wstrzymane:

[![Niestandardowe transportu wstrzymana](custom-transport-images/customtransportpaused-small.png "wstrzymana niestandardowych transportu")](custom-transport-images/customtransportpaused-large.png#lightbox "wstrzymana transportu niestandardowych")

W aplikacji produkcyjnej prawdopodobnie należy do wykorzystania własnych obrazów mapy bitowej przycisków do osiągnięcia jednolitość visual.


## <a name="related-links"></a>Linki pokrewne

- [Pokazy odtwarzacza wideo (przykład)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
