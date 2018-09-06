---
title: Obrazy platformy Xamarin.Mac
description: Ten artykuł dotyczy pracy z obrazów i ikon w aplikacji platformy Xamarin.Mac. Opisano w nim tworzenia i utrzymywania obrazów potrzebne do utworzenia ikony swojej aplikacji i używanie obrazów w kodzie C# i program Xcode Interface Builder.
ms.prod: xamarin
ms.assetid: C6B539C2-FC6A-4C38-B839-32BFFB9B16A7
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/15/2017
ms.openlocfilehash: 3bc3c731729f92ae3c5ad6126166892a236e2eab
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/13/2018
ms.locfileid: "43780655"
---
# <a name="images-in-xamarinmac"></a>Obrazy platformy Xamarin.Mac

_Ten artykuł dotyczy pracy z obrazów i ikon w aplikacji platformy Xamarin.Mac. Opisano w nim tworzenia i utrzymywania obrazów potrzebne do utworzenia ikony swojej aplikacji i używanie obrazów w kodzie C# i program Xcode Interface Builder._

## <a name="overview"></a>Omówienie

Podczas pracy z C# i .NET w aplikacji platformy Xamarin.Mac, masz dostęp do tego samego obrazu i Ikona narzędzi dla deweloperów pracujących w *języka Objective-C* i *Xcode* jest.

Istnieje kilka sposobów tego obrazu, którego zasoby są używane w aplikacji (wcześniej znane jako systemu Mac OS X) z systemem macOS. Po prostu wyświetlanie obrazu jako część Interfejsem użytkownika aplikacji, przypisując go do kontrolki interfejsu użytkownika, takie jak pasek narzędzi lub element listy źródłowej, do zapewniania ikony, Xamarin.Mac można łatwo dodawać wspaniałe kompozycji do aplikacji z systemem macOS w następujący sposób : 

- **Elementy interfejsu użytkownika** — obrazy mogą być wyświetlane jako tło lub jako część aplikacji, w widoku obrazu (`NSImageView`).
- **Przycisk** — obrazy mogą być wyświetlane w przyciskach (`NSButton`).
- **Obrazu komórki** — w ramach kontroli tabeli na podstawie (`NSTableView` lub `NSOutlineView`), obrazy mogą być używane w komórce obrazu (`NSImageCell`).
- **Element paska narzędzi** — obrazy mogą być dodawane do paska narzędzi (`NSToolbar`) jako element paska narzędzi obrazów (`NSToolbarItem`).
- **Źródło listy ikonę** — jako część listy źródeł (specjalnie sformatowanego `NSOutlineView`).
- **Ikona aplikacji** -szereg obrazów można grupować w `.icns` ustaw i używany jako ikona aplikacji. Zobacz nasze [ikony aplikacji](~/mac/deploy-test/app-icon.md) dokumentacji, aby uzyskać więcej informacji.

Ponadto system macOS zawiera zestaw wstępnie zdefiniowanych obrazów, które mogą być używane w całej aplikacji.

