---
title: Dodawanie rozpoznawania gestów przesuwanie
description: Gest przesuwanie służy do wykrywania, przeciągając i jest realizowana za pomocą klasy PanGestureRecognizer. Typowy scenariusz dla gestu przesuwanie jest poziomie i w pionie obrazu, przeciągnij, aby całej zawartości obrazu można wyświetlać, gdy jest ona wyświetlana w mniejszych niż wymiary obrazu okienko ekranu. Odbywa się przez przeniesienie obrazu wewnątrz okienka ekranu, a przedstawionej w tym artykule.
ms.prod: xamarin
ms.assetid: 42CBD2CF-432D-4F19-A05E-D569BB7F8713
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/21/2016
ms.openlocfilehash: ed38f7ace9e11b009aae768cda2d4af0f36c337e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="adding-a-pan-gesture-recognizer"></a>Dodawanie rozpoznawania gestów przesuwanie

_Gest przesuwanie służy do wykrywania, przeciągając i jest realizowana za pomocą klasy PanGestureRecognizer. Typowy scenariusz dla gestu przesuwanie jest poziomie i w pionie obrazu, przeciągnij, aby całej zawartości obrazu można wyświetlać, gdy jest ona wyświetlana w mniejszych niż wymiary obrazu okienko ekranu. Odbywa się przez przeniesienie obrazu wewnątrz okienka ekranu, a przedstawionej w tym artykule._

## <a name="overview"></a>Omówienie

