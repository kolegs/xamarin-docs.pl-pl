---
title: Rozwiązywanie problemów z profilera Xamarin
description: Rozwiązywanie problemów z profilera Xamarin
ms.prod: xamarin
ms.assetid: 0060E9D1-C003-4E4C-ADE8-B406978FE891
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 10/27/2017
ms.openlocfilehash: 4844c999ceddcee89d4f45f6e41dd4c7f2caf054
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="xamarin-profiler-troubleshooting"></a>Rozwiązywanie problemów z profilera Xamarin

_Rozwiązywanie problemów z profilera Xamarin_

## <a name="logging-and-diagnostics"></a>Rejestrowania i diagnostyki

Zespół Xamarin może pomóc śledzenia problemów, jeśli Podaj nam informacje, w tym:

- Screencast problem, awarii, lub awarii i doprowadziły do niego przepływ pracy.
- Dziennik danych wyjściowych (patrz poniżej).
- **.Mlpd** generowany dla sesji profilowania (patrz poniżej).

### <a name="getting-log-outputs"></a>Pobieranie danych wyjściowych dziennika
W systemie Mac dzienniki są zapisywane w `~/Library/Logs/Xamarin.Profiler/Profiler.<date>.log`.

W systemie Windows są one zapisane na `%appdata%Local//Xamarin/Log/Xamarin.Profiler/Profiler.<date>.log` Dołącz najnowsze dziennika zawsze, gdy przesłać problemu.

Trwa dodawanie więcej rejestrowanie zgodnie z rozszerzana, dzięki czemu te dane wyjściowe powinny powiększania i staną się bardziej użyteczna w czasie.

<a name="gen_mlpd" />

### <a name="generating-mlpd-files"></a>Generowanie plików .mlpd

**.Mlpd** plik jest skompresowane dane wyjściowe mono środowiska uruchomieniowego profilera. Profiler Xamarin graficznego interfejsu użytkownika odczytywać dane z **.mlpd** i wyświetla je dla użytkownika. **.mlpd** pliki są przydatne narzędzia debugowania xamarin, ponieważ pomagają naszych inżynierów diagnozowanie problemów profilera ewentualnych problemów z danymi.

**.Mlpd** dla bieżącej sesji są automatycznie zapisywane w komputerze Mac `/tmp` katalogu i może zostać zidentyfikowane na podstawie sygnatury czasowej. Po włączeniu rejestrowania pierwsze dane wyjściowe będą mieć ścieżkę do **.mlpd** pliku. **.Mlpd** zazwyczaj plik zostanie zapisany w katalogu uruchamiania ~/var/folderów...

**.Mlpd** dla bieżącej sesji można zapisywać przez wybranie **Plik > Zapisz jako...** z menu profilera:

**Visual Studio for Mac**:

![](troubleshooting-images/image17.png "Zapisywanie pliku .mlpd w programie Visual Studio dla komputerów Mac")

**Visual Studio**:

![](troubleshooting-images/image17-vs.png "Zapisywanie pliku .mlpd w programie Visual Studio")


Należy zauważyć, że **.mlpd** zawiera wiele informacji i będzie duży rozmiar pliku.

## <a name="troubleshooting"></a>Rozwiązywanie problemów

Poniższa lista zawiera typowe pytań, obejścia i porady i wskazówki dotyczące korzystania z profilera.

> [!NOTE]
> **Uwaga**: użytkownik musi być Visual Studio **Enterprise** subskrybenta, aby odblokować tę funkcję w albo program Visual Studio Enterprise w systemie Windows lub programu Visual Studio dla komputerów Mac.

#### <a name="i-cant-see-the-ios-profiler-option-or-it-is-greyed-out-visual-studio-and-visual-studio-for-mac"></a>Nie widzę opcji profilera z systemem iOS lub [Visual Studio i Visual Studio for Mac] nieaktywna

Sprawdź następujące ustawienia, aby rozwiązać ten problem:

- Upewnij się, że korzystasz z konfiguracji debugowania
- Upewnij się, że używasz moduł zbierający elementy bezużyteczne SGen.
- Upewnij się, jest to platforma [obsługiwane](~/tools/profiler/index.md#Profiler_Support).
- Upewnij się, że masz prawa licencji.
- Upewnij się, że użytkownik jest zalogowany w i prawidłowo uwierzytelnione.
- [Visual Studio] Należy używać [Visual Studio Enterprise](https://www.visualstudio.com/vs/enterprise/) i mieć ważną licencję Enterprise.


#### <a name="i-get-an-error-when-i-try-to-launch-the-profiler"></a>Błąd podczas próby uruchomienia profilera

Jeśli zostanie uruchomione w tym polu błędu przy użyciu profilera programu Visual Studio:

![](troubleshooting-images/error.png "Pole błędu przy użyciu profilera programu Visual Studio")

Jest zwykle z powodu nie można uruchomić na symulatorze / emulatora. Spróbuj i zwykle uruchamianie aplikacji, rozwiąż problemy, które zapewnia, a następnie spróbuj ponownie użyć profilera.

#### <a name="to-watch-a-specific-thread"></a>Aby obejrzeć określonego wątku

Jeśli masz wątku, który chcesz obejrzeć w szczególności byłoby idealne nazwę wątek na bardzo od jego tworzenia, tak aby uzyskać get `ThreadName` zamiast `0x0`. Na przykład można ustawić nazwy wątku jako interfejsu użytkownika można użyć poniższego kodu:


```csharp
RunOnUiThread (() => {
  Thread.CurrentThread.Name  = "UI";
});
```



## <a name="related-links"></a>Linki pokrewne

- [Wskazówki — za pomocą programu Xamarin](~/tools/profiler/index.md)
- [Dobre praktyki dotyczące pamięci i wydajność](~/cross-platform/deploy-test/memory-perf-best-practices.md)
- [Uwagi do wersji](https://developer.xamarin.com/releases/profiler/preview/)
