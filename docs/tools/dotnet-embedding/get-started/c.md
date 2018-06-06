---
title: Wprowadzenie do korzystania z C
description: Ten dokument zawiera opis sposobu umożliwia osadzanie .NET osadzanie kodu platformy .NET w aplikacji C. Omówiono Użyj .NET osadzanie w Visual Studio 2017 i Visual Studio dla komputerów Mac.
ms.prod: xamarin
ms.assetid: 2A27BE0F-95FB-4C3A-8A43-72540179AA85
author: topgenorth
ms.author: toopge
ms.date: 04/19/2018
ms.openlocfilehash: 248d44f23495e45d9d35b34622de0f3b85ca3e8d
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34794101"
---
# <a name="getting-started-with-c"></a>Wprowadzenie do korzystania z C

## <a name="requirements"></a>Wymagania

Umożliwia osadzanie .NET z C, potrzebne są uruchomione maszyny Mac lub Windows:

### <a name="macos"></a>macOS

* System macOS 10.12 (Sierra) lub nowszy
* Xcode 8.3.2 lub nowszy
* [Mono](http://www.mono-project.com/download/)

### <a name="windows"></a>Windows

* Windows 7, 8, 10 lub nowszy
* Visual Studio 2015 lub nowszego

## <a name="installing-net-embedding-from-nuget"></a>Instalowanie .NET osadzanie z NuGet

Postępuj zgodnie z następującymi [instrukcje](~/tools/dotnet-embedding/get-started/install/install.md) do instalowania i konfigurowania osadzanie .NET projektu.

Wywołanie polecenia, które należy skonfigurować będzie wyglądać (prawdopodobnie z różnych numerów wersji i ścieżek):

### <a name="visual-studio-for-mac"></a>Visual Studio for Mac

```shell
mono {SolutionDir}/packages/Embeddinator-4000.0.4.0.0/tools/Embeddinator-4000.exe --gen=c --outdir=managed_c --platform=macos --compile managed.dll
```

### <a name="visual-studio-2017"></a>Visual Studio 2017

```shell
$(SolutionDir)\packages\Embeddinator-4000.0.2.0.80\tools\Embeddinator-4000.exe --gen=c --outdir=managed_c --platform=windows --compile managed.dll
```

## <a name="generation"></a>Generowanie

### <a name="output-files"></a>Pliki wyjściowe

Jeśli wszystko przebiegnie poprawnie, zostanie wyświetlone następujące dane wyjściowe:

```shell
Parsing assemblies...
    Parsed 'managed.dll'
Processing assemblies...
Generating binding code...
    Generated: managed.h
    Generated: managed.c
    Generated: mscorlib.h
    Generated: mscorlib.c
    Generated: embeddinator.h
    Generated: glib.c
    Generated: glib.h
    Generated: mono-support.h
    Generated: mono_embeddinator.c
    Generated: mono_embeddinator.h
```

Ponieważ `--compile` flagi został przekazany do narzędzia, osadzanie .NET powinien również mieć skompilowany pliki wyjściowe do biblioteki współużytkowanej, która znajduje się obok plików wygenerowanych **libmanaged.dylib** plik na macOS i **managed.dll** w systemie Windows.

Aby korzystać z biblioteki współużytkowanej, można umieścić **managed.h** C nagłówek pliku, co zapewnia deklaracje C odpowiadający odpowiednio zarządzane biblioteki interfejsów API i Połącz z wymienionych wcześniej skompilowany biblioteki udostępnionej.

## <a name="further-reading"></a>Dalsze informacje

* [Ograniczenia osadzania .NET](~/tools/dotnet-embedding/limitations.md)
* [Współtworzenie projekt open source](https://github.com/mono/Embeddinator-4000/blob/master/Contributing.md)
* [Kody błędów wraz z opisami](~/tools/dotnet-embedding/errors.md)
