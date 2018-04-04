---
title: Konfigurowanie aplikacji w iTunes Connect
description: W tym artykule opisano kroki wymagane do instalacji i obsługa aplikacji platformy Xamarin.iOS w iTunes Connect, dzięki czemu może być zwolnione dystrybucji ze sklepu App Store.
ms.prod: xamarin
ms.assetid: 74587317-4b15-4904-9582-dcd914827cbc
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: b54313668a2cb87a6cce0b8c519a06247524df81
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="configuring-an-app-in-itunes-connect"></a>Konfigurowanie aplikacji w iTunes Connect

_W tym artykule opisano kroki wymagane do instalacji i obsługa aplikacji platformy Xamarin.iOS w iTunes Connect, dzięki czemu może być zwolnione dystrybucji ze sklepu App Store._

iTunes Connect to zestaw narzędzi opartych na sieci web, między innymi, zarządzanie aplikacjami systemu iOS ze sklepu App Store. Aplikacji platformy Xamarin.iOS należy prawidłowo skonfigurować i skonfigurowane w iTunes Connect przed może być przesyłany do firmy Apple w celu przeglądu i ostatecznie wydane jako bezpłatną aplikację w sklepie z aplikacjami lub sprzedaży.

Połącz iTunes mogą być używane dla następujących elementów:

- Ustaw nazwę aplikacji (wyświetlane w sklepie z aplikacjami).
- Podaj zrzuty ekranu lub wideo aplikację w akcji na obsługiwanych urządzeniach z systemem iOS.
- Podaj Wyczyść, zwięzły opis aplikacji w tym jego funkcje i korzyści dla użytkownika końcowego.
- Podaj kategorie i podkategorie pomóc użytkownikowi znajdowanie aplikacji w sklepie z aplikacjami.
- Podaj wszystkie — słowo kluczowe dalsze ułatwiają znajdowanie aplikacji użytkownika.
- Podaj adresy URL pomocy technicznej i skontaktuj się z tej witryny sieci Web (wymagane przez firmę Apple).
- Ustawianie klasyfikacji aplikacji, który służy do informowania kontroli rodzicielskiej ze sklepu App Store.
- Wybierz cena sprzedaży lub określ bezpłatnie wydane aplikacji.
- Skonfiguruj opcjonalne technologii sklepu z aplikacjami, takich jak Game Center i zakupu w aplikacji.

