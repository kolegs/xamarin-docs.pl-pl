---
title: Android Wear
description: "Tworzenie aplikacji dla urządzeń z systemem Android wearable."
ms.topic: article
ms.prod: xamarin
ms.assetid: 3BE4A128-2D88-4500-9E48-20375EA99A49
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/16/2018
ms.openlocfilehash: 31114df0b631aea909e82f3a8b836d5ef922d2c1
ms.sourcegitcommit: d450ae06065d8f8c80f3588bc5a614cfd97b5a67
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/21/2018
---
# <a name="android-wear"></a>Android Wear

Android zużycia jest wersja systemu Android, które jest przeznaczona dla wearable urządzeń, takich jak obserwowanie inteligentne. Ta sekcja zawiera instrukcje dotyczące instalowania i konfigurowania narzędzia niezbędne do rozwoju zużycia, przewodnik krok po kroku dotyczące tworzenia pierwszego urządzenia zużycia oraz listę przykłady odwołujące się do tworzenia własnych nosić aplikacji.

##  <a name="getting-startedandroidwearget-startedindexmd"></a>[Wprowadzenie](~/android/wear/get-started/index.md)

Wprowadza nosić systemu Android, zawiera opis sposobu instalowania i konfigurowania komputera do tworzenia zużycia i zawiera kroki ułatwiające tworzenie i uruchamianie pierwszej aplikacji systemu Android nosić na emulator lub zużycia urządzenia.

##  <a name="user-interfaceandroidwearuser-interfaceindexmd"></a>[Interfejs użytkownika](~/android/wear/user-interface/index.md)

Wyjaśniono Android zużycia specyficzne dla kontrolki i łącza do przykłady, które pokazują, jak używać tych kontrolek.

##  <a name="platform-featuresandroidwearplatformindexmd"></a>[Funkcje platformy](~/android/wear/platform/index.md)

Dokumenty w tej sekcji opisano funkcje właściwe dla systemu Android zużycia. Tutaj znajdziesz temat, który opisuje sposób tworzenia WatchFace.

##  <a name="screen-sizesandroidwearscreen-sizesmd"></a>[Rozmiary ekranów](~/android/wear/screen-sizes.md)

Wyświetl podgląd i optymalizowanie interfejsu użytkownika dla rozmiaru ekranu dostępne.

##  <a name="deployment--testingandroidweardeploy-testindexmd"></a>[Wdrażanie i testowanie](~/android/wear/deploy-test/index.md)

Wyjaśniono, jak wdrożyć aplikację systemu Android nosić na urządzenia z systemem Android nosić lub skonfigurowany dla zużycia emulatora systemu Android. Obejmuje on też debugowania porady i informacje dotyczące sposobu konfigurowania połączenia Bluetooth między komputerem programowanie i urządzenia z systemem Android.

##  <a name="wear-apishttpsdeveloperandroidcomreferenceandroidsupportwearable"></a>[Używać interfejsów API](https://developer.android.com/reference/android/support/wearable)

