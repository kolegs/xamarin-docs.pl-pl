---
title: "Ustawienia projektu, które są wymagane dla debugera?"
ms.topic: article
ms.prod: xamarin
ms.assetid: 3A024E4E-ACA3-4C7A-ADEF-541665D15779
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: 6097dc8dfdff8807137ef68be86a08e4c9e23988
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="what-project-settings-are-required-for-the-debugger"></a>Ustawienia projektu, które są wymagane dla debugera?

Aby debuger mógł działać zgodnie z oczekiwaniami (trafień punktów przerwania, wyświetlanie dzienników debugowania itd.) wyświetlane informacje instrumentacja i debugowania dewelopera należy zarówno włączyć.

Wykonaj następujące czynności, aby sprawdzić ustawienia środowiska:

## <a name="visual-studio"></a>Visual Studio
1. Otwórz opcje projektu
2. Przejdź do **kompilacji > Zaawansowane...** Ustaw informacje o debugowaniu **pełny**
3. Ustawienia dla każdej platformy:
   - Przejdź do **Android Opcje > debugowanie opcje**. Znaczników **włączyć Instrumentacji Developer** pole.
   - Przejdź do **kompilacji systemu iOS > Opcje debugowania**. Znaczników **włączyć debugowanie** pole.

## <a name="visual-studio-for-mac"></a>Visual Studio for Mac
1. Otwórz opcje projektu
2. Przejdź do **kompilacji > kompilatora > Opcje ogólne**. Ustaw informacje o debugowaniu **pełny**
3. Ustawienia dla każdej platformy:
  - Przejdź do **kompilacji > kompilacji Android > opcji debugowania**. Znaczników **włączyć Instrumentacji Developer** pole.
  - Przejdź do **kompilacji > iOS debugowania**. Znaczników **włączyć debugowanie** pole.

