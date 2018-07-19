---
title: 'Xamarin.Essentials: Telefon'
description: Klasa PhoneDialer.Document w Xamarin.Essentials umożliwia aplikacji w celu otwarcia numeru telefonu w przypadku wybierania numeru
ms.assetid: E7457942-4D7B-4195-A2FF-417919B9537F
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 34a6c80836d8cb42b1f8fd95718fe248d4701c0f
ms.sourcegitcommit: 7f2e44e6f628753e06a5fe2a3076fc2ec5baa081
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/18/2018
ms.locfileid: "39130796"
---
# <a name="xamarinessentials-phone-dialer"></a>Xamarin.Essentials: Telefon

![NuGet w wersji wstępnej](~/media/shared/pre-release.png)

**PhoneDialer.Document** klasa umożliwia aplikacji w celu otwarcia numeru telefonu w przypadku wybierania numeru.

## <a name="using-phone-dialer"></a>Za pomocą telefonu

Dodaj odwołanie do Xamarin.Essentials w klasie:

```csharp
using Xamarin.Essentials;
```

Funkcja Telefon działa przez wywołanie metody `Open` metody za pomocą numeru telefonu, aby otworzyć program Telefon, za pomocą. Gdy `Open` żądania interfejsu API będzie próbować automatycznie sformatować liczbę, w oparciu kod kraju, jeśli określony.

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
