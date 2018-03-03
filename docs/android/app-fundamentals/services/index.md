---
title: "Tworzenie usług dla systemu Android"
description: "W tym przewodniku omówiono Xamarin.Android usług, które są składniki systemu Android, które umożliwiają pracy bez interfejsu aktywnego użytkownika. Usługi w bardzo często są używane dla zadań, które są wykonywane w tle, takie jak dużo czasu obliczeń, pobierania plików odtwarzanie muzyki i tak dalej. Opisano różne scenariusze, które usługi są odpowiednie do, a pokazano, jak zarówno do wykonywania długotrwałych zadań w tle, a także udostępnia interfejs dla zdalnego wywołania procedury ich wdrażania."
ms.topic: article
ms.prod: xamarin
ms.assetid: BA371A59-6F7A-F62A-02FC-28253504ACC9
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 77a3db2b36a0fb423ac2481fbde80c1a732cac4b
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="creating-android-services"></a>Tworzenie usług dla systemu Android

_W tym przewodniku omówiono Xamarin.Android usług, które są składniki systemu Android, które umożliwiają pracy bez interfejsu aktywnego użytkownika. Usługi w bardzo często są używane dla zadań, które są wykonywane w tle, takie jak dużo czasu obliczeń, pobierania plików odtwarzanie muzyki i tak dalej. Opisano różne scenariusze, które usługi są odpowiednie do, a pokazano, jak zarówno do wykonywania długotrwałych zadań w tle, a także udostępnia interfejs dla zdalnego wywołania procedury ich wdrażania._

## <a name="android-services-overview"></a>Omówienie usług dla systemu android

Aplikacje mobilne nie są takie jak aplikacje komputerowe. Komputery stacjonarne ma dużą ilością zasobów, takich jak nieruchomości ekranu, pamięci, miejsca do magazynowania i połączone zasilania, urządzenia przenośne nie. Tych warunków ograniczających wymusić aplikacje mobilne można zachowywać się inaczej. Na przykład małego ekranu na urządzeniu przenośnym zazwyczaj oznacza to, że tylko jednej aplikacji (tj. działanie) jest widoczny w czasie. Innych działań są przenoszone do tła i wypychana w stanie wstrzymanym, której nie można wykonać pracę. Jednak wyłącznie z powodu aplikacji systemu Android znajduje się w tle nie oznacza jest niemożliwe dla aplikacji kontynuować pracę. 

Aplikacje systemu android składają się z co najmniej jeden cztery następujące składniki podstawowe: _działania_, _emisji odbiorcy_, _dostawców zawartości_i _Usług_. Działania są podstawy wiele wspaniałych aplikacji systemu Android, ponieważ udostępniają one interfejsu użytkownika, który umożliwia użytkownikowi interakcji z aplikacją. Jednak jeśli chodzi o wykonywanie równoczesne lub Praca w tle, działania nie zawsze są najlepszym rozwiązaniem.
 
Jest podstawowy mechanizm Praca w tle w systemie Android _usługi_. Usługi dla systemu Android to składnik, który jest przeznaczony do pracy bez interfejsu użytkownika. Usługi mogą pobrać plik, odtwarzanie muzyki lub zastosować filtr do obrazu. Usługi mogą również służyć do komunikacji międzyprocesowej (_IPC_) między aplikacjami systemu Android. Na przykład użyć jednej aplikacji systemu Android usługi odtwarzacz muzyczny, która jest przez inną aplikację lub aplikację może udostępniać dane (takie jak informacje kontaktowe osoby) do innych aplikacji za pomocą usługi. 

Usługi i możliwość wykonywania pracy w tle, są niezwykle istotne udostępnia interfejs użytkownika płynne i płynnych. Wszystkie aplikacje systemu Android _wątku głównego_ (znanej także jako _wątku interfejsu użytkownika_) uruchamiania działania. Aby zapewnić dynamiczne urządzenia, systemu Android musi mieć możliwość zaktualizuj interfejs użytkownika w wysokości 60 klatek na sekundę. Jeśli wydajność aplikacji systemu Android do dużo pracy w głównym wątku, a następnie Android spowoduje porzucenie ramki, który z kolei powoduje wyświetlenie jerky interfejsu użytkownika (czasami nazywane _janky_). Oznacza to, do wykonania w wątku interfejsu użytkownika powinno być wykonane w przedział czasu między dwóch ramek, około 16 MS (1 sekundę co 60 klatek). 

Aby rozwiązać ten problem, deweloper może użyć do wykonania dodatkowych czynności, które uniemożliwiają interfejsu użytkownika wątków w działaniu. Jednak może to powodować problemy. Jest bardzo możliwe, że Android spowoduje zniszczenie i ponowne utworzenie wielu wystąpień działania. Android nie zostaną jednak automatycznie zniszczenia wątków, które mogłyby powodować przecieki pamięci. Jest podstawowym przykład pojawia się, gdy [urządzenia jest obracana](~/android/app-fundamentals/handling-rotation.md) &ndash; Android spróbuje zniszczyć wystąpienia działania, a następnie utwórz ponownie nowy:

![Gdy urządzenia obraca, wystąpienia 1 zostanie zniszczony i jest tworzone wystąpienie 2](images/image-01.png)

