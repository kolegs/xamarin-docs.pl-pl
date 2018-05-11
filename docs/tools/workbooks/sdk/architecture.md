---
title: Przegląd architektury
ms.prod: xamarin
ms.assetid: 6C0226BE-A0C4-4108-B482-0A903696AB04
author: topgenorth
ms.author: toopge
ms.date: 03/30/2017
ms.openlocfilehash: b80576d3c43340a3c5dc2c6fe1ed20a277f14222
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/09/2018
---
# <a name="architecture-overview"></a>Przegląd architektury

Dwa główne składniki, które muszą działać w połączeniu ze sobą funkcji skoroszyty Xamarin: _agenta_ i _klienta_.

## <a name="interactive-agent"></a>Interakcyjne agenta

Składnik Agent jest mała zestawu specyficzne dla platformy, który jest uruchamiany w kontekście aplikacji .NET.

Skoroszyty Xamarin udostępnia wstępnie zbudowanych aplikacji "pusty" dla wielu platform, takich jak systemu iOS, Android, Mac i WPF. Te aplikacje jawnie host agenta.

Podczas inspekcji na żywo (Xamarin inspektora) agent jest wprowadzone za pomocą debugera IDE do istniejącej aplikacji w ramach regularnych programowanie & debugowania przepływu pracy.

## <a name="interactive-client"></a>Interakcyjne klienta

Klient jest powłoką natywnego (Cocoa dla komputerów Mac, WPF w systemie Windows) obsługującego powierzchni przeglądarki sieci web do prezentowania interfejsu skoroszytu/REPL. Z perspektywy zestawu SDK wszystkie integracji klienta są implementowane w JavaScript i CSS.

Jest on odpowiedzialny (za pośrednictwem Roslyn) kompilowanie kodu źródłowego w małych zestawów i wysyłania ich za pośrednictwem agentowi podłączonej do wykonania. Wyniki działania są wysyłane do klienta w celu renderowania. Każdej komórki w skoroszycie daje jednego zestawu, do którego odwołuje się do zestawu poprzedniej komórki.

Ponieważ agent może być uruchomiony na dowolny typ platformy .NET i ma dostęp do wszystkich elementów w uruchomionej aplikacji, należy uważać, aby serializować wyniki w sposób niezależny od platformy.
