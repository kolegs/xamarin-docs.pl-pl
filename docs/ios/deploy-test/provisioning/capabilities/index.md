---
title: Praca z funkcjami
description: Dodawanie funkcji do aplikacji często wymaga dodatkowej konfiguracji inicjowania obsługi administracyjnej. W tym przewodniku opisano ustawienia wymagane dla wszystkich możliwości.
ms.prod: xamarin
ms.assetid: 98A4676F-992B-4593-8D38-6EEB2EB0801C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/15/2017
ms.openlocfilehash: ff918ac104e7eab4f2e8c0d0be46df240138c97c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="working-with-capabilities"></a>Praca z funkcjami

_Dodawanie funkcji do aplikacji często wymaga dodatkowej konfiguracji inicjowania obsługi administracyjnej. W tym przewodniku opisano ustawienia wymagane dla wszystkich możliwości._

Apple oferuje deweloperom _możliwości_, często znane jako _usługi aplikacji_, jak oznacza rozszerzanie funkcjonalności i rozszerzenie zakresu jakie aplikacje dla systemu iOS. Umożliwiają deweloperom dodać lepszą integrację funkcji platformy do swojej aplikacji, takich jak: możliwość transakcjach pieniężnych inicjowanych z poziomu aplikacji, usług dodatkowych urządzeń, takich jak Siri i inne.
Te możliwości mogą służyć z projektami platformy Xamarin.iOS. Poniżej przedstawiono pełną listę usług:

* Grupy aplikacji
* Skojarzone domen
* Ochrona danych
* Centrum gier
* HealthKit
* HomeKit
* Konfiguracja zasobów bezprzewodowych
* iCloud
* Funkcja zakupu w aplikacji
* Dźwięk Inter-App
* Płatności firmy Apple
* Portfela
* Powiadomienia wypychane
* Osobista sieć VPN
* Używanie programu Siri
* Mapy
* Tryby tła
* Udostępnianie łańcucha kluczy
* Rozszerzenia sieci
* Konfiguracja punktu aktywnego
* Obsługa wielu ścieżek
* Odczytywanie tagu NFC


Możliwości można włączyć za pomocą programu Visual Studio dla komputerów Mac lub ręcznie w portalu dla deweloperów firmy Apple. Pewnych funkcji, takich jak portfela, Apple Pay i usługi iCloud wymagają dodatkowej konfiguracji identyfikatorów aplikacji.

W tym przewodniku objaśniono sposób włączania każdej z tych usług aplikacji w aplikacji w obu Visual Studio dla komputerów Mac i ręcznie za pośrednictwem Centrum deweloperów, włączając dodatkowe ustawienia, które mogą być wymagane. 

## <a name="adding-app-services"></a>Dodawanie usługi aplikacji

Aby korzystać z możliwości, aplikacja musi mieć prawidłowy profil inicjowania obsługi administracyjnej, zawierający identyfikator aplikacji w usłudze poprawne włączone. Tworzenie ten profil inicjowania obsługi administracyjnej albo można automatycznie w programie Visual Studio dla komputerów Mac lub ręcznie w Centrum deweloperów firmy Apple.

W tej sekcji wyjaśniono, jak włączyć większość funkcji za pomocą programu Visual Studio for Mac na automatyczne udostępnianie lub Centrum deweloperów. Istnieją pewne możliwości, takie jak portfela, iCloud Apple Pay i grupy aplikacji, które wymagają dodatkowej konfiguracji. Te opisano szczegółowo w przewodnikach sąsiadujących.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

