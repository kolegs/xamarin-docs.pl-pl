---
title: Przestrzenie nazw XAML zestawu narzędzi Xamarin.Forms
description: XAML używa atrybutu XML xmlns dla deklaracji przestrzeni nazw. W tym artykule przedstawiono składnię przestrzeń nazw XAML i pokazuje, jak deklarować przestrzeń nazw XAML uzyskiwać dostęp do typu.
ms.prod: xamarin
ms.assetid: C03B5553-B199-4A19-9F0F-E5BCE1DB268F
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 06/18/2018
ms.openlocfilehash: 30cbb2c3aebdafe2ebf35598c520ae725e01ce65
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995147"
---
# <a name="xaml-namespaces-in-xamarinforms"></a>Przestrzenie nazw XAML zestawu narzędzi Xamarin.Forms

_XAML używa atrybutu XML xmlns dla deklaracji przestrzeni nazw. W tym artykule przedstawiono składnię przestrzeń nazw XAML i pokazuje, jak deklarować przestrzeń nazw XAML uzyskiwać dostęp do typu._

## <a name="overview"></a>Omówienie

Istnieją dwie deklaracje przestrzeni nazw XAML, które są zawsze wewnątrz elementu głównego pliku XAML. Pierwszy definiuje domyślny obszar nazw, jak pokazano w poniższym przykładzie kodu XAML:

```csharp
xmlns="http://xamarin.com/schemas/2014/forms"
```

Domyślny obszar nazw określa, że elementy zdefiniowane w pliku XAML z prefiksem nie odnoszą się do klasy zestawu narzędzi Xamarin.Forms, takich jak [ `ContentPage` ](xref:Xamarin.Forms.ContentPage).

Drugi deklarację przestrzeni nazw używa `x` prefiksu, jak pokazano w poniższym przykładzie kodu XAML:

```csharp
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
```

XAML używa prefiksów do deklarowania innych niż domyślne obszary nazw, z prefiksem używana przy odwoływaniu się do typów w przestrzeni nazw. `x` Deklarację przestrzeni nazw określa, że elementy zdefiniowane w ramach XAML z prefiksem `x` są używane dla elementów i atrybutów, które są nierozerwalnie związane z XAML (w szczególności specyfikacji XAML 2009).

W poniższej tabeli przedstawiono `x` przestrzeni nazw atrybutów obsługiwane przez zestaw narzędzi Xamarin.Forms:

|Konstrukcja|Opis|
|--- |--- |
|`x:Arguments`|Określa argumenty konstruktora dla innego niż domyślny konstruktor lub dla deklaracji obiektu metody fabryki.|
|`x:Class`|Określa nazwę przestrzeni nazw i klasy dla klasy zdefiniowanej w XAML. Nazwa klasy musi odpowiadać Nazwa klasy w pliku związanym z kodem. Pamiętaj, że ta konstrukcja może wystąpić tylko w elemencie głównym pliku XAML.|
|`x:FactoryMethod`|Określa metodę fabryki, który może służyć do zainicjowania obiektu.|
|`x:FieldModifier`|Określa poziom dostępu dla wygenerowanego pól nazwanych elementów XAML.|
|`x:Key`|Określa unikatowy klucz użytkownika dla każdego zasobu w `ResourceDictionary`. Wartość klucza jest używana do pobierania zasobów XAML i zwykle jest używana jako argument dla `StaticResource` — rozszerzenie znaczników.|
|`x:Name`|Określa nazwę obiektu środowiska uruchomieniowego dla elementu XAML. Ustawienie `x:Name` jest podobne do deklarowania zmiennej w kodzie.|
|`x:TypeArguments`|Określa argumenty typu generycznego do konstruktora typu ogólnego.|

Aby uzyskać więcej informacji na temat `x:FieldModifier` atrybutów, zobacz [modyfikatorów pól](~/xamarin-forms/xaml/field-modifiers.md). Aby uzyskać więcej informacji na temat `x:Arguments`, `x:FactoryMethod`, i `x:TypeArguments` atrybutów, zobacz [Passing Arguments w XAML](~/xamarin-forms/xaml/passing-arguments.md).

