---
title: "Zakup eksploatacyjny produktów"
ms.topic: article
ms.prod: xamarin
ms.assetid: E0CB4A0F-C3FA-3933-58A7-13246971D677
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 7366a4ce5cbb6a3026a7445a03f03b45d89d9210
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="purchasing-consumable-products"></a>Zakup eksploatacyjny produktów

Produkty eksploatacyjny są łatwo zaimplementować, ponieważ nie jest wymagane "Przywracanie". Są one przydatne dla produktów, takich jak waluty w grze lub jednorazowy część funkcji. Użytkownicy ponownie kupić ponownie eksploatacyjny produktów over i — w tryb failover.

## <a name="built-in-product-delivery"></a>Dostarczenie wbudowany produktu

Przykładowy kod towarzyszące tym dokumencie przedstawiono wbudowane produktów — identyfikatory produktów są zapisane na stałe do aplikacji, ponieważ są bezpośrednio powiązane do kodu, który "odblokowuje" funkcję po płatności. Proces zakupu można wizualizowane następująco:   
   
[ ![Wizualizacja procesu zakupu](purchasing-consumable-products-images/image26.png)](purchasing-consumable-products-images/image26.png)     
   
 Podstawowy przepływ pracy jest:   
   
   
   
 1. Dodaje aplikację `SKPayment` do kolejki. W razie potrzeby użytkownika zostanie wyświetlony monit o ich identyfikator Apple ID, a monit o potwierdzenie płatności.   
   
 2. StoreKit wysyła żądanie do serwera w celu przetworzenia.   
   
 3. Po zakończeniu transakcji serwer odpowiada, przyjęcie transakcji.   
   
 4. `SKPaymentTransactionObserver` Podklasy odbieranie odbiera i przetwarza je.   
   
 5. Aplikacja umożliwia produktu (aktualizując `NSUserDefaults` lub innych mechanizmu), a następnie wywołuje w StoreKit `FinishTransaction`.

Istnieje inny typ przepływu pracy — *produktów Server-Delivered* — to znaczy omówiony w dalszej części dokumentu (zobacz sekcję *otrzymania weryfikacji i produktów Server-Delivered*).

## <a name="consumable-products-example"></a>Generowanie przykład produktów

