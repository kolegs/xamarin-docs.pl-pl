---
title: Zmiany sklepu z aplikacjami systemu iOS 11
description: Ten dokument Eksploruje zmiany do sklepu z aplikacjami w systemie iOS 11. Omówiono go ikona magazynu aplikacji, awansowana zakupy w aplikacji, produktu przeprojektowana strona, komunikacji z klientami i wersjach etapowe.
ms.prod: xamarin
ms.assetid: 4A7A03FD-B4F2-4969-8676-A17260730FD6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/13/2016
ms.openlocfilehash: 59f5d2c0c05ec2950ae7cc74e4f5aaa2565020f0
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787393"
---
# <a name="app-store-changes-in-ios-11"></a>Zmiany sklepu z aplikacjami systemu iOS 11

IOS App Store miał pełną one, które nie tylko umożliwia wydajne Przejdź magazynu, ale można też, Deweloper, aby promować aplikację dla użytkowników. Promocje te obejmują aktualizacje na zakupy w aplikacji i aktualizacji do strony produktu. iOS 11 dodaje również aktualizacje dotyczące sposób komunikowania się z użytkownikami, jak dodać ikonę Twojej aplikacji i jak zwolnić aplikacji publicznie.

![Nowy układ magazynu aplikacji](app-store-changes-images/image3.jpg)

Przeprojektowana sklepu zawiera następujące sekcje:

- **Dzisiaj** — ta karta zawiera aplikacje, które są "Wybierz edytora" lub polecanych aplikacji. Wspieranie aplikacji w tym miejscu wprowadź informacje w [wspierania](https://developer.apple.com//contact/app-store/promote/) strony.
- **Gry** — wszystkie aplikacje, które ma ustawioną wartość **gry** kategorii w iTunes Connect można znaleźć w na tej karcie.
- **Aplikacje** — ta karta zawiera wszystkie inne aplikacje. Użytkownicy mogą przeglądać polecane aplikacje, kategorie, top płatnej/wolne.
- **Aktualizacje** — ta aplikacja wyświetla aktualizacje do aplikacji. Nawet w przypadku wersji aplikacji za pomocą [dwufazowej wersji](#Phased_Release), użytkownicy mogą nadal przejdź do tej karty i pobrać najnowszą wersję.
- **Wyszukiwanie** — ta karta umożliwia użytkownikom wyszukiwanie aplikacji.

## <a name="store-icon"></a>Ikona magazynu

Ikony magazynu (lub ikony marketing) nie są zarządzane w iTunes Connect, a zamiast tego muszą być zawarte jako [katalogu zasobów](~/ios/app-fundamentals/images-icons/app-icons.md) Twojego binarnie aplikacji, podobnie jak ikony aplikacji. Ikona 1024 x 1024 magazynu, w formacie PNG, muszą być zawarte w katalogu zasobów o pomyślnym przesyłanie aplikacji dla systemu iOS 11.

Aplikacja spłaszczenia upewnia się, że ten katalog dodatkowych zasobów nie zwiększa rozmiaru aplikacji.


## <a name="in-app-purchases-promoted-in-the-app-store"></a>Zakupy w aplikacji awansowane w sklepie z aplikacjami

Apple wprowadził zakupy w aplikacji mogą szybciej odnajdywać w sklepie z aplikacjami. Teraz można dodać maksymalnie 20 _sklepu z aplikacjami awansowane_ zakupy w aplikacji, które można znaleźć na stronie aplikacji, na stronie produktu aplikacji lub wyszukując.

Aby zakupy w aplikacji wyświetlane w sklepie z aplikacjami musi zawierać następujące dane:

- **Obraz** — musisz podać specjalnie zaprojektowanego obrazu ikony, który opisuje, czego zakupu w aplikacji. Ten obraz musi być inna niż ikonę aplikacji i nie może być zrzut ekranu.
- **Nazwa** — nazwa może zawierać tylko maksymalnie 30 znaków.
- **Opis elementu** — opis można tylko 45 znaków.

Żadnych promocji zakupu w aplikacji mogą ulec przeglądu sklepu aplikacji, zanim będzie można go opublikować.

Aby można podwyższyć poziom zakupy w aplikacji, Otwórz aplikację i przejdź do **Funkcje > Zakupy w aplikacjach**. Przejdź do **promocji magazynu aplikacji (opcjonalnie)** sekcji, a następnie dodać obraz 1024 x 1024 i **zapisać**, jak pokazano na poniższej ilustracji:

![Podwyższanie poziomu Sklepu z aplikacjami części iTune Connect](app-store-changes-images/image4.png)

Należy również dodać `ShouldAddStorePayment` metodę `SKPaymentTransactionObserver` protokołu w aplikacji.

Aby uzyskać więcej informacji o promocjach zakupu w aplikacji, zobacz firmy Apple [podwyższania poziomu swoje zakupy w aplikacjach](https://developer.apple.com/app-store/promoting-in-app-purchases/) strony.

## <a name="redesigned-product-page"></a>Strona produktu zmienioną

Strona produktu wprowadzono następujące zmiany:

- Tytuły, teraz są ustawione na maksymalnie 30 znaków
- Podtytuł został dodany.
    - To krótkie podsumowanie compliments tytuł.
    - Podtytuł powinny mieć maksymalnie 30 znaków
- Podglądy aplikacji
    - Teraz można mieć trzy wideo lub zrzuty ekranu.
    - Filmy wideo autoodtwarzania, gdy użytkownik odwiedza strony.
    - Aby uzyskać więcej informacji, zobacz firmy Apple [aplikacji w wersji zapoznawczej](https://developer.apple.com/app-store/app-previews/) strony.
- Promocyjna tekstu
    - Ta nowa funkcja zapewnia 170 znaków tekstu, co umożliwia opisano często zmiana informacji o aplikacji.
    - Może zostać zaktualizowana w dowolnym momencie bez konieczności przesyłania nowej wersji aplikacji.

## <a name="customer-communication"></a>Komunikacja klienta

W 10.3 Apple uruchomić nowy sposób deweloperom komunikują się bezpośrednio z aplikacji użytkownikom za pomocą możliwość odpowiadanie na recenzje. Można uzyskać dostępu do tej nowej funkcji w programach iTunes połączyć, przechodząc do **aplikacji > działania > klasyfikacje i recenzje**, jak pokazano na poniższej ilustracji:

![Okno dialogowe przedstawiający obszar wprowadź Odpowiedz na komentarz](app-store-changes-images/image5.png)

Istnieje kilka sposobów pod uwagę przy odpowiadaniu na użytkowników:

- Można odpowiedzieć tylko raz, ale obie strony można edytować uwag, jak chcą.
- Użytkownicy otrzymują powiadomienie, gdy odpowiadanie na komentarz.
- Podłącz nowy obsługi klienta, który rola została utworzona w programach iTunes. Użytkownicy z tą rolą lub rolę administratora mogą odpowiadać na komentarz.

Aby uzyskać więcej informacji, zobacz firmy Apple [odpowiada na żądania przeglądami](https://developer.apple.com/app-store/responding-to-reviews/) strony.

<a name="Phased_Release"/>

## <a name="phased-release"></a>Etapowe zlecenia

Z systemem iOS 11 Apple zaimplementowała opcję etapowe wersje aktualizacji do aplikacji. Można włączyć etapowe wersjach umożliwia aktualizację aplikacji stopniowo zostać wydana do klientów, zapewniając, że żądanie nie przeciążać środowiska produkcyjnego.

Etapowe wersje są włączone w iTunes Connect. Kliknij aplikację na pasku bocznym, przewiń do **stopniowo wydanie aktualizacji automatycznych** u dołu, a następnie wybierz opcję **wersji aktualizacji w przypadku 7-dniowego okresu stopniowo wersji**:

![Wyświetlanie opcji stopniowo wersji aktualizacji automatycznych](app-store-changes-images/image6.png)

Aktualizacja jest dostępna od razu do pobrania na karcie Aktualizacje w sklepie z aplikacjami. Etapowe wersje są dostępne tylko dla użytkowników, którzy mają automatyczne pobieranie wybrane.


## <a name="related-links"></a>Linki pokrewne

- [Nowości w systemie iOS 11 (Apple)](https://developer.apple.com/ios/)
- [Strona produktu zaktualizowane sklepu App Store (Apple)](https://developer.apple.com/app-store/product-page/)
- [Aktualizowanie aplikacji dla systemu iOS 11 (WWDC) (klip wideo)](https://developer.apple.com/videos/play/wwdc2017/204/)
