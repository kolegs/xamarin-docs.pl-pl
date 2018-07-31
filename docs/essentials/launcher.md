---
title: Uruchamianie Xamarin.Essentials
description: Klasa uruchamiania w Xamarin.Essentials umożliwia aplikacji w celu otwarcia identyfikatora URI przez system.
ms.assetid: BABF40CC-8BEE-43FD-BE12-6301DF27DD33
author: jamesmontemagno
ms.author: jamont
ms.date: 07/25/2018
ms.openlocfilehash: 3ab820ece127558c1a5ca8b13c05fc4d6f20646e
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353915"
---
# <a name="xamarinessentials-launcher"></a>Xamarin.Essentials: uruchamianie programu

![NuGet w wersji wstępnej](~/media/shared/pre-release.png)

**Uruchamiania** klasa umożliwia aplikacji w celu otwarcia identyfikatora URI przez system. Jest to często używane podczas głębokiego konsolidacji w innej aplikacji niestandardowe schematy identyfikatorów URI. Jeśli chcesz otworzyć przeglądarkę z witryną sieci Web, a następnie powinni zapoznać się z **[przeglądarki](open-browser.md)** interfejsu API.

## <a name="using-launcher"></a>Za pomocą uruchamiania

Dodaj odwołanie do Xamarin.Essentials w klasie:

```csharp
using Xamarin.Essentials;
```

Aby użyć wywołania funkcji uruchamiania `OpenAsync` metody i przekaż `string` lub `Uri` do otwarcia. Opcjonalnie `CanOpenAsync` metoda może służyć do sprawdzania, jeśli schemat identyfikatora URI może być obsługiwany przez aplikację na urządzeniu.

```csharp
public class LauncherTest
{
    public async Task OpenRideShare()
    {
        var supportsUri = await Launcher.CanOpenAsync("lyft://");
        if (supportsUri)
            await Launcher.OpenAsync("lyft://ridetype?id=lyft_line");
    }
}
```

## <a name="api"></a>interfejs API

- [Uruchamianie kodu źródłowego](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Launcher)
- [Dokumentacja interfejsu API uruchamiania](xref:Xamarin.Essentials.Launcher)
