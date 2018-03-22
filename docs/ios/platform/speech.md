---
title: Rozpoznawanie mowy
description: "Ten artykuł przedstawia informacje o nowy interfejs API mowy i pokazuje, jak ją wdrożyć w aplikacji platformy Xamarin.iOS do obsługi rozpoznawania mowy i wykonać transkrypcji mowy (strumienie na żywo lub nagrania audio) na tekst."
ms.topic: article
ms.prod: xamarin
ms.assetid: 64FED50A-6A28-4833-BEAE-63CEC9A09010
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: e868c0ee71688e208c5217d9f5a89ea3acec988c
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/22/2018
---
# <a name="speech-recognition"></a>Rozpoznawanie mowy

_Ten artykuł przedstawia informacje o nowy interfejs API mowy i pokazuje, jak ją wdrożyć w aplikacji platformy Xamarin.iOS do obsługi rozpoznawania mowy i wykonać transkrypcji mowy (strumienie na żywo lub nagrania audio) na tekst._

Nowość w systemach iOS 10, Apple ma wersji interfejsu API rozpoznawania mowy, który umożliwia aplikacji systemu iOS do obsługi rozpoznawania mowy i wykonać transkrypcji mowy (strumienie na żywo lub nagrania audio) na tekst.

Zgodnie z firmy Apple w interfejsie API rozpoznawania mowy ma następujące funkcje i korzyści:

- Bardzo dokładne
- Najnowocześniejsze
- Łatwy w użyciu
- Szybkie
- Obsługuje wiele języków
- Zasady zachowania poufności względem użytkownika

## <a name="how-speech-recognition-works"></a>Jak działa rozpoznawanie mowy

Rozpoznawanie mowy jest zaimplementowana w aplikacji systemu iOS pobierania na żywo lub uprzednio nagranego dźwięku (w jednym z języków obsługiwanych przez interfejs API) i przekazaniem ich do rozpoznawania mowy, która zwraca przekształcania zwykłego tekstu, rozmowy słów.

