---
title: PassKit
description: "Portfela jest system aplikacji dla systemu iOS, która przechowuje i wyświetla kodów kreskowych i inne informacje, aby połączyć transakcji odbiorcy na telefon z świecie rzeczywistym."
ms.topic: article
ms.prod: xamarin
ms.assetid: 74B9973B-C1E8-B727-3F6D-59C1F98BAB3A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: beff54d2b2bb72b2adf1e77819c56004b92e13f7
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="passkit"></a>PassKit

_Portfela jest system aplikacji dla systemu iOS, która przechowuje i wyświetla kodów kreskowych i inne informacje, aby połączyć transakcji odbiorcy na telefon z świecie rzeczywistym._

Portfela jest aplikacja dla urządzenia iPhone i iPod sięga z systemem iOS 6. Przechowuje ona i wyświetla kodów kreskowych i inne informacje, aby połączyć transakcji odbiorcy na telefon z świecie rzeczywistym. Przekazuje są generowane przez sprzedawców i wysłane do klienta za pośrednictwem poczty e-mail, adresy URL lub z poziomu aplikacji dla systemu iOS własnych sklepu. Portfela przechowuje i organizuje wszystkie przebiegi na telefonie, a następnie wyświetla przypomnienia przebiegu na ekranie blokady w zależności od daty/godziny lub lokalizacji urządzenia.

Ten dokument wprowadza portfela, przy użyciu interfejsu API zestawu przekazać z Xamarin.iOS i omówiono sposób wdrożenia przekazuje na serwerze.

 [ ![](passkit-images/image1.png "Portfela przechowuje i organizuje wszystkie przebiegi na telefonie")](passkit-images/image1.png)


## <a name="requirements"></a>Wymagania

Funkcje zestawu magazynu omówionych w tym dokumencie wymagają iOS 6 lub Xcode 4.5, wraz z Xamarin.iOS 6.0.


## <a name="introduction"></a>Wprowadzenie

Problem klucza, który rozwiązuje przekazać Kit jest dystrybucji i kodów kreskowych zarządzania. Niektóre praktyczne obecnie używania kodów kreskowych należą:

-   **Kupowanie online biletów filmu** — klienci są zwykle pocztą e-mail kod kreskowy, który reprezentuje bilet. Ten kod kreskowy drukowany i podjąć w celu kinowych dla wpisu.
-   **Karty lojalność** — klienci zawiera szereg różnych kart specyficzne dla magazynu w użyciu lub portfela, wyświetlania i skanowania, gdy ich zakupu towarów.
-   **Bony** — kupony są dystrybuowane za pośrednictwem poczty e-mail, jako drukowania strony sieci web za pośrednictwem letterboxes i jako kodów kreskowych w gazetach i magazyny. Klienci dostosować je do magazynu skanowania do odbierania towarów, usług lub rabaty w zamian.
-   **Wsiadającymi przekazuje** — podobny do kupowania biletu filmu.


Zestaw przebiegu oferuje alternatywę dla każdego z tych scenariuszy:

-   **Film biletów** — po zakupie, klient dodaje przebiegu biletu zdarzenia (za pośrednictwem poczty e-mail lub łącze do witryny sieci Web). Jako czas dla metod filmu przebiegu będzie automatycznie wyświetlane na ekranie blokady przypomnieniem o, a po przybyciu kinowych przebiegu można łatwo pobrać i wyświetlane w portfela skanowania.
-   **Karty lojalność** — zamiast (lub oprócz) udostępniają kart fizycznych, magazynów można wydać (za pośrednictwem poczty e-mail lub po logowania witryny sieci Web) przebiegu karty magazynu. Magazyn zapewnia dodatkowe funkcje, takie jak Aktualizowanie salda konta w przebiegu za pośrednictwem powiadomień wypychanych i za pomocą usług używanie funkcji geolokalizacji przebiegu może automatycznie są wyświetlane na ekranie blokady po klienta znajduje się w pobliżu lokalizacji magazynu.
-   **Bony** — przekazuje kupon łatwo można wygenerować za pomocą unikatowych parametrów, aby pomóc w śledzeniu i dystrybuowane za pośrednictwem poczty e-mail lub witryna sieci Web łącza. Pobrany kupony można automatycznie wyświetlane na ekranie blokady, gdy użytkownik jest niemal określonej lokalizacji i/lub danego dnia (na przykład gdy zbliża się data wygaśnięcia). Ponieważ kupony są przechowywane na telefon użytkownika, są zawsze pod ręką i nie zagubieniu. Bony może zachęcamy klientów do pobierania aplikacji pomocnika, ponieważ łącza do sklepu z aplikacjami można zintegrować przebiegu zwiększenia interakcji użytkowników z klienta.
-   **Wsiadającymi przekazuje** — po zakończeniu online zaewidencjonowania, klient otrzyma ich przebiegu dołączania za pośrednictwem poczty e-mail lub łącza. To aplikacja pomocnika udostępniane przez dostawcę transportu można obejmują procesu wyboru i również umożliwiać odbiorcy do wykonywania dodatkowych funkcji, takich jak wybrać ich stanowisko lub zawierają najważniejsze nowości. Dostawca transportu umożliwia wysyłanie powiadomień wypychanych przebiegu aktualizacji, jeśli transport jest opóźnione lub anulowane. Jako czas wsiadania podejścia przebiegu będzie widoczny na ekranie blokady przypomnienia i zapewniają szybki dostęp do przebiegu.


Zasadniczo przekazać zestawu zapewnia prosty i wygodny sposób przechowywania i wyświetlania kody kreskowe na urządzeniu iPhone lub iPod touch. Dodatkowy czas i lokalizacja integracji ekran blokady powiadomień wypychanych i pomocnika aplikacji zintegrować ją oferty a foundation bardzo zaawansowane sprzedaży biletów i rozliczeń usługi.