W XAML deklaracje przestrzeni nazw dziedziczą z elementu nadrzędnego do elementu podrzędnego. W związku z tym podczas definiowania przestrzeni nazw w elemencie głównym pliku XAML, wszystkie elementy w tym pliku dziedziczą deklarację przestrzeni nazw.

## <a name="declaring-namespaces-for-types"></a>Deklarowanie przestrzeni nazw typów

Typy mogą odwoływać się do XAML od zadeklarowania przestrzeni nazw XAML z prefiksem, za pomocą deklaracji przestrzeni nazw, określając nazwę przestrzeni nazw środowiska uruchomieniowego języka wspólnego (CLR) i opcjonalnie nazwy zestawu. Jest to osiągane przez zdefiniowanie wartości dla następujących słów kluczowych w obrębie deklaracji przestrzeni nazw:

- **przestrzeń nazw środowiska CLR:** lub **przy użyciu:** — przestrzeń nazw środowiska CLR zadeklarowane w obrębie zestawu, który zawiera typy do udostępnienia jako elementy XAML. This — słowo kluczowe jest wymagana.
- **zestaw =** — zestaw, który zawiera odwołania przestrzeń nazw środowiska CLR. Ta wartość jest nazwa zestawu, bez rozszerzenia pliku. Ścieżka do zestawu, należy ustanowić jako odwołania w pliku projektu zawierającego plik XAML, który będzie odwoływać się do zestawu. This — słowo kluczowe można pominąć, jeśli **przestrzeń nazw środowiska clr** wartość mieści się w tym samym zestawie, co kod aplikacji, który odwołuje się do typów.

Należy pamiętać, że znak, rozdzielając `clr-namespace` lub `using` token od jego wartość jest dwukropek, podczas gdy znak, rozdzielając `assembly` token od jego wartość jest znakiem równości. Znak do użycia między dwoma tokenami jest średnikami.

Poniższy kod ilustruje deklarację przestrzeni nazw XAML:

```xaml
<ContentPage ... xmlns:local="clr-namespace:HelloWorld" ...>
  ...
</ContentPage>
```

Alternatywnie to może być zapisana jako:

```xaml
<ContentPage ... xmlns:local="using:HelloWorld" ...>
  ...
</ContentPage>
```

`local` Prefiks jest Konwencji, używany do wskazania, że typy w przestrzeni nazw są lokalne dla aplikacji. Alternatywnie Jeśli typy znajdują się w innym zestawie, nazwa zestawu powinna można zdefiniować w deklaracji przestrzeni nazw, jak pokazano w poniższym przykładzie kodu XAML:

```xaml
<ContentPage ... xmlns:behaviors="clr-namespace:Behaviors;assembly=BehaviorsLibrary" ...>
  ...
</ContentPage>
```

Prefiks przestrzeni nazw jest następnie określone podczas deklarowania wystąpienie typu z zaimportowaną przestrzenią nazw, co pokazano w poniższym przykładzie kodu XAML:

```xaml
<ListView ...>
  <ListView.Behaviors>
    <behaviors:EventToCommandBehavior EventName="ItemSelected" ... />
  </ListView.Behaviors>
</ListView>
```

## <a name="summary"></a>Podsumowanie

W tym artykule wprowadzono składni przestrzeń nazw XAML i pokazano sposób deklarowania, przestrzeń nazw XAML uzyskiwać dostęp do typu. Używa XAML `xmlns` atrybutu XML dla deklaracji przestrzeni nazw i typów może być przywoływany w XAML od zadeklarowania przestrzeni nazw XAML z prefiksem.


## <a name="related-links"></a>Linki pokrewne

- [Przekazywanie argumentów w XAML](~/xamarin-forms/xaml/passing-arguments.md)
