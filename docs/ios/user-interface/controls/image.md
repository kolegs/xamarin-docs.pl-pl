---
title: Wyświetlanie obrazów
ms.prod: xamarin
ms.assetid: 67CA8DB6-769D-42BB-A137-3AF933789FE1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 406cfe813cbb58111769203f3b6c3fb0c2edad3c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="displaying-images"></a>Wyświetlanie obrazów

Dodawanie obrazów do aplikacji wymaga wykonania dwóch kroków: najpierw dodaj go do projektu. następnie dodaj formanty i kod umożliwiający ich wyświetlenie na ekranie. Zapoznaj się [Praca z obrazami](~/ios/app-fundamentals/images-icons/index.md) artykuł bardziej szczegółowe zakres obsługi w Xamarin.iOS obrazu.

## <a name="adding-images-to-your-app"></a>Dodawanie obrazów do aplikacji

Obrazy można dodać do dowolnego folderu w Visual Studio dla rozwiązania Mac i w razie **Akcja kompilacji** ustawiono **zawartości** , a następnie plik zostanie uwzględniony w aplikacji i może zostać wyświetlony.

Visual Studio for Mac obsługuje również specjalne katalog o nazwie zasoby, które mogą także zawierać pliki obrazów. Pliki w folderze Zasoby powinny mieć **Akcja kompilacji** ustawioną **BundleResource**.

Ten zrzut ekranu przedstawia **Akcja kompilacji** opcje, które są wyświetlane, gdy plik jest klikniętego:

 [![](image-images/image30a.png "Tworzenie menu Akcja")](image-images/image30a.png#lightbox)

Visual Studio for Mac będzie zazwyczaj wybierz poprawny **Akcja kompilacji** automatycznie, ale należy zwrócić uwagę, te ustawienia, zwłaszcza, jeśli można przenosić pliki w projekcie.

### <a name="adding-an-image-file"></a>Dodawanie pliku obrazu

Aby dodać plik do projektu, najpierw kliknij prawym przyciskiem myszy projekt i wybierz polecenie **Dodawanie plików...**

 [![](image-images/image31a.png "Dodaj pliki menu")](image-images/image31a.png#lightbox)

Wybierz obraz (lub obrazów) mają zostać uwzględnione w oknie dialogowym standardowego pliku. Wartość domyślna akcja kompilacji dla obrazów będą **BundleResource** — nie zastąpienie tej wartości, chyba że masz powód.

 [![](image-images/image32a.png "Dodaj pliki w oknie dialogowym")](image-images/image32a.png#lightbox)

Obraz zostanie dodany do projektu i jest dostępny dla załadowana i wyświetlona w kodzie. Ten zrzut ekranu przedstawia obraz dodany do projektu aplikacji systemu iOS:

 [![](image-images/image33a.png "Obraz w projekcie")](image-images/image33a.png#lightbox)

### <a name="what-is-the-resources-directory"></a>Co to jest katalog zasobów?

Pliki umieszczane w katalogu zasobów są traktowane inaczej z zwykłe pliki — zawartość folderu zasobów są kopiowane do katalogu głównego aplikacji i może być przywoływany z tej lokalizacji w kodzie. Może to być przydatne dla wielu powodów:

-  Zapisywanie obrazów skonfigurowane we właściwościach aplikacji, takich jak domyślne obrazy rozruchu i ikony aplikacji.
-  Przechowywanie innych obrazów i plików, niezależnie od kodu, więc są łatwiejsze w zarządzaniu (podkatalogi są zachowywane podczas kopiowania zawartości katalogu zasobów).


Katalog zasobów jest szczególnie przydatna w projektu biblioteki, ponieważ kod można założyć, że te obrazy zostaną skopiowane do katalogu głównego aplikacja odbierająca komunikaty, co ułatwia zapisu biblioteki udostępnionej kodu, wymagających obraz, dźwięk, wideo, XML lub innych pliki.



Katalog zasobów musi mieć nazwę tak, a wszystkie pliki powinny mieć Akcja kompilacji ustawioną na **BundleResource**

## <a name="displaying-the-image"></a>Wyświetlanie obrazów

Aby wyświetlić obraz przy użyciu narzędzia Projektant, widok obrazu powinna być używana jako kontener i jeden obraz lub animacji obrazów można wyświetlić. **Widoku obrazu** ikonę z przybornika przedstawiono poniżej:

 [![](image-images/image35a.png "ImageView w przyborniku")](image-images/image35.png#lightbox)

Przeciągnij **obrazu widoku** z **Toobox** na kontroler widoku. Następnie w obszarze ** widoku obrazu > Obraz ** listy rozwijanej będzie zawierają listę wszystkich dostępnych obrazów pliki w projekcie. Wybierz jedno z nich, aby dodać je do obrazu.

 [![](image-images/image36a.png "ImageView w przyborniku")](image-images/image36.png#lightbox)

### <a name="displaying-the-image-programmatically"></a>Programowo wyświetlania obrazu

Ponieważ blocks.jpg znajduje się w katalogu głównego katalogu zasobów będzie dostępny w czasie wykonywania w katalogu głównym pakietu aplikacji. Aby wyświetlić ten obraz w ImageView kontroli użyć poniższego kodu:

```csharp
imageview1.Image = UIImage.FromBundle ("SF Monkey.png");
```

Jeśli firma Microsoft umieścił obrazu w `/Resources/Pics/blocks.jpg` , a następnie kod obejmuje Pics folder w ścieżce:

```csharp
imageview1.Image = UIImage.FromBundle ("Pics/SF Monkey.png");
```

Plik zasobów odwołuje się nigdy nie trzeba wpisywać `Resources` folderu.


## <a name="related-links"></a>Linki pokrewne

- [Formanty (przykład)](https://developer.xamarin.com/samples/Controls/)
