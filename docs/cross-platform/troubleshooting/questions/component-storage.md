---
title: Gdzie składniki są przechowywane na komputerze?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5EBB49EE-39E5-428B-866F-9FC1BB215B31
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: f0dad6e6219d373eaa9f8410aea7d96c81eceb6b
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="where-are-the-components-stored-on-my-machine"></a>Gdzie składniki są przechowywane na komputerze?

Podczas instalowania składników Xamarin do projektu aplikacji, pobiera umieszczone w dwóch miejscach:

1. W folderze składników na poziomie głównym folderu rozwiązania. Usunięcie składnika z wszystkich projektów w rozwiązaniu, Pobierz zostanie usunięty z tego folderu, a także.

2. Kopia również jest przechowywana w następujących lokalizacjach:
    - System Windows: `%LocalAppData%\Xamarin\Cache\Components`
    - Mac: `~/Library/Caches/Xamarin/Components`

Tak aby całkowicie usunąć składnika z systemu, usuń go z projektów lub rozwiązania i powyżej folderu pamięci podręcznej.
