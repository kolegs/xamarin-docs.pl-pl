---
title: 'Xamarin.Essentials: zamiana tekstu na mowę'
description: Klasa TextToSpeech Xamarin.Essentials umożliwia aplikacji korzystanie z wbudowanej zamiany tekstu na mowę silników mówić tekstowe zaplecze z urządzenia, a także do kwerendy językach obsługujących silnika.
ms.assetid: AEEF03AE-A047-4DF0-B0E8-CC8D9A7B8351
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: ba822870edafce44140caa66b01f4da242fb7779
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353616"
---
# <a name="xamarinessentials-text-to-speech"></a>Xamarin.Essentials: zamiana tekstu na mowę

![NuGet w wersji wstępnej](~/media/shared/pre-release.png)

**TextToSpeech** klasa umożliwia aplikacji korzystanie z wbudowanej zamiany tekstu na mowę silników mówić tekstowe zaplecze z urządzenia, a także do kwerendy językach obsługujących silnika.

## <a name="using-text-to-speech"></a>Za pomocą zamiany tekstu na mowę

Dodaj odwołanie do Xamarin.Essentials w klasie:

```csharp
using Xamarin.Essentials;
```

Funkcja zamiany tekstu na mowę działa przez wywołanie metody `SpeakAsync` metoda tekstu i opcjonalnych parametrów i zwraca po zakończeniu wypowiedź. 

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

Ta metoda przyjmuje opcjonalny `CancellationToken` przestanie wypowiedź po jej uruchomieniu.

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

Automatycznie zamiany tekstu na mowę może umieścić w kolejce żądań mowy z tym samym wątku.

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

Aby uzyskać większą kontrolę nad jak audio jest używany z tyłu `SpeakSettings` umożliwiająca ustawienie woluminu, skoku i ustawień regionalnych.

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

Obsługiwane wartości dla tych parametrów są następujące:

| Parametr | Minimalnie | Maksymalnie |
| --- | :---: | :---: |
| Wysokość | 0 | 2.0 |
| Wolumin | 0 | 1.0 |

### <a name="speech-locales"></a>Ustawienia regionalne mowy

Każdej z platform oferuje ustawień regionalnych mówić wstecz tekst w wielu językach i akcentów. Dotyczy wszystkich platform różne kody i sposobów określania tego, co jest dlaczego Essentials zapewnia dla wielu platform `Locale` klasy i sposób ich za pomocą zapytań `GetLocalesAsync`.

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

- Wypowiedź kolejki nie jest gwarantowana w przypadku ich wywołania przez wiele wątków.
- Odtwarzanie dźwięku w tle nie jest oficjalnie obsługiwany.

## <a name="api"></a>interfejs API

- [Kod źródłowy TextToSpeech](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/TextToSpeech)
- [Dokumentacja interfejsu API TextToSpeech](xref:Xamarin.Essentials.TextToSpeech)
