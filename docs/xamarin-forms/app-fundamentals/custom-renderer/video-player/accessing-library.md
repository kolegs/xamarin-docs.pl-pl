---
title: Uzyskiwanie dostępu do biblioteki wideo urządzenia
description: W tym artykule wyjaśniono, jak uzyskać dostępu do biblioteki wideo urządzenia w aplikacji odtwarzacza wideo, za pomocą platformy Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 364C1D43-EAAE-45B9-BE24-0DA5AE74C4D9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: 7e9f7ad93ae8828155847b923cb2779b3146f63e
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35240703"
---
# <a name="accessing-the-devices-video-library"></a>Uzyskiwanie dostępu do biblioteki wideo urządzenia

Większość nowoczesnych urządzeń przenośnych i komputerów stacjonarnych mają możliwość rekordów wideo przy użyciu aparatu fotograficznego urządzenia. Pliki wideo, które użytkownik tworzy następnie są przechowywane jako pliki na urządzeniu. Te pliki można pobrać z biblioteki obrazów i odgrywa `VideoPlayer` klasy, podobnie jak inne pliki wideo.

## <a name="the-photo-picker-dependency-service"></a>Zdjęcie selektora zależności usługi

Każdej z platform trzy zawiera obiekt, który umożliwia użytkownikowi wybranie zdjęcie lub Nagraj wideo z biblioteki obraz urządzenia. Pierwszym etapem odtwarzanie wideo z biblioteki obraz urządzenia jest kompilowany Usługa zależności, która wywołuje selektora obrazu na każdej z platform. Usługa zależności opisanych poniżej jest bardzo podobny do zdefiniowanych w [ **pobrania zdjęcia z biblioteki obrazów** ](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md) artykułu z tą różnicą, że wideo selektora zwraca nazwę pliku, a nie `Stream`obiektu.

.NET Standard projektu biblioteki definiuje interfejs o nazwie `IVideoPicker` usługi zależności:

```csharp
namespace FormsVideoLibrary
{
    public interface IVideoPicker
    {
        Task<string> GetVideoFileAsync();
    }
}
```

Każdy z trzech platform zawiera klasę o nazwie `VideoPicker` który implementuje ten interfejs.

### <a name="the-ios-video-picker"></a>Selektor wideo z systemem iOS

