---
title: Wyświetlanie obrazów w Xamarin.iOS
description: W tym artykule opisano w tym zasób obrazu w aplikacji platformy Xamarin.iOS i wyświetlania obrazu przy użyciu kodu C# lub przypisywania go do kontroli w systemie iOS projektanta.
ms.prod: xamarin
ms.assetid: 60288B12-49E3-4E87-8690-D04A5EC7A664
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/24/2018
ms.openlocfilehash: 3ae63bb30c7759a1915939a2199d5ffc7dc75d15
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34784276"
---
# <a name="displaying-an-image-in-xamarinios"></a>Wyświetlanie obrazów w Xamarin.iOS

_W tym artykule opisano w tym zasób obrazu w aplikacji platformy Xamarin.iOS i wyświetlania obrazu przy użyciu kodu C# lub przypisywania go do kontroli w systemie iOS projektanta._

W tym artykule opisano szczegółowo następujące tematy:

- [Dodawanie i organizowanie obrazów w aplikacji platformy Xamarin.iOS](#adding-assets) — obejmuje zasoby obrazów i jak mogą być uwzględniony, uporządkowane i zarządzać nim z projektu platformy Xamarin.iOS.
- [Dodawanie obrazów do zawartości katalogów](#asset-catalogs) — zarządzanie obrazami z katalogami zasobów.
    - [Za pomocą wektora obrazów w wykazach zasobów](#Using-Vector-Images-in-Asset-Catalogs) — dzięki wszystkie rozmiary obrazu z jednego wektora.
- [Praca z obrazami szablonu](#Working-with-Template-Images) — przez ustawienie zasób obrazu jako obrazu szablonu, deweloper może być łatwo Kolorowanie aby dopasować zmiany motywu interfejsu użytkownika aplikacji przez ustawienie obrazu `Tint` właściwości.
- [Przy użyciu obrazów z formantami](#controls) — obejmuje korzystanie z zasobów obrazu zawarte w projekcie platformy Xamarin.iOS z kontrolek interfejsu użytkownika, takie jak `UIButton` i `UIImageView` i sposobu pracy z obrazów w języku C# za pomocą `UIImage` obiektu [FromBundle](#frombundle) metody.
- [Wyświetlanie obrazów w Scenorys](#Displaying-an-Image-in-a-Storyboards) — przykład wyświetlania obrazu przy użyciu scenorysu.
- [Wyświetla obraz w kodzie](#Displaying-an-Image-in-Code) — przykład wyświetlania obrazu przy użyciu kodu C#.

<a name="adding-assets" />

## <a name="adding-and-organizing-images-in-a-xamarinios-app"></a>Dodawanie i organizowanie obrazów w aplikacji platformy Xamarin.iOS

Przy dodawaniu obrazu do użycia w aplikacji platformy Xamarin.iOS, deweloper użyje _katalogu zasobów_ do obsługi każdego urządzenia z systemem iOS i rozpoznawania wymagane przez aplikację.

Dodane w systemie iOS 7, **Ustawia obraz katalogi zasobów** zawiera wszystkie wersje lub reprezentacje obrazu, które są niezbędne do obsługi różnych urządzeń i skalować czynników dla aplikacji. Zamiast polegania na nazwę pliku zasobów obrazu (zobacz [obrazów niezależnie od rozdzielczości i nomenklaturę obrazu](~/ios/app-fundamentals/images-icons/displaying-an-image.md)), **Ustawia obraz** Określ obraz, który należy do urządzenia i/lub rozwiązania przy użyciu pliku Json . Jest to preferowany sposób zarządzania i obsługi obrazów w systemie iOS (z systemem iOS 9 lub nowsza).

<a name="asset-catalogs" />

## <a name="adding-images-to-an-asset-catalog-image-set"></a>Ustawianie obrazów dodanie do obrazu w katalogu zasobów

Jak wspomniano powyżej, **Ustawia obraz katalogi zasobów** zawiera wszystkie wersje lub reprezentacje obrazu, które są niezbędne do obsługi różnych urządzeń i skalować czynników dla aplikacji. Zamiast polegania na nazwę pliku zasobów obrazu **Ustawia obraz** Użyj pliku Json, aby określić, który obraz należy do urządzenia i/lub rozdzielczości.

Aby utworzyć nowy zestaw obrazu i dodać do niej obrazy, wykonaj następujące czynności:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. W **Eksploratora rozwiązań**, kliknij dwukrotnie `Assets.xcassets` plik, aby otworzyć do edycji:

    ![](displaying-an-image-images/imageset01.png "Assets.xcassets w Eksploratorze rozwiązań")
2. Kliknij prawym przyciskiem myszy **listę zasobów** i wybierz **nowy zestaw obrazu**:

    ![](displaying-an-image-images/imageset02.png "Dodawanie nowego zestawu obrazów")
3. Wybierz nowy zestaw obrazu i Edytor będą wyświetlane:

    ![](displaying-an-image-images/imageset03.png "Ustaw obraz edytora")
4. W tym miejscu, przeciągnij obrazy dla każdego z różnych urządzeń i i rozwiązania wymagane. (Uwaga: które te rozwiązania pasują do rozwiązania, określona w [rozmiary obrazów i plików](~/ios/app-fundamentals/images-icons/displaying-an-image.md) dokumentu.)
5. Kliknij dwukrotnie plik nowy zestaw obrazu **nazwa** w **listę zasobów** edytowania: ![ ] (displaying-an-image-images/imageset04.png "edycji nazwę nowego zbioru obrazu")

Korzystając z **Ustaw obraz** w systemie iOS projektanta, po prostu wybierz nazwę zestawu z listy rozwijanej w edytorze właściwości:

![](displaying-an-image-images/imageset06.png "Wybierz nazwę zestawu obrazów z listy rozwijanej")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Otwórz katalog zasobów z **Eksploratora rozwiązań**, a w lewym górnym rogu kliknij **Plus** przycisk:

    ![](displaying-an-image-images/asset5.png "Kliknij przycisk")

2. Wybierz **Dodawanie zestawu obrazu** Ustaw obraz edytora pojawi się nowego zestawu obrazów. W tym miejscu, przeciągnij obrazy dla każdego z różnych urządzeń i i rozwiązania wymagane. (Uwaga: które te rozwiązania pasują do rozwiązania, określona w [rozmiary obrazów i plików](~/ios/app-fundamentals/images-icons/displaying-an-image.md) dokumentu):

    ![](displaying-an-image-images/asset7.png "Edytor zestawu obrazów")

### <a name="renaming-an-image-set"></a>Zmiana nazwy zestawu obrazów

Aby zmienić nazwę Set protokołu obrazu, wykonaj następujące czynności:

1. W **Eksploratora rozwiązań**, kliknij dwukrotnie **katalogu zasobów** plik, aby otworzyć do edycji:

    ![](displaying-an-image-images/rename01.png "Katalog zasobów w Eksploratorze rozwiązań")
2. Wybierz **Ustaw obraz** można zmienić nazwy:

    ![](displaying-an-image-images/rename02.png "Wybierz zestaw obrazu można zmienić nazwy")
3. W **Explorer właściwości**, przewiń w dół i wybierz **nazwa**(w obszarze **różne** sekcji):

    ![](displaying-an-image-images/rename03.png "Wybierz nazwę sekcji różne")
4. Wprowadź nowy **nazwa** dla **Ustaw obraz** i zapisać zmiany.

-----

Korzystając z **Ustaw obraz** w kodzie, odwołać go za pomocą nazwy przez wywołanie metody `FromBundle` metody `UIImage` klasy. Na przykład:

```csharp
MonkeyImage.Image = UIImage.FromBundle ("PurpleMonkey");
```

> [!IMPORTANT]
> Jeśli obrazy przypisane do Set protokołu obrazu są nie pojawia się prawidłowo, upewnij się, że prawidłowa nazwa pliku jest używany z `FromBundle` — metoda ( **Ustaw obraz** , a nie nadrzędnego **katalogu zasobów** nazwy). W przypadku obrazów PNG `.png` rozszerzenia można pominąć. W przypadku innych formatów obrazów rozszerzenie jest wymagane (np. `PurpleMonkey.jpg`).

<a name="Using-Vector-Images-in-Asset-Catalogs" />

### <a name="using-vector-images-in-asset-catalogs"></a>Za pomocą wektora obrazów w wykazach zasobów

Począwszy od systemu iOS 8 specjalne **wektor** klasa został dodany do **Ustawia obraz** , która umożliwia deweloperom obejmują **PDF** sformatowany obrazu wektor kasety zamiast niej w tym Mapa bitowa poszczególnych plików w różne metody rozwiązywania. Za pomocą tej metody dostarczania wektor pojedynczego pliku `@1x` rozdzielczości (w formacie pliku PDF wektor) i `@2x` i `@3x` wersje pliku, zostanie wygenerowany w czasie kompilacji i zawartych w pakiecie aplikacji.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](displaying-an-image-images/imageset05.png "Wektor obrazów w edytorze zasobów katalogów")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](displaying-an-image-images/asset8.png "Wektor obrazów w edytorze zasobów katalogów")

-----

Na przykład, jeśli zawiera dewelopera `MonkeyIcon.pdf` pliku jako wektor katalogu zasobów z rozdzielczością x 150px 150px, mapy bitowej następujące zasoby będą objęte pakietu ostatecznej aplikacji podczas jego kompilacji:

- `MonkeyIcon@1x.png` -150px x 150px rozpoznawania.
- `MonkeyIcon@2x.png` -300px x 300px rozpoznawania.
- `MonkeyIcon@3x.png` -450px x 450px rozpoznawania.

Następujące powinna być brana pod uwagę podczas przy użyciu plików PDF wektor obrazów w wykazach zasobów:

- Nie jest wektor pełnej obsługi, ponieważ plik PDF będzie rasteryzacji do mapy bitowej w czasie kompilacji i map bitowych w końcowym aplikacji.
- Nie można dostosować rozmiar obrazu, gdy ustawiono go w katalogu zasobów. Jeśli developer podejmie próbę Zmień rozmiar obrazu (lub w kodzie za pomocą klasy rozmiaru i układu automatycznego) obraz będzie zniekształcony, podobnie jak inne mapy bitowej.
- Katalogi zasobów są tylko zgodne z systemem iOS 7 lub nowszej, jeśli potrzebować aplikacja do obsługi systemu iOS 6 lub niższy, nie może używać zasobów katalogów.

<a name="Working-with-Template-Images" />

## <a name="working-with-template-images"></a>Praca z obrazami szablonu

Oparte na projekt aplikacji systemu iOS, mogą wystąpić razy podczas deweloper musi dostosować ikony lub obrazu w interfejsie użytkownika, aby dopasować zmiany schematu kolorów (na przykład zgodnie z preferencjami użytkownika).

Aby łatwo osiągnąć ten efekt, Przełącz _tryb renderowania_ trwałego obrazu **obrazu szablonu**:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](displaying-an-image-images/templateimage01.png "Tryb renderowania ustawioną obrazu szablonu")](displaying-an-image-images/templateimage01.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](displaying-an-image-images/templateimage01vs.png "Ustawiony tryb renderowania do szablonu")](displaying-an-image-images/templateimage01vs.png#lightbox)

-----

Z systemu iOS projektanta przypisywanie zasób obrazu do formantu interfejsu użytkownika, a następnie ustaw **odcień** do kolorowanie obrazu:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](displaying-an-image-images/templateimage03.png "Ustaw odcień do kolorowanie obrazów")](displaying-an-image-images/templateimage03.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](displaying-an-image-images/templateimage03vs.png "Ustaw odcień do kolorowanie obrazów")](displaying-an-image-images/templateimage03vs.png#lightbox)

-----

Opcjonalnie zasób obrazu i odcień można ustawić bezpośrednio w kodzie:

```csharp
MyIcon.Image = UIImage.FromBundle ("MessageIcon");
MyIcon.TintColor = UIColor.Red;
```

Aby użyć obrazu szablonu, całkowicie z kodu, wykonaj następujące czynności:

```csharp
if (MyIcon.Image != null) {
    var mutableImage = MyIcon.Image.ImageWithRenderingMode (UIImageRenderingMode.AlwaysTemplate);
    MyIcon.Image = mutableImage;
    MyIcon.TintColor = UIColor.Red;
}
```

Ponieważ `RenderMode` właściwość `UIImage` jest tylko do odczytu, użyj `ImageWithRenderingMode` metodę w celu utworzenia nowego wystąpienia obrazu z ustawieniem żądany tryb renderowania.

Istnieją trzy prawdopodobnie ustawienia `UIImage.RenderMode` za pośrednictwem `UIImageRenderingMode` wyliczenia:

* `AlwaysOriginal` -Wymusza obraz, który ma być renderowany jako oryginalny plik obrazu źródłowego bez wprowadzania żadnych zmian.
* `AlwaysTemplate` -Wymusza obraz, który ma być renderowany jako obrazu szablonu przez kolorowanie pikseli z określonym `Tint` kolorów.
* `Automatic` -Albo renderuje obraz jako szablon lub oryginalne opartych na środowisku, który jest używany w. Na przykład, jeśli obraz jest używany w `UIToolBar`, `UINavigationBar`, `UITabBar` lub `UISegmentControl` będzie traktowane jako szablon.

<a name="Adding-new-Assets-Collections" />

## <a name="adding-new-assets-collections"></a>Dodawanie nowej kolekcji zasobów

Podczas pracy z obrazami w wykazach zasoby mogą być razy podczas Nowa kolekcja będzie wymagane, zamiast dodawania wszystkich obrazów aplikacji `Assets.xcassets` kolekcji. Na przykład podczas projektowania zasobów na żądanie.

Aby dodać nowy katalog zasoby do projektu:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Kliknij prawym przyciskiem myszy **Nazwa projektu** w **Eksploratora rozwiązań** i wybierz **Dodaj** > **nowego pliku...**
2. Wybierz **iOS** > **katalogu zasobów**, wprowadź **nazwa** kolekcji i kliknij **nowy** przycisk:

    ![](displaying-an-image-images/asset01.png "Tworzenie nowego katalogu zasobów")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **katalogi zasobów** folder, a następnie wybierz **Dodaj > Nowy katalog zasobów**.
2. Nadaj mu nazwę, a następnie kliknij przycisk **Dodaj**:

    ![](displaying-an-image-images/asset1.png "Tworzenie nowego katalogu zasobów")

-----


W tym miejscu kolekcji można by pracować w taki sam sposób jak domyślne `Assets.xcassets` kolekcji automatycznie dołączony do projektu.

<a name="controls" />

## <a name="using-images-with-controls"></a>Przy użyciu obrazów z formantami

Oprócz używania obrazów do obsługi aplikacji, iOS używa obrazów z typów kontroli aplikacji, takich jak paski kartę, pasków narzędzi pasków nawigacji, tabele i przycisków. Prosty sposób utworzyć obraz wyświetlany w formancie jest przypisanie `UIImage` wystąpienie do formantu `Image` właściwości.

<a name="frombundle" />

### <a name="frombundle"></a>FromBundle

`FromBundle` Wywołania metody jest synchroniczne wywołanie (blokowanie), które zawiera szereg funkcji obrazu podczas ładowania i zarządzania wbudowanych, takich jak buforowanie pomocy technicznej i automatyczną obsługę plików obrazów dla różnych rozdzielczości.  

Poniższy przykład pokazuje, jak ustawić obraz `UITabBarItem` na `UITabBar`:

```csharp
TabBarItem.Image = UIImage.FromBundle ("MyImage");
```

Przy założeniu, że `MyImage` to nazwa zasobu obrazu dodane do katalogu zasobów powyżej. Podczas pracy z katalogu zasobów obrazów, wystarczy określić nazwę zestawu obrazów w `FromBundle` metodę **PNG** sformatowany obrazów:

```csharp
TabBarItem.Image = UIImage.FromBundle ("MyImage");
```

W przypadku innych format obrazu zawiera rozszerzenia o tej nazwie. Na przykład:

```csharp
TabBarItem.Image = UIImage.FromBundle ("MyImage.jpg");
```

Aby uzyskać więcej informacji na temat obrazów i ikon w dokumentacji firmy Apple na [ikon niestandardowych i wskazówki dotyczące tworzenia obrazu](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html).

<a name="Displaying-an-Image-in-a-Storyboards" />

## <a name="displaying-an-image-in-a-storyboards"></a>Wyświetlanie obrazów w Scenorys

Gdy obraz został dodany do projektu platformy Xamarin.iOS przy użyciu katalogi zasobów, można łatwo wyświetlany na scenorysu przy użyciu `UIImageView` w systemie iOS projektanta. Jeśli na przykład dodano następujący zasób obrazu:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](displaying-an-image-images/display01.png "Dodano przykład zasób obrazu")

