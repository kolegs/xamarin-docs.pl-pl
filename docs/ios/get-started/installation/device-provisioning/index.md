---
title: Device Provisioning dla platformy Xamarin.iOS
description: W tym dokumencie opisano sposób aprowizacji urządzenia, dzięki czemu może służyć do testowania aplikacji. Ponadto zawarto informacje, jak skonfigurować aplikację tak, aby możliwe było użycie funkcji, takich jak powiadomienia wypychane.
ms.prod: xamarin
ms.assetid: CACA5236-3C90-F6DF-FD4E-0797B61670CE
ms.technology: xamarin-ios
author: asb3993
ms.author: amburns
ms.date: 05/06/2018
ms.openlocfilehash: f0d6d2343350455a101033aced7cec0c31695503
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353233"
---
# <a name="device-provisioning-for-xamarinios"></a>Urządzenia, inicjowanie obsługi administracyjnej dla platformy Xamarin.iOS

Podczas tworzenia aplikacji platformy Xamarin.iOS istotne jest przetestowanie jej przez wdrażanie aplikacji na urządzeniu fizycznym, dodatkowo do symulatora. Tylko błędy i problemy z wydajnością można orzeczona podczas uruchamiania na urządzeniach, z powodu ograniczeń sprzętowych, takich jak pamięci ani łączności sieciowej. Aby przetestować na urządzeniu fizycznym, urządzenie musi być *aprowizowane*, i firmy Apple należy poinformować, że urządzenie będzie służyć do testowania.

Wyróżnione sekcje na poniższej ilustracji Pokaż kroki wymagane do konfigurowania aprowizacji systemu iOS:

