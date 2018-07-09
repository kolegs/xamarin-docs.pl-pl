---
title: Przykład rzeczywistych przy użyciu Menedżera CocoaPods
description: W tym dokumencie przedstawiono sposób użycia Objective Sharpie do automatycznego generowania definicji powiązania C# z CocoaPod.
ms.prod: xamarin
ms.assetid: 233B781D-5841-4250-9F63-0585231D2112
author: asb3993
ms.author: amburns
ms.date: 03/28/2018
ms.openlocfilehash: bac34f662e24c6b08a67cd8da1f41b37b43b3faf
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37855211"
---
# <a name="real-world-example-using-cocoapods"></a>Przykład rzeczywistych przy użyciu Menedżera CocoaPods

> [!NOTE]
> W tym przykładzie użyto [AFNetworking CocoaPod](https://cocoapods.org/pods/AFNetworking).

Nowość w wersji 3.0 Objective Sharpie obsługuje powiązanie CocoaPods, a nawet zawiera polecenie (`sharpie pod`) do pobierania, konfigurowanie i tworzenie aplikacji CocoaPods, bardzo proste. Należy [zapoznania się z aplikacji CocoaPods](https://cocoapods.org) ogólnie rzecz biorąc przed użyciem tej funkcji.

## <a name="creating-a-binding-for-a-cocoapod"></a>Tworzenie powiązania dla CocoaPod

`sharpie pod` Polecenie ma jedną z opcji globalnych i dwa podpoleceń polecenia:

```bash
$ sharpie pod -help
usage: sharpie pod [OPTIONS] COMMAND [COMMAND_OPTIONS]

Pod Options:
  -d, -dir DIR     Use DIR as the CocoaPods binding directory,
                   defaulting to the current directory

Available Commands:
  init         Initialize a new Xamarin C# CocoaPods binding project
  bind         Bind an existing Xamarin C# CocoaPods project
```

`init` Podpolecenie ma również przydatne pomocy:

```bash
$ sharpie pod init -help
usage: sharpie pod init [INIT_OPTIONS] TARGET_SDK POD_SPEC_NAMES

Init Options:
  -f, -force       Initialize a new Podfile and run actions against
                   it even if one already exists
```

Wiele nazw CocoaPod i subspec można przekazać do `init`.

```bash
$ sharpie pod init ios AFNetworking
** Setting up CocoaPods master repo ...
   (this may take a while the first time)
** Searching for requested CocoaPods ...
** Working directory:
**   - Writing Podfile ...
**   - Installing CocoaPods ...
**     (running `pod install --no-integrate --no-repo-update`)
Analyzing dependencies
Downloading dependencies
Installing AFNetworking (2.6.0)
Generating Pods project
Sending stats
** 🍻 Success! You can now use other `sharpie podn`  commands.
```

Po swojej CocoaPod, teraz można utworzyć powiązania:

```bash
$ sharpie pod bind
```

Spowoduje to projekt CocoaPod Xcode, są skompilowane oceniane i analizowane za Objective Sharpie. Dużo danych wyjściowych konsoli zostanie wygenerowany, ale powinna być rozwiązywana WE definicji powiązania na końcu:

```bash
(... lots of build output ...)

Parsing 19 header files...

Binding...
  [write] ApiDefinitions.cs
  [write] StructsAndEnums.cs

Done.
```

## <a name="next-steps"></a>Następne kroki

Po wygenerowaniu **ApiDefinitions.cs** i **StructsAndEnums.cs** pliki, zapoznaj się z poniższą dokumentację, Generowanie zestawu do użycia w aplikacjach:

- [Omówienie języka Objective-C powiązania](~/cross-platform/macios/binding/overview.md)
- [Tworzenie powiązań bibliotek języka Objective-C](~/cross-platform/macios/binding/objective-c-libraries.md)
- [Wskazówki: Powiązywanie Biblioteka platformy iOS — Objective-C](~/ios/platform/binding-objective-c/walkthrough.md)
- [Usługa Xamarin University kurs: Tworzenie biblioteki powiązań języka Objective-C](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Usługa Xamarin University kurs: Tworzenie biblioteki powiązań języka Objective-C za pomocą narzędzie Objective Sharpie](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)
