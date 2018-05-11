---
title: Aktualizowanie istniejące aplikacji dla systemu iOS
description: Wykonaj następujące kroki, aby zaktualizować istniejące aplikacji platformy Xamarin.iOS przy użyciu interfejsu API Unified.
ms.prod: xamarin
ms.assetid: 303C36A8-CBF4-48C0-9412-387E95024CAB
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 74534333bb0c4ae54dc6816312a5531f29a80ce5
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/09/2018
---
# <a name="updating-existing-ios-apps"></a>Aktualizowanie istniejące aplikacji dla systemu iOS

_Wykonaj następujące kroki, aby zaktualizować istniejące aplikacji platformy Xamarin.iOS przy użyciu interfejsu API Unified._

Aktualizowanie istniejących aplikacji przy użyciu interfejsu API Unified wymaga zmian w pliku projektu się również obszary nazw i interfejsy API używane w kodzie aplikacji.

## <a name="the-road-to-64-bits"></a>Drogi 64-bitowy

Nowe interfejsy API Unified są wymagane do obsługi architektury urządzenia 64-bitowym z aplikacji mobilnej platformy Xamarin.iOS. Począwszy od 1 lutego 2015 Apple wymaga, że wszystkie nowe przesyłanie aplikacji do sklepu iTunes App Store obsługuje architektur 64-bitowych.

Xamarin zapewnia narzędzi dla programu Visual Studio for Mac i Visual Studio do automatyzacji procesu migracji z klasycznego interfejsu API Unified API lub ręcznie przekonwertować pliki projektu. Podczas automatycznego narzędzia przy użyciu wysokiej jest zalecane, w tym artykule opisano obie metody.

### <a name="before-you-start"></a>Przed rozpoczęciem...

Przed zaktualizowaniem istniejącego kodu do interfejsu API Unified zdecydowanie zaleca się usunąć wszystkich *ostrzeżenia kompilacji*. Wiele *ostrzeżenia* w klasycznym interfejsie API staną się błędy po migracji do Unified. Przed rozpoczęciem ich rozwiązanie jest łatwiejsze, ponieważ komunikaty kompilatora z klasycznego interfejsu API często udostępniają wskazówki dotyczące aktualizacji.

## <a name="automated-updating"></a>Automatyczne aktualizowanie

Po rozwiązaniu ostrzeżenia wybierz istniejący projekt dla systemu iOS w programie Visual Studio dla komputerów Mac lub Visual Studio i wybierz polecenie **migracji Xamarin.iOS Unified API** z **projektu** menu. Na przykład:

![](updating-ios-apps-images/beta-tool1.png "Wybierz migracji Xamarin.iOS Unified API z menu projektu")

Musisz zaakceptować tego ostrzeżenia, zanim zostanie uruchomiony automatycznej migracji (oczywiście należy mają kontrolę kopii zapasowych/źródła przed wdrożeniem tej adventure):

![](updating-ios-apps-images/beta-tool2.png "Zgadzam się to ostrzeżenie, zanim zostanie uruchomiony automatycznej migracji")

Narzędzie automatyzuje zasadniczo wszystkie czynności opisane w temacie **aktualizacji ręcznie** sekcji przedstawione poniżej i Sugerowane metody konwersji do interfejsu API Unified istniejącego projektu platformy Xamarin.iOS.

## <a name="steps-to-update-manually"></a>Kroki, aby zaktualizować ręcznie

Po rozwiązaniu ostrzeżenia ponownie, należy wykonać następujące kroki, aby ręcznie zaktualizować aplikacji platformy Xamarin.iOS używanie nowego interfejsu API Unified:

### <a name="1-update-project-type--build-target"></a>1. Typ projektu aktualizacji & docelowy kompilacji

Zmień podtypem projektu w Twojej **csproj** plików ze `6BC8ED88-2882-458C-8E55-DFD12B67127B` do `FEACFBD2-3405-455C-9665-78FE426C6842`. Edytuj **csproj** plik w edytorze tekstów, zastępując pierwszy element `<ProjectTypeGuids>` elementu pokazany:

![](updating-ios-apps-images/csproj.png "Edytuj plik csproj w edytorze tekstów, zastępując pierwszego elementu w elemencie właściwości ProjectTypeGuids, jak pokazano")

Zmień **importu** element, który zawiera `Xamarin.MonoTouch.CSharp.targets` do `Xamarin.iOS.CSharp.targets` pokazany:

![](updating-ios-apps-images/csproj2.png "Zmień element importu, który zawiera Xamarin.MonoTouch.CSharp.targets do Xamarin.iOS.CSharp.targets, jak pokazano")

### <a name="2-update-project-references"></a>2. Aktualizacja odwołania do projektu

Rozwiń projekt aplikacji systemu iOS **odwołania** węzła. Początkowo wyświetli * przerwane - **monotouch** odwołania podobny do tego zrzutu ekranu (ponieważ firma Microsoft właśnie zmienił typ projektu):

![](updating-ios-apps-images/references.png "Początkowo przedstawiono w nim odwołanie uszkodzony monotouch podobny do tego zrzutu ekranu, ponieważ zmienił się typ projektu")

Kliknij prawym przyciskiem myszy projekt aplikacji systemu iOS do **odwołania Edytuj**, następnie kliknij polecenie **monotouch** odwołania i usuń go za pomocą przycisku czerwony symbol "X".

![](updating-ios-apps-images/references-delete-monotouch-sml.png "Kliknij prawym przyciskiem myszy projekt aplikacji systemu iOS do edytowania odwołań, a następnie kliknij odwołanie monotouch i usuń go za pomocą czerwony przycisk X")

Teraz przewiń do końca listy odwołań i znaczników **Xamarin.iOS** zestawu.

