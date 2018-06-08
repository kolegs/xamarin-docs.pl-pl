---
title: Implementowanie widoku
description: Formanty interfejsu użytkownika platformy Xamarin.Forms powinien pochodzić od klasy widoku, który służy do umieszczania układy i kontroli na ekranie. W tym artykule przedstawiono sposób tworzenia niestandardowego modułu renderowania dla platformy Xamarin.Forms kontrolki niestandardowej, która jest używana do wyświetlania podglądu strumienia wideo z aparatu fotograficznego urządzenia.
ms.prod: xamarin
ms.assetid: 915E25E7-4A6B-4F34-B7B4-07D5F4B240F2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/10/2018
ms.openlocfilehash: d270a23fb2fee67e5e3dd415771c975aeb682854
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/07/2018
ms.locfileid: "34848489"
---
# <a name="implementing-a-view"></a>Implementowanie widoku

_Formanty interfejsu użytkownika platformy Xamarin.Forms powinien pochodzić od klasy widoku, który służy do umieszczania układy i kontroli na ekranie. W tym artykule przedstawiono sposób tworzenia niestandardowego modułu renderowania dla platformy Xamarin.Forms kontrolki niestandardowej, która jest używana do wyświetlania podglądu strumienia wideo z aparatu fotograficznego urządzenia._

