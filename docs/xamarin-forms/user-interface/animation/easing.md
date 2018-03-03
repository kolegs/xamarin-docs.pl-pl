---
title: Zwalnianie funkcji
description: "Platformy Xamarin.Forms zawiera klasę dynamiki, która pozwala na określenie funkcji przenoszenia czemu można kontrolować sposób przyspieszenia animacji lub spowolnić nich uruchomiony. W tym artykule przedstawiono, jak używać funkcji sterowania tempem zmian wstępnie zdefiniowane i sposób tworzenia niestandardowych funkcji sterowania tempem zmian."
ms.topic: article
ms.prod: xamarin
ms.assetid: E6F124C7-A161-4C1F-AF40-52F0935E54DE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/14/2016
ms.openlocfilehash: a57fd6e45d744d0e527c811649ce5299ebcd34d5
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="easing-functions"></a>Zwalnianie funkcji

_Platformy Xamarin.Forms zawiera klasę dynamiki, która pozwala na określenie funkcji przenoszenia czemu można kontrolować sposób przyspieszenia animacji lub spowolnić nich uruchomiony. W tym artykule przedstawiono, jak używać funkcji sterowania tempem zmian wstępnie zdefiniowane i sposób tworzenia niestandardowych funkcji sterowania tempem zmian._


[ `Easing` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Easing/) Klasa definiuje liczbę funkcji sterowania tempem zmian, które mogą być używane przez animacje:

- [ `BounceIn` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.BounceIn/) Wyjścia funkcji sterowania tempem odrzuceń animacji na początku.
- [ `BounceOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.BounceOut/) Wyjścia funkcji sterowania tempem odrzuceń animacji na końcu.
- [ `CubicIn` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.CubicIn/) Wyjścia funkcji sterowania tempem powoli przyspiesza animacji.
- [ `CubicInOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.CubicInOut/) Wyjścia funkcji sterowania tempem przyspieszają animacji na początku i zwalnia animacji na końcu.
- [ `CubicOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.CubicOut/) Wyjścia funkcji sterowania tempem szybko zwalnia animacji.
- [ `Linear` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.Linear/) Wyjścia funkcji sterowania tempem używa stałej szybkość pracy i jest domyślnie wyjścia funkcji sterowania tempem.
- [ `SinIn` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SinIn/) Wyjścia funkcji sterowania tempem sprawnie przyspiesza animacji.
- [ `SinInOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SinInOut/) Wyjścia funkcji sterowania tempem sprawnie przyspieszają animacji na początku i sprawnie mimo animacji na końcu.
- [ `SinOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SinOut/) Wyjścia funkcji sterowania tempem sprawnie mimo animacji.
- [ `SpringIn` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringIn/) Wyjścia funkcji sterowania tempem powoduje, że animacja w celu przyspieszenia bardzo szybko pod koniec.
- [ `SpringOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringOut/) Wyjścia funkcji sterowania tempem powoduje, że animacja się szybko zwalnia pod koniec.

`In` i `Out` sufiksy wskazująca, czy efekt podał funkcji sterowania tempem zmian jest widoczny na początku animacji, na końcu lub oba.

