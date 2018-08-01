---
title: Zmiany StoreKit w systemie iOS 6
description: 'iOS 6 wprowadza dwie zmiany do interfejsu API zestawu magazynu: możliwość wyświetlania iTunes (i sklepu z aplikacjami/iBookstore) opcji, gdzie Apple będzie hostować pliki do pobrania zakupu produktów z aplikacji i nowych w aplikacji. Ten dokument wyjaśniono, jak wdrożyć te funkcje z platformy Xamarin.iOS.'
ms.prod: xamarin
ms.assetid: 253D37D7-44C7-D012-3641-E15DC41C2699
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: ff717d1e4ea7da947d5534f1ce790b58d84fdfd4
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787695"
---
# <a name="changes-to-storekit-in-ios-6"></a>Zmiany StoreKit w systemie iOS 6

_iOS 6 wprowadza dwie zmiany do interfejsu API zestawu magazynu: możliwość wyświetlania iTunes (i sklepu z aplikacjami/iBookstore) opcji, gdzie Apple będzie hostować pliki do pobrania zakupu produktów z aplikacji i nowych w aplikacji. Ten dokument wyjaśniono, jak wdrożyć te funkcje z platformy Xamarin.iOS._

Główne zmiany w magazynie zestawu w iOS6 są te dwie nowe funkcje:

-   **Wyświetlanie zawartości w aplikacji i zakup** — użytkowników można kupować i pobierania aplikacji, muzyka, książki i innych iTunes zawartości bez opuszczania aplikacji. Można również połączyć aplikacji do podwyższania poziomu zakupu lub po prostu zachęca przeglądy i klasyfikacje. 
-   **Zawartości hostowanej zakupu w aplikacji** — Apple będzie przechowywać i dostarczanie zawartości skojarzone z produktami zakupu w aplikacji, która eliminuje potrzebę oddzielny serwer do obsługi plików, automatycznie obsługuje pobieranie w tle i umożliwia Pisanie kodu mniej. 


Zaleca się, że ten dokument można odczytać w połączeniu z istniejących Xamarin.iOS [zakupu w aplikacji](~/ios/platform/in-app-purchasing/index.md) dokumentacji.

## <a name="requirements"></a>Wymagania

Funkcje zestawu magazynu omówionych w tym dokumencie wymagają iOS 6 lub Xcode 4.5, wraz z Xamarin.iOS 6.0.


## <a name="in-app-content-display--purchasing"></a>Wyświetlanie zawartości w aplikacji i zakupy

Nowa funkcja zakupu w aplikacji w systemie iOS umożliwia użytkownikom wyświetlanie informacji o produkcie i zakupić lub pobrać produkt za pomocą aplikacji.
Wcześniej aplikacji musi wywołać iTunes App Store i iBookstore, co spowoduje pozostawienie oryginalnej aplikacji użytkownika. Ta nowa funkcja automatycznie zwraca użytkownika do aplikacji, gdy są one wykonywane.

 [![](changes-to-storekit-images/image1.png "Po zakupie, automatycznego powrotu do aplikacji")](changes-to-storekit-images/image1.png#lightbox)

Istnieje wiele scenariuszy, w którym może to być przydatne, w tym (między innymi):

-   **Wspieranie użytkownicy będą oceniać aplikację** — możesz otworzyć stronę Sklepu z aplikacjami, dzięki czemu użytkownik może ocenić i przejrzyj aplikacji bez opuszczania go. 
-   **Wspieranie między aplikacjami** — Zezwalaj użytkownikowi na zobaczenie inne aplikacje, które można opublikować, z możliwością zakupu/Pobierz natychmiast. 
-   **Ułatwienia dla użytkowników Znajdź i Pobierz zawartość** — pomaganie użytkownikom kupić zawartość, która umożliwia znalezienie aplikacji, zarządza lub agreguje (np.) powiązane utworów muzycznych aplikacji można podać listę odtwarzania utworów i Zezwól każdego utworu zakup z poziomu aplikacji). 


Raz `SKStoreProductViewController` został wyświetlony użytkownik może interakcyjnie informacji o produkcie tak, jakby były w iTunes App Store i iBookstore. Możliwości obejmują:

-  Wyświetlanie zrzutów ekranu (dla aplikacji)
-  Próbkowanie utworów muzycznych lub wideo (muzyki, programy telewizyjne i filmy),
-  Przegląda odczytu (i zapisu),
-  Zakup & pobieranie, które odbywa się całkowicie w obrębie kontrolera widoku i zestaw magazynu. Aplikacja nie trzeba podać jakiegokolwiek dodatkowego kodu, aby to zrobić. 


Należy pamiętać, że niektóre opcje w `SKStoreProductViewController` będą nadal życie użytkownikom pozostaw aplikacji i Otwórz aplikację odpowiedniego sklepu, takie jak kliknięcie **pokrewnych produktów** aplikacji lub **Obsługa** łącza.

### <a name="skstoreproductviewcontroller"></a>SKStoreProductViewController

Pokaż produktu w dowolnej aplikacji interfejsu API programu jest bardzo prosta: wymaga tylko utworzyć i wyświetlić `SKStoreProductViewController`. Wykonaj następujące kroki, aby utworzyć i Pokaż produktu:

1.  Utwórz `StoreProductParameters` obiektu do przekazania parametrów do kontrolera widoku, łącznie z `productId` w konstruktorze. 
1.  Utwórz wystąpienie `SKProductViewController` . Przypisz go do poziomu pola klasy. 
1.  Przypisz program obsługi do kontrolera widoku `Finished` zdarzenie, które należy odrzucić kontroler widoku. To zdarzenie jest wywoływane, gdy użytkownik naciśnie anulować; lub, w przeciwnym razie Kończenie znajdujących się w transakcji wewnątrz kontroler widoku. 
1.  Wywołanie `LoadProduct` metody przekazując `StoreProductParameters` i obsługi uzupełniania. Programu obsługi zakończenia powinien Sprawdź żądanie produktu został pomyślnie, a jeśli tak, obecna `SKProductViewController` trybie modalnym. Obsługa błędów odpowiednie należy dodać w przypadku, gdy nie można pobrać produktu. 

