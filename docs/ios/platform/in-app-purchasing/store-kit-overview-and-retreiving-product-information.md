---
title: "Przechowywanie zestaw omówienie i pobierania informacji o produkcie"
ms.topic: article
ms.prod: xamarin
ms.assetid: FC21192E-6325-4389-C060-E92DBB5EBD87
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 2a636a5ee2b027a2b2889c375f1fef5be67c379b
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="store-kit-overview-and-retrieving-product-information"></a>Przechowywanie zestaw omówienie i pobierania informacji o produkcie

Interfejs użytkownika funkcji zakupu w aplikacji jest wyświetlany na zrzutach ekranu poniżej.
Przed każdą transakcję ma miejsce, aplikacja musi pobrać ceny i opis do wyświetlenia tego produktu. Następnie, gdy użytkownik naciśnie **kupić**, aplikacja wysyła żądanie do StoreKit, która zarządza okno dialogowe potwierdzenia i identyfikator Apple ID logowania. Przy założeniu, że transakcja następnie zakończy się powodzeniem, StoreKit powiadamia kodu aplikacji, które musi przechowywać wynik transakcji i zapewnić użytkownikowi dostęp do ich zakupu.   

   
 [ ![](store-kit-overview-and-retreiving-product-information-images/image14.png "StoreKit powiadamia kod aplikacji, który należy przechowywać wynik transakcji i zapewnić użytkownikowi dostęp do ich zakupu")](store-kit-overview-and-retreiving-product-information-images/image14.png)

## <a name="classes"></a>Klasy

Implementowanie zakupy w aplikacji wymaga następujących klas w ramach StoreKit:   
   
 **SKProductsRequest** — żądanie StoreKit zatwierdzonych produktów, sprzedaż (sklepu App Store). Można skonfigurować za pomocą wielu identyfikatorów produktu.

-   **SKProductsRequestDelegate** — deklaruje metod obsługi produktu żądań i odpowiedzi. 
-   **SKProductsResponse** — wysyłane z powrotem do delegata z StoreKit (sklepu App Store). Zawiera SKProducts, odpowiadających produkt, który identyfikatorów wysyłane z żądaniem. 
-   **SKProduct** — produktu pobierane z StoreKit (skonfigurowanym w iTunes Połącz). Zawiera informacje o produkcie, takich jak identyfikator produktu, tytuł, opis i cenę. 
-   **SKPayment** — utworzone za pomocą Identyfikatora produktu i dodane do kolejki płatności do wykonania zakupu. 
-   **SKPaymentQueue** — żądań płatności w kolejce do wysłania do firmy Apple. Powiadomienia są wyzwalane wyniku każdej płatności przetwarzane. 
-   **SKPaymentTransaction** — reprezentuje zakończonej transakcji (żądanie zakupu, które zostały przetworzone przez sklepu z aplikacjami i wysyłane z powrotem do aplikacji za pośrednictwem StoreKit). Transakcja może być zakupione, przywrócone lub nie powiodło się. 
-   **SKPaymentTransactionObserver** — podklasy niestandardowy, który reaguje na zdarzenia generowane przez kolejkę StoreKit płatności. 
-   **Operacje StoreKit są asynchroniczne** — po uruchomieniu SKProductRequest lub SKPayment jest dodawane do kolejki, zwróceniem sterowania do kodu. StoreKit wywoływać metod w Twojej podklasy SKProductsRequestDelegate lub SKPaymentTransactionObserver po odebraniu danych z serwerów firmy Apple. 


Na poniższym diagramie przedstawiono relacje między różnymi klasami StoreKit (klas abstrakcyjnych musi zostać wdrożona w aplikacji):   
   
   
   
 [ ![](store-kit-overview-and-retreiving-product-information-images/image15.png "Relacje między różnymi klasami abstrakcyjnej klasy StoreKit musi zostać wdrożona w aplikacji")](store-kit-overview-and-retreiving-product-information-images/image15.png)   
   
   
   
 Te klasy są szczegółowo w dalszej części tego dokumentu.

## <a name="testing"></a>Testowanie

Większość operacji StoreKit wymaga rzeczywistego urządzenia do testowania. Pobieranie informacji produktu (tj. Cena &amp; opis) będą działać w symulatorze, ale zakupu i operacje przywracania zwróci błąd (np. kod FailedTransaction = 5002 wystąpił nieznany błąd).

