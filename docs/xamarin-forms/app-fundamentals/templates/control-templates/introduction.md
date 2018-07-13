---
title: Wprowadzenie do szablonów kontrolek zestawu narzędzi Xamarin.Forms
description: Szablony kontrolek zestawu narzędzi Xamarin.Forms umożliwia łatwe motywu i ponownej kompozycji strony aplikacji w czasie wykonywania. Ten artykuł zawiera wprowadzenie do szablonów kontrolek.
ms.prod: xamarin
ms.assetid: 8B8E2360-6531-44A3-A7C8-9A8808DE9B86
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 6b7a6c6d9c9c541e1d5e821fc2dac202e98bec62
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994428"
---
# <a name="introduction-to-xamarinforms-control-templates"></a>Wprowadzenie do szablonów kontrolek zestawu narzędzi Xamarin.Forms

_Szablony kontrolek zestawu narzędzi Xamarin.Forms umożliwia łatwe motywu i ponownej kompozycji strony aplikacji w czasie wykonywania. Ten artykuł zawiera wprowadzenie do szablonów kontrolek._

Formanty mają różne właściwości `BackgroundColor` i `TextColor`, który można zdefiniować aspekty wyglądu formantu. Te właściwości można ustawić za pomocą [style](~/xamarin-forms/user-interface/styles/index.md), który może zostać zmieniony w czasie wykonywania, aby zaimplementować podstawowych motywów. Jednak style nie Obsługa czystą separacji między wygląd strony i jego zawartości, a zmiany wprowadzone przez ustawienie tych właściwości są ograniczone.

Szablony kontrolek zapewniają czystą separacji między wygląd strony i jego zawartości, w związku z tym umożliwieniem tworzenia stron, które można łatwo zastosować motyw. Na przykład aplikacja może zawierać szablony kontroli na poziomie aplikacji, które zapewniają motyw jasny i ciemny motyw. Każdy [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) w aplikacji można zastosować motyw przez zastosowanie jednego z szablonów kontroli bez wprowadzania zmian w zawartości będą wyświetlane przez każdą stronę. Ponadto motywy oferowanego przez szablony formantu nie są ograniczone możliwości zmiany właściwości formantów. Można również zmienić kontrolki używaną do zaimplementowania motywu.

## <a name="creating-a-controltemplate"></a>Tworzenie ControlTemplate

A [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) określa wygląd strony lub widoku i zawiera głównego układu, a w układzie kontrolki, które implementują szablonu. Zazwyczaj `ControlTemplate` będą [ `ContentPresenter` ](xref:Xamarin.Forms.ContentPresenter) do oznaczania, gdzie pojawią się zawartości, które mają być wyświetlane przez strony lub widoku. Strona lub widok, który wykorzystuje `ControlTemplate` zdefiniuje mają być wyświetlane przez `ContentPresenter`. Na poniższym diagramie przedstawiono `ControlTemplate` dla strony, która zawiera szereg formantów, w tym `ContentPresenter` oznaczone przez niebieski prostokąt:

![](introduction-images/control-template.png "Szablon kontrolki strony")

A [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) mogą dotyczyć następujących typów, ustawiając ich `ControlTemplate` właściwości:

- [`ContentPage`](xref:Xamarin.Forms.ContentPage)
- [`ContentView`](xref:Xamarin.Forms.ContentView)
- [`TemplatedPage`](xref:Xamarin.Forms.TemplatedPage)
- [`TemplatedView`](xref:Xamarin.Forms.TemplatedView)

Gdy [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) zostanie utworzone i przypisane do tych typów, wszelkie istniejące wygląd zostanie zastąpiony wygląd zdefiniowane w `ControlTemplate`. Ponadto, a także ustawienie wygląd przy użyciu `ControlTemplate` właściwości kontrolki szablony mogą być również stosowane przy użyciu stylów, aby bardziej szczegółowo rozwinąć możliwości motywu.

> [!NOTE]
>  *Co to są `TemplatedPage` i `TemplatedView` typy?* `TemplatedPage` jest klasą bazową dla `ContentPage`i jest najbardziej podstawowym typem strony udostępniane przez zestaw narzędzi Xamarin.Forms. W odróżnieniu od `ContentPage`, `TemplatedPage` nie ma `Content` właściwości. W związku z tym, zawartości nie można bezpośrednio dodać do `TemplatedPage` wystąpienia. Zamiast tego należy zawartość jest dodawana przez ustawienie szablonu kontrolki dla `TemplatedPage` wystąpienia. Podobnie `TemplatedView` jest klasą bazową dla `ContentView`. W odróżnieniu od `ContentView`, `TemplatedView` nie ma `Content` właściwości. W związku z tym, zawartości nie można bezpośrednio dodać do `TemplatedView` wystąpienia. Zamiast tego należy zawartość jest dodawana przez ustawienie szablonu kontrolki dla `TemplatedView` wystąpienia.

Można utworzyć szablony kontrolek XAML i C#:

- Szablony kontroli utworzone w XAML są zdefiniowane w [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) przypisany do [ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources) kolekcji strony lub zazwyczaj do [ `Resources` ](xref:Xamarin.Forms.Application.Resources) kolekcji aplikacji.
- Szablony kontrolek utworzone w języku C# są zazwyczaj definiowane w klasie strony lub w klasie, która może globalnie dostępna.

Wybieranie miejsce definiowania [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) wystąpienia na środowisko, w których można użyć:

- [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) wystąpienia zdefiniowane na poziomie strony będzie stosowany tylko do strony.
- [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) wystąpienia zdefiniowane na poziomie aplikacji można zastosować do stron w całej aplikacji.

Szablony kontrolek niżej w hierarchii widoku pierwszeństwo identyczne ze zdefiniowanymi wyższej w górę. Na przykład [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) o nazwie `DarkTheme` która jest zdefiniowana na poziomie strony mają wyższy priorytet niż o identycznej nazwie szablonu, zdefiniowanych na poziomie aplikacji. W związku z tym szablon kontrolki, który definiuje motyw do zastosowania do każdej strony w aplikacji powinna być zdefiniowana na poziomie aplikacji.


## <a name="related-links"></a>Linki pokrewne

- [Style](~/xamarin-forms/user-interface/styles/index.md)
- [ControlTemplate](xref:Xamarin.Forms.ControlTemplate)
- [ContentPresenter](xref:Xamarin.Forms.ContentPresenter)
