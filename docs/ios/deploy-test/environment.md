---
title: "Środowisko"
ms.topic: article
ms.prod: xamarin
ms.assetid: 67BFD4E1-276C-4B9F-9BD8-A5218D2BD529
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: ef5cfa2a9eee0d5d01922e5b7b3f89a209396c56
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
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