Witryny dewelopera systemu Android zawiera szczegółowe informacje dotyczące klucza nosić interfejsów API, takich jak [Wearable działania](https://developer.android.com/reference/android/support/wearable/activity/package-summary.html), [intencje](https://developer.android.com/reference/com/google/android/wearable/intent/package-summary.html), [uwierzytelniania](https://developer.android.com/reference/android/support/wearable/authentication/package-summary.html), [ Komplikacji](https://developer.android.com/reference/android/support/wearable/complications/package-summary.html), [komplikacji renderowania](https://developer.android.com/reference/android/support/wearable/complications/rendering/package-summary.html), [powiadomienia](https://developer.android.com/reference/android/support/wearable/notifications/package-summary.html), [widoków](https://developer.android.com/reference/android/support/wearable/view/package-summary.html), i [WatchFace](https://developer.android.com/reference/android/support/wearable/watchface/package-summary.html).



## <a name="samples"></a>Przykłady

Możesz znaleźć wiele [przykłady](https://developer.xamarin.com/samples/android/Android%20Wear/) przy użyciu nosić systemu Android (lub przejść bezpośrednio do [github](https://github.com/xamarin/monodroid-samples/tree/master/wear)). 

|Przykład|Opis|Zrzut ekranu|
|--- |--- |--- |
|[SkeletonWear](https://developer.xamarin.com/samples/SkeletonWear/)|Prosty przykład podstawy wearable projektów, w tym GridViewPager i interaktywne powiadomienia.|![Zrzut ekranu przedstawiający Skeletonwear](images/skeleton.png)|
|[WatchViewStub](https://developer.xamarin.com/samples/WatchViewStub/)|Prosty pokaz formantu WatchViewStub, który wykryje kształtu ekranu i automatycznie ładuje układ poprawne.  Zobacz, jak działa WatchViewStub w **Resources/layout/main_actvity.xml** układu.|![Zrzut ekranu przedstawiający WatchViewStub](images/watchview.png)|
|[RecipeAssistant](https://developer.xamarin.com/samples/RecipeAssistant/)|Pokaz zużycia stron powiadomień, w formie przepisu kroki. Powiadomienia są tworzone w RecipeService.cs.|![Zrzut ekranu przedstawiający RecipeAssistant](images/recipeassist.png)|
|[ElizaChat](https://developer.xamarin.com/samples/ElizaChat/)|Przykładowe przyjemne interakcji z "Pomocnik" o nazwie Eliza tworzenie wykorzystująca zwięzłych odpowiedzi przy użyciu powiadomienia interakcyjne zużycia.|![Zrzut ekranu przedstawiający ElizaChat](images/eliza.png)|
|[GridViewPager](https://developer.xamarin.com/samples/GridViewPager/)|GridViewPager implementuje wzorzec 2D nawigacji, gdy użytkownik swipes w pionie i w poziomie do nawigowania opcje i zawartości.|![Zrzut ekranu przedstawiający GridViewPager](images/gridviewpager.png)|
|[WatchFace](https://developer.xamarin.com/samples/monodroid/wear/WatchFace)|WatchFace jest krój czujki niestandardowych z analogowy stylu godzinę, minutę i drugi ręce. W tym przykładzie pokazano, jak utworzyć usługi krój czujki który rysuje bieżący czas i tryb otoczenia dojść i widoczności zdarzenia zmian. Obejmuje on emisji odbiornik nasłuchuje zmiany strefy czasowej i automatycznie aktualizuje odpowiednio czas.|![Zrzut ekranu przedstawiający WatchFace](images/gridviewpager.png)|


##  <a name="videos"></a>Wideo

Zapoznaj się one wideo, łącza, które omówiono w nim Xamarin.Android z zużycia obsługę:

|Opis|Zrzut ekranu|
|--- |--- |
|[Android L i dużo więcej](http://blog.xamarin.com/webinar-recording-android-l-and-so-much-more/) &ndash; Android L Developer Preview wprowadzono nadmiar nowych interfejsów API dla deweloperów móc korzystać z, w tym projektowania materiałów, powiadomień i animacje, kilka.|![Wideo zrzut ekranu przedstawiający prezentacji](images/video-android-l.png)|
|[C# jest w wracasz i oczu: szkła Google i Android nosić](https://www.youtube.com/watch?v=80H8tXByZQc) &ndash; Wearable przetwarzania danych może się wydawać coś z przyszłości (lub epizodu gadżet inspektora), ale wiele osób już są obejmującego przyszłości dzisiaj! C# deweloperów znać te i już narzędzia i umiejętności, aby wykorzystać wearable urządzeń (od Evolve 2014).|![Wideo zrzut ekranu przedstawiający prezentacji](images/video-eyes-ears.png)|
|[What's new in Xamarin.Android](https://www.youtube.com/watch?v=Gpqc2XZIQfU) &ndash; Android L, Android nosić Android TV, automatycznie systemu Android, materiałów projektu i GRAFIKI; co dzieje się tak średniej użytkownikowi jako deweloper Xamarin? z Evolve 2014.|![Wideo zrzut ekranu przedstawiający prezentacji](Images/video-whats-new.png)|


<!--

March 18
http://blog.xamarin.com/android-wear/

August 14
http://blog.xamarin.com/android-l-developer-preview-android-wear-support/

August 27
http://blog.xamarin.com/tips-for-your-first-android-wear-app/

Watch Face
https://github.com/Redth/Xamarin.Wear.WatchFace
-->
