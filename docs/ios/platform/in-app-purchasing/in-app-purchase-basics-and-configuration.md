---
title: "Podstawowe informacje dotyczące zakupu w aplikacji i konfiguracji"
ms.topic: article
ms.prod: xamarin
ms.assetid: 11FB7F02-41B3-2B34-5A4F-69F12897FE10
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 8e0f5b24ff6790aa3bf63eb9112790e0a62ce0a3
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="in-app-purchase-basics-and-configuration"></a>Podstawowe informacje dotyczące zakupu w aplikacji i konfiguracji

Implementowanie zakupy w aplikacji wymaga aplikacji mogą korzystać z interfejsu API StoreKit na urządzeniu. StoreKit zarządza cała komunikacja z serwerami iTunes firmy Apple, aby uzyskać informacje o produkcie i wykonywać transakcje. Profil inicjowania obsługi administracyjnej musi być skonfigurowany do zakupu w aplikacji i informacji o produkcie należy wprowadzić w iTunes Connect.

 [ ![](in-app-purchase-basics-and-configuration-images/image1.png "StoreKit zarządza cała komunikacja z firmy Apple, jak pokazano na tym wykresie.")](in-app-purchase-basics-and-configuration-images/image1.png)

Zapewnienie zakupu w aplikacji przy użyciu sklepu wymaga następujących instalacji i konfiguracji:

-  **Połącz iTunes** — Konfigurowanie produktów, sprzedaż i konfigurowanie kont użytkowników piaskownicy, aby przetestować zakupu. Należy także podano informacji bankowych i podatek do firmy Apple, ich przekazać funduszy zbierane w Twoim imieniu.
-   **System iOS w portalu inicjowania obsługi** — identyfikator pakietu do tworzenia i włączania dostępu do sklepu z aplikacjami dla aplikacji.
-  **Przechowywanie zestawu** — Dodawanie kodu do aplikacji Wyświetlanie produktów, zakupu produktów i przywracanie transakcji.
-  **Kod niestandardowy** — aby śledzić zakupy dokonane przez klientów i podaj produktów lub usług one zakupione. Możesz może również należy zastosować proces po stronie serwera do sprawdzania poprawności potwierdzenia, jeśli produktów składają się z zawartości pobrane z serwera (na przykład książki i problemy magazine).


Istnieją dwa magazynu Kit "serwera środowiska":

-  **Produkcji** — transakcje z pieniędzy. Jest to dostępne tylko za pośrednictwem aplikacji, które zostały przesłane i zatwierdzone przez firmę Apple. Produkty zakupu w aplikacji również musi być sprawdzone i zatwierdzone, zanim staną się dostępne w środowisku produkcyjnym.
-  **Piaskownica** — w przypadku, gdy testowanie sytuacji. Produkty są dostępne w tym miejscu natychmiast po utworzeniu (proces zatwierdzania dotyczy tylko do środowiska produkcyjnego). Transakcje w piaskownicy wymagają użytkowników testowych (nie rzeczywistych Apple ID) do wykonania transakcji.

## <a name="in-app-purchase-rules"></a>Reguły zakupu w aplikacji

Nie można zaakceptować inne formy płatności na cyfrowe produktów lub usług w Twojej aplikacji ani wspomina je lub dotyczą użytkowników je z poziomu aplikacji. Oznacza to, że podczas zakupów w aplikacji jest najbardziej odpowiedni mechanizm płatności nie może zaakceptować kart kredytowych lub PayPal. Brak szczególnych przypadkach do zakupu cyfrowe produkty poza aplikacją, ale do użycia w aplikacji, takich jak zakup książek z witryny sieci Web, skojarzonych z konkretnymi "login" i przy użyciu czy "login", w aplikacji umożliwia użytkownikowi dostęp zakupionych — książki.
Aplikacje, które działają w ten sposób nie mogą wymienić lub łącze do zewnętrznego funkcji zakupów — deweloperzy muszą komunikować się tej możliwości do użytkowników w inny sposób (prawdopodobnie za pośrednictwem poczty e-mail marketing lub bezpośredni channel).

