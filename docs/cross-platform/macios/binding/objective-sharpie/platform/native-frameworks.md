---
title: Tworzenie powiązań struktur natywnych
description: W tym dokumencie opisano, jak używać Objective Sharpie użytkownika framework opcję, aby utworzyć powiązanie z biblioteką dystrybuowanych jako struktury.
ms.prod: xamarin
ms.assetid: 91AE058A-3A1F-41A9-9DE4-4B96880A1869
author: asb3993
ms.author: amburns
ms.date: 01/15/2016
ms.openlocfilehash: ca103ee027597813be51e126aaa05f9aa969af35
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37855100"
---
# <a name="binding-native-frameworks"></a>Tworzenie powiązań struktur natywnych

Czasami natywnej biblioteki jest rozpowszechniany jako [framework](https://developer.apple.com/library/mac/documentation/MacOSX/Conceptual/BPFrameworks/Concepts/WhatAreFrameworks.html). Narzędzie Objective Sharpie udostępnia funkcję jako udogodnienie dla powiązania poprawnie zdefiniowana struktury za pomocą `-framework` opcji.

Na przykład powiązanie [Adobe Creative SDK Framework](https://creativesdk.adobe.com/downloads.html) dla systemu iOS jest prosty:

<pre>$ <b>sharpie bind \
    -framework AdobeCreativeSDKFoundation.framework \
    -sdk iphoneos8.1</b></pre>

W niektórych przypadkach platforma określą **Info.plist** co oznacza, względem których zestaw SDK programu framework powinna być skompilowana. Jeśli istnieje te informacje nie jawne i `-sdk` opcji jest przekazywana, Objective Sharpie wywnioskuje go z programu framework **Info.plist** (albo `DTSDKName` klawisz lub kombinację `DTPlatformName` i `DTPlatformVersion`kluczy).

`-framework` Opcji nie zezwala na pliki nagłówkowe jawne do przekazania. Plik nagłówkowy parasola jest wybierany zgodnie z Konwencją, w oparciu o nazwę framework. Jeśli nie można odnaleźć nagłówka parasola, Objective Sharpie nie podejmie próby Powiąż platformę i należy ręcznie wykonać powiązanie, podając pliki nagłówka poprawne parasola można przeanalizować, oraz wszelkie argumenty framework clang (takie jak `-F`opcji ścieżkę wyszukiwania struktury).

Kulisy określając `-framework` jest po prostu skrótem. Następujące argumenty powiązania są takie same jak `-framework` skrót powyżej.
Szczególne znaczenie jest `-F .` ścieżkę wyszukiwania struktury udostępnionego clang (Zwróć uwagę, miejsca i czasu, które nie są wymagane jako część polecenia).

<pre>$ <b>sharpie bind \
    -sdk iphoneos8.1 \
    AdobeCreativeSDKFoundation.framework/Headers/AdobeCreativeSDKFoundation.h \
    -scope AdobeCreativeSDKFoundation.framework/Headers \
    -c -F .</b></pre>

## <a name="related-links"></a>Linki pokrewne

- [Usługa Xamarin University kurs: Tworzenie biblioteki powiązań języka Objective-C](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Usługa Xamarin University kurs: Tworzenie biblioteki powiązań języka Objective-C za pomocą narzędzie Objective Sharpie](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)

