---
title: "Bezpłatne, inicjowania obsługi administracyjnej"
description: "W wersji Xcode 7 firmy Apple pochodzi ważne zmiany dla wszystkich systemów iOS i Mac deweloperzy — bez udostępniania."
ms.topic: article
ms.prod: xamarin
ms.assetid: A5CE2ECF-8057-49ED-8393-EB0C5977FE4C
ms.technology: xamarin-ios
author: asb3993
ms.author: amburns
ms.date: 03/19/2017
ms.openlocfilehash: 26ac40360b4e706180f4154f4fddcd9c992ad94b
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/15/2018
---
# <a name="free-provisioning"></a>Bezpłatne, inicjowania obsługi administracyjnej

_W wersji Xcode 7 firmy Apple pochodzi ważne zmiany dla wszystkich systemów iOS i Mac deweloperzy — bez udostępniania._

Bezpłatne udostępniania umożliwia deweloperom wdrażanie aplikacji platformy Xamarin.iOS na urządzeniu z systemem iOS **bez** jest częścią żadnego **programie dla deweloperów firmy Apple**. Jest to bardzo korzystne dla deweloperów, jako testowania na urządzeniu umożliwia wiele korzyści, za pośrednictwem testowania w symulatorze, w tym między innymi do pamięci, magazynu, łączności sieciowej między innymi.

Inicjowanie obsługi bez konta deweloperów firmy Apple muszą być wykonywane za pomocą Xcode, co powoduje *tożsamość podpisywania* (zawierający certyfikat dewelopera i klucz prywatny), a *profilu inicjowania obsługi administracyjnej* () zawierające jawny identyfikator aplikacji i UDID urządzenia połączone z systemem iOS).

## <a name="requirements"></a>Wymagania

Przeprowadzać wdrażania z Xamarin.iOS aplikacji na urządzeniu z wolnego udostępniania muszą używać Xcode 7 lub nowszy.

**Identyfikator Apple ID używanego nie musi być połączony do dowolnego programie dla deweloperów firmy Apple.**

Identyfikator pakietu używane w aplikacji muszą być unikatowe i nie były używane w innej aplikacji wcześniej. Identyfikator pakietu, wszystkie używane z wolnego inicjowania obsługi administracyjnej może nie ponownie można użyć ponownie. Jeśli aplikacja ma już rozesłany, nie może obsłużyć tej aplikacji z inicjowaniem obsługi administracyjnej wolne. 

Zapoznaj się [prowadzi dystrybucji aplikacji](~/ios/deploy-test/app-distribution/index.md) Aby uzyskać więcej informacji.

