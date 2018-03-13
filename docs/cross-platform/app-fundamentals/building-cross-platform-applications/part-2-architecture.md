---
title: "Część 2 — architektura"
ms.topic: article
ms.prod: xamarin
ms.assetid: 2176DB2D-E84A-3757-CFAB-04A586068D50
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/27/2017
ms.openlocfilehash: 25cd374ef2b3026f5ac7242ee076c8593ce806da
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="part-2---architecture"></a>Część 2 — architektura

Jest główną cechą tworzenia wieloplatformowych aplikacji do utworzenia architekturę, która pozwala na maksymalizacyjne kodu do udostępniania różnych platform. Przestrzegać następujących programowanie zorientowane na obiekt zasad pomaga utworzyć dobrze zaprojektowanej architektury aplikacji:

-   **Hermetyzacja** — zapewnienie klasy i warstwy architektury nawet tylko ujawnia minimalny interfejs API, który wykonuje ich wymagane funkcje i ukrywa szczegóły implementacji. Na poziomie klasy oznacza to, że obiektów ma zachowywać się jako "czarne pola" i że korzystanie z kodu nie musi wiedzieć, jak ich wykonywania swoich zadań. Na poziomie architektury oznacza to, implementacja wzorce podobne fasad, które zachęcają uproszczony interfejs API organizuje bardziej złożonych interakcje w imieniu kod w bardziej abstrakcyjny warstwy. Oznacza to, że kod interfejsu użytkownika (na przykład) tylko odpowiada za wyświetlanie ekranów i akceptuje dane wejściowe użytkownika; i nigdy nie interakcji z bazą danych bezpośrednio. Podobnie kodu dostępu do danych należy tylko do odczytu i zapisu w bazie danych, ale nigdy nie bezpośrednią interakcję z przycisków i etykiet.
-   **Podział odpowiedzialności** — upewnij się, że każdy składnik (zarówno w architekturze i klasy poziom) Wyczyść i dobrze zdefiniowany cel. Każdego składnika należy wykonać tylko jego określonych zadań i ujawniać te funkcje przy użyciu interfejsu API, który jest dostępny dla innych klas, które muszą używać go.
-   **Polimorfizm** — programowania interfejsu (lub klasy abstrakcyjnej), która obsługuje wiele implementacji oznacza że core kodu można zapisane i udostępniane na platformach, podczas nadal korzysta z funkcji specyficznych dla platformy.


Fizyczne wynikiem jest modelowana po rzeczywistych lub abstrakcyjny jednostki z oddzielnych warstw logicznych aplikacji. Rozdzielić kodu warstw Udostępnij aplikacje łatwiejsze do zrozumienia, testowania i obsługi. Zaleca się, że kod w każdej warstwie mieścić się w osobnych (albo w katalogu lub nawet osobnych projektów dla bardzo dużych aplikacji), jak również logicznie oddzielne (Używanie usługi przestrzenie nazw).

 <a name="Typical_Application_Layers" />


## <a name="typical-application-layers"></a>Typowa aplikacja warstwy

W tym dokumencie i analizy przypadków firma Microsoft można znaleźć w następujących warstw sześciu aplikacji:

-   **Warstwa danych** — Non-volatile trwałości danych, które mogą być bazy danych SQLite, ale można zaimplementowany z plikami XML lub inny mechanizm odpowiedni.
-   **Warstwa dostępu do danych** — otokę Warstwa danych, która udostępnia tworzenia, odczytu, aktualizacji i usuwania (CRUD) dostęp do danych bez narażania szczegóły implementacji do obiektu wywołującego. Na przykład warstwy DAL może zawierać instrukcji SQL zapytań lub zaktualizowanie danych, ale kod odwołujący się nie musi znać to.
-   **Warstwa biznesowa** — (nazywane warstwy logiki biznesowej lub logiki warstwy Biznesowej) zawiera definicje jednostek biznesowych (Model) i logiki biznesowej. Kandydatem do wzorca fasad biznesowych.
-   **Warstwa dostępu do usługi** — używane do dostępu do usług w chmurze: z złożonych usług sieci web (REST, JSON, WCF) proste pobierania danych i obrazów z serwerów zdalnych. Hermetyzuje działanie sieci i udostępnia prosty interfejs API zużywanych przez warstwy aplikacji i interfejsu użytkownika.
-   **Warstwa aplikacji** — kod, który zazwyczaj jest przeznaczona dla konkretnej platformy (zazwyczaj udostępniana na platformach) lub kodu, który jest specyficzne dla aplikacji (nie ogólnie wielokrotnego użytku). Jest dobrym test czy umieścić kod w warstwie aplikacji i warstwy interfejsu użytkownika () w celu ustalenia, czy klasa ma wszystkie formanty wyświetlany lub (b) czy go można udostępniać wielu ekrany lub urządzeń (np. urządzenia iPhone i iPad).
-   **Warstwę Interfejsu użytkownika** — warstwy uwzględniającym działania użytkownika zawiera ekrany, elementów widget i kontrolery który zarządzać nimi.


