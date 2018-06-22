---
title: 'Błąd podczas przesyłania do sklepu z aplikacjami: "Nieprawidłowy pakiet — opcje nie może być osadzony w kodu bitowego są wykrywane przy przekazywaniu"'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 137313FB-3D29-428B-93C1-5A05DC8F7C03
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: cacb9040ddc8582490c68bcfd24e80c4c4679eb4
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
ms.locfileid: "30777707"
---
# <a name="error-when-submitting-to-app-store-invalid-bundle---options-not-allowed-to-be-embedded-in-bitcode-are-detected-in-the-submission"></a>Błąd podczas przesyłania do sklepu z aplikacjami: "Nieprawidłowy pakiet — opcje nie może być osadzony w kodu bitowego są wykrywane przy przekazywaniu"

watchOS a aplikacjami systemu tvOS _wymagają_ kodu bitowego, gdy są one przesyłane do sklepu z aplikacjami. Podczas tworzenia i przesyłanie aplikacji watchOS i systemu tvOS, za pomocą środowiska Xcode 8.3 lub starszym, (za pośrednictwem poczty e-mail powiadomienia) może wystąpić następujący błąd podczas próby przekazania do sklepu z aplikacjami:

>Nieprawidłowy pakiet - nie można przetworzyć aplikację, ponieważ nie może być osadzony w kodu bitowego opcje są wykrywane przy przekazywaniu. Istnieje prawdopodobieństwo, że nie tworzysz aplikację z łańcuchem narzędzi dostępnych w programie Xcode.

Rozwiązanie tego problemu jest tworzenie aplikacji z Xcode 9 i najnowszą wersję platformy Xamarin.iOS.