Jest to potencjalny wyciek pamięci &ndash; Wątek utworzony przez pierwsze wystąpienie działania będzie nadal działać. Jeśli wątek odwołuje się do pierwszego wystąpienia działania, uniemożliwi to Android pamięci zbieranie obiektu. Jednak drugiego wystąpienia działania nadal jest tworzony (który z kolei może utworzyć nowego wątku). Obracanie urządzeń w krótkim odstępie czasu może spalin cała pamięć RAM i wymusić Android zakończenie całej aplikacji w celu odzyskania pamięci.

Jako zasadą Jeśli pracy do wykonania należy outlive działania, następnie usługi powinny być tworzone na wykonanie tej pracy. Jednak jeśli pracy ma zastosowanie tylko w kontekście działania, następnie tworzenie wątku do wykonania pracy może być bardziej odpowiednie. Na przykład tworzenie miniatury zdjęcia, który został dodany do aplikacji w Galerii fotografii powinny prawdopodobnie wystąpią w usłudze. Jednak wątek może być bardziej odpowiednie do odtwarzania niektóre muzyki tylko powinien heard, gdy działanie znajduje się na pierwszym planie.

Praca w tle mogą być podzielone na dwie szerokie klasyfikacje:

1. **Długie uruchomione zadania** &ndash; jest praca jest wykonywana, aż do zatrzymania jawnie. Przykład _długotrwałe zadania_ to aplikacja, która strumieni muzyki lub które należy monitorować danych zbieranych z czujnika. Tych zadań należy uruchomić, nawet jeśli aplikacja nie ma widocznych interfejsu użytkownika.
2. **Zadania wykonywane okresowo** &ndash; (czasami określane jako _zadania_) okresowe zadania to taki, który jest stosunkowo krótki czas trwania (kilka sekund) i jest uruchamiane zgodnie z harmonogramem (tj. raz dziennie na tydzień lub prawdopodobnie tylko raz w Następny 60 sekund). Na przykład jest pobierania pliku z Internetu lub generowania miniaturę obrazu.

Istnieją cztery typy usług z systemem Android:

* **Powiązane usługi** &ndash; A _powiązane usługi_ to usługa, która ma niektórych innych składników (zazwyczaj działanie) powiązane z nim. Powiązane usługi udostępnia interfejs umożliwiający składnika powiązane i usłudze na współdziałanie z sobą. Gdy nie ma żadnych więcej klientów powiązany z usługami, Android zamknie usługę.

* **`IntentService`** &ndash;  _`IntentService`_  Jest podklasą specjalne `Service` klasy, które upraszcza tworzenie usług i użycia. `IntentService` Jest przeznaczona do obsługi poszczególnych odwołań autonomicznego. W przeciwieństwie do usługi, która jednocześnie może obsłużyć wielu wywołań, `IntentService` przypomina _pracy kolejki procesora_ &ndash; pracy jest umieszczone w kolejce i `IntentService` przetwarza każde zadanie jednym naraz w wątku pojedynczego procesu roboczego. Zazwyczaj`IntentService` nie jest powiązany z działania lub fragmentu. 

* **Uruchomiono usługę** &ndash; A _uruchomił usługę_ to usługa, która została uruchomiona przez inne Android składników (na przykład działanie) i jest uruchamiany ciągłe w tle, aż coś jawnie informuje zatrzymanie usługi. W przeciwieństwie do powiązanej usługi uruchomiono usługi nie ma żadnych klientów bezpośrednio powiązane z nim. Z tego powodu należy projektować uruchomionych usług, tak aby ich może zostać bezpiecznie uruchomione ponownie w razie potrzeby.

* **Hybrydowe usługi** &ndash; A _hybrydowego usługi_ to usługa, która ma właściwości _uruchomił usługę_ i _powiązane usługi_. Można uruchomić usługi hybrydowego przez wiąże składnika lub mogą być uruchamiane przez zdarzenie. Składnik klienta może lub nie może być powiązany z usługą hybrydowego. Będzie przechowywać uruchomiona usługa hybrydowych, dopóki nie jest jawnie informację, aby zatrzymać lub dopóki nie zostaną nie ma więcej klientów powiązane z nim.

Typ usługi do użycia zależy od bardzo wymagań aplikacji. Jako zasadą `IntentService` lub powiązane usługi są wystarczające dla większości zadań, które należy wykonać aplikacji systemu Android, więc preferowany do jednej z tych dwóch typów usług. `IntentService` Jest dobrym rozwiązaniem w przypadku "jednorazowej" zadania, takie jak pobieranie pliku, gdy powiązanej usługi może dochodzić do odpowiedniego częste interakcji z działania/Fragment jest wymagana. 

Podczas gdy większość usługi są uruchomione w tle, istnieje specjalne podkategorii znany jako _pierwszego planu usługi_. Jest to usługa, która jest wyższy priorytet (w porównaniu do normalnego użytkowania) do wykonywania niektórych pracy dla użytkownika (na przykład odtwarzanie muzyki). 

Istnieje również możliwość uruchomienia usługi w jego własnym procesie na jednym urządzeniu, jest to czasami określane jako _zdalny_ lub jako _usługi poza procesem_. Wymaga to więcej działań zmierzających do tworzenia, ale może być przydatne w przypadku gdy aplikacja potrzebuje dzielenie funkcji z innymi aplikacjami i w niektórych przypadkach ulepszyć środowisko użytkownika aplikacji. 

Każdy z tych usług ma własne charakterystyki i działania i tak, zostanie omówiona bardziej szczegółowo w ich własnych przewodnikach.