Ponadto można tworzyć niestandardowe funkcji sterowania tempem zmian. Aby uzyskać więcej informacji, zobacz [funkcje łatwiejszym niestandardowe](#customeasing).

## <a name="consuming-an-easing-function"></a>Korzystanie z funkcji sterowania tempem zmian

Metody rozszerzenia animacji w [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) klasy Zezwalaj funkcji sterowania tempem zmian, należy określić parametr metodę końcową, jak pokazano w poniższym przykładzie:

```csharp
await image.TranslateTo(0, 200, 2000, Easing.BounceIn);
await image.ScaleTo(2, 2000, Easing.CubicIn);
await image.RotateTo(360, 2000, Easing.SinInOut);
await image.ScaleTo(1, 2000, Easing.CubicOut);
await image.TranslateTo(0, -200, 2000, Easing.BounceOut);
```

Określając funkcji sterowania tempem zmian dla animacji prędkość animacji staje się z systemem innym niż liniowy i tworzy efekt podał funkcji sterowania tempem zmian. Pominięcie funkcji sterowania tempem zmian podczas tworzenia animacji powoduje animacji korzysta z domyślnego [ `Linear` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.Linear/) łatwiejszym funkcji, która tworzy prędkość liniową.

Aby uzyskać więcej informacji o korzystaniu z metody rozszerzenia animacji w [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) , zobacz [prostych animacji](~/xamarin-forms/user-interface/animation/simple.md). Ułatwianie funkcji również może być zużyte przez [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) klasy. Aby uzyskać więcej informacji, zobacz [animacji niestandardowej](~/xamarin-forms/user-interface/animation/custom.md).

<a name="customeasing" />

## <a name="custom-easing-functions"></a>Ułatwianie funkcje niestandardowe

Istnieją trzy główne metody do tworzenia niestandardowych funkcji sterowania tempem zmian:

1. Tworzenie metody, która przyjmuje `double` argument i zwraca `double` wynik.
1. Utwórz `Func<double, double>`.
1. Określ jako argument do funkcji sterowania tempem zmian [ `Easing` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Easing/) konstruktora.

We wszystkich trzech przypadkach niestandardowej funkcji sterowania tempem zmian powinna zwrócić 0 dla argumentu o wartości 0 i 1 dla argumentu 1. Jednak każda wartość może być zwracany między wartościami argumentu o wartości 0 i 1. Teraz, każde podejście będzie z kolei opisem.

### <a name="custom-easing-method"></a>Niestandardowe łatwiejszym — metoda

Niestandardowej funkcji sterowania tempem zmian można zdefiniować jako metodę, która przyjmuje `double` argument i zwraca `double` wynik, jak pokazano w poniższym przykładzie:

```csharp
await image.TranslateTo(0, 200, 2000, CustomEase);

double CustomEase (double t)
{
  return t == 0 || t == 1 ? t : (int)(5 * t) / 5.0;
}
```

`CustomEase` Metody obcina przychodzące wartość do wartości 0, 0,2 0,4, 0,6, 0,8 i 1. W związku z tym [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) wystąpienia jest translacja w odrębny przechodzi zamiast sprawnie.

### <a name="custom-easing-func"></a>Ułatwianie Func niestandardowe

Niestandardowej funkcji sterowania tempem zmian może także być zdefiniowany jako `Func<double, double>`, jak pokazano w poniższym przykładzie:

```csharp
Func<double, double> CustomEase = t => 9 * t * t * t - 13.5 * t * t + 5.5 * t;
await image.TranslateTo(0, 200, 2000, CustomEase));
```

`CustomEase` `Func` Reprezentuje funkcji sterowania tempem zmian, która rozpoczyna się poza szybkie, spowalnia odwraca kursu i odwraca kursu ponownie w celu przyspieszenia szybko pod koniec. W związku z tym podczas przemieszczania ogólną [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) wystąpienie jest w dół, również tymczasowo odwraca kursu w połowie animacji.

### <a name="custom-easing-constructor"></a>Niestandardowe łatwiejszym — Konstruktor

Niestandardowej funkcji sterowania tempem zmian może także być zdefiniowany jako argument [ `Easing` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Easing/) konstruktora, jak pokazano w poniższym przykładzie:

```csharp
await image.TranslateTo (0, 200, 2000, new Easing (t => 1 - Math.Cos (10 * Math.PI * t) * Math.Exp (-5 * t)));
```

Niestandardowej funkcji sterowania tempem zmian jest określony jako argument funkcji lambda [ `Easing` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Easing/) Konstruktor i używa `Math.Cos` metodę w celu utworzenia efekt powolne upuszczania, który jest tłumione przez `Math.Exp` metody. W związku z tym [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) wystąpienia jest translacja tak, aby wygląda na to, aby porzucić do jego nieaktywnych miejsce.

## <a name="summary"></a>Podsumowanie

Tym artykule przedstawiono, jak używać funkcji sterowania tempem zmian wstępnie zdefiniowane i sposób tworzenia niestandardowych funkcji sterowania tempem zmian. Obejmuje platformy Xamarin.Forms [ `Easing` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Easing/) klasy, która pozwala określić funkcji przenoszenia, która kontroluje sposób przyspieszenia animacji lub spowolnić nich uruchomiony.



## <a name="related-links"></a>Linki pokrewne

- [Asynchroniczna pomoc techniczna — omówienie](~/cross-platform/platform/async.md)
- [Funkcji sterowania tempem zmian (przykład)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/animation/easing/)
- [Ułatwianie](https://developer.xamarin.com/api/type/Xamarin.Forms.Easing/)
- [ViewExtensions](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/)
