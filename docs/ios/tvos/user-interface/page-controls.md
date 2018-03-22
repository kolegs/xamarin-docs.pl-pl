---
title: Praca z formantem strony
description: "Ten artykuł obejmuje projektowanie i Praca z formantem strony wewnątrz aplikacji Xamarin.tvOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: 19198D46-7BBE-4D04-9BFA-7D1C5C9F9FC6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: b1b53fefdd72c36bdffd3c5ade0b8d86da225b14
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/22/2018
---
# <a name="working-with-page-control"></a>Praca z formantem strony

_Ten artykuł obejmuje projektowanie i Praca z formantem strony wewnątrz aplikacji Xamarin.tvOS._

Czasami może być konieczne wyświetlane w aplikacji Xamarin.tvOS kolejne strony lub obrazów. Formant strony została zaprojektowana w celu wyraźnego wyświetlania strony, która jest użytkownik poza maksymalną liczbę stron. Formant strony wyświetla kropek na ciemnym, oval w kształcie tła. Bieżąca strona będzie wyświetlana wypełniony okrąg, pozostałych stronach wyświetlane jako pusty kropek. Formantu strony będzie Przytnij zewnętrzne większość punktów, jeśli jest zbyt duża, aby zmieścić ją w obszarze jej tła.

[![](page-controls-images/page01.png "Formant strony próbki")](page-controls-images/page01.png#lightbox)

Kontrolka strony w elemencie nieinterakcyjnym zaprojektowane, aby przekazać opinię tylko dla użytkownika. Należy dodać inne formanty, aby zmienić numer bieżącej strony (takie jak przyciski lub gestów).

Apple ma poniższe sugestie, korzystając z formantu strony:

- **Używanie na pełne tylko do kolekcji** -formantów strony najlepiej w środowisku pełny ekran, aby wyświetlić wiele stron, które istnieją w jednej kolekcji.
- **Ogranicz liczbę stron** -formantów strony najlepiej dziesięciu (10), lub mniej stron i maksymalnie dwadzieścia (20) stron. Dla ponad dwudziestu stron, należy rozważyć użycie [widok kolekcji](~/ios/tvos/user-interface/collection-views.md) i wyświetlania stron w siatce.

<a name="Page-Controls-and-Storyboards" />

## <a name="page-controls-and-storyboards"></a>Formanty strony i planów

Najprostszym sposobem korzystania z formantów strony w aplikacji Xamarin.tvOS jest dodanie ich do interfejsu użytkownika aplikacji przy użyciu projektanta dla systemu iOS.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

    
1. W **konsoli rozwiązania**, kliknij dwukrotnie `Main.storyboard` pliku i otwórz go do edycji.
1. Przeciągnij **formantu strony** z **przybornika** i upuść go w widoku: 

    [![](page-controls-images/page02.png "Formant strony")](page-controls-images/page02.png#lightbox)
1. W **kartę Widget** z **konsoli właściwości**, można dostosować kilka właściwości formantu strony, takie jak jego **bieżącej strony** i **# stron**: 

    [![](page-controls-images/page03.png "Na karcie widżetu")](page-controls-images/page03.png#lightbox)
1. Następnie dodaj formanty lub gestów do widoku do tyłu i przekazywać je za pomocą kolekcji stron.
1. Na koniec przypisać **nazwy** do formantów, dzięki czemu można odpowiedzieć je w kodzie języka C#. Na przykład: 

    [![](page-controls-images/page04.png "Nazwa kontrolki")](page-controls-images/page04.png#lightbox)
1. Zapisz zmiany.
    

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

    
1. W **Eksploratora rozwiązań**, kliknij dwukrotnie `Main.storyboard` pliku i otwórz go do edycji.
1. Przeciągnij **formantu strony** z **przybornika** i upuść go w widoku: 

    [![](page-controls-images/page02-vs.png "Formant strony")](page-controls-images/page02-vs.png#lightbox)
1. W **kartę Widget** z **Explorer właściwości**, można dostosować kilka właściwości formantu strony, takie jak jego **bieżącej strony** i **# stron**: 

    [![](page-controls-images/page03-vs.png "Na karcie widżetu")](page-controls-images/page03-vs.png#lightbox)
1. Następnie dodaj formanty lub gestów do widoku do tyłu i przekazywać je za pomocą kolekcji stron.
1. Na koniec przypisać **nazwy** do formantów, dzięki czemu można odpowiedzieć je w kodzie języka C#. Na przykład: 

    [![](page-controls-images/page04-vs.png "Nazwa kontrolki")](page-controls-images/page04-vs.png#lightbox)
1. Zapisz zmiany.
    

-----

> [!IMPORTANT]
> Gdy jest można przypisać zdarzenia, takie jak `TouchUpInside` do elementu interfejsu użytkownika (na przykład UIButton) w systemie iOS projektanta, jego zostanie nigdy nie można wywołać, ponieważ Apple TV, nie ma dotykowego ekranu lub obsługuje zdarzenia touch. Zawsze należy używać `Primary Action` zdarzenie, gdy tworzenie obsługi zdarzeń dla systemu tvOS elementy interfejsu użytkownika.




Edytuj kontroler widoku (przykład `ViewController.cs`) i Dodaj kod obsługi stron zmieniane. Na przykład:

```csharp
using System;
using Foundation;
using UIKit;

namespace MySingleView
{
    public partial class ViewController : UIViewController
    {
        #region Computed Properties
        public nint PageNumber { get; set; } = 0;
        #endregion

        #region Constructors
        public ViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Initialize
            PageView.Pages = 6;
            ShowCat ();
        }

        public override void DidReceiveMemoryWarning ()
        {
            base.DidReceiveMemoryWarning ();
            // Release any cached data, images, etc that aren't in use.
        }
        #endregion

        #region Custom Actions
        partial void NextCat (UIBarButtonItem sender) {

            // Display next Cat
            if (++PageNumber > 5) {
                PageNumber = 5;
            }
            ShowCat();

        }

        partial void PreviousCat (UIBarButtonItem sender) {
            // Display previous cat
            if (--PageNumber < 0) {
                PageNumber = 0;
            }
            ShowCat();
        }
        #endregion

        #region Private Methods
        private void ShowCat() {

            // Adjust UI
            PreviousButton.Enabled = (PageNumber > 0);
            NextButton.Enabled = (PageNumber < 5);
            PageView.CurrentPage = PageNumber;

            // Display new cat
            CatView.Image = UIImage.FromFile(string.Format("Cat{0:00}.jpg",PageNumber+1));
        }
        #endregion
    }
}
```

Spójrzmy bliższe spojrzenie na dwie właściwości formantu strony. Po pierwsze aby określić maksymalną liczbę stron, użyj następującego polecenia:

```csharp
PageView.Pages = 6;
```

Aby zmienić numer bieżącej strony, należy użyć poniższego kodu:

```csharp
PageView.CurrentPage = PageNumber;
```

`CurrentPage` Właściwości wynosi zero (0), na podstawie, dzięki czemu pierwszej strony będą miały wartość zero i ostatniego będzie jedną minus maksymalną liczbę stron.

Aby uzyskać więcej informacji na temat pracy z Scenorys, zobacz nasze [Hello, przewodnik Szybki Start systemu tvOS](~/ios/tvos/get-started/hello-tvos.md). 

<a name="Summary" />

## <a name="summary"></a>Podsumowanie

W tym artykule pokrywającego projektowanie i Praca z formantem strony wewnątrz aplikacji Xamarin.tvOS.



## <a name="related-links"></a>Linki pokrewne

- [Przykłady dla systemu tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [systemu tvOS człowieka przewodniki — interfejs](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Przewodnik programowania w języku aplikacji dla systemu tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
