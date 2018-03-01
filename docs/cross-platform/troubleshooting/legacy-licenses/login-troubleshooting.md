---
title: "Dlaczego nie można zalogować się do platformy Xamarin w programie Visual Studio lub Visual Studio dla komputerów Mac?"
ms.topic: article
ms.prod: xamarin
ms.assetid: 6EF2B553-5DF9-41CC-B68F-77A7F64572FA
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: b50969e4c1d75c0bd79c08223dd959241dcb229a
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="why-cant-i-log-into-xamarin-in-visual-studio-or-visual-studio-for-mac"></a>Dlaczego nie można zalogować się do platformy Xamarin w programie Visual Studio lub Visual Studio dla komputerów Mac?

> [!IMPORTANT]
> W tym przewodniku nie ma zastosowania do większości użytkowników MSDN, ponieważ nie są wymagane do własnych lub zaloguj się do kont Xamarin, chyba że za pomocą [przechowywania składników Xamarin](https://components.xamarin.com/) lub [programu Visual Studio for Mac (Mac)](~/cross-platform/get-started/requirements.md). Posiadaczy licencji MSDN powinny odwoływać się do tego [przewodniku opcji licencjonowania](~/cross-platform/get-started/requirements.md) zamiast tego.



## <a name="overview"></a>Omówienie
Istnieje kilka typowych przyczyn, które mogą uniemożliwić logowanie na koncie platformy Xamarin w IDE. Poniżej opisano znane problemy i poprawki.

### <a name="finding-the-login-screen"></a>Znajdowanie ekran logowania

Odwołania znajdują się w tym miejscu ekrany logowania:

- Visual Studio for Mac
   - Prawym górnym rogu ekranu powitalnego
   - **Visual Studio for Mac > konto** (Mac)
   - **Narzędzia > konto** (system Windows)
- Visual Studio
   - **Narzędzia > Konto Xamarin**

## <a name="the-ide-is-connecting-but-the-account-screen-isnt-showing-correct-login-information"></a>IDE nawiązuje połączenie, ale ekranu konta nie jest wyświetlany informacji o logowaniu poprawne

Ten problem zwykle został rozwiązany przez ręczne licencji ponownej synchronizacji.
Postępuj zgodnie z instrukcjami w tym artykule: [jak to zrobić ręcznie resyncronize Xamarin licencji?](~/cross-platform/troubleshooting/legacy-licenses/resync-licenses.md)

## <a name="invalid-account-information"></a>Nieprawidłowe informacje o koncie

Jeśli przejdziesz do witryny sieci Web Xamarin [strony logowania](https://store.xamarin.com/Login?from=%2faccount%2f), można spróbować zalogować się przy użyciu poświadczeń bieżącego konta.
Strona zawiera również łącza resetowania hasła i utworzyć nowe konto.

## <a name="account-is-valid-but-the-ide-cant-connect"></a>To konto jest prawidłowe, ale nie można połączyć z IDE

Jest to zazwyczaj spowodowane tym, gdy Zapora lub inne ustawienia zabezpieczeń blokować IDE uzyskanie dostępu do wymaganych punktów końcowych.
Serwery aktywacji muszą uzyskać dostęp do następujących czynności:

> activation.xamarin.com store.xamarin.com auth.xamarin.com

Jednak kilka innych punktów końcowych są ważne w przypadku programowanie ogólne procesy, takie jak aktualizacje, pobieranie pakietów NuGet, itd. W związku z tym zaleca się upewnij się, że *wszystkie* punktów końcowych są dodawane z [przewodnik konfiguracji zapory Xamarin](~/cross-platform/get-started/installation/firewall.md).

### <a name="ios-in-xamarin-studio-windows"></a>System iOS w programie Xamarin Studio w systemie Windows
Programowanie dla systemu iOS nie jest obsługiwana w programie Xamarin Studio dla systemu Windows. Podczas uzyskiwania dostępu do ekranu logowania w tym miejscu, licencja z systemem iOS nie będą wyświetlane.

Zamiast tego zaloguj się za pomocą platformy Xamarin Studio (Mac) lub Visual Studio za pomocą starszej wersji licencji. Należy pamiętać, że użytkownicy MSDN w systemie Windows nie trzeba zalogować.

## <a name="additional-support"></a>Dodatkowe wsparcie

Jeśli powyższych scenariuszy nie opisano sytuacji lub rozwiązać ten problem, zapoznaj się one [opcje pomocy technicznej](https://www.xamarin.com/support).