[![](images/provisioningdiagram.png "Wyróżnione sekcje w tym obrazie Pokaż kroki wymagane do konfigurowania aprowizacji systemu iOS")](images/provisioningdiagram.png#lightbox)

Po tym następnym krokiem jest Dystrybuuj aplikację. Aby uzyskać więcej informacji na temat wdrażania, odwiedź stronę [dystrybucji aplikacji](~/ios/deploy-test/app-distribution/index.md) przewodników.

Przed wdrożeniem aplikacji do urządzenia, musisz mieć aktywną subskrypcję do programu dla deweloperów firmy Apple, *lub* użyj [aprowizacji BEZPŁATNEJ](~/ios/get-started/installation/device-provisioning/free-provisioning.md). Firma Apple oferuje dwie opcje programu:

- **Apple Developer Program** — niezależnie od tego, czy osoba lub reprezentują organizacji, Program dla deweloperów firmy Apple pozwala na tworzenie, testowanie i dystrybuowanie aplikacji.
- **Apple Developer Enterprise Program** — program przedsiębiorstwo najbardziej nadaje się do organizacji, które chcesz opracować i rozpowszechnić tylko aplikacji wewnętrznych. Uczestnicy programu przedsiębiorstwa nie mają dostępu do usługi iTunes Connect, a aplikacje utworzone przez nie może zostać opublikowany Store aplikacji.

Aby zarejestrować się na żaden z tych programów, odwiedź stronę [portalu Apple Developer Portal](https://developer.apple.com/programs/enroll/) do zarejestrowania. Należy pamiętać, że aby zarejestrować się jako deweloper firmy Apple, należy mieć [identyfikatora Apple ID](https://appleid.apple.com/). Ten przewodnik została utworzona przy założeniu, że **są** członkiem programu dla deweloperów firmy Apple.

Alternatywnie Apple wprowadziła [aprowizacji BEZPŁATNEJ](~/ios/get-started/installation/device-provisioning/free-provisioning.md) w środowisku Xcode 7, co pozwala pojedynczej aplikacji do uruchomienia na jednym urządzeniu *bez* bycia członkiem programu dla deweloperów firmy Apple. Istnieje kilka ograniczeń podczas aprowizowania w ten sposób, zgodnie z opisem [tutaj](~/ios/get-started/installation/device-provisioning/free-provisioning.md#limitations).

Każda aplikacja, która jest uruchamiana na urządzeniu musi zawierać zestaw metadanych (lub *odcisk palca*), który zawiera informacje o aplikacji i deweloperów. Apple używa odciskiem palca, aby upewnić się, że aplikacja nie zostanie naruszony w przypadku wdrażania, lub uruchomione na urządzeniu użytkownika. Jest to osiągane przez wymagania deweloperów aplikacji do zarejestrowania swojego identyfikatora Apple ID dla deweloperów i można skonfigurować identyfikator aplikacji żądanie certyfikatu i zarejestrować urządzenie, na którym aplikacja zostanie wdrożona.

W przypadku wdrażania aplikacji do urządzenia, profil aprowizacji jest również instalowany na urządzeniu z systemem iOS. Profil aprowizacji istnieje w celu zweryfikowania informacji, że aplikacja została podpisana przy użyciu w czasie kompilacji i kryptograficznie jest podpisany przez firmę Apple. Ze sobą profilu inicjowania obsługi administracyjnej i "odciskiem palca" okaże się, jeśli aplikację można wdrożyć na urządzeniu, sprawdzając:

- **Kto** (certyfikaty — aplikacja została podpisana przy użyciu klucza prywatnego, który ma odpowiadający mu klucz publiczny w profilu aprowizacji? Certyfikat powoduje również skojarzenie Deweloper z zespołu deweloperów)
- **Co** (poszczególnych identyfikator aplikacji — nie zestawu identyfikatorem pakietu w pliku Info.plist zgodne z Identyfikatorem aplikacji w profilu aprowizacji?)
- **Gdzie** (— urządzeń jest urządzeń znajdujących się w profilu aprowizacji?)

Następujące kroki, upewnij się, że wszystko, która została utworzona lub używana podczas procesu projektowania, łącznie z aplikacjami i urządzeniami, można prześledzić z kontem usługi dla deweloperów firmy Apple.

## <a name="provisioning-your-device"></a>Aprowizacja urządzenia

Istnieją dwa sposoby, aby aprowizować urządzenia z systemem iOS:

* **Automatycznie (zalecane)** — wybierz **automatycznej aprowizacji** schemat w projekcie, aby program Visual Studio automatycznie utworzyć i zarządzać tożsamości podpisywania, identyfikatory aplikacji i profilów aprowizacji. Aby uzyskać informacji na temat automatycznego zarządzania inicjowania obsługi administracyjnej, zobacz [automatycznej aprowizacji](automatic-provisioning.md) przewodnik. Jest to zalecany sposób udostępniania urządzenia z systemem iOS.

* **Ręcznie** — podpisywania tożsamości, identyfikatory aplikacji i profilów aprowizacji mogą być tworzone i zarządzane za pośrednictwem portalu dla deweloperów firmy Apple, zgodnie z opisem w [aprowizacja ręczna](manual-provisioning.md) przewodnik. Te artefakty mogą być zarządzane zgodnie z opisem w [Zarządzanie kontem Apple](~/cross-platform/macios/apple-account-management.md) przewodnik.

## <a name="provisioning-for-application-services"></a>Inicjowanie obsługi administracyjnej dla usług aplikacji

Apple oferuje szereg specjalne usługi aplikacji, nazywany również możliwości, które mogą być aktywowana dla aplikacji platformy Xamarin.iOS. Te usługi aplikacji muszą być skonfigurowane na obu portalu aprowizacji systemu iOS po **Identyfikatora aplikacji** jest tworzony i **plik Entitlements.plist** pliku część projektu aplikacji platformy Xamarin.iOS. Aby uzyskać informacje na temat dodawania aplikacji usługi do aplikacji, zapoznaj się [wprowadzenie do funkcji](~/ios/deploy-test/provisioning/capabilities/index.md) przewodnik i [Praca z uprawnieniami](~/ios/deploy-test/provisioning/entitlements.md) przewodnik.

* Utwórz identyfikator aplikacji z wymaganymi usługami aplikacji.
* Utwórz nową [profil inicjowania obsługi administracyjnej](#provisioning-your-device) zawierający ten identyfikator aplikacji.
* Ustawianie uprawnień do projektu Xamarin.iOS

## <a name="related-links"></a>Linki pokrewne

- [Bezpłatna aprowizacja](~/ios/get-started/installation/device-provisioning/free-provisioning.md)
- [Dystrybucja aplikacji](~/ios/deploy-test/app-distribution/index.md)
- [Rozwiązywanie problemów](~/ios/deploy-test/troubleshooting.md)
- [Apple — podręczniku dystrybucji aplikacji](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html)
