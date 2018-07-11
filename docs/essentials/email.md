---
title: 'Xamarin.Essentials: wiadomości E-mail'
description: Klasa poczty E-mail w Xamarin.Essentials umożliwia aplikacji można otworzyć domyślnej aplikacji poczty e-mail przy użyciu informacji w tym tematu, treści i adresatów (do, DW, UDW).
ms.assetid: 5FBB6FF0-0E7B-4C29-8F06-91642AF12629
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: aea2f429126180ae3d98bc665bed5574f416ea53
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38848549"
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
            // Some other exception occured
        }
    }
}
```

## <a name="api"></a>interfejs API

- [Kod źródłowy adres e-mail](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Email)
- [Dokumentacja interfejsu API w wiadomości e-mail](xref:Xamarin.Essentials.Email)
