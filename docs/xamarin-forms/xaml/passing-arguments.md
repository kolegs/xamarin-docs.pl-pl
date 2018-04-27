---
title: Przekazywanie argumentów w języku XAML
description: W tym artykule przedstawiono przy użyciu atrybutów XAML, które mogą służyć do przekazywania argumentów innych niż domyślne konstruktorów, wywoływanie metod fabryki i określić typ argument rodzajowy.
ms.prod: xamarin
ms.assetid: 8F3B267F-499E-4D79-9193-FCA99F199519
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 10/25/2016
ms.openlocfilehash: f0b7a83a16a13d38fc9e59456e864084b439b71f
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2018
---
# <a name="passing-arguments-in-xaml"></a>Przekazywanie argumentów w języku XAML

_W tym artykule przedstawiono przy użyciu atrybutów XAML, które mogą służyć do przekazywania argumentów innych niż domyślne konstruktorów, wywoływanie metod fabryki i określić typ argument rodzajowy._

## <a name="overview"></a>Omówienie

Często jest niezbędne do tworzenia wystąpień obiektów z konstruktorów, które wymagają argumentów lub przez wywołanie metody statycznej tworzenia. Można to osiągnąć w języku XAML za pomocą `x:Arguments` i `x:FactoryMethod` atrybuty:

