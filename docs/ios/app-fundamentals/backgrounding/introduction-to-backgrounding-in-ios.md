---
title: Wprowadzenie do Backgrounding w systemie iOS
ms.prod: xamarin
ms.assetid: E214F2C7-E74E-46C7-B5BA-080B30D61250
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 3a82a34a37e53e0c6922ef47717a4100576c2277
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="introduction-to-backgrounding-in-ios"></a>Wprowadzenie do Backgrounding w systemie iOS

iOS reguluje ściśle przetwarzania w tle i udostępnia ją wdrożyć na trzy sposoby:

-  **Zarejestruj zadanie w tle** — Jeśli aplikacja musi ukończyć ważnym zadaniem, poproś iOS nie można przerwać zadania, gdy aplikacja zostanie przeniesiony do tła. Na przykład aplikacja może być konieczne zakończyć logowanie użytkownika lub pobierania dużych plików.
-  **Zarejestruj się jako aplikacji konieczne tła** -aplikacji można zarejestrować jako określonego typu aplikacji, która ma być znane, szczególne wymagania backgrounding, takich jak *Audio* , *VoIP* ,  *Zewnętrznych akcesoriów* , *Newsstand* , i *lokalizacji* . Te aplikacje mogą ciągłego tła przetwarzania uprawnienia tak długo, jak są one wykonywanie zadań, które znajdują się w parametrach typu zarejestrowanej aplikacji.
-  **Włącz aktualizacje w tle** -aplikacji może wyzwolić aktualizacje w tle z *monitorowania Region* lub nasłuchiwanie *znaczących zmian lokalizacji* . Począwszy od systemów iOS 7, aplikacje można również zarejestrować Aby zaktualizować zawartość w tle przy użyciu *pobrać tła* lub *zdalnego powiadomienia* .


## <a name="application-states-and-application-delegate-methods"></a>Stany aplikacji i metody delegata aplikacji

Zanim firma Microsoft Poznaj kod przetwarzania w systemie iOS w tle, musimy zrozumieć wpływa na sposób backgrounding cyklem życia aplikacji systemu iOS.

