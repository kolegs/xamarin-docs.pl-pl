---
title: Wprowadzenie do systemu tvOS 12
description: Niniejszy dokument zawiera ogólne omówienie nowych i zaktualizowanych funkcji systemu tvOS 12, dla której platformy Xamarin w wersji zapoznawczej udostępnia obecnie powiązania C#.
ms.prod: xamarin
ms.assetid: 037F7FFF-2155-4017-B99A-839CE7EC5C9C
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 10/05/2018
ms.openlocfilehash: f7fb8cc379a070b848c5154c9c1d4fbfc8186266
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/18/2018
ms.locfileid: "39615206"
---
# <a name="introduction-to-tvos-12"></a>Wprowadzenie do systemu tvOS 12

Ten dokument zawiera omówienie nowych i zaktualizowanych systemu tvOS 12.

Aby rozpocząć tworzenie aplikacji dla systemu tvOS 12 za pomocą platformy Xamarin, Przyjrzyj się [przewodnik wprowadzenie](~/ios/platform/introduction-to-ios12/get-started.md).

## <a name="tvuikit"></a>TVUIKit

dla systemu tvOS 12 obejmuje TVUIKit, zestaw interfejsów API, które umożliwiają deweloperom systemu tvOS, korzystanie z kontrolek standardowych systemu tvOS, takich jak widoki plakat, przyciski podpisu, widoki kart i widoki monogram. dla systemu tvOS 12 wprowadza również właściwość, która umożliwia etykiety, aby przewijać tekst, który jest za długa była całkowicie widoczna.

## <a name="password-autofill"></a>Autowypełnianie hasła

Przy użyciu systemu tvOS 12 użytkownicy mogą używać swoich urządzeń z systemem iOS do logowania do aplikacji systemu tvOS w jednym naciśnięciem. Ta opcja jest włączona przy użyciu kombinacji `UITextContentType` pola użycia, aby określić nazwę użytkownika i hasło, skojarzone domeny, aby ustanowić relację między aplikację dla systemu iOS i aplikacji systemu tvOS oraz preferowany fokus wymagane wdrażanie środowisk wybierz element, aby przenieść fokus po użytkownik zawiera nazwę użytkownika i hasło.

## <a name="focus-engine-enhancements"></a>Ulepszenia aparatu koncentracji uwagi

dla systemu tvOS 12 zezwala na wszystkie aplikacje, niezależnie od tego, w jaki sposób są one renderowane, wchodzić w interakcje z aparatem fokus. Do interakcji z użytkownikami za pomocą zdalnego Siri aparat fokus może służyć w dowolnej aplikacji wybierz element, podpowiedzi na zmianie fokusu możliwe i naturalny sposób zaktualizować fokus. Ta opcja jest włączona w aplikacjach niestandardowych za pośrednictwem firmy UIKit `IUIFocusItemContainer` interfejsu `UIFocusMovementHint` klasy `IUIFocusItemScrollableContainer` interfejsu oraz innych powiązanych klas i metod.

## <a name="vision-framework"></a>Struktury Vision

Struktury Vision obejmuje wykrywanie twarzy ulepszone, który może wykrywać twarze w różnych położeniach. Żądania zmiany można teraz również wybrać określoną poprawkę algorytm framework wizji.

## <a name="natural-language-framework"></a>Framework języka naturalnego

Struktury języka naturalnego umożliwia aplikacjom wykonywanie różnych rodzajów analizy języka. Na przykład może służyć do identyfikowania części mowy i określić język, reprezentowane przez blok tekstu.

## <a name="deprecations"></a>Deprecations

Przy użyciu systemu tvOS 12 Apple przestarzała OpenGL ES [zachęcanie do deweloperów](https://developer.apple.com/tvos/whats-new/) podczas wdrażania systemu operacyjnego.

## <a name="related-links"></a>Linki pokrewne

- [Przykłady dla systemu tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [dla systemu tvOS — dla deweloperów firmy Apple (Apple)](https://developer.apple.com/tvos/)
- [What's new in systemu tvOS 12 (Apple) (wideo)](https://developer.apple.com/videos/play/wwdc2018/208/)
- [TV (Apple)](https://www.apple.com/tv/)