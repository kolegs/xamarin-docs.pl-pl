---
title: Formanty standardowe w Xamarin.Mac
description: Ten artykuł dotyczy pracy z standardowych formantów AppKit, takie jak przyciski, etykiet, pola tekstu pola wyboru, a segmentem formantów w aplikacji Xamarin.Mac. Opisuje dodanie ich do interfejsu z konstruktora interfejsu i interakcji z nimi w kodzie.
ms.prod: xamarin
ms.assetid: d2593883-d255-431f-9781-75f04d8cecea
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: e9d110d0d7a46d431ea6fca50caffe05903ddce2
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34793048"
---
# <a name="standard-controls-in-xamarinmac"></a>Formanty standardowe w Xamarin.Mac

_Ten artykuł dotyczy pracy z standardowych formantów AppKit, takie jak przyciski, etykiet, pola tekstu pola wyboru, a segmentem formantów w aplikacji Xamarin.Mac. Opisuje dodanie ich do interfejsu z konstruktora interfejsu i interakcji z nimi w kodzie._

Podczas pracy z C# i .NET w aplikacji Xamarin.Mac, masz dostęp do tej samej AppKit formantów, które osoba używająca *Objective-C* i *Xcode* jest. Ponieważ Xamarin.Mac integruje się bezpośrednio z Xcode, można użyć w środowisku Xcode _konstruktora interfejsu_ do tworzenia i obsługi formantów Appkit (lub opcjonalnie utworzyć je bezpośrednio w kodzie języka C#).

Formanty AppKit są elementy interfejsu użytkownika, które są używane do tworzenia interfejsu użytkownika aplikacji Xamarin.Mac. One składają się z elementów, takich jak przyciski, etykiety, pola tekstowego, pola wyboru i formanty segmentu i spowodować błyskawiczny akcje lub widoczne wyniki, gdy użytkownik modyfikuje je.

