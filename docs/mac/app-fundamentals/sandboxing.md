---
title: Tryb piaskownicy aplikacji rozszerzenia Xamarin.Mac
description: W tym artykule opisano tryb piaskownicy aplikacji Xamarin.Mac do wydania w Store aplikacji. Poruszono w nim wszystkie elementy, które bardziej szczegółowo piaskownicy, takich jak kontener katalogów, uprawnień, uprawnienia określone przez użytkownika, separacji uprawnień i wymuszania jądra.
ms.prod: xamarin
ms.assetid: 06A2CA8D-1E46-410F-8C31-00EA36F0735D
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 9e6cfd39260b76c5097d8faa24f406f45681ac8d
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/13/2018
ms.locfileid: "43780582"
---
# <a name="sandboxing-a-xamarinmac-app"></a>Tryb piaskownicy aplikacji rozszerzenia Xamarin.Mac

_W tym artykule opisano tryb piaskownicy aplikacji Xamarin.Mac do wydania w Store aplikacji. Poruszono w nim wszystkie elementy, które bardziej szczegółowo piaskownicy, takich jak kontener katalogów, uprawnień, uprawnienia określone przez użytkownika, separacji uprawnień i wymuszania jądra._

## <a name="overview"></a>Omówienie

Podczas pracy z C# i .NET w aplikacji platformy Xamarin.Mac, masz tych samych możliwości piaskownicy aplikacji, tak jak podczas pracy z języka Objective-C lub Swift.

