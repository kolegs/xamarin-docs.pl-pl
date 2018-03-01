---
title: Niestandardowe pozycjonowanie wideo
ms.topic: article
ms.prod: xamarin
ms.assetid: 6D792264-30FF-46F7-8C1B-2FEF9D277DF4
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: d4446d30491ee796ca93eadf2e107fc9d74748df
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
---
# <a name="custom-video-positioning"></a>Niestandardowe pozycjonowanie wideo

Formanty transportu implementowane przez poszczególnych platform zawierają pasek pozycji. Ten pasek przypomina slider lub ScrollBar i pokazuje bieżącą lokalizację wideo w jego całkowity czas trwania. Ponadto użytkownik można manipulować pasek pozycji, aby przenieść przodu lub do tyłu do nowej pozycji w wideo.

W tym artykule opisano, jak można zaimplementować pasku pozycjonowanie niestandardowe.

## <a name="the-duration-property"></a>Wartość właściwości Duration

Jeden element informacji o który `VideoPlayer` musi obsługiwać niestandardowego paska pozycja jest okresem wideo. `VideoPlayer` Definiuje tylko do odczytu `Duration` właściwości typu `TimeSpan`:

```csharp
namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
        // Duration read-only property
        private static readonly BindablePropertyKey DurationPropertyKey =
            BindableProperty.CreateReadOnly(nameof(Duration), typeof(TimeSpan), typeof(VideoPlayer), new TimeSpan(),
                propertyChanged: (bindable, oldValue, newValue) => ((VideoPlayer)bindable).SetTimeToEnd());

        public static readonly BindableProperty DurationProperty = DurationPropertyKey.BindableProperty;

        public TimeSpan Duration
        {
            get { return (TimeSpan)GetValue(DurationProperty); }
        }

        TimeSpan IVideoPlayerController.Duration
        {
            set { SetValue(DurationPropertyKey, value); }
            get { return Duration; }
        }
        ···
    }
}
```

Podobnie jak `Status` właściwości opisane w [poprzednim artykule](custom-transport.md), to `Duration` właściwość jest tylko do odczytu. Jest ona zdefiniowana z prywatnej `BindablePropertyKey` i można ustawić tylko za pomocą odwołań do `IVideoPlayerController` interfejs, który zawiera tę `Duration` właściwości:

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

Ponadto obsługa zmiany właściwości, która wywołuje metodę o nazwie `SetTimeToEnd` opisane w dalszej części tego artykułu.

Czas trwania wideo jest *nie* dostępna natychmiast po `Source` właściwość `VideoPlayer` jest ustawiona. Plik wideo należy go najpierw częściowo pobrać podstawowej odtwarzacza wideo można określić jego czas trwania.

Poniżej przedstawiono sposób uzyskiwania każdej z platform renderowania czas trwania wideo:

### <a name="video-duration-in-ios"></a>Wideo czasu trwania w systemie iOS

W systemie iOS, czas trwania wideo są uzyskiwane z `Duration` właściwość `AVPlayerItem`, ale nie natychmiast po `AVPlayerItem` jest tworzony. Można ustawić obserwatora systemu iOS dla `Duration` właściwości, ale `VideoPlayerRenderer` uzyskuje czas trwania w `UpdateStatus` metodę, która jest wywoływana 10 razy sekundy:

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
        void OnUpdateStatus(object sender, EventArgs args)
        {
            ···
            if (playerItem != null)
            {
                ((IVideoPlayerController)Element).Duration = ConvertTime(playerItem.Duration);
                ···
            }
        }

        TimeSpan ConvertTime(CMTime cmTime)
        {
            return TimeSpan.FromSeconds(Double.IsNaN(cmTime.Seconds) ? 0 : cmTime.Seconds);

        }
        ···
    }
}
```

`ConvertTime` Metoda konwertuje `CMTime` do obiektu `TimeSpan` wartość.

### <a name="video-duration-in-android"></a>Czas wideo w systemie Android

`Duration` Właściwości Android `VideoView` raporty prawidłowy czas trwania (w milisekundach) podczas `Prepared` zdarzenie `VideoView` jest uruchamiany. Android `VideoPlayerRenderer` klasy używa programu obsługi w celu uzyskania `Duration` właściwości:

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        ···
        void OnVideoViewPrepared(object sender, EventArgs args)
        {
            ···
            ((IVideoPlayerController)Element).Duration = TimeSpan.FromMilliseconds(videoView.Duration);
        }
        ···
    }
}
```

