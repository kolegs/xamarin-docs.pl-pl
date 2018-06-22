---
title: Konfigurowanie Twojego systemu tvOS aplikacji w iTunes Connect
description: W tym artykule przedstawiono dodatkowe przewodnik dotyczący systemu iOS skonfiguruj aplikację w iTunes Connect dla systemu tvOS określonej konfiguracji.
ms.prod: xamarin
ms.assetid: 86C7C5BD-C97D-4F1D-B611-A7694557BFDF
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: d082a980572349da1b7e6155b3aa4de41512796f
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
ms.locfileid: "30779829"
---
# <a name="configure-your-tvos-app-in-itunes-connect"></a>Konfigurowanie Twojego systemu tvOS aplikacji w iTunes Connect

_W tym artykule przedstawiono dodatkowe przewodnik dotyczący systemu iOS skonfiguruj aplikację w iTunes Connect dla systemu tvOS określonej konfiguracji._


Oprócz konfiguracje i ustawienia, które należy wprowadzić następujące systemu iOS [skonfiguruj aplikację w iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md) przewodnik, w tym dokumencie opisano określone konfiguracje, które będą musieli wersji Xamarin.tvOS aplikacji w sklepie z aplikacjami firmy Apple TV.

<a name="Adding-a-tvOS-Release-Version" />

## <a name="adding-a-tvos-release-version"></a>Dodawanie systemu tvOS wersji

Czy tworzysz nową aplikację, aby wydanej w dniu sklepu z aplikacjami firmy Apple TV, lub dodawanie obsługi Apple TV do istniejącej aplikacji systemu iOS, musisz utworzeniu iTunes rekordu Connect i skonfigurowaniu go przy użyciu następujących iOS prowadzi określonych:

- [Tworzenie rekordu Connect iTunes](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#creating)
- [Zarządzanie wideo aplikacji i zrzuty ekranu](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#managing)
- [Zarządzanie nazwę, opis nowości, adresy URL i słowa kluczowe](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#metadata)
- [Obsługa informacje ogólne](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#general)

Opcjonalnie można także mogą wymagać:

- [Utrzymywanie informacji z aplikacji Game Center](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#game-center)
- [Utrzymywanie informacji zakupu w aplikacji](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#iap)

Biorąc pod uwagę powyższe kroki ukończone Otwórz iTunes rekordu Connect i wybierz, aby dodać obsługę systemu tvOS, za pomocą paska bocznego po lewej stronie aplikacji:

[![](itunes-connect-images/connect01.png "Dodaj obsługę systemu tvOS, za pomocą paska bocznego po lewej stronie")](itunes-connect-images/connect01.png#lightbox)

Ekrany określone informacje systemu tvOS będzie dostępna dla danej iTunes rekordu Connect:

[![](itunes-connect-images/connect02.png "Na ekranie informacje systemu tvOS")](itunes-connect-images/connect02.png#lightbox)

<a name="tvOS-Version-Information" />

## <a name="tvos-version-information"></a>informacje o wersji systemu tvOS

Na pasku bocznym po lewej stronie zaznacz **1.0 Przygotowanie do przesyłania** sekcji systemu tvOS aplikacji:

[![](itunes-connect-images/connect03.png "informacje o wersji systemu tvOS")](itunes-connect-images/connect03.png#lightbox)

Na tym ekranie wprowadź następujące informacje:

- Wymagane zrzuty ekranu, opis, słowa kluczowe i adresów URL.
- Aplikacji ogólne informacje, takie jak numer wersji, Copyright i wiek klasyfikacji.
- Opcjonalne zakupy w aplikacji.
- Obsługa opcjonalnie Game Center tablice wyników i osiągnięcia.
- Wymagane informacje Przegląd aplikacji takich jak kontaktu, demonstracyjna konta i notatek.

Po wprowadzeniu wymaganych informacji, kliknij przycisk **zapisać** przycisk w prawym górnym rogu ekranu, aby zapisać zmiany:

[![](itunes-connect-images/connect04.png "gotowy do przesyłania informacji o wersji systemu tvOS")](itunes-connect-images/connect04.png#lightbox)

<a name="Submitting-for-Review" />

## <a name="preparing-to-submit-for-review"></a>Trwa przygotowywanie do przesyłania do przeglądu

Gdy wszystko będzie gotowe na koniec można przesłać Xamarin.tvOS aplikacji do sklepu Apple TV aplikacji do przeglądu, wróć do aplikacji iTunes rekordu Connect i kliknij przycisk **przesłać do przeglądu** przycisk w prawym górnym rogu ekranu:

[![](itunes-connect-images/connect05.png "Przesłać do przeglądu")](itunes-connect-images/connect05.png#lightbox)

<a name="Summary" />

## <a name="summary"></a>Podsumowanie

W tym artykule udzielił Przegląd systemu tvOS określonego ustawienia wymagane w iTunes Connect do wydania systemu tvOS aplikacji do sklepu z aplikacjami firmy Apple TV.



## <a name="related-links"></a>Linki pokrewne

- [Przykłady dla systemu tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [systemu tvOS człowieka przewodniki — interfejs](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Przewodnik programowania w języku aplikacji dla systemu tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
