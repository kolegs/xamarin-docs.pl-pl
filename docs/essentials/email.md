---
title: Xamarin.Essentials poczty E-mail
description: Klasy wiadomości E-mail umożliwia aplikacji można otworzyć domyślnej aplikacji poczty e-mail przy użyciu informacji w tym tematu, treści i adresatów (do, DW, UDW).
ms.assetid: 5FBB6FF0-0E7B-4C29-8F06-91642AF12629
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 3fee30e31dc18665d59f944462959fd3f8166968
ms.sourcegitcommit: 4db5f5c93f79f273d8fc462de2f405458b62fc02
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/19/2018
---
# <a name="xamarinessentials-email"></a>Xamarin.Essentials poczty E-mail

![NuGet w wersji wstępnej](~/media/shared/pre-release.png)

**E-mail** klasa umożliwia aplikacji można otworzyć domyślnej aplikacji poczty e-mail przy użyciu informacji w tym tematu, treści i adresatów (do, DW, UDW).

## <a name="using-email"></a>Za pomocą poczty E-mail

Dodaj odwołanie do Xamarin.Essentials w swojej klasy:

```csharp
using Xamarin.Essentials;
```

Funkcja poczty E-mail działa przez wywołanie metody `ComposeAsync` metody `EmailMessage` zawierający informacje dotyczące wiadomości e-mail:

```csharp
public class EmailTest
{
    public async Task SendEmail(string subject, string body, List<string> recipients)
    {
        try
        {
            var message = new EmailMessage
            {
                Subject = subject,
                Body = body,
                To = recipients,
                //Cc = ccRecipients,
                //Bcc = bccRecipients
            }
            await Email.ComposeAsync(message);
        }
        catch (FeatureNotSupportedException fbsEx)
        {
            // Sms is not supported on this device
        }
        catch (Exception ex)
        {
            // Some other exception occured
        }
    }
}
```

## <a name="api"></a>interfejs API

- [Kod źródłowy poczty e-mail](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Email)
- [Dokumentacja interfejsu API w wiadomości e-mail](xref:Xamarin.Essentials.Email)