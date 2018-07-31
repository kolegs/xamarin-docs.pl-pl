---
title: Techniki uruchamianie procesów w tle dla systemu iOS
description: 'W tym dokumencie, który stanowi łącze do przewodników, które opisują różne techniki backgrounding w systemie iOS: zadania w tle, Usługa transferu w tle, pobieranie w tle i zdalne powiadomienia.'
ms.prod: xamarin
ms.assetid: 011A8D48-1CDC-486A-A2B0-C4946118E7A9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 33c4613e3698556b41a0d01acf2a4ac359ca5dd8
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350972"
---
# <a name="ios-backgrounding-techniques"></a>Techniki uruchamianie procesów w tle dla systemu iOS

W poniższych sekcjach omówimy następujące funkcje systemu iOS obok istniejących opcji backgrounding:

-  [Zadania w tle oportunistyczne](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/ios-backgrounding-with-tasks.md#background_tasks_in_iOS_7) -oszczędzać czas pracy baterii, uruchamiając we fragmentach oportunistyczne zadania w tle, gdy urządzenie jest w stanie aktywności, aby uzyskać inne procesy przetwarzania.
-  [Usługa transferu w tle](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/ios-backgrounding-with-tasks.md#background-transfers) — w niezawodny sposób będą przekazywanie i pobieranie plików, niezależnie od rozmiaru sieci stanu lub pliku.
-  [Pobieranie w tle](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/updating-an-application-in-the-background.md#background_fetch) -Odśwież aplikację z tła odstępach określany przez system.
-  [Zdalne powiadomienia](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/updating-an-application-in-the-background.md#remote_notifications) — użycie powiadomień wypychanych do wyzwalania aktualizacji zawartości w tle, zanim użytkownik otwiera aplikację, z opcją powiadomić użytkownika lub zaktualizować w trybie dyskretnym.
-  **Aktualizacje interfejsu użytkownika w tle** — przygotowywanie aplikacji interfejsu użytkownika dla użytkownika i zaktualizuj migawkę aplikacji, wszystko w tle.
