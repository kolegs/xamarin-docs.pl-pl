---
title: Struktury osadzone w rozszerzeniu Xamarin.iOS
description: W tym dokumencie opisano sposób udostępniania kodu struktury osadzone w aplikacji platformy Xamarin.iOS. Można to zrobić przy użyciu narzędzia mtouch lub odwołania natywne.
ms.prod: xamarin
ms.assetid: F8C61020-4106-46F1-AECB-B56C909F42CB
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/05/2018
ms.openlocfilehash: cce5356fd1d3d9a5cf16370a4843c3541b00a7c0
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351437"
---
# <a name="embedded-frameworks-in-xamarinios"></a>Struktury osadzone w rozszerzeniu Xamarin.iOS

_W tym dokumencie opisano, jak deweloperzy aplikacji można osadzić platformy użytkownika w swoich aplikacjach._

Z systemem iOS 8.0 Apple umożliwianie Tworzenie osadzonego framework umożliwiające udostępnianie kodu między rozszerzeniami aplikacji i głównej aplikacji w środowisku Xcode.

Xamarin.iOS 9.0 dodaje obsługę używania tych struktury osadzone (utworzonych za pomocą edytora Xcode) w aplikacji platformy Xamarin.iOS. *Będzie ono **nie** istnieje możliwość tworzenia struktury osadzone z dowolnego typu projekty Xamarin.iOS, tylko używać istniejących struktur natywnych (Objective-C).*

Istnieją dwa sposoby korzystania z platformy w rozszerzeniu Xamarin.iOS:

- Przekazać struktury do narzędzia mtouch, dodając następujące dodatkowe argumenty mtouch w projekcie **kompilacja systemu iOS** opcje:

  ```csharp
  --framework:/Path/To/My.Framework
  ```

  Ma to zostać ustawione dla każdej konfiguracji projektu.

- Dodawanie odwołania natywne z menu kontekstowego

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Kliknij prawym przyciskiem myszy na projekt i Przeglądaj, aby dodać odwołania natywne

![](embedded-frameworks-images/xam-native-refs.png "Wybierz pozycję Dodaj odwołania natywne w programie Visual Studio dla komputerów Mac")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Kliknij prawym przyciskiem myszy na projekt i Przeglądaj, aby dodać odwołania natywne

![](embedded-frameworks-images/vs-native-refs.png "Wybierz pozycję Dodaj odwołania natywne w programie Visual Studio")

-----

  Będzie on działać w przypadku wszystkich konfiguracji.

W przyszłych wersjach programu Visual Studio dla komputerów Mac i narzędzia środowiska Xamarin dla programu Visual Studio będzie możliwe korzystanie z platformy z poziomu środowiska IDE (bez ręcznego edytowania plików projektu).

Kilka przykładowych projektów można znaleźć na [github](https://github.com/rolfbjarne/embedded-frameworks)

## <a name="limitations"></a>Ograniczenia

- Struktury osadzone są obsługiwane tylko w [ujednoliconej](~/cross-platform/macios/unified/index.md) projektów.
- Struktury osadzone są obsługiwane tylko w projektach w element docelowy wdrożenia co najmniej z systemem iOS 8.0.
- Jeśli rozszerzenie wymaga platforma osadzonych, w aplikacji kontenera musi także posiadać odniesienia do struktury, w przeciwnym razie ramach nie zostaną uwzględnione w zbiorze aplikacji.

## <a name="the-mono-runtime"></a>Środowisko uruchomieniowe Mono

Wewnętrznie Xamarin.iOS korzysta z tej funkcji, aby połączyć się ze środowiskiem uruchomieniowym Mono jako struktura, zamiast łączenie środowiska uruchomieniowego Mono statycznie do każdego rozszerzenia i aplikacji kontenera.

Jest to wykonywane automatycznie, jeśli aplikacja kontenera jest aplikacją Unified, zawiera rozszerzenia i wdrożenie docelowym jest system iOS 8.0 lub nowszej.

Aplikacje bez rozszerzeń będą nadal łączyć się z środowiska uruchomieniowego Mono statycznie, ponieważ zwłoka rozmiar dla za pomocą platformy, jeśli istnieje tylko jedna aplikacja odwoływania się do niego.

To zachowanie może zostać przesłonięta przez dewelopera aplikacji, przez dodanie poniższego jako argumentu mtouch dodatkowe opcje kompilacji systemu iOS projektu:

- `--mono:static`: Łączy statycznie ze środowiskiem uruchomieniowym Mono.
- `--mono:framework`: Łącza za pomocą środowiska uruchomieniowego Mono jako struktura.

Jeden scenariusz łączenia ze środowiskiem uruchomieniowym Mono jako struktura nawet w przypadku aplikacji bez rozszerzenia jest zmniejszenie rozmiaru pliku wykonywalnego, rozwiązywania ograniczenia dotyczące rozmiaru Apple wymusza plik wykonywalny. Odwołanie środowisko uruchomieniowe Mono dodaje około 1.7MB na architekturze (zgodnie z platformy Xamarin.iOS 8.12, jednak jego różni się między wersjami, a nawet między aplikacjami). Mono framework dodaje około 2.3MB na architekturę, co oznacza, że architektura pojedynczej aplikacji bez jakichkolwiek rozszerzeń, dzięki czemu link do aplikacji za pomocą środowiska uruchomieniowego Mono jako platforma zmniejszania pliku wykonywalnego przez ~1.7MB, ale Dodaj ~2.3MB framework, wynikowa w ~0.6MB większe aplikacji razem.

