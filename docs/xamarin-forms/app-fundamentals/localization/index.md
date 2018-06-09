---
title: Lokalizacja platformy Xamarin.Forms
description: Wbudowane .NET framework lokalizacja może służyć do tworzenia i platform aplikacji wielojęzycznych za pomocą platformy Xamarin.Forms. Tekst i obrazy można lokalizować i aplikacje mogą obsługiwać kierunek przepływu od prawej do lewej.
ms.prod: xamarin
ms.assetid: 97BF843B-BDAA-4CEA-8189-6DB54B291D7F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/13/2018
ms.openlocfilehash: 78731924324a1ddd34c0d197070699e2998c1513
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35240596"
---
# <a name="xamarinforms-localization"></a>Lokalizacja platformy Xamarin.Forms

_Wbudowane .NET framework lokalizacja może służyć do tworzenia i platform aplikacji wielojęzycznych za pomocą platformy Xamarin.Forms._

## <a name="string-and-image-localizationtextmd"></a>[Lokalizacja ciągów i obrazów](text.md)

Wbudowany mechanizm lokalizowania zastosowań aplikacji .NET [pliki RESX](http://msdn.microsoft.com/library/ekyft91f(v=vs.90).aspx) i klasy w `System.Resources` i `System.Globalization` przestrzeni nazw. Pliki RESX zawierający przetłumaczone ciągi są osadzone w zestawie platformy Xamarin.Forms, wraz z klasy generowane przez kompilator, która udostępnia silnie typizowane tłumaczenia. Następnie można pobrać przetłumaczony tekst w kodzie.

## <a name="right-to-left-localizationright-to-leftmd"></a>[Lokalizacja w przypadku języków zapisywanych od prawej do lewej](right-to-left.md)

Kierunek przepływu jest kierunek, w którym elementy interfejsu użytkownika na stronie są skanowane przez oczu. Lokalizacja od prawej do lewej dodaje obsługę kierunek przepływu od prawej do lewej dla aplikacji platformy Xamarin.Forms.
