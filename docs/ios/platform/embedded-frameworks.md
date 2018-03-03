---
title: Struktury osadzone
description: "W tym dokumencie opisano, jak deweloperzy aplikacji może mieć osadzone struktury użytkownika w swoich aplikacjach."
ms.topic: article
ms.prod: xamarin
ms.assetid: F8C61020-4106-46F1-AECB-B56C909F42CB
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 62ddf665431a14ce7f4fe8db52cc6ee7a2c4635a
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="embedded-frameworks"></a>Struktury osadzone

_W tym dokumencie opisano, jak deweloperzy aplikacji może mieć osadzone struktury użytkownika w swoich aplikacjach._

Z systemem iOS 8.0 Apple możliwe jej do utworzenia osadzonych platformę, by współużytkowanie kodu rozszerzeń aplikacji i głównej aplikacji w środowisku Xcode.

Xamarin.iOS 9.0 dodaje obsługę korzystających z tych platform embedded (utworzone w programie Xcode) w aplikacji platformy Xamarin.iOS. *Będzie on **nie** możliwe jest tworzenie struktury osadzone z dowolnego typu Xamarin.iOS projektów, tylko używać istniejących natywnych struktur (Objective-C).*

Istnieją dwa sposoby korzystania platform w Xamarin.iOS:

- Przekazania platformę do narzędzia mtouch przez dodanie poniższego do mtouch dodatkowych argumentów w projekcie **kompilacji systemu iOS** opcje:

  ```csharp
  --framework:/Path/To/My.Framework
  ```

  Musi to być ustawiona dla każdej konfiguracji projektu.

- Dodaj odwołania do natywnej z menu kontekstowego

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Kliknij prawym przyciskiem myszy projekt i Przeglądaj, aby dodać odwołanie do natywnego

![](embedded-frameworks-images/xam-native-refs.png "Wybierz opcję Dodaj natywnego odwołań w programie Visual Studio dla komputerów Mac")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Kliknij prawym przyciskiem myszy projekt i Przeglądaj, aby dodać odwołanie do natywnego

![](embedded-frameworks-images/vs-native-refs.png "Wybierz opcję Dodaj natywnego odwołań w programie Visual Studio")

-----

  To będzie działać w przypadku wszystkich konfiguracji.

W przyszłych wersjach programu Visual Studio dla komputerów Mac i narzędzia platformy Xamarin dla Visual Studio będzie możliwe użycie platform z w środowisku IDE (bez ręcznie edytować pliki projektu).

Kilka przykładowych projektach można znaleźć w [github](https://github.com/rolfbjarne/embedded-frameworks)

## <a name="limitations"></a>Ograniczenia

- Struktury osadzone są obsługiwane tylko w [Unified](~/cross-platform/macios/unified/index.md) projektów.
- Struktury osadzone są obsługiwane tylko w projektach z elementem docelowym wdrożenia co najmniej system iOS 8.0.
- Jeśli rozszerzenie wymaga osadzone struktury aplikacji kontenera musi mieć również odwołanie do struktury, w przeciwnym razie platformę nie zostaną uwzględnione w pakiecie aplikacji.

## <a name="the-mono-runtime"></a>Mono środowiska wykonawczego

Wewnętrznie Xamarin.iOS korzysta z tej funkcji, aby połączyć się ze środowiskiem uruchomieniowym Mono jako platforma, zamiast łączenie Mono środowiska uruchomieniowego statycznie do każdego rozszerzenia i aplikacji kontenera.

Odbywa się automatycznie, jeśli kontener jest aplikacja Unified, zawiera rozszerzenia i wdrożenie docelowym jest system iOS 8.0 lub nowszej.

Aplikacje bez rozszerzenia będą nadal łączyć się z Mono środowiska uruchomieniowego statycznie, ponieważ zwłoka rozmiar dla przy użyciu platformy, jeśli istnieje tylko jedna aplikacja odwołuje.

To zachowanie może być zastąpiona przez dewelopera aplikacji przez dodanie poniższego jako argumentu dodatkowe mtouch projektu iOS opcje kompilacji:

- `--mono:static`: Połączenie ze środowiskiem uruchomieniowym Mono statycznie.
- `--mono:framework`: Łącza ze środowiskiem uruchomieniowym Mono jako struktury.

Jednym ze scenariuszy łączenia ze środowiskiem uruchomieniowym Mono jako platforma nawet w przypadku aplikacji bez rozszerzenia jest zmniejszenie rozmiaru pliku wykonywalnego, w celu wyeliminowania żadne ograniczenia rozmiaru, który wymusza Apple plik wykonywalny. Odwołania Mono środowiska uruchomieniowego dodaje około 1.7MB na architektura (zgodnie z Xamarin.iOS 8.12, jednak jego między wersjami, a nawet między aplikacjami). Mono framework dodaje około 2.3MB na architektura, co oznacza, że architektura pojedynczej aplikacji bez wszystkich rozszerzeń, co link do aplikacji ze środowiskiem uruchomieniowym Mono struktury zmniejszania pliku wykonywalnego przez ~1.7MB, ale Dodaj ~2.3MB framework, co w ~0.6MB większy aplikacji razem.