[![](standard-controls-images/intro01.png "Przykład ekranu głównego aplikacji")](standard-controls-images/intro01.png#lightbox)

W tym artykule omówione zostaną następujące czynności podstawowe informacje dotyczące pracy z AppKit formantów w aplikacji Xamarin.Mac. Zdecydowanie zaleca się pracę za pośrednictwem [Hello, Mac](~/mac/get-started/hello-mac.md) artykuł najpierw, w szczególności [wprowadzenie do programów Xcode i kompilatora interfejsu](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) i [gniazda i akcje](~/mac/get-started/hello-mac.md#Outlets_and_Actions) sekcje, w jakiej omawia kluczowe założenia i techniki, które będzie używana w tym artykule.

Może zajść potrzeba Przyjrzyjmy się [udostępnianie klasy języka C# / metody Objective-C](~/mac/internals/how-it-works.md) sekcji [wewnętrzne Xamarin.Mac](~/mac/internals/how-it-works.md) dokumentu również wyjaśniono `Register` i `Export` poleceń używany do przesyłania w górę klas C# do obiektów języka Objective C i elementy interfejsu użytkownika.

<a name="Introduction_to_Controls_and_Views" />

## <a name="introduction-to-controls-and-views"></a>Wprowadzenie do kontrolek i widoków

System macOS (wcześniej znane jako systemu Mac OS X) zawiera standardowy zestaw kontrolek interfejsu użytkownika za pośrednictwem AppKit Framework. One składają się z elementów, takich jak przyciski, etykiety, pola tekstowego, pola wyboru i formanty segmentu i spowodować błyskawiczny akcje lub widoczne wyniki, gdy użytkownik modyfikuje je.

Wszystkie formanty AppKit ma wygląd standardowego, wbudowanego, który ma być odpowiednie dla większości zastosowań, niektóre Określ alternatywny wygląd do użycia w obszarze ramki okna lub w _efekt jaskrawość_ kontekstu, takie jak obszar paska bocznego lub w Widżet Centrum powiadomień.

Apple zasugerować poniższe wskazówki podczas pracy z formantami AppKit

- Unikaj mieszanie rozmiary formantu tego samego widoku.
- Ogólnie rzecz biorąc należy unikać zmiana rozmiaru formantów w pionie.
- Użyj czcionki systemowej i rozmiar prawidłowego tekstu w formancie.
- Użyj prawidłowego odstępów między formantami.

Aby uzyskać więcej informacji, zobacz pleas [o kontrolek i widoki](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsAll.html#//apple_ref/doc/uid/20000957-CH46-SW1) sekcji firmy Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/).

<a name="Using_Controls_in_a_Window_Frame" />

### <a name="using-controls-in-a-window-frame"></a>Korzystanie z formantów ramkę okna

Brak podzbiór formantów AppKit, który zawiera styl wyświetlania, która to umożliwia uwzględnienie w obszarze ramki okna. Na przykład zobacz narzędzi aplikacji poczty:

[![](standard-controls-images/mailapp.png "Ramkę okna Mac")](standard-controls-images/mailapp.png#lightbox)

- **Zaokrąglanie teksturą przycisk** — `NSButton` z styl `NSTexturedRoundedBezelStyle`.
- **Teksturę zaokrąglona Segmentowanych kontroli** — `NSSegmentedControl` z styl `NSSegmentStyleTexturedRounded`.
- **Teksturę zaokrąglona Segmentowanych kontroli** — `NSSegmentedControl` z styl `NSSegmentStyleSeparated`.
- **Zaokrąglanie teksturą Menu podręczne** — `NSPopUpButton` z styl `NSTexturedRoundedBezelStyle`.
- **Zaokrąglanie teksturą Menu rozwijanego** — `NSPopUpButton` z styl `NSTexturedRoundedBezelStyle`.
- **Pasek wyszukiwania** — `NSSearchField`.

Apple zasugerować poniższe wskazówki podczas pracy z AppKit formantów ramkę okna

- Nie używaj stylów określonego formantu ramki okna w treści okna.
- Nie używaj kontrolki okna treści lub style w ramki okna.

Aby uzyskać więcej informacji, zobacz pleas [o kontrolek i widoki](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsAll.html#//apple_ref/doc/uid/20000957-CH46-SW1) sekcji firmy Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/).

<a name="Creating_a_User_Interface_in_Interface_Builder" />

## <a name="creating-a-user-interface-in-interface-builder"></a>Tworzenie interfejsu użytkownika w Konstruktorze interfejsu

Podczas tworzenia nowej aplikacji Xamarin.Mac Cocoa, otrzymasz standardowe okno puste, domyślnie. Ten system windows jest zdefiniowany w `.storyboard` pliku automatycznie dołączony do projektu. Aby edytować w projekcie systemu windows **Eksploratora rozwiązań**, kliknij dwukrotnie `Main.storyboard` pliku:

[![](standard-controls-images/edit01.png "Wybieranie głównego Storyboard w Eksploratorze rozwiązań")](standard-controls-images/edit01.png#lightbox)

Spowoduje to otwarcie okna projektu w Konstruktorze interfejsu w środowisku Xcode:

[![](standard-controls-images/edit02.png "Edytowanie scenorysu w środowisku Xcode")](standard-controls-images/edit02.png#lightbox)

Aby utworzyć interfejsu użytkownika, będzie przeciągnij elementy interfejsu użytkownika (AppKit formantów) z **inspektora biblioteki** do **Edytor interfejsu** w Konstruktorze interfejsu. W poniższym przykładzie **widok Podziel pionowo** formant został narkotyków z **inspektora biblioteki** i umieścić w oknie **Edytor interfejsu**:

[![](standard-controls-images/edit03.png "Wybranie widoku podział z biblioteki")](standard-controls-images/edit03.png#lightbox)

Aby uzyskać więcej informacji na temat tworzenia interfejsu użytkownika w Konstruktorze interfejsu, zobacz nasze [wprowadzenie do programów Xcode i kompilatora interfejsu](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) dokumentacji.

<a name="Sizing_and_Positioning" />

### <a name="sizing-and-positioning"></a>Ustawianie rozmiaru i pozycjonowania

Po formant został uwzględniony w interfejsie użytkownika, użyj **edytora ograniczeń** jej lokalizacja i rozmiar przez ręczne wprowadzanie wartości i sterowanie znajduje się automatycznie i o rozmiarze, gdy formant nadrzędny okna lub widoku Zmieniono rozmiar:

[![](standard-controls-images/edit04.png "Ustawienie z ograniczeniami")](standard-controls-images/edit04.png#lightbox)

Użyj **czerwony I świateł** wokół zewnętrznej **Autoresizing** polu _USB_ formantu do lokalizacji w danym (x, y). Na przykład: 

[![](standard-controls-images/edit05.png "Edytowanie ograniczenia")](standard-controls-images/edit05.png#lightbox)

Określa, że wybrany formant (w **Widok hierarchii** & **Edytor interfejsu**) zostać zablokowana lokalizacji górnego i prawego widoku lub okna zmiany rozmiaru lub przenieść. 

Inne elementy właściwości formantu edytora, takie jak wysokość i szerokość:

[![](standard-controls-images/edit06.png "Ustawienie wysokość")](standard-controls-images/edit06.png#lightbox)

Można też kontrolować sposób wyrównania elementów z ograniczeniami przy użyciu **Edytor wyrównanie**:

[![](standard-controls-images/edit07.png "Edytor wyrównania")](standard-controls-images/edit07.png#lightbox)

> [!IMPORTANT]
> W przeciwieństwie do systemu iOS gdzie (0,0) jest górnym lewym dolnym rogu ekranu, w macOS (0,0) jest niższa po lewej stronie ekranu. Jest to spowodowane macOS używa matematyczne układ współrzędnych z wartości liczbowe zwiększenie wartości w górę i w prawo. Należy to uwzględnić podczas umieszczania AppKit kontrolek interfejsu użytkownika.

<a name="Setting_a_Custom_Class" />

### <a name="setting-a-custom-class"></a>Ustawienie niestandardowej klasy

Brak czasu pracy z formantami AppKit, które będzie trzeba podklasy i istniejącego formantu i Utwórz własny niestandardowy wersja tej klasy. Na przykład Definiowanie niestandardową wersję listy źródeł:

```csharp
using System;
using AppKit;
using Foundation;

namespace AppKit
{
    [Register("SourceListView")]
    public class SourceListView : NSOutlineView
    {
        #region Computed Properties
        public SourceListDataSource Data {
            get {return (SourceListDataSource)this.DataSource; }
        }
        #endregion

        #region Constructors
        public SourceListView ()
        {

        }

        public SourceListView (IntPtr handle) : base(handle)
        {

        }

        public SourceListView (NSCoder coder) : base(coder)
        {

        }

        public SourceListView (NSObjectFlag t) : base(t)
        {

        }
        #endregion

        #region Override Methods
        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();

        }
        #endregion

        #region Public Methods
        public void Initialize() {

            // Initialize this instance
            this.DataSource = new SourceListDataSource (this);
            this.Delegate = new SourceListDelegate (this);

        }

        public void AddItem(SourceListItem item) {
            if (Data != null) {
                Data.Items.Add (item);
            }
        }
        #endregion

        #region Events
        public delegate void ItemSelectedDelegate(SourceListItem item);
        public event ItemSelectedDelegate ItemSelected;

        internal void RaiseItemSelected(SourceListItem item) {
            // Inform caller
            if (this.ItemSelected != null) {
                this.ItemSelected (item);
            }
        }
        #endregion
    }
}
```

Gdzie `[Register("SourceListView")]` ujawnia instrukcji `SourceListView` klasy języka Objective-C, który jest mogą być używane w konstruktora interfejsu. Aby uzyskać więcej informacji, zobacz [udostępnianie klasy języka C# / metody Objective-C](~/mac/internals/how-it-works.md) sekcji [wewnętrzne Xamarin.Mac](~/mac/internals/how-it-works.md) dokumentu wyjaśniono `Register` i `Export` polecenia używane podczas transmisji zapasowe klas C# do obiektów języka Objective C i elementy interfejsu użytkownika.

Powyższy kod w miejscu możesz przeciągnąć kontrolkę AppKit typu podstawowego, który powiększa się, na powierzchnię projektu (w poniższym przykładzie **listy źródeł**), przełącz się do **inspektora tożsamości** i ustaw **klasy niestandardowej** na nazwę, która ujawniony dla języka Objective-C (przykład `SourceListView`):

[![](standard-controls-images/edit10.png "Ustawienie niestandardowej klasy w środowisku Xcode")](standard-controls-images/edit10.png#lightbox)

<a name="Exposing_Outlets_and_Actions" />

### <a name="exposing-outlets-and-actions"></a>Udostępnianie punktów i akcji

Zanim formant AppKit można uzyskać w kodzie języka C#, musi on być udostępniany jako albo **gniazda** lub i **akcji**. Aby zrobić to wybierz daną kontrolkę w jednym **hierarchii interfejsów** lub **Edytor interfejsu** i przejdź do **Asystenta widoku** (Upewnij się, że masz `.h`okna zaznaczony w celu edycji):

[![](standard-controls-images/edit11.png "Wybranie poprawnego pliku do edycji")](standard-controls-images/edit11.png#lightbox)

Przeciągnij formant z formantu AppKit na przyznanie `.h` plik, aby rozpocząć tworzenie **gniazda** lub **akcji**:

[![](standard-controls-images/edit12.png "Przeciąganie w celu utworzenia gniazda lub akcji")](standard-controls-images/edit12.png#lightbox)

Wybierz typ zagrożeń, aby utworzyć i nadaj **gniazda** lub **akcji** **nazwa**: 

[![](standard-controls-images/edit13.png "Konfigurowanie gniazda lub akcji")](standard-controls-images/edit13.png#lightbox)


Aby uzyskać więcej informacji na temat pracy z **gniazda** i **akcje**, można znaleźć pod adresem [gniazda i akcje](~/mac/get-started/hello-mac.md#Outlets_and_Actions) sekcji naszych [wprowadzenie do programów Xcode i interfejsu Konstruktor](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) dokumentacji.

<a name="Synchronizing_Changes_with_Xcode" />

### <a name="synchronizing-changes-with-xcode"></a>Synchronizowanie zmian z Xcode

Po przełączeniu do programu Visual Studio dla komputerów Mac w programie Xcode, wszystkie zmiany wprowadzone w programie Xcode automatycznie zostaną zsynchronizowane z projektu Xamarin.Mac.

W przypadku wybrania `SplitViewController.designer.cs` w **Eksploratora rozwiązań** będzie mógł wyświetlić sposób z **gniazda** i **akcji** zostały przewodowej się w naszym kodzie C#:

[![](standard-controls-images/sync01.png "Synchronizowanie zmian z Xcode")](standard-controls-images/sync01.png#lightbox)

Powiadomienie jak definicji w `SplitViewController.designer.cs` pliku:

```csharp
[Outlet]
AppKit.NSSplitViewItem LeftController { get; set; }

[Outlet]
AppKit.NSSplitViewItem RightController { get; set; }

[Outlet]
AppKit.NSSplitView SplitView { get; set; }
```

Wiersz w górę z definicją w `MainWindow.h` pliku w programie Xcode:

```csharp
@interface SplitViewController : NSSplitViewController {
    NSSplitViewItem *_LeftController;
    NSSplitViewItem *_RightController;
    NSSplitView *_SplitView;
}

@property (nonatomic, retain) IBOutlet NSSplitViewItem *LeftController;

@property (nonatomic, retain) IBOutlet NSSplitViewItem *RightController;

@property (nonatomic, retain) IBOutlet NSSplitView *SplitView;
```

Jak widać, Visual Studio for Mac wykrywa zmiany `.h` pliku, a następnie automatycznie synchronizuje te zmiany w odpowiednich `.designer.cs` pliku do udostępnienia ich do aplikacji. Należy również zauważyć, że `SplitViewController.designer.cs` jest częściowej klasy, dzięki czemu nie ma programu Visual Studio for Mac, można zmodyfikować `SplitViewController.cs ` który zastąpiłaby wszelkie zmiany, które zostały wprowadzone do klasy.

Zwykle nie należy otworzyć `SplitViewController.designer.cs` samodzielnie, jego został przedstawiony tutaj wyłącznie w celach edukacyjnych.

> [!IMPORTANT]
> W większości przypadków programu Visual Studio for Mac automatycznie Zobacz wszystkie zmiany wprowadzone w programie Xcode i zsynchronizować je do projektu Xamarin.Mac. W wystąpieniu wyłączenia synchronizacji nie jest realizowane automatycznie przejdź do programów Xcode i je do programu Visual Studio dla komputerów Mac ponownie. Zwykle będzie to rozpocząć poza cyklu synchronizacji.

<a name="Working_with_Buttons" />

## <a name="working-with-buttons"></a>Praca z przycisków

AppKit udostępnia kilka typów przycisku, który może być używana w projekcie interfejsu użytkownika. Aby uzyskać więcej informacji, zobacz [przyciski](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsButtons.html#//apple_ref/doc/uid/20000957-CH48-SW1) sekcji firmy Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). 

[![](standard-controls-images/buttons01.png "Przykładem przycisku różnych typów")](standard-controls-images/buttons01.png#lightbox)

Jeśli przycisk narażony był za pośrednictwem **gniazda**, następujący kod będzie odpowiadać jej naciśnięcie:

```csharp
ButtonOutlet.Activated += (sender, e) => {
        FeedbackLabel.StringValue = "Button Outlet Pressed";
};
```

W przypadku przycisków, które zostały udostępnione za pośrednictwem **akcje**, `public partial` metody zostanie automatycznie utworzone dla Ciebie nazwą, która została wybrana opcja w środowisku Xcode. Aby odpowiedzieć na **akcji**, wykonania metody częściowej klasy który **akcji** została zdefiniowana w. Na przykład:

```csharp
partial void ButtonAction (Foundation.NSObject sender) {
    // Do something in response to the Action
    FeedbackLabel.StringValue = "Button Action Pressed";
}
```

W przypadku przycisków, które mają stan (takich jak **na** i **poza**), stan można sprawdzić lub zestaw z `State` właściwość `NSCellStateValue` wyliczenia. Na przykład:

```csharp
DisclosureButton.Activated += (sender, e) => {
    LorumIpsum.Hidden = (DisclosureButton.State == NSCellStateValue.On);
};
```

Gdzie `NSCellStateValue` może być:

- **Na** — przycisk jest naciśnięty, czy formant jest zaznaczony (na przykład zaznacz pole wyboru).
- **Wyłącz** — przycisk jest naciśnięty nie lub formant nie jest zaznaczone.
- **Mieszane** -kombinacja **na** i **poza** stanów.

<a name="Mark-a-Button-as-Default-and-Set-Key-Equivalent" />

### <a name="mark-a-button-as-default-and-set-key-equivalent"></a>Zaznacz przycisk jako domyślne i ustawić odpowiednikiem klucza

Dla dowolnego przycisku, który został dodany do projektu interfejsu użytkownika, można oznaczyć jako przycisku _domyślne_ zostanie aktywowany, gdy użytkownik naciśnie przycisk **Return/Enter** klucza na klawiaturze. W macOS ten przycisk, zostanie wyświetlony kolor tła niebieski domyślnie.

Przycisk Ustaw jako domyślny, należy wybrać w Konstruktorze interfejsu w środowisku Xcode. Dalej w **inspektora atrybutu**, wybierz pozycję **klucza równoważne** pole i naciśnij klawisz **Return/Enter** klucza:

[![](standard-controls-images/buttons03.png "Edytowanie odpowiednikiem klucza")](standard-controls-images/buttons03.png#lightbox)

Podobnie można przypisać sekwencja klucza, który może służyć do aktywacji przy użyciu klawiatury, zamiast myszy przycisk. Na przykład przez nacisnąć klawisze Command-C w powyższy obraz.

Gdy aplikacja jest uruchamiana i okna z przycisku jest klucz i fokus, gdy użytkownik naciśnie polecenie-C, Akcja przycisku zostanie uaktywniona (jako — Jeśli użytkownik miał kliknął przycisk).

<a name="Working_with_Checkboxes_and_Radio_Buttons" />

## <a name="working-with-checkboxes-and-radio-buttons"></a>Praca z pola wyboru i przycisków radiowych

AppKit zapewnia kilka typów pól wyboru i grup przycisków radiowych, które mogą być używane w projekcie interfejsu użytkownika. Aby uzyskać więcej informacji, zobacz [przyciski](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsButtons.html#//apple_ref/doc/uid/20000957-CH48-SW1) sekcji firmy Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). 

[![](standard-controls-images/buttons02.png "Przykład pola wyboru dostępne typy")](standard-controls-images/buttons02.png#lightbox)


Pola wyboru i przycisków radiowych (udostępniane za pośrednictwem funkcji **gniazda**) mają stan (takich jak **na** i **poza**), stan można sprawdzić lub zestaw z `State` właściwość `NSCellStateValue` wyliczenia. Na przykład:

```csharp
AdjustTime.Activated += (sender, e) => {
    FeedbackLabel.StringValue = string.Format("Adjust Time: {0}",AdjustTime.State == NSCellStateValue.On);
};
```

Gdzie `NSCellStateValue` może być:

- **Na** — przycisk jest naciśnięty, czy formant jest zaznaczony (na przykład zaznacz pole wyboru).
- **Wyłącz** — przycisk jest naciśnięty nie lub formant nie jest zaznaczone.
- **Mieszane** -kombinacja **na** i **poza** stanów.

Aby wybrać przycisk Grupa przycisków radiowych, ujawnić przycisk radiowy, aby wybrać jako **gniazda** i ustawić jej `State` właściwości. Na przykład:

```csharp
partial void SelectCar (Foundation.NSObject sender) {
    TransportationCar.State = NSCellStateValue.On;
    FeedbackLabel.StringValue = "Car Selected";
}
```

Aby uzyskać kolekcję przycisków radiowych, aby pełnić funkcję grupy i automatycznie obsługi wybranego stanu, Utwórz nową **akcji** i Dołącz do niej co przycisku w grupie:

![](standard-controls-images/buttons04.png "Tworzenie nowej akcji")

Następnie przypisać unikatowy `Tag` do każdego przycisku radiowego w **inspektora atrybutu**:

![](standard-controls-images/buttons05.png "Edytowanie tagów przycisku radiowego")

Zapisz zmiany i wróć do programu Visual Studio dla komputerów Mac, Dodaj kod obsługi **akcji** wszystkich przycisków radiowych są dołączone do:

```csharp
partial void NumberChanged(Foundation.NSObject sender)
{
    var check = sender as NSButton;
    Console.WriteLine("Changed to {0}", check.Tag);
}
```

Można użyć `Tag` właściwości, aby zobaczyć, która została zaznaczona opcja.

<a name="Working_with_Menu_Controls" />

## <a name="working-with-menu-controls"></a>Praca z Menu formantów

AppKit udostępnia kilka typów formantów Menu, które mogą być używane w projekcie interfejsu użytkownika. Aby uzyskać więcej informacji, zobacz [kontrolek Menu](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlswithMenus.html#//apple_ref/doc/uid/20000957-CH100-SW1) sekcji firmy Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). 

[![](standard-controls-images/menu01.png "Przykład kontrolek menu")](standard-controls-images/menu01.png#lightbox)

<a name="Providing-Menu-Control-Data" />

### <a name="providing-menu-control-data"></a>Dostarcza dane formant Menu

Dostępne dla macOS kontrolek Menu można ustawić do wypełnienia listy rozwijanej, albo z listy wewnętrznych (która może być wstępnie zdefiniowanych w Konstruktorze interfejsu lub wypełniane przy użyciu kodu) lub podając źródła danych niestandardowych, zewnętrznych.

<a name="Working-with-Internal-Data" />

#### <a name="working-with-internal-data"></a>Praca z danymi wewnętrzny

Oprócz definiujący elementy w Konstruktorze interfejsu, kontrolek Menu (takich jak `NSComboBox`), podaj kompletny zestaw metod, dzięki którym można dodać, edytować lub usunąć elementy z listy wewnętrznej, które zachowują:

- `Add` -Dodaje nowy element na końcu listy.
- `GetItem` -Zwraca element pod danym indeksem.
- `Insert` -Wstawia nowy element na liście w podanej lokalizacji.
- `IndexOf` -Zwraca indeks danego elementu.
- `Remove` — Usuwa podany element z listy.
- `RemoveAll` — Usuwa wszystkie elementy z listy.
- `RemoveAt` — Usuwa element pod danym indeksem.
- `Count` -Zwraca liczbę elementów na liście.

> [!IMPORTANT]
> Jeśli używasz zewnętrznego źródła danych (`UsesDataSource = true`), wywoływanie metod powyżej spowoduje zgłoszenie wyjątku.

<a name="Working-with-an-External-Data-Source" />

#### <a name="working-with-an-external-data-source"></a>Praca z zewnętrznego źródła danych

Zamiast Podaj wierszy dla formantu Menu za pomocą wbudowanych danych wewnętrznych, można opcjonalnie Użyj zewnętrznego źródła danych i podaj sklepu zapasowy elementy (takie jak bazy danych SQLite).

Aby pracować z zewnętrznego źródła danych, należy utworzyć wystąpienia formantu Menu źródła danych (`NSComboBoxDataSource` na przykład) i Przesłoń kilka metody w celu zapewnienia niezbędnych danych:

- `ItemCount` -Zwraca liczbę elementów na liście.
- `ObjectValueForItem` -Zwraca wartość elementu dla danego indeksu.
- `IndexOfItem` -Zwraca indeks należy podać wartość elementu.
- `CompletedString` -Zwraca pierwszą wartość elementu pasujące częściowo wpisany element wartości. Ta metoda jest wywoływana tylko, jeśli włączono Autouzupełniania (`Completes = true`).

Zobacz [baz danych i polach kombi](~/mac/app-fundamentals/databases.md#Databases-and-ComboBoxes) sekcji [pracy z bazami danych](~/mac/app-fundamentals/databases.md) dokumentu, aby uzyskać więcej informacji.

<a name="Adjusting-the-Lists-Appearance" />

### <a name="adjusting-the-lists-appearance"></a>Dostosowywanie wyglądu listy

Można dostosować wygląd formantu Menu są następujące metody:

- `HasVerticalScroller` -Jeśli `true`, kontrolka będzie wyświetlać pionowy pasek przewijania. 
- `VisibleItems` — Dostosuj liczbę elementów wyświetlane, gdy formant jest otwarty. Wartość domyślna to 5 (5).
- `IntercellSpacing` — Dostosuj ilość miejsca dla danego elementu, zapewniając `NSSize` gdzie `Width` określa lewego i prawego marginesu oraz `Height` Określa odstęp przed i po elemencie.
- `ItemHeight` — Określa wysokość poszczególnych elementów na liście.

W przypadku typów listy rozwijanej `NSPopupButtons`, pierwszy element Menu udostępnia tytuł dla formantu. Na przykład: 

[![](standard-controls-images/menu02.png "Przykład formant menu")](standard-controls-images/menu02.png#lightbox)

Aby zmienić tytuł, ujawnia tego elementu jako **gniazda** i użyć kodu podobnie do następującej:

```csharp
DropDownSelected.Title = "Item 1";
```

<a name="Manipulating-the-Selected-Items" />

### <a name="manipulating-the-selected-items"></a>Manipulowanie wybranych elementów

Poniżej przedstawiono metody i właściwości umożliwia manipulowanie wybrane elementy na liście kontroli Menu:

- `SelectItem` -Wybiera element pod danym indeksem.
- `Select` -Wybierz wartość danego elementu.
- `DeselectItem` — Usuwa zaznaczonego elementu pod danym indeksem.
- `SelectedIndex` -Zwraca indeks aktualnie zaznaczonego elementu.
- `SelectedValue` -Zwraca wartość obecnie wybranego elementu.

Użyj `ScrollItemAtIndexToTop` do prezentowania elementu pod danym indeksem w górnej części listy i `ScrollItemAtIndexToVisible` przewiń do listy, aż będzie widoczna pozycja pod danym indeksem.

<a name="Responding to Events" />

### <a name="responding-to-events"></a>Odpowiadanie na zdarzenia

Menu elementy sterujące udostępniają następujące zdarzenia odpowiedzieć interakcji z użytkownikiem:

- `SelectionChanged` -Jest wywoływane, gdy użytkownik wybrał wartość z listy.
- `SelectionIsChanging` -Jest wywoływana przed aktywne zaznaczenie staje się nowy element wybrano użytkownika.
- `WillPopup` -Jest wywoływana przed wyświetleniem listy rozwijanej elementów.
- `WillDismiss` -Jest wywoływana przed zamknięciem elementów listy rozwijanej.

Dla `NSComboBox` formantów, obejmują wszystkie tego samego zdarzenia jako `NSTextField`, takich jak `Changed` zdarzeń, która jest wywoływana, gdy użytkownik edytuje wartości tekstu w polu kombi.

Opcjonalnie można odpowiadać wewnętrzne elementy Menu dane zdefiniowane w wybierana przez dołączenie element do konstruktora interfejsu **akcji** i użyć kodu podobne do poniższych odpowiedzieć na **akcji** Trwa wyzwalane przez użytkownika:

```csharp
partial void ItemOne (Foundation.NSObject sender) {
    DropDownSelected.Title = "Item 1";
    FeedbackLabel.StringValue = "Item One Selected";
}
```

Aby uzyskać więcej informacji na temat pracy z menu i formantów Menu, zobacz nasze [menu](~/mac/user-interface/menu.md) i [przycisk wyskakujących i listy rozwijane](~/mac/user-interface/menu.md) dokumentacji.

<a name="Working_with_Selection_Controls" />

## <a name="working-with-selection-controls"></a>Praca z formantami wyboru

AppKit zapewnia kilka typów formanty zaznaczenia, których można użyć w projekcie interfejsu użytkownika. Aby uzyskać więcej informacji, zobacz [kontrolki wyboru](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsSelection.html#//apple_ref/doc/uid/20000957-CH49-SW1) sekcji firmy Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). 

[![](standard-controls-images/select01.png "Przykład zaznaczanie formantów")](standard-controls-images/select01.png#lightbox)

Istnieją dwa sposoby śledzenia, gdy formant wyboru ma interakcji z użytkownikiem, w przypadku wystawianego go jako **akcji**. Na przykład:

```csharp
partial void SegmentButtonPressed (Foundation.NSObject sender) {
    FeedbackLabel.StringValue = string.Format("Button {0} Pressed",SegmentButtons.SelectedSegment);
}
```

Lub dołączając **delegata** do `Activated` zdarzeń. Na przykład:

```csharp
TickedSlider.Activated += (sender, e) => {
    FeedbackLabel.StringValue = string.Format("Stepper Value: {0:###}",TickedSlider.IntValue);
};
```

Aby ustawić lub odczytać wartości formant wyboru, użyj `IntValue` właściwości. Na przykład:

```csharp
FeedbackLabel.StringValue = string.Format("Stepper Value: {0:###}",TickedSlider.IntValue);
```

Formanty specjalne (takie jak również koloru i obrazu również) mają właściwości specyficzne dla ich typów wartości. Na przykład:

```csharp
CollorWell.Color = NSColor.Red;
ImageWell.Image = NSImage.ImageNamed ("tag.png");

```

`NSDatePicker` Ma następujące właściwości dla pracy bezpośrednio z datą i godziną:

- **Data** -bieżąca wartość daty i godziny jako `NSDate`.
- **Lokalne** -lokalizacji użytkownika jako `NSLocal`.
- **Parametru TimeInterval** -jako wartość czasu `Double`.
- **Strefa czasowa** -strefa czasowa użytkownika jako `NSTimeZone`.

<a name="Working_with_Indicator_Controls" />

## <a name="working-with-indicator-controls"></a>Praca z kontrolkami wskaźnika

AppKit udostępnia kilka typów formantów wskaźnika, które mogą być używane w projekcie interfejsu użytkownika. Aby uzyskać więcej informacji, zobacz [formanty wskaźnika](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsIndicators.html#//apple_ref/doc/uid/20000957-CH50-SW1) sekcji firmy Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). 

[![](standard-controls-images/level01.png "Przykład wskaźnik formantów")](standard-controls-images/level01.png#lightbox)

Istnieją dwa sposoby, aby sprawdzić, kiedy wskaźnik formantem interakcji z użytkownikiem, albo w przypadku wystawianego go jako **akcji** lub **gniazda** i dołączania **delegata** do `Activated`zdarzeń. Na przykład:

```csharp
LevelIndicator.Activated += (sender, e) => {
    FeedbackLabel.StringValue = string.Format("Level: {0:###}",LevelIndicator.DoubleValue);
};
```

Aby odczytać lub ustaw wartość formantu wskaźnika, użyj `DoubleValue` właściwości. Na przykład:

```csharp
FeedbackLabel.StringValue = string.Format("Rating: {0:###}",Rating.DoubleValue);
```

Po wyświetleniu powinien animowane nieokreślone i asynchroniczne wskaźniki postępu. Użyj `StartAnimation` metodę, aby rozpocząć animacji, gdy są one wyświetlane. Na przykład:

```csharp
Indeterminate.StartAnimation (this);
AsyncProgress.StartAnimation (this);
```

Wywoływanie `StopAnimation` metody przestanie animacji.

<a name="Working_with_Text_Controls" />

## <a name="working-with-text-controls"></a>Praca z formantami

AppKit zapewnia kilka typów formantami, które mogą być używane w projekcie interfejsu użytkownika. Aby uzyskać więcej informacji, zobacz [formantami](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsText.html#//apple_ref/doc/uid/20000957-CH51-SW1) sekcji firmy Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). 

[![](standard-controls-images/text01.png "Przykład kontrolek tekstowych")](standard-controls-images/text01.png#lightbox)

Dla pól tekstowych (`NSTextField`), następujące zdarzenia mogą służyć do śledzenia interakcji z użytkownikiem:

- **Zmienione** — jest uruchamiany w dowolnym momencie użytkownik zmieni wartość pola. Na przykład na każdy znak jest wpisany.
- **EditingBegan** -uruchamiane, gdy użytkownik wybierze pól do edycji.
- **EditingEnded** — gdy użytkownik naciśnie klawisz Enter w polu lub opuszczania pola.

Użyj `StringValue` właściwości do odczytu lub ustaw wartość w polu. Na przykład:

```csharp
FeedbackLabel.StringValue = string.Format("User ID: {0}",UserField.StringValue);
```

W przypadku pól, które wyświetlanie lub edytowanie wartości liczbowe, można użyć `IntValue` właściwości. Na przykład:

```csharp
FeedbackLabel.StringValue = string.Format("Number: {0}",NumberField.IntValue);
```

`NSTextView` Zapewnia pełny tekst polecanych Edytuj i Wyświetl obszaru z formatowaniem wbudowanych. Podobnie jak `NSTextField`, użyj `StringValue` właściwości do odczytu lub ustaw wartość obszaru.

Na przykład złożonego przykładu pracy z widokami tekstu w aplikacji Xamarin.Mac zobacz [SourceWriter Przykładowa aplikacja](https://developer.xamarin.com/samples/mac/SourceWriter/). SourceWriter jest proste źródła edytora kodu, który zapewnia obsługę uzupełniania kodu i wyróżnianie składni proste.

Pełni komentarzem kodu SourceWriter i, jeśli jest dostępna, zostały podane linki z technologii kluczy lub metod do odpowiednich informacji w dokumentacji przewodniki Xamarin.Mac.

<a name="Working_with_Content_Views" />

## <a name="working-with-content-views"></a>Praca z widokami zawartości

AppKit zapewnia kilka typów zawartości widoków, które mogą być używane w projekcie interfejsu użytkownika. Aby uzyskać więcej informacji, zobacz [zawartości widoków](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsView.html#//apple_ref/doc/uid/20000957-CH52-SW1) sekcji firmy Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/).

[![](standard-controls-images/content01.png "Przykładowy widok zawartości")](standard-controls-images/content01.png#lightbox)

<a name="Popovers" />

### <a name="popovers"></a>Popovers

Popover jest przejściowy elementu interfejsu użytkownika, który zawiera funkcję, która jest bezpośrednio powiązana z określonych kontrolkę lub obszar na ekranie. Popover pojawia się nad oknem zawierający formant lub obszar, który jest powiązany z i jego obramowanie zawiera strzałkę, aby wskazać punkt, z którego okazało się.

Aby utworzyć popover, wykonaj następujące czynności:

1. Otwórz `.storyboard` pliku okna, które chcesz dodać popover do przez dwukrotne kliknięcie w **Eksploratora rozwiązań**
2. Przeciągnij **wyświetlić kontroler** z **inspektora biblioteki** na **interfejs edytora**: 

    [![](standard-controls-images/content02.png "Wybiera kontroler widoku z biblioteki")](standard-controls-images/content02.png#lightbox)
4. Zdefiniuj rozmiar i układ **widok niestandardowy**: 

    [![](standard-controls-images/content04.png "Edytowanie układu")](standard-controls-images/content04.png#lightbox)
5. Sterowania kliknij i przeciągnij od źródła menu podręczne na **kontrolera widoku**: 

    [![](standard-controls-images/content05.png "Przeciąganie w celu utworzenia segue")](standard-controls-images/content05.png#lightbox)
6. Wybierz **Popover** z menu podręcznego: 

    [![](standard-controls-images/content06.png "Ustawienie typu segue")](standard-controls-images/content06.png#lightbox)
7. Zapisz zmiany i wróć do programu Visual Studio for Mac synchronizację w programie Xcode.

<a name="Tab_Views" />

### <a name="tab-views"></a>Karta widoków

Widoki karta zawiera listę kartę (który podobne do Segmentowanych formantu wygląda) połączone z zestaw widoków, które są nazywane _okienka_. Po wybraniu nowej karty, pojawi się okienko, w którym jest do niego dołączony. Każdy okienko zawiera własny zestaw kontrolek.

Podczas pracy z widoku kartę w Konstruktorze interfejsu w programie Xcode, użyj **inspektora atrybutu** liczbę kart:

[![](standard-controls-images/content08.png "Liczba kart edycji")](standard-controls-images/content08.png#lightbox)

Wybierz kartę każdego w **hierarchii interfejsów** można ustawić jej **tytuł** i Dodaj elementy interfejsu użytkownika do jego **okienko**:

[![](standard-controls-images/content09.png "Edytowanie karty w środowisku Xcode")](standard-controls-images/content09.png#lightbox)

<a name="Data_Binding_AppKit_Controls" />

## <a name="data-binding-appkit-controls"></a>Formanty AppKit powiązania danych

Przy użyciu techniki kluczy i wartości kodowania i powiązania danych w aplikacji Xamarin.Mac, można znacznie zmniejszyć ilość kodu, które trzeba zapisać i zachować do wypełnienia i pracować z nimi elementy interfejsu użytkownika. Masz również zaletą dalsze oddzielenie danych zapasowy (_modelu danych_) od z przodu kończyć interfejsu użytkownika (_Model-View-Controller_), prowadzących do łatwiejsze w obsłudze, bardziej elastyczne aplikacji Projekt.

Kodowanie klucz-wartość (KVC) to mechanizm służący do uzyskiwania dostępu do właściwości obiektu pośrednio, za pomocą kluczy (specjalnie sformatowana ciągi) do identyfikowania właściwości zamiast dostępu do nich za pośrednictwem zmienne wystąpienia lub metody dostępu (`get/set`). Implementując kluczy i wartości kodowania zgodne metod dostępu do aplikacji Xamarin.Mac, uzyskasz dostęp do innych funkcji macOS, takich jak obserwowania klucz-wartość (KVO), wiązanie danych podstawowych danych, Cocoa powiązania i scriptability.

Aby uzyskać więcej informacji, zobacz [proste powiązanie danych](~/mac/app-fundamentals/databinding.md#Simple_Data_Binding) sekcji naszych [powiązania danych i kluczy i wartości kodowania](~/mac/app-fundamentals/databinding.md) dokumentacji.


<a name="Summary" />

## <a name="summary"></a>Podsumowanie

W tym artykule trwało szczegółowe przyjrzeć się praca z standardowych formantów AppKit takie jak przyciski, etykiety, pola tekstowego, pola wyboru i Segmentowanych formantów w aplikacji Xamarin.Mac. Obejmuje ona dodanie ich do projektu interfejsu użytkownika w Konstruktorze interfejsu w środowisku Xcode, ujawnienia ich do kodu za pomocą akcji i gniazda i Praca z kontrolkami AppKit w kodzie języka C#.

## <a name="related-links"></a>Linki pokrewne

- [MacControls (przykład)](https://developer.xamarin.com/samples/mac/MacControls/)
- [Witaj, Mac](~/mac/get-started/hello-mac.md)
- [Windows](~/mac/user-interface/window.md)
- [Powiązanie danych i kodowanie klucz-wartość](~/mac/app-fundamentals/databinding.md)
- [OS X człowieka Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Formanty i widoki — informacje](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsAll.html#//apple_ref/doc/uid/20000957-CH46-SW1)