Wykonaj następujące polecenie, aby wyświetlić je na scenorysu:

1. Kliknij dwukrotnie `Main.storyboard` w pliku **Eksploratora rozwiązań** go otworzyć do edycji w Projektancie z systemem iOS.
2. Wybierz **obrazu widoku** z **przybornika**:

     ![](displaying-an-image-images/display02.png "Wybierz widok obrazu z przybornika")
3. Przeciągnij widoku obrazu na powierzchni projektowej oraz położenie i rozmiar go zgodnie z wymaganiami:

    ![](displaying-an-image-images/display03.png "Nowy widok obrazu na powierzchni projektu")
4. W **elementu Widget** sekcji **Explorer właściwości** wybierz żądaną **obrazu** zasobów do wyświetlenia:

    ![](displaying-an-image-images/display04.png "Wybierz żądany zasób obrazu do wyświetlenia")
5. W **widoku** użyj **tryb** do kontrolowania sposobu obraz będzie zmiany rozmiaru, kiedy **widoku obrazu** rozmiar jest zmieniany.
6. Z **widoku obrazu** zaznaczone, kliknij ją ponownie, aby dodać **ograniczenia**:

    ![](displaying-an-image-images/display05.png "Dodanie ograniczeń")
7. Przeciągnij uchwyt "T" w kształcie na krawędzi każdej **widoku obrazu** do odpowiedniej strony na "numer pin" obrazu do krawędzi ekranu. W ten sposób **widoku obrazu** będzie zmniejszyć rozmiar i rozwój w miarę rozmiaru ekranu.
8. Zapisz zmiany do scenorysu.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](displaying-an-image-images/display01vs.png "Dodano przykład zasób obrazu")

