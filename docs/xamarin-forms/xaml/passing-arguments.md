---
title: Przekazywanie argumentów w XAML
description: W tym artykule przedstawiono, przy użyciu atrybutów XAML, które mogą służyć do przekazywania argumentów do innych niż domyślne konstruktory, wywoływanie metod fabryki i określić typ argumentu ogólnego.
ms.prod: xamarin
ms.assetid: 8F3B267F-499E-4D79-9193-FCA99F199519
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 10/25/2016
ms.openlocfilehash: 51b72d9143895543715c519a65cf8c82aa4d12f7
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996551"
---
# <a name="passing-arguments-in-xaml"></a>Przekazywanie argumentów w XAML

_W tym artykule przedstawiono, przy użyciu atrybutów XAML, które mogą służyć do przekazywania argumentów do innych niż domyślne konstruktory, wywoływanie metod fabryki i określić typ argumentu ogólnego._

## <a name="overview"></a>Omówienie

Często jest to niezbędne do tworzenia wystąpień obiektów z konstruktorów, które wymagają argumentów lub przez wywołanie metody statyczne tworzenie. Można to osiągnąć w XAML przy użyciu `x:Arguments` i `x:FactoryMethod` atrybuty:

- `x:Arguments` Atrybut jest używany do określania argumentów konstruktora dla innego niż domyślny konstruktor lub dla deklaracji obiektu metody fabryki. Aby uzyskać więcej informacji, zobacz [przekazywania argumentów konstruktora](#constructor_arguments).
- `x:FactoryMethod` Atrybut jest używany do określenia metoda fabryki, który może służyć do zainicjowania obiektu. Aby uzyskać więcej informacji, zobacz [wywoływania metod fabryki](#factory_methods).

Ponadto `x:TypeArguments` atrybut może służyć do określania argumenty typu generycznego do konstruktora typu ogólnego. Aby uzyskać więcej informacji, zobacz [określenia argumentu typu ogólnego](#generic_type_arguments).

<a name="constructor_arguments" />

## <a name="passing-constructor-arguments"></a>Przekazywanie argumentów konstruktora

Argumenty można przekazać za pomocą innego niż domyślny konstruktor `x:Arguments` atrybutu. Każdy argument konstruktora muszą być rozdzielane w obrębie elementu XML, który reprezentuje typ argumentu. Zestaw narzędzi Xamarin.Forms obsługuje następujące elementy dla typów podstawowych:

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

Poniższy przykład kodu demonstruje sposób użycia `x:Arguments` atrybutu z trzema [ `Color` ](xref:Xamarin.Forms.Color) konstruktorów:

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

Liczba elementów w obrębie `x:Arguments` tag i typy tych elementów musi odpowiadać jednej z [ `Color` ](xref:Xamarin.Forms.Color) konstruktorów. `Color` [Konstruktor](xref:Xamarin.Forms.Color.%23ctor(System.Double)) z pojedynczym parametrem wymaga wartości skali szarości z zakresu od 0 (czarny) do 1 (biały). `Color` [Konstruktor](xref:Xamarin.Forms.Color.%23ctor(System.Double,System.Double,System.Double)) z trzema parametrami wymaga czerwonego, zielonego i niebieskiego wartości od 0 do 1. `Color` [Konstruktor](xref:Xamarin.Forms.Color.%23ctor(System.Double,System.Double,System.Double,System.Double)) za pomocą czterech parametrów dodaje kanał alfa jako czwarty parametr.

Poniższych zrzutach ekranu przedstawiono wynik każdego wywołania [ `Color` ](xref:Xamarin.Forms.Color) konstruktora o wartości określonego argumentu:

![](passing-arguments-images/passing-arguments.png "BoxView.Color określony za pomocą x: Arguments")

<a name="factory_methods" />

## <a name="calling-factory-methods"></a>Wywoływanie metod fabryki

Fabryka metody można wywołać w XAML, określając metody przy użyciu `x:FactoryMethod` atrybutu i jego argumentów, za pomocą `x:Arguments` atrybutu. Metoda fabryki jest `public static` metodę, która zwraca obiekty lub wartości tego samego typu jak klasa lub struktura, która definiuje metody.

[ `Color` ](xref:Xamarin.Forms.Color) Struktury definiuje szereg metod fabryki i poniższy przykład kodu demonstruje wywoływania trzy z nich:

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

Liczba elementów w obrębie `x:Arguments` tagu i typy te elementy muszą być zgodne argumenty wywołania metody fabryki. [ `FromRgba` ](xref:Xamarin.Forms.Color.FromRgba(System.Int32,System.Int32,System.Int32,System.Int32)) Metoda fabryki wymaga czterech [ `Int32` ](https://docs.microsoft.com/dotnet/api/system.int32) parametry, które reprezentują wartości czerwony, zielony, niebieski i alfa, od 0 do 255 odpowiednio. [ `FromHsla` ](xref:Xamarin.Forms.Color.FromHsla(System.Double,System.Double,System.Double,System.Double)) Metoda fabryki wymaga czterech [ `Double` ](https://docs.microsoft.com/dotnet/api/system.double) parametry, które reprezentują odcień, nasycenie, jasność i wartości alfa, od 0 do 1 odpowiednio. [ `FromHex` ](xref:Xamarin.Forms.Color.FromHex(System.String)) Metoda fabryki wymaga [ `String` ](https://docs.microsoft.com/dotnet/api/system.string) reprezentujący szesnastkowego ("") kolor RGB.

Poniższych zrzutach ekranu przedstawiono wynik każdego wywołania [ `Color` ](xref:Xamarin.Forms.Color) metoda fabryki wartościami określony argument:

![](passing-arguments-images/factory-methods.png "BoxView.Color określony za pomocą x: FactoryMethod i x: Arguments")

<a name="generic_type_arguments" />

## <a name="specifying-a-generic-type-argument"></a>Określanie Argument typu ogólnego

Można określić argumenty typu generycznego dla konstruktora typu ogólnego przy użyciu `x:TypeArguments` atrybutu, jak pokazano w poniższym przykładzie kodu:

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

[ `OnPlatform` ](xref:Xamarin.Forms.OnPlatform`1) Klasa jest klasą rodzajową i musi być utworzone przy użyciu `x:TypeArguments` atrybut, który jest zgodny z typem docelowym. W [ `On` ](xref:Xamarin.Forms.On) klasy [ `Platform` ](xref:Xamarin.Forms.On.Platform) atrybut może zaakceptować pojedynczy `string` wartość lub wielu rozdzielonych przecinkami `string` wartości. W tym przykładzie [ `StackLayout.Margin` ](xref:Xamarin.Forms.View.Margin) właściwość jest ustawiona na określonych platform [ `Thickness` ](xref:Xamarin.Forms.Thickness).

## <a name="summary"></a>Podsumowanie

W tym artykule pokazano, przy użyciu atrybutów XAML, które mogą służyć do przekazywania argumentów do innych niż domyślne konstruktory, wywoływanie metod fabryki i określić typ argumentu ogólnego.


## <a name="related-links"></a>Linki pokrewne

- [Przestrzeń nazw XAML](~/xamarin-forms/xaml/namespaces.md)
- [Przekazywanie argumentów konstruktora (przykład)](https://developer.xamarin.com/samples/xamarin-forms/xaml/passingconstructorarguments/)
- [Wywoływanie metod fabryki (przykład)](https://developer.xamarin.com/samples/xamarin-forms/xaml/callingfactorymethods/)
