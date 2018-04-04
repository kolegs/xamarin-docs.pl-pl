---
title: Style
description: Dostosowywanie wyglądu za pomocą stylów
ms.prod: xamarin
ms.assetid: 344A34AA-B19A-4765-BC8A-875D9A6B5EA8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: 7a19f7597ee17282bc8b41e7f0e7e3ade2361a50
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="styles"></a>Style

## <a name="introductionintroductionmd"></a>[Wprowadzenie](introduction.md)

Aplikacje platformy Xamarin.Forms często zawierają wiele formantów, które mają identyczne wyglądu. Ustawianie wygląd każdego pojedynczego formantu może być powtarzane i błąd podatnych na błędy. Zamiast tego style mogą być tworzone umożliwiające dostosowanie wyglądu formantu przez grupowanie i ustawienia właściwości dostępnych dla typu formantu.

## <a name="explicit-stylesexplicitmd"></a>[Style jawne](explicit.md)

*Jawne* styl to taki, który jest wybiórczo zastosować do kontroli przez ustawienie ich [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) właściwości.

## <a name="implicit-stylesimplicitmd"></a>[Style niejawne](implicit.md)

*Niejawne* styl to taki, który jest używany przez wszystkie formanty tego samego [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/), bez konieczności styl odwołanie do każdego formantu.

## <a name="global-stylesapplicationmd"></a>[Style globalne](application.md)

Style mogą być udostępniane globalnie przez dodanie ich do aplikacji [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/). Pozwala to uniknąć duplikowania style na stronach lub formantów.

## <a name="style-inheritanceinheritancemd"></a>[Dziedziczenie stylów](inheritance.md)

Style może dziedziczyć z innych style, aby zmniejszyć dublowania i włączanie ponownego użycia.

## <a name="dynamic-stylesdynamicmd"></a>[Style dynamiczne](dynamic.md)

Style nie odpowiadają na zmiany właściwości i pozostają niezmienione na czas trwania aplikacji. Jednak aplikacje może odpowiadać na zmiany stylu dynamicznie w czasie wykonywania za pomocą dynamicznej zasobów.

## <a name="device-stylesdevicemd"></a>[Style urządzenia](device.md)

Platformy Xamarin.Forms obejmuje sześć *dynamiczne* style, znany jako *urządzenia* style w [ `Devices.Styles` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Device+Styles/) klasy. Wszystkie sześć style można zastosować do [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) tylko wystąpienia.
