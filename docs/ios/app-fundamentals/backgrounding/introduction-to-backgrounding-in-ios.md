---
title: Wprowadzenie do tle w systemie iOS
description: 'W tym dokumencie opisano uruchamianie procesów w tle w systemie iOS: Stany aplikacji, metody cyklu życia aplikacji i odświeżanie w tle aplikacji.'
ms.prod: xamarin
ms.assetid: E214F2C7-E74E-46C7-B5BA-080B30D61250
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 804a99817f664989bbac67a4c662357f4ee628c5
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242280"
---
# <a name="introduction-to-backgrounding-in-ios"></a>Wprowadzenie do tle w systemie iOS

iOS reguluje ściśle przetwarzania w tle i oferuje trzy podejścia do jej wdrożenia:

-  **Rejestrowanie zadania w tle** — Jeśli aplikacja musi wykonać ważnym zadaniem, można zadawać w niej systemu iOS nie przerwać zadanie, gdy aplikacja zostanie przeniesiona na tle. Na przykład aplikacja może wymagać zakończyć logowanie użytkownika lub ukończenie pobierania dużych plików.
-  **Zarejestruj się jako aplikacja niezbędne tła** — aplikacja może być rejestrowany jako określony typ aplikacji, która ma znane określone wymagania uruchamianie procesów w tle, takie jak *Audio* , *VoIP* ,  *Akcesorium zewnętrznego* , *aplikacji Newsstand* , i *lokalizacji* . Te aplikacje są dozwolone ciągłe przetwarzania uprawnienia, tak długo, jak długo wykonują zadania, które znajdują się w parametrach typu zarejestrowanej aplikacji w tle.
-  **Włącz aktualizacje w tle** — aplikacje możesz wyzwolić aktualizacje w tle za pomocą *monitorowania Region* lub przez nasłuchiwanie *znaczących zmian lokalizacji* . Począwszy od systemu iOS 7, aplikacje mogą również rejestrować Aby zaktualizować zawartość w tle przy użyciu *pobieranie w tle* lub *zdalne powiadomienia* .


## <a name="application-states-and-application-delegate-methods"></a>Stany aplikacji i metody delegata aplikacji

Zanim przejdziemy do kodu dla przetwarzania w systemie iOS w tle, należy zrozumieć, jak uruchamianie procesów w tle ma wpływ na cykl życia aplikacji dla systemu iOS.

