---
title: Pojęcia SiriKit
description: Tym dokumencie opisano podstawowe pojęcia, które są wymagane do pracy z SiriKit w aplikacji platformy Xamarin.iOS. Na przykład zawiera omówienie intencje i rozszerzenia intencje interfejsu użytkownika, uprawnienia SiriKit projektowanie doskonałego i inne.
ms.prod: xamarin
ms.assetid: 99EC5C1E-484F-4371-8555-58C9F60DE37F
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/02/2017
ms.openlocfilehash: 62b612f2e2725e5856a39e1d4d3fc1288282167a
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34788933"
---
# <a name="understanding-sirikit-concepts"></a>Pojęcia SiriKit

_W tym artykule opisano podstawowe pojęcia, które będą wymagane do pracy z SiriKit w aplikacji platformy Xamarin.iOS._


Nowość w systemach iOS 10, SiriKit umożliwia aplikacji platformy Xamarin.iOS do świadczenia usług, które są dostępne dla użytkownika za pomocą aplikacja map i używanie programu Siri na urządzeniu z systemem iOS. Ta funkcja znajduje się w co najmniej jednego rozszerzenia aplikacji przy użyciu nowego **intencje** i **intencje interfejsu użytkownika** struktury.

SiriKit, dzięki czemu aplikacja systemu iOS do świadczenia usług, które są dostępne dla użytkownika za pomocą aplikacja map i używanie programu Siri na urządzeniu z systemem iOS przy użyciu rozszerzeń aplikacji i nowych **intencje** i **interfejsu użytkownika intencje** struktury.

Używanie programu Siri działa z pojęciem **domen**, grup znać akcji dla zadania pokrewne. Każdy interakcji, która aplikacja ma z Siri muszą należeć do jednej z jego domeny znanej usługi:

- Audio i wideo telefonicznej.
- Rezerwacji jazdy.
- Zarządzanie ćwiczeń.
- Do obsługi komunikatów.
- Wyszukiwanie zdjęcia.
- Wysyłanie lub odbieranie płatności.

Gdy użytkownik wysyła żądanie Siri odwołania rozszerzenia aplikacji usług, SiriKit wysyła rozszerzenia **zamiar** obiektu, który opisuje żądanie użytkownika wraz z danymi pomocniczych. Rozszerzenie aplikacji następnie generuje odpowiednie **odpowiedzi** obiektu dla danego **zamiar**, opisujące, jak rozszerzenie może obsłużyć żądania.

## <a name="the-intents-and-intents-ui-extensions"></a>Intencje i opcje rozszerzenia interfejsu użytkownika

Zarówno Siri, jak i aplikacji mapy interakcji z usługi aplikacji za pośrednictwem dwa różne typy rozszerzeń aplikacji:

- **Rozszerzenie intencje** -mapy z aplikacją i udostępnia Siri zawartość i wykonywanie zadań wymaganych do spełnienia dowolnego obsługiwanych lokalizacji docelowych.
- **Rozszerzenie interfejsu użytkownika intencje** — dostarcza niestandardowego interfejsu użytkownika, który będzie wyświetlany dla zawartości aplikacji wewnątrz Siri lub mapy.

Aplikacja musi dostarczyć rozszerzenie intencje obsługuje SiriKit i jest odpowiedzialny za informacjami, których używanie programu Siri i map może ona powodować problemy dla użytkownika i obsługa intencje.

Tworzenie rozszerzenia interfejsu użytkownika intencje jest opcjonalne, ponieważ Siri zazwyczaj obsługuje wszystkie interakcji z użytkownikiem i ma standardowego, wbudowanego interfejsu użytkownika do prezentowania informacji w każdej z obsługiwanych domen. Zapewniając intencje rozszerzenie interfejsu użytkownika, aplikacja może używać **interfejsu użytkownika zamiar** framework do prezentowania rozbudowane, niestandardowego interfejsu użytkownika wyposażony w aplikacji do znakowania i dodatkowe informacje.

## <a name="siri-and-the-maps-app-role"></a>Używanie programu Siri i roli aplikacji mapy

Rozmowy żądań użytkownika są języka przetwarzane i semantycznie analizowane przez Siri, która włącza te żądania do przydatnych wyników lokalizacji docelowych, które może obsłużyć rozszerzenia zamiar.

Społeczność Maps używa aplikację zamierzone rozszerzenia do wyświetlania informacji w interfejsie mapy w odpowiedzi na działania użytkownika. Takich jak żądania w pobliżu restauracji lub pobieranie przegląda restauracji aplikacji.

Zarówno Siri, jak i mapy zarządzanie wszystkimi interakcji użytkownika i wyświetlić wyniki za pomocą interfejsu standardowy system. Rola rozszerzeń aplikacji jest głównie udostępniania danych, które pobiera wyświetlane. Opcjonalnie aplikacja może udostępniać intencje rozszerzenie interfejsu użytkownika i prezentowanie niestandardowego interfejsu użytkownika w celu zwiększenia domyślnego interfejsu systemu.

## <a name="interacting-with-siri-via-sirikit"></a>Interakcja z Siri za pośrednictwem SiriKit

W tej sekcji przedstawia omówienie sposobu SiriKit zezwala użytkownikowi na interakcję z aplikacji przy użyciu Siri. Dla tego przykładu będziemy używać fałszywych aplikacji MonkeyChat:

[![](understanding-sirikit-images/monkeychat01.png "Ikona MonkeyChat")](understanding-sirikit-images/monkeychat01.png#lightbox)

MonkeyChat zachowuje własną kontaktów w książce znajomych użytkownika, każde skojarzone z nazwą ekranu (na przykład Bobo, na przykład) i umożliwia użytkownikowi wysyłanie rozmowy tekstu do każdego przyjazne nazwy ekranu.

Istnieje wiele sposobów, użytkownik może zainicjować interakcji z aplikacją, ponieważ różne osoby, może być tego samego żądania w wielu różnych formularzach.

Na przykład jeśli użytkownik chce wysłać komunikat do ich friend Bobo, mogą pojawić się następujące konwersacji z Siri:

_Użytkownik: Witaj Siri, Wyślij wiadomość MonkeyChat._<br />
_Używanie programu Siri: komu?_<br />
_Użytkownik: Bobo._<br />
_Używanie programu Siri: Co chcesz powiedzieć, aby Bobo?_<br />
_Użytkownik: Wyślij bananów więcej._<br />

Inna osoba może uniemożliwić tego samego żądania o różnych konwersacji:

_Użytkownik: Wyślij wiadomość do Bobo na MonkeyChat._<br />
_Używanie programu Siri: Co chcesz powiedzieć, aby Bobo?_<br />
_Użytkownik: Wyślij bananów więcej._<br />

I inny użytkownik może uniemożliwić nawet krótszą żądania:

_Użytkownik: MonkeyChat Bobo Wyślij bananów więcej._<br />
_Używanie programu Siri: Ok wysyłania komunikatu Wyślij więcej bananów do Bobo na Monkeychat._<br />

Lub nawet zgłoszenia tego samego żądania w innym języku:

_Użytkownik: MonkeyChat Bobo s'il vous plaît envoyer plus de bananes._<br />
_Używanie programu Siri: Oui, envoi komunikat s'il vous plaît envoyer plus de bananes a Bobo sur Monkeychat._<br />

Jeszcze inny użytkownik może być bardzo pełne ich konwersacji:

_Użytkownik: Witaj Siri, można prosimy nie mnie Preferuj i uruchamianie aplikacji MonkeyChat wysyłanie tekstowy z komunikatem Wyślij bananów więcej._<br />
_Używanie programu Siri: komu?_<br />
_Użytkownik: Moje najlepsze pal Bobo._<br />

Ponadto istnieje wiele sposobów, które Siri może odpowiadać na żądanie, niektóre oparte na jak żądanie zostało utworzone:

- **Przytrzymując przycisk Strona główna** -Siri zapewni więcej visual odpowiedzi z ograniczoną ustne opinii.
- **Przez "Witaj Siri"** -Siri będą bardziej ustne i podaj mniejszą liczbę visual odpowiedzi.

Używanie programu Siri również jest dostosowana do potrzeb ułatwień dostępu użytkownika i będzie interakcji i odpowiadać na podstawie tych potrzeb.

Niezależnie od tego, jak żądanie lub jak Siri odpowiada na żądanie Siri obsługuje konwersacji z użytkownikiem i aplikacji (za pomocą jego rozszerzenia), udostępnia funkcje.

Gdy użytkownik wysyła żądanie ustne Siri, są kroki, które Siri będzie wykonać:

[![](understanding-sirikit-images/monkeychat02.png "Kroki, które będzie wykonać Siri")](understanding-sirikit-images/monkeychat02.png#lightbox)

1. Po pierwsze, Siri przyjmuje audio użytkownika **mowy** i konwertuje ją na tekst.
2. Następnie tekst jest konwertowany na **zamiar**, strukturę reprezentację żądanie użytkownika.
3. W oparciu o zamiar, potrwa Siri **akcji** do wykonania żądania użytkownika.
4. Na koniec przedstawi Siri **odpowiedzi** (visual i ustne) do użytkowników oparte na akcję wykonywaną.

Istnieją trzy sposoby, które aplikacji mogą brać udział w konwersacji użytkownika z Siri:

[![](understanding-sirikit-images/monkeychat03.png "Trzy sposoby aplikacji mogą brać udział w konwersacji użytkowników z Siri")](understanding-sirikit-images/monkeychat03.png#lightbox)

1. **Słownik** — jest to, jak aplikacja informuje Siri słowa musi wiedzieć, aby korzystać z niego.
2. **Aplikacja logiki** — są to akcje i odpowiedzi, które będą miały aplikacji na podstawie podanych opcji.
3. **Interfejs użytkownika** — jest to interfejs użytkownika opcjonalne, niestandardowych, który aplikacji można umożliwić jego odpowiedzi w.

### <a name="example"></a>Przykład

Biorąc pod uwagę powyższe informacje, sprawdź następujące konwersacji czy interakcję z aplikacji MonkeyChat:

_Użytkownik: Witaj Siri, wysyłania komunikatu do Bobo na MonkeyChat._<br />
_Używanie programu Siri: Co chcesz powiedzieć, aby Bobo?_<br />
_Użytkownik: Wyślij bananów więcej._<br />

Pierwsza rola aplikacji przyjmującego konwersacji jest pomoc Siri zrozumieć mowy użytkownika:

[![](understanding-sirikit-images/monkeychat04.png "Pomaga zrozumieć mowy użytkowników Siri")](understanding-sirikit-images/monkeychat04.png#lightbox)

Siri nie ma nazwę "Bobo" w swojej bazie danych, ale aplikacja i ma udostępniać te informacje Siri za pośrednictwem jego słownika. Aplikacja pomaga również w Siri Bobo jest adresata, ponieważ określone je do Siri jako *skontaktuj się z*.

Używanie programu Siri wie więcej jest wymagany do wysyłania wiadomości niż tylko odbiorcy, aby szybko będzie sprawdzać z rozszerzeniem aplikacji, aby zobaczyć, czy wiadomość wymaga zawartości. Ponieważ program MonkeyChat Siri będzie odpowiadać użytkownika z pytaniem: *"Co chcesz powiedzieć, aby Bobo?"*

W powyższym przykładzie użytkownik odpowiedział, *"Wyślij więcej bananów"*, który Siri będzie pakietu w strukturze **zamiar**:

[![](understanding-sirikit-images/monkeychat05.png "Używanie programu Siri będzie pakietu odpowiedzi użytkownika do zamiar strukturalnych")](understanding-sirikit-images/monkeychat05.png#lightbox)

Celem strukturalnych będzie zawierać następujące informacje:

- **Domena:** wiadomości
- **Opcje:** sendMessage
- **Odbiorca:** Bobo
- **Zawartość:** Wyślij bananów więcej

W każdej domenie jest jako zestaw znać *akcje* który można wykonać w nich i oparte na domenie i akcji, mogą zostać dołączone do wielu parametrów celem zero wysyłane do aplikacji.

Celem są następnie wysyłane do rozszerzenia aplikacji do przetwarzania. W związku z tym przetwarzania zamiar, aplikacja wygeneruje **IntentResponse** co dołączany zamiar i obejmują parametry opisujące aplikację aplikacji czy przy użyciu zamiaru.

Obejmuje również każdego IntentResponse **kod odpowiedzi** który informuje Siri Jeśli aplikacja była możliwość wykonania żądania, czy nie. Niektóre domeny są bardzo błędu kody odpowiedzi, które mogą być również wysyłane.

Na koniec będzie zawierać IntentResponse `NSUserActivity` (takich jak te używane do obsługi ręcznie wyłączyć). `NSUserActivity` Będzie służyć do uruchamiania aplikacji, jeśli odpowiedź wymaga je pozostawić w środowisku używanie programu Siri i wprowadź aplikację, aby zakończyć je. 

Używanie programu Siri automatycznie utworzy odpowiedni `NSUserActivity` do Uruchom aplikację i pobranie, w którym użytkownik przerwał pracę w środowisku używanie programu Siri. Jednak aplikacji można podać własne `NSUserActivity` z dostosowane informacje, jeśli jest to wymagane.

Po przetwarzane zamiar i zwrócił odpowiedź na używanie programu Siri aplikacji, następnie stanowi wyniki dla użytkownika (słownie i wizualnie):

[![](understanding-sirikit-images/monkeychat06.png "Wyniki wyświetlane słownie i wizualne")](understanding-sirikit-images/monkeychat06.png#lightbox)

Używanie programu Siri zawiera kilka wbudowanych odpowiedzi interfejsu użytkownika dla każdej z domen, które są dostępne dla aplikacji. Jednak ponieważ MonkeyChat udostępnił opcjonalne rozszerzenie interfejsu użytkownika zamiar, służy do użytkownika w powyższym przykładzie są wyświetlane wyniki konwersacji.

## <a name="the-intent-lifecycle"></a>Konwersji cykl życia

Istnieją trzy główne zadania, które rozszerzenia aplikacji należy wykonać podczas pracy nad lokalizacji docelowych:

[![](understanding-sirikit-images/monkeychat07.png "Konwersji cykl życia")](understanding-sirikit-images/monkeychat07.png#lightbox)

1. Aplikacja musi **rozwiązać** każdego parametru w zdarzeniu. W związku z tym aplikacji wywoła Rozwiąż wiele razy (raz na każdy parametr), a czasami wiele razy dla tego samego parametru aż do aplikacji i użytkownika uzgodnić co to jest wymagany.
2. Aplikacja musi **Potwierdź** można go obsłużyć żądanego zamiar i opisz Siri oczekiwany wynik.
3. Ponadto aplikacja musi **obsługi** celem i wykonaj kroki, aby osiągnąć żądanego wyniku.

### <a name="the-resolve-stage"></a>Na etapie rozwiązanie

Na etapie rozwiązanie pomaga Siri zrozumieć wartości, które użytkownik udostępnił i zapewni, że faktycznie przeznaczone użytkownika co się stanie, jeśli celem jest przetwarzany przez aplikację. 

Ten etap zapewnia również aplikacji na wpływają na zachowanie w Siri podczas konwersacji z użytkownikiem. Aby to zrobić, zapewni aplikacji **odpowiedzi rozwiązania**. Istnieje wiele wstępnie zdefiniowanych odpowiedzi do różnych typów danych, która obsługuje usługę Siri.

Najbardziej typowe rozwiązania odpowiedzi z aplikacji będzie **Powodzenie**, co oznacza aplikacji dopasować określone dane z parametru (takie jak nazwa ekranu użytkownika) do fragmentu informacji bez informacji o.

Może to być razy, gdy aplikacja potrzebuje upewnij się, że danego żądania odpowiada poprawny element informacje, które. W takich przypadkach wyśle **ConfirmationRequired** odpowiedzi, aby zadać pytanie tak lub nie dla użytkownika, takie jak *"Wysyłania wiadomości do Bobo wielki?"*

Może to być innych przypadkach, gdy aplikacja będzie wymagać od użytkownika, z krótką listę opcji. W takim przypadku zapewni aplikacji **Uściślanie** odpowiedzi z listą opcji dwóch do dziesięciu użytkownikowi wybór z takich jak: 

```csharp
Who do you want to message?

* Bobo the Great
* Bobo Jr.
* Little Bobo
```

Używanie programu Siri obsłuży użytkownika dokonywania wyboru, ustnie lub za pomocą Siri interfejsu użytkownika, a wynik zostanie wysłane z powrotem do aplikacji.

W innych przypadkach może nie być wystarczającej ilości informacji dla aplikacji można rozpoznać parametru lub może być zbyt dużo dopasowań do rozwiązania przy użyciu Uściślanie (np. 80 użytkowników Bobo w ich imieniu). W takich przypadkach aplikacja będzie wysyłać **NeedsMoreDetails** odpowiedzi i używanie programu Siri pojawi się monit dokładniej.

Jeśli użytkownik nie poda wartość, która jest wymagana dla tego procesu celem, może wysłać **NeedsValue** odpowiedź ma Siri monit o podanie wartości.

Jeśli aplikacja nie obsługuje wartości udzielanego dla określonego parametru, może wysłać **UnsupportedWithReason** odpowiedzi o podanie przyczyny dlaczego wartości nie są obsługiwane. Używanie programu Siri zostanie następnie Monituj użytkownika o całkowicie nową wartość i podany ich przyczyną jest to wymagane.

Na koniec użyj **NotRequired** odpowiedzi Siri stwierdzić, że aplikacja nie wymaga wartości dla danego parametru. Jeśli użytkownik udostępnia jeden mimo to, po prostu zostanie zignorowany przez Siri.

### <a name="the-confirm-stage"></a>Na etapie Potwierdź

Na etapie upewnij się, ma dwa cele:

- Mówić Siri oczekiwany wynik obsługi celem, dzięki czemu Siri można określić użytkownika, co ma mieć miejsce.
- Zapewnia możliwość wyboru wszystkie stany wymaganych aplikacji może być konieczne wykonanie żądania przedstawiony przez użytkownika, takie jak o wystarczającej ilości oszczędność pieniędzy w banku Aby uregulować płatność do żądanej.

Aplikacja będzie udostępniać **odpowiedzi zamiar** z kroku upewnij się, że powinny zostać wypełnione jako dużej ilości informacji o aplikacji ma dostępna, więc Siri może komunikować się on skutecznie z użytkownikiem.

Oparty na typie domeny i akcji, Siri może monit o potwierdzenie, takich jak przed wysłaniem płatności lub rezerwacji jazdy.

### <a name="the-handle-stage"></a>Na etapie dojścia

Etap obsługi to najważniejszy element pracy z celem, ponieważ jest punkt, w którym aplikacja spełnia żądanie użytkownika, wykonując zadanie, które został poproszony zrobić.

Podobnie jak w fazie upewnij się, aplikacja musi udostępniać te informacje o wynik, jak to możliwe, co Siri można powiązać ten użytkownik. Czasami te informacje będą przedstawić wizualnie lub innym razem Siri będzie po prostu mowy użytkownikowi. 

Czasy mogą wystąpić, gdy aplikacja może wymagać dodatkowego czasu na przetwarzanie danego żądania, takie jak opóźnienia wywołania sieci lub na żywo osoba musi spełnić żądania (na przykład Kończenie i wysyłania kolejności lub zwiększają samochodu do lokalizacji użytkownika). Gdy Siri czeka na odpowiedź od aplikacji, wyświetli oczekiwania interfejsu użytkownika dla użytkownika z informacją o tym, że aplikacja jest przetwarzania żądania.

Najlepiej, jeśli aplikacja musi zapewniać odpowiedzi na używanie programu Siri w ciągu dwóch do trzech sekund co najwyżej. Jeśli aplikacja zna, że danej odpowiedzi będzie trwać dłużej do procesu, musi wysłać **w toku** kod odpowiedzi na używanie programu Siri. Używanie programu Siri poinformuje użytkownika, przetwarza żądanie w tle i będą w dalszym ciągu zrobić nawet wtedy, gdy opuszczą środowiska Siri aplikacji.

## <a name="adding-sirikit-to-the-app"></a>Dodawanie SiriKit do aplikacji

Z SiriKit w systemie iOS 10 Apple utworzył dwóch nowych punktów rozszerzenia:

- **Rozszerzenie intencje** — zapewnia Siri zawartości aplikacji i wykonywanie zadań wymaganych do spełnienia dowolnego obsługiwanych lokalizacji docelowych.
- **Rozszerzenie interfejsu użytkownika intencje** — zapewnia niestandardowego interfejsu użytkownika, który będzie wyświetlany dla zawartości aplikacji wewnątrz Siri. 

Istnieje również interfejs API zapewnienie słów i wyrażeń Siri ułatwiających rozpoznawania w postaci:

- **Słownik aplikacji** -słów i wyrażeń, które są wspólne dla wszystkich użytkowników w aplikacji.
- **Słownik użytkownika** -słów i wyrażeń, które są unikatowe dla użytkownika danej aplikacji.

## <a name="the-intents-extension"></a>Rozszerzenia lokalizacji docelowych

Rozszerzenia lokalizacji docelowych jest odpowiedzialna za obsługę głównego interakcje między aplikacją i używanie programu Siri w następujący sposób:

[![](understanding-sirikit-images/intents01.png "Rozszerzenia lokalizacji docelowych")](understanding-sirikit-images/intents01.png#lightbox)

Rozszerzenie celem może obsługiwać jeden lub więcej lokalizacji docelowych, do deweloperów decyzji o tym, jak chcą zaimplementować SiriKit w aplikacji. Deweloper również można dodać rozszerzenie oddzielne opcje dla każdego zamiar konieczności obsługi.  Inaczej mówiąc, Apple żądań, że deweloper Ogranicz liczbę celem rozszerzenia tak, aby Siri nie ma wiele procesów Otwórz względem aplikacji, które wymagają więcej pamięci i czasu obsługi.

Należy również pamiętać, że rozszerzenie zamiar będą uruchomione w tle, gdy jest aktywny Siri dewelopera. Dzięki temu programowi Siri na aktywnie prowadzenie konwersacji z użytkownikiem podczas nadal komunikacji z rozszerzeniem przetworzyć informacji o żądaniu.

## <a name="privacy-and-security-considerations"></a>Zagadnienia dotyczące zabezpieczeń i prywatności

Apple podjęła dużą środki upewnij się, że użytkownik prywatne informacje są bezpieczne podczas pracy z Siri i jako taki istnieje kilka interakcji, które wymagają od użytkownika zalogowania się na urządzeniu z systemem iOS. Na przykład podczas żądania jazdy lub dokonanie płatności.

Ponadto istnieją określonego zachowania, które mogą chcieć ograniczyć do użytkownika, którego jest się zalogowanym urządzenia aplikacji. W takich sytuacjach mogą żądać aplikacji **ograniczyć gdy zablokowany** zachowanie. Odbywa się przez ustawienie w `Info.plist` pliku.

Lokalne Framework uwierzytelniania jest dostępna dla rozszerzenia zamiar, aplikację można monitowanie użytkownika o dodatkowe uwierzytelnianie informacji, nawet jeśli urządzenie jest już odblokowany.

Na koniec Apple Pay jest dostępna dla rozszerzenia zamiar, więc aplikacja może zakończyć transakcji za pomocą Apple Pay i wbudowane arkusza Apple Pay pojawi się nad interfejsu Siri.

Ponadto Apple chce mieć pewność użytkowników wie, kiedy są wysyłania informacji do 3 aplikacji firm i w związku użytkownika **musi** powiedzieć specyficzna nazwa aplikacji (określoną w polu Nazwa wyświetlana pakietu aplikacji) podczas przesyłania.

Apple zostało zaprojektowane Siri przeprowadzenie fizycznych, płynne konwersacji z użytkownikiem i z tego powodu, nazwę pakietu aplikacji mogą być używane w wielu części mowy, wszędzie tam, gdzie mieścił się sposób naturalny w żądaniu użytkownika.

Jednym z typowych rzeczy, które użytkownicy będą robić jest "verbify" Nazwa aplikacji, innymi słowy, z nazwy aplikacji i używać go jako zlecenie w żądaniu. Na przykład *"MonkeyChat Bobo były one bardzo bananów."*

## <a name="the-intents-ui-extension"></a>Opcje rozszerzenia interfejsu użytkownika

Rozszerzenie interfejsu użytkownika intencje przedstawia informacje o możliwość dostosowania interfejsu użytkownika aplikacji i znakowanie do obsługi Siri i upewnij użytkowników możesz podłączonej do aplikacji. Dla tego rozszerzenia aplikację można przełączyć marki, a także visual i innych informacji do zapisu.

[![](understanding-sirikit-images/intents02.png "Przykładowe dane wyjściowe intencje rozszerzenie interfejsu użytkownika")](understanding-sirikit-images/intents02.png#lightbox)

Zawsze zwraca rozszerzenie interfejsu użytkownika intencje `UIViewController` i dodać niczego potrzeby wewnątrz kontrolera widoku, takie jak wyświetlanie dodatkowe informacje, które wykraczają poza wstępnej reakcji aplikacji. Opcje interfejsu użytkownika można także zaktualizować użytkownika o stanie, długotrwałą zdarzenia, takie jak jak długo trwa jazdy udostępnianie samochodu do ich lokalizacji.

Rozszerzenie interfejsu użytkownika intencje będą zawsze wyświetlane wraz z inną zawartością Siri, taką jak ikona aplikacji i nazwy w górnej części interfejsu użytkownika lub, oparte na zamiar, przyciski (na przykład wysyłanie lub Anuluj) mogą być wyświetlane u dołu.

Istnieje kilka wystąpień, w których aplikacja może zastąpić informacje Siri są wyświetlane użytkownikowi domyślnie w takie jak wiadomości lub mapy, gdzie aplikacji można zastąpić domyślne środowisko z jednym dostosowane do aplikacji.

> [!IMPORTANT]
> W trakcie można dodać elementy interaktywne, takie jak `UIButtons` lub `UITextFields` celem rozszerzenia interfejsu użytkownika `UIViewController`, te są ściśle zabroniony jako zamiar interfejsu użytkownika w nieinterakcyjnym, a użytkownik nie będzie obsługiwać interakcję z nimi.

Jest całkowicie opcjonalne dla aplikacji zapewnić zamiar rozszerzenie interfejsu użytkownika, ponieważ Siri zawiera zestaw domyślnego interfejsu użytkownika dla każdego typu konwersji. Ponadto interfejsy intencje interfejsu użytkownika są dostępne tylko dla niektórych się, że profile, że został uznany za firmy Apple mogą być przydatne dla użytkownika.

## <a name="adding-sirikit-vocabulary"></a>Dodawanie słownictwa SiriKit

Ostatni element wykonawczych SiriKit mieści się w aplikacji, podając wymagane słownika. Wiele aplikacji ma unikatowy sposób opisu informacje użytkownika i unikatowe sposoby od użytkownika podania informacji w aplikacji.

W związku z tym Siri wymaga aplikacji pomocy zrozumienie słów i wyrażeń unikatowy dla aplikacji. Niektóre z tych wyrażeń będzie częścią aplikacji, dzięki czemu każdy użytkownik będzie wiedzieć i ich zrozumienie. Jeszcze inni użytkownicy będą unikatowe dla danego użytkownika aplikacji.

### <a name="app-specific-vocabulary"></a>Słownik określonych aplikacji

Słownik określonych aplikacji definiuje słów i wyrażeń, które będzie znane do wszystkich użytkowników aplikacji, takich jak typy pojazdów lub nazwy ćwiczeń. Ponieważ są to część aplikacji, są zdefiniowane w `AppIntentVocabulary.plist` pliku jako część pakietu głównego aplikacji. Ponadto można lokalizować tych słów i wyrażeń.

Istnieje kilka części do słownika `AppIntentVocabulary.plist` pliku:

- **Przykład aplikacja używa** — te zapewniają zestaw typowe przypadki użycia dla żądań, które użytkownik może wykonać aplikacji. Na przykład: *"Rozpoczynać ćwiczeń MonkeyFit."*
- **Parametry** — te zapewniają zestaw typów niestandardowych parametrów specyficzne dla aplikacji. Na przykład ćwiczeń nazwy aplikacji MonkeyFit. Te obejmują:
    - **Wyrażenie** — zezwala aplikacji na zdefiniowane warunki unikatowy dla aplikacji. Na przykład: "Bananarific" typ ćwiczeń MonkeyFit aplikacji. 
    - **Wymowa** — powoduje wskazówek wymowy Siri jako proste wymowę dla danego frazy. Na przykład "ba nana ri lonej".
    - **Przykład** — zawiera przykład przy użyciu danego frazę w aplikacji. Na przykład *"Start Bananarific w MonkeyFit"*.

Aby uzyskać więcej informacji, zobacz firmy Apple [referencyjnym dotyczącym formatowania pliku słownictwa aplikacji](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/CustomVocabularyKeys.html#//apple_ref/doc/uid/TP40016875-CH10-SW1).

### <a name="user-specific-vocabulary"></a>Słownik określonego użytkownika

Słownik określonego użytkownika będzie zapewnienie słów ani fraz, które są unikatowe dla poszczególnych użytkowników aplikacji. Zapewnia się one w czasie wykonywania z poziomu głównego aplikacji (nie rozszerzeń aplikacji) jako uporządkowany zestaw warunków uporządkowane najważniejszych priorytet użycia dla użytkowników, najważniejsze postanowienia na początku listy.

Spójrz na przykład aplikacji MonkeyChat przedstawionych powyżej. MonkeyChat przechowuje listę wszystkich kontaktów użytkownika, które będą wysyłane Siri za pomocą słownika określonego użytkownika. Również przechowuje listę 10 ostatnich kontaktów, które użytkownik ma messaged i ma zestaw kontaktów ulubionego dla każdego użytkownika. Na przykład ulubionych kontaktów należy na początku naszych użytkownika określonego słownika, następuje ostatnie kontakty następnie rest kontaktów użytkownika.

Następujące typy informacji są obsługiwane przez użytkownika określonego słownika:

- Skontaktuj się z nazwy.
- Nazwy ćwiczeń.
- Nazwy albumów zdjęć.
- Słowa kluczowe zdjęcie.

Jeśli aplikacja opiera się na Książka adresowa systemu iOS, ta informacja jest już dostępne dla Siri aplikacji nie przyniesie podejmowania żadnych działań. Aplikacja musi tylko podać nazwy kontaktów, jeśli aplikacja ma swoją własną unikatową bazę danych, kontaktów.

Podczas projektowania słownictwa, zapewniają tylko wymagane wartości, które użytkownicy wiedzieć, jak i interesujących. Unikaj dostarczanie informacji takich jak numery telefonów i adresów e-mail.

Aplikacja musi zaktualizować Siri niezwłocznie po zmianie słownictwa określonego użytkownika. Użytkownicy są przyzwyczajony do żądania informacji z Siri moment został dodany do swoich urządzeń z systemem iOS. Na przykład: Jeśli użytkownik dodaje nowy kontakt w aplikacji, jak użytkownik zapisuje go wysłać do Siri tych informacji.

Co więcej, aplikacja _musi_ usunąć informacji z słownictwa Siri niezwłocznie, ponieważ użytkownik może stać się zakłócać, jeśli one usunąć informacji, ale Siri był nadal rozpoznać go godzin lub dni później.

> [!IMPORTANT]
> Aplikacja powinna usunąć wszystkie słownictwa określonego użytkownika z Siri Jeśli użytkownik wybierze opcję resetowania aplikacji lub ich wylogowania.

## <a name="sirikit-permissions"></a>Uprawnienia SiriKit

Ostatni element SiriKit skupia się na uprawnienia. Podobnie jak przy użyciu innych funkcji systemu IOS (takie jak zdjęcia, aparat fotograficzny lub kontaktów), użytkownicy muszą udzielić uprawnienia jawne dla aplikacji komunikował się Siri.

Aplikacja jest zapewnienie definiującym, jakie informacje będą umożliwiają używanie programu Siri i podaj przyczynę co dlaczego użytkownikowi należy udzielić dostępu. 

Apple sugeruje się, że aplikacja powinien zażądać uprawnień użytkownika do używania Siri pierwszym uruchomieniu aplikacji po one uaktualnione do systemu iOS 10. Jest to, dzięki czemu użytkownicy wiedzieć o integracji Siri i można wstępnie zatwierdzonych użycia przed wprowadzeniem ich pierwsze żądanie.

## <a name="sirikit-and-maps"></a>SiriKit i map

SiriKit jest integralną częścią systemu IOS i korzysta z większą framework intencje dodane do systemu iOS 10. W ramach opcji została zaprojektowana w celu udostępniania typowe i udostępnionych działań i profile z innymi składnikami systemu.

Framework intencje wykracza poza właśnie Siri integracji i udostępnia inne funkcje, takie jak kontakty integracji, gdzie aplikacja może stać się telefonii domyślne lub aplikacji obsługi komunikatów dla określonego kontaktów. Opcje udostępniają głęboką integrację z CallKit użytkownikom najlepsze środowisko VOIP.

Mapy aplikacji w systemie iOS 10 dodano funkcje, takie jak jazdy udostępniania, gdy użytkownik może zarezerwować jazdy bezpośrednio wewnątrz mapy interfejsu użytkownika. SiriKit zapewnia wspólny punkt rozszerzenia przy użyciu map, innych opcji udostępniania (i innych) może być współużytkowana Siri i map. 

Oznacza to, że jeśli aplikacja przyjęła rozszerzenia SiriKit, również uzyska integracji mapy bezpłatnie. 

## <a name="designing-a-great-siri-experience"></a>Projektowanie wspaniały Siri

Projektowanie środowisko użytkowników podczas integracji aplikacji na używanie programu Siri różni się od projektowanie wspaniałych aplikacji interfejsu użytkownika. W przeciwieństwie do normalnej sytuacje, gdy użytkownik prowadzi interakcję z aplikacji bezpośrednio na ekranie, korzystając z Siri są wielokrotnie, gdy interfejs visual nie jest widoczna na wszystkich. Na przykład gdy użytkownik rozpoczął konwersacji z *"Witaj Siri"*.

### <a name="how-siri-helps-the-developer"></a>Jak Siri pomaga dewelopera

Podczas projektowania aplikacji w interakcje z Siri, aplikacja zostanie tworzenie *konwersacji interfejsu*, co oznacza kontekst jest pochodną konwersacji, który ma Siri z użytkownikiem w imieniu aplikacji.

W przypadku braku odwołanie w visual użytkownik musi zachować informacje o z informacjami w ich head. W związku z tym Siri przedstawia systemu od zera minimalne informacje wymagane do wykonania zadania się, że użytkownik jest chcą wykonywać.

Interfejs konwersacji jest w kształcie pytania i odpowiedzi z użytkownika i używanie programu Siri podczas konwersacji. Dlatego ważne jest można traktować jak Siri zadaje pytania i odpowiedzi podczas projektowania tego interfejsu.

Następujący przykładowy użytkownika, tworząc komunikat, Siri może odpowiedzieć na pytanie *"Gotowy do wysłania?"* . Użytkownik może odpowiedzieć na wiele sposobów, takich jak *"Wyślij ją"*, *"Anuluj"* lub nawet coś całkowicie niezwiązanych ze sobą na to pytanie. Niezależnie od tego, jak konwersacji odtwarzana Siri będzie jego obsługa aplikacji, a jedynie wysłać informacje po jej udostępnieniu.

Istnieje kilka różnych sposobów, czy użytkownik może inicjować konwersację z Siri:

- Przy wybieraniu urządzenie, naciskając przycisk głównej. W takiej sytuacji Siri przedstawia kilka interfejsów visual i mniej ustne odpowiedzi.
- Mówiąc *"Witaj Siri"* i uruchamianie ręce wolnego konwersacji. W takiej sytuacji Siri będzie mniej visual i bardziej ustne.
- Korzystanie z funkcji ułatwień dostępu, takich jak połączenia bluetooth włączone aparaty słuchowe gdzie dostosowywane interfejsu użytkownika dla użytkownika ze specjalnymi potrzebami.
- Za pomocą samochodu odtwarzania, gdy użytkownik musi zachować ich uwagi koncentruje się na kierowania przy zachowaniu odwracania uwagi do minimum.

### <a name="how-the-developer-helps-siri"></a>Jak dewelopera pomaga Siri

Podczas integracji aplikacji z Siri, deweloper musi Testowanie integracji to często i zapewnić ich wprowadzania wiele różnych żądań przez pytania o tym samym elemencie informacji lub zadanie jako wiele różnych sposobów, jak to możliwe.

Ponieważ nie dwa osoby Pomyśl podobne, jest krytyczny, że deweloper pobiera tyle testerów różnych wersji beta jak to możliwe, aby ułatwić Dostosowywanie integracji Siri. Użytkownicy mogą zażądać informacji lub Utwórz żądania w sposób dewelopera nigdy nie programu i to dostrajanie pozwala zagwarantować, że maksymalnego komfortu obsługi przy użyciu swoich aplikacji z Siri najszerszych grupy użytkowników.

Testowanie w środowisku i w różnych sytuacjach. Inicjowanie konwersacji z Siri we wszystkich metod można zapewnić, że konwersacje te pozostają płynu i fizycznych. Testu w lokalizacji, w którym użytkownik będzie więcej niż przy użyciu aplikacji, takich jak w siłowni wiele.

Upewnij się, aplikacja jest zapewnienie wszystkie informacje Siri musi poprawnie reprezentują żądaniu i wyniku dla użytkownika. Jest to szczególnie istotne, podczas korzystania Siri w ręce wolnego sytuacji.

### <a name="siri-design-guidelines"></a>Wytyczne dotyczące projektowania Siri

Zawsze należy pamiętać, że używanie programu Siri występują konwersacji z użytkownikiem w imieniu aplikacji. Deweloper chce nie wiesz, że ta konwersacją pozostaje jako płynne i fizycznych, jak to możliwe.

Tak samo jak z ważne konwersacji, deweloper musi sprawdzić, czy:

- Czy aplikacja jest gotowy do konwersacji.
- Czy aplikacja nasłuchuje dokładnie co użytkownik próbuje wykonać.
- Czy aplikacja prosi odpowiednie pytania w odpowiednim czasie.
- Czy aplikacja odpowiada na żądanie użytkownika jest wyszukiwanie informacji.

#### <a name="preparing-for-the-conversation"></a>Przygotowywanie do konwersacji

Przede wszystkim pamiętać jest aplikacji użytkownicy nie mają być dokładnie takie same jak dewelopera. Może być pochodzą z różnych tła mowy w różnych językach lub mieć specjalnymi potrzebami podczas pracy z aplikacją.

Ponadto ponieważ dewelopera zaprojektowany i zbudowany aplikacji, znają głęboko, jednorodnej zarówno aplikacji przebiega i funkcje, które nie mają typowy użytkownik. Dlatego deweloper może poprosić o żądania Siri inaczej niż zwykłego użytkownika.

Jest to, dlaczego kluczowe znaczenie mają zgodnie z wielu różnych osób, jak to możliwe, interakcji z aplikacji za pomocą Siri. Użytkownicy mogą wprowadzać żądań aplikacji za pomocą Siri, który dewelopera nigdy nie ma traktować z lub w sposób, który nie należy wziąć pod uwagę dewelopera.

#### <a name="ensure-the-app-is-a-good-listener"></a>Upewnij się, że aplikacja jest dobrym odbiornika

Deweloper musi Sprawdź, czy aplikacja jest dobrym odbiornik i otrzymuje charakterystykę konwersacji, które spełniają oczekiwań użytkownika. Ale jest także możliwe, że ich nie podano informacje, czy aplikacja wymaga, aby osiągnąć żądanego zadania.

Istnieje kilka sposobów, że aplikacja może obsłużyć tej sytuacji:

- **Wybierz dobrej domyślne wartości Brak** — na przykład innych aplikacji do udostępniania mogą domyślnie bieżącą lokalizację użytkownika, jeśli nie określono, gdzie chcieli zostać pobrana z.
- **Oszacować** — przy użyciu określonych informacji zebrał aplikacji użytkownika, aplikacja będzie mógł marka i świadome wynik brakujących informacji, takich jak wypełnianie Brak numer telefonu komórkowego z informacje kontaktowe użytkownika. Należy jednak zadbać o uniknąć zły niespodzianki, takich jak pobrania najdroższych opcji itp.
- **Prompt, aby uzyskać więcej informacji** — aplikacja może mieć Siri Monituj użytkownika o brakujących wartości. Jednak klucz, w tym miejscu jest utrzymywanie konwersacje proste i do punktu. Użytkownicy będą szybko stać się sfrustrowani, jeśli mają odpowiedzi na kilka pytań do osiągnięcia ich żądania.
- **Mylne informacje bezpiecznie obsłużyć** — użytkownik może podać wartość aplikacji nie oczekiwano lub który nie obsłuży w danym kontekście. Upewnij się, że aplikacja dotyczy takiej sytuacji do użytkownika w sposób, który pozwala ułatwić ich naprawić.

Gdy aplikacja zostanie zaprezentowany pojedynczą wartość, która jest zagrożona, preferowany sposób obsługi to ma Siri poprosić użytkownika o potwierdzenie. Na przykład *"Czy chodziło Bobo wielki?"* , które mogą odpowiedzieć proste odpowiedzi tak lub nie.

W przypadku sytuacji, w której kilka możliwych wyborów może być poprawne dla pojedynczej wartości Uściślanie jest metoda obsługi preferowany. W takiej sytuacji Siri można monitować użytkownika z maksymalnie dziesięciu możliwe opcje do wyboru. Na przykład:

```csharp
Who do you want to send the message to?

* Bobo the Great!
* Bobo Jr.
* Little Bobo
```

Jeśli nadal pytanie, mieć Siri Monituj użytkownika o podanie odpowiedzi całkowicie nowe, bardziej specyficzne dla danej wartości.

#### <a name="request-final-confirmation"></a>Ostateczne potwierdzenie żądania

Przed aplikacji faktycznie wykonuje zadanie do spełnienia żądania użytkownika, Siri będzie sprawdzać za pomocą rozszerzenia aplikacji upewnij się, że wszystko jest już na miejscu. Na przykład użytkownik ma za mało pieniędzy swoje konta, aby uregulować płatność do żądanego?

Ponadto aplikacja musi upewnij się, że zapewnia wszystkie informacje dotyczące możliwych do Siri, można przedstawić go do użytkownika i Potwierdź, że zadanie zamierza wykonać spełnia ich oczekiwania.

Gdy użytkownik został potwierdzony żądania i aplikację wykonał go, aplikacja musi ponownie upewnij się, że dostarczonej wszystkie wyniki do Siri, można powiązać je z użytkownikiem.

#### <a name="responding-to-the-request"></a>Odpowiada na żądanie

Używanie programu Siri ma kilka wbudowanych interfejsów użytkownika dla każdej z domen i akcji, które bez informacji o. Jednak w odpowiednich przypadkach — składnika aplikacji zapewniają rozszerzenia niestandardowego interfejsu użytkownika zamiar wzbogacić środowisko użytkownika z uwzględnieniem znakowania aplikacji i interfejsu użytkownika lub więcej informacji, nie było obecne w żądaniu.

Inaczej mówiąc, ograniczeń stosuje się podczas projektowania niestandardowych interfejsów Siri. Zazwyczaj użytkownik jest pożądane uzyskać konkretne zadanie wykonywane tak szybko, jak to możliwe, a nie chcesz zostać przeciążony niepotrzebnych informacji.

Należy również zwrócić uwagę zapewnienie wygląda niestandardowego interfejsu użytkownika i odpowiada prawidłowo we wszystkich urządzeń z systemem iOS różnych i orientacji, czy użytkownik może mieć lub można używać urządzeń.

Gdy jest to konieczne, za pomocą interfejsu API SiriKit Ukryj wszystkie nadmiarowych jeszcze nie istnieje w domyślnym Siri interfejsu użytkownika. Jeszcze upewnij się, że aplikacja jest nadal zapewnienie informacje Siri tak go można prezentować słownie w sytuacjach ręce wolnego.

Mogą wystąpić sytuacje, w którym Siri będzie uruchomić aplikację w celu spełnienia żądania użytkownika, takie jak prezentacji zdjęć, które użytkownik zgłosił żądanie. W takich przypadkach nie niespodzianek użytkownika. Wyświetlanie oczekiwane informacje bez konieczności pośrednich kroków lub dalszej interakcji ze strony. Nigdy nie zawierają informacje lub wykonać zadania, użytkownik nie jest oczekiwana.

### <a name="polishing-the-design"></a>Wygładzanie projektu

Istnieje kilka czynności, które sugeruje firmy Apple do Polski projektowania interfejsów konwersacji. Jest najpierw, zapewniając zrozumiałe i zwięzłe słownictwa i użyj przypadków przykłady Siri.

Jednym ze sposobów, że użytkownik odnajduje aplikacji jest inicjując konwersację z Siri oraz z zapytaniem, *"Co można zrobić?"* Używanie programu Siri będzie przedstawiał kilka różnych rzeczy, można go wykonać, łącznie z projektanta aplikacji oraz bohater przykład zastosowań, zapewniającą za pośrednictwem jego `plist` pliku.

Jak napisać przypadki użycia dobrym przykładem:

- Upewnij się, przykłady nazwy aplikacji.
- Zachowaj przykład krótko- i -punkt.
- Podaj wiele przykładów dla każdej lokalizacji docelowych, które obsługuje aplikację.
- Priorytet zarówno intencje i przykłady w nich oparte na najbardziej typowe przypadki użycia aplikacji.
- Upewnij się, że aplikacja będzie zlokalizowanych przykłady.
- Upewnij się, że każdy przykład podane działa zgodnie z oczekiwaniami w aplikacji.
- Uniknąć adresowania w przykładach, więc nie zawierają tekst, takich jak Siri *"Witaj Siri..."*
- Takie jak uniknąć wszelkie niepotrzebne pleasantries *"Proszę"* lub *"Dziękujemy"*.

Poświęć chwilę odpowiednie, aby eksplorować i wypróbuj jak aplikacja może wykorzystywać konwersacji, które występują Siri z użytkownikiem w jej imieniu. Upewnij się, że Porozmawiaj z typowych użytkowników całego procesu jako ich interakcji z i oczekiwań aplikacji mogą ulec zmianie.

Należy pamiętać o do testowania aplikacji w różnych sytuacjach i wszystkie różnych metod do wywołania konwersację z Siri. Testu w lokalizacji rzeczywistych użytkownik może korzystać z aplikacji pakietu office i technicznej.

Dokładamy wszelkich starań konwersacji z Siri (w imieniu aplikacji) płynu naturalnych i "możesz po prostu prawa". 

## <a name="summary"></a>Podsumowanie

W tym artykule ma opisano podstawowe pojęcia musieli używać SiriKit i wskazują, że może współdziałać z aplikacji platformy Xamarin.iOS do świadczenia usług, które są dostępne dla użytkownika za pomocą aplikacja map i używanie programu Siri na urządzeniu z systemem iOS.




## <a name="related-links"></a>Linki pokrewne

- [Przykładowe ElizaChat](https://developer.xamarin.com/samples/monotouch/ios10/ElizaChat/)
- [Przewodnik programowania w języku SiriKit](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/index.html)
- [Odwołanie do lokalizacji docelowych platformy](https://developer.apple.com/reference/intents)
- [Odwołanie do lokalizacji docelowych interfejsu użytkownika platformy](https://developer.apple.com/reference/intentsui)
