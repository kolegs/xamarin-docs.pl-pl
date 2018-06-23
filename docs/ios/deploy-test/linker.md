---
title: Łączenie aplikacji platformy Xamarin.iOS
description: W tym dokumencie opisano przez konsolidatora Xamarin.iOS, aby zmniejszyć jego rozmiar jest używany w celu wyeliminowania nieużywane kod z aplikacji platformy Xamarin.iOS.
ms.prod: xamarin
ms.assetid: 3A4B2178-F264-0E93-16D1-8C63C940B2F9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/24/2017
ms.openlocfilehash: 4bcfc821359e74b34dc2ee11419e8ee86f8cccee
ms.sourcegitcommit: 0be3d10bf08d1f76eab109eb891ed202615ac399
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/22/2018
ms.locfileid: "36321460"
---
# <a name="linking-xamarinios-apps"></a>Łączenie aplikacji platformy Xamarin.iOS

Podczas tworzenia aplikacji, programu Visual Studio for Mac lub Visual Studio wywołuje narzędzie **mtouch** zawierającą konsolidatora dla kodu zarządzanego. Służy do usunięcia z bibliotek klas funkcje, które nie korzysta z aplikacji. Celem jest, aby zmniejszyć rozmiar aplikacji, która będzie dostarczany z tylko niezbędne usługi bits.

Konsolidator używa analizy statycznej w celu określenia ścieżki różnych kodu, których aplikacja jest podatny na wykonaj. Jest nieco duże, musi przechodzić przez co szczegóły każdego zestawu, aby się upewnić, że nic wykrywalny zostanie usunięty. Nie jest włączona domyślnie na symulatorze kompilacji w celu przyspieszenia czas kompilacji podczas debugowania. Jednak ponieważ tworzy aplikacje mniejszych go może przyspieszyć kompilacji drzewa obiektów aplikacji i przekazać do urządzenia, wszystkie *urządzeń (wersja) tworzy* przy użyciu konsolidator domyślnie.

Tak jak konsolidator narzędzia statycznych, nie można oznaczyć do włączenia typy i metody, które wywołana przez odbicie lub dynamicznie utworzyć wystąpienia. Kilka opcji istnieje obejście tego ograniczenia.

<a name="Linker_Behavior" />

## <a name="linker-behavior"></a>Linker Behavior

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Proces łączenia można dostosować za pomocą listy rozwijanej zachowanie konsolidatora w **opcje projektu**. Dostęp do tego projektu z systemem iOS kliknij dwukrotnie i przejdź do **kompilacji systemu iOS > Opcje konsolidatora**, jak pokazano poniżej:

