---
title: "Formanty ręczne aparatu"
description: "AVFoundation Framework umożliwia łatwiejsze niż dotychczas użytkownikom podjęcie zdjęć w celu umożliwienia formanty aparatu ręcznego. Za pomocą tego framework, aplikacja może zająć bezpośrednią kontrolę nad aparatu fokus, białe saldo i ekspozycję. Aplikacja umożliwia także przechwytywania w nawiasach kwadratowych narażenia automatycznie przechwytywania obrazów za pomocą różnych ustawień ekspozycji. W tym artykule potrwa krótki przegląd za pomocą formantów aparatu ręcznego w aplikacji mobilnej proste iOS 8."
ms.topic: article
ms.prod: xamarin
ms.assetid: 56340225-5F3C-4BFC-9A79-61496D7FE5B5
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: f11305fcbf8a5b9bf6552fa31ecfa1c0e8e7a68f
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/22/2018
---
# <a name="manual-camera-controls"></a>Formanty ręczne aparatu

_AVFoundation Framework umożliwia łatwiejsze niż dotychczas użytkownikom podjęcie zdjęć w celu umożliwienia formanty aparatu ręcznego. Za pomocą tego framework, aplikacja może zająć bezpośrednią kontrolę nad aparatu fokus, białe saldo i ekspozycję. Aplikacja umożliwia także przechwytywania w nawiasach kwadratowych narażenia automatycznie przechwytywania obrazów za pomocą różnych ustawień ekspozycji. W tym artykule potrwa krótki przegląd za pomocą formantów aparatu ręcznego w aplikacji mobilnej proste iOS 8._

Formanty aparatu ręczny, dostarczone przez `AVFoundation Framework` w systemie iOS 8, należy zezwolić aplikacji mobilnej wykonać pełną kontrolę nad aparatu fotograficznego urządzenia z systemem iOS. Ten poziom szczegółowych kontroli może służyć do tworzenia profesjonalnych kamer poziomu aplikacji i dostarczenie kompozycji wykonawcy przy Dostosowywanie parametry aparatu podczas wykonywania nadal obrazu lub wideo.

Zastosowanie tych zabezpieczeń również może być przydatne podczas tworzenia aplikacji naukowych lub przemysłowej, gdy wyniki są mniej dostosowana pod kątem poprawności lub zaletą obrazu i są bardziej przeznaczone dla wyróżnianie niektórych funkcji lub elementu obrazu przełączana do.

## <a name="avfoundation-capture-objects"></a>Obiekty AVFoundation przechwytywania

