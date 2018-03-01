---
title: Publikowanie aplikacji
ms.topic: article
ms.prod: xamarin
ms.assetid: 51E19000-040A-2B74-C462-EC57C617085C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 99f66fd0d23f14224bcd915ef7d1c6d81367f173
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="publishing-an-application"></a>Publikowanie aplikacji

Po utworzeniu aplikacji dużą osoby będą z niego korzystać. W tej sekcji opisano kroki związane z publicznej dystrybucji aplikacji utworzonej z platformy Xamarin.Android kanałami, takie jak wiadomości e-mail, serwera sieci web prywatne, Google Play lub Amazon sklepu z aplikacjami dla systemu Android.

<a name="Overview" />

## <a name="overview"></a>Omówienie

Ostatni krok w rozwoju aplikacji platformy Xamarin.Android jest, aby opublikować aplikację. Publikowanie to proces tworzenia aplikacji platformy Xamarin.Android, dzięki czemu jest gotowy, użytkownicy mogą zainstalować na swoich urządzeniach i obejmuje dwa podstawowe zadania:

-   **Przygotowywanie do publikacji** &ndash; tworzone są wersji aplikacji, które można wdrożyć na urządzeniach Android zasilania (zobacz [przygotowywanie aplikacji dla wersji](~/android/deploy-test/release-prep/index.md) uzyskać więcej informacji o Release przygotowywania).

-   **Dystrybucji** &ndash; wersji aplikacji ma zostać udostępnione za pośrednictwem jednego lub kilku różnych kanałów dystrybucji.

Na poniższym diagramie przedstawiono kroki związane z publikowaniem aplikacji platformy Xamarin.Android:

[ ![Skompiluj i wdróż schemat blokowy](images/build-and-deploy-steps.png)](images/build-and-deploy-steps.png)

Jak widać w powyższym diagramie, przygotowania jest taki sam, niezależnie od używanej metody dystrybucji. Istnieje kilka metod można zwolnić aplikacji systemu Android dla użytkowników:

-   **Za pośrednictwem witryny sieci Web** &ndash; aplikacji platformy Xamarin.Android A mogą być udostępniane do pobrania z witryny sieci Web, z której użytkownicy mogą następnie zainstaluj aplikację, klikając łącze.
-   **Za pośrednictwem poczty e-mail** &ndash; umożliwiające użytkownikom instalowanie aplikacji platformy Xamarin.Android z poczty e-mail. Aplikacja zostanie zainstalowana po otwarciu załącznika z urządzenia z systemem Android zasilania.
-   **Przez rynek** &ndash; istnieje kilka rynków aplikacji, które istnieją dystrybucji, takich jak [Google Play](http://play.google.com/) lub [Amazon sklepu z aplikacjami dla systemu Android](http://www.amazon.com/mobile-apps/b?ie=UTF8&node=2350149011) .


Używanie ustalonych marketplace jest najczęściej do publikowania aplikacji, ponieważ zapewnia najszerszych zasięg rynku i największą kontrolę nad dystrybucji. Jednak publikowania aplikacji za pośrednictwem portalu marketplace wymaga dodatkowego nakładu pracy.

Kanały można rozpowszechniać jednocześnie aplikacji platformy Xamarin.Android. Na przykład aplikacji mogą być publikowane w witrynie Google Play, Amazon sklepu z aplikacjami dla systemu Android i również pobrać z serwera sieci web.

Najbardziej przydatny w przypadku kontrolowane podzbiór użytkowników, takich jak środowisku przedsiębiorstwa lub aplikacji, która jest przeznaczone tylko dla małych lub dobrze określony zbiór użytkowników są dwie metody dystrybucji (pobierania lub e-mail).
Serwer i Dystrybucja poczty e-mail są również prostsze publikowania modeli, wymagających mniej przygotowania do publikowania aplikacji.

Program dystrybucji aplikacji mobilnej Amazon umożliwia deweloperom aplikacji mobilnej do rozpowszechniania i sprzedawania ich aplikacji w portalu Amazon. Użytkownicy mogą odnajdywać i zakupów dla aplikacji na urządzeniach Android zasilania za pomocą aplikacji do sklepu z aplikacjami firmy Amazon. Zrzut ekranu przedstawiający sklepu z aplikacjami firmy Amazon uruchomione na urządzeniu z systemem Android pojawia się poniżej:

Google Play jest raczej marketplace kompleksowe i najbardziej popularnych dla aplikacji systemu Android. Google Play umożliwia użytkownikom odnajdywania, Pobierz szybkości i zwrócić dla aplikacji, klikając pojedyncza ikona na urządzeniu lub na komputerze. Google Play zawiera także narzędzi ułatwiających analizy sprzedaży i trendów rynku i do kontrolowania, które urządzenia i użytkownicy mogą pobrać aplikację. Zrzut ekranu uruchamiania na urządzeniu z systemem Android w sklepie Google Play pojawia się poniżej:

[ ![Zrzut ekranu Google Play](images/google-play-app.png)](images/google-play-app.png)

Tej sekcji przedstawiono sposób przekazywania aplikacji do sklepu takich jak Google Play, wraz z odpowiednią materiały promocyjne. Opisano APK rozszerzenia plików, zapewniając omówienie co to są i jak działają. Opisano również usług Google licencjonowania. Na koniec alternatywne metody dystrybucji zostały wprowadzone, łącznie z użyciem protokołu HTTP serwera sieci web, dystrybucji proste poczty e-mail i sklepu z aplikacjami firmy Amazon dla systemu Android.


## <a name="related-links"></a>Linki pokrewne

- [HelloWorldPublishing (przykład)](https://developer.xamarin.com/samples/monodroid/HelloWorldPublishing/)
- [Proces kompilacji](~/android/deploy-test/building-apps/build-process.md)
- [Konsolidacja](~/android/deploy-test/linker.md)
- [Uzyskiwanie Google mapuje klucz interfejsu API](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md)
- [Podpisywania aplikacji](https://source.android.com/security/apksigning/)
- [Publikowanie w witrynie Google Play](http://developer.android.com/distribute/googleplay/publish/index.html)
- [Licencjonowanie aplikacji Google](http://developer.android.com/guide/google/play/licensing/index.html)
- [Android.Play.ExpansionLibrary](https://github.com/mattleibow/Android.Play.ExpansionLibrary)
- [Portal dystrybucji aplikacji mobilnej](https://developer.amazon.com/welcome.html)
- [Dystrybucji aplikacji mobilnej Amazon — często zadawane pytania](https://developer.amazon.com/help/faq.html)
