---
title: Transakcje i weryfikacji
ms.topic: article
ms.prod: xamarin
ms.assetid: 84EDD2B9-3FAA-B3C7-F5E8-C1E5645B7C77
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 332ce13b08f3d021553be5747b21b05d62aba6ea
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="transactions-and-verification"></a>Transakcje i weryfikacji

## <a name="restoring-past-transactions"></a>Przywracanie transakcje w przeszłości

Jeśli aplikacja obsługuje typy produktu, które są przystosowane do przywracania, należy uwzględnić niektóre elementy interfejsu użytkownika, aby użytkownicy mogli przywrócić te zakupy.
Ta funkcja umożliwia klienta, aby dodać produktu dodatkowych urządzeń, lub przywrócić produktu do tego samego urządzenia po są czyszczone lub usunięcie i ponowne zainstalowanie aplikacji. Umożliwiająca przywrócenie są następujące typy produktu:

-  Produkty jednoznacznie składnika
-  Automatycznie odnawialnymi subskrypcji
-  Bezpłatnej subskrypcji


Proces przywracania należy zaktualizować rekordy przechowywane na urządzeniu, aby spełnić produktów. Klienta można przywrócić w dowolnym momencie na dowolnym urządzeniu. Proces przywracania wysyła ponownie wszystkich poprzednich transakcji dla tego użytkownika; następnie kod aplikacji musi ustalić, jakie czynności do wykonania tych informacji (na przykład sprawdzenie już istnieje rekord tego zakupu na urządzeniu, a nie tworzenia rekordu zakupu i włączenie produktu dla użytkownika).

### <a name="implementing-restore"></a>Implementowanie przywracania

Interfejs użytkownika **przywrócić** przycisk wywołuje następujące metody, która wyzwala RestoreCompletedTransactions na `SKPaymentQueue`.

```csharp
public void Restore()
{
   // theObserver will be notified of when the restored transactions start arriving <- AppStore
   SKPaymentQueue.DefaultQueue.RestoreCompletedTransactions();
}
```

StoreKit będzie wysyłać żądania przywracania asynchronicznie do serwerów firmy Apple.   
   
   
   
 Ponieważ `CustomPaymentObserver` jest zarejestrowany jako obserwatora transakcji, odbieranie wiadomości gdy odpowiada serwerów firmy Apple. Odpowiedź będzie zawierać wszystkie transakcje, ten użytkownik ma nigdy wykonywane w tej aplikacji (dla wszystkich swoich urządzeń). Kod pętlę każdą transakcję i wykrywa stan przywrócone i wywołania `UpdatedTransactions` metody go przetworzyć, jak pokazano poniżej:

```csharp
// called when the transaction status is updated
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
       case SKPaymentTransactionState.Restored:
           theManager.RestoreTransaction(transaction);
           break;
default:
           break;
       }
   }
}
```

Jeśli brak dostępnych produktów dla użytkownika, `UpdatedTransactions` nie jest wywoływany.   
   
   
   
 Najprostsza możliwa kodu do przywrócenia danej transakcji w próbce jest samo, jak podczas zakupu ma miejsce, z wyjątkiem `OriginalTransaction` właściwość jest używana do uzyskania dostępu identyfikator produktu:

```csharp
public void RestoreTransaction (SKPaymentTransaction transaction)
{
   // Restored Transactions always have an 'original transaction' attached
   var productId = transaction.OriginalTransaction.Payment.ProductIdentifier;
   // Register the purchase, so it is remembered for next time
   PhotoFilterManager.Purchase(productId); // it's as though it was purchased again
   FinishTransaction(transaction, true);
}
```

Bardziej złożone wdrożenia może sprawdzić inne `transaction.OriginalTransaction` właściwości, takie jak oryginalny numer daty i przyjęcia. Te informacje będą przydatne w przypadku niektórych typów produktu (na przykład subskrypcje).

#### <a name="restore-completion"></a>Przywróć uzupełniania

`CustomPaymentObserver` Ma dwie dodatkowe metody, które będą wywoływane StoreKit po ukończeniu procesu przywracania (pomyślnie lub niepowodzeniem), pokazano poniżej:

```csharp
public override void PaymentQueueRestoreCompletedTransactionsFinished (SKPaymentQueue queue)
{
   Console.WriteLine(" ** RESTORE Finished ");
}
public override void RestoreCompletedTransactionsFailedWithError (SKPaymentQueue queue, NSError error)
{
   Console.WriteLine(" ** RESTORE FailedWithError " + error.LocalizedDescription);
}
```

