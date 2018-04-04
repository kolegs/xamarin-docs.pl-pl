---
title: Dostosowywanie powiązania
description: Powiązanie z platformy Xamarin.Android można dostosować, edytując metadanych, która kontroluje proces wiązania. Ręczne modyfikacje często są niezbędne w celu rozwiązania błędów kompilacji i kształtowania wynikowy interfejsu API, aby był bardziej spójny z C# / .NET. Te przewodniki opisano strukturę tych metadanych, jak modyfikować metadanych i sposobu użycia JavaDoc odzyskać nazwy parametrów metody.
ms.prod: xamarin
ms.assetid: 63C5078D-9E42-4F70-AF8C-8CEEA84FB6AF
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 09/25/2017
ms.openlocfilehash: bb4f3b24be2072cb8b33893899a23951ace63607
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="customizing-bindings"></a>Dostosowywanie powiązania

_Powiązanie z platformy Xamarin.Android można dostosować, edytując metadanych, która kontroluje proces wiązania. Ręczne modyfikacje często są niezbędne w celu rozwiązania błędów kompilacji i kształtowania wynikowy interfejsu API, aby był bardziej spójny z C# / .NET. Te przewodniki opisano strukturę tych metadanych, jak modyfikować metadanych i sposobu użycia JavaDoc odzyskać nazwy parametrów metody._


## <a name="overview"></a>Omówienie
 
Xamarin.Android automatyzuje większa część procesu powiązania; Jednak w niektórych przypadkach ręcznej modyfikacji jest wymagany, aby rozwiązać następujące problemy:

-   Rozpoznawanie kompilacji błędy spowodowane przez brak typów, zaciemnionego typy zduplikowanych nazw, klasy widoczność problemów i innych sytuacjach, w których nie można rozwiązać przez narzędzi platformy Xamarin.Android. 

-   Zmiana mapowanie, które korzysta z platformy Xamarin.Android powiązać interfejsu API systemu Android do różnych typów w języku C# (na przykład wielu deweloperów wolą mapy Java `int` stałe dla C# `enum` stałe).

-   Usuwanie nieużywanych typów, które nie muszą być powiązane. 

-   Dodawanie typów, które nie mają odpowiedników w podstawowej interfejsu API języka Java. 

Możesz wprowadzić niektóre lub wszystkie te zmiany, modyfikując metadanych, która kontroluje proces wiązania.


## <a name="guides"></a>Przewodniki

Poniższe przewodniki opisano metadanych, która kontroluje proces wiązania oraz wyjaśniono, jak można zmodyfikować te metadane w celu rozwiązania tych problemów:

-   [Metadane powiązania Java](~/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata.md) zawiera omówienie metadanych, który jest przesyłany do powiązania Java.
    Opisuje różne ręczne wykonanie czynności, które czasami są wymagane do przeprowadzenia biblioteki powiązanie Java i wyjaśniono, jak kształtu interfejsu API udostępnianych przez powiązanie z lepiej jest zgodna z wytycznymi projektowania platformy .NET.

-   [Nazwy parametrów z Javadoc](~/android/platform/binding-java-library/customizing-bindings/naming-parameters-with-javadoc.md) wyjaśniono, jak odzyskać nazwy parametrów w projekcie powiązanie języka Java przy użyciu Javadoc z powiązania projektu języka Java.


 

