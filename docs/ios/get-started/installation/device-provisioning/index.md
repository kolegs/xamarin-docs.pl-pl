---
title: Inicjowanie obsługi administracyjnej urządzeń
description: Po pomyślnym zainstalowaniu platformy Xamarin.iOS następny krok w rozwoju systemu iOS jest obsługi administracyjnej urządzeniu z systemem iOS. Ten przewodnik może zapoznać żądanie certyfikatów programowanie i profilach, Praca z usługi aplikacji i wdrażania aplikacji na urządzeniu.
ms.prod: xamarin
ms.assetid: CACA5236-3C90-F6DF-FD4E-0797B61670CE
ms.technology: xamarin-ios
author: asb3993
ms.author: amburns
ms.date: 05/06/2018
ms.openlocfilehash: 5265ee366c7e3c0e79e54d320d3d6eb57c2fd92d
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/07/2018
---
# <a name="device-provisioning"></a>Inicjowanie obsługi administracyjnej urządzeń

_Po pomyślnym zainstalowaniu platformy Xamarin.iOS następny krok w rozwoju systemu iOS jest obsługi administracyjnej urządzeniu z systemem iOS. Ten przewodnik może zapoznać żądanie certyfikatów programowanie i profilach, Praca z usługi aplikacji i wdrażania aplikacji na urządzeniu._

Podczas tworzenia aplikacji platformy Xamarin.iOS istotne jest przetestowanie przez wdrożenie aplikacji na urządzeniu fizycznym, ponadto w symulatorze. Tylko urządzenia błędy i problemy z wydajnością można orzeczona uruchomionej na urządzeniu, z powodu limitów sprzętu, takich jak łączności sieciowej lub pamięci. Aby przetestować na urządzeniu fizycznym, urządzenie musi być *elastycznie*, i firmy Apple należy mieć świadomość, że urządzenie będzie używane do testowania.

W sekcjach wyróżnione na poniższym obrazie Pokaż kroki wymagane do konfigurowania inicjowania obsługi administracyjnej systemu iOS:

