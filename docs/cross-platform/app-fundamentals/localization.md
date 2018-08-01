---
title: Lokalizacja interfejsu użytkownika aplikacji
description: Ten dokument zawiera opis pojęć i platform przystosowywanie do warunków międzynarodowych i lokalizacja i sprawdza, czy ich wpływ na projekt aplikacji.
ms.prod: xamarin
ms.assetid: CC6847B2-23FB-4EDE-9F7E-EF29DD46A5C5
author: asb3993
ms.author: amburns
ms.date: 03/22/2017
ms.openlocfilehash: 299fe28a896c2bf2e5c420b330b8e8bde7d9dc22
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34781959"
---
# <a name="localization"></a>Lokalizacja

W tym przewodniku wprowadza pojęcia dotyczące *internacjonalizacji* i *lokalizacja* i linki do instrukcji dotyczących sposobu tworzenia aplikacji dla urządzeń przenośnych Xamarin przy użyciu tych pojęć.

Jeśli chcesz przejść bezpośrednio do szczegóły techniczne dotyczące lokalizowania aplikacji platformy Xamarin, Rozpocznij od jednego z tych instrukcje specyficzne dla platformy:

- [**Platformy Xamarin.Forms** ](~/xamarin-forms/app-fundamentals/localization/index.md) lokalizacja wielu platform przy użyciu pliki RESX.
- [**Xamarin.iOS** ](~/ios/app-fundamentals/localization/index.md) platformy macierzystego lokalizacją.
- [**Xamarin.Android** ](~/android/app-fundamentals/localization.md) platformy macierzystego lokalizacją.

## <a name="i18n-and-l10n"></a>i18n i L10n

*Przystosowywanie do warunków międzynarodowych* to proces polegający na wprowadzaniu kod umożliwiający wyświetlanie różnych języków oraz dostosowania wyświetlanie różnych ustawień regionalnych (takich jak numer i formatowania daty). To jest również nazywany *globalizacji*.

*Lokalizacja* jest to krok następującym — tworzenie zasobów (na przykład ciągi i obrazy) dla każdego języka i paczki je z aplikacją internationalize.

Internacjonalizacji, który często jest obcinana do i18n — skrócona do 18 liter między "i" a "n". Lokalizacja podobnie jest obcinana do L10n — w przypadku 10 litery od "L" i "n".

## <a name="overview"></a>Omówienie

Ten dokument wprowadza pojęcia związane z przystosowywanie do warunków międzynarodowych i lokalizacja oraz sposobów ich zastosowania w opracowywanie aplikacji mobilnych zasadniczo.
Podczas projektowania i tworzenia aplikacji, rzeczy, który może być wcześniej zapisane na stałe, ale które musi sparametryzowana to lokalizacja:

-   Układów ekranu głównego i tekst
-   Ikony, grafiki i kolory,
-   Pliki dźwiękowe i wideo
-   Dynamiczny tekst i formatowania tekstu (na przykład numery, waluty i daty)
 - Zmian układu dla języków (od prawej do lewej) od prawej do lewej, i
-   Sortowanie danych.

Niezależnie od tego, które platformy urządzeń przenośnych aplikacji jest przeznaczony dla tych wskazówek pomoże Ci tworzenie wysokiej jakości aplikacji zlokalizowanego.


## <a name="design-considerations"></a>Zagadnienia dotyczące projektowania

Projektowania aplikacji, dzięki czemu można przetłumaczyć jej zawartość jest nazywany internacjonalizacji. Przystosowywanie do warunków międzynarodowych prawidłowo jest więcej niż zezwala tylko na inny język ciągi do załadowania w czasie wykonywania — dobrze zaprojektowanego aplikacji powinna zezwalać na wszystkich zasobów zostanie zmieniony na podstawie języka i ustawienia regionalne (w tym obrazów, dźwięki i wideo) i możliwość dostosowania Formatowanie i układu, aby poradzić sobie z różnych rozmiarach ciągów.

W tej sekcji omówiono niektóre zagadnienia dotyczące projektowania, należy wziąć pod uwagę podczas tworzenia aplikacji międzynarodowych.

### <a name="layouts-and-string-length"></a>Układy i długość ciągu

