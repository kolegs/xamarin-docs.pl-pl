---
title: Aktualizowanie istniejącej aplikacji Mac
description: Tym dokumencie opisano kroki, które należy przestrzegać podczas aktualizacji aplikacji Xamarin.Mac z klasycznego interfejsu API Unified API.
ms.prod: xamarin
ms.assetid: 26673CC5-C1E5-4BAC-BEF4-9A386B296FD5
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 5e6034b079bba5e884872e4f2096d677fd3641d0
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782414"
---
# <a name="updating-existing-mac-apps"></a>Aktualizowanie istniejącej aplikacji Mac

Aktualizowanie istniejących aplikacji przy użyciu interfejsu API Unified wymaga zmian w pliku projektu się również obszary nazw i interfejsy API używane w kodzie aplikacji.

## <a name="the-road-to-64-bits"></a>Drogi 64-bitowy

Nowe interfejsy API Unified są wymagane do obsługi architektury urządzenia 64-bitowym z Xamarin.Mac aplikacji. Począwszy od 1 lutego 2015 Apple wymaga, że wszystkie nowe przesyłanie aplikacji do sklepu Mac App Store obsługuje architektur 64-bitowych.

Xamarin zapewnia narzędzi dla programu Visual Studio for Mac i Visual Studio do automatyzacji procesu migracji z klasycznego interfejsu API Unified API lub ręcznie przekonwertować pliki projektu. Podczas automatycznego narzędzia przy użyciu wysokiej jest zalecane, w tym artykule opisano obie metody.

### <a name="before-you-start"></a>Przed rozpoczęciem...

Przed zaktualizowaniem istniejącego kodu do interfejsu API Unified zdecydowanie zaleca się usunąć wszystkich *ostrzeżenia kompilacji*. Wiele *ostrzeżenia* w klasycznym interfejsie API staną się błędy po migracji do Unified. Przed rozpoczęciem ich rozwiązanie jest łatwiejsze, ponieważ komunikaty kompilatora z klasycznego interfejsu API często udostępniają wskazówki dotyczące aktualizacji.

## <a name="automated-updating"></a>Automatyczne aktualizowanie

Po rozwiązaniu ostrzeżenia wybierz istniejący projekt Mac w programie Visual Studio dla komputerów Mac lub Visual Studio i wybierz polecenie **migracji Xamarin.Mac Unified API** z **projektu** menu. Na przykład:

![](updating-mac-apps-images/beta-tool1.png "Wybierz migracji Xamarin.Mac Unified API z menu projektu")

Musisz zaakceptować tego ostrzeżenia, zanim zostanie uruchomiony automatycznej migracji (oczywiście należy mają kontrolę kopii zapasowych/źródła przed wdrożeniem tej adventure):

![](updating-mac-apps-images/migrate01.png "Zgadzam się to ostrzeżenie, zanim zostanie uruchomiony automatycznej migracji")

Istnieją dwa typy platformy docelowej obsługiwanych, które można wybrać podczas korzystania z interfejsu API Unified w aplikacji Xamarin.Mac:

- **Xamarin.Mac Mobile Framework** — jest tej samej Zaczekaj .NET framework używane przez Xamarin.iOS i Xamarin.Android obsługi podzbiór pełnego **pulpitu** framework. Jest zalecana framework, ponieważ zapewnia mniejszych średni plików binarnych ze względu na zachowanie połączeń wyższego poziomu.
- **Xamarin.Mac .NET 4.5 Framework** — ta struktura jest ponownie, podzbiór **pulpitu** framework. Jednak przycina znacznie mniej pełnego **pulpitu** framework niż **Mobile** framework i powinien _"tylko służbowych"_ większość pakietów NuGet lub 3 bibliotek strony. Pozwala to deweloperom korzystać standard **pulpitu** zestawy, gdy nadal korzystanie z obsługiwanych struktur, ale ta opcja tworzy większych pakietów aplikacji. Jest to zalecane framework gdzie są używane 3 zestawów platformy .NET firmy, które nie są zgodne z **Xamarin.Mac Mobile Framework**. Aby uzyskać listę obsługiwanych zestawów, zobacz nasze [zestawy](~/cross-platform/internals/available-assemblies.md) dokumentacji.

