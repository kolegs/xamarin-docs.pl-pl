---
title: fastlane dla systemu iOS — certyfikatu
description: 'W tym dokumencie opisano fastlane, narzędzia, która automatyzuje wiele części procesu udostępniania aplikacji dla systemu iOS: żądanie certyfikatów, Dodawanie urządzenia do portalu dla deweloperów firmy Apple, tworzenie identyfikator aplikacji i inne.'
ms.prod: xamarin
ms.assetid: 900FA6FF-F3C9-4D35-993E-B0D88E6B1883
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: fac90f4738046f91183a1acd080a0d0e480c6cbf
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34785200"
---
# <a name="fastlane-for-ios--cert"></a>fastlane dla systemu iOS — certyfikatu

> [!IMPORTANT]
> fastlane zaleca użycie [ `match` ](~/ios/deploy-test/provisioning/fastlane/match.md) narzędzie do generowania i utrzymywania certyfikatów. Użyj `cert` bezpośrednio tylko wtedy, gdy mają pełną kontrolę i zapoznaniu się o podpisywania kodu. Użyj tej akcji, aby pobrać najnowsze podpisywania kodu użyty do tożsamości.

## <a name="overview"></a>Omówienie

Tradycyjnie Inicjowanie obsługi administracyjnej urządzeń jest wykonywane przez każdego członka zespołu deweloperów, za pomocą środowiska Xcode lub w portalu dla deweloperów firmy Apple. Składa się z kilku kroków:

- Żądanie certyfikatu deweloperskiego
- Dodawanie urządzenia do portalu
- Tworzenie Identyfikatora aplikacji
- Tworzenie profilu inicjowania obsługi administracyjnej
- Pobieranie profilów i certyfikatów

Każdy z tych kroków zawiera zmiennych, które należy rozważyć, które są zależne od typu aplikacji, które tworzysz. Więcej informacji na temat czynności wymagane do konfigurowania urządzenia dla rozwoju ręcznie lub za pomocą środowiska Xcode można znaleźć w [Inicjowanie obsługi administracyjnej urządzeń](~/ios/get-started/installation/device-provisioning/index.md) przewodnik.

W tym przewodniku wprowadzono narzędzia fastlane zamiast za pomocą środowiska Xcode i opisano następujące czynności:

- [Co to jest certyfikatu?](#whatiscert)
- [Za pomocą certyfikatu](#using)
- [Dodatkowe opcje](#options)

## <a name="installation"></a>Instalacja

Aby uzyskać informacje na temat instalowania i aktualizowanie fastlane, zapoznaj się wprowadzenie do [fastlane](~/ios/deploy-test/provisioning/fastlane/index.md#Installation) przewodnik.

<a name="whatiscert" />

## <a name="what-is-cert"></a>Co to jest certyfikatu?

CERT zapewnia terminali interfejs, który tworzy nowy kod podpisywania tożsamości (często nazywane dewelopera _certyfikatu_) w przypadku środowisk zarówno rozwoju i dystrybucji.

<a name="using" />

## <a name="using-cert"></a>Za pomocą certyfikatu

Aby użyć narzędzia certyfikatu, wprowadź następujące polecenie w terminalu interfejsu wiersza polecenia:

    fastlane cert

Domyślnie spowoduje to utworzenie certyfikatu dystrybucji. Aby utworzyć certyfikatu deweloperskiego, należy przekazać `--development` flagi:

    fastlane cert --development

certyfikat będzie monitować o podanie identyfikatora Apple ID i hasła, dlatego należy wprowadzić to teraz:

[![](cert-images/fastlane-image1.png "CERT monit o podanie identyfikatora Apple ID i hasła")](cert-images/fastlane-image1.png#lightbox)

> [!IMPORTANT]
> Podano hasło po raz pierwszy jest zapisany w lokalnym macOS łańcucha kluczy. Alternatywnie, zmienne środowiskowe może służyć do przechowywania nazwy użytkownika i hasła, lub można użyć `export fastlane_DONT_STORE_PASSWORD=1` Jeśli nie chcesz je przechowywane w łańcuchu kluczy. Aby uzyskać więcej informacji na temat zarządzania poświadczeń z fastlane odwoływać się do jego fastlane [przewodnik Menedżera poświadczeń](https://github.com/fastlane/fastlane/blob/master/credentials_manager/README.md).

Identyfikator firmy Apple mogą być przekazywane jako argument za pomocą następującego polecenia:

    fastlane cert -u myemailadress@domain.com

Jeśli identyfikator Apple ID jest podłączona do wielu zespołów, będą wyświetlane w tym miejscu. Wybierz numer, który odpowiada zespół, który chcesz użyć:

[![](cert-images/fastlane-image2.png "Wybierz zespół, który chcesz użyć")](cert-images/fastlane-image2.png#lightbox)

Identyfikator zespołu również mogą zostać przekazane za pomocą flagi następujące:

    fastlane cert -l 2TU993NY9J

fastlane będzie sprawdzać, jeśli dostępnych certyfikatów podpisywania jest zainstalowanych na komputerze lokalnym, a w przypadku będzie go używać.

Jeśli nie ma certyfikatu podpisywania, będzie certyfikatu:

- Utwórz nowy klucz prywatny i podpisywania żądania
- Generowanie, Pobierz i zainstaluj certyfikat
- Zaimportuj certyfikat i klucz prywatny do łańcucha kluczy

Po osiągnięciu maksymalnej liczby tożsamości podpisywania dozwolony dla Twojego konta, zostanie zgłoszony wyjątek. Jeśli chcesz utworzyć nową tożsamość podpisywania, należy ręcznie odwołać jeden z istniejących certyfikatów za pośrednictwem Centrum deweloperów i spróbuj ponownie.

> [!NOTE]
> fastlane nie może pobrać istniejącej tożsamości podpisywania z Centrum deweloperów, jeśli nie są już z łańcucha kluczy. Jest to spowodowane klucza prywatnego istnieje tylko na komputerze, lub w wersji exported(*.p12) certyfikatu i nigdy nie w Centrum deweloperów.

<a name="options" />

## <a name="additional-options"></a>Dodatkowe opcje

Następujące opcje może służyć do obsługi dodatkowych zwracanie za pomocą certyfikatu:

- Użyj `-–help` flagę, aby uzyskać listę wszystkich dostępnych poleceń:

        fastlane cert --help

- Użyj `-–verbose` flagę, aby zwiększyć poziom szczegółowości danych wyjściowych

        fastlane cert --development --verbose


## <a name="related-links"></a>Linki pokrewne

- [fastlane — certyfikatu](https://github.com/fastlane/fastlane/blob/master/cert/README.md)
