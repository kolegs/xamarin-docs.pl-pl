---
title: "Gdzie składniki są przechowywane na komputerze?"
ms.topic: article
ms.prod: xamarin
ms.assetid: 5EBB49EE-39E5-428B-866F-9FC1BB215B31
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: 9549363d7aeb5ff7a5cfa79eb38443aaa80b7019
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="where-are-the-components-stored-on-my-machine"></a>Gdzie składniki są przechowywane na komputerze?

Podczas instalowania składników Xamarin do projektu aplikacji, pobiera umieszczone w dwóch miejscach:

1. W folderze składników na poziomie głównym folderu rozwiązania. Usunięcie składnika z wszystkich projektów w rozwiązaniu, Pobierz zostanie usunięty z tego folderu, a także.

2. Kopia również jest przechowywana w następujących lokalizacjach:
    - System Windows: `%LocalAppData%\Xamarin\Cache\Components`
    - Mac: `~/Library/Caches/Xamarin/Components`

Tak aby całkowicie usunąć składnika z systemu, usuń go z projektów lub rozwiązania i powyżej folderu pamięci podręcznej.
