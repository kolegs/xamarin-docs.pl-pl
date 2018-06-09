---
title: Wprowadzenie do tworzenia aplikacji przedsiębiorstwa
description: Ten rozdział zawiera wprowadzenie do tworzenia aplikacji przedsiębiorstwa i wprowadza eShopOnContainers aplikacji mobilnej.
ms.prod: xamarin
ms.assetid: cbce0659-fa03-447a-86ec-140438143230
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 9deb685c92092ceb0e1c775a1e53ac1bce5a4a57
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35242963"
---
# <a name="introduction-to-enterprise-app-development"></a>Wprowadzenie do tworzenia aplikacji przedsiębiorstwa

Niezależnie od platformy deweloperzy aplikacji w organizacji mają kilka inne:

-   Wymagania dotyczące aplikacji, które mogą ulec zmianie.
-   Nowe możliwości biznesowe i wyzwania.
-   Informacji zwrotnych podczas tworzenia, który może znacząco wpłynąć na, zakresu i wymagania dotyczące aplikacji.

Z tych pamiętać jest ważne tworzyć aplikacje, które można łatwo zmodyfikować lub rozszerzone wraz z upływem czasu. Projektowanie dla takich zdolności adaptacyjnych może być trudne, ponieważ wymaga ona architekturę, która umożliwia poszczególnych części aplikacji niezależnie opracowany i przetestowany w izolacji bez wpływu na pozostałą część aplikacji.

Wiele aplikacji przedsiębiorstwa jest dostatecznie złożone, aby wymagać więcej niż jeden developer. Może być istotne wyzwanie decyzji o tym, jak zaprojektować aplikację tak, aby wiele deweloperzy mogą pracować wydajnie na różne części aplikacji niezależnie, przy jednoczesnym zapewnieniu, że elementy grupuje bezproblemowo po zintegrowaniu w aplikacji.

Tradycyjne podejście do projektowania i konfigurowania aplikacji, co powoduje jest określany jako *wbudowanymi* aplikacji, których składniki są ściśle powiązane ze nie wyraźnej separacji między nimi. Zazwyczaj takie podejście wbudowanymi prowadzi do aplikacji, które są trudne i nieefektywne Obsługa, ponieważ może być trudne do rozwiązania usterki bez przerywania innych składników w aplikacji i może być trudne do dodawania nowych funkcji lub zastąpić istniejące funkcje.

Skuteczny środek prawny te problemy jest partycji aplikacji do odrębny, luźno powiązanych składników, które można łatwo integrować razem w aplikacji. Takie podejście zapewnia kilka korzyści:

-   Dzięki temu poszczególne funkcje można opracowany, przetestowane, rozszerzone i obsługiwany przez różne konkretnych osób lub zespołów.
-   Zamienia ją czystej separacji między poziomy możliwości aplikacji, takich jak uwierzytelnianie i dostęp do danych, a pionowy możliwości, takich jak funkcje firmy aplikacji i ponowne użycie. Dzięki temu zależności i interakcje między składnikami aplikacji, co ułatwia zarządzanie.
-   Pomaga zachowanie separacji ról, zezwalając różnych osób lub zespołów skoncentrować się na konkretne zadanie lub część funkcjonalności zgodnie z ich wiedzy. W szczególności zapewnia czyszczący rozdzielenie interfejs użytkownika i logiki biznesowej aplikacji.

Istnieje jednak wiele problemów, które musi zostać rozpoznane podczas partycjonowania aplikację do odrębny, luźno powiązanych składników. Należą do nich następujące elementy:

-   Wybierając sposób zapewnienia czystego separacji między kontrolek interfejsu użytkownika i ich logiki. Jedną z najważniejszych decyzji podczas tworzenia aplikacji platformy Xamarin.Forms przedsiębiorstwa jest czy ma zostać umieszczona logika biznesowa w plików z kodem, lub czy utworzyć czystego separacji między ich logiki, aby zapewnić więcej aplikacji i formantów interfejsu użytkownika łatwy w obsłudze i testować. Aby uzyskać więcej informacji, zobacz [Model-View-ViewModel](~/xamarin-forms/enterprise-application-patterns/mvvm.md).
-   Określanie, czy ma być używany kontener iniekcji zależności. Kontenery iniekcji zależności ograniczyć zależność sprzężenia między obiektami, zapewniając możliwość utworzenia wystąpienia klas z zależnościami, ich wstrzyknięcie i zarządzać ich cykl życia na podstawie konfiguracji kontenera. Aby uzyskać więcej informacji, zobacz [iniekcji zależności](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md).
-   Wybór między platforma pochodząca zdarzeń i słabo połączone komunikacji między składnikami, które są niewygodne przez odwołania do obiektu i typu komunikatu. Aby uzyskać więcej informacji, zobacz Introduction to [komunikacji między luźno powiązane składniki](~/xamarin-forms/enterprise-application-patterns/communicating-between-loosely-coupled-components.md).
-   Podjęcie decyzji o sposobie przechodzenia między stronami, łącznie ze sposobem wywołania nawigacji, i gdzie powinien znajdować się logiki nawigacji. Aby uzyskać więcej informacji, zobacz [nawigacji](~/xamarin-forms/enterprise-application-patterns/navigation.md).
-   Określanie, jak można sprawdzić poprawności danych wejściowych użytkownika pod kątem poprawności. Decyzja musi zawierać sprawdzania poprawności danych wejściowych użytkownika i jak powiadomić użytkowników o błędach weryfikacji. Aby uzyskać więcej informacji, zobacz [weryfikacji](~/xamarin-forms/enterprise-application-patterns/validation.md).
-   Przy wyborze sposobu uwierzytelniania oraz sposób chronić zasoby o autoryzacji. Aby uzyskać więcej informacji, zobacz [uwierzytelniania i autoryzacji](~/xamarin-forms/enterprise-application-patterns/authentication-and-authorization.md).
-   Określanie, jak dostęp do danych zdalnych z sieci web usług, oraz sposób niezawodny sposób pobierania danych, jak dane z pamięci podręcznej. Aby uzyskać więcej informacji, zobacz [podczas uzyskiwania dostępu do danych zdalnych](~/xamarin-forms/enterprise-application-patterns/accessing-remote-data.md).
-   Przy wyborze dotyczące testowania aplikacji. Aby uzyskać więcej informacji, zobacz [testów jednostkowych](~/xamarin-forms/enterprise-application-patterns/unit-testing.md).

Ten przewodnik zawiera wskazówki dotyczące tych problemów, a koncentruje się na podstawowych wzorców i architekturę umożliwiające tworzenie aplikacji wieloplatformowych przedsiębiorstwa, za pomocą platformy Xamarin.Forms. Wskazówki ma na celu pomoc do tworzenia kodu dostosowywalne, łatwy w obsłudze i testować, umożliwi Udokumentowanie typowe scenariusze programowania aplikacji platformy Xamarin.Forms przedsiębiorstwa i oddzielając dotyczy prezentacji, logiki prezentacji i jednostek do obsługi Wzorzec Model-View-ViewModel (MVVM).

## <a name="sample-application"></a>Przykładowa aplikacja

Ten przewodnik zawiera przykładową aplikację, eShopOnContainers, który jest sklepu, która obejmuje następujące funkcje:

-   Uwierzytelniania i autoryzacji dla usługi wewnętrznej bazy danych.
-   Przeglądanie katalogu koszule, mukeja kawy i innych elementów marketingu.
-   Filtrowanie katalogu.
-   Określanie kolejności elementów z katalogu.
-   Wyświetlanie historii zamówień użytkownika.
-   Konfiguracja ustawień.

### <a name="sample-application-architecture"></a>Przykładowa Architektura aplikacji

Rysunek 1-1 zawiera ogólne omówienie architektury przykładowej aplikacji.

![](introduction-images/architecture.png "Architektura wysokiego poziomu eShopOnContainers")

**Rysunek 1-1**: eShopOnContainers Architektura wysokiego poziomu

Przykładowa aplikacja jest dostarczany z trzech aplikacji klienta:

-   Opracowanych aplikacji MVC za pomocą platformy ASP.NET Core.
-   Jednej strony aplikacji JEDNOSTRONICOWEJ opracowany z kątowego 2 i Typescript. Takie podejście dla aplikacji sieci web pozwala uniknąć wykonywania przesłania danych do serwera z każdej operacji.
-   Aplikacji mobilnych opracowanych za pomocą platformy Xamarin.Forms, który obsługuje iOS, Android i Windows platformy Uniwersalnej.

