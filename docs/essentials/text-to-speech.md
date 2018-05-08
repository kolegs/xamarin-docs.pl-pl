---
title: Tekst na mowę Xamarin.Essentials
description: Umożliwia klasy TextToSpeech aplikacji korzystanie z wbudowanych w tekst na mowę aparaty porozmawiać wstecz tekstu z urządzenia, a także do zapytania dostępne języki obsługiwane przez aparat.
ms.assetid: AEEF03AE-A047-4DF0-B0E8-CC8D9A7B8351
ms.technology: xamarin-crossplatform
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: e5ee5a324c6e753d389f7e80df106dbf4af1a8ca
ms.sourcegitcommit: 46d3c9daa45350bdd536d9e105517f3c1c753c5b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/07/2018
---
# <a name="xamarinessentials-text-to-speech"></a>Tekst na mowę Xamarin.Essentials

![NuGet w wersji wstępnej](~/media/shared/pre-release.png)

**TextToSpeech** klasa umożliwia aplikacji korzystanie z wbudowanych w tekst na mowę aparaty porozmawiać wstecz tekstu z urządzenia, a także do zapytania dostępne języki obsługiwane przez aparat.

## <a name="using-text-to-speech"></a>Przy użyciu tekst na mowę

Dodaj odwołanie do Xamarin.Essentials w swojej klasy:

```csharp
using Xamarin.Essentials;
```

Funkcja tekst na mowę działa przez wywołanie metody `SpeakAsync` metody tekstu i opcjonalnych parametrów i zwraca po zakończeniu utterance. 

```csharp
public async Task SpeakNowDefaultSettings()
{
    await TextToSpeech.SpeakAsync("Hello World");

    // This method will block until utterance finishes.
}

public void SpeakNowDefaultSettings2()
{
    TextToSpeech.SpeakAsync("Hello World").ContinueWith((t) => 
    {
        // Logic that will run after utterance finishes.

    }, TaskScheduler.FromCurrentSynchronizationContext());
}
```

Ta metoda przyjmuje opcjonalny CancellationToken przestanie utterance, jeden z jej uruchamiania. 
```csharp
CancellationTokenSource cts;
public async Task SpeakNowDefaultSettings()
{
    cts = new CancellationTokenSource();
    await TextToSpeech.SpeakAsync("Hello World", cancelToken: cts.Token);

    // This method will block until utterance finishes.
}

public void CancelSpeech()
{
    if (cts?.IsCancellationRequested ?? false)
        return;

    cts.Cancel();
}
```

Tekst na mowę zostanie automatycznie kolejka żądań mowy z tym samym wątku. 

```csharp
bool isBusy = false;
public void SpeakMultiple()
{
    isBusy = true;
    Task.Run(async () =>
    {
        await TextToSpeech.SpeakAsync("Hello World 1");
        await TextToSpeech.SpeakAsync("Hello World 2");
        await TextToSpeech.SpeakAsync("Hello World 3");
        isBusy = false;
    });

    // or you can query multiple without a Task:
    Task.WhenAll(
        TextToSpeech.SpeakAsync("Hello World 1"),
        TextToSpeech.SpeakAsync("Hello World 2"),
        TextToSpeech.SpeakAsync("Hello World 3"))
        .ContinueWith((t) => { isBusy = false; }, TaskScheduler.FromCurrentSynchronizationContext());
}
```

### <a name="speech-settings"></a>Ustawienia rozpoznawania mowy

Aby uzyskać większą kontrolę nad jak dźwięk jest używany z tyłu `SpeakSettings` umożliwiająca ustawienie woluminu, wysokości i ustawień regionalnych.

```csharp
public async Task SpeakNow()
{
    var settings = new SpeakSettings()
        {
            Volume = .75,
            Pitch = 1.0
        };

    await TextToSpeech.SpeakAsync("Hello World", settings);
}
```

Obsługiwane wartości dla parametrów są następujące:

| Parametr | Minimalnie | Maksymalnie |
| --- | :---: | :---: |
| Wysokość | 0 | 2.0 |
| Wolumin | 0 | 1.0 |

### <a name="speech-locales"></a>Ustawienia regionalne mowy

Każdej z platform oferuje ustawień regionalnych porozmawiać wstecz tekstu w wielu językach i akcentów. Dotyczy wszystkich platform innej kodów i sposobów określania, który jest dlaczego Essentials zapewnia i platform `Locale` klasy i sposób wyszukiwać w nich z `GetLocalesAsync`.

```csharp
public async Task SpeakNow()
{
    var locales = await TextToSpeech.GetLocalesAsync();

    // Grab the first locale
    var locale = locales.FirstOrDefault();

    var settings = new SpeakSettings()
        {
            Volume = .75,
            Pitch = 1.0,
            Locale = locale
        };

    await TextToSpeech.SpeakAsync("Hello World", settings);
}
```

## <a name="limitations"></a>Ograniczenia

- Utterance kolejki nie jest gwarantowana w przypadku przez wiele wątków.
- Tło odtwarzania audio nie jest oficjalnie obsługiwana.

## <a name="api"></a>interfejs API

- [Kod źródłowy TextToSpeech](https://github.com/xamarin/Essentials/tree/master/Essentials/TextToSpeech)
- [Dokumentacja interfejsu API TextToSpeech](xref:Xamarin.Essentials.TextToSpeech)
