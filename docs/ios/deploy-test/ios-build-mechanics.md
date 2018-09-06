---
title: Mechanika kompilacji systemu iOS
description: Ten przewodnik przedstawiono, jak czas aplikacji i sposób używania metody, które mogą zostać wykorzystane szybsze kompilacje w przypadku wszystkich konfiguracji kompilacji.
ms.prod: xamarin
ms.assetid: 06FD3940-D666-4C9E-BC3E-BBE481EF8012
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 4145368281c2967bd1311389e5e1b1432af2c9b8
ms.sourcegitcommit: e64c3c10d6a36b3b031d6d4dbff7af74ab2b7f21
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/10/2018
ms.locfileid: "43780651"
---
# <a name="ios-build-mechanics"></a>Mechanika kompilacji systemu iOS

_Ten przewodnik przedstawiono, jak czas aplikacji i sposób używania metody, które mogą zostać wykorzystane szybsze kompilacje w przypadku wszystkich konfiguracji kompilacji._

Tworzenie niezawodnych aplikacji to więcej niż tylko pisanie kodu, który działa. Dobrze napisane aplikacji powinna zawierać optymalizacje, które szybsze kompilacje z aplikacjami, które są mniejsze i szybsze uruchamianie wykonywania. Optymalizacje te nie tylko powoduje lepsze środowisko użytkownika, ale także do Ciebie lub każdy deweloper może pracować nad projektem. Istotne jest zapewnienie, że podczas pracy z aplikacją wszystko, co jest upłynął limit czasu często. 

Należy pamiętać, że są bezpieczne i szybkie opcji domyślnych, ale nie są optymalne dla każdej sytuacji. Ponadto wiele opcji może spowolnić albo przyspieszyć cykl tworzenia oprogramowania, w zależności od pojedynczego projektu. Na przykład natywnych obcięcie zajmuje trochę czasu, ale jeśli zdobyte jest bardzo mały rozmiar następnie obcięcie czas nie zostaną odzyskane przez szybsze wdrażanie. Z drugiej strony natywne obcięcie można zmniejszyć aplikacji znacznie, w którym to przypadku będzie szybsze do wdrożenia. To różni się między projektami, a jedynym sposobem, aby wiedzieć, jest przetestowanie.

Szybkość kompilacji Xamarin może mieć wpływ również przez różnych pojemnościach i możliwości komputera niż może mieć wpływ na wydajność: możliwości procesora, szybkość magistrali, ilość pamięci fizycznej, szybkość dysku, szybkość sieci. Te ograniczenia wydajności, które wykraczają poza zakres tego dokumentu, a, spoczywa wyłącznie dewelopera.


## <a name="timing-apps"></a>Czas aplikacji

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Aby włączyć dane wyjściowe diagnostyki programu MSBuild w programie Visual Studio dla komputerów Mac:

