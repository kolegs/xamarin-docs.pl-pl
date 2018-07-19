---
title: Bezpłatna Aprowizacja dla aplikacji platformy Xamarin.iOS
description: W tym dokumencie opisano, jak deweloperzy platformy Xamarin.iOS mogą testowania aplikacji na urządzeniu fizycznym bez konieczności logowania do płatnej Program dla deweloperów firmy Apple.
ms.prod: xamarin
ms.assetid: A5CE2ECF-8057-49ED-8393-EB0C5977FE4C
ms.technology: xamarin-ios
author: asb3993
ms.author: amburns
ms.date: 07/16/2018
ms.openlocfilehash: 22ac17e211562eccbc49cc213e06079e77dd08c0
ms.sourcegitcommit: e98a9ce8b716796f15de7cec8c9465c4b6bb2997
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/18/2018
ms.locfileid: "39111160"
---
# <a name="free-provisioning-for-xamarinios-apps"></a>Bezpłatna aprowizacja dla aplikacji platformy Xamarin.iOS

Bezpłatna aprowizacja umożliwia deweloperom platformy Xamarin.iOS wdrażania i testowania swoich aplikacji na urządzeniach z systemem iOS **bez** bycie częścią **Apple Developer Program**.
Podczas testowania simulator są cenne i wygodne, istotne jest, aby testować aplikacje na urządzeniach fizycznych z systemem iOS, aby sprawdzić, czy one działać prawidłowo w rzeczywistych pamięci, magazynu i ograniczenia łączność w sieci.

Aby użyć aprowizacji bezpłatnej, aby wdrożyć aplikację na urządzeniu:

- Użyj środowiska Xcode, aby utworzyć niezbędne *tożsamość do podpisywania* (certyfikat dewelopera i klucz prywatny) i *profil inicjowania obsługi administracyjnej* (zawierające jawny identyfikator aplikacji i identyfikator UDID urządzenia połączone z systemem iOS).
- Użyj tożsamość do podpisywania i inicjowania obsługi profilu utworzonego przez program Xcode w programie Visual Studio for Mac lub Visual Studio 2017 do wdrażania aplikacji platformy Xamarin.iOS.

> [!IMPORTANT]
> [Automatyczna aprowizacja](~/ios/get-started/installation/device-provisioning/automatic-provisioning.md) zezwala programowi Visual Studio for Mac lub Visual Studio 2017, aby automatycznie skonfigurować urządzenie do testowania dla deweloperów. Jednak automatycznej aprowizacji nie jest zgodny z aprowizacji bezpłatnej. Aby można było korzystać z automatycznej aprowizacji, musi mieć płatne konto programu dla deweloperów firmy Apple.

## <a name="requirements"></a>Wymagania

Wdrażanie aplikacji platformy Xamarin.iOS urządzenia przy użyciu bezpłatnego inicjowania obsługi administracyjnej:

