---
title: Wydajności dla wielu Platform
description: W tym dokumencie opisano różne techniki, które pozwalają zwiększyć wydajność aplikacji mobilnej. Omówiono w nim Profiler, interfejs IDisposable zasobów, słabe odwołania, modułu odzyskiwania pamięci SGen, sposobów zmniejszania rozmiaru i innych.
ms.prod: xamarin
ms.assetid: 9ce61f18-22ac-4b93-91be-5b499677d661
author: asb3993
ms.author: amburns
ms.date: 03/24/2017
ms.openlocfilehash: c529d1d42d582cb49a906ad6fc39a191a7389f58
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997442"
---
# <a name="cross-platform-performance"></a>Wydajności dla wielu Platform

Niską wydajność aplikacji przedstawia na wiele sposobów. Może sprawić, że aplikacja prawdopodobnie nie odpowiada, mogą powodować wolne przewijanie i może zmniejszyć czas pracy baterii. Jednak Optymalizacja wydajności polega na więcej niż tylko wdrażania skutecznego kodu. Należy również uwzględnić środowisko wydajność aplikacji. Na przykład zapewnienie, że operacje są wykonywane bez blokowania użytkownikowi wykonywanie innych działań może pomóc ulepszyć środowisko użytkownika.

<a name="profiler" />

## <a name="use-the-profiler"></a>Użyj Profiler

Podczas tworzenia aplikacji, jest tylko próba optymalizacji kodu, gdy ma zostać profilowania. Profilowanie jest techniką, określania, gdzie optymalizacje kodu mają największy wpływ na zmniejszenie problemy z wydajnością. Program profilujący śledzi wykorzystanie pamięci aplikacji i rejestruje czas działania metod w aplikacji. Te dane ułatwiają do nawigowania ścieżki wykonywania aplikacji, a koszt wykonywania kodu, dzięki czemu mogą być wykrywane najlepsze możliwości optymalizacji.

Profiler Xamarin miar, oceny i znaleźć problemy związane z wydajnością w aplikacji. Może służyć do profilu aplikacji Xamarin.iOS i Xamarin.Android w programie Visual Studio for Mac lub Visual Studio. Aby uzyskać więcej informacji na temat Profiler Xamarin zobacz [wprowadzenie do platformy Xamarin Profiler](~/tools/profiler/index.md).

Poniższe najlepsze rozwiązania są zalecane w przypadku profilowania aplikacji:

- Unikaj profilowanie aplikacji w symulatorze, ponieważ symulator mogą zakłócać wydajność aplikacji.
- W idealnym przypadku profilowania należy wykonać na różnych urządzeniach, ponieważ wykonywania pomiarów wydajności na jednym urządzeniu nie będzie zawsze pokazywać charakterystyki wydajności innych urządzeń. Jednak co najmniej profilowania należy wykonać na urządzeniu, które ma najniższe przewidywanego specyfikację.
- Zamknij wszystkie inne aplikacje, aby upewnić się, że całkowity wpływ profilowaną aplikacją mierzony, a nie inne aplikacje.

<a name="idisposable" />

## <a name="release-idisposable-resources"></a>Zwolnienie zasobów interfejs IDisposable

`IDisposable` Interfejs zapewnia mechanizm przy zwalnianiu zasobów. Zapewnia `Dispose` metodę, która powinna zostać wdrożone w celu jawnie zwalniać zasoby. `IDisposable` nie jest destruktor i powinny być wdrożone tylko w następujących okolicznościach:

- Gdy klasa jest właścicielem niezarządzane zasoby. Typowe niezarządzane zasoby, które wymagają zwalniania obejmują plików, strumieni i połączeń sieciowych.
- Jeśli klasa ma zarządzane `IDisposable` zasobów.

Następnie wywołać konsumenci typu `IDisposable.Dispose` implementacji, aby zwolnić zasoby, jeśli wystąpienie nie jest już wymagane. Dostępne są dwie opcje realizacji tego:

- Dzięki zawijaniu `IDisposable` obiektu `using` instrukcji.
- Dzięki zawijaniu wywołanie `IDisposable.Dispose` w `try` / `finally` bloku.

### <a name="wrapping-the-idisposable-object-in-a-using-statement"></a>Zawijanie obiektu interfejs IDisposable w za pomocą instrukcji

Poniższy przykład kodu pokazuje, jak opakowywać `IDisposable` obiektu `using` instrukcji:

```csharp
public void ReadText (string filename)
{
  ...
  string text;
  using (StreamReader reader = new StreamReader (filename)) {
    text = reader.ReadToEnd ();
  }
  ...
}
```

`StreamReader` Klasy implementuje `IDisposable`i `using` instrukcja oferuje wygodnej składni, która wywołuje `StreamReader.Dispose` metody `StreamReader` obiekt przed jego przechodzenia poza zakres tej asysty. W ramach `using` bloku `StreamReader` obiekt jest tylko do odczytu i nie może zostać przypisany. `using` Instrukcji również zapewnia, że `Dispose` metoda jest wywoływana, nawet jeśli wystąpi wyjątek, ponieważ kompilator implementuje języka pośredniego (IL) dla `try` / `finally` bloku.

### <a name="wrapping-the-call-to-idisposabledispose-in-a-tryfinally-block"></a>Zawijanie wywołanie metody IDisposable.Dispose w bloku Try/Finally

Poniższy przykład kodu pokazuje, jak można opakować wywołanie `IDisposable.Dispose` w `try` / `finally` bloku:

```csharp
public void ReadText (string filename)
{
  ...
  string text;
  StreamReader reader = null;
  try {
    reader = new StreamReader (filename);
    text = reader.ReadToEnd ();
  } finally {
    if (reader != null) {
      reader.Dispose ();
    }
  }
  ...
}
```

`StreamReader` Klasy implementuje `IDisposable`i `finally` Blokuj wywołania `StreamReader.Dispose` metody do zwolnienia zasobu.

Aby uzyskać więcej informacji, zobacz [interfejs IDisposable](xref:System.IDisposable).

<a name="events" />

## <a name="unsubscribe-from-events"></a>Anulowanie subskrypcji zdarzeń

Aby zapobiec przeciekom pamięci, zdarzenia należy anulować w przed jest usunięty obiekt subskrybenta. Do momentu Anulowano zdarzenia, obiekt delegowany dla zdarzenia w obiekcie publikowania odwołuje się do delegata, który hermetyzuje programu obsługi zdarzeń subskrybenta. Tak długo, jak obiekt publikowania zawiera to odwołanie, wyrzucanie elementów bezużytecznych nie spowoduje odzyskanie pamięci obiektu subskrybenta.

Poniższy przykład kodu pokazuje, jak anulować subskrypcję zdarzenia:

```csharp
public class Publisher
{
  public event EventHandler MyEvent;

  public void OnMyEventFires ()
  {
    if (MyEvent != null) {
      MyEvent (this, EventArgs.Empty);
    }
  }
}

public class Subscriber : IDisposable
{
  readonly Publisher publisher;

  public Subscriber (Publisher publish)
  {
    publisher = publish;
    publisher.MyEvent += OnMyEventFires;
  }

  void OnMyEventFires (object sender, EventArgs e)
  {
    Debug.WriteLine ("The publisher notified the subscriber of an event");
  }

  public void Dispose ()
  {
    publisher.MyEvent -= OnMyEventFires;
  }
}
```

`Subscriber` Klasy anuluje subskrypcje ze zdarzenia w jego `Dispose` metody.

Cykle odwołań może również wystąpić podczas korzystania z programów obsługi zdarzeń i składnia lambda, wyrażenia lambda można odwołać się i podtrzymywania obiektów. W związku z tym odwołanie do metody anonimowej można przechowywane w polu i umożliwia anulowanie subskrypcji zdarzeń, jak pokazano w poniższym przykładzie kodu:

