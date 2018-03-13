---
title: MonoTouch.Dialog Json Markup
ms.topic: article
ms.prod: xamarin
ms.assetid: 59F3E18C-3A73-69B8-DA5E-21B19B9DFB98
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 843e66a7979fc1aaa86371a3406c89af3f9ba967
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="monotouchdialog-json-markup"></a>MonoTouch.Dialog Json Markup

Na tej stronie opisano znaczników Json zaakceptowane przez jego MonoTouch.Dialog [JsonElement](https://developer.xamarin.com/api/type/MonoTouch.Dialog.JsonElement/)

Możemy zaczynać się przykładem. Poniżej znajduje się pełną pliku Json, które mogą zostać przekazane do JsonElement.

```csharp
{     
  "title": "Json Sample",
  "sections": [ 
      {
          "header": "Booleans",
          "footer": "Slider or image-based",
          "id": "first-section",
          "elements": [
              { 
                  "type" : "boolean",
                  "caption" : "Demo of a Boolean",
                  "value"   : true
              }, {
                  "type": "boolean",
                  "caption" : "Boolean using images",
                  "value"   : false,
                  "on"      : "favorite.png",
                  "off"     : "~/favorited.png"
              }, {
                      "type": "root",
                      "title": "Tap for nested controller",
                      "sections": [ {
                         "header": "Nested view!",
                         "elements": [
                           {
                             "type": "boolean",
                             "caption": "Just a boolean",
                             "id": "the-boolean",
                             "value": false
                           },
                           {
                             "type": "string",
                             "caption": "Welcome to the nested controller"
                           }
                         ]
                       }
                     ]
                   }
          ]
      }, {
          "header": "Entries",
          "elements" : [
              {
                  "type": "entry",
                  "caption": "Username",
                  "value": "",
                  "placeholder": "Your account username"
              }
          ]
      }
  ]
}
```

Kod znaczników powyżej tworzy następujące interfejsu użytkownika:

 [![](monotouch.dialog-json-markup-images/screen-shot-2012-03-02-at-11.31.31-am.png "Utworzone przez dany kod znaczników interfejsu użytkownika")](monotouch.dialog-json-markup-images/screen-shot-2012-03-02-at-11.31.31-am.png#lightbox)

Wartość każdego elementu w drzewie może zawierać właściwości `"id"`. Możliwe jest w czasie wykonywania można odwoływać się do poszczególnych sekcji lub elementów za pomocą indeksatora JsonElement. Jak to:

```csharp
var jsonElement = JsonElement.FromFile ("demo.json");

var firstSection = jsonElement ["first-section"] as Section;

var theBoolean = jsonElement ["the-boolean"] as BooleanElement
```

 <a name="Root_Element_Syntax" />


## <a name="root-element-syntax"></a>Składnia elementu głównego

Element główny zawiera następujące wartości:

-  `title`
-  `sections` (opcjonalnie)


Element główny może wystąpić wewnątrz sekcji jako element, aby utworzyć kontroler zagnieżdżonych. W takim przypadku dodatkowe właściwości `"type"` musi być ustawiona na `"root"`

 <a name="url" />


### <a name="url"></a>adres URL

Jeśli `"url"` właściwość jest ustawiona, jeśli użytkownik naciska w tym elementów RootElement, kod zażąda pliku z określonego adresu url i spowoduje, że zawartość nowe informacje wyświetlane. Umożliwia to tworzenie rozszerza interfejs użytkownika z serwera użytkownik naciska.

 <a name="group" />


### <a name="group"></a>Grupa

Jeśli ustawiona, to ustawienie groupname dla elementu głównego. Nazwy grupy są używane do pobrania podsumowania, który jest wyświetlany jako wartość elementu głównego z jednego z elementów zagnieżdżonych w elemencie. Jest to wartość pola wyboru lub wartość przycisku radiowego.

 <a name="radioselected" />


### <a name="radioselected"></a>radioselected

Identyfikuje element opcji wybranej w elementów zagnieżdżonych

 <a name="title" />


### <a name="title"></a>Tytuł

Jeśli jest obecny, będzie używany do elementów RootElement tytuł

 <a name="type" />


### <a name="type"></a>— typ

Należy wybrać opcję `"root"` gdy pojawia się w sekcji (używane Aby zagnieździć kontrolerów).

 <a name="sections" />


### <a name="sections"></a>sekcje

To jest tablica Json o poszczególnych sekcji

 <a name="Section_Syntax" />


## <a name="section-syntax"></a>Składnia sekcji

Sekcja zawiera:

-  `header` (opcjonalnie)
-  `footer` (opcjonalnie)
-  `elements` Tablica


 <a name="header" />


### <a name="header"></a>nagłówek

Jeśli istnieje, tekst nagłówka jest wyświetlana jako tytuł sekcji.

 <a name="footer" />


### <a name="footer"></a>Stopki

Jeśli jest obecny, stopka jest wyświetlana w dolnej części sekcji.

 <a name="elements" />


### <a name="elements"></a>elementy

To jest tablica elementów. Każdy element musi zawierać co najmniej jeden klucz `"type"` klucz, który służy do identyfikowania rodzaj elementu do utworzenia.
Niektóre elementy udostępnianie niektóre typowe właściwości, takie jak `"caption"` i `"value"`. Poniżej przedstawiono listę obsługiwanych elementów:

-  `string` elementy (zarówno z włączonymi i wyłączonymi stylów)
-  `entry` (regular lub hasła)
-  `boolean` wartości (z wykorzystaniem przełączników lub obrazy)


Elementy ciąg może służyć jako przyciski dzięki udostępnieniu metod do wywołania, gdy użytkownik naciska w komórce lub akcesoriów,

 <a name="Rendering_Elements" />


## <a name="rendering-elements"></a>Renderowanie elementów

Renderowania elementów są oparte na StringElement C# i StyledStringElement i renderują informacje na różne sposoby jest możliwe do renderowania je na różne sposoby. Najprostsza elementy mogą być tworzone następująco:

```csharp
{
        "type": "string",
        "caption": "Json Serializer",
}
```

Wyświetli prostego ciągu z ustawień domyślnych: czcionka, tło, kolor tekstu i dekoracje. Umożliwia podłączanie akcje te elementy są i będą zachowywać się jak przyciski przez ustawienie `"ontap"` właściwości lub `"onaccessorytap"` właściwości:

```csharp
{
    "type":    "string",
        "caption": "View Photos",
        "ontap:    "Acme.PhotoLibrary.ShowPhotos"
}
```

Powyższego wywołania metody "ShowPhotos" w klasie "Acme.PhotoLibrary". `"onaccessorytap"` Jest podobny, ale będzie można wywołać tylko, jeśli użytkownik naciska na akcesoriów zamiast naciśnięcie komórkę. Aby je włączyć, należy także ustawić urządzenia:

```csharp
{
    "type":     "string",
        "caption":  "View Photos",
        "ontap:     "Acme.PhotoLibrary.ShowPhotos",
        "accessory: "detail-disclosure",
        "onaccessorytap": "Acme.PhotoLibrary.ShowStats"
}
```

Renderowanie elementów można wyświetlić dwa ciągi jednocześnie, jest jeden podpis i inny jest wartość. Jak mają być renderowane w tych ciągów są uzależnione od styl, tę można skonfigurować przy użyciu `"style"` właściwości. Wartość domyślna będzie wyświetlany podpis po lewej i po prawej stronie wartości. Zobacz sekcję dotyczącą stylu, aby uzyskać więcej informacji. Kolory są zakodowane przy użyciu symbol "#" następuje numery szesnastkowych, które reprezentują wartości dla wartości czerwonemu, zielonemu, niebieska i może być alfa. Zawartość może być zakodowany za krótka forma (3 lub 4 cyfr szesnastkowych), który reprezentuje wartości RGB lub RGBA. Lub długą formę (6 lub 8 cyfr), które reprezentują wartości RGB lub RGBA. Krótka wersja jest skrócona do pisania tego samego cyfrę szesnastkową dwa razy. Aby stała "#1bc" jest interpretowany jako czerwony = 0x11, zielony = 0xbb i niebieski = 0xcc. Jeśli nie ma wartości alfa, kolor jest nieprzezroczysta. Kilka przykładów:

```csharp
"background": "#f00"
"background": "#fa08f880"
```

 <a name="accessory" />


### <a name="accessory"></a>Dodatek

Określa rodzaj akcesoriów ma być wyświetlany w nazwę elementu renderowania możliwe wartości to:

-  `checkmark`
-  `detail-disclosure`
-  `disclosure-indicator`


Jeśli wartość nie jest obecny, dodatek nie jest wyświetlana

 <a name="background" />


### <a name="background"></a>tło

Właściwość tła Ustawia kolor tła dla komórki. Wartość jest albo adres URL obrazu (w takim przypadku zostanie wywołany, narzędzie do pobierania obrazu async i tła zostanie zaktualizowany po obrazu jest pobierana) lub może być kolor określone przy użyciu składni kolorów.

 <a name="caption" />


### <a name="caption"></a>Podpis

Ciąg głównej ma być wyświetlany w elemencie renderowania. Czcionek i kolorów może zostać dostosowane przez ustawienie `"textcolor"` i `"font"` właściwości. Styl renderowania jest określany przez `"style"` właściwości.

 <a name="color_and_detailcolor" />


### <a name="color-and-detailcolor"></a>kolor i detailcolor

Kolor, który ma być używany dla głównego tekst lub szczegółowy.

 <a name="detailfont_and_font" />


### <a name="detailfont-and-font"></a>detailfont i czcionki

Czcionki do użycia na potrzeby podpis lub tekst szczegółów. Format specyfikacji czcionki jest nazwa czcionki, a następnie opcjonalnie kreska i rozmiar w punktach.
Specyfikacje poprawną czcionkę są następujące:

-  "Helvetica"
-  "Helvetica 14"


 <a name="linebreak" />


### <a name="linebreak"></a>linebreak

Określa, jak podzielić wierszy. Możliwe wartości to:

-  `character-wrap`
-  `clip`
-  `head-truncation`
-  `middle-truncation`
-  `tail-truncation`
-  `word-wrap`


Zarówno `character-wrap` i `word-wrap` mogą być używane razem z `"lines"` zestaw właściwości wartości zero spowoduje przekształcenie elementu renderowania w elemencie wiele wierszy.

 <a name="ontap_and_onaccessorytap" />


### <a name="ontap-and-onaccessorytap"></a>ontap i onaccessorytap

Te właściwości musi wskazywać nazwę metody statycznej w aplikacji, która przyjmuje obiekt jako parametr. Podczas tworzenia hierarchii przy użyciu metod JsonDialog.FromFile lub JsonDialog.FromJson należy przekazać wartość opcjonalna obiektu. Wartość tego obiektu są następnie przekazywane do metody. Możesz użyć tego do przekazania do metody statycznej określony kontekst. Na przykład:

```csharp
class Foo {
    Foo ()
    {
        root = JsonDialog.FromJson (myJson, this);
    }

    static void Callback (object obj)
    {
        Foo myFoo = (Foo) obj;
        obj.Callback ();
    }
}
```

 <a name="lines" />


### <a name="lines"></a>linie

Jeśli ta jest równa zero, spowoduje to, że rozmiar automatycznie elementu w zależności od zawartości ciągów zawartych. Aby to zrobić, należy także ustawić `"linebreak"` właściwości `"character-wrap"` lub `"word-wrap"`.

 <a name="style" />


### <a name="style"></a>— styl

Styl określa rodzaj stylu komórki, który będzie używany do renderowania zawartości i odpowiada wartości wyliczenia UITableViewCellStyle.
Możliwe wartości to:

-  `"default"`
-  `"value1"`
-  `"value2"`
-  `"subtitle"` : tekst z pomocniczą.


 <a name="subtitle" />


### <a name="subtitle"></a>Podtytuł

Wartość do użycia na potrzeby podtytuł. Jest to skrót do ustawiania stylu jako `"subtitle"` i ustaw `"value"` właściwości na ciąg.
To jest zarówno pojedynczy wpis.

 <a name="textcolor" />


### <a name="textcolor"></a>textColor

Kolor tekstu.

 <a name="value" />


### <a name="value"></a>value

Wartość dodatkowej ma być wyświetlany w elemencie renderowania. Wpływ na układ to `"style"` ustawienie. Czcionek i kolorów może zostać dostosowane przez ustawienie `"detailfont"` i `"detailcolor"`.

 <a name="Boolean_Elements" />


## <a name="boolean-elements"></a>Wartość logiczna elementów

Logiczna elementów należy ustawić typ `"bool"`, może zawierać `"caption"` do wyświetlenia i `"value"` jest ustawiona na wartość true lub false. Jeśli `"on"` i `"off"` właściwości są ustawione, ich muszą być obrazów. Obrazy zostały rozwiązane względem bieżącego katalogu roboczego w aplikacji. Jeśli chcesz pakietu względem plików, możesz użyć `"~"` jako skrót do reprezentowania katalogu pakietu aplikacji. Na przykład `"~/favorite.png"` będzie favorite.png, który jest zawarty w pliku pakietu. Na przykład:

```csharp
{ 
    "type" : "boolean",
    "caption" : "Demo of a Boolean",
    "value"   : true
},

{
    "type": "boolean",
    "caption" : "Boolean using images",
    "value"   : false,
    "on"      : "favorite.png",
    "off"     : "~/favorited.png"
}
```

 <a name="type" />


### <a name="type"></a>— typ

Typ może być ustawiona jako `"boolean"` lub `"checkbox"`. Jeśli ustawiono wartość logiczna użyje UISlider lub obrazów (jeśli obie `"on"` i `"off"` są ustawione). Jeśli ustawiono wartość pola wyboru, zostanie użyty pola wyboru. `"group"` Właściwości może służyć do tagu elementu logiczną jako należące do określonej grupy. Jest to przydatne, jeśli ma również zawierającego głównego `"group"` właściwość jako katalog główny podsumowanie wyników wraz z liczbą wszystkie wartości logiczne (lub pola wyboru) należące do tej samej grupy.

 <a name="Entry_Elements" />


## <a name="entry-elements"></a>Wpis elementów

Zezwalaj użytkownikom na wprowadzanie danych za pomocą elementów wpisu. Typ wpisu elementów jest `"entry"` lub `"password"`. `"caption"` Właściwości ustawiono tekst do wyświetlenia po prawej stronie oraz `"value"` jest ustawiony na wartość początkową Aby ustawić wartość wpisu. `"placeholder"` Służy do wyświetlenia wskazówki użytkownikowi pustych pozycji (stanowi szarym). Oto kilka przykładów:

```csharp
{
        "type": "entry",
        "caption": "Username",
        "value": "",
        "placeholder": "Your account username"
}, {
        "type": "password",
        "caption": "Password",
        "value": "",
        "placeholder": "You password"
}, {
        "type": "entry",
        "caption": "Zip Code",
        "value": "01010",
        "placeholder": "your zip code",
        "keyboard": "numbers"
}, {
        "type": "entry",
        "return-key": "route",
        "caption": "Entry with 'route'",
        "placeholder": "captialization all + no corrections",
        "capitalization": "all",
        "autocorrect": "no"
}
```

 <a name="autocorrect" />


### <a name="autocorrect"></a>Autokorekty

Określa styl korekty automatycznej dla wpisu. Możliwe wartości to true lub false (lub ciągi `"yes"` i `"no"`).

 <a name="capitalization" />


### <a name="capitalization"></a>Wielkość liter

Styl dla wpisu. Możliwe wartości to:

-  `all`
-  `none`
-  `sentences`
-  `words`


 <a name="caption" />


### <a name="caption"></a>Podpis

Podpis dla wejścia

 <a name="keyboard" />


### <a name="keyboard"></a>klawiatura

Typ klawiatury służące do wprowadzania danych. Możliwe wartości to:

-  `ascii`
-  `decimal`
-  `default`
-  `email`
-  `name`
-  `numbers`
-  `numbers-and-punctuation`
-  `twitter`
-  `url`


 <a name="placeholder" />


### <a name="placeholder"></a>Symbol zastępczy

Wskazówka tekst wyświetlany, gdy wpis ma wartość pustą.

 <a name="return-key" />


### <a name="return-key"></a>klawisz Return

Etykieta używany dla klawisz return. Możliwe wartości to:

-  `default`
-  `done`
-  `emergencycall`
-  `go`
-  `google`
-  `join`
-  `next`
-  `route`
-  `search`
-  `send`
-  `yahoo`


 <a name="value" />


### <a name="value"></a>value

Wartość początkowa wpisu

 <a name="Radio_Elements" />


## <a name="radio-elements"></a>Elementy opcji

Elementy opcji mają typ `"radio"`. Wybrany element jest wybierany przez `radioselected` właściwości na jego zawierającego element główny.
Ponadto jeśli jest ustawiona wartość `"group"` właściwość, ten przycisk radiowy, należy do tej grupy.

 <a name="Date_and_Time_Elements" />


## <a name="date-and-time-elements"></a>Data i godzina elementów

Typ elementu `"datetime"`, `"date"` i `"time"` służą do renderowania daty i godziny, daty i godziny. Te elementy przyjmować jako parametry podpisu i wartości. Wartość można pisać w dowolnym formacie obsługiwane przez funkcję .NET DateTime.Parse. Przykład:

```csharp
"header": "Dates and Times",
"elements": [
        {
                "type": "datetime",
                "caption": "Date and Time",
                "value": "Sat, 01 Nov 2008 19:35:00 GMT"
        }, {
                "type": "date",
                "caption": "Date",
                "value": "10/10"
        }, {
                "type": "time",
                "caption": "Time",
                "value": "11:23"
                }                       
]
```

 <a name="Html/Web_Element" />


## <a name="htmlweb-element"></a>Element HTML/sieci Web

Można utworzyć komórki który wybrany zostanie osadzić UIWebView, który renderuje zawartość określony adres URL lokalnego lub zdalnego za pomocą `"html"` typu. Tylko dwie właściwości dla tego elementu są `"caption"` i `"url"`:

```csharp
{
        "type": "html",
        "caption": "Miguel's blog",
        "url": "http://tirania.org/blog" 
}
```