- Identyfikator Apple ID, używane nie może być podłączone do programu Apple Developer.
- Aplikacji platformy Xamarin.iOS należy użyć jawny identyfikator aplikacji, nie uniwersalny identyfikator aplikacji.
- Identyfikator pakietu używane w aplikacji platformy Xamarin.iOS muszą być unikatowe i nie były używane w innej aplikacji wcześniej. Identyfikator pakietu, wszystkie używane aprowizacji BEZPŁATNEJ **nie** być ponownie używane.
- Jeśli masz już rozesłany aplikacji, nie możesz wdrożyć tę aplikację przy użyciu aprowizacji bezpłatnej.
- Jeśli aplikacja korzysta z usługi aplikacji, musisz utworzyć profil inicjowania obsługi administracyjnej, zgodnie z opisem w [Inicjowanie obsługi administracyjnej urządzeń](~/ios/get-started/installation/device-provisioning/index.md#appservices) przewodnik. 

Przyjrzyj się [ograniczenia](#limitations) części tego dokumentu, aby uzyskać więcej informacji na temat ograniczeń związanych z bezpłatnej, inicjowanie obsługi administracyjnej i odwoływać się do [przewodniki dystrybucji aplikacji](~/ios/deploy-test/app-distribution/index.md) Aby uzyskać więcej informacji na temat dystrybuowanie aplikacji dla systemu iOS.

## <a name="testing-on-device-with-free-provisioning"></a>Testowanie na urządzeniu z usługą aprowizacji bezpłatnej

Wykonaj następujące kroki, aby przetestować aplikację platformy Xamarin.iOS przy użyciu aprowizacji bezpłatnej.

### <a name="use-xcode-to-create-a-signing-identity-and-provisioning-profile"></a>Aby utworzyć tożsamość do podpisywania i profil inicjowania obsługi administracyjnej korzystania ze środowiska Xcode

1. Jeśli nie masz identyfikatora Apple ID, [utworzyć](https://appleid.apple.com).
2. Otwórz program Xcode i przejdź do **Xcode > Preferencje**.
3. W obszarze **kont**, użyj **+** przycisk, aby dodać swojego istniejącego identyfikatora Apple ID. Powinny one wyglądać podobnie do następującego zrzutu:

    ![Preferencje Xcode — konta](free-provisioning-images/launchapp1.png "preferencje Xcode — konta")

4. Zamknij program Xcode Preferencje.
5. Podłącz urządzenie z systemem iOS, do której chcesz wdrożyć aplikację.
6. W środowisku Xcode Utwórz nowy projekt. Wybierz **Plik > Nowy > Projekt** i wybierz **aplikacja pojedynczego widoku**.
7. W oknie dialogowym Nowy projekt ustaw **zespołu** do identyfikatora Apple ID, który właśnie został dodany. Na liście rozwijanej powinny wyglądać podobnie do **imię i nazwisko (zespół osobistych)**:

    ![Utwórz nową aplikację](free-provisioning-images/launchapp2.png "Utwórz nową aplikację")

8. Po utworzeniu nowego projektu, wybierz schemat kompilacji programu Xcode, który jest przeznaczony dla urządzenia z systemem iOS (zamiast symulatora).

    ![Wybierz schemat kompilacji programu Xcode](free-provisioning-images/xcodescheme.png "wybierz schemat kompilacji programu Xcode")

9. Otwórz ustawienia projektu aplikacji, wybierając jego węzeł najwyższego poziomu w program Xcode **Nawigatora projektu**.
10. W obszarze **ogólne > tożsamości**, upewnij się, że **identyfikatora pakietu** _dokładnie pasuje_ identyfikator pakietu aplikacji platformy Xamarin.iOS.

    ![Ustaw identyfikator pakietu](free-provisioning-images/launchapp5.png "Ustaw identyfikator pakietu")

    > [!IMPORTANT]
    > Środowisko Xcode tylko będzie utworzyć profilu aprowizacji dla jawny identyfikator aplikacji, a musi być taki sam jak identyfikator aplikacji w aplikacji platformy Xamarin.iOS.
    > Jeśli będą się różnić, nie można używać wolnego inicjowania obsługi administracyjnej do wdrożenia aplikacji platformy Xamarin.iOS.

11. W obszarze **informacje o wdrożeniu**, upewnij się, że cel wdrożenia jest zgodny lub jest niższa niż wersja zainstalowana na urządzeniu z systemem iOS połączone z systemem iOS.
12. W obszarze **podpisywanie**, wybierz opcję **automatycznie zarządzała podpisywania** i wybierz zespół z listy rozwijanej:

    ![Automatycznie zarządzała podpisywania](free-provisioning-images/launchapp6.png "automatycznie zarządzała podpisywania")

    Środowisko Xcode automatycznie wygeneruje profilu aprowizacji i tożsamości podpisywania dla Ciebie. Można to wyświetlić, klikając ikonę informacji obok profil inicjowania obsługi administracyjnej:

    ![Wyświetlanie profilu inicjowania obsługi administracyjnej](free-provisioning-images/launchapp7.png "wyświetlić zawartość profilu inicjowania obsługi administracyjnej")

    > [!TIP]
    > Jeśli występuje błąd podczas próby wygenerowania profilu aprowizacji środowiska Xcode, upewnij się, że ten program Xcode kompilacji aktualnie wybrany schemat jest przeznaczony dla urządzeń połączonych z systemem iOS, a nie z symulatora.

13. Aby przetestować w środowisku Xcode, należy wdrożyć pusta aplikacja do Twojego urządzenia, klikając przycisk uruchamiania.

### <a name="deploy-your-xamarinios-app"></a>Wdrażanie aplikacji platformy Xamarin.iOS

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Połączenia urządzenia z systemem iOS z hostem kompilacji komputera Mac przy użyciu kabla USB lub [bezprzewodowo](~/ios/deploy-test/wireless-deployment.md).
2. W programie Visual Studio dla komputerów Mac **konsoli rozwiązania**, kliknij dwukrotnie **Info.plist**.
3. W **podpisywanie**, wybierz opcję **ręcznego inicjowania obsługi administracyjnej**.
4. Kliknij przycisk **podpisywanie pakietu systemu iOS...** przycisk.
5. Aby uzyskać **konfiguracji**, wybierz opcję **debugowania**.
6. Aby uzyskać **platformy**, wybierz opcję **iPhone**.
7. Wybierz **tożsamość podpisywania** utworzone przez program Xcode.
8. Wybierz **profilu aprowizacji** utworzone przez program Xcode.

    ![Ustaw tożsamość do podpisywania i profil inicjowania obsługi administracyjnej](free-provisioning-images/launchapp8.png "Ustaw tożsamość do podpisywania i profil inicjowania obsługi administracyjnej")

    > [!TIP]
    > Jeśli nie widzisz swojej tożsamości podpisywania lub poprawnego profilu inicjowania obsługi administracyjnej, może być konieczne ponowne uruchomienie programu Visual Studio dla komputerów Mac.

9. Kliknij przycisk **OK** Zapisz i Zamknij **opcje projektu**.
10. Wybierz urządzenie z systemem iOS i uruchomić aplikację.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Upewnij się, że program Visual Studio 2017 został [sparowano z hostem kompilacji Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md).
2. Połączenia urządzenia z systemem iOS z hostem kompilacji komputera Mac przy użyciu kabla USB lub [bezprzewodowo](~/ios/deploy-test/wireless-deployment.md).
3. W programie Visual Studio 2017 **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy nad projektem platformy Xamarin.iOS i wybierz **właściwości**.
4. Przejdź do **podpisywanie pakietu systemu iOS**.
5. Aby uzyskać **konfiguracji**, wybierz opcję **debugowania**.
6. Aby uzyskać **platformy**, wybierz opcję **iPhone**.
7. Wybierz **Aprowizacja ręczna**.
8. Wybierz **tożsamość podpisywania** utworzone przez program Xcode.
9. Wybierz **profilu aprowizacji** utworzone przez program Xcode.
    
    ![Ustaw tożsamość do podpisywania i profil inicjowania obsługi administracyjnej](free-provisioning-images/setprofile-w157.png "Ustaw tożsamość do podpisywania i profil inicjowania obsługi administracyjnej")

    > [!TIP]
    > Środowisko Xcode utworzona ta tożsamość do podpisywania i profil inicjowania obsługi administracyjnej i zapisał je na hoście z systemem Mac kompilacji. Są one dostępne dla programu Visual Studio 2017, ponieważ ma ono [sparowanych](~/ios/get-started/installation/windows/connecting-to-mac/index.md) z hostem kompilacji Mac. Jeśli nie są wyświetlane, może być konieczne ponowne uruchomienie programu Visual Studio 2017.

10. Zapisz i zamknij właściwości projektu.
11. Wybierz urządzenie z systemem iOS i uruchomić aplikację.

-----

## <a name="limitations"></a>Ograniczenia

Apple nałożył kilka ograniczeń w kiedy i jak można korzystać z bezpłatnego inicjowania obsługi administracyjnej do uruchamiania aplikacji na urządzeniu z systemem iOS, zapewniając, że można wdrożyć tylko do *swoje* urządzenia:

- Dostęp do usługi iTunes Connect jest ograniczona, a w związku z tym usługach, takich jak publikowanie w usłudze App Store i usługi TestFlight są niedostępne dla deweloperów, inicjowania obsługi swoich aplikacji za darmo. Konta dewelopera firmy Apple (firmowe lub osobiste) jest wymagana do dystrybucji za pośrednictwem oznacza, że Ad Hoc i wewnętrznych.
- Profile aprowizacji utworzone za pomocą swobodnego udostępniania wygasną po upływie tygodnia i tożsamości podpisywania wygaśnie po roku. 
- Ponieważ środowisko Xcode tylko utworzy profilów aprowizacji dla jawnego identyfikatory aplikacji, musisz wykonaj [powyższych instrukcji](#testing-on-device-with-free-provisioning) dla każdej aplikacji, którą chcesz zainstalować.
- Inicjowanie obsługi administracyjnej dla większości usług aplikacji nie jest możliwe za pomocą swobodnego udostępniania. W tym Apple Pay, Game Center i usługi iCloud, zakupy w aplikacjach, powiadomienia wypychane oraz portfela. Apple zawiera pełną listę możliwości [obsługiwane możliwości (iOS)](https://help.apple.com/developer-account/#/dev21218dfd6) przewodnik. Aby zainicjować obsługę aplikacji do użycia przy użyciu usług aplikacji, odwiedź stronę [Praca z funkcjami](~/ios/deploy-test/provisioning/capabilities/index.md) przewodników.

## <a name="summary"></a>Podsumowanie

Ten przewodnik zbadano zalety i ograniczenia dotyczące korzystania z bezpłatnych inicjowania obsługi administracyjnej do zainstalowania aplikacji na urządzeniu z systemem iOS. Przewodnik krok po kroku, który pokazano, jak instalowanie aplikacji platformy Xamarin.iOS za pomocą swobodnego udostępniania go podać.

## <a name="related-links"></a>Linki pokrewne

- [Inicjowanie obsługi administracyjnej urządzeń](~/ios/get-started/installation/device-provisioning/index.md)
- [Inicjowanie obsługi administracyjnej dla usług aplikacji](~/ios/get-started/installation/device-provisioning/index.md#appservices)