Wykonaj następujące polecenie, aby wyświetlić je na scenorysu:

1. Kliknij dwukrotnie `Main.storyboard` w pliku **Eksploratora rozwiązań** go otworzyć do edycji w Projektancie z systemem iOS.
2. Wybierz **obrazu widoku** z **przybornika**:

     ![](displaying-an-image-images/display02vs.png "Wybierz widok obrazu z przybornika")
3. Przeciągnij widoku obrazu na powierzchni projektowej oraz położenie i rozmiar go zgodnie z wymaganiami:

    ![](displaying-an-image-images/display03vs.png "Nowy widok obrazu na powierzchni projektu")
4. W **elementu Widget** sekcji **Explorer właściwości** wybierz żądaną **obrazu** zasobów do wyświetlenia:

    ![](displaying-an-image-images/display04vs.png "Wybierz żądany zasób obrazu do wyświetlenia")
5. W **widoku** użyj **tryb** do kontrolowania sposobu obraz będzie zmiany rozmiaru, kiedy **widoku obrazu** rozmiar jest zmieniany.
6. Z **widoku obrazu** zaznaczone, kliknij ją ponownie, aby dodać **ograniczenia**:

    ![](displaying-an-image-images/display05vs.png "Dodanie ograniczeń")
7. Przeciągnij uchwyt "T" w kształcie na krawędzi każdej **widoku obrazu** do odpowiedniej strony na "numer pin" obrazu do krawędzi ekranu. W ten sposób **widoku obrazu** będzie zmniejszyć rozmiar i rozwój w miarę rozmiaru ekranu.
8. Zapisz zmiany do scenorysu.

-----


<a name="Displaying-an-Image-in-Code" />

## <a name="displaying-an-image-in-code"></a>Wyświetla obraz w kodzie

Podobnie jak wyświetlanie obrazów w scenorysu, gdy obraz został dodany do projektu platformy Xamarin.iOS przy użyciu katalogi zasobów łatwo wyświetlenia przy użyciu kodu C#.

Wykonaj poniższy przykład:

```csharp
// Create an image view that will fill the
// parent image view and set the image from
// an Image Asset

var imageView = new UIImageView (View.Frame);
imageView.Image = UIImage.FromBundle ("Kemah");

// Add the Image View to the parent view
View.AddSubview (imageView);
```

Ten kod tworzy nową `UIImageView` i nadaje mu początkowy rozmiar i położenie. Następnie ładuje obraz z zasób obrazu dodane do projektu i dodaje `UIImageView` do nadrzędnego `UIView` aby go wyświetlić.

## <a name="related-links"></a>Linki pokrewne

- [Praca z obrazami (przykład)](https://developer.xamarin.com/samples/WorkingWithImages/)
- [Witaj, iPhone](~/ios/get-started/hello-ios/index.md)
- [Niestandardowe ikony, jak i wskazówki dotyczące tworzenia obrazu](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html)
