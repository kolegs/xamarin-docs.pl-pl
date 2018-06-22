---
title: Podstawowe informacje na temat aplikacji platformy Xamarin.Android
description: Podstawowe koncepcje aplikacji
ms.prod: xamarin
ms.assetid: 935B8BFE-23B7-4239-5C87-F4A503B889CB
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: caa51fa0a70da1b799d56c706e6de974f61a14d9
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/03/2018
ms.locfileid: "32436433"
---
# <a name="xamarinandroid-application-fundamentals"></a>Podstawowe informacje na temat aplikacji platformy Xamarin.Android

Ta sekcja zawiera przewodnik dla niektórych zadań rzeczy częściej lub pojęcia, które deweloperzy muszą znać podczas opracowywania aplikacji systemu Android.

## <a name="accessibilityandroidapp-fundamentalsaccessibilitymd"></a>[Ułatwienia dostępu](~/android/app-fundamentals/accessibility.md)

Ta strona informacje dotyczące używania interfejsów API systemu Android ułatwień dostępu do skompilowania aplikacji, zgodnie z [Lista kontrolna ułatwień dostępu](~/cross-platform/app-fundamentals/accessibility.md).

##  <a name="understanding-android-api-levelsandroidapp-fundamentalsandroid-api-levelsmd"></a>[Opis poziomów interfejsu API systemu Android](~/android/app-fundamentals/android-api-levels.md)

W tym przewodniku opisano, jak Android używa poziomy interfejsu API do zarządzania zgodności aplikacji w różnych wersjach systemu android oraz wyjaśniono sposób konfigurowania ustawień projektu platformy Xamarin.Android, aby wdrożyć te poziomy interfejsu API w aplikacji. Ponadto w tym przewodniku wyjaśniono, jak napisać kod środowiska uruchomieniowego, która zajmuje się różne poziomy interfejsu API i zapewnia listę odwołania wszystkie poziomy interfejsu API systemu Android, numery wersji (na przykład 8.0 dla systemu Android), Android nazwy kodu (na przykład Oreo) i kody wersji kompilacji.



##  <a name="resources-in-androidandroidapp-fundamentalsresources-in-androidindexmd"></a>[Zasoby w systemie Android](~/android/app-fundamentals/resources-in-android/index.md)

W tym artykule przedstawiono koncepcję zasobów systemu Android, Xamarin.Android i dokumentów sposobu ich używania. Uwzględniono również sposób użycia zasobów w aplikacji systemu Android do obsługi aplikacji lokalizacji i wielu urządzeń, takich jak różnych rozmiarów ekranu i gęstości.




##  <a name="activity-lifecycleandroidapp-fundamentalsactivity-lifecycleindexmd"></a>[Cykl życia aktywności](~/android/app-fundamentals/activity-lifecycle/index.md)

Działania są podstawowych blok tworzenia aplikacji systemu Android i istnieją w wielu innych stanów. Cykl życia działania rozpoczyna się od wystąpienia kończy zniszczenie i zawiera wiele stanów między nimi. Po zmianie stanu działania wywoływana jest metoda zdarzenia cyklu życia odpowiednie, powiadamiania o zbliżającym się zmiana stanu działania, dzięki czemu jej do wykonania kodu w celu dostosowania do tej zmiany. W tym artykule sprawdza cyklu życia działań i wyjaśniono odpowiedzialność to działanie ma podczas każdego z tych zmian stanu jako część aplikacji dobrze behaved, niezawodne.

##  <a name="localizationandroidapp-fundamentalslocalizationmd"></a>[Lokalizacja](~/android/app-fundamentals/localization.md)

W tym artykule wyjaśniono, jak do zlokalizowania Xamarin.Android na inne języki tłumaczenia ciągów i podając obrazy zastępcze.

## <a name="servicesandroidapp-fundamentalsservicesindexmd"></a>[Usługi](~/android/app-fundamentals/services/index.md)

W tym artykule omówiono usług dla systemu Android, które są składniki systemu Android, które umożliwiają pracy w tle. Go opisano różne scenariusze, które usługi są odpowiednie dla oraz sposób ich wdrażania zarówno do wykonywania zadań tła długotrwałe również zapewniający interfejs dla zdalnych wywołań procedur.

## <a name="broadcast-receiversandroidapp-fundamentalsbroadcast-receiversmd"></a>[Odbiorniki emisji](~/android/app-fundamentals/broadcast-receivers.md)

W tym przewodniku opisano sposób tworzenia i używania emisji odbiornikami, składnik systemu Android, który odpowiada na emisje całego systemu, na platformie Xamarin.Android.



