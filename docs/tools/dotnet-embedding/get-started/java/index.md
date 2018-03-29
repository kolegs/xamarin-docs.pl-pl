---
title: Wprowadzenie do języka Java
ms.topic: article
ms.prod: xamarin
ms.assetid: B9A25E9B-3EC2-489A-8AD3-F78287609747
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 03/28/2018
ms.openlocfilehash: 06df925525c7bc01963274e9e0d5edfe7bb70613
ms.sourcegitcommit: 17a9cf246a4d33cfa232016992b308df540c8e4f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/29/2018
---
# <a name="getting-started-with-java"></a>Wprowadzenie do języka Java


Jest to stronie początkowej dla języka Java, która obejmuje podstawy dla wszystkich obsługiwanych platformach.

## <a name="requirements"></a>Wymagania

Aby użyć osadzanie .NET z językiem Java należy:

* Java 1,8 lub nowszy
* [Mono 5.0](http://www.mono-project.com/download/)

Dla komputerów Mac:
* Xcode 8.3.2 lub nowszy

W systemie Windows:
* Visual Studio 2017 z obsługi języka C++
* Windows 10 SDK

Dla systemu Android:
* [Xamarin.Android 7.5](https://www.visualstudio.com/xamarin/) lub nowszy
* [Android Studio 3.x](https://developer.android.com/studio/index.html) z językiem Java 1.8

Można użyć [programu Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/) do edycji i kompilowania kodu C#.

> [!NOTE]
> Wcześniejszych wersji Xcode, Visual Studio, Xamarin.Android, Android Studio i Mono _może_ działać, ale są zastosowaniem i nieobsługiwane.

## <a name="installation"></a>Instalacja

Osadzanie .NET jest aktualnie dostępny w [NuGet](https://www.nuget.org/packages/Embeddinator-4000/):

```csharp
nuget install Embeddinator-4000
```
To spowoduje umieszczenie `Embeddinator-4000.exe` do `packages/Embeddinator-4000/tools` katalogu.

Ponadto kompilacji Embeddinator ze źródła, zobacz nasze [repozytorium git](https://github.com/mono/Embeddinator-4000/) i [przyczyniając się](https://github.com/mono/Embeddinator-4000/blob/master/docs/Contributing.md) dokumentu, aby uzyskać instrukcje.

## <a name="platforms"></a>Platformy

Java jest obecnie w stanie zapoznawczej macOS, systemu Windows i Android.

Wybrano platformę przez przekazanie `--platform=<platform>` embeddinator argument wiersza polecenia. Obecnie `macOS`, `Windows`, i `Android` są obsługiwane.

### <a name="macos-and-windows"></a>System macOS i systemu Windows

Do tworzenia aplikacji należy można używać żadnych IDE języka Java, obsługujący Java 1.8. Android Studio można użyć nawet dla tego w razie potrzeby [widoczną w tym miejscu](https://stackoverflow.com/questions/16626810/can-android-studio-be-used-to-run-standard-java-projects). Dane wyjściowe pliku JAR można użyć, jak w przypadku dowolnego standardowego pliku jar Java.

### <a name="android"></a>Android

Upewnij się, są już skonfigurowane do opracowywania aplikacji systemu Android przed podjęciem próby go utworzyć za pomocą Embeddinator. [Instrukcjami](~/tools/dotnet-embedding/get-started/java/android.md) założono, że masz już pomyślnie skompilowane i wdrożonych aplikacji systemu Android z komputera.

Android Studio jest zalecane w przypadku rozwoju, ale inne IDEs powinny działać tak długo, jak obsługuje [format pliku AAR](https://developer.android.com/studio/projects/android-library.html).

## <a name="further-reading"></a>Dalsze informacje

* [Wprowadzenie w systemie Android](~/tools/dotnet-embedding/get-started/java/android.md)
* [Wywołania zwrotne w systemie Android](~/tools/dotnet-embedding/android/callbacks.md)
* [Wstępne badanie systemu Android](~/tools/dotnet-embedding/android/index.md)
* [Ograniczenia Embeddinator](~/tools/dotnet-embedding/limitations.md)
* [Współtworzenie projekt open source](https://github.com/mono/Embeddinator-4000/blob/master/docs/Contributing.md)
* [Kody błędów wraz z opisami](~/tools/dotnet-embedding/errors.md)
