---
title: Obrazy
description: "Ten artykuł dotyczy pracy z obrazów i ikon w aplikacji Xamarin.Mac. Opisuje tworzenie i obsługę obrazów potrzebne do utworzenia ikony aplikacji i używanie obrazów w kodzie C# i kompilatora interfejsu w środowisku Xcode."
ms.topic: article
ms.prod: xamarin
ms.assetid: C6B539C2-FC6A-4C38-B839-32BFFB9B16A7
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/15/2017
ms.openlocfilehash: d8098afea87765166db8318b76adf250818a0a6f
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/12/2018
---
# <a name="images"></a>Obrazy

_Ten artykuł dotyczy pracy z obrazów i ikon w aplikacji Xamarin.Mac. Opisuje tworzenie i obsługę obrazów potrzebne do utworzenia ikony aplikacji i używanie obrazów w kodzie C# i kompilatora interfejsu w środowisku Xcode._


## <a name="overview"></a>Omówienie

Podczas pracy z C# i .NET w aplikacji Xamarin.Mac, masz dostęp do tego samego obrazu i Ikona narzędzi dewelopera, pracy w *Objective-C* i *Xcode* jest.

Istnieje kilka sposobów tego obrazu, zasoby są używane wewnątrz aplikacji macOS (wcześniej znane jako systemu Mac OS X). Po prostu wyświetlanie obrazów jako część interfejsu użytkownika aplikacji do przypisywania go do formantu interfejsu użytkownika, np. paska narzędzi lub element listy źródła, aby udostępniać ikony, Xamarin.Mac ułatwia dodawanie kompozycji wspaniałych aplikacji macOS w następujący sposób : 

- **Elementy interfejsu użytkownika** — obrazy mogą być wyświetlane jako tło lub jako część aplikacji, w widoku obrazu (`NSImageView`).
- **Przycisk** — obrazy mogą być wyświetlane w przycisków (`NSButton`).
- **Obrazu komórki** — w ramach kontroli tabeli na podstawie (`NSTableView` lub `NSOutlineView`), obrazy mogą być używane w komórce obrazu (`NSImageCell`).
- **Element paska narzędzi** — obrazy mogą być dodawane do paska narzędzi (`NSToolbar`) jako element paska narzędzi obrazu (`NSToolbarItem`).
- **Źródło ikonę listy** — jako część listy źródeł (specjalnie sformatowana `NSOutlineView`).
- **Ikona aplikacji** -serie obrazów, które można grupować w `.icns` ustaw i używane jako ikonę aplikacji. Zobacz nasze [ikona aplikacji](~/mac/deploy-test/app-icon.md) dokumentacji, aby uzyskać więcej informacji.

Ponadto system macOS zawiera zestaw wstępnie zdefiniowanych obrazów, które mogą być używane w całej aplikacji.

