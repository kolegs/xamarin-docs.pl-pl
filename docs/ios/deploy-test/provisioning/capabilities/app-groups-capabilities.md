---
title: Funkcje grupy aplikacji
description: "Dodawanie funkcji do aplikacji często wymaga dodatkowej konfiguracji inicjowania obsługi administracyjnej. W tym przewodniku opisano ustawienia wymagane dla funkcji grupy aplikacji."
ms.topic: article
ms.prod: xamarin
ms.assetid: 0A61220B-BBAC-492B-9D3B-578986E64064
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/15/2017
ms.openlocfilehash: 687c894fbda8dc8b7c6aceb7eef73971b50de57e
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="app-group-capabilities"></a>Funkcje grupy aplikacji

_Dodawanie funkcji do aplikacji często wymaga dodatkowej konfiguracji inicjowania obsługi administracyjnej. W tym przewodniku opisano ustawienia wymagane dla funkcji grupy aplikacji._

Grupa aplikacji umożliwia różnych aplikacji (lub aplikacji i jej rozszerzenia) dostęp do lokalizacji magazynu udostępnionego pliku. Grupy aplikacji mogą być używane dla danych, takich jak:

*   [Ustawienia Apple Watch](~/ios/watchos/app-fundamentals/settings.md)
*   [NSUserDefaults udostępnionego](~/ios/app-fundamentals/user-defaults.md)
*   [Udostępnione pliki](~/ios/watchos/app-fundamentals/parent-app.md#files)

## <a name="configure-a-new-app-group"></a>Konfigurowanie nowej grupy aplikacji

Lokalizacji udostępnionej została skonfigurowana przy użyciu [grupy aplikacji](https://developer.apple.com/library/content/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19), która jest skonfigurowana w **certyfikaty, identyfikatory & profile** sekcji na [Centrum deweloperów firmy Apple](https://developer.apple.com/account/). Ta wartość musi także odwoływać się do każdego projektu Entitlements.plist.

Grupa aplikacji ma identyfikator, który zazwyczaj jest identyfikator pakietu z grupą. prefiks. Na przykład identyfikator pakietu `com.xamarin.WatchSettings` byłyby grupy aplikacji `group.com.xamarin.WatchSettings`.

Aby utworzyć nową grupę aplikacji, wykonaj następujące czynności:

1.  Odwiedź stronę firmy Apple [Centrum deweloperów systemu iOS](https://developer.apple.com/account/), otwórz Twojej **konta** i zaloguj się.
2.  Wybierz **certyfikaty, identyfikatory & profile**.
3.  W obszarze **identyfikatory** wybierz **grupy aplikacji** i kliknij przycisk  **+**  przycisk, aby utworzyć nową grupę.
4.  Wprowadź **nazwa** i **identyfikator** dla nowej grupy i kliknij przycisk **Kontynuuj** przycisk: 
   
    ![Szczegóły dodać grupy aplikacji](app-groups-capabilities-images/image52.png)

5.  Kliknij przycisk **zarejestrować** przycisk, aby utworzyć grupę i **gotowe** aby powrócić do listy zarejestrowanych grup aplikacji.

## <a name="configure-an-app-to-use-app-groups"></a>Konfigurowanie aplikacji przy użyciu grup aplikacji

Z utworzoną grupę aplikacji należy skonfigurować identyfikatorów aplikacji tak, aby aplikacje może być używany.

Wykonaj następujące czynności:

1.  Odwiedź stronę firmy Apple [Centrum deweloperów systemu iOS](https://developer.apple.com/account/)i zaloguj się przy użyciu konta dewelopera firmy Apple.
2.  Z **zasobów programu** menu, wybierz opcję **certyfikaty, identyfikatory & profile**.
3.  W obszarze **identyfikatory** wybierz **identyfikatorów aplikacji** i kliknij przycisk  **+**  przycisk, aby utworzyć nowy identyfikator.
4.  Wprowadź nazwę dla Identyfikatora aplikacji i nadaj mu identyfikator jawnej aplikacji.
5.  W obszarze **usługi aplikacji** włączyć **grupy aplikacji**, następnie kliknij przycisk Kontynuuj:

    ![Dodaj grupy aplikacji usługi aplikacji](app-groups-capabilities-images/image53.png)

6.  Sprawdź ustawienia i kliknij przycisk **zarejestrować** przycisk, aby utworzyć identyfikator aplikacji.
7.  Kliknij przycisk **gotowe** przycisk, aby powrócić do listy zarejestrowanych identyfikatorów aplikacji.
8.  Wybierz nowo utworzony identyfikator aplikacji z listy i kliknij przycisk **Edytuj** przycisk:

    ![Wybierz identyfikator aplikacji z listy](app-groups-capabilities-images/image54.png)

9.  W ramach usługi **grupy aplikacji**, kliknij przycisk **Edytuj** przycisk:

    ![Wybierz identyfikator aplikacji z listy](app-groups-capabilities-images/image55.png)

10. Wybierz grupę aplikacji, który został utworzony wcześniej, a następnie kliknij przycisk **Kontynuuj** przycisk:

    ![Dodaj grupy aplikacji](app-groups-capabilities-images/image56.png)

11. Kliknij przycisk **przypisać** przycisk, a następnie **gotowe** przycisk, aby powrócić do listy zarejestrowanych identyfikatorów aplikacji.
12. Powtórz te kroki dla wszystkich aplikacji (lub rozszerzenia), które będą używać grupy aplikacji.

## <a name="next-steps"></a>Następne kroki
 
Poniższa lista zawiera dodatkowe kroki, które muszą być podjęte:

* Użyj nazw framework w aplikacji.
* Dodawanie uprawnień wymaganych do aplikacji. Informacje dotyczące uprawnień wymaganych i sposobu dodawania ich została szczegółowo opisana w [Praca z uprawnieniami](~/ios/deploy-test/provisioning/entitlements.md) przewodnik.
* W aplikacji **iOS podpisywania pakietu**, upewnij się, że **uprawnień niestandardowych** ustawiono **Entitlements.plist**. Jest to _nie_ tworzy domyślne ustawienie dla debugowania i symulatora systemu iOS.

Jeśli wystąpią problemy związane z usługi aplikacji, zapoznaj się [Rozwiązywanie problemów](~/ios/deploy-test/provisioning/capabilities/index.md) głównym przewodniku.