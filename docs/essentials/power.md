---
title: 'Xamarin.Essentials: Stan oszczędzanie energii zasilania'
description: Klasa zasilania umożliwia programowi można uzyskać stanu oszczędzania energii, aby ustalić, jeśli urządzenie działa w trybie niskiego poboru energii.
ms.assetid: C176D177-8B77-4A9C-9F3B-27852A8DCD5F
author: charlespetzold
ms.author: chape
ms.date: 06/27/2018
ms.openlocfilehash: 6d8ccb5be69eb1ea7ea63d3f5c373d9284089679
ms.sourcegitcommit: 72450a6a29599fa133ff4f16fb0b1f443d89f9dc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/28/2018
ms.locfileid: "37080398"
---
# <a name="xamarinessentials-power-energy-saver-status"></a>Xamarin.Essentials: Stan oszczędzanie energii zasilania

![NuGet w wersji wstępnej](~/media/shared/pre-release.png)

**Zasilania** klasa udostępnia informacje o stanie oszczędzania energii urządzenia, która wskazuje, czy urządzenie działa w trybie niskiego poboru energii. Aplikacje należy unikać przetwarzania w tle, jeśli znajduje się w stanie oszczędzania energii urządzenia.

## <a name="background"></a>Tło

Urządzeń, które korzystają z baterii można uruchomić w trybie oszczędzania energii niskiego poboru energii. Czasami urządzenia są przełączane w tym trybie automatycznie, na przykład gdy baterii spadnie poniżej pojemności 20%. System operacyjny reaguje na tryb oszczędzania energii ograniczenia działalności, które mają na celu wykorzystanie baterii. Aplikacje może pomóc, unikając przetwarzania w tle lub innych działań podłączone, gdy znajduje się w trybie oszczędzania energii.

Dla urządzeń z systemem Android **zasilania** klasy zwraca użyteczne informacje tylko dla systemu Android w wersji 5.0 (interfejs typu lizak) lub nowszym.

## <a name="using-the-power-class"></a>Używanie klasy zasilania

Dodaj odwołanie do Xamarin.Essentials w swojej klasy:

```csharp
using Xamarin.Essentials;
```

Uzyskaj przy bieżącym stanie oszczędzania energii urządzenia przy użyciu statycznych `Power.EnergySaverStatus` właściwości:

```csharp
// Get energy saver status
var status = Power.EnergySaverStatus;
```

Ta właściwość zwraca element członkowski `EnergySaverStatus` wyliczenia, który może być `On`, `Off`, lub `Unknown`. Jeśli właściwość zwraca `On`, aplikacji, należy unikać przetwarzania w tle lub innych działań, które może używać dużej ilości zasilania.

Aplikację należy również zainstalować program obsługi zdarzeń. **Zasilania** klasa przedstawia zdarzenie jest wyzwalane, gdy zmienia się stan oszczędzania energii:

```csharp
public class EnergySaverTest
{
    public EnergySaverTest()
    {
        // Subscribe to changes of energy-saver status
        Power.EnergySaverStatusChanaged += OnEnergySaverStatusChanaged;
    }

    private void OnEnergySaverStatusChanaged(EnergySaverStatusChanagedEventArgs e)
    {
        // Process change
        var status = e.EnergySaverStatus;
    }
}
```

Jeśli stan oszczędzania energii zmienia się na `On`, aplikacja powinna zostać przerwana w wykonywania przetwarzania w tle. Jeśli stan zmienia się na `Unknown` lub `Off`, aplikacja może wznowić przetwarzania w tle.

## <a name="api"></a>interfejs API

- [Kod źródłowy zasilania](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Power)
- [Dokumentacja interfejsu API zasilania](xref:Xamarin.Essentials.Power)
