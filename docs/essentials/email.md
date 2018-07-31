---
title: 'Xamarin.Essentials: wiadomości E-mail'
description: Klasa poczty E-mail w Xamarin.Essentials umożliwia aplikacji można otworzyć domyślnej aplikacji poczty e-mail przy użyciu informacji w tym tematu, treści i adresatów (do, DW, UDW).
ms.assetid: 5FBB6FF0-0E7B-4C29-8F06-91642AF12629
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: f113cebfebf4238fd4b75ad8ab248e2abf61efea
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353909"
---
# <a name="xamarinessentials-email"></a>Xamarin.Essentials: wiadomości E-mail

![NuGet w wersji wstępnej](~/media/shared/pre-release.png)

**E-mail** klasa umożliwia aplikacji można otworzyć domyślnej aplikacji poczty e-mail przy użyciu informacji w tym tematu, treści i adresatów (do, DW, UDW).

## <a name="using-email"></a>Za pomocą poczty E-mail

Dodaj odwołanie do Xamarin.Essentials w klasie:

```csharp
using Xamarin.Essentials;
```

Funkcje poczty E-mail działa przez wywołanie metody `ComposeAsync` metoda `EmailMessage` zawierający informacje dotyczące wiadomości e-mail:

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
            };
            await Email.ComposeAsync(message);
        }
        catch (FeatureNotSupportedException fbsEx)
        {
            // Email is not supported on this device
        }
        catch (Exception ex)
        {
            // Some other exception occurred
        }
    }
}
```

## <a name="api"></a>interfejs API

- [Kod źródłowy adres e-mail](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Email)
- [Dokumentacja interfejsu API w wiadomości e-mail](xref:Xamarin.Essentials.Email)
