---
title: Powiązanie bibliotek systemu iOS
description: Ten dokument zawiera opis sposobu tworzenia języka C# powiązania z kod języka Objective-C, dzięki czemu można korzystać z natywnych bibliotek i aplikacji CocoaPods aplikacji platformy Xamarin.iOS.
ms.prod: xamarin
ms.assetid: EBDC50DC-B44B-4003-AB2B-1EEB868A5E01
ms.technology: xamarin-ios
ms.custom: xamu-video
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 75c8f9a2eea080c3da031366b314d94929080811
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37855224"
---
# <a name="binding-ios-libraries"></a>Powiązanie bibliotek systemu iOS

Skorzystaj z poniższych linków, aby dowiedzieć się więcej o powiązaniu biblioteki języka Objective-C i CocoaPods Xamarin.iOS i Xamarin.Mac:

- [**Omówienie** ](~/cross-platform/macios/binding/overview.md) -
  w tym artykule opisano, jak działa powiązanie.
- [**Tworzenie powiązań bibliotek języka Objective-C** ](~/cross-platform/macios/binding/objective-c-libraries.md) -
  instrukcje dotyczące sposobu tworzenia powiązania bibliotek języka Objective-C do użycia w projektach platformy Xamarin.
- [**Wpisz definicję podręcznik** ](~/cross-platform/macios/binding/binding-types-reference.md) -
  w tym artykule opisano wszystkie atrybuty, które są dostępne dla autorów powiązania do procesu tworzenia wiązania.

## <a name="objective-sharpiecross-platformmaciosbindingobjective-sharpieindexmd"></a>[Narzędzie Objective Sharpie](~/cross-platform/macios/binding/objective-sharpie/index.md)

Narzędzie Objective Sharpie to narzędzie wiersza polecenia ułatwiające bootstrap w pierwszym przebiegu powiązania.
Działa przez analizowanie plików nagłówka natywną bibliotekę do mapowania publicznego interfejsu API do [powiązanie definicji](~/cross-platform/macios/binding/objective-c-libraries.md) (proces, które w przeciwnym razie odbywa się ręcznie). Narzędzie Objective Sharpie nie tworzy powiązanie samodzielnie, ale może ułatwić rozpoczęcie pracy!

Objective Sharpie 3.0 wprowadzono możliwość powiązania Cocoapods bezpośrednio!

## <a name="walkthrough---binding-an-ios-objective-c-librarywalkthroughmd"></a>[Przewodnik: powiązanie z systemem iOS biblioteki języka Objective-C](walkthrough.md)

Ta strona zawiera przewodnik krok po kroku tworzenia powiązań projektu systemu iOS przy użyciu typu open source [ **InfColorPicker** ](https://github.com/InfinitApps/InfColorPicker) projektu języka Objective-C jako przykład. **InfColorPicker** biblioteka zawiera kontroler widoku wielokrotnego użytku, która pozwala użytkownikowi na wybranie kolor oparty na jego reprezentację HSB, dzięki czemu wybór koloru jest bardziej przyjazny dla użytkownika.
Narzędzie Objective Sharpie będzie służyć do pomocy w procesie powiązania.

## <a name="xamarin-university-lightning-lecture"></a>Wykładu pod kątem obsługi platformy Xamarin University

> [!VIDEO https://youtube.com/embed/ZUoPLcmnf1o]

**iOS powiązania w języku C/C++, przez [Xamarin University](https://university.xamarin.com/)**

## <a name="related-links"></a>Linki pokrewne

- [Tworzenie powiązań języka Objective-C](~/cross-platform/macios/binding/index.md)
- [Powiązania komputerów Mac](~/mac/platform/binding.md)
- [Usługa Xamarin University kurs: Tworzenie biblioteki powiązań języka Objective-C](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Usługa Xamarin University kurs: Tworzenie biblioteki powiązań języka Objective-C za pomocą narzędzie Objective Sharpie](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)