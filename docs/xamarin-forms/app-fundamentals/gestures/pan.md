---
title: Dodawanie aparat rozpoznawania gestów pan
description: W tym artykule wyjaśniono, jak używać gestu pan na poziomie i pionie Przesuwanie obrazu tak, aby cała zawartość obrazu mogą być wyświetlane, gdy jest on wyświetlany w mniejszych niż wymiary obrazu okienko ekranu.
ms.prod: xamarin
ms.assetid: 42CBD2CF-432D-4F19-A05E-D569BB7F8713
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/21/2016
ms.openlocfilehash: 59e9f4c61bda86faa5a55d70ef91411adb14da6d
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/18/2018
ms.locfileid: "38996809"
---
# <a name="adding-a-pan-gesture-recognizer"></a>Dodawanie aparat rozpoznawania gestów pan

_Gest pan służy do wykrywania przepływu palców na ekranie i stosowanie tego przepływu do zawartości i jest implementowane za pomocą `PanGestureRecognizer` klasy. Typowy scenariusz dla gestu panoramowanie jest obrazu, przesuń w pionie i poziomie tak, aby cała zawartość obrazu mogą być wyświetlane, gdy jest on wyświetlany w mniejszych niż wymiary obrazu okienko ekranu. Odbywa się przez przeniesienie obrazu wewnątrz okienka ekranu i została przedstawiona w tym artykule._

Aby element interfejsu użytkownika ruchome za pomocą gestu pan, Utwórz [ `PanGestureRecognizer` ](xref:Xamarin.Forms.PanGestureRecognizer) wystąpienia i obsługiwać [ `PanUpdated` ](xref:Xamarin.Forms.PanGestureRecognizer.PanUpdated) zdarzeń, i Dodaj nowy aparat rozpoznawania gestów do [ `GestureRecognizers` ](xref:Xamarin.Forms.View.GestureRecognizers) kolekcji na element interfejsu użytkownika. Poniższy kod przedstawia przykład `PanGestureRecognizer` dołączone do [ `Image` ](xref:Xamarin.Forms.Image) elementu:

```csharp
var panGesture = new PanGestureRecognizer();
panGesture.PanUpdated += (s, e) => {
  // Handle the pan
};
image.GestureRecognizers.Add(panGesture);
```

Ponadto można to osiągnąć w XAML, jak pokazano w poniższym przykładzie kodu:

```xaml
<Image Source="MonoMonkey.jpg">
  <Image.GestureRecognizers>
    <PanGestureRecognizer PanUpdated="OnPanUpdated" />
  </Image.GestureRecognizers>
</Image>
```

Kod `OnPanUpdated` program obsługi zdarzeń jest dodawane do pliku związanego z kodem:

```csharp
void OnPanUpdated (object sender, PanUpdatedEventArgs e)
{
  // Handle the pan
}
```

> [!NOTE]
> Poprawne przesuwania w systemie Android wymaga [pakietu NuGet 2.1.0-pre1 Xamarin.Forms](https://www.nuget.org/packages/Xamarin.Forms/2.1.0.6501-pre1) co najmniej.

## <a name="creating-a-pan-container"></a>Tworzenie kontenera pan

Ta sekcja zawiera uogólnionego pomocnikiem klasy, który wykonuje dowolne przesuwanie zazwyczaj dopasowany do poruszanie się w obrębie obrazów lub mapy. Obsługa gestu pan Aby wykonać tę operację wymaga talent matematyczny do przekształcania interfejsu użytkownika. Matematyczne ten jest używany do Przesuń tylko w obrębie granic elementu interfejsu użytkownika opakowana. Poniższy kod przedstawia przykład `PanContainer` klasy:

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

Ta klasa może zostać zawinięty wokół elementu interfejsu użytkownika, tak, aby gestu będzie Przesuwanie elementu interfejsu użytkownika opakowana. Ilustruje poniższy przykład kodu XAML `PanContainer` zawijania [ `Image` ](xref:Xamarin.Forms.Image) elementu:

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

Poniższy kod przedstawia przykładowy sposób, w jaki `PanContainer` opakowuje [ `Image` ](xref:Xamarin.Forms.Image) elementu na stronie C#:

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

W obu przykładach [ `WidthRequest` ](xref:Xamarin.Forms.VisualElement.WidthRequest) i [ `HeightRequest` ](xref:Xamarin.Forms.VisualElement.HeightRequest) właściwości są ustawione na wartości szerokości i wysokości obraz jest wyświetlany.

Gdy [ `Image` ](xref:Xamarin.Forms.Image) element odbiera gestu pan, wyświetlany obraz będzie panned. Panoramowanie odbywa się przez `PanContainer.OnPanUpdated` metody, która jest wyświetlana w poniższym przykładzie kodu:

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

Ta metoda aktualizuje zawartość wyświetlanego elementu interfejsu użytkownika opakowana, oparte na użytkownika pan gestu. Jest to osiągane przy użyciu wartości [ `TotalX` ](xref:Xamarin.Forms.PanUpdatedEventArgs.TotalX) i [ `TotalY` ](xref:Xamarin.Forms.PanUpdatedEventArgs.TotalY) właściwości [ `PanUpdatedEventArgs` ](xref:Xamarin.Forms.PanUpdatedEventArgs) wystąpienia do obliczania kierunku i odległość panoramowanie. `App.ScreenWidth` i `App.ScreenHeight` właściwości zapewniają wysokość i szerokość okienka ekranu i są ustawione na wartości wysokość ekranu urządzenia i szerokości ekranu przez odpowiednie projekty specyficzne dla platformy. Element opakowana użytkownika jest następnie panned, ustawiając jego [ `TranslationX` ](xref:Xamarin.Forms.VisualElement.TranslationX) i [ `TranslationY` ](xref:Xamarin.Forms.VisualElement.TranslationY) właściwości do obliczonych wartości.

Podczas przesuwania zawartości w elemencie, który nie zajmuje pełny ekran, wysokość i szerokość okienka ekranu można uzyskać od elementu [ `Height` ](xref:Xamarin.Forms.VisualElement.Height) i [ `Width` ](xref:Xamarin.Forms.VisualElement.Width) właściwości.

> [!NOTE]
> Wyświetlanie obrazów w wysokiej rozdzielczości może znacznie zwiększyć zużycie pamięci aplikacji. W związku z tym ich powinien zostać utworzony tylko podczas wymagane i powinny zostać opublikowane, jak aplikacja nie wymaga już je. Aby uzyskać więcej informacji, zobacz [zoptymalizować zasoby obrazów](~/xamarin-forms/deploy-test/performance.md#optimizeimages).

## <a name="related-links"></a>Linki pokrewne

- [PanGesture (przykład)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithGestures/PanGesture/)
- [GestureRecognizer](xref:Xamarin.Forms.GestureRecognizer)
- [PanGestureRecognizer](xref:Xamarin.Forms.PanGestureRecognizer)