Aplikacja nie może musi zawierać wszystkie warstwy — na przykład warstwa dostępu do usługi nie będzie istnieć w aplikacji, która nie dostępu do zasobów sieciowych. Bardzo prostą aplikację może scalić warstwy danych i warstwa dostępu do danych, ponieważ operacje są bardzo podstawowe.

 <a name="Common_Mobile_Software_Patterns" />


## <a name="common-mobile-software-patterns"></a>Wspólne wzorce oprogramowanie dla urządzeń przenośnych

Wzorce są ustalonych sposobem przechwytywania cyklicznego rozwiązania typowych problemów. Istnieje kilka kluczowych wzorców, które są przydatne zrozumieć, tworzyć aplikacje mobilne łatwy w obsłudze/zrozumiałe.

-   **Model, widok, ViewModel (MVVM)** — Model-View-ViewModel wzorzec jest popularnych z platform, które obsługują wiązania danych, takich jak platformy Xamarin.Forms. Został on popularized przez włączone XAML zestawów SDK, takich jak Windows Presentation Foundation (WPF) i Silverlight; gdzie ViewModel działa jako łącząca między danymi (Model) i interfejs użytkownika (Widok) za pomocą poleceń i powiązań danych.
-   **Model, widok, kontroler (MVC)** — typowe i często niewłaściwie rozumiana wzorzec MVC jest najczęściej używana podczas kompilowania interfejsy użytkownika i zapewnia rozdzielenie rzeczywiste definicji ekranu interfejsu użytkownika (Widok), aparat za nią, który obsługuje interakcja (kontroler), a dane, które wypełnia ją (Model). Model jest rzeczywiście całkowicie opcjonalny element i w związku z tym core zrozumienia tego wzorca znajduje się w widoku i kontrolera. MVC to podejście popularnych w aplikacjach systemu iOS.
-   **Fasad firm** — alias wzorzec Manager zapewnia uproszczony punktu wejścia złożonych zadań. Na przykład w aplikacji zadań śledzenia, może być `TaskManager` klasy za pomocą metod, takich jak `GetAllTasks()` , `GetTask(taskID)` , `SaveTask (task)` itp. `TaskManager` Klasa udostępnia fasad do przebiega faktycznie zapisywania/pobierania zadań obiektów.
-   **Pojedyncze** — wzorzec Singleton zapewnia sposób, w którym kiedykolwiek może istnieć tylko jedno wystąpienie określonego obiektu. Na przykład gdy przy użyciu systemu SQLite w aplikacjach mobilnych, tylko ma jedno wystąpienie bazy danych. Przy użyciu wzorca Singleton jest zapewnienie to prosty sposób.
-   **Dostawca** — wzorzec coined przez firmę Microsoft (podobnie raczej strategii lub podstawowa iniekcji zależności) wspieranie ponownego użycia kodu w aplikacji Silverlight, WPF i WinForms. Udostępniony kodu mogą być zapisywane z interfejsem lub abstrakcyjną klasą i implementacje konkretnych specyficzne dla platformy są zapisywane i przekazano, gdy jest używany kod.
-   **Asynchroniczne** — nie należy mylić ze słowem kluczowym Async Async wzorzec jest używany podczas pracy długotrwałe musi być wykonywane bez interfejsu użytkownika lub przetwarzania bieżącego. W najprostszej postaci wzorca asynchronicznego po prostu opisuje czy długotrwałych zadań powinna zostać rozpoczęte w innym wątku (lub podobny abstrakcji wątku, takie jak zadania) podczas bieżącego wątku kontynuuje przetwarzanie i nasłuchuje na odpowiedź z proces w tle , a następnie aktualizuje interfejsu użytkownika, gdy dane i/lub stanu jest zwracany.


Każdy z wzorców zostaną sprawdzone w bardziej szczegółowo sposób ich wykorzystania praktyczne jest przedstawiony analizy przypadków. Wikipedia przedstawiono bardziej szczegółowe opisy [MVVM](https://en.wikipedia.org/wiki/Model–view–viewmodel), [MVC](https://en.wikipedia.org/wiki/Model–view–controller), [fasady](http://en.wikipedia.org/wiki/Facade_pattern), [Singleton](http://en.wikipedia.org/wiki/Singleton_pattern), [strategii](http://en.wikipedia.org/wiki/Strategy_pattern)i [dostawcy](http://en.wikipedia.org/wiki/Provider_model) wzorców (i [wzorce projektowe](http://en.wikipedia.org/wiki/Design_Patterns) zwykle).
