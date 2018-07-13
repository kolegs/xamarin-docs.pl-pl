---
title: Dodawanie rozpoznawania gest Uszczypnięcia
description: W tym artykule wyjaśniono, jak używać gest uszczypnięcia do wykonywania interaktywne powiększenie obrazu w lokalizacji uszczypnięcia.
ms.prod: xamarin
ms.assetid: 832F7810-F0CF-441A-B04A-3975F3FB8B29
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/21/2016
ms.openlocfilehash: 37befdcd4ccbcd49e3cebda92d55ae6f70da2ad6
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998705"
---
# <a name="adding-a-pinch-gesture-recognizer"></a>Dodawanie rozpoznawania gest Uszczypnięcia

_Gest uszczypnięcia jest używany do operacji interaktywnych powiększenia i jest implementowane za pomocą klasy PinchGestureRecognizer. Jest to typowy scenariusz dla gest uszczypnięcia przeprowadzić interaktywne powiększenie obrazu w lokalizacji uszczypnięcia. Odbywa się przy użyciu skalowania zawartość okienka ekranu i została przedstawiona w tym artykule._

## <a name="overview"></a>Omówienie

Aby element interfejsu użytkownika którą można powiększać z gest uszczypnięcia, Utwórz [ `PinchGestureRecognizer` ](xref:Xamarin.Forms.PinchGestureRecognizer) wystąpienia i obsługiwać [ `PinchUpdated` ](xref:Xamarin.Forms.PinchGestureRecognizer.PinchUpdated) zdarzeń, i Dodaj nowy aparat rozpoznawania gestów do [ `GestureRecognizers` ](xref:Xamarin.Forms.View.GestureRecognizers) kolekcji na element interfejsu użytkownika. Poniższy kod przedstawia przykład `PinchGestureRecognizer` dołączone do [ `Image` ](xref:Xamarin.Forms.Image) elementu:

```csharp
var pinchGesture = new PinchGestureRecognizer();
pinchGesture.PinchUpdated += (s, e) => {
  // Handle the pinch
};
image.GestureRecognizers.Add(pinchGesture);
```

Ponadto można to osiągnąć w XAML, jak pokazano w poniższym przykładzie kodu:

```xaml
<Image Source="waterfront.jpg">
  <Image.GestureRecognizers>
    <PinchGestureRecognizer PinchUpdated="OnPinchUpdated" />
  </Image.GestureRecognizers>
</Image>
```

Kod `OnPinchUpdated` program obsługi zdarzeń jest dodawane do pliku związanego z kodem:

```csharp
void OnPinchUpdated (object sender, PinchGestureUpdatedEventArgs e)
{
  // Handle the pinch
}
```

## <a name="creating-a-pinchtozoom-container"></a>Tworzenie kontenera PinchToZoom

Obsługa gest uszczypnięcia, aby wykonać operację powiększania wymaga talent matematyczny do przekształcania interfejsu użytkownika. Ta sekcja zawiera klasę pomocnika uogólnionego do wykonywania obliczeń, który może służyć do interaktywnego powiększenia dowolnego elementu interfejsu użytkownika. Poniższy kod przedstawia przykład `PinchToZoomContainer` klasy:

```csharp
public class PinchToZoomContainer : ContentView
{
  ...

  public PinchToZoomContainer ()
  {
    var pinchGesture = new PinchGestureRecognizer ();
    pinchGesture.PinchUpdated += OnPinchUpdated;
    GestureRecognizers.Add (pinchGesture);
  }

  void OnPinchUpdated (object sender, PinchGestureUpdatedEventArgs e)
  {
    ...
  }
}
```

Ta klasa może zostać zawinięty wokół elementu interfejsu użytkownika, tak, aby gest uszczypnięcia zostanie powiększony elementu interfejsu użytkownika opakowana. Ilustruje poniższy przykład kodu XAML `PinchToZoomContainer` zawijania [ `Image` ](xref:Xamarin.Forms.Image) elementu:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:PinchGesture;assembly=PinchGesture"
             x:Class="PinchGesture.HomePage">
    <ContentPage.Content>
        <Grid Padding="20">
            <local:PinchToZoomContainer>
                <local:PinchToZoomContainer.Content>
                    <Image Source="waterfront.jpg" />
                </local:PinchToZoomContainer.Content>
            </local:PinchToZoomContainer>
        </Grid>
    </ContentPage.Content>
