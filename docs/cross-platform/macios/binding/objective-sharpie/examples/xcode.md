---
title: Przykład rzeczywistych za pomocą projektu Xcode
description: Tym dokumencie opisano sposób używania projektu Xcode jako bezpośrednie dane wejściowe do Objective Sharpie, co upraszcza proces tworzenia powiązania C# na kod języka Objective-C.
ms.prod: xamarin
ms.assetid: 168AA64C-E181-4937-A1F2-AD095B9A36F2
author: asb3993
ms.author: amburns
ms.date: 01/15/2016
ms.openlocfilehash: 05c55dc7cd20de2d216d1f267ea5a73631748a0a
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37855250"
---
# <a name="real-world-example-using-an-xcode-project"></a>Przykład rzeczywistych za pomocą projektu Xcode

**W tym przykładzie użyto [POP biblioteki z usługi Facebook](https://github.com/facebook/pop).**

Nowość w wersji 3.0 Objective Sharpie obsługuje projekty Xcode jako dane wejściowe. Te projekty Określ pliki nagłówkowe poprawne i flagi kompilatora niezbędne do skompilowania natywną bibliotekę, a w związku z tym konieczne powiązać go za. Narzędzie Objective Sharpie wybierze pierwszy _docelowej_ i pozostawiać domyślnej konfiguracji projektu, gdy nie zalecił inaczej.

Zanim Objective Sharpie próbuje przeanalizować plików projektu i nagłówek, jego jego tworzenia. Projekt ma często fazy kompilacji, które będą poprawnie struktury pliki nagłówkowe dla zewnętrznego korzystania i integracji, dlatego najlepiej zawsze Kompiluj pełny projekt przed podjęciem próby powiązać go.

<pre>$ <b>git clone https://github.com/facebook/pop.git</b>
Cloning into 'pop'...
   <em>(more git clone output)</em>

$ <b>cd pop</b>
$ <b>sharpie bind pop.xcodeproj -sdk iphoneos9.0</b></pre>

## <a name="related-links"></a>Linki pokrewne

- [Usługa Xamarin University kurs: Tworzenie biblioteki powiązań języka Objective-C](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Usługa Xamarin University kurs: Tworzenie biblioteki powiązań języka Objective-C za pomocą narzędzie Objective Sharpie](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)