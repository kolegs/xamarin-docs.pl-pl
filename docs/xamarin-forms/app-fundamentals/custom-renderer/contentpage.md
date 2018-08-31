---
title: Dostosowywanie obiektu ContentPage
description: ContentPage jest element wizualny, który wyświetla pojedynczy widok i zajmuje większą część ekranu. W tym artykule przedstawiono sposób tworzenia niestandardowego modułu renderowania dla strony ContentPage, dzięki czemu deweloperzy mogą zastąpić domyślne renderowanie natywnych, dostosowując swoje własne specyficzne dla platformy.
ms.prod: xamarin
ms.assetid: A4E61D93-73D9-4668-8D1C-DB6FC2491822
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 2369b249681b926476cf3938c51c99745eba9098
ms.sourcegitcommit: 8888cb7d75f4469f2a1195b9a426a2e1fbf46bd8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/31/2018
ms.locfileid: "38995745"
---
# <a name="customizing-a-contentpage"></a>Dostosowywanie obiektu ContentPage

_ContentPage jest element wizualny, który wyświetla pojedynczy widok i zajmuje większą część ekranu. W tym artykule przedstawiono sposób tworzenia niestandardowego modułu renderowania dla strony ContentPage, dzięki czemu deweloperzy mogą zastąpić domyślne renderowanie natywnych, dostosowując swoje własne specyficzne dla platformy._