- `x:Arguments` Atrybut służy do określania argumentów konstruktora z systemem innym niż domyślny konstruktor lub deklaracji obiektu metody fabryki. Aby uzyskać więcej informacji, zobacz [przekazywanie argumentów konstruktora](#constructor_arguments).
- `x:FactoryMethod` Atrybut służy do określania metody fabryki, która może służyć do zainicjowania obiektu. Aby uzyskać więcej informacji, zobacz [podczas wywoływania metody fabryki](#factory_methods).

Ponadto `x:TypeArguments` atrybut może służyć do określania argumentów typu ogólnego do konstruktora typu ogólnego. Aby uzyskać więcej informacji, zobacz [określenie ogólnego argumentu typu](#generic_type_arguments).

<a name="constructor_arguments" />

## <a name="passing-constructor-arguments"></a>Przekazywanie argumentów konstruktora

Argumenty mogą zostać przekazane do za pomocą konstruktora domyślnego z systemem innym niż `x:Arguments` atrybutu. Każdy argument konstruktora muszą być rozdzielane w obrębie elementu XML, który reprezentuje typ argumentu. Podstawowe typy platformy Xamarin.Forms obsługuje następujące elementy:

- `x:Object`
- `x:Boolean`
- `x:Byte`
- `x:Int16`
- `x:Int32`
- `x:Int64`
- `x:Single`
- `x:Double`
- `x:Decimal`
- `x:Char`
- `x:String`
- `x:TimeSpan`
- `x:Array`
- `x:DateTime`

Poniższy przykład kodu pokazuje, za pomocą `x:Arguments` atrybutu z trzema [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) konstruktory:

```xaml
<BoxView HeightRequest="150" WidthRequest="150" HorizontalOptions="Center">
  <BoxView.Color>
    <Color>
      <x:Arguments>
        <x:Double>0.9</x:Double>
      </x:Arguments>
    </Color>
  </BoxView.Color>
</BoxView>
<BoxView HeightRequest="150" WidthRequest="150" HorizontalOptions="Center">
  <BoxView.Color>
    <Color>
      <x:Arguments>
        <x:Double>0.25</x:Double>
        <x:Double>0.5</x:Double>
        <x:Double>0.75</x:Double>
      </x:Arguments>
    </Color>
  </BoxView.Color>
</BoxView>
<BoxView HeightRequest="150" WidthRequest="150" HorizontalOptions="Center">
  <BoxView.Color>
    <Color>
      <x:Arguments>
        <x:Double>0.8</x:Double>
        <x:Double>0.5</x:Double>
        <x:Double>0.2</x:Double>
        <x:Double>0.5</x:Double>
      </x:Arguments>
    </Color>
  </BoxView.Color>
</BoxView>
```

Liczba elementów w obrębie `x:Arguments` tagu i typy tych elementów musi pasować [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) konstruktorów. `Color` [Konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Color.Color/p/System.Double/) z pojedynczym parametrem wymaga wartości skali szarości z zakresu od 0 (czarny) do 1 (biały). `Color` [Konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Color.Color/p/System.Double/System.Double/System.Double/) z trzema parametrami wymaga czerwony, zielonemu i niebieskiemu wartości od 0 do 1. `Color` [Konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Color.Color/p/System.Double/System.Double/System.Double/System.Double/) z cztery parametry dodaje kanału alfa jako czwartego parametru.

Poniższe zrzuty ekranu wskazuje wynik każdego wywołania [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) konstruktora o wartości określony argument:

![](passing-arguments-images/passing-arguments.png "BoxView.Color określony za pomocą x: Arguments")

<a name="factory_methods" />

## <a name="calling-factory-methods"></a>Wywoływanie metody fabryki

Można wywołać metody fabryki w języku XAML, określając metody nazwy przy użyciu `x:FactoryMethod` atrybut i argumenty przy użyciu `x:Arguments` atrybutu. Metoda fabryki jest `public static` metodę zwracającą obiektów lub wartości tego samego typu co klasy lub struktury, która definiuje metody.

[ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) Struktury definiuje wiele metod fabryki i poniższy przykład kodu pokazuje wywołanie trzy z nich:

```xaml
<BoxView HeightRequest="150" WidthRequest="150" HorizontalOptions="Center">
  <BoxView.Color>
    <Color x:FactoryMethod="FromRgba">
      <x:Arguments>
        <x:Int32>192</x:Int32>
        <x:Int32>75</x:Int32>
        <x:Int32>150</x:Int32>                      
        <x:Int32>128</x:Int32>
      </x:Arguments>
    </Color>
  </BoxView.Color>
</BoxView>
<BoxView HeightRequest="150" WidthRequest="150" HorizontalOptions="Center">
  <BoxView.Color>
    <Color x:FactoryMethod="FromHsla">
      <x:Arguments>
        <x:Double>0.23</x:Double>
        <x:Double>0.42</x:Double>
        <x:Double>0.69</x:Double>
        <x:Double>0.7</x:Double>
      </x:Arguments>
    </Color>
  </BoxView.Color>
</BoxView>
<BoxView HeightRequest="150" WidthRequest="150" HorizontalOptions="Center">
  <BoxView.Color>
    <Color x:FactoryMethod="FromHex">
      <x:Arguments>
        <x:String>#FF048B9A</x:String>
      </x:Arguments>
    </Color>
  </BoxView.Color>
</BoxView>
```

Liczba elementów w obrębie `x:Arguments` tagu i typy te elementy muszą być zgodne argumenty wywołania metody fabryki. [ `FromRgba` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromRgba/p/System.Int32/System.Int32/System.Int32/System.Int32/) Metoda fabryki wymaga cztery [ `Int32` ](https://docs.microsoft.com/dotnet/api/system.int32) parametry, które reprezentują czerwony, zielony, niebieski i alfa wartości od 0 do 255 odpowiednio. [ `FromHsla` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromHsla/p/System.Double/System.Double/System.Double/System.Double/) Metoda fabryki wymaga cztery [ `Double` ](https://docs.microsoft.com/dotnet/api/system.double) parametry, które reprezentują odcień, nasycenie, jasność i wartości alfa od 0 do 1 odpowiednio. [ `FromHex` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromHex/p/System.String/) Wymaga metoda fabryki [ `String` ](https://docs.microsoft.com/dotnet/api/system.string) reprezentujący szesnastkowym kolor RGB, ().

Poniższe zrzuty ekranu wskazuje wynik każdego wywołania [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) metoda fabryki wartościami określony argument:

![](passing-arguments-images/factory-methods.png "BoxView.Color określony z x: factorymethod — i x: Arguments")

<a name="generic_type_arguments" />

## <a name="specifying-a-generic-type-argument"></a>Określenie argumentu typu ogólnego

Można określić argumenty typu ogólnego dla konstruktora typu ogólnego, używając `x:TypeArguments` atrybutu, jak pokazano w poniższym przykładzie:

```xaml
<ContentPage ...>
  <StackLayout>
    <StackLayout.Margin>
      <OnPlatform x:TypeArguments="Thickness">
        <On Platform="iOS" Value="0,20,0,0" />
        <On Platform="Android" Value="5, 10" />
        <On Platform="UWP" Value="10" />
      </OnPlatform>
    </StackLayout.Margin>
  </StackLayout>
</ContentPage>
```

[ `OnPlatform` ](https://developer.xamarin.com/api/type/Xamarin.Forms.OnPlatform%3CT%3E/) Klasa jest klasą szablonową i musi być utworzone z `x:TypeArguments` atrybut, który jest zgodny z typem docelowym. W [ `On` ](https://developer.xamarin.com/api/type/Xamarin.Forms.On/) klasy [ `Platform` ](https://developer.xamarin.com/api/property/Xamarin.Forms.On.Platform/) atrybut może przyjąć pojedynczy `string` wartość lub wielu rozdzielana przecinkami `string` wartości. W tym przykładzie [ `StackLayout.Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) właściwość jest ustawiona na poszczególnych platform [ `Thickness` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/).

## <a name="summary"></a>Podsumowanie

W tym artykule przedstawiono przy użyciu atrybutów XAML, które mogą służyć do przekazywania argumentów innych niż domyślne konstruktorów, wywoływanie metod fabryki i określić typ argument rodzajowy.


## <a name="related-links"></a>Linki pokrewne

- [Przestrzeń nazw XAML](~/xamarin-forms/xaml/namespaces.md)
- [Przekazywanie argumentów konstruktora (przykład)](https://developer.xamarin.com/samples/xamarin-forms/xaml/passingconstructorarguments/)
- [Wywoływanie metody fabryki (przykład)](https://developer.xamarin.com/samples/xamarin-forms/xaml/callingfactorymethods/)