## <a name="pass-kit-ecosystem"></a>Przekaż ekosystemu Kit

Przebieg zestawu nie jest tylko interfejs API w CocoaTouch, a nie jest częścią większej ekosystemem aplikacji, danych i usług, które ułatwiają bezpiecznego udostępniania i zarządzania kodów kreskowych i innych danych. Ten diagram wysokiego poziomu przedstawia różnych obiektów, które mogą być wykonywane w tworzenie i używanie przekazuje:

 [ ![](passkit-images/image2.png "Ten wysokiego poziomu diagram przedstawia jednostek zaangażowane w tworzenie i używanie przebiegów")](passkit-images/image2.png)

Każda część ekosystemu ma jasno określonych ról:

-   **Portfela** — aplikację wbudowanych iOS firmy Apple (dla urządzenia iPhone i iPod touch), która przechowuje i wyświetla przebiegów. Jest to miejsce tylko, że mają być renderowane przekazuje do użytku w rzeczywistych (ie kod kreskowy zostanie wyświetlona, wraz z wszystkich danych zlokalizowanego w przebiegu).
-   **Pomocniczy aplikacji** — aplikacje dla systemu iOS 6, utworzony przez dostawców przebiegu, aby rozszerzyć funkcjonalność przebiegi, które wydają, takie jak dodanie wartości do karty magazynu, zmiana stanowisko na przebieg wsiadania lub innych funkcji specyficznych dla firm. Pomocnik aplikacje nie są wymagane do przekazywania powinna być użyteczna.
-   **Serwer** — serwer bezpieczny, gdzie przekazuje może być wygenerowany i podpisany do dystrybucji. Aplikacji Pomocnik może połączyć się z serwerem, aby wygenerować nowe przebiegi lub aktualizacje do istniejących przekazuje żądania. Opcjonalnie może implementować interfejsu API usługi sieci web, który portfela spowodowałoby wywołanie do przebiegów aktualizacji.
-   **Serwery APNS** — serwer ma możliwość powiadomić portfela aktualizacji do przekazywania na danym urządzeniu przy użyciu usługi APNS. Powiadomienie wypychane do portfela, który zostanie następnie skontaktuj się z serwerem szczegółowe informacje o zmianie. Nie trzeba zaimplementować APNS dla tej funkcji Pomocnik aplikacji (można słuchać `PKPassLibraryDidChangeNotification` ).
-   **Kanał aplikacji** — aplikacje, który nie jest bezpośrednio modyfikowany przekazuje (np. aplikacje pomocnika czy), ale które mogą zwiększyć ich narzędzie przez rozpoznawania przekazuje i stosowanie ich do dodania do portfela. Klientów poczty, przeglądarek sieci społecznościowych i innych aplikacjach agregacji danych wszystkie napotkać załączników lub łącza do przebiegów.


Cały ekosystem wygląda złożony, więc warto zauważyć, że niektóre składniki są opcjonalne i możliwe jest znacznie prostsza implementacje przekazać zestawu.

## <a name="what-is-a-pass"></a>Co to jest przebiegu?

Przebieg jest kolekcją dane reprezentujące biletu, dywidendy lub karty. Mogą być przeznaczone do jednorazowego użytku przez osobę (i w związku z tym zawiera szczegółowe informacje, takie jak numer i stanowisko alokacji transmitowane) lub może ona wiele token użycia, który może być współużytkowane przez dowolną liczbę użytkowników (np. kupon dyskontowa). Szczegółowy opis znajduje się w firmy Apple [o przekazać pliki](https://developer.apple.com/library/prerelease/ios/#documentation/UserExperience/Reference/PassKit_Bundle/Chapters/Introduction.html) dokumentu.


### <a name="types"></a>Types

Obecnie obsługiwane pięć typów, które można rozróżnić w aplikacji portfela układ i górnej krawędzi przebiegu:

-  **Zdarzenie biletu** — mała półokrągłymi wycinania.
-   **Przebieg dołączania** — pozycje w ikona obok siebie, specyficzne dla transportu można określić (np.) Magistrala pociągu, samolotowy).
-   **Przechowywanie karty** — zaokrąglony top, takie jak karta kredytowa lub debetowa.
-  **Kupon** — perforowanym wzdłuż górnej.
-  **Ogólny** — taki sam jak karta magazynu, zaokrąglony top.


Przebieg pięć typów są wyświetlane w tym zrzucie ekranu pokazano (w kolejności: papieru wartościowego, ogólny, przechowywać karty, przebieg wsiadania i biletów zdarzeń):

 [ ![](passkit-images/image3.png "Przebieg pięć typów są wyświetlane w tym zrzut ekranu")](passkit-images/image3.png)

### <a name="file-structure"></a>Struktura pliku

Plik — dostęp próbny jest rzeczywiście archiwum ZIP z **.pkpass** rozszerzenia niektórych określonych JSON plików (wymagane), różnych obrazu plików (opcjonalnie) oraz zlokalizowanych ciągów (również opcjonalnie).

-   **pass.JSON** — jest to wymagane. Zawiera wszystkie informacje o przebiegu.
-   **Manifest.JSON** — jest to wymagane. Zawiera wartości skrótu SHA1 dla każdego pliku w przebiegu, z wyjątkiem pliku podpisu i ten plik (manifest.json).
-   **podpis** — jest to wymagane. Utworzone przez podpisywania `manifest.json` pliku z certyfikatem wygenerowane w portalu inicjowania obsługi systemu iOS.
-  **logo.PNG** — jest to opcjonalne.
-  **Background.PNG** — jest to opcjonalne.
-  **icon.PNG** — jest to opcjonalne.
-  **Ciągi zlokalizowania plików** — jest to opcjonalne.


