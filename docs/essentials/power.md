---
title: 'Xamarin.Essentials: Stan oszczędzanie energii zasilania'
description: Klasa energii umożliwia programowi można uzyskać stanu oszczędzania energii, aby określić, jeśli urządzenie działa w tryb niskiego poboru energii.
ms.assetid: C176D177-8B77-4A9C-9F3B-27852A8DCD5F
author: charlespetzold
ms.author: chape
ms.date: 06/27/2018
ms.openlocfilehash: 760a305280269734034a817182a8c2a07894ca2b
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353493"
---
# <a name="xamarinessentials-power-energy-saver-status"></a>Xamarin.Essentials: Stan oszczędzanie energii zasilania

![NuGet w wersji wstępnej](~/media/shared/pre-release.png)

**Power** klasa udostępnia informacje o stanie oszczędzania energii urządzenia, która wskazuje, czy urządzenie działa w trybie mniejszej mocy. Aplikacje należy unikać przetwarzania w tle, jeśli znajduje się w stanie oszczędzania energii urządzenia.

## <a name="background"></a>Tło

Urządzeń, które korzystają z baterii, można umieścić w trybie oszczędzania energii niskim poziomie zasilania. Czasami urządzenia są przełączane w ten tryb automatycznie, na przykład, gdy baterii spadnie poniżej 20% pojemności. System operacyjny odpowiada tryb oszczędzania energii, zmniejszając działań, które mają na celu wykorzystanie baterii. Aplikacje mogą pomóc, unikając przetwarzania w tle lub innych działań, zadania, gdy tryb oszczędzania energii znajduje się w.

Dla urządzeń z systemem Android **Power** Klasa zwraca istotnych informacji tylko dla systemu Android w wersji 5.0 (Lollipop) lub nowszego.

## <a name="using-the-power-class"></a>Używanie klasy zasilania

Dodaj odwołanie do Xamarin.Essentials w klasie:

```csharp
using Xamarin.Essentials;
```

Uzyskaj bieżący stan oszczędzania energii urządzenia przy użyciu statycznych `Power.EnergySaverStatus` właściwości:

```csharp
// Get energy saver status
var status = Power.EnergySaverStatus;
```

Właściwość ta zwraca element członkowski `EnergySaverStatus` wyliczenia, który może być `On`, `Off`, lub `Unknown`. Jeśli właściwość ta zwraca `On`, aplikacji, należy unikać przetwarzania w tle lub innych działań, które może zajmować dużo mocy.

Aplikację należy także zainstalować program obsługi zdarzeń. **Power** klasa udostępnia zdarzenia, które są wyzwalane, gdy zmieni się stan oszczędzania energii:

```csharp
public class EnergySaverTest
{
    public EnergySaverTest()
    {
        // Subscribe to changes of energy-saver status
        Power.EnergySaverStatusChanged += OnEnergySaverStatusChanged;
    }

    private void OnEnergySaverStatusChanged(EnergySaverStatusChangedEventArgs e)
    {
        // Process change
        var status = e.EnergySaverStatus;
    }
}
```

Jeśli stan oszczędzania energii zmieni się na `On`, aplikacja powinna zostać przerwana w wykonywania przetwarzania w tle. Jeśli stan zmieni się na `Unknown` lub `Off`, aplikacja może wznowić przetwarzania w tle.

## <a name="api"></a>interfejs API

- [Kod źródłowy zasilania](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Power)
- [Dokumentacja interfejsu API usługi Power](xref:Xamarin.Essentials.Power)
