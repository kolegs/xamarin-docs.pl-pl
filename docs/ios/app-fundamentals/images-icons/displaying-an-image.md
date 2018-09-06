---
title: Wyświetlanie obrazów w rozszerzeniu Xamarin.iOS
description: W tym artykule opisano w tym zasób obrazu w aplikacji platformy Xamarin.iOS i wyświetlanie obrazu, przy użyciu kodu C# lub, przypisując go do kontroli w narzędziu iOS Designer.
ms.prod: xamarin
ms.assetid: 60288B12-49E3-4E87-8690-D04A5EC7A664
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/24/2018
ms.openlocfilehash: 4b2bddeb6b04b5c5288f501fce0d6bb03e0b6584
ms.sourcegitcommit: e64c3c10d6a36b3b031d6d4dbff7af74ab2b7f21
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/10/2018
ms.locfileid: "43780625"
---
# <a name="displaying-an-image-in-xamarinios"></a>Wyświetlanie obrazów w rozszerzeniu Xamarin.iOS

_W tym artykule opisano w tym zasób obrazu w aplikacji platformy Xamarin.iOS i wyświetlanie obrazu, przy użyciu kodu C# lub, przypisując go do kontroli w narzędziu iOS Designer._

## <a name="adding-and-organizing-images-in-a-xamarinios-app"></a>Dodawanie i organizowania obrazów w aplikacji platformy Xamarin.iOS

Podczas dodawania obrazu do użycia w aplikacji platformy Xamarin.iOS, deweloper będzie używać _wykaz zasobów_ do obsługi każdego urządzenia z systemem iOS i rozdzielczości wymagane przez aplikację.

Dodano w systemie iOS 7, **zestawów obrazu wykazów zasobów** zawiera wszystkie wersje lub reprezentacje obrazu, które są niezbędne do obsługi różnych urządzeń i skalowanie czynników dla aplikacji. Zamiast polegania na nazwę pliku zasobów obrazu (zobacz [nomenklatury obrazu i obrazów niezależnie od rozdzielczości](~/ios/app-fundamentals/images-icons/displaying-an-image.md)), **zestawów obrazu** użyć pliku Json, aby określić, który obraz należy do urządzenia i/lub rozwiązania . Jest to preferowany sposób zarządzania i obsługi obrazów w systemie iOS (z systemem iOS 9 lub nowsza).

## <a name="adding-images-to-an-asset-catalog-image-set"></a>Dodawanie obrazów do obrazu wykazu zasobów zestawu

Jak wspomniano powyżej, **zestawów obrazu wykazów zasobów** zawiera wszystkie wersje lub reprezentacje obrazu, które są niezbędne do obsługi różnych urządzeń i skalowanie czynników dla aplikacji. Zamiast polegania na Nazwa pliku zasobów obrazu **zestawów obrazu** użyć pliku Json, aby określić, który obraz należy do urządzenia i/lub rozwiązania.

Aby utworzyć nowy zestaw obrazów i dodać do niej obrazy, wykonaj następujące czynności:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. W **Eksploratora rozwiązań**, kliknij dwukrotnie `Assets.xcassets` plik, aby otworzyć do edycji:

    ![](displaying-an-image-images/imageset01.png "Assets.xcassets w Eksploratorze rozwiązań")
2. Kliknij prawym przyciskiem myszy **listy Assets** i wybierz **nowy zestaw obrazów**:

    ![](displaying-an-image-images/imageset02.png "Dodawanie nowy zestaw obrazów")
3. Wybierz nowy zestaw obrazów i pojawi się Edytor:

    ![](displaying-an-image-images/imageset03.png "Ustaw obraz edytora")
4. W tym miejscu, przeciągnij w obrazach, dla każdego z różnych urządzeń i i rozdzielczości wymagane. 
5. Kliknij dwukrotnie nowy zestaw obrazów **nazwa** w **listy Assets** go edytować: ![](displaying-an-image-images/imageset04.png "edytowanie nazwy nowy zestaw obrazów")

