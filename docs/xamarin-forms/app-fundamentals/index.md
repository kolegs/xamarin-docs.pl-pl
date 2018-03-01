---
title: Podstawy aplikacji
description: Eksploracja podstawy rozwoju platformy Xamarin.Forms
ms.topic: article
ms.prod: xamarin
ms.assetid: 7B516BBC-F7E1-4387-9779-7754E2E69723
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/28/2017
ms.openlocfilehash: afa3bf25b1448d98c49c95a66bd0f4dc55bde39e
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="application-fundamentals"></a>Podstawy aplikacji

## <a name="accessibilityaccessibilityindexmd"></a>[Ułatwienia dostępu](accessibility/index.md)

Wskazówek, aby włączyć funkcje ułatwień dostępu (takie jak obsługa narzędzi do odczytywania zawartości ekranu) z platformy Xamarin.Forms.

## <a name="app-classapplication-classmd"></a>[Klasa aplikacji](application-class.md)

`Application` Klasy jest punkt początkowy dla platformy Xamarin.Forms — każda aplikacja musi implementować podklasy `App` można ustawić stronę początkową. Zapewnia także `Properties` kolekcji do przechowywania danych proste. Może być zdefiniowany w języku C# lub języka XAML.

## <a name="app-lifecycleapp-lifecyclemd"></a>[Cykl życia aplikacji](app-lifecycle.md)

`Application` Klasy `OnStart`, `OnSleep`, i `OnResume` zdarzenia modalne nawigacji, a także metody umożliwiają obsługę zdarzeń cyklu życia aplikacji z niestandardowego kodu.

## <a name="behaviorsbehaviorsindexmd"></a>[Zachowania](behaviors/index.md)

Formanty interfejsu użytkownika można łatwo rozszerzyć bez podklasy przez dodawanie funkcji za pomocą zachowań.

## <a name="custom-rendererscustom-rendererindexmd"></a>[Niestandardowe moduły renderowania](custom-renderer/index.md)

Renderuje niestandardowe umożliwiają deweloperom "override" odwzorowanie domyślne platformy Xamarin.Forms formantów, aby dostosować wygląd i zachowanie na każdej z platform (przy użyciu natywnych zestawów SDK w razie potrzeby).

## <a name="data-bindingdata-bindingindexmd"></a>[Powiązanie danych](data-binding/index.md)

Powiązanie danych łączy właściwości dwa obiekty możliwości zmiany w jednej właściwości są automatycznie odzwierciedlane w innych właściwości. Powiązanie danych jest integralną częścią Model-View-ViewModel ([MVVM](~/xamarin-forms/enterprise-application-patterns/mvvm.md)) Architektura aplikacji.

## <a name="dependency-servicedependency-serviceindexmd"></a>[Zależność usługi](dependency-service/index.md)

`DependencyService` Udostępnia prosty lokalizatora, aby mogli kod do interfejsów w kodzie udostępnionego i podaj implementacje specyficzne dla platformy, które są automatycznie rozwiązany, co ułatwia odwołania funkcje specyficzne dla platformy w platformy Xamarin.Forms.

## <a name="effectseffectsindexmd"></a>[Efekty](effects/index.md)

Efekty Zezwalaj kontrolki natywne każdej platformy można dostosować i są zwykle używane do zmian małych style.

## <a name="gesturesgesturesindexmd"></a>[Gestów](gestures/index.md)

Platformy Xamarin.Forms [ `GestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.GestureRecognizer/) klasa obsługuje tap, powiększanie i przesuwanie gestów na formanty interfejsu użytkownika.

## <a name="localizationlocalizationmd"></a>[Lokalizacja](localization.md)

Wbudowane .NET framework lokalizacja może służyć do tworzenia i platform aplikacji wielojęzycznych za pomocą platformy Xamarin.Forms.

## <a name="local-databasesdatabasesmd"></a>[Lokalnych baz danych](databases.md)

Platformy Xamarin.Forms obsługuje opartej na bazie danych aplikacji przy użyciu aparatu bazy danych SQLite, co umożliwia przy ładowaniu i zapisywaniu obiektów w kodzie udostępnionego.

## <a name="messaging-centermessaging-centermd"></a>[Centrum wiadomości](messaging-center.md)

Platformy Xamarin.Forms `MessagingCenter` umożliwia wyświetlanie modeli i inne składniki do komunikowania się z bez znajomości cokolwiek innego niż prosty kontraktu komunikatu.

## <a name="navigationnavigationindexmd"></a>[Nawigacji](navigation/index.md)

Platformy Xamarin.Forms udostępnia wiele zastosowań nawigacji innej strony, w zależności od `Page` wpisz używane.

## <a name="templatestemplatesindexmd"></a>[Szablony](templates/index.md)

Kontroli Szablony zapewniają możliwość łatwego motywu i motyw środowiska odzyskiwania stron aplikacji w czasie wykonywania, gdy szablony danych zapewniają możliwość definiowania prezentację danych na obsługiwanych formantów.

## <a name="triggerstriggersmd"></a>[Wyzwalacze](triggers.md)

Zaktualizuj kontrolki reagowanie na zmiany właściwości i zdarzeń w języku XAML.


## <a name="related-links"></a>Linki pokrewne

- [Wprowadzenie do platformy Xamarin.Forms](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
