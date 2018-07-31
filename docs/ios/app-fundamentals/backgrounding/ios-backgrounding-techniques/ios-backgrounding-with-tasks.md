---
title: iOS uruchamianie procesów w tle za pomocą zadań
description: W tym dokumencie opisano, jak używać zadań w tle do wykonywania długotrwałych zadań, po umieszczeniu w tle aplikacji.
ms.prod: xamarin
ms.assetid: 205D230E-C618-4D69-96EE-4B91D7819121
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 9d304ee64e7716413febc475e721f5eb39043109
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351541"
---
# <a name="ios-backgrounding-with-tasks"></a>iOS uruchamianie procesów w tle za pomocą zadań

Najprostszym sposobem wykonania uruchamianie procesów w tle w systemie iOS jest podział wymagań dotyczących backgrounding na zadania i uruchamianie zadań w tle. Zadania są objęte ograniczeniami limitu czasu i zazwyczaj pobrać około 600 sekund (10 minut) czasu przetwarzania po aplikacji została przeniesiona do tła w systemie iOS 6 i mniej niż 10 minut w systemie iOS 7 +.

Zadania w tle mogą być podzielone na trzy kategorie:

1.  **Zadania w tle Safe** — wywoływany w dowolnym miejscu aplikacji, w którym masz zadania nie chcesz przerwane należy wprowadzić ją tła.
1.  **Zadania DidEnterBackground** — wywoływany podczas `DidEnterBackground` metoda cyklu życia aplikacji ułatwiają czyszczenia oraz zapisywanie stanu.
1.  **Transfery (system iOS 7 +) w tle** -specjalny rodzaj zadanie w tle używany do wykonywania transfery w sieci w systemie iOS 7. W odróżnieniu od typowych zadań transferów w tle nie ma ustalonej limitu czasu.


Bezpieczne tła i `DidEnterBackground` zadania są bezpiecznie korzystać zarówno z systemem iOS 6, jak i dla systemu iOS 7, przy czym pewne niewielkie różnice. Teraz zbadać te dwa rodzaje zadań, które bardziej szczegółowo.

## <a name="creating-background-safe-tasks"></a>Tworzenie zadań tła palety

Niektóre aplikacje zawiera zadania, które nie powinny zostać przerwane przez system iOS należy ją zmienić stan. Jednym ze sposobów, aby chronić te zadania są przerwane jest zarejestrowanie iOS jako długotrwałych zadań. Użytkownik może używać tego wzorca dowolne miejsce w aplikacji gdzie nie chcesz, że zadanie jest przerwane powinien put użytkownika aplikacji w tle. Doskonałym kandydatem do ten wzorzec może być zadania, takie jak wysyłanie informacji o rejestracji nowego użytkownika do serwera lub weryfikowanie logowania.

Poniższy fragment kodu przedstawia, rejestrowanie wykonywanie zadania w tle:

```csharp
nint taskID = UIApplication.SharedApplication.BeginBackgroundTask( () => {});

//runs on main or background thread
FinishLongRunningTask(taskID);

UIApplication.SharedApplication.EndBackgroundTask(taskID);
```

Proces rejestracji pary zadań o unikatowym identyfikatorze `taskID`i otacza go w dopasowywanie `BeginBackgroundTask` i `EndBackgroundTask` wywołania. Aby wygenerować identyfikator, firma Microsoft wywołania `BeginBackgroundTask` metody `UIApplication` obiektu, a następnie uruchom długotrwałe zadanie, zazwyczaj w nowym wątku. Po zakończeniu zadania nazywamy `EndBackgroundTask` i przekaż w ten sam identyfikator. Jest to ważne, ponieważ dla systemu iOS zostanie zakończone jej `BeginBackgroundTask` wywołania nie ma pasującego `EndBackgroundTask`.

> [!IMPORTANT]
> Zadania w tle safe można uruchomić w głównym wątku lub wątku w tle, w zależności od potrzeb aplikacji.