### <a name="video-duration-in-uwp"></a>Wideo czas trwania w platformy uniwersalnej systemu Windows

`NaturalDuration` Właściwość `MediaElement` jest `TimeSpan` wartości i zaczyna obowiązywać, gdy `MediaElement` uruchamiany `MediaOpened` zdarzeń:

```csharp
namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        ···
        void OnMediaElementMediaOpened(object sender, RoutedEventArgs args)
        {
            ((IVideoPlayerController)Element).Duration = Control.NaturalDuration.TimeSpan;
        }
        ···
    }
}
```

## <a name="the-position-property"></a>Właściwości Position

`VideoPlayer` musi również `Position` właściwość, która zwiększa od zera do `Duration` jako odtwarzania wideo. `VideoPlayer` implementuje tę właściwość, takich jak `Position` właściwości platformy uniwersalnej systemu Windows `MediaElement`, normalne właściwość można powiązać z publicznych `set` i `get` metody dostępu:

```csharp
namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
        // Position property
        public static readonly BindableProperty PositionProperty =
            BindableProperty.Create(nameof(Position), typeof(TimeSpan), typeof(VideoPlayer), new TimeSpan(),
                propertyChanged: (bindable, oldValue, newValue) => ((VideoPlayer)bindable).SetTimeToEnd());

        public TimeSpan Position
        {
            set { SetValue(PositionProperty, value); }
            get { return (TimeSpan)GetValue(PositionProperty); }
        }
        ···
    }
}
```

`get` Akcesor zwraca bieżącą pozycję wideo odtwarzany, ale `set` akcesor ma na celu odpowiadanie na manipulowanie użytkownika pozycja paska przenosząc wideo pozycji przodu lub do tyłu.

W systemach iOS i Android, właściwość, która uzyskuje bieżącego położenia ma tylko `get` dostępu, a `Seek` metoda jest dostępna do wykonania tego zadania drugiego. Jeśli myślisz o jego oddzielnej `Seek` metoda wydaje się być podejście rozsądnego więcej niż jeden `Position` właściwości. Pojedynczy `Position` właściwość ma problemu związanego z używaniem: podczas odtwarzania wideo `Position` właściwości muszą zostać stale zaktualizowane w celu uwzględnienia nowej pozycji. Ale nie chcesz, aby większość zmian `Position` właściwość, aby spowodować odtwarzacza wideo przejść do nowej pozycji wideo. Jeśli tak się stanie, odtwarzacza wideo będzie odpowiadać przez wyszukiwanie do ostatniego wartość `Position` wyprzedzeniem nie właściwości i wideo.

Pomimo trudności z wdrożeniem `Position` właściwości o `set` i `get` metod dostępu, takie podejście wybrano, ponieważ jest ona zgodna z platformy UWP `MediaElement`, i ma duże korzyści z wiązania z danymi: `Position` Właściwość `VideoPlayer` może być powiązana z suwaka, który służy do wyświetlenia pozycji i wyszukiwanie do nowej pozycji. Jednak kilka kroków jest niezbędne w przypadku wdrażania to `Position` właściwości, aby uniknąć sprzężeń zwrotnych.

### <a name="setting-and-getting-ios-position"></a>Ustawianie i pobieranie położenia systemu iOS

