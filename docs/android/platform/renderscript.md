---
title: Wprowadzenie do Renderscript
description: Ten przewodnik zawiera wprowadzenie Renderscript oraz wyjaśniono sposób używania wewnętrznej Renderscript interfejsu API w aplikacji platformy Xamarin.Android tego poziomu elementów docelowych interfejsu API 17 lub nowszej.
ms.prod: xamarin
ms.assetid: 378793C7-5E3E-40E6-ABEE-BEAEF64E6A47
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: f9e21a51c409c5444f137a63eb2c6fadfef03cbe
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="an-introduction-to-renderscript"></a>Wprowadzenie do Renderscript

_Ten przewodnik zawiera wprowadzenie Renderscript oraz wyjaśniono sposób używania wewnętrznej Renderscript interfejsu API w aplikacji platformy Xamarin.Android tego poziomu elementów docelowych interfejsu API 17 lub nowszej._

## <a name="overview"></a>Omówienie

Renderscript to architektura programowania utworzone przez firmę Google w celu poprawienia wydajności aplikacji systemu Android, które wymagają dużą ilością zasobów obliczeniowych. Jest niewielkie, wysoka wydajność interfejsu API na podstawie [C99](http://en.wikipedia.org/wiki/C99). Niski poziom interfejsu API, który będzie uruchamiany na procesorach GPU i DSPs, dlatego Renderscript jest odpowiednia dla aplikacji systemu Android, które może trzeba będzie wykonać jedną z następujących czynności:

* Grafika
* Przetwarzania obrazów
* Szyfrowanie
* Przetwarzanie sygnału
* Procedury matematyczne

Użyje Renderscript `clang` i skompilować na kod bajtowy LLVM, które jest połączone w plik APK skryptów. Gdy aplikacja jest uruchamiana po raz pierwszy, zostanie skompilowany kod bajtowy LLVM w kod maszynowy dla procesorów na urządzeniu. Taka architektura umożliwia aplikacji systemu Android posiadają zapisania go dla każdego procesora na urządzeniu się wykorzystać zalety kod maszynowy bez.

Istnieją dwa składniki do procedury Renderscript:

1. **Środowisko uruchomieniowe Renderscript** &ndash; to macierzystych interfejsów API, które służą do wykonywania Renderscript. W tym wszelkie Renderscripts napisane dla aplikacji.

2. **Zarządzany otok z platformy systemu Android** &ndash; zarządzanych klas, które umożliwia aplikacji systemu Android do kontrolowania i interakcji z Renderscript środowiska uruchomieniowego i skryptów. Oprócz klasy struktura dostępna dla środowiska uruchomieniowego Renderscript kontrolowanie Android łańcuch narzędzi zbadanie kodu źródłowego Renderscript i wygenerować otoki zarządzanej klasy do użycia przez aplikację systemu Android.

Na poniższym diagramie przedstawiono, jak te składniki są powiązane:

![Diagram pokazujący, jak platformy systemu Android współdziała ze środowiskiem uruchomieniowym Renderscript](renderscript-images/renderscript-01.png)

Istnieją trzy ważne pojęcia dotyczące korzystania z Renderscripts w aplikacji systemu Android:

1. **Kontekst** &ndash; A zarządzane udostępniane przez zestaw SDK systemu Android, która przydzielania zasobów do Renderscript i umożliwia aplikacji systemu Android przekazać i odbierać dane z Renderscript interfejsu API.

2. **A _obliczeniowe jądra_**  &ndash; znanej także jako _jądra głównego_ lub _jądra_, ta procedura, która wykonuje pracę. Jądra jest bardzo podobne do funkcji C; jest działania równoległego procedury, które będą uruchamiane przez wszystkie dane w alokacji pamięci.

3. **Pamięci przydzielonej** &ndash; dane są przekazywane do i z jądra za pośrednictwem  _[alokacji](https://developer.xamarin.com/api/type/Android.Renderscripts.Allocation/)_. Jądra może mieć jedno wejście i/lub jeden wyjściowy alokacji.

[Android.Renderscripts](https://developer.xamarin.com/api/namespace/Android.Renderscripts/) przestrzeń nazw zawiera klasy do interakcji z Renderscript środowiska uruchomieniowego. W szczególności [ `Renderscript` ](https://developer.xamarin.com/api/type/Android.Renderscripts.RenderScript/) klasy będzie Zarządzanie cyklem życia i zasobami Renderscript aparatu. Należy zainicjować aplikacji systemu Android, co najmniej jeden [ `Android.Renderscripts.Allocation` ](https://developer.xamarin.com/api/type/Android.Renderscripts.Allocation/) obiektów. Alokacja jest zarządzanego interfejsu API, który jest odpowiedzialny za alokacji i uzyskiwania dostępu do pamięci, która jest udostępniana między aplikacji systemu Android i środowiska uruchomieniowego Renderscript. Zazwyczaj dla danych wejściowych jest tworzony co alokacji i Alokacja opcjonalnie innego zostanie utworzony do przechowywania danych wyjściowych jądra. Aparat środowiska wykonawczego Renderscript i klasy skojarzone zarządzany otok będzie zarządzanie dostępem do pamięci w posiadaniu alokacje, nie istnieje potrzeba dla deweloperów aplikacji systemu Android wykonać dodatkowe pracę.

Alokacja będzie zawierać co najmniej jeden [Android.Renderscripts.Elements](https://developer.xamarin.com/api/type/Android.Renderscripts.Element/).
Elementy są specjalistyczną odmianą, które opisują dane w każdej alokacji.
Typ elementu danych wyjściowych alokacji muszą być zgodne typy elementu wejściowego. Podczas wykonywania, Renderscript będzie iteracja każdy Element wejściowy alokacji równolegle i zapisać wyniki z danymi wyjściowymi alokacji. Istnieją dwa typy elementów:

- **Typ prosty** &ndash; koncepcyjnie jest identyczny z typem danych C `float` lub `char`.

- **Typ złożony** &ndash; tego typu jest podobny do C `struct`.

Aparat Renderscript będzie wykonywać sprawdzanie czasu wykonywania, aby upewnić się, że elementy w każdej alokacji są zgodne z co jest wymagane przez jądro. Jeśli typ danych elementów w alokacji niezgodny typ danych, który oczekuje jądra, zostanie zgłoszony wyjątek.

Wszystkie jądra Renderscript zostaną opakowane przez typ, który jest elementem potomnym [ `Android.Renderscripts.Script` ](https://developer.xamarin.com/api/type/Android.Renderscripts.Script/) klasy. `Script` Klasa jest używana do Ustaw parametry Renderscript, ustaw odpowiednie `Allocations`, i uruchom Renderscript. Istnieją dwa `Script` podklasy w zestawie SDK systemu Android:


- **`Android.Renderscripts.ScriptIntrinsic`** &ndash; Niektóre z najczęściej zadań Renderscript są dołączone do zestawu SDK systemu Android i są dostępne przez podklasę [ScriptIntrinsic](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsic/) klasy. Nie jest wymagane dla dewelopera mają żadnych dodatkowych czynności, aby użyć te skrypty w aplikacjach, jak są już udostępniane.

- **`ScriptC_XXXXX`** &ndash; Znany również jako _skryptów użytkownika_, są skrypty, które są zapisywane przez deweloperów i pakowane w plik APK. W czasie kompilacji Android łańcuch narzędzi wygeneruje klasy otoki zarządzanych, które umożliwią skryptów do użycia w aplikacji systemu Android.
  Nazwa te wygenerowane klasy jest nazwa pliku Renderscript, prefiksem `ScriptC_`. Zapisywanie i zawierających skryptów użytkownika nie jest oficjalnie obsługiwana przez platformy Xamarin.Android i poza zakres tego podręcznika.

Z tych dwóch typów, tylko `StringIntrinsic` jest obsługiwana przez platformy Xamarin.Android. W tym przewodniku będzie omawiać temat Używanie skryptów wewnętrzne w aplikacji platformy Xamarin.Android.

## <a name="requirements"></a>Wymagania

Ten przewodnik jest przeznaczony dla aplikacji platformy Xamarin.Android tego poziomu docelowego interfejsu API 17 lub nowszej. Korzystanie z _skryptów użytkownika_ nie jest uwzględnione w tym przewodniku.

[Biblioteka obsługi V8 Xamarin.Android](https://www.nuget.org/packages/Xamarin.Android.Support.v8.RenderScript/) backports instrinsic Renderscript interfejsu API dla aplikacji, które odnoszą się do starszych wersji zestawu SDK systemu Android. Dodawanie tego pakietu do projektu platformy Xamarin.Android powinna zezwalać na aplikacje starszych wersji docelowej tego zestawu SDK systemu Android wykorzystać wewnętrzne skryptów.

## <a name="using-intrinsic-renderscripts-in-xamarinandroid"></a>Za pomocą wewnętrznej Renderscripts platformie Xamarin.Android

Wewnętrzne skrypty są doskonały sposób wykonywania zadań intensywnie korzystających z minimalną ilością dodatkowy kod. Zostały ręcznie dostosowana do oferty optymalnej wydajności dużych sekcję między urządzeniami.
Nie jest rzadko wewnętrzne skryptu do uruchomienia 10 x szybsze niż kodu zarządzanego i 2-3 razy x po niż implementacja niestandardowa C. Wiele scenariusze przetwarzania typowych są objęte wewnętrzne skryptów. Ta lista wewnętrzna skryptów opisano skrypty bieżącej platformie Xamarin.Android:

- [ScriptIntrinsic3DLUT](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsic3DLUT//) &ndash; konwertuje RGB do RGBA przy użyciu tabeli odnośników 3D. 

- [ScriptIntrinsicBLAS](https://developer.android.com/reference/android/renderscript/ScriptIntrinsicBLAS.html) &ndash; Provideshigh wydajności Renderscript interfejsów API do [BLAS](http://www.netlib.org/blas/). BLAS (podstawowe algebraiczną liniowy podprogramy) są procedury, które zapewniają standardowych bloków konstrukcyjnych do wykonywania operacji macierzy i wektor podstawowe. 

- [ScriptIntrinsicBlend](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicBlend) &ndash; razem miesza dwa alokacji.

- [ScriptIntrinsicBlur](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicBlur) &ndash; dotyczy rozmycia Gaussa alokacji.

- [ScriptIntrinsicColorMatrix](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicColorMatrix/) &ndash; stosuje macierzy kolorów do alokacji (np. zmiana kolorów, Dostosuj hue).

- [ScriptIntrinsicConvolve3x3](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicConvolve3x3/) &ndash; dotyczy macierzy kolorów 3 x 3 alokacji.

- [ScriptIntrinsicConvolve5x5](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicConvolve5x5/) &ndash; dotyczy macierzy kolorów 5 x 5 alokacji.

- [ScriptIntrinsicHistogram](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicHistogram/) &ndash; filtr histogram wewnętrznej.

- [ScriptIntrinsicLUT](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicLUT/) &ndash; dotyczy tabeli odnośników na kanał buforu.

- [ScriptIntrinsicResize](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicResize/) &ndash; skryptu do wykonania zmiany rozmiaru 2D alokacji.

- [ScriptIntrinsicYuvToRGB](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicYuvToRGB/) &ndash; konwertuje buforu YUV RGB.

Szczegółowe informacje na temat każdego z wewnętrznych skryptów, zapoznaj się z dokumentacją interfejsu API.

Podstawowe czynności w przypadku używania Renderscript w aplikacji systemu Android są opisane dalej.

**Tworzenie kontekstu Renderscript** &ndash; [ `Renderscript` ](https://developer.xamarin.com/api/type/Android.Renderscripts.RenderScript/) klasy jest zarządzany otokę kontekstu Renderscript będzie kontroli inicjowania, zarządzanie zasobami i wyczyścić. Obiekt Renderscript jest tworzony przy użyciu `RenderScript.Create` fabryki metody pobierającej kontekst dla systemu Android (na przykład działanie) jako parametr. Następujący wiersz kodu pokazano, jak zainicjować kontekstu Renderscript:

```csharp
Android.Renderscripts.RenderScript renderScript = RenderScript.Create(this);
```

**Tworzenie przydziałów** &ndash; w zależności od wewnętrznej skryptu, może być konieczne utworzyć co najmniej dwa `Allocation`s. [ `Android.Renderscripts.Allocation` ](https://developer.xamarin.com/api/type/Android.Renderscripts.Allocation/) Klasa ma kilka metod fabryki ułatwiające tworzenie wystąpień alokacji dla wewnętrznych. Na przykład poniższy fragment kodu przedstawia sposób tworzenia alokacji bitmap.

```csharp
Android.Graphics.Bitmap originalBitmap;
Android.Renderscripts.Allocation inputAllocation = Allocation.CreateFromBitmap(renderScript,
                                                     originalBitmap,
                                                     Allocation.MipmapControl.MipmapFull,
                                                     AllocationUsage.Script);
```

Często, będzie konieczne utworzenie `Allocation` do przechowywania danych wyjściowych skryptu. Ta poniższy fragment kodu przedstawia sposób użycia `Allocation.CreateTyped` pomocnika do utworzenia wystąpienia drugiej `Allocation` aby taki sam typ jak oryginał:

```csharp
Android.Renderscripts.Allocation outputAllocation = Allocation.CreateTyped(renderScript, inputAllocation.Type);
```

**Utwórz wystąpienie otoki skryptu** &ndash; klas otoki skryptu wewnętrzne powinny mieć metody pomocnicze (zwykle nazywane `Create`) dla wystąpienia obiektu dla tego skryptu. Poniższy fragment kodu przedstawiono przykładowy sposób tworzenia wystąpienia `ScriptIntrinsicBlur` rozmycia obiektu. `Element.U8_4` Metody pomocnika zostanie utworzony Element, który opisuje typ danych 4 pola wartości całkowite 8-bitową, bez znaku, do przechowania danych `Bitmap` obiektu:

```csharp
Android.Renderscripts.ScriptIntrinsicBlur blurScript = ScriptIntrinsicBlur.Create(renderScript, Element.U8_4(renderScript));
```

**Przypisz Allocation(s), Ustaw parametry i uruchom skrypt** &ndash; `Script` klasa udostępnia `ForEach` metodę Renderscript aktualnie ma uruchomiony. Ta metoda będzie iteracja każdego `Element` w `Allocation` zawierający dane wejściowe. W niektórych przypadkach może być konieczne w celu zapewnienia `Allocation` przechowuje dane wyjściowe.
`ForEach` spowoduje zastąpienie zawartości wyjściowej alokacji. Do wykonania z fragmentów kodu z poprzednich kroków, w tym przykładzie pokazano, jak przypisać wejściowych alokacji, ustaw parametr i na koniec uruchom skrypt (Kopiowanie wyników z danymi wyjściowymi alokacji):

```csharp
blurScript.SetInput(inputAllocation);
blurScript.SetRadius(25);  // Set a pamaeter
blurScript.ForEach(outputAllocation);
```

Możesz wyewidencjonować [rozmywa obrazu z Renderscript](https://developer.xamarin.com/recipes/android/other_ux/drawing/blur_an_image_with_renderscript/) przepisu, jest pełny przykład można użyć skryptu wewnętrzne platformie Xamarin.Android.

## <a name="summary"></a>Podsumowanie

W tym przewodniku wprowadzono Renderscript i jak z niego korzystać w aplikacji platformy Xamarin.Android. Omówiono pokrótce Renderscript jest i jak działa w aplikacji systemu Android. Go opisano niektóre najważniejsze składniki Renderscript i różnica między _skryptów użytkownika_ i _skryptów instrinsic_. Na koniec w tym przewodniku omówiono kroki przy użyciu skryptu wewnętrzne w aplikacji platformy Xamarin.Android.



## <a name="related-links"></a>Linki pokrewne

- [Przestrzeń nazw Android.Renderscripts](https://developer.xamarin.com/api/namespace/Android.Renderscripts/)
- [Rozmywa obrazu z Renderscript](https://developer.xamarin.com/recipes/android/other_ux/drawing/blur_an_image_with_renderscript/)
- [Renderscript](https://developer.android.com/guide/topics/renderscript/compute.html)
- [Samouczek: Wprowadzenie do Renderscript](https://software.intel.com/en-us/articles/renderscript-basic-sample-for-android-os)