## <a name="performing-tasks-during-didenterbackground"></a>Wykonywanie zadań podczas DidEnterBackground

Oprócz zadań długotrwałych safe tła, rejestracja może służyć do uruchamiał zadania, ponieważ aplikacja jest umieszczeniem w tle. systemu iOS zapewnia metodę zdarzeń w *elemencie AppDelegate* klasę o nazwie `DidEnterBackground` który może służyć do zapisania stanu aplikacji, zapisywanie danych użytkownika i szyfrować poufnej zawartości, zanim aplikacja wchodzi w tle. Aplikacja ma około 5 sekund do zwrócenia z tej metody lub będzie zostać zakończone. W związku z tym, zadania oczyszczania, które może zająć więcej niż 5 sekund może zostać wywołana z wewnątrz `DidEnterBackground` metody. Te zadania musi być wywoływany w oddzielnym wątku.

Proces jest niemal identyczny jak w przypadku rejestrowania długotrwałe zadanie. Poniższy fragment kodu przedstawia to w akcji:

```csharp
public override void DidEnterBackground (UIApplication application) {
  nint taskID = UIApplication.SharedApplication.BeginBackgroundTask( () => {});
  new Task ( () => {
    DoWork();
    UIApplication.SharedApplication.EndBackgroundTask(taskID);
  }).Start();
}
```

Zaczniemy przez zastąpienie `DidEnterBackground` method in Class metoda `AppDelegate`, gdzie zarejestrujemy nasz zadania za pomocą `BeginBackgroundTask` ile My mieliśmy w poprzednim przykładzie. Następnie możemy zduplikować nowego wątku i wykonywać naszych długotrwałe zadanie. Należy pamiętać, że `EndBackgroundTask` teraz Wykonano wywołanie z wewnątrz długotrwałe zadanie, ponieważ `DidEnterBackground` metoda ma już zwróciła.

