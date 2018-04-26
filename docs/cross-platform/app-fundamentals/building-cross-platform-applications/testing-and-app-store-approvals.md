---
title: Część 6 - testowania i zatwierdzeń sklepu z aplikacjami
ms.prod: xamarin
ms.assetid: 46E0578A-7EB9-C105-ABB0-A043E501F36B
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: efe0f20207f6e4ec990af736f1d8e930445e59b9
ms.sourcegitcommit: dc882e9631b4ed52596b944a6fbbdde309346943
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/26/2018
---
# <a name="part-6---testing-and-app-store-approvals"></a>Część 6 - testowania i zatwierdzeń sklepu z aplikacjami

## <a name="testing"></a>Testowanie

Wiele aplikacji (nawet aplikacji systemu Android, na niektóre sklepy z), należy przekazać proces zatwierdzania przed ich opublikowaniem; Dzięki testowania jest szczególnie ważne, aby upewnić się, aplikacja osiągnie rynku (wspominając już zakończy się pomyślnie z klientami). Testowanie może mieć wiele form, z poziomu projektanta jednostki testy w celu zarządzania testowania wersji beta w różnych sprzętu.

### <a name="test-on-all-platforms"></a>Test na wszystkich platformach

Brak niewielkich różnic między .NET obsługuje na systemu Windows phone, tablecie i urządzeń komputerowych, a także ograniczenia w systemie iOS, które uniemożliwią dynamiczny kod zostanie wygenerowany w locie. Albo plan testów kod na wielu platformach podczas opracowywania, lub zaplanowanie czasu zrefaktoryzuj i zaktualizować model część aplikacji na końcu projektu.

Zawsze jest dobrym rozwiązaniem w emulatorze/symulatorze umożliwia testowanie wielu wersji systemu operacyjnego, a także możliwości/konfiguracji różnych urządzeń.

Należy również przetestować na tyle urządzenia innego sprzętu fizycznego, jak to możliwe.

#### <a name="devices-in-cloud"></a>Urządzenia w chmurze

Przenośne ekosystemu Telefon i tablet rośnie cały czas, uniemożliwiając test coraz większej liczby dostępnych urządzeń. Aby rozwiązać ten problem, liczbę usług oferuje możliwość zdalnego sterowania wiele różnych urządzeń, dzięki czemu aplikacje można instalować i przetestowane bez konieczności inwestowania bezpośrednio w partiach sprzętu.