[![](images/provisioningdiagram.png "Wyróżnione sekcje w tym obrazie Pokaż kroki wymagane do konfigurowania inicjowania obsługi administracyjnej systemu iOS")](images/provisioningdiagram.png#lightbox)

Po to następnym krokiem jest Dystrybuuj aplikację. Aby uzyskać więcej informacji dotyczących wdrażania, odwiedź stronę [dystrybucji aplikacji](~/ios/deploy-test/app-distribution/index.md) przewodników.

Przed wdrożeniem aplikacji na urządzeniu, musisz subskrypcja jest aktywna na programie dla deweloperów firmy Apple *lub* użyj [wolnego inicjowania obsługi administracyjnej](~/ios/get-started/installation/device-provisioning/free-provisioning.md). Apple oferuje dwie opcje program:

- **Programie dla deweloperów firmy Apple** — niezależnie od tego, czy osoba lub reprezentują organizacji, Apple Developer Program umożliwia tworzenie, testowanie i przeprowadzić dystrybucję aplikacji.
- **Apple Developer Enterprise Program** — program przedsiębiorstwa jest najbardziej odpowiednie dla organizacji, które mają być opracować i rozpowszechnić tylko wewnętrznych aplikacji. Elementy członkowskie programu przedsiębiorstwa nie mają dostępu do iTunes Connect i nie można opublikować aplikacje utworzone do sklepu z aplikacjami.


Aby zarejestrować się w jednej z tych programów, odwiedź stronę [portalu dla deweloperów firmy Apple](https://developer.apple.com/programs/enroll/) do zarejestrowania. Należy pamiętać, że aby zarejestrować jako deweloperów firmy Apple, konieczne [identyfikator Apple ID](https://appleid.apple.com/). Ten przewodnik został utworzony przy założeniu, że **są** członkiem programie dla deweloperów firmy Apple.

Alternatywnie, Firma Apple wprowadziła [wolnego inicjowania obsługi administracyjnej](~/ios/get-started/installation/device-provisioning/free-provisioning.md) w środowisku Xcode 7, dzięki czemu jednej aplikacji do uruchomienia na jednym urządzeniu *bez* bycia członkiem programie dla deweloperów firmy Apple. Istnieje kilka ograniczeń podczas inicjowania obsługi administracyjnej w ten sposób, zgodnie z opisem [tutaj](~/ios/get-started/installation/device-provisioning/free-provisioning.md#limitations).

Dowolnej aplikacji uruchomionej na urządzeniu musi zawierać zestawu metadanych (lub *odcisk palca*), który zawiera informacje o aplikacji i deweloperów. Apple korzysta z takim odcisku palca, aby upewnić się, że aplikacja nie jest modyfikowany podczas wdrażania, lub systemem, urządzenia użytkownika. Jest to osiągane przez wymaganie deweloperzy aplikacji, aby zarejestrować ich identyfikator Apple ID jako deweloper i można skonfigurować identyfikator aplikacji, żądanie certyfikatu i zarejestrować urządzenie, na którym aplikacja zostanie wdrożona.

W przypadku wdrażania aplikacji do urządzenia, profil inicjowania obsługi administracyjnej jest również instalowany na urządzeniu z systemem iOS. Profil inicjowania obsługi administracyjnej istnieje można zweryfikować informacje, że aplikacja została podpisana z podczas kompilacji i kryptograficznie jest podpisany przez firmę Apple. Ze sobą profilu inicjowania obsługi administracyjnej i "odciskiem palca" okaże się, jeśli aplikację można wdrożyć na urządzeniu przez sprawdzenie:

- **Kto** (certyfikaty — aplikacja została podpisana z kluczem prywatnym, który ma odpowiadający mu klucz publiczny w profilu inicjowania obsługi administracyjnej? Certyfikat powoduje również skojarzenie Deweloper z zespołu deweloperów)
- **Co** (poszczególnych identyfikator aplikacji — nie zestaw identyfikator pakietu w pliku Info.plist zgodne z Identyfikatorem aplikacji w profilu inicjowania obsługi administracyjnej)?
- **Gdzie** (— urządzeń jest zawarty w profilu inicjowania obsługi administracyjnej urządzeniu?)

Tych kroków upewnij się, że wszystko, co jest tworzony lub używane podczas procesu projektowania, w tym aplikacji i urządzeń, można prześledzić kontu deweloperów firmy Apple.

<a name="Provisioning_Profile" />

## <a name="provisioning-your-device"></a>Inicjowanie obsługi administracyjnej urządzeniu

Istnieją dwa sposoby obsługi administracyjnej urządzeniu z systemem iOS:

* **Automatycznie (zalecane)** — wybierz **automatyczne udostępnianie** schemat w projekcie mają Visual Studio automatycznie Utwórz i zarządzaj nimi Twojej tożsamości podpisywania, identyfikatorów aplikacji i profile inicjowania obsługi. Aby uzyskać informacje dotyczące automatycznego zarządzania udostępniania, zobacz [automatyczne udostępnianie](automatic-provisioning.md) przewodnik. Jest to zalecany sposób inicjowania obsługi administracyjnej urządzeniu z systemem iOS.

* **Ręcznie** — podpisywania tożsamości, identyfikatorów aplikacji i profile inicjowania obsługi można tworzyć i zarządzane za pośrednictwem portalu dla deweloperów firmy Apple, zgodnie z opisem w [ręcznego inicjowania obsługi administracyjnej](manual-provisioning.md) przewodnik. Następnie można zarządzać tych artefaktów, zgodnie z opisem w [zarządzania kontem firmy Apple](~/cross-platform/macios/apple-account-management.md) przewodnik.


<a name="appservices" />

## <a name="provisioning-for-application-services"></a>Inicjowanie obsługi administracyjnej dla usług aplikacji

Apple zapewnia szereg specjalnych usług aplikacji, nazywane również możliwości, które mogą być uaktywniony na potrzeby aplikacji platformy Xamarin.iOS. Te usługi aplikacji muszą być skonfigurowane na iOS portalu inicjowania obsługi podczas **identyfikator aplikacji** jest tworzony w **Entitlements.plist** pliku część projekt aplikacji platformy Xamarin.iOS. Informacje na temat dodawania do aplikacji usługi aplikacji, zapoznaj się [wprowadzenie do funkcji](~/ios/deploy-test/provisioning/capabilities/index.md) przewodnik i [Praca z uprawnieniami](~/ios/deploy-test/provisioning/entitlements.md) przewodnik.

* Utwórz identyfikator aplikacji z usługami wymagana aplikacja.
* Utwórz nową [profil inicjowania obsługi administracyjnej](#Provisioning_Profile) zawierający ten identyfikator aplikacji.
* Ustawianie uprawnień w projekcie platformy Xamarin.iOS

## <a name="related-links"></a>Linki pokrewne

- [Bezpłatna aprowizacja](~/ios/get-started/installation/device-provisioning/free-provisioning.md)
- [Dystrybucja aplikacji](~/ios/deploy-test/app-distribution/index.md)
- [Rozwiązywanie problemów](~/ios/deploy-test/troubleshooting.md)
- [Apple — podręczniku dystrybucji aplikacji](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html)