> [!IMPORTANT]
> dla systemu iOS używa [programu alarmowego mechanizm](http://developer.apple.com/library/ios/qa/qa1693/_index.html) zapewnienie reaguje interfejs użytkownika aplikacji. Aplikacja, która zużywa zbyt dużo czasu w `DidEnterBackground` przestanie odpowiadać w interfejsie użytkownika. Umożliwia uruchamianie zadań w tle określonego `DidEnterBackground` do zwrócenia w odpowiednim czasie, utrzymywanie dynamicznego interfejsu użytkownika co uniemożliwia zabijanie aplikacji przez strażnika.


## <a name="handling-background-task-time-limits"></a>Limity czasu zadań obsługi w tle

iOS wpływają ograniczeniami na jak długo zadanie w tle można uruchomić, a jeśli `EndBackgroundTask` w wyznaczonym czasie nie zostanie nawiązane połączenie, aplikacja zostanie zakończona. Przez śledzenie pozostały czas uruchamianie procesów w tle i używanie obsługi wygaśnięcia, gdy jest to konieczne, firma Microsoft można uniknąć, zamykając aplikację dla systemu iOS.

### <a name="accessing-background-time-remaining"></a>Uzyskiwanie dostępu do pozostały czas tła

Jeśli aplikacji przy użyciu zarejestrowanego zadania pobiera przeniesione do tła, zarejestrowanego zadania otrzyma około 600 sekund do uruchomienia. Możemy sprawdzić, jak długo zadanie będzie musiał wykonać przy użyciu statycznych `BackgroundTimeRemaining` właściwość `UIApplication` klasy. Poniższy kod umożliwi nam czasu w sekundach, który opuścił nasze zadania w tle:

```csharp
double timeRemaining = UIApplication.SharedApplication.BackgroundTimeRemaining;
```

### <a name="avoiding-app-termination-with-expiration-handlers"></a>Unikanie zakończenia aplikacji z obsługą wygaśnięcia

Oprócz dając dostęp do `BackgroundTimeRemaining` właściwości systemu iOS zapewnia łagodne sposób obsługi tła czasu wygaśnięcia za pośrednictwem **obsługi wygaśnięcia**. Jest to opcjonalne bloku kodu, które są wykonywane, gdy czas przydzielony dla zadania wkrótce wygaśnie. Wywołuje kod w obsłudze wygaśnięcia `EndBackgroundTask` i przekazuje w identyfikatorze zadania, co oznacza, że aplikacja zachowuje się prawidłowo i uniemożliwia zakończenie aplikacji nawet wtedy, gdy zadanie jest uruchamiane Przekroczono limit czasu dla systemu iOS. `EndBackgroundTask` musi być wywołana wewnątrz procedury obsługi wygaśnięcia, a także w trakcie normalnego działania. 

Procedury obsługi wygaśnięcia jest wyrażona jako funkcja anonimowa użycie wyrażenia lambda, jak przedstawiono poniżej:

```csharp
Task.Factory.StartNew( () => {

    //expirationHandler only called if background time allowed exceeded
    var taskId = UIApplication.SharedApplication.BeginBackgroundTask(() => {
        Console.WriteLine("Exhausted time");
        UIApplication.SharedApplication.EndBackgroundTask(taskId); 
    });
    while(myFlag == true)
    {
        Console.WriteLine(UIApplication.SharedApplication.TimeRemaining);
        myFlag = SomeCalculationNeedsMoreTime();
    }
    //Only called if loop terminated due to myFlag and not expiration of time
    UIApplication.SharedApplication.EndBackgroundTask(taskId);
});
```

Podczas obsługi wygaśnięcia nie są wymagane dla kodu do uruchomienia, zawsze należy używać do obsługi wygaśnięcia za pomocą zadania w tle.

 <a name="background_tasks_in_iOS_7" />

## <a name="background-tasks-in-ios-7"></a>Zadania w tle w systemie iOS 7 +

Największe zmiany w systemie iOS 7 w odniesieniu do zadań w tle jest nie sposobu implementacji zadania, ale podczas uruchamiania.

Odwołaj wstępnego dla systemu iOS 7, zadania uruchomione w tle ma 600 sekund do ukończenia. Jednym z powodów ten limit jest, że zadanie uruchomione w tle zachowa urządzenia wznowione na czas trwania zadania:

 [![](ios-backgrounding-with-tasks-images/ios6.png "Wykres zadania, utrzymywanie jej wznowione wstępnego systemu iOS 7")](ios-backgrounding-with-tasks-images/ios6.png#lightbox)

przetwarzanie w tle systemu iOS 7 jest zoptymalizowany pod kątem dłuższy czas pracy baterii. W systemie iOS 7, uruchamianie procesów w tle staje się oportunistyczne: zamiast urządzenia wznowione, przestrzegać przejdzie do uśpienia, zamiast wykonywać ich przetwarzanie we fragmentach, gdy urządzenie wraca do stanu aktywności do obsługi połączeń telefonicznych, powiadomienia, przychodzących wiadomości e-mail i innych zadań Typowe przerw w działaniu. Na poniższym diagramie przedstawiono wgląd w sposób zadanie może nie działać się:

 [![](ios-backgrounding-with-tasks-images/ios7.png "Wykres zadania jest dzielony na fragmenty po dla systemu iOS 7")](ios-backgrounding-with-tasks-images/ios7.png#lightbox)

Zadania, w czasie wykonywania nie jest już ciągłe, zadań wykonujących transfery sieci muszą być obsługiwane inaczej w systemie iOS 7. Deweloperzy są zachęcani do użycia `NSURlSession` interfejsu API do obsługi transfery w sieci. Następnej sekcji przedstawiono omówienie procedury transferów w tle.

 <a name="background-transfers" />

## <a name="background-transfers"></a>Transferów w tle

Szkielet transferów w tle w systemie iOS 7 jest nowym `NSURLSession` interfejsu API. `NSURLSession` Umożliwia nam tworzenie zadań:

1.  Transfer zawartości za pośrednictwem przerw w działaniu sieci i urządzeń.
1.  Przekazywanie i pobieranie dużych plików ( *usługi transferu w tle* ).


Przyjrzyjmy się bliżej jak to działa.

### <a name="nsurlsession-api"></a>Interfejs API sesji NSURLSession

 `NSURLSession` jest to zaawansowany interfejs API transferu zawartości za pośrednictwem sieci. Zapewnia zestaw narzędzi do obsługi transferu danych za pośrednictwem przerwami w łączności sieciowej i zmiany stanów aplikacji.

`NSURLSession` Interfejsu API powoduje utworzenie jednego lub kilku sesji, które z kolei zduplikować zadania wahadłowe bloki powiązanych danych w sieci. Zadania są uruchamiane asynchronicznie do transferu danych, szybko i niezawodnie. Ponieważ `NSURLSession` jest asynchroniczna, każdej sesji wymaga zakończenia Blok obsługi, aby umożliwić systemu i aplikacji, o których wiadomo, kiedy przenoszenie zostało zakończone.

Aby przeprowadzić transferu sieciowego, który jest prawidłowy w wstępnego dla systemu iOS 7 i po system iOS 7, sprawdź, czy `NSURLSession` można umieścić w kolejce transferu, a następnie użyć zadania w tle regularne do transferu, jeśli nie jest:

```csharp
if ([NSURLSession class]) {
  // Create a background session and enqueue transfers
}
else {
  // Start a background task and transfer directly
  // Do NOT make calls to update the UI here!
}
```

> [!IMPORTANT]
> Należy unikać wykonywania wywołań do aktualizacji interfejsu użytkownika w tle w kodzie 6 zgodne z systemem iOS, iOS 6, nie obsługuje aktualizacje interfejsu użytkownika w tle i zakończą działanie aplikacji.


`NSURLSession` Interfejsu API zawiera bogaty zestaw funkcji, aby obsługiwać uwierzytelnianie, zarządzanie transfery nie powiodło się i raportowania błędów po stronie klienta — ale nie po stronie serwera —. Pomaga mostek, który przerw w działaniu w zadaniu wykonawczego wprowadzona w systemie iOS 7, a także zapewnia obsługę transferu dużych plików, szybko i niezawodnie. Następnej sekcji przedstawiono ten drugi funkcji.

### <a name="background-transfer-service"></a>Usługa transferu w tle

Przed systemu iOS 7 zawodne był przekazywania lub pobierania plików w tle. Zadania w tle uzyskać przez ograniczony czas do uruchomienia, ale czas potrzebny na transfer pliku zależy od sieci i rozmiaru pliku. W systemie iOS 7, możemy użyć `NSURLSession` do pomyślnie przekazywania i pobierania dużych plików. Określonych `NSURLSession` typ sesji, który obsługuje transfery sieci dużych plików w tle jest znany jako *usługi transferu w tle*.

Transfery inicjowane z użyciem usługi transferu w tle są zarządzane przez system operacyjny i zapewniają interfejsy API do obsługi uwierzytelniania i błędy. Ponieważ transfer nie są powiązane przez dowolne limitu czasu, umożliwia przekazywanie lub pobieranie dużych plików, automatyczne aktualizowanie zawartości w tle i nie tylko. Zapoznaj się [wskazówki transferu w tle](~/ios/app-fundamentals/backgrounding/ios-backgrounding-walkthroughs/background-transfer-walkthrough.md) szczegółowe informacje na temat wdrażania usługi.

Usługa transferu w tle często jest powiązany z pobieranie w tle lub zdalne powiadomienia aplikacji odświeżyć zawartość w tle. W dwóch następnych sekcjach wprowadzeniu koncepcji rejestrowanie całych aplikacji do uruchamiania w tle na zarówno dla systemu iOS 6 lub iOS 7.