[Kod InAppPurchaseSample](https://developer.xamarin.com/samples/monotouch/StoreKit/) zawiera projekt o nazwie *materiałów* implementującej basic "Waluta w grze" (zwane "małp środków"). Przykład obejmuje implementowania dwa produkty zakupu w aplikacji, aby zezwolić użytkownikowi kupić jako wiele "małp środków" jak życzą — w rzeczywistej aplikacji byłoby również sposób ich wydatków!   
   
   
   
 Aplikacja jest wyświetlany w obszarze te zrzuty ekranu — każdy zakup dodaje więcej "małp środków" równowagi użytkownika:   
   
   
   
 [ ![Każdy zakup dodaje więcej środków małp równowagi użytkowników](purchasing-consumable-products-images/image27.png)](purchasing-consumable-products-images/image27.png)   
   
   
   
 Interakcje między klas niestandardowych StoreKit i sklepu wyglądać następująco:   
   
   
   
 [ ![Interakcje między klas niestandardowych StoreKit i sklepu z aplikacjami](purchasing-consumable-products-images/image28.png)](purchasing-consumable-products-images/image28.png)

&nbsp;

### <a name="viewcontroller-methods"></a>Metody ViewController

Oprócz tych właściwości i metody wymagane do pobrania informacji o produkcie kontroler widoku wymaga dodatkowych powiadomień obserwatorów do nasłuchiwania na powiadomienia związane z zakupu. Są to po prostu `NSObjects` który zostanie zarejestrowane i usunięte w `ViewWillAppear` i `ViewWillDisappear` odpowiednio.

```csharp
NSObject succeededObserver, failedObserver;
```

Konstruktor spowoduje również utworzenie `SKProductsRequestDelegate` podklasy ( `InAppPurchaseManager`) który z kolei tworzy i rejestruje `SKPaymentTransactionObserver` ( `CustomPaymentObserver`).   
   
   
   
 Pierwsza część przetwarzania transakcji zakupu w aplikacji ma obsługiwać naciśnięcie przycisku, gdy użytkownik żąda kupić, jak pokazano w poniższym kodzie z przykładowej aplikacji:

```csharp
buy5Button.TouchUpInside += (sender, e) => {
   iap.PurchaseProduct (Buy5ProductId);
};
buy10Button.TouchUpInside += (sender, e) => {
   iap.PurchaseProduct (Buy10ProductId);
};
```

   
   
 Druga część interfejsu użytkownika jest obsługa powiadomienia że transakcja zakończyła się pomyślnie, w tym przypadku aktualizując wyświetlanego salda:

```csharp
priceObserver = NSNotificationCenter.DefaultCenter.AddObserver (InAppPurchaseManager.InAppPurchaseManagerTransactionSucceededNotification,
(notification) => {
   balanceLabel.Text = CreditManager.Balance() + " monkey credits";
});
```

Ostatnia część interfejsu użytkownika jest wyświetlenie komunikatu, jeśli transakcja została anulowana z jakiegoś powodu. W przykładowym kodzie komunikat po prostu są zapisywane w oknie danych wyjściowych:

```csharp
failedObserver = NSNotificationCenter.DefaultCenter.AddObserver (InAppPurchaseManager.InAppPurchaseManagerTransactionFailedNotification,
(notification) => {
   Console.WriteLine ("Transaction Failed");
});
```

Oprócz tych metod kontrolera widoku transakcji zakupu eksploatacyjny produktu wymaga również kod na `SKProductsRequestDelegate` i `SKPaymentTransactionObserver`.

### <a name="inapppurchasemanager-methods"></a>Metody InAppPurchaseManager

Implementuje kod przykładowy liczba zakupu odpowiednich metod w klasie InAppPurchaseManager tym `PurchaseProduct` metodę, która tworzy `SKPayment` wystąpienia i dodaje go do kolejki w celu przetworzenia:

```csharp
public void PurchaseProduct(string appStoreProductId)
{
   SKPayment payment = SKPayment.PaymentWithProduct (appStoreProductId);
   SKPaymentQueue.DefaultQueue.AddPayment (payment);
}
```

Dodawanie płatność do kolejki jest operacja asynchroniczna. Aplikacja odzyskał sterowania podczas StoreKit przetwarza transakcji i przesyła je do serwerów firmy Apple. Jest to w tym momencie zweryfikuje iOS, że użytkownik jest zalogowany do sklepu z aplikacjami i monit o jej podanie identyfikatora Apple ID i hasła w razie potrzeby.   
   
   
   
 Zakładając, że użytkownik pomyślnie jest uwierzytelniany w usłudze App Store i zgadza się z transakcją `SKPaymentTransactionObserver` będzie odbierać StoreKit w odpowiedzi i Wywołaj następującą metodę do zrealizowania transakcji i Zakończ ją.

```csharp
public void CompleteTransaction (SKPaymentTransaction transaction)
{
   var productId = transaction.Payment.ProductIdentifier;
   // Register the purchase, so it is remembered for next time
   PhotoFilterManager.Purchase(productId);
   FinishTransaction(transaction, true);
}
```

Ostatnim krokiem jest zapewnienie powiadomić StoreKit, pomyślnie spełnione transakcji, wywołując `FinishTransaction`:

```csharp
public void FinishTransaction(SKPaymentTransaction transaction, bool wasSuccessful)
{
   // remove the transaction from the payment queue.
   SKPaymentQueue.DefaultQueue.FinishTransaction(transaction);  // THIS IS IMPORTANT - LET'S APPLE KNOW WE'RE DONE !!!!
   using (var pool = new NSAutoreleasePool()) {
       NSDictionary userInfo = NSDictionary.FromObjectsAndKeys(new NSObject[] {transaction},new NSObject[] {new NSString("transaction")});
       if (wasSuccessful) {
           // send out a notification that we've finished the transaction
           NSNotificationCenter.DefaultCenter.PostNotificationName (InAppPurchaseManagerTransactionSucceededNotification, this, userInfo);
       } else {
           // send out a notification for the failed transaction
           NSNotificationCenter.DefaultCenter.PostNotificationName (InAppPurchaseManagerTransactionFailedNotification, this, userInfo);
       }
   }
}
```

Gdy produkt jest dostarczany, `SKPaymentQueue.DefaultQueue.FinishTransaction` musi być wywołana w celu usunięcia transakcji z kolejki płatności.

### <a name="skpaymenttransactionobserver-custompaymentobserver-methods"></a>Metody SKPaymentTransactionObserver (CustomPaymentObserver)

Wywołania StoreKit `UpdatedTransactions` metody, gdy odbiera odpowiedź z serwerami firmy Apple i przekazuje tablicę `SKPaymentTransaction` obiekty kod do sprawdzenia. Metoda w pętli każdą transakcję i wykonuje innej funkcji, w zależności od stanu transakcji (jak pokazano poniżej):

```csharp
public override void UpdatedTransactions (SKPaymentQueue queue, SKPaymentTransaction[] transactions)
{
   foreach (SKPaymentTransaction transaction in transactions)
   {
       switch (transaction.TransactionState)
       {
           case SKPaymentTransactionState.Purchased:
              theManager.CompleteTransaction(transaction);
               break;
           case SKPaymentTransactionState.Failed:
              theManager.FailedTransaction(transaction);
               break;
           default:
               break;
       }
   }
}
```

`CompleteTransaction` Metoda została opisanych wcześniej w tej sekcji — zapisuje szczegóły zakupu, aby `NSUserDefaults`, kończenie znajdujących się w transakcji z StoreKit i na koniec powiadamia interfejsu użytkownika do zaktualizowania.

### <a name="purchasing-multiple-products"></a>Zakup wielu produktów

Jeśli to ma sens w aplikacji, aby kupić wielu produktów, użyj `SKMutablePayment` klasy i ustaw dla pola Ilość:

```csharp
public void PurchaseProduct(string appStoreProductId)
{
   SKMutablePayment payment = SKMutablePayment.PaymentWithProduct (appStoreProductId);
   payment.Quantity = 4; // hardcoded as an example
   SKPaymentQueue.DefaultQueue.AddPayment (payment);
}
```

Kod obsługi zakończonej transakcji musi także zbadać właściwości ilość do zrealizowania poprawnie zakupu:

```csharp
public void CompleteTransaction (SKPaymentTransaction transaction)
{
   var productId = transaction.Payment.ProductIdentifier;
   var qty = transaction.Payment.Quantity;
   if (productId == ConsumableViewController.Buy5ProductId)
       CreditManager.Add(5 * qty);
   else if (productId == ConsumableViewController.Buy10ProductId)
       CreditManager.Add(10 * qty);
   else
       Console.WriteLine ("Shouldn't happen, there are only two products");
   FinishTransaction(transaction, true);
}
```

Gdy użytkownik zakupi wielu ilości, alert potwierdzenia StoreKit będzie odzwierciedlać ilość i cenie jednostkowej całkowita cen, który one będą naliczane opłaty, jak pokazano na poniższym zrzucie ekranu:

[ ![Potwierdzenie zamówienia zakupu](purchasing-consumable-products-images/image30.png)](purchasing-consumable-products-images/image30.png)

## <a name="handling-network-outages"></a>Obsługa awarii sieci

Zakupy w aplikacji wymagają działającego połączenia sieciowego dla StoreKit do komunikacji z serwerami firmy Apple. Jeśli połączenie sieciowe jest niedostępne, następnie zakupu w aplikacji będzie niedostępna.

### <a name="product-requests"></a>Żądania produktu

Jeśli sieć jest niedostępna podczas wprowadzania `SKProductRequest`, `RequestFailed` metody `SKProductsRequestDelegate` podklasy ( `InAppPurchaseManager`) zostanie wywołana, jak pokazano poniżej:

```csharp
public override void RequestFailed (SKRequest request, NSError error)
{
   using (var pool = new NSAutoreleasePool()) {
       NSDictionary userInfo = NSDictionary.FromObjectsAndKeys(new NSObject[] {error},new NSObject[] {new NSString("error")});
       // send out a notification for the failed transaction
       NSNotificationCenter.DefaultCenter.PostNotificationName (InAppPurchaseManagerRequestFailedNotification, this, userInfo);
   }
}
```

Następnie ViewController nasłuchuje powiadomienia i wyświetla komunikat w przyciski zakupu:

```csharp
requestObserver = NSNotificationCenter.DefaultCenter.AddObserver (InAppPurchaseManager.InAppPurchaseManagerRequestFailedNotification,
(notification) => {
   Console.WriteLine ("Request Failed");
   buy5Button.SetTitle ("Network down?", UIControlState.Disabled);
   buy10Button.SetTitle ("Network down?", UIControlState.Disabled);
});
```

Ponieważ połączenie sieciowe mogą być przejściowy na urządzeniach przenośnych, aplikacje mogą chcesz monitorować stan sieci przy użyciu platformy SystemConfiguration, a następnie spróbuj ponownie, gdy połączenie sieciowe jest dostępne. Odwoływać się do firmy Apple lub się, że używa jej.

### <a name="purchase-transactions"></a>Transakcje zakupu

Kolejki płatności StoreKit będą przechowywane i do przodu zakupu żądań w miarę możliwości, gdy sieć nie powiodło się podczas procesu zakupu wpływ awaria sieci będą się różnić w zależności od.   
   
   
   
 Jeśli błąd wystąpi podczas transakcji, `SKPaymentTransactionObserver` podklasy ( `CustomPaymentObserver`) będzie miał `UpdatedTransactions` wywołano metodę i `SKPaymentTransaction` klasy będzie w stanie niepowodzenia.

```csharp
public override void UpdatedTransactions (SKPaymentQueue queue, SKPaymentTransaction[] transactions)
{
   foreach (SKPaymentTransaction transaction in transactions)
   {
       switch (transaction.TransactionState)
       {
           case SKPaymentTransactionState.Purchased:
               theManager.CompleteTransaction(transaction);
               break;
           case SKPaymentTransactionState.Failed:
               theManager.FailedTransaction(transaction);
               break;
           default:
               break;
       }
   }
}
```

`FailedTransaction` Metody wykrywa, czy wystąpił błąd z powodu anulowania użytkownika, jak pokazano poniżej:

```csharp
public void FailedTransaction (SKPaymentTransaction transaction)
{
   //SKErrorPaymentCancelled == 2
   if (transaction.Error.Code == 2) // user cancelled
       Console.WriteLine("User CANCELLED FailedTransaction Code=" + transaction.Error.Code + " " + transaction.Error.LocalizedDescription);
   else // error!
       Console.WriteLine("FailedTransaction Code=" + transaction.Error.Code + " " + transaction.Error.LocalizedDescription);
   FinishTransaction(transaction,false);
}
```

Nawet jeśli transakcja się nie powiedzie, `FinishTransaction` można wywołać metody, aby usunąć z kolejki płatności transakcji:

```csharp
SKPaymentQueue.DefaultQueue.FinishTransaction(transaction);
```

Przykładowy kod następnie wysyła powiadomienie, aby ViewController można wyświetlić wiadomość. Aplikacje nie powinny wskazywać dodatkowych komunikatów, jeśli transakcja została anulowana przez użytkownika. Inne kody błędów, które mogą wystąpić następujące:

```csharp
FailedTransaction Code=0 Cannot connect to iTunes Store
FailedTransaction Code=5002 An unknown error has occurred
FailedTransaction Code=5020 Forget Your Password?
Applications may detect and respond to specific error codes, or handle them in the same way.
```

## <a name="handling-restrictions"></a>Ograniczenia dotyczące obsługi

**Ustawienia > Ogólne > ograniczenia** funkcji systemu IOS umożliwia zablokowanie niektórych funkcji urządzenia.   
   
   
   
 Można zbadać, czy użytkownik może dokonywanie zakupów w aplikacji za pośrednictwem `SKPaymentQueue.CanMakePayments` metody. Jeśli zostanie zwrócona wartość false następnie użytkownik nie może uzyskać dostępu do zakupu w aplikacji. Jeśli nastąpiła zakupu StoreKit automatycznie wyświetli komunikat o błędzie dla użytkownika. Weryfikując wartość tego aplikacji zamiast tego można ukryć przyciski zakupu lub wykonać niektóre akcje, aby pomóc użytkownikowi.   
   
   
   
 W `InAppPurchaseManager.cs` pliku `CanMakePayments` metoda opakowuje funkcję StoreKit następująco:

```csharp
public bool CanMakePayments()
{
   return SKPaymentQueue.CanMakePayments;
}
```

Aby przetestować tę metodę, należy użyć **ograniczenia** funkcji systemu IOS, aby wyłączyć **zakupy w aplikacji**:   
   
   
   
 [ ![Funkcja ograniczenia systemu IOS, aby wyłączyć zakupy w aplikacji](purchasing-consumable-products-images/image31.png)](purchasing-consumable-products-images/image31.png)   
   
   
   
 Ten przykładowy kod z `ConsumableViewController` reaguje na `CanMakePayments` zwrócenie wartości false, wyświetlając **wyłączone sklepu AppStore** tekst na przyciskach wyłączone.

```csharp
// only if we can make payments, request the prices
if (iap.CanMakePayments()) {
   // now go get prices, if we don't have them already
   if (!pricesLoaded)
       iap.RequestProductData(products); // async request via StoreKit -> App Store
} else {
   // can't make payments (purchases turned off in Settings?)
   // the buttons are disabled by default, and only enabled when prices are retrieved
   buy5Button.SetTitle ("AppStore disabled", UIControlState.Disabled);
   buy10Button.SetTitle ("AppStore disabled", UIControlState.Disabled);
}
```

Aplikacja wygląda podczas **zakupy w aplikacji** funkcji jest ograniczony — przyciski zakupu są wyłączone.   
   
   
   
 [ ![Aplikacja wygląda na to, gdy zakupy w aplikacji, funkcja jest ograniczone zakupu, które przyciski są wyłączone](purchasing-consumable-products-images/image32.png)](purchasing-consumable-products-images/image32.png)   
   
   
   

Informacje o produkcie może nadal być żądane, gdy `CanMakePayments` jest false, więc aplikacja nadal może pobrać i wyświetlić ceny. Oznacza to, jeśli firma Microsoft usunęła `CanMakePayments` wyboru z kodu nadal przycisków zakupu jest aktywne, ale próba zakupu użytkownik zostanie wyświetlony komunikat który **zakupy w aplikacji nie są dozwolone** (generowane przez StoreKit gdy kolejka płatności jest dostępny):   
   
   
   
 [ ![Zakupy w aplikacji nie są dozwolone.](purchasing-consumable-products-images/image33.png)](purchasing-consumable-products-images/image33.png)   
   
   
   
 Praktyczne aplikacje może mieć różne podejścia do obsługi ograniczenia, takie jak całkowicie ukrywanie przycisków i prawdopodobnie oferty bardziej szczegółowy komunikat niż alertu, który StoreKit pokazuje automatycznie.