Poniżej przedstawiono struktura katalogów plik — dostęp próbny (jest to zawartość archiwum ZIP):

 [ ![](passkit-images/image4.png "W tym miejscu przedstawiono struktura katalogów plik — dostęp próbny")](passkit-images/image4.png)

### <a name="passjson"></a>pass.json

JSON się, ponieważ przekazuje są zwykle tworzone na serwerze — oznacza, że generowanie kodu jest niezależny od platformy na serwerze. Są trzy kluczowych informacji w każdym przebiegu:

-   **teamIdentifier** — w ten sposób wszystkie przebiegi Generowanie do konta sklepu z aplikacjami. Ta wartość jest widoczny w portalu inicjowania obsługi systemu iOS.
-   **passTypeIdentifier** — rejestrowanie w portalu inicjowania obsługi administracyjnej do grupy razem przekazuje (jeśli można utworzyć więcej niż jednego typu). Na przykład w kawiarni może utworzyć magazynu przekazać karty umożliwia klientom uzyskanie środków lojalność, ale również oddzielne przekazać typ kuponu do tworzenia i rozpowszechniania kupony. Tego samego kawiarni może nawet mógł pomieścić zdarzenia utworów muzycznych na żywo i wystawiania przekazuje biletu zdarzenia dla tych.
-   **numer seryjny** — unikatowy ciąg w ramach tego `passTypeidentifier` . Wartość jest nieprzezroczysta dla portfela, ale jest ważne w przypadku śledzenia określonych przebiegi podczas komunikacji z serwerem.


Istnieje wiele kluczy JSON w każdym przebiegu, poniżej przedstawiono przykład:

``` 
{
   "passTypeIdentifier":"com.xamarin.passkitdoc.banana",  //Type Identifier (iOS Provisioning Portal)
   "formatVersion":1,                                     //Always 1 (for now)
   "organizationName":"Xamarin",                          //The name which appears on push notifications
   "serialNumber":"12345436XYZ",                          //A number for you to identify this pass
   "teamIdentifier":"XXXAAA1234",                         //Your Team ID
   "description":"Xamarin Demo",                          //
   "foregroundColor":"rgb(54,80,255)",                    //color of the data text (note the syntax)
   "backgroundColor":"rgb(209,255,247)",                  //color of the background
   "labelColor":"rgb(255,15,15)",                         //color of label text and icons
   "logoText":"Banana ",                                  //Text that appears next to logo on top
   "barcode":{                                            //Specification of the barcode (optional)
      "format":"PKBarcodeFormatQR",                       //Format can be QR, Text, Aztec, PDF417
      "message":"FREE-BANANA",                            //What to encode in barcode
      "messageEncoding":"iso-8859-1"                      //Encoding of the message
   },
   "relevantDate":"2012-09-15T15:15Z",                    //When to show pass on screen. ISO8601 formatted.
  /* The following fields are specific to which type of pass. The name of this object specifies the type, e.g., boardingPass below implies this is a boarding pass. Other options include storeCard, generic, coupon, and eventTicket */
   "boardingPass":{
/*headerFields, primaryFields, secondaryFields, and auxiliaryFields are arrays of field object. Each field has a key, label, and value*/
      "headerFields":[          //Header fields appear next to logoText
         {
            "key":"h1-label",   //Must be unique. Used by iOS apps to get the data.
            "label":"H1-label", //Label of the field
            "value":"H1"        //The actual data in the field
         },
         {
            "key":"h2-label",
            "label":"H2-label",
            "value":"H2"
         }
      ],
      "primaryFields":[       //Appearance differs based on pass type
         {
            "key":"p1-label",
            "label":"P1-label",
            "value":"P1"
         }
      ],
      "secondaryFields":[     //Typically appear below primaryFields
         {
            "key":"s1-label",
            "label":"S1-label",
            "value":"S1"
         }
      ],
      "auxiliaryFields":[    //Appear below secondary fields
         {
            "key":"a1-label",
            "label":"A1-label",
            "value":"A1"
         }
      ],
      "transitType":"PKTransitTypeAir"  //Only present in boradingPass type. Value can
                                        //Air, Bus, Boat, or Train. Impacts the picture
                                        //that shows in the middle of the pass.
   }
}
```

### <a name="barcodes"></a>Kodów kreskowych

Obsługiwane są tylko 2D formaty: typu PDF417, Aztec, QR. Apple oświadczeń, że nieodpowiedni do skanowania na ekranie phone podświetlenia kodów kreskowych 1D.

Tekst alternatywny wyświetlany poniżej kod kreskowy jest opcjonalny — niektóre sprzedawców chcesz mieć możliwość odczytu/typu ręcznie.

Kodowanie ISO 8859-1 jest najbardziej typowych wyboru, kodowanie, które jest używane przez skanowania systemów, które będzie odczytywać Twoje przebiegów.

### <a name="relevancy-lock-screen"></a>Dokładność (ekranu blokady)

Istnieją dwa typy danych, które mogą powodować przebiegu, który będzie wyświetlany na ekranie blokady:

 **Lokalizacja**

Można określić maksymalnie 10 lokalizacje w przebiegu, np magazynów, które klient często odwiedza lub lokalizacji kinowych lub lotniska. Klienta można ustawić tych lokalizacji za pomocą aplikacji Asystent lub dostawcę można określić je z danych użycia (jeśli są zbierane za zgodą klienta).

Podczas przebiegu jest wyświetlany na ekranie blokady, ogrodzenia jest obliczana tak, że gdy użytkownik opuszcza obszar przebiegu jest ukryta na ekranie blokady. Promień jest powiązany do przekazania stylu, aby uniemożliwić nadużycia.

 **Data i godzina**

Można określić tylko jeden daty/godziny w przebiegu. Data i godzina jest przydatna do wyzwalania powiadomień ekranu blokady dla wstępu i biletów zdarzeń.

