---
title: Wydajność i Platform
description: Istnieje wiele technik zwiększającą wydajność aplikacji skompilowanej za pomocą platformy Xamarin. Zbiorczo te techniki znacznie zmniejszyć ilość pracy wykonywana przez Procesora i ilości pamięci używanej przez aplikację. W tym artykule opisano i omówiono te techniki.
ms.topic: article
ms.prod: xamarin
ms.assetid: 9ce61f18-22ac-4b93-91be-5b499677d661
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/24/2017
ms.openlocfilehash: e8b597221e806c2338d6f1965d3d151f998a3011
ms.sourcegitcommit: 20ca85ff638dbe3a85e601b5eb09b2f95bda2807
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/28/2018
---
# <a name="cross-platform-performance"></a>Wydajność i Platform

_Istnieje wiele technik zwiększającą wydajność aplikacji skompilowanej za pomocą platformy Xamarin. Zbiorczo te techniki znacznie zmniejszyć ilość pracy wykonywana przez Procesora i ilości pamięci używanej przez aplikację. W tym artykule opisano i omówiono te techniki._

Niską wydajnością przedstawia na wiele sposobów. Go aplikacja prawdopodobnie nie odpowiada, może spowodować wolne przewijanie i może zmniejszyć czas pracy baterii. Jednak optymalizacji wydajności wymaga więcej niż tylko wdrażania wydajność kodu. Środowisko użytkownika wydajność aplikacji, należy również rozważyć. Na przykład zapewniając wykonanie operacji bez blokowania użytkownika wykonywanie innych działań może pomóc ulepszyć środowisko użytkownika.


<a name="profiler" />

## <a name="use-the-profiler"></a>Użyj profilera

Podczas tworzenia aplikacji, należy tylko próba optymalizacji kodu, gdy ma zostać profilowaniu. Profilowanie to technika, określając, gdzie ma największy wpływ ograniczyć problemy z wydajnością optymalizacji kodu. Profiler śledzi wykorzystanie pamięci aplikacji i rejestruje czas działania metod w aplikacji. Te dane ułatwiają poruszać się po ścieżek wykonywania aplikacji i kosztów wykonywania kodu, tak, aby mogły być odnajdowane najlepsze możliwości optymalizacji.

Xamarin Profiler miar, oceny i łatwy sposób znaleźć problemy związane z wydajnością w aplikacji. Może służyć do profilu aplikacji platformy Xamarin.iOS i Xamarin.Android z wewnątrz programu Visual Studio for Mac lub Visual Studio. Aby uzyskać więcej informacji na temat profilera Xamarin, zobacz [wprowadzenie do platformy Xamarin profilera](~/tools/profiler/index.md).

Następujące najlepsze rozwiązania są zalecane w przypadku profilowania aplikacji:

- Uniknąć profilowania aplikacji w symulatorze, zgodnie z symulator może zakłócić działanie aplikacji.
- Najlepiej, jeśli profilowania powinien być wykonywany na różnych urządzeniach, zgodnie z wykonaniem pomiarów wydajności na jednym urządzeniu nie będzie zawsze pokazuj charakterystyki wydajności innych urządzeń. Jednak co najmniej profilowania powinien być wykonywany na urządzeniu, które ma najniższe przewidywanego specyfikacji.
- Zamknij wszystkie inne aplikacje, aby upewnić się, że pełnego wpływu aplikacji PROFILOWANEGO mierzony, a nie inne aplikacje.

<a name="idisposable" />

## <a name="release-idisposable-resources"></a>Zwolnić zasoby interfejs IDisposable

`IDisposable` Zapewnia mechanizm za zwolnienie zasobów. Zapewnia `Dispose` metodę, która powinna zostać wdrożone jawnego zwolnienia zasobów. `IDisposable` nie jest destruktora, a tylko powinny być implementowane w następujących okolicznościach:

- Gdy klasa jest właścicielem zasoby niezarządzane. Typowe niezarządzane zasoby, które wymagają zwalniania Dołącz pliki, strumieni i połączeń sieciowych.
- Gdy klasa właścicielem zarządzane `IDisposable` zasobów.

Typ konsumentów może wywoływać `IDisposable.Dispose` implementacji, aby zwolnić zasoby, jeśli wystąpienie nie jest już wymagane. Istnieją dwie metody na osiągnięcie tego celu:

- Zawijania `IDisposable` obiektu w `using` instrukcji.
- Zawijania wywołanie `IDisposable.Dispose` w `try` / `finally` bloku.

### <a name="wrapping-the-idisposable-object-in-a-using-statement"></a>Zawijanie obiektu interfejs IDisposable w przy użyciu instrukcji

Poniższy przykład kodu pokazuje sposób zawijania `IDisposable` obiektu w `using` instrukcji:

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

`StreamReader` Klasa implementuje `IDisposable`i `using` instrukcji oferuje wygodny składnię, która wywołuje `StreamReader.Dispose` metoda `StreamReader` obiektu przed jego przechodzi poza zakresem. W ramach `using` bloku `StreamReader` obiekt jest tylko do odczytu i nie może zostać przypisany. `using` Instrukcji również zapewnia, że `Dispose` metoda jest wywoływana, nawet jeśli wystąpi wyjątek, jak kompilator implementuje języku pośrednim (IL) dla `try` / `finally` bloku.

### <a name="wrapping-the-call-to-idisposabledispose-in-a-tryfinally-block"></a>Zawijanie wywołanie metody IDisposable.Dispose w bloku Try/Finally

Poniższy przykład kodu pokazuje sposób zawijania wywołanie `IDisposable.Dispose` w `try` / `finally` bloku:

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

`StreamReader` Klasa implementuje `IDisposable`i `finally` zablokować wywołania `StreamReader.Dispose` metodę, aby zwolnić zasobu.

