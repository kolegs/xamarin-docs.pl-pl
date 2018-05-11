---
title: Blokada ekranu Xamarin.Essentials
description: Klasa ScreenLock żądanie można zapobiec objętych uśpiony, gdy aplikacja jest uruchomiona na ekranie.
ms.assetid: 6B67C114-315E-4199-AA72-3F90E85A4909
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 7175362dcb7f85746ea85447936d7fe3e2fd026b
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/09/2018
---
# <a name="xamarinessentials-screen-lock"></a>Blokada ekranu Xamarin.Essentials

![NuGet w wersji wstępnej](~/media/shared/pre-release.png)

**ScreenLock** klasy mogą żądać aby objętych uśpiony, gdy aplikacja jest uruchomiona na ekranie.

## <a name="using-screenlock"></a>Przy użyciu ScreenLock

Dodaj odwołanie do Xamarin.Essentials w swojej klasy:

```csharp
using Xamarin.Essentials;
```

Funkcja blokady ekranu działa przez wywołanie metody `RequestActive` i `RequestRelease` metody żądania automatycznemu wyłączaniu ekranu.

```csharp
public class ScreenLockTest
{
    public void ToggleScreenLock()
    {
        if (ScreenLock.IsMonitoring)
            ScreenLock.RequestActive();
        else
            ScreenLock.RequestRelease;
    }
}
```

## <a name="api"></a>interfejs API

- [Ekran blokady kodu źródłowego](https://github.com/xamarin/Essentials/tree/master/Essentials/ScreenLock)
- [Dokumentacja interfejsu API blokady ekranu](xref:Xamarin.Essentials.ScreenLock)
