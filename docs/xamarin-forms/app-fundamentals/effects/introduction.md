---
title: Wprowadzenie do efekty
description: Efekty Zezwalaj kontrolki natywne każdej platformy można dostosować i są zwykle używane do zmian małych style. Ten artykuł zawiera wprowadzenie do efekty, przedstawia granicę między efekty i niestandardowe moduły renderowania i opisano klasy PlatformEffect.
ms.prod: xamarin
ms.assetid: 30CB8615-8F39-4762-BDB7-333D2B57D112
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 7b859dd58551675c121c5600ba9c691e4280a03b
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2018
---
# <a name="introduction-to-effects"></a>Wprowadzenie do efekty

_Efekty Zezwalaj kontrolki natywne każdej platformy można dostosować i są zwykle używane do zmian małych style. Ten artykuł zawiera wprowadzenie do efekty, przedstawia granicę między efekty i niestandardowe moduły renderowania i opisano klasy PlatformEffect._

Platformy Xamarin.Forms [stron, układów i kontrolek](~/xamarin-forms/user-interface/controls/index.md) przedstawia wspólnego interfejsu API do opisywania interfejsów i platform przenośnych użytkownika. Każdej strony układu i kontroli jest inaczej renderowane na każdej platformie za pomocą `Renderer` klasy, która z kolei tworzy macierzystego formantu (odpowiadającej reprezentacja platformy Xamarin.Forms), rozmieszcza ją na ekranie i dodaje zachowanie określone w udostępnionego Kod.

Deweloperzy mogą implementować własne niestandardowe `Renderer` klas, aby dostosować wygląd i/lub zachowanie formantu. Jednak implementacja klasy niestandardowego modułu renderowania do wykonania dostosowywania prostego formantu jest często odpowiedzi ciężki. Efekty upraszcza ten proces, dzięki czemu kontrolki natywne każdej platformy można łatwo dostosować.

Efekty są tworzone w projektach specyficzne dla platformy przez podklasy `PlatformEffect` sterowania, a następnie efekty są używane przez dołączenie do odpowiednich formantu w projekcie platformy Xamarin.Forms przenośnej biblioteki klasy (PCL) lub biblioteki udostępnione.

## <a name="why-use-an-effect-over-a-custom-renderer"></a>Dlaczego warto używać efekt za pośrednictwem niestandardowego modułu renderowania?

Efekty uprościć Dostosowywanie formantu, wielokrotnego użytku i mogą nadać parametry do dalszego zwiększenia ponownego użycia.

Wszystko, co może zostać osiągnięty przy wpływ również można uzyskać z niestandardowego modułu renderowania. Jednak niestandardowe moduły renderowania zapewniają większą elastyczność i dostosowywania niż efekty. Poniższe wskazówki listy okoliczności, w których można wybierać efekt za pośrednictwem niestandardowego modułu renderowania:

- Efekt jest zalecane, gdy zmiana właściwości formantu specyficzne dla platformy zapewni pożądany wynik.
- Niestandardowego modułu renderowania jest wymagany, gdy istnieje potrzeba do przesłonięcia metod kontroli specyficzne dla platformy.
- Niestandardowego modułu renderowania jest wymagany, gdy trzeba zastąpić kontrolkę specyficzne dla platformy, która implementuje formantu platformy Xamarin.Forms.

## <a name="subclassing-the-platformeffect-class"></a>Tworzenie podklas klas PlatformEffect

W poniższej tabeli wymieniono w obszarze nazw `PlatformEffect` klasy na każdej platformie i typy jego właściwości:

|Platforma|Przestrzeń nazw|Kontener|Formant|
|--- |--- |--- |--- |
|iOS|Xamarin.Forms.Platform.iOS|UIView|UIView|
|Android|Xamarin.Forms.Platform.Android|Grupie widoków|Widok|
|Platforma uniwersalna systemu Windows (UWP)|Xamarin.Forms.Platform.UWP|FrameworkElement|FrameworkElement|

Każdy specyficzne dla platformy `PlatformEffect` klasy udostępnia następujące właściwości:

- `Container` — odwołuje się do sterowania specyficzne dla platformy są używane do implementowania układu.
- `Control` — odwołuje się do sterowania specyficzne dla platformy, są używane do implementowania kontroli platformy Xamarin.Forms.
- `Element` — odwołuje się formant platformy Xamarin.Forms, który jest renderowany.

Efekty nie ma informacji o typie o kontenera, kontroli lub są one dołączone do, ponieważ może być dołączone do dowolnych elementu. W związku z tym efekt jest dołączony do elementu, który nie obsługuje on powinien bezpiecznie zmniejszyć lub zgłosić wyjątek. Jednak `Container`, `Control`, i `Element` właściwości mogą być rzutowane na ich typ implementujący. Dla więcej informacji o tych typów zobacz [renderowania klasy podstawowej i kontrolki natywne](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Każdy specyficzne dla platformy `PlatformEffect` klasy udostępnia następujące metody, które musi zostać zastąpiona w celu wykonania wpływ:

- [`OnAttached`](https://developer.xamarin.com/api/member/Xamarin.Forms.Effect.OnAttached()/) — wywoływane, gdy wpływ jest dołączony do formantu platformy Xamarin.Forms. Zastąpiona wersja tej metody, w każdej klasie efekt platfom specyficzne jest miejscem do wykonania Dostosowywanie formantu, wraz z obsługą wyjątków w przypadku efekt nie można zastosować do określonego formantu platformy Xamarin.Forms.
- [`OnDetached`](https://developer.xamarin.com/api/member/Xamarin.Forms.Effect.OnDetached()/) — wywoływane, gdy efekt jest odłączony od formantu platformy Xamarin.Forms. Zastąpiona wersja tej metody, w każdej klasie efekt specyficzne dla platformy jest miejscem do wykonania oczyszczania efekt takich jak deserializować rejestrowania programu obsługi zdarzeń.

Ponadto `PlatformEffect` przedstawia [ `OnElementPropertyChanged` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformEffect%3CTContainer,TControl%3E.OnElementPropertyChanged/p/System.ComponentModel.PropertyChangedEventArgs/) metodę, która również może zostać zastąpiona. Ta metoda jest wywoływana, gdy zmieniono właściwość elementu. Zastąpiona wersja tej metody, w każdej klasie specyficzne dla platformy efekt jest miejscem do reagowania na zmiany właściwości możliwej do wiązania w formancie platformy Xamarin.Forms. Sprawdź właściwości, które uległy zmianie zawsze należy, jak to zastąpienie może zostać wywołana wiele razy.


## <a name="related-links"></a>Linki pokrewne

- [Niestandardowe programy renderujące](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