</ContentPage>
```

Poniższy kod przedstawia przykładowy sposób, w jaki `PinchToZoomContainer` opakowuje [ `Image` ](xref:Xamarin.Forms.Image) elementu na stronie C#:

```csharp
public class HomePageCS : ContentPage
{
  public HomePageCS ()
  {
    Content = new Grid {
      Padding = new Thickness (20),
      Children = {
        new PinchToZoomContainer {
          Content = new Image { Source = ImageSource.FromFile ("waterfront.jpg") }
        }
      }
    };
  }
}
```

Gdy [ `Image` ](xref:Xamarin.Forms.Image) element odbiera gest uszczypnięcia, wyświetlany obraz będzie mieć powiększonej lub w poziomie. Wartość powiększenia odbywa się przez `PinchZoomContainer.OnPinchUpdated` metody, która jest wyświetlana w poniższym przykładzie kodu:

```csharp
void OnPinchUpdated (object sender, PinchGestureUpdatedEventArgs e)
{
  if (e.Status == GestureStatus.Started) {
    // Store the current scale factor applied to the wrapped user interface element,
    // and zero the components for the center point of the translate transform.
    startScale = Content.Scale;
    Content.AnchorX = 0;
    Content.AnchorY = 0;
  }
  if (e.Status == GestureStatus.Running) {
    // Calculate the scale factor to be applied.
    currentScale += (e.Scale - 1) * startScale;
    currentScale = Math.Max (1, currentScale);

    // The ScaleOrigin is in relative coordinates to the wrapped user interface element,
    // so get the X pixel coordinate.
    double renderedX = Content.X + xOffset;
    double deltaX = renderedX / Width;
    double deltaWidth = Width / (Content.Width * startScale);
    double originX = (e.ScaleOrigin.X - deltaX) * deltaWidth;

    // The ScaleOrigin is in relative coordinates to the wrapped user interface element,
    // so get the Y pixel coordinate.
    double renderedY = Content.Y + yOffset;
    double deltaY = renderedY / Height;
    double deltaHeight = Height / (Content.Height * startScale);
    double originY = (e.ScaleOrigin.Y - deltaY) * deltaHeight;

    // Calculate the transformed element pixel coordinates.
    double targetX = xOffset - (originX * Content.Width) * (currentScale - startScale);
    double targetY = yOffset - (originY * Content.Height) * (currentScale - startScale);

    // Apply translation based on the change in origin.
    Content.TranslationX = targetX.Clamp (-Content.Width * (currentScale - 1), 0);
    Content.TranslationY = targetY.Clamp (-Content.Height * (currentScale - 1), 0);

    // Apply scale factor.
    Content.Scale = currentScale;
  }
  if (e.Status == GestureStatus.Completed) {
    // Store the translation delta's of the wrapped user interface element.
    xOffset = Content.TranslationX;
    yOffset = Content.TranslationY;
  }
}
```

Ta metoda aktualizuje poziom powiększenia opakowany element interfejsu użytkownika oparte na gest uszczypnięcia użytkownika. Jest to osiągane przy użyciu wartości [ `Scale` ](xref:Xamarin.Forms.PinchGestureUpdatedEventArgs.Scale), [ `ScaleOrigin` ](xref:Xamarin.Forms.PinchGestureUpdatedEventArgs.ScaleOrigin) i [ `Status` ](xref:Xamarin.Forms.PinchGestureUpdatedEventArgs.Status) właściwości [ `PinchGestureUpdatedEventArgs` ](xref:Xamarin.Forms.PinchGestureUpdatedEventArgs) wystąpienia, aby obliczyć współczynnik skali, które mają być stosowane w źródle gest uszczypnięcia. Elementu opakowana użytkownika jest następnie powiększone w źródle gest uszczypnięcia, ustawiając jego [ `TranslationX` ](xref:Xamarin.Forms.VisualElement.TranslationX), [ `TranslationY` ](xref:Xamarin.Forms.VisualElement.TranslationY), i [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) właściwości do obliczonych wartości.

## <a name="summary"></a>Podsumowanie

Gest uszczypnięcia służy do przeprowadzania interaktywne powiększenia i jest implementowane za pomocą [ `PinchGestureRecognizer` ](xref:Xamarin.Forms.PinchGestureRecognizer) klasy.


## <a name="related-links"></a>Linki pokrewne

- [PinchGesture (przykład)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithGestures/PinchGesture/)
- [GestureRecognizer](xref:Xamarin.Forms.GestureRecognizer)
- [PinchGestureRecognizer](xref:Xamarin.Forms.PinchGestureRecognizer)
