---
title: 'Xamarin.Essentials: Blokada ekranu'
description: W tym dokumencie opisano klasy ScreenLock w Xamarin.Essentials, które mogą żądać aby objętych uśpiony, gdy aplikacja jest uruchomiona na ekranie.
ms.assetid: 6B67C114-315E-4199-AA72-3F90E85A4909
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 3c8110b7abc86fe1d12485579f134997718540e6
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782911"
---
# <a name="xamarinessentials-screen-lock"></a>Xamarin.Essentials: Blokada ekranu

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
        if (!ScreenLock.IsActive)
            ScreenLock.RequestActive();
        else
            ScreenLock.RequestRelease();
    }
}
```

## <a name="api"></a>interfejs API

- [Ekran blokady kodu źródłowego](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/ScreenLock)
- [Dokumentacja interfejsu API blokady ekranu](xref:Xamarin.Essentials.ScreenLock)
