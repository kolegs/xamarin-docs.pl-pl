---
title: "Konteneryzowanych Mikrousług"
ms.topic: article
ms.prod: xamarin
ms.assetid: 5872ad92-04e0-4f1a-9691-79d5602f5683
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 3ecbd5a301a64417ab5fb27bd8632b6d9790a7ac
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
---
# <a name="containerized-microservices"></a>Konteneryzowanych Mikrousług

Tworzenie aplikacji klient serwer spowodowało fokus na tworzeniu aplikacji warstwowych, używające określonych technologii w każdej warstwie. Takie aplikacje są często nazywane *wbudowanymi* aplikacji i są umieszczone na sprzęcie wstępnie skalowania obliczeniowymi godzinami szczytu. Główne wady tego podejścia programowanie są ściśle sprzężenia między składnikami w każdej warstwie, czy poszczególne składniki nie można łatwo skalować, a koszt testowania. Proste aktualizacja może mieć nieprzewidziane skutki na pozostałych warstwy, i dlatego zmiany do składnika aplikacji wymaga jego całą warstwy być powtórnie testowane i wdrożone.

Szczególnie dotyczące poszczególnych składników nie można łatwo skalowania w wieku chmury. Wbudowanymi aplikacji zawiera funkcje właściwe dla domeny i zazwyczaj jest podzielona przez funkcjonalności warstwy frontonu, logika biznesowa i magazynu danych. Wbudowanymi aplikacji jest skalowana w klonowania całej aplikacji na wielu komputerach, zgodnie z opisami w rysunek 8-1.

![](containerized-microservices-images/monolithicapp.png "Podejście skalowania wbudowanymi aplikacji")

**Rysunek 8-1**: wbudowanymi aplikacji odbierającej podejście

## <a name="microservices"></a>Mikrousług

Mikrousług oferują różne podejścia do tworzenia aplikacji i wdrażania, metody, która nadaje się do elastyczność skalowania i wymagania dotyczące niezawodności nowoczesnych aplikacji w chmurze. Aplikacja mikrousług jest rozłożone na niezależne składników, które współpracują ze sobą w celu dostarczenia ogólne działanie aplikacji. Mikrousługi termin zwrócono szczególną uwagę, że aplikacje powinien składać się z usługi duże, aby uwzględnić kwestie niezależne tak, aby każdy mikrousługi implementuje jednej funkcji. Ponadto każdy mikrousługi ma dobrze zdefiniowanego umów, dzięki czemu innych mikrousług może komunikować się i udostępniać dane. Typowe przykłady mikrousług obejmują zakupy wózkach sklepowych, spisu przetwarzania, zakupu podsystemów i przetwarzanie płatności.

Mikrousług można skalowalnego w poziomie niezależnie, w porównaniu do bardzo duże wbudowanymi aplikacji, które razem. Oznacza to, że mogą być skalowane określonego obszaru funkcjonalności, która wymaga więcej przetwarzania zasilania lub sieci przepustowość do obsługi żądanie, zamiast niepotrzebnie skalowania w poziomie innych obszarach aplikacji. Takie podejście, gdzie mikrousług są wdrożone i skalowane niezależnie, przedstawiono na rysunku 8-2 tworzenia wystąpień usług na komputerach.

![](containerized-microservices-images/microservicesapp.png "Podejście skalowania aplikacji Mikrousług")

**Rysunek 8-2**: Mikrousług aplikacji odbierającej podejście

Mikrousługi skalowalnego w poziomie można niemal natychmiastowe, umożliwia dostosowanie do zmieniających się obciążeń aplikacji. Na przykład pojedynczy mikrousługi w funkcji udostępnianych w sieci web aplikacji może być tylko mikrousługi w aplikacji, która musi być skalowana w poziomie do obsługi dodatkowych ruchu przychodzącego.

