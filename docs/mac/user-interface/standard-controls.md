---
title: Kontrolki standardowe platformie Xamarin.Mac
description: Ten artykuł dotyczy pracy przy użyciu standardowych kontrolek AppKit, takie jak przyciski, etykiety, pola tekstowe, pola wyboru i segmentowanych kontrolek w aplikacji platformy Xamarin.Mac. Opisano w nim dodanie ich do interfejsu z programu Interface Builder i interakcji z nimi w kodzie.
ms.prod: xamarin
ms.assetid: d2593883-d255-431f-9781-75f04d8cecea
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 1760e764c4918f597c95a4c811be9166e33bb1f3
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/13/2018
ms.locfileid: "43780585"
---
# <a name="standard-controls-in-xamarinmac"></a>Kontrolki standardowe platformie Xamarin.Mac

_Ten artykuł dotyczy pracy przy użyciu standardowych kontrolek AppKit, takie jak przyciski, etykiety, pola tekstowe, pola wyboru i segmentowanych kontrolek w aplikacji platformy Xamarin.Mac. Opisano w nim dodanie ich do interfejsu z programu Interface Builder i interakcji z nimi w kodzie._

Podczas pracy z C# i .NET w aplikacji platformy Xamarin.Mac, masz dostęp do tej samej strukturze AppKit formantów, które osoba używająca *języka Objective-C* i *Xcode* jest. Ponieważ rozszerzenia Xamarin.Mac integruje się bezpośrednio za pomocą edytora Xcode, można użyć program Xcode _programu Interface Builder_ do tworzenia i obsługi kontrolek Appkit (lub opcjonalnie utworzyć je bezpośrednio w kodzie języka C#).

Kontrolek AppKit są elementy interfejsu użytkownika, które są używane do tworzenia interfejsu użytkownika aplikacji platformy Xamarin.Mac. One składają się z elementów, takich jak przyciski, etykiety, pola tekstowe, pola wyboru i kontrolki Segmentowane i spowodować natychmiastowe akcje lub wynikami widoczna po użytkownik modyfikuje je.

