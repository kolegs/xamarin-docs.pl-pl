---
title: Zasoby dla systemu android
description: "W tym artykule pojęcia związane z Android zasobów na platformie Xamarin.Android i będzie udokumentować sposób ich użycia. Uwzględniono również sposób użycia zasobów w aplikacji systemu Android do obsługi aplikacji lokalizacji i wielu urządzeń, takich jak różnych rozmiarów ekranu i gęstości."
ms.topic: article
ms.prod: xamarin
ms.assetid: C0DCC856-FA36-04CD-443F-68D26075649E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/01/2018
ms.openlocfilehash: 6546870d85f7b77e60dff0cb9e6075f982c9cb8e
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="android-resources"></a>Zasoby dla systemu android

_W tym artykule pojęcia związane z Android zasobów na platformie Xamarin.Android i będzie udokumentować sposób ich użycia. Uwzględniono również sposób użycia zasobów w aplikacji systemu Android do obsługi aplikacji lokalizacji i wielu urządzeń, takich jak różnych rozmiarów ekranu i gęstości._


## <a name="overview"></a>Omówienie

Aplikacja systemu Android rzadko jest tylko kod źródłowy. Są często wiele plików, które tworzą aplikacji: wideo, obrazy, czcionki i plików audio, tylko w celu kilka. Zbiorczo te pliki kodu źródłowego z systemem innym niż są określane jako zasobów i kompilowania (wraz z kodu źródłowego) podczas procesu kompilacji i dostarczana w APK dla dystrybucji i instalacji na urządzeniach:

![Diagram tworzenia pakietów](images/packaging-diagram.png)

Zasoby oferują wiele korzyści, do aplikacji systemu Android:

-  **Separacją kodu** &ndash; oddziela kodu źródłowego z obrazów, ciągi, menu, animacji kolorów,... itd. Jako takie zasoby mogą pomóc znacznie przy lokalizacji.

-  **Wiele urządzeń** &ndash; umożliwia łatwiejsze konfiguracji różnych urządzeń bez zmian w kodzie.

-  **Sprawdzanie kompilacji** &ndash; zasoby są statyczne i skompilowanych do aplikacji. Umożliwia to użycie zasobów, które mają być sprawdzane w czasie kompilacji, gdy będzie łatwo catch i poprawić błędy, w przeciwieństwie do czasu wykonywania, gdy jest bardziej trudne do zlokalizowania i kosztowne rozwiązać.

Po uruchomieniu nowego projektu platformy Xamarin.Android specjalne katalog o nazwie zasobów jest tworzone wraz z niektórych podkatalogi:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Folder zasobów i zawartość](images/resources-folder-vs.png)

Na ilustracji powyżej zasoby aplikacji są zorganizowane według ich typu w tych podkatalogach: obrazy zostanie umieszczona **obiektów drawable** katalog; widoków go w programie **układu** podkatalogu itp.
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Folder zasobów i zawartość](images/resources-folder-xs.png)

Na ilustracji powyżej zasoby aplikacji są zorganizowane według ich typu w tych podkatalogach: obrazy zostanie umieszczona **mipmap** katalog; widoków go w programie **układu** podkatalogu itp.
 
-----

Istnieją dwa sposoby dostępu do tych zasobów w aplikacji platformy Xamarin.Android: *programowo* w kodzie i *deklaratywnie* w kodzie XML przy użyciu specjalnej składni XML.

Te zasoby są nazywane *zasoby domyślne* i są używane przez wszystkie urządzenia, chyba że określony jest dokładniejsze dopasowanie. Ponadto każdy typ zasobu opcjonalnie może mieć *alternatywnych zasobów* że Android mogą korzystać z pod kątem konkretnych urządzeń. Na przykład można podać zasobów pod kątem ustawień regionalnych użytkownika, do rozmiaru ekranu lub jeśli urządzenie jest obrócony o 90 stopni z pionowej na poziomą, itp. W każdym z tych przypadków Android załaduje zasobów przeznaczonych do użycia przez aplikację bez żadnych dodatkowych kodowania wysiłku przez dewelopera.

Alternatywne zasoby są określone przez dodanie krótkich ciągów, nazywany *kwalifikator*, koniec katalog zawierający dany typ zasobów.

Na przykład **zasobów/obiektów drawable-de** będzie określać obrazów dla urządzeń, które są ustawiane w ustawieniach regionalnych niemieckim, podczas **zasobów/obiektów drawable-fr** zawiera obrazy urządzeń ustawioną francuskim ustawień regionalnych. Przykład zapewnienia alternatywnych zasobów są widoczne na obrazie poniżej której ta sama aplikacja jest uruchamiana tylko ustawienia regionalne zmiany urządzenia:

![Przykład ekrany dla różnych ustawień regionalnych](images/localized-screenshots.png)

W tym artykule będzie kompleksowe Spójrz na korzystanie z zasobów i obejmować następujące tematy:

-  **Android podstawy zasobów** &ndash; przy użyciu domyślnych zasobów programowo i deklaratywnie, dodawanie typów zasobów, takich jak obrazy i czcionki do aplikacji.

-  **Konfiguracja urządzenia** &ndash; obsługi różnych rozdzielczości ekranu i gęstości w aplikacji.

-  **Lokalizacja** &ndash; przy użyciu zasobów do obsługi różnych regionach, aplikacja może być używana.


## <a name="related-links"></a>Linki pokrewne

- [Używanie elementów zawartości systemu Android](~/android/app-fundamentals/resources-in-android/android-assets.md)
- [Podstawy aplikacji](http://developer.android.com/guide/topics/fundamentals.html)
- [Zasoby aplikacji](http://developer.android.com/guide/topics/resources/index.html)
- [Obsługa wielu ekranów](http://developer.android.com/guide/practices/screens_support.html)