Klasyczny model skalowalności aplikacji jest równoważeniem obciążenia, bezstanowych warstwa z udostępnionego magazynu danych zewnętrznych do przechowywania danych. Stanowe mikrousług zarządzać własnych danych, zwykle magazynować je lokalnie na serwerach, na których są umieszczane, uniknąć obciążenia sieci dostępu i złożoność między usługą operacji. To umożliwia najszybszym możliwości przetwarzania danych i wyeliminować potrzebę systemy buforowania. Ponadto skalowalnych mikrousług stanowe zwykle partycjonowania danych między ich wystąpienia, przepływność rozmiaru i transferu danych po przekroczeniu których może obsługiwać jeden serwer zarządzania.

Mikrousług obsługuje również niezależnych aktualizacji. Tym luźne powiązanie między mikrousług zapewnia rozwoju aplikacji szybkie i niezawodne. Natury niezależne, rozproszona obsługuje aktualizacji stopniowych, gdy tylko podzestaw wystąpień jednej mikrousługi zostanie zaktualizowana w danym momencie. W związku z tym jeśli zostanie wykryty problem, buggy aktualizacji można wycofać, zanim wszystkie wystąpienia zaktualizować uszkodzony kod lub konfiguracji. Podobnie mikrousług zwykle użyć wersji schematu, tak aby klienci Zobacz spójne wersji podczas aktualizacji są stosowane, niezależnie od tego, które mikrousługi są przesyłane z wystąpienia.

W związku z tym mikrousługi aplikacje mają wiele korzyści za pośrednictwem wbudowanymi aplikacjami:

-   Każdy mikrousługi jest stosunkowo mały, łatwe w zarządzaniu i rozwijać.
-   Każdy mikrousługi można opracowany i wdrażać niezależnie od innych usług.
-   Każdy mikrousługi można niezależnie skalowalnych w poziomie. Na przykład usługi katalogu lub Usługa koszyka zakupów mogą pojawić się w więcej niż porządkowania usługi skalowalnych w poziomie. W związku z tym wynikowy infrastruktury wydajniej zużyje zasobów w trakcie skalowania.
-   Każdy mikrousługi izoluje wszelkie problemy. Na przykład jeśli wystąpi problem w usłudze ich tylko wpływ tej usługi. Inne usługi można kontynuować do obsługi żądań.
-   Każdy mikrousługi służy najnowszych technologii. Ponieważ mikrousług są autonomiczne i uruchom side-by-side, najnowszych technologii i platform można, zamiast wymuszaniu za pomocą starszej architektury, które mogą być używane przez aplikację wbudowanymi.

Rozwiązania na podstawie mikrousługi ma również wady:

-   Wybór sposobu partycji aplikacji do mikrousług może być wyzwaniem, zgodnie z każdym mikrousługi musi być całkowicie autonomiczny, na całej trasie, włączając odpowiedzialność za jej źródeł danych.
-   Deweloperzy muszą implementować komunikacja między usługami komunikacji, który dodaje złożoność i czas oczekiwania do aplikacji.
-   Niepodzielne transakcji między wieloma mikrousług zazwyczaj nie są możliwe. W związku z tym wymagania biznesowe musi obejmować spójność ostateczna między mikrousług.
-   W środowisku produkcyjnym brak złożoność działania, wdrażania i zarządzania systemem, naruszony wiele niezależnych usług.
-   Bezpośrednia komunikacja klient mikrousługi może utrudnić Refaktoryzuj umów mikrousług. Na przykład w czasie jak system jest podzielona na partycje do usług może być konieczna zmiana. Jednej usługi może być podzielony na dwie lub więcej usług, i może scalić dwóch usług. Gdy klienci komunikują się bezpośrednio z mikrousług, to refaktoryzacji pracy mogą być dzielone zgodności z aplikacjami klienta.

## <a name="containerization"></a>Przechowywanie w kontenerach

Przechowywanie w kontenerach jest podejście do rozwoju oprogramowania, w którym aplikacja oraz jej kontrolą wersji zestawu zależności, a także jej konfiguracji środowiska pobieranej jako pliki manifestu wdrożenia, jednym pakiecie jako obraz kontenera przetestowane jako jednostki, i wdrażany system operacyjny hosta.