[![Uruchom przykład w aplikacji](image-images/intro01.png "przykład uruchamianie aplikacji")](image-images/intro01-large.png#lightbox)

W tym artykule omówiono podstawowe informacje dotyczące pracy z obrazów i ikon w aplikacji platformy Xamarin.Mac. Zdecydowanie zalecane jest, pracy za pośrednictwem [Witaj, Mac](~/mac/get-started/hello-mac.md) artykuł najpierw, w szczególności [wprowadzenie do programu Xcode i programu Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) i [gniazd i akcje](~/mac/get-started/hello-mac.md#outlets-and-actions) sekcje w postaci, w jakiej omawia kluczowe założenia i technik, które będzie używany w tym artykule.


## <a name="adding-images-to-a-xamarinmac-project"></a>Dodawanie obrazów do projektu rozszerzenia Xamarin.Mac

Podczas dodawania obrazu do użycia w aplikacji platformy Xamarin.Mac, że dostępne są różne miejsc sposobów, że deweloper może zawierać plik obrazu do źródła projektu:

- **Drzewo głównego projektu [przestarzałe]** — obrazy można dodać bezpośrednio do drzewa projektów. Podczas wywoływania obrazów przechowywanych w drzewie głównego projektu z kodu, lokalizacja folderu nie jest określony. Na przykład: `NSImage image = NSImage.ImageNamed("tags.png");`. 
- **Folderu zasobów [przestarzałe]** -specjalne **zasobów** folder jest dla każdego pliku, który stanie się częścią aplikacji użytkownika pakietu takich jak ikony, uruchom ekranu lub ogólne, obrazy (lub dowolnego innego obrazu lub pliku dewelopera chce dodać). Podczas wywoływania obrazów przechowywanych w **zasobów** folderu z kodu, podobnie jak obrazów przechowywanych w drzewie projektu głównego, lokalizacja folderu nie została określona. Na przykład: `NSImage.ImageNamed("tags.png")`.
- **Niestandardowe Folder lub podfolder [przestarzałe]** — Deweloper można dodać folderu niestandardowego do drzewa źródłowego projektów i przechowywać obrazy w. Lokalizacja, w której plik zostanie dodany, może być zagnieżdżona w podfolderze, aby dodatkowo ułatwić porządkowanie projektu. Na przykład, jeśli dodano Deweloper `Card` folderu projektu i podfolderem folderu `Hearts` do tego folderu, a następnie zapisać obraz **Jack.png** w `Hearts` folderze `NSImage.ImageNamed("Card/Hearts/Jack.png")` będzie załadowania obrazu w środowisko uruchomieniowe.
- **Zestawy obrazu z katalogu zasobów [preferowane]** — dodano w wersji OS X El Capitan **zestawów obrazu wykazów zasobów** zawiera wszystkie wersje lub liczbami w postaci obrazu, które są niezbędne do obsługi różnych urządzeń i skalowanie czynników usługi aplikacja. Zamiast polegania na nazwę pliku zasobów obrazu (**@1x**, **@2x**).

<a name="asset-catalogs" />

### <a name="adding-images-to-an-asset-catalog-image-set"></a>Dodawanie obrazów do obrazu wykazu zasobów zestawu

Jak wspomniano powyżej, **zestawów obrazu wykazów zasobów** zawiera wszystkie wersje lub liczbami w postaci obrazu, które są niezbędne do obsługi różnych urządzeń i skalowanie czynników dla swojej aplikacji. Zamiast polegania na nazwę pliku zasobów obrazu (patrz obrazów niezależnie od rozdzielczości i nomenklatury obraz powyżej), **zestawów obrazu** Określ obraz, który należy do urządzenia i/lub rozwiązania za pomocą edytora zasobów.

1. W **konsoli rozwiązania**, kliknij dwukrotnie **Assets.xcassets** plik, aby otworzyć do edycji: 

    ![Wybieranie Assets.xcassets](image-images/imageset01.png "wybierając Assets.xcassets")
2. Kliknij prawym przyciskiem myszy **listy Assets** i wybierz **nowy zestaw obrazów**: 

    [![Dodawanie nowego zestawu obrazów](image-images/imageset02.png "dodawanie nowy zestaw obrazów")](image-images/imageset02-large.png#lightbox)
3. Wybierz nowy zestaw obrazów i pojawi się Edytor: 

    [![Wybierając nowy zestaw obrazów](image-images/imageset03.png "wybierając nowy zestaw obrazów")](image-images/imageset03-large.png#lightbox)
4. W tym miejscu możemy przeciągnąć na obrazach dla każdego z różnych urządzeń i rozdzielczości wymagane. 
5. Kliknij dwukrotnie nowy zestaw obrazów **nazwa** w **listy Assets** go edytować: 

    [![Edytowanie obrazu Ustaw nazwę](image-images/imageset04.png "edycji obrazu nazwy zestawu")](image-images/imageset04-large.png#lightbox)
    
Specjalny **wektor** klasy, ponieważ została dodana do **zestawów obrazu** temu obejmują _PDF_ wektor obrazu w casset zamiast tego w tym pliki poszczególnych mapy bitowej w formacie różne metody rozwiązywania. Za pomocą tej metody, podaj plik pojedynczego wektora **@1x** rozwiązania (w formacie pliku PDF wektor) i **@2x** i **@3x** wersji pliku zostanie wygenerowany w czasie kompilacji i zawartych w pakiecie aplikacji.

[![Obraz, ustaw interfejs edytora](image-images/imageset05.png "obraz, ustaw interfejs Edytora")](image-images/imageset05-large.png#lightbox)

Na przykład Jeśli dołączysz `MonkeyIcon.pdf` pliku jako wektor katalog zasobów z rozdzielczością x 150px 150px, następujące zasoby będą objęte pakietu ostatecznej aplikacji podczas został skompilowany mapy bitowej:

1. **MonkeyIcon@1x.png** -150px x 150px rozwiązania.
2. **MonkeyIcon@2x.png** -300 pikseli x 300px rozwiązania.
3. **MonkeyIcon@3x.png** -450px x 450px rozwiązania.

Następujące czynności powinny należy brać pod uwagę podczas korzystania z plików PDF Obrazy wektorowe w wykazy zasobów:

- Nie jest obsługa pełnego wektora jako plik PDF będzie rasteryzowany do mapy bitowej w czasie kompilacji i mapy bitowe, w gotowych aplikacji.
- Nie można dostosować rozmiar obrazu, gdy jest ono ustawione w katalogu zasobów. Jeśli spróbujesz zmienić rozmiar obrazu (lub w kodzie za pomocą automatycznego układu oraz klasy rozmiaru) obraz zostanie naruszona, podobnie jak inne mapy bitowej.

Korzystając z **obrazu zestawie** w program Xcode Interface Builder, nazwa zestawu może po prostu wybierz z listy rozwijanej w **Inspektor atrybut**: **

![Wybieranie obrazu zestawu w program Xcode Interface Builder](image-images/imageset06.png "wybraniu obrazu zestawu w program Xcode Interface Builder")

<a name="Adding-new-Assets-Collections"/>

### <a name="adding-new-assets-collections"></a>Dodawanie nowych kolekcji zasobów

Podczas pracy z obrazów w wykazach zasoby mogą być razy, gdy chcesz utworzyć nową kolekcję, zamiast opcji dodawania wszystkich obrazów do **Assets.xcassets** kolekcji. Na przykład podczas projektowania zasoby na żądanie.

Aby dodać nowy katalog zasobów do projektu:

1. Kliknij prawym przyciskiem myszy nad projektem w **konsoli rozwiązania** i wybierz **Dodaj** > **nowy plik...**
2. Wybierz **Mac** > **wykaz zasobów**, wprowadź **nazwa** kolekcji, a następnie kliknij przycisk **New** przycisku: 

    ![Dodawanie nowy katalog zasobów](image-images/asset01.png "dodawanie nowy katalog zasobów")

W tym miejscu możesz pracować z tą kolekcją w taki sam sposób jak domyślne **Assets.xcassets** kolekcji automatycznie uwzględnione w projekcie.


### <a name="adding-images-to-resources"></a>Dodawanie obrazów do zasobów

> [!IMPORTANT]
> Ta metoda Praca z obrazami w aplikację dla systemu macOS została zastąpiona przez firmę Apple. Należy używać [zestawów obrazu wykazu zasobów](#asset-catalogs) Menedżera aplikacji obrazów zamiast tego.

Zanim użyjesz pliku obrazu w aplikacji platformy Xamarin.Mac (lub w kodzie języka C# od programu Interface Builder) musi być włączone do projektu **zasobów** folder jako **zasobów pakietu**. Aby dodać plik do projektu, wykonaj następujące czynności:

1. Kliknij prawym przyciskiem myszy **zasobów** folderu w projekcie w **konsoli rozwiązania** i wybierz **Dodaj** > **Dodawanie plików...** : 

    ![Dodawanie pliku](image-images/add01.png "dodawania pliku")
2. Z **Dodaj pliki** okno dialogowe, wybierz opcję obrazów, plików do dodania do projektu, wybierz `BundleResource` dla **Akcja kompilacji zastąpienie** i kliknij przycisk **Otwórz** przycisku:

    [![Wybieranie plików do dodania](image-images/add02.png "Wybieranie plików do dodania")](image-images/add02-large.png#lightbox)
3. Jeśli pliki nie znajdują się już w **zasobów** folderu, użytkownik zostanie zapytany, czy chcesz **kopiowania**, **przenieść** lub **łącze** plików. Wybierz, które każdy kolory Twoje potrzeby, zwykle, które będą **kopiowania**:

    ![Wybierając akcję Dodaj](image-images/add04.png "wybierając Dodaj akcję")
4. Nowe pliki zostaną uwzględnione w projekcie i odczytu do użytku: 

    ![Nowe pliki obrazów, dodane do konsoli rozwiązania](image-images/add03.png "nowe pliki obrazów, dodane do konsoli rozwiązania")
5. Powtórz te czynności dla wszystkie pliki obrazu wymagane.

Można użyć dowolnego png, jpg lub pliku pdf jako obrazu źródłowego w aplikacji platformy Xamarin.Mac. W następnej sekcji przyjrzymy Dodawanie wersji o wysokiej rozdzielczości naszych obrazów i ikon do obsługi (wyświetlacz retina) na podstawie Mac.

> [!IMPORTANT]
> W przypadku dodawania obrazów w celu **zasobów** folderu, możesz pozostawić **Akcja kompilacji zastąpienie** równa **domyślne**. Wartość domyślna to Akcja kompilacji dla tego folderu `BundleResource`.

## <a name="provide-high-resolution-versions-of-all-app-graphics-resources"></a>Podaj wersji o wysokiej rozdzielczości wszystkich zasobów graficznych aplikacji

Zasoby grafiki, dodać do aplikacji platformy Xamarin.Mac (ikony, niestandardowe formanty, niestandardowych kursorów, niestandardowych kompozycji itp.) konieczne wersji o wysokiej rozdzielczości, oprócz ich wersji standardowej rozdzielczości. Jest to wymagane, dlatego, że aplikacja będzie wyglądać najlepszy przebieg na ekranie (wyświetlacz retina) do dyspozycji komputera Mac.


### <a name="adopt-the-2x-naming-convention"></a>Przyjmuje @2x konwencje nazewnictwa

> [!IMPORTANT]
> Ta metoda Praca z obrazami w aplikację dla systemu macOS została zastąpiona przez firmę Apple. Należy używać [zestawów obrazu wykazu zasobów](#asset-catalogs) Menedżera aplikacji obrazów zamiast tego.

Po utworzeniu wersji standard i wysokiej rozdzielczości obrazu, wykonaj tę konwencję nazewnictwa dla pary obrazu podczas umieszczania ich w projekcie platformy Xamarin.Mac:

- **Rozdzielczość standardowa**  - **rozszerzenia ImageName.filename** (przykład: **tags.png**)
- **O wysokiej rozdzielczości**   -  **ImageName@2x.filename-extension** (przykład: **tags@2x.png**)

Po dodaniu do projektu, będą wyświetlane w następujący sposób:

![Pliki obrazów w konsoli rozwiązania](image-images/add03.png "pliki obrazów w konsoli rozwiązania")

Gdy obraz jest przypisany do elementu interfejsu użytkownika w programu Interface Builder będzie po prostu wybierz plik w _ImageName_**.** _rozszerzenie nazwy pliku_ formatu (przykład: **tags.png**). Takie same dla kodu C# przy użyciu obrazu, będzie pobranie pliku w _ImageName_**.** _rozszerzenie nazwy pliku_ formatu.

Uruchomienie aplikacji rozszerzenia Xamarin.Mac jest na komputerze Mac, _ImageName_**.** _rozszerzenie nazwy pliku_ obraz w formacie, które będą używane w standardowych wyświetlacze o rozdzielczości **ImageName@2x.filename-extension** obraz będzie pobierany automatycznie wyświetlanie Retina baz Mac.


## <a name="using-images-in-interface-builder"></a>Używanie obrazów w programu Interface Builder

Dowolny zasób obrazu, które zostały dodane do **zasobów** folder w swojej platformy Xamarin.Mac projektu i ustawiono akcję kompilacji na **BundleResource** zostanie automatycznie pokazana w programu Interface Builder i może być wybrany jako część elementu interfejsu użytkownika (jeśli obsługi obrazów).

Aby użyć obrazu w programu interface builder, wykonaj następujące czynności:

1. Dodawanie obrazu do **zasobów** folder z **Build Action** z `BundleResource`: 

     ![Zasób obrazu w konsoli rozwiązania](image-images/ib00.png "zasób obrazu w konsoli rozwiązania")
2. Kliknij dwukrotnie **Main.storyboard** plik, aby go otworzyć do edycji w programu Interface Builder: 

     [![Edytowanie głównego storyboard](image-images/ib01.png "edytowania głównego storyboard")](image-images/ib01-large.png#lightbox)
3. Przeciągnij element interfejsu użytkownika, wykorzystującej obrazów na powierzchnię projektową (na przykład **element paska narzędzi obrazów**): 

     ![Element paska narzędzi do edycji](image-images/ib02.png "edycji elementu paska narzędzi")
4. Wybierz obraz, który został dodany do **zasobów** folderu w **nazwa obrazu** listy rozwijanej: 

     [![Wybieranie obrazu element paska narzędzi](image-images/ib03.png "wybraniu obrazu dla elementu paska narzędzi")](image-images/ib03-large.png#lightbox)
5. Wybrany obraz zostanie wyświetlony na powierzchnię: 

     ![Obraz jest wyświetlany w edytorze paska narzędzi](image-images/ib04.png "obraz jest wyświetlany w edytorze paska narzędzi")
6. Zapisz zmiany i powrócić do programu Visual Studio dla komputerów Mac do synchronizacji z narzędziem Xcode.

Powyższe kroki pracy dla każdego elementu interfejsu użytkownika, który umożliwia ich właściwości obrazu, należy ustawić **Inspektor atrybutu**. Ponownie Jeśli dołączono **@2x** wersję pliku obrazu, jego zostaną automatycznie użyte na wyświetlanie (wyświetlacz retina) na podstawie Mac.

> [!IMPORTANT]
> Jeśli obraz nie jest dostępna w **nazwa obrazu** listy rozwijanej, zamknij projekt .storyboard w środowisku Xcode i otwórz go ponownie z programu Visual Studio dla komputerów Mac. Jeśli obraz, który nadal jest niedostępna, upewnij się, że jego **Build Action** jest `BundleResource` i że obraz został dodany do **zasobów** folderu.

## <a name="using-images-in-c-code"></a>Używanie obrazów w kodzie języka C#

Podczas ładowania obrazu do pamięci w aplikacji platformy Xamarin.Mac przy użyciu kodu C#, obraz, który będzie przechowywany w `NSImage` obiektu. Jeśli plik obrazu została uwzględniona w pakiecie aplikacji rozszerzenia Xamarin.Mac (dołączone do zasobów), użyj poniższego kodu, można załadować obrazu:

```csharp
NSImage image = NSImage.ImageNamed("tags.png");
```

Powyższy kod używa statycznego `ImageNamed("...")` metody `NSImage` klasy, aby załadować danego obrazu do pamięci z **zasobów** folderu, jeśli obraz nie zostanie znaleziony, `null` zostaną zwrócone. Jeśli zostały uwzględnione, takich jak obrazy przypisane w programu Interface Builder **@2x** wersję pliku obrazu, jego zostaną automatycznie użyte na wyświetlanie (wyświetlacz retina) na podstawie Mac.

Aby załadować obrazów spoza zbioru aplikacji (z systemu Mac), użyj następującego kodu:

```csharp
NSImage image = new NSImage("/Users/KMullins/Documents/photo.jpg")
```

<a name="Working-with-Template-Images"/>

## <a name="working-with-template-images"></a>Praca z obrazami szablonu

Na podstawie projektu aplikacji systemu macOS, możliwe czasy, kiedy trzeba będzie dostosować ikonę lub obraz w interfejsie użytkownika, aby dopasować zmiany w schemacie kolorów (na przykład, w oparciu o preferencje użytkowników).

Aby uzyskać ten efekt, Przełącz _tryb renderowania_ środka obrazu, aby **obrazu szablonu**:

[![Ustawienie obrazu szablonu](image-images/templateimage01.png "ustawienie obrazu szablonu")](image-images/templateimage01-large.png#lightbox)

Korzystając z programu Xcode Interface Builder przypisywać zasób obrazu na kontrolkę interfejsu użytkownika:

![Wybieranie obrazu w program Xcode Interface Builder](image-images/templateimage02.png "wybraniu obrazu w program Xcode Interface Builder")

Lub opcjonalnie ustaw źródło obrazu w kodzie:

```csharp
MyIcon.Image = NSImage.ImageNamed ("MessageIcon");
```

Dodaj następującą funkcję, publicznej do kontrolera widoku:

```csharp
public NSImage ImageTintedWithColor (NSImage image, NSColor tint)
{
    var tintedImage = image.Copy () as NSImage;
    var frame = new CGRect (0, 0, image.Size.Width, image.Size.Height);

    // Apply tint
    tintedImage.LockFocus ();
    tint.Set ();
    NSGraphics.RectFill (frame, NSCompositingOperation.SourceAtop);
    tintedImage.UnlockFocus ();
    tintedImage.Template = false;

    // Return tinted image
    return tintedImage;
}
```

Na koniec odcień obrazu szablonu, należy wywołać tę funkcję, względem obrazu kolorować:

```csharp
MyIcon.Image = ImageTintedWithColor (MyIcon.Image, NSColor.Red);
```

<a name="Using_Images_with_Table_Views" />

## <a name="using-images-with-table-views"></a>Przy użyciu obrazów z widoki tabel

Do dołączenia obrazu komórki w `NSTableView`, musisz zmienić, jak dane są zwracane przez widok tabeli `NSTableViewDelegate's` `GetViewForItem` metodę `NSTableCellView` zamiast typowej `NSTextField`. Na przykład:

```csharp
public override NSView GetViewForItem (NSTableView tableView, NSTableColumn tableColumn, nint row)
{

    // This pattern allows you reuse existing views when they are no-longer in use.
    // If the returned view is null, you instance up a new view
    // If a non-null view is returned, you modify it enough to reflect the new data
    NSTableCellView view = (NSTableCellView)tableView.MakeView (tableColumn.Title, this);
    if (view == null) {
        view = new NSTableCellView ();
        if (tableColumn.Title == "Product") {
            view.ImageView = new NSImageView (new CGRect (0, 0, 16, 16));
            view.AddSubview (view.ImageView);
            view.TextField = new NSTextField (new CGRect (20, 0, 400, 16));
        } else {
            view.TextField = new NSTextField (new CGRect (0, 0, 400, 16));
        }
        view.TextField.AutoresizingMask = NSViewResizingMask.WidthSizable;
        view.AddSubview (view.TextField);
        view.Identifier = tableColumn.Title;
        view.TextField.BackgroundColor = NSColor.Clear;
        view.TextField.Bordered = false;
        view.TextField.Selectable = false;
        view.TextField.Editable = true;

        view.TextField.EditingEnded += (sender, e) => {

            // Take action based on type
            switch(view.Identifier) {
            case "Product":
                DataSource.Products [(int)view.TextField.Tag].Title = view.TextField.StringValue;
                break;
            case "Details":
                DataSource.Products [(int)view.TextField.Tag].Description = view.TextField.StringValue;
                break; 
            }
        };
    }

    // Tag view
    view.TextField.Tag = row;

    // Setup view based on the column selected
    switch (tableColumn.Title) {
    case "Product":
        view.ImageView.Image = NSImage.ImageNamed ("tags.png");
        view.TextField.StringValue = DataSource.Products [(int)row].Title;
        break;
    case "Details":
        view.TextField.StringValue = DataSource.Products [(int)row].Description;
        break;
    }

    return view;
}
```

Istnieje kilka wierszy, w tym polu zainteresowania. Najpierw dla kolumn, które chcemy, aby uwzględnić obrazu, utworzymy nowy `NSImageView` wymagany rozmiar i położenie, musimy również utworzyć nową `NSTextField` i umieść jego bieżącej pozycji oparte na informację, czy używasz obrazu:

```csharp
if (tableColumn.Title == "Product") {
    view.ImageView = new NSImageView (new CGRect (0, 0, 16, 16));
    view.AddSubview (view.ImageView);
    view.TextField = new NSTextField (new CGRect (20, 0, 400, 16));
} else {
    view.TextField = new NSTextField (new CGRect (0, 0, 400, 16));
}
```

Po drugie, musimy uwzględnić nowy widok obrazu i pole tekstowe w obiekcie nadrzędnym `NSTableCellView`:

```csharp
view.AddSubview (view.ImageView);
...

view.AddSubview (view.TextField);
...

```

Na koniec musimy wskazać pole tekstowe, można zmniejszyć i Zwiększaj komórka widoku tabeli:

```csharp
view.TextField.AutoresizingMask = NSViewResizingMask.WidthSizable;
```

Przykładowe dane wyjściowe:

[![Przykładem wyświetlanie obrazów w aplikacji](image-images/tables01.png "przykładem wyświetlanie obrazów w aplikacji")](image-images/tables01-large.png#lightbox)

Aby uzyskać więcej informacji na temat pracy z widoki tabel, zobacz nasze [widoki tabel](~/mac/user-interface/table-view.md) dokumentacji.

<a name="Using_Images_with_Outline_Views" />

## <a name="using-images-with-outline-views"></a>Przy użyciu obrazów z widoki konspektu

Do dołączenia obrazu komórki w `NSOutlineView`, musisz zmienić, jak dane są zwracane przez widok konspektu `NSTableViewDelegate's` `GetView` metodę `NSTableCellView` zamiast typowej `NSTextField`. Na przykład:

```csharp
public override NSView GetView (NSOutlineView outlineView, NSTableColumn tableColumn, NSObject item) {
    // Cast item
    var product = item as Product;

    // This pattern allows you reuse existing views when they are no-longer in use.
    // If the returned view is null, you instance up a new view
    // If a non-null view is returned, you modify it enough to reflect the new data
    NSTableCellView view = (NSTableCellView)outlineView.MakeView (tableColumn.Title, this);
    if (view == null) {
        view = new NSTableCellView ();
        if (tableColumn.Title == "Product") {
            view.ImageView = new NSImageView (new CGRect (0, 0, 16, 16));
            view.AddSubview (view.ImageView);
            view.TextField = new NSTextField (new CGRect (20, 0, 400, 16));
        } else {
            view.TextField = new NSTextField (new CGRect (0, 0, 400, 16));
        }
        view.TextField.AutoresizingMask = NSViewResizingMask.WidthSizable;
        view.AddSubview (view.TextField);
        view.Identifier = tableColumn.Title;
        view.TextField.BackgroundColor = NSColor.Clear;
        view.TextField.Bordered = false;
        view.TextField.Selectable = false;
        view.TextField.Editable = !product.IsProductGroup;
    }

    // Tag view
    view.TextField.Tag = outlineView.RowForItem (item);

    // Allow for edit
    view.TextField.EditingEnded += (sender, e) => {

        // Grab product
        var prod = outlineView.ItemAtRow(view.Tag) as Product;

        // Take action based on type
        switch(view.Identifier) {
        case "Product":
            prod.Title = view.TextField.StringValue;
            break;
        case "Details":
            prod.Description = view.TextField.StringValue;
            break; 
        }
    };

    // Setup view based on the column selected
    switch (tableColumn.Title) {
    case "Product":
        view.ImageView.Image = NSImage.ImageNamed (product.IsProductGroup ? "tags.png" : "tag.png");
        view.TextField.StringValue = product.Title;
        break;
    case "Details":
        view.TextField.StringValue = product.Description;
        break;
    }

    return view;
}
```

Istnieje kilka wierszy, w tym polu zainteresowania. Najpierw dla kolumn, które chcemy, aby uwzględnić obrazu, utworzymy nowy `NSImageView` wymagany rozmiar i położenie, musimy również utworzyć nową `NSTextField` i umieść jego bieżącej pozycji oparte na informację, czy używasz obrazu:

```csharp
if (tableColumn.Title == "Product") {
    view.ImageView = new NSImageView (new CGRect (0, 0, 16, 16));
    view.AddSubview (view.ImageView);
    view.TextField = new NSTextField (new CGRect (20, 0, 400, 16));
} else {
    view.TextField = new NSTextField (new CGRect (0, 0, 400, 16));
}
```

Po drugie, musimy uwzględnić nowy widok obrazu i pole tekstowe w obiekcie nadrzędnym `NSTableCellView`:

```csharp
view.AddSubview (view.ImageView);
...

view.AddSubview (view.TextField);
...

```

Na koniec musimy wskazać pole tekstowe, można zmniejszyć i Zwiększaj komórka widoku tabeli:

```csharp
view.TextField.AutoresizingMask = NSViewResizingMask.WidthSizable;
```

Przykładowe dane wyjściowe:

[![Przykład obrazu są wyświetlane w widoku konspektu](image-images/outline01.png "przykład obrazu są wyświetlane w widoku konspektu")](image-images/outline01-large.png#lightbox)

Aby uzyskać więcej informacji na temat pracy z widoki konspektu, zobacz nasze [widoki konspektu](~/mac/user-interface/outline-view.md) dokumentacji.


## <a name="summary"></a>Podsumowanie

W tym artykule jest zajęta szczegółowe omówienie pracy z obrazów i ikon w aplikacji platformy Xamarin.Mac. Firma dostrzegła różnych typów i korzysta z obrazów, jak używać obrazów i ikon w program Xcode Interface Builder i sposób pracy z obrazów i ikon w kodzie języka C#.



## <a name="related-links"></a>Linki pokrewne

- [MacImages (przykład)](https://developer.xamarin.com/samples/mac/MacImages/)
- [Witaj, Mac](~/mac/get-started/hello-mac.md)
- [Widoki tabel](~/mac/user-interface/table-view.md)
- [Widoki konspektu](~/mac/user-interface/outline-view.md)
- [System macOS X Human Interface Guidelines](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
- [Informacje o wysokiej rozdzielczości dla systemu OS X](https://developer.apple.com/library/content/documentation/GraphicsAnimation/Conceptual/HighResolutionOSX/Introduction/Introduction.html)
