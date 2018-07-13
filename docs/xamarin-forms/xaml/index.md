---
title: eXtensible Application Markup Language (XAML)
description: XAML jest deklaratywnym językiem znaczników, który może służyć do definiowania interfejsów użytkownika. Interfejs użytkownika jest zdefiniowana w pliku XML przy użyciu składni XAML, podczas gdy zachowania w czasie wykonywania jest zdefiniowana w pliku oddzielne związanym z kodem.
ms.prod: xamarin
ms.assetid: CD30EECC-8AC1-4CF5-A4FE-348420A6231E
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 06/18/2018
ms.openlocfilehash: f593e5d084d8cd7071d17195663478d430d994b7
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995485"
---
# <a name="extensible-application-markup-language-xaml"></a>eXtensible Application Markup Language (XAML)

_XAML jest deklaratywnym językiem znaczników, który może służyć do definiowania interfejsów użytkownika. Interfejs użytkownika jest zdefiniowana w pliku XML przy użyciu składni XAML, podczas gdy zachowania w czasie wykonywania jest zdefiniowana w pliku oddzielne związanym z kodem._

> [!VIDEO https://youtube.com/embed/H6UOrSyhTEE]

**Rozwój 2016: Staje się głównym XAML**

> [!NOTE]
> Wypróbuj [XAML Standard (wersja zapoznawcza)](standard/index.md)

<a name="xaml" />

## <a name="xaml-basicsxaml-basicsindexmd"></a>[XAML — podstawy](xaml-basics/index.md)

XAML umożliwia deweloperom definiowanie interfejsów użytkownika w aplikacjach Xamarin.Forms przy użyciu znaczników zamiast kodu. XAML nigdy nie jest wymagane w programie Xamarin.Forms, ale biorąc i jest często bardziej spójną wizualnej i bardziej zwięzłą niż równoważny kod. XAML szczególnie dobrze nadaje się do użytku z popularnej architektury Model-View-ViewModel (MVVM) w aplikacji: XAML definiuje widok, który jest połączony z ViewModel kodu za pomocą powiązania danych opartych na XAML.

## <a name="xaml-compilationxamlcmd"></a>[Kompilacja języka XAML](xamlc.md)

XAML może być opcjonalnie kompilowane bezpośrednio do języka pośredniego (IL) za pomocą kompilatora XAML (XAMLC). W tym artykule opisano sposób używania XAMLC i jego korzyści.

## <a name="xaml-previewerxaml-previewermd"></a>[Program podglądu języka XAML](xaml-previewer.md)

[Podgląd XAML programu](~/xamarin-forms/xaml/xaml-previewer.md) ogłoszeniem na konferencji Xamarin Evolve 2016 jest dostępny do testowania w kanał alfa.

## <a name="xaml-namespacesnamespacesmd"></a>[Przestrzeń nazw XAML](namespaces.md)

Używa XAML `xmlns` atrybut deklaracje przestrzeni nazw XML. W tym artykule przedstawiono składnię przestrzeń nazw XAML i pokazuje, jak deklarować przestrzeń nazw XAML uzyskiwać dostęp do typu.

## <a name="xaml-markup-extensionsmarkup-extensionsindexmd"></a>[Rozszerzenia struktury znaczników XAML](markup-extensions/index.md)

XAML zawiera rozszerzenia znaczników dla ustawienie atrybutów do wartości lub obiektów poza mogą być wyrażone za pomocą zwykłe ciągi. Należą do nich odwołuje się do stałych, właściwości statycznych i pola, słowniki zasobów i powiązania danych.

## <a name="field-modifiersfield-modifiersmd"></a>[Modyfikatory pól](field-modifiers.md)

`x:FieldModifier` Atrybut namespace określa poziom dostępu dla wygenerowanego pól nazwanych elementów XAML.

## <a name="passing-argumentspassing-argumentsmd"></a>[Przekazywanie argumentów](passing-arguments.md)

XAML może służyć do przekazywania argumentów do innych niż domyślne konstruktory lub metod fabryki. W tym artykule przedstawiono, przy użyciu atrybutów XAML, które mogą służyć do przekazywania argumentów do konstruktorów, wywoływanie metod fabryki i określić typ argumentu ogólnego.

## <a name="bindable-propertiesbindable-propertiesmd"></a>[Właściwości możliwe do wiązania](bindable-properties.md)

W interfejsie Xamarin.Forms funkcje wspólne właściwości środowiska uruchomieniowego (języka wspólnego CLR) języka jest rozszerzany, które można powiązać właściwości. Właściwości możliwej do wiązania to specjalny rodzaj właściwości, których wartość właściwości jest śledzona przez system właściwości zestawu narzędzi Xamarin.Forms. Ten artykuł zawiera wprowadzenie do właściwości możliwej do wiązania i pokazuje, jak utworzyć i korzystać z nich.

## <a name="attached-propertiesattached-propertiesmd"></a>[Dołączone właściwości](attached-properties.md)

Dołączona właściwość to specjalny rodzaj właściwości możliwej do wiązania, zdefiniowane w jedną klasę, ale dołączone do innych obiektów i rozpoznawalną w XAML jako atrybutu, który zawiera klasę i nazwę właściwości oddzielony kropką. Ten artykuł zawiera wprowadzenie do dołączonych właściwości i pokazuje, jak utworzyć i korzystać z nich.

## <a name="resource-dictionariesresource-dictionariesmd"></a>[Słowniki zasobów](resource-dictionaries.md)

Zasoby XAML są definicje obiektów, których można użyć więcej niż jeden raz. A [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) umożliwia zasoby zdefiniowane w obrębie jednej lokalizacji, i ponownie używane w całej aplikacji platformy Xamarin.Forms. W tym artykule przedstawiono sposób tworzenia i wykorzystywania `ResourceDictionary`oraz sposób scalania, jeden `ResourceDictionary` do innego.
