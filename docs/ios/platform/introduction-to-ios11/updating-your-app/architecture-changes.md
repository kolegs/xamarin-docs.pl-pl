---
title: Architektura zmiany
description: Poznawanie nowych funkcji systemu IOS 11
ms.prod: xamarin
ms.assetid: 55F62F3F-8570-402B-B7D9-2875F76CB946
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/13/2016
ms.openlocfilehash: 6dd874fd803d2b3de21b0b604cbc2bafa5bf1aec
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="architecture-changes"></a>Architektura zmiany

_Poznawanie nowych funkcji systemu IOS 11_

Jednym z najważniejszych zmian, które należy zwrócić uwagę z systemem iOS 11 jest amortyzacja obsługa 32-bitowych aplikacji, zgodnie z opisem w [firmy Apple](https://developer.apple.com/news/?id=06282017b) naciśnij wersji. Wszystkie nowe aplikacje i aktualizacje istniejących aplikacji musi obsługiwać 64-bitowych. aplikacje 32-bitowych **nie będą uruchamiane** w systemie iOS 11.

Aby zaktualizować aplikację w programie Visual Studio dla komputerów Mac, wykonaj następujące kroki:

1. Kliknij prawym przyciskiem myszy nazwę projektu, aby otworzyć **opcje projektu**.
2. Przejdź do **kompilacji systemu iOS** kartę.
3. Ustaw **obsługiwane architektury** listy rozwijanej **x86_64** dla **Debug | iPhoneSimulator** i **wersji | iPhoneSimulator**:

    ![Zmień architektury symulatora listy rozwijanej](architecture-changes-images/image1.png)

4. Dla urządzeń z systemem iOS, zmień konfigurację, aby **Debug | iPhone** i ustaw **obsługiwane architektury** listy rozwijanej **ARM64**:

    ![Zmień urządzenie architektury listy rozwijanej](architecture-changes-images/image2.png)

5. Zmień konfigurację, aby **wersji | iPhone** i ustaw **obsługiwane architektury** listy rozwijanej **ARM64**.

Aby uzyskać więcej informacji na 32-bitowe i 64-bitowe architektury, zobacz [zagadnień dotyczących platformy 32/x 64](~/cross-platform/macios/32-and-64/index.md#ios) przewodnik.

## <a name="related-links"></a>Linki pokrewne

- [Nowości w systemie iOS 11 (Apple)](https://developer.apple.com/ios/)
- [Strona produktu zaktualizowane sklepu App Store (Apple)](https://developer.apple.com/app-store/product-page/)
- [Aktualizowanie aplikacji dla systemu iOS 11 (WWDC) (klip wideo)](https://developer.apple.com/videos/play/wwdc2017/204/)