Korzystając z **obrazu zestawie** w narzędziu iOS Designer, po prostu wybierz nazwę zestawu z listy rozwijanej w edytorze właściwości:

![](displaying-an-image-images/imageset06.png "Wybierz nazwę zestawu obrazów z listy rozwijanej")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Otwórz katalog zasobów z **Eksploratora rozwiązań**, a w lewym górnym rogu kliknij **oraz** przycisku:

    ![](displaying-an-image-images/asset5.png "Kliknij przycisk")

2. Wybierz **Dodawanie zestawu obrazów** edytora obrazu zestawie pojawi się nowy zestaw obrazów. W tym miejscu, przeciągnij w obrazach, dla każdego z różnych urządzeń i i rozdzielczości wymagane. 

    ![](displaying-an-image-images/asset7.png "Edytor zestawu obrazów")

### <a name="renaming-an-image-set"></a>Zmiana nazwy zestaw obrazów

Aby zmienić nazwę obrazu, w zestawie, wykonaj następujące czynności:

1. W **Eksploratora rozwiązań**, kliknij dwukrotnie **wykaz zasobów** plik, aby otworzyć do edycji:

    ![](displaying-an-image-images/rename01.png "Katalog zasobów, w Eksploratorze rozwiązań")
2. Wybierz **obrazu zestawie** można zmienić nazwy:

    ![](displaying-an-image-images/rename02.png "Wybierz obraz zestawu można zmienić nazwy")
3. W **Eksplorator właściwości**, przewiń w dół i wybierz **nazwa**(w obszarze **różne** sekcji):

    ![](displaying-an-image-images/rename03.png "Wybierz nazwę sekcji różne")
4. Wprowadź nową **nazwa** dla **obrazu zestawie** i Zapisz zmiany.

-----

Korzystając z **obrazu zestawie** w kodzie, odwołaj się do niego według nazwy, wywołując `FromBundle` metody `UIImage` klasy. Na przykład:

```csharp
MonkeyImage.Image = UIImage.FromBundle ("PurpleMonkey");
```

> [!IMPORTANT]
> Jeśli obrazy przypisane do obrazu, w zestawie nie są poprawnie wyświetlane, upewnij się, że poprawna nazwa pliku jest używana z `FromBundle` — metoda ( **obrazu zestawie** i nie nadrzędnym **wykaz zasobów** nazwy). W przypadku obrazów PNG `.png` rozszerzenia można pominąć. W przypadku innych formatów obrazów rozszerzenie jest wymagany (np.) `PurpleMonkey.jpg`).

### <a name="using-vector-images-in-asset-catalogs"></a>W wykazach zasobów przy użyciu obrazy wektorowe

Począwszy od systemu iOS jest 8 specjalne **wektor** klasy, ponieważ została dodana do **zestawów obrazu** umożliwiająca dla deweloperów uwzględnić **PDF** obraz wektor kasety zamiast tego w tym formacie mapy bitowej poszczególnych plików w różnych rozdzielczościach. Za pomocą tej metody, podaj plik pojedynczego wektora `@1x` rozwiązania (w formacie pliku PDF wektor) i `@2x` i `@3x` wersji pliku zostanie wygenerowany w czasie kompilacji i zawartych w pakiecie aplikacji.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](displaying-an-image-images/imageset05.png "Obrazy wektorowe w edytorze wykazy zasobów")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](displaying-an-image-images/asset8.png "Obrazy wektorowe w edytorze wykazy zasobów")

-----

Na przykład, jeśli zawiera Deweloper `MonkeyIcon.pdf` pliku jako wektor katalog zasobów z rozdzielczością x 150px 150px, następujące zasoby będą objęte pakietu ostatecznej aplikacji podczas został skompilowany mapy bitowej:

- `MonkeyIcon@1x.png` -150px x 150px rozwiązania.
- `MonkeyIcon@2x.png` -300 pikseli x 300px rozwiązania.
- `MonkeyIcon@3x.png` -450px x 450px rozwiązania.

