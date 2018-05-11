---
title: Powiązanie natywnego struktur
ms.prod: xamarin
ms.assetid: 91AE058A-3A1F-41A9-9DE4-4B96880A1869
author: asb3993
ms.author: amburns
ms.date: 01/15/2016
ms.openlocfilehash: 219295ad9299b36a763289c4aef8e52cd859ceea
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/10/2018
---
# <a name="binding-native-frameworks"></a>Powiązanie natywnego struktur

Czasami natywnej biblioteki jest rozpowszechniany jako [framework](https://developer.apple.com/library/mac/documentation/MacOSX/Conceptual/BPFrameworks/Concepts/WhatAreFrameworks.html). Sharpie celu zapewnia funkcji dla powiązania poprawnie zdefiniowany struktur za pomocą `-framework` opcji.

Na przykład powiązanie [Adobe Creative SDK Framework](https://creativesdk.adobe.com/downloads.html) dla systemu iOS jest prosty:

<pre>$ <b>sharpie bind \
    -framework AdobeCreativeSDKFoundation.framework \
    -sdk iphoneos8.1</b></pre>

W niektórych przypadkach platforma będzie określać **Info.plist** co oznacza, względem których SDK framework ma być kompilowana. Jeśli te informacje nie jawne i `-sdk` została przekazana opcja, cel Sharpie wywnioskuje go zwrócony przez strukturę **Info.plist** (albo `DTSDKName` klucza lub kombinację `DTPlatformName` i `DTPlatformVersion`kluczy).

`-framework` Opcji nie zezwala na pliki nagłówkowe jawne do przekazania. Plik nagłówka parasola jest wybierany przez Konwencję na podstawie nazwy framework. Jeśli nie można odnaleźć nagłówka parasola, Sharpie cel nie będzie podejmować próby powiązania w ramach i zapewniając parasola poprawne pliki nagłówka do analizy, wraz z żadnych argumentów framework dla programu clang należy dokonać ręcznej powiązania (takie jak `-F`opcję ścieżki wyszukiwania framework).

Pod maską określając `-framework` jest po prostu skrótu. Następujące argumenty powiązania są takie same jak `-framework` skrócona powyżej.
Specjalne znaczenie jest `-F .` ścieżki wyszukiwania framework do clang (należy pamiętać, miejsca i okres, które są wymagane polecenia).

<pre>$ <b>sharpie bind \
    -sdk iphoneos8.1 \
    AdobeCreativeSDKFoundation.framework/Headers/AdobeCreativeSDKFoundation.h \
    -scope AdobeCreativeSDKFoundation.framework/Headers \
    -c -F .</b></pre>