Można aktualizować za pomocą wypychania lub za pomocą interfejsu API PassKit, tak, aby można było zaktualizować daty/godziny w przypadku wielokrotnego użytku bilet (na przykład biletu sezonu wojennych lub sportowych złożone).

### <a name="localization"></a>Lokalizacja

Tłumaczenie przebiegu w wielu językach jest podobny do lokalizowania aplikacji systemu iOS — Utwórz język określone katalogi z `.lproj` rozszerzenia i umieścić zlokalizowanych elementów wewnątrz. Tłumaczeń tekstu powinny być wprowadzane do `pass.strings` plików, gdy zlokalizowane obrazy powinny mieć taką samą nazwę jak obraz zastępują one w folderze głównym przebiegu.

## <a name="security"></a>Zabezpieczenia

Przekazuje są podpisane za pomocą certyfikatu prywatnego generowany w portalu inicjowania obsługi systemu iOS. Dostępne są następujące kroki, aby zarejestrować przebiegu:

1.  Obliczenia dla każdego pliku w katalogu przebiegu skrótu SHA1 (nie dołączaj `manifest.json` lub `signature` pliku, które powinny istnieć na tym etapie mimo to).
1.  Zapis `manifest.json` jako JSON klucza i wartości listę nazw plików z jego skrót.
1.  Zaloguj się przy użyciu certyfikatu `manifest.json` plik i zapisać wynik w pliku o nazwie `signature` .
1.  ZIP wszystko i nadaj wynikowy plik `.pkpass` rozszerzenia pliku.


Ponieważ klucz prywatny jest wymagany do podpisywania przebiegu, ten proces należy wykonać jedynie na serwera zabezpieczeń, którą kontrolujesz. NIE PRZEPROWADZAJ dystrybucji kluczy Generowanie przebiegów w aplikacji i spróbuj.

 
## <a name="configuration-and-setup"></a>Konfiguracja i ustawienia

Ta sekcja zawiera szczegółowe instrukcje dotyczące instalacji Twoje dane inicjowania obsługi administracyjnej i utworzyć Twojego pierwszego przejścia.

### <a name="provisioning-passkit"></a>Inicjowanie obsługi administracyjnej PassKit

Aby do przekazywania do sklepu z aplikacjami należy wprowadzić muszą być połączone z konta dewelopera. To wymaga wykonania dwóch kroków:

1.  Przebiegu musi być zarejestrowana przy użyciu unikatowego identyfikatora, nazywany identyfikatorem przekazać typu.
1.  Musi zostać wygenerowany certyfikat do podpisania przebiegu za dewelopera podpisu cyfrowego.

Aby utworzyć następujące przekazania identyfikator typu.


<a name="create-passid"/>

#### <a name="create-a-pass-type-id"></a>Utwórz identyfikator typu — dostęp próbny

Pierwszym krokiem jest ustanowienie identyfikator typu przekazać dla każdego innego _typu_ przebiegu obsługiwany. Identyfikator przekazać (lub identyfikator typu przekazać) tworzy unikatowy identyfikator dla przebiegu. Aby połączyć przebiegu z konta dewelopera przy użyciu certyfikatu użyjemy tego Identyfikatora.

