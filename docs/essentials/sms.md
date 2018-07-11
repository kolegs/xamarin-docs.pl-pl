---
title: 'Xamarin.Essentials: wiadomości SMS'
description: Klasa programu Sms w Xamarin.Essentials umożliwia aplikacji można otworzyć domyślnej aplikacji SMS przy użyciu określonego komunikatu do wysłania do adresata.
ms.assetid: 81A757F2-6F2A-458F-B9BE-770ADEBFAB58
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: a93a67b83ea8f435a5e3ad5d26e1d6cbbb7092f7
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38815600"
---
# <a name="xamarinessentials-sms"></a>Xamarin.Essentials: wiadomości SMS

![NuGet w wersji wstępnej](~/media/shared/pre-release.png)

**Sms** klasa umożliwia aplikacji można otworzyć domyślnej aplikacji SMS przy użyciu określonego komunikatu do wysłania do adresata.

## <a name="using-sms"></a>Za pomocą wiadomości Sms

Dodaj odwołanie do Xamarin.Essentials w klasie:

```csharp
using Xamarin.Essentials;
```

Funkcje programu SMS działa przez wywołanie metody `ComposeAsync` metoda `SmsMessage` zawiera adresata wiadomości oraz treść wiadomości, które są opcjonalne.

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

- [SMS — kod źródłowy](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Sms)
- [Dokumentacja interfejsu API programu SMS](xref:Xamarin.Essentials.Sms)