[![](linker-images/image1.png "Opcje konsolidatora")](linker-images/image1.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Proces łączenia można dostosować za pomocą listy rozwijanej zachowanie konsolidatora w **właściwości projektu** w programie Visual Studio.

Wykonaj następujące czynności:

1. Kliknij prawym przyciskiem myszy **Nazwa projektu** w **Eksploratora rozwiązań** i wybierz **właściwości**:

    ![](linker-images/linking01w.png "Kliknij prawym przyciskiem myszy nazwę projektu w Eksploratorze rozwiązań i wybierz polecenie Właściwości")
2. W **właściwości projektu**, wybierz pozycję **kompilacji systemu IOS**:

    ![](linker-images/linking02w.png "Kompilacji systemu IOS wybierz opcję")
3. Postępuj zgodnie z instrukcjami poniżej, aby zmienić opcje połączeń.

-----

Trzy podstawowe opcje są oferowane są opisane poniżej:


### <a name="dont-link"></a>Nie zawierają łącza

Wyłączanie łączenia zostanie upewnij się, że zestawy nie są modyfikowane. Ze względu na wydajność jest ustawienie domyślne przy środowiskiem IDE jest przeznaczony dla symulatora systemu iOS. Dla urządzeń kompilacji to powinien służyć tylko jako rozwiązanie alternatywne zawsze, gdy konsolidator zawiera usterkę, która uniemożliwia aplikację do uruchamiania. Jeśli aplikacja działa tylko z *- nolink*, Prześlij [usterek — raport](http://bugzilla.xamarin.com).

Odpowiada to *- nolink* przy użyciu mtouch narzędzia wiersza polecenia.

<a name="Link_SDK_assemblies_only" />

### <a name="link-sdk-assemblies-only"></a>Łącze tylko zestawy SDK

W tym trybie konsolidator zestawów użytkownika spowoduje pozostawienie niezmienionej i zmniejszy rozmiar zestawów SDK (tj. co to jest dostarczany z ponad Xamarin.iOS) przez usunięcie wszystko, co nie korzysta z aplikacji. To ustawienie domyślne, jeśli środowiskiem IDE jest przeznaczony dla urządzeń z systemem iOS.

Jest to opcja najprostszej, ponieważ nie wymaga żadnych zmian w kodzie. Różnica w stosunku do wszystkich połączeń jest, że konsolidator nie można wykonać kilka optymalizacji w tym trybie, więc istnieje zależność między pracy niezbędne do powiązania wszystko i rozmiar końcowy aplikacji.

Odpowiada to *- linksdk* przy użyciu mtouch narzędzia wiersza polecenia.

<a name="Link_all_assemblies" />

### <a name="link-all-assemblies"></a>Łączenie wszystkich zestawów

Podczas łączenia wszystko, konsolidator służy cały zestaw jego optymalizacje aby aplikacja była możliwie jak najmniejszy. Zmodyfikuje ona kod użytkownika, które mogą być dzielone w każdym przypadku, gdy kod używa funkcji w taki sposób, który nie może wykryć analizy statycznej konsolidatora. W takich przypadkach, np. usługi sieci Web, odbicia lub serializacji, niektóre adjustements mogą być wymagane w aplikacji, aby połączyć wszystkie elementy.

Odpowiada to *- linkall* przy użyciu narzędzia wiersza polecenia **mtouch**.

<a name="Controlling_the_Linker" />

## <a name="controlling-the-linker"></a>Kontrolowanie konsolidatora

Gdy używasz konsolidator go zostanie czasami spowoduje usunięcie kod, który może być wywołana dynamicznie, nawet pośrednio. Tak, aby pokrywał tych przypadkach konsolidator zawiera kilka funkcji i opcji, aby umożliwić większą kontrolę w swoich działań.

<a name="Preserving_Code" />

### <a name="preserving-code"></a>Zachowywanie kodu

Gdy używasz konsolidator czasami można usunąć kod, który może być wywołana dynamicznie przy użyciu System.Reflection.MemberInfo.Invoke albo przez eksportowanie metody przy użyciu języka Objective-C `[Export]` atrybutu, a następnie ręcznie wywoływania selektor .

W takich przypadkach można nakazać konsolidator, aby wziąć pod uwagę albo całej klasy były używane lub poszczególne elementy członkowskie zostaną zachowane, stosując `[Xamarin.iOS.Foundation.Preserve]` atrybutu zarówno na poziomie klasy lub poziomu elementu członkowskiego. Każdy członek, który nie jest połączony statycznie przez aplikację jest warunkiem usunięte. Ten atrybut dlatego służy do oznaczania elementów członkowskich, które nie są wywoływane statycznie, ale nadal wymagane przez aplikację.

Na przykład można utworzyć wystąpienia dynamicznie typy, można zachować domyślnego konstruktora dla typów. Jeśli używasz serializacji XML, można zachować właściwości typów.

Ten atrybut można stosować każdy element członkowski typu lub samego typu. Jeśli chcesz zachować całą typu za pomocą składni `[Preserve
(AllMembers = true)]` w typie.

Czasami chcesz zachować niektóre elementy członkowskie, ale tylko wtedy, jeśli typ zawierający została zachowana. W takich przypadkach użycia `[Preserve (Conditional=true)]`

Jeśli nie chcesz przełączyć zależności w bibliotekach Xamarin-Załóżmy, że tworzysz Biblioteka klas przenośnych dla wielu platform (PCL) - można nadal używać tego atrybutu.

Aby to zrobić, należy po prostu zadeklarować klasy PreserveAttribute i używany w kodzie, jak to:

```csharp
public sealed class PreserveAttribute : System.Attribute {
    public bool AllMembers;
    public bool Conditional;
}
```

Nie jest naprawdę istotne, w których przestrzeń nazw to zdefiniowano, konsolidator wygląda ten atrybut o nazwie typu.

 <a name="Skipping_Assemblies" />

### <a name="skipping-assemblies"></a>Pomijanie zestawów

Istnieje możliwość Określ zestawy, które powinny być wykluczone z procesu konsolidatora, podczas gdy inne zestawy z zwykle połączony. Jest to przydatne, jeśli przy użyciu `[Preserve]` na niektóre zestawy jest niemożliwe (np. 3 kod firmy) lub tymczasowo dla usterki.

Odpowiada to `--linkskip` przy użyciu mtouch narzędzia wiersza polecenia.

Korzystając z **łączy wszystkie zestawy** opcję, jeśli chcesz sprawdzić konsolidator, aby pominąć całego zestawy, umieścić **mtouch dodatkowe argumenty** opcje zestawu najwyższego poziomu:

```csharp
--linkskip=NameOfAssemblyToSkipWithoutFileExtension
```

Jeśli chcesz, aby konsolidator, aby pominąć wiele zestawów, zawierają wiele `linkskip` argumentów:

```csharp
--linkskip=NameOfFirstAssembly --linkskip=NameOfSecondAssembly
```

Nie ma interfejsu użytkownika, aby użyć tej opcji, ale jego można podać w programie Visual Studio dla okna dialogowego Opcje projektu Mac lub w okienku właściwości projektu programu Visual Studio w **mtouch dodatkowe argumenty** pola tekstowego. (Np. *--linkskip = mscorlib* połączyć innych zestawów w rozwiązaniu, ale nie połączyć mscorlib.dll).

<a name="Disabling_Link_Away" />

### <a name="disabling-link-away"></a>Wyłączanie "Optymalizacji Link"

Konsolidator usunie kod, który jest bardzo prawdopodobne do użycia na urządzeniach, np. nieobsługiwany lub niedozwolone. W rzadkich przypadkach jest to możliwe, że aplikacja lub biblioteka zależy od tego kodu (praca lub nie). Od Xamarin.iOS 5.0.1 można należy poinstruować konsolidator, aby pominąć ten rodzaj optymalizacji.

Odpowiada to *- nolinkaway* przy użyciu mtouch narzędzia wiersza polecenia.

Nie ma interfejsu użytkownika, aby użyć tej opcji, ale jego można podać w programie Visual Studio dla okna dialogowego Opcje projektu Mac lub w okienku właściwości projektu programu Visual Studio w **mtouch dodatkowe argumenty** pola tekstowego. (Np. *--nolinkaway* nie może usunąć dodatkowego kodu (około 100 kb)).

### <a name="marking-your-assembly-as-linker-ready"></a>Oznaczenie używanego zestawu jako gotowe do konsolidatora

Użytkownicy mogą wybrać tylko zestawy SDK, a nie wszystkie łączenia do ich kodu.  Oznacza to również który żadnych bibliotek innych firm, które nie są częścią podstawowych na platformie Xamarin zestawu SDK nie zostanie połączone.

Dzieje się tak zazwyczaj, ponieważ nie chcą ręcznie dodać `[Preserve]` atrybuty do ich kodu.  Efektem ubocznym jest bibliotek innej firmy nie zostaną połączone, czy zazwyczaj jest dobrą domyślne ustawienie, ponieważ nie jest możliwość sprawdzenia, czy inna biblioteka jest przyjazną konsolidatora.

Jeśli masz biblioteki w projekcie, lub jesteś deweloperem wielokrotnego użytku bibliotek i ma konsolidator, aby traktować jako skorelowane z zestawu, musisz wykonać jest dodać atrybut poziomu zestawu [ `LinkerSafe` ](https://developer.xamarin.com/api/type/Foundation.LinkerSafeAttribute/), podobnie do następującej:

```csharp
[assembly:LinkerSafe]
```

Biblioteka nie faktycznie trzeba odwołania do bibliotek platformy Xamarin dla tego.  Na przykład, jeśli tworzysz przenośnej biblioteki klas, które zostanie uruchomione na innych platformach, nadal można `LinkerSafe` atrybutu.
Wyszukuje konsolidatora Xamarin `LinkerSafe` atrybutu według nazwy, a nie przez jego rzeczywistego typu.  Oznacza to, że można napisać ten kod i będzie ona również działać:

```csharp
[assembly:LinkerSafe]
// ... assembly attribute should be at top, before source
class LinkerSafeAttribute : System.Attribute {}
```

## <a name="custom-linker-configuration"></a>Konfiguracja niestandardowych konsolidatora

Postępuj zgodnie z [instrukcje dotyczące tworzenia pliku konfiguracji konsolidatora](~/cross-platform/deploy-test/linker.md).


## <a name="related-links"></a>Linki pokrewne

- [Konfiguracja konsolidatora niestandardowego](~/cross-platform/deploy-test/linker.md)
- [Łączenie dla komputerów Mac](~/mac/deploy-test/linker.md)
- [Łączenie w systemie Android](~/android/deploy-test/linker.md)
