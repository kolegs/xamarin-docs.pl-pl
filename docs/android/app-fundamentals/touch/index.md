---
title: Gestów na platformie Xamarin.Android i dotykiem
description: Ekranów dotykowych na wielu urządzeniach współczesnych Zezwalaj użytkownikom na interakcję z urządzeniami, szybkie i skuteczne jak fizyczną i intuicyjne. Interakcji nie jest ograniczona tylko w celu wykrywania touch proste — można również gesty. Na przykład gestu powiększanie gestem uszczypnięcia jest często przykładem punkty zaciskające części ekranu z dwoma palcami, które użytkownik może powiększanie lub pomniejszanie. W tym przewodniku sprawdza touch i gestów w systemie Android.
ms.prod: xamarin
ms.assetid: 61874769-978A-4562-9B2A-7FFD45F58B38
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 3700f9c73fb58fefcdba7987c9931e385cd52d38
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/03/2018
---
# <a name="touch-and-gestures-in-xamarinandroid"></a>Gestów na platformie Xamarin.Android i dotykiem

_Ekranów dotykowych na wielu urządzeniach współczesnych Zezwalaj użytkownikom na interakcję z urządzeniami, szybkie i skuteczne jak fizyczną i intuicyjne. Interakcji nie jest ograniczona tylko w celu wykrywania touch proste — można również gesty. Na przykład gestu powiększanie gestem uszczypnięcia jest często przykładem punkty zaciskające części ekranu z dwoma palcami, które użytkownik może powiększanie lub pomniejszanie. W tym przewodniku sprawdza touch i gestów w systemie Android._

## <a name="touch-overview"></a>Touch — omówienie

iOS i Android są podobne w sposób obsługują touch. Jednocześnie może obsługiwać wielodotyku - wiele punktów kontaktu na ekranie - i gestów złożonych. W tym przewodniku przedstawiono niektóre podobieństw pojęć, a także szczegóły dotyczące implementowania touch i gestów na obu platform.

Używa android `MotionEvent` obiektu w celu hermetyzacji touch danych i metody dla obiektu widoku do nasłuchiwania poprawki.

Oprócz przechwytywania danych touch, zarówno dla systemu iOS i Android podanie oznacza interpretowanie wzorce poprawek do gestów. Z kolei można te aparaty rozpoznawania gestów interpretować polecenia specyficzne dla aplikacji, takie jak obrót obrazu lub Włącz strony. Android udostępnia kilka gestów obsługiwanych, jak również zasoby, aby upewnić się, dodawanie gestów niestandardowych złożonych łatwe.

Czy pracujesz nad Android lub iOS, wybór między poprawek i aparatów rozpoznawania gestów mogą być mylące. W tym przewodniku zaleca się, że ogólnie rzecz biorąc, powinien preferowane aparaty rozpoznawania gestów. Aparaty rozpoznawania gestów są zaimplementowane jako odrębny klasy, które zapewniają większą rozdzielenie problemy i lepsze hermetyzacji. Dzięki temu można łatwo udostępniać logiki między różnymi widokami, minimalizując ilość kodu napisanego.

W tym przewodniku w formacie podobne dla każdego systemu operacyjnego: pierwszy, platformy touch interfejsy API są wprowadzone i wyjaśniono, jak są one foundation, na które touch interakcji są tworzone. Następnie możemy Poznaj świecie aparaty rozpoznawania gestów — najpierw eksploracji niektórych typowych gestów i kończenie pracy z tworzeniem gestów niestandardowych dla aplikacji. Na koniec zostanie wyświetlone jak śledzić poszczególnych palców, aby utworzyć finger-paint program przy użyciu touch niskiego poziomu śledzenia.

## <a name="sections"></a>Sekcje

-  [Dotyk w systemie Android](~/android/app-fundamentals/touch/android-touch-walkthrough.md)
-  [Wskazówki: Używanie Touch w systemie Android](~/android/app-fundamentals/touch/android-touch-walkthrough.md)
-  [Obsługa wielodotyku śledzenia](touch-tracking.md)

## <a name="summary"></a>Podsumowanie

W tym przewodniku możemy zbadać touch w systemie Android. Dla obu systemów operacyjnych dowiedzieliśmy się, jak włączyć touch i odpowiadanie na zdarzenia touch. Następnie dowiedzieliśmy się o gestów i niektóre aparaty rozpoznawania gestów czy zarówno dla systemu Android i iOS umożliwiają obsługę niektórych bardziej typowych scenariuszy. Firma Microsoft zbadać tworzenie gestów niestandardowych i ich wdrażania w aplikacjach. Przewodnik przedstawiono pojęcia i interfejsów API dla każdego systemu operacyjnego, w akcji, a także pokazano, jak śledzić poszczególne palców.



## <a name="related-links"></a>Linki pokrewne

- [Android Touch Start (przykład)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/Touch_start)
- [Android Touch końcowego (na przykład)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/Touch_final)
- [FingerPaint (przykład)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/FingerPaint)
