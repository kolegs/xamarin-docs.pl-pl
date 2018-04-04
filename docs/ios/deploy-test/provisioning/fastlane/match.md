---
title: fastlane dla systemu iOS - zgodne
ms.prod: xamarin
ms.assetid: C4A2A67E-0643-4CED-B1A9-79D65054F3CA
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 729bfb5bf19034fc5eed2350a3fe5f481224a385
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="fastlane-for-ios---match"></a>fastlane dla systemu iOS - zgodne

Tradycyjnie Inicjowanie obsługi administracyjnej urządzeń zostało wykonane przez każdy element członkowski zespół deweloperów, za pomocą środowiska Xcode lub w portalu dla deweloperów firmy Apple. Składa się z kilku kroków:

- Żądanie certyfikatu deweloperskiego
- Dodawanie urządzenia do portalu
- Tworzenie Identyfikatora aplikacji
- Tworzenie profilu inicjowania obsługi administracyjnej
- Pobieranie profilów i certyfikatów

Więcej informacji na temat czynności wymagane do konfigurowania urządzenia dla rozwoju ręcznie lub za pomocą środowiska Xcode można znaleźć w [Povisioning urządzenia](~/ios/get-started/installation/device-provisioning/index.md) przewodnik.

W tym przewodniku przedstawiono sposób użycia narzędzia fastlane zamiast za pomocą środowiska Xcode.

## <a name="installation"></a>Instalacja