> [!IMPORTANT]
> Nie wszystkie funkcje, które mogą być dodawane lub zarządzanych w programie Visual Studio dla komputerów Mac. Poniższa lista zawiera obsługiwane możliwości:
>
>* HealthKit 
>* HomeKit 
>* Osobista sieć VPN 
>* Konfiguracja zasobów bezprzewodowych 
>* Dźwięk Inter-App 
>* SiriKit 
>* Hotspot 
>* Rozszerzenia sieci 
>* Odczytywanie tagu NFC
>* Obsługa wielu ścieżek 
>
>Powiadomienia wypychane, Game Center zakupu w aplikacji, map, udostępnianie łańcucha kluczy, skojarzone domen i funkcji ochrony danych nie są obecnie obsługiwane. Aby dodać tych funkcji, użyj ręcznego inicjowania obsługi administracyjnej i postępuj zgodnie z instrukcjami [Centrum deweloperów](#devcenter) sekcji.


Możliwości są dodawane do **Entitlements.plist** w programie Visual Studio dla komputerów Mac. Aby dodać możliwości, wykonaj następujące czynności:

1. Otwórz **Info.plist** pliku aplikacji systemu iOS i upewnij się, **automatycznie zarządzać podpisywania** jest zaznaczone. Postępuj zgodnie z instrukcjami [automatyczne udostępnianie](~/ios/get-started/installation/device-provisioning/automatic-provisioning.md) przewodnika, jeśli potrzebujesz pomocy:

    ![Automatycznie zarządzać opcja podpisywania](images/manage-signing.png)

2. Otwórz **Entitlements.plist** plik i wybierz możliwości, które chcesz dodać:

    ![Dodawanie funkcji do pliku entitlements.plist](images/image17.png)

    Wybieranie możliwości wykonuje dwie czynności:
    * Dodaje tej funkcji do Identyfikatora aplikacji
    * Dodaje parę klucza i wartości uprawnienia do pliku Entitlements.plist.

    Visual Studio for Mac, informujący, gdy te zadania zostały przeprowadzone przez wyświetlanie następujący komunikat o pomyślnym:

    ![Dodawanie funkcji do pliku entitlements.plist](images/image18.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Zgodnie z obecnie nie jest obsługiwane dla automatycznego inicjowania obsługi w programie Visual Studio 2017, należy użyć [Centrum deweloperów](#devcenter) utworzyć identyfikator aplikacji z usługami właściwej aplikacji.

-----

<!--
<a name="xcode" />

## Xcode

Xamarin developers can also use Xcode to quickly create a provisioning profile with a suitable App ID. This process, described below, can be used for any app service in the list:

1.  Open Xcode and create a ‘dummy’ project. Give the dummy project the same name as your Xamarin.iOS project. The bundle identifier should be identical to the bundle identifier of your Xamarin.iOS project:

    ![Xcode Create Project](images/image1.png)

2.  Ensure **Automatically manage signing** is selected:

    ![Automatically manage signing selection](images/image2.png)

3.  Once the app has been created, go to the tab named **Capabilities**:

    ![Xcode Capabilities tab](images/image3.png)

4.  Browse to the capability that you wish to add, and move the switch to the **ON** position.
5.  This will create a provisioning profile with an App ID that contains the capability and adds the entitlement to the profile.
6.  In Visual Studio for Mac / Visual Studio, browse to **Project Options > Bundle Signing** and set the provisioning profile to the one that was just created in Xcode:

    ![Visual Studio for Mac Project Options](images/image4.png)
-->

<a name="devcenter" />

## <a name="developer-center"></a>Centrum deweloperów

Przy użyciu Centrum deweloperów jest procesem dwóch krok, który wymaga tworzenia identyfikator aplikacji, a następnie utwórz profil inicjowania obsługi administracyjnej przy użyciu tego Identyfikatora aplikacji. Te kroki są szczegółowo opisane poniżej.

### <a name="creating-an-app-id-with-an-app-service"></a>Tworzenie Identyfikatora aplikacji z usługi aplikacji

1.  Przejdź do [Centrum deweloperów firmy Apple](https://developer.apple.com/account) na Mac (kompilacji hosta mac Jeśli za pomocą komputera z systemem windows) i zaloguj się.
2.  Wybierz **certyfikaty identyfikatory i profile**:

    ![Centrum deweloperów firmy Apple](images/image5.png)

3.  W obszarze **identyfikatory**, wybierz pozycję **identyfikatorów aplikacji**:

    ![Wybór identyfikator aplikacji w Centrum deweloperów](images/image6.png)

4.  Naciśnij klawisz **+** przycisk w prawym górnym rogu, aby utworzyć nowy identyfikator aplikacji.
5.  Wprowadź opis Identyfikatora aplikacji, wybierz jawny identyfikator aplikacji, a następnie wprowadź identyfikator pakietu w formacie `com.domain.appname`. Ten identyfikator pakietu, powinien być zgodny z Identyfikatorem pakietu w projekcie:

    ![Dodawanie szczegółów identyfikator aplikacji](images/image7.png)

6.  W obszarze **usługi aplikacji** wybierz usługę lub usługi, które są wymagane w aplikacji:

    ![Strona wybierania usług aplikacji](images/image8.png)

7.  Naciśnij klawisz **kontynuować**.
8.  Potwierdź swój identyfikator aplikacji. Każda usługa będzie w jednym z następujących stanów: **włączone**, **wyłączone**, lub **konfigurowalne**, jak pokazano poniżej. Jeśli jest **włączona,** jest gotowa do użycia w profilu inicjowania obsługi administracyjnej. Jeśli jest **konfigurowalne**, dodatkowe ustawienia jest wymagana dla tej funkcji. Te dodatkowe kroki są opisane bardziej szczegółowo w kolejnych sekcjach.

    ![Potwierdzenie identyfikator aplikacji](images/image9.png)

9.  Kliknij przycisk **zarejestrować** , a następnie **gotowe**. Nowo utworzony identyfikator aplikacji powinien być wyświetlany na liście identyfikatorów aplikacji systemu iOS.


<a name="provisioningprofile" />

### <a name="creating-a-provisioning-profile"></a>Tworzenie profilu inicjowania obsługi administracyjnej

Teraz Utwórz profil inicjowania obsługi administracyjnej, który zawiera ten identyfikator aplikacji. Wykonaj następujące czynności:

1.  W Centrum deweloperów firmy Apple, przejdź do **profile inicjowania obsługi > wszystkie**:

    ![Inicjowanie obsługi profilu](images/image10.png)

2.  Naciśnij klawisz **+** przycisk w prawym górnym rogu, aby utworzyć nowy profil inicjowania obsługi administracyjnej.
3.  Wybierz typ profilu inicjowania obsługi administracyjnej, który należy i kliknij przycisk **Kontynuuj**:

    ![Wybieranie profilu inicjowania obsługi administracyjnej](images/image11.png)

4.  Z listy rozwijanej wybierz identyfikator aplikacji, który został utworzony w powyższych krokach, a następnie naciśnij klawisz **Kontynuuj**:

    ![Wybór identyfikator aplikacji](images/image12.png)

5.  Wybierz certyfikaty używane do podpisywania aplikacji i naciśnij klawisz **Kontynuuj**:

    ![Wybór certyfikatu](images/image13.png)

6.  Wybierz urządzenia, które mają zostać uwzględnione w tym profilu i naciśnij klawisz **Kontynuuj**:

    ![Wybierz urządzenia, profilu inicjowania obsługi administracyjnej](images/image14.png)

7.  Nadaj nazwę profilu, dzięki czemu można zidentyfikować i naciśnij klawisz **Kontynuuj** do generowania profil:

    ![Nazwa profilu inicjowania obsługi administracyjnej](images/image15.png)

8.  Naciśnij klawisz **Pobierz** przycisk, aby go pobrać, a następnie kliknij dwukrotnie plik w wyszukiwanie, aby zainstalować profil inicjowania obsługi administracyjnej.

9. Jeśli używasz programu Visual Studio dla komputerów Mac upewnij się, że **automatycznie zarządzać podpisywania** opcja jest cofnąć wybranego w **Info.plist** pliku

10. W programie Visual Studio for Mac / Visual Studio, przejdź do **opcje projektu > podpisywania pakietu** i ustaw ten, który został właśnie utworzony profil inicjowania obsługi administracyjnej:

    ![Visual Studio for Mac opcje projektu](images/image16.png)

> [!IMPORTANT]
> Może również należy ustawić uprawnienia kluczach w pliku Entitlement.plist i ochrony prywatności w pliku Info.plist. Więcej informacji na temat tych uprawnień znajduje się w [Praca z uprawnieniami](~/ios/deploy-test/provisioning/entitlements.md) przewodnik.

<a name="nextsteps" />

## <a name="next-steps"></a>Następne kroki

Po włączeniu możliwości po stronie serwera jest nadal pracy, które należy wykonać, aby umożliwić aplikacji w taki sposób korzystać z funkcji. Poniższa lista zawiera dodatkowe kroki, które muszą być podjęte:

*   Użyj nazw framework w aplikacji.
*   Dodawanie uprawnień wymaganych do aplikacji. Informacje dotyczące uprawnień wymaganych i sposobu dodawania ich została szczegółowo opisana w [wprowadzenie do uprawnień](~/ios/deploy-test/provisioning/entitlements.md) przewodnik.

<a name="troubleshooting" />

## <a name="troubleshooting-capabilities"></a>Funkcje do rozwiązywania problemów

Poniższa lista zawiera szczegóły dotyczące niektórych typowych problemów, które można utworzyć roadblocks podczas opracowywania aplikacji z włączoną usługą aplikacji.

-   Upewnij się, że poprawny identyfikator została prawidłowo utworzona i zarejestrowana w **certyfikaty, identyfikatory & profile** części portalu dla deweloperów firmy Apple.
-   Upewnij się, że usługa zostały dodane do Identyfikatora aplikacji (lub rozszerzenia) i że usługa jest skonfigurowana do używania aplikacji grupy/sprzedawcy identyfikator/kontenera utworzone powyżej w **certyfikaty, identyfikatory & profile** z portalu dla deweloperów firmy Apple.
-   Upewnij się, że zostały zainstalowane profile inicjowania obsługi i identyfikatorów aplikacji i że aplikacja **Info.plist** (w projekcie Xamarin) jest przy użyciu jednej z identyfikatorów aplikacji skonfigurowanych powyżej.
-   Upewnij się, że aplikacja **Entitlements.plist** pliku (w projekcie Xamarin) jest włączona usługa poprawne.
-   Upewnij się, że odpowiednie klucze prywatności są ustawione w pliku info.plist
-   W aplikacji **iOS podpisywania pakietu**, upewnij się, że **uprawnień niestandardowych** ustawiono **Entitlements.plist**. Jest to _nie_ tworzy domyślne ustawienie dla debugowania i symulatora systemu iOS.

<a name="summary" />

## <a name="summary"></a>Podsumowanie

W tym przewodniku wyjaśniono możliwości, lub _usługi aplikacji_i opisano sposób ich można włączyć w programie Visual Studio i w Centrum deweloperów firmy Apple. On również szczegółowe sposobu konfigurowania usług bardziej skomplikowany, takich jak portfela, iCloud Apple Pay i grupy aplikacji. Na koniec objętych usługą kolejne kroki w celu pobierania ustawiania i proste opcji rozwiązywania problemów.