Aby uzyskać informacje dotyczące aplikacji sieci web, zobacz [Architecting i opracowywanie nowoczesnych aplikacji sieci Web platformy ASP.NET Core i Microsoft Azure](http://aka.ms/WebAppEbook).

Przykładowa aplikacja zawiera następujące usługi wewnętrznej bazy danych:

-   Mikrousługi tożsamości, która korzysta z ASP.NET Core Identity oraz IdentityServer.
-   Mikrousługi katalogu, które jest oparte na danych tworzenia, odczytu, aktualizowanie i usuwanie usługi (CRUD), który wykorzystuje bazy danych programu SQL Server przy użyciu platformy EntityFramework Core.
-   Porządkowania mikrousługi, które jest oparte na domenie usługa, która wykorzystuje wzorce projektowe opartych na domenie.
-   Mikrousługi koszyka, która jest usługą CRUD opartych na danych, która korzysta z pamięci podręcznej Redis.

Te usługi wewnętrznej bazy danych są zaimplementowane jako mikrousług przy użyciu platformy ASP.NET Core MVC, a następnie są wdrażane jako kontenery unikatowe w obrębie jednego hosta Docker. Zbiorczo te usługi wewnętrznej bazy danych są określane jako eShopOnContainers zawierają odwołania do aplikacji. Aplikacje klienckie komunikują się z usługami zaplecza za pośrednictwem interfejsu sieci web Representational State Transfer (REST). Aby uzyskać więcej informacji na temat mikrousług i Docker, zobacz [Mikrousług konteneryzowanych](~/xamarin-forms/enterprise-application-patterns/containerized-microservices.md).

Informacje o implementacji usługi wewnętrznej bazy danych, zobacz [Mikrousług .NET: Architektura konteneryzowanych aplikacji .NET](https://aka.ms/microservicesebook).

### <a name="mobile-app"></a>Aplikacji mobilnej

Ten przewodnik koncentruje się na tworzeniu organizacji i platform aplikacji przy użyciu platformy Xamarin.Forms i korzysta z aplikacji mobilnej eShopOnContainers jako przykład. Rysunek 1 i 2 zawiera strony z aplikacji mobilnej eShopOnContainers, które zapewniają funkcje opisane wcześniej.

[![](introduction-images/screenshots.png "Aplikacja mobilna eShopOnContainers")](introduction-images/screenshots-large.png#lightbox "eShopOnContainers aplikacji mobilnej")

**Rysunek 1 i 2**: eShopOnContainers aplikacji mobilnej

Aplikacja mobilna zużywa udostępniany przez aplikację odwołanie eShopOnContainers usługi wewnętrznej bazy danych. Jednak może być skonfigurowana do pracy z danymi z usług zasymulować dla tych, którzy chcą uniknąć wdrażanie usług zaplecza.

Aplikacja mobilna eShopOnContainers wykonuje następujące funkcje platformy Xamarin.Forms:

-   XAML
-   Formanty
-   Powiązania
-   Konwertery
-   Style
-   Animacje
-   Polecenia
-   Zachowania
-   Wyzwalacze
-   Efekty
-   Niestandardowe moduły renderowania
-   MessagingCenter
-   Formanty niestandardowe

Aby uzyskać więcej informacji dotyczących tej funkcji, zobacz [dokumentacji platformy Xamarin.Forms](~/xamarin-forms/index.yml), i [tworzenia aplikacji mobilnych za pomocą platformy Xamarin.Forms](https://aka.ms/xamebook).

Ponadto testy jednostkowe są dostępne dla niektórych klas w aplikacji mobilnej eShopOnContainers.

#### <a name="mobile-app-solution"></a>Rozwiązania mobilne aplikacji

Rozwiązanie aplikacji mobilnej eShopOnContainers organizuje kodu źródłowego i innych zasobów w projektach. Wszystkie projekty użyć folderów do organizowania kodu źródłowego i innych zasobów na kategorie. W poniższej tabeli przedstawiono projektów, które tworzą eShopOnContainers aplikacji mobilnej:

|Projekt|Opis|
|--- |--- |
|eShopOnContainers.Core|Ten projekt jest projekt biblioteki (PCL) klas przenośnych, który zawiera kod udostępnionego i udostępnionych interfejsu użytkownika.|
|eShopOnContainers.Droid|Ten projekt zawiera określonego kodu dla systemu Android i jest punkt wejścia dla aplikacji systemu Android.|
|eShopOnContainers.iOS|Ten projekt zawiera z systemem iOS kod i jest punkt wejścia dla aplikacji systemu iOS.|
|eShopOnContainers.UWP|Ten projekt zawiera kod systemu Windows platformy Uniwersalnej i jest punkt wejścia dla aplikacji systemu Windows.|
|eShopOnContainers.TestRunner.Droid|Ten projekt jest Android uruchamiający eShopOnContainers.UnitTests projektu.|
|eShopOnContainers.TestRunner.iOS|Ten projekt jest uruchamiający iOS eShopOnContainers.UnitTests projektu.|
|eShopOnContainers.TestRunner.Windows|Ten projekt jest uruchamiający platformy uniwersalnej systemu Windows dla projektu eShopOnContainers.UnitTests.|
|eShopOnContainers.UnitTests|Ten projekt zawiera testy jednostkowe dla projektów eShopOnContainers.Core.|

Klasy z aplikacji mobilnej eShopOnContainers mogą być ponownie używane w dowolnej aplikacji platformy Xamarin.Forms żadnych modyfikacji.

##### <a name="eshoponcontainerscore-project"></a>eShopOnContainers.Core Project

Projekt PCL eShopOnContainers.Core zawiera następujące foldery:

|Folder|Opis|
|--- |--- |
|Animacje|Zawiera klasy, które umożliwiają animacje do użycia w języku XAML.|
|Zachowania|Zawiera zachowania, które są dostępne, aby wyświetlić klasy.|
|Formanty|Zawiera formanty niestandardowe używane przez aplikację.|
|Konwertery|Zawiera konwertery wartości, które są stosowane niestandardowej logiki do powiązania.|
|Efekty|Zawiera `EntryLineColorEffect` klasy, która służy do zmiany koloru obramowania określonych `Entry` kontrolki.|
|Wyjątki|Zawiera niestandardowego `ServiceAuthenticationException`.|
|Rozszerzenia|Zawiera metody rozszerzenia dla `VisualElement` i `IEnumerable` klasy.|
|Pomocnicy|Zawiera klasy pomocy dla aplikacji.|
|Modele|Zawiera klasy modelu dla aplikacji.|
|Właściwości|Zawiera `AssemblyInfo.cs`, plik metadanych zestawu .NET.|
|Usługi|Zawiera interfejsy i klasy, które implementują usług, które są dostarczane do aplikacji.|
|Wyzwalacze|Zawiera `BeginAnimation` wyzwalacz, który służy do wywoływania animacji w języku XAML.|
|Sprawdzanie poprawności|Zawiera klasy zajmujących się sprawdzanie poprawności danych wejściowych.|
|ViewModels|Zawiera logikę aplikacji, który jest dostępny dla stron.|
|Widoki|Zawiera strony dla aplikacji.|

##### <a name="platform-projects"></a>Projekty platformy

Projekty platformy zawierają implementacje efekt, implementacji niestandardowego modułu renderowania i inne zasoby specyficzne dla platformy.

## <a name="summary"></a>Podsumowanie

Narzędzia deweloperskie i platform aplikacji mobilnej na platformie Xamarin i podaj kompleksowe rozwiązanie dla klientów urządzeń przenośnych B2E B2B i B2C aplikacji, umożliwiając udostępnianie kodu we wszystkich platform docelowych (z systemem iOS, Android i Windows) i pomoc, aby zmniejszyć Całkowity koszt posiadania. Aplikacje mogą udostępniać ich interfejsu i aplikacji logiki kod użytkownika, przy zachowaniu platformy natywnego wyglądu i działania.

Deweloperzy aplikacji w przedsiębiorstwie stają przed kilka wyzwania, które można zmienić architektury aplikacji podczas tworzenia. W związku z tym należy do tworzenia aplikacji, tak aby można modyfikować i rozszerzony w czasie. Projektowanie dla takich zdolności adaptacyjnych może być trudne, ale zazwyczaj polega to na partycje aplikacji do odrębny, luźno powiązanych składników, które można łatwo integrować razem w aplikacji.


## <a name="related-links"></a>Linki pokrewne

- [Pobieranie książki elektronicznej (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (przykład)](https://github.com/dotnet-architecture/eShopOnContainers)
