---
title: Definicje interfejsu & pliki Typy wyliczeniowe
description: W tym dokumencie opisano ApiDefinitions.cs i StructsAndEnums.cs pliki, które generuje Objective Sharpie. Pliki te są następnie używane do dostępu do kodu języka Objective-C za pomocą języka C#.
ms.prod: xamarin
ms.assetid: AC2087C0-BA54-46D8-B70C-6972941C8F73
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: df8d4508db14116a5b36e893f161ac891d58dc46
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37855185"
---
# <a name="apidefinitions--structsandenums-files"></a>Definicje interfejsu & pliki Typy wyliczeniowe

Po pomyślnym uruchomieniu Objective Sharpie generuje `Binding/ApiDefinitions.cs` i `Binding/StructsAndEnums.cs` plików.
Te dwa pliki są dodawane do projektu powiązania w programie Visual Studio dla komputerów Mac lub przekazywana bezpośrednio do `btouch` lub `bmac` narzędzi, aby wygenerować ostateczny powiązania.

W *niektórych* przypadkach te wygenerowane pliki jest wszystko, czego potrzebujesz, jednak częściej dewelopera należy ręcznie zmodyfikować tych wygenerowanych plików, aby rozwiązać problemy, których nie można obsłużyć automatycznie za pomocą narzędzia (takie jak te oflagowane za pomocą [ `Verify` atrybut](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md)).

Następne kroki, należą:

- **Dostosowywanie nazw**: czasami warto dostosować nazwy metod i klas, aby dopasować wytycznych projektowych programu .NET Framework.
- **Metody lub właściwości**: Algorytm heurystyczny używany przez Objective Sharpie czasami wybierze metodę, aby zmienić właściwość. W tym momencie można zdecydować, czy jest to oczekiwane zachowanie, czy nie.
- **Podpinanie zdarzeń**: można połączyć swoje klas z Twoich zajęciach delegata i automatycznego generowania zdarzeń dla osób.
- **Dołączyć powiadomienia**: nie jest możliwe do wyodrębnienia kontrakt interfejsu API powiadomień z plików nagłówkowych czystego, będzie to wymagało podróży w dokumentacji interfejsu API. Chcąc silnie typizowaną powiadomienia, należy zaktualizować wynik.
- **Danych nadzoru interfejsu API**: na tym etapie możesz wybrać zapewniają dodatkowe konstruktorów, należy dodać metody (aby dać składni inicjowania w konstrukcji języka C#), przeciążanie operatora i implementowanie interfejsów w pliku dodatkowe definicje.

Zobacz [powiązanie interfejsu API](~/cross-platform/macios/binding/objective-c-libraries.md) opis, aby zobaczyć, jak te pliki mieści się w procesie powiązania, jak pokazano na poniższym diagramie:

![](apidefinitions-structsandenums-images/binding-flowchart.png "Na tym diagramie przedstawiono proces wiązania")

Zapoznaj się [powiązanie odwołania do typów](~/cross-platform/macios/binding/binding-types-reference.md) Aby uzyskać więcej informacji na temat zawartości tych plików.

## <a name="related-links"></a>Linki pokrewne

- [Usługa Xamarin University kurs: Tworzenie biblioteki powiązań języka Objective-C](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Usługa Xamarin University kurs: Tworzenie biblioteki powiązań języka Objective-C za pomocą narzędzie Objective Sharpie](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)
