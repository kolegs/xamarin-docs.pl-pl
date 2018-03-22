---
title: iOS Backgrounding z zadaniami
ms.topic: article
ms.prod: xamarin
ms.assetid: 205D230E-C618-4D69-96EE-4B91D7819121
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: ad75dfac55add7e03ffbdb910e0e62ebd0fd6c18
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/22/2018
---
# <a name="ios-backgrounding-with-tasks"></a>iOS Backgrounding z zadaniami

Najprostszym sposobem wykonania backgrounding w systemie iOS jest podział wymagań backgrounding na zadania i uruchom zadania w tle. Zadania są objęte ograniczeniami limit czasu i zwykle w systemie iOS 7 + pobrać około 600 sekund (10 minut) czas przetwarzania, gdy aplikacja zostanie przeniesiona do tła w systemie iOS 6 i mniejsza niż 10 minut.

Zadania w tle mogą być dzielone na trzy kategorie:

1.  **Zadania tła palety** — wywołana w dowolnym miejscu aplikacji, gdzie masz zadania nie ma przerwania aplikacji należy wprowadzić w tle.
1.  **Zadania DidEnterBackground** — wywołane podczas `DidEnterBackground` metody cyklem życia aplikacji w oczyszczania i zapisywania stanu.
1.  **Transfery (iOS 7 +) w tle** -specjalny typ zadania w tle używane do przeprowadzania transferów sieci w systemie iOS 7. W przeciwieństwie do regularnych zadań transfery w tle nie ma wstępnie określoną limit czasu.


Bezpieczne dla tła i `DidEnterBackground` zadania są bezpiecznie korzystać na zarówno iOS 6 lub iOS 7, przy czym niektóre niewielkie różnice. Umożliwia badanie tych dwóch typów zadań bardziej szczegółowo.

## <a name="creating-background-safe-tasks"></a>Tworzenie zadań tła palety

Niektóre aplikacje zawiera zadania, które nie powinny zostać przerwane przez iOS powinien aplikacji zmiany stanu. Jednym ze sposobów ochrony tych zadań z zostanie przerwane jest je zarejestrować iOS jako długotrwałych zadań. Można używać tego wzorca dowolne miejsce w aplikacji których nie chcesz się, że zadanie zostanie przerwane powinien put użytkownika aplikacji w tle. Doskonałym kandydatem do tego wzorca będzie zadania, takie jak wysyłanie informacji o rejestracji nowego użytkownika z serwerem lub weryfikowanie informacji o logowaniu.

Poniższy fragment kodu przedstawia rejestrowanie wykonywanie zadania w tle:

```csharp
nint taskID = UIApplication.SharedApplication.BeginBackgroundTask( () => {});

//runs on main or background thread
FinishLongRunningTask(taskID);

UIApplication.SharedApplication.EndBackgroundTask(taskID);
```

Proces rejestracji pary zadań z unikatowym identyfikatorem `taskID`i jest zawijana w odpowiadającym `BeginBackgroundTask` i `EndBackgroundTask` wywołania. Aby wygenerować identyfikator, możemy wywoływania `BeginBackgroundTask` metoda `UIApplication` obiektu, a następnie uruchom długotrwałe zadanie, zazwyczaj w nowym wątku. Po zakończeniu zadania nazywamy `EndBackgroundTask` i przebiegu w tym samym identyfikatorze. Jest to ważne, ponieważ iOS spowoduje przerwanie aplikacji, jeśli `BeginBackgroundTask` wywołania nie ma odpowiadającego mu `EndBackgroundTask`.

> [!IMPORTANT]
> Bezpieczne tła zadań można uruchamiać na wątku głównego lub wątku w tle, w zależności od potrzeb aplikacji.


## <a name="performing-tasks-during-didenterbackground"></a>Wykonywanie zadań podczas DidEnterBackground

Oprócz tworzenia długotrwałe zadanie tła palety, rejestracji można rozpocząć wyłączyć zadania, ponieważ aplikacja jest jest umieszczany w tle. iOS zapewnia metoda zdarzeń w *AppDelegate* klasy o nazwie `DidEnterBackground` który może służyć do Zapisz stan aplikacji, Zapisz dane użytkownika i szyfrowania poufnej zawartości, zanim aplikacja przechodzi w tle. Aplikacja ma około pięciu sekund do zwrócenia z tej metody lub pobrać proces zakończony. W związku z tym zadania oczyszczania, które może zająć więcej niż pięciu sekund może zostać wywołana z wewnątrz `DidEnterBackground` metody. Te zadania muszą być wywoływane w oddzielnym wątku.

Proces jest niemal identyczna z rejestrowania zadań długotrwałe. Poniższy fragment kodu przedstawiono to w akcji:

```csharp
public override void DidEnterBackground (UIApplication application) {
  nint taskID = UIApplication.SharedApplication.BeginBackgroundTask( () => {});
  new Task ( () => {
    DoWork();
    UIApplication.SharedApplication.EndBackgroundTask(taskID);
  }).Start();
}
```

Zaczniemy przez zastąpienie `DidEnterBackground` metody w `AppDelegate`, gdzie możemy zarejestrować nasze zadania za pomocą `BeginBackgroundTask` jak robiliśmy w poprzednim przykładzie. Następnie możemy zduplikować nowego wątku i wykonać naszych długotrwałe zadanie. Należy pamiętać, że `EndBackgroundTask` teraz wywołanie z wewnątrz długotrwałe zadanie, ponieważ `DidEnterBackground` metoda ma już zwróciła.

> [!IMPORTANT]
> iOS używa [programu alarmowego mechanizm](http://developer.apple.com/library/ios/qa/qa1693/_index.html) zapewnienie reaguje interfejsu użytkownika aplikacji. Aplikacja, która zużywa zbyt dużo czasu, w `DidEnterBackground` będzie odpowiadać w interfejsie użytkownika. Zasób wyłączyć zadania w tle umożliwia `DidEnterBackground` do zwrócenia w odpowiednim czasie, pamiętając reakcji interfejsu użytkownika i uniemożliwia programu alarmowego skasowanie aplikacji.


## <a name="handling-background-task-time-limits"></a>Limit czasu zadania obsługi w tle

iOS umieszcza strict limity na jak długo zadanie w tle można uruchomić, a jeśli `EndBackgroundTask` wywołanie nie zostało utworzone w wyznaczonym czasie, aplikacja zostanie zakończona. Przez śledzenie pozostały czas backgrounding i za pomocą obsługi wygaśnięcia, gdy jest to konieczne, firma Microsoft uniknąć przerywanie aplikacji systemu iOS.

### <a name="accessing-background-time-remaining"></a>Dostęp do tła pozostały czas:

Jeśli aplikacji z zadaniami w zarejestrowany pobiera przeniesiony do tła, zarejestrowane zadania zostanie wyświetlona około 600 sekund do uruchomienia. Musimy sprawdzić czas, jaki ma zadania do wykonania za pomocą statycznych `BackgroundTimeRemaining` właściwość `UIApplication` klasy. Następujący kod będzie Przekaż nam czas w sekundach, który opuścił nasze zadania w tle:

```csharp
double timeRemaining = UIApplication.SharedApplication.BackgroundTimeRemaining;
```

### <a name="avoiding-app-termination-with-expiration-handlers"></a>Unikanie zakończenia aplikacji z obsługą wygaśnięcia

Oprócz zapewnienia dostępu do `BackgroundTimeRemaining` właściwości, z systemem iOS umożliwia bezpieczne obsługi tła czas wygaśnięcia za pośrednictwem **obsługi wygaśnięcia**. Jest to opcjonalne blok kodu, która zostanie wykonana po czas przydzielony dla zadania, które niedługo wygasną. Kod w obsłudze wygaśnięcia wywołuje `EndBackgroundTask` i przebiegów w Identyfikatora zadania, która wskazuje, że aplikacja zachowuje się również i uniemożliwia iOS zakończenie aplikacji, nawet wtedy, gdy zadanie jest uruchamiane limit czasu. `EndBackgroundTask` musi zostać wywołana wewnątrz obsługi wygaśnięcia, a także w trakcie normalnego przebiegu wykonywania. 

Program obsługi wygaśnięcia jest wyrażony jako funkcji anonimowej za pomocą wyrażenia lambda, jak przedstawiono poniżej:

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

Podczas obsługi wygaśnięcia nie są wymagane dla kodu do uruchomienia, zawsze należy używać programu obsługi, który wygaśnięcia zadania w tle.

 <a name="background_tasks_in_iOS_7" />

## <a name="background-tasks-in-ios-7"></a>Zadania w tle w systemie iOS 7 +

Największych zmiany w systemie iOS 7 w odniesieniu do zadania w tle jest nie sposobu implementacji zadania, ale podczas uruchamiania.

Odwołaj wstępnego systemu iOS 7, zadania uruchomione w tle ma 600 sekund do wykonania. Jedną z przyczyn ten limit jest, że zadania uruchomione w tle zachowa urządzenia wznowione na czas trwania zadania:

 [![](ios-backgrounding-with-tasks-images/ios6.png "Wykres zadań utrzymywanie aplikacji wznowione wstępnego systemu iOS 7")](ios-backgrounding-with-tasks-images/ios6.png#lightbox)

przetwarzanie w tle dla systemu iOS 7 jest zoptymalizowana pod kątem dłuższy czas pracy baterii. W systemie iOS 7, backgrounding staje się oportunistyczne: zamiast utrzymywanie urządzenia wznowione, zgodne urządzenie przejdzie do uśpienia, a zamiast tego wykonaj ich przetwarzania w fragmentów podczas działania urządzenia do obsługi połączeń telefonicznych, powiadomienia, przychodzących wiadomości e-mail i innych zadań Typowe przerw w działaniu. Na poniższym diagramie przedstawiono wgląd w sposób zadanie może być uszkodzona zapasową:

 [![](ios-backgrounding-with-tasks-images/ios7.png "Wykres zadania jest dzielony na fragmenty po systemów iOS 7")](ios-backgrounding-with-tasks-images/ios7.png#lightbox)

Czas wykonywania zadania nie jest już ciągłego, zadań, które wykonuje transferów sieci można obsłużyć inaczej w systemie iOS 7. Deweloperzy są zachęcani do użycia `NSURlSession` interfejsu API do obsługi transferów sieci. Następna sekcja zawiera omówienie transfery w tle.

 <a name="background-transfers" />

## <a name="background-transfers"></a>Transfery w tle

Szkielet transfery w tle w systemie iOS 7 jest nowy `NSURLSession` interfejsu API. `NSURLSession` Umożliwia tworzenie zadań:

1.  Transfer zawartości za pośrednictwem sieci i urządzenia przerwami w działaniu.
1.  Przekazywanie i pobieranie dużych plików ( *Usługa transferu w tle* ).


Spójrzmy bliższe spojrzenie na jak to działa.

### <a name="nsurlsession-api"></a>NSURLSession interfejsu API

 `NSURLSession` jest zaawansowanym interfejsu API transferu zawartości w sieci. Zapewnia zestaw narzędzi do obsługi transferu danych za pośrednictwem sieci przerw i zmiany stanów aplikacji.

`NSURLSession` Interfejsu API tworzy co najmniej jednej sesji, które z kolei zduplikować zadania wahadłowe bloki powiązanych danych w sieci. Zadania są uruchamiane asynchronicznie do transferu danych, szybkie i niezawodne. Ponieważ `NSURLSession` jest asynchroniczne, co sesja wymaga bloku obsługi uzupełniania, aby umożliwić systemu i aplikacji wiedzieć, po zakończeniu transferu.

Aby przeprowadzić transferu sieciowego, który jest prawidłowy na wstępne systemu iOS 7 i po systemów iOS 7, sprawdź, czy `NSURLSession` można umieścić w kolejce transferu, a następnie użyć zadania tła regularne do transferu, jeśli nie jest:

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
> Należy unikać wykonywania wywołań do aktualizacji interfejsu użytkownika w tle w kodzie 6 zgodne z systemem iOS, iOS 6, nie obsługuje aktualizacje interfejsu użytkownika w tle, a zakończy aplikacji.


`NSURLSession` Interfejsu API zawiera bogaty zestaw funkcji, aby obsługiwać uwierzytelnianie, zarządzania transferami nie powiodło się i raportować błędy po stronie klienta — ale nie po stronie serwera —. Pomaga mostek, który przerw w zadaniu wykonawczego wprowadzone w systemie iOS 7, a także zapewnia obsługę transferu dużych plików, szybkie i niezawodne. Następna sekcja opisuje tej drugiej funkcji.

### <a name="background-transfer-service"></a>Usługa transferu w tle

Przed iOS 7 przekazywanie lub pobieranie plików w tle została niepewna. Zadania w tle uzyskać przez ograniczony czas do uruchomienia, ale czas potrzebny na transfer pliku zależy od sieci i rozmiaru pliku. W systemie iOS 7, możemy użyć `NSURLSession` pomyślnie przekazywania i pobierania dużych plików. Szczególne `NSURLSession` typ sesji, który obsługuje sieci transferu dużych plików w tle jest znany jako *Usługa transferu w tle*.

Transfery inicjowane za pomocą usługi transferu w tle są zarządzane przez system operacyjny i podaj interfejsów API do obsługi uwierzytelniania i błędów. Ponieważ transferów nie są powiązane przez dowolnego limit czasu, ich umożliwia przekazywanie lub pobieranie dużych plików, automatyczne aktualizowanie zawartości w tle i inne. Zapoznaj się [wskazówki transferu w tle](~/ios/app-fundamentals/backgrounding/ios-backgrounding-walkthroughs/background-transfer-walkthrough.md) szczegółowe informacje na temat wdrażania usługi.

Usługa transferu w tle jest często łączyć się z pobieranie w tle lub zdalnego powiadomienia do aplikacji, Odśwież zawartość w tle. W dwóch następnych sekcjach wprowadzeniu koncepcji rejestrowania całej aplikacji do uruchamiania w tle na zarówno dla systemu iOS 6 lub iOS 7.

