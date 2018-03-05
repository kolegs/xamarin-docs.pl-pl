---
title: Sandboxing aplikacji Xamarin.Mac
description: "W tym artykule omówiono sandboxing aplikacją Xamarin.Mac w wersji ze sklepu App Store. Obejmuje wszystkie elementy, które wchodzą w sandboxing, takie jak katalogi kontenera, uprawnień, uprawnienia użytkownika, separacji uprawnień i wymuszania jądra."
ms.topic: article
ms.prod: xamarin
ms.assetid: 06A2CA8D-1E46-410F-8C31-00EA36F0735D
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 9e64f1962e35372a6058f4b515efa5a61c1c9e45
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
---
# <a name="sandboxing-a-xamarinmac-app"></a>Sandboxing aplikacji Xamarin.Mac

_W tym artykule omówiono sandboxing aplikacją Xamarin.Mac w wersji ze sklepu App Store. Obejmuje wszystkie elementy, które wchodzą w sandboxing, takie jak katalogi kontenera, uprawnień, uprawnienia użytkownika, separacji uprawnień i wymuszania jądra._

## <a name="overview"></a>Omówienie

Podczas pracy z C# i .NET w aplikacji Xamarin.Mac, masz tego samego możliwość piaskownicy aplikacji, jak w przypadku pracy z języka Objective-C lub Swift.

[![Przykład uruchomionej aplikacji](sandboxing-images/intro01.png "przykładem uruchomionej aplikacji")](sandboxing-images/intro01-large.png)

