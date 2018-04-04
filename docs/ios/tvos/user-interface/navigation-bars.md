---
title: Praca z kontrolerami nawigacji
description: Ten artykuł obejmuje projektowanie i Praca z paskami nawigacji wewnątrz aplikacji Xamarin.tvOS.
ms.prod: xamarin
ms.assetid: 74E396B7-87F0-46F7-BC6C-827DB8884C97
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 8a9a1c852137a2bcc0d46615e69eef0a245a9768
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="working-with-navigation-controllers"></a>Praca z kontrolerami nawigacji

_Ten artykuł obejmuje projektowanie i Praca z paskami nawigacji wewnątrz aplikacji Xamarin.tvOS._

Pasek nawigacyjny można dodać na początku widoków do wyświetlenia, tytuł i opcjonalnie przycisków na pasku nawigacyjnym. Zwykle są one używane, gdy użytkownik ma przeszedł ze strony głównej, takich jak widok tabeli, kolekcji lub Menu Widok podrzędny pokazywanie szczegółów wybranego elementu.

[![](navigation-bars-images/navbar01.png "Pasek nawigacyjny próbki")](navigation-bars-images/navbar01.png#lightbox)

Ponadto do tytułu (który jest wyświetlany na środku) pasków nawigacji może zawierać jeden lub więcej przycisków paska nawigacji (`UIBarButtonItem`) po lewej i prawej stronie paska.

> [!IMPORTANT]
> Pasek nawigacyjny są całkowicie niewidoczne domyślnie. Należy zachować ostrożność, aby upewnić się, zawartość paska nawigacyjnego pozostaje można odczytać zawartości podrzędne. Na przykład, gdy zawartość w widoku tabeli lub kolekcji Przewija na jego podstawie.




<a name="Navigation-Bars-and-Storyboards" />

## <a name="navigation-bars-and-storyboards"></a>Pasków nawigacji i planów

Najprostszym sposobem, aby pracować z pasków nawigacji w aplikacji Xamarin.tvOS jest dodanie ich do interfejsu użytkownika aplikacji przy użyciu projektanta dla systemu iOS.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)


1. W **konsoli rozwiązania**, kliknij dwukrotnie `Main.storyboard` pliku i otwórz go do edycji.
1. Przeciągnij **pasek nawigacyjny** z **przybornika** i upuść go w widoku w górnej części ekranu: 

    [![](navigation-bars-images/navbar02.png "Pasek nawigacyjny")](navigation-bars-images/navbar02.png#lightbox)
1. Kliknij dwukrotnie **pasek nawigacyjny** zaznacz do **element nawigacji**. W **elementu Widget** karcie **konsoli właściwości**, można ustawić **tytuł**: 

    [![](navigation-bars-images/navbar03.png "Ustaw tytuł")](navigation-bars-images/navbar03.png#lightbox)
1. Następnie można dodać jeden lub więcej **elementy przycisk paska** na dowolnym jej końcu paska: 

    [![](navigation-bars-images/navbar04.png "A element przycisku Superpaska")](navigation-bars-images/navbar04.png#lightbox)
1. Na koniec przewodowy up **elementy przycisk paska** do akcji w **zdarzenia** na karcie **Explorer właściwości**: 

    [![](navigation-bars-images/navbar05.png "A paska akcji elementu przycisku")](navigation-bars-images/navbar05.png#lightbox)
1. Zapisz zmiany.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)


1. W **Eksploratora rozwiązań**, kliknij dwukrotnie `Main.storyboard` pliku i otwórz go do edycji.
1. Przeciągnij **pasek nawigacyjny** z **przybornika** i upuść go w widoku w górnej części ekranu: 

    [![](navigation-bars-images/navbar02-vs.png "Pasek nawigacyjny")](navigation-bars-images/navbar02-vs.png#lightbox)
1. Kliknij dwukrotnie **pasek nawigacyjny** zaznacz do **element nawigacji**. W **elementu Widget** karcie **Explorer właściwości**, można ustawić **tytuł**: 

    [![](navigation-bars-images/navbar03-vs.png "Ustaw tytuł")](navigation-bars-images/navbar03-vs.png#lightbox)
1. Następnie można dodać jeden lub więcej **elementy przycisk paska** na dowolnym jej końcu paska: 

    [![](navigation-bars-images/navbar04-vs.png "A paska przycisk elementów")](navigation-bars-images/navbar04-vs.png#lightbox)
1. Na koniec przewodowy up **elementy przycisk paska** do akcji w **zdarzenia** na karcie **Explorer właściwości**: 

    [![](navigation-bars-images/navbar05-vs.png "A paska akcji elementu przycisków")](navigation-bars-images/navbar05-vs.png#lightbox)
1. Zapisz zmiany.


-----

> [!IMPORTANT]
> Gdy jest można przypisać zdarzenia, takie jak `TouchUpInside` do elementu interfejsu użytkownika (na przykład UIButton) w systemie iOS projektanta, jego zostanie nigdy nie można wywołać, ponieważ Apple TV, nie ma dotykowego ekranu lub obsługuje zdarzenia touch. Zawsze należy używać `Primary Action` zdarzenie, gdy tworzenie obsługi zdarzeń dla systemu tvOS elementy interfejsu użytkownika.




Poniższy kod zapewnia przykład programów obsługi zdarzeń na trzy różne BarButtonItems: `ShowFirstHotel`, `ShowSecondHotel`, i `ShowThirdHotel`. Po kliknięciu każdego elementu, obrazu tła `HotelImage` zostanie zmieniona. To jest edytowany w kontroler widoku (przykład `ViewController.cs`) plików:

```csharp
using System;
using Foundation;
using UIKit;

namespace MySingleView
{
    public partial class ViewController : UIViewController
    {
        #region Constructors
        public ViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();
            // Perform any additional setup after loading the view, typically from a nib.
        }

        public override void DidReceiveMemoryWarning ()
        {
            base.DidReceiveMemoryWarning ();
            // Release any cached data, images, etc that aren't in use.
        }
        #endregion

        #region Custom Actions
        partial void ShowFirstHotel (UIBarButtonItem sender) {
            // Change background image
            HotelImage.Image = UIImage.FromFile("Motel01.jpg");
        }

        partial void ShowSecondHotel (UIBarButtonItem sender) {
            // Change background image
            HotelImage.Image = UIImage.FromFile("Motel02.jpg");
        }

        partial void ShowThirdHotel (UIBarButtonItem sender) {
            // Change background image
            HotelImage.Image = UIImage.FromFile("Motel03.jpg");
        }
        #endregion
    }
}
```

Tak długo, jak przycisk `Enabled` właściwość jest `true` i nie jest objęty przez inny formant lub widok, można go było przy użyciu zdalnego Siri elementu fokusu.

Aby uzyskać więcej informacji na temat pracy z Scenorys, zobacz nasze [Hello, przewodnik Szybki Start systemu tvOS](~/ios/tvos/get-started/hello-tvos.md). 

<a name="Summary" />

## <a name="summary"></a>Podsumowanie

W tym artykule pokrywającego projektowanie i Praca z paskami nawigacji wewnątrz aplikacji Xamarin.tvOS.



## <a name="related-links"></a>Linki pokrewne

- [Przykłady dla systemu tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [systemu tvOS człowieka przewodniki — interfejs](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Przewodnik programowania w języku aplikacji dla systemu tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
