---
title: Subskrypcje i raportowanie
ms.prod: xamarin
ms.assetid: 27EE4234-07F5-D2CD-DC1C-86E27C20141E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 7ba47e8f0ec114845c14269e81bb7f078a5d4936
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="subscriptions-and-reporting"></a>Subskrypcje i raportowanie

## <a name="about-non-renewing-subscriptions"></a>O innych niż — odnawianiu subskrypcji

Odnawianie inne niż subskrypcje są przeznaczone dla produktów, które reprezentują sprzedaży usługi z ograniczeniem czasowym, takich jak (jeden tydzień dostęp do aplikacji nawigacji) lub czas dostępu do archiwum danych.   
   
   
   
 Podstawowe różnice między-odnawiania subskrypcji i innych typów produktu:

-  Definicja produktu w iTunes Connect nie obejmuje określenie. Kod aplikacji musi mieć możliwość wnioskować okres ważności z identyfikatora produktu. 
-  Można ich zakupić wielokrotnie (na przykład eksploatacyjny produktu). Aplikacje muszą zarządzać subskrypcji termin/wygaśnięcia i odnawiania i uniemożliwić użytkownikowi kupować subskrypcje nakładające się. 
-  Zakupów nie są obsługiwane przez funkcję StoreKit przywracania. Jeśli subskrypcja powinny być dostępne we wszystkich urządzeń użytkownika, aplikacja będzie ma do projektowania i zaimplementować tę funkcję w połączeniu z serwerem zdalnym. Aplikacje są również odpowiedzialne za kopia zapasowa stanu subskrypcji w przypadkach, gdy urządzenie zostanie utworzona kopia zapasowa następnie przywrócone z-kopii zapasowych. 
-  Przegląd wdrożenia
-  Subskrypcje odnawianie inne niż zwykle powinny zostać wdrożone, za pomocą przepływu pracy dostarczane serwera i zarządzane jak eksploatacyjny produktów. 


## <a name="about-free-subscriptions"></a>Temat bezpłatnej subskrypcji

Subskrypcji bezpłatnych umożliwiają deweloperom umieszczenia wolnego zawartości w aplikacjach Newsstand (nie można ich używać w aplikacjach innych niż Newsstand). Po uruchomieniu bezpłatna subskrypcja będzie dostępny dla wszystkich urządzeń użytkownika. Nigdy nie wygasa subskrypcji bezpłatnych; końcowy one tylko po odinstalowaniu aplikacji.

### <a name="implementation-overview"></a>Przegląd wdrożenia

Subskrypcji bezpłatnych znacznie przypominają odnawialnymi automatycznie subskrypcji. Aplikacja musi mieć "zakupić" produkt bezpłatnej subskrypcji w iTunes Connect. Przy zakupie przez użytkownika, zakupu bezpłatnej subskrypcji powinny być weryfikowane jak produktu odnawialnymi automatycznie subskrypcji. Bezpłatna subskrypcja transakcji można przywrócić.


## <a name="about-auto-renewable-subscriptions"></a>Temat odnawialnymi automatycznie subskrypcji

Automatycznie odnawialnymi subskrypcje są używane głównie w aplikacjach Newsstand. Reprezentują produkt, który daje użytkownikowi dostęp do zawartości dynamicznej dla danego okresu czasu, który jest skonfigurowany w programach iTunes Connect (Ustaw okresy od 7 dni do 1 rok). Subskrypcje odnowić automatycznie, ładowania identyfikatora Apple ID po zakończeniu każdego okresu subskrypcji, chyba że użytkownik zdecyduje się na wychodzący. Ten typ produktu dobrze się sprawdza magazyn lub wiadomości subskrypcji, gdy użytkownik uzyskuje dostęp do poszczególnych problemów opublikowane podczas ich subskrypcja jest prawidłowa.

### <a name="implementation-overview"></a>Przegląd wdrożenia

Subskrypcje odnawialnymi automatycznie powinny być implementowane za pomocą przepływu pracy produktów Server-Delivered (odwoływać się do *otrzymania weryfikacji i produktów Server-Delivered* sekcji).

#### <a name="shared-secret"></a>Wspólny klucz tajny

W aplikacji zakupu wspólny klucz tajny, należy użyć w żądania JSON podczas weryfikowania subskrypcji odnawialnymi automatycznie na serwerze. Wspólny klucz tajny jest utworzone/dostęp za pomocą programu iTunes Connect.