W przykładzie tych metod nie rób nic, jednak w rzeczywistej aplikacji może zadecydować o stosowaniu wiadomość do użytkownika lub niektóre inne funkcje.

## <a name="securing-purchases"></a>Zabezpieczanie zakupów

Dwa przykłady w tym dokumentów użyj `NSUserDefaults` śledzić zakupy:   
   
 **Materiałów** — "saldo" zakupy kredytowej jest prostą `NSUserDefaults` wartość całkowitą, która zwiększa się o każdym zakupu.   
   
 **Inne niż materiałów** — każdy zakup filtru fotografii są przechowywane jako pary klucz wartość w `NSUserDefaults`.

Przy użyciu `NSUserDefaults` zachowuje prosty przykład kodu, ale nie oferuje bardzo bezpieczne rozwiązanie, ponieważ może ona być technicznie stanowiska użytkownicy, można zaktualizować ustawień (pomijanie mechanizmu płatności).   
   
Uwaga: Rzeczywiste aplikacji powinna przyjąć mechanizm bezpiecznego przechowywania zakupionych zawartość, która nie podlega naruszeniu użytkownika. Może to obejmować szyfrowania i/lub innych technik, włącznie z uwierzytelnianiem serwera zdalnego.   
   
 Ten mechanizm powinien również zaprojektowana tak, aby skorzystać z wbudowanych funkcji tworzenia kopii zapasowych i odzyskiwania systemu iOS, iTunes i iCloud. Daje to pewność, że po użytkownika przywracania kopii zapasowej poprzedniego zakupów będą dostępne natychmiast.   
   
   
 Guide można znaleźć firmy Apple Secure kodowania dodatkowe wskazówki specyficzne dla systemu iOS.

## <a name="receipt-verification-and-server-delivered-products"></a>Odbieranie weryfikacji i produktów dostarczonych przez serwer

Przykłady w tym dokumencie wykonanej do tej pory ma składa się wyłącznie z aplikacji komunikują się bezpośrednio z serwerami sklepu z aplikacjami do przeprowadzania transakcji zakupu, które Odblokuj funkcje lub możliwości już zakodowanej w aplikacji.   
   
   
   
 Apple zapewnia dodatkowy poziom zabezpieczeń zakupu, zezwalając potwierdzenia zakupu niezależnie należy zweryfikować przez inny serwer, który może służyć do sprawdzania poprawności żądania przed dostarczeniem zawartości cyfrowej w ramach zakupu (takie jak cyfrowe książki lub Magazine).   
   
   
   
 **Wbudowane produktów** — podobnie jak w przykładach zawartych w tym dokumencie, produktu zostały zakupione istnieje jako funkcje, które zostały wydane z aplikacją. Funkcja zakupu w aplikacji umożliwia użytkownikowi dostęp do funkcji.
Identyfikatory produktu są zapisane na stałe.   
   
 **Serwer dostarczyć produktów** — obejmuje produkt do pobrania zawartości, który jest przechowywany na serwerze zdalnym do momentu pomyślnego transakcji powoduje pobranie zawartości.
Przykłady mogą obejmować książki lub magazine problemy. Identyfikatory produktu zazwyczaj pochodzą z zewnętrznego serwera (gdzie produktu hostowana jest zawartość również). Aplikacje, muszą implementować niezawodny sposób rejestrowania po zakończeniu transakcji, tak aby zawartość Pobierz kończy się niepowodzeniem może być ponowne próby bez naruszania użytkownika.

### <a name="server-delivered-products"></a>Produkty dostarczane przez serwer

Niektóre produktu elementu zawartości, takie jak książki i magazyny (lub nawet poziomie meczu), muszą zostać pobrane z serwera zdalnego podczas procesu zakupu. Oznacza to, że dodatkowy serwer jest wymagana do przechowywania i dostarczania zawartości produktu, po jego zakupu.

#### <a name="getting-prices-for-server-delivered-products"></a>Pobieranie ceny produktów dostarczonych przez serwer

