---
title: 'Xamarin.Essentials: Blokada ekranu'
description: W tym dokumencie opisano klasy ScreenLock w Xamarin.Essentials, które mogą żądać, aby zapobiec objętych trybie uśpienia, gdy aplikacja jest uruchomiona na ekranie.
ms.assetid: 6B67C114-315E-4199-AA72-3F90E85A4909
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 3c8110b7abc86fe1d12485579f134997718540e6
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38848573"
---
# <a name="xamarinessentials-screen-lock"></a>Xamarin.Essentials: Blokada ekranu

![NuGet w wersji wstępnej](~/media/shared/pre-release.png)

**ScreenLock** klasy poprosić, aby zapobiec objętych trybie uśpienia, gdy aplikacja jest uruchomiona na ekranie.

## <a name="using-screenlock"></a>Za pomocą ScreenLock

Dodaj odwołanie do Xamarin.Essentials w klasie:

```csharp
using Xamarin.Essentials;
```

Funkcje blokady ekranu działa przez wywołanie metody `RequestActive` i `RequestRelease` metody poprosić wyłączenie ekranu.

```csharp
public class ScreenLockTest
{
    public void ToggleScreenLock()
    {
        if (!ScreenLock.IsActive)
            ScreenLock.RequestActive();
        else
            ScreenLock.RequestRelease();
    }
}
```

## <a name="api"></a>interfejs API

- [Kod źródłowy blokady ekranu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/ScreenLock)
- [Dokumentacja interfejsu API blokady ekranu](xref:Xamarin.Essentials.ScreenLock)
