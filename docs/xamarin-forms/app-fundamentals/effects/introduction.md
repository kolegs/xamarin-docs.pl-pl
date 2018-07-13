---
title: Wprowadzenie do efektów
description: Efekty Zezwalaj natywne kontrolki na każdej z platform można dostosować i są zwykle używane do zmiany stylów małe. Ten artykuł zawiera wprowadzenie do efektów, przedstawia granic między efekty i niestandardowe programy renderujące i opisuje klasy PlatformEffect.
ms.prod: xamarin
ms.assetid: 30CB8615-8F39-4762-BDB7-333D2B57D112
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: d3fa958e999a10832d5fa15e4190077955b0e6df
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997385"
---
# <a name="introduction-to-effects"></a>Wprowadzenie do efektów

_Efekty Zezwalaj natywne kontrolki na każdej z platform można dostosować i są zwykle używane do zmiany stylów małe. Ten artykuł zawiera wprowadzenie do efektów, przedstawia granic między efekty i niestandardowe programy renderujące i opisuje klasy PlatformEffect._

Zestaw narzędzi Xamarin.Forms [stron, układy i kontrolek](~/xamarin-forms/user-interface/controls/index.md) przedstawia wspólny interfejs API do opisywania interfejsów użytkownika mobilnego dla wielu platform. Każdej strony układu i kontroli jest inaczej renderowane na każdej platformie za pomocą `Renderer` klasę, która z kolei tworzy formant natywnego (odpowiadających na reprezentację w postaci zestawu narzędzi Xamarin.Forms) rozmieszcza go na ekranie, a następnie dodaje zachowanie, które określono w udostępnionej Kod.

Deweloperzy mogą implementować własne niestandardowe `Renderer` klasy, aby dostosować wygląd lub zachowanie kontrolki. Jednak implementacja klasy niestandardowego modułu renderowania do wykonania dostosowywania prosty formant jest często odpowiedzi ciężki. Efekty upraszcza ten proces, dzięki czemu natywne kontrolki na poszczególnych platform, aby łatwiej można dostosować.

Efekty są tworzone w projektach specyficzne dla platformy przez podklasy `PlatformEffect` kontrolę i efektów, które są używane przez dołączenie w kontrolce biblioteki zestawu narzędzi Xamarin.Forms .NET Standard lub Projekt Biblioteka udostępniona.

## <a name="why-use-an-effect-over-a-custom-renderer"></a>Dlaczego warto używać efektu za pośrednictwem niestandardowego modułu renderowania?

Efekty uprościć Dostosowywanie kontrolki, wielokrotnego użytku i mogą być parametryzowane w celu dalszego zwiększenia ponownego użycia.

Wszystko, co można osiągnąć przy użyciu efektu również można osiągnąć za pomocą niestandardowego modułu renderowania. Jednak niestandardowe programy renderujące oferuje większą elastyczność i możliwości dostosowania niż efekty. Poniższe wskazówki listy okoliczności, w której ma zostać wybierz efekt za pośrednictwem niestandardowego modułu renderowania:

- Efekt jest zalecane, gdy zmiana właściwości kontrolki specyficzne dla platformy osiągnąć oczekiwany rezultat.
- Niestandardowego modułu renderowania jest wymagany, gdy istnieje potrzeba do zastępowania metod kontroli specyficzne dla platformy.
- Niestandardowego modułu renderowania jest wymagany, gdy trzeba zastąpić formant specyficzne dla platformy, który implementuje kontrolkę zestawu narzędzi Xamarin.Forms.

## <a name="subclassing-the-platformeffect-class"></a>Tworzenie podklasy klasy PlatformEffect

W poniższej tabeli wymieniono przestrzeń nazw dla `PlatformEffect` klasy w każdej z platform i typów właściwości:

|Platforma|Przestrzeń nazw|Kontener|Formant|
|--- |--- |--- |--- |
|iOS|Xamarin.Forms.Platform.iOS|UIView|UIView|
|Android|Xamarin.Forms.Platform.Android|Grupie widoków|Widok|
|Platforma uniwersalna systemu Windows (UWP)|Xamarin.Forms.Platform.UWP|FrameworkElement|FrameworkElement|

Każdy specyficzne dla platformy `PlatformEffect` klasa udostępnia następujące właściwości:

- `Container` — odwołuje się do sterowania specyficzne dla platformy, używany do implementowania układu.
- `Control` — odwołuje się do sterowania specyficzne dla platformy, używany do implementowania kontroli zestawu narzędzi Xamarin.Forms.
- `Element` — odwołuje się do kontrolki zestawu narzędzi Xamarin.Forms, który jest renderowany.

Efekty nie mają typu informacji na temat kontenerów, formantu lub elementu, które są dołączone do ponieważ one mogą być dołączane do dowolnego elementu. W związku z tym gdy efekt jest dołączony do elementu, który nie jest obsługiwana jest powinien pogarszanie lub zgłosić wyjątek. Jednak `Container`, `Control`, i `Element` właściwości mogą być rzutowane na ich typie implementującym. Aby dowiedzieć się więcej o tych typów, zobacz [natywne kontrolki i klasy podstawowej renderowania](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Każdy specyficzne dla platformy `PlatformEffect` klasa udostępnia następujące metody, które musi zostać zastąpiona w celu zaimplementowania wpływ:

- [`OnAttached`](xref:Xamarin.Forms.Effect.OnAttached) -wywoływana, gdy efekt jest dołączony do kontrolki zestawu narzędzi Xamarin.Forms. Zastąpione wersję tej metody, w każdej klasie specyficzne dla platfom efekt jest w tym miejscu Przeprowadź Dostosowywanie formantu, wraz z obsługi wyjątków w przypadku, gdy efekt nie można zastosować do określonej kontrolki zestawu narzędzi Xamarin.Forms.
- [`OnDetached`](xref:Xamarin.Forms.Effect.OnDetached) -wywoływana, gdy efekt jest odłączony od kontrolki zestawu narzędzi Xamarin.Forms. Zastąpione wersję tej metody, w każdej klasie efekt specyficzne dla platformy jest wykonanie czyszczenia efekt takich jak wyrejestrowywanie program obsługi zdarzeń w tym miejscu.

Ponadto `PlatformEffect` udostępnia [ `OnElementPropertyChanged` ](xref:Xamarin.Forms.PlatformEffect`2.OnElementPropertyChanged(System.ComponentModel.PropertyChangedEventArgs)) metody, która również może zostać zastąpiona. Ta metoda jest wywoływana, gdy zmieniono właściwość elementu. Zastąpione wersję tej metody, w każdej klasie efekt specyficzne dla platformy jest używana do reagowania na zmiany właściwości możliwej do wiązania dla kontrolki zestawu narzędzi Xamarin.Forms. Sprawdź właściwości, które uległy zmianie powinien zawsze się, jak to zastąpienie może być wywoływana wiele razy.


## <a name="related-links"></a>Linki pokrewne

- [Niestandardowe programy renderujące](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