W sklepie iTunes wybierz stronę główną Connect **Moje aplikacje**:   
   
 [![](subscriptions-and-reporting-images/image2.png "Wybierz Moje aplikacje")](subscriptions-and-reporting-images/image2.png#lightbox)  
 
Wybierz aplikację, a następnie wybierz polecenie **zakupy w aplikacji** karty:

[![](subscriptions-and-reporting-images/image6.png "Kliknij kartę Zakupy w aplikacji")](subscriptions-and-reporting-images/image6.png#lightbox)

W dolnej części strony, wybierz **widoku lub wygenerować wspólny klucz tajny**:
   
 [![](subscriptions-and-reporting-images/image40.png "Wybierz widok lub wygenerować wspólny klucz tajny")](subscriptions-and-reporting-images/image40.png#lightbox)

 [![](subscriptions-and-reporting-images/image41.png "Generowanie wspólny klucz tajny")](subscriptions-and-reporting-images/image41.png#lightbox)   
   
   
   
 Aby użyć wspólny klucz tajny, należy uwzględnić go w ładunku JSON, który są wysyłane do serwerów firmy Apple, podczas sprawdzania poprawności potwierdzenia zakupu w aplikacji dla automatycznie odnawialnymi subskrypcji, jak to:

```csharp
{
   "receipt-data" : "(receipt bytes here)",
   "password"     : "(shared secret bytes here)"
}
```

Pole stanu odpowiedzi będzie mieć wartość zero, jeśli zakup jest nieprawidłowa w przypadku innych typów produktu.

#### <a name="downloading-items-after-the-initial-subscription-term"></a>Pobieranie elementów po początkowej subskrypcji

W ramach dostarczanie subskrypcji produktów kod często należy sprawdzić najnowszych odbieranie znane względem serwerów firmy Apple. Jeśli subskrypcja ma automatycznie — odnowiony od momentu ostatniej weryfikacji, odpowiedź JSON zawiera dodatkowe pola, które powiadamiają o aplikacji transakcji, która nastąpiła (który powinien być rozszerzany ważności subskrypcji). Odpowiedź JSON zawiera:

```csharp
{
   "status" : 0,
   "receipt" : { (receipt here) },
   "latest_receipt" : "(base-64 encoded receipt here)",
   "latest_receipt_info" : { (latest receipt info here) }
}
```

Jeśli stan jest równy zero subskrypcja jest nadal ważny, a pozostałe pola zawiera prawidłowe dane. Jeśli stan jest 21006 subskrypcja wygasła. Zobacz [weryfikowanie odnawialnymi automatyczne przyjęcie subskrypcji](https://developer.apple.com/library/ios/releasenotes/General/ValidateAppStoreReceipt/Chapters/ValidateRemotely.html) dokumentacji innych kodów błędów.

#### <a name="restoring-auto-renewable-subscriptions"></a>Przywracanie odnawialnymi automatycznie subskrypcji

Zostanie wyświetlony ponownie wiele transakcji — oryginalnej transakcji zakupu oraz osobnej transakcji dla każdego okresu czasu, który został odnowiony subskrypcji. Należy śledzić daty rozpoczęcia i warunków, aby zrozumieć, co to jest okres ważności.   
   
   
   
 Obiekt SKPaymentTransaction nie ma subskrypcji — należy użyć innego Identyfikatora produktu dla każdego warunku i pisania kodu, który można ekstrapolacja okresu subskrypcji od daty zakupu transakcji.

#### <a name="testing-auto-renewal"></a>Testowanie automatycznego odnawiania

Aby ułatwić testowanie subskrypcji, czasie ich trwania są kompresowane podczas testowania w piaskownicy. 1 tydzień subskrypcje odnowić co 3 minuty 1 rok subskrypcje odnowić co godzinę. Subskrypcje spowoduje automatyczne odnawianie maksymalnie 6 razy podczas testowania w piaskownicy.

## <a name="reporting"></a>Raportowanie

iTunes Connect ( [itunesconnect.apple.com](http://itunesconnect.apple.com)) zawiera:   
   
 **Sprzedaż i trendów** — Wyświetla szczegóły aplikacji pliki do pobrania, aktualizacje i zakupy w aplikacji.   
   
 **Płatności i raporty finansowe** — szczegóły dochód uzyskany z aplikacji, a także listę płatności, które zostały wprowadzone i ile są wobec.

Poniżej przedstawiono przykładowy raport sprzedaży i trendów:   

 [![](subscriptions-and-reporting-images/image42.png "Przykładowy raport sprzedaży i trendów")](subscriptions-and-reporting-images/image42.png#lightbox)   
   
 Istnieje również [ **MS Connect Mobile**aplikacji dla systemu iOS (łącze iTunes)](http://itunes.apple.com/us/app/itunes-connect-mobile/id376771144?mt=8).
zrzuty ekranu iPhone niektórych dostępne statystyki są wyświetlane tutaj:   
   
 [![](subscriptions-and-reporting-images/image43.png "zrzuty ekranu iPhone niektórych dostępne statystyki")](subscriptions-and-reporting-images/image43.png#lightbox)
