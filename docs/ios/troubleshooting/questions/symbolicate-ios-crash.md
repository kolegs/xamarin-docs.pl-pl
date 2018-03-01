---
title: "Gdzie można znaleźć pliku .dSYM symbolicate dzienniki awarii systemu iOS?"
ms.topic: article
ms.prod: xamarin
ms.assetid: CB8607B9-FFDA-4617-8210-8E43EC512588
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 223e1977bb6229760d6428fdca2f5372b1e25c23
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="where-can-i-find-the-dsym-file-to-symbolicate-ios-crash-logs"></a>Gdzie można znaleźć pliku .dSYM symbolicate dzienniki awarii systemu iOS?

Podczas tworzenia aplikacji systemu iOS w programie visual studio, plik .dSYM, który może służyć do symbolicate raporty awarii kończy się na hoście kompilacji w ścieżce:
```
    /Users/<username>/Library/Caches/Xamarin/mtbs/builds/<appname>/<guid>/bin/iPhone/<configuration>
```

Należy pamiętać, że `~/Library` folder jest ukryty. Domyślnie w wyszukiwanie, tak aby w przypadku należy użyć wyszukiwania w **Przejdź > Przejdź do folderu** menu, a następnie wprowadź: `~/Library/Caches/Xamarin/mtbs/builds/` można otworzyć folderu.  

Alternatywnie możesz odkryć `~/Library` folder przy użyciu **Pokaż opcje wyświetlania** panelu folderu macierzystego. Po wybraniu folderu macierzystego na pasku bocznym w wyszukiwanie i użyj menu wyszukiwania **Widok > Pokaż opcje wyświetlania** (lub cmd j), a następnie pojawi się pole wyboru, aby **Pokaż Folder biblioteki**.


### <a name="see-also"></a>Zobacz też
- Rozszerzona procedura symbolicating iOS awarii raportów: [http://jmillerdev.net/symbolicating-ios-crash-files-xamarin-ios/](http://jmillerdev.net/symbolicating-ios-crash-files-xamarin-ios/)
- [Demystifying dzienniki awarii aplikacji systemu iOS](https://www.raywenderlich.com/23704/demystifying-ios-application-crash-logs)
