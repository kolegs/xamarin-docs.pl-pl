---
title: Ograniczenia osadzania .NET
ms.topic: article
ms.prod: xamarin
ms.assetid: EBBBB886-1CEF-4DF4-AFDD-CA96049F878E
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 110eb4169103f99c1538a1846fd231069863530c
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="net-embedding-limitations"></a>Ograniczenia osadzania .NET


Tego dokumentu opisano ograniczenia .NET osadzenia (Embeddinator 4000) i, jeśli to możliwe, udostępnia obejścia dla nich.

## <a name="general"></a>Ogólne

### <a name="use-more-than-one-embedded-library-in-a-project"></a>Użyj więcej niż jedną bibliotekę osadzony w projekcie

Nie jest możliwe mieć dwóch środowisk uruchomieniowych mono istniejących umieszczone w tej samej aplikacji. Oznacza to, że nie można użyć dwóch różnych bibliotekami generowane 4000 embeddinator wewnątrz tej samej aplikacji.

**Obejście problemu:** generatora umożliwia utworzenie jedna biblioteka, która obejmuje kilka zestawów (z różnych projektów).

### <a name="subclassing"></a>Tworzenie podklas

Embeddinator ułatwia integrację środowiska uruchomieniowego mono w aplikacjach przez udostępnianie stanowi zestaw interfejsów API gotowe do użycia dla języka docelowego i platformy.

Jednak nie jest to integracji dwukierunkowej, np. nie można podklasą typu zarządzanego i kodu zarządzanego do wywołania zwrotnego w kodzie natywnym, ponieważ kod zarządzany nie rozpoznaje tego współistnienie oczekiwać.

W zależności od potrzeb może być możliwe do części obejście tego ograniczenia, np.

* kod zarządzany może p/invoke do kodu natywnego. Wymagane jest dostosowanie kodu zarządzanego umożliwiają dostosowanie z kodu natywnego;

* Użyj produktów, takich jak Xamarin.iOS i udostępnianie biblioteki zarządzanej, która umożliwiałaby ObjC (w tym przypadku) do podklasy niektórych zarządzanych podklasy NSObject.


## <a name="objc-generated-code"></a>ObjC wygenerowany kod

### <a name="nullability"></a>Dopuszczania wartości null

W .NET, nie ma żadnych metadanych, który Poinformuj nas, czy dopuszczalny jest odwołanie o wartości null lub nie dla interfejsu API. Większość interfejsów API zgłosi `ArgumentNullException` Jeśli nie może sprostać `null` argumentu. Jest to problem, ponieważ ObjC obsługi wyjątków jest coś lepiej unikać.

Ponieważ firma Microsoft nie może wygenerować adnotacje dokładne dopuszczania wartości null w plikach nagłówka i chcesz zminimalizować zarządzane wyjątki możemy domyślnie argumentów innych niż null (`NS_ASSUME_NONNULL_BEGIN`) i Dodaj niektóre z konkretnym, gdy dokładność jest możliwe, adnotacje dopuszczania wartości null.
