---
title: Dodawanie wzorca Tap aparat rozpoznawania gestów gestu
description: W tym artykule wyjaśniono, jak używać gest wykrywania wzorca tap w aplikacji platformy Xamarin.Forms. Wykrywanie wzorca TAP jest implementowane za pomocą klasy TapGestureRecognizer.
ms.prod: xamarin
ms.assetid: 1D150BAF-4157-49BC-90A0-153323B8EBCF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/21/2016
ms.openlocfilehash: e602ae1f140640d9a895b65d78feab3d0a3b7861
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994857"
---
# <a name="adding-a-tap-gesture-gesture-recognizer"></a>Dodawanie wzorca Tap aparat rozpoznawania gestów gestu

_Gest służy do wykrywania wzorca tap i jest implementowane za pomocą klasy TapGestureRecognizer._

## <a name="overview"></a>Omówienie

Aby element interfejsu użytkownika możesz klikać z gest, Utwórz [ `TapGestureRecognizer` ](xref:Xamarin.Forms.TapGestureRecognizer) wystąpienia i obsługiwać [ `Tapped` ](xref:Xamarin.Forms.TapGestureRecognizer.Tapped) zdarzeń i Dodaj nowy aparat rozpoznawania gestów do [ `GestureRecognizers` ](xref:Xamarin.Forms.View.GestureRecognizers) kolekcji na element interfejsu użytkownika. Poniższy kod przedstawia przykład `TapGestureRecognizer` dołączone do [ `Image` ](xref:Xamarin.Forms.Image) elementu:

```csharp
var tapGestureRecognizer = new TapGestureRecognizer();
tapGestureRecognizer.Tapped += (s, e) => {
    // handle the tap
};
image.GestureRecognizers.Add(tapGestureRecognizer);
```

Domyślnie obraz, który będzie odpowiadać na podsłuchu jednego. Ustaw [ `NumberOfTapsRequired` ](xref:Xamarin.Forms.TapGestureRecognizer.NumberOfTapsRequired) właściwość czekać na dwukrotne wybranie (lub więcej podsłuchu w razie potrzeby).

```csharp
tapGestureRecognizer.NumberOfTapsRequired = 2; // double-tap
```

Gdy [ `NumberOfTapsRequired` ](xref:Xamarin.Forms.TapGestureRecognizer.NumberOfTapsRequired) ma wartość powyżej, program obsługi zdarzeń będzie można wykonać tylko jeśli podsłuchu przeprowadzone okres (okres to nie można konfigurować). Jeśli podsłuchu drugi (lub późniejszy) nie pojawiają się w tym okresie skutecznie są ignorowane i ponownym uruchomieniu "count naciśnij".

<a name="Using_Xaml" />

## <a name="using-xaml"></a>Przy użyciu języka Xaml

Aparat rozpoznawania gestów można dodać do kontroli w języku Xaml przy użyciu dołączonych właściwości. Składnia służąca do dodawania [ `TapGestureRecognizer` ](xref:Xamarin.Forms.TapGestureRecognizer) na obrazie poniżej pokazano (w tym przypadku definiowania *naciśnij dwukrotnie* zdarzeń):

```xaml
<Image Source="tapped.jpg">
    <Image.GestureRecognizers>
        <TapGestureRecognizer
                Tapped="OnTapGestureRecognizerTapped"
                NumberOfTapsRequired="2" />
  </Image.GestureRecognizers>
</Image>
```

Kod obsługi zdarzeń (w przykładzie) zwiększa licznik i będzie zmieniał zdjęcie od koloru do czerni &amp; białe.

```csharp
void OnTapGestureRecognizerTapped(object sender, EventArgs args)
{
    tapCount++;
    var imageSender = (Image)sender;
    // watch the monkey go from color to black&white!
    if (tapCount % 2 == 0) {
        imageSender.Source = "tapped.jpg";
    } else {
        imageSender.Source = "tapped_bw.jpg";
    }
}
```

## <a name="using-icommand"></a>Za pomocą interfejsu ICommand

Użyj aplikacji, które zazwyczaj używają wzorca Mvvm `ICommand` zamiast bezpośrednio łącząc się procedury obsługi zdarzeń. [ `TapGestureRecognizer` ](xref:Xamarin.Forms.TapGestureRecognizer) Może łatwo obsługiwać `ICommand` albo przez ustawienie powiązania w kodzie:

```csharp
var tapGestureRecognizer = new TapGestureRecognizer();
tapGestureRecognizer.SetBinding (TapGestureRecognizer.CommandProperty, "TapCommand");
image.GestureRecognizers.Add(tapGestureRecognizer);
```

lub przy użyciu języka Xaml:

```xaml
<Image Source="tapped.jpg">
    <Image.GestureRecognizers>
        <TapGestureRecognizer
            Command="{Binding TapCommand}"
            CommandParameter="Image1" />
    </Image.GestureRecognizers>
</Image>
```

Kompletny kod dla tego modelu widoku znajdują się w próbce. Odpowiedni `Command` poniżej przedstawiono szczegóły implementacji:

```csharp
public class TapViewModel : INotifyPropertyChanged
{
    int taps = 0;
    ICommand tapCommand;
    public TapViewModel () {
        // configure the TapCommand with a method
        tapCommand = new Command (OnTapped);
    }
    public ICommand TapCommand {
        get { return tapCommand; }
    }
    void OnTapped (object s)  {
        taps++;
        Debug.WriteLine ("parameter: " + s);
    }
    //region INotifyPropertyChanged code omitted
}
```

## <a name="summary"></a>Podsumowanie

Gest służy do wykrywania wzorca tap i jest implementowane za pomocą [ `TapGestureRecognizer` ](xref:Xamarin.Forms.TapGestureRecognizer) klasy. Można określić liczbę podsłuchu rozpoznawanie dwukrotnego (lub naciśnij trzykrotnie lub więcej naciśnięcia) zachowanie.


## <a name="related-links"></a>Linki pokrewne

- [TapGesture (przykład)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithGestures/TapGesture/)
- [GestureRecognizer](xref:Xamarin.Forms.GestureRecognizer)
- [TapGestureRecognizer](xref:Xamarin.Forms.TapGestureRecognizer)
