---
title: SiriKit
ms.topic: article
ms.prod: xamarin
ms.assetid: 8F75300B-B591-42ED-9D17-001992A5C381
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/07/2017
ms.openlocfilehash: 557521bc3bce41b9023acbf31a344a57cb63d2a1
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="sirikit"></a>SiriKit

SiriKit została wprowadzona w systemie iOS 10, przez pewną liczbę domen usługi (w tym ćwiczeń, jazdy rezerwacji i wywołań). Zapoznaj się [sekcji SiriKit](~/ios/platform/sirikit/index.md) SiriKit koncepcji i sposobu wdrażania SiriKit w aplikacji.

![Wersja demonstracyjna listy zadań Siri](sirikit-images/sirikit.png)

SiriKit w systemie iOS 11 dodaje następujące nowe i zaktualizowane domen konwersji:

- [**Wyświetla listę i uwagi dotyczące** ](#listsnotes) — nowy! Udostępnia interfejs API dla aplikacji do przetworzenia zadań i notatek.
- **Kody Visual** — nowy! Używanie programu Siri można wyświetlić kody QR, aby udostępnić informacje kontaktowe lub uczestniczenie w transakcjach płatności.
- **Płatności** — dodano intencje wyszukiwania i transferu dla interakcje płatności.
- **Rezerwacji, które wywołują** — dodano anulować intencje jazdy i przesyłania opinii.

Inne nowe funkcje:

- [**Nazwy aplikacji alternatywnych** ](#alternativenames) — zawiera aliasy, które ułatwiają klientom Poinformuj Siri pod kątem aplikacji oferując alternatywnych nazw/wymowy.
- **Uruchamianie ćwiczeń** — pozwala, aby uruchomić ćwiczeń w tle.

Poniżej opisano niektóre z tych funkcji. Więcej informacji dotyczących innych, można znaleźć w [dokumentacji SiriKit firmy Apple](https://developer.apple.com/documentation/sirikit).

<a name="listsnotes" />

## <a name="lists-and-notes"></a>Listy i uwagi

W nowej domenie list i uwagi udostępnia interfejs API dla aplikacji do przetworzenia zadań i notatek za pomocą żądań głosowego Siri.

**Zadania**

- Mieć tytuł i stan ukończenia.
- Opcjonalnie terminu i lokalizację.

**Uwagi**

- Mieć tytuł i polem zawartości.

Zarówno zadań i notatek można zorganizowane w grupy. Pozostałej części tej sekcji opisano sposób wykonania tej nowej domeny za pomocą SiriKit, za pomocą [TasksNotes SiriKit przykład](https://developer.xamarin.com/samples/monotouch/ios11/SiriKitSample/).

### <a name="how-to-process-a-sirikit-request"></a>Sposób przetwarzania żądań SiriKit

Proces żądania SiriKit, wykonaj następujące czynności:

1. **Rozwiąż** — Sprawdź poprawność parametrów i żądać dalszych informacji na użytkownika (jeśli jest to wymagane).
2. **Potwierdź** — końcowego sprawdzania poprawności i weryfikacji można przetworzyć żądania.
3. **Obsługa** — operacji (Aktualizowanie danych lub przeprowadzanie operacji sieciowych).

Pierwsze dwa kroki są opcjonalne (mimo że zalecane), a ostatnim krokiem jest wymagana.
Istnieją bardziej szczegółowe instrukcje w [sekcji SiriKit](~/ios/platform/sirikit/index.md).

### <a name="resolve-and-confirm-methods"></a>Rozwiązania i Potwierdź metody

Te metody opcjonalne umożliwiają kodu wykonywania sprawdzania poprawności, wybierz ustawienia domyślne lub dodatkowe informacje o żądaniu użytkownika.

Na przykład dla `IINCreateTaskListIntent` jest wymaganej metody interfejsu `HandleCreateTaskList`. Istnieją cztery metody opcjonalne, które zapewniają większą kontrolę nad interakcji Siri:

- `ResolveTitle` — Weryfikuje tytuł, ustawia domyślny tytuł (w razie potrzeby) lub sygnalizuje, że dane nie są wymagane.
- `ResolveTaskTitles` — Weryfikuje na liście zadań używany przez użytkownika.
- `ResolveGroupName` — Sprawdza poprawność nazwy grupy, wybierze domyślnej grupy lub sygnalizuje, że dane nie są wymagane.
- `ConfirmCreateTaskList` — Weryfikuje, czy można wykonać żądanej operacji, ale nie wykonuje (tylko `Handle*` metody należy modyfikować danych).

### <a name="handle-the-intent"></a>Dojście celem

Istnieje sześć intencje w domenie, list i uwagi, trzech zadań i trzy notatek.
Metody, które musi implementować, aby obsługiwać te opcje są:

- Dla zadania:
  - `HandleAddTasks`
  - `HandleCreateTaskList`
  - `HandleSetTaskAttribute`
- Aby uzyskać informacje o:
  - `HandleCreateNote`
  - `HandleAppendToNote`
  - `HandleSearchForNotebookItems`

Każda metoda ma określonego typu konwersji przekazywania, która zawiera wszystkie informacje Siri jest analizowana z żądania użytkownika (i prawdopodobnie zaktualizowany w `Resolve*` i `Confirm*` metody).
Aplikacji należy przeanalizować dane dostarczone, a następnie wykonać niektóre akcje do przechowywania lub w inny sposób procesu danych i zwracanie wyniku, który Siri mówi i pokazuje użytkownikowi.

### <a name="response-codes"></a>Kody odpowiedzi

Wymagane `Handle*` i opcjonalne `Confirm*` metod wskazać kod odpowiedzi przez ustawienie wartości dla obiektów, które przekazują do ich obsługi uzupełniania. Odpowiedzi pochodzą z `INCreateTaskListIntentResponseCode` wyliczenie:

- `Ready` — Zwraca w fazie potwierdzania (tj. z `Confirm*` metody, ale nie z `Handle*` — metoda).
- `InProgress` — Używany dla długotrwałych zadań (na przykład operacji sieciowych/server).
- `Success` — Odpowiada szczegóły powodzenie operacji (tylko z `Handle*` metody).
- `Failure` — Oznacza, że wystąpił błąd i nie można ukończyć operacji.
- `RequiringAppLaunch` — Nie można przetworzyć przy użyciu zamiaru, ale operacja jest możliwa w aplikacji.
- `Unspecified` — Nie używaj: komunikat o błędzie będzie widoczny dla użytkownika.

Dowiedz się więcej o tych metod i odpowiedzi w firmy Apple [SiriKit zawiera listę i uwagi dotyczące dokumentacji](https://developer.apple.com/documentation/sirikit/lists_and_notes).

### <a name="implementing-lists-and-notes"></a>Implementacja list i uwagi

[TasksNotes SiriKit przykład](https://developer.xamarin.com/samples/monotouch/ios11/SiriKitSample/) został utworzony, wykonując następujące kroki, aby dodać obsługę SiriKit do pusta aplikacja systemu iOS.

Najpierw Aby dodać obsługę SiriKit, wykonaj te kroki dla aplikacji systemu iOS:

1. Znaczników **SiriKit** w **Entitlements.plist**.
2. Dodaj **prywatności — opis użycia Siri** klucza **Info.plist**, wraz z komunikatu dla klientów.
3. Wywołanie `INPreferences.RequestSiriAuthorization` metody w aplikacji, monit, aby umożliwić używanie programu Siri interakcji.
4. Dodaj SiriKit do Identyfikatora aplikacji w portalu dla deweloperów i Utwórz ponownie profili inicjowania obsługi administracyjnej, aby uwzględnić nowe uprawnienia.

Następnie należy dodać nowy projekt rozszerzenia do aplikacji do obsługi żądań Siri:

1. Kliknij prawym przyciskiem myszy rozwiązanie, a następnie wybierz pozycję **Dodaj > Dodawanie nowego projektu...** .
2. Wybierz **systemu iOS > Rozszerzenia > rozszerzenia intencje** szablonu.
3. Dwa nowe projekty zostaną dodane: próba i IntentUI. Dostosowywanie interfejsu użytkownika jest opcjonalne, więc próbki zawiera tylko kod w **zamiar** projektu.

Projekt rozszerzenia jest przetwarzania wszystkich żądań SiriKit. Jako rozszerzenie oddzielne go nie ma automatycznie sposób komunikować się z głównym aplikacji — zazwyczaj jest to rozpoznać zaimplementowanie udostępnionego magazynu plików korzystania z grup aplikacji.

#### <a name="configure-the-intenthandler"></a>Skonfiguruj IntentHandler

`IntentHandler` Klasy jest punkt wejścia dla żądań Siri — co celem jest przekazywana do `GetHandler` metody, która zwraca obiekt, który może obsłużyć żądania.

Poniższy kod przedstawia implementację prostego:

```csharp
[Register("IntentHandler")]
public partial class IntentHandler : INExtension, IINNotebookDomainHandling
{
  protected IntentHandler(IntPtr handle) : base(handle)
  {}
  public override NSObject GetHandler(INIntent intent)
  {
    // This is the default implementation.  If you want different objects to handle different intents,
    // you can override this and return the handler you want for that particular intent.
    return this;
  }
  // add intent handlers here!
}
```

Musi dziedziczyć po klasie `INExtension`, i próbka przechodzi do obsługi list i uwagi dotyczące lokalizacji docelowych, dlatego też implementuje `IINNotebookDomainHandling`.

> [!NOTE]
> **Uwagi dotyczące nazewnictwa:** Brak Konwencji w .NET dla interfejsów do wielką prefiksem `I`, które odpowiada Xamarin powiązań protokołów z zestawu SDK dla systemu iOS.
>
> Xamarin zachowuje również nazwy typu z systemem iOS i Apple używa dwóch pierwszych znaków nazwy typu do uwzględnienia w ramach należącego do typu.
>
> Aby uzyskać `Intents` framework, typy są poprzedzane prefiksem `IN*` (np.) `INExtension`), ale są one _nie_ interfejsów.
> Również wynika, że protokołów (które stają się interfejsów w języku C#) na końcu dwa `I`s, takich jak `IINAddTasksIntentHandling`.

#### <a name="handling-intents"></a>Opcje obsługi

Każda próba (Dodaj zadanie, ustaw atrybut zadanie itp.) jest zaimplementowana w pojedynczej metody podobny do przedstawionego poniżej. Metoda, należy wykonać trzy główne funkcje:

1. **Przetwarzanie zamiar** — analizowane przez Siri danych ma zostać udostępnione w `intent` obiektu specyficzne dla typu założeń. Aplikacja może zweryfikować danych za pomocą opcjonalnych `Resolve*` metody.
2. **Sprawdzanie poprawności i aktualizowanie magazynu danych** — zapisywanie danych w systemie plików (przy użyciu grupy aplikacji, aby aplikacji systemu iOS głównego również dostęp do niego) lub przy użyciu żądania sieci.
3. **Podaj odpowiedź** — użyj `completion` obsługi wysłać odpowiedź z powrotem do Siri do odczytu/ekranu dla użytkownika:

```csharp
public void HandleCreateTaskList(INCreateTaskListIntent intent, Action<INCreateTaskListIntentResponse> completion)
{
  var list = TaskList.FromIntent(intent);
  // TODO: have to create the list and tasks... in your app data store
  var response = new INCreateTaskListIntentResponse(INCreateTaskListIntentResponseCode.Success, null)
  {
    CreatedTaskList = list
  };
  completion(response);
}
```

Zwróć uwagę, że `null` jest przekazywany jako drugiego parametru odpowiedzi — jest to parametr działania użytkownika, a jeśli nie zostanie podany, zostanie użyta wartość domyślna.
Typ działania niestandardowego można ustawić tak długo, jak aplikacji systemu iOS obsługuje go za pomocą `NSUserActivityTypes` klucza w **Info.plist**. Następnie można obsłużyć tego przypadku po otwarciu aplikacji i wykonywania określonych operacji (np. otwierając do kontrolera widoku odpowiednich i ładowania danych z operacji Siri).

Przykładzie również hardcodes `Success` wynik, ale w rzeczywistych scenariuszach raportowanie błędów prawidłowego powinny zostać dodane.

### <a name="test-phrases"></a>Testowanie wyrażenia

Następujących fraz testu powinny działać w przykładowej aplikacji:

- "Można utworzyć spożywczych jabłek, bananów i gruszek w TasksNotes"
- "Dodaj zadanie WWDC w TasksNotes"
- "Dodaj zadanie WWDC do listy szkolenia w TasksNotes"
- "Znak uczestniczyć WWDC jako zakończony w TasksNotes"
- "W TasksNotes Przypomnij kupić iphone, gdy pojawia się Strona główna"
- "Znak kupić iPhone, jak zostało ukończone w TasksNotes"
- "Przypomnij pozostawienie Narzędzia główne, przy 8 am w TasksNotes"

![Utwórz nowy przykład listy](sirikit-images/createtasklist-sml.png) ![Zestaw zadań jako pełny przykład](sirikit-images/settaskattribute-sml.png)

> [!NOTE]
> 11 dla systemu iOS Simulator obsługuje testowanie za pomocą Siri (w przeciwieństwie do wcześniejszych wersji).
>
> Testowania na urządzeniach prawdziwe, nie zapomnij skonfigurować Identyfikatora aplikacji i Inicjowanie obsługi profilów SiriKit pomocy technicznej.

<a name="alternativenames" />

## <a name="alternative-names"></a>Alternatywne nazwy

Ta nowa funkcja systemu iOS 11 oznacza, że można skonfigurować alternatywne nazwy dla aplikacji w taki sposób ułatwić użytkownikom wyzwalanie ona poprawnie z Siri. Dodaj następujące klucze do **Info.plist** plików projektu aplikacji systemu iOS:

![Info.plist przedstawiający aplikacji alternatywne nazwy kluczy i wartości](sirikit-images/alternative-names.png)

Przy użyciu zestawu nazwy alternatywnej aplikacji następujących fraz również będzie działać w przypadku przykładowej aplikacji (które faktycznie nosi nazwę **TasksNotes**):

- "Można utworzyć spożywczych jabłek, bananów i gruszek w _MonkeyNotes_"
- "Dodaj zadanie WWDC w _MonkeyTodo_"


## <a name="troubleshooting"></a>Rozwiązywanie problemów

Niektóre błędy, które mogą wystąpić podczas przeprowadzania próbki lub dodawanie SiriKit do własnych aplikacji:

### <a name="nsinternalinconsistencyexception"></a>NSInternalInconsistencyException

_Zgłoszono wyjątek języka Objective-C.  Nazwa: NSInternalInconsistencyException Przyczyna: użycie klasy < INPreferences: 0x60400082ff00 > z aplikacji wymaga com.apple.developer.siri uprawnienia. Czy włączono możliwości Siri w projekcie Xcode?_

- SiriKit zaznaczono w **Entitlements.plist**.
- **Entitlements.plist** jest skonfigurowany w **opcje projektu > kompilacji > iOS podpisywania pakietu**.

  [![Opcje projektu przedstawiający poprawnie ustawić uprawnień](sirikit-images/set-entitlements-sml.png)](sirikit-images/set-entitlements.png#lightbox)

- (dla wdrożenia urządzenia) Identyfikator aplikacji ma włączone SiriKit i profil inicjowania obsługi administracyjnej pobrane.


## <a name="related-links"></a>Linki pokrewne

- [SiriKit (Apple)](https://developer.apple.com/documentation/sirikit)
- [Przykładowe SiriKit TasksNotes](https://developer.xamarin.com/samples/monotouch/ios11/SiriKitSample/)
- [Nowości w SiriKit (WWDC) (klip wideo)](https://developer.apple.com/videos/play/wwdc2017/214/)
