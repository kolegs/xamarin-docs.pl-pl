---
title: "Środowisko"
ms.topic: article
ms.prod: xamarin
ms.assetid: 9801644A-89BB-4491-AD28-7F3B97D2CD62
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: c46d3aa151d4a596ca3da881a3c4e5e38bb41cbd
ms.sourcegitcommit: d450ae06065d8f8c80f3588bc5a614cfd97b5a67
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/21/2018
---
# <a name="environment"></a>Środowisko

*Środowiska wykonawczego* zestaw zmiennych środowiskowych, które wpływają na wykonanie programu. Zmienne środowiskowe można ustawić tymczasowo we właściwościach projektu lub trwale przez określenie dodatkowych argumentów do narzędzia do tworzenia pakietów mtouch.

## <a name="temporary-environment-variables"></a>Zmienne środowiskowe tymczasowego

Zmienne środowiskowe tymczasowe są ustawione w projekcie **właściwości**/**opcje** okna w **Uruchom > Ogólne** sekcji. Te zmienne środowiskowe obowiązują tylko podczas uruchamiania aplikacji przy użyciu programu Visual Studio dla komputerów Mac, jeśli aplikacja jest uruchomiona ręcznie, naciskając pozycję na nim, które te zmienne środowiskowe nie są ustawione.

## <a name="permanent-environment-variables"></a>Zmienne środowiskowe stałych

Zmienne środowiskowe stałe są ustawiane przez określenie dodatkowych argumentów do narzędzia do tworzenia pakietów mtouch. Te zmienne środowiskowe są kompilowane do pliku wykonywalnego i zostanie ustawiona, nawet jeśli nie uruchomiono aplikację z programu Visual Studio dla komputerów Mac.

## <a name="example"></a>Przykład

```csharp
# log all exceptions to the device log
--setenv:MONO_TRACE=E:all

# to pass multipe environment variables, use --setenv multiple times
--setenv:MONO_TRACE=E:all --setenv:GC_DONT_GC=1
```

