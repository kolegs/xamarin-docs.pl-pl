---
title: Szablony danych zestawu narzędzi Xamarin.Forms
description: Obiekt DataTemplate służy do określania wyglądu danych na obsługiwanych formantów i zwykle wiąże dane mają być wyświetlane.
ms.prod: xamarin
ms.assetid: 838F4BDB-B719-457F-8633-27E9B267A2A0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/11/2017
ms.openlocfilehash: 771ae22c3e28a4fce758bbfd6a3bd63bafb75e53
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994974"
---
# <a name="xamarinforms-data-templates"></a>Szablony danych zestawu narzędzi Xamarin.Forms

_Obiekt DataTemplate służy do określania wyglądu danych na obsługiwanych formantów i zwykle wiąże dane mają być wyświetlane._

## <a name="introductionintroductionmd"></a>[Wprowadzenie](introduction.md)

Szablony danych zestawu narzędzi Xamarin.Forms zapewniają możliwość definiowania prezentacji danych na obsługiwanych formantów. Ten artykuł zawiera wprowadzenie do szablonów danych, sprawdzając, dlatego ich użycie jest konieczne.

## <a name="creating-a-datatemplatecreatingmd"></a>[Tworzenie DataTemplate](creating.md)

Szablony danych mogą być tworzone w tekście, w [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary), lub z typu niestandardowego lub odpowiedni typ komórki zestawu narzędzi Xamarin.Forms. Wbudowany szablon należy używać, jeśli nie ma potrzeby to ponowne użycie szablonu danych w innym miejscu. Alternatywnie szablonu danych może nastąpić, definiując je jako typ niestandardowy, lub poziom kontroli zasobu strony lub na poziomie aplikacji.

## <a name="creating-a-datatemplateselectorselectormd"></a>[Tworzenie DataTemplateSelector](selector.md)

A [ `DataTemplateSelector` ](xref:Xamarin.Forms.DataTemplateSelector) może służyć do wybierz [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) w czasie wykonywania na podstawie wartości właściwości powiązanych z danymi. Dzięki temu wiele `DataTemplate` wystąpień, które mają być stosowane do tego samego typu obiektu, aby dostosować wygląd obiektów określonego. W tym artykule przedstawiono sposób tworzenia i wykorzystywania `DataTemplateSelector`.


## <a name="related-links"></a>Linki pokrewne

- [Szablony danych (przykład)](https://developer.xamarin.com/samples/xamarin-forms/templates/datatemplates/)
