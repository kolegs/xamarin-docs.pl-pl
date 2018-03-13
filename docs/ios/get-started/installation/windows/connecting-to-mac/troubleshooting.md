---
title: "Rozwiązywanie problemów z połączenia"
description: "Ten przewodnik zawiera kroki rozwiązywania problemów, które mogą wystąpić przy użyciu nowego Menedżera połączeń, w tym łączności i problemy z protokołem SSH."
ms.topic: article
ms.prod: xamarin
ms.assetid: A1508A15-1997-4562-B537-E4A9F3DD1F06
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 5263d32ace14eb803bfd65b6a9b2ea5992ee1413
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/12/2018
---
# <a name="connection-troubleshooting"></a>Rozwiązywanie problemów z połączenia

_Ten przewodnik zawiera kroki rozwiązywania problemów, które mogą wystąpić przy użyciu nowego Menedżera połączeń, w tym łączności i problemy z protokołem SSH._

## <a name="log-file-location"></a>Lokalizacja pliku dziennika

- **Mac** — ~/Library/Logs/Xamarin-[główne. POMOCNICZA]
- **Windows** — %LOCALAPPDATA%\Xamarin\Logs

Może znajdować się pliki dziennika, przechodząc do **pomocy &gt; Xamarin &gt; Zip dzienniki** w programie Visual Studio.


## <a name="wheres-the-xamarin-build-host-app"></a>Gdy kompilacji usługi Xamarin hosta aplikacji

Hosta kompilacji Xamarin ze starszych wersji platformy Xamarin.iOS nie jest już wymagane. Visual Studio teraz automatycznie wdraża agenta za pośrednictwem zdalnej nazwy logowania i uruchamia go w tle. Nie ma żadnych dodatkowych aplikacji, który będzie uruchamiany na komputerach Mac lub Windows.


## <a name="troubleshooting-remote-login"></a>Rozwiązywanie problemów z logowania zdalnego

