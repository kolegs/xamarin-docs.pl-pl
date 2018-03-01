---
title: "Przykład rzeczywistych przy użyciu projektu Xcode"
ms.topic: article
ms.prod: xamarin
ms.assetid: 168AA64C-E181-4937-A1F2-AD095B9A36F2
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 01/15/2016
ms.openlocfilehash: 34efeb2b505f6076623f22f36c2a48a52d6f399f
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="real-world-example-using-an-xcode-project"></a>Przykład rzeczywistych przy użyciu projektu Xcode


**W tym przykładzie użyto [biblioteki POP z usługi Facebook](https://github.com/facebook/pop).**

Nowość w wersji 3.0 Sharpie cel obsługuje projektów Xcode jako dane wejściowe. Te projekty Określ pliki nagłówkowe poprawne i flagi kompilatora niezbędne do skompilowania natywnej biblioteki i w związku z tym konieczne powiązać zbyt. Sharpie celu wybierze pierwszy _docelowej_ i domyślnej konfiguracji projektu, gdy nie zalecił inaczej.

Przed Sharpie cel próbuje analizy plików projektu i nagłówek, jego jego tworzenia. Projekty często mają fazy kompilacji, które będzie poprawnie struktury pliki nagłówkowe dla zewnętrznych konsumenckich i integracji, dlatego najlepiej zawsze skompilować projekt pełnego przed podjęciem próby powiązać go.

<pre>$ <b>git clone https://github.com/facebook/pop.git</b>
Cloning into 'pop'...
   <em>(more git clone output)</em>

$ <b>cd pop</b>
$ <b>sharpie bind pop.xcodeproj -sdk iphoneos9.0</b></pre>

