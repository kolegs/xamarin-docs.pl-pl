---
title: Funkcje easingu w interfejsie Xamarin.Forms
description: Zestaw narzędzi Xamarin.Forms zawiera klasę dynamiki, która pozwala na określenie funkcji przenoszenia tej kontrolki jak przyspieszyć lub spowolnić, ponieważ są one uruchamiane w animacji. W tym artykule przedstawiono, jak używać wstępnie zdefiniowanych funkcji sterowania tempem zmian oraz sposób tworzenia niestandardowych funkcji sterowania tempem zmian.
ms.prod: xamarin
ms.assetid: E6F124C7-A161-4C1F-AF40-52F0935E54DE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/14/2016
ms.openlocfilehash: 1c75771173d94a18c7c1cc5100c64d45bdc32078
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998124"
---
# <a name="easing-functions-in-xamarinforms"></a>Funkcje easingu w interfejsie Xamarin.Forms

_Zestaw narzędzi Xamarin.Forms zawiera klasę dynamiki, która pozwala na określenie funkcji przenoszenia tej kontrolki jak przyspieszyć lub spowolnić, ponieważ są one uruchamiane w animacji. W tym artykule przedstawiono, jak używać wstępnie zdefiniowanych funkcji sterowania tempem zmian oraz sposób tworzenia niestandardowych funkcji sterowania tempem zmian._


[ `Easing` ](xref:Xamarin.Forms.Easing) Klasy definiuje kilka funkcji sterowania tempem zmian, które mogą być używane przez animacje:

- [ `BounceIn` ](xref:Xamarin.Forms.Easing.BounceIn) Funkcja sterowania tempem zmian odbija animacji na początku.
- [ `BounceOut` ](xref:Xamarin.Forms.Easing.BounceOut) Funkcja sterowania tempem zmian odbija animacji na końcu.
- [ `CubicIn` ](xref:Xamarin.Forms.Easing.CubicIn) Funkcja sterowania tempem zmian powoli przyspiesza animacji.
- [ `CubicInOut` ](xref:Xamarin.Forms.Easing.CubicInOut) Funkcja sterowania tempem zmian przyspiesza animacji na początku i zwalnia animacji na końcu.
- [ `CubicOut` ](xref:Xamarin.Forms.Easing.CubicOut) Funkcja sterowania tempem zmian szybko zwalnia animacji.
- [ `Linear` ](xref:Xamarin.Forms.Easing.Linear) Funkcja sterowania tempem zmian używa stałej szybkości pracy i jest domyślna funkcja sterowania tempem zmian.
- [ `SinIn` ](xref:Xamarin.Forms.Easing.SinIn) Funkcja sterowania tempem zmian płynnie przyspiesza animacji.
- [ `SinInOut` ](xref:Xamarin.Forms.Easing.SinInOut) Funkcja sterowania tempem zmian płynnie przyspiesza animacji na początku i sprawnie mimo animacji na końcu.
- [ `SinOut` ](xref:Xamarin.Forms.Easing.SinOut) Funkcja sterowania tempem zmian sprawnie mimo animacji.
- [ `SpringIn` ](xref:Xamarin.Forms.Easing.SpringIn) Funkcja sterowania tempem zmian powoduje, że animacja się bardzo szybko przyspieszyć pod koniec.
- [ `SpringOut` ](xref:Xamarin.Forms.Easing.SpringOut) Funkcja sterowania tempem zmian powoduje, że animacja szybko spowalniania pod koniec.

`In` i `Out` sufiksy wskazująca, czy efekt, dostarczone przez funkcję sterowania tempem zmian jest widoczne na początku animacji, na końcu lub obu.