> [!IMPORTANT]
> Te kroki rozwiązywania problemów jest przeznaczone głównie dla problemów, które mają miejsce podczas początkowej konfiguracji na nowy system.  Jeśli ma wcześniej zostały używany połączenia pomyślnie w określonym środowisku, a następnie połączenie nagle lub sporadycznie przestaje działać, możesz (w większości przypadków) pominąć wprost do sprawdzania, czy pomaga w następujących: 
>   * Kasowanie procesu pozostałość zgodnie z poniższym opisem w obszarze [błędy z powodu istniejące procesy hosta usługi kompilacji](#errors). 
>   * Wyczyść agentów, zgodnie z opisem w [wyczyszczenie brokera, IDB, kompilacji i agenci projektanta](#clearing), przewodowej połączenia internetowego i Połącz bezpośrednio za pomocą adresu IP, zgodnie z opisem w [nie można nawiązać połączenia MacBuildHost.local. Spróbuj ponownie. ](#tryagain).  
> Jeśli żadna z tych opcji rozwiązać ten problem, następnie postępuj zgodnie z instrukcjami wyświetlanymi w [kroku 9](#stepnine) do pliku nowy raport o usterce.

1. Sprawdź, czy są zainstalowane opartym na systemie niezgodne wersje platformy Xamarin.iOS Zrobić to z programu Visual Studio 2017 upewnij się, że znajdują się na **stabilna** kanału dystrybucji w programie Visual Studio dla komputerów Mac. W programie Visual Studio 2015 lub starszym, upewnij się, są w tym samym kanale dystrybucji na obu IDEs.
    * W programie Visual Studio dla komputerów Mac, przejdź do **programu Visual Studio for Mac > sprawdzić aktualizacje...**  do wyświetlania lub zmieniania **kanału aktualizacji**.
    * W programie Visual Studio 2015 i starszych Sprawdź kanału dystrybucji w obszarze **Narzędzia > Opcje > Xamarin > innych**.

2. Upewnij się, że **logowania zdalnego** jest włączona na komputerach Mac. Ustaw dostęp dla **tylko ci użytkownicy**i upewnij się, że użytkowników komputerów Mac znajduje się na liście lub grupy:

    [![](troubleshooting-images/troubleshooting-image1.png "Ustaw dostęp tylko dla tych użytkowników")](troubleshooting-images/troubleshooting-image1.png#lightbox)

3. Sprawdź, czy zapora zezwala na połączenia przychodzące za pośrednictwem portu 22 — wartość domyślna dla SSH:

    [![](troubleshooting-images/troubleshooting-image2.png "Sprawdź, czy zapora zezwala na połączenia przychodzące za pośrednictwem portu 22")](troubleshooting-images/troubleshooting-image2.png#lightbox)

    Jeśli zostało wyłączone **automatycznie zezwala podpisanego oprogramowania do odbierania przychodzących połączeń**, OS X wyświetli się okno dialogowe podczas parowania pytaniem umożliwić `mono-sgen` lub `mono-sgen32` do odbierania przychodzących połączeń. Należy kliknąć przycisk **Zezwalaj** w tym oknie dialogowym:

    [![](troubleshooting-images/troubleshooting-image4a.png "W tym oknie dialogowym kliknij przycisk Zezwalaj")](troubleshooting-images/troubleshooting-image4a.png#lightbox)

4. Upewnij się, możesz zalogować się do konta użytkownika na tym Mac i mają aktywną sesję graficznego interfejsu użytkownika.

5. Upewnij się, że łączysz się z komputera Mac za pomocą _username_ zamiast _imię i nazwisko_. Dzięki temu można uniknąć znane ograniczenie pełnych nazw, które zawierają znaki akcentowane.

    Można znaleźć sieci _username_ uruchamiając `whoami` w **Terminal.app**.

    Na przykład w poniższym zrzucie ekranu, nazwa konta będzie **amyb** i nie **oparzenia Amy**:

    [![](troubleshooting-images/troubleshooting-image5a.png "Uzyskiwanie nazwy konta z terminala aplikacji")](troubleshooting-images/troubleshooting-image5a.png#lightbox)


6. Sprawdź, czy adres IP używany dla komputerów Mac jest poprawna. Można znaleźć adres IP w obszarze **preferencjach systemowych > Udostępnianie > logowania zdalnego** na komputerach Mac.

    [![](troubleshooting-images/troubleshooting-image17.png "Adres IP w aplikacji preferencjach systemowych")](troubleshooting-images/troubleshooting-image17.png#lightbox)

7. Po potwierdzeniu adres MAC, spróbuj `ping` na ten adres w `cmd.exe` w systemie Windows:

        ping 10.1.8.95

    Jeśli polecenie ping nie powiedzie się, a następnie Mac nie jest _rutowalne_ z komputera z systemem Windows. Ten problem będzie musiał zostać rozwiązane na poziomie sieci lokalnej konfiguracji między komputerami 2. Upewnij się, że obie maszyny są w tej samej sieci lokalnej.

8. Następnie należy sprawdzić, czy `ssh` klient z OpenSSH może pomyślnie połączyć się z Mac z systemem Windows. Jednym ze sposobów zainstalować ten program jest zainstalowanie [Git dla systemu Windows](https://git-for-windows.github.io/). Następnie należy uruchomić **Git Bash** wiersza polecenia i podjęcie próby `ssh` w do komputera Mac za pomocą nazwy użytkownika i adres IP adresów:

        ssh amyb@10.1.8.95

<a name="stepnine" />

9. Jeśli **powiedzie się w kroku 8**, można spróbować uruchomić prostego polecenia, takich jak `ls` za pośrednictwem połączenia:

        ssh amyb@10.1.8.95 'ls'

    Powinno to listę zawartości katalogu macierzystego na komputerach Mac. Jeśli `ls` polecenie działa poprawnie, ale nadal nie połączenia programu Visual Studio, możesz sprawdzić [znane problemy i ograniczenia](#knownissues) sekcji o komplikacji specyficzne dla platformy Xamarin. Jeśli żaden z nich są zgodne problemu, skontaktuj się z [nowy raport o usterkach](https://bugzilla.xamarin.com/newbug) i Dołącz dzienniki opisana w sekcji [Sprawdź pliki dziennika, pełne](#verboselogs).

10. Jeśli **krok 8 kończy się niepowodzeniem**, można uruchomić następujące polecenie w terminalu dla komputerów Mac, aby sprawdzać, czy serwer SSH akceptują _żadnych_ połączeń:

        ssh localhost

11. Jeśli krok 8 nie powiedzie się, ale **kroku 10 zakończy się powodzeniem**, problem jest najprawdopodobniej port 22 na hoście kompilacji Mac nie jest niedostępny z systemu Windows z powodu konfiguracji sieci. Ewentualne problemy z konfiguracją obejmują:

    - Ustawienia zapory OS X jest brak zezwolenia połączenia. Należy sprawdzić krok 3.

        Czasami konfiguracji dla aplikacji, OS X zapory można także zakończyć w nieprawidłowym stanie, gdy ustawienia wyświetlane w preferencjach systemowych nie odzwierciedlają zachowanie rzeczywistych. Usunięcie pliku konfiguracji (**/Library/Preferences/com.apple.alf.plist**) i ponowne uruchomienie komputera może pomóc przywrócić domyślne zachowanie. Jednym ze sposobów usuwania pliku polega na przejściu **/Library/Preferences** w obszarze **Przejdź &gt; przejdź do folderu** w wyszukiwania, a następnie przenieść **com.apple.alf.plist** pliku Kosz.

    - Ustawienia zapory jednego routery między Mac i komputerze z systemem Windows blokuje połączenia.

    - Systemu Windows jest brak zezwolenia na połączenia wychodzące do zdalnego port 22. Będzie to rzadko. Istnieje możliwość konfigurowania Zapory systemu Windows, aby nie zezwalaj na połączenia wychodzące, ale ustawienie domyślne to wszystkich połączeń wychodzących.

    - Host kompilacji Mac jest brak zezwolenia dostęp do portu 22 z wszystkie hosty zewnętrznej za pośrednictwem `pfctl` reguły. Jest to mało prawdopodobne, jeśli nie wiadomo, że skonfigurowano `pfctl` w przeszłości.

12. Jeśli krok 8 zakończy się niepowodzeniem i **kroku 10 kończy się niepowodzeniem**, a następnie problem jest prawdopodobne, że proces serwera SSH dla komputerów Mac nie jest uruchomiona lub nie jest skonfigurowana do zezwalania bieżącego użytkownika do logowania się w. W takim przypadku należy sprawdzić ustawienia zdalnej nazwy logowania w kroku 2 przed zbadać wszelkie możliwości bardziej skomplikowane.

<a name="knownissues" />

## <a name="known-issues-and-limitations"></a>Znane problemy i ograniczenia

> [!NOTE]
> Ta sekcja dotyczy tylko, jeśli nawiązano już połączenie pomyślnie hosta kompilacji Mac w Mac nazwę użytkownika i hasło przy użyciu OpenSSH SSH klienta, zgodnie z opisem w kroku 8 i 9 powyżej.

### <a name="invalid-credentials-please-try-again"></a>"Nieprawidłowych poświadczeń. Spróbuj ponownie."

Znane przyczyny:

- **Ograniczenie** — ten błąd może wystąpić podczas próby logowania do hosta kompilacji przy użyciu konta _imię i nazwisko_ Jeśli nazwa zawiera znak akcentowane. Jest to ograniczenie z [biblioteki SSH.NET](https://sshnet.codeplex.com/) Xamarin używany dla połączenia SSH. **Obejście**: zobacz krok 5 powyżej.

### <a name="unable-to-authenticate-with-ssh-keys-please-try-to-log-in-with-credentials-first"></a>"Nie można uwierzytelnić z kluczy SSH. Spróbuj najpierw zaloguj się przy użyciu poświadczeń"

Znane Przyczyna:

- **Ograniczenia zabezpieczeń SSH** — ten komunikat najczęściej oznacza, że jeden z plików lub katalogów w pełni kwalifikowana ścieżka **$HOME/.ssh/authorized\_klucze** Mac ma uprawnienia do zapisu dla włączone_innych_ lub _grupy_ elementów członkowskich. **Poprawka wspólnej**: Uruchom `chmod og-w "$HOME"` w terminalu wiersza polecenia na komputerach Mac. Szczegółowe informacje o którym określonego pliku lub katalogu powoduje problem, uruchom `grep sshd /var/log/system.log > "$HOME/Desktop/sshd.log"` w terminalu, a następnie otwórz **sshd.log** plik z pulpitu, a następnie wyszukaj "Uwierzytelnianie zostało odrzucone: zły własność lub tryby".

### <a name="trying-to-connect-never-completes"></a>"Próby nawiązania połączenia..." nigdy nie wykonuje

- **Błąd [#52264](https://bugzilla.xamarin.com/show_bug.cgi?id=52264)**  — ten problem może wystąpić na program Xamarin 4.1 w przypadku **powłoka logowania** w **zaawansowane opcje** menu kontekstowe dla użytkowników komputerów Mac w  **Preferencje systemu &gt; użytkowników &amp; grup** ma ustawioną wartość innych niż **/bin/bash**. (Począwszy od platformy Xamarin 4.2, w tym scenariuszu zamiast prowadzi do komunikat o błędzie "Nie może nawiązać połączenia".) **Obejście**: zmiana **powłoka logowania** oryginalnego domyślną z **/bin/bash**.

<a name="tryagain" />

### <a name="couldnt-connect-to-macbuildhostlocal-please-try-again"></a>"Nie może połączyć się z MacBuildHost.local. Spróbuj ponownie."

Zgłoszony przyczyny:

- **Błąd** — w przypadku kilku użytkowników miały ten komunikat o błędzie, wraz z bardziej szczegółowe informacje o błędzie w plikach dziennika "Wystąpił nieoczekiwany błąd podczas konfigurowania SSH dla użytkownika... Upłynął limit czasu operacji sesji"podczas próby logowania do hosta kompilacji przy użyciu usługi Active Directory lub inne konto użytkownika domeny usługi katalogu. **Obejście problemu:** zalogować się do hosta kompilacji zamiast konta użytkownika lokalnego.

- **Błąd** — w przypadku niektórych użytkowników umieścić ten błąd podczas próby nawiązania połączenia z hostem kompilacji przez dwukrotne kliknięcie nazwy komputerów Mac w oknie dialogowym połączenia. **Możliwym obejściem**: [ręcznie dodać Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md#manual-add) przy użyciu adresu IP.

- **Błąd [#35971](https://bugzilla.xamarin.com/show_bug.cgi?id=35971)**  — w przypadku niektórych użytkowników zostało uruchomione przez ten błąd, korzystając z połączenia sieci bezprzewodowej między hostem kompilacji Mac i systemu Windows. **Możliwym obejściem**: Przenieś oba komputery do połączeń sieci przewodowej.

- **Błąd [#36642](https://bugzilla.xamarin.com/show_bug.cgi?id=36642)**  — na platformie Xamarin 4.0, zostanie wyświetlony ten komunikat w każdej chwili **$ głównej/.bashrc** plik na Mac zawiera błąd. (Począwszy od program Xamarin 4.1 błędów w **.bashrc** pliku nie będzie miało wpływ na proces łączenia.) **Obejście**: Przenieś **.bashrc** plików do lokalizacji kopii zapasowej (lub usuń go, jeśli wiadomo, że nie będzie potrzebny).

- **Błąd [#52264](https://bugzilla.xamarin.com/show_bug.cgi?id=52264)**  — ten błąd może wystąpić, jeśli **powłoka logowania** w **zaawansowane opcje** menu kontekstowe dla użytkowników komputerów Mac w **systemu Preferencje > Użytkownicy i grupy** ma ustawioną wartość innych niż **/bin/bash**. **Obejście**: zmiana **powłoka logowania** oryginalnego domyślną z **/bin/bash**.

- **Ograniczenie** — ten błąd może wystąpić, jeśli host kompilacji Mac jest podłączony do routera, który nie ma dostępu do Internetu (lub Mac jest przy użyciu serwera DNS, który upłynął limit czasu po otrzymaniu monitu, dla wyszukiwania wstecznego DNS komputer z systemem Windows). Visual Studio będzie zająć około 30 sekund można pobrać odcisku palca SSH i ostatecznie mogły nawiązać połączenia.

    **Możliwym obejściem**: Dodaj "UseDNS nie" do **sshd\_config** pliku. Pamiętaj uzyskać informacje dotyczące tego ustawienia SSH, przed zmianą. Zobacz na przykład [unix.stackexchange.com/questions/56941/what-is-the-point-of-sshd-usedns-option](http://unix.stackexchange.com/questions/56941/what-is-the-point-of-sshd-usedns-option).

    W poniższych krokach opisano jeden ze sposobów Zmień ustawienie. Musisz zalogować się na konto administratora dla komputerów Mac do wykonania procedury.

    1. Potwierdź lokalizację **sshd\_config** pliku uruchamiając `ls /etc/ssh/sshd_config` i `ls /etc/sshd_config` Terminal wiersza polecenia. Wszystkie pozostałe kroki, należy użyć lokalizacji, która jest _nie_ zwracać "Brak pliku lub katalogu".

        [![](troubleshooting-images/troubleshooting-image18.png "Uruchomiony "/etc/ssh/sshd_config ls" i "/etc/sshd_config ls" w terminalu")](troubleshooting-images/troubleshooting-image18.png#lightbox)

    3. Uruchom `cp /etc/ssh/sshd_config "$HOME/Desktop/"` w terminalu, aby skopiować plik na pulpicie.

    4. Otwórz plik z pulpitu w edytorze tekstów. Na przykład można uruchomić `open -a TextEdit "$HOME/Desktop/sshd_config"` w terminalu.

    5. Dodaj następujący wiersz w dolnej części pliku:

            UseDNS no

    6. Usuń wszystkie wiersze, które powiedzieć `UseDNS yes` się upewnić, że nowe ustawienie zostanie uwzględnione.

    7. Zapisz plik.

    8. Uruchom `sudo cp "$HOME/Desktop/sshd_config" /etc/ssh/sshd_config` w terminalu, aby skopiować edytowanego pliku w miejscu. Jeśli zostanie wyświetlony monit, wprowadź hasło.

    9. Wyłączenie i ponowne włączenie **logowania zdalnego** w obszarze **preferencjach systemowych &gt; udostępniania &gt; logowania zdalnego** ponownie serwer SSH.

<a name="clearing" />

### <a name="clearing-the-broker-idb-build-and-designer-agents-on-the-mac"></a>Wyczyszczenie brokera, IDB, kompilacji i projektanta agentów dla komputerów Mac

Jeśli pliki dziennika wskazuje na problem podczas "Instalowanie", "Przekazywanie" lub "Początkowy" kroki dla każdego z agentów Mac, możesz spróbować usunąć **XMA** folder pamięci podręcznej, aby wymusić Visual Studio, aby ponownie przekazać je.

1. Uruchom następujące polecenie w terminalu dla komputerów Mac:

        open "$HOME/Library/Caches/Xamarin"

2. Kliknij Control **XMA** i wybierz polecenie **Przenieś do Kosza**:

    [![](troubleshooting-images/troubleshooting-image8.png "Przenieś XMA folder Kosza")](troubleshooting-images/troubleshooting-image8.png#lightbox)

3. Brak pamięci podręcznej w systemie Windows oraz które mogą pomóc wyczyścić. Otwórz wiersz polecenia jako Administrator w systemie Windows:

        del %localappdata%\Temp\Xamarin\XMA

## <a name="warning-messages"></a>Komunikaty ostrzegawcze

W tej sekcji omówiono kilka wiadomości, które mogą być wyświetlane w oknie danych wyjściowych i dzienniki, które zwykle można zignorować.

### <a name="there-is-a-mismatch-between-the-installed-xamarinios--and-the-local-xamarinios"></a>"Wystąpiła niezgodność między... zainstalowanych Xamarin.iOS i lokalnej platformy Xamarin.iOS"

Tak długo, jak została potwierdzona, zarówno Mac i systemu Windows zostały zaktualizowane do tej samej kanałów dystrybucji Xamarin, to ostrzeżenie jest do pominięcia.

### <a name="failed-to-execute-ls-usrbinmono-exitstatus1"></a>"Nie można wykonać"/usr/bin/mono ls": ExitStatus = 1"

To jest komunikat do pominięcia pod warunkiem, systemem Mac OS X 10.11 (El Capitan) lub nowszej. Ten komunikat nie jest problemu, OS X 10.11, ponieważ sprawdza również Xamarin **/usr/local/bin/mono**, jest poprawna oczekiwana lokalizacja dla `mono` na OS X 10.11.

### <a name="bonjour-service-macbuildhost-did-not-respond-with-its-ip-address"></a>"Usługa Bonjour 'MacBuildHost' nie odpowiedział swój adres IP."

Ten komunikat jest do pominięcia, chyba że można zauważyć, że okno dialogowe połączenia nie wyświetla adres IP hosta kompilacji Mac. Jeśli adres IP _jest_ Brak, w tym oknie dialogowym, nadal możesz [ręcznie dodać Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md#manual-add).

### <a name="invalid-user-a-from-101895-and-inputuserauthrequest-invalid-user-a-preauth"></a>"Nieprawidłowy użytkownik z 10.1.8.95" i "wejściowych\_userauth\_żądania: Nieprawidłowy użytkownik autoryzacji [wstępnej]"

Można zauważyć to wiadomości w **sshd.log**. Komunikaty te są częścią procesu normalne połączenia. Ich występowania, ponieważ nazwa użytkownika używa Xamarin **a** tymczasowo podczas pobierania _odcisk palca SSH_.

## <a name="output-window-and-log-files"></a>Okno danych wyjściowych i pliki dziennika

Jeśli program Visual Studio trafienia wystąpił błąd podczas nawiązywania połączenia z hostem kompilacji, są 2 lokalizacje, aby sprawdzić, aby przejrzeć dodatkowe komunikaty: w oknie danych wyjściowych i pliki dziennika.

### <a name="output-window"></a>Okno wyniku

W oknie danych wyjściowych jest najlepszym miejscem, aby rozpocząć. Wyświetla komunikaty dotyczące połączenie główne kroki i błędów. Aby wyświetlać komunikaty Xamarin w oknie danych wyjściowych:

1. Wybierz **Widok > dane wyjściowe** z menu lub kliknij przycisk **dane wyjściowe** kartę.
2. Kliknij przycisk **Pokaż dane wyjściowe z** menu rozwijanego.
3. Wybierz **Xamarin**.

[![](troubleshooting-images/troubleshooting-image11.png "Wybierz Xamarin na karcie dane wyjściowe")](troubleshooting-images/troubleshooting-image11.png#lightbox)

### <a name="log-files"></a>Pliki dziennika

Jeśli w oknie danych wyjściowych nie zawiera wystarczających informacji do zdiagnozowania problemu, pliki dziennika są miejsca dalej do wyszukiwania. Pliki dziennika zawierają dodatkowych diagnostyczne komunikatów, które nie są wyświetlane w oknie danych wyjściowych. Aby wyświetlić pliki dziennika:

1. Uruchom program Visual Studio.

    > [!IMPORTANT]
>  Należy pamiętać, że **.svclogs** nie są domyślnie włączone. Dostęp do nich należy do uruchamiania programu Visual Studio z dziennikami pełne, zgodnie z objaśnieniem w [dzienniki wersji](~/cross-platform/troubleshooting/questions/version-logs.md#visual-studio-startup-verbose-logs) przewodnik. Aby uzyskać więcej informacji, zapoznaj się [rozszerzenia Rozwiązywanie problemów z dziennika aktywności](https://blogs.msdn.microsoft.com/visualstudio/2010/02/24/troubleshooting-extensions-with-the-activity-log/) blogu.

2. Spróbuj połączyć się z hostem kompilacji.

3. Po Visual Studio trafienia błąd połączenia, zbierz dzienniki z **Pomoc > Xamarin > Zip dzienniki**:

    [![](troubleshooting-images/troubleshooting-image12.png "Zbieranie dzienników z pomocy > Xamarin > Zip dzienników")](troubleshooting-images/troubleshooting-image12.png#lightbox)

4. Po otwarciu pliku zip, zostanie wyświetlona lista plików, podobnie jak w poniższym przykładzie. Błędy połączenia są najważniejsze pliki  **\*Ide.log** i  **\*Ide.svclog** plików. Te pliki zawierają tej wiadomości w dwóch nieco różne formaty. **.Svclog** XML i jest przydatne, jeśli chcesz przeglądać wiadomości. **Log** jest zwykły tekst i jest przydatne, jeśli chcesz przefiltrować komunikaty przy użyciu narzędzia wiersza polecenia.


    Aby przejrzeć wszystkie komunikaty, wybierz i Otwórz **.svclog** pliku:

    [![](troubleshooting-images/troubleshooting-image13.png "Wybierz plik svclog")](troubleshooting-images/troubleshooting-image13.png#lightbox)

5. **.Svclog** plik zostanie otwarty w **przeglądarki danych śledzenia usługi Microsoft**. Możesz przeglądać wiadomości przez wątek, aby zobaczyć powiązane grupy wiadomości. Aby przejść przez wątek, najpierw wybierz **wykres** , a następnie kliknij **tryb układu** menu rozwijane i wybierz **wątku**:

    [![](troubleshooting-images/troubleshooting-image14.png "Kliknij menu rozwijane w trybie układu i wybierz wątku")](troubleshooting-images/troubleshooting-image14.png#lightbox)

<a name="verboselogs" />

### <a name="verbose-log-files"></a>Plików pełnego dziennika

Jeśli pliki dziennika normalne nie zapewniają wystarczających informacji do zdiagnozowania problemu, co technika ostatniej próby jest Włącz pełne rejestrowanie. Pełne dzienniki również są preferowane w raportach usterek.

1. Zamknij program Visual Studio.

2. Uruchom [ **wiersza polecenia dewelopera**](https://msdn.microsoft.com/en-us/library/ms229859(v=vs.110).aspx).

3. Uruchom następujące polecenie w wierszu polecenia, aby uruchomić program Visual Studio z pełne rejestrowanie:

    ```bash
    devenv /log
    ```

4. Próba połączenia z hostem kompilacji w programie Visual Studio.

5. Po Visual Studio trafienia błąd połączenia, zbierz dzienniki z **Pomoc > Xamarin > Zip dzienniki**.

6. Uruchom następujące polecenie w terminalu dla komputerów Mac, aby skopiować wszystkie ostatnie komunikaty dziennika z serwera SSH do pliku na pulpicie:

    ```bash
    grep sshd /var/log/system.log > "$HOME/Desktop/sshd.log"
    ```

Jeśli te pliki dziennika pełne nie udostępniają wystarczającej liczby wskazówek, aby rozwiązać ten problem, bezpośrednio, skontaktuj się z [nowy raport o usterkach](https://bugzilla.xamarin.com/newbug) i Dołącz plik zip z kroku 5 i pliku log w kroku 6.

## <a name="troubleshooting-build-and-deployment-errors"></a>Rozwiązywanie problemów z kompilacji i błędy wdrożenia

W tej sekcji opisano kilka problemów, które mogą wystąpić po Visual Studio pomyślnie łączy się z hosta kompilacji.

### <a name="unable-to-connect-to-address1921681222-with-usermacuser"></a>"Nie można nawiązać połączenia z adresem ='192.168.1.2:22 ' z użytkownikiem ="macuser""

Znane przyczyny:

- **Funkcja zabezpieczeń program Xamarin 4.1** — ten błąd _będzie_ się zdarzyć, jeśli starszą wersję Xamarin 4.0 po użyciu Xamarin 4.1 lub nowszej. W takim przypadku błąd zostaną dołączone dodatkowe ostrzeżenie "klucz prywatny jest zaszyfrowany, ale hasło jest puste". Jest to _zamierzone_ zmienić ze względu na nową funkcją w program Xamarin 4.1. **Zalecane poprawki**: Usuń **identyfikator\_rsa** i **identyfikator\_rsa.pub** z **%LOCALAPPDATA%\Xamarin\MonoTouch**, a następnie ponownie nawiąż połączenie Mac hosta kompilacji.

- **Ograniczenia zabezpieczeń SSH** — Jeżeli ten komunikat towarzyszy dodatkowy ostrzeżenie "nie może uwierzytelnić użytkownika przy użyciu istniejącego ssh kluczy", często oznacza to, jednego z plików lub katalogów w pełni kwalifikowana ścieżka **$ HOME/.ssh/authorized\_klucze** na Mac ma uprawnienia do zapisu włączone dla _innych_ lub _grupy_ elementów członkowskich. **Poprawka wspólnej**: Uruchom `chmod og-w "$HOME"` w terminalu wiersza polecenia na komputerach Mac. Szczegółowe informacje o którym określonego pliku lub katalogu powoduje problem, uruchom `grep sshd /var/log/system.log > "$HOME/Desktop/sshd.log"` w terminalu, a następnie otwórz **sshd.log** plik z pulpitu, a następnie wyszukaj "Uwierzytelnianie zostało odrzucone: zły własność lub tryby".

### <a name="solutions-cannot-be-loaded-from-a-network-share"></a>Nie można załadować rozwiązania z udziału sieciowego

Rozwiązań zostanie skompilowany, tylko jeśli są na lokalnego systemu plików lub mapowany dysk.

Rozwiązania, które są zapisywane w udziale sieciowym może zgłosić błędy, lub całkowicie odmówić do skompilowania. Wszelkie **.sln** pliki używane w programie Visual Studio ma zostać zapisany w lokalnym systemie plików systemu Windows.

Następujący błąd jest zgłaszany z powodu tego problemu:

```bash
error : Building from a network share path is not supported at the moment. Please map a network drive to '\\SharedSources\HelloWorld\HelloWorld' or copy the source to a local directory.
```

usterki pokrewne: [#36195](https://bugzilla.xamarin.com/show_bug.cgi?id=36195)

### <a name="missing-provisioning-profiles-or-failed-to-create-the-a-fat-library-error"></a>Brak profile inicjowania obsługi lub "nie można utworzyć biblioteki fat" Błąd

Uruchom środowisko Xcode dla komputerów Mac i upewnij się, że komputer firmy Apple konta dewelopera jest zalogowany i jest pobrać profilu programowanie dla systemu iOS:

[![](troubleshooting-images/troubleshooting-image7.png "Pobierany jest zapewnienie, że konto dewelopera Apple jest zalogowany i profilu programowanie dla systemu iOS")](troubleshooting-images/troubleshooting-image7.png#lightbox)

### <a name="a-socket-operation-was-attempted-to-an-unreachable-network"></a>"Operacja gniazda podjęto sieć nieosiągalny"

Zgłoszony przyczyny:

- **Rozszerzenie [#36118](https://bugzilla.xamarin.com/show_bug.cgi?id=36118)**  — ten błąd może uniemożliwić kompilacje pomyślne, korzystając z programu Visual Studio jest adres IPv6 do połączenia z hostem kompilacji. (Połączenie host kompilacji nie obsługuje jeszcze adresów IPv6.)

### <a name="xamarinios-visual-studio-plugin-fails-to-load-after-reinstallation-of-betaalpha-channel"></a>Xamarin.iOS Visual Studio wtyczki nie udało się załadować po ponownej instalacji kanału alfa/beta

Usterka odpowiednich [#40781](https://bugzilla.xamarin.com/show_bug.cgi?id=40781).

Ten problem może się tak zdarzyć, gdy program Visual Studio nie można odświeżyć pamięci podręcznej składników MEF. Jeśli tak jest, mogą pomóc w instalacji tego rozszerzenia programu Visual Studio: [https://visualstudiogallery.msdn.microsoft.com/22b94661-70c7-4a93-9ca3-8b6dd45f47cd](https://visualstudiogallery.msdn.microsoft.com/22b94661-70c7-4a93-9ca3-8b6dd45f47cd)

Spowoduje to wyczyszczenie pamięci podręcznej składników MEF usługi Visual Studio umożliwiają rozwiązywanie problemów z uszkodzenie pamięci podręcznej.

<a name="errors" />

### <a name="errors-due-to-existing-build-host-processes-on-the-mac"></a>Błędy z powodu istniejące procesy hosta kompilacji dla komputerów Mac

Procesy z poprzedniej kompilacji obsługi połączeń czasami może zakłócać działanie bieżącego aktywnego połączenia. Aby sprawdzić wszelkie istniejące procesy, należy zamknąć program Visual Studio, a następnie uruchom następujące polecenia w terminalu w Mac:

```bash
ps -A | grep mono
```

[![](troubleshooting-images/troubleshooting-image10.png "Uruchamianie poleceń w terminalu dla komputerów Mac")](troubleshooting-images/troubleshooting-image10.png#lightbox)

Aby skasować istniejące procesy, użyj następującego polecenia:

```bash
killall mono
```

### <a name="clearing-the-mac-build-cache"></a>Wyczyszczenie pamięci podręcznej kompilacji Mac

Jeśli rozwiązać problem kompilacji i chce mieć pewność, że zachowanie nie jest powiązany z żadnym z kompilacji tymczasowych plików przechowywanych na Mac, można usunąć folderu pamięci podręcznej kompilacji.

1. Uruchom następujące polecenie w terminalu dla komputerów Mac:

    ```bash
    open "$HOME/Library/Caches/Xamarin"
    ```

2. Kliknij Control **MTB** i wybierz polecenie **Przenieś do Kosza**:

    [![](troubleshooting-images/troubleshooting-image9.png "Przenieś folder MTB Kosza")](troubleshooting-images/troubleshooting-image9.png#lightbox)


## <a name="related-links"></a>Linki pokrewne

- [Nawiązywanie połączenia z komputerem Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md)
- [Łączenie Mac do środowiska Visual Studio z XMA (klip wideo)](https://university.xamarin.com/lightninglectures/xamarin-mac-agent)