Cykl życia aplikacji dla systemu iOS jest kolekcją stanów aplikacji i metody służące do przechodzenia między nimi. Aplikacja przejścia między Stanami na podstawie zachowania użytkownika i backgrounding wymagań aplikacji. Poniższy diagram przedstawia przepływu:

 [![](introduction-to-backgrounding-in-ios-images/applicationlifecycle-.png "Diagram stanów aplikacji i metody delegata aplikacji")](introduction-to-backgrounding-in-ios-images/applicationlifecycle-.png#lightbox)

-  **Nieuruchomione** — aplikacja nie została jeszcze uruchomiona na urządzeniu.
-  **Uruchamianie/aktywny** — aplikacja znajduje się na ekranie i wykonuje kod na pierwszym planie.
-  **Nieaktywne** — aplikacja zostanie przerwana przez przychodzące połączenie telefoniczne, tekst lub innych zakłóceń.
-  **Backgrounded** — aplikacja zostanie przeniesiona na tle i kontynuuje wykonywanie kodu w tle.
-  **Zawieszone** — Jeśli aplikacja nie ma żadnego kodu do uruchamiania w tle lub jeśli całego kodu zostało ukończone, aplikacja będzie *zawieszone* przez system operacyjny. Procesu wstrzymania aplikacji pozostaje aktywne, ale aplikacja nie może wykonać żadnego kodu w tym stanie.
-  **Wróć do nie działa/zakończenia (rzadkich)** — od czasu do czasu, proces aplikacji jest niszczony, a aplikacja zwraca *nieuruchomiona* stanu. Dzieje się tak w sytuacjach małej ilości pamięci lub jeśli użytkownik ręcznie kończy działanie aplikacji.


Od momentu wprowadzenia Obsługa wielozadaniowości rzadko kończy bezczynności aplikacje systemów iOS i zamiast tego przechowuje swoje procesy *zawieszone* w pamięci. Utrzymywanie aktywny proces aplikacji zapewnia aplikacja uruchamia szybko przy następnym jej otwarcia przez użytkownika. Oznacza to również, swobodnie przenosić aplikacje z *zawieszone* stanu z powrotem do *Backgrounded* stanu bez rysowania zwiększenie użycia zasobów systemowych. System iOS 7 wykorzystuje tę funkcję za pomocą nowych interfejsów API, które umożliwiają aplikacjom można wstrzymać zadania w tle, gdy przejdzie w tryb uśpienia, zawartość aktualizacji bezpośrednio w tle bez interakcji z użytkownikiem i nie tylko. Omówimy nowe interfejsy API [technik uruchamianie procesów w tle dla systemu iOS](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/index.md).

## <a name="application-lifecycle-methods"></a>Metody cyklu życia aplikacji

Po zmianie stanu aplikacji dla systemu iOS powiadamia aplikację za pomocą metody zdarzeń w `AppDelegate` klasy:

-  `OnActivated` — Jest to po raz pierwszy, kiedy aplikacja jest uruchomiona, i za każdym razem, gdy aplikacja będzie działać na pierwszy plan. Jest to w tym miejscu umieść kod, który musi zostać uruchomiony za każdym razem, gdy aplikacja jest otwarty.
-  `OnResignActivation` — Jeśli użytkownik otrzymuje przerwy, takie jak tekst lub połączenia telefonicznego, ta metoda jest wywoływana, a aplikacja jest tymczasowo zdezaktywowano. Użytkownik, należy zaakceptować połączenia telefonicznego, aplikacji zostanie wysłana do tła.
-  `DidEnterBackground` -Wywoływana, gdy aplikacja przejdzie w stan backgrounded, ta metoda nadaje aplikacji około pięciu sekundach, aby przygotować się do zakończenia możliwe. Tym razem umożliwia zapisywanie danych użytkownika i zadania i usuwanie informacji poufnych na ekranie.
-  `WillEnterForeground` — W przypadku użytkownika zwraca do aplikacji backgrounded lub został wstrzymany i uruchamia go na pierwszym planie, `WillEnterForeground` jest wywoływana. Jest to czas na przygotowanie aplikacji do wykonania na pierwszym planie, ponownego wypełniania dowolny stan zapisany podczas `DidEnterBackground` .  `OnActivated` będzie można wywołać natychmiast, po ukończeniu tej metody.
-  `WillTerminate` — Aplikacja zostanie zamknięta, a jej proces zostanie zniszczony. Tylko ta metoda jest wywoływana, jeśli wielozadaniowość nie jest dostępna w wersji systemu operacyjnego lub urządzenia, jeśli jest za mało pamięci lub jeśli użytkownik ręcznie kończy backgrounded aplikacji. Należy pamiętać, że nie wywoła wstrzymania aplikacji, które zostać zakończone `WillTerminate` .


Na poniższym diagramie przedstawiono, jak Stany aplikacji i cyklem życia metod dopasowania:

 [![](introduction-to-backgrounding-in-ios-images/image2.png "Na tym diagramie przedstawiono, jak Stany aplikacji i cyklem życia metod dopasowania")](introduction-to-backgrounding-in-ios-images/image2.png#lightbox)

## <a name="user-controls-for-backgrounding-in-ios"></a>Formanty użytkownika w tle w systemie iOS

System iOS 7 wprowadzono kilka funkcji, które zapewniają użytkownikom większą kontrolę nad backgrounded stanu aplikacji. Zarówno przełącznik aplikacji, jak i ustawienie odświeżanie w tle aplikacji wpływa na cyklu życia aplikacji.

### <a name="app-switcher"></a>Przełącznik aplikacji

Przełącznik aplikacji jest ważne kontroli wprowadzonej w systemie iOS 7. Jest uruchamiane przez dwukrotne naciśnięcie **Home** znajdujący się i pokazuje aplikacje, którego procesy są aktywne:

 [![](introduction-to-backgrounding-in-ios-images/app-switcher-.png "Przenoszenie między aplikacjami za pomocą przełącznika aplikacji")](introduction-to-backgrounding-in-ios-images/app-switcher-.png#lightbox)

Za pomocą przełącznika aplikacji, użytkownicy można przewijać migawek wszystkich backgrounded i wstrzymania aplikacji. Naciskając aplikacji spowoduje uruchomienie go na pierwszy plan. Palcem w górę spowoduje usunięcie aplikacji z tła przerywa jego przetwarzania. Firma Microsoft będzie Przyjrzyj się bliżej przełącznik aplikacji w [pokaz dotyczący cyklu życia aplikacji systemu iOS](~/ios/app-fundamentals/backgrounding/application-lifecycle-demo.md) w następnej sekcji.

> [!IMPORTANT]
> Przełącznik aplikacji nie są wyświetlane różnice między aplikacjami backgrounded i wstrzymania.



### <a name="background-app-refresh-settings"></a>Ustawienia aplikacji odświeżania w tle

System iOS 7 zwiększa użytkownikowi kontrolę nad cyklem życia aplikacji, dzięki czemu użytkownicy mogą zrezygnować z uruchamianie procesów w tle dla aplikacji [zarejestrowany dla przetwarzania w tle](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/registering-applications-to-run-in-background.md). *Nie uniemożliwia uruchomienia zadania w tle aplikacji*.

Użytkownicy mogą zmieniać tego ustawienia, przechodząc do <span class="uiitem">Ustawienia > Ogólne > Odświeżanie w tle aplikacja</span> i edytowania backgrounding uprawnień dla wybranej aplikacji. Jeśli odświeżanie w tle aplikacji jest ustawiony na wyłączone, aplikacja zawieszone natychmiast po wprowadzeniu tła, a uniemożliwił podczas przetwarzania w tle:

 [![](introduction-to-backgrounding-in-ios-images/settings-.png "Ustawienia aplikacji odświeżania w tle")](introduction-to-backgrounding-in-ios-images/settings-.png#lightbox)

Deweloperzy można sprawdzić stan tła odświeżania aplikacji za pomocą `BackgroundRefreshStatus` interfejsu API. Na przykład dotyczyć [przepisu Sprawdź ustawienie tła Refresh](https://github.com/xamarin/recipes/tree/master/Recipes/ios/multitasking/check_background_refresh_setting).

Omówiliśmy podstawowe informacje o cyklu życia aplikacji systemu iOS, a funkcje kontroli cyklu życia aplikacji. Następnie Zobaczmy zarządzania cyklem życia aplikacji dla systemu iOS w działaniu.

