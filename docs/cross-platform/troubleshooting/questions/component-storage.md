---
title: Gdzie są przechowywane składniki na moim komputerze?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5EBB49EE-39E5-428B-866F-9FC1BB215B31
author: asb3993
ms.author: amburns
ms.date: 05/08/2018
ms.openlocfilehash: 4152c8ef7eeba3748d9244e27e48f3f9a2c0019b
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350722"
---
# <a name="where-are-the-components-stored-on-my-machine"></a>Gdzie są przechowywane składniki na moim komputerze?

W celu instalowania składników platformy Xamarin w projekcie aplikacji, pobiera on umieszczony w dwóch miejscach:

1. W folderze składników na poziomie głównym folderu rozwiązania. Możesz usunąć składnik ze wszystkich projektów w rozwiązaniu, będą usuwane z tego folderu, a także.

2. Kopia jest również przechowywana w następujących lokalizacjach:
    - Windows: `%LocalAppData%\Xamarin\Cache\Components`
    - Mac: `~/Library/Caches/Xamarin/Components`

Dlatego aby całkowicie usunąć składnik w systemie, usuń go z projektów/rozwiązań i folderu pamięci podręcznej powyżej.