Jeśli Twoja aplikacja korzysta z usługi aplikacji, a następnie musisz utworzyć profil inicjowania obsługi administracyjnej zgodnie z opisem w [Inicjowanie obsługi administracyjnej urządzeń](~/ios/get-started/installation/device-provisioning/index.md#appservices) przewodnik. Widać więcej ograniczeń [odpowiedniej sekcji](#limitations) poniżej.


## <a name="a-namelaunching--launching-your-app"></a><a name="launching" /> Uruchamianie aplikacji

Do korzystania z bezpłatnej obsługi administracyjnej do wdrażania aplikacji na urządzeniu, będzie używać Xcode do tworzenia tożsamości podpisywania i profile inicjowania obsługi administracyjnej, a następnie użyje programu Visual Studio for Mac lub Visual Studio wybierz poprawnego profilu do podpisania aplikacji z. Wykonaj przewodnik krok po kroku poniżej, aby to zrobić:

1. Jeśli nie ma identyfikatora Apple ID, utworzyć w [appleid.apple.com](https://appleid.apple.com/account).
2. Otwórz środowisko Xcode i przejdź do **Xcode > Preferencje**.
3. W obszarze **kont**, użyj  **+**  przycisk, aby dodać do istniejącego identyfikatora firmy Apple. Powinien wyglądać podobnie jak na poniższym zrzucie ekranu:

  [![](free-provisioning-images/launchapp1.png "Xcode preferencji konta")](free-provisioning-images/launchapp1.png#lightbox)

4. Podłącz urządzenie z systemem iOS, które chcesz wdrożyć i Utwórz nowy projekt pusty widok pojedynczego systemu iOS w programie Xcode. Ustaw **zespołu** listy rozwijanej Apple ID, który został dodany. Powinna być w formacie podobnym do `your name (Personal Team - your Apple ID)`:

  [![](free-provisioning-images/launchapp2.png "Utwórz tożsamość podpisywania")](free-provisioning-images/launchapp2.png#lightbox)

5. W obszarze **ogólne > tożsamości** sekcji, upewnij się, że identyfikator pakietu jest zgodny _dokładnie_ identyfikator pakietu aplikacji platformy Xamarin.iOS i upewnij się, cel wdrożenia pasuje do lub jest niższy niż urządzenia z systemem iOS połączonych. Ten krok jest niezwykle ważne, ponieważ Xcode tylko utworzy profilu inicjowania obsługi administracyjnej z jawny identyfikator aplikacji:

  [![](free-provisioning-images/launchapp5.png "Tworzenie profilu inicjowania obsługi administracyjnej z jawny identyfikator aplikacji")](free-provisioning-images/launchapp5.png#lightbox)

6. W sekcji podpisywanie wybierz **automatycznie zarządzać podpisywania** i wybierz z listy rozwijanej zespół:

  [![](free-provisioning-images/launchapp6.png "Wybierz automatycznie zarządzać podpisywania i wybierz z listy rozwijanej zespół")](free-provisioning-images/launchapp6.png#lightbox)

7. Poprzedni krok automatycznie wygeneruje profilu inicjowania obsługi administracyjnej i tożsamości podpisywania dla Ciebie. Można wyświetlić tego klikając ikonę informacji obok profilu inicjowania obsługi administracyjnej:

  [![](free-provisioning-images/launchapp7.png "Widok profil inicjowania obsługi administracyjnej")](free-provisioning-images/launchapp7.png#lightbox)

8. Aby przetestować w środowisku Xcode, należy wdrożyć pusta aplikacja do Twojego urządzenia, klikając przycisk uruchamiania.

9. Zwracane ze środowiskiem IDE z tym samym urządzeniu podłączony i kliknij prawym przyciskiem myszy nazwę projektu platformy Xamarin.iOS otworzyć **opcje projektu** okna dialogowego. Przejdź do sekcji podpisywania pakietu z systemem iOS i jawnie ustaw Twojej tożsamości podpisywania i profil inicjowania obsługi administracyjnej:

  [![](free-provisioning-images/launchapp8.png "Ustaw tożsamość podpisywania i profil inicjowania obsługi administracyjnej")](free-provisioning-images/launchapp8.png#lightbox)

Jeśli nie widzisz Twojej tożsamości podpisywania lub poprawnego profilu inicjowania obsługi administracyjnej w środowiskiem IDE, może być konieczne uruchom go ponownie.


## <a name="a-namelimitations-limitations"></a><a name="limitations" />Ograniczenia

Apple nałożył różne ograniczenia na kiedy i jak można wolnego inicjowania obsługi administracyjnej do uruchamiania aplikacji na urządzeniu z systemem iOS, zapewniając, że można wdrożyć tylko do *Twojego* urządzenia. Są one wymienione w tej sekcji.

Dostęp do programu iTunes Connect również jest ograniczony i w związku z tym usługach, takich jak publikowania w sklepie App Store i TestFlight są niedostępne dla deweloperów za darmo udostępniania swoich aplikacji. Konto Apple Developer (Enterprise lub osobiste) jest wymagana do dystrybucji za pośrednictwem oznacza Ad Hoc i wewnętrznych.

Inicjowanie obsługi administracyjnej Profile utworzone w ten sposób wygaśnie po tydzień tożsamości podpisywania po roku. Ponadto profile aprowizacji będzie można tworzyć tylko z jawnym identyfikatorów aplikacji i dlatego należy postępować zgodnie z instrukcjami [powyżej](#launching) dla każdej aplikacji, które chcesz zainstalować.

Inicjowanie obsługi administracyjnej dla większości aplikacji usługi również nie jest możliwe z inicjowaniem obsługi administracyjnej wolne. Możliwości obejmują:

- Płatności firmy Apple
- Centrum gier
- iCloud
- Zakup w aplikacji
- Powiadomienia wypychane
- Portfela (został Passbook)

Pełna lista jest dostarczonymi przez firmę Apple w ich [obsługiwane możliwości](https://developer.apple.com/library/prerelease/ios/documentation/IDEs/Conceptual/AppDistributionGuide/SupportedCapabilities/SupportedCapabilities.html#//apple_ref/doc/uid/TP40012582-CH38-SW1) przewodnik. Aby udostępnić aplikację do użycia z usługi aplikacji, odwiedź stronę [Praca z funkcjami](~/ios/deploy-test/provisioning/capabilities/index.md) przewodników.


## <a name="summary"></a>Podsumowanie

Ten przewodnik zawiera zbadane, korzyści i ograniczenia dotyczące korzystania z bezpłatnej inicjowania obsługi administracyjnej do zainstalowania aplikacji na urządzeniu z systemem iOS. On również nawiązaniem, krok po kroku, w celu zainstalowania aplikacji platformy Xamarin.iOS przy użyciu wolnego inicjowania obsługi administracyjnej.

## <a name="related-links"></a>Linki pokrewne

- [Inicjowanie obsługi administracyjnej urządzeń](~/ios/get-started/installation/device-provisioning/index.md)
- [Inicjowanie obsługi administracyjnej dla usług aplikacji](~/ios/get-started/installation/device-provisioning/index.md#appservices)