Ponieważ te produkty zdalnie są dostarczane, również jest można dodać więcej produktów wraz z upływem czasu (bez aktualizowania kodu aplikacji), takie jak dodanie więcej książki lub nowe problemy magazynu. Tak, aby aplikację można wykryć tych produktów wiadomości i wyświetlić je użytkownikowi, dodatkowy serwer należy przechowywać i dostarczyć te informacje.   
   
   
   
 [ ![](transactions-and-verification-images/image38.png "Pobieranie ceny produktów dostarczonych przez serwer")](transactions-and-verification-images/image38.png)   
   
   
   
 1. Informacje o produkcie muszą być przechowywane w wielu miejscach: na serwerze i w iTunes Connect. Ponadto każdy produkt ma skojarzone pliki zawartości. Te pliki będą dostarczane po pomyślnym zakupu.   
   
 2. Gdy użytkownik żąda kupić produktu, aplikacja musi ustalić, jakie produkty są dostępne. Te informacje mogą być buforowane, ale ma zostać dostarczony z serwera zdalnego przechowywania głównego wykazu produktów.   
   
 3. Serwer zwraca listę identyfikatorów produktu dla aplikacji do analizy.   
   
 4. Następnie aplikacja określa, która z tych identyfikatorów produktu do wysłania do StoreKit można pobrać cen wraz z opisami.   
   
 5. StoreKit wysyła listę identyfikatorów produktu do serwerów firmy Apple.   
   
 6. Serwery programu iTunes odpowiadać, podając informacje prawidłowego produktu (opis i bieżąca cena).   
   
 7. Aplikacji `SKProductsRequestDelegate` jest przekazywany informacji o produkcie do wyświetlenia dla użytkownika.

#### <a name="purchasing-server-delivered-products"></a>Zakup produktów dostarczonych przez serwer