Ponadto aplikacja powinny mieć atrakcyjne, wysokiej rozdzielczości kompozycji dostępne w przypadku postanawia funkcji, w sklepie z aplikacjami firmy Apple. Aby uzyskać więcej informacji, zobacz firmy Apple [iTunes przewodnik dewelopera Connect](https://developer.apple.com/support/itunes-connect/).

## <a name="managing-agreements-tax-and-banking"></a>Zarządzanie umów podatku i bankowości

**Umów podatku i bankowości** sekcji iTunes połączenia jest używana, podaj wymagane informacje finansowe odnoszące się do płatności dewelopera programu iTunes i podatku withholdings i śledzić stan umowy w miejscu z Firmy Apple. Aby można było wydać aplikację systemu iOS ze sklepu App Store (bezpłatnie lub do sprzedaży), musisz zostały spełnione odpowiednie umowy i zgodzili się na wszelkie modyfikacje istniejące umowy.

[![](itunesconnect-images/agreement01.png "Zarządzanie umów podatku i bankowości")](itunesconnect-images/agreement01.png#lightbox)

W tym miejscu można wykonywać następujące czynności:

- Podaj **agenta Team** i zdefiniuj innych ról użytkownika dla Twojego konta Connect iTunes, takich jak **Admin** lub **Finance**.
- Zarejestruj się w i obsługa **kontrakty** umożliwiające organizacji, aby dystrybuować aplikacje w sklepie z aplikacjami.
- Określ **firmy** nazwę (sprzedawcy w sklepie z aplikacjami) używany do odpowiada kontrakty banków informacji i informacjami, które są skojarzone z daną organizacją.
- Podaj **banków** i **podatku** informacji, jeśli zamierzasz sprzedawać aplikacji za pośrednictwem sklepu z aplikacjami.

Ponownie, te informacje _musi_ jest poprawnie ustawiona, up, w miejscu i aktualizować oprogramowanie przed iOS aplikacji może zostać przesłane do programu iTunes Connect do przeglądu i wersji. Aby uzyskać więcej informacji, zobacz firmy Apple [Zarządzanie umowy, podatku i bankowości](https://developer.apple.com/library/ios/documentation/LanguagesUtilities/Conceptual/iTunesConnect_Guide/Chapters/ManagingContractsandBanking.html#//apple_ref/doc/uid/TP40011225-CH21-SW1) dokumentacji.

<a name="creating" />

## <a name="creating-an-itunes-connect-record"></a>Tworzenie rekordu Connect iTunes

Przed Xamarin.iOS, które mogą być przekazywane aplikacji iTunes Connect do dystrybucji za pośrednictwem sklepu z aplikacjami należy utworzyć rekord dla aplikacji w iTunes Connect. Ten rekord zawiera wszystkie informacje o aplikacji, pojawi się w sklepie z aplikacjami (w dowolną liczbę języków zgodnie z wymaganiami), a wszystkie informacje wymagane do zarządzania aplikacją proces dystrybucji. Ponadto będzie używany do konfigurowania technologii sklepu z aplikacjami, takich jak iAd sieci aplikacji lub Centrum gier.

Aby dodać aplikację systemu iOS do iTunes Connect, musisz być **agenta Team** lub użytkownika z **Admin** lub **techniczne** roli.

Wykonaj następujące czynności [iTunes Connect](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa):

1. Polecenie **Moje aplikacje**:

    [![](itunesconnect-images/add01.png "Polecenie Moje aplikacje")](itunesconnect-images/add01.png#lightbox)
2. Kliknij przycisk **+** w prawym górnym po lewej stronie rogu i wybierz pozycję **nowych aplikacji dla systemu iOS**:

    [![](itunesconnect-images/add02.png "Dodawanie nowej aplikacji systemu iOS")](itunesconnect-images/add02.png#lightbox)
3. iTunes wyświetli Connect **nowych aplikacji dla systemu iOS** okna dialogowego:

    [![](itunesconnect-images/add03.png "Okno dialogowe nowego aplikacji systemu iOS")](itunesconnect-images/add03.png#lightbox)
4. Wprowadź **nazwa** i **numer wersji** dla aplikacji, jak powinny być wyświetlane w sklepie z aplikacjami.
5. Wybierz **podstawowy język**.
6. Wprowadź **SKU** numer, to jest unikatowy, stała, identyfikator, który ma być używane ścieżki aplikacji. Nie można wyświetlić użytkownikowi końcowemu i jego _nie_ można zmienić po utworzeniu aplikacji.
7. Wybierz **identyfikator pakietu** dla aplikacji, podczas przydzielania aplikacji utworzony w Centrum deweloperów. Ten sam identyfikator pakietu musi być używany do podpisywania pakietu dystrybucji w programie Visual Studio dla komputerów Mac lub Visual Studio. Aby uzyskać więcej informacji, zobacz nasze [tworzenia profilu dystrybucji](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#provisioningprofile) i [wybór profilu dystrybucji w projekcie platformy Xamarin.iOS](~/ios/get-started/installation/device-provisioning/index.md) dokumentacji.
8. Kliknij przycisk **Utwórz** przycisku do tworzenia nowych iTunes Connect rekordu dla aplikacji.

Nowa aplikacja zostanie utworzony w iTunes Connect i będzie gotowy do wprowadź wymagane informacje opis cennik, kategorii, klasyfikacje, np.:

[![](itunesconnect-images/add04.png "Nowa aplikacja zostanie utworzona w iTunes Connect")](itunesconnect-images/add04.png#lightbox)

<a name="managing" />

## <a name="managing-app-videos-and-screenshots"></a>Zarządzanie wideo aplikacji i zrzuty ekranu

Jednym z najważniejszych elementów do pomyślnie marketingowej aplikacji systemu iOS w sklepie z aplikacjami to doskonały zestaw zrzuty ekranu i, opcjonalnie, Podgląd wideo. Użyj rzeczywistego widoków aplikacji uruchamiane z zaznacz interakcji z użytkownikiem i pokazują unikatowe funkcje tego programu. Umożliwia aplikacji w wersji zapoznawczej wideo zapewniają użytkownikom informacje o tym, co to jest przykład korzystanie z aplikacji.

Apple sugeruje następujące podejmując zrzuty ekranu:

- Optymalizacja zrzut ekranu najlepsze prezentacji na urządzeniach z systemem iOS obsługiwane przez aplikację i upewnij się, że zawartość jest czytelna.
- Nie ramki zrzut ekranu obrazu urządzenia z systemem iOS.
- Pokaż widok rzeczywistych aplikacji, w trybie pełnoekranowym, bez grafiki lub obramowania wokół zrzut ekranu.
- Zawsze usunąć pasek stanu ze zrzutami ekranu, iTunes Connect oczekuje zrzuty ekranu wymiarów, które są wykluczone z tego obszaru.
- Jeśli to możliwe, zrzutów ekranu na prawdziwe, wysokiej rozdzielczości sprzętu z systemem iOS siatkówki (nie w symulatorze systemu iOS).
- Pierwszy zrzut ekranu pojawi się w wynikach wyszukiwania w sklepie z aplikacjami na telefonie iPhone i iPad Jeśli brak obrazu podglądu aplikacji jest niedostępny, więc umieszczenie najlepsze zrzut ekranu najpierw.
- Umożliwia przedstawienie historii aplikacji podczas wyróżniania momenty, które ułatwiają atrakcyjnej wszystkie pięć zrzutów ekranu.

Firma Apple wymaga zrzuty ekranu i filmy wideo należy podać na każdym rozmiaru ekranu i rozwiązania, który obsługuje aplikacja. Ponadto pionowej i poziomej wersji powinny zostać spełnione, oparte na orientacje obsługiwane.

Obecnie wymagane są następujące rozmiaru ekranu i rozwiązania:

[!include[](~/ios/includes/table-itunesconnect.md)]

### <a name="editing-screenshots-in-itunes-connect"></a>Edytowanie zrzuty ekranu w iTunes Connect

Wykonaj następujące czynności [iTunes Connect](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa):

1. Polecenie **Moje aplikacje**.
2. Kliknięcie aplikacji **ikona**.
3. Wybierz **wersji** kartę.
4. Przewiń do **zrzuty ekranu** sekcji.
5. Wybierz **rozmiar obrazu** i przeciągnij w wymaganych obrazów (do 5 według rozmiaru ekranu):

    [![](itunesconnect-images/screenshot01.png "Wybierz rozmiar obrazu i przeciągnij w wymaganych obrazów")](itunesconnect-images/screenshot01.png#lightbox)
6. Powtórz dla wszystkich rozmiarów ekranu wymagane.
7. Kliknij przycisk **zapisać** przycisk w górnej części ekranu, aby zapisać zmiany.

> [!NOTE]
> Uwaga: Apple spowoduje odrzucenie przesłane, jeśli zrzuty ekranu lub aplikacji wyświetlanie podglądu wideo nie zgadzają się bieżącą funkcjonalność w aplikacji.

<a name="metadata" />

## <a name="managing-name-description-whats-new-keywords-and-urls"></a>Zarządzanie nazwę, opis nowości, adresy URL i słowa kluczowe

Ta sekcja iTunes połączyć rekord aplikacji zawiera zlokalizowane informacje o aplikacji, jakie operacje, wszelkie modyfikacje nowe wersje, słowa kluczowe używane w wyszukiwania i obsługa iAd oraz wszelkie obsługi adresów URL.

### <a name="app-name"></a>Nazwa aplikacji

Wybierz nazwę opisową aplikacji, która odzwierciedla działania aplikacji. Spróbuj zachować go jako krótko- i powinien być możliwie krótki. Nazwa aplikacji odgrywa kluczową rolę w sposób użytkownikom wyszukiwanie i odnalezienia, więc nazwa proste i łatwa do zapamiętania. Należy zwrócić szczególną uwagę na sposób nazwa jest wyświetlana podczas przeglądania na urządzeniu z systemem iOS (iPad, iPhone i iPod touch).

Apple sugeruje następujące wytyczne, wybierając Nazwa aplikacji:

- Powinna być krótka, proste i łatwa do zapamiętania.
- Upewnij się, że nie narusza, prawa autorskie lub znaki towarowe 3 strony.
- Być zgodne funkcjonalność aplikacji.
- Podaj zlokalizowanych nazw dla obcego rynkach odpowiednim.

### <a name="description"></a>Opis

Zapis wyczyść zwięzły i szczegółowy opis aplikacji i ich funkcje. Kilka pierwszych wierszy są najważniejsze i praktycznych dużą wrażenie pierwszy i rysowanie użytkownika w. Opisano, co sprawia, że aplikacja specjalne i oddziela go od innych, podobne aplikacji.

Apple sugeruje następujące do pisania opis aplikacji:

- Obejmują akapitu krótki otwierania lub dwóch i krótki listy punktowane najważniejszych funkcji.
- Zawierają opisy zlokalizowanych obcego rynkach, gdzie jest to odpowiednie.
- To recenzje użytkownika, wyróżnienie lub opinie tylko na końcu, jeśli w ogóle.
- W celu zwiększenia czytelności, należy użyć podziały wiersza i punktorów.
- Należy pamiętać o jak Wyświetla opisem aplikacji w sklepie z aplikacjami dla każdego typu urządzenia, aby upewnić się, że najważniejszych zdań w opisie są łatwo widoczne.

### <a name="whats-new"></a>Nowości

Podczas przekazywania nowej wersji aplikacji, **nowości w tej wersji** pole jest dostępne powinna zostać ukończona dokładnie i odnoszącej.

Apple sugeruje poniższe wskazówki podczas wypełniania co to jest nowe informacje:

- Dodaj wiadomości zachęcić pracowników do aktualizacji.
- Listę elementów według ważności i wskazanie zmiany i poprawki.
- Przedstawia zmiany w języku zwykłe i autentyczne zamiast żargonu technicznych.

### <a name="keywords"></a>Słowa kluczowe

Przemyślane i strategicznych słów kluczowych dotyczących funkcje aplikacji ułatwiają użytkownikom łatwo zlokalizować aplikacji w sklepie z aplikacjami. Ponadto jeśli aplikacja obsługuje zapasowej iAd reklam, iAd aplikacji sieci używa słowa kluczowe w przypadku wybrania reklam do obiektów docelowych w aplikacji.

W przypadku wybrania słowa kluczowe, Apple sugeruje następujące:

- Nie używaj konkurencyjnych nazwy aplikacji, nazwy firmy lub produktu lub nazwy chronionym znakiem towarowym.
- Nie należy używać słowa ogólnego lub warunków.
- Unikaj warunki nieodpowiednie lub niepożądanych lub słowa nie ma znaczenia, takich jak nazwy renomy.
- Lokalizowanie słowa kluczowe dla obcego rynkach, gdy jest to konieczne.

### <a name="urls"></a>adresy URL

Firma Apple wymaga, że deweloper podaj łącze do witryny sieci Web do obsługi dowolnej problem lub pytania, na które użytkownik może mieć o aplikacji. Wymagają one również link do zasady ochrony prywatności aplikacji (które ponownie, musi znajdować się w witrynie sieci Web).

Opcjonalnie możesz podać link do marketing informacji hostowanych w witrynie sieci Web, który może służyć do zapewnienia więcej informacji o aplikacji, niż jest dostępne w sklepie z aplikacjami.

### <a name="editing-name-description-whats-new-keywords-and-urls-in-itunes-connect"></a>Edytowanie nazwy, opisu, co to jest nowy, słowa kluczowe i adresy URL w iTunes Connect

Wykonaj następujące czynności [iTunes Connect](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa):

1. Polecenie **Moje aplikacje**.
2. Kliknięcie aplikacji **ikona**.
3. Wybierz **wersji** kartę.
4. Przewiń do **nazwa** sekcji.
5. Wypełnij wszystkie wymagane informacje:

    [![](itunesconnect-images/name01.png "Edytowanie nazwy, opisu, co to jest nowy, słowa kluczowe i adresy URL iTunes Connect")](itunesconnect-images/name01.png#lightbox)
6. Kliknij przycisk **zapisać** przycisk w górnej części ekranu, aby zapisać zmiany.

> [!IMPORTANT]
> Uwaga: Apple spowoduje odrzucenie przesłane, jeśli nazwa, opis, co to jest nowy, słowa kluczowe lub adresy URL nie zgadzają się bieżącą funkcjonalność w aplikacji.

<a name="general" />

## <a name="maintaining-general-app-information"></a>Obsługa informacji o aplikacji ogólne

Ta część programu iTunes, z którą połączyć rekord aplikacji zawiera unikatowy identyfikator aplikacji (jak przypisane przez firmę Apple), aplikacja należy do kategorii, klasyfikacji, copyright i informacje o firmie zwolnienie aplikację.

### <a name="app-icon"></a>Ikona aplikacji

> [!IMPORTANT]
>  Ikony aplikacji nie są przesyłane za pośrednictwem usługi Connect iTunes. Musi być przesłane za pośrednictwem **AppIcon** obrazu zestawu do projektu **Assets.xcassets** pliku. Aby uzyskać więcej informacji, zobacz [ikonę aplikacji sklepu](~/ios/app-fundamentals/images-icons/app-store-icon.md) przewodnik.

Ikona aplikacji jest kroju aplikacji do użytkowników, więc należy ją do zapamiętania i wyświetlania dobrze przy mały rozmiar. Ikony do zapamiętania są czyste, proste i natychmiast rozpoznawalnych.

Apple sugeruje poniższe wskazówki podczas projektowania jako ikony aplikacji:

- Ikona odpowiedni dla twojej aplikacji.
- Tworzenie prostego ikonę, która jest zgodna z projektu aplikacji.
- Unikaj używania słowa w Twojej ikony.
- Wziąć pod uwagę globalnie: ikona jednej aplikacji jest używany w wszystkie obszary magazynu.

Obraz pikseli 1024 x 1024 jest wymagany dla ikony aplikacji, który będzie wyświetlany w sklepie z aplikacjami.

Aby uzyskać więcej informacji, zobacz firmy Apple [iOS Human Interface Guidelines](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/MobileHIG/index.html#//apple_ref/doc/uid/TP40006556) i opis dużych ikon aplikacji części [ogólne informacje o aplikacji](https://developer.apple.com/library/ios/documentation/LanguagesUtilities/Conceptual/iTunesConnect_Guide/Appendices/Properties.html#//apple_ref/doc/uid/TP40011225-CH26-SW7) dokumentacji

### <a name="app-id"></a>Identyfikator aplikacji

Jest to unikatowy numer identyfikacyjny przypisany do aplikacji przez firmę Apple, gdy iTunes zostaje utworzony rekord połączenia. Podczas wywoływania tej Apple kilka interfejsów opartych na sieci web udostępnia, w tym informacji o magazynie aplikacji w witrynie sieci Web, można użyć tego numeru.

### <a name="version-number"></a>Numer wersji

Jest to bieżącej wersji aktywnej aplikacji, jak użytkownikowi w sklepie z aplikacjami.

### <a name="category-and-sub-category"></a>Kategorii i podkategorii

Jeden ważnym aspektem skutecznego odnajdowania aplikacji jest kategorię, która jest wyświetlana w ze sklepu App Store. Kategorie umożliwiają użytkownikom przeglądania zbiór aplikacji i znajdowanie te, które są zainteresowani. iTunes Connect można przypisać do dwóch różnych kategoriach, definiując aplikacji. Upewnij się, że dokładnie wybierz kategorie, które najlepiej opisują główną funkcją aplikacji.

### <a name="ratings"></a>Klasyfikacje

Wszystkie aplikacje muszą mieć klasyfikację w sklepie z aplikacjami. Ta klasyfikacja służy do informuje kontroli rodzicielskiej i ograniczyć dostęp do elementów podrzędnych na podstawie typu i zawartości aplikacji. Podczas definiowania aplikacji, iTunes Connect zawiera listę opisów zawartości, dla których możesz zidentyfikować częstotliwość zawartość jest wyświetlana w aplikacji. Te opcje są konwertowane na klasyfikacji, który jest wyświetlany w sklepie z aplikacjami.

Podczas tworzenia aplikacji dla dzieci, sklepu ma specjalne kategorii 11 przestarzałe elementy podrzędne i w obszarze. Nawet jeśli aplikacja nie jest przeznaczone specjalnie na dzieci, możesz pomóc klientom podejmować decyzje dobrej przez zapewnienie odpowiedniej klasyfikacji zawartości.

> [!IMPORTANT]
> Uwaga: Apple spowoduje odrzucenie znalezione obscenicznych, pornograficznych, obraźliwe lub zniesławiających przesłanych informacji aplikacji.

### <a name="copyright-and-company-information"></a>Informacje o firmie i prawa autorskie

Apple udostępnianie informacji o prawach autorskich aplikacji i wymaga informacji o firmie wydanie aplikacji, takie jak jego adres i informacje kontaktowe (który jest wymagany dla aplikacji wydane do sklepu z aplikacjami koreańskim). Te informacje będą wyświetlane w sklepie z aplikacjami zgodnie z potrzebami.

### <a name="editing-general-app-information-in-itunes-connect"></a>Edytowanie ogólne informacje o aplikacji w iTunes Connect

Wykonaj następujące czynności [iTunes Connect](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa):

1. Polecenie **Moje aplikacje**.
2. Kliknięcie aplikacji **ikona**.
3. Wybierz **wersji** kartę.
4. Przewiń do **ogólne informacje o aplikacji** sekcji.
5. Wypełnij wszystkie wymagane informacje:

    [![](itunesconnect-images/general01.png "Edytowanie ogólne informacje o aplikacji w iTunes Connect")](itunesconnect-images/general01.png#lightbox)
6. Polecenie **Edytuj** przycisk przez **klasyfikacji** można ustawić informacji o klasyfikacji:

    [![](itunesconnect-images/general02.png "Edytowanie wartości")](itunesconnect-images/general02.png#lightbox)
6. Kliknij przycisk **zapisać** przycisk w górnej części ekranu, aby zapisać zmiany.

> [!NOTE]
> Uwaga: Apple spowoduje odrzucenie przesłane, jeśli kategorii lub klasyfikacji nie zgadzają się bieżącą funkcjonalność w aplikacji.

<a name="game-center" />

## <a name="maintaining-game-center-information"></a>Utrzymywanie informacji z aplikacji Game Center

Dla aplikacji gier dla systemu iOS, które obsługują firmy Apple Game Center, możesz podać informacje, takie jak tablice wiodące i osiągnięcia w grę, które są dostępne dla użytkownika i jeśli aplikacja jest zgodny z wielu graczy przez połączenie sieciowe.

### <a name="editing-game-center-information-in-itunes-connect"></a>Edytowanie aplikacji Game Center informacji w iTunes Connect

Wykonaj następujące czynności [iTunes Connect](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa):

1. Polecenie **Moje aplikacje**.
2. Kliknięcie aplikacji **ikona**.
3. Wybierz **wersji** kartę.
4. Przewiń do **Game Center** sekcji.
5. Przerzuć przełącznika przez **Game Center** sekcji do **na** pozycji.
5. Wypełnij wszystkie wymagane informacje:

    [![](itunesconnect-images/gamecenter01.png "Edytowanie aplikacji Game Center informacji w iTunes Connect")](itunesconnect-images/gamecenter01.png#lightbox)
6. Kliknij przycisk **zapisać** przycisk w górnej części ekranu, aby zapisać zmiany.

Użyj **Game Center** kartę, aby aktywować Game Center i obsługa dostępne **tablice wyników** lub **osiągnięć** dla tej aplikacji:

[![](itunesconnect-images/gamecenter02.png "Aktywacja aplikacji Game Center")](itunesconnect-images/gamecenter02.png#lightbox)

[![](itunesconnect-images/gamecenter03.png "Obsługa wszystkie dostępne tablice wyników lub osiągnięcia dla tej aplikacji")](itunesconnect-images/gamecenter03.png#lightbox)

## <a name="maintaining-app-review-information"></a>Obsługa aplikacji przeglądanie informacji o

Użyj tej sekcji, aby podać informacje wymagane do pracowników firmy Apple, które będzie można przeglądania aplikacji, takich jak informacje kontaktowe (Jeśli weryfikacja ma jakieś pytania), wszelkie demonstracyjna konta, które mogą być wymagane, a wszelkie uwagi, które mogą pomóc tester Przejrzyj pomyślnie aplikacji.

### <a name="editing-app-review-information-in-itunes-connect"></a>Edytowanie informacji o przegląd aplikacji w iTunes Connect

Wykonaj następujące czynności [iTunes Connect](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa):

1. Polecenie **Moje aplikacje**.
2. Kliknięcie aplikacji **ikona**.
3. Wybierz **wersji** kartę.
4. Przewiń do **aplikacji Przegląd informacji** sekcji.
5. Wypełnij wszystkie wymagane informacje:

    [![](itunesconnect-images/review01.png "Edytowanie informacji o przegląd aplikacji w iTunes Connect")](itunesconnect-images/review01.png#lightbox)
6. Wybierz jak chcesz aplikacji do zwolnienia do sklepu z aplikacjami po zostały pomyślnie sprawdzone:

    [![](itunesconnect-images/review02.png "Edytowanie informacji o wersji w iTunes Connect")](itunesconnect-images/review02.png#lightbox)
6. Kliknij przycisk **zapisać** przycisk w górnej części ekranu, aby zapisać zmiany.


## <a name="maintaining-pricing-information"></a>Obsługa uzyskać informacje o cenach

Jeśli planujesz wydaniem aplikacji do sprzedaży, należy ustawić ceny sprzedaży, wybierając jedną z firmy Apple dostępne warstwy cenowe i datę danego cennik obowiązywania. Na przykład, począwszy od czasu pisania tego dokumentu **warstwy 1** cennik wygląda podobnie do następującej:

[![](itunesconnect-images/price01.png "Obsługa uzyskać informacje o cenach")](itunesconnect-images/price01.png#lightbox)

### <a name="educational-discount"></a>Zniżki edukacyjnych

Zaznacz to pole wyboru, jeśli chcesz mieć możliwość rabatem instytucjom edukacyjnym za wiele kopii jednocześnie. Szczegóły rabat znajdują się w najnowszej **płatnej umowy aplikacji**, który należy się zalogować przed tej aplikacji będzie dostępny dla klientów z instytucji edukacyjnych.

### <a name="custom-business-to-business-application"></a>Niestandardowych aplikacji biznesowych do biznesowej

Aplikacja, która jest skonfigurowana jako **niestandardowych aplikacji biznesowych do biznesowej** tylko będą dostępne dla **Volume Purchase Program** klientów, które określisz w iTunes Connect i będzie on tylko dostępne w odpowiednich terytoriów (na przykład stany USA Klienci programu zakupów zbiorczych woluminu, należy użyć USA Aplikacji sklepu Volume Purchase Program for Business).

Niestandardowych aplikacji biznesowych i firm nie będą dostępne dla instytucji edukacyjnych lub ogólne klientów do sklepu z aplikacjami. Aby dowiedzieć się więcej o *aplikacji sklepu Volume Purchase Program for Business*, odwiedź stronę firmy Apple [— często zadawane pytania](http://vpp.itunes.apple.com/faq) strony. Aby dowiedzieć się więcej na temat sposobu klientów można uzyskać dostęp do **Volume Purchase Program**, odwiedź stronę firmy Apple [programy wdrażania](http://enroll.vpp.itunes.apple.com) strony.

### <a name="editing-pricing-information-in-itunes-connect"></a>Edytowanie informacji o cenach w iTunes Connect

Wykonaj następujące czynności [iTunes Connect](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa):

1. Polecenie **Moje aplikacje**.
2. Kliknięcie aplikacji **ikona**.
3. Wybierz **cennik** karty:

    [![](itunesconnect-images/price02.png "Edytowanie informacji o cenach w iTunes Connect")](itunesconnect-images/price02.png#lightbox)
4. Wybierz **Data udostępnienia**.
5. Wybierz żądaną cen z **cen warstwy** listy rozwijanej.
5. Opcjonalnie włączyć **edukacyjnych rabaty**.
6. Opcjonalnie zdefiniować aplikacji jako **niestandardowych aplikacji biznesowych do biznesowej**.
6. Kliknij przycisk **zapisać** przycisk, aby zapisać zmiany.

<a name="iap" />

## <a name="maintaining-in-app-purchase-information"></a>Utrzymywanie informacji zakupu w aplikacji

Jeśli planujesz sprzedaży wirtualnego produkty w aplikacji z poziomu aplikacji (np. nowe poziomy gier lub funkcje aplikacji) użyjesz tej sekcji można tworzyć i obsługiwać zakupu tych elementów.

[![](itunesconnect-images/inapp01.png "Utrzymywanie informacji zakupu w aplikacji")](itunesconnect-images/inapp01.png#lightbox)

Aby uzyskać więcej informacji na temat pracy z zakupy w aplikacji w aplikacji platformy Xamarin.iOS, zobacz nasze [zakupu w aplikacji](~/ios/platform/in-app-purchasing/index.md) dokumentacji.

## <a name="viewing-application-reviews"></a>Wyświetlanie aplikacji przeglądów

Po zwolnieniu aplikacji do sklepu z aplikacjami użytkowników, którzy zakupu lub bezpłatnie pobrać aplikację można zapisać przeglądami aplikacji i pozostawić klasyfikacji w formie gwiazdek. Użyj tej sekcji, aby zobaczyć te recenzje. Na przykład:

[![](itunesconnect-images/reviews01.png "Wyświetlanie aplikacji przeglądów")](itunesconnect-images/reviews01.png#lightbox)

## <a name="summary"></a>Podsumowanie

W tym artykule opisano sposób umożliwia przygotowanie aplikację platformy Xamarin.iOS wydania do sklepu iTunes Connect oraz jak zachować wszystkie informacje o aplikacji w magazynie.

## <a name="related-links"></a>Linki pokrewne

- [Praca z obrazami](~/ios/app-fundamentals/images-icons/index.md)
- [Podręcznik przepływu pracy programowania aplikacji systemu iOS: dystrybucja aplikacji](http://developer.apple.com/library/ios/#documentation/Xcode/Conceptual/ios_development_workflow/35-Distributing_Applications/distributing_applications.html)
- [Wskazówki dotyczące przesyłania sklepu z aplikacjami](https://developer.apple.com/appstore/resources/submission/tips.html)
- [Wytyczne dotyczące sklepu App Store](https://developer.apple.com/appstore/resources/approval/guidelines.html)
- [Przewodnik dewelopera Connect iTunes](https://developer.apple.com/library/ios/documentation/LanguagesUtilities/Conceptual/iTunesConnect_Guide/Chapters/About.html#//apple_ref/doc/uid/TP40011225-CH1-SW1)