W systemie iOS `CurrentTime` właściwość `AVPlayerItem` obiektu wskazuje bieżącą pozycję odtwarzania wideo. IOS `VideoPlayerRenderer` ustawia `Position` właściwości w `UpdateStatus` obsługi w tym samym czasie, który ustawia `Duration` właściwości:

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
        void OnUpdateStatus(object sender, EventArgs args)
        {
            ···
            if (playerItem != null)
            {
                ···
                ((IElementController)Element).SetValueFromRenderer(VideoPlayer.PositionProperty, ConvertTime(playerItem.CurrentTime));
            }
        }
        ···
    }
}
```

Mechanizm renderujący wykrywa, kiedy `Position` właściwość z `VideoPlayer` została zmieniona w `OnElementPropertyChanged` zastąpić i używa tej nowej wartości do wywołania `Seek` metoda `AVPlayer` obiektu:

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            ···
            else if (args.PropertyName == VideoPlayer.PositionProperty.PropertyName)
            {
                TimeSpan controlPosition = ConvertTime(player.CurrentTime);

                if (Math.Abs((controlPosition - Element.Position).TotalSeconds) > 1)
                {
                    player.Seek(CMTime.FromSeconds(Element.Position.TotalSeconds, 1));
                }
            }
        }
        ···
    }
}
```

Należy pamiętać, że zawsze `Position` właściwości w `VideoPlayer` ustawiono z `OnUpdateStatus` obsługi, `Position` uruchamiany właściwości `PropertyChanged` zdarzeń, który jest wykrywany w `OnElementPropertyChanged` zastąpienia. Większość tych zmian `OnElementPropertyChanged` metoda powinna nic nie rób. W przeciwnym razie przy każdej zmianie pozycji wideo go będzie można przenieść do tej samej pozycji, którą właśnie osiągnął!

Aby uniknąć tego sprzężenia zwrotnego `OnElementPropertyChanged` tylko wywołania metod `Seek` podczas różnica między `Position` właściwości i bieżącą pozycję `AVPlayer` jest większa niż 1 sekunda.

### <a name="setting-and-getting-android-position"></a>Ustawianie i pobieranie położenia systemu Android

Tak jak w przypadku renderowania dla systemu iOS, Android `VideoPlayerRenderer` ustawia nową wartość dla `Position` właściwości w `OnUpdateStatus` programu obsługi. `CurrentPosition` Właściwość `VideoView` zawiera nowe położenie jednostki w milisekundach:

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        ···
        void OnUpdateStatus(object sender, EventArgs args)
        {
            ···
            TimeSpan timeSpan = TimeSpan.FromMilliseconds(videoView.CurrentPosition);
            ((IElementController)Element).SetValueFromRenderer(VideoPlayer.PositionProperty, timeSpan);
        }
        ···
    }
}
```

Tak jak w przypadku renderowania dla systemu iOS, Android renderowania wymaga także `SeekTo` metody `VideoView` podczas `Position` właściwość zostanie zmieniona, ale tylko wtedy, gdy zmiana jest inny niż więcej niż jednej sekundy `CurrentPosition` wartość `VideoView`:

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        ···
        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            ···
            else if (args.PropertyName == VideoPlayer.PositionProperty.PropertyName)
            {
                if (Math.Abs(videoView.CurrentPosition - Element.Position.TotalMilliseconds) > 1000)
                {
                    videoView.SeekTo((int)Element.Position.TotalMilliseconds);
                }
            }
        }
        ···
    }
}
```

### <a name="setting-and-getting-uwp-position"></a>Ustawianie i pobieranie położenia platformy uniwersalnej systemu Windows

Platformy uniwersalnej systemu Windows `VideoPlayerRenderer` dojść `Position` w taki sam sposób jak systemów iOS i Android renderowania, ale ponieważ `Position` właściwości platformy uniwersalnej systemu Windows `MediaElement` jest również `TimeSpan` wartości, niezbędne jest brak konwersji:

```csharp
namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        ···
        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            ···
            else if (args.PropertyName == VideoPlayer.PositionProperty.PropertyName)
            {
                if (Math.Abs((Control.Position - Element.Position).TotalSeconds) > 1)
                {
                    Control.Position = Element.Position;
                }
            }
        }
        ···
        void OnUpdateStatus(object sender, EventArgs args)
        {
            ((IElementController)Element).SetValueFromRenderer(VideoPlayer.PositionProperty, Control.Position);
        }
        ···
    }
}
```

## <a name="calculating-a-timetoend-property"></a>Obliczanie właściwości TimeToEnd

Odtwarzaczy czasami wideo pokazują czas pozostały w wideo. Ta wartość rozpoczyna się od czasu trwania wideo, gdy wideo rozpoczyna się i zmniejsza do zera po zakończeniu wideo.

