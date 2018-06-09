---
title: Szablony danych platformy Xamarin.Forms
description: Obiekt DataTemplate służy do określania wyglądu danych na obsługiwanych formantów i zazwyczaj wiąże dane mają być wyświetlane.
ms.prod: xamarin
ms.assetid: 838F4BDB-B719-457F-8633-27E9B267A2A0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/11/2017
ms.openlocfilehash: b461cb8be9dcf618b7a497b001186fde5f2b2aa1
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35239859"
---
# <a name="xamarinforms-data-templates"></a>Szablony danych platformy Xamarin.Forms

_Obiekt DataTemplate służy do określania wyglądu danych na obsługiwanych formantów i zazwyczaj wiąże dane mają być wyświetlane._

## <a name="introductionintroductionmd"></a>[Wprowadzenie](introduction.md)

Szablony danych platformy Xamarin.Forms zapewniają możliwość definiowania prezentację danych na obsługiwanych formantów. Ten artykuł zawiera wprowadzenie do szablonów danych, sprawdzając, dlatego ich użycie jest konieczne.

## <a name="creating-a-datatemplatecreatingmd"></a>[Tworzenie DataTemplate](creating.md)

Szablony danych można tworzyć w tekście, w [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/), lub niestandardowy typ lub odpowiedniego typu komórki platformy Xamarin.Forms. Szablon wbudowanego należy używać, gdy nie istnieje potrzeba ponowne użycie szablonu danych w innym miejscu. Alternatywnie szablon danych mogą być ponownie używane, definiując go jako niestandardowy typ, lub poziom kontroli zasobów strony, lub na poziomie aplikacji.

## <a name="creating-a-datatemplateselectorselectormd"></a>[Tworzenie DataTemplateSelector](selector.md)

A [ `DataTemplateSelector` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/) można wybrać [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) w czasie wykonywania na podstawie wartości właściwości powiązanych z danymi. Dzięki temu wiele `DataTemplate` wystąpień, które ma zostać zastosowany do tego samego typu obiektu, aby dostosować wygląd określonego obiektów. W tym artykule przedstawiono sposób tworzenia i zużywać `DataTemplateSelector`.


## <a name="related-links"></a>Linki pokrewne

- [Szablony danych (przykład)](https://developer.xamarin.com/samples/xamarin-forms/templates/datatemplates/)