IOS `VideoPicker` korzysta z systemu iOS [ `UIImagePickerController` ](https://developer.xamarin.com/api/type/UIKit.UIImagePickerController/) na dostęp do biblioteki obrazów, określając, że powinny być ograniczone do wideo (określanych jako "filmy") w systemie iOS `MediaType` właściwości. Zwróć uwagę, że `VideoPicker` implementuje jawnie `IVideoPicker` interfejsu. Spójrz również `Dependency` atrybut, który identyfikuje tę klasę jako zależność usługi. Są to dwóch wymagań, które umożliwiają platformy Xamarin.Forms można znaleźć usługi zależności w projekcie platformy:

```csharp
using System;
using System.Threading.Tasks;
using UIKit;
using Xamarin.Forms;

[assembly: Dependency(typeof(FormsVideoLibrary.iOS.VideoPicker))]

namespace FormsVideoLibrary.iOS
{
    public class VideoPicker : IVideoPicker
    {
        TaskCompletionSource<string> taskCompletionSource;
        UIImagePickerController imagePicker;

        public Task<string> GetVideoFileAsync()
        {
            // Create and define UIImagePickerController
            imagePicker = new UIImagePickerController
            {
                SourceType = UIImagePickerControllerSourceType.SavedPhotosAlbum,
                MediaTypes = new string[] { "public.movie" }
            };

            // Set event handlers
            imagePicker.FinishedPickingMedia += OnImagePickerFinishedPickingMedia;
            imagePicker.Canceled += OnImagePickerCancelled;

            // Present UIImagePickerController;
            UIWindow window = UIApplication.SharedApplication.KeyWindow;
            var viewController = window.RootViewController;
            viewController.PresentModalViewController(imagePicker, true);

            // Return Task object
            taskCompletionSource = new TaskCompletionSource<string>();
            return taskCompletionSource.Task;
        }

        void OnImagePickerFinishedPickingMedia(object sender, UIImagePickerMediaPickedEventArgs args)
        {
            if (args.MediaType == "public.movie")
            {
                taskCompletionSource.SetResult(args.MediaUrl.AbsoluteString);
            }
            else
            {
                taskCompletionSource.SetResult(null);
            }
            imagePicker.DismissModalViewController(true);
        }

        void OnImagePickerCancelled(object sender, EventArgs args)
        {
            taskCompletionSource.SetResult(null);
            imagePicker.DismissModalViewController(true);
        }
    }
}
```

### <a name="the-android-video-picker"></a>Selektor wideo dla systemu Android

Implementacja systemu Android `IVideoPicker` wymaga metody wywołania zwrotnego, która jest częścią działania aplikacji. Z tego powodu `MainActivity` klasa definiuje dwie właściwości pola i metody wywołania zwrotnego:

```csharp
namespace VideoPlayerDemos.Droid
{
    ···
    public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsAppCompatActivity
    {
        protected override void OnCreate(Bundle bundle)
        {
            Current = this;
            ···
        }

        // Field, properties, and method for Video Picker
        public static MainActivity Current { private set; get; }

        public static readonly int PickImageId = 1000;

        public TaskCompletionSource<string> PickImageTaskCompletionSource { set; get; }

        protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
        {
            base.OnActivityResult(requestCode, resultCode, data);

            if (requestCode == PickImageId)
            {
                if ((resultCode == Result.Ok) && (data != null))
                {
                    // Set the filename as the completion of the Task
                    PickImageTaskCompletionSource.SetResult(data.DataString);
                }
                else
                {
                    PickImageTaskCompletionSource.SetResult(null);
                }
            }
        }
    }
}
```

`OnCreate` Metody w `MainActivity` przechowuje własne wystąpienie w statycznych `Current` właściwości. Dzięki temu wdrożenia `IVideoPicker` uzyskanie `MainActivity` wystąpienia uruchamiania **Wybierz wideo** selektora:

```csharp
using System;
using System.Threading.Tasks;
using Android.Content;
using Xamarin.Forms;

// Need application's MainActivity
using VideoPlayerDemos.Droid;

[assembly: Dependency(typeof(FormsVideoLibrary.Droid.VideoPicker))]

namespace FormsVideoLibrary.Droid
{
    public class VideoPicker : IVideoPicker
    {
        public Task<string> GetVideoFileAsync()
        {
            // Define the Intent for getting images
            Intent intent = new Intent();
            intent.SetType("video/*");
            intent.SetAction(Intent.ActionGetContent);

            // Get the MainActivity instance
            MainActivity activity = MainActivity.Current;

            // Start the picture-picker activity (resumes in MainActivity.cs)
            activity.StartActivityForResult(
                Intent.CreateChooser(intent, "Select Video"),
                MainActivity.PickImageId);

            // Save the TaskCompletionSource object as a MainActivity property
            activity.PickImageTaskCompletionSource = new TaskCompletionSource<string>();

            // Return Task object
            return activity.PickImageTaskCompletionSource.Task;
        }
    }
}
```

Dodatki do `MainActivity` obiektu są tylko kod w [ **VideoPlayerDemos** ](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/) rozwiązanie, gdy kod aplikacji normalne musi zostać zmienione do obsługi `FormsVideoLibrary` klasy.

### <a name="the-uwp-video-picker"></a>Selektor wideo platformy uniwersalnej systemu Windows

Implementacja platformy uniwersalnej systemu Windows `IVideoPicker` platformy uniwersalnej systemu Windows używa interfejsu [ `FileOpenPicker` ](/uwp/api/Windows.Storage.Pickers.FileOpenPicker/). Rozpocznie się wyszukiwanie plików z biblioteki obrazów i ogranicza typów plików MP4 i WMV (Windows Media Video):

```csharp
using System;
using System.Threading.Tasks;
using Windows.Storage;
using Windows.Storage.Pickers;
using Xamarin.Forms;

[assembly: Dependency(typeof(FormsVideoLibrary.UWP.VideoPicker))]

namespace FormsVideoLibrary.UWP
{
    public class VideoPicker : IVideoPicker
    {
        public async Task<string> GetVideoFileAsync()
        {
            // Create and initialize the FileOpenPicker
            FileOpenPicker openPicker = new FileOpenPicker
            {
                ViewMode = PickerViewMode.Thumbnail,
                SuggestedStartLocation = PickerLocationId.PicturesLibrary
            };

            openPicker.FileTypeFilter.Add(".wmv");
            openPicker.FileTypeFilter.Add(".mp4");

            // Get a file and return the path
            StorageFile storageFile = await openPicker.PickSingleFileAsync();
            return storageFile?.Path;
        }
    }
}
```

## <a name="invoking-the-dependency-service"></a>Wywołania zależności usługi

**Odtwarzanie wideo biblioteki** strony [ **VideoPlayerDemos** ](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/) program pokazano, jak korzystać z usługi zależności selektora wideo. Plik XAML zawiera `VideoPlayer` wystąpienia i `Button` etykietą **Pokaż biblioteki wideo**:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.PlayLibraryVideoPage"
             Title="Play Library Video">
    <StackLayout>
        <video:VideoPlayer x:Name="videoPlayer"
                           VerticalOptions="FillAndExpand" />

        <Button Text="Show Video Library"
                Margin="10"
                HorizontalOptions="Center"
                Clicked="OnShowVideoLibraryClicked" />
    </StackLayout>
</ContentPage>
```

Zawiera plik CodeBehind `Clicked` obsługę `Button`. Wywołania zależności usługi wymaga wywołania `DependencyService.Get` uzyskanie wykonania `IVideoPicker` interfejs w projekcie platformy. `GetVideoFileAsync` Następnie wywoływana jest metoda w tym wystąpieniu:

```csharp
namespace VideoPlayerDemos
{
    public partial class PlayLibraryVideoPage : ContentPage
    {
        public PlayLibraryVideoPage()
        {
            InitializeComponent();
        }

       async void OnShowVideoLibraryClicked(object sender, EventArgs args)
        {
            Button btn = (Button)sender;
            btn.IsEnabled = false;

            string filename = await DependencyService.Get<IVideoPicker>().GetVideoFileAsync();

            if (!String.IsNullOrWhiteSpace(filename))
            {
                videoPlayer.Source = new FileVideoSource
                {
                    File = filename
                };
            }

            btn.IsEnabled = true;
        }
    }
}
```

`Clicked` Program obsługi używa tej nazwy pliku do utworzenia `FileVideoSource` obiektu i ustawić ją na `Source` właściwość `VideoPlayer`.

Każdy z `VideoPlayerRenderer` klasy zawiera kod w jego `SetSource` metody dla obiektów typu `FileVideoSource`. Zostały one przedstawione poniżej:

### <a name="handling-ios-files"></a>Obsługa plików z systemem iOS

Wersja systemu iOS `VideoPlayerRenderer` procesów `FileVideoSource` obiektów przy użyciu statycznych `Asset.FromUrl` metodę o nazwie pliku. To utworzenie `AVAsset` obiekt reprezentujący plik w bibliotece obrazów urządzenia:

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
            else if (Element.Source is FileVideoSource)
            {
                string uri = (Element.Source as FileVideoSource).File;

                if (!String.IsNullOrWhiteSpace(uri))
                {
                    asset = AVAsset.FromUrl(new NSUrl(uri));
                }
            }
            ···
        }
        ···
    }
}
```

### <a name="handling-android-files"></a>Obsługa plików z systemem Android

Podczas przetwarzania obiektów typu `FileVideoSource`, wdrożenia dla systemu Android `VideoPlayerRenderer` używa `SetVideoPath` metody `VideoView` do określenia pliku w bibliotece obrazów urządzenia:

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
            else if (Element.Source is FileVideoSource)
            {
                string filename = (Element.Source as FileVideoSource).File;

                if (!String.IsNullOrWhiteSpace(filename))
                {
                    videoView.SetVideoPath(filename);
                    hasSetSource = true;
                }
            }
            ···
        }
        ···
    }
}
```

### <a name="handling-uwp-files"></a>Obsługa plików platformy uniwersalnej systemu Windows

Podczas przetwarzania obiektów typu `FileVideoSource`, implementacji platformy uniwersalnej systemu Windows `SetSource` metody musi utworzyć `StorageFile` obiektu, otworzyć tego pliku do odczytu i przekazać do obiektu strumienia `SetSource` metody `MediaElement`:

```csharp
namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
        async void SetSource()
        {
            bool hasSetSource = false;
            ···
            else if (Element.Source is FileVideoSource)
            {
                // Code requires Pictures Library in Package.appxmanifest Capabilities to be enabled
                string filename = (Element.Source as FileVideoSource).File;

                if (!String.IsNullOrWhiteSpace(filename))
                {
                    StorageFile storageFile = await StorageFile.GetFileFromPathAsync(filename);
                    IRandomAccessStreamWithContentType stream = await storageFile.OpenReadAsync();
                    Control.SetSource(stream, storageFile.ContentType);
                    hasSetSource = true;
                }
            }
            ···
        }
        ···
    }
}
```

W przypadku wszystkich platform trzy wideo rozpoczęcia odtwarzania niemal natychmiast po wideo źródła jest ustawiona, ponieważ plik na urządzeniu i nie muszą zostać pobrane.



## <a name="related-links"></a>Linki pokrewne

- [Pokazy odtwarzacza wideo (przykład)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
- [Pobrania zdjęcia z biblioteki obrazów](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md)
