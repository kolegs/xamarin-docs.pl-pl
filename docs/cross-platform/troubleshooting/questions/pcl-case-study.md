---
title: Rozwiązywanie problemów związanych z System.Diagnostics.Tracing i przepływu danych TPL
description: 'Analiza przypadku PCL: jak rozwiązać problemy związane z System.Diagnostics.Tracing przez pakiet NuGet przepływu danych TPL firmy Microsoft?'
ms.prod: xamarin
ms.assetid: 7986A556-382D-4D00-ACCF-3589B4029DE8
ms.technology: xamarin-cross-platform
ms.date: 04/17/2018
author: asb3993
ms.author: amburns
ms.openlocfilehash: b1b56b0e831edbb6327f3ca66f6ec8dc780b46f2
ms.sourcegitcommit: 775a7d1cbf04090eb75d0f822df57b8d8cff0c63
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/18/2018
---
# <a name="pcl-case-study-how-can-i-resolve-problems-related-to-systemdiagnosticstracing-for-the-microsoft-tpl-dataflow-nuget-package"></a>Analiza przypadku PCL: jak rozwiązać problemy związane z System.Diagnostics.Tracing przez pakiet NuGet przepływu danych TPL firmy Microsoft?

> [!IMPORTANT]
> Ten przykład określonego `System.Diagnostic.Tracing` już generuje błędy domyślnie najnowszej wersji programu Xamarin. Gdy obejście sugerowane będą nadal działać, należy pamiętać, że niektóre usterki wymienione w sekcji "warstwy błędy" zostały określone.
> Ponadto należy pamiętać, że .NET Standard jest teraz preferowany sposób wdrażania interfejsów API architektury .NET i platform.

## <a name="summary"></a>Podsumowanie

Xamarin.iOS i Xamarin.Android nie implementują 100% każdy profil PCL umożliwiająca jako odwołania. Dla wygody praktyczne w programie Visual Studio for Mac, Visual Studio i Menedżer pakietów NuGet, projekty Xamarin zezwala na używanie kilka profilów, które mają tylko _niekompletne_ implementacji. Na przykład Xamarin.iOS ani Xamarin.Android obecnie zawiera pełną implementację typu "System.Diagnostics.Tracing" PCL przestrzeni nazw. To ograniczenie powoduje trzy warstwy błędy podczas próby użycia domyślnie `portable-net45+win8+wpa81` wersji pakietu NuGet przepływu danych TPL firmy Microsoft.

## <a name="workaround-switch-the-app-project-to-reference-the-portable-net45win8wp8wpa81-version-of-the-tpl-dataflow-library"></a>Obejście problemu: Przełączać projekt aplikacji, aby odwołać `portable-net45+win8+wp8+wpa81` wersji biblioteki przepływu danych TPL

(Unika wszystkie trzy warstwy błędów i działa w przypadku wszystkich aktualnych wersji programu Xamarin).

1. Otwórz projekt aplikacji **.csproj** plik w edytorze tekstów.

2. Znajdź wiersz podobny do poniższego:

    ```xml
    <HintPath>..\packages\Microsoft.Tpl.Dataflow.4.5.24\lib\portable-net45+win8+wpa81\System.Threading.Tasks.Dataflow.dll</HintPath>
    ```

3. Zmień `portable-net45+win8+wpa81` do `portable-net45+win8+wp8+wpa81` (`+wp8` zostanie dodany):

    ```xml
    <HintPath>..\packages\System.Threading.Tasks.Dataflow.4.5.25\lib\portable-net45+win8+wp8+wpa81\System.Threading.Tasks.Dataflow.dll</HintPath>
    ```

### <a name="explanation"></a>Wyjaśnienie

`portable-net45+win8+wp8+wpa81` Wersji biblioteki nie odwołuje się **System.Diagnostics.Tracing.dll** _na wszystkich_, przez co pozwala uniknąć całkowicie wszystkie trzy warstwy problemów.

### <a name="limitations"></a>Ograniczenia

- `portable-net45+win8+wp8+wpa81` Wersji biblioteki nie może zawierać 100% funkcji `portable-net45+win8+wpa81` wersji.

- Menedżer pakietów NuGet instaluje `portable-net45+win8+wpa81` wersji pakietu PCL NuGet domyślnie, dlatego należy dostosować odwołanie ręcznie.

## <a name="details-about-the-three-layers-of-errors"></a>Szczegółowe informacje o trzy warstwy błędów