1. Kliknij przycisk **Visual Studio for Mac > Preferencje...**
2. W widoku drzewa po lewej stronie wybierz **projektów > Tworzenie**
3. W okienku po prawej stronie Ustaw poziom szczegółowości dziennika menu rozwijane **diagnostycznych**: [ ![](ios-build-mechanics-images/image2.png "ustawienie poziomu szczegółowości dziennika")](ios-build-mechanics-images/image2.png#lightbox)
4. Kliknij przycisk **OK**
5. Uruchom program Visual Studio dla komputerów Mac
6. Czyszczenie i ponownie pakiet
7. Wyświetl diagnostyczne dane wyjściowe w konsoli błędów (Widok > okienka > błędy), klikając przycisk danych wyjściowych kompilacji


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Aby włączyć dane wyjściowe diagnostyki programu MSBuild w programie Visual Studio:

1. Kliknij przycisk **Narzędzia > Opcje...**
2. W widoku drzewa po lewej stronie wybierz **projekty i rozwiązania > Kompilowanie i uruchamianie**
3. W okienku po prawej stronie Ustaw *kompilacji MSBuild danych wyjściowych listy rozwijanej poziom szczegółowości* do **diagnostycznych**: [ ![](ios-build-mechanics-images/image2-vs.png "ustawienie MSBuild dane wyjściowe kompilacji poziom szczegółowości")](ios-build-mechanics-images/image2-vs.png#lightbox)
4. Kliknij przycisk **OK**
5. Czyszczenie i ponownie pakiet.
6. Dane wyjściowe diagnostyki jest widoczna w panelu wyjścia.

-----

## <a name="timing-mtouch"></a>Chronometraż mtouch

Aby wyświetlić informacje dotyczące mtouch procesu kompilacji, należy przekazać `--time --time` na argumenty mtouch w swojej **opcje projektu**. Wyniki znajdują się w danych wyjściowych kompilacji, wyszukując `MTouch` zadań:

```csharp
Setup: 36 ms
Resolve References: 982 ms
Extracted native link info: 987 ms
...
Total time: 1554 ms
```

## <a name="connecting-from-visual-studio-with-build-host"></a>Nawiązywanie połączenia z programu Visual Studio z hostem kompilacji

Narzędzia Xamarin z technicznego punktu widzenia działa na dowolnym komputerze Mac, uruchamianą OS X 10.10 Yosemite lub nowszej. Jednak środowiska deweloperskie i czasy kompilacji mogą napotykać według wydajności dla komputerów Mac.

W stanie rozłączenia, programu Visual Studio w Windows tylko wykonuje fazy kompilacji C# i nie jest podejmowana próba wykonania łączenie lub kompilacja AOT, pakiet aplikacji do _.app_ pakietu lub znak pakietu aplikacji. (Fazy kompilacji C# jest rzadko "wąskie gardło wydajności"). Próba określenia, gdzie w potoku kompilacji spowalnia, tworząc bezpośrednio na hostem kompilacji komputera Mac w programie Visual Studio dla komputerów Mac.


Ponadto jest jednym z bardziej powszechne miejsc dla sluggishness połączenie sieciowe między komputerem Windows i hostem kompilacji komputera Mac. Może to być spowodowane fizycznych przeszkody w sieci, za pomocą połączenia bezprzewodowego lub konieczności, przechodzić przez nasycony maszyny (np. Usługa Mac w chmurze).

## <a name="simulator-tricks"></a>Symulator wskazówki

 
Podczas opracowywania aplikacji mobilnych, istotne jest, aby szybko wdrożyć kod. Z różnych powodów takich jak szybkość i braku wymagania dotyczące aprowizacji urządzeń deweloperzy często możliwość wdrażania na symulatorze wstępnie zainstalowanych lub w emulatorze. Dla producentów narzędzi deweloperskich decyzja zapewnienie symulator lub w emulatorze sprowadza się do kompromis między szybkością i zgodności. 

Apple zawiera symulator do tworzenia aplikacji dla systemu iOS, podwyższanie poziomu szybkość nad zgodności, tworząc mniej restrykcyjne środowisko do uruchamiania kodu. To środowisko mniej restrykcyjne uprawnienia umożliwia platformy Xamarin na potrzeby kompilatora tylko w czas (JIT) symulator (w przeciwieństwie do [AOT](~/ios/internals/architecture.md) na urządzeniu), co oznacza, że kompilacja jest skompilowanych do natywnego kodu w czasie wykonywania. Zgodnie z komputerem Mac jest znacznie wyższa niż urządzenie, umożliwia to lepszą wydajność.

Symulator używa uruchamiania udostępnionej aplikacji, umożliwiając uruchamianie, ponowne użycie, w przeciwieństwie kompilowana za każdym razem jest wymagane na urządzeniu.

Biorąc pod uwagę powyższe informacje, Poniższa lista zawiera pewne informacje o krokach do wykonania podczas kompilowania i wdrażania aplikacji w symulatorze, aby zapewnić najlepszą wydajność.
 
### <a name="tips"></a>Porady

- Dla kompilacji: 
  - Usuń zaznaczenie pola wyboru **obrazów PNG zoptymalizować** opcji w opcjach projektu. Tego rodzaju optymalizacji nie jest konieczne dla kompilacji w symulatorze.
  - Ustaw dla konsolidatora **nie łącz**. Wyłączanie konsolidator jest szybsze, ponieważ jej wykonanie zajmuje dużo czasu.
  - Wyłączanie uruchamiania udostępnionej aplikacji przy użyciu `--nofastsim` flaga powoduje, że kompilacje symulator jest znacznie wolniejsze. Usuń tę flagę, gdy nie jest już wymagany.
  - Używanie bibliotek natywnych jest wolniejsze, ponieważ udostępnionego simlauncher głównego pliku wykonywalnego nie można użyć ponownie w takiej sytuacji i plik wykonywalny specyficzne dla aplikacji ma być tworzone dla każdej kompilacji.
- Do wdrożenia
  - Zawsze Zachowuj symulator uruchomiona, gdy jest to możliwe. Może potrwać do 12 sekund do zimnego symulatora.
- Dodatkowe porady
  - Preferuj przed ponowną kompilację, kompilacji, ponieważ Rebuild czyści przed kompilacją. Czyszczenie może zająć dużo czasu, ponieważ powoduje usunięcie odwołań, które mogłyby zostać użyte.
  - Korzystaj z faktu, że symulator nie wymusza piaskownicy. O dużych zasobów, takich jak filmy wideo lub inne zasoby dołączone do projektu można utworzyć pliku kosztownych operacji kopiowania, za każdym razem, gdy aplikacja jest uruchamiana w symulatorze. Należy unikać tych kosztownych operacji przez umieszczenie tych plików w katalogu macierzystym i odwoływać się do nich w aplikacji przez pełną ścieżkę pliku.  
  - W razie wątpliwości użyj `--time --time` flagę, aby zmierzyć zmiany

Poniższy zrzut ekranu przedstawia sposób ustawić te opcje symulatora w opcji dla systemu iOS:

[![](ios-build-mechanics-images/image3.png "Ustawianie opcji")](ios-build-mechanics-images/image3.png#lightbox)

## <a name="device-tricks"></a>Wskazówki urządzenia

Wdrażanie na urządzeniu jest podobny do wdrażania w symulatorze, symulator jest mały podzbiór kompilacji używany dla urządzeń z systemem iOS. Tworzenie urządzenia wymaga dużo więcej kroków, ale ma tę zaletę, zapewniając dodatkowe możliwości, aby zoptymalizować aplikację.

### <a name="build-configurations"></a>Konfiguracje kompilacji

Istnieje kilka konfiguracji kompilacji, które podano podczas wdrażania aplikacji dla systemu iOS. Należy dysponować dobrą znajomością każdej konfiguracji, aby dowiedzieć się, kiedy i dlaczego należy optymalizacji.

 - Debugowanie
  - W związku z tym jest to konfiguracja głównego, powinny być używane, gdy aplikacja jest w fazie projektowania, która powinna, Działaj tak szybko, jak to możliwe.
 - Wydanie
  - Kompilacje wydania to te, które są dostarczane do użytkowników i naciskiem na wydajność jest najważniejsza. Korzystając z konfiguracji wydania, możesz chcieć użyć kompilatora optymalizującego LLVM i zoptymalizować pliki PNG.

 
Jest również ważne, aby zrozumieć relację między kompilowania i wdrażania. Czas wdrożenia jest funkcją rozmiar aplikacji. Większej aplikacji zajmuje więcej czasu wdrażania. Minimalizując rozmiar aplikacji, można skrócić czas wdrażania.

Minimalizacja rozmiaru aplikacji również może skrócić czas kompilacji. Jest to spowodowane usuwanie kod z aplikacji zajmuje mniej czasu niż natywnie kompilowania kodu, które nie będą używane. Mniejszych plików obiektów oznacza szybszej konsolidacji, co powoduje utworzenie mniejszy plik wykonywalny z mniejszą liczbą symbole, aby wygenerować. Oszczędzanie miejsca, w związku z tym, ma opłacalności podwójne, co jest dlaczego **SDK łącze** jest tworzy domyślną dla wszystkich urządzeń. 

> [!NOTE]
> **SDK łącze** opcja może pojawić się jako tylko zestawy tylko łącze Framework SDK lub SDK łącze w zależności od używanego środowiska IDE.
 

### <a name="tips"></a>Porady

- Kompilacja: 
  - Tworzenie pojedynczej architektura (np. ARM64) jest szybsza niż FAT pliku binarnego (np. ARMv7 + ARM64)
  - Należy unikać optymalizacji pliki PNG podczas debugowania
  - Należy wziąć pod uwagę, łączenie wszystkich zestawów. Optymalizowanie każdy zestaw 
  - Wyłączenie tworzenia symboli debugowania przy użyciu `--dsym=false`. Należy jednak pamiętać, że wyłączenie to oznacza, czy raporty o awariach można tylko symbolicated na tym komputerze, którego zbudowana aplikacja i tylko wtedy, gdy aplikacja nie została usunięta.

 
Niektóre działania, których należy unikać są następujące:

- FAT pliki binarne (debugowanie) 
- Wyłącz konsolidatora `--nolink` 
- Wyłączanie obcięcie 
  - Symbole `--nosymbolstrip` 
  - IL (wersja) `--nostrip`.  
 
Dodatkowe porady 

- Jak w symulatorze Preferuj kompilacji przed ponowną kompilację 
  - Czy AOT zestawów (pliki obiektów) są buforowane. 
- W związku z symboli, uruchamianie dsymutil i ponieważ kończy się są większe, dodatkowy czas, do przekazania do urządzeń ono wydajność debugowania kompilacji trwa dłużej. 
- Domyślnie kompilacji wydania wykona pasek zestawy IL. Które zajmuje trochę czasu i jest prawdopodobnie płynących z powrotem wdrażanie mniejszych .app na urządzeniu.
- Unikaj wdrażania dużych plików statycznych przy każdej kompilacji (debugowanie) 
  - Użyj UIFileSharingEnabled (pliku info.plist) 
    - Zasoby można przekazać jeden raz 
- W razie wątpliwości użyj `--time --time` flagę, aby zmierzyć zmiany

Poniższy zrzut ekranu przedstawia sposób ustawić te opcje symulatora w opcji dla systemu iOS:

[![](ios-build-mechanics-images/image4.png "Ustawianie opcji")](ios-build-mechanics-images/image4.png#lightbox)

## <a name="using-the-linker"></a>Za pomocą konsolidatora

Podczas kompilowania aplikacji, mtouch używa konsolidatora dla kodu zarządzanego, który usuwa kod, który nie korzysta z aplikacji. Teoretycznie zapewnia mniejsze i w związku z tym szybsze kompilacje. Aby uzyskać więcej informacji na temat konsolidator dotyczą [konsolidacji w systemie iOS](~/ios/deploy-test/linker.md) przewodnik.

Korzystając z konsolidator, należy wziąć pod uwagę następujące opcje:

- Wybieranie **nie łącz** urządzenia zajmuje większą ilość czasu kompilacji i generuje również większej aplikacji. 
  - Firma Apple odrzuci aplikacje, jeśli są większe niż limit rozmiaru. Zależne od `MinimumOSVersion`, może to być nawet sam 60 MB. 
  - Natywny plik wykonywalny jest uwzględniane. 
  - Przy użyciu nie łącze jest szybsza, symulator kompilacji ponieważ kompilacja JIT jest używana (w przeciwieństwie do drzewa obiektów aplikacji na urządzeniu).
- Łącze zestawu SDK jest opcją domyślną.
- Łączy wszystkie może nie być bezpieczne używanie, szczególnie w przypadku, gdy używasz kodu oznacza to nie własne takie rozszerzeń Nuget lub składniki. Jeśli nie chcesz połączyć zestawy cały kod z tych usług są dołączane do aplikacji, potencjalnie tworzenia aplikacji większy. 
  - Jednak jeśli wybierzesz **wszystkie łącza** aplikacji może ulec awarii, szczególnie, jeśli składniki zewnętrzne są używane. Jest to spowodowane niektórych składników, od niektórych typów przy użyciu odbicia.
  - Analiza statyczna i odbicie nie współpracują ze sobą. 

Metoda narzędzia zachować wewnątrz aplikacji przy użyciu [ `[Preserve]` atrybutu](~/ios/deploy-test/linker.md). 

Jeśli nie masz dostępu do kodu źródłowego lub jest ona generowana przez narzędzie i nie chcesz go zmienić, mogą nadal być powiązane, tworząc plik XML, który opisuje typy i elementy członkowskie, które muszą zostać zachowane. Następnie można dodać flagę `--xml={file.name}.xml` opcje projektu, które przetwarzane kodu dokładnie tak, jakby były przy użyciu atrybutów.


### <a name="partially-linking-applications"></a>Częściowo łączenie aplikacji 

Istnieje również możliwość częściowo połączyć aplikacje, aby pomóc zoptymalizować ten czas kompilacji aplikacji:

- Użyj `Link All` i pominąć niektóre zestawy 
  - Niektóre z optymalizacji rozmiaru aplikacji zostaną utracone.
  - Brak dostępu do kodu źródłowego jest wymagana.
  - Na przykład `--linkall --linkskip=fieldserviceiOS` .
 
- Użyj `Link SDK` opcji i użyj `[LinkerSafe]` atrybutu zestawów, należy 
  - Dostęp do kodu źródłowego jest wymagane.
  - Informuje system, że zestaw jest bezpiecznie połączyć, i są przetwarzane tak, jakby był zestaw SDK platformy Xamarin.
 
### <a name="objective-c-bindings"></a>Powiązań języka Objective-C 

- Za pomocą `[Assembly: LinkerSafe]` atrybut wiązania pozwala zaoszczędzić czas i rozmiaru.

- SmartLink 
  - Na stronie natywne 
  - Użyj `[LinkWith (SmartLink=true)]` atrybutu
  - Dzięki temu natywnych konsolidator, aby wyeliminować kodu natywnego, z którą jest nawiązywane połączenie z biblioteki. 
  - Uwaga tego wyszukiwanie dynamiczne symboli nie będą działać z tym. 

## <a name="summary"></a>Podsumowanie

Ten przewodnik przedstawione instrukcje czasu aplikacji systemu iOS i opcji, aby wziąć pod uwagę, które są zależne od konfiguracji kompilacji i opcje projektu. 

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
