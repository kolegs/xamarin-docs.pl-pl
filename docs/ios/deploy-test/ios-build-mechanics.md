---
title: Mechanika kompilacji systemu iOS
description: Ten przewodnik opisuje sposób czasu aplikacji i sposób użycia metod, które można zastosować dla przyspieszają kompilacje dla wszystkich konfiguracji kompilacji.
ms.prod: xamarin
ms.assetid: 06FD3940-D666-4C9E-BC3E-BBE481EF8012
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: df84e78709b0ff16087c4bb9816c5d45f6ec33ed
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="ios-build-mechanics"></a>Mechanika kompilacji systemu iOS

_Ten przewodnik opisuje sposób czasu aplikacji i sposób użycia metod, które można zastosować dla przyspieszają kompilacje dla wszystkich konfiguracji kompilacji._

Tworzenie wspaniałych aplikacji jest więcej niż tylko pisanie kodu, który działa. Dobrze napisane aplikacji powinna zawierać optymalizacji, które wykonać szybsze kompilacji dla aplikacji, które są mniejsze i działają szybciej. Te optymalizacje nie będą tylko skutkować lepsze środowisko pracy użytkownika, ale także lub używająca w projekcie. Jest to niezbędne do zapewnienia, że podczas pracy z aplikacją wszystko przypada często. 

Należy pamiętać, że opcje domyślne są bezpieczne i szybkie, ale nie są optymalne dla każdej sytuacji. Ponadto wiele opcji może spowolnić albo przyspieszenia cykl programowania, w zależności od pojedynczego projektu. Na przykład natywnego usuwanie czas, ale jeśli zdobytych jest bardzo mały rozmiar następnie usuwanie czasu nie zostaną odzyskane przy szybsze wdrażanie. Z drugiej strony usuwanie natywnego można zmniejszyć aplikacji znacznie, w tym przypadku będzie szybsze do wdrożenia. To wygląda różnie w projektach, a jedynym sposobem, aby wiedzieć, służy do testowania.

Szybkość kompilacji Xamarin również mogą mieć wpływ różne możliwości i możliwości komputera niż może mieć wpływ na wydajność: możliwości procesora, szybkości magistrali, ilość fizycznej pamięci, szybkości dysku, szybkość sieci. Ograniczenia te wydajności wykraczają poza zakres tego dokumentu i są odpowiedzialne za dewelopera.


## <a name="timing-apps"></a>Czas aplikacji

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Aby włączyć dane wyjściowe diagnostyki programu MSBuild w programie Visual Studio dla komputerów Mac:

1. Kliknij przycisk **programu Visual Studio for Mac > Preferencje...**
2. W widoku drzewa po lewej stronie wybierz **projekty > kompilacji**
3. W okienku po prawej stronie, ustawić poziom szczegółowości dziennika listy rozwijanej **diagnostycznych**: [ ![ ] (ios-build-mechanics-images/image2.png "ustawienie szczegółowości dziennika")](ios-build-mechanics-images/image2.png#lightbox)
4. Kliknij przycisk **OK**
5. Uruchom ponownie program Visual Studio dla komputerów Mac
6. Czyszczenie i skompiluj ponownie pakiet
7. Wyświetl wyjście diagnostyczne w konsoli błędy (Widok > konsole > błędy) przez kliknięcie przycisku wyjścia kompilacji


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Aby włączyć dane wyjściowe diagnostyki programu MSBuild w programie Visual Studio:

1. Kliknij przycisk **Narzędzia > Opcje...**
2. W widoku drzewa po lewej stronie wybierz **projekty i rozwiązania > Tworzenie i uruchamianie**
3. W okienku po prawej stronie, ustaw *listy rozwijanej poziom szczegółowości danych wyjściowych kompilacji MSBuild* do **diagnostycznych**: [ ![ ] (ios-build-mechanics-images/image2-vs.png "ustawienie MSBuild dane wyjściowe kompilacji poziom szczegółowości")](ios-build-mechanics-images/image2-vs.png#lightbox)
4. Kliknij przycisk **OK**
5. Czyszczenie i skompiluj ponownie pakiet.
6. Dane wyjściowe diagnostyki jest widoczna w panelu wyjścia.

-----

## <a name="timing-mtouch"></a>Mtouch chronometrażu

Aby wyświetlić informacje dotyczące procesu kompilacji mtouch, należy przekazać `--time --time` do argumentów mtouch Twojego **opcje projektu**. Wyniki znajdują się w wyjścia kompilacji, wyszukując `MTouch` zadań:

```csharp
Setup: 36 ms
Resolve References: 982 ms
Extracted native link info: 987 ms
...
Total time: 1554 ms
```

## <a name="connecting-from-visual-studio-with-build-host"></a>Łączenie z programu Visual Studio z hostem kompilacji

Narzędzia platformy Xamarin technicznie działa na wszelkie Mac, można uruchomić OS X 10.10 Yosemite lub nowszym. Jednak projektanta uruchomień oraz czas kompilacji mogą napotykać przez wydajności komputerów Mac.

W stanie rozłączenia, Visual Studio w systemie Windows tylko wykonuje fazy kompilacji C# i nie jest podejmowana próba wykonania łączenie lub kompilacji drzewa obiektów aplikacji, pakietu dla aplikacji do _.app_ pakietu lub znak pakietu aplikacji. (Fazy kompilacji C# jest rzadko wąskie gardło). Próba określaniu, gdzie w potoku kompilacji jest spowolnieniem według budynków bezpośrednio na hoście kompilacji Mac w programie Visual Studio dla komputerów Mac.


Ponadto jest jednym z miejsc częściej dla sluggishness połączenie sieciowe między komputerem z systemem Windows i Mac hosta kompilacji. Może to być spowodowane fizycznych przeszkody w sieci za pomocą połączenia bezprzewodowego lub przemieszczać się za pośrednictwem nasycenia maszyny (na przykład usługi Mac w chmurze).

## <a name="simulator-tricks"></a>Symulator wskazówki

 
Podczas opracowywania aplikacji dla urządzeń przenośnych, jest niezbędne do szybkiego wdrażania kodu. Z różnych powodów takich jak szybkość i brak inicjowania obsługi wymagania dotyczące urządzeń deweloperzy często woli wdrażać wstępnie zainstalowanego symulatorze lub emulator. Dla producentów narzędzia dla deweloperów decyzji symulator lub emulator zawiera zależność między szybkości oraz zapewnia zgodność. 

Apple zapewnia symulatora dla opracowywania aplikacji systemu iOS, podwyższenie poziomu szybkość nad zgodności tworząc mniej restrykcyjnych środowiska uruchamiania kodu. To środowisko mniej restrykcyjnych umożliwia program Xamarin, aby używać tylko w czasie () przy użyciu kompilatora JIT dla symulator (w przeciwieństwie do [drzewa obiektów aplikacji](~/ios/internals/architecture.md) na urządzenie), co oznacza, że kompilacji jest skompilowanych do natywnego kodu w czasie wykonywania. Mac jest znacznie szybsze niż urządzenie, umożliwi to lepszą wydajność.

Symulator używa uruchamiania aplikacji udostępnionych, umożliwiając uruchamianie zostanie ponownie przeciwieństwie konstruowany każdorazowo, zgodnie z wymaganiami na urządzeniu.

Biorąc pod uwagę powyższe informacje, na poniższej liście daje niektóre informacje o krokach do wykonania podczas tworzenia i wdrażania aplikacji w symulatorze, aby zapewnić najlepszą wydajność.
 
### <a name="tips"></a>Porady

- Dla kompilacji: 
  - Usuń zaznaczenie pola wyboru **obrazów PNG zoptymalizować** opcji w obszarze Opcje projektu. Optymalizacja nie jest niezbędna dla kompilacji w symulatorze.
  - Ustaw konsolidator **łącze nie**. Wyłączanie konsolidator przebiega szybciej, ponieważ jej wykonanie zajmuje dużo czasu.
  - Wyłączanie uruchamiania aplikacji udostępnionych za pomocą `--nofastsim` flaga powoduje, że symulator kompilacji jest znacznie mniejsza. Usuń tę flagę, gdy nie jest już wymagane.
  - Przy użyciu natywnych bibliotek jest wolniejsze, ponieważ udostępnionego simlauncher głównego pliku wykonywalnego nie można użyć ponownie w takiej sytuacji i plik wykonywalny specyficzne dla aplikacji ma być tworzone dla każdej kompilacji.
- Do wdrożenia
  - Zawsze Zachowuj symulatora uruchomiony, gdy jest to możliwe. Symulator może potrwać maksymalnie 12 sekund zimny start.
- Dodatkowe porady
  - Preferuj kompilacji przed ponowną kompilację, ponieważ Rebuild czyści przed kompilacją. Czyszczenie może zająć dużo czasu, ponieważ spowoduje usunięcie odwołania, których można użyć.
  - Skorzystaj z faktu, że symulator nie wymusza piaskownicy. O dużych zasobów, takich jak pliki wideo i innych zasobów dołączony do projektu można utworzyć pliku kosztowne operacje kopiowania za każdym razem, gdy aplikacja jest uruchamiana w symulatorze. Uniknąć tych operacji kosztowne przez umieszczenie tych plików w katalogu macierzystym i odwoływać je w aplikacji przez pełną ścieżkę pliku.  
  - W razie wątpliwości, użyj `–time –time` flagi do mierzenia zmiany

Poniższy zrzut ekranu przedstawia sposób ustawić te opcje symulatora w opcjach iOS:

[![](ios-build-mechanics-images/image3.png "Ustawianie opcji")](ios-build-mechanics-images/image3.png#lightbox)

## <a name="device-tricks"></a>Lewy urządzenia

Wdrażanie na urządzeniu jest podobny do wdrażania na symulatorze, symulator jest mały podzbiór kompilacji używany dla urządzenia z systemem iOS. Budynek urządzenia wymaga dużo więcej czynności, ale ma tę zaletę, zapewniając dodatkowe możliwości w celu optymalizacji aplikacji.

### <a name="build-configurations"></a>Konfiguracje kompilacji

Istnieje wiele konfiguracji kompilacji podana podczas wdrażania aplikacji systemu iOS. Należy dysponować dobrą znajomością każdej konfiguracji, należy wiedzieć, kiedy i dlaczego należy optymalizacji.

 - Debugowanie
  - Jest to konfiguracja głównego, powinien być używany, gdy aplikacja jest w fazie projektowania, którą należy, w związku z tym, jak szybki, jak to możliwe.
 - Wydanie
  - Kompilacjami wydania są tymi, które są wysyłane do użytkowników i fokus na wydajność jest podstawowym. Podczas korzystania z wersji konfiguracji, można użyć kompilatora optymalizującego LLVM i zoptymalizować pliki PNG.

 
Jest również wziąć pod uwagę relacji między tworzenie i wdrażanie. Czas wdrażania jest funkcją rozmiar aplikacji. Większej aplikacji trwa dłużej niż do wdrożenia. Minimalizując rozmiar aplikacji, można skrócić czas wdrażania.

Minimalizowanie rozmiaru aplikacji pomaga również zmniejszyć czas kompilacji. Jest to spowodowane usunięciem kod z aplikacji trwa krócej niż natywnie kompilowanie kodu, które nie będą używane. Mniejsze pliki obiektu oznacza szybszej konsolidacji co powoduje mniejszy plik wykonywalny przy użyciu mniejszej liczby symboli do wygenerowania. Oszczędzanie miejsca, w związku z tym ma opłacalności double, który jest dlaczego **SDK łącze** jest tworzy domyślną dla wszystkich urządzeń. 

> [!NOTE]
> **SDK łącze** opcji mogą być widoczne tylko zestawy tylko łącze Framework zestawów SDK lub zestawu SDK łącze w zależności od używanego środowiska IDE.
 

### <a name="tips"></a>Porady

- Kompilacja: 
  - Tworzenie jednego architektura (np. ARM64) jest szybsza niż FAT binarnym (np. ARMv7 + ARM64)
  - Unikaj optymalizacji PNG, pliki podczas debugowania
  - Należy rozważyć łączenie wszystkich zestawów. Każdy zestaw optymalizacji 
  - Wyłączenie tworzenia symboli debugowania za pomocą `--dsym=false`. Należy jednak pamiętać, że wyłączenie to będzie oznaczać, że raporty awarii można tylko symbolicated na tej maszynie wbudowanej aplikacji i tylko wtedy, gdy aplikacja nie została usunięta.

 
Niektóre elementy, których należy unikać są:

- Pliki binarne FAT (debugowanie) 
- Wyłącz konsolidator `–nolink` 
- Wyłączanie usuwanie 
  - Symbole `--nosymbolstrip` 
  - IL (wersja) `--nostrip`.  
 
Dodatkowe porady 

- Ponieważ w symulatorze, Preferuj kompilacji przed Skompiluj ponownie 
  - Czy drzewa obiektów aplikacji są w pamięci podręcznej zestawów (pliki obiektu) 
- Z powodu symbole systemem dsymutil i ponieważ kończy się jest większy, dodatkowy czas przekazywania na urządzeniach ono wydajność debugowania kompilacji zajmuje więcej czasu. 
- Domyślnie kompilacjami wydania wykona paska IL zestawów. Który przyjmuje tylko nieco czasu i jest prawdopodobnie płynących z powrotem wdrażania mniejszych .app do urządzeń.
- Należy unikać rozmieszczania dużych plików statycznych przy każdej kompilacji (debugowanie) 
  - Użyj UIFileSharingEnabled (info.plist) 
    - Zasoby, które mogą być przekazywane raz 
- W razie wątpliwości, użyj `–time –time` flagi do mierzenia zmiany

Poniższy zrzut ekranu przedstawia sposób ustawić te opcje symulatora w opcjach iOS:

[![](ios-build-mechanics-images/image4.png "Ustawianie opcji")](ios-build-mechanics-images/image4.png#lightbox)

## <a name="using-the-linker"></a>Za pomocą konsolidatora

Podczas tworzenia aplikacji, mtouch używa konsolidator dla zarządzanego kodu, co powoduje usunięcie kodu, który nie korzysta z aplikacji. Teoretycznie zapewnia mniejszych i w związku z tym szybciej kompilacji. Aby uzyskać więcej informacji na temat konsolidator dotyczą [konsolidacji w systemie iOS](~/ios/deploy-test/linker.md) przewodnik.

Korzystając z konsolidator, należy wziąć pod uwagę następujące opcje:

- Wybieranie **łącze nie** urządzenia przyjmuje większa ilość czasu kompilacji, a także generuje większe aplikacji. 
  - Apple spowoduje odrzucenie aplikacji, jeśli są większe niż limit rozmiaru. Zależne od `MinimumOSVersion`, może to być możliwie najmniejsze 60 MB. 
  - Dołączanie natywnego pliku wykonywalnego. 
  - Nie przy użyciu łącza jest szybsze symulatora kompilacji ponieważ kompilacja JIT jest używana (w przeciwieństwie do drzewa obiektów aplikacji na urządzeniu).
- Łącze zestawu SDK jest domyślną opcją.
- Łącze wszystkie nie może być bezpiecznie korzystać, zwłaszcza w przypadku, gdy używasz czyli kodu nie własne takie NuGets lub składników. Jeśli nie chcesz połączyć zestawy cały kod z tych usług są dołączone do aplikacji, tworzenie potencjalnie większego aplikacji. 
  - Jednak jeśli wybierzesz **wszystkie łącza** aplikacja może awarii, zwłaszcza w przypadku, gdy są używane składników zewnętrznych. Jest to spowodowane niektórych składników za pomocą odbicia dla niektórych typów.
  - Analizy statycznej i odbicie nie współdziałają ze sobą. 

Instrukcje narzędzi zapewnić wewnątrz aplikacji przy użyciu [ `[Preserve]` atrybutu](~/ios/deploy-test/linker.md). 

Jeśli nie masz dostępu do kodu źródłowego lub jest ona generowana przez narzędzie i nie chcesz je zmienić, nadal można łączyć, tworząc plik XML, który opisano wszystkie typy i elementy członkowskie, które muszą zostać zachowane. Następnie można dodać flagę `--xml={file.name}.xml` opcje projektu, które przetwarzane kodu dokładnie tak, jakby były przy użyciu atrybutów.


### <a name="partially-linking-applications"></a>Aplikacje częściowo połączeń 

Istnieje również możliwość częściowo połączyć aplikacje, aby ułatwić optymalne czas kompilacji aplikacji:

- Użyj `Link All` i pominąć niektóre z zestawów 
  - Niektóre z optymalizacji rozmiaru aplikacji zostaną utracone.
  - Brak dostępu do kodu źródłowego jest wymagana.
  - Na przykład `--linkall --linkskip=fieldserviceiOS` .
 
- Użyj `Link SDK` a `[LinkerSafe]` atrybutu zestawów należy 
  - Dostęp do kodu źródłowego wymagane.
  - Informuje system, że zestaw jest bezpieczne połączyć, i jest przetwarzana tak, jakby był SDK platformy Xamarin.
 
### <a name="objective-c-bindings"></a>Powiązania Objective-C 

- Przy użyciu `[Assembly: LinkerSafe]` atrybut wiązania można oszczędzić czas i rozmiar.

- SmartLink 
  - Wykonywane po stronie natywnego 
  - Użyj `[LinkWith (SmartLink=true)]` atrybutu
  - Dzięki temu natywnego konsolidator, aby wyeliminować kodu natywnego, z którą jest nawiązywane połączenie z biblioteki. 
  - Uwaga tego dynamiczne wyszukiwania symboli nie będą działać z tym. 

## <a name="summary"></a>Podsumowanie

W tym przewodniku przedstawione jak czas opcje wziąć pod uwagę, które są zależne od konfiguracji kompilacji i opcje projektu i aplikacji systemu iOS. 

<!-----
# Benchmarks

## Layer 1: building again after making modifications, but _without_ cleaning should be faster 
 
The app should build a bit more quickly if you have only made changes to a subset of the libraries and you do not clean the build before re-deploying. 
 
 
 
### Clean build time 
178 seconds 
 
 
### Build again (without cleaning) after making _no changes_ 
12.5 seconds 
 
 
### Build again (without cleaning) after changing 1 line in "ViewIOS/ImageResourcesHelper.cs" 
3 trials: 45 seconds, 43 seconds, 43 seconds 
 
 
### Build again (without cleaning) after changing 1 line in each of the following files 
 
- ViewIOS/ImageResourcesHelper.cs 
- Sales.Native.Core.Tools/UIComponents/ListView/IListView.cs 
- View.Models/Mailing/MailingModel.cs 
 
3 trials: 45 seconds, 45 seconds, 45 seconds 
 
 
 
### Build again (without cleaning) after changing 1 line in each of the following files 
 
- ViewIOS/ImageResourcesHelper.cs 
- Sales.Native.Core.Tools/UIComponents/ListView/IListView.cs 
- View.Models/Mailing/MailingModel.cs 
- Sales.Native.Core.IOS.Ext/ServiceInterfaces/AlertDialog/Dialog.cs 
- Sales.Native.Core.Tools.IOS.Ext/BaseViews/BaseNavigationViewController.cs 
- View.Common/Services/DataTransferResult.cs 
 
45 seconds 
 
 
 
 
 
 
## Layer 2: "app thinning" aka "device specific builds" 
 
The idea of "app thinning" is that the IDE will only build the 1 architecture needed for the specific device that you're deploying to (rather than _both_ 32-bit and 64-bit architectures). 
 
As of the latest "Xamarin 4" builds, you can now enable "app thinning" in Visual Studio via the "Project Options -> iOS Build -> Enable device-specific builds" setting. 
 
Or if you prefer you can achieve a similar result by changing the "Project Options -> iOS Build -> Advanced [tab] -> Supported architectures" to select just _one_ architecture (for example ARM64 if you are developing on a 64-bit device). 
 
 
 
(Caveat: I ran the following builds in Visual Studio for Mac on the Mac rather than on the command line.) 
 
### Clean build time without "device specific builds" 
177 seconds 
 
 
 
### Clean build time _with_ "device specific builds"  
2 trials: 106 seconds, 98 seconds 
 
 
 
### Build again (without cleaning) after changing 1 line in "ViewIOS/ImageResourcesHelper.cs" 
2 trials: 31 seconds, 31 seconds 
 
 
* * * 
 
 
## Using the same strategy, but explicitly setting "Supported architectures" to select ARM64 _only_ (rather than using "device specific builds") 
 
(These builds were again run on the command line using `xbuild`.) 
 
 
 
### Clean build time with "Supported architectures" set to ARM64 _only_ 
2 trials: 80 seconds, 91 seconds 
 
 
 
### Build again (without cleaning) after changing 1 line in "ViewIOS/ImageResourcesHelper.cs" 
2 trials: 26 seconds, 26 seconds 
 
 
 
 
 
[1] Mac system used for testing: MacBookAir5,2 
 
- 2.0 GHz Core i7 (I7-3667U) 
 
2 Cores with hyper-threading 
 
L2 Cache (per Core): 256 KB 
L3 Cache: 4 MB 
 
- Standard MacBook soldered-in solid-state storage 
 
- 8 GB RAM 
---->


## <a name="related-links"></a>Linki pokrewne

- [Wpis w blogu](https://blog.xamarin.com/xamarin-ios-build-improvements/)
- [Łączenie w systemie iOS](~/ios/deploy-test/linker.md)
- [Konfiguracja konsolidatora niestandardowego](~/cross-platform/deploy-test/linker.md)