1. **System.Diagnostics.Tracing.dll** fasad zestawu jest obecnie znajduje się poza wszystkie wersje Mac Xamarin.Android (niepublicznych usterki 34888) i nie istnieją ze wszystkich wersji platformy Xamarin.iOS niższy niż 9.0 (lub niższy niż XamarinVS 3.11.1443 w systemie Windows) (ustalone w [usterki 32388](https://bugzilla.xamarin.com/show_bug.cgi?id=32388)). Ten problem spowoduje, że jedną z następujących błędów w zależności od celu wdrożenia i konsolidatora ustawienia:

    - Xamarin.Android.Common.targets: Błąd: wyjątek podczas ładowania zestawów: System.IO.FileNotFoundException: nie można załadować zestawu "System.Diagnostics.Tracing, Version = 4.0.0.0, Culture = neutral, PublicKeyToken = b03f5f7f11d50a3a". Prawdopodobnie nie istnieje w Mono profilu Android?

    - Nie można załadować pliku lub zestawu 'System.Diagnostics.Tracing' lub jednej z jego zależności. W systemie nie można odnaleźć określonego pliku. (System.IO.FileNotFoundException)

    - MTOUCH: błąd MT3001: można drzewa nie obiektów aplikacji zestawu "/ Users/macuser/Projects/TPLDataflow/UnifiedSingleViewIphone1/obj/iPhone/Debug/mtouch-cache/64/Build/System.Threading.Tasks.Dataflow.dll"

    - MTOUCH: błąd MT2002: nie można rozpoznać zestawu: "System.Diagnostics.Tracing, wersja = 4.0.0.0, Culture = neutral, PublicKeyToken = b03f5f7f11d50a3a"

2. Bieżący [Mono implementacji typu "System.Diagnostics.Tracing"](https://github.com/mono/mono/blob/master/mcs/class/corlib/System.Diagnostics.Tracing/EventSource.cs) brakuje niektórych przeciążenia metody ([usterki 27337](https://bugzilla.xamarin.com/show_bug.cgi?id=27337)). Ten problem spowoduje, że jeden z następujących błędów konsolidatora podczas tworzenia aplikacji platformy Xamarin:

    - / Library/Frameworks/Mono.framework/External/xbuild/Xamarin/Android/Xamarin.Android.Common.targets: błąd: błąd podczas wykonywania zadań LinkAssemblies: błąd XA2006: odwołanie do metadanych elementu "System.Void System.Diagnostics.Tracing.EventSource:: WriteEvent(System.Int32,System.Object[]) "(zdefiniowane w" System.Threading.Tasks.Dataflow, wersja = 4.5.24.0, Culture = neutral, PublicKeyToken = b03f5f7f11d50a3a ") z" System.Threading.Tasks.Dataflow, wersja 4.5.24.0, Culture = = neutral, PublicKeyToken = b03f5f7f11d50a3a "nie można rozpoznać.

    - MTOUCH: błąd MT2002: nie można rozpoznać odwołania "System.Void System.Diagnostics.Tracing.EventSource::WriteEvent(System.Int32,System.Object[])" z "System.Diagnostics.Tracing, Version = 4.0.0.0, Culture = neutral, PublicKeyToken = b03f5f7f11d50a3a"

3. Bieżący [Mono implementacji typu "System.Diagnostics.Tracing"](https://github.com/mono/mono/blob/master/mcs/class/corlib/System.Diagnostics.Tracing/EventSource.cs) jest również obecnie _pusty_ "zastępczego" implementacji ([usterki 34890](https://bugzilla.xamarin.com/show_bug.cgi?id=34890)). Próba użycia tych metod w czasie wykonywania w związku z tym mogą powodować nieoczekiwane rezultaty. Dla _określonego_ przypadku przepływu danych tpl firmy Microsoft, prawdopodobnie wywołań `WriteEvent(System.Int32,System.Object[])` nie są istotne dla większości zachowanie biblioteki, więc poprawkę "warstwy 2" ([usterki 27337](https://bugzilla.xamarin.com/show_bug.cgi?id=27337), dodanie pustego implementacje) będzie prawdopodobnie wystarczające w większości przypadków użycia przepływu danych TPL firmy Microsoft.

## <a name="questions--answers"></a>Pytania i odpowiedzi

### <a name="i-was-able-to-leave-linking-enabled-with-the-codeportable-net45win8wpa81code-version-of-the-library-on-older-versions-of-xamarinios-or-on-xamarinandroid-how-did-that-work"></a>Została pozostaw łączenie z włączoną <code>portable-net45+win8+wpa81</code> wersji biblioteki na starsze wersje platformy Xamarin.iOS lub platformy Xamarin.Android. Jak to zadziała?

#### <a name="answer"></a>Odpowiedzi

Jest _możliwe_ Aby uzyskać kompilacji do ukończenia "pomyślnie" (z łączenia włączone) w starszych wersji platformy Xamarin.iOS lub Xamarin.Android dla komputerów Mac, jeśli zawiera odwołanie do `System.Diagnostics.Tracing.dll` _odwołanie zestawu_ \[1\] zamiast _zestawu fasad_ \[2], ale Niestety nie jest to "Popraw" obejście tego problemu. Zestawy referencyjne są przeznaczone tylko do użycia podczas kompilowania _przenośnych bibliotek_, kod nie specyficzne dla platformy, takich jak aplikacje. Podjęto próbę _Uruchom_ kod źródłowy znajdujący się w zestawy odwołań (a nie tylko kompilacji na nim) jest może powodować nieoczekiwane wyniki. Poprawka poprawne będą Mono zespół Dodaj brakujące `WriteEvent(System.Int32,System.Object[])` przeciążenia do [ `EventSource` ](https://github.com/mono/mono/blob/master/mcs/class/corlib/System.Diagnostics.Tracing/EventSource.cs) typu ([usterki 27337](https://bugzilla.xamarin.com/show_bug.cgi?id=27337)). Teraz najlepszym rozwiązaniem jest, aby przełączyć się do `portable-net45+win8+wp8+wpa81` wersji biblioteki przepływu danych TPL firmy Microsoft zgodnie z opisem w powyższej sekcji rozwiązania.

(Dla każdego, kto może odczytu w tym artykule po przejrzeniu pokrewne starszej, briefer odpowiedzi z StackOverflow (<http://stackoverflow.com/a/23591322/2561894>), należy pamiętać, że został różnicy między zestawów odwołań fasad zestawu _nie_ określonych w tym miejscu.)

**\[1\] "Odwołanie zestawu" lokalizacje**

System Windows: `C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETPortable\v4.5\System.Diagnostics.Tracing.dll`

Mac (Mono): `/Library/Frameworks/Mono.framework/Versions/Current/lib/mono/xbuild-frameworks/.NETPortable/v4.5/System.Diagnostics.Tracing.dll`

**\[2\] lokalizacje "Fasady zestawu"**

System Windows: `C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETFramework\v4.5\Facades\System.Diagnostics.Tracing.dll`

Mac (Mono): `/Library/Frameworks/Mono.framework/Versions/Current/lib/mono/4.5/Facades/System.Diagnostics.Tracing.dll`


### <a name="will-it-help-if-i-manually-add-a-reference-to-the-systemdiagnosticstracing-facade-assembly"></a>Zostanie pomaga, jeśli ręcznie dodać odwołanie do zestawu fasad "System.Diagnostics.Tracing"?

_W szczególności może rozwiązać ten problem, wykonując te kroki 2?_

1. _Kopiuj `System.Diagnostics.Tracing.dll` fasad zestawu w folderze projektu aplikacji z jednej z następujących lokalizacji:_

    System Windows: `C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETFramework\v4.5\Facades\System.Diagnostics.Tracing.dll`

    Mac (Mono): `/Library/Frameworks/Mono.framework/Versions/Current/lib/mono/4.5/Facades/System.Diagnostics.Tracing.dll`

2. _Dodaj odwołanie do zestawu fasad w projekcie aplikacji platformy Xamarin.iOS lub platformy Xamarin.Android._

#### <a name="answer"></a>Odpowiedzi

Nie, to nie pomoże.

- Dla platformy Xamarin.iOS 9.0 lub dowolnego najnowszej wersji platformy Xamarin.Android w systemie Windows to rozwiązanie jest ściśle nadmiarowa i może spowodować błędy kompilacji, podobnie jak "zespół"System.Diagnostics.Tracing"o tej samej tożsamości został już zaimportowany.".

- Xamarin.iOS 8.10 lub niższy lub Xamarin.Android dla komputerów Mac, to rozwiązanie pomaga, ale _tylko_ problemu zestawu brakuje "warstwy 1". Będzie on _nie_ rozwiązać błędy konsolidatora "warstwy 2", dlatego nie jest kompletne rozwiązanie.

### <a name="can-i-use-the-systemdiagnosticstracing-nuget-packagehttpswwwnugetorgpackagessystemdiagnosticstracing-to-solve-the-problem"></a>Można użyć [pakietu System.Diagnostics.Tracing NuGet](https://www.nuget.org/packages/System.Diagnostics.Tracing/) rozwiązania tego problemu?

#### <a name="answer"></a>Odpowiedzi

Nie, pakiet NuGet 3.0 "System.Diagnostics.Tracing" obejmuje implementacje specyficzne dla platformy "DNXCore50" i "netcore50". Go jawnie _pomija_ implementacji ("MonoAndroid") platformy Xamarin.Android i Xamarin.iOS ("MonoTouch" i "xamarinios"). Oznacza to, że instalowanie pakietu _efektu_ dla platformy Xamarin.Android i Xamarin.iOS projektów. Pakiet NuGet przyjęto założenie, że obu tych platform zapewniają ich _własnych_ implementacji typów. To założenie jest "Popraw" w tym sensie, że ma Mono __ implementacji przestrzeni nazw, ale jako punkty opisanych w obszarze \#2 i \#3 "Szczegółowe informacje o trzy warstwy błędy" powyżej, Implementacja jest obecnie niekompletne. Tak więc będzie poprawki właściwe dla Mono zespołowi rozwiązać [usterki 27337](https://bugzilla.xamarin.com/show_bug.cgi?id=27337) i [usterki 34890](https://bugzilla.xamarin.com/show_bug.cgi?id=34890).

## <a name="next-steps"></a>Następne kroki

Aby uzyskać dodatkową pomoc, skontaktuj się z nami, lub jeśli ten problem pozostanie nawet po użyciu powyższe informacje, zobacz [jakie opcje pomocy technicznej są dostępne dla platformy Xamarin?](~/cross-platform/troubleshooting/support-options.md) informacji o opcjach kontaktu, masz sugestie, oraz jak Plik nowe usterki, w razie potrzeby.