[Test aplikacji Centrum](https://docs.microsoft.com/appcenter/test-cloud/preparing-for-upload/uitest) oferuje łatwe testowanie iOS i Android aplikacji na kilkuset różnych urządzeń.

### <a name="test-management"></a>Zarządzanie testami

Podczas testowania aplikacji w obrębie organizacji lub zarządzania programu w wersji beta z użytkowników zewnętrznych, istnieją dwa problemy:

- **Dystrybucji** — Zarządzanie procesu udostępniania (szczególnie w przypadku urządzeń z systemem iOS) i uzyskiwanie zaktualizowane wersje oprogramowania do testerów.
- **Opinie** — zbieranie informacji na temat korzystania z aplikacji i szczegółowe informacje na wszelkie błędy, które mogą wystąpić.


Istnieje wiele pomocy usługi w celu rozwiązania tych problemów, zapewniając infrastrukturę, która jest wbudowana w aplikacji do zbierania i raportowania w przypadku użycia i błędów i również usprawnienie procesu udostępniania pomoc rejestracji i zarządzania ich urządzeniami i testerów .

[Visual Studio aplikacji Centrum](/appcenter/) oferuje rozwiązanie tych problemów, dystrybucji wersji testu, raportowania awarii i informacje o użyciu rozbudowanych aplikacji.

### <a name="test-automation"></a>Automatyzacja testów

Xamarin [UITest](https://docs.microsoft.com/appcenter/test-cloud/preparing-for-upload/uitest) może służyć do tworzenia interfejsu użytkownika zautomatyzowanych skryptów testu, które można uruchomić lokalnie lub przekazane do [aplikacji Centrum testowania](https://docs.microsoft.com/appcenter/test-cloud/).

## <a name="unit-testing"></a>Testowanie jednostkowe

### <a name="touchunit"></a>Touch.Unit

Xamarin.iOS obejmuje strukturę testowania jednostkowego o nazwie Touch.Unit następującego JUnit/NUnit styl pisania testów.

Odwoływać się do naszej [testów jednostkowych z Xamarin.iOS](~/ios/deploy-test/touch.unit.md) dokumentację, aby uzyskać więcej informacji dotyczących pisania testów i systemem Touch.Unit.

### <a name="andrunit"></a>Andr.Unit

Jest odpowiednikiem Touch.Unit dla systemu Android o nazwie Andr.Unit open source. Możesz pobrać go z [github](https://github.com/spouliot/Andr.Unit) i przeczytaj informacje o narzędziu na [ @spouliotw blogu](http://spouliot.wordpress.com/2011/10/30/andr-unit-joins-the-family/).

## <a name="app-store-approvals"></a>Zatwierdzenia sklepu z aplikacjami

Firmy Apple i Microsoft działać magazynie tylko na platformach ich: App Store i Marketplace odpowiednio. Zarówno blokowanie swoje urządzenia i wdrożenie procesu przeglądu rygorystyczne aplikacji do kontroli jakości aplikacje dostępne do pobrania. Otwórz charakter firmy android oznacza, że istnieją różne opcje magazynu od firmy Google Play, który jest powszechnie dostępna i ma żaden proces przeglądu do sklepu z aplikacjami firmy Amazon dla systemu Android i specyficzne dla sprzętu działań, takich jak aplikacje Samsung, które mają bardziej ograniczone dystrybucji i wdrożenie procesu zatwierdzania.

Oczekiwanie na aplikację, należy sprawdzić może być stresujące — naciskom firm często oznacza, że wnioski o zatwierdzenie bardzo mało marginesu błędu wcześniejsza od daty uruchamiania "docelowej". Sam proces może potrwać do dwóch tygodni, a nie zawsze przezroczysty: Istnieje ograniczona opinie o postępie aplikacji dopóki zostanie ostatecznie odrzucone lub zatwierdzone. Odrzucenia oznacza brak okno marketingu możliwości, zwłaszcza, jeśli występuje więcej niż jeden raz i przekazać tygodni między datą oryginalnego uruchomienia i, jeśli aplikacja jest ostatecznie zatwierdzone.

### <a name="be-prepared"></a>Można go przygotować

Pierwszy element porady: wyprzedzeniem zaplanować uruchamianie aplikacji i uwzględnienie możliwości odrzucenia i ponowne przesłanie. Każdy magazyn należy utworzyć konto przed przesłaniem aplikacji — tym możliwie jak najszybciej.
Podczas zapisywania Google Play tylko zajmuje kilka minut, jeśli aplikacje będą wolne, proces pobiera znacznie więcej wysiłku, jeśli ich sprzedaży i trzeba podać bankowości i informacjami. Firmy Apple i procesy firmy Microsoft są zarówno bardziej zaangażowany od firmy Google, może upłynąć tygodnia lub więcej, aby utworzyć swoje konto zatwierdzone tak uwzględnić teraz planów uruchamiania.

Po zatwierdzeniu Twoje konto, możesz przystąpić do przesyłania aplikacji. Rzeczywisty proces do przesyłania aplikacji zostało opisane w następującej dokumentacji:

- [Publikowanie w sklepie iOS App Store firmy Apple](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)
- [Przygotowywanie aplikacji do sklepu Google Play](~/android/deploy-test/publishing/publishing-to-google-play/index.md)
- Odwiedzić deweloperów systemu Windows [Centrum deweloperów systemu Windows](https://developer.microsoft.com/windows/windows-apps) informacji o przesyłanie ich aplikacji.

W pozostałej części tej sekcji omówiono czynności, które należy wziąć pod uwagę, aby upewnić się, że aplikacja została zatwierdzona bez żadnych hiccups.

### <a name="quality"></a>Jakość

Wydaje się oczywista, ale aplikacje często zostaną odrzucone, ponieważ nie spełniają pewnego poziomu jakości: po wszystkich jest to powód, dlaczego wyselekcjonowanych magazynów ma procesu zatwierdzania w pierwszej kolejności!

Awarie są Częstą przyczyną odrzucenia. Jeśli jest zbyt łatwe awarii Twojej aplikacji ma gwarancji odrzucane. Większość deweloperów nie przesyłaj swoje aplikacje przy założeniu, że będą one awarii, ale często nie. Dokładnie przetestuj aplikację przed przesłaniem go, koncentrujących się nie tylko w upewnieniu się, że wszystko działa, ale także że obsługi typowych scenariuszy przenośnych błąd, takie jak problemy z siecią i ograniczenia zasobów, takich jak pamięć lub miejsce do magazynowania. Użyj symulator i fizyczne urządzenia do testowania — niezależnie od tego, jak kod jest uruchamiany w symulatorze, tylko urządzenia wykaże rzeczywistą wydajność aplikacji. Użyj jako wiele różnych urządzeń, jak znaleźć i zarejestrować zespół testerzy wersji beta, jeśli można — usług innych firm może ułatwić zarządzanie, rozkład beta i przesyłania opinii.

Wszystkie systemy operacyjne przenośnych będą kill aplikacji, która nie uruchamia się wystarczająco szybko. Czas dozwolony jest różny, ale ogólnie aplikacje powinny mieć na celu odpowiadać za kilka sekund i użyć zadania w tle w pracy, który będzie trwać dłużej. Aplikacje, które trwa zbyt długo, aby załadować lub nie odpowiada wystarczająco regularnie używane są zostanie odrzucone. Zawsze dostarczaj opinie użytkowników, gdy coś jest wykonywane w tle lub aplikacja pojawi się do awarii i jeszcze raz odrzucane.

### <a name="check-your-edge-cases"></a>Sprawdź z przypadków krawędzi

Typowe pułapki dla deweloperów nie powiodło się przypadkami krawędzi, zwłaszcza tych, które wymagają ponownego konfigurowania ich symulator lub urządzenie do testowania poprawnie. Można go łatwo zapomnieć nie każdy klient będzie "Zezwalaj" aplikację, aby uzyskać dostępu do ich lokalizacji, ponieważ po deweloperów raz przyjęła żądanie, ich nigdy nie pojawi się ponownie. Uprawnienia i użycie sieci w szczególności są koncentruje na podczas procesu zatwierdzania, co oznacza, że małych nadzoru w tych obszarach może spowodować odrzucenia.

Poniższa lista jest dobry punkt wyjścia do sprawdzania przypadków krawędzi, które mogły zostać pominięte:

- **Klienci mogą "Odrzuć" dostęp do usług** — szczególnie w systemach iOS, dostęp do danych, takie jak lokalizacja geograficzna informacji jest podawana tylko wtedy, jeśli użytkownik przyznaje uprawnienia do aplikacji. Testerów aplikacji często należy ponownie zainstalować tę aplikację w początkowym stanie i nie zezwalaj na wszystkie żądania uprawnień, aby upewnić się, że aplikacja działa prawidłowo. Włączenie uprawnień i Wyłącz można zweryfikować poprawne zachowanie, jak klienci zmienić zdanie.
- **Klienci są wszędzie** — nie wolno zakładać, że aplikacja będzie można używać tylko w miasta lub kraju, w którym został opracowany! Kod, który współpracuje z współrzędne GPS, wartości daty i godziny i waluty ma wpływ lokalizacji klienta i ustawień regionalnych. Wszystkie oferty platform symulator, które pozwalają określić różne lokalizacje i ustawienia regionalne - go użyć do przetestowania lokalizacje, w innych hemispheres oraz które formatowania daty i waluty inaczej. Wartości współrzędne geograficzne mogą być dodatnie lub ujemne, separator dziesiętny może być okres lub przecinkami i dat może być sformatowany dużą ilość sposobów - zająć!
- **Wolne połączenia sieciowe** — deweloperzy aplikacji często pracować "idealne rozwiązanie w świecie" szybkie, zawsze pracy łączność oczywiście nie jest w świecie rzeczywistym. Testowanie połączenia powolnej sieci (takich jak niska połączenia sieci 3 G), a także bez dostępu do sieci jest kluczowa zapewnienie, że nie wysyłasz buggy aplikacji. Proces zatwierdzania będzie zawsze należy uwzględniać testu z urządzeniem w tryb samolotowy, dlatego upewnij się, że zostały przetestowane który dla siebie.
- **Zmienia się sprzętu** — Pamiętaj, aby przetestować na sprzęcie najstarsze, najwolniejsze, który ma być obsługiwana. Istnieją dwie kwestie, które mogą wpływać na aplikację: wydajności, które mogą być niezdatna do użycia na starszych urządzeń i obsługę funkcji sprzętu, takich jak aparatu fotograficznego, mikrofonu, GPS, żyroskop lub innych składników opcjonalnych. Aplikacje powinny pogorszyć bezpiecznie (i nie awaria) gdy składnik jest niedostępny.

### <a name="guidelines-are-more-than-just-a-guide"></a>Wytyczne mają więcej niż tylko "Przewodnik"

Apple jest Słynne jest strict o przestrzeganie ich Human Interface Guidelines zgodnie z jedną z kluczowych możliwości platformy jest spójności (i postrzegana wzrost użyteczność). Microsoft podjęła podejście podobne z implementacji interfejsu użytkownika aplikacje w stylu aplikacji systemu Windows. Proces zatwierdzania dla obu platform będzie obejmować oceniane pod kątem przestrzegania zasady klas związanych z projektowaniem aplikacji.

Która nie jest znaczy innowacji interfejsu użytkownika nie jest obsługiwany lub zachęca, ale pewne zagadnienia "właśnie nie zrobisz" czy aplikacji zostaną odrzucone.

W systemach iOS w tym niewłaściwie korzysta z wbudowanej ikony lub przy użyciu innych utrwalonego metafory w sposób zgodny z systemem innym niż; na przykład za pomocą ikony "tworzą" do wszelkich innych niż tworzenia nowej zawartości.

Deweloperzy systemu Windows należy podobnie ostrożnie; powszechnym błędem nie powiodło się poprawnie obsługuje wstecz sprzętu przycisku zgodnie z wytycznymi firmy Microsoft.

Zachęca Twojej projektantów do odczytywania i postępuj zgodnie z wytycznymi projektowania dla każdej platformy.

### <a name="implementing-platform-specific-features"></a>Implementowanie funkcji specyficznych dla platformy

Elementy są nieco bardziej restrykcyjne, jeśli chodzi o implementacji usług specyficzne dla platformy, szczególnie w systemie iOS. Aby uniknąć automatycznego odrzucenia przez firmę Apple, dostępne są niektóre reguły, które należy wykonać przy użyciu następujących funkcji z systemem iOS:

- **Zakupy w aplikacjach** — aplikacje nie musi wdrożyć mechanizmy zewnętrznej płatności cyfrowe produktów, w tym waluty w grę, funkcji aplikacji, Magazyn subskrypcji i wiele innych. aplikacje dla systemu iOS, należy użyć usługi opartej na iTunes firmy Apple dla tego rodzaju funkcji. Istnieje luka — aplikacje, takie jak czytnika Kindle i niektóre aplikacje oparte na subskrypcji umożliwiają zakup w innym miejscu zawartość, która pobiera dołączony do "konto", który można następnie uzyskać dostęp za pomocą aplikacji, jednak, w tym przypadku nie może zawierać aplikacja łączy lub odwołuje się do poza aplikacji proces zakupu (lub ponownie, będzie odrzucane).
- **Kopia zapasowa iCloud** — pojawienie iCloud firmy Apple osoby dokonujące przeglądu jest znacznie bardziej rygorystycznych jak aplikacje za pomocą magazynu (Upewnij się, przez klienta zdalnego tworzenia kopii zapasowej środowiska jest przyjemne). Aplikacje, że niepotrzebnie miejsce stanie kopii zapasowej może odrzucane, odpowiednio tak użycie folderu pamięci podręcznej i wykonaj firmy Apple do pozostałych wskazówek dotyczących magazynu.
- **Newsstand** — gazecie oraz Magazyn aplikacji są doskonałym rozwiązaniem dla Newsstand firmy Apple, jednak aplikacji musi zawierać co najmniej jeden odnawianie automatycznie subskrypcji i pomocy technicznej tła pobierania do zatwierdzenia.
- **Mapuje** — jest coraz częściej, aby dodać nakładki i inne funkcje do mapy przenośne, jednak należy uważać nie zasłaniać mapy "środków na korzystanie z" informacji (takich jak logo firmy Google w iOS5) jak w ten sposób spowoduje odrzucenie.

### <a name="manage-your-metadata"></a>Zarządzanie metadanych

Oprócz widocznych problemów technicznych, które mogą skutkować aplikacji odrzucane są niektóre aspekty aspektów Twoje zgłoszenie, co może spowodować odrzucenia, szczególnie wokół metadanych (opis, słowa kluczowe i obrazy marketing) który Prześlij z aplikacji do wyświetlenia w sklepie z aplikacjami lub Marketplace.

- **Obrazów** — postępuj zgodnie z wytycznymi platformy dla ikony aplikacji i przechowywania obrazów. Nie używaj chronionym znakiem towarowym obrazów, firma Microsoft w tym samouczku aplikacji Pobierz odrzucone, ponieważ ich ikony umieszczony rysunek iPhone!
- **Znaki towarowe** — należy unikać używania znakami dowolnego innego niż własny. Odmowa aplikacji dla zauważyć znakami w opisie aplikacji lub nawet w przypadku słowa kluczowe w sklepie z aplikacjami firmy Apple.
- **Opis elementu** — nie należy użyć słowa "beta" lub w jakikolwiek sposób wskazać aplikacji nie jest gotowa do pierwszych czasu. Nie zawierać innych platform urządzeń przenośnych (nawet, jeśli aplikacja jest i platform). Co najważniejsze upewnij się, że aplikacja jest dokładnie co się podaje robi. Jeśli w opisie lista licznych funkcji, ma lepiej go oczywiste sposobu używania każdej z tych funkcji lub otrzymasz odrzucenia "funkcji wymienionych w opisie aplikacji nie jest zaimplementowana".

Umieść jako nakład do metadanych aplikacji jako do projektowania i testowania. Aplikacje odrzucone dla mniejszych naruszeń w metadanych, warto poświęcenie czasu na pobrać go w prawo.

### <a name="app-stores-not-for-everyone"></a>Sklepów z aplikacjami: Nie dla wszystkich użytkowników

Głównym celem magazynów na każdej platformie jest dystrybucja konsumenta — możliwości dotarcia do dowolnej liczby klientów, jak to możliwe. Jednak nie wszystkie aplikacje są stosowane na konsumentów, jest coraz częściej podstawowej aplikacji wewnętrznych i ekstranetu podobne, które wymagają dystrybucji ograniczony do pracowników, dostawców lub klientów. Te aplikacje nie są "na sprzedaż" i nie wymagają zatwierdzenia od czasu dystrybucji formanty developer do zamkniętego grupy użytkowników.
Obsługa tego typu wdrożenia zależy od platformy.

Android zapewnia większą elastyczność w tym zakresie: aplikacje można instalować bezpośrednio z załącznika adres URL lub adres e-mail (o ile urządzenia konfiguracji umożliwia on). Oznacza to, że jest prosta, aby utworzyć i dystrybuować wewnętrznych aplikacji firmowych lub publikowanie aplikacji dla określonych odbiorców lub dostawców.

Apple udostępnia opcję wewnętrznych wdrażania dla deweloperów zarejestrowane w systemie iOS Developer Enterprise Program, który pomija procesu zatwierdzania sklepu z aplikacjami i umożliwia firmom dystrybucji wewnętrznych aplikacji dla użytkowników.
Niestety ta licencja nie usuwa potrzebę dystrybucji aplikacji korzystających z ekstranetu podobne do innych grup zamknięte przez klientów i dostawców. [Enterprise (i Ad Hoc) wdrożenia](~/ios/deploy-test/app-distribution/ipa-support.md)

### <a name="app-store-summary"></a>Podsumowanie sklepu z aplikacjami

Przegląd procesu może być czasochłonnym zadaniem, ale podobnie jak pozostałe cyklu programistycznym może pomóc zapewnić powodzenie z pewnego planowania i dbałość o szczegóły. Wszystkie chodzi w dół o wykonanie kilku prostych kroków: przeczytane i zrozumiane dotyczące interfejsu użytkownika musi być zgodne, zgodne z regułami wdrażania funkcji specyficznych dla platformy, należy dokładnie przetestować (następnie przetestować dalszą) i na koniec upewnij się, metadane aplikacji przed wysłaniem jest poprawna.

Jeden wyraz ostatniej zalecenia dla deweloperów publikowania w sklepie Google Play: Brak procesu zatwierdzania może wydawać się, jak ją ułatwia zadania —, ale klienci będą potrzebują więcej niż zespołu przeglądu. Zgodna z tymi wytycznymi, jako że aplikację można odrzucane, w przeciwnym razie będzie klientów czynności odrzucenia.