Każdy widok platformy Xamarin.Forms ma towarzyszący renderowania dla każdej platformy, która tworzy wystąpienie macierzystego formantu. Gdy [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) jest renderowany przez aplikację platformy Xamarin.Forms w systemie iOS, `ViewRenderer` tworzenia wystąpienia klasy, która z kolei tworzy natywny `UIView` formantu. Na platformie Android `ViewRenderer` natywny tworzy wystąpienie klasy `View` formantu. W systemie Windows platformy Uniwersalnej, `ViewRenderer` natywny tworzy wystąpienie klasy `FrameworkElement` formantu. Aby uzyskać więcej informacji na temat klasy macierzystego formantu, mapowane na formanty platformy Xamarin.Forms i renderowania, zobacz [renderowania klasy podstawowej i kontrolki natywne](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Na poniższym diagramie przedstawiono związek między [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) i odpowiednie natywnego formantów, które implementuje ona:

![](view-images/view-classes.png "Relacja między jego implementującej klasy natywnej i klasy widoku")

Proces renderowania może służyć do zaimplementowania dostosowań specyficzne dla platformy przez utworzenie niestandardowego modułu renderowania dla [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) na każdej z platform. Proces ten wygląda następująco:

1. [Utwórz](#Creating_the_Custom_Control) kontrolkę niestandardową platformy Xamarin.Forms.
1. [Korzystać z](#Consuming_the_Custom_Control) kontrolka niestandardowa z platformy Xamarin.Forms.
1. [Utwórz](#Creating_the_Custom_Renderer_on_each_Platform) niestandardowego modułu renderowania dla formantu w każdej z platform.

Każdy element teraz omówione zostaną z kolei, aby zaimplementować `CameraPreview` renderowania wyświetlający podglądu strumienia wideo z aparatu fotograficznego urządzenia. Naciskając pozycję w strumieniu wideo spowoduje zatrzymanie i należy ją uruchomić.

<a name="Creating_the_Custom_Control" />

## <a name="creating-the-custom-control"></a>Tworzenie formantu niestandardowego

Formant niestandardowy mogą być tworzone przez podklasy [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) klasy, jak pokazano w poniższym przykładzie:

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

`CameraPreview` Formant niestandardowy jest tworzony w projekcie (PCL) biblioteki klas przenośnych i definiuje interfejsu API dla formantu. Udostępnia kontrolkę niestandardową `Camera` właściwość, która służy do określania, czy powinien być wyświetlany strumienia wideo z przodu lub tylnej aparatu w urządzeniu. Jeśli wartość nie jest określona dla `Camera` właściwości, gdy formant nie zostanie utworzony, domyślne Określanie tylnej aparatu.

<a name="Consuming_the_Custom_Control" />

## <a name="consuming-the-custom-control"></a>Korzystanie z formantu niestandardowego

`CameraPreview` Formant niestandardowy może być przywoływany w XAML w projekcie PCL deklarowanie przestrzeni nazw dla lokalizacji, a następnie użyć prefiksu przestrzeni nazw w elemencie kontrolki niestandardowej. Poniższy kod przedstawia przykład sposobu `CameraPreview` kontrolki niestandardowej, może być zużyte przez strony XAML:

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

`local` Prefiks przestrzeni nazw może mieć nazwę żadnych czynności. Jednak `clr-namespace` i `assembly` wartości muszą być zgodne szczegóły kontrolki niestandardowej. Po zadeklarowaniu obszaru nazw prefiks jest używany do odwołania kontrolki niestandardowej.

Poniższy kod przedstawia przykład sposobu `CameraPreview` kontrolki niestandardowej mogą być używane przez stronę C#:

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

Wystąpienie `CameraPreview` kontrolki niestandardowej będzie używany do wyświetlania podglądu strumienia wideo z aparatu fotograficznego urządzenia. Jako uzupełnienie opcjonalnie określając wartość `Camera` właściwości, dostosowywanie formantu zostanie przeprowadzone w niestandardowego modułu renderowania.

Teraz można dodać niestandardowego modułu renderowania do każdego projektu aplikacji do tworzenia kontrolek podglądu aparatu fotograficznego specyficzne dla platformy.

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>Tworzenie niestandardowego modułu renderowania na każdej platformie

Proces tworzenia klasy niestandardowego modułu renderowania wygląda następująco:

1. Utwórz podklasę `ViewRenderer<T1,T2>` klasy, która renderuje kontrolkę niestandardową. Pierwszy argument typu powinien być kontrolki niestandardowej jest mechanizm renderujący, w tym przypadku `CameraPreview`. Drugi argument typu powinny być macierzystego formantu, który będzie implementowany kontrolki niestandardowej.
1. Zastąpienie `OnElementChanged` metodę, która renderuje niestandardowej logiki kontroli i zapisu, aby dostosować go. Ta metoda jest wywoływana po utworzeniu odpowiedniego formantu platformy Xamarin.Forms.
1. Dodaj `ExportRenderer` atrybutu klasy niestandardowego modułu renderowania, aby określić, że będą używane do renderowania Kontrolki niestandardowe platformy Xamarin.Forms. Ten atrybut służy do rejestrowania niestandardowego modułu renderowania z platformy Xamarin.Forms.

> [!NOTE]
> W przypadku większości elementów platformy Xamarin.Forms jest opcjonalne zapewnić niestandardowego modułu renderowania w każdym projekcie platformy. Jeśli nie jest zarejestrowany niestandardowego modułu renderowania, domyślne renderowanie dla klasy podstawowej formantu będzie używany. Jednak niestandardowe moduły renderowania są wymagane w każdym projekcie platformy podczas renderowania [widoku](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) elementu.

Na poniższym diagramie przedstawiono obowiązki każdego projektu w przykładowej aplikacji, oraz relacje między nimi:

![](view-images/solution-structure.png "Obowiązki CameraPreview niestandardowe renderowania projektu:")

`CameraPreview` Kontrolki niestandardowej jest renderowany przez klasy renderowania specyficzne dla platformy, które wynikają z `ViewRenderer` klasy dla każdej platformy. Powoduje to w każdym `CameraPreview` kontrolki niestandardowej renderowanego specyficzne dla platformy kontroli, jak pokazano na poniższych zrzutach ekranu:

![](view-images/screenshots.png "CameraPreview na każdej platformie")

`ViewRenderer` Klasy ujawnia `OnElementChanged` metodę, która jest wywoływana po utworzeniu platformy Xamarin.Forms formantu niestandardowego do renderowania kontrolki na odpowiednich macierzystego. Ta metoda przyjmuje `ElementChangedEventArgs` parametr, który zawiera `OldElement` i `NewElement` właściwości. Te właściwości reprezentuje elementu platformy Xamarin.Forms który renderującego *został* dołączyć i elementu platformy Xamarin.Forms który renderującego *jest* dołączony do, odpowiednio. W przykładowej aplikacji `OldElement` właściwość będzie `null` i `NewElement` właściwości będzie zawierać odwołanie do `CameraPreview` wystąpienia.

Zastąpiona wersja `OnElementChanged` metody w każdej klasie renderowania specyficzne dla platformy jest miejscem do wykonania podczas tworzenia wystąpienia macierzystego formantu i dostosowywania. `SetNativeControl` Metodę macierzystego formantu, a ta metoda będzie również przypisać odwołania formantu do `Control` właściwości. Ponadto można uzyskać odwołanie do formantu platformy Xamarin.Forms, który jest renderowany przy użyciu `Element` właściwości.

W niektórych sytuacjach `OnElementChanged` metoda może być wywołana wiele razy. W związku z tym aby zapobiec przecieki pamięci, należy uważać podczas tworzenia wystąpienia nowego macierzystego formantu. Podejście do użycia podczas tworzenia wystąpienia nowego macierzystego formantu w niestandardowego modułu renderowania pokazano w poniższym przykładzie kodu:

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

Nowe macierzystego formantu były tworzone tylko raz, jeśli `Control` jest właściwość `null`. Kontrolki tylko należy skonfigurować i subskrypcję procedury obsługi zdarzeń, gdy niestandardowego modułu renderowania jest dołączony do nowego elementu platformy Xamarin.Forms. Podobnie programy obsługi zdarzeń, które zostały subskrybuje powinien mieć tylko anulować w zmiany renderującego dołączonego do elementu. Przyjmowanie takie podejście pomoże utworzyć wydajności renderowania niestandardowych, które nie występują z przecieki pamięci.

Każda klasa niestandardowego modułu renderowania zostanie nadany `ExportRenderer` atrybut, który rejestruje mechanizm renderujący platformy Xamarin.Forms. Atrybut przyjmuje dwa parametry — Nazwa typu formantu niestandardowego platformy Xamarin.Forms renderowanego i nazwę typu niestandardowego modułu renderowania. `assembly` Prefiks atrybutu określa, że ten atrybut ma zastosowanie do całego zestawu.

W poniższych sekcjach omówiono implementacji każdej klasy niestandardowego modułu renderowania specyficzne dla platformy.

### <a name="creating-the-custom-renderer-on-ios"></a>Tworzenie modułu renderowania niestandardowe w systemie iOS

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

Pod warunkiem że `Control` właściwość jest `null`, `SetNativeControl` metoda jest wywoływana można utworzyć nowy `UICameraPreview` kontroli i przypisać odwołanie do niej do `Control` właściwości. `UICameraPreview` Formant jest specyficzne dla platformy kontrolki niestandardowej, która używa `AVCapture` interfejsów API w celu zapewnienia strumienia podglądu z aparatu fotograficznego. Udostępnia ona `Tapped` zdarzeń, który jest obsługiwany przez `OnCameraPreviewTapped` metody do zatrzymywania i uruchamiania podglądu wideo, jest on wybrany. `Tapped` Zdarzeń jest subskrybentem podczas niestandardowego modułu renderowania jest dołączony do nowego elementu platformy Xamarin.Forms i Anulowano subskrypcję tylko, gdy element renderującego jest dołączony do zmiany.

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

Pod warunkiem że `Control` właściwość jest `null`, `SetNativeControl` metoda jest wywoływana można utworzyć nowy `CameraPreview` kontroli i przypisać odwołanie do niej do `Control` właściwości. `CameraPreview` Formant jest specyficzne dla platformy kontrolki niestandardowej, która używa `Camera` interfejsu API w celu zapewnienia strumienia podglądu z aparatu fotograficznego. `CameraPreview` Kontroli jest następnie konfigurowana, pod warunkiem, że niestandardowego modułu renderowania jest dołączony do nowego elementu platformy Xamarin.Forms. Ta konfiguracja obejmuje tworzenie nowych natywny `Camera` obiekt na dostęp aparatu konkretnego sprzętu i rejestrowania programu obsługi zdarzeń do przetwarzania `Click` zdarzeń. Z kolei ten program obsługi będzie zatrzymać i uruchomić podglądu wideo, jest on wybrany. `Click` Zdarzeń jest Anulowano subskrypcję, jeśli element platformy Xamarin.Forms renderującego jest dołączony do zmiany.

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

Pod warunkiem że `Control` właściwość jest `null`, nowy `CaptureElement` zostanie uruchomiony i `SetupCamera` wywoływana jest metoda, która używa `MediaCapture` interfejsu API w celu zapewnienia strumienia podglądu z aparatu fotograficznego. `SetNativeControl` Wywoływana jest metoda następnie przypisać odwołania do `CaptureElement` wystąpienie do `Control` właściwości. `CaptureElement` Kontrolować ujawnia `Tapped` zdarzeń, który jest obsługiwany przez `OnCameraPreviewTapped` metody do zatrzymywania i uruchamiania podglądu wideo, jest on wybrany. `Tapped` Zdarzeń jest subskrybentem podczas niestandardowego modułu renderowania jest dołączony do nowego elementu platformy Xamarin.Forms i Anulowano subskrypcję tylko, gdy element renderującego jest dołączony do zmiany.

> [!NOTE]
> Należy zatrzymać i usuwania obiektów, które zapewniają dostęp do kamery w aplikacji platformy uniwersalnej systemu Windows. Błąd w tym celu może zakłócać inne aplikacje, które próbują uzyskać dostęp aparatu fotograficznego urządzenia. Aby uzyskać więcej informacji, zobacz [wyświetlić podgląd aparatu](/windows/uwp/audio-video-camera/simple-camera-preview-access/).

## <a name="summary"></a>Podsumowanie

W tym artykule ma przedstawiono sposób tworzenia niestandardowego modułu renderowania dla platformy Xamarin.Forms kontrolki niestandardowej, która jest używana do wyświetlania podglądu strumienia wideo z aparatu fotograficznego urządzenia. Formanty interfejsu użytkownika platformy Xamarin.Forms powinien pochodzić od [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) klasy, która służy do umieszczania układy i kontroli na ekranie.


## <a name="related-links"></a>Linki pokrewne

- [CustomRendererView (przykład)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/view/)
