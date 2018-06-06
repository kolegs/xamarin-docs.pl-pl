---
title: 'Xamarin.Essentials: programu SMS'
description: Klasa programu Sms w Xamarin.Essentials włącza aplikację można otworzyć domyślnej aplikacji programu SMS z określonego komunikatu do wysłania do adresata.
ms.assetid: 81A757F2-6F2A-458F-B9BE-770ADEBFAB58
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: a93a67b83ea8f435a5e3ad5d26e1d6cbbb7092f7
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783090"
---
# <a name="xamarinessentials-sms"></a>Xamarin.Essentials: programu SMS

![NuGet w wersji wstępnej](~/media/shared/pre-release.png)

**Sms** klasa umożliwia aplikacji można otworzyć domyślnej aplikacji programu SMS z określonego komunikatu do wysłania do adresata.

## <a name="using-sms"></a>Za pomocą programu Sms

Dodaj odwołanie do Xamarin.Essentials w swojej klasy:

```csharp
using Xamarin.Essentials;
```

Funkcja programu SMS działa przez wywołanie metody `ComposeAsync` metody `SmsMessage` zawierający adresata wiadomości i treści wiadomości, które są opcjonalne.

```csharp
public class SmsTest
{
    public async Task SendSms(string messageText, string recipient)
    {
        try
        {
            var message = new SmsMessage(messageText, recipient);
            await Sms.ComposeAsync(message);
        }
        catch (FeatureNotSupportedException ex)
        {
            // Sms is not supported on this device.
        }
        catch (Exception ex)
        {
            // Other error has occurred.
        }
    }
}
```

## <a name="api"></a>interfejs API

- [Kod źródłowy programu SMS](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Sms)
- [Dokumentacja interfejsu API programu SMS](xref:Xamarin.Essentials.Sms)