```csharp
public class Subscriber : IDisposable
{
  readonly Publisher publisher;
  EventHandler handler;

  public Subscriber (Publisher publish)
  {
    publisher = publish;
    handler = (sender, e) => {
      Debug.WriteLine ("The publisher notified the subscriber of an event");
    };
    publisher.MyEvent += handler;
  }

  public void Dispose ()
  {
    publisher.MyEvent -= handler;
  }
}
```

`handler` Pola odwołania do metody anonimowej i jest używany dla subskrypcji zdarzeń i anulowanie subskrypcji.

<a name="weakreferences" />

## <a name="use-weak-references-to-prevent-immortal-objects"></a>Słabe odwołania Użyj, aby zapobiec Immortal obiektów

> [!NOTE]
> Deweloperzy systemu iOS należy zapoznać się z dokumentacją na [unikanie odwołań cyklicznych w systemie iOS](~/ios/deploy-test/performance.md#avoid-strong-circular-references) zapewnienie ich aplikacje używają pamięci wydajnie.

<a name="lazy" />

## <a name="delay-the-cost-of-creating-objects"></a>Opóźnienie kosztu tworzenia obiektów

Inicjalizacja z opóźnieniem może służyć do odroczenia tworzenia obiektu, aż najpierw jest używana. Ta technika służy głównie do zwiększenia wydajności, należy unikać obliczeń i zmniejszyć wymagania dotyczące pamięci.


Należy wziąć pod uwagę przy użyciu inicjowania z opóźnieniem dla obiektów, które są kosztowne w tym przypadku dwóch scenariuszy:

- Obiekt nie może być używany przez aplikację.
- Inne kosztownych operacji musi wykonać, zanim obiekt zostanie utworzony.

`Lazy<T>` Klasa jest używana do definiowania typu inicjowany z opóźnieniem, jak pokazano w poniższym przykładzie kodu:

```csharp
void ProcessData(bool dataRequired = false)
{
  Lazy<double> data = new Lazy<double>(() =>
  {
    return ParallelEnumerable.Range(0, 1000)
                 .Select(d => Compute(d))
                 .Aggregate((x, y) => x + y);
  });

  if (dataRequired)
  {
    if (data.Value > 90)
    {
      ...
    }
  }
}

double Compute(double x)
{
  ...
}
```

Inicjalizacja z opóźnieniem występuje po raz pierwszy `Lazy<T>.Value` dostępu do właściwości. Opakowany typ jest tworzone zwracanych na pierwszym dostępu i przechowywane, aby uzyskać dostęp do wszystkich przyszłych.

Aby uzyskać więcej informacji na temat inicjowania z opóźnieniem, zobacz [inicjowania z opóźnieniem](https://msdn.microsoft.com/library/dd997286(v=vs.110).aspx).

<a name="async" />

## <a name="implement-asynchronous-operations"></a>Implementowania asynchronicznych operacji

.NET zawiera asynchroniczne wersje wielu interfejsów API. W odróżnieniu od synchronicznych interfejsów API asynchronicznych interfejsów API, upewnij się, że wątek wykonywania aktywnych nigdy nie blokuje wątek wywołujący dla znaczną ilość czasu. Dlatego podczas wywoływania interfejsu API z wątku interfejsu użytkownika, użycie asynchronicznego interfejsu API, jeśli jest ona dostępna. Pozwoli to zachować wątku interfejsu użytkownika zostało odblokowane, które pomogą w ulepszaniu środowiska użytkownika z aplikacją.

Dodatkowo długotrwałe operacje powinien być wykonywany na wątku w tle, aby uniknąć blokowania wątku interfejsu użytkownika. .NET zapewnia `async` i `await` słów kluczowych, które umożliwiają pisanie kodu asynchronicznego, wykonuje długotrwałych operacji w wątku w tle, która uzyskuje dostęp do wyników po zakończeniu. Jednakże podczas długotrwałych operacji może być wykonywany asynchronicznie za pomocą `await` — słowo kluczowe, to nie gwarantuje, że operacja będzie działać w wątku w tle. Zamiast tego można to osiągnąć przez przekazanie długotrwała operacja `Task.Run`, jak pokazano w poniższym przykładzie kodu:

```csharp
public class FaceDetection
{
  ...
  async void RecognizeFaceButtonClick(object sender, EventArgs e)
  {
    await Task.Run(() => RecognizeFace ());
    ...
  }

  async Task RecognizeFace()
  {
    ...
  }
}
```

`RecognizeFace` Metoda jest wykonywana na wątku w tle za pomocą `RecognizeFaceButtonClick` metoda oczekiwanie, aż do `RecognizeFace` metoda zakończy się przed kontynuowaniem.

Długotrwałe operacje powinien obsługiwać anulowania. Na przykład kontynuowanie długotrwałej operacji może stać się konieczne, jeśli użytkownik przechodzi w aplikacji. Wzorzec do wdrażania anulowania jest następująca:

- Utwórz `CancellationTokenSource` wystąpienia. To wystąpienie będzie zarządzać i wysyłania powiadomień anulowania.
- Przekaż `CancellationTokenSource.Token` wartość właściwości każdego zadania, które powinny być można anulować.
- Mechanizm dla każdego zadania reagowanie na operację anulowania.
- Wywołaj `CancellationTokenSource.Cancel` metody w celu zapewnienia anulowane — powiadomienie.

> [!IMPORTANT]
> `CancellationTokenSource` Klasy implementuje `IDisposable` interfejsu, a więc `CancellationTokenSource.Dispose` metoda powinna ona zostać wywołana po `CancellationTokenSource` wystąpienie zostało zakończone z.



Aby uzyskać więcej informacji, zobacz [Omówienie obsługi asynchronicznej](~/cross-platform/platform/async.md).

<a name="sgen" />

## <a name="use-the-sgen-garbage-collector"></a>Użyj modułu odzyskiwania pamięci SGen

Zarządzane języków, takich jak C# Użyj zbierania elementów bezużytecznych odzyskiwaniu pamięci przydzielonej do obiektów, które nie są już w użyciu. Są dwa moduły zbierające pamięci używane przez platformę Xamarin:

- [**SGen** ](http://www.mono-project.com/docs/advanced/garbage-collector/sgen/) — jest to pokoleniowego modułu odzyskiwania pamięci a domyślny moduł odśmiecania pamięci na platformie Xamarin.
- [**Boehm** ](http://www.hboehm.info/gc/) — jest to zachowawcze, pokoleniowego modułu zbierającego elementy bezużyteczne. Jest to domyślny moduł odśmiecania pamięci używany na potrzeby aplikacji platformy Xamarin.iOS, które używają klasycznego interfejsu API.

SGen korzysta z jednego z trzech sterty do alokowania miejsca dla obiektów:

-  **Przedszkola** — jest to, gdzie są przydzielane nowe małych obiektów. Gdy przedszkola zabraknie miejsca, nastąpi pomocnicza wyrzucania elementów bezużytecznych. Wszystkie obiekty na żywo zostanie przeniesiona do głównych sterty.
-  **Główna sterty** — jest to, gdzie są przechowywane długotrwałe obiekty. Jeśli w stosie główne nie ma wystarczającej ilości pamięci, główne wyrzucania elementów bezużytecznych zostanie przeprowadzona. Jeśli główna wyrzucania elementów bezużytecznych nie powiedzie się w celu zwolnienia wystarczającej ilości pamięci SGen będzie monitować systemu dla większej ilości pamięci.
-  **Duża przestrzeń obiektu** — jest to, gdzie przechowywane są obiekty, które wymagają ponad 8000 bajtów. Duże obiekty nie rozpocznie w przedszkola, ale zamiast tego zostanie przydzielony w stercie tego.

Jest jedną z zalet SGen, że czas potrzebny do wykonania pomocnicza wyrzucania elementów bezużytecznych jest proporcjonalna do liczby nowych obiektów na żywo, które zostały utworzone od czasu ostatniego pomocnicza wyrzucania elementów bezużytecznych. Zmniejszy to wpływu na wyrzucanie elementów bezużytecznych na wydajność aplikacji, ponieważ te pomocnicze wyrzucania elementów bezużytecznych będzie trwać krócej niż główne wyrzucania elementów bezużytecznych. Główne wyrzucania elementów bezużytecznych nastąpi, ale mniejszą częstotliwością.

### <a name="reducing-pressure-on-the-garbage-collector"></a>Zmniejszenie wykorzystania moduł odśmiecania pamięci

Po uruchomieniu wyrzucania elementów bezużytecznych SGen przestanie wątki aplikacji podczas jej przejmuje pamięć. Gdy jest on odzyskać pamięci, aplikacja może krótki wstrzymanie lub odtwarzanie przerywane w interfejsie użytkownika. Jak widocznych jest ta przerwa, zależy od dwa czynniki:

1. **Częstotliwość** — jak często wyrzucanie elementów bezużytecznych występuje. Częstotliwość wyrzucania elementów bezużytecznych zwiększy większej ilości pamięci jest przydzielony między kolekcje.
1. **Czas trwania** — jak długo potrwa każdego poszczególnych wyrzucania elementów bezużytecznych. Jest to około proporcjonalna do liczby obiektów na żywo, które są zbierane.

Łącznie oznacza to, że wiele obiektów są przydzielone, ale nie pozostaną aktywne, będzie wiele krótki wyrzucania. Z drugiej strony Jeśli nowe obiekty są przydzielane wolno i obiekty pozostają aktywne, będzie mniej, ale dłużej wyrzucania elementów bezużytecznych.

Aby zmniejszyć wykorzystanie moduł odśmiecania pamięci, należy przestrzegać następujących wytycznych:

- Należy unikać wyrzucania elementów bezużytecznych w ścisłej pętli, korzystając z pul obiektu. Jest to szczególnie istotne dla gier, które należy utworzyć większość w nich obiekty z wyprzedzeniem.
- Jawnie zwalniać zasoby, takie jak strumienie, połączeń sieciowych, dużych bloków pamięci i plików, gdy nie są już wymagane. Aby uzyskać więcej informacji, zobacz [zwolnienia zasobów interfejsu IDisposable](#idisposable).
- Anulowanie rejestracji procedury obsługi zdarzeń, gdy nie są już wymagane, aby obiekty sinv do zebrania. Aby uzyskać więcej informacji, zobacz [Anuluj subskrypcję zdarzeń](#events).

<a name="linker" />

## <a name="reduce-the-size-of-the-application"></a>Zmniejsz rozmiar aplikacji.

Jest ważne, aby zrozumieć proces kompilacji na każdej platformie, aby dowiedzieć się, z której pochodzi aplikacji o takim rozmiarze pliku wykonywalnego:

- aplikacje dla systemu iOS są z wyprzedzeniem of-time (AOT) kompilowany na język asemblera ARM. Program .NET framework jest uwzględnione, nieużywane klasami trwa wycięte tylko wtedy, gdy włączono opcję konsolidatora w odpowiednie.
- Aplikacje systemu android są kompilowane do języka pośredniego (IL) i pakiecie z programem MonoVM i just-in-time (JIT) kompilacja. Klasy framework nieużywane wyrazy i frazy tylko wtedy, gdy włączono opcję konsolidatora w odpowiednie.
- Aplikacje Windows Phone są kompilowane do IL i wykonywane przez wbudowane środowisko uruchomieniowe.

Ponadto jeśli aplikacja wykonuje zwiększone użycie typów ogólnych, a następnie dalsze zwiększy rozmiaru końcowego pliku wykonywalnego, ponieważ będzie ona zawierać natywnie kompilowane wersje ogólne możliwości.

Aby zmniejszyć rozmiar aplikacji platformy Xamarin obejmuje konsolidatora w ramach narzędzia do kompilacji. Domyślnie konsolidator jest wyłączona i musi być włączona w opcjach projektu dla aplikacji. W czasie kompilacji zostanie przeprowadzone analizę statyczną aplikacji, aby określić, które typy i elementy członkowskie, używanych przez aplikację. Go następnie usunie wszystkie nieużywane typów i metod z aplikacji.

Poniższy zrzut ekranu przedstawia konsolidator opcje w programie Visual Studio dla komputerów Mac dla projektu platformy Xamarin.iOS:

![](memory-perf-best-practices-images/linker-options-ios.png)

Poniższy zrzut ekranu przedstawia konsolidator opcje w programie Visual Studio dla komputerów Mac dla projektu platformy Xamarin.Android:

![](memory-perf-best-practices-images/linker-options-droid.png)

Konsolidator zawiera trzy różne ustawienia, aby kontrolować swoje zachowanie:

-  **Nie łącz** — nie nieużywane typy i metody zostaną usunięte przez konsolidator. Ze względu na wydajność jest ustawieniem domyślnym dla kompilacji debugowania.
-  **Połącz tylko Framework SDK/pakiet SDK zestawy** — to ustawienie tylko zmniejszy rozmiar tych zestawów, które są dostarczane przez środowisko Xamarin. Będzie to miało wpływu kod użytkownika.
-  **Łącz wszystkie zestawy** — jest to bardziej agresywnego optymalizacji, które będą ukierunkowane na zestaw SDK zestawów i kodem użytkownika. Dla powiązania spowoduje to usunięcie pola zapasowego nieużywane i wprowadzić każdego wystąpienia (lub powiązane obiekty) jaśniejszego, zużywa mniej pamięci.

*Łączy wszystkie zestawy* należy używać ostrożnie, ponieważ może spowodować jej uszkodzenie aplikacji w nieoczekiwany sposób. Analizy statycznej, które jest wykonywane przez konsolidator nie może poprawnie zidentyfikować wszystkie kod, który jest wymagany, co w kodzie, zbyt dużo usuwany z skompilowanej aplikacji. Ta sytuacja będzie objawiać tylko w czasie wykonywania, gdy wystąpiła awaria aplikacji. W związku z tym należy dokładnie przetestować aplikację po zmianie zachowanie konsolidatora.

Jeśli testy ujawnić, czy konsolidator jest nieprawidłowo usunięty, klasy lub metody, możliwe oznaczyć typu lub metody, które nie są statyczne odwołania, ale są wymagane przez aplikację przy użyciu jednej z następujących atrybutów:

-  `Xamarin.iOS.Foundation.PreserveAttribute` — Ten atrybut jest dla projektów platformy Xamarin.iOS.
-  `Android.Runtime.PreserveAttribute` — Ten atrybut jest dla projektów platformy Xamarin.Android.

Na przykład może być konieczne zachować konstruktory domyślne typów, które są dynamicznie tworzone. Ponadto użycia serializacji XML może wymagać, że właściwości typów zostaną zachowane.

Aby uzyskać więcej informacji, zobacz [konsolidatora dla systemu iOS](~/ios/deploy-test/linker.md) i [konsolidatora dla systemu Android](~/android/deploy-test/linker.md).

### <a name="additional-size-reduction-techniques"></a>Sposobów zmniejszania rozmiaru dodatkowe

Istnieją szerokiej gamy architektury procesorów tego zasilania urządzeń przenośnych. W związku z tym, tworzyć Xamarin.iOS i Xamarin.Android *fat pliki binarne* zawierających skompilowanych wersji aplikacji dla każdej architektury Procesora. Zapewnia to, że aplikacja mobilna może działać na urządzeniu, niezależnie od architektury Procesora.

Poniższe kroki, można jeszcze bardziej ograniczyć rozmiar pliku wykonywalnego aplikacji:

- Upewnij się, że jest generowany kompilację wydania.
- Zmniejsz liczbę architektury, w których aplikacja została opracowana z myślą, aby uniknąć FAT pliku binarnego, generowany.
- Upewnij się, że kompilator LLVM jest używana, aby wygenerować bardziej zoptymalizowanego pliku wykonywalnego.
- Zmniejsz rozmiar zarządzanego kodu aplikacji. Można to zrobić przez włączenie konsolidatora w każdym zestawie (*wszystkie łącza* dla projektów systemu iOS i *łącz wszystkie zestawy* dla projektów systemu Android).

Aplikacje dla systemu android może zostać podzielony w osobnym pliku APK dla każdego interfejsu ABI ("Architektura").
Dowiedz się więcej w tym wpisie w blogu: [jak można zachować Your Android rozmiar niedziałającej aplikacji](http://motzcod.es/post/112072508362/how-to-keep-your-android-app-size-down).

<a name="optimizeimages" />

## <a name="optimize-image-resources"></a>Optymalizuj zasoby obrazów

Obrazy są najbardziej kosztowne zasoby, których aplikacje, i często są przechwytywane w wysokiej rozdzielczości. Spowoduje to utworzenie pełną szczegółów Wyraziste obrazy, aplikacje, które zazwyczaj wyświetlania tych obrazów wymagają więcej użycie procesora CPU do dekodowania obrazu i większej ilości pamięci do przechowywania zdekodowany obrazu. Jest marnotrawstwa można dekodować obraz o wysokiej rozdzielczości w pamięci, gdy będzie on skalowany w do mniejszego rozmiaru do wyświetlenia. Zamiast tego należy ograniczyć wpływ użycia i pamięci procesora CPU przez tworzenie wielu wersji rozwiązania przechowywane obrazy znajdujące się blisko formaty wyświetlania warunków prognozowanych. Na przykład obraz wyświetlany w widoku listy należy prawdopodobnie niższej rozdzielczości niż obraz wyświetlany na pełnym ekranie. Ponadto, skalowane w dół wersje obrazów o wysokiej rozdzielczości mogą być załadowane do wydajnego wyświetlane bez wpływu na minimalnej pamięci. Aby uzyskać więcej informacji, zobacz [obciążenia duże mapy bitowe efektywnie](https://developer.xamarin.com/recipes/android/resources/general/load_large_bitmaps_efficiently/).

Niezależnie od rozpoznawania obrazów wyświetlania zasobów obrazu może znacznie zwiększyć zużycie pamięci przez aplikację. W związku z tym ich powinien zostać utworzony tylko podczas wymagane i powinny zostać opublikowane, jak aplikacja nie wymaga już je.

<a name="activationperiod" />

## <a name="reduce-the-application-activation-period"></a>Zmniejsz okres aktywacji aplikacji

Wszystkie aplikacje mają *okres aktywacji*, czyli czasu między podczas uruchamiania aplikacji oraz gdy aplikacja jest gotowa do użycia. Ten okres aktywacji zapewnia użytkownikom ich wyglądu aplikacji, a więc jest ważne ograniczyć okres aktywacji i użytkownik postrzega, aby uzyskanie pozytywnych, pierwsze wrażenie aplikacji.

Zanim aplikacja wyświetli jego początkowej interfejsu użytkownika, powinien udostępniać ekran powitalny, aby poinformować użytkownika, że aplikacja jest uruchamiana. Jeśli aplikacja nie może szybko wyświetlić jego początkowej interfejsu użytkownika, na ekranie powitalnym należy użyć informować użytkownika postępu przy użyciu okres aktywacji umożliwiają zabezpieczenie, które aplikacja nie odpowiada. Może być to zabezpieczenie, pasek postępu, lub podobne kontroli.

W okresie aktywacji aplikacje są wykonywane logiki aktywacji, które często obejmuje ładowanie i przetwarzanie zasobów. Okres aktywacji można zmniejszyć przez zapewnienie, że wymagane zasoby są pakowane w aplikacji, zamiast pobierania zdalnie. Na przykład w niektórych sytuacjach może być odpowiednie do ładowania danych symbolu zastępczego przechowywane lokalnie, w okresie aktywacji. Następnie po początkowej interfejsu użytkownika jest wyświetlany, a użytkownik ma możliwość interakcji z aplikacją, symbol zastępczy danych można stopniowo zastąpić ze zdalnego źródła. Ponadto logiki aktywacji aplikacji należy wykonywać tylko wtedy pracy, które są wymagane, aby umożliwić użytkownikowi rozpocząć korzystanie z aplikacji. Może to ułatwić, jeśli jeszcze opóźnienia podczas ładowania dodatkowych zestawów, ponieważ zestawy są ładowane, są one używane po raz pierwszy.

<a name="webservicecommunication" />

## <a name="reduce-web-service-communication"></a>Zmniejsz komunikacji usług sieci Web

Łączenie się z usługą sieci web z aplikacji może mieć wpływ na wydajność aplikacji. Na przykład zwiększenie wykorzystania przepustowości sieci spowoduje zwiększone użycie baterii urządzenia. Ponadto użytkownicy może używać aplikacji w środowisku ograniczonej przepustowości. Dlatego jest rozsądne ograniczyć wykorzystanie przepustowości między aplikacją a usługą sieci web.

Jedno z podejść do zmniejszyć wykorzystanie przepustowości aplikacji jest Kompresuj dane przed przeniesieniem ich za pośrednictwem sieci. Jednak dodatkowe użycie procesora CPU z procesem kompresję także może spowodować użycie zwiększone baterii. W związku z tym to kosztem powinny być dobrze przemyślane przed podjęciem decyzji, czy przenieść skompresowanych danych za pośrednictwem sieci.

Inną kwestią do rozważenia jest format danych przesyłanych między aplikacją a usługą sieci web. Dwa podstawowe formaty są Extensible Markup Language (XML) i JavaScript Object Notation (JSON). Kod XML jest format wymiany danych oparte na tekście, tworzącego ładunków stosunkowo dużej ilości danych, ponieważ zawiera on dużą liczbę znaki formatowania. JSON jest formatem wymiany danych tekstowych, który tworzy ładunków compact danych, co spowodowało mniejsze wymagania dotyczące przepustowości podczas wysyłania danych i odbierania danych. W związku z tym JSON, często jest preferowanym formacie dla aplikacji mobilnych.

Zaleca się użycia obiektów transferu danych (dto), podczas przesyłania danych między aplikacją a usługą sieci web. Obiekt DTO zawiera zestaw danych przesyłania przez sieć. Wykorzystując dto, większej ilości danych, mogą zostać przesłane w pojedynczym wywołaniu zdalnego, co może pomóc zmniejszyć liczbę wywołań zdalnych wykonanych przez aplikację. Ogólnie rzecz biorąc zdalnego wywołania wykonywania większy ładunek danych ma podobne ilość czasu, jako wywołanie obsługująca tylko ładunku małych danych.

Dane pobrane z usługi sieci web powinno być buforowane lokalnie, za pomocą dane w pamięci podręcznej jest wykorzystywana, zamiast wielokrotnie pobrane z usługi sieci web. Jednakże przyjmując takie podejście odpowiedniej strategii buforowania również powinny być zrealizowane Aby zaktualizować dane w lokalnej pamięci podręcznej, jeśli zmieni się w usłudze sieci web.

## <a name="summary"></a>Podsumowanie

W tym artykule opisano i omówiono techniki pozwalające zwiększyć wydajność aplikacji utworzonych za pomocą platformy Xamarin. Zbiorczo te techniki znacznie zmniejszyć ilość pracy wykonywanej przez Procesora i ilość pamięci używanej przez aplikację.

## <a name="related-links"></a>Linki pokrewne

- [Xamarin.iOS Performance](~/ios/deploy-test/performance.md)
- [Xamarin.Android Performance](~/android/deploy-test/performance.md)
- [Wprowadzenie do platformy Xamarin Profiler](~/tools/profiler/index.md)
- [Xamarin.Forms Performance](~/xamarin-forms/deploy-test/performance.md)
- [Asynchroniczna pomoc techniczna — omówienie](~/cross-platform/platform/async.md)
- [Interfejs IDisposable](xref:System.IDisposable)
- [Unikanie typowych pułapek w aplikacjach platformy Xamarin (wideo)](https://university.xamarin.com/guestlectures/avoiding-common-pitfalls-in-xamarin-apps)
