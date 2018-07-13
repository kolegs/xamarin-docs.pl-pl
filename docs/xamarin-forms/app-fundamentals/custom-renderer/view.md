---
title: Implementowanie kontrolki View
description: W tym artykule opisano sposób tworzenia niestandardowego modułu renderowania dla zestawu narzędzi Xamarin.Forms formantu niestandardowego, który służy do wyświetlania podglądu strumienia wideo z aparatu fotograficznego urządzenia.
ms.prod: xamarin
ms.assetid: 915E25E7-4A6B-4F34-B7B4-07D5F4B240F2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/10/2018
ms.openlocfilehash: 8ee9926eb3b726673711141e7c75a68b607d02d3
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994705"
---
# <a name="implementing-a-view"></a>Implementowanie kontrolki View

_Kontrolek interfejsu użytkownika niestandardowego zestawu narzędzi Xamarin.Forms powinien pochodzić od klasy widoku, która służy do umieszczania, układy i formanty na ekranie. W tym artykule przedstawiono sposób tworzenia niestandardowego modułu renderowania dla zestawu narzędzi Xamarin.Forms formantu niestandardowego, który służy do wyświetlania podglądu strumienia wideo z aparatu fotograficznego urządzenia._

Każdego widoku interfejsu Xamarin.Forms ma towarzyszący modułu renderowania dla każdej platformy, która tworzy wystąpienie kontrolki natywne. Gdy [ `View` ](xref:Xamarin.Forms.View) jest renderowany przez aplikację platformy Xamarin.Forms w systemie iOS, `ViewRenderer` tworzenia wystąpienia klasy, która z kolei tworzy macierzystej `UIView` kontroli. Na platformie Android `ViewRenderer` klasy tworzy macierzystej `View` kontroli. Na Universal Windows Platform (platformy UWP), `ViewRenderer` klasy tworzy macierzystej `FrameworkElement` kontroli. Aby uzyskać więcej informacji na temat renderowania i klasy natywnych kontrolek, mapowane kontrolek zestawu narzędzi Xamarin.Forms, zobacz [natywne kontrolki i klasy podstawowej renderowania](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Na poniższym diagramie przedstawiono relację między [ `View` ](xref:Xamarin.Forms.View) odpowiedniego natywne kontrolki, które ją implementują i:

![](view-images/view-classes.png "Relacja między widok klasy i jej implementującej klasy natywne")

Proces renderowania może służyć do zaimplementowania dostosowań specyficzne dla platformy przez utworzenie niestandardowego modułu renderowania dla [ `View` ](xref:Xamarin.Forms.View) na każdej platformie. Proces ten jest w następujący sposób:

1. [Utwórz](#Creating_the_Custom_Control) formantu niestandardowego zestawu narzędzi Xamarin.Forms.
1. [Używanie](#Consuming_the_Custom_Control) kontrolki niestandardowej z zestawu narzędzi Xamarin.Forms.
1. [Utwórz](#Creating_the_Custom_Renderer_on_each_Platform) niestandardowego modułu renderowania kontrolki na każdej platformie.

Każdy element zostaną teraz dokładniej omówione w implementacji `CameraPreview` modułu renderowania, który wyświetla podgląd strumienia wideo z aparatu fotograficznego urządzenia. Naciskając strumienia wideo uruchamiać i zatrzymywać ją.

<a name="Creating_the_Custom_Control" />

## <a name="creating-the-custom-control"></a>Tworzenie kontrolki niestandardowej

Można utworzyć niestandardowego formantu przez podklasy [ `View` ](xref:Xamarin.Forms.View) klasy, jak pokazano w poniższym przykładzie kodu:

```csharp
public class CameraPreview : View
{
  public static readonly BindableProperty CameraProperty = BindableProperty.Create (
    propertyName: "Camera",
    returnType: typeof(CameraOptions),
    declaringType: typeof(CameraPreview),
    defaultValue: CameraOptions.Rear);

  public CameraOptions Camera {
    get { return (CameraOptions)GetValue (CameraProperty); }
    set { SetValue (CameraProperty, value); }
  }
}
```

`CameraPreview` Formant niestandardowy zostanie utworzone w projekcie (PCL) biblioteka klas przenośnych i definiuje interfejs API dla formantu. Formant niestandardowy uwidacznia `Camera` właściwość, która służy do określania, czy powinien być wyświetlany strumienia wideo z przodu lub tyłu aparat w urządzeniu. Jeśli wartość nie jest określona dla `Camera` właściwości po utworzeniu kontrolki, jego wartość domyślna to określanie tylnej aparatu.

<a name="Consuming_the_Custom_Control" />

## <a name="consuming-the-custom-control"></a>Korzystanie z kontrolki niestandardowej

`CameraPreview` Formantu niestandardowego może być przywoływany w XAML w projekcie aplikacji PCL deklarowanie przestrzeni nazw dla lokalizacji i używając prefiksu przestrzeni nazw w elemencie kontrolki niestandardowej. Poniższy kod przedstawia przykładowy sposób, w jaki `CameraPreview` kontrolki niestandardowej mogą być wykorzystane przez strony XAML:

```xaml
<ContentPage ...
             xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer"
             ...>
    <ContentPage.Content>
        <StackLayout>
            <Label Text="Camera Preview:" />
            <local:CameraPreview Camera="Rear"
              HorizontalOptions="FillAndExpand" VerticalOptions="FillAndExpand" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

`local` Prefiks przestrzeni nazw może mieć nazwę niczego. Jednak `clr-namespace` i `assembly` wartości muszą być zgodne szczegóły kontrolki niestandardowej. Po zadeklarowaniu przestrzeń nazw prefiks jest używany do odwołania kontrolki niestandardowej.

Poniższy kod przedstawia przykładowy sposób, w jaki `CameraPreview` kontrolki niestandardowej mogą być wykorzystane przez strony C#:

```csharp
public class MainPageCS : ContentPage
{
  public MainPageCS ()
  {
    ...
    Content = new StackLayout {
      Children = {
        new Label { Text = "Camera Preview:" },
        new CameraPreview {
          Camera = CameraOptions.Rear,
          HorizontalOptions = LayoutOptions.FillAndExpand,
          VerticalOptions = LayoutOptions.FillAndExpand
        }
      }
    };
  }
}
```

Wystąpienie `CameraPreview` formantu niestandardowego, będzie używany do wyświetlania podglądu strumienia wideo z aparatu fotograficznego urządzenia. Oprócz opcjonalnie określając wartość dla `Camera` właściwości i dostosowywanie kontrolki będą przeprowadzane w niestandardowego modułu renderowania.

Teraz można dodać niestandardowego modułu renderowania do każdego projektu aplikacji do tworzenia kontrolki podglądu aparatu specyficzne dla platformy.

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>Tworzenie niestandardowego modułu renderowania na każdej platformie

Proces tworzenia klasy niestandardowego modułu renderowania jest następująca:

1. Utwórz podklasę `ViewRenderer<T1,T2>` klasę, która renderuje kontrolki niestandardowej. Pierwszy argument typu należy kontrolka niestandardowa jest modułu renderowania, w tym przypadku `CameraPreview`. Drugi argument typu powinien być natywne formant, który będzie implementowany kontrolki niestandardowej.
1. Zastąp `OnElementChanged` metodę, która renderuje niestandardowe logiki kontrola i zapisz go dostosować. Ta metoda jest wywoływana, gdy zostanie utworzony odpowiedni formant zestawu narzędzi Xamarin.Forms.
1. Dodaj `ExportRenderer` atrybutów do klasy niestandardowego modułu renderowania, aby określić, że będą używane do renderowania formantu niestandardowego zestawu narzędzi Xamarin.Forms. Ten atrybut służy do rejestrowania niestandardowego modułu renderowania przy użyciu zestawu narzędzi Xamarin.Forms.

> [!NOTE]
> W przypadku większości elementów zestawu narzędzi Xamarin.Forms jest opcjonalny w celu zapewnienia niestandardowego modułu renderowania w każdym projekcie platformy. Jeśli nie jest zarejestrowany niestandardowego modułu renderowania, domyślne renderowanie dla klasy podstawowej kontrolki będą używane. Jednakże, niestandardowe programy renderujące są wymagane w każdym projekcie platformy podczas renderowania [widoku](xref:Xamarin.Forms.View) elementu.

Na poniższym diagramie przedstawiono obowiązki każdego projektu w przykładowej aplikacji, oraz relacje między nimi:

![](view-images/solution-structure.png "CameraPreview niestandardowego modułu renderowania projektu obowiązki")

`CameraPreview` Formantu niestandardowego jest renderowany przez klasy renderowania specyficzne dla platformy, które wynikają z `ViewRenderer` klasy dla każdej platformy. Skutkuje to każda `CameraPreview` kontrolek niestandardowych, które są renderowane z kontrolkami specyficzne dla platformy, jak pokazano na poniższych zrzutach ekranu:

![](view-images/screenshots.png "CameraPreview na każdej platformie")

`ViewRenderer` Klasy ujawnia `OnElementChanged` metody, która jest wywoływana podczas tworzenia formantu niestandardowego zestawu narzędzi Xamarin.Forms do renderowania odpowiedniej kontrolki natywne. Ta metoda przyjmuje `ElementChangedEventArgs` parametr, który zawiera `OldElement` i `NewElement` właściwości. Te właściwości reprezentują element zestawu narzędzi Xamarin.Forms, modułu renderowania *został* dołączyć i element zestawu narzędzi Xamarin.Forms, modułu renderowania *jest* dołączone do, odpowiednio. W przykładowej aplikacji `OldElement` właściwość będzie miała `null` i `NewElement` właściwość będzie zawierać odwołanie do `CameraPreview` wystąpienia.

Zastąpione wersję `OnElementChanged` metody, w każdej klasie renderowania specyficzne dla platformy jest używana do wykonywania wystąpienia natywne kontrolki i dostosowywania. `SetNativeControl` Metoda powinna służyć natywnych formantu, a ta metoda również spowoduje przypisanie odwołania kontrolki do `Control` właściwości. Ponadto można uzyskać odwołanie do formantu Xamarin.Forms, który jest renderowany przy użyciu `Element` właściwości.

W niektórych sytuacjach `OnElementChanged` metoda może być wywoływana wiele razy. W związku z tym aby zapobiec przeciekom pamięci, należy uważać podczas tworzenia wystąpienia nowego formantu natywnych. W poniższym przykładzie kodu pokazano podejście do użycia podczas tworzenia wystąpienia nowego formantu natywne w niestandardowego modułu renderowania:

```csharp
protected override void OnElementChanged (ElementChangedEventArgs<NativeListView> e)
{
  base.OnElementChanged (e);

  if (Control == null) {
    // Instantiate the native control and assign it to the Control property with
    // the SetNativeControl method
  }

  if (e.OldElement != null) {
    // Unsubscribe from event handlers and cleanup any resources
  }

  if (e.NewElement != null) {
    // Configure the control and subscribe to event handlers
  }
}
```

Nowy formant natywnych były tworzone tylko raz, gdy `Control` właściwość `null`. Kontrolki powinny być konfigurowane tylko i subskrypcję procedury obsługi zdarzeń, gdy niestandardowego modułu renderowania jest dołączony do nowego elementu zestawu narzędzi Xamarin.Forms. Podobnie programy obsługi zdarzeń, które zostały zasubskrybowane przez powinna składać się wyłącznie usuwania subskrypcji podczas zmiany elementu, który modułu renderowania jest dołączony do. Przyjęcie tego podejścia ma na celu tworzenie wydajnych niestandardowego modułu renderowania, nie odczuwają przecieków pamięci.

Każda klasa niestandardowego modułu renderowania zostanie nadany `ExportRenderer` atrybut, który rejestruje modułu renderowania za pomocą zestawu narzędzi Xamarin.Forms. Ten atrybut przyjmuje dwa parametry — Nazwa typu formantu niestandardowego zestawu narzędzi Xamarin.Forms renderowanego i nazwę typu niestandardowego modułu renderowania. `assembly` Prefiks atrybutu określa, że ten atrybut ma zastosowanie do całego zestawu.

W poniższych sekcjach omówiono wykonania każdej klasy specyficzne dla platformy niestandardowego modułu renderowania.

### <a name="creating-the-custom-renderer-on-ios"></a>Tworzenie niestandardowego modułu renderowania w systemie iOS

Poniższy przykład kodu pokazuje niestandardowego modułu renderowania dla platformy iOS:

```csharp
[assembly: ExportRenderer (typeof(CameraPreview), typeof(CameraPreviewRenderer))]
namespace CustomRenderer.iOS
{
    public class CameraPreviewRenderer : ViewRenderer<CameraPreview, UICameraPreview>
    {
        UICameraPreview uiCameraPreview;

        protected override void OnElementChanged (ElementChangedEventArgs<CameraPreview> e)
        {
            base.OnElementChanged (e);

            if (Control == null) {
                uiCameraPreview = new UICameraPreview (e.NewElement.Camera);
                SetNativeControl (uiCameraPreview);
            }
            if (e.OldElement != null) {
                // Unsubscribe
                uiCameraPreview.Tapped -= OnCameraPreviewTapped;
            }
            if (e.NewElement != null) {
                // Subscribe
                uiCameraPreview.Tapped += OnCameraPreviewTapped;
            }
        }

        void OnCameraPreviewTapped (object sender, EventArgs e)
        {
            if (uiCameraPreview.IsPreviewing) {
                uiCameraPreview.CaptureSession.StopRunning ();
                uiCameraPreview.IsPreviewing = false;
            } else {
                uiCameraPreview.CaptureSession.StartRunning ();
                uiCameraPreview.IsPreviewing = true;
            }
        }
        ...
    }
}
```

Pod warunkiem, że `Control` właściwość `null`, `SetNativeControl` metoda jest wywoływana w celu utworzenia wystąpienia nowego `UICameraPreview` kontroli i przypisywanie odwołania do niej `Control` właściwości. `UICameraPreview` Formant jest specyficzne dla platformy niestandardowy formant, który używa `AVCapture` interfejsów API, aby zapewnić Podgląd strumienia z aparatu fotograficznego. Udostępnia ona `Tapped` zdarzeń, który jest obsługiwany przez `OnCameraPreviewTapped` metody do zatrzymywania i uruchamiania podglądu wideo, gdy jego dotknięcie. `Tapped` Subskrybuje zdarzenia niestandardowego modułu renderowania jest dołączony do nowego elementu zestawu narzędzi Xamarin.Forms, gdy Anulowano tylko, gdy element renderer jest dołączony do zmiany.

### <a name="creating-the-custom-renderer-on-android"></a>Tworzenie niestandardowego modułu renderowania w systemie Android

Poniższy przykład kodu pokazuje niestandardowego modułu renderowania dla platformy systemu Android:

```csharp
[assembly: ExportRenderer(typeof(CustomRenderer.CameraPreview), typeof(CameraPreviewRenderer))]
namespace CustomRenderer.Droid
{
    public class CameraPreviewRenderer : ViewRenderer<CustomRenderer.CameraPreview, CustomRenderer.Droid.CameraPreview>
    {
        CameraPreview cameraPreview;

        public CameraPreviewRenderer(Context context) : base(context)
        {
        }

        protected override void OnElementChanged(ElementChangedEventArgs<CustomRenderer.CameraPreview> e)
        {
            base.OnElementChanged(e);

            if (Control == null)
            {
                cameraPreview = new CameraPreview(Context);
                SetNativeControl(cameraPreview);
            }

            if (e.OldElement != null)
            {
                // Unsubscribe
                cameraPreview.Click -= OnCameraPreviewClicked;
            }
            if (e.NewElement != null)
            {
                Control.Preview = Camera.Open((int)e.NewElement.Camera);

                // Subscribe
                cameraPreview.Click += OnCameraPreviewClicked;
            }
        }

        void OnCameraPreviewClicked(object sender, EventArgs e)
        {
            if (cameraPreview.IsPreviewing)
            {
                cameraPreview.Preview.StopPreview();
                cameraPreview.IsPreviewing = false;
            }
            else
            {
                cameraPreview.Preview.StartPreview();
                cameraPreview.IsPreviewing = true;
            }
        }
        ...
    }
}
```

Pod warunkiem, że `Control` właściwość `null`, `SetNativeControl` metoda jest wywoływana w celu utworzenia wystąpienia nowego `CameraPreview` kontroli i odwołania do niej przypisać `Control` właściwości. `CameraPreview` Formant jest specyficzne dla platformy niestandardowy formant, który używa `Camera` interfejsu API w celu zapewnienia Podgląd strumienia z aparatu fotograficznego. `CameraPreview` Kontroli jest następnie konfigurowana, pod warunkiem, że niestandardowego modułu renderowania jest dołączony do nowego elementu zestawu narzędzi Xamarin.Forms. Ta konfiguracja obejmuje utworzenie nowego native `Camera` obiekt dostępu do aparatu fotograficznego konkretnego sprzętu i rejestrowania programu obsługi zdarzeń do przetworzenia `Click` zdarzeń. Z kolei ten program obsługi uruchamiać i zatrzymywać podglądu wideo po jego dotknięcie. `Click` Zdarzeń jest subskrypcja została anulowana, jeśli element zestawu narzędzi Xamarin.Forms modułu renderowania jest dołączony do zmiany.

### <a name="creating-the-custom-renderer-on-uwp"></a>Tworzenie niestandardowego modułu renderowania na platformy uniwersalnej systemu Windows

Poniższy przykład kodu pokazuje niestandardowego modułu renderowania dla platformy uniwersalnej systemu Windows:

```csharp
[assembly: ExportRenderer(typeof(CameraPreview), typeof(CameraPreviewRenderer))]
namespace CustomRenderer.UWP
{
    public class CameraPreviewRenderer : ViewRenderer<CameraPreview, Windows.UI.Xaml.Controls.CaptureElement>
    {
        ...
        CaptureElement _captureElement;
        bool _isPreviewing;

        protected override void OnElementChanged(ElementChangedEventArgs<CameraPreview> e)
        {
            base.OnElementChanged(e);

            if (Control == null)
            {
                ...
                _captureElement = new CaptureElement();
                _captureElement.Stretch = Stretch.UniformToFill;

                SetupCamera();
                SetNativeControl(_captureElement);
            }
            if (e.OldElement != null)
            {
                // Unsubscribe
                Tapped -= OnCameraPreviewTapped;
                ...
            }
            if (e.NewElement != null)
            {
                // Subscribe
                Tapped += OnCameraPreviewTapped;
            }
        }

        async void OnCameraPreviewTapped(object sender, TappedRoutedEventArgs e)
        {
            if (_isPreviewing)
            {
                await StopPreviewAsync();
            }
            else
            {
                await StartPreviewAsync();
            }
        }
        ...
    }
}
```

Pod warunkiem, że `Control` właściwość `null`, nowy `CaptureElement` zostanie uruchomiony i `SetupCamera` wywoływana jest metoda, która używa `MediaCapture` interfejsu API w celu zapewnienia Podgląd strumienia z aparatu fotograficznego. `SetNativeControl` Następnie wywoływana jest metoda, aby przypisać odwołanie do `CaptureElement` wystąpienia do `Control` właściwości. `CaptureElement` Kontrolować ujawnia `Tapped` zdarzeń, który jest obsługiwany przez `OnCameraPreviewTapped` metody do zatrzymywania i uruchamiania podglądu wideo, gdy jego dotknięcie. `Tapped` Subskrybuje zdarzenia niestandardowego modułu renderowania jest dołączony do nowego elementu zestawu narzędzi Xamarin.Forms, gdy Anulowano tylko, gdy element renderer jest dołączony do zmiany.

> [!NOTE]
> Jest ważne zatrzymać i usunąć obiekty, które zapewniają dostęp do aparatu w aplikacji platformy uniwersalnej systemu Windows. Niewykonanie tej czynności może kolidować z innymi aplikacjami, które próbują dostęp do aparatu urządzenia. Aby uzyskać więcej informacji, zobacz [wyświetlić podgląd aparatu](/windows/uwp/audio-video-camera/simple-camera-preview-access/).

## <a name="summary"></a>Podsumowanie

Ten artykuł ma pokazano sposób tworzenia niestandardowego modułu renderowania dla zestawu narzędzi Xamarin.Forms formantu niestandardowego, który służy do wyświetlania podglądu strumienia wideo z aparatu fotograficznego urządzenia. Kontrolek interfejsu użytkownika niestandardowego zestawu narzędzi Xamarin.Forms powinien pochodzić od [ `View` ](xref:Xamarin.Forms.View) klasy, która służy do umieszczania, układy i formanty na ekranie.


## <a name="related-links"></a>Linki pokrewne

- [CustomRendererView (przykład)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/view/)
