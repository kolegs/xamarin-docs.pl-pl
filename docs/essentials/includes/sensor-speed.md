---
ms.topic: include
ms.openlocfilehash: e4dfd1ac12f3010939d483381a785091d71599ed
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353272"
---
## <a name="sensor-speedxrefxamarinessentialssensorspeed"></a>[Szybkość czujnika](xref:Xamarin.Essentials.SensorSpeed)

- **Najszybszy** — pobieranie danych z czujników tak szybko, jak to możliwe (nie musi zwracać w wątku interfejsu użytkownika).
- **Gra** — oceń odpowiednie dla gier (nie musi zwracać w wątku interfejsu użytkownika).
- **Normalny** — szybkość domyślna nadające się do zmiany orientacji ekranu.
- **Interfejs użytkownika** — oceń nadające się do interfejsu użytkownika ogólne.

Jeśli obsługi zdarzenia nie musi działać w wątku interfejsu użytkownika, a jeśli program obsługi zdarzeń musi uzyskać dostęp do elementów interfejsu użytkownika, użyj [ `MainThread.BeginInvokeOnMainThread` ](~/essentials/main-thread.md) metodę, aby uruchomić ten kod w wątku interfejsu użytkownika.