[![Przykład uruchomić aplikacji](image-images/intro01.png "przykład uruchamianie aplikacji")](image-images/intro01-large.png#lightbox)

W tym artykule omówione zostaną następujące czynności podstawowe informacje dotyczące pracy z obrazów i ikon w aplikacji Xamarin.Mac. Zdecydowanie zaleca się pracę za pośrednictwem [Hello, Mac](~/mac/get-started/hello-mac.md) artykuł najpierw, w szczególności [wprowadzenie do programów Xcode i kompilatora interfejsu](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) i [gniazda i akcje](~/mac/get-started/hello-mac.md#Outlets_and_Actions) sekcje, w jakiej omawia kluczowe założenia i techniki, które będzie używana w tym artykule.


## <a name="adding-images-to-a-xamarinmac-project"></a>Dodawanie obrazów do projektu Xamarin.Mac

Przy dodawaniu obrazu do użycia w aplikacji Xamarin.Mac, istnieje kilka miejsca i sposób, że deweloper może zawierać plik obrazu do źródła projektu:

- **Drzewo projektu [przestarzały]** — obrazy można dodać bezpośrednio do drzewa projektów. Podczas wywoływania metody obrazy przechowywane w drzewie projektu z kodu, lokalizacja folderu, nie została określona. Na przykład: `NSImage image = NSImage.ImageNamed("tags.png");`. 
- **Folderu zasobów [przestarzały]** -specjalną **zasobów** jest folder dla każdego pliku, który będzie częścią aplikacji do pakietu, takich jak ikony, uruchom ekranu lub ogólne obrazów (lub inne obrazu lub pliku projektanta chce dodać). Podczas wywoływania metody obrazy przechowywane w **zasobów** folder z kodu, po prostu, takich jak obrazy przechowywane w drzewie projektu, lokalizacja folderu nie została określona. Na przykład: `NSImage.ImageNamed("tags.png")`.
- **Niestandardowe Folder lub podfolder [przestarzały]** -dewelopera można dodać folderu niestandardowego do drzewa źródła projektów i obrazy są przechowywane. Lokalizacja, w której plik zostanie dodany mogą być zagnieżdżane w podfolderze do dalszego ułatwić organizowanie projektu. Na przykład dodać dewelopera `Card` folderu do projektu i podfolderem folderu `Hearts` do tego folderu, a następnie zapisać obrazu **Jack.png** w `Hearts` folderu, `NSImage.ImageNamed("Card/Hearts/Jack.png")` czy ładowanie obrazu na środowisko uruchomieniowe.
- **Ustawia obraz katalogu zasobów [preferowane]** — dodano w Capitan El OS X **Ustawia obraz katalogi zasobów** zawiera wszystkie wersje lub reprezentacje obrazu, które są niezbędne do obsługi różnych urządzeń i skalować czynniki użytkownika aplikacja. Zamiast polegania na nazwę pliku zasobów obrazu (**@1x**,  **@2x** ).

<a name="asset-catalogs" />

### <a name="adding-images-to-an-asset-catalog-image-set"></a>Dodawanie obrazów do obrazu w katalogu zasobów ustawić

Jak wspomniano powyżej, **Ustawia obraz katalogi zasobów** zawiera wszystkie wersje lub reprezentacje obrazu niezbędne do obsługi różnych urządzeń i skalować czynników dla aplikacji. Zamiast polegania na nazwę pliku zasobów obrazu (zobacz obrazów niezależnie od rozdzielczości i nomenklaturę obrazu powyżej), **Ustawia obraz** Określ obraz, który należy do urządzenia i/lub rozpoznawania za pomocą edytora zasobów.

1. W **konsoli rozwiązania**, kliknij dwukrotnie **Assets.xcassets** plik, aby otworzyć do edycji: 

    ![Wybieranie Assets.xcassets](image-images/imageset01.png "wybranie Assets.xcassets")
2. Kliknij prawym przyciskiem myszy **listę zasobów** i wybierz **nowy zestaw obrazu**: 

    [![Dodawanie nowego zestawu obrazu](image-images/imageset02.png "Dodawanie nowego zestawu obrazów")](image-images/imageset02-large.png#lightbox)
3. Wybierz nowy zestaw obrazu i Edytor będą wyświetlane: 

    [![Wybranie nowego zestawu obrazu](image-images/imageset03.png "wybranie nowego zestawu obrazów")](image-images/imageset03-large.png#lightbox)
4. W tym miejscu możemy przeciągnij w obrazach dla każdego z różnych urządzeń i rozwiązania wymagane. 
5. Kliknij dwukrotnie plik nowy zestaw obrazu **nazwa** w **listę zasobów** go edytować: 

    [![Edytowanie obrazu Ustaw nazwę](image-images/imageset04.png "edycji obrazu Nazwa zestawu")](image-images/imageset04-large.png#lightbox)
    
Specjalny **wektor** klasa został dodany do **Ustawia obraz** która pozwala obejmują _PDF_ wektor obrazu w casset zamiast niej w tym mapy bitowej poszczególnych plików w formacie różne metody rozwiązywania. Za pomocą tej metody, podaj wektor pojedynczego pliku  **@1x**  rozdzielczości (w formacie pliku PDF wektor) i  **@2x**  i  **@3x**  wersje pliku, zostanie wygenerowany w czasie kompilacji i zawartych w pakiecie aplikacji.

[![Obraz ustawić interfejs edytora](image-images/imageset05.png "obrazu ustawić interfejs Edytora")](image-images/imageset05-large.png#lightbox)

Na przykład, Jeśli dołączysz `MonkeyIcon.pdf` pliku jako wektor katalogu zasobów z rozdzielczością x 150px 150px, mapy bitowej następujące zasoby będą objęte pakietu ostatecznej aplikacji podczas jego kompilacji:

1. **MonkeyIcon@1x.png** -150px x 150px rozpoznawania.
2. **MonkeyIcon@2x.png** -300px x 300px rozpoznawania.
3. **MonkeyIcon@3x.png** -450px x 450px rozpoznawania.

Następujące powinna być brana pod uwagę podczas przy użyciu plików PDF wektor obrazów w wykazach zasobów:

- Nie jest wektor pełnej obsługi, ponieważ plik PDF będzie rasteryzacji do mapy bitowej w czasie kompilacji i map bitowych w końcowym aplikacji.
- Nie można dostosować rozmiar obrazu po jego ustawieniu w katalogu zasobów. Jeśli próba zmiany rozmiaru obrazu (lub w kodzie za pomocą klasy rozmiaru i układu automatycznego) obraz będzie zniekształcony podobnie jak inne mapy bitowej.

Korzystając z **Ustaw obraz** w Konstruktorze interfejsu w programie Xcode, nazwa zestawu można po prostu wybierz z listy rozwijanej w **inspektora atrybutu**: **

![Wybieranie obrazu ustawić w Konstruktorze interfejsu w środowisku Xcode](image-images/imageset06.png "Wybieranie obrazu ustawić w Konstruktorze interfejsu w środowisku Xcode")

<a name="Adding-new-Assets-Collections"/>

### <a name="adding-new-assets-collections"></a>Dodawanie nowej kolekcji zasobów

Podczas pracy z obrazami w wykazach zasoby mogą być razy, aby utworzyć nową kolekcję, zamiast dodawania wszystkich obrazów do **Assets.xcassets** kolekcji. Na przykład podczas projektowania zasobów na żądanie.

Aby dodać nowy katalog zasoby do projektu:

1. Kliknij prawym przyciskiem myszy projekt w **konsoli rozwiązania** i wybierz **Dodaj** > **nowego pliku...**
2. Wybierz **Mac** > **katalogu zasobów**, wprowadź **nazwa** kolekcji i kliknij **nowy** przycisk: 

    ![Dodawanie nowego katalogu zasobów](image-images/asset01.png "Dodawanie nowego katalogu zasobów")

W tym miejscu możesz pracować z kolekcji w taki sam sposób jak domyślne **Assets.xcassets** kolekcji automatycznie dołączony do projektu.


### <a name="adding-images-to-resources"></a>Dodawanie obrazów do zasobów

> [!IMPORTANT]
> Praca z obrazami w aplikacji macOS ta metoda została zastąpiona przez firmę Apple. Należy używać [zasobów katalogu obrazu zestawy](#asset-catalogs) Menedżera aplikacji obrazy zamiast tego.

Przed użyciem pliku obrazu w aplikacji Xamarin.Mac (lub w kodzie języka C# z konstruktora interfejsu) musi być dołączony do projektu **zasobów** folder jako **zasobów pakietu**. Aby dodać plik do projektu, wykonaj następujące czynności:

1. Kliknij prawym przyciskiem myszy **zasobów** folder w projekcie w **konsoli rozwiązania** i wybierz **Dodaj** > **Dodawanie plików...** : 

    ![Dodawanie pliku](image-images/add01.png "Dodawanie pliku")
2. Z **Dodaj pliki** wybierz pozycję obrazy plików do dodania do projektu, zaznacz `BundleResource` dla **Akcja kompilacji zastąpienie** i kliknij przycisk **Otwórz** przycisk:

    [![Wybieranie plików do dodania](image-images/add02.png "Wybieranie plików do dodania")](image-images/add02-large.png#lightbox)
3. Jeśli pliki nie znajdują się już w **zasobów** folderu, użytkownik zostanie zapytany, jeśli chcesz **kopiowania**, **Przenieś** lub **łącze** plików. Wybierz, które co mechanizmy potrzeb, najczęściej, które będą **kopiowania**:

    ![Wybranie akcji Dodaj](image-images/add04.png "wybierania akcji Dodaj")
4. Nowe pliki zostanie dołączony do projektu i odczytu do użycia: 

    ![Nowe pliki obrazów, dodane do konsoli do rozwiązania](image-images/add03.png "nowe pliki obrazów, dodane do konsoli do rozwiązania")
5. Należy powtórzyć wymagane pliki obrazów.

W aplikacji Xamarin.Mac, można użyć dowolnego png, jpg lub plik pdf jako obrazu źródłowego. W następnej sekcji przyjrzymy Dodawanie wysokiej rozdzielczości wersji naszych obrazów i ikon do obsługi siatkówki na podstawie Mac.

> [!IMPORTANT]
> W przypadku dodawania obrazów do **zasobów** folderu, można pozostawić **Akcja kompilacji zastąpienie** ustawioną **domyślne**. Wartość domyślna to Akcja kompilacji dla tego folderu `BundleResource`.

## <a name="provide-high-resolution-versions-of-all-app-graphics-resources"></a>Podaj wersji o wysokiej rozdzielczości wszystkich zasobów grafiki aplikacji

Zasób graficzne dodany do aplikacji Xamarin.Mac (ikony, niestandardowe kontrolki, niestandardowych kursorów, niestandardowych kompozycji itp.) musi być wersji o wysokiej rozdzielczości, oprócz ich wersje standard rozpoznawania. Jest to wymagane, dzięki czemu aplikacja będzie wyglądać najlepiej jest przy komputerze Mac Uruchom na ekranie siatkówki.


### <a name="adopt-the-2x-naming-convention"></a>Przyjmuje @2x konwencji nazewnictwa

> [!IMPORTANT]
> Praca z obrazami w aplikacji macOS ta metoda została zastąpiona przez firmę Apple. Należy używać [zasobów katalogu obrazu zestawy](#asset-catalogs) Menedżera aplikacji obrazy zamiast tego.

Podczas tworzenia wersji standard i o wysokiej rozdzielczości obrazu, wykonaj tę konwencję nazewnictwa dla pary obrazu podczas umieszczania ich w projekcie Xamarin.Mac:

- **Standard rozpoznawania**  - **rozszerzenia ImageName.filename** (przykład: **tags.png**)
- **O wysokiej rozdzielczości**   -   **ImageName@2x.filename-extension**  (przykład:  **tags@2x.png** )

Jeśli dodano do projektu, czy pojawią się one w następujący sposób:

![Pliki obrazów w konsoli rozwiązania](image-images/add03.png "plików obrazów w konsoli rozwiązania")

Gdy obraz jest przypisany do elementu interfejsu użytkownika w Konstruktorze interfejs będzie wystarczy wybrać plik w _Nazwa_obrazu_**.** _rozszerzenie nazwy pliku_ formatu (przykład: **tags.png**). Taki sam dla przy użyciu obrazu w kodzie języka C#, będzie pobranie pliku w _Nazwa_obrazu_**.** _rozszerzenie nazwy pliku_ format.

Uruchomienie aplikacji Xamarin.Mac jest na komputerze Mac _Nazwa_obrazu_**.** _rozszerzenie nazwy pliku_ obraz w formacie będą używane w standardowe wyświetlacze o rozdzielczości  **ImageName@2x.filename-extension**  obrazu będzie automatycznie pobierane wyświetlanie siatkówki podstawowych Mac.


## <a name="using-images-in-interface-builder"></a>Używanie obrazów w Konstruktorze — interfejs

Dowolnego zasobu obrazu została dodana do **zasobów** folderu w Xamarin.Mac Twojego projektu i została wybrana akcja kompilacji **BundleResource** automatycznie będą widoczne w interfejsie konstruktora i może być wybrany jako część elementu interfejsu użytkownika (jeśli obsługi obrazów).

Aby użyć obrazu w Konstruktorze interfejsu, wykonaj następujące czynności:

1. Dodawanie obrazu do **zasobów** folder o **Akcja kompilacji** z `BundleResource`: 

     ![Zasób obrazu w konsoli rozwiązania](image-images/ib00.png "zasób obrazu w konsoli do rozwiązania")
2. Kliknij dwukrotnie **Main.storyboard** plik, aby otworzyć do edycji w Konstruktorze interfejsu: 

     [![Edytowanie głównego storyboard](image-images/ib01.png "edytowania głównego storyboard")](image-images/ib01-large.png#lightbox)
3. Przeciągnij element interfejsu użytkownika, który pobiera obrazy na powierzchnię projektu (na przykład **element paska narzędzi obrazu**): 

     ![Edytowanie elementów paska narzędzi](image-images/ib02.png "edycji elementem paska narzędzi")
4. Wybierz obraz, który został dodany do **zasobów** folderu w **nazwa obrazu** listy rozwijanej: 

     [![Wybieranie obrazu dla elementu narzędzi](image-images/ib03.png "Wybieranie obrazu elementem paska narzędzi")](image-images/ib03-large.png#lightbox)
5. Wybrany obraz zostanie wyświetlony na powierzchni projektu: 

     ![Obraz jest wyświetlany w edytorze paska narzędzi](image-images/ib04.png "obraz jest wyświetlany w edytorze paska narzędzi")
6. Zapisz zmiany i wróć do programu Visual Studio for Mac synchronizację w programie Xcode.

Powyższe kroki pracy elementów interfejsu użytkownika, które umożliwia ich właściwość obrazu w **inspektora atrybutu**. Ponownie jeśli zostały uwzględnione  **@2x**  wersję pliku obrazu, jego zostaną automatycznie użyte na Mac na podstawie wyświetlania siatkówki.

> [!IMPORTANT]
> Jeśli obraz nie jest dostępna w **nazwa obrazu** listy rozwijanej, zamknij projekt .storyboard w środowisku Xcode i otwórz go ponownie z programu Visual Studio dla komputerów Mac. Jeśli obraz jest nadal niedostępna, upewnij się, że jego **Akcja kompilacji** jest `BundleResource` oraz że obraz został dodany do **zasobów** folderu.

## <a name="using-images-in-c-code"></a>Używanie obrazów w kodzie języka C#

Podczas ładowania obrazu do pamięci w aplikacji Xamarin.Mac przy użyciu kodu C#, obrazu będzie przechowywana w `NSImage` obiektu. Jeśli plik obrazu zostało uwzględnione w pakiecie aplikacji Xamarin.Mac (dołączone do zasobów), należy użyć poniższego kodu można załadować obrazu:

```csharp
NSImage image = NSImage.ImageNamed("tags.png");
```

Powyższy kod używa statycznych `ImageNamed("...")` metody `NSImage` klasę, aby załadować danego obrazu do pamięci z **zasobów** folderu, jeśli nie można znaleźć obrazu, `null` zostaną zwrócone. Jeśli zostały uwzględnione, takich jak obrazy przypisane w Konstruktorze interfejsu  **@2x**  wersję pliku obrazu, jego zostaną automatycznie użyte na Mac na podstawie wyświetlania siatkówki.

Aby załadować obrazy poza pakietu aplikacji (z systemu Mac), należy użyć poniższego kodu:

```csharp
NSImage image = new NSImage("/Users/KMullins/Documents/photo.jpg")
```

<a name="Working-with-Template-Images"/>

## <a name="working-with-template-images"></a>Praca z obrazami szablonu

Oparte na projekt aplikacji macOS, może być potrzebne do dostosowywania ikony lub obrazu w interfejsie użytkownika, aby dopasować zmiany schematu kolorów (na przykład zgodnie z preferencjami użytkownika).

Aby uzyskać ten efekt, Przełącz _tryb renderowania_ środka obrazu, aby **obrazu szablonu**:

[![Ustawienie obrazu szablonu](image-images/templateimage01.png "ustawienie obrazu szablonu")](image-images/templateimage01-large.png#lightbox)

Z konstruktora interfejsu Xcode Przypisz zasób obrazu do formantu interfejsu użytkownika:

![Wybieranie obrazu w Konstruktorze interfejsu w środowisku Xcode](image-images/templateimage02.png "Wybieranie obrazu w Konstruktorze interfejsu w środowisku Xcode")

Lub opcjonalnie ustawić źródło obrazu w kodzie:

```csharp
MyIcon.Image = NSImage.ImageNamed ("MessageIcon");
```

Dodaj następującą funkcję publicznej do kontrolera widoku:

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

Na koniec odcień obrazu szablonu, należy wywołać tej funkcji względem obrazu do kolorowanie:

```csharp
MyIcon.Image = ImageTintedWithColor (MyIcon.Image, NSColor.Red);
```

<a name="Using_Images_with_Table_Views" />

## <a name="using-images-with-table-views"></a>Przy użyciu obrazów z widokami tabeli

Aby uwzględnić obrazu jako część komórki w `NSTableView`, musisz zmienić, jak dane są zwracane przez widok tabeli `NSTableViewDelegate's` `GetViewForItem` metodę `NSTableCellView` zamiast typowe `NSTextField`. Na przykład:

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

Istnieje kilka wierszy odsetek tutaj. Najpierw dla kolumn, które chcemy Dołącz obraz, utworzymy nową `NSImageView` wymagany rozmiar i położenie, będziemy również utworzyć nową `NSTextField` i umieścić położenia domyślnego oparte na czy używamy obrazu:

```csharp
if (tableColumn.Title == "Product") {
    view.ImageView = new NSImageView (new CGRect (0, 0, 16, 16));
    view.AddSubview (view.ImageView);
    view.TextField = new NSTextField (new CGRect (20, 0, 400, 16));
} else {
    view.TextField = new NSTextField (new CGRect (0, 0, 400, 16));
}
```

Po drugie, musimy dołączyć nowy widok obrazu i pola tekstowego w obiekcie nadrzędnym `NSTableCellView`:

```csharp
view.AddSubview (view.ImageView);
...

view.AddSubview (view.TextField);
...

```

Ponadto musimy sprawdzić pola tekstowego, czy można zmniejszyć rozmiar i powiększania wraz z komórkę tabeli w widoku:

```csharp
view.TextField.AutoresizingMask = NSViewResizingMask.WidthSizable;
```

Przykład danych wyjściowych:

[![Przykład wyświetlania obrazu w aplikacji](image-images/tables01.png "przykład wyświetlania obrazu w aplikacji")](image-images/tables01-large.png#lightbox)

Aby uzyskać więcej informacji na temat pracy z widokami tabeli, zobacz nasze [widoków tabel](~/mac/user-interface/table-view.md) dokumentacji.

<a name="Using_Images_with_Outline_Views" />

## <a name="using-images-with-outline-views"></a>Przy użyciu obrazów z widokami konspektu

Aby umieścić obraz jako część komórki w `NSOutlineView`, musisz zmienić, jak dane są zwracane przez wyświetlanie konspektu `NSTableViewDelegate's` `GetView` metodę `NSTableCellView` zamiast typowe `NSTextField`. Na przykład:

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

Istnieje kilka wierszy odsetek tutaj. Najpierw dla kolumn, które chcemy Dołącz obraz, utworzymy nową `NSImageView` wymagany rozmiar i położenie, będziemy również utworzyć nową `NSTextField` i umieścić położenia domyślnego oparte na czy używamy obrazu:

```csharp
if (tableColumn.Title == "Product") {
    view.ImageView = new NSImageView (new CGRect (0, 0, 16, 16));
    view.AddSubview (view.ImageView);
    view.TextField = new NSTextField (new CGRect (20, 0, 400, 16));
} else {
    view.TextField = new NSTextField (new CGRect (0, 0, 400, 16));
}
```

Po drugie, musimy dołączyć nowy widok obrazu i pola tekstowego w obiekcie nadrzędnym `NSTableCellView`:

```csharp
view.AddSubview (view.ImageView);
...

view.AddSubview (view.TextField);
...

```

Ponadto musimy sprawdzić pola tekstowego, czy można zmniejszyć rozmiar i powiększania wraz z komórkę tabeli w widoku:

```csharp
view.TextField.AutoresizingMask = NSViewResizingMask.WidthSizable;
```

Przykład danych wyjściowych:

[![Przykład obrazu będzie wyświetlany w widoku konspektu](image-images/outline01.png "przykład obrazu będzie wyświetlany w widoku konspektu")](image-images/outline01-large.png#lightbox)

Aby uzyskać więcej informacji na temat pracy z widokami konspektu, zobacz nasze [widoków konspektu](~/mac/user-interface/outline-view.md) dokumentacji.


## <a name="summary"></a>Podsumowanie

W tym artykule trwało szczegółowe przyjrzeć się praca z obrazami i ikony w aplikacji Xamarin.Mac. Firma Microsoft przedstawiono różne typy i korzysta z obrazów, jak używać obrazów i ikon w Konstruktorze interfejsu w środowisku Xcode i sposobu pracy z obrazów i ikon w kodzie języka C#.



## <a name="related-links"></a>Linki pokrewne

- [MacImages (przykład)](https://developer.xamarin.com/samples/mac/MacImages/)
- [Witaj, Mac](~/mac/get-started/hello-mac.md)
- [Widoki tabel](~/mac/user-interface/table-view.md)
- [Widoki konspektu](~/mac/user-interface/outline-view.md)
- [System macOS X Human Interface Guidelines](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
- [Informacje o wysokiej rozdzielczości dla systemu OS X](https://developer.apple.com/library/content/documentation/GraphicsAnimation/Conceptual/HighResolutionOSX/Introduction/Introduction.html)
