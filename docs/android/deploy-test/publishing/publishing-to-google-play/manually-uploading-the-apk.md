---
title: "Ręcznie przekazać plik APK"
ms.topic: article
ms.prod: xamarin
ms.assetid: 1309C251-ABF0-4412-B1F5-200DC8321A9D
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: 37e38ddd84b50709bec147c54cdfa9f79404a39f
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="manually-uploading-the-apk"></a>Ręcznie przekazać plik APK


Przy pierwszym przesłaniu APK Google Play (lub jeśli używana jest wcześniejszą wersję platformy Xamarin.Android) plik APK, należy ręcznie przekazać za pośrednictwem [odtwarzanie konsoli dla deweloperów Google](https://play.google.com/apps/publish). W tym przewodniku opisano kroki wymagane do tego procesu. 


## <a name="google-play-developer-console"></a>Konsoli dla deweloperów Google Play

Po skompilowaniu plik APK i promocyjna zasoby przygotowany, aplikacji, należy przekazać do witryny Google Play. W tym celu należy zalogować się do [odtwarzanie konsoli dla deweloperów Google](https://play.google.com/apps/publish), obok pokazano. Kliknij przycisk **publikowania aplikacji systemu Android w witrynie Google Play** przycisk, aby zainicjować proces dystrybucji aplikacji.

[![Konsoli dla deweloperów Google Play](manually-uploading-the-apk-images/00-google-play-developer-console-sml.png)](manually-uploading-the-apk-images/00-google-play-developer-console.png#lightbox)

Jeśli masz już istniejącą aplikację zarejestrowana w usłudze Google Play, kliknij przycisk **Dodaj nową aplikację** przycisk:

[![Dodaj nowy przycisk aplikacji](manually-uploading-the-apk-images/01-existing-app-sml.png)](manually-uploading-the-apk-images/01-existing-app.png#lightbox)

Gdy **Dodaj nową APLIKACJĘ** zostanie wyświetlone okno dialogowe, wprowadź nazwę aplikacji i kliknij pozycję **przekazać APK**:

[![Przekaż przycisk APK](manually-uploading-the-apk-images/02-add-new-application-sml.png)](manually-uploading-the-apk-images/02-add-new-application.png#lightbox)

Następnym ekranie umożliwia aplikacji do opublikowania alfa testowania, testowania wersji beta lub produkcji. W poniższym przykładzie **testowania ALFA** wybrana karta. Ponieważ **moja_aplikacja** nie używa licencjonowania usług **klucz licencji Get** przycisku nie musi być wybrana dla tego przykładu. W tym miejscu **przekazać Twojego pierwszego APK do alfa** przycisku do publikowania kanału alfa:

[![Przekaż Twojego pierwszego APK przycisk alfa](manually-uploading-the-apk-images/03-upload-to-alpha-sml.png)](manually-uploading-the-apk-images/03-upload-to-alpha.png#lightbox)

**Przekazać nowy APK do ALFA** zostanie wyświetlone okno dialogowe. Można przekazać plik APK albo klikając **przeglądanie plików z** przycisk lub przez przeciąganie i upuszczanie plik APK: 

[![Przekaż nowy APK do okna dialogowego alfa](manually-uploading-the-apk-images/04-upload-dialog-sml.png)](manually-uploading-the-apk-images/04-upload-dialog.png#lightbox)

Pamiętaj przekazać plik APK gotowe do wydania, który ma być dystrybuowane.
Następnego okna dialogowego wskazuje postęp przekazywania APK:

[![Przekaż wskazanie postępu](manually-uploading-the-apk-images/05-upload-progress-sml.png)](manually-uploading-the-apk-images/05-upload-progress.png#lightbox)

Po przekazaniu plik APK, możliwe jest na wybór metody testowania:

[![Wybierz okno dialogowe testowania — metoda](manually-uploading-the-apk-images/06-select-testing-method-sml.png)](manually-uploading-the-apk-images/06-select-testing-method.png#lightbox)

Aby uzyskać więcej informacji na temat testowania aplikacji, zobacz firmy Google [Konfigurowanie testy alfa/beta](https://support.google.com/googleplay/android-developer/answer/3131213?hl=en) przewodnik.

Po przekazaniu plik APK jest zapisywany jako projekt. Nie można opublikować, dopóki bardziej szczegółowe informacje znajdują się do witryny Google Play zgodnie z opisem.


## <a name="store-listing"></a>Lista magazynu

Kliknij przycisk **wyświetlania magazynu** w **odtwarzanie konsoli dla deweloperów Google** do Podaj informacje o Google Play będzie wyświetlana użytkownikom możliwości aplikacji: 

[![Okno dialogowe listy magazynu](manually-uploading-the-apk-images/07-store-listing-sml.png)](manually-uploading-the-apk-images/07-store-listing.png#lightbox)


### <a name="graphics-assets"></a>Zasoby graficzne

Przewiń w dół do **zasobów GRAFIKI** sekcji **wyświetlania magazynu** strony:

[![Sekcja zasoby graficzne](manually-uploading-the-apk-images/08-graphic-assets-sml.png)](manually-uploading-the-apk-images/08-graphic-assets.png#lightbox)

Wszystkie zasoby promocyjne, które zostały wcześniej przygotowane, są przekazywane w tej sekcji. Wskazówki są podane określające, jakie zasoby promocyjne, należy określić czy format powinny być podawane w.


### <a name="categorization"></a>Kategoryzacji

Po **zasobów GRAFIKI** sekcja jest **KATEGORYZACJI** wybierz typ aplikacji i kategorii:

[![Sekcja kategorii](manually-uploading-the-apk-images/09-categorization-sml.png)](manually-uploading-the-apk-images/09-categorization.png#lightbox)

Po następnej sekcji omówiono klasyfikacji zawartości.


### <a name="contact-details"></a>Szczegóły dotyczące kontaktu

Ostatnia część ta strona jest **szczegóły kontaktu** sekcji. W tej sekcji służy do zbierania informacji kontaktowych o Deweloper aplikacji:

[![Sekcja szczegóły kontaktu](manually-uploading-the-apk-images/10-contact-details-sml.png)](manually-uploading-the-apk-images/10-contact-details.png#lightbox)

Można podać adres URL aplikacji w zasadach zachowania poufności **zasady zachowania poufności informacji** sekcji opisane powyżej.


## <a name="content-rating"></a>Ocena zawartości

Kliknij przycisk **ocena zawartości** w **konsoli dla deweloperów Google Play**. Na tej stronie można określić klasyfikacji zawartości dla aplikacji. Google Play wymaga, aby wszystkie aplikacje Określ klasyfikacji zawartości. Kliknij przycisk **Kontynuuj** przycisk, aby ukończyć kwestionariusz klasyfikacji zawartości:

[![Sekcja klasyfikacji zawartości](manually-uploading-the-apk-images/11-content-rating-sml.png)](manually-uploading-the-apk-images/11-content-rating.png#lightbox)

Wszystkie aplikacje w sklepie Google Play musi sklasyfikowane zgodnie z system klasyfikacji Google Play. Oprócz klasyfikacji zawartości, wszystkie aplikacje muszą być zgodne z firmy Google [Developer zawartości zasad](http://www.android.com/us/developer-content-policy.html).

Poniżej wymieniono czterech poziomów w systemie klasyfikacji Google Play i zawiera zasady jako funkcji lub zawartości, które wymagałyby lub wymusić poziomu klasyfikacji: 

-   **Wszyscy** &ndash; nie może uzyskać dostęp, publikowanie i udostępnianie danych lokalizacji. Nie mogą udostępniać zawartość wygenerowaną przez użytkowników. Nie może włączyć komunikację między użytkownikami. 

-   **Niska dojrzałości** &ndash; aplikacjom dostęp, ale nie udostępniają danych lokalizacji. Sceny łagodne lub kreskówki przemocy. 

-   **Średnia dojrzałości** &ndash; odwołania do DS, alkohol lub tobacco. Motywy hazard lub hazard symulowane. Obraźliwych zawartość. Niestosownych wyrażeń lub surowego humor. Sugerującą lub seksualnego odwołań. 
    Przemocy intensywny wirtualnych. Realistyczne przemocy. Umożliwia użytkownikom znajdowanie siebie nawzajem. Dzięki czemu użytkownicy mogą komunikować się ze sobą. 
    Udostępnianie danych lokalizacji użytkownika. 

-   **Wysoka dojrzałości** &ndash; fokus na zużycie lub sprzedaż alkoholu, tobacco lub ds. Fokus na sugerującą lub seksualnego odwołań. Graficzne przemocy. 

Elementy na liście średnia dojrzałości są subiektywne, jako takie jest możliwe, że wskazówki, które mogą być wymagane przez ocenę średnia dojrzałości mogą być intensywny, aby zagwarantować klasyfikacji dojrzałości wysokiej. 


## <a name="pricing-amp-distribution"></a>Cennik &amp; dystrybucji

Kliknij przycisk **ceny i dystrybucji** w **konsoli dla deweloperów Google Play**. Na tej stronie określić cenę, jeśli jest to aplikacja płatną.
Alternatywnie aplikacji mogą być dystrybuowane bezpłatne dla wszystkich użytkowników. Gdy aplikacja jest określony jako wolne, musi pozostać wolne.
Google Play nie zezwala na aplikację, która może zostać zmieniony na aplikację o cenach (jednak jest możliwe do sprzedaży zawartości za pomocą rozliczeń w aplikacji z bezpłatną aplikację). Google Play umożliwi płatnych aplikacji można zmienić na bezpłatną aplikację w dowolnym momencie.

Konto handlowe jest wymagany do przed opublikowaniem aplikacji płatną. Aby to zrobić, kliknij przycisk **skonfigurować konto handlowe** i postępuj zgodnie z instrukcjami.

[![Cennik i dystrybucji okna dialogowego](manually-uploading-the-apk-images/12-pricing-sml.png)](manually-uploading-the-apk-images/12-pricing.png#lightbox)


### <a name="manage-countries"></a>Zarządzanie krajach

Następna sekcja **Zarządzanie krajach**, zapewnia kontrolę nad jakie krajach aplikacji może być distibuted do:

[![Okno dialogowe krajach Zarządzanie](manually-uploading-the-apk-images/13-manage-countries-sml.png)](manually-uploading-the-apk-images/13-manage-countries.png#lightbox)


### <a name="other-information"></a>Inne informacje

Przewiń w dół dalej do określenia, czy ta aplikacja zawiera reklam. Ponadto **kategorie urządzeń** sekcji udostępnia opcje umożliwiające opcjonalnie rozpowszechnianie aplikacji Android nosić, Android TV lub automatycznie Android:

[![Zawiera sekcja reklam](manually-uploading-the-apk-images/14-contains-ads-sml.png)](manually-uploading-the-apk-images/14-contains-ads.png#lightbox)

Po tej sekcji są dodatkowe opcje, które można wybrać, takich jak Rezygnacja do **przeznaczone dla rodziny** i dystrybucja aplikacji za pomocą usługi Google Play dla instytucji edukacyjnych.


### <a name="consent"></a>Zgody

W dolnej części **cennik &amp; dystrybucji** strona jest **zgody** sekcji.
To jest obowiązkowy sekcji i służy do deklarowania, czy aplikacja spełnia [Android wytyczne zawartości](http://www.android.com/market/terms/developer-content-policy.html#hl=us) i potwierdzenia, że aplikacja podlega przepisom USA:

[![Sekcja zgody](manually-uploading-the-apk-images/15-consent-sml.png)](manually-uploading-the-apk-images/15-consent.png#lightbox)

Jest znacznie więcej do publikowania aplikacji platformy Xamarin.Android nie mogą być uwzględnione w tym przewodniku.
Aby uzyskać więcej informacji na temat publikowania aplikacji w sklepie Google Play, zobacz [Witamy w Centrum pomocy konsoli deweloperów Google Play](https://support.google.com/googleplay/android-developer#topic=3450769).



## <a name="google-play-filters"></a>Filtry Google Play

Podczas przeglądania witryny Google Play dla aplikacji, są one mogą przeszukiwać wszystkie opublikowane aplikacje. Gdy użytkownicy przeglądają Google Play z urządzenia z systemem Android, wyniki są nieco inne. Wyniki będą filtrowane zgodnie z zgodności urządzenia, który jest używany. Na przykład jeśli aplikacja może wysyłać wiadomości SMS, następnie Google Play nie wyświetli tej aplikacji na dowolnych urządzeniach, którego nie można wysłać wiadomości SMS. Filtry, które są stosowane do wyszukiwania są tworzone z następujących czynności:

1.  Konfiguracja sprzętowa urządzenia.
2.  Deklaracje w aplikacjach pliku manifestu.
3.  Operatora, który jest używany (jeśli istnieje).
4.  Lokalizacja urządzenia.

Istnieje możliwość dodać elementy do manifestu aplikacji, które ułatwiają kontrolę filtrowania aplikacji w sklepie Google Play. Poniższe listy manifestu elementów i atrybutów, które mogą służyć do filtrowania aplikacji:

-   [obsługuje ekranu](http://developer.android.com/guide/topics/manifest/supports-screens-element.html) &ndash; Google Play użyje atrybutów w celu określenia, jeśli aplikację można wdrożyć na urządzeniu, na podstawie rozmiaru ekranu. 
    Google Play przyjmie założenie, że Android można dostosować układu mniejszych ekranach większy, ale nie odwrotnie. Aby aplikacja deklaruje obsługę normalne ekrany będą wyświetlane w wyszukiwaniach dla dużych ekranów, ale nie małych ekranów. Jeśli nie ma aplikacji platformy Xamarin.Android `<supports-screen>` elementu w pliku manifestu, a następnie Google Play przyjmie wszystkich atrybutów ma wartość true i że aplikacja obsługuje wszystkich rozmiarów ekranu. Ten element musi zostać dodany do **AndroidManifest.xml** ręcznie. 

-   [używa konfiguracji](http://developer.android.com/guide/topics/manifest/uses-configuration-element.html) &ndash; tego elementu manifestu jest używany do żądania pewnych funkcji sprzętu, takie jak typ klawiatury, urządzenia nawigacji, ekran dotykowy, itp. Ten element musi zostać dodany do **AndroidManifest.xml** ręcznie. 

-   [używa funkcji](http://developer.android.com/guide/topics/manifest/uses-feature-element.html) &ndash; tego manifestu elementu deklaruje funkcji sprzętu lub oprogramowania, które urządzenie musi mieć Aby działanie aplikacji. Ten atrybut ma tylko charakter informacyjny. Google Play nie wyświetlają aplikacji na urządzeniach, które nie spełniają warunki tego filtru. Jest nadal możliwe do zainstalowania aplikacji za pomocą innych środków (ręcznie lub pobieranie). Ten element musi zostać dodany do **AndroidManifest.xml** ręcznie. 

-   [Biblioteka używa](http://developer.android.com/guide/topics/manifest/uses-library-element.html) &ndash; ten element określa, że niektóre współużytkowanych bibliotekach musi być obecny na urządzeniu, na przykład map programu Google. Ten element można także określić z `Android.App.UsesLibaryAttribute`. Na przykład: 

    ```csharp
    [assembly: UsesLibrary("com.google.android.maps", true)]
    ```

-   [używa uprawnień](http://developer.android.com/guide/topics/manifest/uses-permission-element.html) &ndash; ten element jest używany w celu uwzględnienia niektórych funkcji sprzętowego, które są wymagane do uruchomienia aplikacji, które mogą nie zostały poprawnie zadeklarowane z `<uses-feature>` elementu. Na przykład jeśli aplikacja żąda uprawnienia do korzystania z aparatu fotograficznego, następnie Google Play założenia, że urządzenia musi aparatu, nawet jeśli dostępny jest nie `<uses-feature>` element deklarowanie aparatu. Ten element może zostać ustawiony przy `Android.App.UsesPermissionsAttribute`. Na przykład: 

    ```csharp
    [assembly: UsesPermission(Manifest.Permission.Camera)]
    ```

-   [zestaw sdk używa](http://developer.android.com/guide/topics/manifest/uses-sdk-element.html) &ndash; element służy do deklarowania minimalny poziom interfejsu API systemu Android wymagane dla aplikacji. Ten element może ustawić w opcjach programu Xamarin.Android projektu platformy Xamarin.Android. 

-   [ekrany zgodny](http://developer.android.com/guide/topics/manifest/compatible-screens-element.html) &ndash; ten element służy do filtrowania aplikacji, które nie są zgodne, rozmiaru ekranu i gęstości określonego przez ten element. Większość aplikacji nie powinni używać tego filtru. Jest on przeznaczony do gier określonych wysokiej wydajności lub aplikacji, które wymagane ścisłą kontrolę dystrybucji aplikacji. `<support-screen>` Preferowane jest atrybut wymienionych powyżej. 

-   [obsługuje gl — tekstury](http://developer.android.com/guide/topics/manifest/supports-gl-texture-element.html) &ndash; ten element służy do deklarowania tekstury GL tworzenie kompresji, które wymaga aplikacji. Większość aplikacji nie powinni używać tego filtru. Jest on przeznaczony do gier określonych wysokiej wydajności lub aplikacji, które wymagane ścisłą kontrolę dystrybucji aplikacji. 

Aby uzyskać więcej informacji o konfigurowaniu manifest aplikacji, zobacz Android [Manifest aplikacji](https://developer.android.com/guide/topics/manifest/manifest-intro.html) tematu.
