---
title: Formant obrazu
ms.topic: article
ms.prod: xamarin
ms.assetid: B741C207-3427-46F3-9C90-A52BF8933FA4
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 4184d7babc396a6b6179e6876dced34b773474b4
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/22/2018
---
# <a name="image-control"></a>Formant obrazu

udostępnia watchOS [ `WKInterfaceImage` ](https://developer.xamarin.com/api/type/WatchKit.WKInterfaceImage/) formantu do wyświetlania obrazów i prostych animacji. Niektóre formanty mogą mieć również obraz tła (takie jak przyciski, grup i kontrolery interfejsu).

![](image-images/image-walkway.png "Obraz przedstawiający Apple Watch") ![ ] (image-images/image-animation.png "Apple Watch z prostej animacji")
<!-- watch image courtesy of http://infinitapps.com/bezel/ -->

Obrazy katalogu zasobów umożliwia dodawanie obrazów do zestawu czujki aplikacji.
Tylko  **@2x**  wersji są wymagane, ponieważ wszystkie Obejrzyj urządzeń ma Wyświetla siatkówki.

![](image-images/asset-universal-sml.png "Tylko 2 wersje x są wymagane, ponieważ wszystkie Obejrzyj urządzeń ma Wyświetla siatkówki")

Jest dobrym rozwiązaniem, upewnij się, że odpowiedni rozmiar wyświetlania czujki obrazów samodzielnie. *Unikaj* przy użyciu obrazów niepoprawnie rozmiarze (szczególnie duże z nich) i skalowanie, aby je wyświetlić na czujki.

Rozmiary czujki Kit (38mm i 42mm) obrazu katalogu zasobów umożliwia określ różne obrazy dla każdego rozmiaru ekranu.

![](image-images/asset-watch-sml.png "Rozmiary zestawu czujki 38 do 42mm obrazu katalogu zasobów służy do określ różne obrazy dla każdego rozmiaru ekranu")


## <a name="images-on-the-watch"></a>Obrazy w czujki

Najbardziej wydajnym sposobem wyświetlania obrazów jest *je uwzględnić w projekcie aplikacji czujki* i wyświetlić je za pomocą `SetImage(string imageName)` metody.

Na przykład [WatchKitCatalog](https://developer.xamarin.com/samples/WatchKitCatalog/) próbki ma liczbę obrazów dodane do katalogu zasobów w projekcie aplikacji watch:

![](image-images/asset-whale-sml.png "Przykładowe WatchKitCatalog ma liczbę obrazów dodane do katalogu zasobów w projekcie aplikacji czujki")

Te można efektywnie załadowana i wyświetlona przy użyciu czujki `SetImage` za pomocą parametru Nazwa ciągu:

```csharp
myImageControl.SetImage("Whale");
myOtherImageControl.SetImage("Worry");
```

### <a name="background-images"></a>Obrazy tła

Ta sama logika dotyczy `SetBackgroundImage (string imageName)` na `Button`, `Group`, i `InterfaceController` klasy. Najlepszą wydajność, uzyskuje się poprzez zapisywania w czujki aplikacja.


## <a name="images-in-the-watch-extension"></a>Obrazy w rozszerzeniu czujki

Oprócz ładowania obrazów, które są przechowywane w samej aplikacji czujki, można wysyłać obrazów z pakietu rozszerzenia do aplikacji czujki do wyświetlenia lub można pobrać obrazów z lokalizacji zdalnej i wyświetlić te.

Do ładowania obrazów z rozszerzenia czujki, Utwórz `UIImage` wystąpienia, a następnie wywołać `SetImage` z `UIImage` obiektu.

Na przykład [WatchKitCatalog](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/) próbki ma obrazu o nazwie **Bumblebee** w projekcie rozszerzenia watch:

![](image-images/asset-bumblebee-sml.png "Przykład WatchKitCatalog ma obrazu o nazwie Bumblebee w projekcie rozszerzenia czujki")

Poniższy kod spowoduje:

- obraz ładowany do pamięci, i
- wyświetlane na czujki.

```csharp
using (var image = UIImage.FromBundle ("Bumblebee")) {
    myImageControl.SetImage (image);
}
```


## <a name="animations"></a>Animacji

Aby animować utworzenie zestawu obrazów, one wszystkich rozpoczynać się od tego samego prefiksu i sufiksu liczbowego.

[WatchKitCatalog](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/) próbki ma szereg numerowane obrazów w projekcie aplikacji czujki z **magistrali** prefiksu:

![](image-images/asset-bus-animation-sml.png "Przykładowe WatchKitCatalog ma szereg numerowane obrazów w projekcie aplikacji czujki z prefiksem magistrali")

Aby wyświetlić te obrazy jako animacji, najpierw załadować przy użyciu obrazu `SetImage` z prefiks nazwy, a następnie wywołania `StartAnimating`:

```csharp
animatedImage.SetImage ("Bus");
animatedImage.StartAnimating ();
```

Wywołanie `StopAnimating` w formancie obrazu można zatrzymać Zapętlanie animacji:

```csharp
animatedImage.StopAnimating ();
```


<a name="cache" />

## <a name="appendix-caching-images-watchos-1"></a>Dodatek: Buforowania obrazów (watchOS 1)

> [!IMPORTANT]
> aplikacje watchOS 3 działać na urządzeniu. Następujące informacje są watchOS 1 tylko dla aplikacji.

Jeśli aplikacja używa wielokrotnie obrazu, który jest przechowywany w rozszerzeniu (lub pobraniu), istnieje możliwość buforowania obrazu w magazynie czujki, aby zwiększyć wydajność w przypadku kolejnych Wyświetla.

Użyj `WKInterfaceDevice`s `AddCachedImage` metody przesyłanie obrazu do wyrażenie kontrolne, a następnie użyć `SetImage` za pomocą parametru Nazwa obrazu jako ciąg w celu wyświetlenia go:

```csharp
var device = WKInterfaceDevice.CurrentDevice;
using (var image = UIImage.FromBundle ("Bumblebee")) {
    if (!device.AddCachedImage (image, "Bumblebee")) {
            Console.WriteLine ("Image cache full.");
        } else {
            cachedImage.SetImage ("Bumblebee");
        }
    }
}
```

Zawartość pamięci podręcznej obrazów za pomocą kodu można zbadać `WKInterfaceDevice.CurrentDevice.WeakCachedImages`.


### <a name="managing-the-cache"></a>Zarządzanie pamięcią podręczną

Pamięć podręczna o rozmiarze około 20 MB. Jest ona przechowywana uruchomieniach aplikacji i wypełniał jest obowiązek wyczyszczenie plików za pomocą `RemoveCachedImage` lub `RemoveAllCachedImages` metody `WKInterfaceDevice.CurrentDevice` obiektu.



## <a name="related-links"></a>Linki pokrewne

- [WatchKitCatalog (przykład)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [Doc obrazu firmy Apple](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/Images.html)