1. W [sekcji certyfikaty identyfikatory i profile systemu IOS w portalu inicjowania obsługi](https://developer.apple.com/account/overview.action), przejdź do **identyfikatory** i wybierz **przekazać identyfikatorów typu** . Następnie wybierz  **+**  przycisk, aby utworzyć nowy typ przebiegu: [ ![ ] (passkit-images/passid.png "utworzyć nowy typ — dostęp próbny")](passkit-images/passid.png)

2.   Podaj **opis** (nazwa) i **identyfikator** (unikatowy ciąg) dla przebiegu. Należy pamiętać, że wszystkie identyfikatory typu przekazać musi rozpoczynać się od ciągu `pass.` w tym przykładzie używamy `pass.com.xamarin.coupon.banana` : [ ![ ] (passkit-images/register.png "Podaj opis i identyfikator")](passkit-images/register.png)


3.   Potwierdź identyfikator przekazywane przez naciśnięcie przycisku **zarejestrować** przycisku.


<a name="generate" />

#### <a name="generate-a-certificate"></a>Wygeneruj certyfikat

Aby utworzyć nowy certyfikat dla tego Identyfikatora przekazać typ, wykonaj następujące czynności:

1.  Wybierz nowo utworzony identyfikator przekazywania na liście, a następnie kliknij przycisk **Edytuj** : [ ![ ] (passkit-images/pass-done.png "wybierz z listy nowy identyfikator przekazywania")](passkit-images/pass-done.png)

    Następnie wybierz opcję **Utwórz certyfikat...** :

    [ ![](passkit-images/cert-dist.png "Wybierz opcję utworzenia certyfikatu")](passkit-images/cert-dist.png)


2.  Wykonaj kroki, aby utworzyć certyfikat podpisywania żądania obsługi.
  
3. Naciśnij klawisz **Kontynuuj** znajdujący się w portalu dla deweloperów i przekazać plik CSR, aby wygenerować certyfikat.

4. Pobierz certyfikat, a następnie kliknij dwukrotnie w celu zainstalowania go w sieci łańcucha kluczy.


Teraz, certyfikatu został utworzony dla tego Identyfikatora przekazać typ, w następnej sekcji opisano sposób tworzenia przekazywanym ręcznie.

Aby uzyskać więcej informacji na inicjowania obsługi administracyjnej dla portfela, zapoznaj się [Praca z funkcjami](~/ios/deploy-test/provisioning/capabilities/wallet-capabilities.md) przewodnik.

 <a name="Create_a_Pass_Manually" />

### <a name="create-a-pass-manually"></a>Ręczne tworzenie — dostęp próbny

Teraz, utworzyliśmy przekazać typ, można ręcznie spreparować przebieg do testowania w symulatorze lub na urządzeniu. Kroki umożliwiające utworzenie przekazywanym są:

-  Utwórz katalog zawierający pliki przebiegu.
-  Utwórz plik pass.json, który zawiera wszystkie wymagane dane.
-  Obejmują obrazy w folderze (jeśli jest to wymagane).
-  Obliczanie wartości skrótu SHA1 dla każdego pliku w folderze i zapisać manifest.json.
-  Manifest.json Zaloguj się przy użyciu pliku pobranego certyfikatu .p12.
-  ZIP zawartość katalogu i zmiana nazwy z rozszerzeniem .pkpass.


Brak niektórych plików źródłowych w przykładowym kodzie tego artykułu, który może służyć do generowania przebiegu. Użyj plików w `CouponBanana.raw` katalogu CreateAPassManually katalogu. Występują następujące pliki:

 [ ![](passkit-images/image18.png "Te pliki są obecne")](passkit-images/image18.png)

Otwórz pass.json i Edycja kodu JSON. Należy zaktualizować co najmniej `passTypeIdentifier` i `teamIdentifer` odpowiadające konta dewelopera firmy Apple.

```csharp
"passTypeIdentifier" : "pass.com.xamarin.coupon.banana",
"teamIdentifier" : "?????????",
```

Następnie należy obliczyć wartości skrótu dla każdego pliku i utworzyć `manifest.json` pliku. Ekran będzie wyglądać następująco po wykonaniu tych czynności:

```csharp
{
  "icon@2x.png" : "30806547dcc6ee084a90210e2dc042d5d7d92a41",
  "icon.png" : "87e9ffb203beb2cce5de76113f8e9503aeab6ecc",
  "pass.json" : "c83cd1441c17ecc6c5911bae530d54500f57d9eb",
  "logo.png" : "b3cd8a488b0674ef4e7d941d5edbb4b5b0e6823f",
  "logo@2x.png" : "3ccd214765507f9eab7244acc54cc4ac733baf87"
}
```

Następnie podpis musi zostać wygenerowany dla tego pliku, używa certyfikatu, który został wygenerowany dla tego identyfikatora. Typ przebiegu (plik .p12)

 <a name="Signing_On_a_Mac" />


#### <a name="signing-on-a-mac"></a>Podpisywanie na komputerze Mac

Pobierz **portfela inicjatora pomocy technicznej materiały** z [pliki do pobrania firmy Apple](https://developer.apple.com/downloads/index.action?name=Passbook) lokacji. Użyj `signpass` narzędzia, aby przekształcić przekazywanym (spowoduje to również obliczenie SHA1 skróty i ZIP dane wyjściowe do pliku .pkpass) w folderze.

 <a name="Signing_On_a_PC" />


#### <a name="signing-on-a-pc"></a>Zalogować się na komputerze

W przykładzie kod w tym artykule jest to projekt o nazwie `signpassnet` systemem w .NET w systemie Windows. Próbuje naśladować narzędzia firmy Apple, ale zawiera mniej kodu walidacji.

 <a name="Testing" />


#### <a name="testing"></a>Testowanie

Gdyby Sprawdź dane wyjściowe z tych narzędzi (przez ustawienie Nazwa pliku zip, a następnie otwierając), zobaczysz następujące pliki (należy pamiętać, dodanie `manifest.json` i `signature` plików):

 [ ![](passkit-images/image19.png "Badanie dane wyjściowe z tych narzędzi")](passkit-images/image19.png)

Po podpisany ZIPped i zmienić nazwy pliku (np.) Aby `BananaCoupon.pkpass`) przeciągnij go do symulatora do testowania, lub Wyślij wiadomość e-mail do siebie, aby pobrać rzeczywistego urządzenia. Powinien zostać wyświetlony ekran, aby **Dodaj** przebiegu w następujący sposób:

 [ ![](passkit-images/image20.png "Dodaj ekran — dostęp próbny")](passkit-images/image20.png)

Zwykle tego procesu będzie można zautomatyzować na serwerze, jednak ręczne tworzenie przebieg może być dostępny dla małych firm, które tworzysz tylko kupony, które nie wymagają obsługi serwera zaplecza.

 <a name="Wallet" />

## <a name="wallet"></a>Portfela

Portfela jest centralna ekosystemu przekazać zestawu. Ten zrzut ekranu przedstawia pusty portfela i wyglądu listy przebieg i przekazuje poszczególnych:

 [ ![](passkit-images/image21.png "Ten zrzut ekranu przedstawia pusty portfela i wyglądu listy przebieg i przekazuje poszczególnych")](passkit-images/image21.png)

Funkcje portfela:

-  Jest to miejsce tylko, który przekazuje są renderowane z ich kod kreskowy skanowania.
-  Użytkownik może zmienić ustawień aktualizacji. Włączenie powiadomień wypychanych może wyzwolić aktualizacje danych w przebiegu.
-  Użytkownika można włączyć lub wyłączyć integrację ekranu blokady. Włączenie to umożliwia przebiegu automatycznie pojawiać się w ich ekran blokady, na podstawie odpowiednich czas i lokalizację danych osadzonych w przebiegu.
-  Po stronie odwrotnej przebiegu obsługuje ściągania do odświeżania, jeśli podano server-Adres URL sieci web w formacie JSON przekazać.
-  Pomocnik aplikacji można otworzyć (lub pobrać) Jeśli identyfikator aplikacji jest podany w formacie JSON przekazywania.
-  Przekazuje mogą zostać usunięte (z cute animacji strzępienia).


 <a name="Getting_Passes_into_Wallet" />

## <a name="adding-passes-into-wallet"></a>Dodawanie przekazuje do portfela

Przekazuje mogą być dodawane do portfela w następujący sposób:

* **Kanał aplikacji** — te nie manipulowania przekazuje bezpośrednio, po prostu ładować pliki przebieg i zaoferowania użytkownikowi możliwość dodawania ich do portfela. 

* **Pomocniczy aplikacji** — te są zapisywane przez dostawców dystrybucji przekazuje i oferowanie dodatkowych funkcji przeglądania lub je edytować. Aplikacji platformy Xamarin.iOS mają pełny dostęp do interfejsu API zestawu przekazać do tworzenia i modyfikowania przebiegów. Następnie można dodać przekazuje portfela przy użyciu `PKAddPassesViewController`. Ten proces został opisany bardziej szczegółowo w **aplikacji Pomocnika** sekcji tego dokumentu.

### <a name="conduit-applications"></a>Kanał aplikacji

Kanał aplikacje to aplikacje pośredniego, może odbierać przebiegów w imieniu użytkownika, które powinny być zaprogramowane do rozpoznania content-type i zapewniać funkcje do dodania do portfela. Kanał aplikacji należą:

-   **Poczta** — rozpoznaje załączników przebiegu.
-   **Safari** — rozpoznaje przekazać Content-Type, po kliknięciu łącza przekazać adres URL.
-   **Inne niestandardowe aplikacje** — dowolną aplikację, odbierania załączników lub otworzyć łącza (klientów mediów społecznościowych, czytelnicy poczty itp.).


Ten zrzut ekranu pokazuje, jak **poczty** w systemie iOS 6 rozpoznaje załącznika przebieg i (jeśli dotknięciu) oferuje **Dodaj** go do portfela.

 [ ![](passkit-images/image22.png "Ten zrzut ekranu pokazuje, jak poczty w systemie iOS 6 rozpoznaje załącznika — dostęp próbny")](passkit-images/image22.png)

 [ ![](passkit-images/image23.png "Ten zrzut ekranu pokazuje, jak poczta oferuje Dodaj załącznik przebiegu portfela")](passkit-images/image23.png)

Jeśli tworzysz aplikację, która może być kanał dla przebiegów mogą być rozpoznawane przez:

-  **Rozszerzenie pliku** -.pkpass
-  **Typ MIME** -application/vnd.apple.pkpass
-  **UTI** — com.apple.pkpass


Podstawowe operacje aplikacji kanał jest pobrać plik — dostęp próbny i wywołać przekazać zestawu `PKAddPassesViewController` umożliwiają opcję, aby dodać przebiegu do ich portfela w trybie użytkownika. Implementacja tego kontrolera widoku zostało opisane w następnej sekcji na **aplikacji Pomocnika**.

Kanał aplikacji jest konieczne udostępniane dla określonego Identyfikatora typu przekazać w taki sam sposób czy pomocnika aplikacji.

## <a name="companion-applications"></a>Pomocnik aplikacji

Aplikacja pomocnika zapewnia dodatkowe funkcje do pracy z przekazuje, w tym tworzenie przekazywanym, informacje związane z przebiegu aktualizacji i w przeciwnym razie zarządzania przekazuje skojarzone z aplikacją.

Pomocnik aplikacje nie powinny podejmować mają zostać zduplikowane funkcje portfela. Nie są one przeznaczone do wyświetlenia przekazuje do skanowania.

Ta pozostałej części tej sekcji opisano sposób tworzenia podstawowa pomocnika aplikacja wchodzącą w interakcje z przekazać zestawu.

### <a name="provisioning"></a>Inicjowanie obsługi administracyjnej

Ponieważ portfela to technologia magazynu, aplikacja powinna być przygotowana oddzielnie i nie można użyć profilu inicjowania obsługi administracyjnej Team lub identyfikator aplikacji symboli wieloznacznych. Zapoznaj się [Praca z funkcjami](~/ios/deploy-test/provisioning/capabilities/wallet-capabilities.md) przewodniku, aby utworzyć unikatowy identyfikator aplikacji i profilu inicjowania obsługi administracyjnej dla aplikacji portfela.

### <a name="entitlements"></a>Uprawnienia

**Entitlements.plist** pliku powinna zawierać wszystkie ostatnie projektu platformy Xamarin.iOS. Aby dodać nowy plik Entitlements.plist, postępuj zgodnie z instrukcjami [Praca z uprawnieniami](~/ios/deploy-test/provisioning/entitlements.md) przewodnik.

Aby ustawić uprawnienia, wykonaj następujące czynności:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Kliknij dwukrotnie **Entitlements.plist** pliku w konsoli do rozwiązania, aby otworzyć Edytor Entitlements.plist:

![](passkit-images/image31.png "Edytor Entitlements.plst")

W sekcji portfela, wybierz **włączyć portfela** opcji

![](passkit-images/image32.png "Włącz uprawnień portfela")


Jest domyślną opcją dla aplikacji do zezwalania na przekazywanie wszystkich typów. Jest jednak można ograniczyć aplikacji i dopuszcza tylko podzbiór typów przebiegu zespołu. Aby włączyć ten wybierz **Zezwalaj podzbiór zespołu przekazywania typów** , a następnie wprowadź identyfikator typu pass podzbioru, który chcesz zezwolić.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Kliknij dwukrotnie **Entitlements.plist** plik, aby otworzyć plik XML źródła.

Aby dodać uprawnienia portfela, należy ustawić **właściwości** do `Passbook Identifiers` na liście rozwijanej, która automatycznie ustawi **typu** `Array`. Następnie należy określić ciąg **wartość** do `$(TeamIdentifierPrefix)*`:

![](passkit-images/image33.png "Włącz uprawnień portfela")

To spowoduje włączenie aplikacji zezwala na przekazywanie wszystkich typów. Ograniczenie aplikacji i dopuszcza tylko podzbiór typów przebiegu zespołu, ustaw wartość ciągu:

`$(TeamIdentifierPrefix)pass.$(CFBundleIdentifier)`

Gdzie `pass.$(CFBundleIdentifier)` jest przekazać identyfikator, który został utworzony [powyżej](~/ios/platform/passkit.md)

-----

### <a name="debugging"></a>Debugowanie

Jeśli masz problemy z wdrożenia aplikacji, sprawdź, czy używasz właściwego **profilu inicjowania obsługi administracyjnej** i `Entitlements.plist` został wybrany jako **uprawnień niestandardowych** pliku w **iPhone podpisywania pakietu** opcje.

Jeśli ten błąd występuje w przypadku wdrażania:

```csharp
Installation failed: Your code signing/provisioning profiles are not correctly configured (error: 0xe8008016)
```

następnie przy użyciu `pass-type-identifiers` uprawnień do tablicy jest niepoprawny (lub nie odpowiada **profilu inicjowania obsługi administracyjnej**). Sprawdź poprawność identyfikatorów przekazać typ i identyfikator zespołu.

 <a name="Classes" />

## <a name="classes"></a>Klasy

Następujące klasy przekazać zestawu są dostępne dla aplikacji, które mają dostęp do przebiegów:

-  **PKPass** — instancję przebiegu.
-  **PKPassLibrary** — zapewnia przekazuje na urządzeniu dostępu do interfejsu API.
-  **PKAddPassesViewController** — używany do wyświetlania hasło dla użytkownika zapisać w ich portfela.
-  **PKAddPassesViewControllerDelegate** — deweloperów platformy Xamarin.iOS


## <a name="example"></a>Przykład

Odwołuje się do projektu PassLibrary w przykładowym kodzie w tym artykule. Przedstawia on następujące typowych funkcji, które będą wymagane w aplikacji pomocnika portfela:

### <a name="check-that-wallet-is-available"></a>Sprawdź, czy portfela jest dostępne

Portfela nie jest dostępny na urządzeniu iPad, więc aplikacji należy sprawdzić przed podjęciem próby wykonania dostęp do funkcji przekazywania zestawu.

```csharp
if (PKPassLibrary.IsAvailable) {
    // create an instance and do stuff...
}
```

### <a name="creating-a-pass-library-instance"></a>Tworzenie wystąpienia biblioteki — dostęp próbny

Biblioteka przekazać zestawu nie jest klasą pojedynczą, utworzyć i przechowywać i wystąpienia interfejsu API zestawu przekazać dostępu do aplikacji.

```csharp
if (PKPassLibrary.IsAvailable) {
    library = new PKPassLibrary ();
    // do stuff...
}
```

### <a name="get-a-list-of-passes"></a>Pobierz listę przebiegów

Aplikacje można zażądać listy przebiegów z biblioteki. Ta lista jest przefiltrowana przez przekazać Kit automatycznie, dzięki czemu może zobaczyć tylko przebiegi, które zostały utworzone za pomocą Identyfikatora zespołu i które są wymienione w Twoje uprawnienia do.

```csharp
var passes = library.GetPasses ();  // returns PKPass[]
```

Należy pamiętać, że symulator nie zastosować filtr do listy przebiegów zwrócone, ta metoda zawsze należy badać w rzeczywistych urządzeń. Tej listy mogą być wyświetlane w UITableView, wygląd aplikacji przykładowej podobny do tego, po dodaniu dwóch kupony:

 [ ![](passkit-images/image29.png "Wygląd aplikacji przykładowej, takich jak to po dodaniu dwóch kupony")](passkit-images/image29.png)


### <a name="displaying-passes"></a>Wyświetlanie przebiegów

Ograniczony zestaw informacji jest dostępna do renderowania przebiegów w aplikacjach pomocnika.

Wybierz z tego zestawu właściwości standardowych do wyświetlania listy przebiegów, tak jak w przypadku przykładowy kod.

```csharp
string passInfo =
                "Desc:" + pass.LocalizedDescription
                + "\nOrg:" + pass.OrganizationName
                + "\nID:" + pass.PassTypeIdentifier
                + "\nDate:" + pass.RelevantDate
                + "\nWSUrl:" + pass.WebServiceUrl
                + "\n#" + pass.SerialNumber
                + "\nPassUrl:" + pass.PassUrl;
```

Ten ciąg jest wyświetlany jako alertu w przykładzie:

 [ ![](passkit-images/image30.png "Alert kupon wybrane w próbce.")](passkit-images/image30.png)

Można również użyć `LocalizedValueForFieldKey()` metody do pobierania danych z polami w przebiegach zaprojektowany (od momentu będzie wiadomo, które pola powinien znajdować się). Przykład kodu nie pokazuj tego.

### <a name="loading-a-pass-from-a-file"></a>Ładowanie przekazywanym z pliku

Ponieważ przekazywanym można dodać tylko portfela za zgodą użytkownika, umożliwiających im zdecydować przedstawia kontrolera widoku. Ten kod jest używany w **Dodaj** przycisku na przykład, aby załadować wbudowanych przebiegu, osadzonego w aplikacji (użytkownik powinien Zamień ten element, który jest zarejestrowany):

```csharp
NSData nsdata;
using ( FileStream oStream = File.Open (newFilePath, FileMode.Open ) ) {
        nsdata = NSData.FromStream ( oStream );
}
var err = new NSError(new NSString("42"), -42);
var newPass = new PKPass(nsdata,out err);
var pkapvc = new PKAddPassesViewController(newPass);
NavigationController.PresentModalViewController (pkapvc, true);
```

Zostanie wyświetlony przebiegu **Dodaj** i **anulować** opcje:

 [ ![](passkit-images/image20.png "Przebieg wyświetlone opcje Dodaj i Anuluj")](passkit-images/image20.png)

### <a name="replace-an-existing-pass"></a>Zastąp istniejące hasło

Zastępowanie istniejących przebiegu nie wymaga uprawnień użytkownika, jednak zakończy się niepowodzeniem, jeśli przebiegu jeszcze nie istnieje.

```csharp
if (library.Contains (newPass)) {
     library.Replace (newPass);
}
```

### <a name="editing-a-pass"></a>Edytowanie — dostęp próbny

PKPass nie jest modyfikowalna, dlatego nie można zaktualizować obiektów przebiegu w kodzie. Aby zmienić dane w przebiegu, aplikacja musi mieć dostęp do serwera sieci web, które można rejestrować przekazuje i wygeneruj nowy plik — dostęp próbny ze zaktualizowanymi wartościami, które można pobrać aplikacji.

Przebiegu tworzenia pliku muszą być konfigurowane na serwerze, ponieważ przekazuje muszą być podpisane przy użyciu certyfikatu, który musi znajdować się prywatne i bezpieczne.

Po wygenerowaniu zaktualizowany plik — dostęp próbny, użyj `Replace` metody zastąpienie starych danych na urządzeniu.

### <a name="display-a-pass-for-scanning"></a>Wyświetl przekazywanym skanowania

Jak wcześniej wspomniano, tylko portfela można wyświetlić przekazywanym skanowania. Przebieg można wyświetlić przy użyciu `OpenUrl` metody, jak pokazano:

 `UIApplication.SharedApplication.OpenUrl (p.PassUrl);`

### <a name="receiving-notifications-of-changes"></a>Odbieranie powiadomień o zmianach

Aplikacje można nasłuchiwać zmian wprowadzonych do biblioteki przekazywania przy użyciu `PKPassLibraryDidChangeNotification`. Zmiany może być spowodowana przez powiadomienia wyzwolenie w tle, dlatego jest dobrym rozwiązaniem do nasłuchiwania w aplikacji.

```csharp
noteCenter = NSNotificationCenter.DefaultCenter.AddObserver (PKPassLibrary.DidChangeNotification, (not) => {
    BeginInvokeOnMainThread (() => {
        new UIAlertView("Pass Library Changed", "Notification Received", null, "OK", null).Show();
        // refresh the list
        var passlist = library.GetPasses ();
        table.Source = new TableSource (passlist, library);
        table.ReloadData ();
    });
}, library);  // IMPORTANT: must pass the library in
```

Należy przekazać wystąpienia biblioteki podczas rejestrowania powiadomienia, ponieważ PKPassLibrary nie jest klasą pojedynczą.

## <a name="server-processing"></a>Przetwarzanie na serwerze

Szczegółowe omówienie tworzenia aplikacji serwera do obsługi zestawu przekazać wykracza poza zakres tego artykułu wstępne.

Kodu platformy .NET w *signpassnet* próbki można jako podstawa dla metody po stronie serwera, która może powodować generowanie przebiegów.

Widok [WWDC wideo: wprowadzenie do aplikacji Passbook, część 2](https://developer.apple.com/videos/wwdc/2012/?include=309#309) od minut 27: 00, aby uzyskać więcej informacji.

### <a name="other-resources"></a>Inne zasoby

Zobacz [dotnet passbook](https://github.com/tomasmcguinness/dotnet-passbook) Otwórz kodu po stronie serwera źródłowego C#.

## <a name="push-notifications"></a>Powiadomienia wypychane

Szczegółowe omówienie przebiegów aktualizacji za pomocą powiadomień wypychanych wykracza poza zakres tego artykułu wstępne.

Czy trzeba zaimplementować interfejsu API REST przypominającej zdefiniowane przez firmę Apple do odpowiadania na żądania sieci web z portfela, gdy wymagane są aktualizacje. Kodu platformy .NET w *signpassnet* próbki można służący jako podstawy do generowania nowe przebiegi wyniku te żądania.

Widok [WWDC wideo: wprowadzenie do aplikacji Passbook, część 2](https://developer.apple.com/videos/wwdc/2012/?include=309#309) od minut 27: 00, aby uzyskać więcej informacji.

## <a name="summary"></a>Podsumowanie

W tym artykule wprowadzono przekazać zestawu, opisano niektóre przyczyny, dlaczego jest przydatne i opisano poszczególne elementy, które muszą zostać zaimplementowane dla pełnego rozwiązania przekazać zestawu. Go opisane kroki wymagane do skonfigurowania konta dewelopera firmy Apple do utworzenia przebiegi, proces, aby przekazywanym ręcznie, a także sposób uzyskać dostęp do interfejsów API zestawu przekazać z aplikacji platformy Xamarin.iOS.

## <a name="related-links"></a>Linki pokrewne

- [CreateAPassManually (przykład)](https://developer.xamarin.com/samples/PassKit/)
- [Przykładowe PassKit](https://developer.xamarin.com/samples/monotouch/PassKit/)
- [Wprowadzenie do systemu iOS 6](~/ios/platform/introduction-to-ios6/index.md)
- [Przewodnik programowania w języku Passbook](https://developer.apple.com/library/prerelease/ios/#documentation/UserExperience/Conceptual/PassKit_PG/Chapters/Introduction.html)
- [Używanie aplikacji Passbook dla deweloperów](https://developer.apple.com/passbook/)
- [Informacje o plikach — dostęp próbny](https://developer.apple.com/library/prerelease/ios/#documentation/UserExperience/Reference/PassKit_Bundle/Chapters/Introduction.html)
- [Przekaż odwołanie do zestawu platformy](https://developer.apple.com/library/prerelease/ios/#documentation/UserExperience/Reference/PassKit_Framework/_index.html)
- [Przekaż odwołanie do zestawu platformy](https://developer.apple.com/library/prerelease/ios/#documentation/UserExperience/Reference/PassKit_Framework/_index.html)
- [Używanie aplikacji Passbook odwołania do usługi sieci Web](https://developer.apple.com/library/prerelease/ios/#documentation/PassKit/Reference/PassKit_WebService/WebService.html)
