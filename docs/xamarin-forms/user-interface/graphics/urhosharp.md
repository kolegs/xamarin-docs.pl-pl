---
title: Przy użyciu UrhoSharp w platformy Xamarin.Forms
description: UrhoSharp można dodać do aplikacji dla zaawansowanych wizualizacji grafiki 3D
ms.prod: xamarin
ms.assetid: 0646B98E-CC04-4537-9715-9F82338FD7FF
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/11/2016
ms.openlocfilehash: fbe07b81c8818378c3f6c12e09ae74bca2d89543
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/07/2018
ms.locfileid: "34847488"
---
# <a name="using-urhosharp-in-xamarinforms"></a>Przy użyciu UrhoSharp w platformy Xamarin.Forms

## <a name="what-is-urhosharp"></a>Co to jest UrhoSharp?

[UrhoSharp](~/graphics-games/urhosharp/index.md) jest zaawansowany aparat 3D dla deweloperów platformy Xamarin i .NET. [Wprowadzenie](~/graphics-games/urhosharp/introduction.md) więcej opisano informacje o bibliotece UrhoSharp i [te informacje](~/graphics-games/urhosharp/using.md) opisano, jak program sceny i akcje.

UrhoSharp może zostać użyty do renderowania grafiki w aplikacji platformy Xamarin.Forms.
To [próbki](https://github.com/xamarin/urho-samples/tree/master/FormsSample) pokazano, jak UrhoSharp może służyć do utworzenia interaktywnych wykresu 3W:

![](urhosharp-images/ios-animation.gif "UrhoSharp 3D interaktywny wykres w systemie iOS")
![](urhosharp-images/android-animation.gif "UrhoSharp 3D interaktywny wykres w systemie Android")

## <a name="adding-the-urhosharp-nuget-packages"></a>Dodawanie pakietów UrhoSharp Nuget

Przed użyciem UrhoSharp, deweloperzy muszą dodać pakiet UrhoSharp Nuget do ich rozwiązania. W tym przewodniku założono projekt platformy Xamarin.Forms z systemem iOS, Android i .NET Standard projektu biblioteki. Cały kod zostanie zapisany w .NET Standard projektu biblioteki; ale UrhoSharp Nuget musi zostać dodany do projektów dla systemu Android i iOS za.

Pakiet UrhoSharp.Forms Nuget zawiera wszystkie obiekty, potrzebne do utworzenia obiektów UrhoSharp. Pakiet nuget UrhoSharp.Forms zawiera `UrhoSurface` klasy, która jest używana do hosta UrhoSharp w platformy Xamarin.Forms.
Aby rozpocząć, kliknij prawym przyciskiem myszy **pakiety** folderu projektu biblioteki .NET Standard i wybierz **Dodawanie pakietów...** . Wprowadź wyszukiwany termin **UrhoSharp.Forms**, wybierz pozycję **UrhoSharp dla platformy Xamarin.Forms**, następnie kliknij przycisk **Dodaj pakiet**.

[![](urhosharp-images/add-package-sml.png "Okno dialogowe pakiety Dodawanie")](urhosharp-images/add-package.png#lightbox "pakietów okno dialogowe Dodawanie")

Pakiet UrhoSharp.Forms NuGet zostaną dodane do projektu:

![](urhosharp-images/packages.png "Folder pakietów")

Powtórz powyższe kroki dla projektów specyficzne dla platformy (na przykład iOS i Android).

## <a name="walkthrough-adding-urhosharp-to-a-xamarinforms-app"></a>Wskazówki: Dodawanie UrhoSharp do aplikacji platformy Xamarin.Forms

Te kroki opisują kodu w przykładowym UrhoSharp platformy Xamarin.Forms:

1. [Tworzenie strony formularzy Xamarin](#1)
2. [Dodaj UrhoSurface](#2)
3. [Tworzenie aplikacji Urho](#3)
4. [Dodaj klasę wykresy do UrhoSurface](#4)
5. [Interakcja z UrhoSharp](#5)

Zauważ, że próbki korzysta z funkcji języka C# 6 i nie może skompilować w starszych wersjach programu Visual Studio.

<a name="1"/>

### <a name="1-create-a-xamarin-forms-page"></a>1. Tworzenie strony formularzy Xamarin

Poniższy kod strony platformy Xamarin.Forms `UrhoPage` przed związanych z Urho kod został dodany:

```csharp
public class UrhoPage : ContentPage
{
  Slider selectedBarSlider, rotationSlider;

  public UrhoPage()
  {
    // we'll add Urho later

    rotationSlider = new Slider(0, 500, 250);

    selectedBarSlider = new Slider(0, 5, 2.5);

    Title = " UrhoSharp + Xamarin.Forms";
    Content = new StackLayout {
      Padding = new Thickness (12, 12, 12, 40),
      VerticalOptions = LayoutOptions.FillAndExpand,
      Children = {
        rotationSlider,
        new Label { Text = "SELECTED VALUE:" },
        selectedBarSlider,
      }
    };
  }
```

<a name="2"/>

### <a name="2-add-the-urhosurface"></a>2. Dodaj UrhoSurface

UrhoSharp może być hostowana w `ContentPage` , takich jak inne formanty platformy Xamarin.Forms.
Poniżej przedstawiono fragment kodu `UrhoSurface` dodany do strony platformy Xamarin.Forms:

```csharp
using Urho;
using Urho.Forms;
...
public class UrhoPage : ContentPage
{
  UrhoSurface urhoSurface;

  public UrhoPage()
  {
    urhoSurface = new UrhoSurface();
    urhoSurface.VerticalOptions = LayoutOptions.FillAndExpand;
...
    Content = new StackLayout {
    Padding = new Thickness (12, 12, 12, 40),
    VerticalOptions = LayoutOptions.FillAndExpand,
    Children = {
      urhoSurface,  // added
      new Label { Text = "ROTATION:" },
      rotationSlider,
      new Label { Text = "SELECTED VALUE:" },
      selectedBarSlider,
    }
  };
```

<a name="3"/>

### <a name="3-build-a-urho-application"></a>3. Tworzenie aplikacji Urho

Zapoznaj się `Charts` klasy dla implementacji grafiki 3D Urho używane w tym przykładzie. Konspekt podstawowy kod jest wyświetlany poniżej — należy pamiętać, że klasa implementuje `Urho.Application` który różni się od `Xamarin.Forms.Application` klasy, która jest zaimplementowana w **App.cs**.

```csharp
using Urho;
using Urho.Actions;
using Urho.Gui;
using Urho.Shapes;

namespace FormsSample
{
    public class Charts : Urho.Application
    {
    public Charts (ApplicationOptions options = null) : base(options) { }
    protected override void Start ()
    {
      ...
    }
    protected override void OnUpdate(float timeStep)
    {
      ...
    }
```

[Dokumentacji UrhoSharp](~/graphics-games/urhosharp/index.md) zawiera więcej informacji na temat tworzenia sceny 3W i akcji.

<a name="4"/>

### <a name="4-add-the-charts-class-to-the-urhosurface"></a>4. Dodaj klasę wykresy do UrhoSurface

Użyj `UrhoSurface.Show<T>` metody ogólnej, aby dodać aplikację Urho do strony platformy Xamarin.Forms. Poniższy fragment kodu zawiera kod dodatkowe wymagane do utworzenia `Charts` klasy:

```csharp
public class UrhoPage : ContentPage
{
  Charts urhoApp;
  ...
  protected override async void OnAppearing ()
  {
    urhoApp = await urhoSurface.Show<Charts> (new ApplicationOptions(assetsFolder: null)
      { Orientation = ApplicationOptions.OrientationType.Portrait });
  }
```

Uwaga: `Show<T>` metoda jest asynchroniczne i powinna zostać wywołana z `await` — słowo kluczowe.

<a name="5"/>

### <a name="5-interacting-with-urhosharp"></a>5. Interakcja z UrhoSharp

Przykład umożliwia paski wykresu i modyfikować. `Charts` Klasy ujawnia `Bars` i `SelectedBar` umożliwiające interakcji.

Każdy "pasek" ma obsługi zdarzeń zaznaczenia po `Charts` zrenderowaniu klasy przez Iterowanie po narażonych `Bars` kolekcji:

```csharp
protected override async void OnAppearing ()
{
  urhoApp = await urhoSurface.Show<Charts>(new ApplicationOptions(assetsFolder: null) { Orientation = ApplicationOptions.OrientationType.Portrait });
  foreach (var bar in urhoApp.Bars)
  {
    bar.Selected += OnBarSelection;
  }
}
```

Program obsługi zdarzeń używa wartości platformy Xamarin.Forms `Slider` formantu, aby dostosować wysokość danego paska:

```csharp
private void OnBarSelection(Bar bar)
{
  //reset value
  selectedBarSlider.ValueChanged -= OnValuesSliderValueChanged;
  selectedBarSlider.Value = bar.Value;
  selectedBarSlider.ValueChanged += OnValuesSliderValueChanged;
}

void OnValuesSliderValueChanged(object sender, ValueChangedEventArgs e)
{  // C# 6
  if (urhoApp?.SelectedBar != null)
  {
    urhoApp.SelectedBar.Value = (float)e.NewValue;
  }
}
```

Na koniec okablować się dwa `Slider` kontrolki, aby po zmianie wartości ma wpływ na kanwie UrhoSharp. Pierwszy suwaka obrót wykresu 3W i drugi suwak dopasowuje wysokość wybranego paska.

```csharp
rotationSlider = new Slider(0, 500, 250);
rotationSlider.ValueChanged += (s, e) => urhoApp?.Rotate((float)(e.NewValue - e.OldValue));

selectedBarSlider = new Slider(0, 5, 2.5);
selectedBarSlider.ValueChanged += OnValuesSliderValueChanged;
```

Animacje w [w górnej części strony](#) Pokaż uruchamianie przykładowych.

## <a name="summary"></a>Podsumowanie

Ta strona przedstawia, jak UrhoSharp może służyć do dodania wizualizacji danych 3D do platformy Xamarin.Forms. Odczyt [dokumentacji UrhoSharp](~/graphics-games/urhosharp/index.md) Aby uzyskać więcej informacji na temat tworzenia sceny Urho, które mogą być zawarte w aplikacji platformy Xamarin.Forms przy użyciu metody przedstawionych powyżej.


## <a name="related-links"></a>Linki pokrewne

- [Wykresy przykładowy (C# 6)](https://github.com/xamarin/urho-samples/tree/master/FormsSample)
- [UrhoSharp](~/graphics-games/urhosharp/index.md)