Aby uzyskać więcej informacji, zobacz [interfejs IDisposable](https://developer.xamarin.com/api/type/System.IDisposable/).

<a name="events" />

## <a name="unsubscribe-from-events"></a>Anulowanie subskrypcji zdarzeń

Aby zapobiec przecieki pamięci, zdarzenia należy anulować w przed obiekt subskrybenta jest usunięty z. Dopóki zdarzenia jest Anulowano subskrypcję, delegata zdarzenia w publikacji obiekt odwołuje się do delegata, który hermetyzuje abonenta obsługi zdarzeń. Tak długo, jak obiekt publikowania zawiera to odwołanie, wyrzucanie elementów bezużytecznych nie będzie odzyskiwanie pamięci obiektu subskrybenta.

Poniższy przykładowy kod przedstawia sposób anulowanie subskrypcji zdarzeń:

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

`Subscriber` Klasy cofnięć subskrypcji od zdarzenia w jego `Dispose` metody.

Cykle odwołania może również wystąpić, korzystając z procedury obsługi zdarzeń i składnia lambda wyrażenia lambda można odwoływać się i podtrzymywania obiektów. W związku z tym odwołaniem do metody anonimowej można przechowywanych w polu i umożliwia anulowanie subskrypcji zdarzeń, jak pokazano w poniższym przykładzie:

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

`handler` Pola przechowuje odwołania do metody anonimowej i jest używany dla subskrypcji zdarzeń i anulowanie subskrypcji.

<a name="weakreferences" />

## <a name="use-weak-references-to-prevent-immortal-objects"></a>Słabe odwołania Użyj, aby zapobiec Immortal obiektów

> [!NOTE]
> Deweloperzy systemu iOS należy zapoznać się z dokumentacją na [unikanie odwołania cykliczne w systemie iOS](~/ios/deploy-test/performance.md#avoidcircularreferences) zapewnienie swoje aplikacje efektywnie korzystać z pamięci.

<a name="lazy" />

## <a name="delay-the-cost-of-creating-objects"></a>Opóźnienie koszt Tworzenie obiektów

Inicjalizacja z opóźnieniem może służyć do mają odraczać Tworzenie obiektu, dopóki nie zostanie najpierw użyta. Ta metoda służy głównie do zwiększenia wydajności, uniknąć obliczeń i zmniejszyć wymagania dotyczące pamięci.


Rozważ użycie incjalizacji obiektów, które są kosztowne w tym przypadku dwóch scenariuszy:

- Obiekt nie może być używany w aplikacji.
- Inne kosztowne operacje należy wykonać przed utworzeniem obiektu.

`Lazy<T>` Klasa służy do definiowania opóźnieniem zainicjować typu, jak pokazano w poniższym przykładzie:

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

Inicjalizacja z opóźnieniem występuje po raz pierwszy `Lazy<T>.Value` dostępu do właściwości. Opakowanej typu jest utworzony i zwracany w pierwszym dostępie i przechowywane na dostęp do wszystkich przyszłych.

Aby uzyskać więcej informacji na temat Inicjalizacja z opóźnieniem, zobacz [Incjalizacji](https://msdn.microsoft.com/en-us/library/dd997286(v=vs.110).aspx).

<a name="async" />

## <a name="implement-asynchronous-operations"></a>Implementuje operacji asynchronicznych

.NET zawiera wiele interfejsów API, jego asynchronicznych wersji. W przeciwieństwie do interfejsów API synchroniczne asynchroniczne interfejsów API, upewnij się, że wątek active wykonanie nigdy nie blokuje wątek wywołujący dla znaczną ilość czasu. W związku z tym podczas wywoływania funkcji API z wątku interfejsu użytkownika, użyj interfejsu API asynchronicznego, jeśli jest dostępna. Pozwoli to zachować wątku interfejsu użytkownika, odblokowany, które pomogą w ulepszaniu środowiska użytkownika w aplikacji.

Ponadto długotrwałe operacje powinna zostać wykonana na wątku w tle, aby uniknąć zablokowania wątku interfejsu użytkownika. Udostępnia .NET `async` i `await` słów kluczowych, które umożliwić zapis asynchroniczny kod, który wykonuje długotrwałe operacje wątku w tle i uzyskuje dostęp do wyników po zakończeniu. Jednakże podczas długotrwałe operacje mogą być wykonywane asynchronicznie z `await` — słowo kluczowe, to nie gwarantuje, że operacja będzie uruchamiana na wątku w tle. Zamiast tego można to zrobić przez przekazanie długotrwałej operacji `Task.Run`, jak pokazano w poniższym przykładzie:

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

`RecognizeFace` Metoda jest wykonywana na wątku w tle z `RecognizeFaceButtonClick` metody oczekiwania do czasu `RecognizeFace` metoda kończy się przed kontynuowaniem.

Długotrwałe operacje powinien obsługiwać anulowania. Na przykład kontynuowanie długotrwałej operacji mogą stać się zbędne, jeśli użytkownik przechodzi w aplikacji. Wzorzec wykonywania anulowania jest następujący:

- Utwórz `CancellationTokenSource` wystąpienia. To wystąpienie zostanie zarządzania i wysyłania powiadomień do anulowania.
- Przekaż `CancellationTokenSource.Token` wartości właściwości do każdego zadania, które powinny istnieć możliwość anulowania.
- Mechanizm dla każdego zadania odpowiada do anulowania.
- Wywołanie `CancellationTokenSource.Cancel` metodę w celu zapewnienia anulowania powiadomień.

> [!IMPORTANT]
> `CancellationTokenSource` Klasa implementuje `IDisposable` interfejsu i dlatego element `CancellationTokenSource.Dispose` metoda powinna być wywoływana raz `CancellationTokenSource` wystąpienie znajduje się symbol.



Aby uzyskać więcej informacji, zobacz [Przegląd pomocy technicznej Async](~/cross-platform/platform/async.md).

<a name="sgen" />

## <a name="use-the-sgen-garbage-collector"></a>Użyj modułu zbierającego elementy bezużyteczne SGen

Zarządzane języków, takich jak C# Użyj zbierania elementów bezużytecznych odzyskiwanie pamięci przydzielonej dla obiektów, które nie są już w użyciu. Są dwa moduły zbierające pamięci używane przez platformy Xamarin:

- [**SGen** ](http://www.mono-project.com/docs/advanced/garbage-collector/sgen/) — jest to pokoleniowej modułu zbierającego elementy bezużyteczne, moduł zbierający elementy bezużyteczne domyślnej na platformie Xamarin.
- [**Boehm** ](http://www.hboehm.info/gc/) — jest to zachowawcze, pokoleniowej modułu zbierającego elementy bezużyteczne. Jest moduł zbierający elementy bezużyteczne domyślna używana dla aplikacji platformy Xamarin.iOS, które korzystają z klasycznego interfejsu API.

SGen — korzysta z jednego z trzech stosów, aby przydzielić miejsce dla obiektów:

-  **Szkółki** — jest to, gdy są przydzielone nowe małych obiektów. Gdy szkółki zabraknie miejsca, nastąpi pomocnicza wyrzucania elementów bezużytecznych. Wszystkie obiekty na żywo zostanie przeniesiona do głównych stosu.
-  **Główna sterty** — jest to, gdzie są przechowywane długotrwała obiektów. Jeśli w stercie głównych nie ma wystarczającej ilości pamięci, głównych wyrzucania elementów bezużytecznych zostanie przeprowadzona. Jeśli główna wyrzucania elementów bezużytecznych nie powiedzie się w celu zwolnienia wystarczającej ilości pamięci, a następnie SGen poprosi większej ilości pamięci systemu.
-  **Duże miejsce obiektu** — jest to, gdzie są przechowywane obiekty, które wymagają więcej niż 8000 bajtów. Duże obiekty nie rozpocznie w szkółki, ale zamiast tego zostanie przydzielone w tym stosie.

Jedną z zalet SGen — jest to czas potrzebny do wykonania pomocnicza wyrzucania elementów bezużytecznych proporcjonalna do liczby nowych obiektów na żywo, utworzonych od ostatniego zebrania pomocnicza pamięci. Zmniejsza to wpływ operacji wyrzucania elementów bezużytecznych na wydajność aplikacji, jak te pomocnicza wyrzucania będzie trwać krócej niż głównych wyrzucania elementów bezużytecznych. Główne wyrzucania będą nadal występują, ale rzadziej.

### <a name="reducing-pressure-on-the-garbage-collector"></a>Zmniejszenie nacisk na moduł zbierający elementy bezużyteczne

Po uruchomieniu wyrzucania elementów bezużytecznych SGen przestanie ona wątków aplikacji podczas jej zwraca pamięci. Gdy jest on odzyskać pamięci, może wystąpić krótkie Wstrzymaj lub odtwarzanie przerywane w interfejsie użytkownika aplikacji. Jak odczuwa jest ta przerwa jest zależna od dwa składniki:

1. **Częstotliwość** — jak często występuje wyrzucanie elementów bezużytecznych. Częstotliwość wyrzucania spowoduje zwiększenie ilości dostępnej pamięci jest przydzielony między kolekcji.
1. **Czas trwania** — jak długo trwa każdego poszczególnych wyrzucanie elementów bezużytecznych. Jest to około proporcjonalna do liczby obiektów na żywo, które są zbierane.

Zbiorczo oznacza to, że jeśli wiele obiektów są przydzielone, ale nie pozostaną aktywne, będzie wiele krótkich wyrzucania. Z drugiej strony Jeśli obiekty pozostaną aktywne nowe obiekty są przydzielane powoli, będzie mniej, ale dłużej wyrzucania.

Aby zmniejszyć nacisk na moduł garbage collector, skorzystaj z następujących wskazówek:

- Unikaj wyrzucanie elementów bezużytecznych w pętli ścisłej przy użyciu obiektu pule. Jest to szczególnie istotne dla gier, które należy utworzyć większość w nich obiekty z wyprzedzeniem.
- Jawnego zwolnienia zasobów, takich jak strumienie, połączeń sieciowych, duże bloki pamięci i plików, gdy nie są już wymagane. Aby uzyskać więcej informacji, zobacz [wersji interfejsu IDisposable zasobów](#idisposable).
- Usuń zaznaczenie pola Zarejestruj procedury obsługi zdarzeń, gdy nie są już wymagane, aby obiekty do zebrania. Aby uzyskać więcej informacji, zobacz [anulowania subskrypcji zdarzeń](#events).

<a name="linker" />

## <a name="reduce-the-size-of-the-application"></a>Zmniejsz rozmiar aplikacji.

Należy wziąć pod uwagę procesu kompilacji na każdej platformie, aby zrozumieć, z której pochodzi rozmiar pliku wykonywalnego aplikacji:

- aplikacji systemu iOS są z wyprzedzeniem o czasu (drzewa obiektów aplikacji) do języka asemblera ARM. .NET framework jest dostępny, z klasami nieużywane trwa wycięte tylko wtedy, gdy jest włączona opcja odpowiednie konsolidatora.
- Kompilowania na języku pośrednim (IL) i wchodzących w skład MonoVM i just in time (JIT) Kompilacja aplikacji systemu android. Klasy nieużywane framework zostaną wycięte tylko wtedy, gdy jest włączona opcja odpowiednie konsolidatora.
- Kompilowania do IL i wykonywane przez wbudowane środowisko uruchomieniowe aplikacji Windows Phone.

Ponadto jeśli aplikacja udostępnia zwiększone użycie typów ogólnych, a następnie dodatkowo spowoduje zwiększenie rozmiaru pliku wykonywalnego końcowego, ponieważ będzie zawierać natywnie skompilowany wersji ogólne możliwości.

Aby zmniejszyć rozmiar aplikacji platformy Xamarin obejmuje konsolidator jako część pakietu narzędzi kompilacji. Domyślnie konsolidator jest wyłączona, a musi być włączona w opcjach projektu dla aplikacji. W czasie kompilacji zostanie przeprowadzone analizy statycznej aplikacji, aby określić, które typy i elementy członkowskie, używanych przez aplikację. Następnie będzie możliwe usunięcie z aplikacji wszelkie niewykorzystane typy i metody.

Poniższy zrzut ekranu przedstawia konsolidator opcje programu Visual Studio dla komputerów Mac dla projektu platformy Xamarin.iOS:

![](memory-perf-best-practices-images/linker-options-ios.png)

Poniższy zrzut ekranu przedstawia konsolidator opcje programu Visual Studio dla komputerów Mac dla projektu platformy Xamarin.Android:

![](memory-perf-best-practices-images/linker-options-droid.png)

Konsolidator udostępnia trzy różne ustawienia kontrolowanie swojego zachowania:

-  **Nie zawierają łącza** — nie nieużywane typy i metody zostaną usunięte przez konsolidator. Ze względu na wydajność jest ustawieniem domyślnym dla kompilacje debugowania.
-  **Link tylko Framework SDK/pakiet SDK zestawy** — to ustawienie tylko zmniejsza rozmiar te zestawy, które są wysyłane przez Xamarin. Będzie to miało wpływu kodu użytkownika.
-  **Połącz wszystkie zestawy** — jest to bardziej agresywną optymalizacji, która będzie obowiązywać SDK zestawów i kodem użytkownika. Dla powiązania to spowoduje usunięcie zapasowego nieużywanych pól i każdego wystąpienia (lub powiązane obiekty) należy jaśniejszego zużywa mniej pamięci.

*Łączy wszystkie zestawy* powinny używać ostrożnie, ponieważ może spowodować jej uszkodzenie aplikacji w nieoczekiwany sposób. Analizę statyczną, które jest przeprowadzane przez konsolidator nie może poprawnie zidentyfikować cały kod, który jest wymagany, co powoduje zbyt dużą ilość kodu usuwana z skompilowanej aplikacji. Ta sytuacja będzie manifestu się tylko w czasie wykonywania, podczas gdy aplikacja przestaje działać. Z tego powodu należy dokładnie przetestować aplikację po zmianie zachowanie konsolidatora.

Jeśli testowania ujawnić, że konsolidator ma niepoprawnie usunięty klasy lub metoda można oznaczyć typu lub metody, które nie są wywoływane statycznie, ale są wymagane przez aplikację przy użyciu jednej z następujących atrybutów:

-  `Xamarin.iOS.Foundation.PreserveAttribute` — Ten atrybut jest dla projektów platformy Xamarin.iOS.
-  `Android.Runtime.PreserveAttribute` — Ten atrybut jest dla projektów platformy Xamarin.Android.

Na przykład może być konieczne w celu zachowania domyślnych konstruktorów typów, które są dynamicznie utworzyć wystąpienia. Ponadto użycie serializacji XML może wymagać zachowywanie właściwości typów.

Aby uzyskać więcej informacji, zobacz [konsolidatora dla systemu iOS](~/ios/deploy-test/linker.md) i [konsolidatora dla systemu Android](~/android/deploy-test/linker.md).

### <a name="additional-size-reduction-techniques"></a>Sposobów zmniejszania rozmiaru dodatkowe

Brak szerokiej gamy architektury Procesora tego zasilania urządzeń przenośnych. W związku z tym tworzy Xamarin.iOS i Xamarin.Android *fat plików binarnych* zawierających skompilowanej wersji aplikacji dla każdej architektury Procesora. Dzięki temu, że aplikacji mobilnej można uruchomić na urządzeniu niezależnie od architektury Procesora.

Poniższe kroki można jeszcze bardziej ograniczyć rozmiar pliku wykonywalnego aplikacji:

- Upewnij się, że jest generowany kompilacji wydania.
- Zmniejsz liczbę architektur aplikacji zaprojektowano pod kątem, aby uniknąć FAT tworzonym pliku binarnego.
- Upewnij się, że kompilator LLVM jest w użyciu, aby wygenerować bardziej zoptymalizowanego pliku wykonywalnego.
- Zmniejsz rozmiar zarządzanego kodu aplikacji. Można to zrobić przez włączenie konsolidator na każdy zestaw (*wszystkie łącza* dla projektów dla systemu iOS i *połączyć wszystkie zestawy* dla projektów w systemie Android).

Aplikacje dla systemu android również może zostać podzielony na oddzielnych APK dla każdego interfejsu ABI ("architecture").
Dowiedz się więcej na ten wpis w blogu: [jak do zachować swój Android App rozmiar w dół](http://motzcod.es/post/112072508362/how-to-keep-your-android-app-size-down).

<a name="optimizeimages" />

## <a name="optimize-image-resources"></a>Optymalizacja zasoby obrazów

Obrazy są niektóre z najdroższych zasobów korzystających z aplikacji, a często są przechwytywane w wysokiej rozdzielczości. Gdy spowoduje to utworzenie żywe obrazy pełne szczegółów, aplikacje, które zwykle wyświetlania tych obrazów wymagają więcej użycie procesora CPU zdekodować obrazu i większej ilości pamięci do przechowywania obrazu zdekodowana. Jest niepotrzebne zdekodować rozdzielczości obraz w pamięci, gdy będzie on skalowany w na mniejszy do wyświetlenia. Zamiast tego należy ograniczyć wpływ użycia i pamięć procesora CPU przez tworzenie wielu wersji rozpoznawania przechowywane obrazy, które zbliża się rozmiary przewidywane wyświetlania. Na przykład obraz wyświetlany w widoku listy należy najprawdopodobniej niższej rozdzielczości niż obraz wyświetlany w trybie pełnoekranowym. Ponadto skalowany w dół wersji obrazów o wysokiej rozdzielczości można załadować wydajnie wyświetlania wpływu minimalnej pamięci. Aby uzyskać więcej informacji, zobacz [obciążenia dużych map bitowych wydajnie](https://developer.xamarin.com/recipes/android/resources/general/load_large_bitmaps_efficiently/).

Niezależnie od rozdzielczość obrazu wyświetlania zasobów obrazu może znacznie zwiększyć zużycie pamięci w aplikacji. W związku z tym ich powinien zostać utworzony tylko gdy jest wymagana, a powinny zostać zwolnione, jak aplikacja nie wymaga już je.

<a name="activationperiod" />

## <a name="reduce-the-application-activation-period"></a>Zmniejsz okres aktywacji aplikacji

Wszystkie aplikacje *okres aktywacji*, która jest czas od uruchomienia aplikacji oraz gdy aplikacja jest gotowa do użycia. Ten okres aktywacji zapewnia użytkownikom ich wyglądu aplikacji i dlatego ważne jest, aby ograniczyć okres aktywacji i użytkownika z punktu widzenia użytkownika, aby uzyskanie pozytywnych wrażenie pierwszej aplikacji.

Przed jego początkowej interfejsu użytkownika są wyświetlane w aplikacji, powinien udostępniać ekran powitalny, aby poinformować użytkownika, że aplikacja jest uruchamiana. Jeśli aplikacja nie może szybko wyświetlić jego początkowej interfejsu użytkownika, pojawi się ekran powitalny należy poinformować użytkownika postępu przez okres aktywacji oferowanie zabezpieczenie, które aplikacji nie odpowiada. To zabezpieczenie można pasek postępu lub podobne formantu.

W okresie aktywacji aplikacji wykonywania logiki aktywacji, która często obejmuje ładowania i przetwarzania zasobów. Okres aktywacji można zmniejszyć za zapewnienie, że wymagane zasoby są zawarte w aplikacji, zamiast pobieranych zdalnie. Na przykład w niektórych sytuacjach może być odpowiednie w okresie aktywacji, aby załadować dane przechowywane lokalnie — symbol zastępczy. Następnie po początkowej interfejsu użytkownika jest wyświetlane, a użytkownik ma możliwość interakcji z aplikacją, symbol zastępczy danych mogą być stopniowo zastępowane ze źródła zdalnego. Ponadto aplikacja logiki aktywacji należy wykonywać tylko wtedy pracy, które są wymagane, aby umożliwić użytkownikowi rozpocząć korzystanie z aplikacji. Może to ułatwić Jeśli wybrano opcję opóźnienia ładowania następującej liczby dodatkowych zestawów, ponieważ zestawy są załadowane, są one używane po raz pierwszy.

<a name="webservicecommunication" />

## <a name="reduce-web-service-communication"></a>Zmniejsz komunikacji usługi sieci Web

Łączenie z usługą sieci web z aplikacji może mieć wpływ na wydajność aplikacji. Na przykład zwiększenie użycia przepustowości sieci spowoduje zwiększenie użycia baterii urządzenia. Ponadto użytkownicy mogą używać aplikacji w środowisku ograniczonej przepustowości. W związku z tym jest za pośrednictwem ograniczyć wykorzystanie przepustowości między aplikacją a usługą sieci web.

Jeden ze sposobów zmniejsza użycie przepustowości aplikacji jest Kompresuj dane przed przeniesieniem ich przez sieć. Jednak dodatkowe użycie procesora CPU w procesie kompresji również może spowodować użycie zwiększona baterii. W związku z tym ta zależnościami powinny być dokładnie oceniane przed podjęciem decyzji o przeniesienie skompresowane dane za pośrednictwem sieci.

Inny problem wziąć pod uwagę jest format danych, który przechodzi między aplikacją a usługą sieci web. Dwa podstawowe formaty są Extensible Markup Language (XML) i JavaScript Object Notation (JSON). Kod XML jest format wymiany danych tekstowych, tworzącego ładunków stosunkowo dużej ilości danych, ponieważ zawiera dużą liczbę formatowanie znaków. JSON to format wymiany danych tekstowych tworzącego ładunków compact danych, co powoduje mniejsze wymagania dotyczące przepustowości podczas wysyłania danych i odbierania danych. W związku z tym JSON jest często w formacie aplikacji dla urządzeń przenośnych.

Zaleca się podczas transferu danych między aplikacją a usługą sieci web za pomocą obiektów transfer danych (DTOs). DTO zawiera zestaw danych do przesyłania przez sieć. Wykorzystując DTOs większej ilości danych mogą zostać przesłane w jednym wywołaniu zdalnym, co może pomóc zmniejszyć liczbę wywołań zdalnych wykonanych przez aplikację. Ogólnie rzecz biorąc zdalne wywołanie wykonywania większy ładunek danych ma podobne ilość czasu jako wywołanie, które obsługuje tylko ładunku niewielkie zbiory danych.

Dane pobrane z usługi sieci web powinno być buforowane lokalnie, buforowane dane są używane zamiast wielokrotnie pobrane z usługi sieci web. Jednak przyjmując takie podejście odpowiedniego strategii buforowania należy również wdrożyć, aby zaktualizować dane w lokalnej pamięci podręcznej, jeśli zmieni się w usłudze sieci web.

## <a name="summary"></a>Podsumowanie

W tym artykule opisano i opisem techniki zwiększania wydajności aplikacji utworzony za pomocą platformy Xamarin. Zbiorczo te techniki znacznie zmniejszyć ilość pracy wykonywana przez Procesora i ilości pamięci używanej przez aplikację.

## <a name="related-links"></a>Linki pokrewne

- [Xamarin.iOS Performance](~/ios/deploy-test/performance.md)
- [Xamarin.Android Performance](~/android/deploy-test/performance.md)
- [Wprowadzenie do programu Xamarin](~/tools/profiler/index.md)
- [Xamarin.Forms Performance](~/xamarin-forms/deploy-test/performance.md)
- [Asynchroniczna pomoc techniczna — omówienie](~/cross-platform/platform/async.md)
- [Interfejs IDisposable](https://developer.xamarin.com/api/type/System.IDisposable/)
- [Unikanie typowych problemów w aplikacji platformy Xamarin (klip wideo)](https://university.xamarin.com/guestlectures/avoiding-common-pitfalls-in-xamarin-apps)
