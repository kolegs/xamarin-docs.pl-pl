---
title: Aktualizowanie istniejącej aplikacji interfejsu API Unified
description: Ten dokument łącza do różnych prowadnic, które opisują jak aktualizować aplikacje platformy Xamarin do interfejsu API Unified. Zawarto informacje aplikacji platformy Xamarin.iOS, Xamarin.Mac aplikacji. Typy natywne w wieloplatformowych aplikacji i projekty powiązanie aplikacji platformy Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 8A654C95-5DCA-4BB5-A582-F96C2BECC81C
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 2d09be7b85980e5c5a8eb209dc1b4ff3136c34b3
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34781634"
---
# <a name="updating-existing-apps-to-the-unified-api"></a>Aktualizowanie istniejącej aplikacji interfejsu API Unified

> [!IMPORTANT]
> Xamarin klasycznego interfejsu API, który poprzedzone interfejsu API Unified, jest przestarzała. 
> - Ostatnia wersja platformy Xamarin.iOS do obsługi klasycznego interfejsu API (monotouch.dll) została 9.10 platformy Xamarin.iOS.
> - Xamarin.Mac nadal obsługuje klasycznego interfejsu API, ale nie jest aktualizowane. Ponieważ jest przestarzały, deweloperzy należy przenieść ich aplikacji interfejsu API Unified.

## <a name="how-to-update-your-apps"></a>Jak aktualizować aplikacje

Xamarin University ma bezpłatnych wideo na **uaktualniania do systemu iOS interfejsu API Unified**. Odwiedź stronę [Xamarin University Lightning wykładów](http://university.xamarin.com/lightninglectures) Aby obejrzeć!

[ ![](updating-apps-images/xamu-video-sml.png "Xamarin University")](http://university.xamarin.com/lightninglectures)

Istnieją trzy kroki, aby zaktualizować swoje aplikacje:

1. Usuń wszystkie ostrzeżenia kompilatora na istniejący kod, szczególnie odnoszące się do interfejsów API przestarzałe.

2. Narzędzie migracji z wbudowanej w Visual Studio for Mac można zaktualizować plików projektu i przestrzenie nazw.

3. Usuń pozostałe błędy kompilatora odnoszących się do nowego [typy 64](~/cross-platform/macios/nativetypes.md) i [innych interfejsów API](~/cross-platform/macios/unified/overview.md#deprecated-typos) które uległy zmianie. Zapoznaj się z [te wskazówki](~/cross-platform/macios/unified/updating-tips.md) dodatkowe informacje na temat ręcznej aktualizacji, które mogą być wymagane.

Przewodniki określonych są dostępne dla każdego produktu w celu aktualizacji aplikacji do ujednoliconego interfejsu API i Obsługa 64-bitowego:

### <a name="xamarinios-appscross-platformmaciosunifiedupdating-ios-appsmd"></a>[Aplikacji platformy Xamarin.iOS](~/cross-platform/macios/unified/updating-ios-apps.md)

Istniejące aplikacje platformy Xamarin.iOS można zaktualizować interfejsu API Unified, za pomocą narzędzia automatycznej migracji z wbudowanej w Visual Studio dla komputerów Mac. Niektóre dodatkowe poprawki może następnie być wymagane, zgodnie z objaśnieniem w [tych instrukcji](~/cross-platform/macios/unified/updating-ios-apps.md) i [porady](~/cross-platform/macios/unified/updating-tips.md).

###  <a name="xamarinmac-appscross-platformmaciosunifiedupdating-mac-appsmd"></a>[Xamarin.Mac aplikacji](~/cross-platform/macios/unified/updating-mac-apps.md)

Istniejące aplikacje Xamarin.Mac można zaktualizować interfejsu API Unified, za pomocą narzędzia automatycznej migracji z wbudowanej w Visual Studio dla komputerów Mac. Niektóre dodatkowe poprawki może następnie być wymagane, zgodnie z objaśnieniem w [tych instrukcji](~/cross-platform/macios/unified/updating-mac-apps.md) i [porady](~/cross-platform/macios/unified/updating-tips.md).

###  <a name="xamarinforms-appscross-platformmaciosunifiedupdating-xamarin-forms-appsmd"></a>[Aplikacje platformy Xamarin.Forms](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md)

Wykonaj te instrukcje, aby zaktualizować projekt dla systemu iOS, aby za pomocą interfejsu API Unified istniejącego rozwiązania platformy Xamarin.Forms. Obsługa interfejsu API Unified tylko jest dostępna w 1.3 platformy Xamarin.Forms lub nowszym, więc [zgodnie z instrukcjami](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md) również opisano sposób aktualizowania aplikacji platformy Xamarin.Forms do wersji 1.3. Te [porady](~/cross-platform/macios/unified/updating-tips.md) może ułatwić aktualizowanie kodu natywnego iOS niestandardowe moduły renderowania lub zależności usługi.

## <a name="working-with-native-types-in-cross-platform-appscross-platformmaciosnativetypesmd"></a>[Praca z typami natywnymi w aplikacjach międzyplatformowych](~/cross-platform/macios/nativetypes.md)

W tym artykule omówiono korzystanie nowe typy natywnego interfejsu API Unified (nint, nuint, nfloat) z systemem iOS w aplikacji i platform, gdy kod jest współużytkowany z urządzeń z systemem innym niż z systemem iOS, takich jak Android i Windows Phone w systemach operacyjnych. Zapewnia wgląd w stosowania natywnych typów, a udostępnia kilka możliwych rozwiązań przypadkach gdy nowy typ musi być stosowana z kodem i platform.

## <a name="update-bindings-to-the-unified-api"></a>Aktualizacja powiązania z ujednolicony interfejs API

Klienci, którzy utworzono powiązania do bibliotek języka Objective-C, musisz zaktualizować projektu powiązania w celu odzwierciedlenia zmian w podstawowej interfejsu API (gdzie niektórych typów można tworzyć 64-bitowe).
Wykonaj te instrukcje, aby [zaktualizować istniejący projekt powiązanie do obsługi interfejsu API Unified](~/cross-platform/macios/unified/update-binding.md).

## <a name="related-links"></a>Linki pokrewne

- [Aktualizowanie aplikacji dla systemu iOS](~/cross-platform/macios/unified/updating-ios-apps.md)
- [Aktualizowanie aplikacji dla komputerów Mac](~/cross-platform/macios/unified/updating-mac-apps.md)
- [Aktualizowanie aplikacji platformy Xamarin.Forms](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md)
- [Aktualizowanie powiązania](~/cross-platform/macios/unified/update-binding.md)
- [Aktualizowanie porady](~/cross-platform/macios/unified/updating-tips.md)
- [Klasycznym vs różnice Unified API](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
