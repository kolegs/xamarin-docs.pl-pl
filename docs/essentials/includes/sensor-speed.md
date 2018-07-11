---
ms.topic: include
ms.openlocfilehash: 5c11c94956f8d56c66c50a9a480177c5b77c2643
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37947422"
---
## <a name="sensor-speedxrefxamarinessentialssensorspeed"></a>[Szybkość czujnika](xref:Xamarin.Essentials.SensorSpeed)

- **Najszybszy** — pobieranie danych z czujników tak szybko, jak to możliwe (nie musi zwracać w wątku interfejsu użytkownika).
- **Gra** — oceń odpowiednie dla gier (nie musi zwracać w wątku interfejsu użytkownika).
- **Normalny** — szybkość domyślna nadające się do zmiany orientacji ekranu.
- **Interfejs użytkownika** — oceń nadające się do interfejsu użytkownika ogólne.

Jeśli obsługi zdarzenia nie musi działać w wątku interfejsu użytkownika, a jeśli program obsługi zdarzeń musi uzyskać dostęp do elementów interfejsu użytkownika, użyj [ `MainThread.BeginInvokeOnMainThread` ](~/essentials/main-thread.md) metodę, aby uruchomić ten kod w wątku interfejsu użytkownika.