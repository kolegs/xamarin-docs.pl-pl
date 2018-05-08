---
title: Lokalizacja platformy Xamarin.Forms
description: Wbudowane .NET framework lokalizacja może służyć do tworzenia i platform aplikacji wielojęzycznych za pomocą platformy Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 97BF843B-BDAA-4CEA-8189-6DB54B291D7F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/13/2018
ms.openlocfilehash: 29624eeccc8542b3296774f6b77480b664bee478
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/07/2018
---
# <a name="localization"></a>Lokalizacja

_Wbudowane .NET framework lokalizacja może służyć do tworzenia i platform aplikacji wielojęzycznych za pomocą platformy Xamarin.Forms._

## <a name="string-and-image-localizationtextmd"></a>[Ciąg i Lokalizacja obrazu](text.md)

Wbudowany mechanizm lokalizowania zastosowań aplikacji .NET [pliki RESX](http://msdn.microsoft.com/library/ekyft91f(v=vs.90).aspx) i klasy w `System.Resources` i `System.Globalization` przestrzeni nazw. Pliki RESX zawierający przetłumaczone ciągi są osadzone w zestawie platformy Xamarin.Forms, wraz z klasy generowane przez kompilator, która udostępnia silnie typizowane tłumaczenia. Następnie można pobrać przetłumaczony tekst w kodzie.

## <a name="right-to-left-localizationright-to-leftmd"></a>[Lokalizacja od prawej do lewej](right-to-left.md)

Kierunek przepływu jest kierunek, w którym elementy interfejsu użytkownika na stronie są skanowane przez oczu. Lokalizacja od prawej do lewej dodaje obsługę kierunek przepływu od prawej do lewej dla aplikacji platformy Xamarin.Forms.
