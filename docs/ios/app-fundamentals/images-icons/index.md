---
title: Obrazów i ikon
description: Ta sekcja zawiera różne artykuły, które obejmują pracy z obrazami w aplikacji platformy Xamarin.iOS, na przykład za pomocą ich jako ikony, uruchom ekrany lub włączenie ich w formantach i umożliwiający ikony typu niestandardowego dokumentu.
ms.prod: xamarin
ms.assetid: 0AB8CC07-11E4-0D75-4119-AED1A1252424
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: fd191c898d5bb015d2d394d42db1049bb0128fb7
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="images-and-icons"></a>Obrazów i ikon

_Ta sekcja zawiera różne artykuły, które obejmują pracy z obrazami w aplikacji platformy Xamarin.iOS, na przykład za pomocą ich jako ikony, uruchom ekrany lub włączenie ich w formantach i umożliwiający ikony typu niestandardowego dokumentu._

Istnieje kilka sposobów tego obrazu, zasoby są używane w aplikacji systemu iOS. Po prostu wyświetlanie obrazów jako część interfejsu użytkownika aplikacji, takich jak przypisywanie do formantu interfejsu użytkownika `UIButton` lub `UIImageView`, by zapewniać ikon i ekranów uruchamiania, Xamarin.iOS ułatwia dodawanie dużą kompozycji do aplikacji systemu iOS w następujący sposób: 

- **Obrazy niezależnie od rozdzielczości** — Użyj wbudowana obsługa systemu iOS na do pracy z obrazami rozwiązania innego urządzenia i typy (iPhone, iPad itp.).
- **Ustawia obraz katalogu zasobów** -użyj **zasobów katalogu obrazu zestawy** do zarządzania i grupy wszystkich wersji trwałego danego obrazu wymagane przez aplikację.
- **Obrazy w systemie iOS projektanta** -Użyj projektanta dla systemu iOS, aby ustawić obrazy dla formantów.
- **Obrazy w kodzie** — użyj `UIImage` metod klasy do ładowania i korzystanie z zasobów obrazu i przypisać je do kontrolek interfejsu użytkownika w kodzie języka C#.
- **Ikona aplikacji** — zdefiniuj ikonę aplikacji wymagane przez wszystkie aplikacje systemu iOS. Jest to ikonę, która będzie wybierz użytkownika z ekranu głównego z systemem iOS można uruchomić aplikacji. Ponadto ta ikona jest stosowana przez Centrum gier, jeśli ma to zastosowanie.
- **Wyróżnione ikona** — zdefiniuj ikona uwagi w aplikacji. Zawsze, gdy użytkownik wprowadza nazwę aplikacji w wyszukiwanie Spotlight, ta ikona jest wyświetlana.
- **Ikona ustawień** — zdefiniuj aplikacji **ustawienia** ikony. Jeśli użytkownik wprowadzi **ustawienia** aplikacji na urządzeniu z systemem iOS, ta ikona pojawi się na końcu listy ustawień dla aplikacji. 
- **Uruchamianie ekrany** — zdefiniuj ekranu uruchamiania aplikacji. Po naciśnięciu ikonę aplikacji i przed zostanie wyświetlony pierwszy widok, pojawi się pusty ekran. Na szczęście iOS obsługuje wyświetlania obrazu zamiast pustego ekranu przy użyciu scenorysu. 
- **Ikona programu iTunes** — Podaj ikona iTune. Jeśli przy użyciu metody Ad Hoc dostarczania aplikacji (zarówno dla użytkowników w firmach i testowania wersji beta w rzeczywistych urządzeń), deweloper również musi zawierać 512 x 512 i obraz 1024 x 1024, który będzie używany do reprezentowania aplikacji w programach iTunes.
- **Dokument ikony** -używany jest obraz jako ikona dla aplikacji platformy Xamarin.iOS obsługuje lub tworzy określonego dokumentu.

Istnieje kilka kwestii, które powinny brane pod uwagę podczas tworzenia obrazu zasobów dla aplikacji systemu iOS, a także kilka miejsc, w których używany będzie tych zasobów. Każdy z tych miała wpływ na nie tylko liczbę zasobów obrazu będzie wymagane, ale sposobu tworzenia tych zasobów. W poniższych tematach znajdziesz typy obrazów zasoby, które będzie wymagane, jak te zasoby są uwzględnione w pakiecie aplikacji i jak są używane zasoby obrazu zapewnienie wymaganej funkcjonalności:


## <a name="displaying-an-imageiosapp-fundamentalsimages-iconsdisplaying-an-imagemd"></a>[Wyświetlanie obrazów](~/ios/app-fundamentals/images-icons/displaying-an-image.md)

W tym artykule opisano w tym zasób obrazu w aplikacji platformy Xamarin.iOS i wyświetlania obrazu przy użyciu kodu C# lub przypisywania go do kontroli w systemie iOS projektanta.

## <a name="application-iconsiosapp-fundamentalsimages-iconsapp-iconsmd"></a>[Ikony aplikacji](~/ios/app-fundamentals/images-icons/app-icons.md)

Ten artykuł obejmuje tym i zarządzanie zasób obrazu w aplikacji platformy Xamarin.iOS ma być używany jako ikonę aplikacji.

## <a name="alternate-app-iconsiosapp-fundamentalsimages-iconsalternate-app-iconsmd"></a>[Alternatywne ikony aplikacji](~/ios/app-fundamentals/images-icons/alternate-app-icons.md)

Apple został dodany kilka ulepszeń w systemach iOS 10.3 Zezwalaj aplikacji na zarządzanie jego ikonę:

 - `ApplicationIconBadgeNumber` -Pobiera lub ustawia identyfikator ikony aplikacji w Springboard.
 - `SupportsAlternateIcons` -Jeśli `true` aplikacja ma inny zestaw ikon.
 - `AlternateIconName` -Zwraca nazwę alternatywnego ikony aktualnie wybrane lub `null` Jeśli za pomocą ikony podstawowego.
 - `SetAlternameIconName` — Użyj tej metody, aby przełączyć się ikona aplikacji do danego ikona alternatywny.


## <a name="launch-screensiosapp-fundamentalsimages-iconslaunch-screensmd"></a>[Ekrany uruchamiania](~/ios/app-fundamentals/images-icons/launch-screens.md)

W tym artykule omówiono przy użyciu specjalnego scenorysu zapewnienie uniwersalnych ekranu uruchamiania dla każdego rozmiar urządzenia z systemem iOS i rozdzielczość.

## <a name="custom-document-typesiosapp-fundamentalsimages-iconscustom-document-typesmd"></a>[Niestandardowe typy dokumentów](~/ios/app-fundamentals/images-icons/custom-document-types.md)

Ten artykuł obejmuje tym i zarządzanie zasób obrazu w aplikacji platformy Xamarin.iOS do użycia jako ikona niestandardowy typ dokumentu.



## <a name="related-links"></a>Linki pokrewne

- [Praca z obrazami (przykład)](https://developer.xamarin.com/samples/WorkingWithImages/)
- [Witaj, iPhone](~/ios/get-started/hello-ios/index.md)
- [Niestandardowe ikony, jak i wskazówki dotyczące tworzenia obrazu](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html)