Aby wprowadzić możliwością przeciągania z gestów Przesuwanie elementu interfejsu użytkownika, należy utworzyć [ `PanGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PanGestureRecognizer/) wystąpienia i obsługiwać [ `PanUpdated` ](https://developer.xamarin.com/api/event/Xamarin.Forms.PanGestureRecognizer.PanUpdated/) zdarzeń, i Dodaj nowy aparat rozpoznawania gestów do [ `GestureRecognizers` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.GestureRecognizers/) kolekcji dla elementu interfejsu użytkownika. Poniższy kod przedstawia przykład `PanGestureRecognizer` dołączony do [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) elementu:

```csharp
var panGesture = new PanGestureRecognizer();
panGesture.PanUpdated += (s, e) => {
  // Handle the pan
};
image.GestureRecognizers.Add(panGesture);
```

Ponadto można to osiągnąć w języku XAML, jak pokazano w poniższym przykładzie:

```xaml
<Image Source="MonoMonkey.jpg">
  <Image.GestureRecognizers>
    <PanGestureRecognizer PanUpdated="OnPanUpdated" />
  </Image.GestureRecognizers>
</Image>
```

Kod `OnPanUpdated` program obsługi zdarzeń jest następnie dodawana do pliku CodeBehind:

```csharp
void OnPanUpdated (object sender, PanUpdatedEventArgs e)
{
  // Handle the pan
}
```

> [!NOTE]
> Poprawne przesuwanie w systemie Android wymaga [pakietu NuGet 2.1.0-pre1 platformy Xamarin.Forms](https://www.nuget.org/packages/Xamarin.Forms/2.1.0.6501-pre1) co najmniej.

## <a name="creating-a-pan-container"></a>Tworzenie kontenera przesuwanie

Ta sekcja zawiera uogólniony Klasa pomocy, która wykonuje dowolne przesuwanie zwykle dopasowany do poruszanie się w obrębie obrazy lub mapy. Obsługa gestu przesuwanie do wykonania operacji przeciągania wymaga niektórych matematyczne do przekształcania interfejsu użytkownika. Matematyczne ten jest używany do przeciągnij tylko w obrębie granic elementu interfejsu użytkownika opakowana. Poniższy kod przedstawia przykład `PanContainer` klasy:

```csharp
public class PanContainer : ContentView
{
  double x, y;

  public PanContainer ()
  {
    // Set PanGestureRecognizer.TouchPoints to control the
    // number of touch points needed to pan
    var panGesture = new PanGestureRecognizer ();
    panGesture.PanUpdated += OnPanUpdated;
    GestureRecognizers.Add (panGesture);
  }

  void OnPanUpdated (object sender, PanUpdatedEventArgs e)
  {
    ...
  }
}
```

Ta klasa może otaczający element interfejsu użytkownika tak, aby gestu przesuwanie będzie przeciągnij element interfejsu użytkownika opakowana. Przedstawia poniższy przykładowy kod XAML `PanContainer` zawijanie [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) elementu:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:PanGesture"
             x:Class="PanGesture.HomePage">
    <ContentPage.Content>
        <AbsoluteLayout>
            <local:PanContainer>
                <Image Source="MonoMonkey.jpg" WidthRequest="1024" HeightRequest="768" />
            </local:PanContainer>
        </AbsoluteLayout>
    </ContentPage.Content>
</ContentPage>
```

Poniższy kod przedstawia przykład sposobu `PanContainer` opakowuje [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) elementu na stronie C#:

```csharp
public class HomePageCS : ContentPage
{
  public HomePageCS ()
  {
    Content = new AbsoluteLayout {
      Padding = new Thickness (20),
      Children = {
        new PanContainer {
          Content = new Image {
            Source = ImageSource.FromFile ("MonoMonkey.jpg"),
            WidthRequest = 1024,
            HeightRequest = 768
          }
        }
      }
    };
  }
}
```

W obu przykładach [ `WidthRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.WidthRequest/) i [ `HeightRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.HeightRequest/) właściwości są ustawione na wartości szerokości i wysokości wyświetlania obrazu.

Gdy [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) element odbiera gestu przesuwanie, wyświetlany obraz zostanie przeciągnięty. Przeciąganie odbywa się za pośrednictwem `PanContainer.OnPanUpdated` metodę, która jest wyświetlana w poniższym przykładzie:

```csharp
void OnPanUpdated (object sender, PanUpdatedEventArgs e)
{
  switch (e.StatusType) {
  case GestureStatus.Running:
    // Translate and ensure we don't pan beyond the wrapped user interface element bounds.
    Content.TranslationX =
      Math.Max (Math.Min (0, x + e.TotalX), -Math.Abs (Content.Width - App.ScreenWidth));
    Content.TranslationY =
      Math.Max (Math.Min (0, y + e.TotalY), -Math.Abs (Content.Height - App.ScreenHeight));
    break;

  case GestureStatus.Completed:
    // Store the translation applied during the pan
    x = Content.TranslationX;
    y = Content.TranslationY;
    break;
  }
}
```

Ta metoda aktualizacji można przeglądać zawartość elementu interfejsu użytkownika zawinięty, oparte na gestu przesuwanie użytkownika. Jest to osiągane przy użyciu wartości [ `TotalX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.PanUpdatedEventArgs.TotalX/) i [ `TotalY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.PanUpdatedEventArgs.TotalY/) właściwości [ `PanUpdatedEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PanUpdatedEventArgs/) wystąpienie do obliczenia kierunek i odległość przesuwanie. `App.ScreenWidth` i `App.ScreenHeight` właściwości Podaj wysokość i szerokość okienka ekranu, a ustawiono ekranu szerokości i wysokości ekranu urządzenia przez odpowiednie projekty specyficzne dla platformy. Następnie przeciągnąć elementu opakowana użytkownika, ustawiając jego [ `TranslationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/) i [ `TranslationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/) właściwości do obliczonych wartości.

Podczas przesuwania zawartości w elemencie, który nie zajmuje pełny ekran, wysokość i szerokość okienka ekranu można uzyskać elementu [ `Height` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Height/) i [ `Width` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Width/) właściwości.

> [!NOTE]
> Wyświetlanie obrazów o wysokiej rozdzielczości mogą znacznie zwiększyć zużycie pamięci aplikacji. W związku z tym ich powinien zostać utworzony tylko gdy jest wymagana, a powinny zostać zwolnione, jak aplikacja nie wymaga już je. Aby uzyskać więcej informacji, zobacz [optymalizacji zasobów obrazu](~/xamarin-forms/deploy-test/performance.md#optimizeimages).

## <a name="summary"></a>Podsumowanie

Gest przesuwanie służy do wykrywania, przeciągając i jest realizowana za pomocą [ `PanGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PanGestureRecognizer/) klasy.



## <a name="related-links"></a>Linki pokrewne

- [PanGesture (przykład)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithGestures/PanGesture/)
- [GestureRecognizer](https://developer.xamarin.com/api/type/Xamarin.Forms.GestureRecognizer/)
- [PanGestureRecognizer](https://developer.xamarin.com/api/type/Xamarin.Forms.PanGestureRecognizer/)
