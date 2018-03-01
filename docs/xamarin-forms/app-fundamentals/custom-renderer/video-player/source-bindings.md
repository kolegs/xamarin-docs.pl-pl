---
title: "Powiązania źródła wideo do odtwarzacza"
ms.topic: article
ms.prod: xamarin
ms.assetid: 504E0C7E-051A-4AF2-B654-BAB4D0957928
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: d0842a54f725e5a9504668f977ba06648a96ee6d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="binding-video-sources-to-the-player"></a>Powiązania źródła wideo do odtwarzacza

Gdy `Source` właściwość `VideoPlayer` widok został przestawiony nowy plik wideo, istniejące wideo zatrzymuje odtwarzanie i rozpocznie się nowe wideo. Wskazuje na **wideo w sieci Web wybierz** strony [ **VideoPlayerDemos** ](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/) próbki. Strona zawiera `ListView` z trzech plików wideo, do których odwołuje się z tytułami **App.xaml** pliku:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.SelectWebVideoPage"
             Title="Select Web Video">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="2*" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>
        
        <video:VideoPlayer x:Name="videoPlayer"
                           Grid.Row="0" />

        <ListView Grid.Row="1"
                  ItemSelected="OnListViewItemSelected">
            <ListView.ItemsSource>
                <x:Array Type="{x:Type x:String}">
                    <x:String>Elephant's Dream</x:String>
                    <x:String>Big Buck Bunny</x:String>
                    <x:String>Sintel</x:String>
                </x:Array>
            </ListView.ItemsSource>
        </ListView>
    </Grid>
</ContentPage>
```

Po wybraniu wideo `ItemSelected` jest wykonywana przez program obsługi zdarzeń w pliku CodeBehind. Program obsługi usuwa wszystkie spacje i apostrofów z tytułu i użyty jako klucz uzyskanie takiego zasoby zdefiniowane w **App.xaml** pliku. Czy `UriVideoSource` następnie ustawiono obiektu `Source` właściwość `VideoPlayer`.

```csharp
namespace VideoPlayerDemos
{
    public partial class SelectWebVideoPage : ContentPage
    {
        public SelectWebVideoPage()
        {
            InitializeComponent();
        }

        void OnListViewItemSelected(object sender, SelectedItemChangedEventArgs args)
        {
            if (args.SelectedItem != null)
            {
                string key = ((string)args.SelectedItem).Replace(" ", "").Replace("'", "");
                videoPlayer.Source = (UriVideoSource)Application.Current.Resources[key];
            }
        }
    }
}
```

Po załadowaniu najpierw strony, nie wybrano elementów w `ListView`, więc musisz wybrać host rozpocząć odtwarzanie wideo:

[![Wybierz wideo w sieci Web](source-bindings-images/selectwebvideo-small.png "Wybierz wideo w sieci Web")](source-bindings-images/selectwebvideo-large.png "Wybierz wideo w sieci Web")

`Source` Właściwość `VideoPlayer` nie jest obsługiwana przez właściwości możliwej do wiązania, co oznacza, że może być elementem docelowym powiązania danych. Wskazuje na **powiązać VideoPlayer** strony. Znaczniki w **BindToVideoPlayer.xaml** plik jest obsługiwany przez następujące klasy, która hermetyzuje tytuł wideo i odpowiadające mu `VideoSource` obiektu:

```csharp
namespace VideoPlayerDemos
{
    public class VideoInfo
    {
        public string DisplayName { set; get; }

        public VideoSource VideoSource { set; get; }

        public override string ToString()
        {
            return DisplayName;
        }
    }
}
```

`ListView` w **BindToVideoPlayer.xaml** plik zawiera tablicę tych `VideoInfo` obiektów, każdą z nich zainicjowany z tytułem wideo i `UriVideoSource` obiektu ze słownika zasobów w  **App.XAML**:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:VideoPlayerDemos"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.BindToVideoPlayerPage"
             Title="Bind to VideoPlayer">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="2*" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <video:VideoPlayer x:Name="videoPlayer"
                           Grid.Row="0"
                           Source="{Binding Source={x:Reference listView},
                                            Path=SelectedItem.VideoSource}" />

        <ListView x:Name="listView"
                  Grid.Row="1">
            <ListView.ItemsSource>
                <x:Array Type="{x:Type local:VideoInfo}">
                    <local:VideoInfo DisplayName="Elephant's Dream"
                                     VideoSource="{StaticResource ElephantsDream}" />

                    <local:VideoInfo DisplayName="Big Buck Bunny"
                                     VideoSource="{StaticResource BigBuckBunny}" />

                    <local:VideoInfo DisplayName="Sintel"
                                     VideoSource="{StaticResource Sintel}" />
                </x:Array>
            </ListView.ItemsSource>
        </ListView>
    </Grid>
</ContentPage>
```

`Source` Właściwość `VideoPlayer` jest powiązany z `ListView`. `Path` Powiązania jest określony jako `SelectedItem.VideoSource`, która jest złożony ścieżki składające się z dwóch właściwości: `SelectedItem` jest właściwością `ListView`. Wybrany element jest typu `VideoInfo`, który ma `VideoSource` właściwości.

W przypadku pierwszego **Wybierz wideo w sieci Web** strony, nie początkowo wybrano elementów z `ListView`, dlatego należy wybrać jedną z wideo przed rozpoczęciem odtwarzania.


## <a name="related-links"></a>Linki pokrewne

- [Pokazy odtwarzacza wideo (przykład)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
