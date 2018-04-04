---
title: Dostosowywanie wartość ContentPage
description: Wartość ContentPage jest element wizualny, która wyświetla pojedynczego widoku i zajmuje większość ekranu. W tym artykule przedstawiono sposób tworzenia niestandardowego modułu renderowania strony wartość ContentPage umożliwiają deweloperom zastąpienie renderowania natywnego domyślne z własne dostosowania specyficzne dla platformy.
ms.prod: xamarin
ms.assetid: A4E61D93-73D9-4668-8D1C-DB6FC2491822
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 58f5a64f85dbe5a6889e6ff598c14fdfd9b0a5df
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="customizing-a-contentpage"></a>Dostosowywanie wartość ContentPage

_Wartość ContentPage jest element wizualny, która wyświetla pojedynczego widoku i zajmuje większość ekranu. W tym artykule przedstawiono sposób tworzenia niestandardowego modułu renderowania strony wartość ContentPage umożliwiają deweloperom zastąpienie renderowania natywnego domyślne z własne dostosowania specyficzne dla platformy._

Formant co platformy Xamarin.Forms ma towarzyszący renderowania dla każdej platformy, która tworzy wystąpienie macierzystego formantu. Gdy [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) jest renderowany przez aplikację platformy Xamarin.Forms w systemie iOS `PageRenderer` tworzenia wystąpienia klasy, która z kolei tworzy natywny `UIViewController` formantu. Na platformie Android `PageRenderer` tworzy wystąpienie klasy `ViewGroup` formantu. Windows Phone i Windows platformy Uniwersalnej `PageRenderer` tworzy wystąpienie klasy `FrameworkElement` formantu. Aby uzyskać więcej informacji na temat klasy macierzystego formantu, mapowane na formanty platformy Xamarin.Forms i renderowania, zobacz [renderowania klasy podstawowej i kontrolki natywne](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Na poniższym diagramie przedstawiono związek między [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) i odpowiednie natywnego formantów, które implementuje ona:

![](contentpage-images/contentpage-classes.png "Relacja między wartość ContentPage klasy i wdrażanie kontrolki natywne")

Proces renderowania można podjąć zaletą do zaimplementowania dostosowań specyficzne dla platformy przez utworzenie niestandardowego modułu renderowania dla [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) na każdej z platform. Proces ten wygląda następująco:

1. [Utwórz](#Creating_the_Xamarin.Forms_Page) strony platformy Xamarin.Forms.
1. [Korzystać z](#Consuming_the_Xamarin.Forms_Page) stronę z platformy Xamarin.Forms.
1. [Utwórz](#Creating_the_Page_Renderer_on_each_Platform) niestandardowego modułu renderowania strony na każdej z platform.

Każdy element teraz omówione zostaną z kolei, aby zaimplementować `CameraPage` zapewnia aparatu na żywo, źródła danych i możliwość przechwytywania zdjęcie.

<a name="Creating_the_Xamarin.Forms_Page" />

## <a name="creating-the-xamarinforms-page"></a>Tworzenie strony platformy Xamarin.Forms

Niezmienionym [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) można dodać do projektu udostępnionego platformy Xamarin.Forms, jak pokazano w poniższym przykładzie kodu XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="CustomRenderer.CameraPage">
    <ContentPage.Content>
    </ContentPage.Content>
</ContentPage>
```

Podobnie, plik CodeBehind dla [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) również powinny pozostać bez zmian, jak pokazano w poniższym przykładzie:

```csharp
public partial class CameraPage : ContentPage
{
    public CameraPage ()
    {
        // A custom renderer is used to display the camera UI
        InitializeComponent ();
    }
}
```

Poniższy przykład kodu pokazuje, jak można utworzyć strony w języku C#:

```csharp
public class CameraPageCS : ContentPage
{
    public CameraPageCS ()
    {
    }
}
```

Wystąpienie `CameraPage` będzie używany do wyświetlania na żywo aparatu źródła danych na każdej z platform. Dostosowywanie formantu zostanie przeprowadzone w niestandardowego modułu renderowania, więc nie dodatkowe implementacja jest wymagana w `CameraPage` klasy.

<a name="Consuming_the_Xamarin.Forms_Page" />

## <a name="consuming-the-xamarinforms-page"></a>Korzystanie z platformy Xamarin.Forms strony

Pustych `CameraPage` musi zostać wyświetlony przez aplikację platformy Xamarin.Forms. Dzieje się tak, gdy przycisk na `MainPage` dotknięciu wystąpienia, który z kolei wykonuje `OnTakePhotoButtonClicked` metody, jak pokazano w poniższym przykładzie:

```csharp
async void OnTakePhotoButtonClicked (object sender, EventArgs e)
{
    await Navigation.PushAsync (new CameraPage ());
}
```

Ten kod po prostu przechodzi do `CameraPage`, na którym niestandardowe moduły renderowania będzie dostosować wygląd strony na każdej z platform.

<a name="Creating_the_Page_Renderer_on_each_Platform" />

## <a name="creating-the-page-renderer-on-each-platform"></a>Tworzenie modułu renderowania strony na każdej platformie

Proces tworzenia klasy niestandardowego modułu renderowania wygląda następująco:

1. Utwórz podklasę `PageRenderer` klasy.
1. Zastąpienie `OnElementChanged` metodę, która renderuje natywnego logiki strony i zapisu do dostosowania strony. `OnElementChanged` Metoda jest wywoływana po utworzeniu odpowiedniego formantu platformy Xamarin.Forms.
1. Dodaj `ExportRenderer` atrybutu klasy renderowania strony, aby określić, że będą używane do renderowania strony platformy Xamarin.Forms. Ten atrybut służy do rejestrowania niestandardowego modułu renderowania z platformy Xamarin.Forms.

> [!NOTE]
> Jest to pozycja opcjonalna zapewnienie renderowania strony, w każdym projekcie platformy. Jeśli renderowania strony nie jest zarejestrowany, domyślne renderowanie strony będzie używany.

Na poniższym diagramie przedstawiono obowiązki każdego projektu w przykładowej aplikacji, wraz z relacji między nimi:

![](contentpage-images/solution-structure.png "Obowiązki CameraPage niestandardowe renderowania projektu:")

`CameraPage` Wystąpienia jest renderowana przez specyficzne dla platformy `CameraPageRenderer` klasy, które wynikają z `PageRenderer` klasy dla tej platformy. Powoduje to w każdym `CameraPage` wystąpienie renderowanego źródło aparatu na żywo, jak pokazano na poniższych zrzutach ekranu:

![](contentpage-images/screenshots.png "CameraPage na każdej platformie")

`PageRenderer` Klasy ujawnia `OnElementChanged` metodę, która jest wywoływana po utworzeniu strony platformy Xamarin.Forms renderowanie odpowiednich macierzystego formantu. Ta metoda przyjmuje `ElementChangedEventArgs` parametr, który zawiera `OldElement` i `NewElement` właściwości. Te właściwości reprezentuje elementu platformy Xamarin.Forms który renderującego *został* dołączyć i elementu platformy Xamarin.Forms który renderującego *jest* dołączony do, odpowiednio. W przykładowej aplikacji `OldElement` właściwość będzie `null` i `NewElement` właściwości będzie zawierać odwołanie do `CameraPage` wystąpienia.

Zastąpiona wersja `OnElementChanged` metoda `CameraPageRenderer` klasa jest używana do wykonywania dostosowania strony natywnej. Odwołanie do wystąpienia page platformy Xamarin.Forms, który jest renderowany można uzyskać za pośrednictwem `Element` właściwości.

Każda klasa niestandardowego modułu renderowania zostanie nadany `ExportRenderer` atrybut, który rejestruje mechanizm renderujący platformy Xamarin.Forms. Atrybut przyjmuje dwa parametry — Nazwa typu renderowanej strony platformy Xamarin.Forms i nazwę typu niestandardowego modułu renderowania. `assembly` Prefiks atrybutu określa, że ten atrybut ma zastosowanie do całego zestawu.

W poniższych sekcjach omówiono wykonania `CameraPageRenderer` niestandardowego modułu renderowania dla każdej platformy.

### <a name="creating-the-page-renderer-on-ios"></a>Tworzenie modułu renderowania strony w systemie iOS

Poniższy przykład kodu pokazuje renderowanie strony dla platformy iOS:

```csharp
[assembly:ExportRenderer (typeof(CameraPage), typeof(CameraPageRenderer))]
namespace CustomRenderer.iOS
{
    public class CameraPageRenderer : PageRenderer
    {
        ...

        protected override void OnElementChanged (VisualElementChangedEventArgs e)
        {
            base.OnElementChanged (e);

            if (e.OldElement != null || Element == null) {
                return;
            }

            try {
                SetupUserInterface ();
                SetupEventHandlers ();
                SetupLiveCameraStream ();
                AuthorizeCameraUse ();
            } catch (Exception ex) {
                System.Diagnostics.Debug.WriteLine (@"          ERROR: ", ex.Message);
            }
        }
        ...
    }
}
```

Wywołania do klasy podstawowej `OnElementChanged` metoda tworzy iOS `UIViewController` formantu. Strumień na żywo aparatu renderowania tylko pod warunkiem że mechanizm renderujący nie jest już dołączony do istniejącego elementu platformy Xamarin.Forms, pod warunkiem, że istnieje wystąpienie strony który jest renderowany przez niestandardowego modułu renderowania.

Strona jest następnie dostosowane przez szereg metod, które używają `AVCapture` interfejsów API w celu zapewnienia strumień na żywo z aparatu fotograficznego oraz możliwość przechwytywania zdjęcie.

### <a name="creating-the-page-renderer-on-android"></a>Tworzenie modułu renderowania strony w systemie Android

Poniższy przykład kodu pokazuje renderowanie strony dla platformy systemu Android:

```csharp
[assembly: ExportRenderer(typeof(CameraPage), typeof(CameraPageRenderer))]
namespace CustomRenderer.Droid
{
    public class CameraPageRenderer : PageRenderer, TextureView.ISurfaceTextureListener
    {
        ...
        public CameraPageRenderer(Context context) : base(context)
        {
        }

        protected override void OnElementChanged(ElementChangedEventArgs<Page> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null || Element == null)
            {
                return;
            }

            try
            {
                SetupUserInterface();
                SetupEventHandlers();
                AddView(view);
            }
            catch (Exception ex)
            {
                System.Diagnostics.Debug.WriteLine(@"           ERROR: ", ex.Message);
            }
        }
        ...
    }
}
```

Wywołania do klasy podstawowej `OnElementChanged` metoda tworzy Android `ViewGroup` kontroli, która to grupa widoków. Strumień na żywo aparatu renderowania tylko pod warunkiem że mechanizm renderujący nie jest już dołączony do istniejącego elementu platformy Xamarin.Forms, pod warunkiem, że istnieje wystąpienie strony który jest renderowany przez niestandardowego modułu renderowania.

Strona jest następnie dostosowane przez wywoływanie szereg metod korzystających z `Camera` interfejsu API w celu zapewnienia strumień na żywo z aparatu fotograficznego oraz możliwość przechwytywania fotografii, przed `AddView` metoda jest wywoływana, aby dodać aparatu na żywo strumienia interfejsu użytkownika Służącego do `ViewGroup`.

### <a name="creating-the-page-renderer-on-windows-phone"></a>Tworzenie modułu renderowania strony na Windows Phone

Poniższy przykład kodu pokazuje renderowanie strony dla platformy Windows Phone:

```csharp
[assembly: ExportRenderer (typeof(CameraPage), typeof(CameraPageRenderer))]
namespace CustomRenderer.WinPhone81
{
    public class CameraPageRenderer : PageRenderer
    {
        ...

        protected override void OnElementChanged (VisualElementChangedEventArgs e)
        {
            base.OnElementChanged (e);

            if (e.OldElement != null || Element == null) {
                return;
            }

            try {
                ...
                var container = ContainerElement as Canvas;

                SetupUserInterface ();
                SetupEventHandlers ();
                SetupLiveCameraStream ();
                container.Children.Add (page);
            }
            ...
        }

        protected override Size ArrangeOverride(Size finalSize)
        {
            page.Arrange(new Windows.Foundation.Rect(0, 0, finalSize.Width, finalSize.Height));
            return finalSize;
        }
        ...
    }
}
```

Wywołania do klasy podstawowej `OnElementChanged` metoda tworzy Windows Phone `Canvas` kontroli realizacją strony. Strumień na żywo aparatu renderowania tylko pod warunkiem że mechanizm renderujący nie jest już dołączony do istniejącego elementu platformy Xamarin.Forms, pod warunkiem, że istnieje wystąpienie strony który jest renderowany przez niestandardowego modułu renderowania.

Na platformie Windows Phone typu odwołanie do natywnego strony używane na platformie jest możliwy za pośrednictwem `ContainerElement` właściwości, z `Canvas` kontrolować są typu odwołanie do `FrameworkElement`. Strona jest następnie dostosowane przez wywoływanie szereg metod korzystających z `MediaCapture` interfejsu API w celu zapewnienia strumień na żywo z aparatu fotograficznego oraz możliwość przechwytywania zdjęcie przed dodaniem do dostosowanej strony `Canvas` do wyświetlenia.

Podczas implementowania niestandardowego modułu renderowania, która jest pochodną `PageRenderer` na środowiska uruchomieniowego systemu Windows `ArrangeOverride` metody również powinny być implementowane ułożyć formantów strony, ponieważ podstawowy mechanizm renderujący nie może ustalić, co należy zrobić z nimi. W przeciwnym razie wartość pusta strona wyników. W związku z tym, w tym przykładzie `ArrangeOverride` wywołania metody `Arrange` metoda `Page` wystąpienia.

> [!NOTE]
> Należy zatrzymać i usuwania obiektów, które zapewniają dostęp do kamery w aplikacji Windows Phone 8.1 WinRT. Błąd w tym celu może zakłócać inne aplikacje, które próbują uzyskać dostęp aparatu fotograficznego urządzenia. Aby uzyskać więcej informacji, zobacz `CleanUpCaptureResourcesAsync` metody w projekcie Windows Phone w rozwiązaniu próbki i [Szybki Start: Przechwytywanie obrazu wideo przy użyciu interfejsu API MediaCapture](https://msdn.microsoft.com/library/windows/apps/xaml/dn642092.aspx).

### <a name="creating-the-page-renderer-on-uwp"></a>Tworzenie modułu renderowania strony na platformy uniwersalnej systemu Windows

Poniższy przykład kodu pokazuje renderowanie strony dla platformy uniwersalnej systemu Windows:

```csharp
[assembly: ExportRenderer(typeof(CameraPage), typeof(CameraPageRenderer))]
namespace CustomRenderer.UWP
{
    public class CameraPageRenderer : PageRenderer
    {
        ...
        protected override void OnElementChanged(ElementChangedEventArgs<Xamarin.Forms.Page> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null || Element == null)
            {
                return;
            }

            try
            {
                ...
                SetupUserInterface();
                SetupCamera();

                this.Children.Add(page);
            }
            ...
        }

        protected override Size ArrangeOverride(Size finalSize)
        {
            page.Arrange(new Windows.Foundation.Rect(0, 0, finalSize.Width, finalSize.Height));
            return finalSize;
        }
        ...
    }
}

```

Wywołania do klasy podstawowej `OnElementChanged` metoda tworzy `FrameworkElement` kontroli realizacją strony. Strumień na żywo aparatu renderowania tylko pod warunkiem że mechanizm renderujący nie jest już dołączony do istniejącego elementu platformy Xamarin.Forms, pod warunkiem, że istnieje wystąpienie strony który jest renderowany przez niestandardowego modułu renderowania. Strona jest następnie dostosowane przez wywoływanie szereg metod korzystających z `MediaCapture` interfejsu API w celu zapewnienia strumień na żywo z aparatu fotograficznego oraz możliwość przechwytywania zdjęcie przed dodaniem do dostosowanej strony `Children` kolekcji do wyświetlenia.

Podczas implementowania niestandardowego modułu renderowania, która jest pochodną `PageRenderer` na platformy uniwersalnej systemu Windows, `ArrangeOverride` metody również powinny być implementowane ułożyć formantów strony, ponieważ podstawowy mechanizm renderujący nie może ustalić, co należy zrobić z nimi. W przeciwnym razie wartość pusta strona wyników. W związku z tym, w tym przykładzie `ArrangeOverride` wywołania metody `Arrange` metoda `Page` wystąpienia.

> [!NOTE]
> Należy zatrzymać i usuwania obiektów, które zapewniają dostęp do kamery w aplikacji platformy uniwersalnej systemu Windows. Błąd w tym celu może zakłócać inne aplikacje, które próbują uzyskać dostęp aparatu fotograficznego urządzenia. Aby uzyskać więcej informacji, zobacz [wyświetlić podgląd aparatu](https://msdn.microsoft.com/windows/uwp/audio-video-camera/simple-camera-preview-access).

## <a name="summary"></a>Podsumowanie

Ten artykuł ma przedstawiono sposób tworzenia niestandardowego modułu renderowania dla [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) strony umożliwiają deweloperom zastąpienie renderowania natywnego domyślne z własne dostosowania specyficzne dla platformy. A `ContentPage` jest element wizualny, która wyświetla pojedynczego widoku i zajmuje większość ekranu.


## <a name="related-links"></a>Linki pokrewne

- [CustomRendererContentPage (przykład)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/contentpage/)