![](updating-ios-apps-images/references-add-xamarinios-sml.png "Teraz przewiń do końca listy odwołań i znaczników zestawu platformy Xamarin.iOS")

Naciśnij klawisz **OK** zapisać odwołania do projektu.

### <a name="3-remove-monotouch-from-namespaces"></a>3. Usuń MonoTouch z przestrzeni nazw

Usuń **MonoTouch** prefiks z przestrzeni nazw w `using` instrukcji lub wszędzie tam, gdzie classname ma zostać w pełni kwalifikowana (np.) `MonoTouch.UIKit` staje się tylko `UIKit`).

### <a name="4-remap-types"></a>4. Ponowne mapowanie typów

[Typy natywne](~/cross-platform/macios/nativetypes.md) zostały wprowadzone, która zastępuje niektóre typy, które były wcześniej używane, takie jak wystąpienia `System.Drawing.RectangleF` z `CoreGraphics.CGRect` (na przykład). Pełną listę typów można znaleźć w [natywnych typów](~/cross-platform/macios/nativetypes.md) strony.

### <a name="5-fix-method-overrides"></a>5. Usuń zastąpień — metoda

Niektóre `UIKit` metody miały ich podpisu, zmieniona do używania nowych [natywnych typów](~/cross-platform/macios/nativetypes.md) (takie jak `nint`). Jeśli niestandardowe podklasy zastępować te metody podpisy nie będzie już zgodny i będą powodować błędy. Usuń te zastąpienia metody zmieniając podklasy odpowiadające nowego podpisu używanie typów natywnych.

Przykłady obejmują zmiana `public override int NumberOfSections (UITableView tableView)` do zwrócenia `nint` i zmianę zarówno zwracany typ i typy parametrów w `public override int RowsInSection (UITableView tableView, int section)` do `nint`.

## <a name="considerations"></a>Uwagi

Następujące kwestie należy brane pod uwagę podczas konwertowania istniejącego projektu platformy Xamarin.iOS klasycznego interfejsu API na nowy interfejs API Unified Jeśli danej aplikacji zależy od składnika lub pakietu NuGet.

### <a name="components"></a>Składniki

Każdego składnika, który jest dołączony do aplikacji będzie również muszą zostać zaktualizowane do interfejsu API Unified lub wystąpi konflikt podczas kompilacji. Dla każdego składnika dołączone zastąpić bieżącą wersję za pomocą nowej wersji w magazynie składników Xamarin obsługującego interfejsu API Unified i wykonać czystą kompilację. Każdego składnika, który nie został jeszcze przekonwertowany przez autora, będą wyświetlane tylko ostrzeżenie w magazynie składników 32-bitowym.

### <a name="nuget-support"></a>Obsługa NuGet

Gdy firma Microsoft przyczyniły się zmiany NuGet do pracy z obsługą Unified API, nie została nową wersję programu NuGet, dlatego firma Microsoft ocenia jak uzyskać NuGet do rozpoznawania nowych interfejsów API.

Do tego czasu, podobnie jak składniki musisz przełączyć dowolnego pakietu NuGet, zostały uwzględnione w projekcie na wersję obsługującą interfejsy API Unified i wykonaj czystą kompilację później.

> [!IMPORTANT]
> Jeśli masz wystąpił błąd w formie _"błąd 3 nie może zawierać zarówno"monotouch.dll"i"Xamarin.iOS.dll"w tym samym projekcie platformy Xamarin.iOS —"Xamarin.iOS.dll"odwołuje się do jawnie, gdy"monotouch.dll"odwołuje się do niego" xxx, wersja = 0.0.000, Culture = neutral, PublicKeyToken = null ""_ po przekonwertowaniu aplikacji do interfejsów API Unified, jest zazwyczaj z powodu konieczności składnika lub pakietu NuGet w projekcie, który nie został jeszcze zaktualizowany do interfejsu API Unified. Należy usunąć istniejący składnik/NuGet, aktualizacja do wersji, która obsługuje interfejsy API Unified i wykonać czystą kompilację.

## <a name="enabling-64-bit-builds-of-xamarinios-apps"></a>Włączanie 64-bitowym kompilacji aplikacji platformy Xamarin.iOS

Dla aplikacji mobilnej platformy Xamarin.iOS, który został przekonwertowany do interfejsu API Unified Deweloper nadal musi umożliwiają tworzenie aplikacji na komputerach 64-bitowych z opcji aplikacji. Zobacz **Włączanie 64 bitowej kompilacje dla aplikacji platformy Xamarin.iOS** z [zagadnień dotyczących platformy 32/x 64](~/cross-platform/macios/32-and-64/index.md#enable-64) kompilacje dokumentu, aby uzyskać szczegółowe instrukcje na temat włączania 64-bitowym.

## <a name="finishing-up"></a>Trwa kończenie

Czy chcesz konwertować aplikacji platformy Xamarin.iOS z klasycznego do interfejsów API Unified przy użyciu metody automatycznie lub ręcznie, istnieje kilka wystąpień, które będą dalej, wymagają ręcznej interwencji. Zobacz nasze [porady dotyczące aktualizowanie kodu interfejsu API Unified](~/cross-platform/macios/unified/updating-tips.md) dokument pod kątem znanych problemów i pracy arounds.

## <a name="related-links"></a>Linki pokrewne

- [Porady dotyczące aktualizowania kodu na potrzeby ujednoliconego interfejsu API](~/cross-platform/macios/unified/updating-tips.md)
- [Praca z typami natywnymi w aplikacjach międzyplatformowych](~/cross-platform/macios/native-types-cross-platform.md)
- [Klasycznym vs różnice Unified API](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
- [Migrowanie Unified API (klip wideo)](http://university.xamarin.com/lightninglectures/migrating-to-the-unified-api)