Jednak ponieważ nie można użyć zakupy w aplikacjach dla fizycznej towarów, w tym przypadku, gdy można użyć mechanizm alternatywny płatności (np. karta płatnicza, konto PayPal) z poziomu aplikacji.

Apple musi zatwierdzić wszystkie produkty, przed przechodzi ona na sprzedaży — nazwa, opis i zrzut ekranu produktu jest wymagany do przeglądu. Czasy przeglądu produktu są takie same jak w przypadku przeglądami aplikacji.

Nie można wybrać żadnych cen dla produktu — możesz wybrać "warstwę cen", która ma określoną wartość w każdej kraju/waluty, który obsługuje firmy Apple. Nie może mieć warstwy cen różne na różnych rynkach.

## <a name="configuration"></a>Konfiguracja

Przed zapisaniem dowolny kod zakupów w aplikacji należy wykonania dodatkowych czynności konfiguracji w iTunes Connect ( [itunesconnect.apple.com](http://itunesconnect.apple.com)) i portalu inicjowania obsługi systemu iOS ( [developer.apple.com/iOS](http://developer.apple.com/iOS)).

Te trzy kroki powinna zakończyć się przed pisania żadnego kodu:

-  **Konto dewelopera Apple** — przesyłania informacji bankowych i podatków, do firmy Apple.
-  **System iOS w portalu inicjowania obsługi** — upewnij się, aplikacja ma prawidłowy identyfikator aplikacji (nie symbol wieloznaczny gwiazdką * w nim) i włączono w aplikacji zakupu.
-  **iTunes Zarządzanie aplikacjami Connect** — Dodaj produktów do aplikacji.


### <a name="apple-developer-account"></a>Konto dewelopera firmy Apple

Tworzenie i dystrybucja bezpłatnych aplikacji wymaga bardzo małego konfiguracji w [iTunes Connect](https://itunesconnect.apple.com), jednak do sprzedaży płatnych aplikacji i zakupy w aplikacji wymaga podania Apple informacji bankowych i podatków. Polecenie **umów podatku i bankowości** z poziomu menu głównego, pokazano poniżej:

 [ ![](in-app-purchase-basics-and-configuration-images/image2.png "Polecenie umowy, podatku i banku z poziomu menu głównego")](in-app-purchase-basics-and-configuration-images/image2.png)

Konto dewelopera ma **płatnej aplikacji systemu iOS** kontraktu w efekcie, jak pokazano w tym zrzut ekranu:

 [ ![](in-app-purchase-basics-and-configuration-images/image3.png "Konto dewelopera ma iOS, które aplikacje płatnej kontraktu w celu")](in-app-purchase-basics-and-configuration-images/image3.png)

Nie można przetestować dowolne funkcje StoreKit, aż do uzyskania **płatnej aplikacji systemu iOS** kontraktu — StoreKit wywołań w kodzie zakończy się niepowodzeniem do czasu przetworzenia Apple Twojej **umów podatku i bankowości** informacje.

### <a name="ios-provisioning-portal"></a>System iOS w portalu inicjowania obsługi

Nowe aplikacje są zainstalowane w **identyfikatorów aplikacji** sekcji **systemu iOS w portalu inicjowania obsługi**. Aby utworzyć nowy identyfikator aplikacji, przejdź do [Member Center portalu inicjowania obsługi systemu IOS](https://developer.apple.com/membercenter/index.action), przejdź do **certyfikaty identyfikatory i profile** części portalu i kliknij pozycję **identyfikatorów** w obszarze *aplikacji dla systemu iOS*. Następnie kliknij przycisk "+" na początku prawej strony, aby wygenerować nowy identyfikator aplikacji.


Tworzenie nowego formularza **identyfikatorów aplikacji**

 wygląda następująco:

 [ ![](in-app-purchase-basics-and-configuration-images/image4.png "Formularz służący do tworzenia nowych identyfikatorów aplikacji")](in-app-purchase-basics-and-configuration-images/image4.png)

Wprowadź odpowiednie dla elementu *opis*, więc można łatwo zidentyfikować ten identyfikator aplikacji na liście. Aby uzyskać *prefiks Identyfikatora aplikacji*, wybierz nazwę zespołu.

#### <a name="bundle-identifierapp-id-suffix-format"></a>Format Identyfikatora sufiksu identyfikator/aplikacji pakietu

Można użyć dowolnego ciągu dla Twojego **identyfikator pakietu** (o ile jest unikatowa w ramach konta), jednak zaleca firmy Apple, wykonaj format wstecznego DNS zamiast możesz użyć dowolnego dowolnego ciągu. Przykładowej aplikacji, który towarzyszy w tym artykule używa com.xamarin.storekit.testing identyfikatora pakietu, jednak byłoby jednakowo można użyć identyfikatora, takich jak my_store_example (nawet jeśli firmy Apple nie zaleca się jej).

> [!IMPORTANT]
> **Uwaga**: Apple umożliwia również — symbol wieloznaczny gwiazdka do dodania na koniec **identyfikator pakietu** tak, aby pojedynczy identyfikator aplikacji może służyć do wielu aplikacji, jednak _identyfikatorów aplikacji — symbol wieloznaczny nie może służyć do W AppPurchase_. Przykład, że identyfikator pakietu — symbol wieloznaczny może być com.xamarin.*

#### <a name="enabling-app-services"></a>Włączanie usług aplikacji

Należy pamiętać, że **zakupu w aplikacji** zostaną automatycznie włączone na liście usług:

 [ ![](in-app-purchase-basics-and-configuration-images/image5.png "Funkcja zakupu w aplikacji zostaną automatycznie włączone na liście usług")](in-app-purchase-basics-and-configuration-images/image5.png)

#### <a name="provisioning-profiles"></a>Profile aprowizacji

Utworzyć rozwoju i produkcji profile inicjowania obsługi, ponieważ zwykle wybierając identyfikator aplikacji, który skonfigurowano do zakupu w aplikacji. Zapoznaj się [Inicjowanie obsługi administracyjnej urządzeń z systemem iOS](~/ios/get-started/installation/device-provisioning/index.md) i [publikowania do sklepu z aplikacjami](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md) przewodniki, aby uzyskać więcej informacji.

## <a name="itunes-connect"></a>iTunes Connect

Kliknij przycisk **Moje aplikacje** w iTunes Connect do tworzenia lub edytowania wpisu aplikacji systemu iOS. Strona informacje o aplikacji jest następujący:

 [ ![](in-app-purchase-basics-and-configuration-images/image6.png "Strona informacje o aplikacji")](in-app-purchase-basics-and-configuration-images/image6.png)

Kliknij przycisk **zakupy w aplikacji** do tworzenia lub edytowania produktów do sprzedaży. Ten zrzut ekranu przedstawia przykładową aplikację z kilku produktów już dodany:

 [ ![](in-app-purchase-basics-and-configuration-images/image7.png "Przykładowa aplikacja z kilku produktów już dodana")](in-app-purchase-basics-and-configuration-images/image7.png)

Proces dodawania nowych produktów obejmuje dwa kroki:

1.   Wybierz typ produktu: [ ![ ] (in-app-purchase-basics-and-configuration-images/image8.png "wybierz typ produktu")](in-app-purchase-basics-and-configuration-images/image8.png) 
2.   Wprowadź atrybuty produktu, w tym identyfikator produktu, warstwa cenowa i opisy zlokalizowanych: [ ![ ] (in-app-purchase-basics-and-configuration-images/image9.png "wprowadzania atrybutów produktów")](in-app-purchase-basics-and-configuration-images/image9.png)

Poniżej opisano pól wymaganych dla każdego produktu zakupu w aplikacji:


### <a name="reference-name"></a>Nazwa odwołania

Nazwa odwołania nie jest wyświetlana dla użytkowników; jest do użytku wewnętrznego i jest wyświetlany tylko w iTunes Connect.

### <a name="product-id-format"></a>Format Identyfikatora produktu

Identyfikator produktu może zawierać alfanumeryczne (A-Z, a-z, 0-9), podkreślenia (_) i znaki kropki (.). Mimo że można użyć dowolnego ciągu z identyfikatorów, Firma Apple zaleca format wstecznego DNS. Na przykład przykładowa aplikacja korzysta z tego identyfikatora pakietu:

 `com.xamarin.storekit.testing`

W związku z tym Konwencji do identyfikowania produktów zakupu w aplikacji, należy w następujący sposób:

```csharp
com.xamarin.storekit.testing.consume5credits
com.xamarin.storekit.testing.consume10credits
com.xamarin.storekit.testing.sepia
com.xamarin.storekit.testing.greyscale
```

Konwencja nazewnictwa nie jest wymuszana, po prostu zalecenia ułatwiające zarządzanie produktów. Co więcej, niezależnie od tej samej Konwencji wstecznego DNS, identyfikatory produktu są *niezwiązanych* identyfikator pakietu i czy nie musi rozpoczynać się od tych samych parametrach. Nadal będzie można użyć identyfikatory, takie jak photo_product_greyscale (nawet jeśli firmy Apple nie zaleca się jej).

Identyfikator produktu nie jest wyświetlana dla użytkowników, ale jest używana do odwołania produktu w kodzie aplikacji.

### <a name="product-type"></a>Typ produktu

Istnieje pięć typów produktu zakupu w aplikacji, które może zaoferować:

1.  **Generowanie** — elementy "używane się", takie jak walutę w grze odtwarzacz może przeznaczyć. Jeśli użytkownik wykona kopii zapasowej/przywracania, w przeciwnym razie odświeżył swoich urządzeń eksploatacyjny transakcji nie pobrać przywrócić również (które skutecznie pozwoli uzyskać takie same korzyści ponownego odtwarzacz). Kod aplikacji należy przygotować elementu eksploatacyjny zaraz po zakończeniu transakcji.
1.  **Jednoznacznie składnika** — należące do użytkownika "" raz zakupionych produktów, takie jak cyfrowe problem magazine lub poziomie meczu.
1.  **Subskrypcje odnawialnymi automatycznie** — podobnie jak Subskrypcja magazynu w rzeczywistych z końcem okresu subskrypcji firmy Apple automatycznie ponownie opłaty klienta i nieograniczony czas lub do klienta jawnie rozszerza subskrypcji Umożliwia anulowanie. Jest to metoda preferowanych płatności dla aplikacji Newsstand (w rzeczywistości aplikacji musi obsługiwać tej formy płatności zatwierdzenia dla dystrybucji Newsstand).
1.  **Zwolnij subskrypcji** — mogą być tworzone tylko w aplikacji z włączoną obsługą Newsstand i umożliwia klientowi dostęp do zawartości subskrypcji na swoich urządzeniach. Subskrypcji bezpłatnych nigdy nie wygasa.
1.  **Odnawianie bez subskrypcji** — powinien być używany do sprzedaży czas dostępu do zawartości statycznej, takich jak jeden miesiąc dostęp do archiwum zdjęcie.


 *W tym dokumencie opisano obecnie tylko pierwszy produktu dwa typy (niestandardowe i inne niż niestandardowe).*

 <a name="Price_Tiers" />

### <a name="price-tiers"></a>Ceny warstw

Nie zezwala na wybranie dowolnego ceny dla produktów pakietu sklepu z aplikacjami — Apple zapewnia stały ceny warstw, które są dostępne. Ceny są ustalone w każdej waluty i Apple zastrzega sobie prawo do zmiany ceny względną (na przykład po zmianie przez współczynnik względną walutowych między danej waluty i dolara Amerykańskiego).

Apple zawiera macierz cen ułatwiające wybranie poprawne warstwy waluty/cen, który ma. Fragment macierzy cen (sierpień 2012) jest następujący:

 [ ![](in-app-purchase-basics-and-configuration-images/image10.png "Fragment macierzy cen — sierpień 2012")](in-app-purchase-basics-and-configuration-images/image10.png)

W okresie przygotowywania niniejszych materiałów (czerwiec 2013) są 87 warstwy z USD 0,99 do 999,99 USD. Cennik macierzy pokazuje ceny, że klienci będą płatności i również wielkość otrzymany od firmy Apple — jest to mniej ich opłat 30% i również wszystkie podatki lokalne są wymagane do zbierania (powiadomienia w przykładzie, że sprzedających amerykańskich lub kanadyjskich otrzymywać 70 c do 99 p c mień, podczas gdy Australian sprzedających odbierać tylko 63 c ze względu na "towarów &amp; podatku usług" pobierane w cenie sprzedaży).

Cennik produktu może zostać zaktualizowana w dowolnym momencie, w tym cen zaplanowane zmiany, które zostały wprowadzone w przyszłości. Ten zrzut ekranu pokazuje, jak zmiany ceny datę w przyszłości zostanie dodany — ceny jest tymczasowo zmieniane z warstwy 1 do warstwy 3 dla miesiąca września tylko:

 [ ![](in-app-purchase-basics-and-configuration-images/image11.png "Gdzie ceny jest tymczasowo zmieniane z warstwy 1 do warstwy 3 dla miesiąca września tylko zmiany ceny datę w przyszłości")](in-app-purchase-basics-and-configuration-images/image11.png)

### <a name="free-products-not-supported"></a>Bezpłatne produkty nie są obsługiwane

Mimo że Apple udostępnił specjalnych opcji bezpłatnej subskrypcji dla aplikacji Newsstand, nie jest możliwe wartość zero (bezpłatnie) ceny dla innych typów zakupu w aplikacji. Podczas gdy można edytować (ie. niższe) cen promocji sprzedaży, nie można wprowadzić bezpłatne, za pomocą programu iTunes Połącz zakupy w aplikacji.

### <a name="localization"></a>Lokalizacja

W iTunes Connect można wprowadzić inną nazwę i opis tekstu dla dowolnej liczby obsługiwanych języków. Każdego z języków można dodać/edytować w za pomocą menu podręcznego:

 [ ![](in-app-purchase-basics-and-configuration-images/image12.png "Każdego z języków można dodać/edytować w za pomocą menu podręcznego")](in-app-purchase-basics-and-configuration-images/image12.png)   
   
   
   
 Podczas wyświetlania informacji o produkcie w aplikacji, zlokalizowany tekst jest dostępny do wyświetlenia za pośrednictwem StoreKit. Wyświetlania waluty również musi być lokalizowany do wyświetlenia poprawne symboli i dziesiętnych formatowanie — formatowanie jest uwzględnione w dalszej części dokumentu.

### <a name="app-store-review"></a>Przejrzyj sklepu z aplikacjami

Taki sam jak aplikacje — każdy z produktów jest sprawdzone przez firmę Apple przed uzyskaniem dostępu do Przejdź w promocji. Produkty, mogą zostać odrzucone dla Forefront nazwy lub opisu lub Apple może zdecydować, że wybrano typ produktu problem (np.) już utworzono książki lub magazine problem, ale używany typ eksploatacyjny produktu). Recenzje produktów może zająć przeglądu aplikacji.

Przy pierwszym uruchomieniu aplikacji jest przesyłany z zakupów w aplikacji włączone (czy jest nowa aplikacja lub funkcja została dodana do istniejącego) należy wybrać niektórych produktów, aby przesłać z nim. ITunes Connect portal spowoduje wyświetlenie monitu w tym celu opisane w tym zrzut ekranu:

 [ ![](in-app-purchase-basics-and-configuration-images/image13.png "ITunes Connect portal spowoduje wyświetlenie monitu o przesyłanie niektórych produktów, a także")](in-app-purchase-basics-and-configuration-images/image13.png)   
   
   
   
 Aplikacji i zakupy w aplikacji zostanie zweryfikowana, tak aby wszystkie zatwierdzenia jednocześnie (tak, aplikacja nie prowadzi do magazynu bez żadnych produktów zatwierdzonych!).

Po zatwierdzeniu Twoje pierwszej wersji z możliwością zakupu w aplikacji, można dodać dodatkowe produkty i przesłać je do przeglądu w dowolnym momencie. Można przesłać nowej wersji wraz z produktów określonych zakupu w aplikacji przy użyciu **szczegóły wersji** strony zgodnie z sugestią, monit.

Zapoznaj się [wytyczne przeglądu sklepu aplikacji](https://developer.apple.com/appstore/guidelines.html) Aby uzyskać więcej informacji.

 [Część 2 - Omówienie zestawu magazynu i informacji o produkcie pobierania](~/ios/platform/in-app-purchasing/store-kit-overview-and-retreiving-product-information.md)
