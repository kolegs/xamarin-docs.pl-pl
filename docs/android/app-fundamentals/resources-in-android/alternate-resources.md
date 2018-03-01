---
title: "Alternatywne zasobów"
ms.topic: article
ms.prod: xamarin
ms.assetid: AE5A864E-192D-475E-C731-99249C2E7D9E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: e421a52b1ae97b0beef59352a756401ed661051e
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="alternate-resources"></a>Alternatywne zasobów

Alternatywne zasoby są tych zasobów, które odnoszą się do określonego urządzenia lub konfiguracji środowiska wykonawczego, takich jak bieżący język, rozmiaru ekranu lub gęstość pikseli. Jeśli Android może dopasować z zasobem, który jest bardziej szczegółowy dla określonego urządzenia lub Konfiguracja niż domyślny zasób, ten zasób będzie używany zamiast tego. Jeśli nie znajdzie alternatywny zasobów, który odpowiada bieżącej konfiguracji, domyślnych zasobów zostanie załadowany. Jak Android decyduje o tym, jakie zasoby będą używane przez aplikacje będzie można opisane bardziej szczegółowo poniżej, w sekcji lokalizacji zasobów

Alternatywne zasoby są zorganizowane jako podkatalogu w folderze zasobów zgodnie z typem zasobu, podobnie jak domyślnych zasobów. Nazwę podkatalogu alternatywnych zasobów ma postać: _ResourceType_-_kwalifikatora_

*Kwalifikator* jest nazwą, która identyfikuje konfiguracji określonego urządzenia.
W polu Nazwa każdego z nich rozdzielony myślnikiem może istnieć więcej niż jednym kwalifikatorem. Na przykład na poniższym zrzucie ekranu przedstawiono prosty projekt, który ma alternatywnych zasobów dla różnych konfiguracji, takich jak ustawienia regionalne, ekranu w pikselach, rozmiaru ekranu i orientację:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Alternatywne zasobów](alternate-resources-images/alternate-resources-vs.png)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Alternatywne zasobów](alternate-resources-images/alternate-resources-xs.png)
 
-----
 

Dodawanie kwalifikatory typu zasobu obowiązują następujące reguły:

1. Może istnieć więcej niż jednym kwalifikatorem z każdym kwalifikatorem oddzielonymi łącznikiem.

2. Kwalifikatory może być określony tylko jeden raz.

3. Kwalifikatory musi być w kolejności, w jakiej znajdują się w poniższej tabeli.

Poniżej przedstawiono możliwe kwalifikatory dla odwołania:

- **MCC i MNC** &ndash; [numer kierunkowy kraju przenośnych](http://en.wikipedia.org/wiki/List_of_mobile_country_codes) (MCC) i opcjonalnie [kodu sieci komórkowej](http://en.wikipedia.org/wiki/Mobile_Network_Code) (MNC). Karta SIM zapewni MCC, gdy urządzenie jest podłączone do sieci zapewnia MNC. Chociaż jest możliwe do docelowego ustawień regionalnych przy użyciu kodu kraju przenośnych, zaleca się rozwiązaniem jest użycie kwalifikatora języka wymienionymi poniżej. Na przykład do zasobów docelowych Niemczech, będzie kwalifikator `mcc262`. Do zasobów docelowych dla urządzeń przenośnych T w Stanach Zjednoczonych kwalifikator jest `mcc310-mnc026`.
  Pełną listę kodów kraju przenośnych i sieci komórkowej zobacz <http://mcclist.com/>.

- **Język** &ndash; dwuliterowych [kod języka 639 1 ISO](http://en.wikipedia.org/wiki/ISO_639-1) i opcjonalnie następują litery dwa [kodu regionu ISO 3166-alfa-2](http://en.wikipedia.org/wiki/ISO_3166-1_alpha-2). 
  Jeśli oba kwalifikatorów są udostępniane, a następnie są rozdzielone `-r`. Na przykład do ustawień regionalnych docelowy mówiąc francuski, a następnie kwalifikatora `fr` jest używany. Pod kątem French-Canadian ustawień regionalnych, `fr-rCA` mają być używane. Aby uzyskać pełną listę kodów języków i kodów regionów, zobacz [kody reprezentacja nazwy języków](http://www.loc.gov/standards/iso639-2/php/English_list.php) i [nazwy kraju i elementy kodu](http://www.iso.org/iso/country_codes/iso_3166_code_lists/country_names_and_code_elements.htm).

- **Najmniejsza szerokość** &ndash; określa aplikacja jest przeznaczona do wykonania na najmniejszą szerokość ekranu. Opisane bardziej szczegółowo w [tworzenie zasobów na różnych ekranach](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md). 
  Dostępne w poziom interfejsu API 13 (Android 3.2) lub nowszym. Na przykład kwalifikator `sw320dp` służy do urządzenia docelowego, którego wysokość i szerokość jest co najmniej 320dp.

- **Szerokość dostępne** &ndash; minimalną szerokość ekranu w w formacie*N*punktu dystrybucji, gdzie *N* to szerokość w pikselach niezależnych pikseli.
  Ta wartość może zmienić obracania urządzenia. Opisane bardziej szczegółowo w [tworzenie zasobów na różnych ekranach](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md). 
  Dostępne w poziomie interfejsu API 13 (Android 3.2) lub nowszym. Przykład: w720dp kwalifikator jest używany do urządzeń, które mają szerokość 720dp co najmniej.

- **Wysokość dostępne** &ndash; minimalną wysokość ekranu w formacie h*N*punktu dystrybucji, gdzie *N* jest wysokość w punkcie dystrybucji. Ta wartość może zmienić obracania urządzenia. Opisane bardziej szczegółowo w [tworzenie zasobów na różnych ekranach](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md). 
  Dostępne w poziomie interfejsu API 13 (Android 3.2) lub nowszym. Na przykład h720dp kwalifikator jest używany do urządzeń, które mają wysokość 720dp co najmniej

- **Rozmiar ekranu** &ndash; tego kwalifikatora jest generalizacji rozmiaru ekranu, która dotyczy tych zasobów. Zostało opisane bardziej szczegółowo w [tworzenie zasobów na różnych ekranach](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md). 
  Możliwe wartości to `small`, `normal`, `large`, i `xlarge`. Dodane w poziom interfejsu API 9 (Android 2.3 języka w systemie Android 2.3.1/Android 2.3.2)

- **Aspekt ekranu** &ndash; jest oparta na współczynnik proporcji nie orientacji ekranu. Długie ekranu jest większa. Dodane w poziom interfejsu API 4 (dla systemu Android w wersji 1.6). Możliwe wartości to długo i notlong.

- **Orientacji ekranu** &ndash; orientacja pionowa lub pozioma. Można to zmienić okres istnienia aplikacji.
  Możliwe wartości to `port` i `land`.

- **Dock — tryb** &ndash; dla urządzeń w samochodu dock lub dokowanie biurku. Dodane w poziom interfejsu API 8 (Android 2.2.x). Możliwe wartości to `car` i `desk`.

- **Tryb nocy** &ndash; czy aplikacja działa w nocy lub w dniu. Mogą ulec zmianie przez cały okres istnienia aplikacji i jest przeznaczona do dają deweloperom możliwość używania ciemniejszego wersje interfejsu w nocy. Dodane w poziom interfejsu API 8 (Android 2.2.x). Możliwe wartości to `night` i `notnight`.

- **Ekran gęstość pikseli (dpi)** &ndash; liczbę pikseli w danym obszarze na ekranie fizycznych. Zwykle wyrażony jako punkty na cal (dpi). Możliwe wartości to:

    - `ldpi` &ndash; Ekrany gęstość niski.

    - `mdpi` &ndash; Średnia gęstość ekranów

    - `hdpi` &ndash; Ekrany o wysokiej gęstości

    - `xhdpi` &ndash; Ekrany o bardzo wysokiej gęstości

    - `nodpi` &ndash; Zasoby, które nie mogą być skalowane

    - `tvdpi` &ndash; Wprowadzona w poziom interfejsu API 13 (Android 3.2) dla ekranów między mdpi i hdpi.

- **Typ ekran dotykowy** &ndash; Określa typ urządzenie może mieć ekran dotykowy. Możliwe wartości to `notouch` (nie ekran dotykowy), `stylus` (opornościowych ekran dotykowy odpowiednie dla pióra), i `finger` (ekran dotykowy).

- **Klawiatura dostępności** &ndash; Określa, jakiego rodzaju klawiatury jest dostępna. Może to spowodować zmianę przez cały okres istnienia aplikacji &ndash; na przykład gdy użytkownik uruchomi klawiatury sprzętu. Możliwe wartości to:

    - `keysexposed` &ndash; Urządzenie ma klawiatura nie jest dostępna. Jeśli nie ma żadnych klawiatura programowa włączone, następnie ta jest używana tylko po otwarciu klawiatury sprzętu.

    - `keyshidden` &ndash; Urządzenie ma klawiatury sprzętu, lecz jest ukryta i nie klawiatura programowa jest włączona.

    - `keyssoft` &ndash; urządzenie ma klawiatura programowa włączone.

- **Podstawowe metody wprowadzania tekstu** &ndash; Użyj, aby określić, jakiego rodzaju sprzętu klucze są dostępne dla danych wejściowych. Możliwe wartości to:

    - `nokeys` &ndash; Nie ma żadnych kluczy sprzętu dla danych wejściowych.

    - `qwerty` &ndash; Brak dostępnej qwerty klawiatury.

    - `12key` &ndash; Brak klawiatury sprzętu klucza 12


- **Dostępność klucza nawigacji** &ndash; dla podczas nawigacji (kierunkowe konsoli) lub konsoli d 5-sposób jest dostępna. Można to zmienić okres istnienia aplikacji. Możliwe wartości to:

    - `navexposed` &ndash; klawisze nawigacji są dostępne dla użytkownika

    - `navhidden` &ndash; klawisze nawigacji nie są dostępne.

-  **Podstawową metodą nawigacji Non-Touch** &ndash; rodzaj nawigacji jest dostępny na urządzeniu. Możliwe wartości to:

    - `nonav` &ndash; dostępne funkcje tylko nawigacji jest ekran dotykowy

    - `dpad` &ndash; d konsoli (kierunkowe konsola) jest dostępna dla nawigacji

    - `trackball` &ndash; urządzenie ma wskazujących nawigacji

    - `wheel` &ndash; nietypowe scenariusz, w których istnieje jedno lub więcej kierunkowe koła dostępne

-  **Wersja platformy (poziom interfejsu API)** &ndash; poziom interfejsu API, obsługiwane przez urządzenie w formacie v*N*, gdzie *N* poziom interfejsu API, który docelowej. Na przykład v11 będzie obowiązywać 11 (3.0 dla systemu Android) poziom interfejsu API urządzenia.


Aby uzyskać pełne informacje o zasobie Zobacz Kwalifikatory [zapewnianie zasobów](http://developer.android.com/guide/topics/resources/providing-resources.html) w witrynie sieci Web deweloperów systemu Android.


## <a name="how-android-determines-what-resources-to-use"></a>Jak Android Określa, jakie zasoby do użycia

Jest bardzo możliwe i prawdopodobne, że aplikacja systemu Android będzie zawierać wiele zasobów. Należy zrozumieć, jak Android zostanie Wybierz zasoby dla aplikacji uruchomionej na urządzeniu.

Android określa zasoby podstawowej przez Iterowanie za pośrednictwem następującego testu reguł:

- **Eliminowanie sprzeczne kwalifikatory** &ndash; na przykład, jeśli orientacji urządzenia jest pionowy, następnie wszystkie katalogi zasobów pozioma zostanie odrzucone.

- **Ignoruj kwalifikatory nieobsługiwane** &ndash; nie wszystkie kwalifikatory są dostępne na wszystkich poziomach interfejsu API. Jeśli katalog zasobów zawiera kwalifikator, który nie jest obsługiwany przez urządzenie, że katalog zasobów zostanie zignorowany.

- **Zidentyfikuj dalej kwalifikator najwyższy priorytet** &ndash; powyższej tabeli wybierz dalej kwalifikator najwyższy priorytet (od góry do dołu).

- **Zachowaj wszystkie katalogi zasobów kwalifikatora** &ndash; przypadku katalogów żadnych zasobów, zgodnych kwalifikator do powyższej tabeli wybierz dalej kwalifikator najwyższy priorytet (od góry do dołu).

Na poniższym schemacie przedstawiono również te reguły:

[![Schemat blokowy zasobów](alternate-resources-images/flowchart-sml.png)](alternate-resources-images/flowchart.png)

Gdy system jest szuka zasobów specyficznych dla gęstości i nie są one dostępne, spróbuje zlokalizować innych gęstość określonych zasobów i skalować je. Android nie musi używać domyślnych zasobów.
Na przykład podczas wyszukiwania dla zasobu o małej gęstości i nie jest dostępny, Android może wybrać wersji o wysokiej gęstości zasobu nad zasobami domyślnej lub gęstość średnia liczba godzin. Dzieje się tak, ponieważ zasobu o wysokiej gęstości może być skalowany w dół według współczynnika 0,5, co spowoduje mniej problemów widoczność niż skalowania zasobu gęstość średni wymagające współczynnik 0,75.

Na przykład wziąć pod uwagę aplikacja, która ma następujące katalogi obiektów drawable zasobów:

    drawable
    drawable-en
    drawable-fr-rCA
    drawable-en-port
    drawable-en-notouch-12key
    drawable-en-port-ldpi
    drawable-port-ldpi
    drawable-port-notouch-12key

I teraz aplikacja jest uruchamiana na urządzeniu przy użyciu następującej konfiguracji:

- **Ustawienia regionalne** &ndash; pl pl.
- **Orientacja** &ndash; portu
- **Ekran gęstość** &ndash; hdpi
- **Typ ekran dotykowy** &ndash; notouch
- **Podstawowej metody wejściowe** &ndash; 12key

Najpierw francuskim zasoby są eliminowane jako one w konflikcie z ustawieniami regionalnymi `en-GB`, pozostawiając nam z:

    drawable
    drawable-en
    drawable-en-port
    drawable-en-notouch-12key
    drawable-en-port-ldpi
    drawable-port-ldpi
    drawable-port-notouch-12key

Następnie wybrany pierwszy kwalifikator z powyższej tabeli kwalifikatory: MCC i MNC. Nie ma nie katalogi zasobów, które zawierają tego kwalifikatora, więc kod MCC/MNC jest ignorowana.

Kwalifikator dalej jest zaznaczone, który jest językiem. Brak zasobów zgodnych kod języka. Wszystkie katalogi zasobów, które nie odpowiadają kod języka `en` są odrzucane, dzięki czemu lista zasobów jest teraz:

    drawable-en-port
    drawable-en-notouch-12key
    drawable-en-port-ldpi

Kwalifikator dalej, która występuje jest do orientacji ekranu, wszystkie katalogi zasobów, która nie pasuje do orientacji ekranu `port` są eliminowane:

    drawable-en-port
    drawable-en-port-ldpi

Następnie jest kwalifikator dla ekranu w pikselach, `ldpi`, które powoduje wyłączenie jeden katalog więcej zasobów:

    drawable-en-port-ldpi

W wyniku tego procesu systemu Android wykorzystuje zasoby obiektów drawable w katalogu zasobów `drawable-en-port-ldpi` dla urządzenia.

> [!NOTE]
> **Uwaga:** kwalifikatory rozmiaru ekranu Podaj jeden wyjątek tego procesu wyboru. Istnieje możliwość dla systemu Android wybrać zasoby, które są przeznaczone do ekranu mniejsze niż zapewnia jakie bieżącego urządzenia. Na przykład urządzenie większym ekranem mogą korzystać z zasobów zapewniają normalnym rozmiarze ekranu. Jednak to odwrotnej nie jest prawdziwe: na tym samym urządzeniu większym ekranem nie będzie używać zasobów dla xlarge ekranu. Jeśli dla systemu Android nie może znaleźć zestaw zasobów, który dopasowuje rozmiar danego ekranu, aplikacja ulegnie awarii.