Szczegółowe informacje dotyczące struktur docelowych oraz skutki wybranie określony element docelowy aplikacji Xamarin.Mac można znaleźć naszych [docelowych platform](~/mac/platform/target-framework.md) dokumentacji. 

Narzędzie automatyzuje zasadniczo wszystkie czynności opisane w temacie **aktualizacji ręcznie** sekcji przedstawione poniżej i Sugerowane metody konwersji istniejącego projektu Xamarin.Mac interfejsu API Unified.

## <a name="steps-to-update-manually"></a>Kroki, aby zaktualizować ręcznie

Po rozwiązaniu ostrzeżenia ponownie, należy wykonać następujące kroki, aby ręcznie zaktualizować Xamarin.Mac aplikacji za pomocą nowego interfejsu API Unified:

### <a name="1-update-project-type--build-target"></a>1. Typ projektu aktualizacji & docelowy kompilacji

Zmień podtypem projektu w Twojej **csproj** plików ze `42C0BBD9-55CE-4FC1-8D90-A7348ABAFB23` do `A3F8F2AB-B479-4A4A-A458-A89E7DC349F1`. Edytuj **csproj** plik w edytorze tekstów, zastępując pierwszy element `<ProjectTypeGuids>` elementu pokazany:

![](updating-mac-apps-images/csproj.png "Edytuj plik csproj w edytorze tekstów, zastępując pierwszego elementu w elemencie właściwości ProjectTypeGuids, jak pokazano")

Zmień **importu** element, który zawiera `Xamarin.Mac.targets` do `Xamarin.Mac.CSharp.targets` pokazany:

![](updating-mac-apps-images/csproj2.png "Zmień element importu, który zawiera Xamarin.Mac.targets do Xamarin.Mac.CSharp.targets, jak pokazano")

Dodaj następujące wiersze kodu po `<AssemblyName>` elementu:

```xml
<TargetFrameworkVersion>v2.0</TargetFrameworkVersion>
<TargetFrameworkIdentifier>Xamarin.Mac</TargetFrameworkIdentifier>

```

Przykład:

![](updating-mac-apps-images/csproj3.png "Dodaj następujące wiersze kodu po elemencie < AssemblyName >")

### <a name="2-update-project-references"></a>2. Aktualizacja odwołania do projektu

Rozwiń projekt aplikacji Mac **odwołania** węzła. Początkowo wyświetli * przerwane - **XamMac** odwołania podobny do tego zrzutu ekranu (ponieważ firma Microsoft właśnie zmienił typ projektu):

![](updating-mac-apps-images/references.png "Początkowo przedstawiono w nim odwołanie uszkodzony XamMac podobny do tego zrzutu ekranu")

Kliknij przycisk **koło zębate ikonę** obok **XamMac** wpis, a następnie wybierz **usunąć** usunąć uszkodzone odwołanie.

Obok, kliknij prawym przyciskiem myszy **odwołania** folderu w **Eksploratora rozwiązań** i wybierz **odwołania Edytuj**. Przewiń w dół na liście odwołań i zaznacz oprócz **Xamarin.Mac**.

![](updating-mac-apps-images/references2.png "Przewiń w dół na liście odwołań i zaznacz oprócz Xamarin.Mac")

Naciśnij klawisz **OK** zapisać odwołania do projektu.

### <a name="3-remove-monomac-from-namespaces"></a>3. Usuń MonoMac z przestrzeni nazw

Usuń **MonoMac** prefiks z przestrzeni nazw w `using` instrukcji lub wszędzie tam, gdzie classname ma zostać w pełni kwalifikowana (np.) `MonoMac.AppKit` staje się tylko `AppKit`).

### <a name="4-remap-types"></a>4. Ponowne mapowanie typów

[Typy natywne](~/cross-platform/macios/nativetypes.md) zostały wprowadzone, która zastępuje niektóre typy, które były wcześniej używane, takie jak wystąpienia `System.Drawing.RectangleF` z `CoreGraphics.CGRect` (na przykład). Pełną listę typów można znaleźć w [natywnych typów](~/cross-platform/macios/nativetypes.md) strony.

### <a name="5-fix-method-overrides"></a>5. Usuń zastąpień — metoda

