---
title: Przykład rzeczywistych przy użyciu programu CocoaPods
description: Tym dokumencie przedstawiono sposób użycia Sharpie cel do automatycznego generowania definicji powiązanie C# z CocoaPod.
ms.prod: xamarin
ms.assetid: 233B781D-5841-4250-9F63-0585231D2112
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/28/2018
ms.openlocfilehash: cbcafc8d77304d117f8130cf0d6a89dd2a5ed3c8
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="real-world-example-using-cocoapods"></a>Przykład rzeczywistych przy użyciu programu CocoaPods

> [!NOTE]
> W tym przykładzie użyto [AFNetworking CocoaPod](https://cocoapods.org/pods/AFNetworking).

Nowość w wersji 3.0 obsługuje powiązania programu CocoaPods Sharpie cel, a nawet zawiera polecenie (`sharpie pod`) do pobierania, konfigurowanie i kompilowania programu CocoaPods bardzo proste. Należy [zapoznania się z programu CocoaPods](https://cocoapods.org) ogólnie przed użyciem tej funkcji.

## <a name="creating-a-binding-for-a-cocoapod"></a>Tworzenie powiązania dla CocoaPod

`sharpie pod` Polecenie ma jedną opcję globalne i dwa polecenia:

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

`init` Podpolecenie ma również przydatne Pomoc:

```bash
$ sharpie pod init -help
usage: sharpie pod init [INIT_OPTIONS] TARGET_SDK POD_SPEC_NAMES

Init Options:
  -f, -force       Initialize a new Podfile and run actions against
                   it even if one already exists
```

Można podać wiele nazw CocoaPod i subspec do `init`.

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

Po CocoaPod Twojego, można teraz utworzyć powiązania:

```bash
$ sharpie pod bind
```

Spowoduje to projektu CocoaPod Xcode jest zbudowany obliczone i analizowane przez Sharpie cel. Dużo danych wyjściowych konsoli zostanie wygenerowany, ale powinno spowodować definicji powiązania na końcu:

```bash
(... lots of build output ...)

Parsing 19 header files...

Binding...
  [write] ApiDefinitions.cs
  [write] StructsAndEnums.cs

Done.
```

## <a name="next-steps"></a>Następne kroki

Po wygenerowaniu **ApiDefinitions.cs** i **StructsAndEnums.cs** pliki, zapoznaj się z informacjami w następującej dokumentacji, można wygenerować zestawu do użycia w aplikacjach:

- [Omówienie języka Objective-C powiązania](~/cross-platform/macios/binding/overview.md)
- [Powiązanie bibliotek języka Objective C](~/cross-platform/macios/binding/objective-c-libraries.md)
- [Wskazówki: Powiązywanie biblioteka języka Objective C dla systemu iOS](~/ios/platform/binding-objective-c/walkthrough.md)