Następujące czynności powinny należy brać pod uwagę podczas korzystania z plików PDF Obrazy wektorowe w wykazy zasobów:

- Nie jest obsługa pełnego wektora jako plik PDF będzie rasteryzowany do mapy bitowej w czasie kompilacji i mapy bitowe, w gotowych aplikacji.
- Nie można go zmienić rozmiar obrazu, gdy jest ono ustawione w katalogu zasobów. Jeśli Deweloper próbuje zmienić rozmiar obrazu (lub w kodzie za pomocą automatycznego układu oraz klasy rozmiaru) obraz będzie zniekształcony podobnie jak inne mapy bitowej.
- Wykazy zasobów są tylko zgodne z systemem iOS 7 lub nowszym, jeśli aplikacja musi obsługiwać system iOS 6 lub niższy, nie można użyć wykazów zasobów.

## <a name="working-with-template-images"></a>Praca z obrazami szablonu

Oparte na projekt aplikacji systemu iOS, możliwe czasy, kiedy programiście Dostosowywanie ikony lub obrazu w interfejsie użytkownika, aby dopasować zmiany w schemacie kolorów (na przykład, w oparciu o preferencje użytkowników).

Aby łatwo uzyskać ten efekt, Przełącz _tryb renderowania_ elementu zawartości obrazu do **obrazu szablonu**:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](displaying-an-image-images/templateimage01.png "Ustawiony tryb renderowania obrazu szablonu")](displaying-an-image-images/templateimage01.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](displaying-an-image-images/templateimage01vs.png "Tryb renderowania ustawiony jako szablon")](displaying-an-image-images/templateimage01vs.png#lightbox)

-----

W narzędziu iOS Designer, przypisz zasób obrazu na kontrolkę interfejsu użytkownika, a następnie ustaw **odcień** kolorować obrazu:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](displaying-an-image-images/templateimage03.png "Ustaw odcień kolorować obrazu")](displaying-an-image-images/templateimage03.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](displaying-an-image-images/templateimage03vs.png "Ustaw odcień kolorować obrazu")](displaying-an-image-images/templateimage03vs.png#lightbox)

-----

Opcjonalnie zasób obrazu i odcień można ustawić bezpośrednio w kodzie:

```csharp
MyIcon.Image = UIImage.FromBundle ("MessageIcon");
MyIcon.TintColor = UIColor.Red;
```

Aby użyć obrazu szablonu, całkowicie w kodzie, wykonaj następujące czynności:

```csharp
if (MyIcon.Image != null) {
    var mutableImage = MyIcon.Image.ImageWithRenderingMode (UIImageRenderingMode.AlwaysTemplate);
    MyIcon.Image = mutableImage;
    MyIcon.TintColor = UIColor.Red;
}
```

Ponieważ `RenderMode` właściwość `UIImage` jest tylko do odczytu, użyj `ImageWithRenderingMode` metodę, aby utworzyć nowe wystąpienie obrazu z ustawieniem żądany tryb renderowania.

Dostępne są trzy prawdopodobnie ustawienia `UIImage.RenderMode` za pośrednictwem `UIImageRenderingMode` wyliczenia:

* `AlwaysOriginal` -Wymusza obraz, który ma być renderowany jako oryginalnego pliku obrazu źródłowego bez wprowadzania żadnych zmian.
* `AlwaysTemplate` -Wymusza obraz, który ma być renderowana jako obraz szablonu przez kolorowanie pikseli z określonym `Tint` kolorów.
* `Automatic` -Ponieważ szablon lub oryginalny oparte na środowisko, w którym jest używana w albo renderuje obraz. Na przykład, jeśli obraz, który jest używany w `UIToolBar`, `UINavigationBar`, `UITabBar` lub `UISegmentControl` są traktowani jako szablon.

## <a name="adding-new-assets-collections"></a>Dodawanie nowych kolekcji zasobów

Podczas pracy z obrazów w wykazach zasoby mogą być czasy, kiedy nowa kolekcja będzie wymagane, zamiast opcji dodawania wszystkich aplikacji obrazów, aby `Assets.xcassets` kolekcji. Na przykład podczas projektowania zasoby na żądanie.

Aby dodać nowy katalog zasobów do projektu:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Kliknij prawym przyciskiem myszy **Nazwa projektu** w **Eksploratora rozwiązań** i wybierz **Dodaj** > **nowy plik...**
2. Wybierz **iOS** > **wykaz zasobów**, wprowadź **nazwa** kolekcji, a następnie kliknij przycisk **New** przycisku:

    ![](displaying-an-image-images/asset01.png "Tworzenie nowego katalogu zasobów")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **wykazy zasobów** folder, a następnie wybierz **Dodaj > Nowy katalog zasobów**.
2. Nadaj jej nazwę, a następnie kliknij przycisk **Dodaj**:

    ![](displaying-an-image-images/asset1.png "Tworzenie nowego katalogu zasobów")

-----

W tym miejscu można pracowano kolekcji w taki sam sposób jak domyślne `Assets.xcassets` kolekcji automatycznie uwzględnione w projekcie.

## <a name="using-images-with-controls"></a>Przy użyciu obrazów z formantami

Oprócz przy użyciu obrazów w celu zapewnienia obsługi aplikacji, z systemem iOS korzysta z obrazów za pomocą typów kontroli aplikacji, takich jak paski kart, paski narzędzi, paski nawigacji, tabele i przyciski. Prostym sposobem utworzenia obrazu są wyświetlane w kontrolce jest przypisanie `UIImage` wystąpienie formantu `Image` właściwości.

### <a name="frombundle"></a>FromBundle

`FromBundle` Wywołania metody jest synchroniczne wywołanie (blokowanie), które ma wiele funkcji obrazu podczas ładowania i zarządzania wbudowanych, takich jak buforowanie, pomoc techniczna i automatyczną obsługę plików obrazów dla różnych rozdzielczości.  

Poniższy przykład pokazuje, jak ustawić obraz `UITabBarItem` na `UITabBar`:

```csharp
TabBarItem.Image = UIImage.FromBundle ("MyImage");
```

Przy założeniu, że `MyImage` to nazwa zasobu obrazu dodane do katalogu zasobów powyżej. Podczas pracy z katalogu zasobów obrazów, wystarczy określić nazwę zestawu obrazów w `FromBundle` metodę **PNG** sformatowane obrazów:

```csharp
TabBarItem.Image = UIImage.FromBundle ("MyImage");
```

W przypadku innych formatów obrazu zawiera rozszerzenia o tej nazwie. Na przykład:

```csharp
TabBarItem.Image = UIImage.FromBundle ("MyImage.jpg");
```

Aby uzyskać więcej informacji na temat obrazów i ikon, zobacz dokumentację firmy Apple na [Niestandardowa ikona i wytyczne dotyczące tworzenia obrazu](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html).

## <a name="displaying-an-image-in-a-storyboards"></a>Wyświetlanie obrazów w scenorysów

Po dodaniu projektu platformy Xamarin.iOS przy użyciu wykazy zasobów obrazu można łatwo wyświetlane przy użyciu scenorysu `UIImageView` w narzędziu iOS Designer. Na przykład, jeśli dodano następujący zasób obrazu:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](displaying-an-image-images/display01.png "Dodano przykład zasób obrazu")

