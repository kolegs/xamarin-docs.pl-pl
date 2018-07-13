---
title: Podstawy aplikacji platformy Xamarin.Forms
description: Poznawanie podstawy tworzenia aplikacji platformy Xamarin.Forms, w tym wszystkie wymagane podstawowe pojęcia, za pośrednictwem poprawek, takich jak lokalizacja i ułatwienia dostępu.
ms.prod: xamarin
ms.assetid: 7B516BBC-F7E1-4387-9779-7754E2E69723
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/28/2017
ms.openlocfilehash: 515dbd2683619cfcfb7a6c8ecac6bc147265ef7d
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995615"
---
# <a name="xamarinforms-application-fundamentals"></a>Podstawy aplikacji platformy Xamarin.Forms

## <a name="accessibilityaccessibilityindexmd"></a>[Ułatwienia dostępu](accessibility/index.md)

Porady, aby włączyć funkcje ułatwień dostępu (takich jak Obsługa narzędzia czytnik ekranu) z zestawem narzędzi Xamarin.Forms.

## <a name="app-classapplication-classmd"></a>[Klasa aplikacji](application-class.md)

`Application` Klasy to punkt początkowy dla zestawu narzędzi Xamarin.Forms — każda aplikacja musi zaimplementować podklasę `App` ustawienie strony początkowej. Zapewnia także `Properties` kolekcji do przechowywania danych proste. Mogą być definiowane w języku C# lub XAML.

## <a name="app-lifecycleapp-lifecyclemd"></a>[Cykl życia aplikacji](app-lifecycle.md)

`Application` Klasy `OnStart`, `OnSleep`, i `OnResume` metody, a także zdarzeń nawigacji modalne umożliwiają obsługę zdarzenia cyklu życia aplikacji za pomocą kodu niestandardowego.

## <a name="behaviorsbehaviorsindexmd"></a>[Zachowania](behaviors/index.md)

Kontrolek interfejsu użytkownika można łatwo rozszerzyć bez podklasy za pomocą zachowań, aby dodać funkcje.

## <a name="custom-rendererscustom-rendererindexmd"></a>[Niestandardowe programy renderujące](custom-renderer/index.md)

Renderuje niestandardowe umożliwiają deweloperom "override" domyślne renderowanie kontrolek zestawu narzędzi Xamarin.Forms, dostosowywanie ich wyglądu i działania na każdej platformie (przy użyciu natywnych zestawów SDK w razie potrzeby).

## <a name="data-bindingdata-bindingindexmd"></a>[Powiązanie danych](data-binding/index.md)

Powiązanie danych łączy właściwości dwóch obiektów, dzięki czemu zmiany w jednej właściwości są automatycznie odzwierciedlane w innych właściwości. Wiązanie danych jest integralną częścią Model-View-ViewModel ([MVVM](~/xamarin-forms/enterprise-application-patterns/mvvm.md)) architektury aplikacji.

## <a name="dependency-servicedependency-serviceindexmd"></a>[Zależność usługi](dependency-service/index.md)

`DependencyService` Zapewnia proste lokalizatora można kod do interfejsów w ze współużytkowanym kodem i zapewniają implementacji specyficznych dla platformy, które są automatycznie rozwiązywane, co ułatwia odwoływać się do funkcji specyficznych dla platformy w interfejsie Xamarin.Forms.

## <a name="effectseffectsindexmd"></a>[Efekty](effects/index.md)

Efekty Zezwalaj natywne kontrolki na każdej z platform można dostosować i są zwykle używane do zmiany stylów małe.

## <a name="filesfilesmd"></a>[Pliki](files.md)

Obsługa przy użyciu zestawu narzędzi Xamarin.Forms plików można osiągnąć przy użyciu kodu biblioteki .NET Standard lub przy użyciu zasobów osadzonych.

## <a name="gesturesgesturesindexmd"></a>[Gesty](gestures/index.md)

Xamarin.Forms [ `GestureRecognizer` ](xref:Xamarin.Forms.GestureRecognizer) klasy obsługuje wzorca tap, uszczypnięcia i gestów pan kontrolek interfejsu użytkownika.

## <a name="localizationlocalizationindexmd"></a>[Lokalizacja](localization/index.md)

Wbudowana Struktura .NET w lokalizacji może służyć do tworzenia aplikacji wielojęzycznych dla wielu platform za pomocą zestawu narzędzi Xamarin.Forms.

## <a name="local-databasesdatabasesmd"></a>[Lokalne bazy danych](databases.md)

Zestaw narzędzi Xamarin.Forms obsługuje opartego na bazie danych aplikacji przy użyciu aparatu bazy danych SQLite, dzięki czemu możliwe do ładowania i zapisywania obiektów w współużytkowanym kodem.

## <a name="messaging-centermessaging-centermd"></a>[Centrum wiadomości](messaging-center.md)

Zestaw narzędzi Xamarin.Forms `MessagingCenter` umożliwia wyświetlanie modeli i inne składniki do komunikowania się z bez znajomości nic innego oprócz proste kontraktu komunikatu.

## <a name="navigationnavigationindexmd"></a>[Nawigacja](navigation/index.md)

Zestaw narzędzi Xamarin.Forms oferuje pewną liczbę innej strony nawigacji środowisk, w zależności od `Page` wpisz używane.

## <a name="templatestemplatesindexmd"></a>[Szablony](templates/index.md)

Szablony kontrolek umożliwiają możliwość łatwego motywu i ponownej kompozycji strony aplikacji w czasie wykonywania, a dane Szablony zapewniają możliwość definiowania prezentacji danych na obsługiwanych formantów.

## <a name="triggerstriggersmd"></a>[Wyzwalacze](triggers.md)

Zaktualizuj kontrolki, odpowiadanie na zmiany właściwości i zdarzenia w XAML.


## <a name="related-links"></a>Linki pokrewne

- [Wprowadzenie do zestawu narzędzi Xamarin.Forms](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
