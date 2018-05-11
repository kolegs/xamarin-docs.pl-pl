---
title: Transfer danych Xamarin.Essentials
description: Klasa DataTransfer włącza aplikację do udostępniania danych, takich jak sieci web i tekst łącza do innych aplikacji na urządzeniu.
ms.assetid: B7B01D55-0129-4C87-B515-89F8F4E94665
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: baec3bdd89cb98d7595a524b6b9c4263ca18aa41
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/10/2018
---
# <a name="xamarinessentials-data-transfer"></a>Transfer danych Xamarin.Essentials

![NuGet w wersji wstępnej](~/media/shared/pre-release.png)

**DataTransfer** klasa umożliwia aplikacji do udostępniania danych, takich jak sieci web i tekst łącza do innych aplikacji na urządzeniu.

## <a name="using-data-transfer"></a>Przy użyciu transferu danych

Dodaj odwołanie do Xamarin.Essentials w swojej klasy:

```csharp
using Xamarin.Essentials;
```

Transfer danych funkcji działa przez wywołanie metody `RequestAsync` metody z ładunku żądania danych zawierający informacje do udostępnienia do innych aplikacji. Można łączyć tekstu oraz identyfikatora Uri i każdej platformy obsługuje filtrowanie na podstawie zawartości.

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

Interfejs użytkownika do udostępnienia do aplikacji zewnętrznych, które pojawia się po wysłaniu żądania:

![Transfer danych](data-transfer-images/data-transfer.png)

## <a name="platform-differences"></a>Różnice dotyczące platformy

| Platforma | Różnica |
| --- | --- |
| Android | Właściwość podmiotu jest używana dla żądanego tematu wiadomości. |
| iOS | Temat nie jest używany. |
| iOS | Tytuł nie jest używany. |
| Platforma UWP | Tytuł będzie domyślna nazwa aplikacji, jeśli nie ustawiona. |
| Platforma UWP | Temat nie jest używany. |

## <a name="api"></a>interfejs API

- [Kod źródłowy transferu danych](https://github.com/xamarin/Essentials/tree/master/Essentials/DataTransfer)
- [Dokumentacja API transferu danych](xref:Xamarin.Essentials.DataTransfer)
