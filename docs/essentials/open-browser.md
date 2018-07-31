---
title: Otwórz Xamarin.Essentials przeglądarki
description: Klasa przeglądarki w Xamarin.Essentials umożliwia aplikacji w celu otwarcia link sieci web w przeglądarce preferowanych zoptymalizowanego systemu lub zewnętrznej przeglądarki.
ms.assetid: BABF40CC-8BEE-43FD-BE12-6301DF27DD33
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 7e58d439f5a6eaafe9b1b5e7ca874a986e468cb9
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353285"
---
# <a name="xamarinessentials-browser"></a>Xamarin.Essentials: przeglądarki

![NuGet w wersji wstępnej](~/media/shared/pre-release.png)

**Przeglądarki** klasa umożliwia aplikacji w celu otwarcia link sieci web w przeglądarce preferowanych zoptymalizowanego systemu lub zewnętrznej przeglądarki.

## <a name="using-browser"></a>Za pomocą przeglądarki

Dodaj odwołanie do Xamarin.Essentials w klasie:

```csharp
using Xamarin.Essentials;
```

Funkcje przeglądarki działa przez wywołanie metody `OpenAsync` metody z `Uri` i `BrowserLaunchMode`.

```csharp

public class BrowserTest
{
    public async Task OpenBrowser(Uri uri)
    {
        await Browser.OpenAsync(uri, BrowserLaunchMode.SystemPreferred);
    }
}
```

## <a name="platform-implementation-specifics"></a>Funkcje specyficzne dla implementacji platformy

# <a name="androidtabandroid"></a>[Android](#tab/android)

Tryb uruchamiania Określa, jak uruchomieniu przeglądarki:

## <a name="system-preferred"></a>System preferowane

[Chrome kart niestandardowych](https://developer.chrome.com/multidevice/android/customtabs) będzie próbował służyć załadować identyfikatora Uri i zachować świadomości nawigacji.

## <a name="external"></a>Zewnętrzna

`Intent` Będzie służyć do żądania, identyfikatora Uri można otworzyć za pośrednictwem przeglądarki normalne systemów.

# <a name="iostabios"></a>[iOS](#tab/ios)

## <a name="system-preferred"></a>System preferowane

[SFSafariViewController](https://developer.xamarin.com/api/type/SafariServices.SFSafariViewController/) do służy do ładowania identyfikator Uri i zapewnianie świadomości nawigacji.

## <a name="external"></a>Zewnętrzna

Standardowa `OpenUrl` w aplikacji głównej jest używany do uruchomienia domyślną przeglądarkę poza aplikacją.

# <a name="uwptabuwp"></a>[PLATFORMY UNIWERSALNEJ SYSTEMU WINDOWS](#tab/uwp)

Przeglądarka domyślna zawsze zostanie uruchomiony bez względu na to `BrowserLaunchMode`.

--------------

## <a name="api"></a>interfejs API

- [Kod źródłowy w przeglądarce](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Browser)
- [Przeglądarka interfejsu API, dokumentacji](xref:Xamarin.Essentials.Browser)
