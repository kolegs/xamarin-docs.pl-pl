---
title: ApiDefinitions & StructsAndEnums pliki
description: W tym dokumencie opisano ApiDefinitions.cs i StructsAndEnums.cs pliki, które generuje Sharpie cel. Pliki te są następnie używane do dostępu kod języka Objective-C w języku C#.
ms.prod: xamarin
ms.assetid: AC2087C0-BA54-46D8-B70C-6972941C8F73
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 3b991f6105c6053f473b049d195aaef63cbcdd57
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34780896"
---
# <a name="apidefinitions--structsandenums-files"></a>ApiDefinitions & StructsAndEnums pliki

Po pomyślnym uruchomieniu Sharpie cel generuje `Binding/ApiDefinitions.cs` i `Binding/StructsAndEnums.cs` plików.
Te dwa pliki są dodawane do powiązania projektu programu Visual Studio dla komputerów Mac lub przekazywane bezpośrednio do `btouch` lub `bmac` narzędzi do tworzenia końcowego powiązania.

W *niektórych* przypadkach te wygenerowanych plików może być wszystko, czego potrzebujesz, jednak wygenerowane częściej deweloper musi ręcznie zmodyfikować te pliki, aby rozwiązać problemy, które nie może być automatycznie obsługiwane przez narzędzia (na przykład tych oflagowane z [ `Verify` atrybut](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md)).

Oto niektóre z następne kroki:

- **Dostosowywanie nazw**: czasami można dostosować nazwy metod i klasy, aby być zgodna z wytycznymi projektowania programu .NET Framework.
- **Metody lub właściwości**: heurystyki używanego przez cel Sharpie czasami przyjmują metodę można włączyć w właściwości. W tym momencie można zdecydować, czy jest to oczekiwane zachowanie, czy nie.
- **Podpinanie zdarzeń**: można połączyć z klas z Twojej klasy obiektów delegowanych i automatycznego generowania zdarzeń dla tych.
- **Podłączanie powiadomienia**: nie można wyodrębnić kontrakt interfejsu API powiadomień z pliki nagłówkowe czystego, będzie to wymagało podróży z dokumentacją interfejsu API. Jeśli chcesz, aby jednoznacznie powiadomienia, należy zaktualizować wynik.
- **Selekcjonowanie i filtracja zasobów interfejsu API**: W tym momencie można podać dodatkowe konstruktorów, Dodaj metody (w celu umożliwienia inicjowania w konstrukcji składni języka C#), przeładowania operatora i implementowanie interfejsów w pliku definicji dodatkowe.

Zobacz [powiązanie interfejs API](~/cross-platform/macios/binding/objective-c-libraries.md) opis, aby zobaczyć, jak te pliki mieści się w procesie powiązania, jak pokazano na poniższym diagramie:

![](apidefinitions-structsandenums-images/binding-flowchart.png "Proces wiązania jest wyświetlany na tym diagramie")

Zapoznaj się [powiązanie odwołania do typów](~/cross-platform/macios/binding/binding-types-reference.md) uzyskać więcej informacji o zawartości tych plików.

