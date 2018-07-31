---
title: Jakie ustawienia projektu są wymagane dla debugera?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 3A024E4E-ACA3-4C7A-ADEF-541665D15779
author: asb3993
ms.author: amburns
ms.date: 05/08/2018
ms.openlocfilehash: 646ef7f708be2de6a851ace25d69a7c2f0b18a83
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350810"
---
# <a name="what-project-settings-are-required-for-the-debugger"></a>Jakie ustawienia projektu są wymagane dla debugera?

Aby debuger ma działać zgodnie z oczekiwaniami (trafień punktów przerwania, dzienniki debugowania wyświetlania itp.) wyświetlanie informacji o Instrumentacji i debugowania dla deweloperów należy włączyć.

Wykonaj następujące kroki, aby sprawdzić ustawienia środowiska:

## <a name="visual-studio"></a>Visual Studio
1. Otwórz opcje projektu
2. Przejdź do **kompilacji > Zaawansowane...** Ustaw informacje o debugowaniu **pełne**
3. Ustawienia dla każdej platformy:
   - Przejdź do **opcje systemu Android > Opcje debugowania**. Znaczników **Włącz instrumentację dla dewelopera** pole.
   - Przejdź do **kompilacja systemu iOS > Opcje debugowania**. Znaczników **włączyć debugowanie** pole.

## <a name="visual-studio-for-mac"></a>Visual Studio for Mac
1. Otwórz opcje projektu
2. Przejdź do **kompilacji > kompilatora > Opcje ogólne**. Ustaw informacje o debugowaniu **pełne**
3. Ustawienia dla każdej platformy:
  - Przejdź do **kompilacji > kompilacja systemu Android > Opcje debugowania**. Znaczników **Włącz instrumentację dla dewelopera** pole.
  - Przejdź do **kompilacji > debugowanie systemu iOS**. Znaczników **włączyć debugowanie** pole.

