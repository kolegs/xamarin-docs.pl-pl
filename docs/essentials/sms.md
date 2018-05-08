---
title: Xamarin.Essentials SMS
description: Klasa Sms włącza aplikację można otworzyć domyślnej aplikacji programu SMS z określonego komunikatu do wysłania do adresata.
ms.assetid: 81A757F2-6F2A-458F-B9BE-770ADEBFAB58
ms.technology: xamarin-crossplatform
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 460aea01381934f1862946c6e17e314560e889f2
ms.sourcegitcommit: 46d3c9daa45350bdd536d9e105517f3c1c753c5b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/07/2018
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
