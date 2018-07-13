---
title: Wprowadzenie do systemu tvOS 12
description: Niniejszy dokument zawiera ogólne omówienie nowych i zaktualizowanych funkcji systemu tvOS 12, dla której platformy Xamarin w wersji zapoznawczej udostępnia obecnie powiązania C#.
ms.prod: xamarin
ms.assetid: 037F7FFF-2155-4017-B99A-839CE7EC5C9C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/25/2018
ms.openlocfilehash: e45d9944a2f4fc392b5a78efb4a7751d19641c73
ms.sourcegitcommit: cfb72be633e335147d156af3ef9527151b9e31d9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39030668"
---
# <a name="introduction-to-tvos-12"></a>Wprowadzenie do systemu tvOS 12

![Wersja zapoznawcza](~/media/shared/preview.png)

> [!WARNING]
> 12 pomocy technicznej platformy Xamarin dla systemu tvOS jest obecnie w wersji zapoznawczej, co oznacza, że może ona zawierać błędy, nie jest pełną funkcją, i mogą ulec zmianie. Na użytek go tylko do eksperymentowania.

> [!NOTE]
> - Przegląd [wprowadzenie](~/ios/platform/introduction-to-ios12/get-started.md) wskazówki, aby uzyskać instrukcje na temat zacznij tworzyć aplikacje dla systemu tvOS 12 za pomocą platformy Xamarin dla systemów 12 i.
> - Aby uzyskać więcej informacji, przeczytaj platformy Xamarin w wersji zapoznawczej [wersji wpis w blogu](https://releases.xamarin.com/preview-release-xcode-10-beta-3/).

Ten dokument zawiera ogólne omówienie systemu tvOS nowych i zaktualizowanych funkcji 12 które Xamarin w wersji zapoznawczej wersji udostępnia obecnie powiązania C#.

## <a name="password-autofill"></a>Autowypełnianie hasła

Przy użyciu systemu tvOS 12 użytkownicy mogą używać swoich urządzeń z systemem iOS do logowania do aplikacji systemu tvOS w jednym naciśnięciem. Ta opcja jest włączona przy użyciu kombinacji `UITextContentType` pola użycia, aby określić nazwę użytkownika i hasło, skojarzone domeny, aby ustanowić relację między aplikację dla systemu iOS i aplikacji systemu tvOS oraz preferowany fokus wymagane wdrażanie środowisk wybierz element, aby przenieść fokus po użytkownik zawiera nazwę użytkownika i hasło.

## <a name="focus-engine-enhancements"></a>Ulepszenia aparatu koncentracji uwagi

dla systemu tvOS 12 zezwala na wszystkie aplikacje, niezależnie od tego, w jaki sposób są one renderowane, wchodzić w interakcje z aparatem fokus. Do interakcji z użytkownikami za pomocą zdalnego Siri aparat fokus może służyć w dowolnej aplikacji wybierz element, podpowiedzi na zmianie fokusu możliwe i naturalny sposób zaktualizować fokus. Ta opcja jest włączona w aplikacjach niestandardowych za pośrednictwem firmy UIKit `IUIFocusItemContainer` interfejsu `UIFocusMovementHint` klasy `IUIFocusItemScrollableContainer` interfejsu oraz innych powiązanych klas i metod.

## <a name="related-links"></a>Linki pokrewne

- [Przykłady dla systemu tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [dla systemu tvOS — dla deweloperów firmy Apple (Apple)](https://developer.apple.com/tvos/)
- [What's new in systemu tvOS 12 (Apple) (wideo)](https://developer.apple.com/videos/play/wwdc2018/208/)
- [TV (Apple)](https://www.apple.com/tv/)
- Xamarin (wersja zapoznawcza) [wersji wpis w blogu](https://releases.xamarin.com/preview-release-xcode-10-beta-3/)