[![](speech-images/speech01.png "Jak działa rozpoznawanie mowy")](speech-images/speech01.png#lightbox)

### <a name="keyboard-dictation"></a>Dyktowania klawiatury

Podczas rozpoznawania mowy traktować większość użytkowników na urządzeniu z systemem iOS, sądzą o wbudowanych Asystenta głosowego Siri, wydanej wraz z klawiatury dyktowania w systemie iOS 5 z iPhone 4S.

Klawiatura dyktowania jest obsługiwana przez dowolny element interfejsu, który obsługuje TextKit (takich jak `UITextField` lub `UITextArea`) i jest uaktywniany przez użytkownika przycisku dyktowania (bezpośrednio na lewo od spacji) w klawiatury wirtualnych z systemem iOS.

Apple wydała następujące statystyki dyktowania klawiatury (zbierane od 2011):

- Dyktowania klawiatury ma powszechnie używane, ponieważ został wydany w systemie iOS 5.
- Około 65 000 aplikacje z niego korzystać na dzień.
- O innej wszystkich iOS dyktowania odbywa się w 3 aplikacji firmy.

Klawiatura dyktowania jest bardzo łatwe w użyciu, ponieważ nie wymaga żadnych nakładu pracy w części dewelopera, innych niż z użyciem elementu interfejsu TextKit w projekcie interfejsu użytkownika aplikacji. Dyktowania Klawiatura ma również możliwość nie wymaga żadnych żądań specjalnych uprawnień z aplikacji, zanim będzie można użyć.

Aplikacje korzystające z nowych interfejsów API rozpoznawania mowy wymaga specjalnych uprawnień do przyznane przez użytkownika, ponieważ wymaga rozpoznawania mowy transmisji i tymczasowego przechowywania danych na serwerach firmy Apple. Zobacz nasze [zabezpieczeń i prywatności rozszerzenia](~/ios/app-fundamentals/security-privacy.md) dokumentacji, aby uzyskać szczegółowe informacje.

Podczas dyktowania klawiatury jest łatwa do wdrożenia, pochodzą z kilku ograniczeń i wady:

- Wymaga to użycia pola tekstowego danych wejściowych i wyświetlanie klawiatury.
- W przypadku na żywo sygnału wejściowego tylko audio i aplikacja nie ma kontroli nad procesem rejestrowania dźwięku.
- Zapewnia nie kontroluje język, który jest używany do interpretowania mowy użytkownika.
- Nie istnieje sposób dla aplikacji sprawdzić, czy przycisk dyktowania jest dostępny dla użytkownika.
- Aplikacji nie można dostosować proces nagrania audio.
- Zapewnia bardzo skrócona zestaw wyników, który nie zawiera informacje, takie jak harmonogram i zaufania.

### <a name="speech-recognition-api"></a>Rozpoznawanie mowy interfejsu API

Nowość w systemach iOS 10, Apple wydała interfejs API rozpoznawania mowy, który zapewnia bardziej zaawansowanych aplikacji systemu iOS do zaimplementowania rozpoznawania mowy. Ten interfejs API jest taka sama jak używającej Apple zasilania zarówno Siri, jak i dyktowania klawiatury i jest w stanie zapewnia dokładność najnowocześniejsze szybkiego zapisu.

Wyniki dostarczone przez interfejs API rozpoznawania mowy niewidocznie są dostosowane do poszczególnych użytkowników, bez konieczności zbierania lub dostęp do danych prywatnych użytkownika aplikacji.

Interfejs API rozpoznawania mowy udostępnia wyniki z powrotem do wywołującego aplikacji w pobliżu w czasie rzeczywistym, użytkownik jest czytanie i zapewnia więcej informacji na temat wyników tłumaczenia niż tylko tekst. Należą do nich następujące elementy:

- Wiele interpretacji powiedział użytkownika.
- Poziomy zaufania dla poszczególnych tłumaczenia.
- Informacje o chronometrażu.

Jak już wspomniano, należy podać audio do tłumaczenia, albo przez źródło danych na żywo, lub z uprzednio nagranego źródła i w każdym z języków ponad 50 i dialekty obsługiwane przez system iOS 10.

Interfejs API rozpoznawania mowy mogą być używane na dowolnym urządzeniu z systemem iOS z systemem iOS 10 i w większości przypadków wymaga połączenia internetowego na żywo, ponieważ zbiorczego tłumaczeń odbywa się na serwerach firmy Apple. Inaczej mówiąc, niektóre nowszej systemu iOS, które urządzenia obsługują zawsze na, tłumaczenia na urządzeniach określonych języków.

Apple dołączył interfejs API dostępności, aby określić, czy danego języka jest dostępny do tłumaczenia w danym momencie. Aplikacji należy używać tego interfejsu API, zamiast testować połączenie z Internetem się bezpośrednio.

Jak wspomniano powyżej w sekcji dyktowania klawiatury, rozpoznawanie mowy wymaga transmisji i tymczasowego przechowywania danych na serwerach firmy Apple przez internet i jako taki aplikacji _musi_ zażądać uprawnień do wykonania rozpoznawania, umieszczając w niej `NSSpeechRecognitionUsageDescription` klucza w jego `Info.plist` plików i wywoływania `SFSpeechRecognizer.RequestAuthorization` metody. 

Zależnie od źródła audio, używany do rozpoznawania mowy, innych zmian w aplikacji `Info.plist` plik może być wymagane. Zobacz nasze [zabezpieczeń i prywatności rozszerzenia](~/ios/app-fundamentals/security-privacy.md) dokumentacji, aby uzyskać szczegółowe informacje.

## <a name="adopting-speech-recognition-in-an-app"></a>Przyjmowanie rozpoznawanie mowy w aplikacji

Istnieją cztery główne kroki, które deweloper musi przyjąć rozpoznawanie mowy w aplikacji systemu iOS:

- Podaj opis użycia w aplikacji `Info.plist` plik za pomocą `NSSpeechRecognitionUsageDescription` klucza. Na przykład aplikacja kamery może zawierać następujący opis _"Umożliwia podjęcie zdjęcie właśnie mówiąc słowo"ser"."_
- Żądania autoryzacji przez wywołanie metody `SFSpeechRecognizer.RequestAuthorization` metody do prezentowania wyjaśnienie (w `NSSpeechRecognitionUsageDescription` klucza powyżej) Dlaczego aplikacja potrzebuje mowy uznania dostępu dla użytkownika w oknie dialogowym i zezwolić im na akceptowanie lub odrzucanie.
- Utwórz żądanie rozpoznawania mowy:
    * Uprzednio nagranego dźwięku na dysku, można użyć `SFSpeechURLRecognitionRequest` klasy.
    * Audio na żywo (lub audio z pamięci), należy użyć `SFSPeechAudioBufferRecognitionRequest` klasy.
- Przekaż żądania rozpoznawania mowy do rozpoznawania mowy (`SFSpeechRecognizer`) do rozpoczęcia rozpoznawania. Aplikacja Opcjonalnie można przechowywać na zwróconego `SFSpeechRecognitionTask` na monitorowanie i śledzenie wyników rozpoznawania.

Te kroki zostanie omówiona szczegółowo poniżej.

### <a name="providing-a-usage-description"></a>Podanie opisu użycia

Aby zapewnić wymagane `NSSpeechRecognitionUsageDescription` klucza w `Info.plist` plików, wykonaj następujące czynności:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Kliknij dwukrotnie `Info.plist` plik, aby otworzyć do edycji.
2. Przełącz się do **źródła** widoku: 

    [![](speech-images/speech02.png "Wyświetl źródło")](speech-images/speech02.png#lightbox)
3. Polecenie **Dodaj nowy wpis**, wprowadź `NSSpeechRecognitionUsageDescription` dla **właściwości**, `String` dla **typu** i **opis użycia** jako **wartość**. Na przykład: 

    [![](speech-images/speech03.png "Dodawanie NSSpeechRecognitionUsageDescription")](speech-images/speech03.png#lightbox)
4. Jeśli aplikacja będzie obsługiwał na żywo przekształcania audio, trzeba będzie również opis użycia mikrofon. Polecenie **Dodaj nowy wpis**, wprowadź `NSMicrophoneUsageDescription` dla **właściwości**, `String` dla **typu** i **opis użycia** jako **wartość**. Na przykład: 

    [![](speech-images/speech04.png "Dodawanie NSMicrophoneUsageDescription")](speech-images/speech04.png#lightbox)
4. Zapisz zmiany w pliku.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Kliknij dwukrotnie `Info.plist` plik, aby otworzyć do edycji.
3. Polecenie **Dodaj nowy wpis**, wprowadź `NSSpeechRecognitionUsageDescription` dla **właściwości**, `String` dla **typu** i **opis użycia** jako **wartość**. Na przykład: 

    [![](speech-images/speech03w.png "Dodawanie NSSpeechRecognitionUsageDescription")](speech-images/speech03w.png#lightbox)
4. Jeśli aplikacja będzie obsługiwał na żywo przekształcania audio, trzeba będzie również opis użycia mikrofon. Polecenie **Dodaj nowy wpis**, wprowadź `NSMicrophoneUsageDescription` dla **właściwości**, `String` dla **typu** i **opis użycia** jako **wartość**. Na przykład: 

    [![](speech-images/speech04w.png "Dodawanie NSMicrophoneUsageDescription")](speech-images/speech04w.png#lightbox)
4. Zapisz zmiany w pliku.

-----

> [!IMPORTANT]
> Nie można podać powyższe `Info.plist` kluczy (`NSSpeechRecognitionUsageDescription` lub `NSMicrophoneUsageDescription`) może skutkować niepowodzeniem bez ostrzeżenia, jeśli próby uzyskania dostępu do rozpoznawania mowy lub mikrofon audio na żywo do aplikacji.




### <a name="requesting-authorization"></a>Żąda autoryzacji

Aby poprosić o autoryzacji wymagane użytkownika, który umożliwia aplikacji dostęp do rozpoznawania mowy, Edytuj klasy głównym kontrolera widoku i Dodaj następujący kod:

```csharp
using System;
using UIKit;
using Speech;

namespace MonkeyTalk
{
    public partial class ViewController : UIViewController
    {
        protected ViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }

        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Request user authorization
            SFSpeechRecognizer.RequestAuthorization ((SFSpeechRecognizerAuthorizationStatus status) => {
                // Take action based on status
                switch (status) {
                case SFSpeechRecognizerAuthorizationStatus.Authorized:
                    // User has approved speech recognition
                    ...
                    break;
                case SFSpeechRecognizerAuthorizationStatus.Denied:
                    // User has declined speech recognition
                    ...
                    break;
                case SFSpeechRecognizerAuthorizationStatus.NotDetermined:
                    // Waiting on approval
                    ...
                    break;
                case SFSpeechRecognizerAuthorizationStatus.Restricted:
                    // The device is not permitted
                    ...
                    break;
                }
            });
        }
    }
}
```

`RequestAuthorization` Metody `SFSpeechRecognizer` klasy zażąda uprawnienia użytkownika do rozpoznawania mowy dostępu przy użyciu z powodu zapewnianej przez dewelopera w `NSSpeechRecognitionUsageDescription` klucza z `Info.plist` pliku.

A `SFSpeechRecognizerAuthorizationStatus` wynik zostanie zwrócony do `RequestAuthorization` procedura wywołania zwrotnego metody, który może służyć do wykonania akcji na podstawie użytkownika uprawnienia. 

> [!IMPORTANT]
> Apple sugeruje, oczekiwania, aż użytkownik rozpoczął akcję w aplikacji, która wymaga rozpoznawania mowy przed wysłaniem żądania to uprawnienie.

### <a name="recognizing-pre-recorded-speech"></a>Rozpoznawanie mowy z uprzednio nagranego

Jeśli aplikacja chce rozpoznawanie mowy z uprzednio nagranego pliku WAV lub MP3, może użyć poniższego kodu:

```csharp
using System;
using UIKit;
using Speech;
using Foundation;
...

public void RecognizeFile (NSUrl url)
{
    // Access new recognizer
    var recognizer = new SFSpeechRecognizer ();

    // Is the default language supported?
    if (recognizer == null) {
        // No, return to caller
        return;
    }

    // Is recognition available?
    if (!recognizer.Available) {
        // No, return to caller
        return;
    }

    // Create recognition task and start recognition
    var request = new SFSpeechUrlRecognitionRequest (url);
    recognizer.GetRecognitionTask (request, (SFSpeechRecognitionResult result, NSError err) => {
        // Was there an error?
        if (err != null) {
            // Handle error
            ...
        } else {
            // Is this the final translation?
            if (result.Final) {
                Console.WriteLine ("You said, \"{0}\".", result.BestTranscription.FormattedString);
            }
        }
    });
}
```

Spojrzenie na ten kod szczegółowo, najpierw próbuje utworzyć rozpoznawania mowy (`SFSpeechRecognizer`). Jeśli domyślny język nie jest obsługiwany dla rozpoznawania mowy `null` jest zwracany i wyjścia funkcji.

Jeśli aparat rozpoznawania mowy jest dostępna w języku domyślnym, aplikacja sprawdza, czy jest obecnie w użyciu rozpoznawania `Available` właściwości. Na przykład rozpoznawania nie mogą być dostępne, jeśli urządzenie nie ma aktywne połączenie internetowe.

A `SFSpeechUrlRecognitionRequest` jest tworzona na podstawie `NSUrl` lokalizację pliku wstępnie zarejestrowane urządzenia z systemem iOS, a jego jest przekazywany do rozpoznawania mowy do przetworzenia z procedura wywołania zwrotnego.

Gdy wywołanie zwrotne jest wywoływana, jeśli `NSError` nie jest `null` wystąpił błąd, który musi być obsługiwane. Ponieważ rozpoznawanie mowy jest wykonywana stopniowo, procedura wywołania zwrotnego może być wywoływana więcej niż raz, więc `SFSpeechRecognitionResult.Final` właściwości jest testowany czy tłumaczenia została zakończona i jest zapisywany najlepszą wersję tłumaczenia (`BestTranscription`).

### <a name="recognizing-live-speech"></a>Recognizing Live Speech

Jeśli aplikacja chce rozpoznawanie mowy na żywo, proces jest bardzo podobny do rozpoznawania mowy z uprzednio nagranego. Na przykład:

```csharp
using System;
using UIKit;
using Speech;
using Foundation;
using AVFoundation;
...

#region Private Variables
private AVAudioEngine AudioEngine = new AVAudioEngine ();
private SFSpeechRecognizer SpeechRecognizer = new SFSpeechRecognizer ();
private SFSpeechAudioBufferRecognitionRequest LiveSpeechRequest = new SFSpeechAudioBufferRecognitionRequest ();
private SFSpeechRecognitionTask RecognitionTask;
#endregion
...

public void StartRecording ()
{
    // Setup audio session
    var node = AudioEngine.InputNode;
    var recordingFormat = node.GetBusOutputFormat (0);
    node.InstallTapOnBus (0, 1024, recordingFormat, (AVAudioPcmBuffer buffer, AVAudioTime when) => {
        // Append buffer to recognition request
        LiveSpeechRequest.Append (buffer);
    });

    // Start recording
    AudioEngine.Prepare ();
    NSError error;
    AudioEngine.StartAndReturnError (out error);

    // Did recording start?
    if (error != null) {
        // Handle error and return
        ...
        return;
    }

    // Start recognition
    RecognitionTask = SpeechRecognizer.GetRecognitionTask (LiveSpeechRequest, (SFSpeechRecognitionResult result, NSError err) => {
        // Was there an error?
        if (err != null) {
            // Handle error
            ...
        } else {
            // Is this the final translation?
            if (result.Final) {
                Console.WriteLine ("You said \"{0}\".", result.BestTranscription.FormattedString);
            }
        }
    });
}

public void StopRecording ()
{
    AudioEngine.Stop ();
    LiveSpeechRequest.EndAudio ();
}

public void CancelRecording ()
{
    AudioEngine.Stop ();
    RecognitionTask.Cancel ();
}
```

Spojrzenie na ten kod szczegółowo, tworzy kilku zmiennych prywatnych, aby obsługiwać ten proces rozpoznawania:

```csharp
private AVAudioEngine AudioEngine = new AVAudioEngine ();
private SFSpeechRecognizer SpeechRecognizer = new SFSpeechRecognizer ();
private SFSpeechAudioBufferRecognitionRequest LiveSpeechRequest = new SFSpeechAudioBufferRecognitionRequest ();
private SFSpeechRecognitionTask RecognitionTask;
```

Stosuje AV Foundation rejestrowanie dźwięku, który zostanie przekazany do `SFSpeechAudioBufferRecognitionRequest` można obsłużyć żądania rozpoznawania:

```csharp
var node = AudioEngine.InputNode;
var recordingFormat = node.GetBusOutputFormat (0);
node.InstallTapOnBus (0, 1024, recordingFormat, (AVAudioPcmBuffer buffer, AVAudioTime when) => {
    // Append buffer to recognition request
    LiveSpeechRequest.Append (buffer);
});
```

Aplikacja próbuje uruchomić rejestrowanie i wszelkie błędy są obsługiwane, jeśli nie można uruchomić rejestrowania:

```csharp
AudioEngine.Prepare ();
NSError error;
AudioEngine.StartAndReturnError (out error);

// Did recording start?
if (error != null) {
    // Handle error and return
    ...
    return;
}
```

Zadanie rozpoznawania została uruchomiona i dojścia jest ograniczone do zadań rozpoznawania (`SFSpeechRecognitionTask`):

```csharp
RecognitionTask = SpeechRecognizer.GetRecognitionTask (LiveSpeechRequest, (SFSpeechRecognitionResult result, NSError err) => {
    ...
});
```

Wywołanie zwrotne jest używany w sposób podobny do użyty powyżej dla uprzednio nagranego mowy.

Jeśli rejestrowanie jest zatrzymany przez użytkownika, aparat Audio i żądań rozpoznawania mowy jest także informacja:

```csharp
AudioEngine.Stop ();
LiveSpeechRequest.EndAudio ();
```

Jeśli użytkownik anuluje rozpoznawania, aparat Audio i rozpoznawania zadania nie są informacji:

```csharp
AudioEngine.Stop ();
RecognitionTask.Cancel ();
```

Ważne jest, aby wywołać `RecognitionTask.Cancel` Jeśli użytkownik anuluje tłumaczenia, aby zwolnić pamięć i procesora urządzenia.

> [!IMPORTANT]
> Nie dostarczając `NSSpeechRecognitionUsageDescription` lub `NSMicrophoneUsageDescription` `Info.plist` kluczy może skutkować niepowodzeniem bez ostrzeżenia, jeśli próbujesz uzyskać dostęp do rozpoznawania mowy lub mikrofon audio na żywo aplikacji (`var node = AudioEngine.InputNode;`). Zobacz **podanie opisu użycia** sekcji powyżej, aby uzyskać więcej informacji.

## <a name="speech-recognition-limits"></a>Limity rozpoznawania mowy

Podczas pracy z rozpoznawanie mowy w aplikacji systemu iOS, Apple nakłada następujące ograniczenia:

- Rozpoznawanie mowy jest bezpłatna do wszystkich aplikacji, ale jego użycie nie jest nieograniczone:
    - Urządzenia z systemem iOS poszczególnych mają ograniczoną liczbę uznania, które mogą być wykonywane na dzień.
    - Aplikacji spowoduje ograniczenie globalnie na podstawie żądania na dzień.
- Aplikacji muszą być przygotowane do obsługi rozpoznawania mowy połączenia sieciowego i użycia szybkość limit błędów.
- Rozpoznawanie mowy może mieć wysokiego kosztu zarówno zużycie baterii i dużym ruchem w sieci na urządzeniu z systemem iOS użytkownika, w związku z tym, Apple związany z ograniczeniem strict audio czasu trwania mowy max około jednej minuty.

Jeśli aplikacja jest rutynowo naciśnięcie limit przepustowości szybkość, Apple zapyta, czy dewelopera skontaktować się z nimi.

## <a name="privacy-and-usability-considerations"></a>Zagadnienia dotyczące użyteczność i ochrona prywatności

Apple ma następujące propozycję jest przezroczysty i odpowiednich poufności użytkownika, po tym rozpoznawanie mowy w aplikacji systemu iOS:

- Podczas rejestrowania mowy użytkownika, pamiętaj wyraźnie wskazuje, że rejestrowanie odbywa się w interfejsie użytkownika aplikacji. Na przykład aplikacja może Odtwarzaj dźwięk "rejestrowanie" i Wyświetl wskaźnik rejestrowania.
- Rozpoznawanie mowy nie jest używany do informacji poufnych użytkownika, takie jak hasła, dane kondycji lub informacji finansowych.
- Pokaż wyniki rozpoznawania _przed_ działają na nich. To nie tylko udostępnia informacje zwrotne o co aplikacja wykonuje, ale zezwala użytkownikowi na obsługi błędów rozpoznawania pisma ręcznego, jak zostały wprowadzone.

## <a name="summary"></a>Podsumowanie

Ten artykuł zawiera prezentowane nowy interfejs API mowy i pokazano, jak ją wdrożyć w aplikacji platformy Xamarin.iOS do obsługi rozpoznawania mowy i wykonać transkrypcji mowy (strumienie na żywo lub nagrania audio) na tekst. 



## <a name="related-links"></a>Linki pokrewne

- [SpeakToMe (przykład)](https://developer.xamarin.com/samples/monotouch/ios10/SpeakToMe/)
