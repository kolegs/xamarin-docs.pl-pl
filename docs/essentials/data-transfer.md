---
title: 'Xamarin.Essentials: Transfer danych'
description: Klasa DataTransfer w Xamarin.Essentials umożliwia aplikacji udostępnianie danych, takich jak sieci web i tekst łącza do innych aplikacji na urządzeniu.
ms.assetid: B7B01D55-0129-4C87-B515-89F8F4E94665
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 31e27556a6681b144084d2177cf3fde8fe8e5459
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353522"
---
# <a name="xamarinessentials-data-transfer"></a>Xamarin.Essentials: Transfer danych

![NuGet w wersji wstępnej](~/media/shared/pre-release.png)

**DataTransfer** klasa umożliwia aplikacji udostępnianie danych, takich jak sieci web i tekst łącza do innych aplikacji na urządzeniu.

## <a name="using-data-transfer"></a>Przy użyciu transferu danych

Dodaj odwołanie do Xamarin.Essentials w klasie:

```csharp
using Xamarin.Essentials;
```

Funkcje transferu danych działa przez wywołanie metody `RequestAsync` metody za pomocą danych ładunku żądania, zawierający informacje do udostępnienia do innych aplikacji. Tekst i identyfikatora Uri mogą być mieszane i każdej z platform będzie obsługiwać filtrowanie na podstawie zawartości.

```csharp

public class DataTransferTest
{
    public async Task ShareText(string text)
    {
        await DataTransfer.RequestAsync(new ShareTextRequest
            {
                Text = text,
                Title = "Share Text"
            });
    }

    public async Task ShareUri(string uri)
    {
        await DataTransfer.RequestAsync(new ShareTextRequest
            {
                Uri = uri,
                Title = "Share Web Link"
            });
    }
}
```

Interfejs użytkownika do udostępnienia do aplikacji zewnętrznej, który pojawia się po wysłaniu żądania:

![Transfer danych](data-transfer-images/data-transfer.png)

## <a name="platform-differences"></a>Różnice dotyczące platform

# <a name="androidtabandroid"></a>[Android](#tab/android)

* `Subject` Właściwość jest używana dla żądanego tematu wiadomości.

# <a name="iostabios"></a>[iOS](#tab/ios)

* `Subject` Nie jest używany.
* `Title` Nie jest używany.

# <a name="uwptabuwp"></a>[PLATFORMY UNIWERSALNEJ SYSTEMU WINDOWS](#tab/uwp)

* `Title` Domyślnie nazwa aplikacji Jeśli nie będzie ustawiony.
* `Subject` Nie jest używany.

-----

## <a name="api"></a>interfejs API

- [Kod źródłowy transferu danych](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/DataTransfer)
- [Dokumentacja API transferu danych](xref:Xamarin.Essentials.DataTransfer)
