---
title: Wyświetlanie obrazów za pomocą rozszerzenia Xamarin.iOS
description: Ten dokument zawiera opis sposobu wyświetlania obrazów w rozszerzeniu Xamarin.iOS. Obejmuje ona Dodawanie obrazów do aplikacji, programowo lub za pomocą narzędzia iOS Designer.
ms.prod: xamarin
ms.assetid: 67CA8DB6-769D-42BB-A137-3AF933789FE1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/13/2018
ms.openlocfilehash: 9777b4abf6e7f370178bcff2cb40666612888a9f
ms.sourcegitcommit: cb80df345795989528e9df78eea8a5b45d45f308
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39038381"
---
# <a name="displaying-images-with-xamarinios"></a>Wyświetlanie obrazów za pomocą rozszerzenia Xamarin.iOS

Dodawanie obrazów do swojej aplikacji wymaga dwóch kroków: najpierw Dodaj obrazów do projektu. następnie dodaj formanty i kod, aby wyświetlić je na ekranie. Zapoznaj się [Praca z obrazami](~/ios/app-fundamentals/images-icons/index.md) artykuł, aby uzyskać bardziej szczegółowe pokrycia obrazu, obsługa w rozszerzeniu Xamarin.iOS.

## <a name="adding-images-to-your-app"></a>Dodawanie obrazów do swojej aplikacji

Obrazy można dodać do dowolnego folderu programu Visual Studio dla komputerów Mac rozwiązania i jeśli **Build Action** jest ustawiona na **zawartości** , a następnie plik zostanie on uwzględniony razem z aplikacją i mogą być wyświetlane.

Program Visual Studio for Mac obsługuje również specjalne katalog o nazwie **zasobów** , może również zawierać pliki obrazów. Pliki w folderze zasobów powinny mieć **Build Action** równa **BundleResource**.

Ten zrzut ekranu przedstawia **Build Action** opcje, które są wyświetlane, gdy plik jest klikniętego prawym przyciskiem myszy:

 [![](image-images/image30a.png "Tworzenie menu akcji")](image-images/image30a.png#lightbox)

Visual Studio dla komputerów Mac będą w większości przypadków wybrać poprawny **Build Action** automatycznie, ale należy pamiętać o tych ustawieniach, zwłaszcza, jeśli możesz przenosić pliki w projekcie.

### <a name="adding-an-image-file"></a>Dodawanie pliku obrazu

Aby dodać plik do projektu, najpierw kliknij prawym przyciskiem myszy projekt i wybierz polecenie **Dodawanie plików...**

 [![](image-images/image31a.png "Dodaj pliki menu")](image-images/image31a.png#lightbox)

Wybierz obraz (lub obrazów) mają zostać uwzględnione w oknie dialogowym standardowego pliku. Wartość domyślna akcja kompilacji, aby obrazy będą **BundleResource** — nie zastąpić tę wartość, chyba że masz powód.

 [![](image-images/image32a.png "Dodawanie okna dialogowego plików")](image-images/image32a.png#lightbox)

Obraz, który zostanie dodany do projektu i dostępne do ładowania i wyświetlane w kodzie. Ten zrzut ekranu przedstawia obrazu, który został dodany do projektu aplikacji systemu iOS:

 [![](image-images/image33a.png "Obraz w projekcie")](image-images/image33a.png#lightbox)

### <a name="what-is-the-resources-directory"></a>Co to jest katalog zasobów?

Pliki umieszczone w **zasobów** katalogu są traktowane inaczej od zwykłe pliki — zawartość **zasobów** folderu są kopiowane do katalogu głównego aplikacji i mogą być przywoływane z tego miejsca w Kod. Może to być przydatne w wielu powodów:

-  Przechowywanie obrazów skonfigurowany we właściwościach aplikacji, takich jak domyślne uruchamiania obrazy i ikony aplikacji.
-  Przechowywanie inne obrazy i pliki oddzielnie od kodu, więc są one łatwiejsze w zarządzaniu (podkatalogi są zachowywane w przypadku kopiowania zawartości katalogu zasobów).


**Zasobów** katalog jest szczególnie przydatne w projekcie biblioteki, ponieważ kod, można założyć, że te obrazy zostaną skopiowane do katalogu głównego aplikacja odbierająca komunikaty, dzięki czemu łatwiej jest zapisu udostępnionych bibliotek kodu, które wymagają obraz, dźwięk, wideo, XML lub innych plików.

**Zasobów** katalogu musi mieć więc nazwę, a wszystkie pliki powinny mieć ustawienie akcji kompilacji **BundleResource**.

## <a name="displaying-the-image"></a>Wyświetlanie obrazu

W narzędziu iOS Designer, należy użyć **widoku obrazu** do wyświetlania obrazu lub animowany serii obrazów. **Widoku obrazu** ikonę z przybornika znajdują się poniżej:

 [![](image-images/image35a.png "ImageView w przyborniku")](image-images/image35.png#lightbox)

Przeciągnij **widoku obrazu** z **przybornika** na kontroler widoku. Następnie w obszarze **widoku obrazu > obraz** listy rozwijanej będzie podać listę wszystkich plików obrazów dostępnych w projekcie. Wybierz dowolny z nich, aby dodać je do obrazu.

 [![](image-images/image36a.png "ImageView w przyborniku")](image-images/image36.png#lightbox)

### <a name="displaying-the-image-programmatically"></a>Programowe wyświetlanie obrazu

Ponieważ **SF Monkey.jpg** znajduje się w folderze głównym **zasobów** katalogu będzie on dostępny w czasie wykonywania w katalogu głównym pakietu aplikacji. Aby wyświetlić ten obraz w kontrolce widok obrazu, użyj następującego kodu:

```csharp
imageview1.Image = UIImage.FromBundle("SF Monkey.png");
```

Firma Microsoft ma umieszczenie obrazu w **Monkey.jpg SF-zasobów/Pics**, wówczas zawierałoby kod **Pics** folderu na ścieżce:

```csharp
imageview1.Image = UIImage.FromBundle("Pics/SF Monkey.png");
```

Plik zasobów odwołuje się do nigdy nie trzeba wpisywać **zasobów** folderu.

## <a name="related-links"></a>Linki pokrewne

- [Formanty (przykład)](https://developer.xamarin.com/samples/Controls/)