### <a name="example"></a>Przykład

*ProductView* projektu w *StoreKit* przykładowy kod w tym artykule implementuje `Buy` metodę, która akceptuje jakimkolwiek produktem firmy Apple ID i wyświetla `SKStoreProductViewController`. Poniższy kod wyświetla informacje o produkcie dla dowolnego podanego Identyfikatora firmy Apple:

```csharp
void Buy (int productId)
{
    var spp = new StoreProductParameters(productId);
    var productViewController = new SKStoreProductViewController ();
    // must set the Finished handler before displaying the view controller
    productViewController.Finished += (sender, err) => {
        // Apple's docs says to use this method to close the view controller
        this.DismissModalViewControllerAnimated (true);
    };
    productViewController.LoadProduct (spp, (ok, err) => { // ASYNC !!!
        if (ok) {
            PresentModalViewController (productViewController, true);
        } else {
            Console.WriteLine (" failed ");
            if (err != null)
                Console.WriteLine (" with error " + err);
        }
    });
}
```

Aplikacja wygląda, to podczas uruchamiania — pobierania ani zakupu występuje w całości w `SKStoreProductViewController`:

 [![](changes-to-storekit-images/image2.png "Aplikacja wygląda, to podczas uruchamiania")](changes-to-storekit-images/image2.png#lightbox)

### <a name="supporting-older-operating-systems"></a>Obsługa starszych systemów operacyjnych

Przykładowa aplikacja zawiera kod, który pokazuje, jak otworzyć Sklepu z aplikacjami, iTunes lub iBookstore we wcześniejszych wersjach systemu IOS. Użyj `OpenUrl` metodę, aby otworzyć poprawnie przygotowany **itunes.com** adresu URL.

Można zaimplementować sprawdzenie wersji, aby określić, które kodu do uruchomienia, jak to:

```csharp
if (UIDevice.CurrentDevice.CheckSystemVersion (6,0)) {
    // do iOS6+ stuff, using SKStoreProductViewController as shown above
} else {
    // don't do stuff requiring iOS 6.0, use the old syntax 
    // (which will take the user out of your app)
    var nsurl = new NSUrl("http://itunes.apple.com/us/app/angry-birds/id343200656?mt=8");
    UIApplication.SharedApplication.OpenUrl (nsurl);
}
```

### <a name="errors"></a>błędy

Następujący błąd wystąpi, jeśli używasz identyfikator Apple ID jest nieprawidłowy, ponieważ może być mylące oznacza problem sieci lub uwierzytelniania pewnego rodzaju.

 `Error Domain=SKErrorDomain Code=5 "Cannot connect to iTunes Store"`

### <a name="reading-objective-c-documentation"></a>Odczytywanie dokumentację języka Objective C

Deweloperzy odczytywania informacje magazynie zestawu w portalu dla deweloperów firmy Apple zobaczą protokół — [SKStoreProductViewControllerDelegate](https://developer.apple.com/library/prerelease/ios/#documentation/StoreKit/Reference/SKITunesProductViewControllerDelegate_ProtocolRef/Reference/Reference.html) — opisanych w odniesieniu do tej nowej funkcji. Protokół delegat ma tylko jedną metodę — productViewControllerDidFinish —, który został udostępniony jako `Finished` zdarzenia w `SKStoreProductViewController` w Xamarin.iOS.


## <a name="determining-apple-ids"></a>Określanie identyfikatorów firmy Apple

Identyfikator Apple ID, wymagane przez `SKStoreProductViewController` jest *numer* (nie należy mylić z identyfikatorów pakietu takich jak "com.xamarin.mwc2012"). Istnieje kilka różnych sposobów, które można znaleźć Identyfikatora firmy Apple dla produktów, które chcesz wyświetlić, wymienionych poniżej:

### <a name="itunesconnect"></a>iTunesConnect

Dla aplikacji, które można opublikować, jest łatwe do odnalezienia **identyfikator Apple ID** w iTunes Connect:

 [![](changes-to-storekit-images/image3.png "Znajdowanie identyfikatora Apple ID w iTunes Connect")](changes-to-storekit-images/image3.png#lightbox)

 <a name="Search_API" />


### <a name="search-api"></a>Interfejs API wyszukiwania

Apple udostępnia interfejs API dynamicznej wyszukiwania do badania wszystkich produktów w sklepie z aplikacjami, iTunes i iBookstore. Informacje o sposobie dostępu wyszukiwania interfejsu API można znaleźć w [zasobów partnerskiego firmy Apple](http://www.apple.com/itunes/affiliates/resources/documentation/itunes-store-web-service-search-api.html), mimo że interfejsu API jest udostępniane innym osobom (nie tylko zarejestrowanych podmioty stowarzyszone). Aby dowiedzieć się, można przeanalizować wynikowy JSON `trackId` oznacza to identyfikator firmy Apple do używania z `SKStoreProductViewController`.

Wyniki zawiera również inne metadane, w tym adresy URL kompozycji, które mogą służyć do renderowania produktu w aplikacji i wyświetlane informacje.

Oto kilka przykładów:

-   **Aplikacja iBooks*- [http://itunes.apple.com/search?term=ibooks&amp; jednostki = oprogramowania&amp;kraju = us](http://itunes.apple.com/search?term=ibooks&amp;entity=software&amp;country=us) 
-   **Kropka i iBook kangura*- [http://itunes.apple.com/search?term=dot+and+the+kangaroo&amp; jednostki = książki elektronicznej&amp;kraju = us](http://itunes.apple.com/search?term=dot+and+the+kangaroo&amp;entity=ebook&amp;country=us) 


### <a name="enterprise-partner-feed"></a>Organizacji partnera źródła

Apple zapewnia zatwierdzonych partnerom zrzutu pełnych danych swoich produktów w formularzu do pobrania plików prostych gotowe do bazy danych. Jeśli masz prawo do uzyskiwania dostępu do [źródła partnera Enterprise](http://www.apple.com/itunes/affiliates/resources/documentation/itunes-enterprise-partner-feed.html) , a następnie identyfikator Apple ID dla każdego produktu znajduje się w tym zestawie danych.

Należy pamiętać, że w przypadku wielu użytkowników z organizacji partnera źródła danych są elementami członkowskimi [programu partnerskiego](http://www.apple.com/itunes/affiliates) umożliwiająca prowizji do nabytych sprzedaży produktu. `SKStoreProductViewController` nie obsługuje identyfikatorów powiązana (w czasie zapisywania; to może być dodane przez firmę Apple w przyszłości).

### <a name="direct-product-links"></a>Linki bezpośrednie produktu

Identyfikator Apple ID produktu można wywnioskować na podstawie jego iTunes łącze URL podglądu.
W dowolnym iTunes łącze produktu (dla aplikacji, muzyki lub książek) Znajdź część URL zaczynającym się od `id` i użyj liczba, która jest zgodna.

Na przykład jest bezpośredniego łącza do iBooks

```csharp
http://itunes.apple.com/us/app/ibooks/id364709193?mt=8
```

Identyfikator firmy Apple **364709193**. Podobnie w przypadku aplikacji MWC2012 bezpośredniego łącza jest

```csharp
http://itunes.apple.com/us/app/mwc-2012-unofficial/id496963922?mt=8
```

Identyfikator firmy Apple **496963922**.

## <a name="in-app-purchase-hosted-content"></a>Funkcja zakupu w aplikacji hostowaną zawartość

Jeśli zakupy w aplikacji składają się z do pobrania zawartością (taką jak książki lub innego nośnika, gier grafikę poziomu i konfiguracji lub innych dużych plików) następnie te pliki używane na serwerze sieci web i aplikacji było dołączyć kod, aby bezpiecznie pobierać je po zakup. W systemie iOS 6 Apple wprowadziła opcją, gdzie będzie hostować pliki na serwerach, usuwając konieczność osobnego serwera. Funkcja jest dostępna tylko dla produktów jednoznacznie składnika (niestandardowe lub subskrypcji). Zalety korzystania z usługi hostingu firmy Apple obejmują:

-  Zapisz koszty hostingu & przepustowości.
-  Prawdopodobnie większą skalowalność niż niezależnie od hosta serwera, którego obecnie korzystasz. 
-  Mniejsza ilość kodu do zapisu, ponieważ nie masz żadnych przetwarzania po stronie serwera kompilacji. 
-  Pobieranie w tle została zaimplementowana dla Ciebie.


Uwaga: testowanie hostowaną zawartość zakupu w aplikacji w systemie iOS Simulator nie jest obsługiwana, więc należy przetestować z rzeczywistego urządzenia.

### <a name="hosted-content-basics"></a>Obsługiwane podstawowe informacje o zawartości

Przed iOS 6, zostały dwa sposoby zapewnienia produktu (opisany bardziej szczegółowo w [zakupu w aplikacji platformy Xamarin w](~/ios/platform/in-app-purchasing/index.md) dokumentacji):

-   **Wbudowane produktów** — funkcje, które są "odblokowane" po zakupie, ale które są wbudowane w aplikacji (albo jako kod lub zasobów osadzonych). Przykłady wbudowanych produktów odblokowane filtry zdjęcie lub ulepszeń w grę. 
-   **Serwer dostarczyć produktów** — po zakupie, aplikacja musi pobrać zawartość z serwera, na którym działa. Ta zawartość jest pobrane podczas zakupu, przechowywane na urządzeniu i następnie renderowane jako część dostarczanie produktu. Przykładami książek, Magazyn problemy lub poziomy gier, które składają się z tła grafikę i plików konfiguracji. 


W systemie iOS firmy Apple 6 oferuje odmianą produktów dostarczonych przez serwer: one będzie obsługiwać pliki zawartości na serwerach. Dzięki temu można łatwiej można je utworzyć produktów dostarczonych przez serwer, ponieważ nie musi działać oddzielny serwer i pobieranie w tle funkcji, która wcześniej była zapisu samodzielnie zawiera zestaw magazynu. Aby skorzystać z hostingu firmy Apple, Włącz hostingu zawartości dla nowych produktów zakupu w aplikacji i ten kod zestawu magazynu, aby wykorzystać go zmodyfikować. Pliki zawartości produktu są następnie utworzony za pomocą środowiska Xcode i przekazane do serwerów firmy Apple w celu przeglądu i wersji.

 [![](changes-to-storekit-images/image4.png "Proces kompilacji i dostarczyć")](changes-to-storekit-images/image4.png#lightbox)

Zapewnienie zakupu w aplikacji przy użyciu sklepu z aplikacjami *z hostowaną zawartość* wymaga następujących instalacji i konfiguracji:

-   **Połącz iTunes** — możesz *musi* zostały podane informacji bankowych i podatek do firmy Apple, więc ich przekazać funduszy zbierane w Twoim imieniu. Następnie można skonfigurować produktów, sprzedaż i konfigurowanie kont użytkowników piaskownicy, aby przetestować zakupu.  *Należy także skonfigurować hostowanej zawartości**dla produktów innych niż niestandardowe, które chcesz udostępnić przez firmę Apple* *.* 
-   **System iOS w portalu inicjowania obsługi** — Tworzenie identyfikatora pakietu i włączanie dostępu do sklepu z aplikacjami dla aplikacji, jak w przypadku dowolnej aplikacji, która obsługuje zakupu w aplikacji. 
-   **Przechowywanie zestawu** — Dodawanie kodu do aplikacji Wyświetlanie produktów, zakupu produktów i przywracanie transakcji.  *W systemie iOS 6 zestawu magazynu będzie również zarządzać, pobierania zawartości produktu, a w tle, z aktualizacji w toku.* 
-   **Kod niestandardowy** — aby śledzić zakupy dokonane przez klientów i podaj produktów lub usług one zakupione. Korzystanie z nowych klas zestawu sklepu iOS 6, takich jak `SKDownload` do pobierania zawartości hostowanej przez firmę Apple. 


W poniższych sekcjach opisano sposób wdrożenia zawartości hostowanej, tworzenie i przekazywanie pakietu zarządzania zakupu i Pobierz proces, za pomocą przykładowy kod dla tego artykułu.


### <a name="sample-code"></a>Przykładowy kod

Przykładowy projekt *HostedNonConsumables* (w StoreKitiOS6.zip) demonstruje sposób tworzenia aplikacji zawartości hostowanej używa. Aplikacja oferuje dwie "książki rozdziałach" do sprzedaży, zawartość, dla której jest obsługiwana na serwerach firmy Apple. Zawartość składa się z plików tekstowych i obrazów, mimo że bardziej złożonej zawartości mogą być używane w rzeczywistej aplikacji.

Aplikacji wygląda tak przed, podczas i po zakupu:

 [![](changes-to-storekit-images/image5.png "Aplikacja wygląda, to przed, podczas i po zakupu")](changes-to-storekit-images/image5.png#lightbox)

Plik tekstowy i obrazu są pobierane i kopiowane do katalogu dokumentów aplikacji. Zobacz [Praca z dokumentacją systemu plików](~/ios/app-fundamentals/file-system.md) uzyskać więcej informacji o różnych katalogach dostępne do przechowywania danych aplikacji.

## <a name="itunes-connect"></a>iTunes Connect

Podczas tworzenia nowych produktów, które będą korzystać z firmy Apple elementu zawartości hosting Pamiętaj o wybraniu **jednoznacznie składnika** typ produktu. Inne typy produktu nie obsługują hosting zawartości. Ponadto nie należy włączać zawartości hosting dla *istniejących* produktów, sprzedaż; Włącz tylko hostingu zawartości dla nowych produktów.

 [![](changes-to-storekit-images/image6.png "Wybierz typ produktu jednoznacznie składnika")](changes-to-storekit-images/image6.png#lightbox)

Wprowadź **identyfikator produktu**. Są to wymagane później podczas tworzenia zawartości dla tego produktu.

 [![](changes-to-storekit-images/image7.png "Wprowadź identyfikator produktu")](changes-to-storekit-images/image7.png#lightbox)

Hosting zawartości znajduje się w sekcji szczegółów. Przed zakupu w aplikacji środowiska produkcyjnego po prostu usuń zaznaczenie pola wyboru "Host zawartości z Apple" Jeśli chcesz anulować (nawet, jeśli zostały przekazane zawartość testową). Jednak hosting zawartości nie można usunąć po zakupu w aplikacji stała się na żywo.

 [![](changes-to-storekit-images/image8.png "Hostowanie zawartości z firmy Apple")](changes-to-storekit-images/image8.png#lightbox)

Po włączeniu hostowanie zawartości, produkt wejdzie **oczekiwanie na przekazanie** stanu i pokazuj tego komunikatu:

 [![](changes-to-storekit-images/image9.png "Produkt zostanie wprowadź oczekiwania dla stanu przekazywania i pokazuj tego komunikatu")](changes-to-storekit-images/image9.png#lightbox)

Zawartość musi zostać utworzone z Xcode i przekazać za pomocą narzędzia archiwum. Instrukcje dotyczące tworzenia pakietów zawartości znajduje się w następnej sekcji **tworzenie. Pliki PKG**.

## <a name="creating-pkg-files"></a>Tworzenie. Pliki PKG

Pliki zawartości, które można przekazać do firmy Apple muszą spełniać następujące ograniczenia:

-  Rozmiar nie może przekraczać 2GB.
-  Nie może zawierać kod wykonywalny (lub łączy symbolicznych, które poza zawartości). 
-  Musi być poprawnie sformatowana (w tym plik .plist) i ma .pkg rozszerzenia pliku. Spowoduje to automatycznie po wykonaniu tych instrukcji za pomocą środowiska Xcode. 


Można dodać wiele różnych plików i typów plików, pod warunkiem, że spełniają te ograniczenia. Zawartość jest Zip przed dostarczeniem do aplikacji i rozpakowane przez zestaw magazynu przed kod uzyskuje dostęp do jej.

Po przekazaniu pakietu zawartości, można zastąpić z zawartością nowsza. Nowa zawartość należy przekazać i do przeglądu/zatwierdzenia za pomocą standardowego procesu. Należy zwiększyć `ContentVersion` w zaktualizowanych pakietów zawartości.

### <a name="xcode-in-app-purchase-content-projects"></a>Projekty zawartości zakupu w aplikacji Xcode

Tworzenie pakietów zawartości dla produktów zakupu w aplikacji obecnie wymaga Xcode. Jest wymagane; kodowania OBJECTIVE-C nie Środowisko Xcode nie może nowy typ projektu dla tych pakietów, który zawiera tylko pliki i listę właściwości.

Nasze przykładowej aplikacji ma rozdziałów książek na sprzedaż — każdy pakiet zawartości rozdział będzie zawierać:

-  plik tekstowy i
-  obraz do reprezentowania działu.


Najpierw wybrać **Plik > Nowy projekt** z menu i wybierając polecenie **zawartości zakupu w aplikacji**:

 [![](changes-to-storekit-images/image10.png "Wybierz zawartość zakupu w aplikacji")](changes-to-storekit-images/image10.png#lightbox)

Wprowadź **nazwa produktu** i **identyfikator firmy** tak, aby **identyfikator pakietu** odpowiada **identyfikator produktu** wprowadzony w programach iTunes Połącz dla tego produktu.

 [![](changes-to-storekit-images/image11.png "Wprowadź nazwę i identyfikator")](changes-to-storekit-images/image11.png#lightbox)

Teraz trzeba będzie pusty **zawartości zakupu w aplikacji** projektu. Możesz kliknąć prawym przyciskiem myszy i **Dodawanie plików...** lub przeciągnij je do **Nawigatora projektu**. Upewnij się, że **ContentVersion** jest prawidłowa (go powinien rozpocząć od 1.0, ale jeśli chcesz później zaktualizować zawartości, pamiętaj, aby zwiększyć jego).

Ten zrzut ekranu przedstawia Xcode z plikami zawartości dołączony do projektu i wpisy plist widoczne w oknie głównym:

 [![](changes-to-storekit-images/image12.png "Ten zrzut ekranu przedstawia Xcode z plikami zawartości dołączony do projektu i wpisy plist widoczne w oknie głównym")](changes-to-storekit-images/image12.png#lightbox)

Po dodaniu wszystkich plików zawartości można zapisać tego projektu i edytować go ponownie później lub rozpocząć procesu przekazywania.

## <a name="uploading-pkg-files"></a>Przekazywanie. Pliki PKG

Najprostszym sposobem przekazywania pakietów zawartości jest z **Xcode archiwum narzędzia**. Wybierz **produktu > archiwum** z menu, aby rozpocząć:

 ![](changes-to-storekit-images/image13.png "Wybierz Archiven")

Pakiet zawartości zostanie następnie wyświetlona w archiwum, jak pokazano poniżej. Ikona i typ archiwum Pokaż to powiadomienie **archiwum zawartości zakupu w aplikacji**. Kliknij przycisk **sprawdzania poprawności...** Aby sprawdzić naszej zawartości pakietu błędy bez faktycznie preforming przekazywania.

 [![](changes-to-storekit-images/image14.png "Sprawdzanie poprawności pakietu")](changes-to-storekit-images/image14.png#lightbox)

Zaloguj się za pomocą programu iTunes poświadczenia Connect:

 [![](changes-to-storekit-images/image15.png "Zaloguj się za pomocą programu iTunes poświadczenia Connect")](changes-to-storekit-images/image15.png#lightbox)

Wybierz właściwą i zakupu w aplikacji, aby skojarzyć tę zawartość z:

 [![](changes-to-storekit-images/image16.png "Wybieranie właściwej aplikacji i zakupu w aplikacji, aby skojarzyć tę zawartość z")](changes-to-storekit-images/image16.png#lightbox)

Powinien zostać wyświetlony następujący komunikat:

 ![](changes-to-storekit-images/image17.png "Przykład żaden komunikat problemów")

Teraz przejdź za pomocą podobnej procedury, ale kliknięcie **dystrybucji...** faktycznie będzie przekazywać zawartość.

 [![](changes-to-storekit-images/image18.png "Rozpowszechnianie aplikacji")](changes-to-storekit-images/image18.png#lightbox)

Wybierz opcję pierwszy, aby przekazywać zawartość:

 ![](changes-to-storekit-images/image19.png "Przekazywanie zawartości")

Zaloguj się ponownie:

 [![](changes-to-storekit-images/image15.png "Logowanie w")](changes-to-storekit-images/image15.png#lightbox)

Wybierz właściwej aplikacji i rekord zakupu w aplikacji, które można przekazać zawartości do:

 [![](changes-to-storekit-images/image20.png "Wybierz rekord zakupu aplikacji i w aplikacji")](changes-to-storekit-images/image20.png#lightbox)

Zaczekaj, aż przekazanie plików:

 [![](changes-to-storekit-images/image21.png "Okno dialogowe przekazywania zawartości")](changes-to-storekit-images/image21.png#lightbox)

Po zakończeniu przekazywania informujące, że zawartość zostało przesłane do sklepu z aplikacjami zostanie wyświetlony komunikat.

 [![](changes-to-storekit-images/image22.png "Przykład pomyślne ukończenie przekazywania komunikatów")](changes-to-storekit-images/image22.png#lightbox)

Po którym zostały wykonane, po powrocie do strony produktu na iTunes Connect zostanie Pokaż szczegóły pakietu i znajdować się w **gotowy do przesyłania** stanu. Gdy produkt jest w tym stanie, możesz rozpocząć testowanie w środowisku piaskownicy. NIE należy do przesyłania produktu do testowania w piaskownicy.

 [![](changes-to-storekit-images/image23.png "iTunes Connect zostanie Pokaż szczegóły pakietu i można w oknie gotowy do przesyłania stanu")](changes-to-storekit-images/image23.png#lightbox)

Może potrwać pewien czas (np.) kilka minut) między przekazywania archiwum i iTunes aktualizacji stanu połączenia. Możesz przesłać produktu do przeglądu oddzielnie, lub przesłać je w połączeniu z danych binarnych aplikacji. Tylko wtedy, gdy Apple ma zatwierdzonych zawartość go będą dostępne w środowisku produkcyjnym sklepu z aplikacjami zakupu w aplikacji.

### <a name="pkg-file-format"></a>Format pliku PKG

Za pomocą narzędzia archiwum i Xcode Utwórz i przekaż pakiet zawartości hostowanej oznacza, że nigdy nie zobaczyć zawartość pakietu. Pliki i katalogi w pakietach utworzone dla przykładowej aplikacji wyglądać tak, z `plist` pliku w folderze głównym, a pliki produktu w `Contents` podkatalogu:

 [![](changes-to-storekit-images/image24.png "Plik plist w katalogu głównym, a pliki produktu w podkatalogu zawartości")](changes-to-storekit-images/image24.png#lightbox)

Należy pamiętać, struktura katalogów pakietu (szczególnie lokalizację plików w `Contents` podkatalogu) ponieważ będą potrzebne zrozumieć te informacje, aby wyodrębnić pliki z pakietu na urządzeniu.

### <a name="updating-package-content"></a>Trwa aktualizowanie zawartości pakietu

Procedury aktualizowania zawartości po jej zatwierdzeniu:

-  Edytuj projekt zawartości zakupu w aplikacji w środowisku Xcode.
-  Zwiększyć numer wersji w.
-  Ponownie przekazać do iTunes Connect. Kolejne nabywców automatycznie pobierze najnowszą wersję, ale użytkownicy, którzy już mają starą wersję nie otrzyma powiadomienia. 
-  Aplikacja jest odpowiedzialny za powiadamiania użytkowników i wspierania je, aby pobrać nowszą wersję zawartości. Aplikację należy utworzyć również funkcję, która pobiera nowej wersji. Należy to zrobić za pomocą funkcji Przywracanie zestawu magazynu. 
-  Aby ustalić, czy istnieje nowsza wersja, należy można utworzyć funkcji w aplikacji można pobrać SKProducts (np.) sam proces, który służy do pobierania cen produktu) i porównywania właściwości ContentVersion. 

## <a name="purchasing-overview"></a>Zakup — omówienie

Przed przeczytaniem tej części, przejrzyj istniejące [dokumentacji zakupu w aplikacji](~/ios/platform/in-app-purchasing/index.md).

Kolejność zdarzeń, gdy produkt o hostowaną zawartość została zakupiona i przedstawiono pobierania na tym diagramie:

 [![](changes-to-storekit-images/image25.png "Kolejność zdarzeń, gdy produkt o hostowaną zawartość została zakupiona i Pobierz")](changes-to-storekit-images/image25.png#lightbox)

1.  Można tworzyć nowych produktów w iTunes Połącz przy użyciu hostowanej zawartości włączone. Rzeczywistej zawartości jest zbudowane oddzielnie w środowisku Xcode (jako po prostu jako przeciągania plików do folderu) i następnie zarchiwizowane i przekazać do iTunes (kodowanie nie jest wymagane). Każdego produktu jest następnie przesłać do zatwierdzenia, po upływie którego będzie można kupić. W przykładowym kodzie te identyfikatory produktu są zapisane na stałe, ale obsługi zawartości w firmie Apple jest bardziej elastyczne przechowywać listę produktów dostępnych na serwerze zdalnym, dzięki czemu mogą być aktualizowane podczas przesyłania nowych produktów i zawartości do iTunes Connect. 
1.  Gdy użytkownik zakupi produktu, transakcja jest umieszczone w kolejce płatności do przetwarzania. 
1.  Zestaw magazynu przesyła żądanie zakupu iTunes serwerów w celu przetwarzania. 
1.  Transakcja jest zakończona na serwerach programu iTunes (np.) Klient jest rozliczana) i potwierdzenia jest zwracana do aplikacji, z dołączonych w tym Określa, czy jest dostępna do pobrania informacji o produkcie (i jeśli tak, rozmiar pliku i innych metadanych). 
1.  Kod należy sprawdzić, czy produkt jest downloadble i tak aby żądania pobierania zawartości, który również jest umieszczony w kolejce płatności. Zestaw magazynu wysyła to żądanie do serwerów programu iTunes. 
1.  Serwer zwraca pliku zawartości do Kit magazynu, który zawiera wywołanie zwrotne do zwracania postęp pobierania oraz czas pozostały szacuje w kodzie. 
1.  Po wykonaniu tych czynności, Uzyskaj powiadomiony i przekazany lokalizację plików w folderze z pamięcią podręczną. 
1.  Kod powinien skopiowania plików i sprawdź, Zapisz tutaj stan, który należy pamiętać, że produkt został kupiony. Korzystając z okazji, aby poprawnie ustawiona flaga kopii zapasowej dla nowych plików (wskazówkę: jeśli pochodzi z serwera i nigdy nie są edytowane przez użytkownika, dlatego prawdopodobnie Kontynuuj tworzenie ich kopii zapasowych, ponieważ użytkownik może zawsze pobierają je z serwerami firmy Apple w przyszłości). 
1.  Wywołanie FinishTransaction. Jest to ważne, ponieważ powoduje usunięcie z kolejki płatności transakcji. Ważne jest również czy nie wymagają FinishTransaction dopiero po skopiowaniu zawartości z katalogu pamięci podręcznej. Po wywołaniu FinishTransaction plików pamięci podręcznej mogą szybko zostaną usunięte. 


## <a name="implementing-hosted-content-purchase"></a>Implementowanie hostowanej zawartości zakupu

Poniższe informacje, należy przeczytać łącznie z pełną [dokumentacji zakupy w aplikacji](~/ios/platform/in-app-purchasing/index.md). Informacje przedstawione w tym dokumencie koncentruje się na różnice między hostowaną zawartość i poprzedniej implementacji.


### <a name="classes"></a>Klasy

Następujące klasy zostały dodane lub zmienione w celu obsługi hostowanej zawartości w systemie iOS 6:

-   **SKDownload** — nowa klasa, która reprezentuje operację pobierania. Interfejs API umożliwia więcej niż jeden na produktu, jednak początkowo tylko jeden z nich została zaimplementowana. 
-   **SKProduct** — nowe właściwości dodany: `Downloadable` , `ContentVersion` , `ContentLengths` tablicy. 
-   **SKPaymentTransaction** — nową właściwość dodany: `Downloads` , który zawiera kolekcję `SKDownload` obiekty, jeśli ten produkt ma hostowaną zawartość dostępna do pobrania. 
-   **SKPaymentQueue** — nowa metoda dodany: `StartDownloads` . Wywołanie tej metody za pomocą `SKDownload` obiektów można pobrać ich hostowaną zawartość. Pobieranie może wystąpić w tle. 
-   **SKPaymentTransactionObserver** — nowej metody: `UpdateDownloads` . Zestaw magazynu wywołuje tę metodę postępu informacje o bieżących operacji pobierania. 


Szczegóły nowej `SKDownload` klasy:

-   **Postęp** — wartość z zakresu od 0-1, używanej do wyświetlania wskaźnik procent wykonania dla użytkownika. Nie używaj postępu == 1, aby wykryć, czy pobranie zostanie ukończone, sprawdź stan == zakończone. 
-   **TimeRemaining** — oszacowanie pobierania pozostały czas, w sekundach. -1 oznacza, że nadal oblicza szacowania. 
-   **Stan** — aktywny, oczekiwania, zostało zakończone, nie powiodło się, wstrzymana, anulowane. 
-   **ContentURL** — lokalizacja, w których zawartość została umieszczona na dysku w pliku `Cache` katalogu. Wypełnione tylko po zakończeniu pobierania. 
-   **Błąd** — Określ, czy ta właściwość stanu nie powiodło się. 


Interakcje między klasami w przykładowym kodzie przedstawiono na tym diagramie (kod specyficzne dla hostowanej zakupy zawartości znajduje się na zielono):

 [![](changes-to-storekit-images/image26.png "Zakupy zawartości hostowanej jest wyświetlany na zielono na tym diagramie")](changes-to-storekit-images/image26.png#lightbox)

Przykładowy kod, w których zastosowano te klasy przedstawiono w dalszej części tej sekcji:

### <a name="custompaymentobserver-skpaymenttransactionobserver"></a>CustomPaymentObserver (SKPaymentTransactionObserver)

Zmień istniejące `UpdatedTransactions` należy przesłonić, aby sprawdzić zawartość do pobrania i wywołanie `StartDownloads` w razie potrzeby:

```csharp
public override void UpdatedTransactions (SKPaymentQueue queue, SKPaymentTransaction[] transactions)
{
    foreach (SKPaymentTransaction transaction in transactions) {
        switch (transaction.TransactionState) {
        case SKPaymentTransactionState.Purchased:
            // UPDATED FOR iOS 6
            if (transaction.Downloads != null && transaction.Downloads.Length > 0) {
                // Purchase complete, and it has downloads... so download them!
                SKPaymentQueue.DefaultQueue.StartDownloads (transaction.Downloads);
                // CompleteTransaction() call has moved after downloads complete
            } else {
                // complete the transaction now
                theManager.CompleteTransaction(transaction);
            }
            break;
        case SKPaymentTransactionState.Failed:
            theManager.FailedTransaction(transaction);
            break;
        case SKPaymentTransactionState.Restored:
            // TODO: you must decide how to handle restored transactions.
            // Triggering all the downloads at once is not advisable.
            theManager.RestoreTransaction(transaction);
            break;
        default:
            break;
        }
    }
}
```

Nowe przeciążonej `UpdatedDownloads` są wyświetlane poniżej. Zestaw magazynu wywołuje tę metodę po `StartDownloads` jest wyzwalane w `UpdatedTransactions`. Ta metoda jest wywoływana *wielokrotnie* odstępach nieokreślony, aby zapewnić użytkownikowi postęp pobierania, a następnie ponownie po zakończeniu pobierania. Powiadomienie metody akceptuje tablicę `SKDownload` obiekty, dlatego każde wywołanie metody można podać stan pobranie wielu materiałów w kolejce. Jak pokazano w implementacji poniżej są następujące stany pobierania sprawdzane co czasu i odpowiednich działań.

```csharp
// ENTIRELY NEW METHOD IN iOS6
public override void PaymentQueueUpdatedDownloads (SKPaymentQueue queue, SKDownload[] downloads)
{
    Console.WriteLine (" -- PaymentQueueUpdatedDownloads");
    foreach (SKDownload download in downloads) {
        switch (download.DownloadState) {
        case SKDownloadState.Active:
            // TODO: implement a notification to the UI (progress bar or something?)
            Console.WriteLine ("Download progress:" + download.Progress);
            Console.WriteLine ("Time remaining:   " + download.TimeRemaining); // -1 means 'still calculating'
            break;
        case SKDownloadState.Finished:
            Console.WriteLine ("Finished!!!!");
            Console.WriteLine ("Content URL:" + download.ContentUrl);

            // UNPACK HERE! Calls FinishTransaction when it's done
            theManager.SaveDownload (download);

            break;
        case SKDownloadState.Failed:
            Console.WriteLine ("Failed"); // TODO: UI?
            break;
        case SKDownloadState.Cancelled:
            Console.WriteLine ("Cancelled"); // TODO: UI?
            break;
        case SKDownloadState.Paused:
        case SKDownloadState.Waiting:
            break;
        default:
            break;
        }
    }
}
```

### <a name="inapppurchasemanager-skproductsrequestdelegate"></a>InAppPurchaseManager (SKProductsRequestDelegate)

Ta klasa zawiera nową metodę `SaveDownload` jest to po każdym pobierania zostało ukończone pomyślnie.

Hostowana zawartość została pobrana pomyślnie i rozpakowane do `Cache` katalogu. Struktury. Plik PKG wymaga wszystkich plików do zapisania w `Contents` podkatalogów, a więc poniższy kod wyodrębnienie plików z poziomu `Contents` podkatalogu.

Kod iteruje wszystkie pliki zawartości pakietu i kopiuje je do `Documents` katalogu, w podfolderze o nazwie odpowiadającej `ProductIdentifier`. Na koniec wywołuje `CompleteTransaction`, które wywołuje `FinishTransaction` można usunąć z kolejki płatności transakcji.

```csharp
// ENTIRELY NEW METHOD IN iOS 6
public void SaveDownload (SKDownload download)
{
    var documentsPath = Environment.GetFolderPath (Environment.SpecialFolder.Personal); // Documents folder
    var targetfolder = System.IO.Path.Combine (documentsPath, download.Transaction.Payment.ProductIdentifier);
    // targetfolder will be "/Documents/com.xamarin.storekitdoc.montouchimages/" or something like that
    if (!System.IO.Directory.Exists (targetfolder))
        System.IO.Directory.CreateDirectory (targetfolder);
    foreach (var file in System.IO.Directory.EnumerateFiles 
             (System.IO.Path.Combine(download.ContentUrl.Path, "Contents"))) { // Contents directory is the default in .PKG files
        var fileName = file.Substring (file.LastIndexOf ("/") + 1);
        var newFilePath = System.IO.Path.Combine(targetfolder, fileName);
        if (!System.IO.File.Exists(newFilePath)) // HACK: this won't support new versions...
            System.IO.File.Copy (file, newFilePath);
        else
            Console.WriteLine ("already exists " + newFilePath);
    }
    CompleteTransaction (download.Transaction); // so it gets 'finished'
}
```

Gdy `FinishTransaction` jest nazywany pobrane pliki są już gwarancji w `Cache` katalogu. Powinny zostać skopiowane wszystkie pliki przed wywołaniem `FinishTransaction`.


## <a name="other-considerations"></a>Inne zagadnienia

Przykładowy kod powyżej przedstawiono dość proste stosowania hostowanej zakupu zawartości. Istnieją pewne dodatkowe punkty, które należy wziąć pod uwagę:

### <a name="detecting-updated-content"></a>Wykrywanie zaktualizowanej zawartości

W trakcie można zaktualizować pakietów zawartości hostowanej zestawu magazynu nie zapewnia żadnych mechanizm Przekładanie te aktualizacje do użytkowników, którzy już zostały pobrane i zakupionych produktu. Do wykonania tej funkcji może sprawdzić nowy kod `SKProduct.ContentVersion` właściwości (Jeśli `SKProduct` jest `Downloadable`) regularnie i wykrywa, czy wartość jest zwiększany. Alternatywnym rozwiązaniem jest utworzenie systemu powiadomień wypychanych.

### <a name="installing-updated-content-versions"></a>Instalowanie zaktualizowanych wersji zawartości

W powyższym przykładowym kodzie pomija kopiowania plików, jeśli plik już istnieje. NIE jest dobrym rozwiązaniem, jeśli chcesz obsługuje nowszych wersji pobierania zawartości.

Alternatywą może być do kopiowania zawartości do folderu o nazwie dla wersji i śledzenie bieżącej wersji (np.) w `NSUserDefaults` lub wszędzie tam, gdzie są przechowywane rekordy zakupu ukończone).

### <a name="restoring-transactions"></a>Przywracanie transakcji

Gdy `SKPaymentQueue.DefaultQueue.RestoreCompletedTransactions` jest wywoływana, zestaw magazynu zwraca wszystkie wcześniejsze transakcje dla użytkownika. Jeśli zakupione dużą liczbę elementów lub jeśli każdego zakupu ma bardzo dużych pakietów zawartości, przywracania może doprowadzić do dużą ilość ruchu sieciowego jako wszystko pobiera umieszczone w kolejce do pobrania na raz.

Należy rozważyć śledzenie, czy produkt został kupiony oddzielnie od rzeczywistej pobieranie skojarzonego pakietu zawartości.

### <a name="pausing-restarting-and-canceling-downloads"></a>Wstrzymywanie, ponowne uruchomienie i anulowanie pliki do pobrania

Mimo że przykładowy kod nie wykazują tej funkcji, istnieje możliwość wstrzymywanie i wznawianie hostowanej pobierania zawartości. `SKPaymentQueue.DefaultQueue` Ma metody `PauseDownloads`, `ResumeDownloads` i `CancelDownloads`.

Jeśli kod wywołuje `FinishTransaction` w kolejce płatności przed Trwa pobieranie `Finished` , a następnie tego pobierania została anulowana automatycznie.

### <a name="setting-the-skip-backup-flag-on-the-downloaded-content"></a>Ustawienie flagi kopii zapasowej SKIP na pobranej zawartości

Wskazówki dotyczące tworzenia kopii zapasowej iCloud firmy Apple sugerują, która ma zostać zawartości niezwiązanych z użytkownikiem, który jest łatwo przywrócić z serwera *nie* być kopii zapasowej (ponieważ niepotrzebnie użyje iCloud magazynie). Zapoznaj się [Praca w systemie plików](~/ios/app-fundamentals/file-system.md) dokumentacji, aby uzyskać więcej informacji na temat ustawiania atrybutu kopii zapasowej.

## <a name="summary"></a>Podsumowanie

Ten artykuł ma wprowadzono dwie nowe funkcje zestawu magazynu w iOS6: zakup programu iTunes i innej zawartości z aplikacji i przy użyciu serwera firmy Apple do obsługi zakupy w aplikacji. To wprowadzenie powinny być odczytywane w połączeniu z istniejącym [dokumentacji zakupu w aplikacji](~/ios/platform/in-app-purchasing/index.md) pełną pokrycia wdrażania funkcji zestawu magazynu.

## <a name="related-links"></a>Linki pokrewne

- [StoreKit (przykład)](https://developer.xamarin.com/samples/StoreKit/)
- [Zakupy w aplikacjach](~/ios/platform/in-app-purchasing/index.md)
- [Odwołanie StoreKit Framework](https://developer.apple.com/library/prerelease/ios/#documentation/StoreKit/Reference/StoreKit_Collection/_index.html)
- [Odwołania do klasy SKStoreProductViewController](https://developer.apple.com/library/ios/documentation/StoreKit/Reference/SKITunesProductViewController_Ref/SKStoreProductViewController.html)
- [Dokumentacja interfejsu API wyszukiwania iTunes](http://www.apple.com/itunes/affiliates/resources/documentation/itunes-store-web-service-search-api.html)
- [SKDownload](https://developer.apple.com/library/prerelease/ios/#documentation/StoreKit/Reference/SKDownload_Ref/Introduction/Introduction.html)
- [SKPaymentQueue](https://developer.apple.com/library/prerelease/ios/documentation/StoreKit/Reference/SKPaymentQueue_Class/Reference/Reference.html#/apple_ref/occ/instm/SKPaymentQueue/cancelDownloads:)
- [SKProduct](https://developer.apple.com/library/prerelease/ios/documentation/StoreKit/Reference/SKProduct_Reference/Reference/Reference.html#/apple_ref/occ/instp/SKProduct/downloadable)
- [WWDC wideo: Sprzedających się produktów z zestawem magazynu](https://developer.apple.com/videos/wwdc/2012/?include=302#302)
