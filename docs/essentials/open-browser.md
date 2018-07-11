---
title: Otwórz Xamarin.Essentials przeglądarki
description: Klasa przeglądarki w Xamarin.Essentials umożliwia aplikacji w celu otwarcia link sieci web w przeglądarce preferowanych zoptymalizowanego systemu lub zewnętrznej przeglądarki.
ms.assetid: BABF40CC-8BEE-43FD-BE12-6301DF27DD33
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 563d3899cffb80c0215d90e8e4392046c4635256
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38815709"
---
# <a name="xamarinessentials-browser"></a>Xamarin.Essentials: przeglądarki

![NuGet w wersji wstępnej](~/media/shared/pre-release.png)

**Przeglądarki** klasa umożliwia aplikacji w celu otwarcia link sieci web w przeglądarce preferowanych zoptymalizowanego systemu lub zewnętrznej przeglądarki.

## <a name="using-browser"></a>Za pomocą przeglądarki

Dodaj odwołanie do Xamarin.Essentials w klasie:

```csharp
using Xamarin.Essentials;
```

Funkcje przeglądarki działa przez wywołanie metody `OpenAsync` metody z `Uri` i `BrowserLaunchType`.

```csharp

public class BrowserTest
{
    public async Task OpenBrowser(Uri uri)
    {
        await Browser.OpenAsync(uri, BrowserLaunchType.SystemPreferred);
    }
}
```

## <a name="platform-implementation-specifics"></a>Funkcje specyficzne dla implementacji platformy

# <a name="androidtabandroid"></a>[Android](#tab/android)

Typ uruchomienia Określa, jak uruchomieniu przeglądarki:

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

Przeglądarka domyślna zawsze zostanie uruchomiony bez względu na to `BrowserLaunchType`.

--------------

## <a name="api"></a>interfejs API

- [Kod źródłowy w przeglądarce](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Browser)
- [Przeglądarka interfejsu API, dokumentacji](xref:Xamarin.Essentials.Browser)
