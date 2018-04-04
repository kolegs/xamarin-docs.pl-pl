---
title: Techniki Backgrounding systemu iOS
ms.prod: xamarin
ms.assetid: 011A8D48-1CDC-486A-A2B0-C4946118E7A9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 261507e8cbca8e94f5cabbb010dcd444c432d96c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="ios-backgrounding-techniques"></a>Techniki Backgrounding systemu iOS

W poniższych sekcjach przeanalizujemy następujące funkcje systemu iOS będą widoczne obok istniejących opcji backgrounding:

-  [Zadania w tle oportunistyczne](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/ios-backgrounding-with-tasks.md#background_tasks_in_iOS_7) -oszczędzać czas pracy baterii przez uruchomienie zadania w tle w oportunistyczne fragmentów, gdy urządzenie jest w stanie aktywności dla innych procesów.
-  [Usługa transferu w tle](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/ios-backgrounding-with-tasks.md#background-transfers) — niezawodnie przekazywanie i pobieranie plików niezależnie od rozmiaru sieci stanu lub pliku.
-  [Pobieranie w tle](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/updating-an-application-in-the-background.md#background_fetch) -Odśwież aplikację w tle odstępach określić systemu.
-  [Powiadomienia zdalnego](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/updating-an-application-in-the-background.md#remote_notifications) — użycie powiadomień wypychanych, aby wyzwolić aktualizacje zawartości w tle, zanim użytkownik otwiera aplikację, z opcją powiadamia użytkownika lub zaktualizować w trybie dyskretnym.
-  **Aktualizacje interfejsu użytkownika w tle** — przygotowanie interfejsu użytkownika dla użytkownika i zaktualizuj migawkę aplikacji, wszystko w tle.
