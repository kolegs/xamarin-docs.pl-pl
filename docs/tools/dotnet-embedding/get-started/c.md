---
title: Wprowadzenie do korzystania z C
ms.topic: article
ms.prod: xamarin
ms.assetid: 2A27BE0F-95FB-4C3A-8A43-72540179AA85
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: ea8335348416dc00074d83e09b74521da7abcb66
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="getting-started-with-c"></a>Wprowadzenie do korzystania z C


## <a name="requirements"></a>Wymagania

Aby C za pomocą osadzanie .NET należy uruchomionej maszyny Mac lub Windows:

* System macOS 10.12 (Sierra) lub nowszy
* Xcode 8.3.2 lub nowszy

* Windows 7, 8, 10 lub nowszy
* Visual Studio 2013 lub nowszy

* [Mono](http://www.mono-project.com/download/)


## <a name="installation"></a>Instalacja

Następnym krokiem jest pobrać i zainstalować narzędzia osadzanie .NET na tym komputerze.

Binarnych kompilacji dla generatory C i Java nadal nie są dostępne, ale wkrótce będzie.

Alternatywą jest możliwe utworzenie z naszym repozytorium git, zobacz [przyczyniając się](https://github.com/mono/Embeddinator-4000/blob/master/docs/Contributing.md) dokumentu, aby uzyskać instrukcje.


## <a name="generation"></a>Generowanie

Aby wygenerować kod w języku C, należy wywołać narzędzia .NET osadzanie przekazanie flagi prawo pod kątem języka C:

### <a name="windows"></a>System Windows:

```csharp
$ build/lib/Debug/Embeddinator-4000.exe --gen=c --output=managed_c --platform=windows --compile managed.dll
```

Upewnij się, że wywołanie z powłoki poleceń programu Visual Studio określonej wersji programu Visual Studio można teraz Profilowanie grup.

### <a name="macos"></a>macOS

```csharp
$ mono build/lib/Debug/Embeddinator-4000.exe --gen=c --output=managed_c --platform=macos --compile managed.dll
```

### <a name="output-files"></a>Pliki wyjściowe

Jeśli wszystko przebiegnie poprawnie, zostanie wyświetlone następujące dane wyjściowe:

```csharp
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

Ponieważ `--compile` flagi został przekazany do narzędzia, osadzanie .NET powinien również mieć skompilowany pliki wyjściowe do biblioteki współużytkowanej, która znajduje się obok plików wygenerowanych `libmanaged.dylib` pliku na macOS, i `managed.dll` w systemie Windows.

Aby korzystać z biblioteki udostępnionej, mogą obejmować `managed.h` C nagłówek pliku, co zapewnia deklaracje C odpowiadający odpowiednio zarządzane biblioteki interfejsów API i Połącz z wymienionych wcześniej skompilowany biblioteki udostępnionej.

## <a name="further-reading"></a>Dalsze informacje

* [Ograniczenia Embeddinator](~/tools/dotnet-embedding/limitations.md)
* [Współtworzenie projekt open source](https://github.com/mono/Embeddinator-4000/blob/master/docs/Contributing.md)
* [Kody błędów wraz z opisami](~/tools/dotnet-embedding/errors.md)
