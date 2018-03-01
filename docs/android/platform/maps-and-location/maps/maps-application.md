---
title: Mapowania aplikacji
ms.topic: article
ms.prod: xamarin
ms.assetid: 929EACB8-8950-50E1-093C-43FB5F1F1CD5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/05/2017
ms.openlocfilehash: a6bfebb5272da3fd50f4f165fc25bb75574a0b63
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="maps-application"></a>Mapowania aplikacji

Najprostszym sposobem, aby pracować przy użyciu map platformie Xamarin.Android ma korzystać z aplikacji wbudowanej mapy, pokazano poniżej:

[![Zrzut ekranu aplikacji map programu Google wbudowane](maps-application-images/01-mapsapplication.png)](maps-application-images/01-mapsapplication.png)

Podczas używania aplikacji mapy, mapy nie będzie częścią aplikacji. Zamiast tego aplikacja uruchomienia aplikacji map i załadować mapy zewnętrznie. Następna sekcja sprawdza, czy uruchomić mapy jak powyżej przy użyciu platformy Xamarin.Android.

<a name="Creating_the_Intent" />

## <a name="creating-the-intent"></a>Tworzenie celem

Praca z mapy aplikacja jest równie proste jak tworzenie celem z odpowiedni identyfikator URI, ustawienie akcji ActionView i wywołanie metody StartActivity. Na przykład następujący kod uruchamia aplikację mapy wyśrodkowany w danym współrzędne geograficzne:

```csharp
var geoUri = Android.Net.Uri.Parse ("geo:42.374260,-71.120824");
var mapIntent = new Intent (Intent.ActionView, geoUri);
StartActivity (mapIntent);
```

Ten kod jest są wystarczające do uruchomienia na mapie pokazano poprzedni zrzut ekranu. Oprócz określenia współrzędne geograficzne, schemat identyfikatora URI dla mapowania obsługuje kilka innych opcji.

<a name="Geo_Uri_Scheme" />

## <a name="geo-uri-scheme"></a>Schemat identyfikatora URI Geo

Powyższy kod używany schemat geograficznie do utworzenia identyfikatora URI. Ten schemat identyfikatora URI obsługuje wiele formatów wymienione poniżej:

-   `geo:latitude,longitude` &ndash; Otwiera aplikację mapy skupia się na lat/długa. 

-   `geo:latitude,longitude?z=zoom` &ndash; Otwiera mapy aplikacji skupia się na lat/długa i powiększony do określonego poziomu. Poziom powiększenia można zakresu od 1 do 23: 1 wyświetla całą ziemi i 23 znajduje się najbliżej poziom powiększenia.

-   `geo:0,0?q=my+street+address` &ndash; Otwiera aplikację mapy do lokalizacji ulicy. 

-   `geo:0,0?q=business+near+city` &ndash; Otwiera aplikację map i wyświetla wyniki wyszukiwania adnotacjami. 


Wersje identyfikatora URI wykonać zapytanie (czyli ulicy warunki adres lub wyszukiwania) umożliwia pobrać lokalizacji, która jest następnie wyświetlone na mapie usługi geocoder firmy Google. Na przykład identyfikator URI `geo:0,0?q=coop+Cambridge` powoduje mapy pokazano poniżej:

[![Zrzut ekranu przedstawiający map programu Google terminem wyszukiwania](maps-application-images/02-mapsearch.png)](maps-application-images/02-mapsearch.png)


<a name="Street_View" />

Aby uzyskać więcej informacji na temat schematy identyfikatorów URI geograficznie zobacz [Pokaż lokalizację na mapie](http://developer.android.com/guide/components/intents-common.html#Maps).


## <a name="street-view"></a>Ulica widoku

Oprócz schemat geograficznie Android obsługuje również, ładowania ulicy widoków z celem. Poniżej przedstawiono przykład aplikacji ulicy widoku uruchamiana z platformy Xamarin.Android:

[![Zrzut ekranu widoku ulicy](maps-application-images/03-streetview.png)](maps-application-images/03-streetview.png)

Aby uruchomić ulicy widoku, po prostu użyć `google.streetview` schemat identyfikatora URI, jak pokazano w poniższym kodzie:

```csharp
var streetViewUri = Android.Net.Uri.Parse (
       "google.streetview:cbll=42.374260,-71.120824&cbp=1,90,,0,1.0&mz=20");  
var streetViewIntent = new Intent (Intent.ActionView, streetViewUri);  
StartActivity (streetViewIntent);
```

Schemat identyfikatora URI google.streetview używane powyżej ma następującą postać:

```csharp
google.streetview:cbll=lat,lng&cbp=1,yaw,,pitch,zoom&mz=mapZoom
```

Jak widać, istnieje kilka parametrów obsługiwanych, wymienione poniżej:

-   `lat` &ndash; Szerokość geograficzna lokalizacji, która ma być wyświetlany w widoku ulicy.

-   `lng` &ndash; Długość geograficzna lokalizacji, która ma być wyświetlany w widoku ulicy.

-   `pitch` &ndash; Kąt panorama ulicy widoku, mierzony w Centrum w stopniach 90 stopni w przypadku prostych w dół i -90 stopni jest w górę.

-   `yaw` &ndash; Centrum widoku z panorama ulicy widoku mierzone wskazówek zegara stopni od północy.

-   `zoom` &ndash; Powiększenie mnożnik dla widoku ulicy panorama, gdzie jest to 1.0 = powiększenia normalne, 2.0 = powiększony 2 x 3.0 = powiększony 4 x itp.

-   `mz` &ndash; Poziom powiększenia mapy, który będzie używany podczas przechodzenia do aplikacji mapy z widoku ulicy.


Praca z wbudowanej mapy aplikacji lub ulicy widoku to prosty sposób można szybko dodać mapowania obsługi. Jednak w systemie Android interfejsu API map oferuje bardziej precyzyjną kontrolę nad środowiskiem mapowania.
