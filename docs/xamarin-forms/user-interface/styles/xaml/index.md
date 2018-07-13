---
title: Określanie stylów aplikacji Xamarin.Forms przy użyciu stylów XAML
description: Ten przewodnik wyjaśnia, jak dostosować wygląd aplikacji Xamarin.Forms przy użyciu stylów XAML.
ms.prod: xamarin
ms.assetid: 344A34AA-B19A-4765-BC8A-875D9A6B5EA8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: 5145572b30302e58c36250fff40e8b637fcd221f
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995078"
---
# <a name="styling-xamarinforms-apps-using-xaml-styles"></a>Określanie stylów aplikacji Xamarin.Forms przy użyciu stylów XAML

## <a name="introductionintroductionmd"></a>[Wprowadzenie](introduction.md)

Aplikacje Xamarin.Forms często zawierają wiele formantów, które mają identyczne wygląd. Ustawienie wygląd każdego pojedynczego formantu może być powtarzane i występowania błędów. Zamiast tego stylów mogą być tworzone umożliwiające dostosowanie wyglądu formantu przez grupowanie i właściwości ustawień dostępnych w kontrolce o typie.

## <a name="explicit-stylesexplicitmd"></a>[Style jawne](explicit.md)

*Jawne* styl to taki, który jest wybiórczo zastosować do kontrolek, ustawiając ich [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) właściwości.

## <a name="implicit-stylesimplicitmd"></a>[Style niejawne](implicit.md)

*Niejawne* styl to taki, który jest używany przez wszystkie kontrolki w tej samej [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType), bez konieczności każdy formant, aby odwołać stylu.

## <a name="global-stylesapplicationmd"></a>[Style globalne](application.md)

Style mogą być udostępniane globalnie, dodając je do aplikacji [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary). Pomaga to uniknąć duplikowania style różnych stron i formantów.

## <a name="style-inheritanceinheritancemd"></a>[Dziedziczenie stylów](inheritance.md)

Style może dziedziczyć z innymi stylami, aby zmniejszyć dublowania i umożliwiają wielokrotne użycie.

## <a name="dynamic-stylesdynamicmd"></a>[Style dynamiczne](dynamic.md)

Style nie reagować na zmiany właściwości i pozostają bez zmian w czasie trwania operacji aplikacji. Jednak aplikacje może reagować na zmiany wprowadzone w stylu dynamicznie w czasie wykonywania przy użyciu dynamicznych zasobów.

## <a name="device-stylesdevicemd"></a>[Style urządzenia](device.md)

Xamarin.Forms obejmuje sześć *dynamiczne* stylów, znane jako *urządzenia* style w [ `Devices.Styles` ](xref:Xamarin.Forms.Device.Styles) klasy. Wszystkie sześć style mogą być stosowane do [ `Label` ](xref:Xamarin.Forms.Label) tylko wystąpień.
