---
title: Przy użyciu narzędzia Projektant systemu Android
description: Ten temat dotyczy wskazówki projektanta platformy Xamarin.Android. Pokazuje, jak utworzyć interfejs użytkownika aplikacji dla przeglądarki kolorów; Ten interfejs użytkownika jest tworzone wyłącznie w projektancie.
ms.prod: xamarin
ms.assetid: 70FF2F9A-71BD-317E-C881-A44D82DF1BD8
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/10/2018
ms.openlocfilehash: 8d1dc410d5336d9c2505a18720cc7f734e838c39
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/07/2018
ms.locfileid: "33798635"
---
# <a name="using-the-android-designer"></a>Przy użyciu narzędzia Projektant systemu Android

_Ten temat dotyczy wskazówki projektanta platformy Xamarin.Android. Pokazuje, jak utworzyć interfejs użytkownika aplikacji dla przeglądarki kolorów; Ten interfejs użytkownika jest tworzone wyłącznie w projektancie._


## <a name="overview"></a>Omówienie

Interfejsy użytkownika dla systemu android mogą być tworzone deklaratywnie przy użyciu plików XML lub programowane pisanie kodu. Projektant Xamarin.Android umożliwia deweloperom tworzenie i modyfikowanie deklaratywne układów wizualnie, bez konieczności konieczność powtarzania ręcznie edytować pliki XML. Projektant udostępnia również opinie w czasie rzeczywistym, które umożliwia deweloperowi ocenić zmiany interfejsu użytkownika bez konieczności ponownego wdrażania aplikacji na urządzeniu lub emulatorze. To jest znacznie przyspieszyć pracę nad interfejsu użytkownika dla systemu Android. W tym artykule możemy przedstawić przewodnik pokazujący sposób wizualnie tworzyć interfejsu użytkownika za pomocą projektanta platformy Xamarin.Android.


## <a name="walkthrough"></a>Wskazówki

Celem tego przewodnika jest tworzenie interfejsu użytkownika dla aplikacji przeglądarki przykład kolorów, które wyświetla listę kolory, nazwy i wartości RGB za pomocą projektanta dla systemu Android. Wyjaśniono, jak dodać elementy widget na powierzchnię projektu, a także sposobu określania układu te elementy widget wizualnie. Po wykonaniu tej wyjaśniamy sposób modyfikowania elementów widget w trybie interakcyjnym na powierzchni projektu lub przy użyciu narzędzia Projektant **konsoli właściwości**. Będzie ponadto sprawdzić wygląd naszego projektu możemy uruchomić aplikację.

Dzieła!


### <a name="creating-a-new-project"></a>Tworzenie nowego projektu

Pierwszym krokiem jest utworzenie nowego projektu platformy Xamarin.Android.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Uruchom program Visual Studio, a następnie kliknij przycisk **nowy projekt...**  wybierz **Visual C\# > Android > aplikacji systemu Android (Xamarin)** szablonu:

[![Pusta aplikacja systemu android](designer-walkthrough-images/vs/01-android-app-sml.w157.png)](designer-walkthrough-images/vs/01-android-app.w157.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Uruchom program Visual Studio for Mac i kliknij przycisk **nowe rozwiązanie...** . Wybierz **aplikacji systemu Android** szablon i kliknij przycisk **dalej**:

[![Pusta aplikacja systemu android](designer-walkthrough-images/xs/01-android-app-sml.png)](designer-walkthrough-images/xs/01-android-app.png#lightbox)

-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Nazwa nowej aplikacji **DesignerWalkthrough** i kliknij przycisk **OK**.

[![Nazwa aplikacji](designer-walkthrough-images/vs/02-name-app-sml.png)](designer-walkthrough-images/vs/02-name-app.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Nazwa nowej aplikacji **DesignerWalkthrough**. W obszarze **platform docelowych**, wybierz pozycję **r i największy** i kliknij przycisk **dalej**:

[![Nazwa aplikacji](designer-walkthrough-images/xs/02-designer-walkthrough-sml.png)](designer-walkthrough-images/xs/02-designer-walkthrough.png#lightbox)

W następnym oknie dialogowym kliknij **Utwórz**.

-----



### <a name="adding-a-layout"></a>Dodawanie układu

Utwórzmy **LinearLayout** użyjemy do przechowywania elementów interfejsu naszych użytkowników.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

W programie Visual Studio, kliknij prawym przyciskiem myszy **zasobów/układ** w **Eksploratora rozwiązań** i wybierz **Dodaj > Nowy element...** . W **Dodaj nowy element** okno dialogowe, wybierz opcję **Android układu**. Nadaj nazwę plikowi **ListItem.axml** i kliknij przycisk **Dodaj**:

[![Nowy układ](designer-walkthrough-images/vs/03-new-layout-sml.w157.png)](designer-walkthrough-images/vs/03-new-layout.w157.png#lightbox)

Nowy **ListItem** układu jest wyświetlany w Projektancie:

[![Widok projektanta](designer-walkthrough-images/vs/04-designer-view-sml.png)](designer-walkthrough-images/vs/04-designer-view.png#lightbox)

Kliknij przycisk **źródła** u dołu projektanta, aby wyświetlić źródło XML dla tego układu:

[![Projektanta XML](designer-walkthrough-images/vs/05-designer-xml-sml.png)](designer-walkthrough-images/vs/05-designer-xml.png#lightbox)

Z **widoku** menu, kliknij przycisk **inne okna > konspekt dokumentu** otworzyć **konspekt dokumentu**. **Konspekt dokumentu** wskazuje, czy układ obecnie zawiera jeden **LinearLayout** elementu widget:

[![Konspekt dokumentu](designer-walkthrough-images/vs/06-document-outline-sml.png)](designer-walkthrough-images/vs/06-document-outline.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

W programie Visual Studio for Mac, kliknij prawym przyciskiem myszy **zasobów/układ** w **rozwiązania** konsoli, a następnie wybierz **Dodaj > Nowy plik...** . W **nowy plik** okno dialogowe, wybierz opcję **Android > układu**. Nadaj nazwę plikowi **ListItem** i kliknij przycisk **nowy**:

[![Nowy układ](designer-walkthrough-images/xs/03-new-layout-sml.png)](designer-walkthrough-images/xs/03-new-layout.png#lightbox)

Nowy **ListItem** układu jest wyświetlany w Projektancie:

[![Widok projektanta](designer-walkthrough-images/xs/04-designer-view-sml.png)](designer-walkthrough-images/xs/04-designer-view.png#lightbox)

Kliknij przycisk **źródła** u dołu projektanta, aby wyświetlić źródło XML dla tego układu. Po kliknięciu **konspekt dokumentu** kartę po prawej stronie, będzie wyświetlana, czy układ obecnie zawiera jeden **LinearLayout** elementu widget:

[![Projektanta XML](designer-walkthrough-images/xs/05-designer-xml-sml.png)](designer-walkthrough-images/xs/05-designer-xml.png#lightbox)

-----



### <a name="creating-the-list-item-user-interface"></a>Tworzenie interfejsu użytkownika elementu listy

Następnie zamierzamy utworzyć interfejsu użytkownika dla aplikacji przeglądarki kolorów.
Kliknij przycisk **projektanta** kartę, aby powrócić na powierzchnię projektu.
W **przybornika**, przewiń w dół do **obrazów & nośnika** sekcji i przejrzeć każdego elementu, do momentu zlokalizowania *ImageView*:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Znajdź ImageView](designer-walkthrough-images/vs/07-locate-imageview-sml.png)](designer-walkthrough-images/vs/07-locate-imageview.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Znajdź ImageView](designer-walkthrough-images/xs/06-locate-imageview-sml.png)](designer-walkthrough-images/xs/06-locate-imageview.png#lightbox)

-----

Alternatywnie można wprowadzić *ImageView* na pasku wyszukiwania, aby zlokalizować `ImageView` elementu widget:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![ImageView wyszukiwania](designer-walkthrough-images/vs/08-imageview-search-sml.png)](designer-walkthrough-images/vs/08-imageview-search.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![ImageView wyszukiwania](designer-walkthrough-images/xs/07-imageview-search-sml.png)](designer-walkthrough-images/xs/07-imageview-search.png#lightbox)

-----

Przeciągnij `ImageView` na powierzchnię projektu:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![ImageView na kanwie](designer-walkthrough-images/vs/09-imageview-on-canvas-sml.png)](designer-walkthrough-images/vs/09-imageview-on-canvas.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![ImageView na kanwie](designer-walkthrough-images/xs/08-imageview-on-canvas-sml.png)](designer-walkthrough-images/xs/08-imageview-on-canvas.png#lightbox)

-----

To `ImageView` będzie używany do wyświetlania w aplikacji przeglądarki kolor próbnika kolorów.

Następnie przeciągnij `LinearLayout (Vertical)` widżet z **przybornika** do projektanta. Należy pamiętać, że niebieski konspektu wskazuje granice dodany `LinearLayout`i **konspekt dokumentu** wskazuje, że znajduje się bezpośrednio pod `imageView1 (ImageView)`:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Niebieski konspektu](designer-walkthrough-images/vs/10-blue-outline-sml.png)](designer-walkthrough-images/vs/10-blue-outline.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Niebieski konspektu](designer-walkthrough-images/xs/10-blue-outline-sml.png)](designer-walkthrough-images/xs/10-blue-outline.png#lightbox)

-----

Po wybraniu `ImageView` w Projektancie niebieski konspektu przenosi otaczającego `ImageView`; na liście **konspekt dokumentu**, przenosi zaznaczenie do `imageView1 (ImageView)`:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Wybierz ImageView](designer-walkthrough-images/vs/11-select-imageview-sml.png)](designer-walkthrough-images/vs/11-select-imageview.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Wybierz ImageView](designer-walkthrough-images/xs/11-select-imageview-sml.png)](designer-walkthrough-images/xs/11-select-imageview.png#lightbox)

-----

Następnie przeciągnij `Text (Large)` widżet z **przybornika** do nowo dodany `LinearLayout`. Powiadomienie projektancie używany zielony wyróżnione wskaż, gdzie zostanie wstawiony nowego elementu widget:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Zielony najważniejsze funkcje](designer-walkthrough-images/vs/12-green-highlight-sml.png)](designer-walkthrough-images/vs/12-green-highlight.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Zielony najważniejsze funkcje](designer-walkthrough-images/xs/12-green-highlight-sml.png)](designer-walkthrough-images/xs/12-green-highlight.png#lightbox)

-----

Następnie dodaj `Text (Small)` poniżej elementu widget `Text (Large)` elementu widget:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Dodawanie elementu widget małego tekstu](designer-walkthrough-images/vs/13-add-small-text-sml.png)](designer-walkthrough-images/vs/13-add-small-text.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Dodawanie elementu widget małego tekstu](designer-walkthrough-images/xs/13-add-small-text-sml.png)](designer-walkthrough-images/xs/13-add-small-text.png#lightbox)

-----

W tym momencie projektanta powinien wyglądać Poniższy zrzut ekranu:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Układu projektanta](designer-walkthrough-images/vs/14-raw-layout-sml.png)](designer-walkthrough-images/vs/14-raw-layout.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Układu projektanta](designer-walkthrough-images/xs/14-raw-layout-sml.png)](designer-walkthrough-images/xs/14-raw-layout.png#lightbox)

-----

Jeśli dwa `textView` elementy widget nie są w `linearLayout1`, można je, aby przeciągnąć `linearLayout1` w **konspekt dokumentu** i umieść je, aby były wyświetlane, jak pokazano na poprzednim zrzucie ekranu (wcięta w obszarze `linearLayout1`).



### <a name="arranging-the-user-interface"></a>Rozmieszczanie interfejsu użytkownika

Umożliwia modyfikowanie interfejsu użytkownika, aby wyświetlić `ImageView` po lewej stronie z dwóch `TextView` elementy widget skumulowany z prawej strony `ImageView`.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  Wybierz `ImageView`.

2.  W **okna właściwości**, kliknij przycisk **według kategorii** sortowania ikony, jak i przewiń w dół do **układ — grupie widoków** sekcji.

3.  Zmień `layout_width` ustawienie `wrap_content`:

![Ustaw zawijania zawartości](designer-walkthrough-images/vs/15-wrap-content.png "zestaw zawijania zawartości")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1.  Z `ImageView` zaznaczone, kliknij przycisk **właściwości** kartę.

2.  Po prostu poniżej **właściwości** , kliknij pozycję **układu**.

3.  Przewiń w dół do **grupie widoków** i zmienić `Width` ustawienie `wrap_content`:

[![Zestaw zawijania zawartości](designer-walkthrough-images/xs/15-wrap-content-sml.png)](designer-walkthrough-images/xs/15-wrap-content.png#lightbox)

-----

Inny sposób, aby zmienić `Width` ustawienie służy do kliknij trójkąt po prawej stronie elementu widget, aby przełączyć jego ustawienie szerokości do `wrap_content`:

[![Przeciągnij, aby ustawić szerokości](designer-walkthrough-images/xs/16-width-arrow-sml.png)](designer-walkthrough-images/xs/16-width-arrow.png#lightbox)

Ponowne kliknięcie trójkąt zwraca `Width` ustawienie `match_parent`.

Następnie należy przełączyć się do **konspekt dokumentu** i wybierz katalog główny `LinearLayout`:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Wybierz główny LinearLayout](designer-walkthrough-images/vs/16-root-linearlayout-sml.png)](designer-walkthrough-images/vs/16-root-linearlayout.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Wybierz główny LinearLayout](designer-walkthrough-images/xs/17-root-linearlayout-sml.png)](designer-walkthrough-images/xs/17-root-linearlayout.png#lightbox)

-----
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Z certyfikatem głównym `LinearLayout` zaznaczone, wróć do **właściwości** okna, kliknij przycisk **alfabetycznie** sortowania ikony, jak i przewijania, do momentu znalezienia `orientation`. Zmień `orientation` ustawienie `horizontal`:

![Wybierz orientację poziomą](designer-walkthrough-images/vs/17-horizontal-orientation.png "wybierz orientacji poziomej")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Z certyfikatem głównym `LinearLayout` zaznaczone, wróć do **właściwości** i kliknij polecenie **elementu Widget**. Zmień `Orientation` ustawienie `horizontal`:

[![Wybierz orientacji poziomej](designer-walkthrough-images/xs/18-horizontal-orientation-sml.png)](designer-walkthrough-images/xs/18-horizontal-orientation.png#lightbox)

-----

W tym momencie projektanta powinien wyglądać Poniższy zrzut ekranu:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Układu projektanta](designer-walkthrough-images/vs/18-designer-layout-sml.png)](designer-walkthrough-images/vs/18-designer-layout.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Układu projektanta](designer-walkthrough-images/xs/19-designer-layout-sml.png)](designer-walkthrough-images/xs/19-designer-layout.png#lightbox)

-----


### <a name="modifying-the-spacing"></a>Modyfikacja odstępów

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Następnie firma Microsoft będzie zmodyfikować ustawienia dopełnienia i marginesów w interfejsie użytkownika, aby zapewnić więcej miejsca między elementy widget. Wybierz `ImageView`, kliknij przycisk **według kategorii** ikonę wyszukiwania w **właściwości** okno i przewiń w dół do **układu** sekcji. Zmień `Min Height` do `70dp`, `Min Width` do `50dp`i `padding` do `10dp`. Dotyczy to wypełnienia wokół wszystkich stron `ImageView` i elongates go w pionie:

[![Wartość uzupełniania](designer-walkthrough-images/vs/19-padding-widths-sml.png)](designer-walkthrough-images/vs/19-padding-widths.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Następnie firma Microsoft będzie zmodyfikować ustawienia dopełnienia i marginesów w interfejsie użytkownika, aby zapewnić więcej miejsca między elementy widget. Wybierz `ImageView` i kliknij przycisk **układu** w obszarze **właściwości**. Zmień `Padding` do `10dp`, `Min Width` do `50dp`i `Min Height` do `70dp`. Dotyczy to wypełnienia wokół wszystkich stron `ImageView` i elongates go w pionie:

[![Wartość uzupełniania](designer-walkthrough-images/xs/20-padding-widths-sml.png)](designer-walkthrough-images/xs/20-padding-widths.png#lightbox)

-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Prawy dolny, lewy i dopełnienia ustawienia z góry można skonfigurować niezależnie, wprowadzając wartości do `paddingBottom`, `paddingLeft`, `paddingRight`, i `paddingTop` pola odpowiednio.
Na przykład ustawić `paddingLeft` do `5dp` i `paddingBottom`, `paddingRight`, i `paddingTop` pól do `10dp`:

[![Ustawienia niestandardowe dopełnienia](designer-walkthrough-images/vs/20-custom-padding-sml.png)](designer-walkthrough-images/vs/20-custom-padding.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Prawej, dolnej i z lewej strony dopełnienie ustawienia można skonfigurować niezależnie, wprowadzając wartości do `Top`, `Right`, `Bottom`, i `Left` odpowiednio wypełniania pól. Na przykład ustawić `Left` wartość do uzupełniania `5dp` i `Top`, `Right`, i `Bottom` padding wartości `10dp`. Należy pamiętać, że `Padding` ustawienie zmienia się na listę wartości rozdzielaną przecinkami:

[![Ustawienia niestandardowe dopełnienia](designer-walkthrough-images/xs/21-custom-padding-sml.png)](designer-walkthrough-images/xs/21-custom-padding.png#lightbox)

-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Następnie dostosować położenie `LinearLayout` elementu widget, który zawiera dwa `TextView` elementy widget. W **konspekt dokumentu**, wybierz pozycję `linearLayout1`. W **właściwości** okna, przewiń do **układ — grupie widoków** sekcji. Ustaw `layout_marginBottom`, `layout_marginLeft`, `layout_marginRight`, i `layout_marginTop` do `5dp`, `5dp`, `0dp`, i `5dp` odpowiednio:

[![Ustawianie marginesów](designer-walkthrough-images/vs/21-margins-sml.png)](designer-walkthrough-images/vs/21-margins.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Następnie dostosować położenie `LinearLayout` elementu widget, który zawiera dwa `TextView` elementy widget. W **konspekt dokumentu**, wybierz pozycję `linearLayout1`. W **właściwości** okienku wybierz **układu** kartę. Przewiń w dół do **grupie widoków** sekcji i ustaw `Left`, `Top`, `Right`, i `Bottom` marży do `5dp`, `5dp`, `0dp`, i `5dp` odpowiednio:

[![Ustawianie marginesów](designer-walkthrough-images/xs/22-margins-sml.png)](designer-walkthrough-images/xs/22-margins.png#lightbox)

-----



### <a name="removing-the-default-image"></a>Usuwanie domyślnego obrazu

Ponieważ używamy `ImageView` wyświetlane kolory (zamiast obrazy), umożliwia usunięcie domyślne źródło obrazu dodane przez szablon.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  Wybierz `ImageView`.

2.  W **właściwości**, Znajdź `src` pola.

3.  Wyczyść `src` ustawienie, aby była pusta:

![Wyczyść ustawienie src ImageView](designer-walkthrough-images/vs/22-clear-img-src.png "wyczyść ustawienie src ImageView")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1.  Wybierz `ImageView`.

2.  Kliknij przycisk **elementu Widget** w obszarze **właściwości**.

3.  Wyczyść `Src` ustawienie, aby była pusta:

[![Wyczyść ustawienie src ImageView](designer-walkthrough-images/xs/23-clear-src-sml.png)](designer-walkthrough-images/xs/23-clear-src.png#lightbox)

-----


### <a name="adding-a-listview-container"></a>Dodawanie kontenera ListView

Teraz, gdy **ListItem** dodamy układ jest definiowany, `ListView` do układu głównego. To `ListView` będzie zawierać listę **elementy ListItems**.
W **przybornika**, zlokalizuj `ListView` elementu widget i przeciągnij go na powierzchnię projektu. `ListView` W Projektancie będą puste, z wyjątkiem niebieskie linie wskazujących obramowania, gdy jest wybrana. Możesz wyświetlić **konspekt dokumentu** do sprawdzenia, czy **ListView** została poprawnie dodana:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Nowy element ListView](designer-walkthrough-images/vs/23-new-listview.png "nowego elementu ListView")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Nowy element ListView](designer-walkthrough-images/xs/24-new-listview-sml.png)](designer-walkthrough-images/xs/24-new-listview.png#lightbox)

-----

Domyślnie `ListView` podano `Id` wartość `@+id/listView1`.
Otwórz **elementu Widget** w obszarze **właściwości** i zmienić `Id` do `@+id/myListView`:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Zmień identyfikator myListView](designer-walkthrough-images/vs/24-change-id.png "zmiany nazwy identyfikatora myListView")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Zmień identyfikator myListView](designer-walkthrough-images/xs/25-change-id-sml.png)](designer-walkthrough-images/xs/25-change-id.png#lightbox)

-----

W tym momencie nasze interfejs użytkownika jest gotowa do użycia.



### <a name="running-the-application"></a>Uruchamianie aplikacji


Otwórz **MainActivity.cs** i zastąp jego kod następującym kodem:

```csharp
using System;
using Android.App;
using Android.Content;
using Android.Runtime;
using Android.Views;
using Android.Widget;
using Android.OS;
using System.Collections.Generic;

namespace DesignerWalkthrough
{
    [Activity (Label = "DesignerWalkthrough", MainLauncher = true, Icon = "@drawable/icon")]
    public class MainActivity : Activity
    {
        List<ColorItem> colorItems = new List<ColorItem> ();
        ListView listView;

        protected override void OnCreate (Bundle savedInstanceState)
        {
            base.OnCreate (savedInstanceState);

            SetContentView (Resource.Layout.Main);
            listView = FindViewById<ListView> (Resource.Id.myListView);

            colorItems.Add (new ColorItem () { Color = Android.Graphics.Color.DarkRed,
                                               ColorName = "Dark Red", Code = "8B0000" });
            colorItems.Add (new ColorItem () { Color = Android.Graphics.Color.SlateBlue,
                                               ColorName = "Slate Blue", Code = "6A5ACD" });
            colorItems.Add (new ColorItem () { Color = Android.Graphics.Color.ForestGreen,
                                               ColorName = "Forest Green", Code = "228B22" });

            listView.Adapter = new ColorAdapter (this, colorItems);
        }
    }
    public class ColorAdapter : BaseAdapter<ColorItem>
    {
        List<ColorItem> items;
        Activity context;
        public ColorAdapter(Activity context, List<ColorItem> items)
            : base()
        {
            this.context = context;
            this.items = items;
        }
        public override long GetItemId(int position)
        {
            return position;
        }
        public override ColorItem this[int position]
        {
            get { return items[position]; }
        }
        public override int Count
        {
            get { return items.Count; }
        }
        public override View GetView (int position, View convertView, ViewGroup parent)
        {
            var item = items[position];

            View view = convertView;
            if (view == null) // no view to re-use, create new
                view = context.LayoutInflater.Inflate(Resource.Layout.ListItem, null);
            view.FindViewById<TextView>(Resource.Id.textView1).Text = item.ColorName;
            view.FindViewById<TextView>(Resource.Id.textView2).Text = item.Code;
            view.FindViewById<ImageView>(Resource.Id.imageView1).SetBackgroundColor(item.Color);

            return view;
        }
    }

    public class ColorItem
    {
        public string ColorName { get; set; }
        public string Code { get; set; }
        public Android.Graphics.Color Color { get; set; }
    }
}

```

Ten kod używa niestandardowego `ListView` karty, aby załadować informacji o kolorze i wyświetlić te dane w Interfejsie użytkownika właśnie utworzyliśmy. Aby zachować w tym przykładzie krótkie, informacji o kolorze jest ustalony na liście, ale karta może zostać zmodyfikowany do wyodrębnienia informacji o kolorze ze źródła danych lub można go obliczyć na bieżąco. Aby uzyskać więcej informacji na temat `ListView` kart, zobacz [widokach listy i karty](~/android/user-interface/layouts/list-view/index.md).

Skompiluj i uruchom aplikację. Poniższy zrzut ekranu znajduje się przykład wyświetlania aplikacji uruchomionej na urządzeniu:

[![Zrzut ekranu końcowego](designer-walkthrough-images/xs/26-final-screenshot-sml.png)](designer-walkthrough-images/xs/26-final-screenshot.png#lightbox)



## <a name="summary"></a>Podsumowanie

W tym artykule Rezygnacja za pośrednictwem jak utworzyć interfejs użytkownika za pomocą projektanta Xamarin.Android w programie Visual Studio for Mac. Następnie możemy wykazać, jak utworzyć interfejs dla pojedynczego elementu na liście.
Na bieżąco analizujemy jak dodać elementy widget i określania układu wizualnego, a także jak przydzielanie zasobów i ustawić różne właściwości dla tych elementów widget. Podsumowując firma Microsoft przedstawiono sposób uruchamiania utworzone w Projektancie interfejsu w przykładowej aplikacji.