Czy pobieranie wideo lub zdjęć na urządzeniu z systemem iOS przy użyciu aparatu, proces stosowany do przechwycenia tych obrazów jest przede wszystkim takie same. Dotyczy to aplikacji korzystających z formantów aparatu domyślnej zautomatyzowanej lub te, które wykorzystać nowe formanty aparatu ręczny:

 [![](intro-to-manual-camera-controls-images/image1.png "Przegląd obiektów funkcji przechwytywania AVFoundation")](intro-to-manual-camera-controls-images/image1.png#lightbox)

Dane wejściowe są brane z `AVCaptureDeviceInput` do `AVCaptureSession` poprzez `AVCaptureConnection`. Wynik jest albo dane wyjściowe jako obraz nadal lub strumienia wideo. Cały proces jest kontrolowany przez `AVCaptureDevice`.

## <a name="manual-controls-provided"></a>Ręczne kontroli określonej

Przy użyciu nowych interfejsów API dostarczonych przez system iOS 8, aplikacja może przejąć kontrolę nad następujące funkcje aparatu:

-  **Ręczne fokus** — zezwolenie użytkownikom na przejęcie kontroli nad fokus bezpośrednio, aplikacja może zapewniają większą kontrolę nad obrazu podjęte.
-  **Ręczne narażenia** — zapewniając ręcznej kontroli nad narażenia, aplikacja może udostępniać większą swobodę użytkownikom i zezwolić im na osiągnięcie stylizowane wygląd.
-  **Ręczne saldo biały** — biały saldo jest używane do dostosowywania kolor na obrazku — często aby realistyczne. Różne źródła światła mieć temperatury kolorów, a ustawienia aparatu używane do przechwycenia obrazu jest dostosowana do kompensacji tych różnic. Ponownie zezwalając kontrola użytkownika nad saldo białe, użytkownicy mogą wprowadzać dostosowania, które nie mogą być wykonywane automatycznie.


iOS 8 udostępnia rozszerzenia i ulepszenia iOS istniejących interfejsów API, aby zapewnić tym precyzyjną kontrolę nad obrazu proces przechwytywania.

## <a name="bracketed-capture"></a>W nawiasach kwadratowych przechwytywania

Oddzielona przechwytywania na podstawie ustawień od formantów aparatu ręcznego przedstawionych powyżej i umożliwia aplikacji do przechwytywania chwilę w czasie na różne sposoby.

Krótko mówiąc, oddzielona przechwytywania jest serii nadal obrazów wykonanych za pomocą różnych ustawień z obrazu do obrazu.

## <a name="requirements"></a>Wymagania

Poniższe elementy są wymagane do wykonania czynności przedstawionych w tym artykule:

-  **Xcode 7 i iOS 8 lub nowszego** — Xcode 7 i iOS 8 lub nowszych interfejsów API firmy Apple muszą być zainstalowane i skonfigurowane na komputerze dewelopera.
-  **Visual Studio for Mac** — najnowszej wersji programu Visual Studio for Mac powinna być zainstalowana i skonfigurowana na urządzeniu użytkownika.
-  **System iOS 8 urządzenia** — urządzenia z systemem iOS z najnowszą wersją systemu IOS 8. Nie można przetestować funkcji aparatu w symulatorze systemu iOS.


## <a name="general-av-capture-setup"></a>Ustawienia ogólne AV przechwytywania

Nagrywanie wideo na urządzeniu z systemem iOS, ma kodu ogólnej konfiguracji, który jest zawsze wymagane. Ta sekcja obejmuje instalację minimalnego wymaganego do rejestrowania wideo z aparatu fotograficznego urządzenia z systemem iOS i wyświetlania wideo w czasie rzeczywistym w `UIImageView`.

### <a name="output-sample-buffer-delegate"></a>Delegat buforu przykładowe dane wyjściowe

Przede wszystkim potrzebne będzie pełnomocnika, aby monitorować buforu przykładowe dane wyjściowe i wyświetlania obrazu pobierany z bufora `UIImageView` w aplikacji interfejsu użytkownika.

Poniższe procedury monitorujący buforu próbki i zaktualizuj interfejs użytkownika:

```csharp
using System;
using Foundation;
using UIKit;
using System.CodeDom.Compiler;
using System.Collections.Generic;
using System.Linq;
using AVFoundation;
using CoreVideo;
using CoreMedia;
using CoreGraphics;

namespace ManualCameraControls
{
    public class OutputRecorder : AVCaptureVideoDataOutputSampleBufferDelegate
    {
        #region Computed Properties
        public UIImageView DisplayView { get; set; }
        #endregion

        #region Constructors
        public OutputRecorder ()
        {

        }
        #endregion

        #region Private Methods
        private UIImage GetImageFromSampleBuffer(CMSampleBuffer sampleBuffer) {

            // Get a pixel buffer from the sample buffer
            using (var pixelBuffer = sampleBuffer.GetImageBuffer () as CVPixelBuffer) {
                // Lock the base address
                pixelBuffer.Lock (0);

                // Prepare to decode buffer
                var flags = CGBitmapFlags.PremultipliedFirst | CGBitmapFlags.ByteOrder32Little;

                // Decode buffer - Create a new colorspace
                using (var cs = CGColorSpace.CreateDeviceRGB ()) {

                    // Create new context from buffer
                    using (var context = new CGBitmapContext (pixelBuffer.BaseAddress,
                        pixelBuffer.Width,
                        pixelBuffer.Height,
                        8,
                        pixelBuffer.BytesPerRow,
                        cs,
                        (CGImageAlphaInfo)flags)) {

                        // Get the image from the context
                        using (var cgImage = context.ToImage ()) {

                            // Unlock and return image
                            pixelBuffer.Unlock (0);
                            return UIImage.FromImage (cgImage);
                        }
                    }
                }
            }
        }
        #endregion

        #region Override Methods
        public override void DidOutputSampleBuffer (AVCaptureOutput captureOutput, CMSampleBuffer sampleBuffer, AVCaptureConnection connection)
        {
            // Trap all errors
            try {
                // Grab an image from the buffer
                var image = GetImageFromSampleBuffer(sampleBuffer);

                // Display the image
                if (DisplayView !=null) {
                    DisplayView.BeginInvokeOnMainThread(() => {
                        // Set the image
                        if (DisplayView.Image != null) DisplayView.Image.Dispose();
                        DisplayView.Image = image;

                        // Rotate image to the correct display orientation
                        DisplayView.Transform = CGAffineTransform.MakeRotation((float)Math.PI/2);
                    });
                }

                // IMPORTANT: You must release the buffer because AVFoundation has a fixed number
                // of buffers and will stop delivering frames if it runs out.
                sampleBuffer.Dispose();
            }
            catch(Exception e) {
                // Report error
                Console.WriteLine ("Error sampling buffer: {0}", e.Message);
            }
        }
        #endregion
    }
}
```

Z tej procedury w miejscu `AppDelegate` można zmodyfikować, aby otworzyć sesję przechwytywania AV do rejestrowania źródła wideo na żywo.

### <a name="creating-an-av-capture-session"></a>Tworzenie sesji przechwytywania AV

Sesja przechwytywania AV jest używana do sterowania nagrywanie wideo na żywo z aparatu fotograficznego urządzenia z systemem iOS i jest wymagane do pobrania wideo do aplikacji systemu iOS. Ponieważ przykładzie `ManualCameraControl` Przykładowa aplikacja używa sesji przechwytywania w kilku różnych miejscach, zostanie on skonfigurowany w `AppDelegate` i dostępne dla całej aplikacji.

Wykonaj poniższe czynności, aby zmodyfikować aplikację `AppDelegate` i Dodaj kod konieczne:


1. Kliknij dwukrotnie `AppDelegate.cs` plików w Eksploratorze rozwiązań, aby otworzyć do edycji.
1. Dodaj następujące instrukcje using do początku pliku:
    
    ```
    using System;
    using Foundation;
    using UIKit;
    using System.CodeDom.Compiler;
    using System.Collections.Generic;
    using System.Linq;
    using AVFoundation;
    using CoreVideo;
    using CoreMedia;
    using CoreGraphics;
    using CoreFoundation;
    ```

1. Dodaj następujące zmienne prywatne i właściwości obliczanej `AppDelegate` klasy:
    
    ```
    #region Private Variables
    private NSError Error;
    #endregion
    
    #region Computed Properties
    public override UIWindow Window {get;set;}
    public bool CameraAvailable { get; set; }
    public AVCaptureSession Session { get; set; }
    public AVCaptureDevice CaptureDevice { get; set; }
    public OutputRecorder Recorder { get; set; }
    public DispatchQueue Queue { get; set; }
    public AVCaptureDeviceInput Input { get; set; }
    #endregion
    ```
  
1. Zastąp metodę Zakończono i zmień, aby:
    
    ```
    public override void FinishedLaunching (UIApplication application)
    {
        // Create a new capture session
        Session = new AVCaptureSession ();
        Session.SessionPreset = AVCaptureSession.PresetMedium;
    
        // Create a device input
        CaptureDevice = AVCaptureDevice.DefaultDeviceWithMediaType (AVMediaType.Video);
        if (CaptureDevice == null) {
            // Video capture not supported, abort
            Console.WriteLine ("Video recording not supported on this device");
            CameraAvailable = false;
            return;
        }
    
        // Prepare device for configuration
        CaptureDevice.LockForConfiguration (out Error);
        if (Error != null) {
            // There has been an issue, abort
            Console.WriteLine ("Error: {0}", Error.LocalizedDescription);
            CaptureDevice.UnlockForConfiguration ();
            return;
        }
    
        // Configure stream for 15 frames per second (fps)
        CaptureDevice.ActiveVideoMinFrameDuration = new CMTime (1, 15);
    
        // Unlock configuration
        CaptureDevice.UnlockForConfiguration ();
    
        // Get input from capture device
        Input = AVCaptureDeviceInput.FromDevice (CaptureDevice);
        if (Input == null) {
            // Error, report and abort
            Console.WriteLine ("Unable to gain input from capture device.");
            CameraAvailable = false;
            return;
        }
    
        // Attach input to session
        Session.AddInput (Input);
    
        // Create a new output
        var output = new AVCaptureVideoDataOutput ();
        var settings = new AVVideoSettingsUncompressed ();
        settings.PixelFormatType = CVPixelFormatType.CV32BGRA;
        output.WeakVideoSettings = settings.Dictionary;
    
        // Configure and attach to the output to the session
        Queue = new DispatchQueue ("ManCamQueue");
        Recorder = new OutputRecorder ();
        output.SetSampleBufferDelegate (Recorder, Queue);
        Session.AddOutput (output);
    
        // Let tabs know that a camera is available
        CameraAvailable = true;
    }
    ```  
  
1. Zapisz zmiany w pliku.


Z tego kodu w miejscu kontrolek aparatu ręcznego można łatwo zaimplementować eksperymenty i testowania.

## <a name="manual-focus"></a>Ręczne fokus

Zezwolenie użytkownikom końcowym zająć formanty fokus bezpośrednio, aplikacja może zapewniać więcej artystyczny kontrolę nad obrazu podjęte.

Na przykład professional fotografa można wygładzanie fokus obrazu do osiągnięcia [efekt Bokeh](http://en.wikipedia.org/wiki/Bokeh):

[![](intro-to-manual-camera-controls-images/image2.png "Efekt Bokeh")](intro-to-manual-camera-controls-images/image2.png#lightbox)

Lub Utwórz [efekt ściągnięcia fokus](http://www.mediacollege.com/video/camera/focus/pull.html), takich jak:

[![](intro-to-manual-camera-controls-images/image3.png "Efekt ściągania fokus")](intro-to-manual-camera-controls-images/image3.png#lightbox)

Służące lub Edytor aplikacje medyczne aplikacja może być programowane poruszanie obiektywu dla eksperymentów. W obu przypadkach nowy interfejs API umożliwia użytkownikowi końcowemu lub aplikacji, aby przejąć kontrolę nad fokusu w czasie obrazu jest zajęta.

### <a name="how-focus-works"></a>Jak działa fokus

Przed omówieniem szczegóły kontrolowanie fokusu w aplikacji systemu IOS 8. Spójrzmy szybkie na działanie fokusu w urządzeniu z systemem iOS:

[![](intro-to-manual-camera-controls-images/image4.png "Jak działa fokusu w urządzeniu z systemem iOS")](intro-to-manual-camera-controls-images/image4.png#lightbox)

Jasny wprowadza obiektyw na urządzeniu z systemem iOS i koncentruje się na czujnik obrazu. Odległość obiektyw z czujnika Określa, gdzie punkt centralny (obszar, gdy obraz zostanie wyświetlony najlepszą jakość obrazu) jest w relacji z czujnika. Dostępne jest obiektyw z czujnika, obiekty odległość prawdopodobnie najlepszą jakość obrazu i bliżej, obok obiektów prawdopodobnie najlepszą jakość obrazu.

W urządzeniu z systemem iOS obiektywu jest przenoszony bliżej do lub od czujnika pól i sprężyny. W związku z tym dokładne pozycjonowanie obiektywu nie jest możliwe, będą się różnić z urządzenia do urządzenia, a mogą mieć wpływ parametry, takie jak orientacji urządzenia lub wieku urządzenia i sprężyny.

### <a name="important-focus-terms"></a>Warunki ważne fokus

Podczas pracy z fokusem, istnieje kilka warunków, które dewelopera należy zapoznać się z:

-  **Głębokość pola** — odległość między obiektami najbliższej i najdalej fokusu. 
-  **Makro** — jest to niedługo koniec spektrum fokus, najbliższego odległość, jaką można skupić się obiektywu.
-  **Infinity** — jest to daleko koniec spektrum fokus, najdalszych odległość, na jaką można skupić się obiektywu.
-  **Odległość Hyperfocal** — jest to punkt w spektrum fokus który najdalszych obiektu w ramce tylko na końcu daleko fokus. Innymi słowy jest to głównych pozycji, które pozwala zmaksymalizować głębokość pola. 
-  **Pozycja obiektywu** — to co kontroluje wszystkie powyższe inne postanowienia. To jest odległość obiektyw z czujnika i tym samym kontrolerem fokus.


Z tych warunków i wiedzy na uwadze nowe formanty fokus ręcznego może pomyślnie wdrożonych w aplikacji systemu iOS 8.

### <a name="existing-focus-controls"></a>Dysponujące fokus

System iOS 7 i wcześniejsze wersje, pod warunkiem dysponujące fokus za pośrednictwem `FocusMode`właściwość jako:

-   `AVCaptureFocusModeLocked` — Fokus jest zablokowana na jednym fokus.
-   `AVCaptureFocusModeAutoFocus` — Aparatu wachlarzy obiektyw przez wszystkie punkty centralny, aż znajdzie sharp fokus, a następnie pozostaje.
-   `AVCaptureFocusModeContinuousAutoFocus` — Aparatu refocuses zawsze, gdy wykryje warunek poza fokusu.


Istniejące formanty w nim również podane można ustawić punktu odsetek za pośrednictwem`FocusPointOfInterest` właściwości, dzięki czemu użytkownik może nacisnąć skoncentrować się na określonym obszarze. Aplikacji mogą również śledzić ruch obiektyw monitorowanie `IsAdjustingFocus` właściwości.

Ponadto ograniczenie zakresu został dostarczony przez `AutoFocusRangeRestriction` właściwość jako:

-   `AVCaptureAutoFocusRangeRestrictionNear` — Ogranicza autofocus głębokości pobliskich. Przydatne w sytuacjach, takich jak skanowanie kod QR lub kod kreskowy.
-   `AVCaptureAutoFocusRangeRestrictionFar` — Ogranicza autofocus oddalonych głębokości. Przydatne w sytuacjach, w których obiekty, które nie ma znaczenia w polu widoku (na przykład, że ramka okna).


Na koniec istnieje `SmoothAutoFocus` właściwość, która spowalnia algorytm fokus automatycznie i etapami mniejszą liczbę do należy unikać przenoszenia artefakty podczas rejestrowania wideo.

### <a name="new-focus-controls-in-ios-8"></a>Nowe formanty fokusu w systemie iOS 8

Oprócz funkcji już dostarczona przez system iOS 7 i nowszym następujące funkcje są teraz dostępne do kontrolowania fokusu w systemie iOS 8:

-  Pełne ręcznej kontroli nad pozycji obiektyw podczas blokowania fokus.
-  Klucz wartość obserwacji pozycji obiektyw w dowolnym trybie fokusu.


Do implementowania funkcji powyżej `AVCaptureDevice` klasy została zmodyfikowana, aby uwzględnić tylko do odczytu `LensPosition` właściwość używana do Pobierz bieżące położenie obiektyw.

Ręczne kontrolować pozycji obiektyw przechwytywania urządzenie musi być w trybie fokusu zablokowany. Przykład:

 `CaptureDevice.FocusMode = AVCaptureFocusMode.Locked;`

`SetFocusModeLocked` Metody przechwytywania urządzenia służy do korygowania pozycja obiektyw. Aby otrzymywać powiadomienia, gdy zmiany zostały uwzględnione, należy podać procedury opcjonalne wywołania zwrotnego. Przykład:

```csharp
ThisApp.CaptureDevice.LockForConfiguration(out Error);
ThisApp.CaptureDevice.SetFocusModeLocked(Position.Value,null);
ThisApp.CaptureDevice.UnlockForConfiguration();
```

Jak w powyższym kodzie przechwytywania urządzenie musi być zablokowany dla konfiguracji przed zmiany w położeniu obiektyw. Prawidłowe wartości pozycji obiektyw to od 0,0 do 1,0.

### <a name="manual-focus-example"></a>Przykład ręczne fokus

Kod ogólne AV ustawienia przechwytywania w miejscu `UIViewController` mogą być dodawane do scenorysu aplikacji lub skonfigurowane w następujący sposób:

[![](intro-to-manual-camera-controls-images/image5.png "UIViewController można dodać do aplikacji scenorysu i skonfigurowane w sposób pokazany poniżej")](intro-to-manual-camera-controls-images/image5.png#lightbox)

Widok zawiera następujące główne elementy:

-  A `UIImageView` który wyświetli źródła wideo.
-  A `UISegmentedControl` spowoduje to zmianę trybu fokusu z automatycznego na zablokowana.
-  A `UISlider` które wyświetlić i zaktualizować bieżącego położenia obiektyw.


Wykonaj następujące czynności przesyłania zapasowe kontroler widoku dla sterowanie fokusem ręczny:


1. Dodaj następujące instrukcje using:

    ```csharp
    using System;
    using Foundation;
    using UIKit;
    using System.CodeDom.Compiler;
    using System.Collections.Generic;
    using System.Linq;
    using AVFoundation;
    using CoreVideo;
    using CoreMedia;
    using CoreGraphics;
    using CoreFoundation;
    using System.Timers;
    ```  
  
1. Dodaj następujących zmiennych prywatnych:

    ```csharp
    #region Private Variables
    private NSError Error;
    private bool Automatic = true;
    #endregion
    ```  
  
1. Dodaj następujące właściwości obliczanej:

    ```csharp
    #region Computed Properties
    public AppDelegate ThisApp {
        get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
    }
    public Timer SampleTimer { get; set; }
    #endregion
    ```  
  
1. Zastąpienie `ViewDidLoad` — metoda i Dodaj następujący kod:

    ```csharp
    public override void ViewDidLoad ()
    {
        base.ViewDidLoad ();
    
        // Hide no camera label
        NoCamera.Hidden = ThisApp.CameraAvailable;
    
        // Attach to camera view
        ThisApp.Recorder.DisplayView = CameraView;
    
        // Create a timer to monitor and update the UI
        SampleTimer = new Timer (5000);
        SampleTimer.Elapsed += (sender, e) => {
            // Update position slider
            Position.BeginInvokeOnMainThread(() =>{
                Position.Value = ThisApp.Input.Device.LensPosition;
            });
        };
    
        // Watch for value changes
        Segments.ValueChanged += (object sender, EventArgs e) => {
    
            // Lock device for change
            ThisApp.CaptureDevice.LockForConfiguration(out Error);
    
            // Take action based on the segment selected
            switch(Segments.SelectedSegment) {
            case 0:
                // Activate auto focus and start monitoring position
                Position.Enabled = false;
                ThisApp.CaptureDevice.FocusMode = AVCaptureFocusMode.ContinuousAutoFocus;
                SampleTimer.Start();
                Automatic = true;
                break;
            case 1:
                // Stop auto focus and allow the user to control the camera
                SampleTimer.Stop();
                ThisApp.CaptureDevice.FocusMode = AVCaptureFocusMode.Locked;
                Automatic = false;
                Position.Enabled = true;
                break;
            }
    
            // Unlock device
            ThisApp.CaptureDevice.UnlockForConfiguration();
        };
    
        // Monitor position changes
        Position.ValueChanged += (object sender, EventArgs e) => {
    
            // If we are in the automatic mode, ignore changes
            if (Automatic) return;
    
            // Update Focus position
            ThisApp.CaptureDevice.LockForConfiguration(out Error);
            ThisApp.CaptureDevice.SetFocusModeLocked(Position.Value,null);
            ThisApp.CaptureDevice.UnlockForConfiguration();
        };
    }
    ```  
  
1. Zastąpienie `ViewDidAppear` — metoda i dodaj następujące polecenie, aby rozpocząć rejestrowanie, załadowanie widoku:

    ```csharp
    public override void ViewDidAppear (bool animated)
    {
        base.ViewDidAppear (animated);
    
        // Start udating the display
        if (ThisApp.CameraAvailable) {
            // Remap to this camera view
            ThisApp.Recorder.DisplayView = CameraView;
    
            ThisApp.Session.StartRunning ();
            SampleTimer.Start ();
        }
    }
    ```  
  
1. Aparatem w trybie automatycznym suwak przeniesie automatycznie zgodnie z aparatu fotograficznego dopasowuje fokus:

    [![](intro-to-manual-camera-controls-images/image6.png "Suwak przeniesie automatycznie zgodnie z aparatu dopasowuje fokusu w tej przykładowej aplikacji")](intro-to-manual-camera-controls-images/image6.png#lightbox)
1. Wybierz segment zablokowany, a następnie przeciągnij suwak pozycji, aby dostosować położenie obiektyw ręcznie:

    [![](intro-to-manual-camera-controls-images/image7.png "Ręcznie dostosowania pozycji obiektyw")](intro-to-manual-camera-controls-images/image7.png#lightbox)
1. Zatrzymaj aplikację.


Powyższy kod pokazuje, jak monitorować pozycji obiektyw, gdy aparat jest w trybie automatycznym lub użyj suwaka do sterowania położeniem obiektyw, gdy jest on w trybie zablokowany.

## <a name="manual-exposure"></a>Ręczne zagrożeń

Narażenia odwołuje się do jasność obrazu względem jasności źródła i jest określany przez ilość światła trafienia czujnika, jak długo i korzyści stopniem czujnik (ISO mapowanie). Zapewniając ręcznej kontroli nad narażenia, aplikacja może udostępniać większą swobodę dla użytkownika końcowego i zezwolić im na osiągnięcie stylizowane wygląd.

Za pomocą formantów narażenia ręczny, użytkownik może zająć obrazu z nierealistycznie jasny ciemny i moody:

[![](intro-to-manual-camera-controls-images/image8.png "Obraz przedstawiający skutkami nierealistycznie jasny ciemny i moody próbki")](intro-to-manual-camera-controls-images/image8.png#lightbox)

Ponownie można to zrobić automatycznie za pomocą sterowanie programowe, w przypadku aplikacji naukowych lub za pomocą formantów ręczne udostępniane przez interfejs użytkownika aplikacji. W obu przypadkach nowego systemu iOS 8 narażenia interfejsów API zapewniają precyzyjną kontrolę nad ekspozycję aparatu.

### <a name="how-exposure-works"></a>Jak działa zagrożeń

Przed omówieniem szczegóły kontrolowanie zagrożeń w aplikacji systemu IOS 8. Spójrzmy szybki w sposób działania zagrożeń:

[![](intro-to-manual-camera-controls-images/image9.png "Jak działa zagrożeń")](intro-to-manual-camera-controls-images/image9.png#lightbox)

Dostępne są następujące trzy podstawowe elementy, które pochodzą ze sobą do kontrolowania narażenia:

-  **Migawki szybkości** — jest to czas, który migawki jest otwarty, aby umożliwić światła na czujnik aparatu. Krótszy czas migawki jest otwarty, mniej światła let i dokładnym obraz jest (rozmycia mniej ruchu). Im dłuższy migawki jest otwarty, więcej światła jest pozostawić w i bardziej ruchu rozmycia, która występuje.
-  **Mapowanie ISO** — to jest termin pobierają z fotografii filmu i odwołuje się do wrażliwości chemicznych w filmu światło. ISO niskiej wartości filmu ma mniej ziarna i bardziej precyzyjną odtwarzania kolorów; niskich wartości ISO na cyfrowe czujniki ma mniej szumu czujnika, ale mniej jasność. Im wyższa wartość ISO, jaśniejszy obraz, ale więcej szumu czujnika. "ISO" na cyfrowe czujnik jest miarą [korzyści elektronicznych](http://en.wikipedia.org/wiki/Gain), nie funkcji fizycznej. 
-  **Otwarcie obiektywu** — jest to rozmiar otwierania obiektyw. Na wszystkich urządzeniach z systemem iOS otwarcie obiektyw został rozwiązany, tak aby były tylko dwie wartości, które mogą służyć do Dopasowanie ekspozycji migawki i ISO.


### <a name="how-continuous-auto-exposure-works"></a>Jak ciągła ustawienie automatyczne współdziała zagrożeń

Przed learning sposób działania ręczne zagrożeń, jest bardzo pomysł, aby zrozumieć, jak ciągła narażenia automatycznie działa w urządzeniu z systemem iOS.

[![](intro-to-manual-camera-controls-images/image10.png "Jak automatycznie ciągłego narażenia działa w urządzeniu z systemem iOS")](intro-to-manual-camera-controls-images/image10.png#lightbox)

Najpierw jest automatycznie bloku zagrożeń, ma zadania obliczania idealne zagrożeń i jest stale podawana Statystyka zliczania. Używa tych informacji do obliczania optymalnej kombinację ISO i migawki, aby uzyskać sceny również włączone. Ten cykl nazywa się AE pętli.

### <a name="how-locked-exposure-works"></a>Jak zablokowanym działa zagrożeń

Następnie Przeanalizujmy jak zablokowanym działa narażenia na urządzeniach z systemem iOS.

[![](intro-to-manual-camera-controls-images/image11.png "Jak zablokowanym narażenia działa na urządzeniach z systemem iOS")](intro-to-manual-camera-controls-images/image11.png#lightbox)

Ponownie masz bloku narażenia automatycznie próbuje obliczanie optymalnej iOS i wartości czasu trwania. Jednak w tym trybie bloku AE jest odłączony od aparatu Statystyka zliczania.

### <a name="existing-exposure-controls"></a>Dysponujące zagrożeń

System iOS 7 i nowszych Podaj następujące dysponujące narażenia za pośrednictwem `ExposureMode` właściwości:

-   `AVCaptureExposureModeLocked` — Przykłady sceny raz i używa tych wartości w ramach sceny.
-   `AVCaptureExposureModeContinuousAutoExposure` — Przykłady sceny stale, aby upewnić się również włączone.


`ExposurePointOfInterest` Może służyć do naciśnij, aby udostępnić sceny Wybieranie obiektu docelowego w celu udostępnienia na i aplikacji można monitorować `AdjustingExposure` właściwości, aby zobaczyć, gdy jest dopasowywany zagrożeń.

### <a name="new-exposure-controls-in-ios-8"></a>Nowe formanty zagrożeń w systemie iOS 8

Oprócz funkcji już dostarczona przez system iOS 7 i nowszym następujące funkcje są teraz dostępne do kontrolowania zagrożeń w systemie iOS 8:

-  Pełni ręczne narażenia niestandardowych.
-  GET, Set i klucz wartość obserwować systemów IOS i migawki (czas trwania).


Do implementowania funkcji powyżej, nowy `AVCaptureExposureModeCustom` tryb został dodany. Jeśli kamery w trybie niestandardowym, poniższy kod może służyć do dostosowania czas trwania narażenia i ISO:

```csharp
CaptureDevice.LockForConfiguration(out Error);
CaptureDevice.LockExposure(DurationValue,ISOValue,null);
CaptureDevice.UnlockForConfiguration();
```

W trybach Auto i zablokowane aplikacji można dostosować odchylenia standardowego automatyczne zagrożeń przy użyciu następującego kodu:

```csharp
CaptureDevice.LockForConfiguration(out Error);
CaptureDevice.SetExposureTargetBias(Value,null);
CaptureDevice.UnlockForConfiguration();
```

Zakresy minimalne i maksymalne ustawienia zależą od urządzenie, na którym aplikacja jest uruchomiona, nigdy nie należy ich twarde kodowane. Zamiast tego należy użyć następujących właściwości można pobrać zakresów wartości minimalną i maksymalną:

-   `CaptureDevice.MinExposureTargetBias` 
-   `CaptureDevice.MaxExposureTargetBias` 
-   `CaptureDevice.ActiveFormat.MinISO` 
-   `CaptureDevice.ActiveFormat.MaxISO` 
-   `CaptureDevice.ActiveFormat.MinExposureDuration` 
-   `CaptureDevice.ActiveFormat.MaxExposureDuration` 


Jak w powyższym kodzie urządzenia przechwytywania musi być zablokowany dla konfiguracji przed zmiana zagrożeń.

### <a name="manual-exposure-example"></a>Przykład ręczne zagrożeń

Kod ogólne AV ustawienia przechwytywania w miejscu `UIViewController` mogą być dodawane do scenorysu aplikacji lub skonfigurowane w następujący sposób:

[![](intro-to-manual-camera-controls-images/image12.png "UIViewController można dodać do aplikacji scenorysu i skonfigurowane w sposób pokazany poniżej")](intro-to-manual-camera-controls-images/image12.png#lightbox)

Widok zawiera następujące główne elementy:

-  A `UIImageView` który wyświetli źródła wideo.
-  A `UISegmentedControl` spowoduje to zmianę trybu fokusu z automatycznego na zablokowana.
-  Cztery `UISlider` formantów, które będą Pokaż i przesunięcie, czas trwania, ISO i odchylenia aktualizacji.


Wykonaj następujące czynności przesyłania zapasowe kontroler widoku dla formantu narażenia ręczny:


1. Dodaj następujące instrukcje using:
    
    ```
    using System;
    using Foundation;
    using UIKit;
    using System.CodeDom.Compiler;
    using System.Collections.Generic;
    using System.Linq;
    using AVFoundation;
    using CoreVideo;
    using CoreMedia;
    using CoreGraphics;
    using CoreFoundation;
    using System.Timers;
    ```  
  
1. Dodaj następujących zmiennych prywatnych:

    ```csharp
    #region Private Variables
    private NSError Error; 
    private bool Automatic = true;
    private nfloat ExposureDurationPower = 5;
    private nfloat ExposureMinimumDuration = 1.0f/1000.0f;
    #endregion
    ```  
  
1. Dodaj następujące właściwości obliczanej:

    ```csharp
    #region Computed Properties
    public AppDelegate ThisApp {
        get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
    }
    public Timer SampleTimer { get; set; }
    #endregion
    ```  
  
1. Zastąpienie `ViewDidLoad` — metoda i Dodaj następujący kod:

    ```csharp
    public override void ViewDidLoad ()
    {
        base.ViewDidLoad ();
    
        // Hide no camera label
        NoCamera.Hidden = ThisApp.CameraAvailable;
    
        // Attach to camera view
        ThisApp.Recorder.DisplayView = CameraView;
    
        // Set min and max values
        Offset.MinValue = ThisApp.CaptureDevice.MinExposureTargetBias;
        Offset.MaxValue = ThisApp.CaptureDevice.MaxExposureTargetBias;
    
        Duration.MinValue = 0.0f;
        Duration.MaxValue = 1.0f;
    
        ISO.MinValue = ThisApp.CaptureDevice.ActiveFormat.MinISO;
        ISO.MaxValue = ThisApp.CaptureDevice.ActiveFormat.MaxISO;
    
        Bias.MinValue = ThisApp.CaptureDevice.MinExposureTargetBias;
        Bias.MaxValue = ThisApp.CaptureDevice.MaxExposureTargetBias;
    
        // Create a timer to monitor and update the UI
        SampleTimer = new Timer (5000);
        SampleTimer.Elapsed += (sender, e) => {
            // Update position slider
            Offset.BeginInvokeOnMainThread(() =>{
                Offset.Value = ThisApp.Input.Device.ExposureTargetOffset;
            });
    
            Duration.BeginInvokeOnMainThread(() =>{
                var newDurationSeconds = CMTimeGetSeconds(ThisApp.Input.Device.ExposureDuration);
                var minDurationSeconds = Math.Max(CMTimeGetSeconds(ThisApp.CaptureDevice.ActiveFormat.MinExposureDuration), ExposureMinimumDuration);
                var maxDurationSeconds = CMTimeGetSeconds(ThisApp.CaptureDevice.ActiveFormat.MaxExposureDuration);
                var p = (newDurationSeconds - minDurationSeconds) / (maxDurationSeconds - minDurationSeconds);
                Duration.Value = (float)Math.Pow(p, 1.0f/ExposureDurationPower);
            });
    
            ISO.BeginInvokeOnMainThread(() => {
                ISO.Value = ThisApp.Input.Device.ISO;
            });
    
            Bias.BeginInvokeOnMainThread(() => {
                Bias.Value = ThisApp.Input.Device.ExposureTargetBias;
            });
        };
    
        // Watch for value changes
        Segments.ValueChanged += (object sender, EventArgs e) => {
    
            // Lock device for change
            ThisApp.CaptureDevice.LockForConfiguration(out Error);
    
            // Take action based on the segment selected
            switch(Segments.SelectedSegment) {
            case 0:
                // Activate auto exposure and start monitoring position
                Duration.Enabled = false;
                ISO.Enabled = false;
                ThisApp.CaptureDevice.ExposureMode = AVCaptureExposureMode.ContinuousAutoExposure;
                SampleTimer.Start();
                Automatic = true;
                break;
            case 1:
                // Lock exposure and allow the user to control the camera
                SampleTimer.Stop();
                ThisApp.CaptureDevice.ExposureMode = AVCaptureExposureMode.Locked;
                Automatic = false;
                Duration.Enabled = false;
                ISO.Enabled = false;
                break;
            case 2:
                // Custom exposure and allow the user to control the camera
                SampleTimer.Stop();
                ThisApp.CaptureDevice.ExposureMode = AVCaptureExposureMode.Custom;
                Automatic = false;
                Duration.Enabled = true;
                ISO.Enabled = true;
                break;
            }
    
            // Unlock device
            ThisApp.CaptureDevice.UnlockForConfiguration();
        };
    
        // Monitor position changes
        Duration.ValueChanged += (object sender, EventArgs e) => {
    
            // If we are in the automatic mode, ignore changes
            if (Automatic) return;
    
            // Calculate value
            var p = Math.Pow(Duration.Value,ExposureDurationPower);
            var minDurationSeconds = Math.Max(CMTimeGetSeconds(ThisApp.CaptureDevice.ActiveFormat.MinExposureDuration),ExposureMinimumDuration);
            var maxDurationSeconds = CMTimeGetSeconds(ThisApp.CaptureDevice.ActiveFormat.MaxExposureDuration);
            var newDurationSeconds = p * (maxDurationSeconds - minDurationSeconds) +minDurationSeconds;
    
            // Update Focus position
            ThisApp.CaptureDevice.LockForConfiguration(out Error);
            ThisApp.CaptureDevice.LockExposure(CMTime.FromSeconds(p,1000*1000*1000),ThisApp.CaptureDevice.ISO,null);
            ThisApp.CaptureDevice.UnlockForConfiguration();
        };
    
        ISO.ValueChanged += (object sender, EventArgs e) => {
    
            // If we are in the automatic mode, ignore changes
            if (Automatic) return;
    
            // Update Focus position
            ThisApp.CaptureDevice.LockForConfiguration(out Error);
            ThisApp.CaptureDevice.LockExposure(ThisApp.CaptureDevice.ExposureDuration,ISO.Value,null);
            ThisApp.CaptureDevice.UnlockForConfiguration();
        };
    
        Bias.ValueChanged += (object sender, EventArgs e) => {
    
            // If we are in the automatic mode, ignore changes
            // if (Automatic) return;
    
            // Update Focus position
            ThisApp.CaptureDevice.LockForConfiguration(out Error);
            ThisApp.CaptureDevice.SetExposureTargetBias(Bias.Value,null);
            ThisApp.CaptureDevice.UnlockForConfiguration();
        };
    }
    ```  
  
1. Zastąpienie `ViewDidAppear` — metoda i dodaj następujące polecenie, aby rozpocząć rejestrowanie, załadowanie widoku:

    ```csharp
    public override void ViewDidAppear (bool animated)
    {
        base.ViewDidAppear (animated);
    
        // Start udating the display
        if (ThisApp.CameraAvailable) {
            // Remap to this camera view
            ThisApp.Recorder.DisplayView = CameraView;
    
            ThisApp.Session.StartRunning ();
            SampleTimer.Start ();
        }
    }
    ```  
  
1. Aparatem w trybie automatycznym suwaki przeniesie automatycznie zgodnie z aparatu fotograficznego dopasowuje zagrożeń:

    [![](intro-to-manual-camera-controls-images/image13.png "Suwaki przeniesie automatycznie zgodnie z aparatu fotograficznego dopasowuje zagrożeń")](intro-to-manual-camera-controls-images/image13.png#lightbox)
1. Wybierz segment zablokowane i przeciągnij suwak odchylenia, aby dopasować odchylenia automatyczne narażenia ręcznie:

    [![](intro-to-manual-camera-controls-images/image14.png "Dopasowywanie odchylenia automatyczne narażenia ręcznie")](intro-to-manual-camera-controls-images/image14.png#lightbox)
1. Wybierz niestandardowe segmentu i z suwaków czas trwania i ISO ręcznie kontrolować zagrożeń:

    [![](intro-to-manual-camera-controls-images/image15.png "Przeciągnij suwaki czas trwania i ISO, aby ręcznie kontroli zagrożeń")](intro-to-manual-camera-controls-images/image15.png#lightbox)
1. Zatrzymaj aplikację.


Powyższy kod pokazuje, jak monitorować ustawienia zagrożeń, gdy aparat jest w trybie automatycznym i jak kontrolować ryzyko w trybach zablokowany lub niestandardowe za pomocą suwaków.

## <a name="manual-white-balance"></a>Ręczne saldo białe

Formanty saldo biały umożliwiają użytkownikom dopasować saldo colosr w obrazie, aby je wyglądało bardziej. Różne źródła światła mieć temperatury kolorów, a ustawienia aparatu używane do przechwycenia obrazu musi zostać dostosowana to kompensacji tych różnic. Ponownie zezwalając kontrola użytkownika nad białe saldo one można dostosować professional będących procedury automatycznego niezdolne do osiągnięcia artystyczny efekty.

[![](intro-to-manual-camera-controls-images/image16.png "Obraz przykładowej przedstawiający saldo biały ręcznego dopasowania")](intro-to-manual-camera-controls-images/image16.png#lightbox)

Na przykład czasu letniego ma blueish rzutowanie świateł żarówki wolframu mieć odcień powielane, żółty pomarańczowy. (Złudzenia, kolory "zimnych" mają wyższe temperatury kolor niż "ciepłych" kolorów. Kolor temperatury są środek fizyczne nie Percepcyjna jeden).

Człowieka zdanie jest bardzo dobrze sobie radzi kompensowanie dla różnice w temperatury kolorów, ale jest to nie aparatu. Aparat polega na zwiększania kolor na pasma przeciwnego dostosowywać różnice kolorów.

Nowy system iOS 8 narażenia interfejsu API zezwala aplikacji na przejęcie kontroli nad proces i podaj precyzyjną kontrolę nad ustawienia równoważenia białe aparatu.

### <a name="how-white-balance-works"></a>Jak białe działa saldo

Przed omówieniem szczegóły kontrolowanie białe saldo w aplikacji systemu IOS 8. Spójrzmy szybki w sposób białe pracy saldo:

Do badania wrażenie kolor [CIE 1931 RGB kolorów i miejsca kolor XYZ CIE 1931](http://en.wikipedia.org/wiki/CIE_1931_color_space) są pierwszy matematycznie zdefiniowanych przestrzenie. 1931 zostały utworzone przez Międzynarodowej Komisji na oświetlenia ().

[![](intro-to-manual-camera-controls-images/image17.png "Przestrzeń kolorów RGB CIE 1931 i CIE 1931 XYZ przestrzeń kolorów")](intro-to-manual-camera-controls-images/image17.png#lightbox)

Powyżej wykres ten przedstawia nam wszystkie kolory widoczny dla człowieka oka bezpośrednich do jasnozielony jasny czerwony niebieski. Dowolnego punktu na diagramie można wykreślić wartości X i Y, jak pokazano na powyższym wykresie.

Widoczne na wykresie istnieją wartości X i Y, które można wykreślić na wykresie, które będą poza zakresem widzenia ludzi, i w związku z tym nie można odtworzyć kolorów te według aparatu.

Krzywej mniejszym wykresie powyżej jest nazywany [Planckian Locus](http://en.wikipedia.org/wiki/Planckian_locus), które wyraża Temperatura koloru (w kelvin — stopniach), mają wyższe numery po stronie niebieski (hotter) i małe cyfry po stronie czerwony (chłodnicy). Są one przydatne w sytuacjach, typowe oświetlenia.

W warunkach oświetlenia mieszanych dostosowań białe saldo będzie konieczne różni się od Planckian Locus, aby wprowadzić wymagane zmiany. W takich sytuacjach potrzebne dostosowania się przesunięte albo zielonego lub stronie czerwony/amarantowy CIE skalowania.

Urządzenia z systemem iOS kompensacji rzutowania kolor przez zwiększania przeciwną korzyści kolorów. Na przykład jeśli sceny ma za dużo blue, następnie wzmocnienie czerwonego będzie boosted kompensacji. Korzyści, te wartości są kalibrować dla konkretnych urządzeń, dzięki czemu są one zależne od urządzenia.

### <a name="existing-white-balance-controls"></a>Dysponujące saldo białe

System iOS 7 i nowszych dostępne następujące dysponujące saldo biały za pośrednictwem `WhiteBalanceMode` właściwości:

-   `AVCapture WhiteBalance ModeLocked` — Przykłady sceny raz i używając tych wartości w całym sceny.
-   `AVCapture WhiteBalance ModeContinuousAutoExposure` — Przykłady sceny stale, aby upewnić się również zrównoważone.


I aplikacji można monitorować `AdjustingWhiteBalance` właściwości, aby zobaczyć, gdy jest dopasowywany zagrożeń.

### <a name="new-white-balance-controls-in-ios-8"></a>Nowe formanty saldo biały w systemie iOS 8

Oprócz funkcji już dostarczona przez system iOS 7 i nowszym następujące funkcje są teraz dostępne do kontroli saldo biały w systemie iOS 8:

-  Uzyskuje pełni ręcznej kontroli nad urządzeniem RGB.
-  GET, Set i obserwować kluczy i wartości RGB urządzenie uzyska.
-  Obsługa saldo biały przy użyciu karty szary.
-  Procedury konwersji do i z przestrzenie niezależnie od urządzenia.


Do implementowania funkcji powyżej `AVCaptureWhiteBalanceGain` struktury została dodana do następujących członków:

-   `RedGain` 
-   `GreenGain` 
-   `BlueGain` 


Korzyści maksymalną białe saldo jest obecnie cztery (4) i może być gotowy z `MaxWhiteBalanceGain` właściwości. Dlatego dozwolonego zakresu od jednego (1) do `MaxWhiteBalanceGain` (4) obecnie.

`DeviceWhiteBalanceGains` Właściwości może służyć do przestrzegania bieżącej wartości. Użyj `SetWhiteBalanceModeLockedWithDeviceWhiteBalanceGains` aby dopasować saldo uzyska gdy aparat jest w trybie zablokowanym saldo białe.

#### <a name="conversion-routines"></a>Procedury konwersji

Procedury konwersji zostały dodane do systemu iOS 8 ułatwiające konwertowanie do i z przestrzenie niezależnie od urządzenia. Do wykonania procedury konwersji `AVCaptureWhiteBalanceChromaticityValues` struktury została dodana do następujących członków:

-   `X` -jest wartością z przedziału od 0 do 1.
-   `Y` -jest wartością z przedziału od 0 do 1.


`AVCaptureWhiteBalanceTemperatureAndTintValues` Struktury dodano także z następujących członków:

-   `Temperature` -jest liczbą zmiennoprzecinkową wartość punktów w stopniach kelvin —.
-   `Tint` -jest przesunięcie z zielonym lub amarantowym z zakresu od 0 do 150 z wartości dodatnie kierunku zielony i ujemną wartość kierunku w karmazynowego.


Użyj `CaptureDevice.GetTemperatureAndTintValues`i `CaptureDevice.GetDeviceWhiteBalanceGains`metody konwersję między temperatury i odcień, trójchromatyczna RGB uzyskania przestrzenie.

> [!NOTE]
> Procedury konwersji są bardziej dokładne bliżej ma wartość do skonwertowania Planckian Locus.




#### <a name="gray-card-support"></a>Obsługa kart szarości

Apple używany jest termin World szary do odwoływania się do obsługi kart szary, wbudowane w system iOS 8. Pozwala użytkownikowi skupić się na fizycznej karty szary, który obejmuje co najmniej 50% Centrum ramki i użyty aby dopasować saldo biały. Karta szary służy do osiągnięcia biały wyświetlonym neutralne.

To może być zaimplementowany w aplikacji przez użytkownika o fizycznej karty szarego przed aparatu, umieść monitorowania `GrayWorldDeviceWhiteBalanceGains` właściwości i dopiero wartości rozliczenia w dół.

Aplikacja może następnie zablokować zyski saldo biały dla `SetWhiteBalanceModeLockedWithDeviceWhiteBalanceGains` metodę przy użyciu wartości z `GrayWorldDeviceWhiteBalanceGains` właściwości w celu zastosowania zmian.

Urządzenie przechwytywania musi być zablokowany dla konfiguracji przed zmiany w równowadze biały.

### <a name="manual-white-balance-example"></a>Przykład ręczne saldo białe

Kod ogólne AV ustawienia przechwytywania w miejscu `UIViewController` mogą być dodawane do scenorysu aplikacji lub skonfigurowane w następujący sposób:

[![](intro-to-manual-camera-controls-images/image18.png "UIViewController można dodać do aplikacji scenorysu i skonfigurowane w sposób pokazany poniżej")](intro-to-manual-camera-controls-images/image18.png#lightbox)

Widok zawiera następujące główne elementy:

-  A `UIImageView` który wyświetli źródła wideo.
-  A `UISegmentedControl` spowoduje to zmianę trybu fokusu z automatycznego na zablokowana.
-  Dwa `UISlider` formantów, które będą Pokaż i temperatury i odcień aktualizacji.
-  A `UIButton` używane do pobierania próbek miejsca karty szary (szary World) i ustawić saldo biały przy użyciu tych wartości.


Wykonaj następujące czynności przesyłania zapasowe kontroler widoku kontrolę salda biały ręczny:


1. Dodaj następujące instrukcje using:

    ```csharp
    using System;
    using Foundation;
    using UIKit;
    using System.CodeDom.Compiler;
    using System.Collections.Generic;
    using System.Linq;
    using AVFoundation;
    using CoreVideo;
    using CoreMedia;
    using CoreGraphics;
    using CoreFoundation;
    using System.Timers;
    ```  
  
1. Dodaj następujących zmiennych prywatnych:

    ```csharp
    #region Private Variables
    private NSError Error; 
    private bool Automatic = true;
    #endregion
    ```
  
1. Dodaj następujące właściwości obliczanej:

    ```csharp
    #region Computed Properties
    public AppDelegate ThisApp {
        get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
    }
    public Timer SampleTimer { get; set; }
    #endregion
    ```  
  
1. Dodaj następującą metodę prywatne, aby ustawić nowe biały temperatury i odcień:

    ```csharp
    #region Private Methods
    void SetTemperatureAndTint() {
        // Grab current temp and tint
        var TempAndTint = new AVCaptureWhiteBalanceTemperatureAndTintValues (Temperature.Value, Tint.Value);

        // Convert Color space
        var gains = ThisApp.CaptureDevice.GetDeviceWhiteBalanceGains (TempAndTint);

        // Set the new values
        if (ThisApp.CaptureDevice.LockForConfiguration (out Error)) {
            gains = NomralizeGains (gains);
            ThisApp.CaptureDevice.SetWhiteBalanceModeLockedWithDeviceWhiteBalanceGains (gains, null);
            ThisApp.CaptureDevice.UnlockForConfiguration ();
        }
    }
    
    AVCaptureWhiteBalanceGains NomralizeGains (AVCaptureWhiteBalanceGains gains)
    {
        gains.RedGain = Math.Max (1, gains.RedGain);
        gains.BlueGain = Math.Max (1, gains.BlueGain);
        gains.GreenGain = Math.Max (1, gains.GreenGain);

        float maxGain = ThisApp.CaptureDevice.MaxWhiteBalanceGain;
        gains.RedGain = Math.Min (maxGain, gains.RedGain);
        gains.BlueGain = Math.Min (maxGain, gains.BlueGain);
        gains.GreenGain = Math.Min (maxGain, gains.GreenGain);

        return gains;
    }
    #endregion
    ```   
  
1. Zastąpienie `ViewDidLoad` — metoda i Dodaj następujący kod:

    ```csharp
    public override void ViewDidLoad ()
    {
        base.ViewDidLoad ();

        // Hide no camera label
        NoCamera.Hidden = ThisApp.CameraAvailable;

        // Attach to camera view
        ThisApp.Recorder.DisplayView = CameraView;

        // Set min and max values
        Temperature.MinValue = 1000f;
        Temperature.MaxValue = 10000f;

        Tint.MinValue = -150f;
        Tint.MaxValue = 150f;

        // Create a timer to monitor and update the UI
        SampleTimer = new Timer (5000);
        SampleTimer.Elapsed += (sender, e) => {
            // Convert color space
            var TempAndTint = ThisApp.CaptureDevice.GetTemperatureAndTintValues (ThisApp.CaptureDevice.DeviceWhiteBalanceGains);

            // Update slider positions
            Temperature.BeginInvokeOnMainThread (() => {
                Temperature.Value = TempAndTint.Temperature;
            });

            Tint.BeginInvokeOnMainThread (() => {
                Tint.Value = TempAndTint.Tint;
            });
        };

        // Watch for value changes
        Segments.ValueChanged += (sender, e) => {
            // Lock device for change
            if (ThisApp.CaptureDevice.LockForConfiguration (out Error)) {

                // Take action based on the segment selected
                switch (Segments.SelectedSegment) {
                case 0:
                // Activate auto focus and start monitoring position
                    Temperature.Enabled = false;
                    Tint.Enabled = false;
                    ThisApp.CaptureDevice.WhiteBalanceMode = AVCaptureWhiteBalanceMode.ContinuousAutoWhiteBalance;
                    SampleTimer.Start ();
                    Automatic = true;
                    break;
                case 1:
                // Stop auto focus and allow the user to control the camera
                    SampleTimer.Stop ();
                    ThisApp.CaptureDevice.WhiteBalanceMode = AVCaptureWhiteBalanceMode.Locked;
                    Automatic = false;
                    Temperature.Enabled = true;
                    Tint.Enabled = true;
                    break;
                }

                // Unlock device
                ThisApp.CaptureDevice.UnlockForConfiguration ();
            }
        };

        // Monitor position changes
        Temperature.TouchUpInside += (sender, e) => {

            // If we are in the automatic mode, ignore changes
            if (Automatic)
                return;

            // Update white balance
            SetTemperatureAndTint ();
        };

        Tint.TouchUpInside += (sender, e) => {

            // If we are in the automatic mode, ignore changes
            if (Automatic)
                return;

            // Update white balance
            SetTemperatureAndTint ();
        };

        GrayCardButton.TouchUpInside += (sender, e) => {

            // If we are in the automatic mode, ignore changes
            if (Automatic)
                return;

            // Get gray card values
            var gains = ThisApp.CaptureDevice.GrayWorldDeviceWhiteBalanceGains;

            // Set the new values
            if (ThisApp.CaptureDevice.LockForConfiguration (out Error)) {
                ThisApp.CaptureDevice.SetWhiteBalanceModeLockedWithDeviceWhiteBalanceGains (gains, null);
                ThisApp.CaptureDevice.UnlockForConfiguration ();
            }
        };
    }
    ``` 
  
1. Zastąpienie `ViewDidAppear` — metoda i dodaj następujące polecenie, aby rozpocząć rejestrowanie, załadowanie widoku:

    ```csharp
    public override void ViewDidAppear (bool animated)
    {
        base.ViewDidAppear (animated);
    
        // Start udating the display
        if (ThisApp.CameraAvailable) {
            // Remap to this camera view
            ThisApp.Recorder.DisplayView = CameraView;
    
            ThisApp.Session.StartRunning ();
            SampleTimer.Start ();
        }
    }
    ```  
  
1. Zapisz zmiany kodu i uruchomić aplikację.
1. Aparatem w trybie automatycznym suwaki przeniesie automatycznie zgodnie z aparatu dopasowuje saldo białe:

    [![](intro-to-manual-camera-controls-images/image19.png "Suwaki przeniesie automatycznie zgodnie z aparatu dopasowuje saldo białe")](intro-to-manual-camera-controls-images/image19.png#lightbox)
1. Wybierz segment zablokowane i przeciągnij suwaki Temp i odcień, aby dopasować białe saldo ręcznie:

    [![](intro-to-manual-camera-controls-images/image20.png "Przeciągnij suwaki Temp i odcień, aby dopasować białe saldo ręcznie")](intro-to-manual-camera-controls-images/image20.png#lightbox)
1. Z segmentem zablokowany nadal zaznaczone umieść szarego kart fizycznych na wierzchu z aparatu fotograficznego, a następnie wybierz przycisk karty szary, aby dostosować białe Saldo na świecie szary:

    [![](intro-to-manual-camera-controls-images/image21.png "Naciśnij przycisk karty szary aby dopasować białe saldo World szarości")](intro-to-manual-camera-controls-images/image21.png#lightbox)
1. Zatrzymaj aplikację.

Powyższy kod pokazuje, jak monitorować ustawienia saldo białe, gdy aparat jest w trybie automatycznym lub kontrolować białe saldo jest w trybie zablokowane za pomocą suwaków.

## <a name="bracketed-capture"></a>W nawiasach kwadratowych przechwytywania

Oddzielona przechwytywania na podstawie ustawień od formantów aparatu ręcznego przedstawionych powyżej i umożliwia aplikacji do przechwytywania chwilę w czasie na różne sposoby.

Krótko mówiąc, oddzielona przechwytywania jest serii nadal obrazów wykonanych za pomocą różnych ustawień z obrazu do obrazu.

[![](intro-to-manual-camera-controls-images/image22.png "Jak działa oddzielona przechwytywania")](intro-to-manual-camera-controls-images/image22.png#lightbox)

Przy użyciu oddzielona przechwytywania w systemie iOS 8, aplikacji można ustawienia wstępnego serii ręczne formantów aparatu, wydać polecenie pojedynczego i mieć bieżącej sceny zwracać serie obrazów dla każdego ustawienia ręcznej.

### <a name="bracketed-capture-basics"></a>Podstawowe informacje dotyczące przechwytywania w nawiasach kwadratowych

Ponownie oddzielona przechwytywania jest serii nadal obrazów wykonanych za pomocą różnych ustawień z obrazu do obrazu. Oddzielona przechwytywania dostępne typy to:

-  **Automatycznie nawiasu narażenia** — gdy wszystkie obrazy mają wartość odchylenia zróżnicowane.
-  **Ręczne nawiasu narażenia** — gdy wszystkie obrazy mają zróżnicowane migawki (czas trwania) i ISO wielkość.
-  **Proste nawiasu serii** — szereg nadal obrazów w krótkim przedziale czasu.


### <a name="new-bracketed-capture-controls-in-ios-8"></a>Nowe oddzielona przechwytywania formanty w systemie iOS 8

Wszystkie polecenia oddzielona przechwytywania zostały wdrożone w `AVCaptureStillImageOutput` klasy. Użyj `CaptureStillImageBracket`metodę, aby pobrać serie obrazów z podanej tablicy ustawień.

Dwa nowe klasy zostały wdrożone do obsługi ustawień:

-   `AVCaptureAutoExposureBracketedStillImageSettings` — Ma jedną właściwość `ExposureTargetBias`używane można ustawić odchylenia do automatycznego narażenia nawiasu. 
-   `AVCaptureManual`  `ExposureBracketedStillImageSettings` — Ma dwie właściwości `ExposureDuration` i `ISO`, umożliwia ustawianie migawki i ISO dla nawiasu ręczne zagrożeń. 


### <a name="bracketed-capture-controls-dos-and-donts"></a>Formanty w nawiasach kwadratowych przechwytywania porady

#### <a name="dos"></a>Zadania

Oto lista zagadnień, które powinny zostać wykonane w trakcie używania funkcji przechwytywania oddzielona formanty w systemie iOS 8:

-  Przygotowywanie aplikacji do sytuacji najgorszych przechwytywania, wywołując `PrepareToCaptureStillImageBracket` metody.
-  Załóżmy, będzie buforów przykładowe pochodzą z tej samej puli udostępnionego.
-  Aby zwolnić pamięci, która została przydzielona przez poprzednie wywołanie prepare, należy wywołać `PrepareToCaptureStillImageBracket` ponownie i wysłać go, tablicy jednego obiektu.


#### <a name="donts"></a>— Ostrzeżenia

Oto lista czynności, które nie należy wykonywać podczas używania funkcji przechwytywania oddzielona formanty w systemie iOS 8:

-  Nie można mieszać oddzielona przechwytywania typy ustawień w jednym przechwytywania.
-  Żądanie nie więcej niż `MaxBracketedCaptureStillImageCount` obrazów w jednym przechwytywania.


### <a name="bracketed-capture-details"></a>Szczegóły przechwytywania w nawiasach kwadratowych

Następujące szczegóły powinny brane pod uwagę podczas pracy z oddzielona przechwytywania w systemie iOS 8:

-  Ustawienia w nawiasach kwadratowych tymczasowo zastąpić `AVCaptureDevice` ustawienia.
-  Flash i nadal ustawienia stabilizacji obrazu są ignorowane.
-  Wszystkie obrazy musi używać tego samego formatu danych wyjściowych (jpeg, png, itp.)
-  Podgląd wideo może porzucić ramki.
-  Przechwytywanie w nawiasach kwadratowych jest obsługiwana na wszystkich urządzeniach, zgodne z systemem iOS 8.


Dzięki tym informacjom pamiętać Spójrzmy na przykład przy użyciu oddzielona przechwytywania w systemie iOS 8.

### <a name="bracket-capture-example"></a>Przykład przechwytywania nawiasu

Kod ogólne AV ustawienia przechwytywania w miejscu `UIViewController` mogą być dodawane do scenorysu aplikacji lub skonfigurowane w następujący sposób:

[![](intro-to-manual-camera-controls-images/image23.png "UIViewController można dodać do aplikacji scenorysu i skonfigurowane w sposób pokazany poniżej")](intro-to-manual-camera-controls-images/image23.png#lightbox)

Widok zawiera następujące główne elementy:

-  A `UIImageView` który wyświetli źródła wideo.
-  Trzy `UIImageViews` który wyświetli wyniki przechwytywania.
-  A `UIScrollView` do przechowywania wyników widoków i źródła wideo.
-  A `UIButton` użyć w celu podjęcia oddzielona przechwytywania z niektórych ustawień wstępnych.


Wykonaj następujące czynności przesyłania zapasowe kontroler widoku oddzielona Capture:


1. Dodaj następujące instrukcje using:

    ```csharp
    using System;
    using System.Drawing;
    using Foundation;
    using UIKit;
    using System.CodeDom.Compiler;
    using System.Collections.Generic;
    using System.Linq;
    using AVFoundation;
    using CoreVideo;
    using CoreMedia;
    using CoreGraphics;
    using CoreFoundation;
    using CoreImage;
    ```  
  
1. Dodaj następujących zmiennych prywatnych:

    ```csharp
    #region Private Variables
    private NSError Error;
    private List<UIImageView> Output = new List<UIImageView>();
    private nint OutputIndex = 0;
    #endregion
    ```    
  
1. Dodaj następujące właściwości obliczanej:

    ```csharp
    #region Computed Properties
    public AppDelegate ThisApp {
        get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
    }
    #endregion
    ```  
  
1. Dodaj następującą metodę prywatnych do tworzenia widoków obrazu wyjściowego:

    ```csharp
    #region Private Methods
    private UIImageView BuildOutputView(nint n) {
    
        // Create a new image view controller
        var imageView = new UIImageView (new CGRect (CameraView.Frame.Width * n, 0, CameraView.Frame.Width, CameraView.Frame.Height));
    
        // Load a temp image
        imageView.Image = UIImage.FromFile ("Default-568h@2x.png");
    
        // Add a label
        UILabel label = new UILabel (new CGRect (0, 20, CameraView.Frame.Width, 24));
        label.TextColor = UIColor.White;
        label.Text = string.Format ("Bracketed Image {0}", n);
        imageView.AddSubview (label);
    
        // Add to scrolling view
        ScrollView.AddSubview (imageView);
    
        // Return new image view
        return imageView;
    }
    #endregion
    ```  
  
1. Zastąpienie `ViewDidLoad` — metoda i Dodaj następujący kod:
    
    ```
    public override void ViewDidLoad ()
    {
        base.ViewDidLoad ();
    
        // Hide no camera label
        NoCamera.Hidden = ThisApp.CameraAvailable;
    
        // Attach to camera view
        ThisApp.Recorder.DisplayView = CameraView;
    
        // Setup scrolling area
        ScrollView.ContentSize = new SizeF (CameraView.Frame.Width * 4, CameraView.Frame.Height);
    
        // Add output views
        Output.Add (BuildOutputView (1));
        Output.Add (BuildOutputView (2));
        Output.Add (BuildOutputView (3));
    
        // Create preset settings
        var Settings = new AVCaptureBracketedStillImageSettings[] {
            AVCaptureAutoExposureBracketedStillImageSettings.Create(-2.0f),
            AVCaptureAutoExposureBracketedStillImageSettings.Create(0.0f),
            AVCaptureAutoExposureBracketedStillImageSettings.Create(2.0f)
        };
    
        // Wireup capture button
        CaptureButton.TouchUpInside += (sender, e) => {
            // Reset output index
            OutputIndex = 0;
    
            // Tell the camera that we are getting ready to do a bracketed capture
            ThisApp.StillImageOutput.PrepareToCaptureStillImageBracket(ThisApp.StillImageOutput.Connections[0],Settings,async (bool ready, NSError err) => {
                // Was there an error, if so report it
                if (err!=null) {
                    Console.WriteLine("Error: {0}",err.LocalizedDescription);
                }
            });
    
            // Ask the camera to snap a bracketed capture
            ThisApp.StillImageOutput.CaptureStillImageBracket(ThisApp.StillImageOutput.Connections[0],Settings, (sampleBuffer, settings, err) =>{
                // Convert raw image stream into a Core Image Image
                var imageData = AVCaptureStillImageOutput.JpegStillToNSData(sampleBuffer);
                var image = CIImage.FromData(imageData);
    
                // Display the resulting image
                Output[OutputIndex++].Image = UIImage.FromImage(image);
    
                // IMPORTANT: You must release the buffer because AVFoundation has a fixed number
                // of buffers and will stop delivering frames if it runs out.
                sampleBuffer.Dispose();
            });
        };
    }
    ```
    
  
1. Zastąpienie `ViewDidAppear` — metoda i Dodaj następujący kod:

    ```csharp
    public override void ViewDidAppear (bool animated)
    {
        base.ViewDidAppear (animated);
    
        // Start udating the display
        if (ThisApp.CameraAvailable) {
            // Remap to this camera view
            ThisApp.Recorder.DisplayView = CameraView;
    
            ThisApp.Session.StartRunning ();
        }
    }
    
    ```  
    
1. Zapisz zmiany kodu i uruchomić aplikację.
1. Ramka sceny i naciśnij przycisk nawiasu przechwytywania:

    [![](intro-to-manual-camera-controls-images/image24.png "Ramka sceny i naciśnij przycisk nawiasu przechwytywania")](intro-to-manual-camera-controls-images/image24.png#lightbox)
1. Przejdź od prawej do lewej, aby wyświetlić trzy obrazy wykonywaną przez oddzielona przechwytywania:

    [![](intro-to-manual-camera-controls-images/image25.png "Przejdź od prawej do lewej, aby wyświetlić trzy obrazy wykonywaną przez oddzielona przechwytywania")](intro-to-manual-camera-controls-images/image25.png#lightbox)
1. Zatrzymaj aplikację.


Powyższy kod pokazuje, jak skonfigurować i wykonać automatycznego narażenia oddzielona przechwytywania w systemie iOS 8.

## <a name="summary"></a>Podsumowanie

W tym artykule nasz zostały objęte wprowadzenie do nowych formantów aparatu ręcznego dostarczane przez system iOS 8 i omówione podstawy co robią, i jak działają. Serwis przykłady fokus ręczny, ręczne zagrożeń i ręczne biały równowagi. Na koniec firma Microsoft udostępniła przykład biorąc oddzielona przechwytywania za pomocą formantów już wspomniano aparatu ręczne

## <a name="related-links"></a>Linki pokrewne

- [ManualCameraControls (przykład)](https://developer.xamarin.com/samples/monotouch/ManualCameraControls)
- [Wprowadzenie do systemu iOS 8](~/ios/platform/introduction-to-ios8.md)