##  <a name="permissionsandroidapp-fundamentalspermissionsmd"></a>[Uprawnienia](~/android/app-fundamentals/permissions.md)

Obsługę narzędzi wbudowanych w programie Visual Studio for Mac lub Visual Studio umożliwia tworzenie i Dodaj uprawnienia do manifestu systemu Android. Ten dokument zawiera opis sposobu dodawania uprawnień w programie Visual Studio i Xamarin Studio.



##  <a name="graphics-and-animationandroidapp-fundamentalsgraphics-and-animationmd"></a>[Grafiki i animacje](~/android/app-fundamentals/graphics-and-animation.md)

Android zapewnia bardzo zaawansowane i różnych framework do obsługi 2D grafiki i animacji. Ten dokument zawiera wprowadzenie tych platform oraz opisano, jak utworzyć niestandardowe grafiki i animacji i używać go w aplikacji platformy Xamarin.Android.


##  <a name="cpu-architecturesandroidapp-fundamentalscpu-architecturesmd"></a>[Architektury procesorów](~/android/app-fundamentals/cpu-architectures.md)

Xamarin.Android obsługuje kilka architektury Procesora, łącznie z urządzeniami 32-bitowe i 64-bitowych. W tym artykule wyjaśniono, jak wybrać aplikację do co najmniej jeden architektury Procesora obsługiwany system Android.




##  <a name="handling-rotationandroidapp-fundamentalshandling-rotationmd"></a>[Obsługa obrotu](~/android/app-fundamentals/handling-rotation.md)

W tym artykule opisano sposób obsługi zmiany orientacji urządzenia platformy Xamarin.Android. Uwzględniono również sposób pracy z systemem Android zasobów można automatycznie załadować zasobów w orientacji określonego urządzenia, jak programowo obsługi orientacji zmiany. Przedstawiono techniki zachowanie stanu podczas obracania urządzenia.



##  <a name="android-audioandroidapp-fundamentalsandroid-audiomd"></a>[Android Audio](~/android/app-fundamentals/android-audio.md)

System operacyjny Android zapewnia zaawansowaną obsługę multimediów, obejmujące audio i wideo. Ten przewodnik koncentruje się na audio w systemie Android i obejmuje odtwarzanie oraz nagrywanie dźwięku za pomocą wbudowanych odtwarzacz audio i klasy rejestratora, jak również interfejs API audio niskiego poziomu. Obejmuje ona również pracy ze zdarzeniami Audio emisji przez inne aplikacje, dzięki czemu deweloperzy mogą tworzyć aplikacje dobrze behaved.




##  <a name="notificationsandroidapp-fundamentalsnotificationsindexmd"></a>[Powiadomienia](~/android/app-fundamentals/notifications/index.md)

W tej sekcji wyjaśniono, jak do zaimplementowania powiadomień lokalnych i zdalnych w platformy Xamarin.Android. Zawiera opis różnych elementów interfejsu użytkownika dla systemu Android powiadomień, a w tym artykule omówiono interfejsu API użytkownika związane z tworzenie i wyświetlanie powiadomienie. Dla zdalnego powiadomienia są objaśnione zarówno Google Cloud Messaging i Firebase Cloud Messaging. Szczegółowe wskazówki i przykładowy kod są uwzględniane.



##  <a name="touchandroidapp-fundamentalstouchindexmd"></a>[Dotyk](~/android/app-fundamentals/touch/index.md)

W tej sekcji wyjaśniono, że pojęcia i szczegóły dotyczące implementowania touch gestów w systemie Android. Interfejsy API Touch są wprowadzone i wyjaśniono następuje eksploracji aparaty rozpoznawania gestów.



##  <a name="httpclient-stack-and-ssltlsandroidapp-fundamentalshttp-stackmd"></a>[Stos HttpClient i protokoły SSL/TLS](~/android/app-fundamentals/http-stack.md)

W tej sekcji opisano selektorów HttpClient stosu i implementacji protokołów SSL/TLS dla systemu Android. Te ustawienia określają HttpClient i SSL/TLS implementację, które będą używane przez aplikacje platformy Xamarin.Android.


##  <a name="writing-responsive-applicationswriting-responsive-appsmd"></a>[Pisanie reakcji aplikacji](writing-responsive-apps.md)

W tym artykule omówiono sposób użycia wątkowość do zachowania aplikacji platformy Xamarin.Android reakcji przenosząc długotrwałych zadań do wątku w tle.