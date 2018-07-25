---
title: Pobrania zdjęcia z biblioteki obrazów
description: W tym artykule wyjaśniono, jak korzystać z zestawu narzędzi Xamarin.Forms DependencyService klasy do pobrania zdjęcia z biblioteki obrazów na telefonie.
ms.prod: xamarin
ms.assetid: 4F51B0E7-6A63-403C-B488-500CCBCE75DD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/06/2017
ms.openlocfilehash: dafa60ff57f34bd4169af48e380079d9637d8d26
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241110"
---
# <a name="picking-a-photo-from-the-picture-library"></a>Pobrania zdjęcia z biblioteki obrazów

W tym artykule opisano proces tworzenia aplikacji, która umożliwia użytkownikom do pobrania zdjęcia z biblioteki obrazów na telefonie. Ponieważ Xamarin.Forms nie zawiera tej funkcji, jest konieczne użycie [ `DependencyService` ](xref:Xamarin.Forms.DependencyService) dostęp do natywnych interfejsów API na każdej platformie.  W tym artykule opisano następujące kroki dotyczące korzystania z `DependencyService` dla tego zadania:

- **[Tworzenie interfejsu](#Creating_the_Interface)**  &ndash; zrozumieć, jak interfejs jest tworzony w współużytkowanym kodem.
- **[iOS implementacji](#iOS_Implementation)**  &ndash; Dowiedz się, jak implementować interfejs w kodzie natywnym dla systemu iOS.
- **[Implementacja systemu android](#Android_Implementation)**  &ndash; Dowiedz się, jak implementować interfejs w kodzie natywnym dla systemu Android.
- **[Universal Windows Platform implementacji](#UWP_Implementation)**  &ndash; Dowiedz się, jak implementować interfejs w kodzie natywnym do uniwersalnej platformy Windows (UWP).
- **[Implementacja w kodzie udostępnionego](#Implementing_in_Shared_Code)**  &ndash; Dowiedz się, jak używać `DependencyService` do wywołania w natywnych implementacji ze współużytkowanym kodem.

<a name="Creating_the_Interface" />

## <a name="creating-the-interface"></a>Tworzenie interfejsu

Najpierw Utwórz interfejs udostępnionego kodu, który wyraża odpowiednich funkcji. W przypadku aplikacji pobrania zdjęcia tylko jedna metoda jest wymagany. To jest zdefiniowany w [ `IPicturePicker` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/DependencyServiceSample/IPicturePicker.cs) interfejsu, biblioteki .NET Standard przykładowego kodu:

```csharp
namespace DependencyServiceSample
{
    public interface IPicturePicker
    {
        Task<Stream> GetImageStreamAsync();
    }
}
```

`GetImageStreamAsync` Metoda jest zdefiniowana jako asynchroniczny, ponieważ metoda musi zwracać się szybko, ale nie może on zwrócić `Stream` obiektu wybranego zdjęcia, dopóki użytkownik nie ma przeglądania biblioteki obrazów i wybrano jedną.

Ten interfejs jest wdrażany na wszystkich platformach przy użyciu kodu specyficznego dla platformy.

<a name="iOS_Implementation" />

## <a name="ios-implementation"></a>Implementacja systemu iOS

Implementacja systemu iOS `IPicturePicker` interfejs używa [ `UIImagePickerController` ](https://developer.xamarin.com/api/type/UIKit.UIImagePickerController/) zgodnie z opisem w [ **Wybierz zdjęcie z galerii** ](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/video_and_photos/choose_a_photo_from_the_gallery) przepisu i [przykładowego kodu](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/video_and_photos/choose_a_photo_from_the_gallery).

Wdrożenia z systemem iOS znajduje się w [ `PicturePickerImplementation` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/iOS/PicturePickerImplementation.cs) klasy w projekcie dla systemu iOS w przykładowym kodzie. Aby uwidocznić tej klasy `DependencyService` menedżera, klasa jest oznaczona symbolem [`assembly`] atrybutu typu `Dependency`, i klasy muszą być publiczne i jawne Implementowanie `IPicturePicker` interfejsu:

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

`GetImageStreamAsync` Metoda tworzy `UIImagePickerController` i inicjuje ją, aby wybrać obrazy z biblioteki zdjęć. Wymagane są dwa programy obsługi zdarzeń: jeden dla, gdy użytkownik wybierze zdjęcia i inne po użytkownik anuluje wyświetlanie biblioteki zdjęć. `PresentModalViewController` Następnie wyświetla biblioteki zdjęć użytkownika.

W tym momencie `GetImageStreamAsync` metoda musi zwracać `Task<Stream>` obiektu do kodu, który ją wywołuje. To zadanie zostało wykonane tylko wtedy, gdy użytkownik zakończy, interakcji z biblioteki zdjęć i nosi nazwę jednego z programów obsługi zdarzeń. W sytuacjach, podobny do powyższego [ `TaskCompletionSource` ](https://msdn.microsoft.com/library/dd449174(v=vs.110).aspx) klasy jest niezbędne. Klasa oferuje `Task` obiektu prawidłowego typu ogólnego do zwrócenia z `GetImageStreamAsync` metod i klas później mogą być sygnalizowane po zakończeniu zadania.

`FinishedPickingMedia` Program obsługi zdarzeń jest wywoływana, gdy użytkownik wybrał obrazu. Program obsługi zapewnia jednak `UIImage` obiektu i `Task` musi zwracać .NET `Stream` obiektu. Odbywa się w dwóch etapach: `UIImage` obiekt jest najpierw konwertowany na plik JPEG w pamięci, przechowywane w `NSData` obiektu, a następnie `NSData` obiekt jest konwertowany na .NET `Stream` obiektu. Wywołanie `SetResult` metody `TaskCompletionSource` obiektu kończy zadanie, podając `Stream` obiektu:

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

Aplikacja systemu iOS wymaga zgody użytkownika na dostęp do biblioteki zdjęć na telefonie. Dodaj następujący kod do `dict` sekcji w pliku Info.plist:

```xml
<key>NSPhotoLibraryUsageDescription</key>
<string>Picture Picker uses photo library</string>
```


<a name="Android_Implementation" />

## <a name="android-implementation"></a>Implementacja systemu android

Implementacja systemu Android używa techniki opisanej w [ **wybierz obraz** ](https://github.com/xamarin/recipes/tree/master/Recipes/android/other_ux/pick_image) przepisu i [przykładowego kodu](https://github.com/xamarin/recipes/tree/master/Recipes/android/other_ux/pick_image). Jednak metoda, która jest wywoływana, gdy użytkownik wybrał obrazu z biblioteki obrazów jest `OnActivityResult` zastąpienia w klasie, która pochodzi od klasy `Activity`. Z tego powodu normalną [ `MainActivity` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/Droid/MainActivity.cs) klasy w projekcie dla systemu Android ma został uzupełniony pola, właściwości i zastępowania metody `OnActivityResult` metody:

```csharp
public class MainActivity : FormsAppCompatActivity
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

`OnActivityResult`Zastąpienie wskazuje plik obrazu wybranego z systemem Android `Uri` obiekt, ale może to być konwertowane na .NET `Stream` obiektu przez wywołanie metody `OpenInputStream` metody `ContentResolver` obiektu, który został uzyskany z działania `ContentResolver` właściwości.

Podobnie jak wykonania dla systemu iOS, Android implementacji korzysta `TaskCompletionSource` celu sygnalizowania, że po ukończeniu zadania. To `TaskCompletionSource` obiekt jest zdefiniowany jako publiczną właściwość w `MainActivity` klasy. Dzięki temu właściwość odwoływać się do [ `PicturePickerImplementation` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/Droid/PicturePickerImplementation.cs) klasy w projekcie dla systemu Android. Jest to klasa, z `GetImageStreamAsync` metody:

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

            // Start the picture-picker activity (resumes in MainActivity.cs)
            MainActivity.Instance.StartActivityForResult(
                Intent.CreateChooser(intent, "Select Picture"),
                MainActivity.PickImageId);

            // Save the TaskCompletionSource object as a MainActivity property
            MainActivity.Instance.PickImageTaskCompletionSource = new TaskCompletionSource<Stream>();

            // Return Task object
            return MainActivity.Instance.PickImageTaskCompletionSource.Task;
        }
    }
}
```

Ta metoda uzyskuje dostęp do `MainActivity` klasy do kilku celów: dla `Instance` właściwości dla `PickImageId` dla pole `TaskCompletionSource` właściwości oraz wywołanie `StartActivityForResult`. Ta metoda jest definiowany przez `FormsAppCompatActivity` klasy, która jest klasą bazową dla `MainActivity`.

<a name="UWP_Implementation" />

## <a name="uwp-implementation"></a>Implementacja platformy uniwersalnej systemu Windows

W odróżnieniu od implementacji systemu Android i iOS nie jest wymagane implementacji selektora fotografii dla platformy uniwersalnej Windows `TaskCompletionSource` klasy. [ `PicturePickerImplementation` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/UWP/PicturePickerImplementation.cs) Klasy używa [ `FileOpenPicker` ](/uwp/api/Windows.Storage.Pickers.FileOpenPicker/) klasy w celu uzyskania dostępu do biblioteki zdjęć. Ponieważ `PickSingleFileAsync` metody `FileOpenPicker` sama jest asynchroniczna, `GetImageStreamAsync` metoda po prostu użyć `await` przy użyciu tej metody (i innych metod asynchronicznych) i zwracają `Stream` obiektu:


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

<a name="Implementing_in_Shared_Code" />

## <a name="implementing-in-shared-code"></a>Wdrażanie w udostępnionego kodu

Teraz, gdy interfejs został zaimplementowany dla poszczególnych platform, aplikacji w bibliotece programu .NET Standard można z niej korzystać.

[ `App` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/DependencyServiceSample/DependencyServiceSample.cs) Tworzy klasę `Button` do pobrania zdjęcia:

```csharp
Button pickPictureButton = new Button
{
    Text = "Pick Photo",
    VerticalOptions = LayoutOptions.CenterAndExpand,
    HorizontalOptions = LayoutOptions.CenterAndExpand
};
stack.Children.Add(pickPictureButton);
```

`Clicked` Program obsługi używa `DependencyService` klasy, aby wywołać `GetImageStreamAsync`. Powoduje to wywołanie w projekcie platformy. Jeśli metoda zwraca `Stream` obiektu, a następnie tworzy program obsługi `Image` element dla tego obrazu z `TabGestureRecognizer`i zastępuje `StackLayout` na stronie, z którego `Image`:

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

Naciskając `Image` strony powróci do typowej wartości elementu.


## <a name="related-links"></a>Linki pokrewne

- [Wybierz zdjęcie z galerii (iOS)](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/video_and_photos/choose_a_photo_from_the_gallery)
- [Wybierz obraz (Android)](https://github.com/xamarin/recipes/tree/master/Recipes/android/other_ux/pick_image)
- [DependencyService (przykład)](https://developer.xamarin.com/samples/xamarin-forms/DependencyService/DependencyServiceSample)