Każdy formant zestawu narzędzi Xamarin.Forms ma towarzyszący modułu renderowania dla każdej platformy, która tworzy wystąpienie kontrolki natywne. Gdy [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) jest renderowany przez aplikację platformy Xamarin.Forms w systemie iOS `PageRenderer` tworzenia wystąpienia klasy, która z kolei tworzy macierzystej `UIViewController` kontroli. Na platformie Android `PageRenderer` tworzy wystąpienie klasy `ViewGroup` kontroli. Na Universal Windows Platform (platformy UWP), `PageRenderer` tworzy wystąpienie klasy `FrameworkElement` kontroli. Aby uzyskać więcej informacji na temat renderowania i klasy natywnych kontrolek, mapowane kontrolek zestawu narzędzi Xamarin.Forms, zobacz [natywne kontrolki i klasy podstawowej renderowania](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Na poniższym diagramie przedstawiono relację między [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) odpowiedniego natywne kontrolki, które ją implementują i:

![](contentpage-images/contentpage-classes.png "Relacja między klasą ContentPage i implementowanie natywne kontrolki")

Proces renderowania może podjąć zalet do zaimplementowania dostosowań specyficzne dla platformy przez utworzenie niestandardowego modułu renderowania dla [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) na każdej platformie. Proces ten jest w następujący sposób:

1. [Utwórz](#Creating_the_Xamarin.Forms_Page) strony zestawu narzędzi Xamarin.Forms.
1. [Używanie](#Consuming_the_Xamarin.Forms_Page) strony z zestawu narzędzi Xamarin.Forms.
1. [Utwórz](#Creating_the_Page_Renderer_on_each_Platform) niestandardowego modułu renderowania dla strony na każdej platformie.

Każdy element zostaną teraz dokładniej omówione w implementacji `CameraPage` zapewnia kamery na żywo, źródła danych i możliwość przechwytywania zdjęcie.

<a name="Creating_the_Xamarin.Forms_Page" />

## <a name="creating-the-xamarinforms-page"></a>Tworzenie strony zestawu narzędzi Xamarin.Forms

Niezmienione [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) można dodać do projektu udostępnionego zestawu narzędzi Xamarin.Forms, jak pokazano w poniższym przykładzie kodu XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="CustomRenderer.CameraPage">
    <ContentPage.Content>
    </ContentPage.Content>
</ContentPage>
```

Podobnie, plik CodeBehind dla [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) również powinna pozostać niezmieniona, jak pokazano w poniższym przykładzie kodu:

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

Poniższy przykład kodu pokazuje, jak strony można tworzyć w języku C#:

```csharp
public class CameraPageCS : ContentPage
{
    public CameraPageCS ()
    {
    }
}
```

Wystąpienie `CameraPage` będzie służyć do wyświetlania na żywo aparatu źródła danych na każdej platformie. Dostosowywanie formantu będą przeprowadzane w niestandardowego modułu renderowania, dzięki czemu nie dodatkowe implementacja jest wymagana w `CameraPage` klasy.

<a name="Consuming_the_Xamarin.Forms_Page" />

## <a name="consuming-the-xamarinforms-page"></a>Wykorzystywanie strony zestawu narzędzi Xamarin.Forms

Pusty `CameraPage` musi zostać wyświetlony przez aplikację platformy Xamarin.Forms. Operacja wykonywana, gdy przycisk na `MainPage` dotknięcie wystąpienia, które z kolei wykonuje `OnTakePhotoButtonClicked` metodzie, jak pokazano w poniższym przykładzie kodu:

```csharp
async void OnTakePhotoButtonClicked (object sender, EventArgs e)
{
    await Navigation.PushAsync (new CameraPage ());
}
```

Ten kod po prostu przechodzi do `CameraPage`, na które niestandardowe programy renderujące umożliwia dostosowywanie wyglądu strony na każdej platformie.

<a name="Creating_the_Page_Renderer_on_each_Platform" />

## <a name="creating-the-page-renderer-on-each-platform"></a>Tworzenie modułu renderowania strony na każdej platformie

Proces tworzenia klasy niestandardowego modułu renderowania jest następująca:

1. Utwórz podklasę `PageRenderer` klasy.
1. Zastąp `OnElementChanged` metodę, która renderuje natywnych logiki strony i zapisu do dostosowania strony. `OnElementChanged` Metoda jest wywoływana, gdy zostanie utworzony odpowiedni formant zestawu narzędzi Xamarin.Forms.
1. Dodaj `ExportRenderer` atrybutów do klasy modułu renderowania strony, aby określić, że będą używane do renderowania strony zestawu narzędzi Xamarin.Forms. Ten atrybut służy do rejestrowania niestandardowego modułu renderowania przy użyciu zestawu narzędzi Xamarin.Forms.

> [!NOTE]
> Jest to opcjonalne zapewnić mechanizm renderujący stronę w każdym projekcie platformy. Jeśli renderowanie strony nie jest zarejestrowany, domyślne renderowanie strony będą używane.

Na poniższym diagramie przedstawiono obowiązki każdego projektu w przykładowej aplikacji wraz z relacji między nimi:

![](contentpage-images/solution-structure.png "CameraPage niestandardowego modułu renderowania projektu obowiązki")

`CameraPage` Wystąpienia jest renderowany przy specyficzne dla platformy `CameraPageRenderer` klas, które wynikają z `PageRenderer` klasy dla danej platformy. Skutkuje to każdego `CameraPage` wystąpienia są renderowane przy użyciu kanału informacyjnego kamery na żywo, jak pokazano na poniższych zrzutach ekranu:

![](contentpage-images/screenshots.png "CameraPage na każdej platformie")

`PageRenderer` Klasy ujawnia `OnElementChanged` metody, która jest wywoływana po utworzeniu strony zestawu narzędzi Xamarin.Forms do renderowania odpowiedniej kontrolki natywne. Ta metoda przyjmuje `ElementChangedEventArgs` parametr, który zawiera `OldElement` i `NewElement` właściwości. Te właściwości reprezentują element zestawu narzędzi Xamarin.Forms, modułu renderowania *został* dołączyć i element zestawu narzędzi Xamarin.Forms, modułu renderowania *jest* dołączone do, odpowiednio. W przykładowej aplikacji `OldElement` właściwość będzie miała `null` i `NewElement` właściwość będzie zawierać odwołanie do `CameraPage` wystąpienia.

Zastąpione wersję `OnElementChanged` method in Class metoda `CameraPageRenderer` klasy jest w tym miejscu Przeprowadź Dostosowywanie strony natywnej. Odwołanie do wystąpienia strony zestawu narzędzi Xamarin.Forms, który jest renderowany można uzyskać za pośrednictwem `Element` właściwości.

Każda klasa niestandardowego modułu renderowania zostanie nadany `ExportRenderer` atrybut, który rejestruje modułu renderowania za pomocą zestawu narzędzi Xamarin.Forms. Ten atrybut przyjmuje dwa parametry — Nazwa typu renderowanej strony zestawu narzędzi Xamarin.Forms i nazwę typu niestandardowego modułu renderowania. `assembly` Prefiks atrybutu określa, że ten atrybut ma zastosowanie do całego zestawu.

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
                System.Diagnostics.Debug.WriteLine (@"            ERROR: ", ex.Message);
            }
        }
        ...
    }
}
```

Wywołania do klasy bazowej `OnElementChanged` metoda tworzy wystąpienie systemu iOS `UIViewController` kontroli. Strumień na żywo aparatu renderowania tylko pod warunkiem że mechanizm renderujący nie jest już dołączony do istniejącego elementu zestawu narzędzi Xamarin.Forms, pod warunkiem, że istnieje wystąpienie strony który jest renderowany przez niestandardowego modułu renderowania.

Strona jest następnie dostosowywać przez szereg metod, które używają `AVCapture` interfejsów API, aby zapewnić transmisji strumieniowej na żywo z kamery i możliwość przechwytywania zdjęcie.

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
                System.Diagnostics.Debug.WriteLine(@"            ERROR: ", ex.Message);
            }
        }
        ...
    }
}
```

Wywołania do klasy bazowej `OnElementChanged` metoda tworzy wystąpienie aplikacji Android `ViewGroup` formant, który jest grupą widoków. Strumień na żywo aparatu renderowania tylko pod warunkiem że mechanizm renderujący nie jest już dołączony do istniejącego elementu zestawu narzędzi Xamarin.Forms, pod warunkiem, że istnieje wystąpienie strony który jest renderowany przez niestandardowego modułu renderowania.

Następnie dostosowanego strony za pomocą szeregu metod, które używają `Camera` interfejsu API w celu zapewnienia transmisji strumieniowej na żywo z kamery i możliwość przechwytywania zdjęcie, przed `AddView` metoda jest wywoływana, aby dodać kamery na żywo strumienia interfejsu użytkownika w celu `ViewGroup`. Należy pamiętać, że w systemie Android należy również zastąpić `OnLayout` metodę, aby wykonywać operacje miary i układu w widoku. Aby uzyskać więcej informacji, zobacz [przykładowy moduł renderowania ContentPage](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/contentpage/).

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
                SetupBasedOnStateAsync();

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

Wywołania do klasy bazowej `OnElementChanged` metoda tworzy wystąpienie `FrameworkElement` kontroli wyrenderowaniu strony. Strumień na żywo aparatu renderowania tylko pod warunkiem że mechanizm renderujący nie jest już dołączony do istniejącego elementu zestawu narzędzi Xamarin.Forms, pod warunkiem, że istnieje wystąpienie strony który jest renderowany przez niestandardowego modułu renderowania. Następnie dostosowanego strony za pomocą szeregu metod, które używają `MediaCapture` interfejsu API w celu zapewnienia transmisji strumieniowej na żywo z kamery i możliwość przechwytywania zdjęcie, przed dodaniem dostosowanej strony do `Children` kolekcji do wyświetlenia.

Podczas implementowania niestandardowego modułu renderowania, która pochodzi od klasy `PageRenderer` na platformie UWP, `ArrangeOverride` metoda również powinny być zrealizowane rozmieścić formanty strony, ponieważ podstawowy mechanizm renderujący nie wie, co należy zrobić z nimi. W przeciwnym razie pusta strona wyników. W związku z tym, w tym przykładzie `ArrangeOverride` wywołania metody `Arrange` metody `Page` wystąpienia.

> [!NOTE]
> Jest ważne zatrzymać i usunąć obiekty, które zapewniają dostęp do aparatu w aplikacji platformy uniwersalnej systemu Windows. Niewykonanie tej czynności może kolidować z innymi aplikacjami, które próbują dostęp do aparatu urządzenia. Aby uzyskać więcej informacji, zobacz [wyświetlić podgląd aparatu](https://msdn.microsoft.com/windows/uwp/audio-video-camera/simple-camera-preview-access).

## <a name="summary"></a>Podsumowanie

Ten artykuł ma pokazano sposób tworzenia niestandardowego modułu renderowania dla [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) strony, dzięki czemu deweloperzy mogą zastąpić domyślne renderowanie natywnych, dostosowując swoje własne specyficzne dla platformy. A `ContentPage` to element graficzny, która wyświetla pojedynczy widok i zajmuje większą część ekranu.


## <a name="related-links"></a>Linki pokrewne

- [CustomRendererContentPage (przykład)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/contentpage/)