Aby uzyskać informacje na temat instalowania fastlane, zapoznaj się wprowadzenie do [fastlane](~/ios/deploy-test/provisioning/fastlane/index.md#Installation) przewodnik.

<a name="whatismatch" />

## <a name="what-is-match"></a>Co to jest dopasowanie?

dopasowanie odpowiada on za tworzenie i utrzymywanie kodu podpisywanie certyfikatów i inicjowania obsługi administracyjnej profile i umożliwia zespół deweloperów systemu iOS do udostępniania jednego kodu podpisywania tożsamości dla wszystkich deweloperów.

W przypadku wdrażania aplikacji do sklepu z aplikacjami, prowadzenie testowania wersji beta lub instalowania aplikacji na urządzeniu, każdego członka zespołu deweloperów ma swoje własne tożsamości podpisywania. Może to spowodować konflikt tożsamości i profile, trzeba ręcznie utworzyć Obróć i zarządzanie profilami i identyfikatorów aplikacji.

Zamiast tego dopasowania tworzy i przechowuje wszystkie certyfikaty i profile dla Ciebie i przechowuje je w repozytorium git prywatnych. Dzięki temu wszystkich deweloperów w zespole dostępu i używać tych poświadczeń. Z kolei zapewnia to dodatkowe zabezpieczenie certyfikaty: tylko w repozytorium git prywatne, one również są zaszyfrowane z [hasło](#passphrase). Przechowywanie artefaktów w repozytorium podpisywania kodu dzięki agentów zespołu i Administratorzy, aktualizacji i Obróć certyfikatów zgodnie z potrzebami, co oznacza, że mniej jest czas dystrybucję nowych certyfikatów do każdego deweloperów.

> [!IMPORTANT]
> dopasowanie nie obsługuje obecnie profile wewnętrznych przedsiębiorstwa.

<a name="initializing" />

## <a name="initializing-your-project-with-match"></a>Inicjowanie swój projekt z dopasowania

Jeśli jesteś administratorem team Tworzenie repozytorium git prywatne, za pomocą witryną github.com lub bitbucket.com upewnij się, że w przypadku dodania wszystkich członków celu repozytorium zespołu.

Przy użyciu terminala, zmień katalog w katalogu projektu i uruchom:

    fastlane match init

Po wyświetleniu monitu wprowadź adres URL repozytorium git:

 [![](match-images/fastlane-image7.png "Wprowadź adres URL repozytorium git")](match-images/fastlane-image7.png#lightbox)

Adres URL można znaleźć i skopiować, klikając **klonowania lub pobrać** przycisk w witrynie github.com, jak przedstawiono poniżej:

[![](match-images/fastlane-image6.png "Adres URL w klonowania lub pobierania przycisk w witrynie github.com")](match-images/fastlane-image6.png#lightbox)

Inicjowanie projektu tworzy matchfile — plik tekstowy, który może być edytowany do przekazania do narzędzia do dopasowania zmiennych środowiskowych. Poniżej przedstawiono przykład matchfile:

[![](match-images/fastlane-image8.png "Przykład matchfile")](match-images/fastlane-image8.png#lightbox)

<a name="running" />

## <a name="running-match"></a>Uruchomiona dopasowania

> [!NOTE]
> zaleca fastlane zgodne przed uruchomieniem po raz pierwszy, należy rozważyć wyczyszczenie istniejące profile i certyfikaty przy użyciu [Dopasuj polecenie nuke](#using).

W zależności od środowiska wymagane, których można użyć dowolnej z poniższych poleceń, aby utworzyć nowy certyfikat i profil inicjowania obsługi administracyjnej i zapisz go w nowego repozytorium git:

    fastlane match appstore

    fastlane match adhoc

    fastlane match development

Oprócz tworzenia nowych certyfikatów i profile, przy użyciu dowolnego z tych poleceń spowoduje dodanie (lub zaktualizować, jeśli już istnieje) do Twojego repozytorium git następujące elementy:

- Folderu certyfikatów
- Folder Profile
- Plik readme podstawowe instrukcje
- Dopasowanie wersji

[![](match-images/fastlane-image9.png "Struktury projektu w repozytorium git")](match-images/fastlane-image9.png#lightbox)

Profile inicjowania obsługi administracyjnej są zainstalowane w `~/Library/MobileDevice/Provisioning Profiles`. Certyfikaty i klucze prywatne są zainstalowane bezpośrednio w Twojej łańcucha kluczy.

<a name="using" />

### <a name="using-the-nuke-command"></a>Przy użyciu `nuke` polecenia

Jeśli masz untidy certyfikatów, możesz użyć `nuke` do odwoływania certyfikatów oraz profile dla każdego środowiska przy użyciu następujących poleceń:

    fastlane match nuke

Aby można było odwołać wszystkie certyfikaty i profile inicjowania obsługi administracyjnej dla określonego środowiska:

    fastlane match nuke development

 lub

    fastlane match nuke distribution

fastlane potwierdzi pliki, które zostaną usunięte przed usunięciem żadnych czynności.

<a name="passphrase" />

### <a name="passphrase"></a>Hasło

Podczas uruchamiania `match` po raz pierwszy, użytkownik jest proszony o ustawienia hasła dla repozytorium git. Upewnij się, że Zapamiętaj hasło, ponieważ będzie on potrzebny po uruchomieniu dopasowania na innym komputerze. Jest to dodatkową warstwę zabezpieczeń: wszystkie pliki będą szyfrowane przy użyciu biblioteki OpenSSL. Każda kolejne działania `match` na nowych komputerach będzie monitować o podanie tego hasła — po początkowym wprowadzić go hasło zostaną dodane do lokalnej łańcucha kluczy.

Aby ustawić hasła do odszyfrowywania profili za pomocą zmiennej środowiskowej, użyj `MATCH_PASSWORD`.

<a name="options" />

## <a name="additional-options"></a>Dodatkowe opcje

Następujące opcje można zapewnić dodatkową pomoc przy użyciu dopasowania:

- Użyj `-–help` flagę, aby uzyskać listę wszystkich dostępnych poleceń:

        fastlane match cert --help

- Użyj `-–verbose` flagę, aby zwiększyć poziom szczegółowości danych wyjściowych:

        fastlane match --development --verbose

- Użyj `--force_for_new_devices` flagę, aby wymusić udostępnianie profile odnowienia, jeśli liczba urządzenia w portalu dla deweloperów zmieniła się "

        fastlane match development --force_for_new_devices

## <a name="related-links"></a>Linki pokrewne

- [fastlane - zgodne](https://github.com/fastlane/fastlane/blob/master/match/README.md)
