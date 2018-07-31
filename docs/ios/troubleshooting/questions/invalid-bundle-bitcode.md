---
title: 'Błąd podczas przesyłania do App Store: "Nieprawidłowy pakiet — opcje nie mogą być osadzone w bitcode są wykrywane w przesyłania"'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 137313FB-3D29-428B-93C1-5A05DC8F7C03
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/03/2018
ms.openlocfilehash: 393c1ed81c68d21b610781dfe09de97969e031d1
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350927"
---
# <a name="error-when-submitting-to-app-store-invalid-bundle---options-not-allowed-to-be-embedded-in-bitcode-are-detected-in-the-submission"></a>Błąd podczas przesyłania do App Store: "Nieprawidłowy pakiet — opcje nie mogą być osadzone w bitcode są wykrywane w przesyłania"

systemu watchOS aplikacje i aplikacje dla systemu tvOS _wymagają_ bitcode przesyłanej do Store aplikacji. Podczas kompilowania i przesyłanie aplikacji systemu watchOS i tvOS za pomocą środowiska Xcode 8.3 lub starszym, (za pośrednictwem powiadomienia e-mail) może wystąpić następujący błąd podczas próby przekazania do Store aplikacji:

>Nieprawidłowy pakiet — nie można przetworzyć aplikację, ponieważ wykryto opcje, które nie mogą być osadzone w bitcode przy przekazywaniu. Prawdopodobnie są nie Kompilowanie aplikacji za pomocą łańcucha udostępniane w środowisku Xcode.

Rozwiązanie tego problemu jest tworzyć aplikacje za pomocą środowiska Xcode 9 i najnowszej wersji rozszerzenia Xamarin.iOS.