`VideoPlayer` obejmuje tylko do odczytu `TimeToEnd` właściwość, która jest obsługiwane wyłącznie w ramach klasy na podstawie zmian w `Duration` i `Position` właściwości:

```csharp
namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
        private static readonly BindablePropertyKey TimeToEndPropertyKey =
            BindableProperty.CreateReadOnly(nameof(TimeToEnd), typeof(TimeSpan), typeof(VideoPlayer), new TimeSpan());

        public static readonly BindableProperty TimeToEndProperty = TimeToEndPropertyKey.BindableProperty;

        public TimeSpan TimeToEnd
        {
            private set { SetValue(TimeToEndPropertyKey, value); }
            get { return (TimeSpan)GetValue(TimeToEndProperty); }
        }

        void SetTimeToEnd()
        {
            TimeToEnd = Duration - Position;
        }
        ···
    }
}
```

`SetTimeToEnd` Metoda jest wywoływana z obu programów obsługi zmienić właściwości `Duration` i `Position`.

## <a name="a-custom-slider-for-video"></a>Niestandardowe kontrolki slider wideo

Istnieje możliwość zapisu formantu niestandardowego paska pozycji lub używać platformy Xamarin.Forms `Slider` lub klasą pochodzącą z `Slider`, takich jak `PositionSlider` klasy. Klasa definiuje dwie nowe właściwości o nazwie `Duration` i `Position` typu `TimeSpan` powinny być powiązany z danymi dwie właściwości o tej samej nazwie w `VideoPlayer`. Zwróć uwagę, że domyślna powiązanie rodzaj `Position` właściwości jest dwukierunkowe:

```csharp
namespace FormsVideoLibrary
{
    public class PositionSlider : Slider
    {
        public static readonly BindableProperty DurationProperty =
            BindableProperty.Create(nameof(Duration), typeof(TimeSpan), typeof(PositionSlider), new TimeSpan(1),
                                    propertyChanged: (bindable, oldValue, newValue) =>
                                    {
                                        double seconds = ((TimeSpan)newValue).TotalSeconds;
                                        ((PositionSlider)bindable).Maximum = seconds <= 0 ? 1 : seconds;
                                    });

        public TimeSpan Duration
        {
            set { SetValue(DurationProperty, value); }
            get { return (TimeSpan)GetValue(DurationProperty); }
        }

        public static readonly BindableProperty PositionProperty =
            BindableProperty.Create(nameof(Position), typeof(TimeSpan), typeof(PositionSlider), new TimeSpan(0),
                                    defaultBindingMode: BindingMode.TwoWay,
                                    propertyChanged: (bindable, oldValue, newValue) =>
                                    {
                                        double seconds = ((TimeSpan)newValue).TotalSeconds;
                                        ((PositionSlider)bindable).Value = seconds;
                                    });

        public TimeSpan Position
        {
            set { SetValue(PositionProperty, value); }
            get { return (TimeSpan)GetValue(PositionProperty); }
        }

        public PositionSlider()
        {
            PropertyChanged += (sender, args) =>
            {
                if (args.PropertyName == "Value")
                {
                    TimeSpan newPosition = TimeSpan.FromSeconds(Value);

                    if (Math.Abs(newPosition.TotalSeconds - Position.TotalSeconds) / Duration.TotalSeconds > 0.01)
                    {
                        Position = newPosition;
                    }
                }
            };
        }
    }
}
```

Obsługa zmiany właściwości dla `Duration` zestawy właściwości `Maximum` właściwości podstawowych `Slider` do `TotalSeconds` właściwość `TimeSpan` wartość. Podobnie, zmienić właściwości obsługi dla `Position` ustawia `Value` właściwość `Slider`. W ten sposób odpowiadającego `Slider` śledzi pozycja `PositionSlider`.

`PositionSlider` Są aktualizowane z podstawową `Slider` w tylko jednym wystąpieniu: gdy użytkownik modyfikuje `Slider` wskazująca, czy zaawansowane lub cofnąć do nowej pozycji wideo. Wykryto w `PropertyChanged` obsługi w Konstruktorze `PositionSlider`. Sprawdza, czy program obsługi zmiany w `Value` właściwości, i jeśli różni się od `Position` właściwości, a następnie `Position` ustawiono właściwość z `Value` właściwości.

