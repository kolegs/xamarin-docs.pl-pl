---
title: 'Xamarin.Essentials: Telefon'
description: Klasa PhoneDialer.Document w Xamarin.Essentials umożliwia aplikacji w celu otwarcia link sieci web w przeglądarce preferowanych zoptymalizowanego systemu lub zewnętrznej przeglądarki.
ms.assetid: E7457942-4D7B-4195-A2FF-417919B9537F
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 6733e43ed4174d1dd78b2e8f70268eb54adadb98
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782854"
---
# <a name="xamarinessentials-phone-dialer"></a>Xamarin.Essentials: Telefon

![NuGet w wersji wstępnej](~/media/shared/pre-release.png)

**PhoneDialer.Document** klasa umożliwia aplikacji w celu otwarcia link sieci web w przeglądarce preferowanych zoptymalizowanego systemu lub zewnętrznej przeglądarki.

## <a name="using-phone-dialer"></a>Przy użyciu telefonu

Dodaj odwołanie do Xamarin.Essentials w swojej klasy:

```csharp
using Xamarin.Essentials;
```

Funkcja Telefon działa przez wywołanie metody `Open` metody za pomocą numeru telefonu, aby otworzyć program Telefon z. Gdy `Open` żądania interfejsu API automatycznie podejmie próbę numer oparte na kod kraju, jeśli określony format.

```csharp
public class PhoneDialerTest
{
    public async Task PlacePhoneCall(string number)
    {
        try
        {
            PhoneDialer.Open(number);
        }
        catch (ArgumentNullException anEx)
        {
            // Number was null or white space
        }
        catch (FeatureNotSupportedException ex)
        {
            // Phone Dialer is not supported on this device.
        }
        catch (Exception ex)
        {
            // Other error has occurred.
        }
    }
}
```

## <a name="api"></a>interfejs API

- [Telefon kodu źródłowego](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/PhoneDialer)
- [Dokumentacja interfejsu API wybierania numeru telefonu](xref:Xamarin.Essentials.PhoneDialer)