Uwaga: StoreKit nie będzie działać w symulatorze systemu iOS. Podczas uruchamiania aplikacji w symulatorze systemu iOS, StoreKit rejestruje ostrzeżenie, jeśli aplikacja próbuje pobrać kolejki płatności. Testowanie magazynie muszą być wykonane na rzeczywistego urządzenia.   
   
   
   
 Ważne: Nie Zaloguj się przy użyciu konta testu w aplikacji ustawienia. Możesz użyć aplikacji ustawień do podpisania poza wszelkie istniejące konto Apple ID, a następnie poczekaj, aby wyświetlić monit *w ramach programu zakupów w aplikacji sekwencji* się zalogować za pomocą testu identyfikatora firmy Apple.   
   
   
   

Próba logowania do rzeczywistego magazynu przy użyciu konta testu, zostanie automatycznie przekonwertowany na rzeczywistą identyfikator firmy Apple. To konto nie będzie już niemożliwe do testowania.

Do testowania kodu StoreKit należy wylogować regularne iTunes testowe konto i logowania przy użyciu konta specjalne testu (utworzonych w iTunes Connect), który jest połączony z magazynu testów. Aby wylogować się z bieżącego konta odwiedziny **Ustawienia > iTunes i sklepu z aplikacjami** w sposób pokazany poniżej:

 [ ![](store-kit-overview-and-retreiving-product-information-images/image16.png "Aby wylogować się z bieżącego konta odwiedziny iTunes ustawienia i sklepu z aplikacjami")](store-kit-overview-and-retreiving-product-information-images/image16.png)
 
następnie zaloguj się przy użyciu konta testowego *StoreKit na żądanie w aplikacji*:



Aby utworzyć użytkowników testowych w iTunes Connect kliknij **użytkownikami i rolami** na stronie głównej.

 [ ![](store-kit-overview-and-retreiving-product-information-images/image17.png "Aby utworzyć użytkowników testowych w programach iTunes Connect kliknij użytkowników i ról na stronie głównej")](store-kit-overview-and-retreiving-product-information-images/image17.png)

Wybierz **testerów piaskownicy**

 [ ![](store-kit-overview-and-retreiving-product-information-images/image18.png "Wybieranie testerów piaskownicy")](store-kit-overview-and-retreiving-product-information-images/image18.png)

Zostanie wyświetlona lista istniejących użytkowników. Można dodać nowego użytkownika lub usuwanie istniejącego rekordu. Nie ma portalu (obecnie) pozwalają Wyświetl lub Edytuj istniejące testowanie użytkowników, dlatego zalecane jest pozostawienie dobre wyniki każdego użytkownika testu, który jest tworzony (szczególnie hasło przypisywane). Po usunięciu użytkownika testowego adresu e-mail nie można użyć ponownie dla innego konta testowego.  
   
 [ ![](store-kit-overview-and-retreiving-product-information-images/image19.png "Zostanie wyświetlona lista istniejących użytkowników")](store-kit-overview-and-retreiving-product-information-images/image19.png)   
   
 Nowi użytkownicy testu mają podobne atrybuty do rzeczywistego Identyfikatora firmy Apple (takie jak nazwa, hasło, poufne pytanie i odpowiedź). Rejestrowanie wszystkich szczegółów wprowadzanym w tym miejscu. **Sklepie iTunes wybierz** pola określi walutę i zakupy w aplikacjach język będzie używany, gdy zalogowany jako ten użytkownik.

 [ ![](store-kit-overview-and-retreiving-product-information-images/image20.png "Określa pole sklepie iTunes Wybierz waluty i język dla ich zakupy w aplikacji użytkownika")](store-kit-overview-and-retreiving-product-information-images/image20.png)

## <a name="retrieving-product-information"></a>Trwa pobieranie informacji o produkcie

Pierwszym krokiem w sprzedaży produktu zakupu w aplikacji jest wyświetlanie: pobieranie bieżącej ceny i opis ze sklepu z aplikacjami do wyświetlenia.   
   
   
   
 Niezależnie od tego, jakiego typu produktów aplikacji (które mogą być zmienione, jednoznacznie składnika lub typu subskrypcji), zapłacisz proces pobierania informacji o produkcie do wyświetlenia jest taka sama. Kod InAppPurchaseSample dołączony ten artykuł zawiera projekt o nazwie *materiałów* który pokazuje, jak można pobrać informacji o produkcyjnym do wyświetlenia. Widoczny jest sposób:

-  Utwórz implementacja `SKProductsRequestDelegate` i wdrożenie `ReceivedResponse` metody abstrakcyjnej. Przykładowy kod wywołuje to `InAppPurchaseManager` klasy. 
-  Skontaktuj się z StoreKit, aby zobaczyć, czy są dozwolone płatności (przy użyciu `SKPaymentQueue.CanMakePayments` ). 
-  Utwórz wystąpienie `SKProductsRequest` przy użyciu identyfikatorów produktu, które zostały zdefiniowane w iTunes Connect. Odbywa się w tym przykładzie `InAppPurchaseManager.RequestProductData` metody. 
-  Wywołaj metodę Start na `SKProductsRequest` . Powoduje wywołanie asynchroniczne serwerom sklepu z aplikacjami. Delegat ( `InAppPurchaseManager` ) będzie zwrotnego wywoływana z wyników. 
-  Delegat ( `InAppPurchaseManager` ) `ReceivedResponse` metody aktualizacje interfejsu użytkownika z danymi zwracanymi ze sklepu z aplikacjami (cen produktu & opisy lub komunikaty o produktach nieprawidłowy). 

Ogólny interakcji wygląda następująco ( **StoreKit** są wbudowane w systemach iOS i **sklepu z aplikacjami** reprezentuje serwerów firmy Apple):

 [ ![](store-kit-overview-and-retreiving-product-information-images/image21.png "Trwa pobieranie informacji o produkcie wykresu")](store-kit-overview-and-retreiving-product-information-images/image21.png)

### <a name="displaying-product-information-example"></a>Wyświetlanie przykład — informacje o produkcie

