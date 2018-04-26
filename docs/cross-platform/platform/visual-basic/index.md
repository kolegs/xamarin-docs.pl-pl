---
title: Przenośne języku Visual
description: W tym przewodniku wyjaśniono, jak Visual Basic może służyć do zapisu projektów przenośnej biblioteki klasy (PCL), które mogą być używane w rozwiązaniach przeznaczonych dla platformy Xamarin.iOS i platformy Xamarin.Android.
ms.prod: xamarin
ms.assetid: f264c632-8feb-4015-a5e5-cb9c681c787d
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: f37ff8a2d07681ba8e4ec3ed6ad7e5bbd85e6502
ms.sourcegitcommit: dc882e9631b4ed52596b944a6fbbdde309346943
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/26/2018
---
# <a name="portable-visual-basicnet"></a>Przenośne języku Visual

Xamarin iOS i Android projektów nie obsługują natywnie Visual Basic; Jednak deweloperzy mogą używać przenośnej biblioteki klas, aby przeprowadzić migrację istniejącego kodu języka Visual Basic z systemami iOS i Android lub do zapisu znaczna część ich logiki aplikacji w języku Visual Basic. Aplikacje platformy Xamarin.Forms mogą być tworzone wyłącznie w języku Visual Basic (z wyjątkiem niestandardowe moduły renderowania, zależności usług i plik codebehind XAML).

## <a name="requirements"></a>Wymagania

Obsługa bibliotek klas przenośnych został dodany w Xamarin.Android 4.10.1, Xamarin.iOS 7.0.4 i Xamarin Studio 4.2, co oznacza, że wszystkie projekty Xamarin utworzone za pomocą tych narzędzi można zastosować zestawy PCL Visual Basic.

Aby utworzyć i skompilować przenośnej biblioteki klas języka Visual Basic należy użyć programu Visual Studio w systemie Windows (Visual Studio 2012 lub nowsza).

> [!NOTE]
> Biblioteki języka Visual Basic można tworzyć tylko i skompilowana przy użyciu programu Visual Studio. Xamarin.iOS i Xamarin.Android nie obsługują język Visual Basic.
>
> Jeśli działa wyłącznie w programie Visual Studio może odwoływać się projekt Visual Basic z projektów platformy Xamarin.iOS i platformy Xamarin.Android.
>
> Jeśli projektów dla systemu Android i iOS musi także zostać załadowany w programie Visual Studio dla komputerów Mac należy odwoływać się do zestawu danych wyjściowych z PCL Visual Basic.


## <a name="creating-a-visual-basicnet-pcl"></a>Tworzenie Visual PCL języku

W tej sekcji przedstawiono sposób tworzenia biblioteki klas przenośnych Visual Basic przy użyciu programu Visual Studio.
Biblioteki można następnie odwoływać się do innych projektów, w tym aplikacji platformy Xamarin.iOS, Xamarin.Android i platformy Xamarin.Forms.

### <a name="creating-a-pcl"></a>Tworzenie PCL

Podczas dodawania PCL Visual Basic w programie Visual Studio należy wybrać profil, który opisano, jakie platformy powinna być zgodna z biblioteki. Profile opisano we wprowadzeniu do PCL dokumentu.

Kroki tworzenia PCL i wybranie jego profilu są:

1.  W **nowy projekt** ekranu wybierz **Visual Basic > biblioteki klas (przenośna)** opcji:

    [![](images/image1-sml.png "Utwórz nową bibliotekę przenośne Visual Basic")](images/image1.png#lightbox)

1.  Visual Studio natychmiast monit z następującymi **dodać przenośnej biblioteki klas** okna dialogowego, dzięki czemu można skonfigurować profil. Znaczników platformy wymagane do obsługi i naciśnij klawisz **OK**.

    [![](images/image2-sml.png "Wybierz profil PCL, wybierając platformy")](images/image2.png#lightbox)

1.  Projekt Visual Basic PCL pojawi się jak pokazano w **Eksploratora rozwiązań** podobnie do następującej:

    [![](images/image3-sml.png "Pusty projekt programu Visual Studio PCL")](images/image3.png#lightbox)


PCL jest teraz gotowy do kodu języka Visual Basic do dodania. Projekty PCL może odwoływać się inne projekty (projekty aplikacji, projekty bibliotek i nawet innych projektów PCL).

### <a name="editing-the-pcl-profile"></a>Edytowanie profilu PCL

Profil PCL (która kontroluje, które platformy PCL jest zgodny z) można wyświetlać i zmieniony przez kliknięcie prawym przyciskiem myszy projekt i wybierając polecenie **właściwości > Biblioteka > zmian...** . Wynikowa okna dialogowego jest wyświetlany w tym zrzut ekranu:

 [![](images/image4-sml.png "Edytuj profil PCL we właściwościach projektu")](images/image4.png#lightbox)

Zmiana profilu po kodu został już dodany do PCL jest możliwe, czy biblioteki już zostanie skompilowany, jeśli kod odwołuje się do funkcji, które nie są częścią nowo wybranego profilu.


## <a name="summary"></a>Podsumowanie

W tym artykule wykazała jak korzystać z kodu języka Visual Basic w aplikacjach platformy Xamarin.IOS przy użyciu programu Visual Studio i przenośnej biblioteki klas. Mimo że Xamarin nie obsługuje bezpośredniego Visual Basic, kompilowania kodu Visual Basic w PCL umożliwia kod napisany w języku Visual Basic do uwzględnienia w systemów iOS i Android aplikacje.

Następujące strony opisano sposób korzystania z Visual PCLs języku w aplikacji platformy Xamarin.Forms lub native:

- [Tworzenie natywnych aplikacji platformy Xamarin.iOS i Xamarin.Android, które używają VB](native-apps.md)
- [Tworzenie aplikacji platformy Xamarin.Forms z VB](xamarin-forms.md)


## <a name="related-links"></a>Linki pokrewne

- [TaskyPortableVB (przykład)](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/TaskyPortableVB)
- [XamarinFormsVB (przykład)](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/XamarinFormsVB)
- [Programowanie wieloplatformowych aplikacji za pomocą programu .NET Framework (Microsoft)](http://msdn.microsoft.com/library/gg597391(v=vs.110).aspx)