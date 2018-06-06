---
title: Ograniczenia osadzania .NET
description: W tym dokumencie opisano ograniczenia osadzenia .NET, narzędzia, która pozwala na korzystanie z kodu platformy .NET w innych językach programowania.
ms.prod: xamarin
ms.assetid: EBBBB886-1CEF-4DF4-AFDD-CA96049F878E
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: fdd3ac4cd57ac7f79f9071d62e758625b30f05dd
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34794127"
---
# <a name="net-embedding-limitations"></a>Ograniczenia osadzania .NET

Tego dokumentu opisano ograniczenia osadzenia .NET i, jeśli to możliwe, udostępnia obejścia dla nich.

## <a name="general"></a>Ogólne

### <a name="use-more-than-one-embedded-library-in-a-project"></a>Użyj więcej niż jedną bibliotekę osadzony w projekcie

Nie jest możliwe mieć dwóch środowisk uruchomieniowych Mono istniejących umieszczone w tej samej aplikacji. Oznacza to, że nie można użyć dwie różne biblioteki .NET osadzanie generowane w tej samej aplikacji.

**Obejście problemu:** generatora umożliwia utworzenie jedna biblioteka, która obejmuje kilka zestawów (z różnych projektów).

### <a name="subclassing"></a>Tworzenie podklas

Osadzanie .NET ułatwia integrację środowiska uruchomieniowego Mono w aplikacjach przez udostępnianie stanowi zestaw interfejsów API gotowe do użycia dla języka docelowego i platformy.

Jednak nie jest dwukierunkowe integracji, np. nie można podklasą typu zarządzanego i kodu zarządzanego do wywołania zwrotnego w kodzie natywnym, ponieważ kod zarządzany nie rozpoznaje tego współistnienie oczekuje.

W zależności od potrzeb może być możliwe do części obejście tego ograniczenia, np.

* kod zarządzany może p/invoke do kodu natywnego. Wymagane jest dostosowanie kodu zarządzanego umożliwiają dostosowanie z kodu natywnego;

* Użyj produktów, takich jak Xamarin.iOS i udostępnianie biblioteki zarządzanej, która umożliwiałaby Objective-C (w tym przypadku) do podklasy niektórych zarządzanych NSObject podklasy.

## <a name="objective-c-generated-code"></a>Wygenerowany kod języka Objective C

### <a name="nullability"></a>Dopuszczania wartości null

Brak metadanych, w którym Poinformuj nas, czy dopuszczalny jest odwołanie o wartości null lub nie dla interfejsu API nie istnieje. Większość interfejsów API zgłosi `ArgumentNullException` Jeśli nie może sprostać `null` argumentu. Jest to problem, ponieważ obsługę wyjątków w języku Objective C jest coś lepiej unikać.

Ponieważ firma Microsoft nie może wygenerować adnotacje dokładne dopuszczania wartości null w plikach nagłówka i chcesz zminimalizować zarządzane wyjątki możemy domyślnie argumentów innych niż null (`NS_ASSUME_NONNULL_BEGIN`) i Dodaj niektóre z konkretnym, gdy dokładność jest możliwe, adnotacje dopuszczania wartości null.

### <a name="bitcode-ios"></a>Bitcode (iOS)

Osadzanie .NET nie obsługuje obecnie kodu bitowego w systemach iOS, w którym włączono obsługę niektóre szablony projektu Xcode. Ten będzie musiał go wyłączyć, aby pomyślnie struktur generowanego łącza.

![Opcja kodu bitowego](images/ios-bitcode-option.png)