Ponadto można tworzyć niestandardowych funkcji sterowania tempem zmian. Aby uzyskać więcej informacji, zobacz [niestandardowe funkcje Easingu](#customeasing).

## <a name="consuming-an-easing-function"></a>Korzystanie z funkcji sterowania tempem zmian

Metody rozszerzające animacji w [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions) klasy pozwalają funkcji sterowania tempem zmian, należy określić jako parametru metody końcowego, jak pokazano w poniższym przykładzie kodu:

```csharp
await image.TranslateTo(0, 200, 2000, Easing.BounceIn);
await image.ScaleTo(2, 2000, Easing.CubicIn);
await image.RotateTo(360, 2000, Easing.SinInOut);
await image.ScaleTo(1, 2000, Easing.CubicOut);
await image.TranslateTo(0, -200, 2000, Easing.BounceOut);
```

Określanie funkcji sterowania tempem zmian dla animacji, szybkość animacji staje się inny niż liniowy i daje efekt, dostarczone przez funkcję sterowania tempem zmian. Podczas tworzenia animacji z pominięciem funkcji sterowania tempem zmian powoduje, że animacji użyć domyślnej [ `Linear` ](xref:Xamarin.Forms.Easing.Linear) ułatwianie funkcji, która tworzy liniowej szybkość pracy.

Aby uzyskać więcej informacji o używaniu metody rozszerzenia animacji w [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions) klasy, zobacz [proste animacje](~/xamarin-forms/user-interface/animation/simple.md). Funkcje easingu również mogą być używane przez [ `Animation` ](xref:Xamarin.Forms.Animation) klasy. Aby uzyskać więcej informacji, zobacz [niestandardowe animacje](~/xamarin-forms/user-interface/animation/custom.md).

<a name="customeasing" />

## <a name="custom-easing-functions"></a>Niestandardowe funkcje Easingu

Istnieją trzy główne podejścia do tworzenia niestandardowych funkcji sterowania tempem zmian:

1. Utwórz metodę, która przyjmuje `double` argument i zwraca `double` wynik.
1. Utwórz `Func<double, double>`.
1. Określ funkcji sterowania tempem zmian jako argumenty [ `Easing` ](xref:Xamarin.Forms.Easing) konstruktora.

We wszystkich trzech przypadkach funkcji sterowania tempem zmian niestandardowej powinien zwrócić 0 dla argumentu 0 i 1 dla argumentu 1. Jednak każda wartość mogą być zwracane między wartości argumentu, 0 i 1. Teraz każde podejście będzie z kolei omówiono.

### <a name="custom-easing-method"></a>Niestandardowe ułatwianie — metoda

Niestandardowa funkcja sterowania tempem zmian można zdefiniować jako metody, która przyjmuje `double` argument i zwraca `double` wynik, jak pokazano w poniższym przykładzie kodu:

```csharp
await image.TranslateTo(0, 200, 2000, CustomEase);

double CustomEase (double t)
{
  return t == 0 || t == 1 ? t : (int)(5 * t) / 5.0;
}
```

`CustomEase` Metoda obcina przychodzących wartość do wartości 0, 0.2, 0,4, 0,6, 0,8 i 1. W związku z tym [ `Image` ](xref:Xamarin.Forms.Image) wystąpienia jest tłumaczony w przechodzi dyskretnych, a nie bez problemów.

### <a name="custom-easing-func"></a>Ułatwianie Func niestandardowe

Można również określić niestandardową funkcję sterowania tempem zmian jako `Func<double, double>`, jak pokazano w poniższym przykładzie kodu:

```csharp
Func<double, double> CustomEase = t => 9 * t * t * t - 13.5 * t * t + 5.5 * t;
await image.TranslateTo(0, 200, 2000, CustomEase));
```

`CustomEase` `Func` Reprezentuje funkcję sterowania tempem zmian, który rozpoczyna się szybką, spowalnia odwraca kurs i odwraca ponownie kurs szybkie działanie w końcowej. W związku z tym, podczas gdy ogólny przepływ [ `Image` ](xref:Xamarin.Forms.Image) wystąpienie jest w dół, również tymczasowo odwraca kurs w połowie animacji.

### <a name="custom-easing-constructor"></a>Ułatwianie Konstruktor niestandardowy

Można również określić niestandardową funkcję sterowania tempem zmian jako argument [ `Easing` ](xref:Xamarin.Forms.Easing) konstruktora, jak pokazano w poniższym przykładzie kodu:

```csharp
await image.TranslateTo (0, 200, 2000, new Easing (t => 1 - Math.Cos (10 * Math.PI * t) * Math.Exp (-5 * t)));
```

Niestandardowych funkcji sterowania tempem zmian jest określony jako argument funkcji lambda, aby [ `Easing` ](xref:Xamarin.Forms.Easing) Konstruktor i używa `Math.Cos` metodę w celu utworzenia wpływ upuszczania powolne, jaki jest wytłumione przez `Math.Exp` metody. W związku z tym [ `Image` ](xref:Xamarin.Forms.Image) wystąpienia jest tłumaczony wygląda tak, jakby pomijać jej ostatnim miejscu nieaktywnych.

## <a name="summary"></a>Podsumowanie

W tym artykule pokazano, jak używać wstępnie zdefiniowanych funkcji sterowania tempem zmian oraz sposób tworzenia niestandardowych funkcji sterowania tempem zmian. Obejmuje zestaw narzędzi Xamarin.Forms [ `Easing` ](xref:Xamarin.Forms.Easing) klasy, która pozwala na określenie funkcji przenoszenia, który kontroluje, jak przyspieszyć animacji lub spowolnić, ponieważ są one uruchamiane.



## <a name="related-links"></a>Linki pokrewne

- [Asynchroniczna pomoc techniczna — omówienie](~/cross-platform/platform/async.md)
- [Funkcje easingu (przykład)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/animation/easing/)
- [Ułatwianie](xref:Xamarin.Forms.Easing)
- [ViewExtensions](xref:Xamarin.Forms.ViewExtensions)