Kontener jest izolowanym, zasobów kontrolowanych i przenośnych środowisku operacyjnym, gdzie aplikacja może działać bez dotknięcie zasobów innych kontenerów lub hosta. W związku z tym kontenerem wyszukuje i działa jak nowo zainstalowanym komputerze fizycznym lub maszynie wirtualnej.

Istnieje wiele podobieństw między maszynami wirtualnymi i kontenerów, jak pokazano na rysunku 8-3.

![](containerized-microservices-images/containersvsvirtualmachines.png "Podejście skalowania aplikacji Mikrousług")

**Rysunek 8-3**: porównanie maszyn wirtualnych i kontenerów

Kontener ma zainstalowany system operacyjny, ma system plików i jest możliwy za pośrednictwem sieci, tak jakby był on maszyny fizycznej lub wirtualnej. Jednak technologii i pojęć używanych przez kontenery bardzo różnią się od maszyny wirtualnej. Maszyny wirtualne obejmują aplikacje, wymagane zależności i systemu operacyjnego gościa pełna. Kontenery aplikacji i jego zależności, ale udostępniać systemu operacyjnego innych kontenerów, uruchomione jako procesach izolowanych na system operacyjny hosta (jako uzupełnienie kontenery funkcji Hyper-V, uruchamianych wewnątrz specjalną maszyną wirtualną na kontenera). W związku z tym kontenery udostępnianie zasobów i zwykle wymagają mniej zasobów niż maszyny wirtualne.

Zaletą podejście opracowywania i wdrażania zorientowane na kontenera jest wyeliminowanie większości problemów, które wynikają z ustawień środowiska niespójne i problemów, które pochodzą z nimi. Ponadto kontenery umożliwić funkcji skalowania w górę szybkiego aplikacji poprzez wystąpień nowe kontenery zgodnie z wymaganiami.

Kluczowe założenia podczas tworzenia i Praca z kontenerami są:

-   Host kontenera: Fizycznej lub maszyna wirtualna skonfigurowana z kontenerami hosta. Kontener hosta uruchomi kontenerach.
-   Kontener obrazu: Obraz składa się z związek między warstwami systemy plików skumulowany na siebie i jest podstawą kontenera. Obraz nie ma stanu i nigdy nie zmienia się jest wdrożona w różnych środowiskach.
-   Kontener: Kontener jest wystąpieniem środowiska uruchomieniowego obrazu.
-   Kontener obrazu systemu operacyjnego: Kontenery są wdrażane z obrazów. Obraz systemu operacyjnego kontenera jest pierwszą warstwę w potencjalnie wiele warstw obrazu wchodzące w skład kontenera. System operacyjny kontenera jest niemodyfikowalna i nie może być modyfikowany.
-   Repozytorium kontenera: Każdym razem, gdy obraz kontener jest tworzony, obrazu i jego zależności są przechowywane w lokalnym repozytorium. Obrazy te mogą być ponownie używane wiele razy na hoście kontenera. Kontener obrazy mogą też być przechowywane w rejestrze publicznych lub prywatnych, takich jak [Centrum Docker](https://hub.docker.com/), dzięki czemu mogą być używane na hostach w różnych kontenerów.

Podczas implementowania mikrousługi aplikacji opartych na i Docker stał się implementację standardowe kontenera, która została przyjęta przez większość platform oprogramowania i dostawców chmury przedsiębiorstwa coraz wdrażają kontenerów.

Aplikacja referencyjna eShopOnContainers używa Docker do obsługi cztery konteneryzowanych mikrousług zaplecza, jak pokazano na rysunku 8-4.

![](containerized-microservices-images/microservicesarchitecture.png "eShopOnContainers odwołania mikrousług zaplecza aplikacji")

**Rysunek 8-4**: eShopOnContainers odwołania mikrousług zaplecza aplikacji

Architektura usług zaplecza w aplikacji odwołania jest rozłożone na wiele podrzędnych systemach autonomicznych w formie brać mikrousług i kontenerów. Każdy mikrousługi zapewnia jednego obszaru funkcji: tożsamość usługi, usługi katalogu porządkowania usługi i Usługa koszyka.

Każdy mikrousługi ma własną bazę danych, dzięki któremu można całkowicie całkowicie niezależna od innych mikrousług. W miarę potrzeby spójności między bazami danych z różnych mikrousług odbywa się przy użyciu zdarzenia na poziomie aplikacji. Aby uzyskać więcej informacji, zobacz [komunikacji między Mikrousług](#communication_between_microservices).

Aby uzyskać więcej informacji o aplikacji odwołania, zobacz [Mikrousług .NET: Architektura konteneryzowanych aplikacji .NET](https://aka.ms/microservicesebook).

<a name="communication_between_client_and_microservices" />

## <a name="communication-between-client-and-microservices"></a>Komunikacja między klientem i Mikrousług

Aplikacja mobilna eShopOnContainers komunikuje się z pomocą konteneryzowanych mikrousług zaplecza *kierować klientów do mikrousługi* komunikację, co przedstawiono na rysunku 8-5.

![](containerized-microservices-images/directclienttomicroservicecommunication.png "Podejście skalowania aplikacji Mikrousług")

**Rysunek 8-5**: bezpośrednia komunikacja klient mikrousługi

Z bezpośrednia komunikacja klient mikrousługi aplikacji mobilnej wysyła żądania do każdego mikrousługi bezpośrednio za pomocą jego publiczny punkt końcowy, używając innego portu TCP na mikrousługi. W środowisku produkcyjnym punkt końcowy będzie zwykle mapowane na mikrousługi równoważenia obciążenia, który rozdziela żądania między dostępnych wystąpień.

> [!TIP]
> Należy rozważyć użycie interfejsu API bramy komunikacji. Bezpośrednia komunikacja klient mikrousługi mogą mieć wady podczas kompilowania mikrousługi dużych i złożonych aplikacji opartej na, ale jest większe niż odpowiednie dla małych aplikacji. Podczas projektowania mikrousługi dużych aplikacji opartej na zawierające dziesiątki mikrousług, należy wziąć pod uwagę przy użyciu interfejsu API bramy komunikacji. Aby uzyskać więcej informacji, zobacz [Mikrousług .NET: Architektura konteneryzowanych aplikacji .NET](https://aka.ms/microservicesebook).

<a name="communication_between_microservices" />

## <a name="communication-between-microservices"></a>Komunikacja między Mikrousług

Aplikacji na podstawie mikrousług jest systemu rozproszonego, potencjalnie jest uruchomiony na wielu komputerach. Każde wystąpienie usługi jest zwykle procesu. W związku z tym usługi musi współdziałać przy użyciu protokołu komunikacji między procesami, takich jak HTTP, TCP, zaawansowane komunikatów usługi kolejkowania wiadomości protokołu (protokół AMQP) lub binarne protokołów, w zależności od charakteru każdej usługi.

Dwa podejścia typowe dla komunikacji mikrousługi mikrousługi są HTTP na podstawie połączenia REST podczas wykonywania zapytania dotyczącego danych i lekkie asynchroniczne wiadomości podczas komunikacji między mikrousług wielu aktualizacji.

Wiadomości na podstawie sterowane zdarzeniami komunikacji asynchronicznej jest krytyczne, gdy propagowanie zmian w wielu mikrousług. Z tej metody mikrousługi publikuje zdarzenie, gdy coś Godny uwagi spowodowany, na przykład aktualizuje jednostki biznesowej. Inne mikrousług subskrybować tych zdarzeń. Następnie po mikrousługi odbiera zdarzenia, aktualizuje własną jednostki biznesowe, które z kolei może prowadzić do zdarzeń publikowana. To publikacji / subskrypcji funkcji zwykle odbywa się z magistralą zdarzeń.

Magistrala zdarzeń umożliwia komunikację między mikrousług, bez konieczności składniki znać jawnie od siebie, jak pokazano w rysunek 8-6 publikowania / subskrypcji.

![](containerized-microservices-images/eventbus.png "Publikowania / subskrypcji z magistralą zdarzeń")

**Rysunek 8 — 6:** publikowania / subskrypcji z magistralą zdarzeń

Z perspektywy aplikacji magistrali zdarzeń jest po prostu publikowanie-subskrypcji kanału udostępniane za pośrednictwem interfejsu. Jednak sposób jest implementowany przez magistralę zdarzenia może się różnić. Implementacja magistrali zdarzeń można na przykład użyć RabbitMQ, usługi Azure Service Bus lub innych magistrali usług, takich jak NServiceBus i MassTransit. Rysunek 8-7 pokazuje, jak magistrali zdarzeń jest używane w aplikacji eShopOnContainers odwołania.

![](containerized-microservices-images/microservicesarchitecturewitheventbus.png "Komunikacji asynchronicznej sterowane zdarzeniami w aplikacji odwołania")

**Rysunek 8-7:** komunikacji asynchronicznej sterowane zdarzeniami w aplikacji odwołania

Szyny zdarzeń eShopOnContainers implementowane za pomocą RabbitMQ, zawiera jeden do wielu asynchronicznej funkcji publikowania / subskrypcji. Oznacza to, że po opublikowaniu zdarzenia, może istnieć wiele subskrybentów nasłuchiwania dla tego samego zdarzenia. Rysunek 8-9 przedstawiono tę relację.

![](containerized-microservices-images/eventdrivencommunication.png "Komunikacja jeden do wielu")

**Rysunek 8-9**: jeden do wielu komunikacji

Takie podejście komunikacji jeden do wielu używa zdarzenia do implementacji transakcje biznesowe, które obejmują wiele usług, zapewniając spójność ostateczna między usługami. Transakcja ostatecznego spójne składa się z serii kroków rozproszonych. W związku z tym po odebraniu polecenia UpdateUser mikrousługi profilu użytkownika są aktualizowane szczegóły użytkownika w swojej bazie danych i publikuje zdarzeń UserUpdated magistrali zdarzeń. Zarówno mikrousługi koszyka, jak i porządkowania mikrousługi subskrybuje odbieranie tego zdarzenia, a w odpowiedzi aktualizacji informacji o ich nabywców w odpowiednich bazach danych.

> [!NOTE]
> Szyny zdarzeń eShopOnContainers implementowane za pomocą RabbitMQ, jest przeznaczona do użycia tylko jako Weryfikacja koncepcji. Dla systemów produkcyjnych należy rozważyć zdarzeń alternatywnych implementacji magistrali.

Informacje o implementacji magistrali zdarzeń, zobacz [Mikrousług .NET: Architektura konteneryzowanych aplikacji .NET](https://aka.ms/microservicesebook).

## <a name="summary"></a>Podsumowanie

Mikrousług oferują podejście do tworzenia aplikacji i wdrażania, który jest odpowiedni do wymagań elastyczności, skalę i niezawodność nowoczesnych aplikacji w chmurze. Jedną z wielu zalet mikrousług jest, aby mogły być skalowalnych w poziomie niezależnie, co oznacza, że określonym obszarze funkcjonalności mogą być skalowane która wymaga więcej przetwarzania zasilania lub sieci przepustowość do obsługi żądanie, bez niepotrzebnie skalowania obszarów Aplikacja, która nie występuje żądanie zwiększenia.

Kontener jest izolowanym, zasobów kontrolowanych i przenośnych środowisku operacyjnym, gdzie aplikacja może działać bez dotknięcie zasobów innych kontenerów lub hosta. Podczas implementowania mikrousługi aplikacji opartych na i Docker stał się implementację standardowe kontenera, która została przyjęta przez większość platform oprogramowania i dostawców chmury przedsiębiorstwa coraz wdrażają kontenerów.


## <a name="related-links"></a>Linki pokrewne

- [Pobieranie książki elektronicznej (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (sample)](https://github.com/dotnet-architecture/eShopOnContainers)
