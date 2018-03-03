---
title: "Omówienie dystrybucji aplikacji"
description: "Ten dokument zawiera omówienie metod dystrybucji, które są dostępne dla aplikacji Xamarin.tvOS i służy jako wskaźnik do bardziej szczegółowych dokumentów na temat."
ms.topic: article
ms.prod: xamarin
ms.assetid: D5E0F446-C083-4E21-9788-FC84D32D00C4
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: c0cfe437b03a1f0dea05a506b1dfce62a4658bb4
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="app-distribution-overview"></a>Omówienie dystrybucji aplikacji

_Ten dokument zawiera omówienie metod dystrybucji, które są dostępne dla aplikacji Xamarin.tvOS i służy jako wskaźnik do bardziej szczegółowych dokumentów na temat._


Aplikacja Xamarin.tvOS został opracowany, do następnego kroku w cyklu tworzenia oprogramowania po dystrybucję aplikacji dla użytkowników, jak pokazano w sekcji wyróżnione na poniższym diagramie:


[![Przegląd cyklu życia rozwoju oprogramowania](images/publishingdiagram.png)](images/publishingdiagram.png)


Apple oferuje następujące sposoby dystrybucję aplikacji systemu tvOS, które są obsługiwane przez Xamarin.tvOS:

1. [**Sklep App Store**](#Apple-TV-App-Store-Distribution)
2. [**Wewnętrznych (Enterprise)**](#In-House-Distribution) 
2. [**Ad Hoc**](#Ad_Hoc_Distribution) 

Te scenariusze wymagają aplikacje udostępniane przy użyciu odpowiednich *profil inicjowania obsługi administracyjnej*. Profile inicjowania obsługi administracyjnej są pliki zawierające kod podpisywania informacje, a także tożsamości aplikacji i mechanizmu dystrybucji zamierzone. Dystrybucji sklepu nie zawierają one również informacje dotyczące urządzeń, jakie można wdrożyć aplikację do.

<a name="Apple-TV-App-Store-Distribution" />

## <a name="apple-tv-app-store-distribution"></a>Dystrybucji sklepu z aplikacjami firmy Apple TV

Jest to główny sposób, że aplikacje systemu tvOS są dystrybuowane do klientów na urządzeniach Apple TV. Wszystkie aplikacje przesłane do sklepu z aplikacjami firmy Apple TV wymagają zatwierdzenia przez firmę Apple.

Aplikacje są przesyłane do sklepu z aplikacjami za pośrednictwem portalu o nazwie *iTunes Connect*. Zobacz nasze [skonfiguruj aplikację w iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md) informacje w poniższych tematach w przewodniku:

- Zarządzanie umów podatku i banku.
- Tworzenie rekordu Connect iTunes.
- Zarządzanie wideo aplikacji i zrzuty ekranu.
- Zarządzanie aplikacjami nazwę, opis nowości, słowa kluczowe i adresów URL.
- Obsługa aplikacji ogólne informacje.
- Utrzymywanie informacji z aplikacji Game Center.
- Obsługa aplikacji Przegląd informacji.
- Obsługę, informacje o cenach.
- Utrzymywanie informacji zakupu w aplikacji.
- Wyświetlanie przeglądami aplikacji.

Gdy nie napisane specjalnie dla systemu tvOS, ten dokument zawiera informacje na temat sposobu konfigurowania i używania tego portalu do przygotowania aplikacji do publikowania w sklepie z aplikacjami firmy Apple TV. Wiele te same czynności jest wymagane czy konfigurowania aplikacji systemu iOS lub systemu tvOS.

Po wykonaniu wszystkich czynności wymienionych powyżej, zapoznaj się z naszym [konfigurowanie Twojego systemu tvOS aplikacji w iTunes Connect](~/ios/tvos/deploy-test/app-distribution/itunes-connect.md) dodać informacje określonych aplikacji systemu tvOS, które będzie wymagane.

Należy pamiętać, że tylko deweloperów, którzy należą do **programie dla deweloperów firmy Apple** mają dostęp do programu iTunes Connect. Elementy członkowskie **Apple Developer Enterprise Program** nie mają dostępu.

Jeśli występują problemy dotyczące przesyłania Xamarin.tvOS aplikacji do sklepu z aplikacjami firmy Apple TV, zobacz nasze [Rozwiązywanie problemów](~/ios/tvos/troubleshooting.md) przewodnik. Zawiera kilka znanych problemów, które można napotkać i sposobu rozwiązania tych problemów Xamarin.tvOS.

Aby uzyskać więcej informacji, odwiedź stronę [publikowania do sklepu z aplikacjami firmy Apple TV](~/ios/tvos/deploy-test/app-distribution/app-store-publishing.md) przewodnik.

<a name="In-House-Distribution" />

## <a name="in-house-distribution"></a>Dystrybucji wewnętrznych

Czasami nazywane *dystrybucji Enterprise*, wewnętrznych dystrybucji umożliwia członkom **Apple Developer Enterprise Program** Aby przeprowadzić dystrybucję aplikacji wewnętrznie na innych elementach członkowskich o tej samej organizacji. Rozkład wewnętrznych ma zalety nie wymagających przeglądu sklepu z aplikacjami oraz o brak limitu liczby urządzeń, na których można zainstalować aplikacji. Jednak ważne jest, aby należy pamiętać, że **Apple Developer Enterprise Program** czy elementy członkowskie **nie** mają dostęp do programu iTunes Connect i w związku z tym Licencjobiorca jest odpowiedzialna za dystrybucję aplikacji.

Aby uzyskać więcej informacji na temat pobierania konfiguracji i sposób dystrybucji we własnym zakresie aplikacji, zapoznaj się [podręczniku dystrybucji wewnętrznych](~/ios/deploy-test/app-distribution/in-house-distribution.md). Ten dokument jest przeznaczona dla systemu iOS, ale te same techniki są używane dla aplikacji systemu tvOS.

<a name="Ad-Hoc-Distribution" />

## <a name="ad-hoc-distribution"></a>Ad Hoc dystrybucji

Aplikacje Xamarin.tvOS mogą być testowane użytkownika za pośrednictwem dystrybucji ad hoc, która jest dostępna zarówno **programie dla deweloperów firmy Apple**i **Apple Developer Enterprise Program**i umożliwia maksymalnie 100 urządzeń Apple TV do sprawdzenia. Najlepsze przypadek użycia dystrybucji ad hoc jest dystrybucji w firmie, gdy iTunes Connect nie jest opcją.

Aby uzyskać więcej informacji na temat pobierania konfiguracji i sposób dystrybucji we własnym zakresie aplikacji, zapoznaj się [podręczniku dystrybucji Ad Hoc](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md). Ponownie ten dokument jest przeznaczona dla systemu iOS, ale te same techniki są używane dla aplikacji systemu tvOS.

<a name="Summary" />

## <a name="summary"></a>Podsumowanie

W tym artykule udzielił krótkie omówienie mechanizmów dystrybucji, które są dostępne dla aplikacji Xamarin.tvOS. Wprowadzono Apple TV sklepu App Store, Ad Hoc i we własnym zakresie wdrożenia i podano łącza do bardziej szczegółowych informacji.



## <a name="related-links"></a>Linki pokrewne

- [Przykłady dla systemu tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [systemu tvOS człowieka przewodniki — interfejs](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Przewodnik programowania w języku aplikacji dla systemu tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
