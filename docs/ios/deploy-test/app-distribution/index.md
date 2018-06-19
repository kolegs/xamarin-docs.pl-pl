---
title: Omówienie dystrybucji aplikacji platformy Xamarin.iOS
description: Ten dokument zawiera omówienie metod dystrybucji, które są dostępne dla aplikacji platformy Xamarin.iOS i służy jako wskaźnik do bardziej szczegółowych dokumentów na temat.
ms.prod: xamarin
ms.assetid: 341D36DB-BB07-FA94-BCC9-5F8C0B18C179
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 815277e9a4f9384d92bf17376f426cacd40dbc9f
ms.sourcegitcommit: 7a89735aed9ddf89c855fd33928915d72da40c2d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36209443"
---
# <a name="xamarinios-app-distribution-overview"></a>Omówienie dystrybucji aplikacji platformy Xamarin.iOS

_Ten dokument zawiera omówienie metod dystrybucji, które są dostępne dla aplikacji platformy Xamarin.iOS i służy jako wskaźnik do bardziej szczegółowych dokumentów na temat._

Po aplikacji platformy Xamarin.iOS został opracowany, następnym krokiem w cyklu tworzenia oprogramowania jest dystrybucję aplikacji dla użytkowników, jak pokazano w sekcji wyróżnione na poniższym diagramie:


[![](images/publishingdiagram.png "Po aplikacji systemu iOS został opracowany, następnym krokiem jest dystrybucję aplikacji dla użytkowników, jak pokazano w sekcji wyróżnione na tym wykresie")](images/publishingdiagram.png#lightbox)


Apple oferuje następujące sposoby rozpowszechnić aplikację systemu iOS, które są obsługiwane przez Xamarin.iOS:

1. [**Sklep App Store**](#App_Store_Distribution)
2. [**Wewnętrznych (Enterprise)**](#In-House_Distribution)
2. [**Ad Hoc**](#Ad_Hoc_Distribution)

Te scenariusze wymagają aplikacje udostępniane przy użyciu odpowiednich *profil inicjowania obsługi administracyjnej*. Profile inicjowania obsługi administracyjnej są pliki zawierające kod podpisywania informacje, a także tożsamości aplikacji i mechanizmu dystrybucji zamierzone. Dystrybucji sklepu nie zawierają one również informacje dotyczące urządzeń, jakie można wdrożyć aplikację do.

<a name="App_Store_Distribution"/>

## <a name="app-store-distribution"></a>Dystrybucji sklepu z aplikacjami

> [!IMPORTANT]
> Apple [wskazuje](https://developer.apple.com/news/?id=05072018a) czy począwszy 2018 lipca, wszystkie aplikacje i aktualizacje przesłane do sklepu z aplikacjami musi mieć została utworzona za pomocą systemu iOS 11 SDK i [obsługuje wyświetlania iPhone X](~/ios/platform/introduction-to-ios11/updating-your-app/visual-design.md).

Jest to główny sposób, że aplikacje dla systemu iOS są dystrybuowane do klientów na urządzeniach z systemem iOS. Wszystkie aplikacje przesłane do sklepu z aplikacjami wymagają zatwierdzenia przez firmę Apple.

Aplikacje są przesyłane do sklepu z aplikacjami za pośrednictwem portalu o nazwie *iTunes Connect*. [Skonfiguruj aplikację w iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md) przewodnik zapewnia więcej informacji na temat sposobu konfigurowania i używania tego portalu do przygotowania aplikacji platformy Xamarin.iOS do publikowania w sklepie z aplikacjami.

Należy pamiętać, że tylko deweloperów, którzy należą do **programie dla deweloperów firmy Apple** mają dostęp do programu iTunes Connect. Elementy członkowskie **Apple Developer Enterprise Program** nie mają dostępu.

Aby uzyskać więcej informacji, odwiedź stronę [dystrybucji aplikacji sklepu](~/ios/deploy-test/app-distribution/app-store-distribution/index.md) przewodnik.

<a name="In-House_Distribution"/>

## <a name="in-house-distribution"></a>Dystrybucji wewnętrznych

Czasami nazywane *dystrybucji Enterprise*, wewnętrznych dystrybucji umożliwia członkom **Apple Developer Enterprise Program** Aby przeprowadzić dystrybucję aplikacji wewnętrznie na innych elementach członkowskich o tej samej organizacji. Rozkład wewnętrznych ma zalety nie wymagających przeglądu sklepu z aplikacjami oraz o brak limitu liczby urządzeń, na których można zainstalować aplikacji. Jednak ważne jest, aby należy pamiętać, że **Apple Developer Enterprise Program** czy elementy członkowskie **nie** mają dostęp do programu iTunes Connect i w związku z tym Licencjobiorca jest odpowiedzialna za dystrybucję aplikacji.

Aby uzyskać więcej informacji na temat pobierania konfiguracji i sposób dystrybucji we własnym zakresie aplikacji, zapoznaj się [podręczniku dystrybucji wewnętrznych](~/ios/deploy-test/app-distribution/in-house-distribution.md).

<a name="Ad_Hoc_Distribution"/>

## <a name="ad-hoc-distribution"></a>Ad Hoc dystrybucji

Aplikacje platformy Xamarin.iOS mogą być testowane użytkownika za pośrednictwem dystrybucji ad hoc, która jest dostępna zarówno **programie dla deweloperów firmy Apple**i **Apple Developer Enterprise Program**i umożliwia maksymalnie 100 systemu iOS urządzenia, które ma zostać przetestowana. Najlepsze przypadek użycia dystrybucji ad hoc jest dystrybucji w obrębie firmy, gdy iTunes Connect nie jest opcją.

Aby uzyskać więcej informacji na temat pobierania konfiguracji i sposób dystrybucji we własnym zakresie aplikacji, zapoznaj się [podręczniku dystrybucji Ad Hoc](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md).

## <a name="summary"></a>Podsumowanie

W tym artykule udzielił krótkie omówienie mechanizmów dystrybucji, które są dostępne dla aplikacji platformy Xamarin.iOS. Wprowadzono iTunes App Store Ad Hoc i we własnym zakresie wdrożenia i podano łącza do bardziej szczegółowych informacji.

## <a name="related-links"></a>Linki pokrewne

- [Dystrybucja w sklepie App Store](~/ios/deploy-test/app-distribution/app-store-distribution/index.md)
- [Konfigurowanie aplikacji w iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)
- [Publikowanie w sklepie App Store](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)
- [Dystrybucji wewnętrznych](~/ios/deploy-test/app-distribution/in-house-distribution.md)
- [Dystrybucja Ad Hoc](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md)
- [Plik iTunesMetadata.plist](~/ios/deploy-test/app-distribution/itunesmetadata.md)
- [Obsługa IPA](~/ios/deploy-test/app-distribution/ipa-support.md)
- [Rozwiązywanie problemów](~/ios/deploy-test/troubleshooting.md)
