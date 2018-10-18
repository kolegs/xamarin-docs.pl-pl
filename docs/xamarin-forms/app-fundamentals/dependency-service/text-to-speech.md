---
title: Implementowanie zamiany tekstu na mowę
description: W tym artykule wyjaśniono, jak korzystać z zestawu narzędzi Xamarin.Forms DependencyService klasy wywołania interfejsu API zamiany tekstu na mowę natywnych każdej z platform.
ms.prod: xamarin
ms.assetid: 1D6164F9-4ECE-43A6-B583-1F5D5EFC1DDF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/18/2017
ms.openlocfilehash: 6d1948214b97a1b536b07b6420c32e4d27124518
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/18/2018
ms.locfileid: "38997546"
---
# <a name="implementing-text-to-speech"></a>Implementowanie zamiany tekstu na mowę

Ten artykuł przeprowadzi Cię tworzenie aplikacji dla wielu platform, która używa [ `DependencyService` ](xref:Xamarin.Forms.DependencyService) na dostęp do natywnych interfejsów API zamiany tekstu na mowę:

- **[Tworzenie interfejsu](#Creating_the_Interface)**  &ndash; zrozumieć, jak interfejs jest tworzony w współużytkowanym kodem.
- **[iOS implementacji](#iOS_Implementation)**  &ndash; Dowiedz się, jak implementować interfejs w kodzie natywnym dla systemu iOS.
- **[Implementacja systemu android](#Android_Implementation)**  &ndash; Dowiedz się, jak implementować interfejs w kodzie natywnym dla systemu Android.
- **[Implementacja platformy uniwersalnej systemu Windows](#WindowsImplementation)**  &ndash; Dowiedz się, jak implementować interfejs w kodzie natywnym do uniwersalnej platformy Windows (UWP).
- **[Implementacja w kodzie udostępnionego](#Implementing_in_Shared_Code)**  &ndash; Dowiedz się, jak używać `DependencyService` do wywołania w natywnych implementacji ze współużytkowanym kodem.

Aplikacji przy użyciu `DependencyService` mają następującą strukturę:

![](text-to-speech-images/tts-diagram.png "DependencyService struktury aplikacji")

<a name="Creating_the_Interface" />

## <a name="creating-the-interface"></a>Tworzenie interfejsu

Najpierw Utwórz interfejs w kodzie udostępnionego, który określa funkcje, które planujesz wdrożyć. W tym przykładzie interfejs zawiera jedną metodę `Speak`:

```csharp
public interface ITextToSpeech
{
    void Speak (string text);
}
```

Kodowania dla tego interfejsu w kod udostępniony umożliwi dostęp do interfejsów API rozpoznawania mowy na każdej platformie aplikacji platformy Xamarin.Forms.

> [!NOTE]
> Klasy implementującej interfejs musi mieć konstruktora bez parametrów, aby pracować z `DependencyService`.

<a name="iOS_Implementation" />

## <a name="ios-implementation"></a>Implementacja systemu iOS

Interfejs musi zostać wdrożona w każdego projektu specyficznego dla platformy aplikacji. Należy pamiętać, że klasa ma konstruktora bez parametrów, aby `DependencyService` można tworzyć nowych wystąpień.

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

`[assembly]` Atrybut rejestruje klasę jako implementacja `ITextToSpeech` interfejsu, co oznacza, że `DependencyService.Get<ITextToSpeech>()` może służyć w kodzie udostępnionej utworzyć jej wystąpienie.

<a name="Android_Implementation" />

## <a name="android-implementation"></a>Implementacja systemu android

Kodu dla systemu Android jest bardziej skomplikowane niż wersja systemu iOS: wymaga klasy implementującej odziedziczone specyficzne dla systemu Android `Java.Lang.Object` i wdrożenia `IOnInitListener` również interfejs. Wymaga to również dostępu do bieżącego kontekstu dla systemu Android, który jest uwidaczniany przez `MainActivity.Instance` właściwości.

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

`[assembly]` Atrybut rejestruje klasę jako implementacja `ITextToSpeech` interfejsu, co oznacza, że `DependencyService.Get<ITextToSpeech>()` może służyć w kodzie udostępnionej utworzyć jej wystąpienie.

<a name="WindowsImplementation" />

## <a name="universal-windows-platform-implementation"></a>Universal Windows Platform implementacji

Platforma uniwersalna Windows ma interfejs API rozpoznawania mowy w `Windows.Media.SpeechSynthesis` przestrzeni nazw. Pamiętaj, aby znaczników jest tylko zastrzeżenie: **mikrofon** możliwość w manifeście, w przeciwnym razie dostęp do rozpoznawania mowy, interfejsy API są zablokowane.

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

`[assembly]` Atrybut rejestruje klasę jako implementacja `ITextToSpeech` interfejsu, co oznacza, że `DependencyService.Get<ITextToSpeech>()` może służyć w kodzie udostępnionej utworzyć jej wystąpienie.

<a name="Implementing_in_Shared_Code" />

## <a name="implementing-in-shared-code"></a>Wdrażanie w udostępnionego kodu

Teraz możemy zapisu i przetestować kod udostępniony, który uzyskuje dostęp do interfejsu zamiany tekstu na mowę. Ta strona prostego zawiera przycisk, który wywołuje funkcje mowy. Używa ona `DependencyService` wystąpienia `ITextToSpeech` interfejsu &ndash; w czasie wykonywania tego wystąpienia będzie to implementacja specyficzne dla platformy, które ma pełny dostęp do natywnego zestawu SDK.

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

Działania tej aplikacji w systemie iOS, Android lub platformy UWP i naciśnięcie przycisku spowoduje aplikacji do użytkownika, za pomocą mowy natywnego zestawu SDK na każdej platformie.

 ![iOS i Android przycisk zamiany tekstu na mowę](text-to-speech-images/running.png "przykładowe zamiany tekstu na mowę")


## <a name="related-links"></a>Linki pokrewne

- [Za pomocą DependencyService (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UsingDependencyService/)
- [DependencyServiceSample](https://developer.xamarin.com/samples/xamarin-forms/DependencyService/DependencyServiceSample/)

