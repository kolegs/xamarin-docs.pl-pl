---
title: eXtensible Application Markup Language (XAML)
description: XAML jest językiem znaczników deklaratywne, który może służyć do definiowania interfejsów użytkownika. Interfejs użytkownika jest zdefiniowany w pliku XML przy użyciu składni języka XAML podczas zachowania w czasie wykonywania jest zdefiniowany w pliku oddzielne kodem.
ms.prod: xamarin
ms.assetid: CD30EECC-8AC1-4CF5-A4FE-348420A6231E
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 06/18/2018
ms.openlocfilehash: c040c12829708418d0a705b8e9f930989900c678
ms.sourcegitcommit: 7a89735aed9ddf89c855fd33928915d72da40c2d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36209430"
---
# <a name="extensible-application-markup-language-xaml"></a>eXtensible Application Markup Language (XAML)

_XAML jest językiem znaczników deklaratywne, który może służyć do definiowania interfejsów użytkownika. Interfejs użytkownika jest zdefiniowany w pliku XML przy użyciu składni języka XAML podczas zachowania w czasie wykonywania jest zdefiniowany w pliku oddzielne kodem._

> [!VIDEO https://youtube.com/embed/H6UOrSyhTEE]

**Rozwija 2016: Staje się głównym XAML**

> [!NOTE]
> Wypróbuj [Podgląd standardowe XAML](standard/index.md)

<a name="xaml" />

## <a name="xaml-basicsxaml-basicsindexmd"></a>[XAML — podstawy](xaml-basics/index.md)

XAML umożliwia deweloperom interfejsów użytkownika w aplikacji platformy Xamarin.Forms przy użyciu znaczników zamiast kodu. XAML nigdy nie jest wymagana w programie platformy Xamarin.Forms, ale biorąc i jest często bardziej wizualnie spójnego i zwięzły więcej niż równoważne kodu. XAML jest szczególnie dobrze nadają się do użytku z popularnych architektury Model-View-ViewModel (MVVM) w aplikacji: XAML definiuje widok, który jest powiązany kod ViewModel za pośrednictwem powiązania danych opartych na języku XAML.

## <a name="xaml-compilationxamlcmd"></a>[Kompilacja języka XAML](xamlc.md)

XAML mogą być opcjonalnie kompilowane bezpośrednio na język pośredni (IL) z kompilatora XAML (XAMLC). W tym artykule opisano sposób użycia XAMLC i korzyści.

## <a name="xaml-previewerxaml-previewermd"></a>[Program podglądu języka XAML](xaml-previewer.md)

[Podglądu plików XAML](~/xamarin-forms/xaml/xaml-previewer.md) ogłoszenia na Xamarin rozwijać 2016 jest dostępny do testowania kanału alfa.

## <a name="xaml-namespacesnamespacesmd"></a>[Przestrzeń nazw XAML](namespaces.md)

Używa XAML `xmlns` atrybutu XML dla deklaracji przestrzeni nazw. W tym artykule przedstawiono składnię przestrzeni nazw XAML i pokazuje, jak można zadeklarować przestrzeni nazw XAML uzyskiwać dostęp do typu.

## <a name="xaml-markup-extensionsmarkup-extensionsindexmd"></a>[Rozszerzenia struktury znaczników XAML](markup-extensions/index.md)

XAML zawiera rozszerzenia znaczników do ustawiania atrybutów do wartości lub obiektów poza może zostać wyrażona z ciągami proste. Obejmują one odwołujące się do stałe, statycznej właściwości i pola słownikach zasobów i powiązania danych.

## <a name="field-modifiersfield-modifiersmd"></a>[Modyfikatorów pól](field-modifiers.md)

`x:FieldModifier` Atrybutu namespace określa poziom dostępu dla wygenerowanego pól nazwanych elementów XAML.

## <a name="passing-argumentspassing-argumentsmd"></a>[Przekazywanie argumentów](passing-arguments.md)

XAML może służyć do przekazywania argumentów innych niż domyślne konstruktorów lub metody fabryki. W tym artykule przedstawiono przy użyciu atrybutów XAML, które mogą służyć do przekazywania argumentów konstruktorów, wywoływanie metod fabryki i określić typ argument rodzajowy.

## <a name="bindable-propertiesbindable-propertiesmd"></a>[Właściwości możliwe do wiązania](bindable-properties.md)

W platformy Xamarin.Forms funkcji wspólnego języka środowiska uruchomieniowego (języka wspólnego CLR) właściwości zostanie przedłużony właściwości. Właściwości możliwej do wiązania jest specjalnym rodzajem właściwości, gdy wartość właściwości jest śledzony przez system właściwości platformy Xamarin.Forms. Ten artykuł zawiera wprowadzenie do właściwości i pokazuje, jak utworzyć i używać ich.

## <a name="attached-propertiesattached-propertiesmd"></a>[Dołączone właściwości](attached-properties.md)

Dołączona właściwość jest specjalnym rodzajem właściwości możliwej do wiązania, zdefiniowany w jedną klasę, ale dołączone do innych obiektów i rozpoznawalną w języku XAML jako atrybutu, który zawiera klasę i nazwę właściwości oddzielone kropką. Ten artykuł zawiera wprowadzenie do dołączonych właściwości i pokazuje, jak utworzyć i używać ich.

## <a name="resource-dictionariesresource-dictionariesmd"></a>[Słowniki zasobów](resource-dictionaries.md)

Zasoby XAML są definicje obiektów, które mogą być używane w więcej niż raz. A [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) umożliwia zasoby zdefiniowane w jednej lokalizacji i ponownego użycia w aplikacji platformy Xamarin.Forms. W tym artykule przedstawiono sposób tworzenia i zużywać `ResourceDictionary`oraz sposób scalania, co `ResourceDictionary` do innego.
