---
title: "Pobrania zdjęcia z biblioteki obrazów"
description: "Wybierz zdjęcie z telefonu biblioteki obrazów za pomocą DependencyService"
ms.topic: article
ms.prod: xamarin
ms.assetid: 4F51B0E7-6A63-403C-B488-500CCBCE75DD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/06/2017
ms.openlocfilehash: 6ac228f85f3f717ddf95e0dc2e434b13bfec5d06
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
---
# <a name="picking-a-photo-from-the-picture-library"></a>Pobrania zdjęcia z biblioteki obrazów

W tym artykule przedstawiono tworzenie aplikacji, która umożliwia użytkownikowi pobranie zdjęcie z biblioteki obrazów przez telefon. Platformy Xamarin.Forms zawiera tej funkcji, należy użyć [ `DependencyService` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/) dostępu do natywnych interfejsów API na każdej z platform.  W tym artykule opisano następujące kroki do używania `DependencyService` dla tego zadania:

- **[Tworzenie interfejsu](#Creating_the_Interface)**  &ndash; zrozumieć sposób tworzenia interfejsu w kodzie udostępnionego.
- **[iOS implementacji](#iOS_Implementation)**  &ndash; Dowiedz się, jak implementować interfejs w kodzie natywnym dla systemu iOS.
- **[Implementacja systemu android](#Android_Implementation)**  &ndash; Dowiedz się, jak implementować interfejs w kodzie natywnym dla systemu Android.
- **[Uniwersalny implementacji platformy Windows](#UWP_Implementation)**  &ndash; Dowiedz się, jak implementować interfejs w kodzie natywnym dla uniwersalnych platformy systemu Windows (UWP).
- **[Windows Phone implementacji](#Windows_Phone_Implementation)**  &ndash; Dowiedz się, jak implementować interfejs w kodzie natywnym dla systemu Windows Phone 8.1.
- **[Wdrażanie w kodzie udostępnionego](#Implementing_in_Shared_Code)**  &ndash; Dowiedz się, jak używać `DependencyService` do wywołania do implementacji native z udostępnionego kodu.

<a name="Creating_the_Interface" />

## <a name="creating-the-interface"></a>Tworzenie interfejsu

Najpierw Utwórz interfejs w kodzie udostępnionego, który określa informacji dotyczących odpowiednich funkcji. W przypadku aplikacji pobrania fotografii wymagane jest tylko jedna metoda. To jest zdefiniowany w [ `IPicturePicker` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/DependencyServiceSample/IPicturePicker.cs) interfejsu, biblioteki klas przenośnych przykładowy kod:

```csharp
namespace DependencyServiceSample
{
    public interface IPicturePicker
    {
        Task<Stream> GetImageStreamAsync();
    }
}
```

`GetImageStreamAsync` Metody jest zdefiniowany jako asynchroniczny, ponieważ metoda musi zwracać szybko, ale go nie może zwracać `Stream` obiektu wybranego zdjęcia, dopóki użytkownik nie ma przeglądania biblioteki obrazów i wybrać jeden.

Ten interfejs jest wdrażany na wszystkich platformach przy użyciu kodu specyficzne dla platformy.

<a name="iOS_Implementation" />

## <a name="ios-implementation"></a>Implementacja systemu iOS

Implementacja systemu iOS `IPicturePicker` używa interfejsu [ `UIImagePickerController` ](https://developer.xamarin.com/api/type/UIKit.UIImagePickerController/) zgodnie z opisem w [ **Wybierz zdjęcie z galerii** ](https://developer.xamarin.com/recipes/ios/media/video_and_photos/choose_a_photo_from_the_gallery/) przepisu i [przykładowy kod](https://github.com/xamarin/recipes/tree/master/ios/media/video_and_photos/choose_a_photo_from_the_gallery).

Wdrożenia z systemem iOS znajduje się w [ `PicturePickerImplementation` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/iOS/PicturePickerImplementation.cs) klasy w projekcie systemu iOS w przykładowym kodzie. Aby widoczne dla tej klasy `DependencyService` Menedżerze klasy musi być identyfikowany przy [`assembly`] atrybutu typu `Dependency`, i klasa musi być publiczny i jawne Implementowanie `IPicturePicker` interfejsu:

```csharp
[assembly: Dependency (typeof (PicturePickerImplementation))]

namespace DependencyServiceSample.iOS
{
    public class PicturePickerImplementation : IPicturePicker
    {
        TaskCompletionSource<Stream> taskCompletionSource;
        UIImagePickerController imagePicker;

        public Task<Stream> GetImageStreamAsync()
        {
            // Create and define UIImagePickerController
            imagePicker = new UIImagePickerController
            {
                SourceType = UIImagePickerControllerSourceType.PhotoLibrary,
                MediaTypes = UIImagePickerController.AvailableMediaTypes(UIImagePickerControllerSourceType.PhotoLibrary)
            };

            // Set event handlers
            imagePicker.FinishedPickingMedia += OnImagePickerFinishedPickingMedia;
            imagePicker.Canceled += OnImagePickerCancelled;

            // Present UIImagePickerController;
            UIWindow window = UIApplication.SharedApplication.KeyWindow;
            var viewController = window.RootViewController;
            viewController.PresentModalViewController(imagePicker, true);

            // Return Task object
            taskCompletionSource = new TaskCompletionSource<Stream>();
            return taskCompletionSource.Task;
        }
        ...
    }
}

```

`GetImageStreamAsync` Metoda tworzy `UIImagePickerController` inicjowane wybierz obrazy z biblioteki zdjęcie. Wymagane są dwa programy obsługi zdarzeń: jeden dla użytkownik wybrał fotografii, a drugie dla gdy użytkownik anuluje wyświetlanie zdjęć biblioteki. `PresentModalViewController` Następnie wyświetla biblioteka fotografii dla użytkownika.

W tym momencie `GetImageStreamAsync` metoda musi zwracać `Task<Stream>` obiektu do kodu, który wywołuje go. To zadanie zostało wykonane tylko wtedy, gdy użytkownik zakończy interakcji z biblioteki zdjęć i nosi nazwę jednego z programów obsługi zdarzeń. W sytuacjach, takich jak ta [ `TaskCompletionSource` ](https://msdn.microsoft.com/library/dd449174(v=vs.110).aspx) niezbędne jest klasa. Ta klasa dostarcza `Task` obiektu właściwego typu ogólnego z `GetImageStreamAsync` — metoda i klasa później może zostać zgłoszony po zakończeniu zadania.

`FinishedPickingMedia` Program obsługi zdarzeń jest wywoływane, gdy użytkownik wybrał obrazu. Program obsługi zapewnia jednak `UIImage` obiektu i `Task` musi zwracać .NET `Stream` obiektu. Odbywa się w dwóch krokach: `UIImage` obiekt jest najpierw konwertowany na plik JPEG przechowywanych w pamięci `NSData` obiektu, a następnie `NSData` obiektu jest konwertowany na .NET `Stream` obiektu. Wywołanie `SetResult` metody `TaskComkpletionSource` obiektu kończy zadanie, podając `Stream` obiektu:

```csharp
namespace DependencyServiceSample.iOS
{
    public class PicturePickerImplementation : IPicturePicker
    {
        TaskCompletionSource<Stream> taskCompletionSource;
        UIImagePickerController imagePicker;
        ...
        void OnImagePickerFinishedPickingMedia(object sender, UIImagePickerMediaPickedEventArgs args)
        {
            UIImage image = args.EditedImage ?? args.OriginalImage;

            if (image != null)
            {
                // Convert UIImage to .NET Stream object
                NSData data = image.AsJPEG(1);
                Stream stream = data.AsStream();

                // Set the Stream as the completion of the Task
                taskCompletionSource.SetResult(stream);
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

Aplikacji systemu iOS wymaga zgody użytkownika na dostęp do biblioteki fotografii przez telefon. Dodaj następujący kod do `dict` sekcji w pliku Info.plist:

```xml
<key>NSPhotoLibraryUsageDescription</key>
<string>Picture Picker uses photo library</string>
```


<a name="Android_Implementation" />

## <a name="android-implementation"></a>Implementacja systemu android

Implementacja systemu Android używane techniki opisane w [ **wybierz obraz** ](https://developer.xamarin.com/recipes/android/other_ux/pick_image/) przepisu i [przykładowy kod](https://github.com/xamarin/recipes/tree/master/android/other_ux/pick_image). Metoda wywoływana, gdy użytkownik wybrał obrazu z biblioteki obrazów jest jednak `OnActivityResult` zastąpienia w klasie, która jest pochodną `Activity`. Z tego powodu normalnej [ `MainActivity` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/Droid/MainActivity.cs) klasy w projekcie systemu Android ma został uzupełniony pola, właściwości i nadpisanie `OnActivityResult` metody:

```csharp
public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsApplicationActivity
{
    ...
    // Field, property, and method for Picture Picker
    public static readonly int PickImageId = 1000;

    public TaskCompletionSource<Stream> PickImageTaskCompletionSource { set; get; }

    protected override void OnActivityResult(int requestCode, Result resultCode, Intent intent)
    {
        base.OnActivityResult(requestCode, resultCode, intent);

        if (requestCode == PickImageId)
        {
            if ((resultCode == Result.Ok) && (intent != null))
            {
                Android.Net.Uri uri = intent.Data;
                Stream stream = ContentResolver.OpenInputStream(uri);

                // Set the Stream as the completion of the Task
                PickImageTaskCompletionSource.SetResult(stream);
            }
            else
            {
                PickImageTaskCompletionSource.SetResult(null);
            }
        }
    }
}

```

`OnActivityResult`Zastąpienie wskazuje plik wybranego obrazu o Android `Uri` obiekt, ale może to zostać przekształcone .NET `Stream` obiektu przez wywołanie metody `OpenInputStream` metody `ContentResolver` obiektu, który został uzyskany z działania `ContentResolver` właściwości.

Podobnie jak w celu wykonania dla systemu iOS, używa implementacji systemu Android `TaskCompletionSource` sygnalizują po zakończeniu zadania. To `TaskCompletionSource` obiektu jest zdefiniowana jako publiczną właściwość w `MainActivity` klasy. Dzięki temu można odwoływać się do właściwości [ `PicturePickerImplementation` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/Droid/PicturePickerImplementation.cs) klasy w projekcie systemu Android. Jest to klasa z `GetImageStreamAsync` metody:

```csharp
[assembly: Dependency(typeof(PicturePickerImplementation))]

namespace DependencyServiceSample.Droid
{
    public class PicturePickerImplementation : IPicturePicker
    {
        public Task<Stream> GetImageStreamAsync()
        {
            // Define the Intent for getting images
            Intent intent = new Intent();
            intent.SetType("image/*");
            intent.SetAction(Intent.ActionGetContent);

            // Get the MainActivity instance
            MainActivity activity = Forms.Context as MainActivity;

            // Start the picture-picker activity (resumes in MainActivity.cs)
            activity.StartActivityForResult(
                Intent.CreateChooser(intent, "Select Picture"),
                MainActivity.PickImageId);

            // Save the TaskCompletionSource object as a MainActivity property
            activity.PickImageTaskCompletionSource = new TaskCompletionSource<Stream>();

            // Return Task object
            return activity.PickImageTaskCompletionSource.Task;
        }
    }
}
```

Uzyskuje dostęp do metody `MainActivity` klasy do kilku celów: dla `PickImageId` dla pole `TaskCompletionSource` właściwości oraz wywołanie `StartActivityForResult`. Ta metoda jest definiowana za pomocą `FormsApplicationActivity` klasy oznacza to klasa podstawowa `MainActivity`.

<a name="UWP_Implementation" />

## <a name="uwp-implementation"></a>Implementacja platformy uniwersalnej systemu Windows

W przeciwieństwie do implementacji systemu Android i iOS nie wymaga wykonania selektora fotografii dla platformy uniwersalnej systemu Windows `TaskCompletionSource` klasy. [ `PicturePickerImplementation` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/UWP/PicturePickerImplementation.cs) Klasy używa [ `FileOpenPicker` ](/uwp/api/Windows.Storage.Pickers.FileOpenPicker/) klasę, aby uzyskać dostęp do biblioteki zdjęcie. Ponieważ `PickSingleFileAsync` metody `FileOpenPicker` jest asynchroniczne, `GetImageStreamAsync` wystarczy użyć metody `await` z tej metody (i innych metod asynchronicznych) i zwraca `Stream` obiektu:


```csharp
[assembly: Dependency(typeof(PicturePickerImplementation))]

namespace DependencyServiceSample.UWP
{
    public class PicturePickerImplementation : IPicturePicker
    {
        public async Task<Stream> GetImageStreamAsync()
        {
            // Create and initialize the FileOpenPicker
            FileOpenPicker openPicker = new FileOpenPicker
            {
                ViewMode = PickerViewMode.Thumbnail,
                SuggestedStartLocation = PickerLocationId.PicturesLibrary,
            };

            openPicker.FileTypeFilter.Add(".jpg");
            openPicker.FileTypeFilter.Add(".jpeg");
            openPicker.FileTypeFilter.Add(".png");

            // Get a file and return a Stream
            StorageFile storageFile = await openPicker.PickSingleFileAsync();

            if (storageFile == null)
            {
                return null;
            }

            IRandomAccessStreamWithContentType raStream = await storageFile.OpenReadAsync();
            return raStream.AsStreamForRead();
        }
    }
}
```

<a name="Windows_Phone_Implementation" />

## <a name="windows-phone-81-implementation"></a>Windows Phone 8.1 implementacji

Implementacja systemu Windows Phone 8.1 jest podobny do implementacji platformy uniwersalnej systemu Windows z wyjątkiem jedną istotną różnicą, że ma duży wpływ: `PickSingleFileAsync` metody `FileOpenPicker` nie jest dostępna. Zamiast tego `GetImageStreamAsync` należy wywołać metodę `PickSingleFileAndContinue`. Ta metoda zwraca wartość, gdy biblioteka fotografii jest wyświetlany użytkownikowi, ale wybór użytkownika zdjęcie zostanie zasygnalizowane przez wywołanie do `OnActivated` metody `App` klasy.

Z tego powodu [ `App` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/WinPhone/App.xaml.cs) klasy utworzonej przez szablon projektu platformy Xamarin.Forms w projekcie systemu Windows Phone 8.1 ma uzupełniony właściwość typu `TaskCompletionSource` i nadpisanie `OnActivated` metody:

```csharp
namespace DependencyServiceSample.WinPhone
{
    public sealed partial class App : Application
    {
        ...
        public TaskCompletionSource<Stream> TaskCompletionSource { set; get; }

        protected async override void OnActivated(IActivatedEventArgs args)
        {
            base.OnActivated(args);

            IContinuationActivatedEventArgs continuationArgs = args as IContinuationActivatedEventArgs;

            if (continuationArgs != null &&
                continuationArgs.Kind == ActivationKind.PickFileContinuation)
            {
                FileOpenPickerContinuationEventArgs pickerArgs = args as FileOpenPickerContinuationEventArgs;

                if (pickerArgs.Files.Count > 0)
                {
                    // Get the file and a Stream
                    StorageFile storageFile = pickerArgs.Files[0];
                    IRandomAccessStreamWithContentType raStream = await storageFile.OpenReadAsync();
                    Stream stream = raStream.AsStreamForRead();

                    // Set the completion of the Task
                    TaskCompletionSource.SetResult(stream);
                }
                else
                {
                    TaskCompletionSource.SetResult(null);
                }
            }
        }
    }
}
```

`OnActivated` Może można wywołać metody kilka powodów, takich jak uruchamiania aplikacji. Kod ogranicza się tylko tych wywołań podczas selektora otwarcie pliku zostało zakończone, a następnie uzyskuje `Stream` obiekt z `StorageFile`.

[ `PicturePickerImplementation` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/WinPhone/PicturePickerImplementation.cs) Klasa zawiera `GetImageStreamAsync` metodę, która tworzy `FileOpenPicker` i wywołania `PickSingleFileAndContainue`:

```csharp
[assembly: Xamarin.Forms.Dependency(typeof(PicturePickerImplementation))]

namespace DependencyServiceSample.WinPhone
{
    public class PicturePickerImplementation : IPicturePicker
    {
        public Task<Stream> GetImageStreamAsync()
        {
            // Create and initialize the FileOpenPicker
            FileOpenPicker openPicker = new FileOpenPicker
            {
                ViewMode = PickerViewMode.Thumbnail,
                SuggestedStartLocation = PickerLocationId.PicturesLibrary,
            };

            openPicker.FileTypeFilter.Add(".jpg");
            openPicker.FileTypeFilter.Add(".jpeg");
            openPicker.FileTypeFilter.Add(".png");

            // Display the picker for a single file (resumes in OnActivated in App.xaml.cs)
            openPicker.PickSingleFileAndContinue();

            // Create a TaskCompletionSource stored in App.xaml.cs
            TaskCompletionSource<Stream> taskCompletionSource = new TaskCompletionSource<Stream>();
            (Application.Current as App).TaskCompletionSource = taskCompletionSource;

            // Return the Task object
            return taskCompletionSource.Task;
        }
    }
}

```

<a name="Implementing_in_Shared_Code" />

## <a name="implementing-in-shared-code"></a>Implementacja w kodzie udostępnionego

Teraz, gdy interfejs został zaimplementowany dla każdej platformy, aplikacja wspólnej biblioteki klas przenośnych mogą wykorzystać go.

[ `App` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/DependencyServiceSample/DependencyServiceSample.cs) Tworzy klasy `Button` do pobrania zdjęcie:

```csharp
Button pickPictureButton = new Button
{
    Text = "Pick Photo",
    VerticalOptions = LayoutOptions.CenterAndExpand,
    HorizontalOptions = LayoutOptions.CenterAndExpand
};
stack.Children.Add(pickPictureButton);
```

`Clicked` Program obsługi używa `DependencyService` klasy do wywołania `GetImageStreamAsync`. Powoduje to wywołanie w projekcie platformy. Jeśli metoda zwraca `Stream` obiektu, a następnie tworzy program obsługi `Image` element dla tego obrazu z `TabGestureRecognizer`i zastępuje `StackLayout` na stronie z tym `Image`:

```csharp
pickPictureButton.Clicked += async (sender, e) =>
{
    pickPictureButton.IsEnabled = false;
    Stream stream = await DependencyService.Get<IPicturePicker>().GetImageStreamAsync();

    if (stream != null)
    {
        Image image = new Image
        {
            Source = ImageSource.FromStream(() => stream),
            BackgroundColor = Color.Gray
        };

        TapGestureRecognizer recognizer = new TapGestureRecognizer();
        recognizer.Tapped += (sender2, args) =>
        {
            (MainPage as ContentPage).Content = stack;
            pickPictureButton.IsEnabled = true;
        };
        image.GestureRecognizers.Add(recognizer);

        (MainPage as ContentPage).Content = image;
    }
    else
    {
        pickPictureButton.IsEnabled = true;
    }
};
```

Naciskając `Image` element zwraca stronę na normalny.


## <a name="related-links"></a>Linki pokrewne

- [Wybierz zdjęcie z galerii (iOS)](https://developer.xamarin.com/recipes/ios/media/video_and_photos/choose_a_photo_from_the_gallery/)
- [Wybierz obraz (Android)](https://developer.xamarin.com/recipes/android/other_ux/pick_image/)
- [DependencyService (przykład)](https://developer.xamarin.com/samples/xamarin-forms/DependencyService/DependencyServiceSample)
