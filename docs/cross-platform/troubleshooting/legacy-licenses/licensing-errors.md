---
title: Niektóre określone błędy licencjonowania
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: D26BDF2D-923B-4BC1-841A-74583155DB71
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: 8592fe5381c974e999477d0ef6ca6ebdd8b38cc4
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="some-specific-licensing-errors"></a>Niektóre określone błędy licencjonowania

> [!IMPORTANT]
> Poniższe informacje nie dotyczą użytkowników MSDN, ponieważ są one problemy specyficzne dla starszych licencji Xamarin. Jeśli jesteś użytkownikiem MSDN i błędy podobne do zobacz poniżej, spróbuj [aktualizowanie do najnowszej wersji programu Xamarin](https://developer.xamarin.com/recipes/cross-platform/ide/change_updates_channel/) & odwoływać się do tego [przewodniku opcji licencjonowania](~/cross-platform/get-started/requirements.md).



## <a name="invalid-license"></a>"Nieprawidłowa licencja"

> Nieprawidłowa licencja. Należy ponownie uaktywnić Xamarin.Android (XA9999) nie można ustalić wersji używającej licencji. (XA9010)

Te komunikaty może wystąpić w kilku różnych okolicznościach.

-   Bieżąca licencja na dysku może być poza synchronizacja przy użyciu bieżących informacji użytkownika na komputerze. [Odświeżanie plików licencji](~/cross-platform/troubleshooting/legacy-licenses/resync-licenses.md) rozwiąże ten problem w wielu przypadkach.

-   Komputer może mieć 2 aktywacji zduplikowane powodujące konflikt. Ten typ licencji jest już używany przez program Xamarin. Jeśli wystąpią problemy, które wydają się być związane z tym błędem, należy [obsługa poczty E-mail](https://www.xamarin.com/support).

-   Konto Xamarin nie może jeszcze być zaproszony do licencji Xamarin. Zobacz też: [zespołu zarządzania licencjami](~/cross-platform/troubleshooting/legacy-licenses/team-management.md).

## <a name="failed-to-load-android-entitlements"></a>"Nie można załadować Android uprawnień"

> Błąd Mono.VisualStudio.Shell.ShellPackage: 0: nie można załadować uprawnień systemu Android: Nieprawidłowa licencja. Please reactivate Xamarin.Android (XA9999) Mono.VisualStudio.Shell.ShellPackage Error: 0 : Failed license update: Invalid license. Należy ponownie uaktywnić Xamarin.Android (XA9999)

To jest w starszej wersji "Z nieprawidłową licencją" problemu z powyżej. Zastosuj te same kroki rozwiązywania problemów.

## <a name="this-version-was-released-after-your-subscription-expired"></a>"Ta wersja została wydana po wygaśnięciu subskrypcji"

> Błąd XA9000: Tej wersji wydanej po wygaśnięciu subskrypcji (2014-11-11 5:23:41: 00). (XA9000) (Mobile.Android.Utilities) XA9010 błąd: nie można ustalić wersji używającej licencji. (XA9010) (Mobile.Android.Utilities)

Te komunikaty zwykle pojawiają się w dwóch sytuacji:

-   Licencje na dysku są nieaktualne. [Odświeżanie plików licencji](~/cross-platform/troubleshooting/legacy-licenses/resync-licenses.md) rozwiąże ten problem w wielu przypadkach.

-   Xamarin subskrypcji nie jest już aktywna, a bieżąca zainstalowanej wersji programu Xamarin jest nowsza niż data wygaśnięcia subskrypcji. W takim przypadku jest poprawna poprawka [obniżyć](http://kb.xamarin.com/customer/portal/articles/1699777) do wersji programu Xamarin wydaną przed wygaśnięciem subskrypcji. Możesz wysłać wiadomość e-mail na adres [ hello@xamarin.com ](mailto:hello@xamarin.com) żądania najnowszej wersji prawidłową dla Twojej subskrypcji. Jeśli nadal zostanie wyświetlony błąd po zmiany na starszą wersję, należy spróbuj odświeżyć zbyt plików licencji.

## <a name="additional-references"></a>Dodatkowe informacje

-   [Jak odświeżyć licencji](~/cross-platform/troubleshooting/legacy-licenses/resync-licenses.md)
-   [Jak obniżyć Xamarin](http://kb.xamarin.com/customer/portal/articles/1699777-downgrading)
-   [Listę kodów błędów platformy Xamarin.Android](~/android/troubleshooting/errors.md)
-   [Listę kodów błędów MonoTouch (Xamarin.iOS)](~/ios/troubleshooting/mtouch-errors.md)

### <a name="next-steps"></a>Następne kroki
Aby uzyskać dodatkową pomoc, skontaktuj się z nami, lub jeśli ten problem pozostanie nawet po użyciu powyższe informacje, zobacz [jakie opcje pomocy technicznej są dostępne dla platformy Xamarin?](~/cross-platform/troubleshooting/support-options.md) informacji o opcjach kontaktu, masz sugestie, oraz jak Plik nowe usterki, w razie potrzeby.
