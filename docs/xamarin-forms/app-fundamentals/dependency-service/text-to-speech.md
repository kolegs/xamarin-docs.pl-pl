---
title: Implementowanie tekst na mowę
description: Użyć DependencyService do wywołania do natywnego API zamiany tekstu na mowę każdej z platform
ms.prod: xamarin
ms.assetid: 1D6164F9-4ECE-43A6-B583-1F5D5EFC1DDF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/18/2017
ms.openlocfilehash: c9cf700ea798ac316e806c40cb90eedc7ded9fa5
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2018
---
# <a name="implementing-text-to-speech"></a>Implementowanie tekst na mowę

Ten artykuł zawiera informacje pomocne podczas tworzenia aplikacji i platform, która używa [ `DependencyService` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/) dostępu do natywnych interfejsów API zamiany tekstu na mowę:

- **[Tworzenie interfejsu](#Creating_the_Interface)**  &ndash; zrozumieć sposób tworzenia interfejsu w kodzie udostępnionego.
- **[iOS implementacji](#iOS_Implementation)**  &ndash; Dowiedz się, jak implementować interfejs w kodzie natywnym dla systemu iOS.
- **[Implementacja systemu android](#Android_Implementation)**  &ndash; Dowiedz się, jak implementować interfejs w kodzie natywnym dla systemu Android.
- **[Implementacja platformy uniwersalnej systemu Windows](#WindowsImplementation)**  &ndash; Dowiedz się, jak implementować interfejs w kodzie natywnym dla uniwersalnych platformy systemu Windows (UWP).
- **[Wdrażanie w kodzie udostępnionego](#Implementing_in_Shared_Code)**  &ndash; Dowiedz się, jak używać `DependencyService` do wywołania do implementacji native z udostępnionego kodu.

Aplikacji przy użyciu `DependencyService` będzie mieć następującą strukturę:

![](text-to-speech-images/tts-diagram.png "Struktura aplikacji DependencyService")

<a name="Creating_the_Interface" />

## <a name="creating-the-interface"></a>Tworzenie interfejsu

Najpierw Utwórz interfejs w kodzie udostępnionego, który określa funkcje, które planujesz wdrożyć. Na przykład interfejs zawiera jedną metodę `Speak`:

```csharp
public interface ITextToSpeech
{
    void Speak (string text);
}
```

Kodowanie tego interfejsu w kodzie udostępnione umożliwi aplikacji platformy Xamarin.Forms na dostęp do interfejsów API rozpoznawania mowy na każdej z platform.

> [!NOTE]
> Klasy implementującej interfejs musi mieć konstruktora bez parametrów, aby pracować z `DependencyService`.

<a name="iOS_Implementation" />

## <a name="ios-implementation"></a>Implementacja systemu iOS

Interfejs musi być implementowana w każdym projekcie specyficzne dla platformy aplikacji. Należy pamiętać, że klasa ma konstruktora bez parametrów, aby `DependencyService` można utworzyć nowego wystąpienia.

```csharp
[assembly: Dependency(typeof(TextToSpeechImplementation))]
namespace DependencyServiceSample.iOS
{

    public class TextToSpeechImplementation : ITextToSpeech
    {
        public TextToSpeechImplementation() { }

        public void Speak(string text)
        {
            var speechSynthesizer = new AVSpeechSynthesizer();
            var speechUtterance = new AVSpeechUtterance(text)
            {
                Rate = AVSpeechUtterance.MaximumSpeechRate / 4,
                Voice = AVSpeechSynthesisVoice.FromLanguage("en-US"),
                Volume = 0.5f,
                PitchMultiplier = 1.0f
            };

            speechSynthesizer.SpeakUtterance(speechUtterance);
        }
    }
}
```

`[assembly]` Atrybut rejestruje klasę jako implementacja `ITextToSpeech` interfejsu, co oznacza, że `DependencyService.Get<ITextToSpeech>()` można użyć w kodzie udostępniony można utworzyć wystąpienie.

<a name="Android_Implementation" />

## <a name="android-implementation"></a>Implementacja systemu android

Bardziej złożone niż wersja systemu iOS jest kodu dla systemu Android: wymaga klasy implementującej odziedziczone specyficzne dla systemu Android `Java.Lang.Object` i wdrożenie `IOnInitListener` również interfejs. Wymagany jest również dostęp do bieżącego kontekstu Android jest udostępniana przez `MainActivity.Instance` właściwości.

```csharp
[assembly: Dependency(typeof(TextToSpeechImplementation))]
namespace DependencyServiceSample.Droid
{
    public class TextToSpeechImplementation : Java.Lang.Object, ITextToSpeech, TextToSpeech.IOnInitListener
    {
        TextToSpeech speaker;
        string toSpeak;

        public void Speak(string text)
        {
            toSpeak = text;
            if (speaker == null)
            {
                speaker = new TextToSpeech(MainActivity.Instance, this);
            }
            else
            {
                speaker.Speak(toSpeak, QueueMode.Flush, null, null);
            }
        }

        public void OnInit(OperationResult status)
        {
            if (status.Equals(OperationResult.Success))
            {
                speaker.Speak(toSpeak, QueueMode.Flush, null, null);
            }
        }
    }
}
```

`[assembly]` Atrybut rejestruje klasę jako implementacja `ITextToSpeech` interfejsu, co oznacza, że `DependencyService.Get<ITextToSpeech>()` można użyć w kodzie udostępniony można utworzyć wystąpienie.

<a name="WindowsImplementation" />

## <a name="universal-windows-platform-implementation"></a>Implementacja platformy uniwersalnej systemu Windows

Platforma uniwersalna systemu Windows ma mowy interfejsu API w `Windows.Media.SpeechSynthesis` przestrzeni nazw. Pamiętaj, aby znaczników jest tylko zastrzeżenie: **mikrofon** możliwości w manifeście, w przeciwnym razie dostęp do mowy interfejsy API są zablokowane.

```csharp
[assembly:Dependency(typeof(TextToSpeechImplementation))]
public class TextToSpeechImplementation : ITextToSpeech
{
    public async void Speak(string text)
    {
        var mediaElement = new MediaElement();
        var synth = new Windows.Media.SpeechSynthesis.SpeechSynthesizer();
        var stream = await synth.SynthesizeTextToStreamAsync(text);

        mediaElement.SetSource(stream, stream.ContentType);
        mediaElement.Play();
    }
}
```

`[assembly]` Atrybut rejestruje klasę jako implementacja `ITextToSpeech` interfejsu, co oznacza, że `DependencyService.Get<ITextToSpeech>()` można użyć w kodzie udostępniony można utworzyć wystąpienie.

<a name="Implementing_in_Shared_Code" />

## <a name="implementing-in-shared-code"></a>Implementacja w kodzie udostępnionego

Firma Microsoft teraz zapisać i przetestować udostępnionego kodu, który uzyskuje dostęp do interfejsu tekst na mowę. To prosta strona zawiera przycisk, które wyzwala funkcji mowy. Używa `DependencyService` można pobrać wystąpienia `ITextToSpeech` interfejsu &ndash; w czasie wykonywania tego wystąpienia będzie implementację specyficzne dla platformy, która ma pełny dostęp do natywnych zestawu SDK.

```csharp
public MainPage ()
{
    var speak = new Button {
        Text = "Hello, Forms !",
        VerticalOptions = LayoutOptions.CenterAndExpand,
        HorizontalOptions = LayoutOptions.CenterAndExpand,
    };
    speak.Clicked += (sender, e) => {
        DependencyService.Get<ITextToSpeech>().Speak("Hello from Xamarin Forms");
    };
    Content = speak;
}
```

W aplikacji przy użyciu natywnych mowy SDK na każdej platformie, mówiąc spowoduje uruchomienie takiej aplikacji systemu iOS, Android lub platformy uniwersalnej systemu Windows i naciskając przycisk.

 ![iOS i Android przycisk tekst na mowę](text-to-speech-images/running.png "przykładowy tekst na mowę")


## <a name="related-links"></a>Linki pokrewne

- [Przy użyciu DependencyService (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UsingDependencyService/)
- [DependencyServiceSample](https://developer.xamarin.com/samples/xamarin-forms/DependencyService/DependencyServiceSample/)
- [Tekst na mowę skoroszytu](https://developer.xamarin.com/workbooks/xamarin-forms/application-fundamentals/text-to-speech/text-to-speech.workbook)
