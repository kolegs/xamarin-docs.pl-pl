---
title: Animacja w interfejsie Xamarin.Forms
description: Zestaw narzędzi Xamarin.Forms zawiera własną infrastrukturę animacji, która jest prosta do tworzenia proste animacje, będąc również czytanych tworzenie złożonych animacji.
ms.prod: xamarin
ms.assetid: AC0B4127-ECA3-44DA-8A24-A2B10A275083
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/14/2016
ms.openlocfilehash: bebb3e32f298a2079069787dba3453e1817cf64f
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998309"
---
# <a name="animation-in-xamarinforms"></a>Animacja w interfejsie Xamarin.Forms

_Zestaw narzędzi Xamarin.Forms zawiera własną infrastrukturę animacji, która jest prosta do tworzenia proste animacje, będąc również czytanych tworzenie złożonych animacji._

Klasy animacji Xamarin.Forms docelowych różnych właściwości obiektu elementy wizualne typowej animacji, stopniowo zmienia właściwość z jednej wartości do innego w okresie czasu. Zwróć uwagę, że nie interfejs XAML zestawu narzędzi Xamarin.Forms klasy animacji. Jednak animacji są umieszczane w [zachowania](~/xamarin-forms/app-fundamentals/behaviors/index.md) następnie odwołania z XAML.

## <a name="simple-animationssimplemd"></a>[Proste animacje](simple.md)

[ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions) Klasa udostępnia metody rozszerzenia, które może służyć do konstruowania proste animacje, obracanie, skalowanie, tłumaczenie i zanikanie [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) wystąpień. W tym artykule przedstawiono tworzenie i anulowanie animacji przy użyciu `ViewExtensions` klasy.

## <a name="easing-functionseasingmd"></a>[Funkcje easingu](easing.md)

Obejmuje zestaw narzędzi Xamarin.Forms [ `Easing` ](xref:Xamarin.Forms.Easing) klasy, która pozwala na określenie funkcji przenoszenia, który kontroluje, jak przyspieszyć animacji lub spowolnić, ponieważ są one uruchamiane. W tym artykule przedstawiono, jak używać wstępnie zdefiniowanych funkcji sterowania tempem zmian oraz sposób tworzenia niestandardowych funkcji sterowania tempem zmian.

## <a name="custom-animationscustommd"></a>[Niestandardowe animacje](custom.md)

[ `Animation` ](xref:Xamarin.Forms.Animation) Klasa jest elementem składowym wszystkie animacje Xamarin.Forms, za pomocą metody rozszerzające w [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions) klasy tworzenia co najmniej jeden `Animation` obiektów. W tym artykule przedstawiono sposób użycia `Animation` klasa do tworzenia i anulować animacji, synchronizowanie wielu animacji i utworzyć niestandardowe animacje, które animować właściwości, które nie są animowane za pomocą istniejących metod animacji.
