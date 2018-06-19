---
title: Przestrzeń nazw XAML w platformy Xamarin.Forms
description: XAML używa atrybutu XML xmlns dla deklaracji przestrzeni nazw. W tym artykule przedstawiono składnię przestrzeni nazw XAML i pokazuje, jak można zadeklarować przestrzeni nazw XAML uzyskiwać dostęp do typu.
ms.prod: xamarin
ms.assetid: C03B5553-B199-4A19-9F0F-E5BCE1DB268F
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 06/18/2018
ms.openlocfilehash: 25299bc3b56c2fbb748db202e43e75be183cce66
ms.sourcegitcommit: 7a89735aed9ddf89c855fd33928915d72da40c2d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36209300"
---
# <a name="xaml-namespaces-in-xamarinforms"></a>Przestrzeń nazw XAML w platformy Xamarin.Forms

_XAML używa atrybutu XML xmlns dla deklaracji przestrzeni nazw. W tym artykule przedstawiono składnię przestrzeni nazw XAML i pokazuje, jak można zadeklarować przestrzeni nazw XAML uzyskiwać dostęp do typu._

## <a name="overview"></a>Omówienie

Istnieją dwa deklaracje przestrzeni nazw XAML, które są zawsze w elemencie głównym pliku XAML. Pierwszy określa domyślną przestrzeń nazw, jak pokazano w poniższym przykładzie kodu XAML:

```csharp
xmlns="http://xamarin.com/schemas/2014/forms"
```

Określa domyślną przestrzeń nazw, że elementy zdefiniowane w pliku XAML z prefiksem nie odwołują się do klasy platformy Xamarin.Forms, takich jak [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/).

Druga deklaracja przestrzeni nazw używa `x` prefiksu, jak pokazano w poniższym przykładzie kodu XAML:

```csharp
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
```

XAML używa prefiksy, aby zadeklarować przestrzeni nazw z systemem innym niż domyślny, z prefiksem używany podczas odwoływania się do typów w przestrzeni nazw. `x` Deklaracji przestrzeni nazw określa, że elementy zdefiniowane w pliku XAML z prefiksem `x` są używane dla elementów i atrybutów, które są wewnętrzne w języku XAML (w szczególności specyfikację XAML 2009).

W poniższej tabeli przedstawiono `x` obsługiwana przez platformy Xamarin.Forms atrybuty przestrzeni nazw:

|Konstrukcja|Opis|
|--- |--- |
|`x:Arguments`|Określa argumenty konstruktora z systemem innym niż domyślny konstruktor lub deklaracji obiektu metody fabryki.|
|`x:Class`|Określa nazwę przestrzeni nazw i klasy dla klasy zdefiniowany w języku XAML. Nazwa klasy musi odpowiadać nazwie klasy pliku CodeBehind. Należy pamiętać, że ta konstrukcja może występować tylko w elemencie głównym pliku XAML.|
|`x:FactoryMethod`|Określa metodę fabryka, która może służyć do zainicjowania obiektu.|
|`x:FieldModifier`|Określa poziom dostępu dla wygenerowanego pól nazwanych elementów XAML.|
|`x:Key`|Określa unikatowy klucz użytkownika dla każdego zasobu w `ResourceDictionary`. Wartość klucza służy do pobierania zasobu XAML i zazwyczaj jest używane jako argument dla `StaticResource` — rozszerzenie znaczników.|
|`x:Name`|Określa nazwę obiektu środowiska wykonawczego dla elementu XAML. Ustawienie `x:Name` jest podobny do deklarowania zmiennych w kodzie.|
|`x:TypeArguments`|Określa argumenty typu ogólnego do konstruktora typu ogólnego.|

Aby uzyskać więcej informacji na temat `x:FieldModifier` atrybutów, zobacz [modyfikatorów pól](~/xamarin-forms/xaml/field-modifiers.md). Aby uzyskać więcej informacji na temat `x:Arguments`, `x:FactoryMethod`, i `x:TypeArguments` atrybutów, zobacz [przekazywanie argumentów w języku XAML](~/xamarin-forms/xaml/passing-arguments.md).

