---
title: Wprowadzenie do Renderscript
description: Ten przewodnik wprowadza Renderscript i opis sposobu użycia wewnętrzne Renderscript interfejsu API w aplikacji platformy Xamarin.Android tego poziomu obiekty docelowe interfejsu API 17 lub nowszej.
ms.prod: xamarin
ms.assetid: 378793C7-5E3E-40E6-ABEE-BEAEF64E6A47
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 3331eb579f0aa2d7f29508773c588455c134f56a
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241191"
---
# <a name="an-introduction-to-renderscript"></a>Wprowadzenie do Renderscript

_Ten przewodnik wprowadza Renderscript i opis sposobu użycia wewnętrzne Renderscript interfejsu API w aplikacji platformy Xamarin.Android tego poziomu obiekty docelowe interfejsu API 17 lub nowszej._

## <a name="overview"></a>Omówienie

Renderscript to środowisko programowania utworzone przez firmę Google w celu poprawienia wydajności aplikacji dla systemu Android, które wymagają rozbudowane zasoby obliczeniowe. Jest niskopoziomowy odpowiedzialny o wysokiej wydajności na podstawie interfejsu API [C99](http://en.wikipedia.org/wiki/C99). Ponieważ jest niski poziom interfejsu API, który będzie uruchamiany na procesory CPU, procesory GPU lub DSPs, Renderscript jest przeznaczona dla aplikacji systemu Android, które może być konieczne, wykonaj jedną z następujących czynności:

* Grafika
* Przetwarzanie obrazów
* Szyfrowanie
* Przetwarzanie sygnału
* Procedury matematyczne

Użyje Renderscript `clang` i skompiluj skryptów, aby kod bajtowy maszyny wirtualnej niskiego poziomu, który zestawu APK jest zawarte w pakiecie. Gdy aplikacja jest uruchamiana po raz pierwszy, zostanie skompilowany kod bajtowy LLVM w kod maszynowy dla procesorów na urządzeniu. Ta architektura pozwala wykorzystać zalety kod maszynowy bez posiadają zapisywanie dla każdego procesora na urządzeniu samych aplikacji systemu Android.

Istnieją dwa składniki do procedury Renderscript:

1. **Środowisko uruchomieniowe Renderscript** &ndash; to natywnych interfejsów API, który jest odpowiedzialny za wykonywanie Renderscript. Dotyczy to również wszelkich Renderscripts przeznaczony dla aplikacji.

2. **Zarządzany otok z platformy systemu Android** &ndash; zarządzanych klas, które umożliwiają aplikacji systemu Android do kontroli i wchodzić w interakcje za pomocą środowiska uruchomieniowego Renderscript i skryptów. Oprócz klas struktura dostępna do kontrolowania środowiska uruchomieniowego Renderscript łańcucha narzędzi dla systemu Android poszukaj w kodzie źródłowym Renderscript i wygenerować zarządzanych klas otoki do użycia przez aplikację dla systemu Android.

Na poniższym diagramie przedstawiono relacje między tymi składnikami:

![Diagram pokazujący, jak platformy systemu Android wchodzi w interakcje ze środowiskiem uruchomieniowym Renderscript](renderscript-images/renderscript-01.png)

Istnieją trzy ważne pojęcia dotyczące korzystania z Renderscripts w aplikacji systemu Android:

1. **Kontekst** &ndash; A zarządzanych interfejsów API dostarczonych przez zestaw Android SDK, który przydziela zasoby do Renderscript i umożliwia aplikacji systemu Android przekazać i odbierać dane z Renderscript.

2. **A _obliczenia jądra_**  &ndash; znany także jako _jądra głównego_ lub _jądra_ten procedurę, która wykonuje pracę. Jądro jest bardzo podobne do funkcji C; jest to równoległego procedura, która będzie uruchamiana za pośrednictwem wszystkich danych w pamięci przydzielone.

3. **Przydzielona pamięć** &ndash; dane są przekazywane do i z jądra za pośrednictwem  _[alokacji](https://developer.xamarin.com/api/type/Android.Renderscripts.Allocation/)_. Jądra może mieć jeden zestaw danych wejściowych i/lub jeden wyjściowy alokacji.

[Android.Renderscripts](https://developer.xamarin.com/api/namespace/Android.Renderscripts/) przestrzeń nazw zawiera klasy do interakcji ze środowiskiem uruchomieniowym Renderscript. W szczególności [ `Renderscript` ](https://developer.xamarin.com/api/type/Android.Renderscripts.RenderScript/) klasy zarządzać cyklem życia i zasoby aparatu Renderscript. Aplikacja systemu Android musi zostać zainicjowany co najmniej jeden [ `Android.Renderscripts.Allocation` ](https://developer.xamarin.com/api/type/Android.Renderscripts.Allocation/) obiektów. Przydział jest zarządzany interfejs API, który jest odpowiedzialny za alokacji i uzyskiwania dostępu do pamięci, która jest współużytkowana przez aplikacji dla systemu Android i środowiska uruchomieniowego Renderscript. Zazwyczaj jeden alokacji jest tworzona dla danych wejściowych i opcjonalnie inną alokacji jest utworzona w celu przechowywania danych wyjściowych jądra. Aparat środowiska wykonawczego Renderscript i skojarzone zarządzanych klas otoki będzie zarządzanie dostępem do pamięci w posiadaniu przydziały, nie ma potrzeby dla Deweloper aplikacji dla systemu Android w celu wykonywania dodatkowych działań.

Przydział będzie zawierać jeden lub więcej [Android.Renderscripts.Elements](https://developer.xamarin.com/api/type/Android.Renderscripts.Element/).
Elementy są typem wyspecjalizowanym, które opisują dane w każdej alokacji.
Typ elementu danych wyjściowych alokacji muszą być zgodne typy elementu wejściowego. Podczas wykonywania, Renderscript będzie przejść przez każdy Element w danych wejściowych alokacji równolegle i zapisać wyniki w danych wyjściowych alokacji. Istnieją dwa rodzaje elementów:

- **wystarczy wpisać** &ndash; koncepcyjnie jest taki sam jak typ danych C `float` lub `char`.

- **Typ złożony** &ndash; tego typu jest podobny do C `struct`.

Aparat Renderscript będzie wykonywać sprawdzanie czasu wykonywania, aby upewnić się, że elementy w każdej alokacji są zgodne z co jest wymagane przez jądro. Jeśli typ danych elementów w alokacji niezgodny typ danych, który oczekuje jądra, zostanie zgłoszony wyjątek.

Wszystkie jądra Renderscript zostanie zawinięta przy typu, który jest elementem podrzędnym elementu [ `Android.Renderscripts.Script` ](https://developer.xamarin.com/api/type/Android.Renderscripts.Script/) klasy. `Script` Klasa jest używana, aby ustawić parametry dla Renderscript, ustaw odpowiedni `Allocations`, i uruchom Renderscript. Istnieją dwa `Script` podklasy w zestawie SDK dla systemu Android:


- **`Android.Renderscripts.ScriptIntrinsic`** &ndash; Niektóre bardziej typowe zadania Renderscript są dołączone do zestawu Android SDK i są dostępne przez podklasę [ScriptIntrinsic](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsic/) klasy. Nie ma potrzeby dla dewelopera podjąć dodatkowe kroki, aby używać tych skryptów w swoich aplikacji, jak są już udostępniane.

- **`ScriptC_XXXXX`** &ndash; Nazywane również _skrypty użytkownika_, są to skrypty, które są zapisywane przez deweloperów i spakowane w pliku APK. W czasie kompilacji dla systemu Android łańcuch narzędzi wygeneruje zarządzanych klas otoki zezwalające skryptów do użycia w aplikacji dla systemu Android.
  Nazwy tych wygenerowanych klas jest nazwą pliku Renderscript prefiksem `ScriptC_`. Pisanie i dołączanie skrypty użytkownika nie jest oficjalnie obsługiwana przez platformy Xamarin.Android i poza zakres tego podręcznika.

Z tych dwóch typów tylko `StringIntrinsic` jest obsługiwany przez rozszerzenie Xamarin.Android. Ten przewodnik będzie omawiać temat za pomocą skryptów wewnętrzne w aplikacji platformy Xamarin.Android.

## <a name="requirements"></a>Wymagania

Ten przewodnik jest przeznaczony dla aplikacji platformy Xamarin.Android tego poziomu docelowego interfejsu API 17 lub nowszej. Korzystanie z _skrypty użytkownika_ nieuwzględnione w tym przewodniku.

[Xamarin.Android V8 Support Library](https://www.nuget.org/packages/Xamarin.Android.Support.v8.RenderScript/) backports instrinsic Renderscript interfejsu API dla aplikacji przeznaczonych dla starszych wersji zestawu SDK systemu Android. Dodawanie tego pakietu do projektu Xamarin.Android powinna zezwalać na aplikacje tej docelowymi są starsze wersje zestawu Android SDK wykorzystywać wewnętrzne skryptów.

## <a name="using-intrinsic-renderscripts-in-xamarinandroid"></a>Za pomocą wewnętrznej Renderscripts platformie Xamarin.Android

Wewnętrzne skrypty są doskonałym sposobem wykonywania obliczeń zadaniami intensywnie korzystającymi z minimalną ilością dodatkowego kodu. Są one dostępne dostosowana do oferują optymalną wydajność w dużej części w wielu urządzeń.
Nie jest niczym niezwykłym wewnętrzne skrypt, aby uruchomić 10 razy szybciej niż kodu zarządzanego i 2 – 3 x czas po niż w przypadku niestandardowych implementacji C. Wiele scenariuszy typowe przetwarzania są objęte wewnętrzne skryptów. Ta lista wewnętrzne skryptów opisano skrypty bieżącej platformie Xamarin.Android:

- [ScriptIntrinsic3DLUT](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsic3DLUT//) &ndash; konwertuje RGB, aby RGBA przy użyciu tabeli odnośników 3D. 

- [ScriptIntrinsicBLAS](https://developer.android.com/reference/android/renderscript/ScriptIntrinsicBLAS.html) &ndash; wydajności Provideshigh Renderscript interfejsów API do [BLAS](http://www.netlib.org/blas/). BLAS (podstawowe podprogramy Algebry liniowej) są procedury, które zapewniają standardowe bloki konstrukcyjne umożliwiające wykonywanie podstawowych wektora i operacje na macierzach. 

- [ScriptIntrinsicBlend](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicBlend) &ndash; łączy ze sobą dwa alokacji.

- [ScriptIntrinsicBlur](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicBlur) &ndash; dotyczy rozmycia Gaussa alokacji.

- [ScriptIntrinsicColorMatrix](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicColorMatrix/) &ndash; stosuje macierzy kolorów do alokacji (czyli zmiana kolorów, Dostosuj hue).

- [ScriptIntrinsicConvolve3x3](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicConvolve3x3/) &ndash; dotyczy macierzy kolorów 3 x 3 alokacji.

- [ScriptIntrinsicConvolve5x5](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicConvolve5x5/) &ndash; dotyczy macierzy kolorów 5 x 5 alokacji.

- [ScriptIntrinsicHistogram](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicHistogram/) &ndash; filtr histogram wewnętrzne.

- [ScriptIntrinsicLUT](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicLUT/) &ndash; zastosowanie tabeli odnośników na kanał do buforu.

- [ScriptIntrinsicResize](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicResize/) &ndash; skryptów do wykonywania rozmiar alokacji 2D.

- [ScriptIntrinsicYuvToRGB](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicYuvToRGB/) &ndash; konwertuje buforu YUV RGB.

Zapoznaj się z dokumentacją interfejsu API, szczegółowe informacje na temat każdego z wewnętrznych skryptów.

Podstawowe kroki dotyczące korzystania z Renderscript w aplikacji systemu Android są opisane w dalszej części.

**Utwórz kontekst Renderscript** &ndash; [ `Renderscript` ](https://developer.xamarin.com/api/type/Android.Renderscripts.RenderScript/) klasa jest zarządzanych otokę kontekstu Renderscript będzie kontrolować inicjowania, zarządzanie zasobami i wyczyścić. Obiekt Renderscript jest tworzony przy użyciu `RenderScript.Create` fabryki metody, która pobiera kontekst dla systemu Android (na przykład działanie) jako parametr. Następujący wiersz kodu pokazuje, jak można zainicjować kontekstu Renderscript:

```csharp
Android.Renderscripts.RenderScript renderScript = RenderScript.Create(this);
```

**Tworzenie przydziałów** &ndash; zależności wewnętrzne skrypt może być konieczne utworzenie jednego lub dwóch `Allocation`s. [ `Android.Renderscripts.Allocation` ](https://developer.xamarin.com/api/type/Android.Renderscripts.Allocation/) Klasa ma kilka metod fabryki Pomoc dotycząca tworzenia wystąpienia alokacji wewnętrzna. Na przykład poniższy fragment kodu przedstawia sposób tworzenia alokacji dla map bitowych.

```csharp
Android.Graphics.Bitmap originalBitmap;
Android.Renderscripts.Allocation inputAllocation = Allocation.CreateFromBitmap(renderScript,
                                                     originalBitmap,
                                                     Allocation.MipmapControl.MipmapFull,
                                                     AllocationUsage.Script);
```

Często, będzie konieczne utworzenie `Allocation` do przechowywania danych wyjściowych skryptu. Ten poniższy fragment kodu przedstawia sposób użycia `Allocation.CreateTyped` element pomocniczy służący do tworzenia wystąpienia sekundy `Allocation` tego samego typu co oryginalny:

```csharp
Android.Renderscripts.Allocation outputAllocation = Allocation.CreateTyped(renderScript, inputAllocation.Type);
```

**Wystąpienia otoki skryptu** &ndash; klas otoki wewnętrzne skrypt powinien mieć metody pomocnicze (zazwyczaj nazywany `Create`) podczas tworzenia wystąpienia obiektu dla tego skryptu. Poniższy fragment kodu jest przykładem sposobu tworzenia wystąpienia `ScriptIntrinsicBlur` rozmycie obiektu. `Element.U8_4` Metody pomocnika spowoduje utworzenie elementu, który opisuje typ danych, który jest 4 pola wartości 8-bitową, nieoznaczona liczba całkowita, do przechowania danych `Bitmap` obiektu:

```csharp
Android.Renderscripts.ScriptIntrinsicBlur blurScript = ScriptIntrinsicBlur.Create(renderScript, Element.U8_4(renderScript));
```

**Przypisz Allocation(s), Ustaw parametry i uruchom skrypt** &ndash; `Script` klasa udostępnia `ForEach` metodę do uruchomienia w rzeczywistości Renderscript. Ta metoda będzie iteracyjnego `Element` w `Allocation` przechowujący dane wejściowe. W niektórych przypadkach może być konieczne zapewnienie `Allocation` przechowuje dane wyjściowe.
`ForEach` spowoduje zastąpienie zawartości danych wyjściowych alokacji. Aby prowadzić z fragmentów kodu z poprzednich kroków, w tym przykładzie pokazano, jak przypisać przydział danych wejściowych, ustaw parametr, a na koniec uruchom skrypt (Kopiowanie wyników danych wyjściowych alokacji):

```csharp
blurScript.SetInput(inputAllocation);
blurScript.SetRadius(25);  // Set a pamaeter
blurScript.ForEach(outputAllocation);
```

Możesz też chcieć zapoznaj się z [rozmycie obrazu Renderscript](https://github.com/xamarin/recipes/tree/master/Recipes/android/other_ux/drawing/blur_an_image_with_renderscript) przepisu, jest kompletny przykład sposobu użycia skryptu wewnętrzne platformie Xamarin.Android.

## <a name="summary"></a>Podsumowanie

Ten przewodnik wprowadzono Renderscript i jak z niej korzystać w aplikacji platformy Xamarin.Android. Omówiono pokrótce Renderscript jest i jak działa w aplikacji systemu Android. Niektóre z kluczowych składników w Renderscript i różnica między on opisany _skrypty użytkownika_ i _skrypty instrinsic_. Ponadto ten przewodnik omówiono kroki przy użyciu skryptu wewnętrzne w aplikacji platformy Xamarin.Android.



## <a name="related-links"></a>Linki pokrewne

- [Przestrzeń nazw Android.Renderscripts](https://developer.xamarin.com/api/namespace/Android.Renderscripts/)
- [Rozmycie obrazu Renderscript](https://github.com/xamarin/recipes/tree/master/Recipes/android/other_ux/drawing/blur_an_image_with_renderscript)
- [Renderscript](https://developer.android.com/guide/topics/renderscript/compute.html)
- [Samouczek: Wprowadzenie do Renderscript](https://software.intel.com/en-us/articles/renderscript-basic-sample-for-android-os)
