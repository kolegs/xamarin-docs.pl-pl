---
title: Reguły analizy platformy Xamarin.iOS
description: W tym dokumencie opisano zestaw reguł analizy, które Sprawdź ustawienia projektu platformy Xamarin.iOS w celu określenia, czy ustawienia więcej/better-optimized są dostępne.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: C29B69F5-08E4-4DCC-831E-7FD692AB0886
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/06/2018
ms.openlocfilehash: 25d2936f70981ed239626193ba6e4993f1378108
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34788901"
---
# <a name="xamarinios-analysis-rules"></a>Reguły analizy platformy Xamarin.iOS

Analiza platformy Xamarin.iOS to zestaw reguł, które sprawdzić ustawienia projektu, aby pomóc w określeniu, czy lepiej większej zoptymalizowane ustawienia są dostępne.

Uruchom tak często, jak to możliwe na wczesnym etapie znalezienia możliwych ulepszeń oraz skrócić czas tworzenia reguł analizy.

Aby uruchomić reguły, w programie Visual Studio dla komputerów Mac w menu, wybierz **projektu > Uruchom analizę kodu**.

> [!NOTE]
> Analiza platformy Xamarin.iOS działa tylko w aktualnie wybranej konfiguracji. Zdecydowanie zaleca się uruchomienie narzędzia do debugowania **i** konfiguracje wydania.

<a name="XIA0001" />

## <a name="xia0001-disabledlinkerrule"></a>XIA0001: DisabledLinkerRule

- **Problem:** konsolidator jest wyłączona na urządzeniu w trybie debugowania.
- **Poprawka:** należy spróbować uruchomić kod z konsolidator, aby uniknąć wszelkich niespodzianki.
Aby go skonfigurować, przejdź do projektu > kompilacji systemu iOS > zachowanie konsolidatora.

<a name="XIA0002" />

## <a name="xia0002-testcloudagentreleaserule"></a>XIA0002: TestCloudAgentReleaseRule

- **Problem:** kompilacje aplikacji, które zainicjować agenta chmury testowej zostanie odrzucony przez firmę Apple podczas przesyłania, ponieważ używają one prywatnego interfejsu API.
- **Poprawka:** Dodaj lub usuń konieczne #if i definiuje w kodzie.

<a name="XIA0003" />

## <a name="xia0003-ipadebugbuildsrule"></a>XIA0003: IPADebugBuildsRule

- **Problem:** konfigurację debugowania, który korzysta z kluczy podpisywania developer nie powinna generować IPA tylko jest wymagana do dystrybucji, którego obecnie korzysta z Kreatora publikacji.
- **Poprawka:** wyłączyć IPA kompilacji w opcje projektu dla konfiguracji debugowania.

<a name="XIA0004" />

## <a name="xia0004-missing64bitsupportrule"></a>XIA0004: Missing64BitSupportRule

- **Problem:** obsługiwana architektura dla "wersji | urządzenie"nie jest zgodny, brak ARM64 64-bitowym. Jest to problem, ponieważ firmy Apple nie akceptuje tylko aplikacje systemu iOS 32-bitowy w sklepu AppStore.
- **Poprawka:** podwójne kliknięcie projektu systemu iOS, przejdź do kompilacji > iOS kompilacji i zmienić obsługiwane architektury, więc ARM64.

<a name="XIA0005" />

## <a name="xia0005-float32rule"></a>XIA0005: Float32Rule

- **Problem:** nie przy użyciu opcji float32 (--Opcje drzewa obiektów aplikacji = O = float32) prowadzi do hefty wydajność, szczególnie w przypadku urządzeń przenośnych, gdzie jest widoczny wolniejsze Podwójna precyzja matematycznych. Zwróć uwagę, że .NET używa podwójnej precyzji wewnętrznie, nawet w przypadku liczb zmiennoprzecinkowych, dlatego włączenie tej opcji ma wpływ na dokładność i prawdopodobnie zgodności.
- **Poprawka:** podwójne kliknięcie projektu systemu iOS, przejdź do kompilacji > iOS kompilacji i usuń zaznaczenie pola wyboru "Wykonywać wszystkie operacje 32-bitowych liczb zmiennoprzecinkowych jako 64-bit float".

<a name="XIA0006" />

## <a name="xia0006-httpclientavoidmanaged"></a>XIA0006: HttpClientAvoidManaged

- **Problem:** zaleca się używanie natywnej obsługi HttpClient zamiast zarządzanego w celu poprawy wydajności mniejszego rozmiaru pliku wykonywalnego i umożliwia łatwą obsługę nowszych standardów.
- **Poprawka:** podwójne kliknięcie projektu systemu iOS, przejdź do kompilacji > iOS kompilacji i zmień implementację HttpClient NSUrlSession (iOS 7 +) albo CFNetwork obsługi wersji poprzedzającej systemów iOS 7.
