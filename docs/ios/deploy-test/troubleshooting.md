---
title: Rozwiązywanie problemów
description: Porady i wskazówki, aby utworzyć wdrożenie smooth
ms.prod: xamarin
ms.assetid: 65286D09-F74D-4F22-B6CD-D1BCD7FC7992
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/23/2017
ms.openlocfilehash: fe7425bbf6440317cc856d2c727874298f66bc33
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="troubleshooting"></a>Rozwiązywanie problemów

## <a name="code-signing--provisioning"></a>Podpisywanie kodu i inicjowania obsługi

Podpisywanie kodu i inicjowania obsługi administracyjnej z systemem iOS może być dość nieodpowiednich i dlatego jest ważne upewnić się, że kod podpisywanie certyfikatów i profile aprowizacji znajdują się w kolejności.

* Duże zespoły powinien zaniechania przy użyciu przycisku "Napraw problem" w programie Xcode, przedstawiono poniżej:

    [![](troubleshooting-images/fixissue.png "Okno dialogowe rozwiązać problemy")](troubleshooting-images/fixissue.png#lightbox)

    Spowoduje to utworzenie nowego profile inicjowania obsługi administracyjnej i certyfikatów. W najlepszym spowoduje to utworzenie profilu inicjowania obsługi administracyjnej za każdym razem, gdy członek zespołu kliknięciu, co powoduje disorganization przy użyciu profilów. W najgorszym przypadku będzie on odwoływanie certyfikatów dla wszystkich pozostałych użytkowników w firmie, powodując swoje aplikacje przestaną działać.

* Utrzymywanie łańcucha kluczy dostępu podzielone i usunąć wygasłe certyfikaty i profile. Certyfikaty przedsiębiorstwa ostatnio przez 3 lata, podczas gdy inne ostatni tylko roku. Nie można odnowić certyfikatów, więc niezbędnych do utworzenia nowego świadectwa tuż przed starych wygaśnie. Upewnij się, że odwoływanie i Usuń stare certyfikaty i ponowne podpisywanie aplikacji za pomocą nowych certyfikatów.

* Usuń stare profile inicjowania obsługi administracyjnej jako nowe pliki są zainstalowane. Oznacza to, że Visual Studio dla komputerów Mac nie jest w stanie, który ma zdecydować, który profil do użycia. Aby to osiągnąć, najpierw upewnij się usunąć profil w Centrum deweloperów firmy Apple, a następnie przejdź do *Preferencje > Twoje konto > Wyświetl szczegóły...* . Wybierz profil inicjowania obsługi administracyjnej, a następnie kliknij przycisk **Pokaż w wyszukiwarce**. Będzie to ujawnić położenie profil w systemie plików Mac, gdzie następnie można usunąć, za pomocą wyszukiwania.

* Upewnij się, że wszystkie wymagane certyfikaty i odpowiadające im klucze prywatne są dostępne. Dla każdego zespołu należy certyfikat dewelopera (w celu instalowania aplikacji na własnych urządzeniach), a certyfikat dystrybucji (w celu instalowania na innych urządzeniach)

* Uruchom ponownie Xcode i Visual Studio for Mac / Visual Studio po zainstalowaniu nowego profilu inicjowania obsługi administracyjnej lub certyfikatu.


## <a name="testflight"></a>TestFlight

Czasami testowania nie działa dość jako sprawnie zgodnie z planem.  Rozwiąż problemy z TestFlight może ułatwić następujące czynności:

- TestFlight jest dostępna tylko dla aplikacji przeznaczonych dla systemu iOS 8 i nowszych.

- Musi istnieć *profil dystrybucji sklepu z aplikacjami* z uprawnień w wersji beta.

- **Nowe przesyłanie aplikacji dla systemu iOS** okna musi zawierać te same informacje, jakie włączono w aplikacji **Info.plist**, i musi zostać wypełnione wszystkie sekcje. Ikony musi być określona dla aplikacji przed przesłaniem do TestFlight.

- Podczas przekazywania nowej kompilacji potrwa gdzieś od 1 do 5 minut, aż do kompilacji jest wyświetlana w iTunes Connect.

- [TestFlight Beta testowanie przełącznika](~/ios/deploy-test/testflight.md#beta-testing) muszą być włączone dla każdej wersji aplikacji.

- Każdy element członkowski zespół deweloperów, który jest również wewnętrzny tester musi mieć **wewnętrzny Tester** przełącznik włączony.

- Użytkownicy, którzy należą do lub własnego iTunes innego konta Connect nie może być wewnętrzny testerów. Tylko ich może zostać dodana jako zewnętrzne testerów.

- Użytkownicy wewnętrznych i zewnętrznych są dodawani, zaznaczone i zaproszony oddzielnie. Każda lista musi być zarządzane oddzielnie.

- Apple musi zatwierdzić każdej kompilacji dystrybuowanej do zewnętrznego testerów. Zmiana wersji kompilacji nowego przeglądu beta przez firmę Apple jest wymagana. Zmiana numeru kompilacji przeglądu jest opcjonalna.

- Metadane, należy dodać do kompilacji, które są dystrybuowane do zewnętrznego testerów. To jest możliwy, klikając numer kompilacji w **Moje aplikacje > wersja wstępna**.

- Tylko dwie kompilacje można przesłać do przeglądu każdego dnia. Ponieważ zmiana wersji wymusza przeglądu, oznacza to tej wersji, które numery można zmienić tylko dwa razy dziennie.

<a name="Automatically_copy_app_bundles_back_to_Windows" />

## <a name="automatically-copy-app-bundles-back-to-windows"></a>Automatycznie skopiować .app pakietów systemu Windows

[!include[](~/ios/includes/copy-app-bundle-to-windows.md)]