Cykl życia aplikacji systemu iOS jest kolekcją Stany aplikacji i metody służące do przechodzenia między nimi. Aplikacja przejścia między Stanami na podstawie zachowania użytkownika i backgrounding wymagań aplikacji. W poniższym diagramie przedstawiono przepływu:

 [![](introduction-to-backgrounding-in-ios-images/applicationlifecycle-.png "Diagram Stany aplikacji i metody delegata aplikacji")](introduction-to-backgrounding-in-ios-images/applicationlifecycle-.png#lightbox)

-  **Nie działa** — aplikacja nie ma jeszcze uruchomić na urządzeniu.
-  **Uruchamianie/aktywny** -aplikacji znajduje się na ekranie i wykonuje kod na pierwszym planie.
-  **Nieaktywne** -aplikacji zostało przerwane przez przychodzących połączeń telefonicznych, tekst lub innych zakłóceń.
-  **Backgrounded** — aplikacja zostanie przeniesiony do tła i kontynuuje wykonywanie kodu w tle.
-  **Zawieszone** — Jeśli aplikacja nie ma żadnego kodu do uruchomienia w tle lub jeśli całego kodu zostało ukończone, aplikacja będzie *zawieszone* przez system operacyjny. Proces wstrzymania aplikacji jest aktywne, ale aplikacja nie może wykonać kod w tym stanie.
-  **Zwraca do nie działa/zakończenia (rzadkich)** — od czasu do czasu, proces aplikacji zostanie zniszczony i aplikacja powróci do *nieuruchomiona* stanu. Dzieje się w sytuacjach ilości pamięci lub jeśli użytkownik ręcznie zakończenie aplikacji.


Od momentu wprowadzenia Obsługa wielozadaniowości rzadko kończy bezczynności aplikacji systemu iOS i zamiast tego przechowuje ich procesów *zawieszone* w pamięci. Utrzymywanie aktywności procesu aplikacji zapewnia aplikacji uruchamia szybkie przy następnym otwarciu go. Oznacza to również aplikacje mogą swobodnie przemieszczać się z *zawieszone* stanu do *Backgrounded* stanu bez rysowania zasobów systemowych. System iOS 7 wykorzystuje tej funkcji z nowych interfejsów API, które umożliwiają aplikacjom wstrzymać zadania w tle, gdy urządzenie przechodzi w stan uśpienia, zawartość aktualizacji bezpośrednio w tle bez interakcji z użytkownikiem i inne. Omówimy nowe interfejsy API [techniki Backgrounding iOS](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/index.md).

## <a name="application-lifecycle-methods"></a>Metody cyklem życia aplikacji

Po zmianie stanu aplikacji dla systemu iOS powiadamia aplikację za pomocą metody zdarzeń w `AppDelegate` klasy:

-  `OnActivated` — Ta metoda jest wywoływana przy pierwszym uruchomieniu aplikacji i za każdym razem, gdy aplikacja wróci do planu. Jest to miejsce, aby umieść kod, który musi być uruchamiane przy każdym otwarciu aplikacji.
-  `OnResignActivation` — Jeśli użytkownik otrzymuje przerwania, takich jak tekst lub połączeń telefonicznych, ta metoda jest wywoływana, a aplikacja jest tymczasowo dezaktywowane. Użytkownik powinien zaakceptować rozmowy telefonicznej, aplikacja zostanie wysłane do tła.
-  `DidEnterBackground` -Wywoływane, gdy aplikacja przechodzi do stanu backgrounded, ta metoda powoduje aplikacji około pięciu sekundach przygotowanie możliwe rozwiązania. Użyj tego czasu do zapisania danych użytkownika i zadań, a następnie usuń poufne informacje na ekranie.
-  `WillEnterForeground` — Gdy użytkownik zwraca do aplikacji backgrounded lub został wstrzymany i uruchamia go do pierwszego planu, `WillEnterForeground` jest wywoływana. Jest to czas na przygotowania aplikacji do wykonania na pierwszym planie przez przywrócenie z magazynu trwałego każdy stan zapisane podczas `DidEnterBackground` .  `OnActivated` zostanie wywołana natychmiast po zakończeniu tej metody.
-  `WillTerminate` — Aplikacja zostanie zamknięta, a jego procesu zostanie zniszczony. Tylko ta metoda jest wywoływana, jeśli wielozadaniowości nie jest dostępna w wersji systemu operacyjnego lub urządzenia, jeśli jest za mało pamięci lub jeśli użytkownik ręcznie kończy backgrounded aplikacji. Należy pamiętać, nie wywoła wstrzymania aplikacji, które uzyskać zakończone `WillTerminate` .


Na poniższym diagramie przedstawiono, jak Stany aplikacji i metod cyklu życia dopasowania:

 [![](introduction-to-backgrounding-in-ios-images/image2.png "Ten diagram przedstawia, jak Stany aplikacji i metod cyklu życia dopasowania")](introduction-to-backgrounding-in-ios-images/image2.png#lightbox)

## <a name="user-controls-for-backgrounding-in-ios"></a>Formanty użytkownika Backgrounding w systemie iOS

System iOS 7 wprowadzono kilka funkcji, aby umożliwić użytkownikom większą kontrolę nad backgrounded stanu aplikacji. Zarówno przełącznik aplikacji, jak i odświeżanie aplikacji w tle ustawienia mają wpływ na cyklem życia aplikacji.

### <a name="app-switcher"></a>Przełącznik aplikacji

Przełącznik aplikacji jest funkcją kontroli ważne wprowadzone w systemie iOS 7. Jest uruchamiane przez dwukrotne **Home** przycisk i przedstawia aplikacji, w której procesy są aktywności:

 [![](introduction-to-backgrounding-in-ios-images/app-switcher-.png "Przenoszenie między aplikacjami za pomocą przełącznik aplikacji")](introduction-to-backgrounding-in-ios-images/app-switcher-.png#lightbox)

Z tym przełącznikiem aplikacji, użytkowników można przewijać migawek wszystkich backgrounded i wstrzymania aplikacji. Naciskając aplikacji uruchamia go na pierwszym planie. Szybko przesuwając w górę spowoduje usunięcie aplikacji z tła zakończenia procesu. Firma Microsoft podejmie bliższe spojrzenie na przełącznik aplikacji w [pokaz cyklem życia aplikacji systemu iOS](~/ios/app-fundamentals/backgrounding/application-lifecycle-demo.md) w następnej sekcji.

> [!IMPORTANT]
> Przełącznik aplikacji nie są wyświetlane różnica między aplikacjami backgrounded i wstrzymania.



### <a name="background-app-refresh-settings"></a>Ustawienia odświeżania aplikacji w tle

System iOS 7 zwiększa kontroli użytkownika nad cyklem życia aplikacji, dzięki czemu użytkownicy mogą zrezygnować z backgrounding dla aplikacji [zarejestrowany do przetwarzania w tle](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/registering-applications-to-run-in-background.md). *Nie zapobiega aplikacji zadania w tle*.

Użytkownicy mogą zmieniać tego ustawienia, przechodząc do <span class="uiitem">Ustawienia > Ogólne > Odśwież aplikację tła</span> i edytowania backgrounding uprawnienia dla wybranej aplikacji. Jeśli odświeżanie aplikacji w tle jest ustawiony na wyłączone, aplikacja zostanie wstrzymane natychmiast po wprowadzeniu tła i uniemożliwił podczas przetwarzania w tle:

 [![](introduction-to-backgrounding-in-ios-images/settings-.png "Ustawienia odświeżania aplikacji w tle")](introduction-to-backgrounding-in-ios-images/settings-.png#lightbox)

Deweloperzy mogą sprawdzić stan aplikacja odświeżania w tle z `BackgroundRefreshStatus` interfejsu API. Na przykład dotyczą [przepisu Sprawdź ustawienie tła Refresh](https://developer.xamarin.com/recipes/ios/multitasking/check_background_refresh_setting/).

Firma Microsoft zostały omówione podstawy iOS cyklem życia aplikacji i funkcji do kontroli cyklu życia aplikacji. Następnie Zobaczmy cyklem życia aplikacji systemu iOS w akcji.