Teoretycznie wewnętrzny `if` instrukcja może być zapisany jako:

```csharp
if (newPosition.Seconds != Position.Seconds)
{
    Position = newPosition;
}
```

Jednak implementacja Android `Slider` zawiera tylko 1000 kroki odrębny niezależnie od tego, `Minimum` i `Maximum` ustawienia. Długość wideo jest większa niż 1 000 sekund, a następnie dwa różne `Position` odpowiada wartości do tej samej `Value` ustawienie `Slider`, a to `if` instrukcji spowoduje wywołanie wynik fałszywie dodatni do manipulacji użytkownika `Slider`. Jest to bezpieczniejsze zamiast tego Sprawdź, czy nowe położenie i istniejącej pozycji są większe niż setną całkowity czas trwania.

## <a name="using-the-positionslider"></a>Przy użyciu PositionSlider

Dokumentację platformy uniwersalnej systemu Windows [ `MediaElement` ](/uwp/api/Windows.UI.Xaml.Controls.MediaElement/) ostrzega o powiązania `Position` właściwości, ponieważ właściwość często aktualizacji. Dokumentację zaleca się, że Czasomierz można użyć w zapytaniu `Position` właściwości.

To dobry zalecenia, ale trzech `VideoPlayerRenderer` klasy korzystają już pośrednio czasomierz zaktualizować `Position` właściwości. `Position` Właściwości zostanie zmieniona w procedurze obsługi dla `UpdateStatus` zdarzenie, które jest wywoływane tylko 10 razy sekundy.

W związku z tym `Position` właściwość `VideoPlayer` może być powiązana z `Position` właściwość `PositionSlider` bez problemów z wydajnością, jak pokazano w **pasek pozycji niestandardowych** strony:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.CustomPositionBarPage"
             Title="Custom Position Bar">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <video:VideoPlayer x:Name="videoPlayer"
                           Grid.Row="0"
                           AreTransportControlsEnabled="False"
                           Source="{StaticResource ElephantsDream}" />

        ···

        <StackLayout Grid.Row="1"
                     Orientation="Horizontal"
                     Margin="10, 0"
                     BindingContext="{x:Reference videoPlayer}">

            <Label Text="{Binding Path=Position,
                                  StringFormat='{0:hh\\:mm\\:ss}'}"
                   VerticalOptions="Center"/>

            ···

            <Label Text="{Binding Path=TimeToEnd,
                                  StringFormat='{0:hh\\:mm\\:ss}'}"
                   VerticalOptions="Center" />
        </StackLayout>

        <video:PositionSlider Grid.Row="2"
                              Margin="10, 0, 10, 10"
                              BindingContext="{x:Reference videoPlayer}"
                              Duration="{Binding Duration}"           
                              Position="{Binding Position}">
            <video:PositionSlider.Triggers>
                <DataTrigger TargetType="video:PositionSlider"
                             Binding="{Binding Status}"
                             Value="{x:Static video:VideoStatus.NotReady}">
                    <Setter Property="IsEnabled" Value="False" />
                </DataTrigger>
            </video:PositionSlider.Triggers>
        </video:PositionSlider>
    </Grid>
</ContentPage>
```

Ukrywa pierwszy wielokropka (···) `ActivityIndicator`; jest taka sama jak w poprzedniej **transportu niestandardowe** strony. Zwróć uwagę, dwa `Label` wyświetlanie elementów `Position` i `TimeToEnd` właściwości. Wielokropek między tymi dwoma `Label` elementy ukrywa dwa `Button` elementy wyświetlane w **transportu niestandardowe** dla odtwarzania, Wstrzymaj, strony i zatrzymać. Logika CodeBehind również jest taka sama jak **transportu niestandardowe** strony.

[![Niestandardowe pozycjonowanie](custom-positioning-images/custompositioning-small.png "Niestandardowe pozycjonowanie")](custom-positioning-images/custompositioning-large.png "Niestandardowe pozycjonowanie")

Zakończenie z omówieniem `VideoPlayer`.

## <a name="related-links"></a>Linki pokrewne

- [Pokazy odtwarzacza wideo (przykład)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
