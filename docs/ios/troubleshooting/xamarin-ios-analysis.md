---
title: "Reguły analizy platformy Xamarin.iOS"
ms.topic: article
ms.prod: xamarin
ms.assetid: C29B69F5-08E4-4DCC-831E-7FD692AB0886
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/26/2017
ms.openlocfilehash: 7cf627f369b666bb54d0f512dc1361d2a685a057
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="xamarinios-analysis-rules"></a>Reguły analizy platformy Xamarin.iOS


## <a name="a-namexia0001xia0001-disabledlinkerrule"></a><a name="XIA0001"/>XIA0001: DisabledLinkerRule

- **Problem:** konsolidator jest wyłączona na urządzeniu w trybie debugowania.
- **Poprawka:** należy spróbować uruchomić kod z konsolidator, aby uniknąć wszelkich niespodzianki.
Aby go skonfigurować, przejdź do projektu > kompilacji systemu iOS > zachowanie konsolidatora.

## <a name="a-namexia0002xia0002-testcloudagentreleaserule"></a><a name="XIA0002"/>XIA0002: TestCloudAgentReleaseRule

- **Problem:** kompilacje aplikacji, które zainicjować agenta chmury testowej zostanie odrzucony przez firmę Apple podczas przesyłania, ponieważ używają one prywatnego interfejsu API.
- **Poprawka:** Dodaj lub usuń konieczne #if i definiuje w kodzie.

## <a name="a-namexia0003xia0003-ipadebugbuildsrule"></a><a name="XIA0003"/>XIA0003: IPADebugBuildsRule

- **Problem:** konfigurację debugowania, który korzysta z kluczy podpisywania developer nie powinna generować IPA tylko jest wymagana do dystrybucji, którego obecnie korzysta z Kreatora publikacji.
- **Poprawka:** wyłączyć IPA kompilacji w opcje projektu dla konfiguracji debugowania.

## <a name="a-namexia0004xia0004-missing64bitsupportrule"></a><a name="XIA0004"/>XIA0004: Missing64BitSupportRule

- **Problem:** obsługiwana architektura dla "wersji | urządzenie"nie jest zgodny, brak ARM64 64-bitowym. Jest to problem, ponieważ firmy Apple nie akceptuje tylko aplikacje systemu iOS 32-bitowy w sklepu AppStore.
- **Poprawka:** podwójne kliknięcie projektu systemu iOS, przejdź do kompilacji > iOS kompilacji i zmienić obsługiwane architektury, więc ARM64.

## <a name="a-namexia0005xia0005-float32rule"></a><a name="XIA0005"/>XIA0005: Float32Rule

- **Problem:** nie przy użyciu opcji float32 (--Opcje drzewa obiektów aplikacji = O = float32) prowadzi do hefty wydajności koszt specjalnie na urządzeń przenośnych, gdzie jest widoczny wolniejsze Podwójna precyzja matematycznych. Zwróć uwagę, że .NET używa podwójnej precyzji wewnętrznie, nawet w przypadku liczb zmiennoprzecinkowych, dlatego włączenie tej opcji ma wpływ na dokładność i prawdopodobnie zgodności.
- **Poprawka:** podwójne kliknięcie projektu systemu iOS, przejdź do kompilacji > iOS kompilacji i usuń zaznaczenie pola wyboru "Wykonywać wszystkie operacje 32-bitowych liczb zmiennoprzecinkowych jako 64-bit float".
