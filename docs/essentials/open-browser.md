---
title: Otwórz Xamarin.Essentials przeglądarki
description: Klasa przeglądarki umożliwia aplikacji w celu otwarcia link sieci web w przeglądarce preferowanych zoptymalizowanego systemu lub zewnętrznej przeglądarki.
ms.assetid: BABF40CC-8BEE-43FD-BE12-6301DF27DD33
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: b7d04bec569e911df75af999f980da07c868e18f
ms.sourcegitcommit: 3e05b135b6ff0d607bc2378c1b6e66d2eebbcc3e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/12/2018
---
# <a name="xamarinessentials-browser"></a>Xamarin.Essentials przeglądarki

![NuGet w wersji wstępnej](~/media/shared/pre-release.png)

**Przeglądarki** klasa umożliwia aplikacji w celu otwarcia link sieci web w przeglądarce preferowanych zoptymalizowanego systemu lub zewnętrznej przeglądarki.

## <a name="using-browser"></a>Przy użyciu przeglądarki

Dodaj odwołanie do Xamarin.Essentials w swojej klasy:

```csharp
using Xamarin.Essentials;
```

Funkcja przeglądarki działa przez wywołanie metody `OpenAsync` metody z `Uri` i `BrowserLaunchType`.

```csharp

public class BrowserTest
{
    public async Task OpenBrowser(Uri uri)
    {
        await Browser.RequestAsync(uri, BrowserLaunchType.SystemPreferred);
    }
}
```

## <a name="platform-implementation-specifics"></a>Szczegóły implementacji platformy

# <a name="androidtabandroid"></a>[Android](#tab/android)

Typ uruchamiania Określa, jak uruchomić przeglądarki:

## <a name="system-preferred"></a>Preferowany systemu

[Chromowana kart niestandardowych](https://developer.chrome.com/multidevice/android/customtabs) będzie próbował służyć załadować identyfikator Uri i Zachowaj świadomości nawigacji.

## <a name="external"></a>Zewnętrzna

`Intent` Będą używane do żądania identyfikator Uri, można otworzyć za pomocą przeglądarki normalne systemów.

# <a name="iostabios"></a>[iOS](#tab/ios)

## <a name="system-preferred"></a>Preferowany systemu

[SFSafariViewController](https://developer.xamarin.com/api/type/SafariServices.SFSafariViewController/) służy do załadować identyfikator Uri i Zachowaj świadomości nawigacji.

## <a name="external"></a>Zewnętrzna

Standardowe `OpenUrl` na aplikacji głównej jest używany można uruchomić domyślnej przeglądarki lokalizację poza aplikacją.

# <a name="uwptabuwp"></a>[PLATFORMY UNIWERSALNEJ SYSTEMU WINDOWS](#tab/uwp)

Użytkownika domyślnej przeglądarki będzie zawsze uruchamiana niezależnie od tego `BrowserLaunchType`.

--------------

## <a name="api"></a>interfejs API

- [Kod źródłowy przeglądarki](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Browser)
- [Dokumentacja interfejsu API przeglądarki](xref:Xamarin.Essentials.Browser)