Niektóre `AppKit` metody miały ich podpisu, zmieniona do używania nowych [natywnych typów](~/cross-platform/macios/nativetypes.md) (takie jak `nint`). Jeśli niestandardowe podklasy zastępować te metody podpisy nie będzie już zgodny i będą powodować błędy. Usuń te zastąpienia metody zmieniając podklasy odpowiadające nowego podpisu używanie typów natywnych. 

## <a name="considerations"></a>Uwagi

Następujące kwestie należy brane pod uwagę podczas konwertowania istniejącego projektu Xamarin.Mac klasycznego interfejsu API na nowy interfejs API Unified Jeśli danej aplikacji zależy od składnika lub pakietu NuGet. 

### <a name="components"></a>Składniki

Każdego składnika, który jest dołączony do aplikacji będzie również muszą zostać zaktualizowane do interfejsu API Unified lub wystąpi konflikt podczas kompilacji. Dla każdego składnika dołączone zastąpić bieżącą wersję za pomocą nowej wersji w magazynie składników Xamarin obsługującego interfejsu API Unified i wykonać czystą kompilację. Każdego składnika, który nie został jeszcze przekonwertowany przez autora, będą wyświetlane tylko ostrzeżenie w magazynie składników 32-bitowym.

### <a name="nuget-support"></a>Obsługa NuGet

Gdy firma Microsoft przyczyniły się zmiany NuGet do pracy z obsługą Unified API, nie została nową wersję programu NuGet, dlatego firma Microsoft ocenia jak uzyskać NuGet do rozpoznawania nowych interfejsów API. 

Do tego czasu, podobnie jak składniki musisz przełączyć dowolnego pakietu NuGet, zostały uwzględnione w projekcie na wersję obsługującą interfejsy API Unified i wykonaj czystą kompilację później.

> [!IMPORTANT]
> Jeśli wystąpił błąd w formularzu _"błąd 3 nie może zawierać zarówno"monomac.dll"i"Xamarin.Mac.dll"w tym samym projekcie Xamarin.Mac —"Xamarin.Mac.dll"odwołuje się do jawnie, gdy"monomac.dll"odwołuje się do niego" xxx, wersja = 0.0.000, Culture = neutral, PublicKeyToken = null ""_ po przekonwertowaniu aplikacji do interfejsów API Unified, jest zazwyczaj z powodu konieczności składnika lub pakietu NuGet w projekcie, który nie został jeszcze zaktualizowany do interfejsu API Unified. Należy usunąć istniejący składnik/NuGet, aktualizacja do wersji, która obsługuje interfejsy API Unified i wykonać czystą kompilację.

## <a name="enabling-64-bit-builds-of-xamarinmac-apps"></a>Włączanie 64-bitowym kompilacje Xamarin.Mac aplikacji

Dla aplikacji mobilnej Xamarin.Mac, który został przekonwertowany do interfejsu API Unified Deweloper nadal musi umożliwiają tworzenie aplikacji na komputerach 64-bitowych z opcji aplikacji. Zobacz **Włączanie 64 bitowej kompilacji z Xamarin.Mac aplikacje** z [zagadnień dotyczących platformy 32/x 64](~/cross-platform/macios/32-and-64/index.md) kompilacje dokumentu, aby uzyskać szczegółowe instrukcje na temat włączania 64-bitowym.
    
## <a name="finishing-up"></a>Trwa kończenie

Czy chcesz konwertować aplikacji Xamarin.Mac z klasycznego do interfejsów API Unified przy użyciu metody automatycznie lub ręcznie istnieje kilka wystąpień, które będą dalej, wymagają ręcznej interwencji. Zobacz nasze [porady dotyczące aktualizowanie kodu interfejsu API Unified](~/cross-platform/macios/unified/updating-tips.md) dokument pod kątem znanych problemów i pracy arounds.

## <a name="related-links"></a>Linki pokrewne

- [Porady dotyczące aktualizowania kodu na potrzeby ujednoliconego interfejsu API](~/cross-platform/macios/unified/updating-tips.md)
- [Praca z typami natywnymi w aplikacjach międzyplatformowych](~/cross-platform/macios/native-types-cross-platform.md)
- [Klasycznym vs różnice Unified API](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