W języku XAML deklaracje przestrzeni nazw dziedziczyć z elementu nadrzędnego do elementu podrzędnego. W związku z tym podczas definiowania przestrzeni nazw w elemencie głównym pliku XAML, wszystkie elementy w tym pliku dziedziczą deklaracji przestrzeni nazw.

## <a name="declaring-namespaces-for-types"></a>Deklarowanie przestrzeni nazw dla typów

Typy może być przywoływany w XAML od zadeklarowania przestrzeni nazw XAML z prefiksem z deklaracji przestrzeni nazw, określając nazwę przestrzeni nazw środowiska uruchomieniowego języka wspólnego (CLR) i opcjonalnie nazwy zestawu. Jest to osiągane przez określenie wartości dla następujących słów kluczowych w ramach deklaracji przestrzeni nazw:

- **CLR-namespace:** lub **przy użyciu:** — przestrzeń nazw środowiska CLR zadeklarowane w obrębie zestawu, który zawiera typy do udostępnienia jako elementy XAML. To słowo kluczowe jest wymagana.
- **zestaw =** — zestaw zawierający przywoływanego przestrzeń nazw środowiska CLR. Ta wartość jest nazwą zestawu bez rozszerzenia. Ścieżka do zestawu powinny zostać określone jako odwołanie w pliku projektu, który zawiera plik XAML, który będzie odwoływać się do zestawu. To słowo kluczowe można pominąć, jeśli **clr-namespace** wartość mieści się w tym samym zestawie co kod aplikacji, która odwołuje się do typów.

Należy pamiętać, że znak oddzielanie `clr-namespace` lub `using` tokenu z jego wartość jest dwukropek, podczas gdy oddzielanie znak `assembly` tokenu z jego wartość jest znak równości. Znak między dwoma tokenów jest średnikiem.

Poniższy przykład kodu pokazuje deklaracji przestrzeni nazw XAML:

```xaml
<ContentPage ... xmlns:local="clr-namespace:HelloWorld" ...>
  ...
</ContentPage>
```

Alternatywnie to może być zapisany jako:

```xaml
<ContentPage ... xmlns:local="using:HelloWorld" ...>
  ...
</ContentPage>
```

`local` Prefiks jest Konwencji służy do wskazania, że typy w przestrzeni nazw są lokalne dla aplikacji. Alternatywnie Jeśli typy znajdują się w innym zestawie, nazwa zestawu powinien można zdefiniować w deklaracji przestrzeni nazw jak pokazano w poniższym przykładzie kodu XAML:

```xaml
<ContentPage ... xmlns:behaviors="clr-namespace:Behaviors;assembly=BehaviorsLibrary" ...>
  ...
</ContentPage>
```

Prefiks przestrzeni nazw jest następnie określony podczas deklarowania wystąpienia typu z zaimportowaną przestrzenią nazw, jak przedstawiono w poniższym przykładzie kodu XAML:

```xaml
<ListView ...>
  <ListView.Behaviors>
    <behaviors:EventToCommandBehavior EventName="ItemSelected" ... />
  </ListView.Behaviors>
</ListView>
```

## <a name="summary"></a>Podsumowanie

W tym artykule wprowadzono składni przestrzeni nazw XAML i przedstawiono sposób zadeklarować przestrzeni nazw XAML uzyskiwać dostęp do typu. Używa XAML `xmlns` atrybutu XML dla deklaracji przestrzeni nazw i typów może być przywoływany w XAML od zadeklarowania przestrzeni nazw XAML z prefiksem.


## <a name="related-links"></a>Linki pokrewne

- [Przekazywanie argumentów w języku XAML](~/xamarin-forms/xaml/passing-arguments.md)