[![Przykładem uruchomionej aplikacji](sandboxing-images/intro01.png "przykładem uruchomionej aplikacji")](sandboxing-images/intro01-large.png#lightbox)

W tym artykule omówiono podstawowe informacje dotyczące pracy z piaskownicy w aplikacji platformy Xamarin.Mac oraz wszystkie elementy, które bardziej szczegółowo piaskownicy: katalogów kontenera, uprawnień, uprawnienia określone przez użytkownika, separacji uprawnień i wymuszania jądra. Zdecydowanie zalecane jest, pracy za pośrednictwem [Witaj, Mac](~/mac/get-started/hello-mac.md) artykuł najpierw, w szczególności [wprowadzenie do programu Xcode i programu Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) i [gniazd i akcje](~/mac/get-started/hello-mac.md#outlets-and-actions) sekcje w postaci, w jakiej omawia kluczowe założenia i technik, które będzie używany w tym artykule.

Możesz chcieć spojrzeć na [udostępnianie klasy języka C# / metod języka Objective-C](~/mac/internals/how-it-works.md) części [elementach wewnętrznych rozszerzenia Xamarin.Mac](~/mac/internals/how-it-works.md) dokumentów, jak również wyjaśniono `Register` i `Export` atrybutów Umożliwia podłączanie klas języka C# do języka Objective-C obiektów i interfejsu użytkownika elementów.

## <a name="about-the-app-sandbox"></a>Piaskownica aplikacji — informacje

Piaskownica aplikacji zapewnia silne obrony przed uszkodzeniem, który może być spowodowany przez złośliwych aplikacji, są uruchamiane na komputerze Mac, ograniczając dostępu aplikacji do zasobów systemowych.

Piaskownicy aplikacji ma pełne prawa użytkownika, w którym działa aplikacja i można uzyskać dostęp lub podejmować żadnych działań, które użytkownik może. Jeśli aplikacja zawiera luki zabezpieczeń (lub dowolnej platformy, który jest używany), haker może potencjalnie te luki i przejęcie kontroli nad uruchomionego na komputerze Mac przy użyciu aplikacji.

Ograniczając dostęp do zasobów na podstawie poszczególnych aplikacji, piaskownicy aplikacji zawiera linię obrony przed kradzieżą, uszkodzenia lub złośliwego działania ze strony aplikacji uruchomionej na komputerze użytkownika.

Piaskownica aplikacji jest technologia kontroli dostępu wbudowanych systemu macOS (wymuszane na poziomie jądra), który zapewnia strategii dwa cele:

1. Piaskownica aplikacji umożliwia deweloperowi opisują _jak_ aplikacji będą wchodzić w interakcje z systemem operacyjnym i w ten sposób, że udzielono mu prawa dostępu, które są wymagane do swoją pracę i nie więcej.
2. Piaskownica aplikacji umożliwia użytkownikowi bezproblemowo udzielić dalszego dostępu do systemu za pośrednictwem Otwórz i zapisać w oknach dialogowych, przeciągnij i upuść operacji oraz interakcji użytkowników z innych, typowych.

### <a name="preparing-to-implement-the-app-sandbox"></a>Trwa przygotowywanie do wdrożenia piaskownica aplikacji

Dostępne są następujące elementy piaskownica aplikacji, która zostanie omówiona szczegółowo w artykule:

- Katalogi kontenera
- Uprawnienia
- Uprawnienia określone przez użytkownika
- Rozdzielenie uprawnień
- Wymuszanie jądra

Po zrozumieniu tymi szczegółowymi informacjami, będzie można utworzyć plan przyjmowania piaskownica aplikacji w aplikacji platformy Xamarin.Mac.

Po pierwsze należy ustalić, czy aplikacja jest dobrym kandydatem do piaskownicy (większość aplikacji są). Następnie należy rozwiązać wszelkie niezgodności interfejsu API i określić, które elementy piaskownica aplikacji będzie wymagać. Na koniec sprawdź do przy użyciu separacji uprawnień, aby zapewnić maksymalną poziom ochrony aplikacji.

Przyjmując piaskownica aplikacji, może się różnić niektórych lokalizacji systemu plików, używanych przez aplikację. Szczególnie aplikacja będzie mieć katalogu kontenera, który będzie używany dla plików pomocy technicznej dla aplikacji, baz danych, pamięci podręcznych i innych plików, które nie są dokumenty użytkowników. Zarówno w systemach macOS, jak i w środowisku Xcode obsługi migracji tych typów plików z ich starszych lokalizacji do kontenera.

## <a name="sandboxing-quick-start"></a>Przewodnik Szybki start piaskownicy

W tej sekcji utworzymy prostą aplikację platformy Xamarin.Mac, która używa widoku sieci Web (co wymaga połączenia sieciowego, która jest ograniczona w piaskownicy, chyba że wyraźnie wymagane) na przykład wprowadzenie piaskownica aplikacji.

Firma Microsoft będzie zweryfikować, że aplikacja jest w rzeczywistości w trybie piaskownicy i Dowiedz się, jak rozwiązać typowe błędy piaskownica aplikacji.

### <a name="creating-the-xamarinmac-project"></a>Tworzenie projektu platformy Xamarin.Mac

Wykonamy teraz zadania następujące polecenie, aby utworzyć naszego przykładowego projektu:

1. Uruchom program Visual Studio dla komputerów Mac i kliknij przycisk **nowe rozwiązanie...** link.
2. Z **nowy projekt** okno dialogowe, wybierz opcję **Mac** > **aplikacji** > **aplikacja cocoa dla**: 

    [![Tworzenie nowej aplikacji Cocoa](sandboxing-images/sample01.png "utworzenie nowej aplikacji Cocoa")](sandboxing-images/sample01-large.png#lightbox)
3. Kliknij przycisk **dalej** przycisk, wprowadź `MacSandbox` nazwę projektu i kliknij przycisk **Utwórz** przycisku: 

    [![Wprowadź nazwę aplikacji](sandboxing-images/sample02.png "podawania nazwy aplikacji")](sandboxing-images/sample02-large.png#lightbox)
4. W **konsoli rozwiązania**, kliknij dwukrotnie **Main.storyboard** plik, aby otworzyć do edycji w środowisku Xcode: 

    [![Edytowanie głównego storyboard](sandboxing-images/sample03.png "edytowania głównego storyboard")](sandboxing-images/sample03-large.png#lightbox)
5. Przeciągnij **widoku strony sieci Web** na oknie, rozmiar, aby wypełnił obszar zawartości i ustaw ją na powiększanie i zmniejszanie w oknie: 

    [![Dodawanie widoku sieci web](sandboxing-images/sample04.png "Dodawanie widoku sieci web")](sandboxing-images/sample04-large.png#lightbox)
6. Tworzenie ujścia w widoku sieci web o nazwie `webView`: 

    [![Tworzenie nowego punktu sprzedaży](sandboxing-images/sample05.png "tworzenia nowego punktu sprzedaży")](sandboxing-images/sample05-large.png#lightbox)
7. Wróć do programu Visual Studio dla komputerów Mac i kliknij dwukrotnie plik **ViewController.cs** w pliku **konsoli rozwiązania** aby go otworzyć do edycji.
8. Dodaj następującą instrukcję using: `using WebKit;`
9. Wprowadź `ViewDidLoad` wygląd metoda podobne do następującego: 

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();
    webView.MainFrame.LoadRequest(new NSUrlRequest(new NSUrl("http://www.apple.com")));
}
```

10. Zapisz zmiany.

Uruchomiony, z którym aplikacji i upewnij się, czy witryny sieci Web firmy Apple są wyświetlane w oknie w następujący sposób:

[![Wyświetlanie uruchamiania przykładowej aplikacji](sandboxing-images/sample06.png "przedstawiający uruchamiania przykładowej aplikacji")](sandboxing-images/sample06-large.png#lightbox)

<a name="Signing_and_Provisioning_the_App" />

### <a name="signing-and-provisioning-the-app"></a>Podpisywania i inicjowania obsługi administracyjnej aplikacji

Zanim firma Microsoft może umożliwić piaskownica aplikacji, najpierw należy aprowizować i zaloguj się nasza aplikacja platformy Xamarin.Mac.

Pozwól, wykonaj następujące czynności:

1. Zaloguj się do portalu dla deweloperów firmy Apple: 

    [![Logując się do portalu Apple Developer Portal](sandboxing-images/sign01.png "logując się do portalu dla deweloperów firmy Apple")](sandboxing-images/sign01-large.png#lightbox)
2. Wybierz **certyfikaty, identyfikatory i profile**: 

    [![Wybieranie certyfikaty, identyfikatory i profile](sandboxing-images/sign02.png "wybierając certyfikaty, identyfikatory i profile")](sandboxing-images/sign02-large.png#lightbox)
3. W obszarze **aplikacji dla komputerów Mac**, wybierz opcję **identyfikatory**: 

    [![Wybieranie identyfikatory](sandboxing-images/sign03.png "wybierając identyfikatory")](sandboxing-images/sign03-large.png#lightbox)
4. Utwórz nowy identyfikator aplikacji: 

    [![Tworzenie nowego Identyfikatora aplikacji](sandboxing-images/sign04.png "tworzenia nowego Identyfikatora aplikacji")](sandboxing-images/sign04-large.png#lightbox)
5. W obszarze **profilów aprowizacji**, wybierz opcję **rozwoju**: 

    [![Wybieranie rozwoju](sandboxing-images/sign05.png "wybierając rozwoju")](sandboxing-images/sign05-large.png#lightbox)
6. Utwórz nowy profil, a następnie wybierz pozycję **programowania aplikacji dla komputerów Mac.**: 

    [![Tworzenie nowego profilu](sandboxing-images/sign06.png "tworzenia nowego profilu")](sandboxing-images/sign06-large.png#lightbox)
7. Wybierz identyfikator aplikacji utworzone powyżej: 

    [![Wybieranie Identyfikatora aplikacji](sandboxing-images/sign07.png "wybranie Identyfikatora aplikacji")](sandboxing-images/sign07-large.png#lightbox)
8. Wybierz deweloperów dla tego profilu: 

    [![Dodawanie deweloperów](sandboxing-images/sign08.png "deweloperom Dodawanie")](sandboxing-images/sign08-large.png#lightbox)
9. Wybierz komputery, dla tego profilu: 

    [![Wybieranie komputerów dozwolonych](sandboxing-images/sign09.png "wybierając dozwolone komputery")](sandboxing-images/sign09-large.png#lightbox)
10. Nazwij profilu: 

    [![Nadając nazwę profilu](sandboxing-images/sign10.png "nadając nazwę profilu")](sandboxing-images/sign10-large.png#lightbox)
11. Kliknij przycisk **gotowe** przycisku.

> [!IMPORTANT]
> W niektórych przypadkach może być konieczne bezpośrednio pobrać nowy profil aprowizacji z portalu dla deweloperów firmy Apple i go dwukrotnie, aby zainstalować. Może być również konieczne zatrzymać i ponownie uruchomić program Visual Studio dla komputerów Mac, zanim będą mogli korzystać z nowego profilu.

Następnie należy załadować nowego Identyfikatora aplikacji i profilu na komputerze deweloperskim. Teraz należy wykonać następujące czynności:

1. Uruchom program Xcode i wybierz **preferencje** z **Xcode** menu: 

    ![Edytowanie kont w środowisku Xcode](sandboxing-images/sign11.png "edytowanie kont w środowisku Xcode")
2. Kliknij pozycję **Wyświetl szczegóły...**  przycisku: 

    ![Klikając przycisk Wyświetl szczegóły](sandboxing-images/sign12.png "kliknięcie przycisku Wyświetl szczegóły")
3. Kliknij pozycję **Odśwież** przycisk (w dolnym lewym dolnym rogu).
4. Kliknij pozycję **gotowe** przycisku.

Następnie należy wybrać nowego Identyfikatora aplikacji i profilu inicjowania obsługi administracyjnej w projekcie platformy Xamarin.Mac. Teraz należy wykonać następujące czynności:

1. W **konsoli rozwiązania**, kliknij dwukrotnie **Info.plist** plik, aby otworzyć do edycji.
2. Upewnij się, że **identyfikatora pakietu** pasuje do naszego Identyfikatora aplikacji utworzone powyżej (przykład: `com.appracatappra.MacSandbox`): 

    [![Edytowanie identyfikatora pakietu](sandboxing-images/sign13.png "edycji identyfikator pakietu")](sandboxing-images/sign13-large.png#lightbox)
3. Następnie kliknij dwukrotnie **plik Entitlements.plist** pliku i upewnij się, nasze **Store pary klucz-wartość w usłudze iCloud** i **kontenery iCloud** dopasowania wszystkich naszych Identyfikatora aplikacji utworzone powyżej (przykład: `com.appracatappra.MacSandbox`): 

    [![Edytowanie pliku plik Entitlements.plist](sandboxing-images/sign17.png "edycji pliku do pliku Entitlements.plist")](sandboxing-images/sign17-large.png#lightbox)
3. Zapisz zmiany.
4. W **konsoli rozwiązania**, kliknij dwukrotnie plik projektu, aby otworzyć Opcje edycji:  

    ![Editign opcje rozwiązania](sandboxing-images/sign14.png "Editign opcje rozwiązania")
5. Wybierz **podpisywanie dla komputerów Mac**, następnie sprawdź **Podpisz zbiór aplikacji** i **Podpisz pakiet Instalatora**. W obszarze **profil inicjowania obsługi administracyjnej**, wybierz jedną, utworzone powyżej: 

    ![Ustawienie profilu inicjowania obsługi administracyjnej](sandboxing-images/sign15.png "ustawienie profilu inicjowania obsługi administracyjnej")
6. Kliknij przycisk **gotowe** przycisku.

> [!IMPORTANT]
> Może być konieczne zamknięcie i ponowne uruchomienie programu Visual Studio dla komputerów Mac, można go pobrać nowego Identyfikatora aplikacji i profilu aprowizacji, który został zainstalowany przez program Xcode.

#### <a name="troubleshooting-provisioning-issues"></a>Rozwiązywanie problemów inicjowania obsługi administracyjnej

W tym momencie należy próbować uruchomić aplikację i upewnij się, że wszystko podpisane i poprawnie aprowizowany. Jeśli aplikacja nadal działa, tak jak poprzednio, wszystko działa. W przypadku awarii możesz otrzymać okno dialogowe podobny do następującego:

[![Przykład problem w oknie dialogowym aprowizacji](sandboxing-images/sign16.png "przykładem problem okno dialogowe zastrzegania")](sandboxing-images/sign16-large.png#lightbox)

Poniżej przedstawiono najbardziej typowe przyczyny aprowizacji i podpisywania problemy:

- Identyfikator pakietu aplikacji nie pasuje do Identyfikatora aplikacji, z wybranym profilem.
- Identyfikator dewelopera ID jest niezgodny identyfikator dewelopera ID wybranego profilu.
- Identyfikator UUID testowane na komputerze Mac nie jest zarejestrowana w ramach wybranego profilu.

W przypadku problemu rozwiązać ten problem, w portalu dla deweloperów firmy Apple, Odśwież profile w środowisku Xcode i wykonać czystą kompilację w programie Visual Studio dla komputerów Mac.

### <a name="enable-the-app-sandbox"></a>Włącz piaskownica aplikacji

Piaskownica aplikacji możesz włączyć, wybierając pole wyboru w opcjach projektów. Wykonaj następujące czynności:

1. W **konsoli rozwiązania**, kliknij dwukrotnie **plik Entitlements.plist** plik, aby otworzyć do edycji.
2. Sprawdź zarówno **włączenie uprawnień** i **włączyć tryb piaskownicy aplikacji**: 

    [![Uprawnienia do edycji i włączanie piaskownicy](sandboxing-images/sign17.png "edytowania uprawnień i włączanie piaskownicy")](sandboxing-images/sign17-large.png#lightbox)
3. Zapisz zmiany.

W tym momencie możliwe jest piaskownica aplikacji, ale nie podano dostępu do sieci wymagane dla widoku internetowego. Jeśli uruchomisz aplikację teraz, należy uzyskać puste okno:

[![Zablokowany dostęp w sieci web przedstawiająca](sandboxing-images/sample08.png "przedstawiający zablokowany dostęp w sieci web")](sandboxing-images/sample08-large.png#lightbox)

### <a name="verifying-that-the-app-is-sandboxed"></a>Weryfikowanie, czy aplikacja jest w trybie piaskownicy

Oprócz zasobów blokuje zachowanie istnieją trzy podstawowe sposoby stwierdzić, czy został pomyślnie w trybie piaskownicy aplikacji rozszerzenia Xamarin.Mac:

1. W programie Finder, sprawdź zawartość `~/Library/Containers/` folder — Jeśli aplikacja jest w trybie piaskownicy, będzie istnieć folder o nazwie, takich jak identyfikator pakietu aplikacji (przykład: `com.appracatappra.MacSandbox`): 

    [![Otwieranie pakietu aplikacji](sandboxing-images/sample09.png "otwierania pakietu aplikacji")](sandboxing-images/sample09-large.png#lightbox)
2. System uznaje aplikacji piaskownicy w Monitora aktywności:
    - Uruchamianie Monitora aktywności (w obszarze `/Applications/Utilities`). 
    - Wybierz **widoku** > **kolumn** i upewnij się, że **piaskownicy** zaznaczono element menu.
    - Upewnij się, że kolumna piaskownicy odczytuje `Yes` aplikacji: 

    [![Sprawdzanie aplikacji w monitorze działania](sandboxing-images/sample10.png "kontroli aplikacji w monitorze działania")](sandboxing-images/sample10-large.png#lightbox)
3. Sprawdź aplikację binarny jest w trybie piaskownicy:
    - Uruchom aplikację Terminal.
    - Przejdź do aplikacji `bin` katalogu.
    - To polecenie: `codesign -dvvv --entitlements :- executable_path` (gdzie `executable_path` jest ścieżka do aplikacji): 

    [![Sprawdzanie aplikacji w wierszu polecenia](sandboxing-images/sample11.png "kontroli aplikacji w wierszu polecenia")](sandboxing-images/sample11-large.png#lightbox)

### <a name="debugging-a-sandboxed-app"></a>Debugowanie w trybie piaskownicy aplikacji

Debuger łączy do aplikacji platformy Xamarin.Mac za pośrednictwem protokołu TCP, co oznacza, że po włączeniu piaskownicy, jest domyślnie nie można nawiązać połączenia z aplikacji, więc Jeśli spróbujesz uruchomić aplikację bez odpowiednich uprawnień, które są włączone, wystąpi błąd *"nie można nawiązać połączenia Debuger"*. 

[![Ustawianie opcji wymagane](sandboxing-images/debug01.png "ustawienie wymagane opcje")](sandboxing-images/debug01-large.png#lightbox)

**Zezwalaj wychodzących połączeń sieciowych (klient)** uprawnienie jest wymagane dla debugera, włączenie tego jednego z nich będzie zezwolenia na debugowanie normalnie. Ponieważ nie można debugować bez niego, Zaktualizowaliśmy `CompileEntitlements` docelowe dla `msbuild` do automatycznego dodawania tego uprawnienia do uprawnień dla każdej aplikacji, która jest w trybie piaskownicy na potrzeby debugowania kompiluje tylko. Kompilacje wydania, należy użyć uprawnienia określone w pliku uprawnień, bez modyfikacji.

### <a name="resolving-an-app-sandbox-violation"></a>Rozpoznawanie naruszenie piaskownica aplikacji

Naruszenie piaskownicy aplikacji występuje, gdy piaskownicy platformy Xamarin.Mac, aplikacja próbowała uzyskać dostęp do zasobu, który ma być jawnie zabronione. Na przykład naszym widoku strony sieci Web nie jest już możliwe do wyświetlania witryny sieci Web firmy Apple.

Najbardziej typowe źródła naruszenia piaskownicy aplikacji występuje, gdy ustawienia uprawnienia określone w programie Visual Studio dla komputerów Mac nie pasuje do wymagań aplikacji. Ponownie z powrotem do naszym przykładzie Brak połączenia sieciowego uprawnień zachowujących widoku sieci Web od pracy.

#### <a name="discovering-app-sandbox-violations"></a>Wykrywanie naruszenia piaskownica aplikacji

Jeśli istnieje podejrzenie naruszenia piaskownicy aplikacji odbywa się w aplikacji platformy Xamarin.Mac, wyjaśniające, jak wykryć problem polega na użyciu **konsoli** aplikacji.

Wykonaj następujące czynności:

1. Kompilowanie aplikacji w danym i uruchom go z programu Visual Studio dla komputerów Mac.
2. Otwórz **konsoli** aplikacji (z `/Applications/Utilties/`).
3. Wybierz **wszystkie komunikaty** na pasku bocznym i wprowadź `sandbox` w wyszukiwaniu: 

    [![Przykładem problem piaskownicy, w konsoli](sandboxing-images/resolve01.png "przykładem problem piaskownicy, w konsoli programu")](sandboxing-images/resolve01-large.png#lightbox)

Dla naszej aplikacji przykładzie powyżej, można wyświetlić blokuje jądra `network-outbound` ruchu z powodu piaskownica aplikacji, ponieważ firma Microsoft nie żądano prawo.

#### <a name="fixing-app-sandbox-violations-with-entitlements"></a>Naprawianie piaskownica aplikacji naruszeń z uprawnieniami

Teraz, gdy Zaobserwowaliśmy jak znajdowanie naruszeń piaskownicy aplikacji, zobaczmy, jak mogą być rozwiązane przez dostosowanie uprawnień do naszej aplikacji.

Wykonaj następujące czynności:

1. W **konsoli rozwiązania**, kliknij dwukrotnie **plik Entitlements.plist** plik, aby otworzyć do edycji.
2. W obszarze **uprawnień** sekcji wyboru **Zezwalaj wychodzących połączeń sieciowych (klient)** pola wyboru: 

    [![Uprawnienia do edycji](sandboxing-images/sign17.png "uprawnienia do edycji")](sandboxing-images/sign17-large.png#lightbox)
3. Zapisz zmiany w aplikacji.

Jeśli firma Microsoft do celów opisanych powyżej dla naszej przykładowej aplikacji, a następnie tworzyć i uruchom go, zawartości sieci web zostaną wyświetlone zgodnie z oczekiwaniami.

## <a name="the-app-sandbox-in-depth"></a>Piaskownica aplikacji szczegółowe

Mechanizmy kontroli dostępu, które są dostarczane przez piaskownica aplikacji są kilka i łatwa do zrozumienia. Jednak sposób, że piaskownica aplikacji zostanie przyjęte przez każdą aplikację jest unikatowy i w zależności od wymagań aplikacji.

Biorąc pod uwagę starań do ochrony aplikacji rozszerzenia Xamarin.Mac z wykorzystywana przez złośliwy kod, muszą istnieć jednej luki w jednej aplikacji (lub jednej z bibliotek i struktur zużywa) na przejęcie kontroli nad interakcji aplikacji z System.

Piaskownica aplikacji zaprojektowano tak, aby zapobiec przejęcia (lub ograniczyć szkody, które mogą spowodować), umożliwiając określenie interakcje zamierzony aplikacji w systemie. System będzie tylko udzielić dostępu do zasobu, który aplikacja wymaga, aby pobrać jego pracy i od niczego więcej.

Podczas projektowania piaskownica aplikacji, projektowania w scenariuszu najgorszego przypadku. Jeśli aplikacja złamane przez złośliwy kod, jest ograniczona do uzyskiwania dostępu do plików i zasobów w piaskownicy aplikacji.

### <a name="entitlements-and-system-resource-access"></a>Uprawnienia i dostęp do zasobów systemowych

Jak widzieliśmy powyżej aplikacji rozszerzenia Xamarin.Mac, która nie jest w trybie piaskownicy otrzymuje pełne prawa oraz dostępu użytkownika, która jest uruchomiona aplikacja. W przypadku naruszenia zabezpieczeń przez złośliwy kod niechronionych aplikacji może pełnić agenta wrogie działania z szerokiego zakresu potencjalnych za szkody.

Włączając piaskownica aplikacji, Usuń wszystkie oprócz minimalny zestaw uprawnień, które można następnie ponownie włączyć dla tylko do potrzeb przy użyciu uprawnień aplikacji platformy Xamarin.Mac. 

Modyfikowanie zasobów piaskownicy aplikacji w Twojej aplikacji, edytując jej **plik Entitlements.plist** plik i sprawdzanie lub wybierając wymagane z pola listy rozwijanej edytory prawa:

[![Uprawnienia do edycji](sandboxing-images/sign17.png "uprawnienia do edycji")](sandboxing-images/sign17-large.png#lightbox)

### <a name="container-directories-and-file-system-access"></a>Katalogi kontenera i dostęp do systemu plików

Gdy aplikacja platformy Xamarin.Mac przyjmuje piaskownica aplikacji, ma dostęp do następujących lokalizacji:

- **Katalog aplikacji kontenera** — przy pierwszym uruchomieniu, system operacyjny tworzy specjalny _katalogu kontenera_ w przypadku, gdy wszystkie jej zasoby go, że tylko może uzyskać dostępu. Aplikacja będzie mieć pełny odczyt/zapis dostępu do tego katalogu.
- **Katalogi kontener grupy aplikacji** -aplikacji można udzielić dostępu do co najmniej jednej _grupy kontenerów_ który jest współużytkowany przez aplikacje w tej samej grupie.
- **Pliki określone przez użytkownika** — aplikacja automatycznie uzyskuje dostęp do plików, które są jawnie otworzył lub przeciągnąć i upuszczone na aplikację przez użytkownika.
- **Powiązane elementy** — przy użyciu odpowiednich uprawnień, aplikacja może uzyskiwać dostęp do pliku o tej samej nazwie, ale inne rozszerzenie. Na przykład dokument, który został zapisany jako `.txt` pliku i `.pdf`.
- **Katalogach tymczasowych, narzędzie wiersza polecenia, katalogów i czytelny na świecie położenia** — Twoja aplikacja ma różne stopnie dostępu do plików w innych lokalizacjach dobrze zdefiniowanych podaną przez system.

#### <a name="the-app-container-directory"></a>Katalog aplikacji kontenera

Katalogu kontenera aplikacji aplikacji dla platformy Xamarin.Mac ma następującą charakterystykę:

- Znajduje się w ukrytym lokalizacji w katalogu głównej użytkownika (zwykle `~Library/Containers`) i jest dostępny za pomocą `NSHomeDirectory` — funkcja (patrz poniżej) w ramach aplikacji. Ponieważ jest on katalog macierzysty, każdy użytkownik otrzyma własnych kontenerów dla aplikacji.
- Aplikacja ma nieograniczony dostęp do odczytu/zapisu katalogu kontenera i wszystkie jego podkatalogi i pliki w nim.
- Większość interfejsów API, ścieżki wyszukiwania w systemie macOS są względne wobec kontenera aplikacji. Na przykład kontener będzie mieć własną **biblioteki** (dostępne za pośrednictwem `NSLibraryDirectory`), **pomocy technicznej dla aplikacji** i **preferencje** podkatalogów.
- macOS ustanawia i wymusza połączenie między usługą i aplikacji, jak i jej kontenera za pomocą podpisywania kodu. Nawet jest inny aplikacja próbuje podszywały się pod aplikacji przy użyciu jego **identyfikatora pakietu**, nie będą mogli korzystać z kontenera ze względu na podpisywanie kodu.
- Kontener nie jest pliki wygenerowane przez użytkowników. Zamiast tego jest dla plików, takich jak bazy danych, pamięci podręcznych lub inne określone typy danych używane przez aplikację.
- Aby uzyskać _shoebox_ typów aplikacji (np. aplikacji Photo firmy Apple), zawartość użytkownika zostaną umieszczone w kontenerze.

> [!IMPORTANT]
> Niestety, platforma Xamarin.Mac nie ma 100% pokrycia API jeszcze (w przeciwieństwie do rozszerzenia Xamarin.iOS), w wyniku `NSHomeDirectory` interfejsu API nie został zmapowany w bieżącej wersji platformy Xamarin.Mac.

Jako rozwiązanie tymczasowe można użyć następującego kodu:

```csharp
[System.Runtime.InteropServices.DllImport("/System/Library/Frameworks/Foundation.framework/Foundation")]
public static extern IntPtr NSHomeDirectory();

public static string ContainerDirectory {
    get {
        return ((NSString)ObjCRuntime.Runtime.GetNSObject(NSHomeDirectory())).ToString ();
        }
}
```

#### <a name="the-app-group-container-directory"></a>Katalogu kontenera grupy aplikacji

Począwszy od Mac z systemem macOS 10.7.5 (i nowsze), aplikacja może użyć `com.apple.security.application-groups` uprawnienie dostępu do udostępnionych kontenera, który jest wspólny dla wszystkich aplikacji w grupie. Zawartość umożliwiający dostęp do Internetu niezwiązanych z użytkownikiem, np. bazy danych lub innych typów plików pomocy technicznej (np. pamięci podręcznych), można użyć tego udostępnionego kontenera.

Kontenery grupy są automatycznie dodawane do każdej aplikacji izolowanego kontenera (jeśli są częścią grupy), są przechowywane w `~/Library/Group Containers/<application-group-id>`. Identyfikator grupy _musi_ zaczynają się od Identyfikatora zespołu rozwoju i okres, na przykład:

```plist
<team-id>.com.company.<group-name>
```

Aby uzyskać więcej informacji, zobacz firmy Apple [dodanie aplikacji do grupy aplikacji](https://developer.apple.com/library/prerelease/mac/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19) w [uprawnienie kluczach](https://developer.apple.com/library/prerelease/mac/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/AboutEntitlements.html#//apple_ref/doc/uid/TP40011195).

#### <a name="powerbox-and-file-system-access-outside-of-the-app-container"></a>Powerbox system dostępu do plików lub poza kontenerem aplikacji

Aplikacja platformy Xamarin.Mac w trybie piaskownicy, można uzyskać dostęp lokalizacje w systemie plików poza jej kontenerem w następujący sposób:

- W określonym kierunku użytkownika (za pośrednictwem Open i zapisać okien dialogowych lub innych metod, takich jak przeciągania i upuszczania).
- Przy użyciu uprawnień dla określonego pliku lokalizacje w systemie (takie jak `/bin` lub `/usr/lib`).
* Podczas lokalizacji systemu plików jest w przypadku niektórych katalogów znajdujących się na świecie do odczytu (na przykład udostępniania).

_Powerbox_ to technologia zabezpieczeń, która wchodzi w interakcję z użytkownikiem, aby rozwinąć prawa dostępu plik piaskownicy aplikacji rozszerzenia Xamarin.Mac systemu macOS. Powerbox ma żadnych interfejsów API, ale w sposób niewidoczny dla użytkownika jest aktywowany, gdy aplikacja wywołuje `NSOpenPanel` lub `NSSavePanel`. Powerbox dostępu jest włączona za pośrednictwem uprawnienia ustawione dla aplikacji platformy Xamarin.Mac.

Piaskownicy aplikacji wyświetla otwartą lub okno dialogowe zapisywania, okno jest przedstawiony przez Powerbox (a nie w strukturze AppKit) i zatem ma dostęp do dowolnego pliku lub katalogu, który użytkownik ma dostęp do.

Gdy użytkownik wybiera z otwartego pliku lub katalogu lub okno dialogowe zapisywania (lub przeciągnie albo na ikonę aplikacji), Powerbox dodaje skojarzoną ścieżkę do piaskownicy aplikacji.

Ponadto system automatycznie umożliwia następujące polecenie, aby w trybie piaskownicy aplikacji:

- Połączenia z systemem metody wejściowej.
- Wywołaj wybrane przez użytkownika z poziomu usługi **usług** menu (tylko dla usług oznaczone jako _bezpiecznych aplikacji piaskownicy_ przez dostawcę usług).
- Otwarte pliki wybierz przez użytkownika powiązanego z **Otwórz ostatni** menu.
- Użyj kopiowanie i wklejanie między aplikacjami.
- Odczytywać pliki w następujących lokalizacjach na świecie do odczytu:
    - `/bin`
    - `/sbin`
    - `/usr/bin`
    - `/usr/lib`
    - `/usr/sbin`
    - `/usr/share`
    - `/System`
- Odczytywanie i zapisywanie plików w katalogach utworzone przez `NSTemporaryDirectory`.

Czy pliki otworzyć lub zapisać w trybie piaskownicy aplikacji rozszerzenia Xamarin.Mac być domyślny, pozostaną dostępne do momentu zakończenia aplikacji (chyba, że plik był nadal otwarte, gdy kończy działanie aplikacji). Otwarte pliki zostanie przywrócona do piaskownicy aplikacji za pośrednictwem funkcji wznowienia z systemem macOS przy następnym uruchomieniu aplikacji.

Aby zapewnić trwałość do plików znajdujących się poza kontenerem aplikacji rozszerzenia Xamarin.Mac, należy użyć zakładki z zakresu zabezpieczeń (patrz poniżej).

#### <a name="related-items"></a>Powiązane elementy

Piaskownica aplikacji umożliwia aplikacji dostęp do powiązanych elementów, które mają taką samą nazwę, ale inne rozszerzenia. Ta funkcja ma dwie części:) listę powiązanych rozszerzeń aplikacji `Info.plst` plik, (b) kod, aby poinformować piaskownicy, co aplikacja będzie wykonywać czynności z tymi plikami.

Istnieją dwa scenariusze, w którym to logiczne:

1. Aplikacja musi można było zapisać inną wersję pliku (z rozszerzeniem nowe). Na przykład, eksportowanie `.txt` plik `.pdf` pliku. Aby obsłużyć taką sytuację, należy użyć `NSFileCoordinator` dostępu do tego pliku. Wywołasz `WillMove(fromURL, toURL)` metoda najpierw przenieść plik nowe rozszerzenie, a następnie wywołać `ItemMoved(fromURL, toURL)`.
2. Aplikacja musi otworzyć głównego pliku z rozszerzeniem jednego i kilku plików pomocniczych przy użyciu różnych rozszerzeń. Na przykład film i plik z napisami. Użyj `NSFilePresenter` do uzyskania dostępu do plików pomocniczych. Główny plik, aby zapewnić `PrimaryPresentedItemURL` właściwość i pomocniczy plik do `PresentedItemURL` właściwości. Po otwarciu pliku głównego wywołać `AddFilePresenter` metody `NSFileCoordinator` klasy w celu rejestracji dodatkowych plików.

W obu przypadkach aplikacja firmy **Info.plist** pliku musi deklarować typów dokumentów, który może być otwarty w aplikacji. Dla dowolnego typu pliku Dodaj `NSIsRelatedItemType` (o wartości `YES`) do wejścia w `CFBundleDocumentTypes` tablicy.

#### <a name="open-and-save-dialog-behavior-with-sandboxed-apps"></a>Otwieranie i zapisywanie zachowanie okna dialogowego za pomocą aplikacji w trybie piaskownicy

Następujące limity są umieszczane w `NSOpenPanel` i `NSSavePanel` podczas wywoływania je z aplikacją platformy Xamarin.Mac w piaskownicy:

- Nie można wywołać programowo **OK** przycisku.
- Nie można programowo zmienić wybór użytkownika `NSOpenSavePanelDelegate`.

Ponadto wprowadzenie następujących modyfikacji dziedziczenia zostały spełnione:

- **Piaskownicy aplikacji** - `NSOpenPanel` `NSSavePanel``NSPanel``NSWindow``NSResponder``NSObject``NSOpenPanel``NSSavePanel``NSObject``NSOpenPanel``NSSavePanel`

### <a name="security-scoped-bookmarks-and-persistent-resource-access"></a>Zakładki w zakresie zabezpieczeń i dostęp do zasobów trwałe

Jak wspomniano powyżej, aplikacji rozszerzenia Xamarin.Mac w piaskownicy mogą uzyskać dostęp do pliku bądź zasobu poza jej kontenerem za pomocą bezpośredniej interakcji użytkownika (co zostało opisane przez PowerBox). Jednak dostęp do tych zasobów nie automatycznie ulegają zmianie podczas uruchomienia aplikacji lub systemu ponownego uruchomienia.

Za pomocą _zakładki Security-Scoped_, aplikacji rozszerzenia Xamarin.Mac w piaskownicy można zachować intencji użytkownika i zachować dostęp do podanych zasobów po aplikacji, uruchom ponownie.

#### <a name="security-scoped-bookmark-types"></a>Typy zakładki w zakresie zabezpieczeń

Podczas pracy z zakładkami Security-Scoped i trwały dostęp do zasobów, istnieją dwa przypadki użycia sistine:

- **Zakładki App-Scoped udostępnia trwały dostęp do określonych przez użytkownika pliku lub folderu.** 

    Na przykład, jeśli aplikacja w trybie piaskownicy platformy Xamarin.Mac umożliwia Użyj, aby otworzyć do edycji dokumentu zewnętrznego (przy użyciu `NSOpenPanel`), aplikacji, które można utworzyć zakładki App-Scoped tak, aby go w przyszłości dostęp do tego samego pliku.
- **Zakładki Document-Scoped zawiera określony dokument, trwały dostęp do podrzędnego pliku.** 

Na przykład wideo, edycji aplikacji, która tworzy pliku projektu, który ma dostęp do poszczególnych obrazów, klipów wideo i pliki dźwiękowe, które później zostaną połączone w jednym filmu.

Gdy użytkownik importuje plik zasobów do projektu (za pośrednictwem `NSOpenPanel`), aplikacja tworzy zakładki Document-Scoped dla elementu, który jest przechowywany w projekcie, aby plik zawsze jest dostępny do aplikacji.

Zakładki Document-Scoped można rozwiązać przez dowolną aplikację, który można otworzyć danych zakładki i samego dokumentu. Obejmuje to obsługę przenoszenia, umożliwiając wysyłanie plików projektu do innego użytkownika i o wszystkie zakładki dla nich również działać.

> [!IMPORTANT]
> Można zakładki Document-Scoped _tylko_ wskaż pojedynczy plik, a nie folder i plik nie może być w lokalizacji, używaną przez system (takie jak `/private` lub `/Library`).

#### <a name="using-security-scoped-bookmarks"></a>Korzystanie z zakładek w zakresie zabezpieczeń

Za pomocą dowolnego typu Security-Scoped zakładki, wymaga wykonaj następujące czynności:

1. **Ustawić odpowiednie uprawnienia w aplikacji platformy Xamarin.Mac, który musi używać zakładek Security-Scoped** — Ustaw zakładki For App-Scoped `com.apple.security.files.bookmarks.app-scope` klucza uprawnienie `true`. Document-Scoped zakładek, można ustawić `com.apple.security.files.bookmarks.document-scope` klucza uprawnienie `true`.
2. **Utworzyć zakładkę Security-Scoped** — możesz to zrobić dla dowolnego pliku lub folderu, który użytkownik udostępnił dostęp do (za pośrednictwem `NSOpenPanel` na przykład), że musi trwały dostęp do aplikacji. Użyj `public virtual NSData CreateBookmarkData (NSUrlBookmarkCreationOptions options, string[] resourceValues, NSUrl relativeUrl, out NSError error)` metody `NSUrl` klasy w celu utworzenia zakładki.
3. **Rozwiąż zakładki Security-Scoped** — gdy aplikacja potrzebuje dostępu do zasobu ponownie (na przykład po ponownym uruchomieniu) trzeba będzie ją rozpoznać zakładki z adresem URL należące do zakresu zabezpieczeń. Użyj `public static NSUrl FromBookmarkData (NSData data, NSUrlBookmarkResolutionOptions options, NSUrl relativeToUrl, out bool isStale, out NSError error)` metody `NSUrl` klasy, aby rozwiązać zakładki.
4. **Jawnie powiadomić System, który chcesz uzyskać dostęp do pliku z adresu URL Security-Scoped** — ten krok należy wykonać bezpośrednio po uzyskaniu Security-Scoped adresu URL podanego powyżej, lub jeśli zechcesz później odzyskać dostęp do zasobu po opuści dostępu do niego. Wywołaj `StartAccessingSecurityScopedResource ()` metody `NSUrl` klasy, aby można było rozpocząć uzyskiwanie dostępu do adresu URL Security-Scoped.
5. **Jawnie powiadomić systemu, które są wykonywane z adresu URL Security-Scoped dostępu do pliku** — tak szybko, jak to możliwe, powinno poinformować System, gdy aplikacja nie wymaga dostępu do pliku (na przykład, jeśli użytkownik zamknie go). Wywołaj `StopAccessingSecurityScopedResource ()` metody `NSUrl` klasy, aby zatrzymać, uzyskiwanie dostępu do adresu URL Security-Scoped.

Po użytkownik zrzeka się dostępu do zasobu, musisz powrócić do kroku 4 ponownie, aby ponownie ustanowić dostępu. Po uruchomieniu aplikacji rozszerzenia Xamarin.Mac należy powrócić do kroku 3 i ponownie rozpoznawać zakładki.

> [!IMPORTANT]
> Błąd wersji dostęp do zasobów adresu URL Security-Scoped spowoduje, że aplikacji rozszerzenia Xamarin.Mac zostały publicznie udostępnione zasoby jądra. Co w efekcie aplikacji nie będzie można dodać lokalizacji systemu plików do jego kontenera, dopóki nie zostanie uruchomiona ponownie.

### <a name="the-app-sandbox-and-code-signing"></a>Piaskownica aplikacji i podpisywanie kodu

Po Włącz piaskownica aplikacji i włączenia konkretne wymagania w przypadku aplikacji platformy Xamarin.Mac (za pośrednictwem uprawnień do) musi kodu znaku projektu dla piaskownicy zaczęły obowiązywać. Należy wykonać kod podpisania, ponieważ uprawnienia wymagane do piaskownicy aplikacji są połączone z podpisu aplikacji. 

System macOS wymusza połączenie między kontenerem aplikacji oraz jeho signatura kodu w ten sposób żadna inna aplikacja nie będą mogli tego kontenera, nawet wtedy, gdy jest fałszowania, aplikacji, identyfikator pakietu. Mechanizm ten działa następująco:

1. Gdy System tworzy kontener aplikacji, ustawia _listy kontroli dostępu_ (ACL) w danym kontenerze. Wpisu kontroli dostępu początkowej liście zawiera aplikację _wyznaczony wymaganie_ (DR), która opisuje, jak w przyszłych wersjach aplikacji mogą być rozpoznawane (jeśli został uaktualniony).
2. Za każdym razem uruchamia aplikację przy użyciu tego samego Identyfikatora pakietu, system sprawdza, czy podpis kodu aplikacji pasuje do wyznaczonego wymagania określone w jeden z wpisów w liście ACL kontenera. Jeśli system znajdzie dopasowanie, uniemożliwia jej uruchomienie.

Code Signing działa w następujący sposób:

1. Przed utworzeniem projektu platformy Xamarin.Mac, należy uzyskać certyfikatu deweloperskiego, certyfikat dystrybucji i certyfikat Identyfikatora dewelopera z portalu dla deweloperów firmy Apple.
2. Mac App Store dystrybuuje aplikacji rozszerzenia Xamarin.Mac, jest podpisany za pomocą podpisu kod firmy Apple.

Podczas testowania i debugowania, należy używać wersji aplikacji rozszerzenia Xamarin.Mac podpisany (który będzie służyć do utworzenia kontenera aplikacji). Później Jeśli chcesz przetestować lub zainstaluj wersję z Apple Store App go zostanie ona podpisana za pomocą podpisu firmy Apple i nie będzie można uruchomić (ponieważ nie ma taki sam podpis kodu jako oryginalnego kontenera aplikacji). W takiej sytuacji zostanie wyświetlony raport o awarii podobny do następującego:

```csharp
Exception Type:  EXC_BAD_INSTRUCTION (SIGILL)
```

Aby rozwiązać ten problem, należy dostosować wpis listy ACL, aby wskazywał Apple podpisaną wersję aplikacji.

Aby uzyskać więcej informacji na temat tworzenia i pobierania profilów aprowizacji wymaganych dla piaskownicy, zobacz [podpisywania i inicjowania obsługi administracyjnej aplikacji](#Signing_and_Provisioning_the_App) powyższej sekcji.

#### <a name="adjusting-the-acl-entry"></a>Dostosowywanie wpisu na liście ACL

Aby umożliwić Apple podpisaną wersję aplikacji rozszerzenia Xamarin.Mac do uruchomienia, wykonaj następujące czynności:

1. Otwórz aplikację Terminal (w `/Applications/Utilities`).
2. Otwórz okno wyszukiwania, aby Apple podpisaną wersję aplikacji rozszerzenia Xamarin.Mac.
3. Typ `asctl container acl add -file ` w oknie terminala.
4. Przeciągnij ikonę aplikacji rozszerzenia Xamarin.Mac w oknie wyszukiwania i upuść go w oknie terminala.
5. Pełna ścieżka do pliku zostanie dodany do polecenia w terminalu.
6. Naciśnij klawisz **Enter** można wykonać polecenia.

Listy ACL kontenera zawiera teraz wymagania wyznaczonym kod dla obu wersji aplikacji rozszerzenia Xamarin.Mac i macOS teraz umożliwi danej wersji uruchomić.

#### <a name="display-a-list-of-acl-code-requirements"></a>Wyświetl listę określonych wymagań dotyczących kodu listy ACL

Aby wyświetlić listę wymagania dotyczące kodu liście ACL kontenera, wykonując następujące czynności:

1. Otwórz aplikację Terminal (w `/Applications/Utilities`).
2. Typ `asctl container acl list -bundle <container-name>`.
3. Naciśnij klawisz **Enter** można wykonać polecenia.

`<container-name>` To zazwyczaj identyfikator pakietu dla aplikacji platformy Xamarin.Mac.

## <a name="designing-a-xamarinmac-app-for-the-app-sandbox"></a>Projektowanie aplikacji rozszerzenia Xamarin.Mac w piaskownicy aplikacji

Brak Typowy przepływ pracy, którymi należy się kierować podczas projektowania aplikacji rozszerzenia Xamarin.Mac w piaskownicy aplikacji. Inaczej mówiąc, szczegółowe informacje na temat wdrażania piaskownicy w aplikacji mają być unikatowy w danej aplikacji funkcji.

### <a name="six-steps-for-adopting-the-app-sandbox"></a>Sześć kroków do przyjęcia piaskownica aplikacji

Projektowanie aplikacji rozszerzenia Xamarin.Mac piaskownica aplikacji zwykle składa się z następujących czynności:

1. Ustal, czy aplikacja jest odpowiednia dla piaskownicy.
2. Projektowanie strategii tworzenia i dystrybucji.
3. Rozwiąż wszelkie niezgodności interfejsu API.
4. Zastosuj wymaganych uprawnień piaskownicy aplikacji do projektu platformy Xamarin.Mac.
5. Dodaj separacji uprawnień za pomocą XPC.
6. Zaimplementować strategię migracji.

> [!IMPORTANT]
> Należy nie tylko piaskownicy głównego pliku wykonywalnego w pakietu aplikacji, ale także na uwzględnione pomocnika aplikacji lub narzędzia, w tym pakiecie. Jest wymagana dla każdej aplikacji rozproszonej z Mac App Store i, jeśli jest to możliwe powinno się odbywać na jakąkolwiek inną formę dystrybucji aplikacji.

Aby uzyskać listę wszystkich plików binarnych z pliku wykonywalnego w pakiecie aplikacji rozszerzenia Xamarin.Mac wpisz następujące polecenie w terminalu:

```bash
find -H [Your-App-Bundle].app -print0 | xargs -0 file | grep "Mach-O .*executable"
```

Gdzie `[Your-App-Bundle]` jest nazwa i ścieżka do pakietu aplikacji.

### <a name="determine-whether-a-xamarinmac-app-is-suitable-for-sandboxing"></a>Ustal, czy aplikacji rozszerzenia Xamarin.Mac jest odpowiednia dla piaskownicy

Większość aplikacji rozszerzenia Xamarin.Mac są w pełni zgodne z Piaskownicą aplikacji i w związku z tym, odpowiednia dla piaskownicy. Jeśli aplikacja wymaga zachowania, które nie zezwala na piaskownica aplikacji, należy rozważyć alternatywne podejście.

Jeśli aplikacja wymaga jednej z następujących zachowań, jest niezgodny z piaskownicy aplikacji:

- **Usługi autoryzacji** — za pomocą piaskownica aplikacji, nie możesz pracować z funkcji opisanych w [odwołanie C usługi autoryzacji](https://developer.apple.com/library/prerelease/mac/documentation/Security/Reference/authorization_ref/index.html#//apple_ref/doc/uid/TP30000826).
- **Interfejsy API w ułatwienia dostępu** — nie można w piaskownicy aplikacji ułatwień dostępu, takich jak czytniki zawartości ekranu lub aplikacje, które kontrolują inne aplikacje.
- **Wysyłanie zdarzeń firmy Apple do dowolnych aplikacji** — Jeśli aplikacja wymaga wysyłania zdarzeń do firmy Apple do nieznanego, dowolną aplikację, nie może być piaskownicy. Listę znanych aplikacji o nazwie aplikacja nadal może być w trybie piaskownicy oraz uprawnienia będzie konieczne uwzględnienie.NET liście aplikacji o nazwie.
- **Wysyłanie słowników informacje użytkownika w powiadomieniach dystrybuowane do innych zadań** — za pomocą piaskownica aplikacji, nie może zawierać `userInfo` słownika ogłaszając w `NSDistributedNotificationCenter` obiektu dla wiadomości do innych zadań.
- **Ładowanie rozszerzeń jądra** -ładowanie rozszerzeń jądra jest zabronione przez piaskownica aplikacji.
- **Symulowanie użytkownika danych wejściowych w Open i zapisać okien dialogowych** — programowe operowanie Open lub Zapisz okien dialogowych, aby zasymulować lub zmienić dane wejściowe użytkownika jest zabronione przez piaskownica aplikacji.
- **Uzyskiwanie dostępu do lub ustawianie preferencji w innych aplikacjach** -manipulowanie ustawienia innych aplikacji jest zabronione przez piaskownica aplikacji.
- **Konfigurowanie ustawień sieci** -manipulowanie ustawień sieci jest zabronione przez piaskownica aplikacji.
- **Kończenie inne aplikacje** -piaskownica aplikacji zabrania przy użyciu `NSRunningApplication` zakończyć inne aplikacje.

### <a name="resolving-api-incompatibilities"></a>Rozpoznawanie niezgodności interfejsu API

Podczas projektowania aplikacji rozszerzenia Xamarin.Mac w piaskownicy aplikacji, możesz napotkać niezgodności z użyciem niektórych interfejsów API w systemu MacOS.

Poniżej przedstawiono kilka typowych problemów i czynności, które można wykonać w celu rozwiązania tych problemów:

- **Otwieranie, zapisywanie i śledzenie dokumentów** — w przypadku zarządzania dokumentów innych niż przy użyciu dowolnej technologii `NSDocument`, należy przełączyć się do niego dzięki wbudowanej obsłudze piaskownica aplikacji. `NSDocument` automatycznie współpracuje z PowerBox i zapewnia obsługę przechowywania dokumentów w piaskownicy usługi, gdy użytkownik przesuwa je w programie Finder.
- **Zachować dostęp do zasobów systemu plików** — w przypadku platformy Xamarin.Mac, aplikacja jest zależna od trwały dostęp do zasobów poza jej kontenerem używanie zakresu zabezpieczeń zakładek, aby zachować dostęp.
- **Tworzenie elementu logowania dla aplikacji** — za pomocą piaskownica aplikacji, nie można utworzyć elementu przy użyciu nazwy logowania `LSSharedFileList` ani można manipulować stan uruchamiania usług przy użyciu `LSRegisterURL`. Użyj `SMLoginItemSetEnabled` działać zgodnie z opisem w jabłek [dodawania elementów do logowania za pomocą usługi Management Framework](https://developer.apple.com/library/prerelease/mac/documentation/MacOSX/Conceptual/BPSystemStartup/Chapters/CreatingLoginItems.html#//apple_ref/doc/uid/10000172i-SW5-SW1) dokumentacji.
- **Uzyskiwanie dostępu do danych użytkownika** — Jeśli używasz POSIX funkcje takie jak `getpwuid` można uzyskać katalogu macierzystego użytkownika z usług katalogowych, należy wziąć pod uwagę przy użyciu symboli Cocoa lub Core Foundation, takich jak `NSHomeDirectory`.
- **Dostęp do preferencji inne aplikacje** — ponieważ piaskownica aplikacji kieruje ścieżki wyszukiwania interfejsów API do aplikacji kontenera, modyfikowanie preferencje ma miejsce, w tym kontenerze, a także dostęp do innego preferencji aplikacje w niedozwolone. 
- **W widoku sieci Web przy użyciu osadzone wideo HTML5** — Jeśli aplikacja platformy Xamarin.Mac używa aparatu WebKit do odtwarzania osadzone wideo HTML5, możesz połączyć aplikacji struktury Foundation pliki audio i wideo. Piaskownica aplikacji uniemożliwi CoreMedia odtwarzania w tych filmach wideo, w przeciwnym razie.

### <a name="applying-required-app-sandbox-entitlements"></a>Stosowanie wymagane uprawnienia piaskownica aplikacji

Konieczne będzie edytowanie uprawnień dla dowolnej aplikacji platformy Xamarin.Mac, który chcesz uruchomić w piaskownicy aplikacji i sprawdź **włączyć tryb piaskownicy aplikacji** pola wyboru.

Oparte na funkcji aplikacji, może być konieczne włączenie innych uprawnień dostępu do funkcji systemu operacyjnego lub zasobów. Aplikacji piaskownicy działa najlepiej, gdy można zminimalizować uprawnień, które żądania absolutnego minimum wymagane do uruchamiania aplikacji po prostu losowo Włącz więc uprawnień.

Aby określić uprawnienia, które wymaga aplikacji rozszerzenia Xamarin.Mac, wykonaj następujące czynności:

1. Włącz piaskownica aplikacji i uruchamianie aplikacji platformy Xamarin.Mac.
2. Uruchom przy użyciu funkcji aplikacji.
3. Otwórz aplikację konsoli (dostępne w `/Applications/Utilities`) i poszukaj `sandboxd` naruszeń w **wszystkie komunikaty** dziennika.
4. Dla każdego `sandboxd` naruszenie, rozwiąż problem, za pomocą kontenera aplikacji zamiast inne lokalizacje w systemie plików lub zastosować uprawnień piaskownicy aplikacji, aby umożliwić dostęp do ograniczonych funkcji systemu operacyjnego.
5. Uruchom ponownie, a następnie ponownie przetestuj wszystkie funkcje aplikacji platformy Xamarin.Mac.
6. Powtórz tę procedurę, aż wszystkie `sandboxd` naruszenia zostały rozwiązane.

### <a name="add-privilege-separation-using-xpc"></a>Dodawanie uprawnień separacja przy użyciu XPC

Podczas tworzenia aplikacji rozszerzenia Xamarin.Mac w piaskownicy aplikacji, Przyjrzyj się zachowania aplikacji w postanowieniach dotyczących uprawnień i dostępu, a następnie należy wziąć pod uwagę oddzielenie operacji wysokiego ryzyka w ich własnych usług XPC.

Aby uzyskać więcej informacji, zobacz firmy Apple [tworzenie usług XPC](https://developer.apple.com/library/prerelease/mac/documentation/MacOSX/Conceptual/BPSystemStartup/Chapters/CreatingXPCServices.html#//apple_ref/doc/uid/10000172i-SW6) i [demonów i przewodnik dotyczący programowania usług](https://developer.apple.com/library/prerelease/mac/documentation/MacOSX/Conceptual/BPSystemStartup/Chapters/Introduction.html#//apple_ref/doc/uid/10000172i).

### <a name="implement-a-migration-strategy"></a>Wdrożenie strategii migracji

Udostępnimy nowe, w trybie piaskownicy wersję aplikacji platformy Xamarin.Mac, który nie był wcześniej w trybie piaskownicy, musisz upewnij się, że bieżący użytkownicy smooth ścieżki uaktualnienia. 

 Aby uzyskać więcej informacji na temat sposobu implementacji manifest migracji kontenera, przeczytaj firmy Apple [migrowanie aplikacji do piaskownicy](https://developer.apple.com/library/prerelease/mac/documentation/Security/Conceptual/AppSandboxDesignGuide/MigratingALegacyApp/MigratingAnAppToASandbox.html#//apple_ref/doc/uid/TP40011183-CH6-SW1) dokumentacji.

## <a name="summary"></a>Podsumowanie

W tym artykule jest zajęta szczegółowe omówienie w piaskownicy aplikacji platformy Xamarin.Mac. Po pierwsze utworzyliśmy aplikację platformy Xamarin.Mac po prostu Aby wyświetlić podstawowe informacje dotyczące piaskownica aplikacji. Następnie możemy pokazano, jak rozwiązać naruszeń piaskownicy. Następnie skorzystaliśmy szczegółowe Przyjrzyj się piaskownica aplikacji, a na koniec przyjrzeliśmy się projektowanie aplikacji rozszerzenia Xamarin.Mac w piaskownicy aplikacji.



## <a name="related-links"></a>Linki pokrewne

- [Publikowanie w sklepie App Store](~/mac/deploy-test/publishing-to-the-app-store/index.md)
- [Piaskownica aplikacji — informacje](https://developer.apple.com/library/content/documentation/Security/Conceptual/AppSandboxDesignGuide/AboutAppSandbox/AboutAppSandbox.html)
- [Typowe problemy piaskownicy aplikacji](https://developer.apple.com/library/content/qa/qa1773/_index.html)