Ponieważ serwer zdalny wymaga sposób sprawdzania, czy żądanie zawartości jest prawidłowa (tj. zostały uregulowaniu płatności), informacje o potwierdzeniach jest przekazywane do uwierzytelniania. Serwer zdalny przesyła dane do programu iTunes do weryfikacji i w razie powodzenia jest przechowywana zawartość produktu w odpowiedzi do aplikacji.   
   
 [ ![](transactions-and-verification-images/image39.png "Zakup produktów dostarczonych przez serwer")](transactions-and-verification-images/image39.png)   
   
 1. Dodaje aplikację `SKPayment` do kolejki. W razie potrzeby użytkownika zostanie wyświetlony monit o ich identyfikator Apple ID, a monit o potwierdzenie płatności.   
   
 2. StoreKit wysyła żądanie do serwera w celu przetworzenia.   
   
 3. Po zakończeniu transakcji serwer odpowiada, przyjęcie transakcji.   
   
 4. `SKPaymentTransactionObserver` Podklasy odbieranie odbiera i przetwarza je. Ponieważ produkt, należy pobrać z serwera, aplikacja inicjuje żądanie sieciowe z serwerem zdalnym.   
   
 5. Żądanie pobrania towarzyszy odbieranie danych tak, aby serwer zdalny sprawdzić, czy jest autoryzowany do uzyskania dostępu do zawartości. Klient sieci aplikacji czeka na odpowiedź na to żądanie.   
   
 6. Kiedy serwer odbiera żądania zawartości, analizuje dane przyjęcia i wysyła żądanie bezpośrednio do serwerów iTunes Sprawdź odbieranie dotyczy prawidłową transakcję. Serwer powinien używać logikę do ustalenia, czy można wysłać żądania do adresu URL środowisko produkcyjne lub piaskownicy. Apple sugeruje zawsze przy użyciu adresu URL produkcyjnych i przełączanie do piaskownicy, jeśli stan: 21007 (przyjęcie piaskownicy wysyłane do serwera produkcyjnego) — zobacz [techniczne 2259 Uwaga — często zadawane pytania nr 16](https://developer.apple.com/library/ios/#technotes/tn2259/_index.html).   
   
 7. iTunes będzie Sprawdź odbieranie i zwrócenia stanu o wartości zero, jeśli jest ona prawidłowa.   
   
 8. Serwer czeka na odpowiedź iTunes. Jeśli odbierze prawidłowej odpowiedzi kod powinien zlokalizuj plik zawartości skojarzone produktu do uwzględnienia w odpowiedzi do aplikacji.   
   
 9. Aplikacja odbiera i analizuje odpowiedzi, zapisywanie zawartości produktu urządzenia w systemie plików.   
   
 10. Aplikacja umożliwia produktu, a następnie wywołuje w StoreKit `FinishTransaction`. Następnie aplikacji może być opcjonalnie wyświetlany zakupionych zawartości (na przykład strona Pokaż pierwsze zakupionych książki lub problem magazine).

Alternatywną implementację produktu bardzo dużych plików zawartości mogą obsługiwać między innymi, po prostu przechowywania odbieranie transakcji w kroku #9, dzięki czemu można szybko ukończyć transakcji i udostępnia interfejs użytkownika umożliwiający użytkownikom na pobieranie zawartości rzeczywistego produktu w późniejszym czasie. Żądanie pobrania kolejnych ponownie wysłać przechowywanych potwierdzenia w celu dostępu do zawartości pliku wymaganego produktu.

### <a name="writing-server-side-receipt-verification-code"></a>Pisanie kodu weryfikacyjnego otrzymania po stronie serwera

Sprawdzanie poprawności przyjęcia w kodzie po stronie serwera można zrobić za pomocą prostego POST protokołu HTTP żądania/odpowiedzi, która obejmuje kroki #5 – &#8; w diagram przepływu pracy.   
   
   
   
 Wyodrębnij `SKPaymentTansaction.TransactionReceipt` właściwości w aplikacji. To są dane, który musi być wysyłana do iTunes weryfikacji (krok #5).

Kodowanie Base64 dane przyjęcia transakcji (albo w kroku #5 lub #6).

Tworzenie prostego ładunek JSON następująco:

```csharp
{
   "receipt-data" : "(base-64 encoded receipt here)"
}
```

HTTP POST ciąg JSON do [https://buy.itunes.apple.com/verifyReceipt](https://buy.itunes.apple.com/verifyReceipt) w środowisku produkcyjnym lub [https://sandbox.itunes.apple.com/verifyReceipt](https://sandbox.itunes.apple.com/verifyReceipt) do testowania.   
   
 Odpowiedź JSON zawiera następujące klucze:

```csharp
{
   "status" : 0,
   "receipt" : { (receipt repeated here) }
}
```

Stan o wartości zero wskazuje prawidłowy przyjęcia. Serwer można przystąpić do zrealizowania zawartości zakupionych produktów. Klucz otrzymania zawiera słownik JSON z tymi samymi właściwościami co `SKPaymentTransaction` obiekt, który została odebrana w aplikacji, aby kod serwera można zbadać tego słownika można pobrać informacji o takich jak product_id i ilość zakupu.

Zobacz firmy Apple [Sprawdzanie potwierdzenia magazynu](https://developer.apple.com/library/ios/#documentation/NetworkingInternet/Conceptual/StoreKitGuide/VerifyingStoreReceipts/VerifyingStoreReceipts.html#//apple_ref/doc/uid/TP40008267-CH104-SW1) dokumentacji, aby uzyskać dodatkowe informacje.

#### <a name="using-aspnet"></a>Użycie programu ASP.NET

Dla deweloperów języka C# jest przydatne projekt open source o nazwie [APNS ostrych](https://github.com/Redth/APNS-Sharp) otrzymania kod weryfikacyjny, który działa w tym w programie ASP.NET. W szczególności `Receipt.cs` i `ReceiptVerification.cs` plików [ `Jdsoft.Apple.AppStore` ](https://github.com/Redth/APNS-Sharp/tree/master/JdSoft.Apple.AppStore) katalogu mogą być dodawane do witryny sieci Web .NET łatwe Implementowanie kroki #6 do #8 z produktów Server-Delivered diagram przepływu pracy.   
   
   
   
 Komunikacja z serwerami programu iTunes używa formatu JSON, który jest łatwy do przetworzenia w języku C#.   
   
   
   
 Odwoływać się do zakupu w aplikacji firmy Apple [weryfikacji otrzymania](https://developer.apple.com/library/ios/#releasenotes/StoreKit/IAP_ReceiptValidation/_index.html) dokumentacji, aby uzyskać szczegółowe informacje na temat sposobu Sprawdź poprawność potwierdzenia.

### <a name="3rd-party-receipt-verification"></a>Weryfikacja firm otrzymania 3

Istnieją firmy, które oferują platformy w przypadku otrzymania weryfikacji (i innymi), które można użyć zamiast tworzenia własnego serwera. Xamarin nie potwierdza autentyczności tych usług; po prostu są one wymienione w tym miejscu odwołania.

#### <a name="urban-airship"></a>Zurbanizowane Airship

Zurbanizowane Airship zawiera szereg różnych usług zaplecza dla aplikacji systemu iOS, w tym przyjęcia weryfikacji i powiadomieniami wypychanymi.   
   
 [http://urbanairship.com/products/in-app-purchase/](http://urbanairship.com/products/in-app-purchase/)