Wykonaj następujące polecenie, aby wyświetlić go na scenorysu:

1. Kliknij dwukrotnie `Main.storyboard` w pliku **Eksploratora rozwiązań** aby go otworzyć do edycji w narzędziu iOS Designer.
2. Wybierz **obraz widoku** z **przybornika**:

     ![](displaying-an-image-images/display02.png "Wybierz widok obrazu z przybornika")
3. Przeciągnij Wyświetl obraz na powierzchni projektowej i położenie i rozmiar go zgodnie z wymaganiami:

    ![](displaying-an-image-images/display03.png "Nowy widok obrazu na powierzchni projektowej")
4. W **widżet** części **Eksplorator właściwości** wybierz żądany **obraz** zasobów do wyświetlenia:

    ![](displaying-an-image-images/display04.png "Wybierz żądany zasób obrazu do wyświetlenia")
5. W **widoku** sekcji **tryb** do kontrolowania, jak obraz, który będzie zmiany rozmiaru, kiedy **widoku obrazu** zmiany rozmiaru.
6. Za pomocą **widoku obrazu** zaznaczone, kliknij go ponownie, aby dodać **ograniczenia**:

    ![](displaying-an-image-images/display05.png "Dodawanie ograniczenia")
7. Przeciągnij uchwyt "T" w kształcie na każdej krawędzi **widoku obrazu** odpowiedniego boku ekranu, aby obraz, który ma boki "przypinając". W ten sposób **widoku obrazu** będzie zmniejszać i powiększać wraz ze zmienionym rozmiarem ekranu.
8. Zapisz zmiany do scenorysu.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](displaying-an-image-images/display01vs.png "Dodano przykład zasób obrazu")