Ciągi chiński i japoński może być bardzo krótki — czasami co najmniej dwa znaki mogą być przydatne dla etykiety pola wejściowego.

Niemiecki ciągi (na przykład) można bardzo długie. Czasami stosunkowo krótkich programu word w języku angielskim staje się bardzo dużo w innych językach — albo zostać przycięty lub innego nieoczekiwanie ponownym wlaniem układu.

Porównanie kilka elementów ekranu głównego z systemem iOS w języku angielskim, niemiecki i japoński długości ciągu:

[![](localization-images/language-compare-sml.png "Długość ciągu japońskiego niemieckiej wersji programu vs")](localization-images/language-compare.png#lightbox)

Zwróć uwagę, że **ustawienia** w języku angielskim (8 znaków) wymaga translację niemieckiego, ale tylko 2 znaki w języku japońskim 13 znaków.

Układy, w którym wyświetlana etykieta i pole wejściowe są side-by-side są trudne do wykonania, gdy długość etykiety może się znacznie różnić. Często układu, w którym jest wyświetlana etykieta powyżej pola jest łatwiejsze do zlokalizowania, ponieważ pełnej szerokości ekranu jest dostępny na etykiecie i dane wejściowe.

Jako ogólną regułę Jeśli tworzysz układów stałego (szczególnie elementy side-by-side) umożliwiają co najmniej 50% szerokość więcej niż Twoje angielskiej wersji językowej ciągi wymagają etykiety i tekstu. Nie rozwiązać każdy problem, ale zapewnia buforu, który będzie działać w wielu przypadkach.

### <a name="input-validation"></a>Sprawdzania poprawności danych wejściowych

Uważaj założenia podczas zapisywania reguł sprawdzania poprawności. Może wydawać się nieprawidłowe wymagane pole tekstowe dane wejściowe "wymagane" co najmniej trzy znaki w języku angielskim, ponieważ pojedyncze litery bardzo rzadko nie ma żadnego znaczenia. W języku chińskim i japoński jednak pojedynczy znak może być prawidłową wartość wejściową, a weryfikacji komunikat "wymagana jest co najmniej 3 znaki" nie ma sensu w tych językach.

Inne pozornie prostych zadań, takich jak sprawdzanie poprawności adresu e-mail lub adres URL witryny sieci Web staną się bardziej skomplikowane ze znakami nie są ograniczone do podzbioru ASCII.

Zapisu reguł sprawdzania poprawności z internacjonalizacji pamiętać — Wybierz najmniej restrykcyjne zasady albo zapisać logikę, która działa inaczej dla każdego języka.

### <a name="images-and-color"></a>Obrazy i koloru

Nie każdy obraz musi zmienić oparte na wybór języka użytkownika. Wiele ikony lub zdjęć będzie być odpowiednie dla wszystkich użytkowników, niezależnie od tego, język ich mowy.
Niektóre zasoby sensu do zlokalizowania, takich jak:

 - Obrazy przedstawiające osób lub w określonych lokalizacjach — aplikacji mogą uznać bardziej odpowiednie dla użytkowników, jeśli zawiera użytkowników lokalnych/lokalizacji.
 - Ikony — niektóre nadruków mogą być specyficzne dla kultury i użytkownik może ułatwić aplikacji użyj przez Lokalizowanie obrazów, aby odzwierciedlić opis lokalnego.
 - Kolory — niektóre kultur zrozumieć kolory inaczej — czerwony może oznaczać, ostrzeżenie w jeden region, ale powodzenia w innym. Podczas projektowania aplikacji w celu określenia, czy użytkownik powinien tworzenia mechanizm do zlokalizowania kolorów, należy skontaktować się z rodzimych.


### <a name="videos-and-sound"></a>Wideo i dźwięku

Filmy wideo i dźwiękowy istnieje wyzwania specjalne podczas lokalizowania aplikacji, ponieważ jest stosunkowo łatwa do pobrania przetłumaczone ciągi, rejestrowanie wielu voiceover śledzi lub klipów wideo, może być zarówno drogie i trudne.

Wiele kopii pliki dźwiękowe i wideo może znacząco zwiększyć rozmiar aplikacji, (zwłaszcza, jeśli są lokalizowanie w wielu językach lub ma wiele plików multimedialnych). Należy rozważyć, pobieranie tylko zasoby wymagane języka po użytkownik zainstalował aplikację, ale również może to spowodować niską użytkowników w wolno działających sieciach.

Często istnieje wiele sposobów, aby rozwiązać problemy związane z lokalizacją — najważniejsze jest należy wziąć pod uwagę ich początkowych i upewnij się, że aplikacja ma zajmie się nimi.


### <a name="dates-times-numbers-and-currency"></a>Daty, godziny, liczby i waluty

Jeśli używasz formatowania funkcje środowiska .NET, pamiętaj, aby określić kulturę tak, aby separatorów dziesiętnych poprawnie przeanalizować (i uniknąć zgłaszane wyjątki konwersji). Na przykład zarówno 1.99, jak i 1,99 są prawidłowe dziesiętną reprezentacje w zależności od ustawień regionalnych.

Po pochodzi ze znanego źródła danych (tj. z własnego kodu lub usługą sieci web przez Ciebie) można kodowania identyfikator kultury, zgodny z formatowaniem, takie jak InvariantCulture, która będzie działać w przypadku formatowania język angielski standard.

```csharp
double.Parse("1,999.99", CultureInfo.InvariantCulture);
```

Jeśli jest on wprowadzania danych przez użytkownika aplikacji, analizy przy użyciu wystąpienia CultureInfo, które odzwierciedla ich ustawień regionalnych:

```csharp
double.Parse("1 999,99", CultureInfo.CreateSpecificCulture("fr-FR"));
```

Zobacz [analizowanie ciągów liczbowych](http://msdn.microsoft.com/library/xbtzcc4w(v=vs.110).aspx) i [ciągów analizowania daty i godziny](http://msdn.microsoft.com/library/2h3syy57(v=vs.110).aspx) MSDN artykuły, aby uzyskać dodatkowe informacje.

<a name="rtl" />

### <a name="right-to-left-rtl-languages"></a>Języki od prawej do lewej (od prawej do lewej)

Niektóre języki, takie jak arabskiego, hebrajskiego i Urdu (na przykład), są odczytywane z prawej do lewej.
Aplikacje, które obsługują te języki należy używać ekranu projekty, które dostosowania czytniki od prawej do lewej, na przykład:

 - Tekst powinien być wyrównany do prawej.
 - Etykiety powinny być wyświetlane z prawej strony pól wejściowych.
 - Położenie przycisku domyślne zazwyczaj została odwrócona.
 - Szybko przesuwając hierarchiczna nawigacji i animacji (i innych metafory nawigacji i animacje) wykorzystujące kierunek kontekstu również ma być zmieniony.

Zarówno dla systemu iOS i Android obsługują układów od prawej do lewej i renderowania czcionek z wbudowane funkcje, które ułatwiają wprowadzanie korekt powyżej. Platformy Xamarin.Forms nie obsługuje obecnie automatycznie renderowania od prawej do lewej.

### <a name="sorting"></a>Sortowanie

W różnych językach inaczej, zdefiniuj kolejność ich małych liter, nawet wtedy, gdy korzystają z tego samego zestawu znaków.

Zobacz [szczegółowe porównanie ciągów](http://msdn.microsoft.com/library/dd465121(v=vs.110).aspx#the_details_of_string_comparison) w [najlepsze rozwiązania dotyczące przy użyciu ciągów w programie .NET Framework](http://msdn.microsoft.com/library/dd465121(v=vs.110).aspx) na przykład, gdy język (CultureInfo) dotyczy kolejności sortowania.

Jest mało prawdopodobne, możliwości wbudowaną bazą danych na platformach mobilnych będzie obsługiwać sortowania specyficzny dla języka kolejności, może być wymagane do zaimplementowania dodatkowy kod w logice biznesowej.

### <a name="text-search"></a>Wyszukiwanie tekstu

Upewnij się, zapisu i przetestować z algorytmu wyszukiwania z wieloma językami pamiętać. Co należy wziąć pod uwagę, obejmują:

 - Funkcja automatycznego uzupełniania — Jeśli utworzono funkcja Automatyczne zakończenia upewnij się, że jego źródła sugestie dotyczące język użytkownika.
 - Zapytanie pasujących do danych — umożliwia wyszukiwanie kwerendy wprowadzane w określonej języka można wykonać tylko zawartości w tym języku lub względem całą zawartość w swojej aplikacji?
 - Wynikające — Jeśli wyszukiwanie jest zbudowany wyszukiwania podobne słowa, katalogów głównych programu word i inne optymalizacje wyszukiwania, czy te optymalizacje skompilowany dla wszystkich języków, która jest obsługiwana?
 - Sortowanie — upewnij się, że wyniki są sortowane nieprawidłowo (zobacz powyżej).


### <a name="data-from-external-sources"></a>Dane ze źródeł zewnętrznych

Wiele aplikacji pobierania danych ze źródeł zewnętrznych z serwisem Twitter oraz źródła danych RSS pogodzie, wiadomości lub cen akcji. Podczas wyświetlania tego użytkownika, należy wziąć pod uwagę możliwość, że będzie wyświetlać ekranu nie ma zastosowania lub nie można odczytać informacji do nich.

Istnieje kilka strategii, używanego do spróbuj i upewnij się, że aplikacja wyświetla dane użytkownika:

 - Różnych źródeł — aplikacja może pobrać danych z innego źródła, w zależności od języka lub ustawień regionalnych użytkownika. Ceny wiadomości, pogody i zapasów ustawień regionalnych może być większe znaczenie niż coś pobrane ze źródła (Ameryka Północna).
 - Zlokalizowanych wyświetlania — w przypadku wyświetlania Twitter lub fotografii źródła danych, użytkownik powinien być wyświetlany metadanych (np. czas) w języku własnego, nawet jeśli samej zawartości pozostanie w oryginalnym języku.
 - Tłumaczenie — można utworzyć opcji translacji w swojej aplikacji w celu tłumaczenia maszynowego przychodzących danych. Może być automatyczna lub według uznania użytkownika — po prostu upewnij się, Powiadom użytkownika, jeśli to odbywa się, ponieważ tłumaczenia maszyny nigdy nie są doskonałe!

To może wpłynąć na łącza zewnętrzne ścieżki audio i wideo — podczas projektowania aplikacji upewnij się, planowanie sourcing translacji zawartości lub zapewnienia użytkownicy są odpowiednio informowanie przy użyciu interfejsu użytkownika, gdy zawartość nie będzie udostępniana w ich język.


### <a name="dont-over-translate"></a>Nadmiernie nie tłumaczenia

Niektórych ciągów w aplikacji może nie wymagają tłumaczenia lub co najmniej należy zwrócić szczególną uwagę przez translatora. Przykładami mogą być następujące:

 - Adresy URL — Jeśli wyświetlany adres URL, może lub nie może być konieczne można dostosować za pomocą języka. Na przykład facebook.com nie wymaga tłumaczenia auto wykryje języka w lokacji głównej. Innych witryn ustawień regionalnych zawartości, może zaistnieć potrzeba oferują inny adres URL, takie jak yahoo.com i yahoo.fr lub yahoo.it.
 - Numery telefonów — zwłaszcza z różnych kodów kraju lub numerów wywołań, w których konkretnego języka mowy.
 - Szczegóły dotyczące kontaktu — adresy i inne informacje są zależne od języka lub ustawień regionalnych.
 - Znaki towarowe & nazw produktów — niektórych ciągów nie ma potrzeby tłumaczenia, ponieważ są one zawsze zapisywane w tym samym języku.

Na koniec należy uwzględnić szczegółowe instrukcje dotyczące translator, jeśli niektórych ciągów wymagają szczególnego traktowania.


### <a name="formatted-text"></a>Tekst sformatowany

Nie jest zazwyczaj problem z aplikacji mobilnych, ponieważ ciągi zazwyczaj nie są Bogato sformatowane. Jednak jeśli tekstu sformatowanego (takich jak pogrubieniem lub kursywą) jest wymagane w aplikacji upewnij się, translator wie jak jako danych wejściowych, formatowanie, pliki ciągi należy go przechowywać poprawnie i jest poprawnie sformatowana przed wyświetleniem dla użytkownika (tj. przypadkowo Niech własne kody formatowania przedstawiane użytkownika).



## <a name="translation-tips"></a>Wskazówki dotyczące tłumaczenia

Tłumaczenie parametry używane przez aplikacje uważa się częścią procesu lokalizacji. Zwykle to zadanie zostanie zewnętrzna Usługa do usługi tłumaczenia i wykonywane przez personel wielu języków, który może nie wiedzieć, aplikacji lub firmy.

Poniższe porady mogą pomóc tworzą ciągi, które są łatwiejsze do tłumaczenia dokładnie i w związku z tym poprawić jakość zlokalizowanych aplikacji.



### <a name="localize-complete-strings-not-words"></a>Localize — ciągi pełną, nie słowa

Czasami deweloperzy zająć podejście próby Określ pojedyncze słowa lub zdania "wstawki", aby ponownie mogą ich używać w całej aplikacji. Na przykład tekst "masz 5 wiadomości." mogą one określać następujące ciągi do tłumaczenia

**Zły**:

```csharp
"You have"
"no"
"message"
"messages"
```

a następnie spróbuj utworzyć poprawne wyrażenie na bieżąco za pomocą ciągów kodu:

**Zły**:

```csharp
"You have" + " " + numMsgs + " " + "messages"
"You have" + " no " + "messages"
```

**To nie jest zalecane** ponieważ niekoniecznie nie będzie działać dla wszystkich języków i będzie trudne do translator zrozumienie kontekstu krótkich każdy z segmentów. Również prowadzi do ponownego użycia przetłumaczone ciągi, które mogą powodować problemy później Jeśli są używane w różnych kontekstach (i następnie aktualizowany).


### <a name="allow-for-parameter-re-ordering"></a>Zezwalaj na zmiany kolejności parametrów

Niektóre języki programowania wymagają dodatkowych składni, aby określić kolejność parametrów w ciągu, jednak .NET już obsługuje pojęcie numerowane symbole zastępcze, więc

**Dobrym**:

```csharp
"a {0} b {1} cde {3}"
```

można przetłumaczyć następujące (gdzie pozycji i kolejność symboli zastępczych zostanie zmieniona)

```csharp
"{2} {3} f g h {0}"
```

i tokeny będą uporządkowane jako translator przeznaczone. Należy uwzględnić wyjaśnienie co każdego symbolu zastępczego zawiera przy wysyłaniu ciąg do translatora.


### <a name="use-multiple-strings-for-cardinality"></a>Użyj wielu ciągów dla kardynalności

Należy unikać ciągów, takich jak `"You have {0} message/s."` Użyj ciągów specyficznych dla każdego stanu, aby zapewnić lepsze środowisko:

**Dobrym**:

```csharp
"You have no messages."
"You have 1 messages."
"You have 2 messages."
"You have {0} messages."
```

Należy napisać kod aplikacji w celu oceny liczby wyświetlanej, a następnie wybierz odpowiedni ciąg. Niektóre platformy (w tym z systemami iOS i Android) mają wbudowane funkcje automatycznie wybrać najlepsze ciąg mnogiej zgodnie z preferencjami dla bieżącego języka/ustawień regionalnych.


### <a name="allowing-for-gender"></a>Zezwala na płci

Alfabetu łacińskiego języków czasami użyć innych wyrazów w zależności od rodzaju podmiotu. Jeśli aplikacja zna płci, powinien umożliwić przetłumaczone ciągi to odzwierciedlić.

Dostępna jest również bardziej oczywisty przypadek nawet w języku angielskim, gdy ciągi odnoszą się do określonej osoby lub użytkownik aplikacji. Na przykład niektóre witryny Pokaż komunikaty, takich jak `"Bob commented on his post"` warto ciągów dla męskiego, żeńskiego i niebinarne lub nieznany płci:

**Dobrym**:

```csharp
"{0} commented on his post"
"{0} commented on her post"
"{0} commented on their post"
```

### <a name="dont-reuse-strings"></a>Nie należy używać ponownie ciągów

Lub dokładniej nie ponownie użyć ciągów tak, ponieważ są one podobne, gdy sam ciąg ma innego celu lub znaczenie.

Na przykład: Wyobraź sobie ma włączony/wyłączony w aplikacji i kontrolka przełącznika wymaga tekst "na" i "off" do lokalizacji. Również wyświetlić wartość tego ustawienia w innym miejscu w aplikacji w etykietę tekstową. Należy korzystać z różnych parametrów do wyświetlenia przełącznika lub przełącznika stan (nawet jeśli są one ten sam ciąg w języku domyślnym) — na przykład:

-   "Na" — jest wyświetlany na tego samego przełącznika
-   "Wyłączone" — jest wyświetlany na tego samego przełącznika
-   "Na" — jest wyświetlany w etykiecie
-   "Wyłączone" — jest wyświetlany w etykiecie

Zapewnia to elastyczność maksymalną translator:

-   Ze względu na projekt może małe litery, "włączone" i "off" używa tego samego przełącznika, ale Wyświetl etykietę używa wielkimi literami "On" i "Off".
-   W przypadku niektórych języków może być konieczne wartość przełącznika się jako mieści się w kontrolki interfejsu użytkownika, gdy całe słowo (przetłumaczonego) może wystąpić w etykiecie.
-   Można również w przypadku niektórych języków renderowania przełącznika może być użycie dla kultury znajomości "I" i "O", ale nadal ma etykietę do odczytu "On" lub "Off".

### <a name="translation-services"></a>Usługi tłumaczeniowe

#### <a name="machine-translation"></a>Tłumaczenie automatyczne

Aby utworzyć funkcji tłumaczenia w swojej aplikacji, należy wziąć pod uwagę [interfejsu API Azure Translator tekstu](https://azure.microsoft.com/en-au/services/cognitive-services/translator-text-api/).

Do celów testowych można użyć jednego z wielu narzędzi tłumaczenia online do dołączenia tekstu zlokalizowanej w Twojej aplikacji podczas tworzenia:

- [Bing Translator](https://www.bing.com/translator/)
- [Tłumaczenie Google](http://translate.google.com/)

Istnieje wiele innych dostępne. Jakość tłumaczenia maszynowego zazwyczaj nie jest uznawane za dobry zwolnić aplikacji bez najpierw zostanie sprawdzone i przetestowane przez tłumaczy professional lub rodzimych.

#### <a name="professional-translation"></a>Tłumaczenie Professional

Dostępne są także professional tłumaczeń, które otrzymuje z ciągów i przekazać je do ich własnych tłumaczy, zapewniając gotowe tłumaczeń za opłatą.

Jednym z najpopularniejszych usług jest [LionBridge](http://www.lionbridge.com/). Najbardziej usług obsługuje wszystkie typowe typy plików ciągów, XML, RESX i POT lub PO tym.


## <a name="summary"></a>Podsumowanie

W tym artykule wprowadzono niektóre założenia należy znać przed internacjonalizacji aplikacji, a następnie lokalizowanie zasobów, a także objęte jak zmienić preferencje językowe dla każdej platformy.

Tych pojęć można zastosować do różnych technik internacjonalizacji specyficzne dla platformy i między platformami, które można wykonać za pomocą platformy Xamarin.

Kontynuuj, odczytywanie szczegółowe informacje techniczne dla platformy, którą chcesz:

- [Platformy Xamarin.Forms](~/xamarin-forms/app-fundamentals/localization/index.md) lokalizacja wielu platform przy użyciu pliki RESX.
- [Xamarin.iOS](~/ios/app-fundamentals/localization/index.md) platformy macierzystego lokalizacją.
- [Xamarin.Android](~/android/app-fundamentals/localization.md) platformy macierzystego lokalizacją.



## <a name="related-links"></a>Linki pokrewne

- [Omówienie lokalizacja firmy Apple](https://developer.apple.com/internationalization/)
- [Lista kontrolna lokalizacji dla systemu android](http://developer.android.com/distribute/tools/localization-checklist.html)
- [Najlepsze rozwiązania dotyczące tworzenia aplikacji gotowych (MSDN)](http://msdn.microsoft.com/library/w7x1y988%28v=vs.90%29.aspx)
