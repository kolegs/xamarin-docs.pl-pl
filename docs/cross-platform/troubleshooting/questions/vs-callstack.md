---
title: Jak zebrać stosy wywołań bieżącego procesu programu Visual Studio?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 64c24b09-2c4a-43ad-b94d-6cd05a1aee44
author: asb3993
ms.author: amburns
ms.date: 03/30/2017
ms.openlocfilehash: e81c28f0610a0df2e4fe06349685ef5e0744071a
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/09/2018
ms.locfileid: "33919764"
---
# <a name="how-do-i-collect-the-current-call-stacks-of-the-visual-studio-process"></a>Jak zebrać stosy wywołań bieżącego procesu programu Visual Studio?

Gdy graficznego interfejsu użytkownika blokuje (zawiesza się, zawiesza) w programie Visual Studio, ważną cząstką informacji diagnostycznych, aby zbierać to zbiór stosy wywołań z wszystkie wątki tego procesu programu Visual Studio. Aby zapisać te informacje dotyczące zawieszone wystąpienie programu Visual Studio, możesz użyć drugiego wystąpienia programu Visual Studio:

1. Uruchom drugie wystąpienie programu Visual Studio (nowe okno).

2. Zamknij wszystkie otwarte rozwiązań w nowym wystąpieniu programu Visual Studio.

3. Wybierz **Debuguj > dołączyć do procesu**.

  ![](vs-callstack-images/image1.png "Wybierz polecenie Debuguj > dołączyć do procesu")

4. Wybierz wystąpienie oryginalnego zawieszone `devenv.exe` z listy **dostępne procesy**.

5. Wybierz **Debuguj > Przerwij wszystkie**.

  ![](vs-callstack-images/image2.png "Wybierz polecenie Debuguj > Przerwij wszystkie")

6. Wybierz **Debuguj > Zapisz zrzut jako**.

  ![](vs-callstack-images/image3.png "Wybierz polecenie Debuguj > Zapisz zrzut jako")

7. Zmień **Zapisz jako typ** do **minizrzutu (\*dmp)**. Spowoduje to utworzenie znacznie mniejszy plik niż **minizrzutu ze stertą**, i zazwyczaj stos nie jest odpowiednia do diagnozowania blokuje się.

  ![](vs-callstack-images/image4.png "Spowoduje to utworzenie znacznie mniejszy plik niż minizrzutu ze stertą i zwykle stos nie jest odpowiednia do diagnozowania zawiesza")

8. Zapisz plik zrzutu. Jeśli przesyłanie plików w trybie online, możesz go, aby zmniejszyć rozmiar pliku zip.
