---
title: Certyfikaty i identyfikatory
description: "Ten przewodnik przeprowadzi Cię przez proces tworzenia wymagane certyfikaty i identyfikatorów, które będą wymagane do publikowania aplikacji Xamarin.Mac."
ms.topic: article
ms.prod: xamarin
ms.assetid: 393d0066-7f6f-4ac3-a48d-4b5db65bc4cd
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: a1065fb91a23827c4876654470cda5022aa1d3b8
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="certificates-and-identifiers"></a>Certyfikaty i identyfikatory

_Ten przewodnik przeprowadzi Cię przez proces tworzenia wymagane certyfikaty i identyfikatorów, które będą wymagane do publikowania aplikacji Xamarin.Mac._

## <a name="certificates-and-identifiers"></a>Certyfikaty i identyfikatory

Odwiedź stronę [Apple Developer Member Center](http://developer.apple.com) do konfigurowania komputerów Mac na potrzeby programowania. Poniżej przedstawiono menu głównym:

[![Apple Developer Member Center](certificates-identifiers-images/devcenter01.png "Apple Developer Member Center")](certificates-identifiers-images/devcenter01-large.png#lightbox)

Polecenie **certyfikaty, identyfikatory & profile** łącza:

[![Wybieranie certyfikatów, identyfikatory & profile](certificates-identifiers-images/devcenter02.png "wybranie certyfikaty, identyfikatory i profile")](certificates-identifiers-images/devcenter02-large.png#lightbox)

Przejdź do menu **Link certyfikaty** w obszarze **aplikacji Mac** sekcji:

[![Wybieranie link certyfikaty](certificates-identifiers-images/devcenter03.png "kliknąć łącze certyfikatów")](certificates-identifiers-images/devcenter03-large.png#lightbox)

Polecenie **wszystkie** łącze i kliknij  **+**  przycisk:

[![Wybranie wszystkich i dodawania nowego elementu](certificates-identifiers-images/certif01.png "wybranie wszystkich i dodawania nowego elementu")](certificates-identifiers-images/certif01-large.png#lightbox)

Z tym pobierania **certyfikaty pośrednie** (na całym świecie Developer relacji certyfikatu urzędu certyfikacji i urzędu certyfikacji identyfikator deweloperów) w razie potrzeby. Jednak powinny być automatycznie Instalatora dla deweloperów w programie Xcode.

W pozostałej części tej sekcji przedstawiono każdy cztery sekcje, aby zakończyć instalację Mac konta dewelopera.

-   **Zarejestruj identyfikator aplikacji Mac** — dewelopera należy wykonać następujące kroki dla każdej aplikacji, które zapisują.
-   **Zarejestruj macOS systemów** — jest to tylko wymagane podczas dodawania komputerów do testowania.
-   **Tworzenie certyfikatów** — tylko raz wymagana, gdy w górę seting certyfikatów i później, podczas odnawiania.
-   **Utwórz profil inicjowania obsługi administracyjnej** — Deweloper należy wykonać następujące czynności dla każdej nowej aplikacji zapisane i podczas dodawania nowych systemów.

Kliknij przycisk **omówienie** łącze u góry po lewej stronie, aby powrócić do tego menu w dowolnym momencie.

### <a name="register-mac-app-id"></a>Zarejestruj Mac identyfikator aplikacji

Deweloper musi zarejestrować Identyfikatora aplikacji dla każdej aplikacji zapisywane. Skorzystaj z poniższych czynności, aby utworzyć wpis dla podstawowego przykładową aplikację o nazwie "MacWriter".

1. Wprowadź **opis Identyfikatora aplikacji** i wybrać dowolną **usługi aplikacji** wymagających aplikacji: 

    [![Wprowadzanie opisu i aplikacji usługi](certificates-identifiers-images/devcenter04.png "wprowadzić opis i aplikacjami usług")](certificates-identifiers-images/devcenter04-large.png#lightbox)
2. Wprowadź **identyfikator pakietu** dla aplikacji i kliknij przycisk **Kontynuuj** przycisk: 

    [![Wprowadzanie Identyfikatora pakietu](certificates-identifiers-images/devcenter05.png "wprowadzanie Identyfikatora pakietu")](certificates-identifiers-images/devcenter05-large.png#lightbox)
3. Sprawdź informacje i kliknij przycisk **przesyłania** przycisk: 

    [![Weryfikowanie informacji o](certificates-identifiers-images/devcenter06.png "weryfikowania informacji o")](certificates-identifiers-images/devcenter06-large.png#lightbox)

Niektóre **usługi aplikacji** mogą wymagać dalszej konfiguracji (na przykład iCloud). Jeśli tak jest, wybierz właśnie utworzony nowy identyfikator aplikacji i kliknij przycisk **Edytuj** przycisk:

[![Edytowanie nowy identyfikator aplikacji](certificates-identifiers-images/devcenter07.png "edycji nowy identyfikator aplikacji")](certificates-identifiers-images/devcenter07-large.png#lightbox)

Kliknij, aby skonfigurować usługi iCloud **Edytuj** przycisk:

[![Konfigurowanie usługi iCloud](certificates-identifiers-images/devcenter08.png "Konfigurowanie usługi iCloud")](certificates-identifiers-images/devcenter08-large.png#lightbox)

W tym miejscu dewelopera można skonfigurować bazy danych, które mogą używać:

[![Konfigurowanie bazy danych](certificates-identifiers-images/devcenter09.png "Konfigurowanie baz danych")](certificates-identifiers-images/devcenter09-large.png#lightbox)

### <a name="register-macos-systems"></a>Rejestrowanie systemów macOS

Aby utworzyć profil inicjowania obsługi administracyjnej do testowania, deweloper trzeba swoje komputery Mac zarejestrowane. Mogą one zarejestrować maksymalnie 100 komputerów do testowania aplikacji Mac.

W Centrum deweloperów Mac wybierz **wszystkie** z **urządzeń** sekcji, a następnie kliknij przycisk  **+**  przycisk:

[![Dodawanie nowego komputera](certificates-identifiers-images/devcenter10.png "Dodawanie nowego komputera")](certificates-identifiers-images/devcenter10-large.png#lightbox)

Wprowadź **nazwa** i **UUID** komputera, aby dodać, a następnie kliknij przycisk **Kontynuuj** przycisku. Przejrzyj informacje i kliknij przycisk **zarejestrować** przycisk:

[![Wprowadzanie informacji o nowym komputerze](certificates-identifiers-images/devcenter11.png "wprowadzanie informacji o nowym komputerze")](certificates-identifiers-images/devcenter11-large.png#lightbox)

### <a name="create-certificates"></a>Tworzenie certyfikatów

Użyj sekcji certyfikatów, aby utworzyć kilka różnych typów certyfikatów, które będą używane do podpisywania aplikacji dla komputerów Mac:

[![Tworzenie nowego świadectwa](certificates-identifiers-images/certif01.png "Tworzenie nowego certyfikatu")](certificates-identifiers-images/certif01-large.png#lightbox)

Istnieją trzy typy certyfikatów:

-   **Certyfikatu deweloperskiego Mac** — opcjonalne dla rozwoju aplikacji głównej, ale wymagane jeśli dewelopera planuje używać takich funkcji jak iCloud lub wypychania powiadomień. Dewelopera należy certyfikatu deweloperskiego, aby mogli tworzyć profile inicjowania obsługi, które pozwalają im dostęp do tych funkcji.
-   **Mac App Store** — deweloper będzie wymagany certyfikat dla aplikacji i innego certyfikatu Instalatora.
-   **Identyfikator dewelopera** — certyfikaty dla aplikacji i Instalatora, jeśli wybranie do dystrybucji poza Mac App Store.

Następujące sekcje zawierają przykłady tworzenia, każdego z powyższych typów certyfikatów.

#### <a name="mac-development-certificate"></a>Certyfikatu deweloperskiego Mac

Jak wspomniano wcześniej, certyfikat rozwoju aplikacji na komputery Mac nie jest wymagane chyba że macOS funkcje, takie jak iCloud lub powiadomienia wypychane są używane.

Wykonaj następujące polecenie, aby utworzyć nowy certyfikat programowania:

1. Wybierz **programowanie Mac** i kliknąć przycisk **Kontynuuj**: 

     [![Dodawanie certyfikatu deweloperskiego](certificates-identifiers-images/certif02.png "Dodawanie certyfikatu deweloperskiego")](certificates-identifiers-images/certif02-large.png#lightbox)
2. Następnym ekranie wyjaśniono, jak Użyj dostęp łańcucha kluczy, aby utworzyć plik żądania podpisania certyfikatu do przekazania: 

    [![Ekran przekazywania dostępu łańcucha kluczy](certificates-identifiers-images/certif03.png "ekranu przekazywania dostępu łańcucha kluczy")](certificates-identifiers-images/certif03-large.png#lightbox)
3. Wybierz znaczący nazwa pospolita certyfikatu, tak aby później łatwo rozpoznać jest tworzona ostatecznego certyfikatu. Pamiętaj, którym jest zapisany plik, więc znajduje się w następnym kroku: 

     ![Eksportowanie certyfikatu](certificates-identifiers-images/image12.png "eksportowania certyfikatu")
4. Plik żądania certyfikatu (rozszerzenie `.certSigningRequest`) są zapisywane lokalnie na komputerach Mac. Należy pamiętać, w której jest zapisywany (domyślna locationis pulpitu) należy wybrać w następnym kroku: 

     [![Przekazywanie pliku certyfikatu](certificates-identifiers-images/image13.png "przekazywaniem pliku certyfikatu")](certificates-identifiers-images/image13-large.png#lightbox)
5. Kliknij przycisk **Pobierz** uzyskać certyfikat i kliknij dwukrotnie, aby zainstalować go w **łańcucha kluczy**: 

     [![Pobieranie certyfikatu deweloperskiego](certificates-identifiers-images/image15.png "pobierania certyfikatu deweloperskiego")](certificates-identifiers-images/image15-large.png#lightbox)
6. Kliknij przycisk **Pobierz** uzyskać certyfikat i kliknij dwukrotnie, aby zainstalować go w **łańcucha kluczy**. **Narzędzie certyfikat dewelopera** wyświetli certyfikaty następująco: 

     [![Narzędzie certyfikat dewelopera](certificates-identifiers-images/image16.png "Developer narzędzia do obsługi certyfikatów")](certificates-identifiers-images/image16-large.png#lightbox)
7. Zostanie również wyświetlony na **łańcucha kluczy** podobnie do następującej: 

     ![Certyfikat w narzędziu Keychain Access](certificates-identifiers-images/image17.png "certyfikatu w narzędziu Keychain Access")

Jak wcześniej wspomniano, się, że certyfikat dewelopera nie zawsze jest wymagany, chyba że deweloper implementuje macOS funkcje, takie jak iCloud i powiadomieniami wypychanymi. Jest również wymagane do utworzenia **profilu inicjowania obsługi administracyjnej programowanie**, które będą potrzebne do testowania aplikacji sklepu Mac App Store.

#### <a name="mac-app-store-certificates"></a>Certyfikaty Mac App Store

Aby zwolnić aplikację ze sklepu App store **Mac App Store** certyfikat używany do podpisywania aplikacji oraz pakiet Instalatora dla komputerów Mac należy do utworzenia.

1. Wybierz **Mac App Store** jako typ certyfikatu i kliknij przycisk **Kontynuuj** przycisk: 

    [![Tworzenie certyfikatu sklepu z aplikacjami](certificates-identifiers-images/certif04.png "tworzenia certyfikatu sklepu z aplikacjami")](certificates-identifiers-images/certif04-large.png#lightbox)
2. Wybierz typ certyfikatu, który można utworzyć (jeden z każdego typu, aby zwolnić do sklepu z aplikacjami będą potrzebne): 

    [![Wybieranie typu certyfikatu](certificates-identifiers-images/certif05.png "wybierając typ certyfikatu")](certificates-identifiers-images/certif05-large.png#lightbox)
3. Następna strona wyjaśniono, jak używać **dostęp łańcucha kluczy** Generowanie pliku żądania certyfikatu. Postępuj zgodnie z instrukcjami: 

     [![Generowanie żądania łańcucha kluczy](certificates-identifiers-images/certif06.png "generowania żądania łańcucha kluczy")](certificates-identifiers-images/certif06-large.png#lightbox)
4. Wybierz opisową **nazwa pospolita** — na przykład użyj tekst "Aplikacji magazynu aplikacji" w nazwie: 

     ![Wprowadź opisową nazwę](certificates-identifiers-images/image20.png "wprowadź opisową nazwę")
5. Plik żądania certyfikatu (rozszerzenie `.certSigningRequest`) są zapisywane lokalnie na komputerach Mac. Należy pamiętać, w której jest zapisywany (domyślna lokalizacja to pulpitu): 

     [![Zapisywanie certyfikatu](certificates-identifiers-images/image21.png "zapisywanie certyfikatu")](certificates-identifiers-images/image21-large.png#lightbox)
6. Kliknij przycisk **Pobierz** uzyskać certyfikat i kliknij dwukrotnie, aby zainstalować go w **łańcucha kluczy**: 

      [![Pobieranie certyfikatu sklepu z aplikacjami](certificates-identifiers-images/image23.png "pobranie certyfikatu sklepu z aplikacjami")](certificates-identifiers-images/image23-large.png#lightbox)
7. Kliknij przycisk **Kontynuuj** i postępuj zgodnie z dokładnie te same czynności, Pobierz inny certyfikat, teraz będzie na *Instalator*: 

     [![Wybieranie Instalator](certificates-identifiers-images/image24.png "wybranie Instalator")](certificates-identifiers-images/image24-large.png#lightbox)
8. Wybierz opisową **nazwa pospolita** — na przykład użyj tekst "Instalator sklepu z aplikacjami" w nazwie: 

     ![Ustawienie nazwy certyfikatu](certificates-identifiers-images/image25.png "ustawienie nazwy certyfikatu")
9. Plik żądania certyfikatu (rozszerzenie `.certSigningRequest`) są zapisywane lokalnie na komputerach Mac. Należy pamiętać, w której jest zapisywany (domyślna lokalizacja to pulpitu): 

     [![Przekazywanie certyfikatu](certificates-identifiers-images/image26.png "przekazywanie certyfikatu")](certificates-identifiers-images/image26-large.png#lightbox) 

     [![Pobieranie certyfikatu dystrybucji](certificates-identifiers-images/image28.png "pobranie certyfikatu dystrybucji")](certificates-identifiers-images/image28-large.png#lightbox)
10. Kliknij przycisk **Pobierz** uzyskać certyfikat i kliknij dwukrotnie, aby zainstalować go w **łańcucha kluczy**. Narzędzie certyfikat dewelopera Pokaż certyfikaty następująco: 

     [![Narzędzie certyfikat dewelopera](certificates-identifiers-images/image29.png "Developer narzędzia do obsługi certyfikatów")](certificates-identifiers-images/image29-large.png#lightbox)
11. Dwa nowe certyfikaty będą teraz widoczne w **łańcucha kluczy**: 

     [![Certyfikat w narzędziu Keychain Access](certificates-identifiers-images/image30.png "certyfikatu w narzędziu Keychain Access")](certificates-identifiers-images/image30-large.png#lightbox)

#### <a name="developer-id-certificates"></a>Certyfikaty identyfikator Developer

Samodzielnie, aby wydać aplikację Xamarin.Mac (nie wersji za pośrednictwem sklepu Apple App Store) dewelopera będzie wymagany certyfikat dewelopera identyfikator logowania do aplikacji dla wersji i instalacji.

Wykonaj następujące czynności:

1. Z **certyfikaty** sekcji, Rozpocznij od kliknij  **+**  przycisk, a następnie wybierz **Identyfikator dewelopera** przycisku radiowego: 

    [![Dodawanie Identyfikator dewelopera](certificates-identifiers-images/certif07.png "Dodawanie Identyfikator dewelopera")](certificates-identifiers-images/certif07-large.png#lightbox)
2. Kliknij przycisk **Kontynuuj** i wybrać typ Identyfikatora Developer, aby utworzyć: 

    [![Wybieranie typu ID developer](certificates-identifiers-images/certif08.png "wybierając typ Identyfikatora developer")](certificates-identifiers-images/certif08-large.png#lightbox)
3. Dwa będzie wymagane, co do podpisania aplikacji Instalatora i jeden do podpisania aplikacji. Należy zachować ostrożność podczas określania nazwy żądania certyfikatów dla tych kluczy: Użyj nazwy opisowej, zawierających tekst `Application` i `Installer` tak aby były wyróżniającej później.
4. Następnym ekranie będą zwraca szczegółowe informacje dotyczące sposobu tworzenia certyfikatu, kliknij przycisk **Kontynuuj** przycisk: 

    [![Jak utworzyć certyfikat](certificates-identifiers-images/certif09.png "Tworzenie certyfikatu")](certificates-identifiers-images/certif09-large.png#lightbox)
5. Wybierz opisową **nazwa pospolita** — na przykład użyj tekst "Developer identyfikator aplikacji" w nazwie: 

     ![Wprowadź nazwę dla certyfikatu](certificates-identifiers-images/image33.png "wprowadzania nazwy certyfikatu")
6. Plik żądania certyfikatu (rozszerzenie `.certSigningRequest`) są zapisywane lokalnie opartym na systemie Należy pamiętać, w której jest zapisywany (domyślna lokalizacja to pulpitu): 

     [![Przekazywanie certyfikatu](certificates-identifiers-images/certif10.png "przekazywanie certyfikatu")](certificates-identifiers-images/certif10-large.png#lightbox) 

     [![Pobieranie Identyfikatora developer](certificates-identifiers-images/certif11.png "pobieranie Identyfikator dewelopera")](certificates-identifiers-images/certif11-large.png#lightbox)
7. Kliknij przycisk **Pobierz** uzyskać certyfikat i kliknij dwukrotnie, aby zainstalować go w **łańcucha kluczy**.
8. Kliknij przycisk **Kontynuuj** i postępuj zgodnie z dokładnie te same czynności, Pobierz inny certyfikat, teraz będzie na *Instalatora*.
9. Wybierz opisową **nazwa pospolita** — na przykład użyj tekst "Developer identyfikator Instalator" w nazwie: 

     ![Wprowadzanie nazwą pospolitą](certificates-identifiers-images/image38.png "wprowadzania nazwa pospolita")
10. Plik żądania certyfikatu (rozszerzenie `.certSigningRequest`) są zapisywane lokalnie na komputerach Mac. Należy pamiętać, w której jest zapisywany (domyślna lokalizacja to pulpitu): 

     [![Przekazywanie certyfikatu](certificates-identifiers-images/certif10.png "przekazywanie certyfikatu")](certificates-identifiers-images/certif10-large.png#lightbox)
11. Certyfikat jest dostępna do pobrania — kliknij przycisk **Pobierz** przycisk przed kliknięciem przycisku **gotowe**: 

     [![Pobieranie certyfikatu](certificates-identifiers-images/certif11.png "pobierania certyfikatu")](certificates-identifiers-images/certif11-large.png#lightbox)
12. Kliknij przycisk **Pobierz** uzyskać certyfikat i kliknij dwukrotnie, aby zainstalować go w **łańcucha kluczy**. **Narzędzie certyfikat dewelopera** wyświetli certyfikaty następująco: 

     [![Narzędzie certyfikat dewelopera](certificates-identifiers-images/certif12.png "Developer narzędzia do obsługi certyfikatów")](certificates-identifiers-images/certif12-large.png#lightbox)
13. Następujące elementy widoczne w **łańcucha kluczy**: 

     [![Certyfikat w narzędziu Keychain access](certificates-identifiers-images/image43.png "certyfikatu w narzędziu Keychain access")](certificates-identifiers-images/image43-large.png#lightbox)


## <a name="related-links"></a>Linki pokrewne

- [Instalacja](/visualstudio/mac/installation/)
- [Witaj, przykładowe Mac](~/mac/get-started/hello-mac.md)
- [Dystrybucję aplikacji ze sklepu Mac App Store](https://developer.apple.com/devcenter/mac/checklist/)
- [Identyfikator deweloperów i strażnika](https://developer.apple.com/resources/developer-id/)
