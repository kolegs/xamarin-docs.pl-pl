---
title: Dodawanie aparat rozpoznawania gestów gestu Tap
description: W tym artykule opisano sposób użycia gestu naciśnij wykrywania naciśnij w aplikacji platformy Xamarin.Forms. Naciśnij wykrywania jest zaimplementowana w klasie TapGestureRecognizer.
ms.prod: xamarin
ms.assetid: 1D150BAF-4157-49BC-90A0-153323B8EBCF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/21/2016
ms.openlocfilehash: bbe4ca7a1080459b8aeb33640be5158b15e97715
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35240668"
---
# <a name="adding-a-tap-gesture-gesture-recognizer"></a>Dodawanie aparat rozpoznawania gestów gestu Tap

_Gest naciśnij służy do wykrywania naciśnij i jest realizowana za pomocą klasy TapGestureRecognizer._

## <a name="overview"></a>Omówienie

Aby elementu interfejsu użytkownika aktywne z gestów tap, Utwórz [ `TapGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/) wystąpienia i obsługiwać [ `Tapped` ](https://developer.xamarin.com/api/event/Xamarin.Forms.TapGestureRecognizer.Tapped/) zdarzeń i Dodaj nowy aparat rozpoznawania gestów do [ `GestureRecognizers` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.GestureRecognizers/) kolekcji dla elementu interfejsu użytkownika. Poniższy kod przedstawia przykład `TapGestureRecognizer` dołączony do [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) elementu:

```csharp
var tapGestureRecognizer = new TapGestureRecognizer();
tapGestureRecognizer.Tapped += (s, e) => {
    // handle the tap
};
image.GestureRecognizers.Add(tapGestureRecognizer);
```

Domyślnie obraz będzie odpowiadać podsłuchu pojedynczego. Ustaw [ `NumberOfTapsRequired` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TapGestureRecognizer.NumberOfTapsRequired/) właściwości oczekiwania dwukrotne (lub więcej podsłuchu, jeśli jest to wymagane).

```csharp
tapGestureRecognizer.NumberOfTapsRequired = 2; // double-tap
```

Gdy [ `NumberOfTapsRequired` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TapGestureRecognizer.NumberOfTapsRequired/) ma wartość powyżej, program obsługi zdarzeń będzie można wykonywać tylko jeśli podsłuchu przeprowadzone w ciągu okres czasu (okres nie jest to konfigurowalne). Jeśli podsłuchu drugi (lub późniejszy) nie występują w tym okresie skutecznie są ignorowane i ponowne uruchomienie "naciśnij count".

<a name="Using_Xaml" />

## <a name="using-xaml"></a>Przy użyciu kodu Xaml

Aparat rozpoznawania gestów można dodać do formantu w języku Xaml, przy użyciu dołączone właściwości. Składni, aby dodać [ `TapGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/) do obrazu są wyświetlane poniżej (w tym przypadku definiowanie *naciśnij dwa razy* zdarzeń):

```xaml
<Image Source="tapped.jpg">
    <Image.GestureRecognizers>
        <TapGestureRecognizer
                Tapped="OnTapGestureRecognizerTapped"
                NumberOfTapsRequired="2" />
  </Image.GestureRecognizers>
</Image>
```

Kod obsługi zdarzeń (w przykładowych) zwiększa licznik i zmienia się obraz z kolor czarny &amp; białym znakiem.

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

## <a name="using-icommand"></a>Przy użyciu interfejsu ICommand

Użyj aplikacji korzystających ze wzorca Mvvm zwykle `ICommand` zamiast bezpośrednio okablowania się procedury obsługi zdarzeń. [ `TapGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/) Ułatwia `ICommand` albo ustawiając powiązania w kodzie:

```csharp
var tapGestureRecognizer = new TapGestureRecognizer();
tapGestureRecognizer.SetBinding (TapGestureRecognizer.CommandProperty, "TapCommand");
image.GestureRecognizers.Add(tapGestureRecognizer);
```

lub przy użyciu kodu Xaml:

```xaml
<Image Source="tapped.jpg">
    <Image.GestureRecognizers>
        <TapGestureRecognizer
            Command="{Binding TapCommand}"
            CommandParameter="Image1" />
    </Image.GestureRecognizers>
</Image>
```

Kompletny kod dla tego modelu widoku znajdują się w próbce. Odpowiednie `Command` poniżej przedstawiono szczegóły implementacji:

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

Gest naciśnij służy do wykrywania naciśnij i jest realizowana za pomocą [ `TapGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/) klasy. Można określić liczbę podsłuchu rozpoznawanie dwukrotnego (lub trzykrotne wybranie lub więcej naciska) zachowanie.


## <a name="related-links"></a>Linki pokrewne

- [TapGesture (przykład)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithGestures/TapGesture/)
- [GestureRecognizer](https://developer.xamarin.com/api/type/Xamarin.Forms.GestureRecognizer/)
- [TapGestureRecognizer](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/)