W tym artykule omówione zostaną następujące czynności podstawowe informacje dotyczące pracy z sandboxing w aplikacji Xamarin.Mac i wszystkie elementy, które wchodzą w sandboxing: kontener katalogów, uprawnień, uprawnienia użytkownika, separacji uprawnień i wymuszania jądra. Zdecydowanie zaleca się pracę za pośrednictwem [Hello, Mac](~/mac/get-started/hello-mac.md) artykuł najpierw, w szczególności [wprowadzenie do programów Xcode i kompilatora interfejsu](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) i [gniazda i akcje](~/mac/get-started/hello-mac.md#Outlets_and_Actions) sekcje, w jakiej omawia kluczowe założenia i techniki, które będzie używana w tym artykule.

Może zajść potrzeba Przyjrzyjmy się [udostępnianie klasy języka C# / metody Objective-C](~/mac/internals/how-it-works.md) sekcji [wewnętrzne Xamarin.Mac](~/mac/internals/how-it-works.md) dokumentu również wyjaśniono `Register` i `Export` atrybutów umożliwiają połączenie klas C# do obiektów języka Objective C i interfejsu użytkownika elementy.

## <a name="about-the-app-sandbox"></a>O piaskownicy aplikacji

Piaskownica aplikacji udostępnia silną ochronę przed uszkodzeń, które może być spowodowana przez aplikację złośliwego uruchomione na komputerze Mac poprzez ograniczenie dostępu aplikacji do zasobów systemowych.

Piaskownicy aplikacji ma pełne prawa użytkownika, który jest uruchomiona aplikacja i można uzyskać dostępu do lub wykonywać żadnych czynności, które użytkownik może. Jeśli ta aplikacja zawiera luk zabezpieczeń (lub dowolnego platforma, która używa), haker może potencjalnie te luki i korzystać z aplikacji, aby przejąć kontrolę nad Mac, który jest uruchomiony na.

Poprzez ograniczenie dostępu do zasobów na podstawie poszczególnych aplikacji, aplikacji piaskownicy zawiera linię obrony przed kradzieżą, uszkodzenia lub złośliwymi działaniami ze strony aplikacji uruchomionej na komputerze użytkownika.

Piaskownicy aplikacji to technologia kontroli dostępu, wbudowane w system macOS (wymuszane na poziomie jądra), która zapewnia strategii dwa cele:

1. Piaskownica aplikacji umożliwia deweloperowi opisano _jak_ aplikacja będzie wchodzić z systemem operacyjnym i w ten sposób, że udzielono mu prawa dostępu, które są wymagane, aby wykonać zadanie, a nie więcej.
2. Piaskownica aplikacji zezwala użytkownikowi na bezproblemowo dalsze udostępnienia systemu za pośrednictwem Otwórz i Zapisz okien dialogowych, przeciągnij i upuść operacje i interakcji użytkowników z innych, wspólnej.

### <a name="preparing-to-implement-the-app-sandbox"></a>Trwa przygotowywanie do wdrożenia aplikacji piaskownicy

Dostępne są następujące elementy piaskownicy aplikacji, która zostanie omówiona szczegółowo w artykule:

- Katalogi kontenera
- Uprawnienia
- Uprawnienia użytkownika
- Rozdzielenie uprawnień
- Kernel Enforcement

Po zidentyfikowaniu tych informacji będzie można utworzyć plan przyjmowania piaskownicy aplikacji w aplikacji Xamarin.Mac.

Po pierwsze należy ustalić, czy aplikacja jest dobrym kandydatem do sandboxing (większość aplikacji są). Następnie należy rozwiązać wszystkie niezgodności interfejsu API i określić, które elementy piaskownicy aplikacji będą wymagane. Na koniec Szukaj w celu zmaksymalizowania wykorzystania poziom ochrony aplikacji przy użyciu uprawnień separacji.

Przyjmując piaskownicy aplikacji, niektóre lokalizacje w systemie plików przez aplikację może się różnić. Szczególnie aplikacja ma katalogu kontenera, który będzie służyć do plików obsługi aplikacji, baz danych, pamięci podręcznych i innych plików, które nie są dokumenty użytkowników. Zarówno system macOS i Xcode zapewniają obsługę migracji tych typów plików z ich lokalizacje starszy do kontenera.

## <a name="sandboxing-quick-start"></a>Sandboxing szybki start

W tej sekcji utworzymy prostej aplikacji Xamarin.Mac, która używa widoku sieci Web (co wymaga połączenia sieciowego, który jest ograniczona w obszarze sandboxing, chyba że wyraźnie wymagane) na przykład wprowadzenie do korzystania z aplikacji piaskownicy.

Zweryfikujemy się, że aplikacja jest w rzeczywistości w trybie piaskownicy i Dowiedz się, jak rozwiązywanie problemów i rozwiązania typowych błędów aplikacji piaskownicy.

### <a name="creating-the-xamarinmac-project"></a>Tworzenie projektu Xamarin.Mac

Załóżmy wykonaj następujące polecenie, aby utworzyć naszego przykładowego projektu:

1. Uruchom program Visual Studio for Mac i kliknij przycisk **nowe rozwiązanie...** link.
2. Z **nowy projekt** okno dialogowe, wybierz opcję **Mac** > **aplikacji** > **Cocoa aplikacji**: 

    [![Tworzenie nowej aplikacji Cocoa](sandboxing-images/sample01.png "Tworzenie nowej aplikacji Cocoa")](sandboxing-images/sample01-large.png)
3. Kliknij przycisk **dalej** przycisku, wprowadź `MacSandbox` nazwę projektu i kliknij **Utwórz** przycisk: 

    [![Wprowadź nazwę aplikacji](sandboxing-images/sample02.png "wprowadzania nazwy aplikacji")](sandboxing-images/sample02-large.png)
4. W **konsoli rozwiązania**, kliknij dwukrotnie **Main.storyboard** plik, aby otworzyć do edycji w środowisku Xcode: 

    [![Edytowanie głównego storyboard](sandboxing-images/sample03.png "edytowania głównego storyboard")](sandboxing-images/sample03-large.png)
5. Przeciągnij **widoku sieci Web** na okna, rozmiar, aby wypełnił obszar zawartości i ustaw dla niej zwiększyć lub zmniejszyć w oknie: 

    [![Dodawanie widoku sieci web](sandboxing-images/sample04.png "Dodawanie widoku sieci web")](sandboxing-images/sample04-large.png)
6. Tworzenie gniazda widoku sieci web o nazwie `webView`: 

    [![Tworzenie nowego gniazda](sandboxing-images/sample05.png "tworzenie nowych gniazda")](sandboxing-images/sample05-large.png)
7. Wróć do programu Visual Studio for Mac i kliknij dwukrotnie **ViewController.cs** w pliku **konsoli rozwiązania** go otworzyć do edycji.
8. Dodaj następującą instrukcję using: `using WebKit;`
9. Wprowadź `ViewDidLoad` wygląd metody podobne do poniższych: 

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();
    webView.MainFrame.LoadRequest(new NSUrlRequest(new NSUrl("http://www.apple.com")));
}
```

10. Zapisz zmiany.

Uruchom możesz aplikacji i upewnij się, że witryna sieci Web firmy Apple jest wyświetlany w oknie w następujący sposób:

[![Przedstawiający przykładową aplikację wykonywania](sandboxing-images/sample06.png "przedstawiający przykładową aplikację wykonywania")](sandboxing-images/sample06-large.png)

<a name="Signing_and_Provisioning_the_App" />

### <a name="signing-and-provisioning-the-app"></a>Podpisywanie i Inicjowanie obsługi aplikacji

Zanim będzie można włączyć obsługę piaskownicy aplikacji, najpierw musimy udostępnić i podpisywanie aplikacji Xamarin.Mac.

Pozwól, wykonaj następujące czynności:

1. Zaloguj się do portalu dla deweloperów firmy Apple: 

    [![Logowanie do portalu dla deweloperów firmy Apple](sandboxing-images/sign01.png "logowania do portalu dla deweloperów firmy Apple")](sandboxing-images/sign01-large.png)
2. Wybierz **certyfikaty, identyfikatory & profile**: 

    [![Wybieranie certyfikatów, identyfikatory & profile](sandboxing-images/sign02.png "wybranie certyfikaty, identyfikatory i profile")](sandboxing-images/sign02-large.png)
3. W obszarze **aplikacji Mac**, wybierz pozycję **identyfikatory**: 

    [![Wybieranie identyfikatory](sandboxing-images/sign03.png "wybranie identyfikatorów")](sandboxing-images/sign03-large.png)
4. Utwórz nowy identyfikator dla aplikacji: 

    [![Tworzenie nowego Identyfikatora aplikacji](sandboxing-images/sign04.png "tworzenia nowego Identyfikatora aplikacji")](sandboxing-images/sign04-large.png)
5. W obszarze **profile inicjowania obsługi**, wybierz pozycję **programowanie**: 

    [![Wybieranie programowanie](sandboxing-images/sign05.png "wybranie programowanie")](sandboxing-images/sign05-large.png)
6. Utwórz nowy profil, a następnie wybierz **tworzenie aplikacji dla komputerów Mac**: 

    [![Tworzenie nowego profilu](sandboxing-images/sign06.png "tworzenia nowego profilu")](sandboxing-images/sign06-large.png)
7. Wybierz identyfikator aplikacji, które zostały utworzone powyżej: 

    [![Wybieranie identyfikator aplikacji](sandboxing-images/sign07.png "wybranie identyfikator aplikacji")](sandboxing-images/sign07-large.png)
8. Wybierz deweloperzy dla tego profilu: 

    [![Dodawanie deweloperzy](sandboxing-images/sign08.png "programistom dodawanie")](sandboxing-images/sign08-large.png)
9. Wybierz komputery dla tego profilu: 

    [![Wybieranie komputerów dozwolonych](sandboxing-images/sign09.png "wybranie dozwolone komputery")](sandboxing-images/sign09-large.png)
10. Nadaj nazwę profilu: 

    [![Nadanie nazwy profilu](sandboxing-images/sign10.png "nadanie nazwy profilu")](sandboxing-images/sign10-large.png)
11. Kliknij przycisk **gotowe** przycisku.

> [!IMPORTANT]
> W niektórych przypadkach może być konieczne bezpośrednio pobrać nowy profil inicjowania obsługi administracyjnej z portalu dla deweloperów firmy Apple, a następnie kliknij dwukrotnie, aby zainstalować. Może być również konieczne zatrzymać i uruchomić ponownie program Visual Studio dla komputerów Mac, zanim będą mogli uzyskać dostępu do nowego profilu.

Następnie należy załadować nowy identyfikator aplikacji i profilu na komputerze deweloperskim. Teraz należy wykonać następujące czynności:

1. Uruchom środowisko Xcode i wybierz **preferencje** z **Xcode** menu: 

    ![Edytowanie kont w środowisku Xcode](sandboxing-images/sign11.png "edycji kont w środowisku Xcode")
2. Polecenie **wyświetlić szczegóły...**  przycisk: 

    ![Kliknięcie przycisku Wyświetl szczegóły](sandboxing-images/sign12.png "kliknięcie przycisku Wyświetl szczegóły")
3. Polecenie **Odśwież** przycisk (w dolnym rogu po lewej stronie).
4. Polecenie **gotowe** przycisku.

Następnie należy wybierz nowy identyfikator aplikacji i profilu inicjowania obsługi administracyjnej naszych Xamarin.Mac projektu. Teraz należy wykonać następujące czynności:

1. W **konsoli rozwiązania**, kliknij dwukrotnie **Info.plist** plik, aby otworzyć do edycji.
2. Upewnij się, że **identyfikator pakietu** odpowiada naszych identyfikator aplikacji, które zostały utworzone powyżej (przykład: `com.appracatappra.MacSandbox`): 

    [![Edytowanie identyfikator pakietu](sandboxing-images/sign13.png "edycji identyfikator pakietu")](sandboxing-images/sign13-large.png)
3. Następnie kliknij dwukrotnie **Entitlements.plist** i upewnij się, naszych **iCloud magazyn kluczy i wartości** i **iCloud kontenery** wszystkie zgodne naszych identyfikator aplikacji, które zostały utworzone powyżej (przykład: `com.appracatappra.MacSandbox`): 

    [![Edytowanie pliku Entitlements.plist](sandboxing-images/sign17.png "edytowania pliku Entitlements.plist")](sandboxing-images/sign17-large.png)
3. Zapisz zmiany.
4. W **konsoli rozwiązania**, kliknij dwukrotnie plik projektu, aby otworzyć Opcje edycji:  

    ![Editign opcje rozwiązania](sandboxing-images/sign14.png "Editign opcje rozwiązania")
5. Wybierz **podpisywania Mac**, następnie sprawdź **podpisywania pakietu aplikacji** i **podpisywanie pakietu Instalatora**. W obszarze **profil inicjowania obsługi administracyjnej**, wybierz jedną, które zostały utworzone powyżej: 

    ![Ustawienie profilu inicjowania obsługi administracyjnej](sandboxing-images/sign15.png "ustawienie profilu inicjowania obsługi administracyjnej")
6. Kliknij przycisk **gotowe** przycisku.

> [!IMPORTANT]
> **Uwaga:** może być konieczne zamknięcie i ponowne uruchomienie programu Visual Studio dla komputerów Mac uzyskać nowy identyfikator aplikacji i profilu inicjowania obsługi administracyjnej, zainstalowanym Xcode.

#### <a name="troubleshooting-provisioning-issues"></a>Rozwiązywanie problemów inicjowania obsługi administracyjnej

Na tym etapie należy uruchomić aplikację i upewnij się, że wszystko podpisane i poprawnie przygotowana. Jeśli aplikacja nadal działa jak poprzednio, wszystko jest dobra. W przypadku awarii może uzyskać okno dialogowe podobny do następującego:

[![Przykład problem okno dialogowe zastrzegania](sandboxing-images/sign16.png "przykład problem okno dialogowe zastrzegania")](sandboxing-images/sign16-large.png)

Poniżej przedstawiono najczęstszych przyczyn braku obsługi administracyjnej i podpisywania problemy:

- Identyfikator pakietu aplikacji jest niezgodny z Identyfikatorem aplikacji z wybranym profilem.
- Identyfikator dewelopera jest niezgodny z Identyfikatorem Developer wybranego profilu.
- Identyfikator UUID Mac testowane na nie jest zarejestrowana w ramach wybranego profilu.

W przypadku problemu Rozwiąż problem w portalu dla deweloperów firmy Apple, Odśwież profile w środowisku Xcode i wykonaj czystą kompilacji w programie Visual Studio dla komputerów Mac.

### <a name="enable-the-app-sandbox"></a>Włącz piaskownicy aplikacji

Piaskownicy aplikacji można włączyć, zaznaczając pole wyboru w opcjach projektów. Wykonaj następujące czynności:

1. W **konsoli rozwiązania**, kliknij dwukrotnie **Entitlements.plist** plik, aby otworzyć do edycji.
2. Sprawdź zarówno **włączenie uprawnień** i **włączyć Sandboxing aplikacji**: 

    [![Uprawnienia do edycji i włączanie sandboxing](sandboxing-images/sign17.png "uprawnień do edycji i włączanie sandboxing")](sandboxing-images/sign17-large.png)
3. Zapisz zmiany.

W tym momencie piaskownicy aplikacji zostało włączone, ale nie podano dostępu do sieci wymagane dla widoku sieci Web. Jeśli uruchomisz aplikację teraz, należy pobrać puste okno:

[![Wyświetlane jest zablokowany dostęp do sieci web](sandboxing-images/sample08.png "przedstawiający jest zablokowany dostęp do sieci web")](sandboxing-images/sample08-large.png)

### <a name="verifying-that-the-app-is-sandboxed"></a>Weryfikowanie, czy aplikacja jest w trybie piaskownicy

Jako uzupełnienie zasobów blokuje zachowanie istnieją trzy sposoby stwierdzić, czy aplikacja Xamarin.Mac został pomyślnie w trybie piaskownicy:

1. W polu wyszukiwania, sprawdź zawartość `~/Library/Containers/` folderu — Jeśli aplikacja jest w trybie piaskownicy, będą istnieć folder o nazwie, takich jak identyfikator pakietu aplikacji (przykład: `com.appracatappra.MacSandbox`): 

    [![Otwieranie pakietu aplikacji](sandboxing-images/sample09.png "otwierania pakietu aplikacji")](sandboxing-images/sample09-large.png)
2. System zidentyfikuje aplikacji jako piaskownicy w monitorze działania:
    - Uruchamianie Monitora aktywności (w obszarze `/Applications/Utilities`). 
    - Wybierz **widoku** > **kolumn** i upewnij się, że **piaskownicy** zaznaczono element menu.
    - Upewnij się, że kolumna piaskownicy odczytuje `Yes` aplikacji: 

    [![Kontrola aplikacji w monitorze działania](sandboxing-images/sample10.png "sprawdzanie aplikacji w monitorze działania")](sandboxing-images/sample10-large.png)
3. Sprawdź, czy binarny aplikacji jest w trybie piaskownicy:
    - Uruchamiają aplikację terminala.
    - Przejdź do aplikacji `bin` katalogu.
    - Wydać polecenie: `codesign -dvvv --entitlements :- executable_path` (gdzie `executable_path` to ścieżka do aplikacji): 

    [![Kontrola aplikacji w wierszu polecenia](sandboxing-images/sample11.png "sprawdzanie aplikacji w wierszu polecenia")](sandboxing-images/sample11-large.png)

### <a name="debugging-a-sandboxed-app"></a>Debugowanie w trybie piaskownicy aplikacji

Debuger łączy się Xamarin.Mac aplikacji za pośrednictwem protokołu TCP, co oznacza, że po włączeniu sandboxing, jest domyślnie nie można nawiązać połączenia z aplikacji, więc jeśli zostanie podjęta próba uruchomienia aplikacji bez odpowiednich uprawnień, które są włączone, występuje błąd *"nie można nawiązać połączenia z Debuger"*. 

[![Ustawianie opcji wymagane](sandboxing-images/debug01.png "ustawienie wymagane opcje")](sandboxing-images/debug01-large.png)

**Zezwalaj wychodzących połączeń sieciowych (klient)** uprawnienie jest wymagane dla debugera, włączenie tego umożliwi debugowania normalnie. Ponieważ nie można debugować bez niego, zostały zaktualizowane `CompileEntitlements` docelowe dla `msbuild` automatyczne dodawanie uprawnienie do uprawnień dla dowolnej aplikacji, która jest w trybie piaskownicy dla debugowania tylko kompilacje. Kompilacje wydania należy używać uprawnienia określone w pliku uprawnień, nie mają być modyfikowane.

### <a name="resolving-an-app-sandbox-violation"></a>Rozpoznawanie naruszenia piaskownicy aplikacji

Naruszenie piaskownicy aplikacji występuje, gdy Xamarin.Mac piaskownicy, aplikacja próbowała uzyskać dostęp do zasobu, który ma być jawnie zabronione. Na przykład naszych widoku sieci Web nie jest już wyświetlana witryny sieci Web firmy Apple.

Najbardziej typowe źródła naruszeń piaskownicy aplikacji występuje, gdy ustawieniami uprawnień w programie Visual Studio dla komputerów Mac nie pasuje do wymagań aplikacji. Ponownie z powrotem do naszym przykładzie Brak połączenia sieciowego uprawnień, które przechowują widoku sieci Web z pracy.

#### <a name="discovering-app-sandbox-violations"></a>Wykrywanie naruszeń piaskownicy aplikacji

Jeśli zachodzi podejrzenie naruszenia piaskownicy aplikacji występuje w aplikacji Xamarin.Mac, najszybszy sposób odnajdywania problem polega na użyciu **konsoli** aplikacji.

Wykonaj następujące czynności:

1. Kompilowanie aplikacji zagrożona i uruchom go w programie Visual Studio dla komputerów Mac.
2. Otwórz **konsoli** aplikacji (z `/Applications/Utilties/`).
3. Wybierz **wszystkie komunikaty** na pasku bocznym, a następnie wprowadź `sandbox` wyszukiwania: 

    [![Przykład problemem sandboxing w konsoli](sandboxing-images/resolve01.png "przykładem problemem sandboxing w konsoli programu")](sandboxing-images/resolve01-large.png)

Dla aplikacji przykład powyżej, można wyświetlić blokuje jądra `network-outbound` ruchu z powodu piaskownicy aplikacji, ponieważ nie żądano możemy prawo.

#### <a name="fixing-app-sandbox-violations-with-entitlements"></a>Rozwiązania piaskownicy aplikacji naruszeń z uprawnieniami

Teraz, gdy zaobserwowano jak znaleźć aplikacji Sandboxing naruszeń Zobaczmy, jak mogą zostać rozwiązane przez dostosowanie uprawnień do naszej aplikacji.

Wykonaj następujące czynności:

1. W **konsoli rozwiązania**, kliknij dwukrotnie **Entitlements.plist** plik, aby otworzyć do edycji.
2. W obszarze **uprawnień** sekcji wyboru **Zezwalaj wychodzących połączeń sieciowych (klient)** wyboru: 

    [![Edytowanie uprawnień](sandboxing-images/sign17.png "edycji uprawnień")](sandboxing-images/sign17-large.png)
3. Zapisz zmiany w aplikacji.

Jeśli firma Microsoft nie powyższych w naszym przykładzie aplikacji, następnie skompilować i uruchomić go, zawartość sieci web zostaną wyświetlone zgodnie z oczekiwaniami.

## <a name="the-app-sandbox-in-depth"></a>Szczegółowe piaskownicy aplikacji

Mechanizmy kontroli dostępu, które są udostępniane przez piaskownicy aplikacji są kilka i łatwy do zrozumienia. Jednak sposób, że piaskownicy aplikacji przyjmie każdej aplikacji jest unikatowa i zgodnie z wymaganiami aplikacji.

Podana starań do ochrony aplikacji Xamarin.Mac z wykorzystywana przez złośliwy kod, musi istnieć tylko jednej luki w jednej aplikacji (lub jednej z bibliotek i platform zużywa) przejąć kontrolę nad interakcje aplikacji System.

Piaskownica aplikacji została zaprojektowana w celu zapobiegania przejęcia (lub ograniczyć uszkodzenia, który może spowodować), umożliwiając określić interakcje zamierzone aplikacji w systemie. Tylko system będzie zezwalał na dostęp do zasobu, którego aplikacja wymaga, aby pobrać jego pracy i nic więcej.

Podczas projektowania aplikacji piaskownicy, projektowania najgorszego scenariusza. Jeśli aplikacja złamane przez złośliwy kod, jest ograniczona do dostęp do plików i zasobów w aplikacji piaskownicy.

### <a name="entitlements-and-system-resource-access"></a>Uprawnień i dostęp do zasobów systemu

Jak widzieliśmy wcześniej aplikacji Xamarin.Mac, która nie jest w trybie piaskownicy otrzymuje pełne prawa i dostęp użytkownika, który jest uruchomiona aplikacja. W przypadku naruszenia zabezpieczeń przez złośliwy kod niechronionych aplikacji może działać jako agenta dla zachowania szkodliwy z szerokiego zakresu potencjalnych za szkody.

Przez włączenie piaskownicy aplikacji, Usuń wszystkie oprócz minimalny zestaw uprawnień, która będzie następnie ponownie włącz dla tylko potrzeby przy użyciu aplikacji Xamarin.Mac uprawnień. 

Zasoby piaskownicy aplikacji aplikacji można zmodyfikować, edytując jej **Entitlements.plist** plików i sprawdzanie lub wybierz wymagane z pola listy rozwijanej edytory prawa:

[![Edytowanie uprawnień](sandboxing-images/sign17.png "edycji uprawnień")](sandboxing-images/sign17-large.png)

### <a name="container-directories-and-file-system-access"></a>Kontener katalogów i dostępu do systemu plików

Gdy aplikacja Xamarin.Mac przyjmuje piaskownicy aplikacji, ma dostęp do następujących lokalizacji:

- **Katalog aplikacji kontenera** -przy pierwszym uruchomieniu system operacyjny tworzy specjalnego _katalogu kontenera_ Jeżeli wszystkich jej zasobów, że tylko można uzyskać dostępu. Aplikacja będzie mieć pełny odczytu i zapisu do tego katalogu.
- **Katalogi kontenera grupy aplikacji** -aplikacji może otrzymać dostęp do co najmniej jednej _grupy kontenerów_ udostępnione spośród aplikacji w tej samej grupie.
- **Pliki określone przez użytkownika** — aplikacja automatycznie uzyskuje dostęp do plików, które są jawnie otwarty lub przeciągnięto i Upuszczono na aplikacji przez użytkownika.
- **Powiązane elementy** — z odpowiednich uprawnień aplikacji mogą uzyskiwać dostęp do plików o takiej samej nazwie, ale inne rozszerzenie. Na przykład dokument, który został zapisany jako zarówno `.txt` pliku i `.pdf`.
- **Katalogi tymczasowe, narzędzia wiersza polecenia katalogów i określone lokalizacje odczytu world** — aplikacja ma różnym stopniu dostępu do plików w innych lokalizacjach dobrze zdefiniowany jako określony przez system.

#### <a name="the-app-container-directory"></a>Katalog aplikacji kontenera

Aplikację Xamarin.Mac aplikacji kontenera katalog ma następującą charakterystykę:

- Znajduje się w ukrytym lokalizacji w katalogu głównej użytkownika (zwykle `~Library/Containers`) i jest dostępny z `NSHomeDirectory` — funkcja (patrz poniżej) w aplikacji. Ponieważ jest on katalog macierzysty, każdy użytkownik otrzyma własnych kontener dla aplikacji.
- Aplikacja ma nieograniczony dostęp do odczytu/zapisu do katalogu kontenera i wszystkich jego podkatalogów i plików znajdujących się w.
- Większość interfejsów API ścieżka szukania macOS firmy to względem kontenera aplikacji. Na przykład kontener będzie mieć własny **biblioteki** (dostępne za pośrednictwem `NSLibraryDirectory`), **obsługę aplikacji** i **preferencje** podkatalogów.
- System macOS ustanawia i wymusza połączenie między i aplikacji, jak i jego kontenera za pomocą podpisywania kodu. Nawet jest innej aplikacji próbuje sfałszować aplikacji przy użyciu jego **identyfikator pakietu**, nie będzie możliwość dostępu z powodu podpisywania kodu kontenera.
- Kontener nie jest dla użytkownika wygenerowanych plików. Zamiast tego jest plików, które aplikacja korzysta z takich jak bazy danych, pamięci podręcznych lub innych określonych typów danych.
- Aby uzyskać _shoebox_ typów aplikacje (takie jak aplikacja zdjęcia firmy Apple), zawartość użytkownika zostaną umieszczone w kontenerze.

> [!IMPORTANT]
> **Uwaga:** Niestety Xamarin.Mac nie ma pokrycia interfejsu API 100% jeszcze (w przeciwieństwie do Xamarin.iOS), w związku z tym `NSHomeDirectory` interfejsu API nie został zmapowany w bieżącej wersji Xamarin.Mac.

Jako celu tymczasowego obejścia problemu można użyć poniższego kodu:

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

Począwszy od Mac macOS 10.7.5 (i nowsze), aplikacja może użyć `com.apple.security.application-groups` uprawnienia do dostępu udostępnionym kontenerem, który jest wspólny dla wszystkich aplikacji w grupie. Można użyć tego kontenera udostępnionego niezwiązanych z użytkownikiem połączonej zawartości, takich jak bazy danych lub inne pliki pomocnicze (np. pamięci podręcznych).

Kontenery grupy są automatycznie dodawane do każdej aplikacji piaskownicy kontenera (jeśli są częścią grupy) i są przechowywane w `~/Library/Group Containers/<application-group-id>`. Identyfikator grupy _musi_ zaczynać się od Identyfikatora zespołu programowanie i okres, na przykład:

```plist
<team-id>.com.company.<group-name>
```

Aby uzyskać więcej informacji, zobacz firmy Apple [Dodawanie aplikacji do grupy aplikacji](https://developer.apple.com/library/prerelease/mac/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19) w [uprawnienia z informacjami o kluczach](https://developer.apple.com/library/prerelease/mac/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/AboutEntitlements.html#//apple_ref/doc/uid/TP40011195).

#### <a name="powerbox-and-file-system-access-outside-of-the-app-container"></a>Powerbox system dostępu do plików lub spoza kontenera aplikacji

Piaskownicy aplikacja Xamarin.Mac można uzyskać dostęp do lokalizacji plików systemu poza jego kontenera, w następujący sposób:

- W określonym kierunku użytkownika (za pośrednictwem Open i okna dialogowe Zapisz lub innych metod, takich jak przeciąganie i upuszczanie).
- Przy użyciu uprawnień dla określonego pliku lokalizacji systemu (takich jak `/bin` lub `/usr/lib`).
* Gdy lokalizacji systemu plików jest w niektórych katalogów znajdujących się na świecie do odczytu (na przykład udostępniania).

_Powerbox_ jest macOS technologii zabezpieczeń, który wchodzi w interakcję z użytkownikiem, aby rozwinąć prawa dostępu pliku w trybie piaskownicy aplikacji Xamarin.Mac. Powerbox ma żadnego interfejsu API, ale w sposób niewidoczny dla użytkownika jest aktywowany, gdy wywołuje aplikację `NSOpenPanel` lub `NSSavePanel`. Włączono dostępu Powerbox za pośrednictwem uprawnienia ustawione dla aplikacji Xamarin.Mac.

Gdy piaskownicy aplikacji wyświetla Otwórz lub Zapisz okna dialogowego, okno jest przedstawiony przez Powerbox (i nie AppKit) i w związku z tym ma dostęp do plików i katalogów, które użytkownik ma dostęp do.

Gdy użytkownik wybiera pliku lub katalogu z Otwórz lub Zapisz okna dialogowego (lub przeciąga albo na ikonę aplikacji), Powerbox dodaje skojarzoną ścieżkę do aplikacji piaskownicy.

Ponadto system automatycznie umożliwia następujące do aplikacji w trybie piaskownicy:

- Metody wejściowej połączenia z systemem.
- Wywołanie usługi wybrane przez użytkownika z **usług** menu (tylko dla usługi oznaczonej jako _bezpieczne dla aplikacji piaskownicy_ przez dostawcę usługi).
- Wybierz pliki otwarte przez użytkownika z **Otwórz ostatni** menu.
- Użyj kopiowanie i wklejanie między innymi aplikacjami.
- Odczytywać pliki w następujących lokalizacjach world możliwości odczytu:
    - `/bin`
    - `/sbin`
    - `/usr/bin`
    - `/usr/lib`
    - `/usr/sbin`
    - `/usr/share`
    - `/System`
- Odczyt i zapis plików w katalogach utworzone przez `NSTemporaryDirectory`.

Być domyślnie, pliki otworzyć lub zapisać w trybie piaskownicy aplikacji Xamarin.Mac są nadal dostępne aż do zakończenia aplikacji (chyba, że plik był nadal otwarty, gdy kończy działanie aplikacji). Otwarte pliki automatycznie powróci do piaskownicy aplikacji za pomocą funkcji Wznów macOS przy następnym uruchomieniu aplikacji.

Aby dostarczyć trwałości do plików znajdujących się poza kontenerem aplikacji Xamarin.Mac, użyj zakładki zakres zabezpieczeń (zobacz poniżej).

#### <a name="related-items"></a>Elementy pokrewne

Piaskownicy aplikacji dzięki czemu aplikacja może uzyskać dostępu do elementów pokrewnych, które mają taką samą nazwę, ale inne rozszerzenia. Funkcja ma dwie części:) listę powiązanych rozszerzeń w aplikacji `Info.plst` plik, (b) kod mówić piaskownicy, co aplikacja będzie realizacji tych plików.

Istnieją dwa scenariusze, w którym to sens:

1. Aplikacja musi można zapisywać w innej wersji pliku (z rozszerzeniem nowy). Przykładowo, wyeksportowanie `.txt` pliku `.pdf` pliku. Aby obsłużyć taką sytuację, należy użyć `NSFileCoordinator` dostępu do tego pliku. Będzie wywoływać `WillMove(fromURL, toURL)` metody najpierw przenieść go do nowego rozszerzenia, a następnie wywołać `ItemMoved(fromURL, toURL)`.
2. Aplikacja musi otworzyć główny plik z rozszerzeniem jednego i kilku plików pomocniczych z różnymi rozszerzeniami. Na przykład filmu i plik pomocniczą. Użyj `NSFilePresenter` uzyskanie dostępu do plików pomocniczych. Główny plik, aby zapewnić `PrimaryPresentedItemURL` właściwości i dodatkowej plik do `PresentedItemURL` właściwości. Po otwarciu pliku głównego wywołać `AddFilePresenter` metody `NSFileCoordinator` klasy w celu zarejestrowania dodatkowej pliku.

W obu przypadkach aplikacji w **Info.plist** pliku, muszą deklarować typów dokumentów, które można otworzyć aplikację. Dla dowolnego typu pliku Dodaj `NSIsRelatedItemType` (o wartości `YES`) do wejścia w `CFBundleDocumentTypes` tablicy.

#### <a name="open-and-save-dialog-behavior-with-sandboxed-apps"></a>Otwieranie i zapisywanie zachowanie okna dialogowego z aplikacji w trybie piaskownicy

Następujące ograniczenia są umieszczane na `NSOpenPanel` i `NSSavePanel` podczas wywoływania ich w trybie piaskownicy aplikacji Xamarin.Mac:

- Nie można wywołać programowo **OK** przycisku.
- Nie można programowo zmienić wybór użytkownika w `NSOpenSavePanelDelegate`.

Ponadto następujące modyfikacje dziedziczenia zostały spełnione:

- **Piaskownicy aplikacji** - `NSOpenPanel` `NSSavePanel``NSPanel``NSWindow``NSResponder``NSObject``NSOpenPanel``NSSavePanel``NSObject``NSOpenPanel``NSSavePanel`

### <a name="security-scoped-bookmarks-and-persistent-resource-access"></a>Zakładki zakresu zabezpieczeń i dostęp do zasobów trwałych

Jak już wspomniano aplikacji piaskownicy Xamarin.Mac można dostępu do pliku lub zasobów poza jego kontenera i bezpośredniej interakcji użytkownika (zgodnie z PowerBox). Jednak dostęp do tych zasobów nie jest automatycznie trwały spowoduje uruchomienie aplikacji lub systemu zostanie ponownie uruchomiony.

Za pomocą _zakładki Security-Scoped_, aplikacji piaskownicy Xamarin.Mac można zachować zamiarów użytkownika, a obsługa dostępu do podanego zasobów po aplikację, uruchom ponownie.

#### <a name="security-scoped-bookmark-types"></a>Typy zabezpieczeń, zakres zakładek

Podczas pracy z zakładki Security-Scoped i trwały dostęp do zasobów, istnieją dwie przypadki użycia sistine:

- **Zakładki App-Scoped zapewnia trwały dostęp do określonych przez użytkownika pliku lub folderu.** 

    Na przykład, jeśli w trybie piaskownicy aplikacji Xamarin.Mac pozwala korzystać do otwierania zewnętrznego dokumentu do edycji (przy użyciu `NSOpenPanel`), aplikacji, które można utworzyć zakładki App-Scoped tak, aby go w przyszłości dostępu do tego samego pliku.
- **Zakładka Document-Scoped zawiera określonego dokumentu, trwały dostęp do pliku podrzędnych.** 

Na przykład wideo, edytowania aplikacji, która tworzy plik projektu, który ma dostęp do poszczególnych obrazów, klipów wideo i pliki dźwiękowe, które później zostaną połączone w jeden filmu.

Gdy użytkownik importuje plik zasobów do projektu (za pośrednictwem `NSOpenPanel`), aplikacja tworzy zakładki Document-Scoped dla elementu, który jest przechowywany w projekcie, tak, że plik jest zawsze dostępna dla aplikacji.

Zakładka Document-Scoped można rozwiązać przez dowolną aplikację, który można otworzyć danych zakładek i samego dokumentu. W ten sposób realizowany przenośność, co pozwala na wysyłanie plików projektu do innego użytkownika i o wszystkie zakładki dla nich również działać.

> [!IMPORTANT]
> **Uwaga:** można Document-Scoped Bookman _tylko_ wskaż pojedynczy plik, a nie folder, a ten plik nie może znajdować się w lokalizacji używane przez system (takich jak `/private` lub `/Library`).

#### <a name="using-security-scoped-bookmarks"></a>Korzystanie z zakładek w zakresie zabezpieczeń

Za pomocą dowolnego typu Security-Scoped zakładki, wymaga wykonaj następujące czynności:

1. **Ustawić odpowiednie uprawnienia w aplikacji Xamarin.Mac, która powinna być używana zakładki Security-Scoped** — Ustawianie zakładek For App-Scoped `com.apple.security.files.bookmarks.app-scope` klucz uprawnienie do `true`. Dla zakładek Document-Scoped ustawić `com.apple.security.files.bookmarks.document-scope` klucz uprawnienie do `true`.
2. **Tworzenie zakładek Security-Scoped** -będzie w tym pliku lub folderu, który użytkownik udostępnił dostęp do (za pośrednictwem `NSOpenPanel` na przykład), że aplikacja będzie trwały dostęp do. Użyj `public virtual NSData CreateBookmarkData (NSUrlBookmarkCreationOptions options, string[] resourceValues, NSUrl relativeUrl, out NSError error)` metody `NSUrl` klasy w celu utworzenia zakładki.
3. **Rozwiązanie działania zakładki Security-Scoped** — Jeśli aplikacja potrzebuje dostępu do zasobu ponownie (na przykład po ponownym uruchomieniu) należy rozwiązać zakładki z adresem URL zakresu zabezpieczeń. Użyj `public static NSUrl FromBookmarkData (NSData data, NSUrlBookmarkResolutionOptions options, NSUrl relativeToUrl, out bool isStale, out NSError error)` metody `NSUrl` klasy do rozpoznania zakładki.
4. **Jawnie powiadomić systemu, który ma uzyskać dostęp do pliku w adresie URL Security-Scoped** — ten krok należy jednak wykonać bezpośrednio po uzyskiwania adresu URL Security-Scoped powyżej lub, jeśli chcesz później odzyskać dostęp do zasobu po zwalniana, dostęp do niego. Wywołanie `StartAccessingSecurityScopedResource ()` metody `NSUrl` klasy, aby rozpocząć, uzyskiwanie dostępu do adresu URL Security-Scoped.
5. **Jawnie powiadamiania systemu, które są wykonywane podczas uzyskiwania dostępu do pliku w adresie URL Security-Scoped** — tak szybko, jak to możliwe, należy poinformować System gdy aplikacja nie będzie już potrzebował dostępu do pliku (na przykład, jeśli użytkownik zamyka). Wywołanie `StopAccessingSecurityScopedResource ()` metody `NSUrl` klasy, aby zatrzymać, uzyskiwanie dostępu do adresu URL Security-Scoped.

Po zrzeka się dostęp do zasobu, musisz wróć do kroku 4 ponownie, aby ponownie ustanowić dostępu. Po uruchomieniu aplikacji Xamarin.Mac musisz powrócić do kroku 3 i ponownie rozwiązanie działania zakładki.

> [!IMPORTANT]
> **Uwaga:** niepowodzenie zwolnienia dostęp do zasobów Security-Scoped URL spowoduje, że to aplikacja Xamarin.Mac zostały publicznie zasobów jądra. W związku z tym aplikacji już nie będą mogli dodawać lokalizacje w systemie plików do jego kontenera, dopóki nie zostanie uruchomiona ponownie.

### <a name="the-app-sandbox-and-code-signing"></a>Piaskownica aplikacji i podpisywanie kodu

Po włączyć piaskownicy aplikacji i Włącz określone wymagania aplikacji Xamarin.Mac (za pośrednictwem uprawnień) musi kodu znaku projektu dla sandboxing zaczęły obowiązywać. Należy wykonać kod podpisywania, ponieważ uprawnienia wymagane dla aplikacji Sandboxing są połączone z podpis aplikacji. 

System macOS wymusza łącza między kontenera aplikacji i jej podpisania kodu w ten sposób żadna inna aplikacja nie może uzyskać dostępu do tego kontenera, nawet wtedy, gdy jest fałszowanie aplikacje identyfikator pakietu. Ten mechanizm działa w następujący sposób:

1. Gdy System tworzy kontener aplikacji, ustawia _listy kontroli dostępu_ (ACL) w tym kontenerze. Wpisu kontroli dostępu początkowej liście zawiera aplikację _wyznaczony wymaganie_ (DR), który opisuje sposób przyszłych wersji aplikacji mogą być rozpoznawane (jeśli został uaktualniony).
2. Każdym uruchomieniu aplikacji o tym samym identyfikatorze pakietu, system sprawdza zgodność podpisu kodu aplikacji wymienionych na wymagania określone w jeden z wpisów w ACL kontenera. Jeśli w systemie nie znaleziono dopasowania, uniemożliwia aplikacji uruchamiania.

Kod podpisywania działa w następujący sposób:

1. Przed utworzeniem projektu Xamarin.Mac, należy uzyskać certyfikatu deweloperskiego, certyfikat dystrybucji i certyfikat Identyfikator dewelopera z portalu dla deweloperów firmy Apple.
2. Mac App Store dystrybuuje aplikacji Xamarin.Mac, były podpisane podpisem kod firmy Apple.

Podczas testowania i debugowania, należy używać wersji aplikacji Xamarin.Mac podpisane (która będzie służyć do tworzenia kontenera aplikacji). Później Jeśli chcesz przetestować lub zainstaluj wersję ze sklepu z aplikacjami firmy Apple, jego zostaną podpisane podpisem firmy Apple i nie będzie można uruchomić (ponieważ nie ma takiego samego podpisu kodu jak oryginalny kontenera aplikacji). W takim przypadku otrzymasz raport o awarii podobny do następującego:

```csharp
Exception Type:  EXC_BAD_INSTRUCTION (SIGILL)
```

Aby rozwiązać ten problem, należy dostosować wpisu listy ACL wskaż Apple podpisanej wersji aplikacji.

Aby uzyskać więcej informacji na temat tworzenia i pobieranie profile inicjowania obsługi wymaganych do Sandboxing, zobacz [podpisywania i Inicjowanie obsługi aplikacji](#Signing_and_Provisioning_the_App) powyższej sekcji.

#### <a name="adjusting-the-acl-entry"></a>Dopasowywanie wpisu na liście ACL

Aby umożliwić Apple podpisanej wersji aplikacji Xamarin.Mac do wykonania, wykonaj następujące czynności:

1. Otwórz aplikację terminali (w `/Applications/Utilities`).
2. Otwórz okno wyszukiwania do wersji podpisem firmy Apple Xamarin.Mac aplikacji.
3. Typ `asctl container acl add -file ` w oknie terminalu.
4. Przeciągnij ikonę aplikacji Xamarin.Mac z okna wyszukiwania i upuść ją na okno terminalu.
5. Pełna ścieżka do pliku zostanie dodany do polecenia w terminalu.
6. Naciśnij klawisz **Enter** można wykonać polecenia.

Listy ACL kontenera zawiera teraz wymagania kodu wyznaczonych obie wersje aplikacji Xamarin.Mac i macOS teraz umożliwi danej wersji uruchomić.

#### <a name="display-a-list-of-acl-code-requirements"></a>Wyświetl listę ACL wymagania kodu

Lista wymagań kodu można wyświetlić w ACL kontenera, wykonując następujące czynności:

1. Otwórz aplikację terminali (w `/Applications/Utilities`).
2. Typ `asctl container acl list -bundle <container-name>`.
3. Naciśnij klawisz **Enter** można wykonać polecenia.

`<container-name>` Zazwyczaj jest to identyfikator pakietu aplikacji Xamarin.Mac.

## <a name="designing-a-xamarinmac-app-for-the-app-sandbox"></a>Projektowanie aplikacji Xamarin.Mac piaskownicy aplikacji

Brak wspólnego przepływu pracy, który należy wykonać podczas projektowania aplikacji Xamarin.Mac piaskownicy aplikacji. Inaczej mówiąc, mają być unikatowy w danej aplikacji funkcji szczegółowe informacje na temat wdrażania sandboxing w aplikacji.

### <a name="six-steps-for-adopting-the-app-sandbox"></a>Sześć kroków do przyjęcia piaskownicy aplikacji

Projektowanie aplikacji Xamarin.Mac piaskownicy aplikacji zwykle obejmuje następujące kroki:

1. Ustal, czy aplikacja jest odpowiedni dla sandboxing.
2. Projektowania strategii projektowania i dystrybucji.
3. Rozwiąż wszelkie niezgodności interfejsu API.
4. Zastosuj wymaganych uprawnień piaskownicy dla aplikacji do projektu Xamarin.Mac.
5. Dodawanie przy użyciu XPC separacji uprawnień.
6. Wdrożenie strategii migracji.

> [!IMPORTANT]
> **Uwaga:** należy nie tylko piaskownicy głównego pliku wykonywalnego w pakietu aplikacji, ale także na dołączone pomocnika aplikacji lub narzędzia w tym pakiecie. Jest to wymagane dla wszystkich aplikacji rozproszonej ze sklepu Mac App Store i, jeśli to możliwe, należy wykonać dla jakąkolwiek inną formę dystrybucji aplikacji.

Aby uzyskać listę wszystkich wykonywalnego plików binarnych w pakiecie aplikacji Xamarin.Mac wpisz następujące polecenie w terminalu:

```bash
find -H [Your-App-Bundle].app -print0 | xargs -0 file | grep "Mach-O .*executable"
```

Gdzie `[Your-App-Bundle]` jest nazwa i ścieżka do pakietu aplikacji.

### <a name="determine-whether-a-xamarinmac-app-is-suitable-for-sandboxing"></a>Ustalić, czy aplikacja Xamarin.Mac jest odpowiedni dla sandboxing

Większość aplikacji Xamarin.Mac są w pełni zgodne z piaskownicy aplikacji i w związku z tym odpowiednie dla sandboxing. Jeśli aplikacja wymaga zachowania, które nie zezwala na piaskownicy aplikacji, należy rozważyć o innym podejściu.

Jeśli aplikacja wymaga jednej z poniższych zachowań, jest niezgodny z piaskownicy aplikacji:

- **Usługi autoryzacji** -o do piaskownicy aplikacji, nie można użyć funkcji opisanych w [dokumentację C usług autoryzacji](https://developer.apple.com/library/prerelease/mac/documentation/Security/Reference/authorization_ref/index.html#//apple_ref/doc/uid/TP30000826).
- **Ułatwienia dostępu interfejsów API** — nie można wykonać piaskownicy aplikacji ułatwień dostępu, takich jak czytniki lub aplikacje, które kontrolują inne aplikacje.
- **Wysyłanie zdarzeń firmy Apple do dowolnych aplikacji** — Jeśli aplikacja wymaga wysyłania zdarzeń firmy Apple do nieznanego, dowolnych aplikacji, nie może być piaskownicy. Listę znanych aplikacji o nazwie nadal aplikacji może być w trybie piaskownicy oraz uprawnień należy włączyć na liście aplikacji o nazwie.
- **Wyślij słowników informacje użytkownika w powiadomieniach dystrybuowane do wykonywania innych zadań** — z do piaskownicy aplikacji, nie może zawierać `userInfo` słownika zaksięgowania w `NSDistributedNotificationCenter` obiektu do obsługi wiadomości innych zadań.
- **Ładowanie rozszerzenia jądra** -ładowanie rozszerzeń jądra jest zabronione przez piaskownicy aplikacji.
- **Symuluje wejściowego użytkownika w otwieranie i wyświetlanie okien dialogowych zapisywanie** — programowo manipulowanie Otwórz lub Zapisz oknach dialogowych, aby symulować lub zmieniać dane wejściowe użytkownika jest zabronione przez piaskownicę aplikacji.
- **Dostęp do lub Preferencje ustawienia w innych aplikacjach** -manipulowanie ustawienia innych aplikacji jest zabronione przez piaskownicy aplikacji.
- **Konfigurowanie ustawień sieci** -manipulowanie ustawień sieci jest zabronione przez piaskownicy aplikacji.
- **Trwa przerywanie wykonywania innych aplikacji** -zabrania do piaskownicy aplikacji przy użyciu `NSRunningApplication` zakończyć inne aplikacje.

### <a name="resolving-api-incompatibilities"></a>Rozpoznawanie niezgodności interfejsu API

Podczas projektowania aplikacji Xamarin.Mac piaskownicy aplikacji, może wystąpić niezgodności z użycie niektórych macOS interfejsów API.

Poniżej przedstawiono kilka typowych problemów i rzeczy, które można wykonać w celu rozwiązania tych problemów:

- **Otwieranie, zapisywanie i śledzenia dokumentów** — Jeśli zarządzasz dokumentów za pomocą innych technologii innych niż `NSDocument`, należy przełączyć się do niego dzięki wbudowanej obsłudze aplikacji piaskownicy. `NSDocument` automatycznie współpracuje z PowerBox i przechowywanie dokumentów w ramach Twojej piaskownicy, jeśli użytkownik przesuwa je w wyszukiwarce zapewnia obsługę.
- **Zachować dostęp do zasobów systemu plików** — Jeśli Xamarin.Mac trwały dostęp do zasobów poza jego kontenera zależy od aplikacji umożliwia Obsługa dostępu zakładek w zakresie zabezpieczeń.
- **Tworzenie elementu logowania dla aplikacji** -o do piaskownicy aplikacji, nie można utworzyć elementu za pomocą nazwy logowania `LSSharedFileList` ani modyfikowania stanu uruchamiania usługi przy użyciu `LSRegisterURL`. Użyj `SMLoginItemSetEnabled` działać zgodnie z opisem w jabłek [Dodawanie elementów logowania przy użyciu platformy zarządzania usługi](https://developer.apple.com/library/prerelease/mac/documentation/MacOSX/Conceptual/BPSystemStartup/Chapters/CreatingLoginItems.html#//apple_ref/doc/uid/10000172i-SW5-SW1) dokumentacji.
- **Uzyskiwanie dostępu do danych użytkownika** — Jeśli używasz POSIX funkcje takie jak `getpwuid` można uzyskać katalogu macierzystego użytkownika z usług katalogowych, należy wziąć pod uwagę przy użyciu symboli Cocoa lub Core Foundation, takich jak `NSHomeDirectory`.
- **Dostęp do preferencji inne aplikacje** — ponieważ piaskownicy aplikacji kieruje ścieżka szukania interfejsów API do aplikacji kontenera, modyfikacja preferencje ma miejsce, w tym kontenerze oraz dostęp do innego preferencji aplikacje w niedozwolone. 
- **W widoku sieci Web przy użyciu osadzonego wideo HTML5** — Jeśli aplikacja Xamarin.Mac używa WebKit do odtwarzania wideo HTML5 osadzone, możesz połączyć aplikacji framework AV Foundation. Piaskownica aplikacji uniemożliwi CoreMedia play tych klipów wideo, w przeciwnym razie.

### <a name="applying-required-app-sandbox-entitlements"></a>Stosowanie wymaganych uprawnień piaskownicy aplikacji

Należy edytować uprawnień dla dowolnej aplikacji Xamarin.Mac, który ma zostać uruchomiony w piaskownicy aplikacji i sprawdź **włączyć Sandboxing aplikacji** wyboru.

Oparte na funkcji aplikacji, może być konieczne włączenie innych uprawnień dostępu do funkcji systemu operacyjnego lub zasobów. Aplikacja Sandboxing działa najlepiej, gdy zminimalizować uprawnień, które żądanie bez systemu operacyjnego wymaganego do uruchomienia aplikacji sieci tylko losowo Włącz tego uprawnienia.

Aby ustalić, które uprawnienia aplikacji Xamarin.Mac wymaga, wykonaj następujące czynności:

1. Włącz piaskownicy aplikacji i uruchom aplikację Xamarin.Mac.
2. Uruchom przy użyciu funkcji aplikacji.
3. Otwórz aplikację konsoli (dostępne w `/Applications/Utilities`) i poszukaj `sandboxd` naruszeń w **wszystkie komunikaty** dziennika.
4. Dla każdego `sandboxd` naruszenie, rozwiąż problem przy użyciu kontenera aplikacji zamiast inne lokalizacje w systemie plików lub Zastosuj aplikacji piaskownicy uprawnienia umożliwiające dostęp do ograniczonych funkcji systemu operacyjnego.
5. Uruchom ponownie, a następnie ponownie przetestuj wszystkich funkcji aplikacji Xamarin.Mac.
6. Powtórz, aż wszystkie `sandboxd` naruszeń zostały rozwiązane.

### <a name="add-privilege-separation-using-xpc"></a>Dodaj uprawnienie separacja przy użyciu XPC

Podczas opracowywania aplikacji Xamarin.Mac piaskownicy aplikacji, obejrzyj zachowania aplikacji w kategoriach uprawnień i dostępu, a następnie należy wziąć pod uwagę oddzielanie o wysokim ryzyku operacji do ich własnych XPC usług.

Aby uzyskać więcej informacji, zobacz firmy Apple [tworzenie usług XPC](https://developer.apple.com/library/prerelease/mac/documentation/MacOSX/Conceptual/BPSystemStartup/Chapters/CreatingXPCServices.html#//apple_ref/doc/uid/10000172i-SW6) i [przewodnik programowania w języku usług i demonów](https://developer.apple.com/library/prerelease/mac/documentation/MacOSX/Conceptual/BPSystemStartup/Chapters/Introduction.html#//apple_ref/doc/uid/10000172i).

### <a name="implement-a-migration-strategy"></a>Wdrożenie strategii migracji

Są zwalnianie nowej piaskownicy wersji aplikacji Xamarin.Mac, który nie był wcześniej w trybie piaskownicy, należy zapewnić użytkownikom bieżącego łatwość uaktualnień. 

 Aby uzyskać szczegółowe informacje na temat wdrożenia kontenera manifestu migracji, przeczytaj firmy Apple [migrowanie aplikacji do piaskownicy](https://developer.apple.com/library/prerelease/mac/documentation/Security/Conceptual/AppSandboxDesignGuide/MigratingALegacyApp/MigratingAnAppToASandbox.html#//apple_ref/doc/uid/TP40011183-CH6-SW1) dokumentacji.

## <a name="summary"></a>Podsumowanie

W tym artykule trwało szczegółowy widok sandboxing aplikacji Xamarin.Mac. Po pierwsze utworzyliśmy po prostu Xamarin.Mac aplikacji, aby wyświetlić podstawowe informacje o aplikacji piaskownicy. Następnie możemy pokazano, jak rozwiązać naruszeń piaskownicy. Następnie Wybraliśmy szczegółowe Przyjrzyj się piaskownicy aplikacji, a na koniec analizujemy projektowania aplikacji Xamarin.Mac piaskownicy aplikacji.



## <a name="related-links"></a>Linki pokrewne

- [Publikowanie w sklepie App Store](~/mac/deploy-test/publishing-to-the-app-store/index.md)
- [Temat aplikacji piaskownicy](https://developer.apple.com/library/content/documentation/Security/Conceptual/AppSandboxDesignGuide/AboutAppSandbox/AboutAppSandbox.html)
- [Typowe problemy Sandboxing aplikacji](https://developer.apple.com/library/content/qa/qa1773/_index.html)
