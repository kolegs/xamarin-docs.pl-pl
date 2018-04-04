---
title: Wprowadzenie
description: Szablony kontroli platformy Xamarin.Forms zapewniają możliwość łatwo motywu i ponowna motywu strony aplikacji w czasie wykonywania. Ten artykuł zawiera wprowadzenie do szablonów kontrolki.
ms.prod: xamarin
ms.assetid: 8B8E2360-6531-44A3-A7C8-9A8808DE9B86
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 744419cbc457ffb6dab6b46d690151c08ca35d42
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="introduction"></a>Wprowadzenie

_Szablony kontroli platformy Xamarin.Forms zapewniają możliwość łatwo motywu i ponowna motywu strony aplikacji w czasie wykonywania. Ten artykuł zawiera wprowadzenie do szablonów kontrolki._

Formanty mają różne właściwości `BackgroundColor` i `TextColor`, który można zdefiniować aspektów wygląd formantu. Te właściwości można ustawić za pomocą [style](~/xamarin-forms/user-interface/styles/index.md), który może zostać zmieniony w czasie wykonywania do implementacji podstawowych motywów. Jednak style nie Obsługa czyste rozdzielenie wygląd strony i jego zawartości, a zmiany wprowadzone przez ustawienie tych właściwości są ograniczone.

Formant Szablony zapewniają czyste rozdzielenie wygląd strony i jego zawartości, dlatego włączenie tworzenia stron, które można łatwo zastosować motyw. Na przykład aplikacja może zawierać szablonów kontrola na poziomie aplikacji, które zapewniają ciemny motyw i motywu jasny. Każdy [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) w aplikacji można zastosować motyw przez zastosowanie jednego z szablonów kontroli bez zmiany zawartości wyświetlany przez każdej strony. Ponadto kompozycji systemu kontroli szablony nie są ograniczone możliwości zmiany właściwości formantów. Można również zmienić formanty używane do implementowania motywu.

## <a name="creating-a-controltemplate"></a>Tworzenie ControlTemplate

A [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) określa wygląd strony lub widoku i zawiera układzie głównego i w formantach, które implementują szablon układu. Zazwyczaj `ControlTemplate` korzystają [ `ContentPresenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/) do oznaczania, gdy pojawi się zawartość do wyświetlania przez stronę lub widok. Strona lub widok, który wykorzystuje `ControlTemplate` zdefiniuje mają być wyświetlane przez `ContentPresenter`. Na poniższym diagramie przedstawiono `ControlTemplate` dla strony, która zawiera wiele formantów, w tym `ContentPresenter` oznaczony przez niebieski prostokąt:

![](introduction-images/control-template.png "Szablonu formantu strony")

A [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) można stosować do następujących typów, ustawienie ich `ControlTemplate` właściwości:

- [`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)
- [`ContentView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/)
- [`TemplatedPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedPage/)
- [`TemplatedView`](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedView/)

Gdy [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) utworzyć i przypisać do tych typów wszelkie istniejące wygląd jest zastępowany wygląd zdefiniowane w `ControlTemplate`. Ponadto, a także ustawienie wygląd przy użyciu `ControlTemplate` właściwości formantu szablonów można również będą stosowane przy użyciu stylów, aby jeszcze bardziej rozwiń możliwości motywu.

> [!NOTE]
>  *Co to są `TemplatedPage` i `TemplatedView` typy?* `TemplatedPage` jest klasą bazową dla `ContentPage`i jest najbardziej podstawowa podał platformy Xamarin.Forms typ strony. W odróżnieniu od `ContentPage`, `TemplatedPage` nie ma `Content` właściwości. W związku z tym zawartości nie można bezpośrednio dodać do `TemplatedPage` wystąpienia. Zamiast tego zawartość jest dodawana przez ustawienie szablonu kontrolki dla `TemplatedPage` wystąpienia. Podobnie `TemplatedView` jest klasą bazową dla `ContentView`. W odróżnieniu od `ContentView`, `TemplatedView` nie ma `Content` właściwości. W związku z tym zawartości nie można bezpośrednio dodać do `TemplatedView` wystąpienia. Zamiast tego zawartość jest dodawana przez ustawienie szablonu kontrolki dla `TemplatedView` wystąpienia.

Szablony formantu można tworzyć w języku XAML, a w języku C#:

- Formant szablony utworzone w języku XAML są zdefiniowane w [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) przypisany do [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/) kolekcji strony lub więcej zwykle do [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.Resources/) kolekcji aplikacji.
- Szablony formantu utworzone w języku C# są zazwyczaj definiowane w klasie strony lub w klasie mogą uzyskiwać globalnie.

Wybieranie miejsce definiowania [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) wystąpienie wpływ, których można użyć:

- [`ControlTemplate`](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) wystąpienia zdefiniowane na poziomie strony tylko może odnosić się do strony.
- [`ControlTemplate`](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) wystąpienia zdefiniowane na poziomie aplikacji może odnosić się do stron w całej aplikacji.

Szablony formantu niżej w hierarchii widoku pierwszeństwo określone wyższy się. Na przykład [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) o nazwie `DarkTheme` zdefiniowanej na poziomie strony ma pierwszeństwo o identycznej nazwie szablonu, zdefiniowanych na poziomie aplikacji. W związku z tym szablon formantu, który definiuje motyw do zastosowania dla każdej strony w aplikacji powinien być zdefiniowany na poziomie aplikacji.


## <a name="related-links"></a>Linki pokrewne

- [Style](~/xamarin-forms/user-interface/styles/index.md)
- [ControlTemplate](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/)
- [ContentPresenter](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/)
