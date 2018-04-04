---
title: Znak o identyfikatorze Developer
description: Ten przewodnik przeprowadzi Cię przez podpisywania aplikacji Xamarin.Mac o identyfikatorze Developer dla publikacji.
ms.prod: xamarin
ms.assetid: cf7b733b-e08f-4f56-a233-264b29ee4c97
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 1a2726ec46ac51ae9848b318798afba74183360c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="sign-with-developer-id"></a>Znak o identyfikatorze Developer

Jeśli plany dewelopera dystrybucję aplikacji bezpośrednio do użytkowników macOS, Apple zaleca, aby ich podpisać za pomocą jego Identyfikatora Developer, którego nie można zainstalować na systemy macOS z **strażnika** włączone. Jeśli aplikacja nie została podpisana, **strażnika** uniemożliwi użytkownikom instalowanie za pomocą ostrzeżenie (one można obejść to ograniczenie, przytrzymując klawisz Control podczas uruchamiania).

Przeczytaj więcej na temat [identyfikator deweloperów i strażnika](https://developer.apple.com/resources/developer-id/) i [dystrybucja poza Mac App Store](https://developer.apple.com/library/content/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html) w witrynie sieci Web firmy Apple.

## <a name="code-signing-options"></a>Opcje podpisywania kodu

Do tworzenia aplikacji na potrzeby wdrożenia bezpośrednio do zbioru użytkowników (nie za pośrednictwem sklepu Mac App Store) **ustawień podpisywania** do używania **Identyfikator dewelopera**. Upewnij się, aby edytować **wersji** konfiguracji.

 [![](signing-images/config02.png "Opcje podpisywania Mac")](signing-images/config02.png#lightbox)


## <a name="build"></a>Kompilacja

Przed rozpoczęciem budowania, upewnij się aby wybrane prawidłowej konfiguracji i wybierz, aby utworzyć pakiet instalacji w **kompilacji Mac** ustawienia:

[![](signing-images/config03.png "Opcje kompilacji")](signing-images/config03.png#lightbox)

Podczas tworzenia aplikacji, deweloper pojawi się monit korzystanie z obu certyfikatów:

 [![](signing-images/image57.png "Zezwalanie na dostęp łańcucha kluczy")](signing-images/image57.png#lightbox)

 [![](signing-images/image58.png "Zezwalanie na dostęp łańcucha kluczy")](signing-images/image58.png#lightbox)

Po Po skompilowaniu aplikacji deweloper może kliknij prawym przyciskiem myszy projekt i wybierz polecenie **Otwórz Folder zawierający** można znaleźć pliku pakietu (w `bin/Release` katalogu). Ten plik zawiera Instalator dla aplikacji, mogą zostać przekazane z żadnym użytkownikiem macOS dla instalacji.

 [![](signing-images/image59.png "Wybieranie pakietu aplikacji w wyszukiwanie")](signing-images/image59.png#lightbox)

## <a name="related-links"></a>Linki pokrewne

- [Instalacja](~//mac/get-started/installation.md)
- [Witaj, przykładowe Mac](~//mac/get-started/hello-mac.md)
- [Dystrybucję aplikacji ze sklepu Mac App Store](https://developer.apple.com/devcenter/mac/checklist/)
- [Przewodnik narzędzi: Podpisywania aplikacji kodu](https://developer.apple.com/library/mac/#documentation/ToolsLanguages/Conceptual/OSXWorkflowGuide/CodeSigning/CodeSigning.html)
- [Identyfikator deweloperów i strażnika](https://developer.apple.com/resources/developer-id/)
