---
title: Android Wear
description: "Tworzenie aplikacji dla urządzeń z systemem Android wearable."
ms.topic: article
ms.prod: xamarin
ms.assetid: 3BE4A128-2D88-4500-9E48-20375EA99A49
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: ac83b74f39497333de7aa80079784adf61bf2e65
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/12/2018
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



## <a name="samples"></a>Przykłady

Możesz znaleźć wiele [przykłady](https://developer.xamarin.com/samples/android/Android%20Wear/) przy użyciu nosić systemu Android (lub przejść bezpośrednio do [github](https://github.com/xamarin/monodroid-samples/tree/master/wear)). 

<table align="center" border="1" cellpadding="1" cellspacing="1">
  <thead>
      <th>
          <strong>próbki</strong>
      </th>
      <th>
          <strong>Opis elementu</strong>
      </th>
      <th>
          <strong>Zrzut ekranu</strong>
      </th>
  </thead>
  <tbody>
  <tr>
      <td valign="top">
          <a href="https://developer.xamarin.com/samples/SkeletonWear/">SkeletonWear</a>
      </td>
      <td valign="top">
Prosty przykład podstawy wearable projektów, w tym GridViewPager i interaktywne powiadomienia.
      </td>
      <td>
          <img src="Images/skeleton.png" class="tableimg">
      </td>
  </tr>
  <tr>
      <td valign="top">
          <a href="https://developer.xamarin.com/samples/WatchViewStub/">WatchViewStub</a>
      </td>
      <td valign="top">
Prosty pokaz formantu WatchViewStub, który wykryje kształtu ekranu i automatycznie ładuje układ poprawne.
Zobacz, jak działa WatchViewStub w <b>Resources/layout/main_actvity.xml</b> układu.
      </td>
      <td>
          <img src="Images/watchview.png" class="tableimg">
      </td>
  </tr>
  <tr>
      <td valign="top">
          <a href="https://developer.xamarin.com/samples/RecipeAssistant/">RecipeAssistant</a>
      </td>
      <td valign="top">
Pokaz zużycia stron powiadomień, w formie przepisu kroki. Powiadomienia są tworzone w <b>RecipeService.cs</b>.
      </td>
      <td>
          <img src="Images/recipeassist.png" class="tableimg">
      </td>
  </tr>
  <tr>
      <td valign="top">
          <a href="https://developer.xamarin.com/samples/ElizaChat/">ElizaChat</a>
      </td>
      <td valign="top">
Przykładowe przyjemne interakcji z "Pomocnik" o nazwie Eliza tworzenie wykorzystująca zwięzłych odpowiedzi przy użyciu powiadomienia interakcyjne zużycia.
      </td>
      <td>
          <img src="Images/eliza.png" class="tableimg">
      </td>
  </tr>
  <tr>
      <td valign="top">
          <a href="https://developer.xamarin.com/samples/GridViewPager/">GridViewPager</a>
      </td>
      <td valign="top">
GridViewPager implementuje wzorzec 2D nawigacji, gdy użytkownik swipes w pionie i w poziomie do nawigowania opcje i zawartości.
      </td>
      <td>
          <img src="Images/gridviewpager.png" class="tableimg">
      </td>
  </tr>
  <tr>
      <td valign="top">
          <a href="https://developer.xamarin.com/samples/monodroid/wear/WatchFace">WatchFace</a>
      </td>
      <td valign="top">
          <b>WatchFace</b> jest krój czujki niestandardowych z analogowy stylu godzinę, minutę i drugi ręce. W tym przykładzie pokazano, jak utworzyć usługi krój czujki który rysuje bieżący czas i tryb otoczenia dojść i widoczności zdarzenia zmian. Obejmuje on emisji odbiornik nasłuchuje zmiany strefy czasowej i automatycznie aktualizuje odpowiednio czas.
      </td>
      <td>
          <img src="Images/watchface.png" class="tableimg">
      </td>
  </tr>
  </tbody>
</table>

##  <a name="videos"></a>Wideo

Zapoznaj się z tych wideo, obsługę łączy, które omówiono w nim Xamarin.Android z zużycia.

<table align="center" border="0" cellpadding="1" cellspacing="1">
    <tr>
        <td>
        <a href="http://blog.xamarin.com/webinar-recording-android-l-and-so-much-more/"><img src="Images/video-android-l.png" border="0" /></td>
        <td><a href="http://blog.xamarin.com/webinar-recording-android-l-and-so-much-more/">Android L i wiele innych</a>
        <br />
Android L Developer Preview wprowadzono nadmiar nowych interfejsów API dla deweloperów móc korzystać z, w tym projektowania materiałów, powiadomień i animacje, kilka.</td>
    </tr>
    <tr>
        <td>
        <a href="https://www.youtube.com/watch?v=80H8tXByZQc"><img src="Images/video-eyes-ears.png" border="0" /></td>
        <td><a href="https://www.youtube.com/watch?v=80H8tXByZQc">C# jest w wracasz i oczu: szkła Google i nosić systemu Android</a>
        <br />
Wearable przetwarzania danych może się wydawać coś z przyszłości (lub epizodu gadżet inspektora), ale wiele osób już są obejmującego przyszłości już dziś! C# deweloperów znać te i już narzędzia i umiejętności, aby wykorzystać wearable urządzeń (od Evolve 2014).</td>
    </tr>
    <tr>
        <td>
        <a href="https://www.youtube.com/watch?v=Gpqc2XZIQfU"><img src="Images/video-whats-new.png" border="0" /></td>
        <td><a href="https://www.youtube.com/watch?v=Gpqc2XZIQfU">What's new in platformy Xamarin.Android</a>
        <br />
        <i>Android L, zużycia Android Android TV, automatycznie systemu Android, materiału projektu i GRAFIKI; Co to znaczy do użytkownika jako deweloper Xamarin? </i> z rozwijać 2014 r.</td>
    </tr>
</table>


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