[![](standard-controls-images/intro01.png "Przykładowy ekran główny aplikacji")](standard-controls-images/intro01.png#lightbox)

W tym artykule omówiono podstawowe informacje dotyczące pracy z kontrolek AppKit w aplikacji platformy Xamarin.Mac. Zdecydowanie zalecane jest, pracy za pośrednictwem [Witaj, Mac](~/mac/get-started/hello-mac.md) artykuł najpierw, w szczególności [wprowadzenie do programu Xcode i programu Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) i [gniazd i akcje](~/mac/get-started/hello-mac.md#outlets-and-actions) sekcje w postaci, w jakiej omawia kluczowe założenia i technik, które będzie używany w tym artykule.

Możesz chcieć spojrzeć na [udostępnianie klasy języka C# / metod języka Objective-C](~/mac/internals/how-it-works.md) części [elementach wewnętrznych rozszerzenia Xamarin.Mac](~/mac/internals/how-it-works.md) dokumentów, jak również wyjaśniono `Register` i `Export` poleceń Umożliwia o komunikacji sieciowej w górę klas języka C# do języka Objective-C obiektów i elementów interfejsu użytkownika.

<a name="Introduction_to_Controls_and_Views" />

## <a name="introduction-to-controls-and-views"></a>Wprowadzenie do kontrolek i widoków

macOS (wcześniej znane jako systemu Mac OS X) zawiera standardowy zestaw kontrolek interfejsu użytkownika za pośrednictwem strukturze AppKit. One składają się z elementów, takich jak przyciski, etykiety, pola tekstowe, pola wyboru i kontrolki Segmentowane i spowodować natychmiastowe akcje lub wynikami widoczna po użytkownik modyfikuje je.

Wszystkich kontrolek AppKit ma wygląd standardowego, wbudowanego, który będzie odpowiedni dla większości zastosowań, niektóre Określ wygląd alternatywnego do użycia w obszarze ramki okna lub w _efekt jaskrawość_ kontekstu, takich jak obszar paska bocznego lub w Element widget Centrum powiadomień.

Apple zasugerować następujące wskazówki podczas pracy z kontrolek AppKit

- Unikaj łączenia operacji rozmiarów kontroli w jednym widoku.
- Ogólnie rzecz biorąc Unikaj, zmiana rozmiaru formantów w pionie.
- Użyj czcionki systemowej i rozmiar prawidłowego tekstu w formancie.
- Za pomocą właściwych odstępów między kontrolkami.

Aby uzyskać więcej informacji, zobacz pleas [widoków i kontrolek o](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsAll.html#//apple_ref/doc/uid/20000957-CH46-SW1) części firmy Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/).

<a name="Using_Controls_in_a_Window_Frame" />

### <a name="using-controls-in-a-window-frame"></a>Korzystanie z kontrolek ramkę okna

Brak podzbiór kontrolek AppKit, które obejmują styl wyświetlania, która to umożliwia uwzględnienie w oknie ramki obszaru. Aby uzyskać przykład zobacz paska narzędzi aplikacji poczty:

[![](standard-controls-images/mailapp.png "Ramkę okna Mac")](standard-controls-images/mailapp.png#lightbox)

- **ROUND teksturą przycisk** — `NSButton` ze stylem z `NSTexturedRoundedBezelStyle`.
- **Tekstury zaokrąglone kontrolki Segmentowane** — `NSSegmentedControl` ze stylem z `NSSegmentStyleTexturedRounded`.
- **Tekstury zaokrąglone kontrolki Segmentowane** — `NSSegmentedControl` ze stylem z `NSSegmentStyleSeparated`.
- **ROUND teksturą Menu podręcznego** — `NSPopUpButton` ze stylem z `NSTexturedRoundedBezelStyle`.
- **ROUND teksturą Menu rozwijanego** — `NSPopUpButton` ze stylem z `NSTexturedRoundedBezelStyle`.
- **Pasek wyszukiwania** — `NSSearchField`.

Apple zasugerować następujące wskazówki podczas pracy z kontrolek AppKit w ramkę okna

- Nie używaj stylów określonego formantu ramki okna w treści okna.
- Nie używaj kontrolki okna treści lub style w ramki okna.

Aby uzyskać więcej informacji, zobacz pleas [widoków i kontrolek o](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsAll.html#//apple_ref/doc/uid/20000957-CH46-SW1) części firmy Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/).

<a name="Creating_a_User_Interface_in_Interface_Builder" />

## <a name="creating-a-user-interface-in-interface-builder"></a>Tworzenie interfejsu użytkownika w programu Interface Builder

Kiedy tworzysz nową aplikację cocoa dla platformy Xamarin.Mac, otrzymasz standardowego okna puste, domyślnie. Ten system windows jest zdefiniowany w `.storyboard` pliku automatycznie uwzględnione w projekcie. Aby edytować projektu systemu windows w **Eksploratora rozwiązań**, kliknij dwukrotnie `Main.storyboard` pliku:

[![](standard-controls-images/edit01.png "Wybieranie głównego Storyboard w Eksploratorze rozwiązań")](standard-controls-images/edit01.png#lightbox)

Spowoduje to otwarcie projektu okna w program Xcode Interface Builder:

[![](standard-controls-images/edit02.png "Edytowanie scenorysu w środowisku Xcode")](standard-controls-images/edit02.png#lightbox)

Aby utworzyć interfejs użytkownika, będzie przeciągnij elementy interfejsu użytkownika (kontrolek AppKit) z **Inspektor biblioteki** do **Edytor interfejsu** w programu Interface Builder. W poniższym przykładzie **widok Podziel pionowo** kontrolki została leków z **Inspektor biblioteki** i umieszczone na oknie **Edytor interfejsu**:

[![](standard-controls-images/edit03.png "Wybieranie widoku złożonego z biblioteki")](standard-controls-images/edit03.png#lightbox)

Aby uzyskać więcej informacji na temat tworzenia interfejsu użytkownika w programu Interface Builder, zobacz nasze [wprowadzenie do programu Xcode i programu Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) dokumentacji.

<a name="Sizing_and_Positioning" />

### <a name="sizing-and-positioning"></a>Dostosuj rozmiar i położenie

Po kontrolki została uwzględniona w interfejsie użytkownika, użyj **edytora ograniczeń** jej lokalizacja i rozmiar, wprowadzając wartości ręcznie i kontrolować sposób sterowania zostanie automatycznie umieszczony i rozmiar, gdy do nadrzędnego okna lub widoku zmiany rozmiaru:

[![](standard-controls-images/edit04.png "Ustawianie ograniczeń")](standard-controls-images/edit04.png#lightbox)

Użyj **czerwoną I świateł** wokół zewnętrznej **Autoresizing** pole, aby _hokejowego_ formant do danej (x, y) lokalizacji. Na przykład: 

[![](standard-controls-images/edit05.png "Edytowanie ograniczenia")](standard-controls-images/edit05.png#lightbox)

Określa, że wybranej kontrolki (w **Widok hierarchii** & **Edytor interfejsu**) będzie zablokowana do lokalizacji górnego i prawego widoku lub okna zgodnie z jego rozmiar lub przenieść. 

Inne elementy Edytor właściwości kontrolki, takie jak szerokość i wysokość:

[![](standard-controls-images/edit06.png "Ustawianie wysokości")](standard-controls-images/edit06.png#lightbox)

Można także kontrolować sposób wyrównania elementów z ograniczeniami przy użyciu **edytora wyrównanie**:

[![](standard-controls-images/edit07.png "Edytor wyrównania")](standard-controls-images/edit07.png#lightbox)

> [!IMPORTANT]
> W przeciwieństwie do systemu iOS gdzie (0,0) jest w prawym górnym lewym dolnym rogu ekranu, w systemie macOS (0,0) to w dolnym lewym dolnym rogu. Jest to spowodowane z systemem macOS za pomocą matematyczne współrzędnych wartości liczbowe, zwiększenie wartości w górę i w prawo. Należy wziąć pod uwagę w przypadku umieszczenia kontrolek AppKit w interfejsie użytkownika.

<a name="Setting_a_Custom_Class" />

### <a name="setting-a-custom-class"></a>Ustawienia niestandardowej klasy

Brak razy podczas pracy z kontrolek AppKit, które będzie wymagane podklasy i istniejącej kontrolki i utworzyć własne niestandardowe wersję tej klasy. Na przykład zdefiniowanie niestandardowej wersji listy źródeł:

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

Gdzie `[Register("SourceListView")]` ujawnia instrukcji `SourceListView` klasy Objective-C, dlatego mogą być używane w programu Interface Builder. Aby uzyskać więcej informacji, zobacz [udostępnianie klasy języka C# / metod języka Objective-C](~/mac/internals/how-it-works.md) części [elementach wewnętrznych rozszerzenia Xamarin.Mac](~/mac/internals/how-it-works.md) dokumencie wyjaśniono `Register` i `Export` polecenia używane przewodowy zapasowe klas języka C# do języka Objective-C obiektów i elementów interfejsu użytkownika.

Za pomocą powyższego kodu w miejscu, można przeciągnąć kontrolkę AppKit typu podstawowego, który jest kolejnym krokiem na powierzchni projektowej (w poniższym przykładzie **listy źródeł**), przejdź do **Inspektor tożsamości** i ustaw **niestandardowe klasy** na nazwę która widoczne języka Objective-C (przykład `SourceListView`):

[![](standard-controls-images/edit10.png "Ustawienia niestandardowej klasy w środowisku Xcode")](standard-controls-images/edit10.png#lightbox)

<a name="Exposing_Outlets_and_Actions" />

### <a name="exposing-outlets-and-actions"></a>Udostępnianie gniazd i akcje

Zanim kontrolka w postaci AppKit można uzyskiwać w kodzie języka C#, musi być udostępniane jako **ujścia** lub i **akcji**. Zrobić to wybrać daną kontrolkę albo **hierarchii interfejsów** lub **Edytor interfejsu** i przełącz się do **Asystenta ustawień widoku** (Upewnij się, że `.h`okna wybrane do edycji):

[![](standard-controls-images/edit11.png "Wybieranie odpowiedniego pliku do edycji")](standard-controls-images/edit11.png#lightbox)

Przeciągnij formant z formantu AppKit na danym `.h` plik, aby rozpocząć tworzenie **ujścia** lub **akcji**:

[![](standard-controls-images/edit12.png "Przeciąganie w celu utworzenia źródła lub akcji")](standard-controls-images/edit12.png#lightbox)

Wybierz typ zagrożeń, aby utworzyć i nadać **ujścia** lub **akcji** **nazwa**: 

[![](standard-controls-images/edit13.png "Konfigurowanie źródła lub akcji")](standard-controls-images/edit13.png#lightbox)


Aby uzyskać więcej informacji na temat pracy z **gniazd** i **akcje**, zobacz [gniazd i akcje](~/mac/get-started/hello-mac.md#outlets-and-actions) części naszych [wprowadzenie do programu Xcode i interfejsu Konstruktor](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) dokumentacji.

<a name="Synchronizing_Changes_with_Xcode" />

### <a name="synchronizing-changes-with-xcode"></a>Synchronizowanie zmian z narzędziem Xcode

Po przełączeniu się do programu Visual Studio dla komputerów Mac z narzędzia Xcode, wszelkie zmiany wprowadzone w programie Xcode automatycznie zostaną zsynchronizowane z projektem platformy Xamarin.Mac.

Jeśli wybierzesz `SplitViewController.designer.cs` w **Eksploratora rozwiązań** będzie można zobaczyć jak swoje **ujścia** i **akcji** zostały przewodowej się w naszym kodzie języka C#:

[![](standard-controls-images/sync01.png "Synchronizowanie zmian z narzędziem Xcode")](standard-controls-images/sync01.png#lightbox)

Zwróć uwagę jak definicja w `SplitViewController.designer.cs` pliku:

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

Jak widać, Visual Studio dla komputerów Mac wykrywa zmiany `.h` pliku i automatycznie synchronizuje zmiany we właściwej `.designer.cs` plik, aby udostępnić je do aplikacji. Należy również zauważyć, że `SplitViewController.designer.cs` jest klasę częściową, tak aby program Visual Studio for Mac nie trzeba modyfikować `SplitViewController.cs ` który spowodowałoby zastąpienie wszelkie zmiany, które zostały wprowadzone do klasy.

Zwykle nigdy nie należy otwierać `SplitViewController.designer.cs` samodzielnie, jego został przedstawiony tutaj wyłącznie w celach edukacyjnych.

> [!IMPORTANT]
> W większości sytuacji Visual Studio for Mac zostanie automatycznie zobaczyć wszelkie zmiany wprowadzone w środowisku Xcode i synchronizować je do projektu platformy Xamarin.Mac. W wystąpieniu wyłączone synchronizacji nie jest realizowane automatycznie Przełącz się do środowiska Xcode i ich do programu Visual Studio dla komputerów Mac ponownie. Zwykle spowoduje to rozpoczęcie cyklu synchronizacji.

<a name="Working_with_Buttons" />

## <a name="working-with-buttons"></a>Praca z przycisków

AppKit zawiera kilka typów przycisku, który może służyć w projekcie interfejsu użytkownika. Aby uzyskać więcej informacji, zobacz [przyciski](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsButtons.html#//apple_ref/doc/uid/20000957-CH48-SW1) części firmy Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). 

[![](standard-controls-images/buttons01.png "Przykładem typów inny przycisk")](standard-controls-images/buttons01.png#lightbox)

Jeśli przycisk została udostępniona za pośrednictwem **ujścia**, poniższy kod będzie odpowiadać jej naciskana:

```csharp
ButtonOutlet.Activated += (sender, e) => {
        FeedbackLabel.StringValue = "Button Outlet Pressed";
};
```

W przypadku przycisków, która została udostępniona za pośrednictwem **akcje**, `public partial` metoda zostanie automatycznie utworzona z nazwą, która została wybrana w narzędziu Xcode. Aby odpowiedzieć na **akcji**, ukończenie metody częściowej klasy, **akcji** została zdefiniowana w. Na przykład:

```csharp
partial void ButtonAction (Foundation.NSObject sender) {
    // Do something in response to the Action
    FeedbackLabel.StringValue = "Button Action Pressed";
}
```

Dla przycisków, które mają stan (takich jak **na** i **poza**), stan można sprawdzić lub ustawić za pomocą `State` właściwości `NSCellStateValue` wyliczenia. Na przykład:

```csharp
DisclosureButton.Activated += (sender, e) => {
    LorumIpsum.Hidden = (DisclosureButton.State == NSCellStateValue.On);
};
```

Gdzie `NSCellStateValue` może być:

- **Na** — przycisk jest naciśnięty lub formant jest zaznaczony (na przykład zaznacz pole wyboru).
- **Wyłącz** — przycisk nie jest naciśnięty lub kontrolka nie jest zaznaczone.
- **Mieszane** -kombinację **na** i **poza** stanów.

<a name="Mark-a-Button-as-Default-and-Set-Key-Equivalent" />

### <a name="mark-a-button-as-default-and-set-key-equivalent"></a>Oznacz przycisk, ponieważ domyślne, a ustawiony odpowiednik klucza

Dla przycisku, które zostały dodane do projektu interfejsu użytkownika, można oznaczyć ten przycisk jako _domyślne_ przycisku, który zostanie aktywowany, gdy użytkownik naciśnie **Return/Enter** kluczowe na klawiaturze. W systemie macOS ten przycisk, zostanie wyświetlony kolorem niebieskim tle domyślnie.

Aby przycisk Ustaw jako domyślne, wybierz ją w program Xcode Interface Builder. Dalej w **Inspektor atrybut**, wybierz opcję **klucz równoważny** pole i naciśnij klawisz **Return/Enter** klucza:

[![](standard-controls-images/buttons03.png "Edytowanie odpowiednik klucza")](standard-controls-images/buttons03.png#lightbox)

Tak samo możesz przypisać sekwencję klawiszy, który może służyć do aktywowania przycisk za pomocą klawiatury zamiast myszy. Na przykład przez naciśnięcie klawisza klawisze Command-C na powyższej ilustracji.

Gdy aplikacja jest uruchamiana okno z przyciskiem jest klucz i fokus, gdy użytkownik naciśnie polecenie-C, akcji dla przycisku zostanie aktywowany (jako — Jeśli użytkownik ma kliknął przycisk).

<a name="Working_with_Checkboxes_and_Radio_Buttons" />

## <a name="working-with-checkboxes-and-radio-buttons"></a>Praca z polami wyboru i przyciski radiowe

AppKit zawiera kilka typów pól wyboru i grup przycisków radiowych, który może służyć w projekcie interfejsu użytkownika. Aby uzyskać więcej informacji, zobacz [przyciski](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsButtons.html#//apple_ref/doc/uid/20000957-CH48-SW1) części firmy Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). 

[![](standard-controls-images/buttons02.png "Przykład pola wyboru dostępne typy")](standard-controls-images/buttons02.png#lightbox)


Pola wyboru i przyciski radiowe (udostępniane za pośrednictwem **gniazd**) mają stan (takich jak **na** i **poza**), stan można sprawdzić lub ustawić za pomocą `State` właściwości `NSCellStateValue` wyliczenia. Na przykład:

```csharp
AdjustTime.Activated += (sender, e) => {
    FeedbackLabel.StringValue = string.Format("Adjust Time: {0}",AdjustTime.State == NSCellStateValue.On);
};
```

Gdzie `NSCellStateValue` może być:

- **Na** — przycisk jest naciśnięty lub formant jest zaznaczony (na przykład zaznacz pole wyboru).
- **Wyłącz** — przycisk nie jest naciśnięty lub kontrolka nie jest zaznaczone.
- **Mieszane** -kombinację **na** i **poza** stanów.

Wybierz przycisk w grupie przycisków radiowych, ujawnić przycisk radiowy, aby wybrać jako **ujścia** i ustaw jego `State` właściwości. Na przykład:

```csharp
partial void SelectCar (Foundation.NSObject sender) {
    TransportationCar.State = NSCellStateValue.On;
    FeedbackLabel.StringValue = "Car Selected";
}
```

Aby uzyskać kolekcję przycisków radiowych do działania jako grupą i automatycznie obsługiwać wybranego stanu, Utwórz nowy **akcji** i do niej dołączyć każdy przycisk w grupie:

![](standard-controls-images/buttons04.png "Tworzenie nowej akcji")

Następnie należy przypisać unikatową `Tag` do każdego przycisku radiowego w **Inspektor atrybut**:

![](standard-controls-images/buttons05.png "Edytowanie tagów przycisku radiowego")

Zapisz zmiany i powrócić do programu Visual Studio dla komputerów Mac, Dodaj kod, aby obsłużyć **akcji** wszystkie przyciski radiowe są dołączone do:

```csharp
partial void NumberChanged(Foundation.NSObject sender)
{
    var check = sender as NSButton;
    Console.WriteLine("Changed to {0}", check.Tag);
}
```

Możesz użyć `Tag` właściwości, aby zobaczyć, który przycisk radiowy został wybrany.

<a name="Working_with_Menu_Controls" />

## <a name="working-with-menu-controls"></a>Praca z kontrolkami Menu

AppKit zawiera kilka typów formantów Menu, który może służyć w projekcie interfejsu użytkownika. Aby uzyskać więcej informacji, zobacz [kontrolek Menu](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlswithMenus.html#//apple_ref/doc/uid/20000957-CH100-SW1) części firmy Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). 

[![](standard-controls-images/menu01.png "Przykład menu formantów")](standard-controls-images/menu01.png#lightbox)

<a name="Providing-Menu-Control-Data" />

### <a name="providing-menu-control-data"></a>Dostarcza dane formant Menu

Formanty Menu dostępne dla systemu macOS można ustawić do wypełnienia listy rozwijanej opcję z listy wewnętrznych, (które mogą być wstępnie zdefiniowane w programu Interface Builder lub wypełnione przy użyciu kodu) lub podając źródła danych niestandardowych, zewnętrznych.

<a name="Working-with-Internal-Data" />

#### <a name="working-with-internal-data"></a>Praca z danymi wewnętrznego

Oprócz definiowania elementów w programu Interface Builder kontrolek Menu (takie jak `NSComboBox`), zapewniają kompletny zestaw metod, które pozwalają na dodawanie, edytowanie lub usunąć elementy z listy wewnętrznych, na których działają:

- `Add` — Dodaje nowy element na końcu listy.
- `GetItem` -Zwraca element pod danym indeksem.
- `Insert` -Wstawia nowy element na liście w podanej lokalizacji.
- `IndexOf` -Zwraca indeks danego elementu.
- `Remove` -Usuwa określony element z listy.
- `RemoveAll` -Usuwa wszystkie elementy z listy.
- `RemoveAt` -Usuwa element pod danym indeksem.
- `Count` — Zwraca liczbę elementów na liście.

> [!IMPORTANT]
> Jeśli używasz źródła danych Extern (`UsesDataSource = true`), dowolne z powyższych metod wywoływania spowoduje zgłoszenie wyjątku.

<a name="Working-with-an-External-Data-Source" />

#### <a name="working-with-an-external-data-source"></a>Praca z zewnętrznego źródła danych

Zamiast używania wbudowanych danych wewnętrznych, aby zapewnić wierszy dla kontrolki Menu, można opcjonalnie Użyj zewnętrznego źródła danych i Podaj własny magazyn zapasowy elementów (np. bazy danych SQLite).

Aby pracować z zewnętrznego źródła danych, należy utworzyć wystąpienie źródła danych, formant Menu (`NSComboBoxDataSource` na przykład) i przesłonić kilku metod w celu zapewnienia niezbędnych danych:

- `ItemCount` — Zwraca liczbę elementów na liście.
- `ObjectValueForItem` — Zwraca wartość elementu dla podanego indeksu.
- `IndexOfItem` -Zwraca indeks zapewniają wartość elementu.
- `CompletedString` — Zwraca pierwszy pasującej wartości elementu wartość elementu częściowo wpisane. Ta metoda jest wywoływana tylko, jeśli włączono automatycznego uzupełniania (`Completes = true`).

Zobacz [baz danych i polach kombi](~/mac/app-fundamentals/databases.md#Databases-and-ComboBoxes) części [Praca z bazami danych](~/mac/app-fundamentals/databases.md) dokumentu, aby uzyskać więcej informacji.

<a name="Adjusting-the-Lists-Appearance" />

### <a name="adjusting-the-lists-appearance"></a>Dostosowywanie wyglądu listy

Można dostosować wygląd formantu Menu są następujące metody:

- `HasVerticalScroller` -Jeśli `true`, kontrolka będzie wyświetlać pionowy pasek przewijania. 
- `VisibleItems` — Dostosuj liczbę elementów wyświetlanych po otwarciu formantu. Wartość domyślna to pięć (5).
- `IntercellSpacing` — Dostosuj ilość miejsca wokół danego elementu, zapewniając `NSSize` gdzie `Width` określa lewego i prawego marginesu oraz `Height` Określa odstęp przed i po elemencie.
- `ItemHeight` -Określa wysokość każdego elementu na liście.

Lista rozwijana typu `NSPopupButtons`, pierwszy element Menu udostępnia tytuł dla formantu. Na przykład: 

[![](standard-controls-images/menu02.png "Przykład formant menu")](standard-controls-images/menu02.png#lightbox)

Aby zmienić tytuł, udostępnić ten element jako **ujścia** i użyć kodu, podobnie do poniższego:

```csharp
DropDownSelected.Title = "Item 1";
```

<a name="Manipulating-the-Selected-Items" />

### <a name="manipulating-the-selected-items"></a>Manipulowanie wybrane elementy

Poniżej przedstawiono metody i właściwości Zezwalaj na wykonywanie operacji na wybrane elementy na liście formant Menu:

- `SelectItem` -Wybiera element pod danym indeksem.
- `Select` -Wybierz wartość danego elementu.
- `DeselectItem` -Usuwa zaznaczenie elementu pod danym indeksem.
- `SelectedIndex` -Zwraca indeks aktualnie wybranego elementu.
- `SelectedValue` — Zwraca wartość obecnie wybranego elementu.

Użyj `ScrollItemAtIndexToTop` do prezentowania elementu pod danym indeksem w górnej części listy i `ScrollItemAtIndexToVisible` przewiń do listy, dopóki nie będzie widoczna pozycja pod danym indeksem.

<a name="Responding to Events" />

### <a name="responding-to-events"></a>Odpowiadanie na zdarzenia

Menu elementy sterujące udostępniają następujące zdarzenia w odpowiedzi na interakcję z użytkownikiem:

- `SelectionChanged` -Jest wywoływana, gdy użytkownik wybrał wartość z listy.
- `SelectionIsChanging` -Jest wywoływana przed nowego elementu wybranego użytkownika staje się aktywnego zaznaczenia.
- `WillPopup` -Jest wywoływana, zanim zostanie wyświetlona lista rozwijana lista elementów.
- `WillDismiss` -Jest wywoływana przed zamknięciem listę rozwijaną listę elementów.

Dla `NSComboBox` formantów, obejmują one wszystkie te same zdarzenia jako `NSTextField`, takich jak `Changed` zdarzenia, które jest wywoływane, gdy użytkownik edytuje wartość w polu kombi.

Opcjonalnie możesz reagować z wyprzedzeniem wewnętrzne elementy Menu danych zdefiniowanych w programu Interface Builder wybierane przez dołączenie elementu **akcji** i reagować na przy użyciu kodu podobne do następujących **akcji** Trwa wywołany przez użytkownika:

```csharp
partial void ItemOne (Foundation.NSObject sender) {
    DropDownSelected.Title = "Item 1";
    FeedbackLabel.StringValue = "Item One Selected";
}
```

Aby uzyskać więcej informacji na temat pracy z menu i Menu formantów, zobacz nasze [menu](~/mac/user-interface/menu.md) i [przycisk podręczne i wyświetlenie rozwijanego](~/mac/user-interface/menu.md) dokumentacji.

<a name="Working_with_Selection_Controls" />

## <a name="working-with-selection-controls"></a>Praca z kontrolki wyboru

AppKit zawiera kilka typów kontrolki wyboru, które mogą być używane w projekcie interfejsu użytkownika. Aby uzyskać więcej informacji, zobacz [kontrolki wyboru](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsSelection.html#//apple_ref/doc/uid/20000957-CH49-SW1) części firmy Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). 

[![](standard-controls-images/select01.png "Przykład kontrolki wyboru")](standard-controls-images/select01.png#lightbox)

Istnieją dwa sposoby, aby sprawdzić, kiedy kontrolkę wyboru ma interakcji użytkownika, umieszczając go w formie **akcji**. Na przykład:

```csharp
partial void SegmentButtonPressed (Foundation.NSObject sender) {
    FeedbackLabel.StringValue = string.Format("Button {0} Pressed",SegmentButtons.SelectedSegment);
}
```

Lub, dołączając **delegata** do `Activated` zdarzeń. Na przykład:

```csharp
TickedSlider.Activated += (sender, e) => {
    FeedbackLabel.StringValue = string.Format("Stepper Value: {0:###}",TickedSlider.IntValue);
};
```

Aby ustawić lub odczytać wartości kontrolkę wyboru, należy użyć `IntValue` właściwości. Na przykład:

```csharp
FeedbackLabel.StringValue = string.Format("Stepper Value: {0:###}",TickedSlider.IntValue);
```

Formanty specjalistycznych, (na przykład również koloru i obrazu dobrze) mają właściwości specyficzne dla ich typów wartości. Na przykład:

```csharp
CollorWell.Color = NSColor.Red;
ImageWell.Image = NSImage.ImageNamed ("tag.png");

```

`NSDatePicker` Ma następujące właściwości do pracy bezpośrednio z datą i godziną:

- **DateValue** -bieżącą wartość daty i godziny jako `NSDate`.
- **Lokalne** — od lokalizacji użytkownika jako `NSLocal`.
- **TimeInterval** — wartość godziny jako `Double`.
- **Strefa czasowa** -strefy czasowej użytkownika jako `NSTimeZone`.

<a name="Working_with_Indicator_Controls" />

## <a name="working-with-indicator-controls"></a>Praca z kontrolkami wskaźnika

AppKit zawiera kilka typów formantów wskaźnika, który może służyć w projekcie interfejsu użytkownika. Aby uzyskać więcej informacji, zobacz [kontrolki wskaźnik](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsIndicators.html#//apple_ref/doc/uid/20000957-CH50-SW1) części firmy Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). 

[![](standard-controls-images/level01.png "Przykład wskaźnika formantów")](standard-controls-images/level01.png#lightbox)

Istnieją dwa sposoby, aby sprawdzić, kiedy wskaźnik kontrolkę interakcji z użytkownikiem, albo przez uwidaczniania go jako **akcji** lub **ujścia** i dołączanie **delegata** do `Activated`zdarzeń. Na przykład:

```csharp
LevelIndicator.Activated += (sender, e) => {
    FeedbackLabel.StringValue = string.Format("Level: {0:###}",LevelIndicator.DoubleValue);
};
```

Aby odczytać lub ustawić wartość kontrolki wskaźnik, należy użyć `DoubleValue` właściwości. Na przykład:

```csharp
FeedbackLabel.StringValue = string.Format("Rating: {0:###}",Rating.DoubleValue);
```

Po wyświetleniu powinien animowane nieokreślone i asynchroniczne wskaźniki postępu. Użyj `StartAnimation` metody do uruchamiania animacji, gdy są one wyświetlane. Na przykład:

```csharp
Indeterminate.StartAnimation (this);
AsyncProgress.StartAnimation (this);
```

Wywoływanie `StopAnimation` metoda przestanie animacji.

<a name="Working_with_Text_Controls" />

## <a name="working-with-text-controls"></a>Praca z kontrolek tekstu

AppKit zawiera kilka typy kontrolek tekstu, który może służyć w projekcie interfejsu użytkownika. Aby uzyskać więcej informacji, zobacz [kontrolek tekstu](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsText.html#//apple_ref/doc/uid/20000957-CH51-SW1) części firmy Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). 

[![](standard-controls-images/text01.png "Przykład kontrolek tekstu")](standard-controls-images/text01.png#lightbox)

Dla pól tekstowych (`NSTextField`), następujące zdarzenia mogą służyć do śledzenia interakcji z użytkownikiem:

- **Zmienione** — jest uruchamiany w dowolnym momencie użytkownik zmieni wartość pola. Na przykład w każdym wpisany znak.
- **EditingBegan** — jest wywoływane, gdy użytkownik wybierze pól do edycji.
- **EditingEnded** — gdy użytkownik naciśnie klawisz Enter w polu lub opuszczania pola.

Użyj `StringValue` właściwości do odczytu lub ustaw wartość pola. Na przykład:

```csharp
FeedbackLabel.StringValue = string.Format("User ID: {0}",UserField.StringValue);
```

W przypadku pól, które wyświetlenia lub zmodyfikowania wartości liczbowe, można użyć `IntValue` właściwości. Na przykład:

```csharp
FeedbackLabel.StringValue = string.Format("Number: {0}",NumberField.IntValue);
```

`NSTextView` Zapewnia pełny tekst polecane Edytuj i Wyświetl obszar za pomocą wbudowanych formatowania. Podobnie jak `NSTextField`, użyj `StringValue` właściwości do odczytu lub ustaw wartość obszaru.

Na przykład złożonego przykładu pracy z widokami tekstu w aplikacji rozszerzenia Xamarin.Mac zobacz [SourceWriter przykładową aplikację](https://developer.xamarin.com/samples/mac/SourceWriter/). SourceWriter jest proste źródła edytora kodu, który zapewnia obsługę uzupełniania kodu i wyróżniania składni proste.

Kod SourceWriter została ujęta w pełni i ile to możliwe, linki zostały podane z technologii kluczy lub metod do odpowiednich informacji w dokumentacji przewodniki platformy Xamarin.Mac.

<a name="Working_with_Content_Views" />

## <a name="working-with-content-views"></a>Praca z widokami zawartości

AppKit zawiera kilka typów zawartości widoków, które mogą być używane w projekcie interfejsu użytkownika. Aby uzyskać więcej informacji, zobacz [widoki zawartości](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsView.html#//apple_ref/doc/uid/20000957-CH52-SW1) części firmy Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/).

[![](standard-controls-images/content01.png "Przykładowy widok zawartości")](standard-controls-images/content01.png#lightbox)

<a name="Popovers" />

### <a name="popovers"></a>Popovers

Popover jest przejściowy element interfejsu użytkownika, który zawiera funkcje, które są bezpośrednio związane z konkretną kontrolkę lub obszar na ekranie. Popover liczby zmiennoprzecinkowe nad okno, które zawiera formant lub obszar, który jest powiązany z i jego obramowanie zawiera strzałkę, aby wskazać punkt, z którego okazało się.

Aby utworzyć popover, wykonaj następujące czynności:

1. Otwórz `.storyboard` pliku okno, które chcesz dodać popover do przez dwukrotne kliknięcie go w **Eksploratora rozwiązań**
2. Przeciągnij **wyświetlić kontroler** z **Inspektor biblioteki** na **interfejs edytora**: 

    [![](standard-controls-images/content02.png "Wybiera kontroler widoku z biblioteki")](standard-controls-images/content02.png#lightbox)
4. Zdefiniuj rozmiaru oraz układu **widok niestandardowy**: 

    [![](standard-controls-images/content04.png "Edytowanie układu")](standard-controls-images/content04.png#lightbox)
5. Klawisz Control, kliknij i przeciągnij od źródła okna podręcznego na **kontrolera widoku**: 

    [![](standard-controls-images/content05.png "Przeciąganie w celu utworzenia segue")](standard-controls-images/content05.png#lightbox)
6. Wybierz **Popover** z menu podręczne: 

    [![](standard-controls-images/content06.png "Ustawianie typu segue")](standard-controls-images/content06.png#lightbox)
7. Zapisz zmiany i powrócić do programu Visual Studio dla komputerów Mac do synchronizacji z narzędziem Xcode.

<a name="Tab_Views" />

### <a name="tab-views"></a>Karta widoków

Widoki karta zawiera listę kartę (które wygląda podobnie do kontrolki Segmentowane) w połączeniu z zestaw widoków, które są wywoływane _okienka_. Gdy użytkownik wybierze nową kartę, pojawi się okienko, który jest dołączony do niego. Każde okienko zawiera swój własny zestaw kontrolek.

Podczas pracy z karty widoku w program Xcode Interface Builder, użyj **Inspektor atrybut** ustawić liczbę kart:

[![](standard-controls-images/content08.png "Liczba kart do edycji")](standard-controls-images/content08.png#lightbox)

Wybierz każda karta **hierarchii interfejsów** można ustawić jego **tytuł** i dodać elementy interfejsu użytkownika do jego **okienko**:

[![](standard-controls-images/content09.png "Edytowanie karty w środowisku Xcode")](standard-controls-images/content09.png#lightbox)

<a name="Data_Binding_AppKit_Controls" />

## <a name="data-binding-appkit-controls"></a>Kontrolek AppKit powiązania danych

Przy użyciu technik kodowania pary klucz-wartość i powiązania danych w aplikacji platformy Xamarin.Mac, może znacznie skrócić ilość kodu, który trzeba napisać i obsługiwać do wypełniania i pracować z elementami interfejsu użytkownika. Masz także zaletą dalsze oddzielenie danych zapasowy (_modelu danych_) z usługi frontonu zakończenia interfejsu użytkownika (_Model-View-Controller_), prowadzącego do łatwiejsza Obsługa bardziej elastycznych aplikacji Projekt.

Kodowanie klucz-wartość (KVC) to mechanizm służący do uzyskiwania dostępu do właściwości obiektu pośrednio, za pomocą klucze (specjalnie sformatowanego ciągi), aby zidentyfikować właściwości zamiast uzyskanie do nich dostępu za pośrednictwem zmienne wystąpienia lub metody dostępu (`get/set`). Wdrażając kodowanie klucz-wartość zgodne metod dostępu do aplikacji platformy Xamarin.Mac, możesz uzyskać dostęp do innych funkcji systemu macOS, takich jak obserwowania pary klucz-wartość (KVO), powiązań danych, danych podstawowych, cocoa dla powiązania i scriptability.

Aby uzyskać więcej informacji, zobacz [proste powiązanie danych](~/mac/app-fundamentals/databinding.md#Simple_Data_Binding) części naszych [powiązanie danych i kodowanie klucz-wartość](~/mac/app-fundamentals/databinding.md) dokumentacji.


<a name="Summary" />

## <a name="summary"></a>Podsumowanie

W tym artykule jest zajęta szczegółowe omówienie pracy z standardowych kontrolek AppKit takie jak przyciski, etykiety, pola tekstowe, pola wyboru i kontrolki Segmentowane w aplikacji platformy Xamarin.Mac. Objęte go, dodając je do projektu interfejsu użytkownika, w program Xcode Interface Builder, Uwidacznianie ich dla kodu za pośrednictwem gniazd i działań i pracy z kontrolek AppKit w kodzie języka C#.

## <a name="related-links"></a>Linki pokrewne

- [MacControls (przykład)](https://developer.xamarin.com/samples/mac/MacControls/)
- [Witaj, Mac](~/mac/get-started/hello-mac.md)
- [Windows](~/mac/user-interface/window.md)
- [Powiązanie danych i kodowanie klucz-wartość](~/mac/app-fundamentals/databinding.md)
- [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Formanty i widoki — informacje](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsAll.html#//apple_ref/doc/uid/20000957-CH46-SW1)