[InAppPurchaseSample](https://developer.xamarin.com/samples/monotouch/StoreKit/) *materiałów* przykładowy kod pokazuje, jak można pobrać informacji o produkcie. Na ekranie głównym próbki wyświetla informacje dotyczące dwóch produktów, które są pobierane ze sklepu z aplikacjami:   
   
   
   
 [ ![](store-kit-overview-and-retreiving-product-information-images/image23.png "Ekran główny przedstawia produkty informacji pobrane ze sklepu z aplikacjami")](store-kit-overview-and-retreiving-product-information-images/image23.png)   
   
   
   
 Przykładowy kod do pobierania i wyświetlania informacji o produkcie jest co omówiono bardziej szczegółowo poniżej.


#### <a name="viewcontroller-methods"></a>Metody ViewController

`ConsumableViewController` Klasa będzie zarządzać wyświetlanie ceny dla dwóch produktów, których identyfikatory produktu są zapisane na stałe w klasie.

```csharp
public static string Buy5ProductId = "com.xamarin.storekit.testing.consume5credits",
   Buy10ProductId = "com.xamarin.storekit.testing.consume10credits";
List<string> products;
InAppPurchaseManager iap;
public ConsumableViewController () : base()
{
   // two products for sale on this page
   products = new List<string>() {Buy5ProductId, Buy10ProductId};
   iap = new InAppPurchaseManager();
}
```

W klasie poziomu należy również zadeklarowana NSObject, który będzie używany do instalacji `NSNotificationCenter` obserwatora:

```csharp
NSObject priceObserver;
```

W metodzie ViewWillAppear obserwatora jest tworzony i przypisane przy użyciu domyślnego centrum powiadomień:

```csharp
priceObserver = NSNotificationCenter.DefaultCenter.AddObserver (
  InAppPurchaseManager.InAppPurchaseManagerProductsFetchedNotification,
(notification) => {
   // display code goes here, to handle the response from the App Store
}
```

   
   
 Na koniec `ViewWillAppear` metody, wywołaj `RequestProductData` metodą inicjowania StoreKit żądania. Gdy żądanie zostało wykonane, StoreKit asynchronicznie będzie kontaktować się z serwerów firmy Apple, aby uzyskać informacje i umieść go do aplikacji. Jest to osiągane przez `SKProductsRequestDelegate` podklasy ( `InAppPurchaseManager`) który znajduje się w następnej sekcji.

```csharp
iap.RequestProductData(products);
```

Kod, aby wyświetlić cen i opis po prostu pobiera informacje z SKProduct i przypisuje go do kontroli UIKit (Zwróć uwagę, że możemy wyświetlić `LocalizedTitle` i `LocalizedDescription` — StoreKit automatycznie rozpoznaje poprawny tekst i na podstawie ceny Ustawienia konta użytkownika). Następujący kod należy w powiadomieniu utworzone powyżej:

```csharp
priceObserver = NSNotificationCenter.DefaultCenter.AddObserver (
  InAppPurchaseManager.InAppPurchaseManagerProductsFetchedNotification,
(notification) => {
   // display code goes here, to handle the response from the App Store
   var info = notification.UserInfo;
   if (info.ContainsKey(NSBuy5ProductId)) {
       var product = (SKProduct) info.ObjectForKey(NSBuy5ProductId);
       buy5Button.Enabled = true;
       buy5Title.Text = product.LocalizedTitle;
       buy5Description.Text = product.LocalizedDescription;
       buy5Button.SetTitle("Buy " + product.Price, UIControlState.Normal); // price display should be localized
   }
}
```

Na koniec `ViewWillDisappear` — metoda powinno obserwatora zostaną usunięte:

```csharp
NSNotificationCenter.DefaultCenter.RemoveObserver (priceObserver);
```

#### <a name="skproductrequestdelegate-inapppurchasemanager-methods"></a>Metody SKProductRequestDelegate (InAppPurchaseManager)

`RequestProductData` Metoda jest wywoływana, gdy aplikacja chce pobierać cen produktu i inne informacje. Analizuje kolekcji identyfikatory produktów w poprawnym typie danych następnie tworzy `SKProductsRequest` tych informacji. Wywołanie metody rozpoczęcia powoduje, że żądanie sieciowe do serwerów firmy Apple. Żądania będą uruchamiane asynchronicznie i Wywołaj `ReceivedResponse` metody delegata, gdy została pomyślnie ukończona.

```csharp
public void RequestProductData (List<string> productIds)
{
   var array = new NSString[productIds.Count];
   for (var i = 0; i < productIds.Count; i++) {
       array[i] = new NSString(productIds[i]);
   }
   NSSet productIdentifiers = NSSet.MakeNSObjectSet<NSString>(array);
   productsRequest = new SKProductsRequest(productIdentifiers);
   productsRequest.Delegate = this; // for SKProductsRequestDelegate.ReceivedResponse
   productsRequest.Start();
}
```

iOS automatycznie będzie przekierowywać żądania do wersji "piaskownicy" lub "produkcja" App Store, w zależności od tego, jakie profilu inicjowania obsługi administracyjnej aplikacji został uruchomiony z — tak podczas tworzenia i testowania aplikacji żądania będą mieli dostęp do każdego produktu skonfigurowane w programach iTunes Connect (nawet te, które jeszcze nie zostały przesłane lub zatwierdzone przez firmę Apple). Gdy aplikacja jest w środowisku produkcyjnym, żądania StoreKit zwróci tylko te informacje dotyczące **zatwierdzone** produktów.   
   
   
   
 `ReceivedResponse` Zastąpiona metoda jest wywoływana po serwerów firmy Apple udzielono odpowiedzi z danymi. Ponieważ jest to w tle kod należy analizy danych i umożliwia wysyłanie informacji o produkcie do żadnych ViewControllers, który "nasłuchują" powiadomienie powiadomienie. Poniżej przedstawiono kod służący do zbierania informacji o produkcie prawidłowe i wysyłania powiadomień:

```csharp
public override void ReceivedResponse (SKProductsRequest request, SKProductsResponse response)
{
   SKProduct[] products = response.Products;
   NSDictionary userInfo = null;
   if (products.Length > 0) {
       NSObject[] productIdsArray = new NSObject[response.Products.Length];
       NSObject[] productsArray = new NSObject[response.Products.Length];
       for (int i = 0; i < response.Products.Length; i++) {
           productIdsArray[i] = new NSString(response.Products[i].ProductIdentifier);
           productsArray[i] = response.Products[i];
       }
       userInfo = NSDictionary.FromObjectsAndKeys (productsArray, productIdsArray);
   }
   NSNotificationCenter.DefaultCenter.PostNotificationName (InAppPurchaseManagerProductsFetchedNotification, this, userInfo);
}
```

Chociaż nie jest wyświetlany na diagramie `RequestFailed` metoda powinna zostać zastąpiona również tak, aby można było zapewnić o przesłanie opinii do użytkownika w przypadku serwerów aplikacji sklepu nie są dostępne (lub inny błąd występuje). Przykład kodu jedynie zapisuje do konsoli, ale rzeczywistej aplikacji można zbadać w celu `error.Code` właściwości i wdrożyć niestandardowe zachowanie (na przykład alert dla użytkownika).

```csharp
public override void RequestFailed (SKRequest request, NSError error)
{
   Console.WriteLine (" ** InAppPurchaseManager RequestFailed() " + error.LocalizedDescription);
}
```

Ten zrzut ekranu przedstawia przykładową aplikację natychmiast po ładowania (jeśli jest to produkt informacje są niedostępne):

 [ ![](store-kit-overview-and-retreiving-product-information-images/image24.png "Przykładowa aplikacja natychmiast po ładowania, gdy nie są dostępne żadne informacje produktu")](store-kit-overview-and-retreiving-product-information-images/image24.png)

## <a name="invalid-products"></a>Nieprawidłowy produktów

`SKProductsRequest` Może również zostać zwrócona lista nieprawidłowych identyfikatorów produktu. Nieprawidłowy produktów zazwyczaj są zwracane z jednej z następujących czynności:   
   
   
   
 **Błędnie wpisany identyfikator produktu** — akceptowane są tylko prawidłowe identyfikatory produktu.   
   
 **Produkt nie została zatwierdzona** — podczas testowania, wszystkie produkty, które nie są zaznaczone do sprzedaży ma zostać zwrócony przez `SKProductsRequest`; jednak w środowisku produkcyjnym są zwracane tylko zatwierdzone produktów.   
   
 **Identyfikator aplikacji nie jest jawną** — symbol wieloznaczny identyfikatorów aplikacji (gwiazdką) nie zezwalaj na zakup w aplikacji.   
   
 **Nieprawidłowy profil inicjowania obsługi administracyjnej** — Jeśli wprowadzisz zmiany w konfiguracji aplikacji w portalu inicjowania obsługi administracyjnej (np. włączenie zakupy w aplikacji), pamiętaj, aby ponownie wygenerować i Użyj poprawnego profilu inicjowania obsługi podczas kompilowania aplikacji.   
   
 **kontrakt płatnej aplikacji systemu iOS nie znajduje się w miejscu** — StoreKit funkcje nie będą działać w ogóle, chyba że istnieje prawidłowy kontrakt dla Twojego konta dewelopera firmy Apple.   
   
 **Plik binarny jest w stanie odrzucone** — w przypadku wcześniej przesłane dane binarne w stanie odrzucone (albo przez zespół sklepu z aplikacjami, albo przez dewelopera), a następnie StoreKit funkcje nie będą działać.

`ReceivedResponse` Metody w przykładowym kodzie generuje nieprawidłowy produktów do konsoli:

```csharp
public override void ReceivedResponse (SKProductsRequest request, SKProductsResponse response)
{
   // code removed for clarity
   foreach (string invalidProductId in response.InvalidProducts) {
       Console.WriteLine("Invalid product id: " + invalidProductId );
   }
}
```

## <a name="displaying-localized-prices"></a>Wyświetlanie zlokalizowanych ceny

Ceny warstw Określ określonych ceny dla każdego produktu we wszystkich międzynarodowych sklepów z aplikacjami. Aby zapewnić, ceny są wyświetlane prawidłowo dla każdej waluty, należy użyć następującej metody rozszerzenia (zdefiniowany w `SKProductExtension.cs`) zamiast właściwości ceny każdego `SKProduct`:

```csharp
public static class SKProductExtension {
   public static string LocalizedPrice (this SKProduct product)
   {
       var formatter = new NSNumberFormatter ();
       formatter.FormatterBehavior = NSNumberFormatterBehavior.Version_10_4;  
       formatter.NumberStyle = NSNumberFormatterStyle.Currency;
       formatter.Locale = product.PriceLocale;
       var formattedString = formatter.StringFromNumber(product.Price);
       return formattedString;
   }
}
```

Kod, który ustawia tytuł przycisku używa metody rozszerzenia następująco:

```csharp
string Buy = "Buy {0}"; // or a localizable string
buy5Button.SetTitle(String.Format(Buy, product.LocalizedPrice()), UIControlState.Normal);
```

Za pomocą dwóch różnych iTunes testu konta (po jednej dla Ameryki magazynu) i jeden dla japońskiego magazynu powoduje poniższe zrzuty ekranu:   
   
   
   
 [ ![](store-kit-overview-and-retreiving-product-information-images/image25.png "Konta, przedstawiający języka określone wyniki testowe dwóch różnych iTunes")](store-kit-overview-and-retreiving-product-information-images/image25.png)   
   
   
   
 Zwróć uwagę, że magazyn ma wpływ na język, który służy do produktu informacje i cen waluty, gdy ustawienie języka urządzenia wpływa etykiety i innych zlokalizowanej zawartości.   
   
   
   
 Odwołania, który można użyć różnych magazynu testów konta należy **Wyloguj** w **Ustawienia > iTunes i sklepu z aplikacjami** i ponownie uruchom aplikację na logowanie za pomocą innego konta. Aby zmienić język urządzenia przejdź do **Ustawienia > Ogólne > międzynarodowe > języka**.

