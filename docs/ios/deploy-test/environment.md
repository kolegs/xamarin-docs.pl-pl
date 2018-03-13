---
title: "Środowisko"
ms.topic: article
ms.prod: xamarin
ms.assetid: 9801644A-89BB-4491-AD28-7F3B97D2CD62
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 7489c2fe38e8433811c5f298296baebacf1c0727
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/12/2018
---
# <a name="environment"></a>Środowisko

*Środowiska wykonawczego* zestaw zmiennych środowiskowych, które wpływają na wykonanie programu. Zmienne środowiskowe można ustawić tymczasowo we właściwościach projektu lub trwale przez określenie dodatkowych argumentów do narzędzia do tworzenia pakietów mtouch.

## <a name="temporary-environment-variables"></a>Zmienne środowiskowe tymczasowego

Zmienne środowiskowe tymczasowe są ustawione w projekcie **właściwości**/**opcje** okna w **Uruchom > Ogólne** sekcji. Te zmienne środowiskowe obowiązują tylko podczas uruchamiania aplikacji przy użyciu programu Visual Studio dla komputerów Mac, jeśli aplikacja jest uruchomiona ręcznie, naciskając pozycję na nim, które te zmienne środowiskowe nie są ustawione.

## <a name="permanent-environment-variables"></a>Zmienne środowiskowe stałych

Zmienne środowiskowe stałe są ustawiane przez określenie dodatkowych argumentów do narzędzia do tworzenia pakietów mtouch. Te zmienne środowiskowe są kompilowane do pliku wykonywalnego i zostanie ustawiona, nawet jeśli aplikacja nie jest uruchamiane w programie Xamarin Studio.

## <a name="example"></a>Przykład

```csharp
# log all exceptions to the device log
--setenv:MONO_TRACE=E:all

# to pass multipe environment variables, use --setenv multiple times
--setenv:MONO_TRACE=E:all --setenv:GC_DONT_GC=1
```