Wykonaj następujące polecenie, aby wyświetlić go na scenorysu:

1. Kliknij dwukrotnie `Main.storyboard` w pliku **Eksploratora rozwiązań** aby go otworzyć do edycji w narzędziu iOS Designer.
2. Wybierz **obraz widoku** z **przybornika**:

     ![](displaying-an-image-images/display02vs.png "Wybierz widok obrazu z przybornika")
3. Przeciągnij Wyświetl obraz na powierzchni projektowej i położenie i rozmiar go zgodnie z wymaganiami:

    ![](displaying-an-image-images/display03vs.png "Nowy widok obrazu na powierzchni projektowej")
4. W **widżet** części **Eksplorator właściwości** wybierz żądany **obraz** zasobów do wyświetlenia:

    ![](displaying-an-image-images/display04vs.png "Wybierz żądany zasób obrazu do wyświetlenia")
5. W **widoku** sekcji **tryb** do kontrolowania, jak obraz, który będzie zmiany rozmiaru, kiedy **widoku obrazu** zmiany rozmiaru.
6. Za pomocą **widoku obrazu** zaznaczone, kliknij go ponownie, aby dodać **ograniczenia**:

    ![](displaying-an-image-images/display05vs.png "Dodawanie ograniczenia")
7. Przeciągnij uchwyt "T" w kształcie na każdej krawędzi **widoku obrazu** odpowiedniego boku ekranu, aby obraz, który ma boki "przypinając". W ten sposób **widoku obrazu** będzie zmniejszać i powiększać wraz ze zmienionym rozmiarem ekranu.
8. Zapisz zmiany do scenorysu.

-----

## <a name="displaying-an-image-in-code"></a>Wyświetlanie obrazów w kodzie

Podobnie jak wyświetlanie obrazów w scenorysu, gdy obraz został dodany do projektu Xamarin.iOS przy użyciu wykazy zasobów go mogą być łatwo wyświetlane przy użyciu kodu C#.

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

Ten kod tworzy nową `UIImageView` i nadaje jej początkowy rozmiar i położenie. A następnie ładuje obraz z zasobu obrazu dodane do projektu i dodaje `UIImageView` do elementu nadrzędnego `UIView` aby go wyświetlić.

## <a name="related-links"></a>Linki pokrewne

- [Praca z obrazami (przykład)](https://developer.xamarin.com/samples/WorkingWithImages/)
- [Witaj, telefonu iPhone](~/ios/get-started/hello-ios/index.md)
- [Rozmiar obrazu i rozdzielczości (Apple)](https://developer.apple.com/design/human-interface-guidelines/ios/icons-and-images/image-size-and-resolution/)
