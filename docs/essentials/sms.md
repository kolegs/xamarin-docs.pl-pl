---
title: Xamarin.Essentials SMS
description: Klasa Sms włącza aplikację można otworzyć domyślnej aplikacji programu SMS z określonego komunikatu do wysłania do adresata.
ms.assetid: 81A757F2-6F2A-458F-B9BE-770ADEBFAB58
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 5baeee03626ba659ac7e5c06be40039476a67e08
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/09/2018
---
# <a name="xamarinessentials-sms"></a>Xamarin.Essentials SMS

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

- [Kod źródłowy programu SMS](https://github.com/xamarin/Essentials/tree/master/Essentials/Sms)
- [Dokumentacja interfejsu API programu SMS](xref:Xamarin.Essentials.Sms)
