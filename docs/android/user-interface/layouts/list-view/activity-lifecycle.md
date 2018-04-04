---
title: ListView i cyklem życia działania
ms.prod: xamarin
ms.assetid: 40840D03-6074-30A2-74DA-3664703E3367
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 6e15fb8796ae6a616c5eae44059caae3d9478aef
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="listview-and-the-activity-lifecycle"></a>ListView i cyklem życia działania

Działania przejść przez określone stany jako sekwencji aplikacji, takich jak uruchamianie, uruchomiona, jest wstrzymana i zostanie zatrzymany. Aby uzyskać więcej informacji i określonych wytyczne dotyczące obsługi przejścia stanu, zobacz [samouczek cyklu życia działania](~/android/app-fundamentals/activity-lifecycle/index.md).
Ważne jest, aby zrozumieć działanie cykl życia i miejsca z `ListView` kodu w odpowiednich lokalizacjach.

Wszystkie przykłady w tym dokumencie spełniają "zadania instalacji" w działaniu `OnCreate` — metoda (w razie potrzeby) wykonaj "usuwania" w `OnDestroy`. W przykładach użyto zazwyczaj małych zestawów danych, które nie ulegają zmianie, więc ponownie częściej ładowania danych nie jest konieczne.

Jednak jeśli w danych zmienia się często i używa dużej ilości pamięci może być przydatne na potrzeby cyklu życia różnych metod wypełniania, a następnie Odśwież Twojej `ListView`. Na przykład, jeśli w danych źródłowych jest ciągle zmieniana (lub wpływ aktualizacje dla innych działań), a następnie utworzenie karty w `OnStart` lub `OnResume` zapewni najnowszych danych jest wyświetlany za każdym razem działanie jest wyświetlany.

Jeśli karta korzysta z zasobów, takich jak pamięci lub zarządzanego kursora, pamiętaj, aby zwolnienie tych zasobów w metodzie uzupełniające których one zostały utworzone (np.) obiekty utworzone w `OnStart` mogą być usuwane w `OnStop`).


## <a name="configuration-changes"></a>Zmiany konfiguracji

Ważne jest, aby Pamiętaj zmiany tej konfiguracji &ndash; szczególnie ekranu widoczność obracanie i klawiatury &ndash; może spowodować, że bieżące działanie zniszczony i utworzony ponownie (chyba że zostanie określony, w przeciwnym razie przy użyciu `ConfigurationChanges` atrybut). Oznacza to, że w normalnych warunkach, obracanie urządzenia spowoduje `ListView` i `Adapter` można ponownie utworzyć i (chyba że kodu napisanego w `OnPause` i `OnResume`) zaznaczenie pozycji i wiersz przewijania stany zostaną utracone.

Następujący atrybut uniemożliwi działanie zniszczone i ponownie utworzone w wyniku zmiany w konfiguracji:

```csharp
[Activity(ConfigurationChanges="keyboardHidden|orientation")]
```

Następnie należy zastąpić działania `OnConfigurationChanged` odpowiednio odpowiadać na te zmiany. Aby uzyskać więcej informacji na temat sposobu obsługi zmian konfiguracji w dokumentacji.

