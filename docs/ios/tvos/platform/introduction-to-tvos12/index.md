---
title: Wprowadzenie do systemu tvOS 12
description: Ten dokument zawiera ogólne omówienie nowych i zaktualizowanych funkcji w systemu tvOS 12, dla których Xamarin wersji zapoznawczej obecnie udostępnia powiązania C#.
ms.prod: xamarin
ms.assetid: 037F7FFF-2155-4017-B99A-839CE7EC5C9C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/25/2018
ms.openlocfilehash: 03841306ba54e511dbf2f2b86a7c17e9f4669bcd
ms.sourcegitcommit: 3f2737f8abf9b855edf060474aa222e973abda3f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/28/2018
ms.locfileid: "37067174"
---
# <a name="introduction-to-tvos-12"></a>Wprowadzenie do systemu tvOS 12

![Wersja zapoznawcza](~/media/shared/preview.png)

> [!WARNING]
> 12 obsługę systemu tvOS na platformie Xamarin jest obecnie w wersji zapoznawczej, co oznacza, że może ona zawierać usterki, nie jest zakończenie funkcji, i może ulec zmianie. Używać go tylko eksperymenty.

> [!NOTE]
> - Przegląd [wprowadzenie](~/ios/platform/introduction-to-ios12/get-started.md) przewodnik instrukcje na temat rozpocząć tworzenie iOS 12 i systemu tvOS 12 aplikacji za pomocą platformy Xamarin.
> - Aby uzyskać więcej informacji, przeczytaj [informacje o wersji](https://releases.xamarin.com/preview-release-xcode-10-beta/) Xamarin wersja preview.

Ten dokument zawiera omówienie nowych i zaktualizowanych systemu tvOS 12 funkcje, które Xamarin podglądu wersja aktualnie dostarcza powiązania C#.

## <a name="password-autofill"></a>Autowypełnianie hasła

Z systemu tvOS 12 użytkownicy mogą używać swoich urządzeń z systemem iOS do logowania do aplikacji systemu tvOS jednym naciśnięciem. Ta opcja jest włączona przy użyciu kombinacji `UITextContentType` użycia, aby określić nazwę użytkownika i hasło w polach, skojarzone domen do ustanawiania relacji między aplikację systemu iOS i aplikacji systemu tvOS, a fokus preferowanego środowiska wybierz element, aby uzyskać fokusu po użytkownika zawiera nazwę użytkownika i hasło.

## <a name="focus-engine-enhancements"></a>Ulepszenia aparatu fokus

systemu tvOS 12 zezwala na wszystkie aplikacje, niezależnie od tego, jak ich renderowaniem, wchodzić w interakcje z aparatem fokus. Za pomocą interakcji użytkownika za pomocą zdalnego Siri aparat fokus można w dowolnej aplikacji wybierz element, wskazówki na możliwe fokusu i zaktualizować naturalnie fokus. Ta opcja jest włączona w niestandardowych aplikacji za pośrednictwem jego UIKit `IUIFocusItemContainer` interfejsu `UIFocusMovementHint` klasy `IUIFocusItemScrollableContainer` interfejsu i inne powiązane klasy i metody.

## <a name="related-links"></a>Linki pokrewne

- [Przykłady dla systemu tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [systemu tvOS — deweloperów firmy Apple (Apple)](https://developer.apple.com/tvos/)
- [What's new in systemu tvOS 12 (Apple) (klip wideo)](https://developer.apple.com/videos/play/wwdc2018/208/)
- [TV (Apple)](https://www.apple.com/tv/)
- Podgląd Xamarin [informacje o wersji](https://releases.xamarin.com/preview-release-xcode-10-beta/)
