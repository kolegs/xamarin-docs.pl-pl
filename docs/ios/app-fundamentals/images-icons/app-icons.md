---
title: Ikony aplikacji
description: "Ten artykuł obejmuje tym i zarządzanie zasób obrazu w aplikacji platformy Xamarin.iOS ma być używany jako ikonę aplikacji."
ms.topic: article
ms.prod: xamarin
ms.assetid: B7791574-4A0F-4CB6-8C18-36D40B5C91EB
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/22/2017
ms.openlocfilehash: 6de57e9523ff336c2e06e39903280db9c9ab95fa
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="application-icons"></a>Ikony aplikacji

_Ten artykuł obejmuje tym i zarządzanie zasób obrazu w aplikacji platformy Xamarin.iOS ma być używany jako ikonę aplikacji._

Poniższe tematy zostanie omówiona szczegółowo:

* [Aplikacja, uwagi i ustawienia ikon](#icon-types) — różne typy ikon wymagane przez aplikację systemu iOS.
* [Zarządzanie ikon z katalogami zasobów](#managing) — Zarządzanie ikony aplikacji przy użyciu zawartości katalogów.
* [iTunes kompozycji](#itunes) -dostarczanie wymagane iTunes kompozycji Ad Hoc metody dostarczania aplikacji.

<a name="icon-types" />

## <a name="application-spotlight-and-settings-icons"></a>Aplikacja, Spotlight i ustawienia ikon

W ten sam sposób aplikacji platformy Xamarin.iOS można użyć obrazu zasobów dla formantów interfejsu użytkownika i jako ikony dokumentu zasobów obrazu może służyć do zapewnienia ikony aplikacji. Poniższe zrzuty ekranu z iPad przedstawiono trzy zastosowań ikony w systemie iOS:

- **Ikona aplikacji** — wszystkie aplikacje systemu iOS musi definiować ikonę aplikacji. Jest to ikonę, która będzie wybierz użytkownika z ekranu głównego z systemem iOS można uruchomić aplikacji. Ponadto ta ikona jest stosowana przez Centrum gier, jeśli ma to zastosowanie. Przykład: 

    [ ![](app-icons-images/000.png "Ikona aplikacji")](app-icons-images/000-full.png)
- **Wyróżnione ikona** — zawsze, gdy użytkownik wprowadza nazwę aplikacji w wyszukiwanie Spotlight, ta ikona jest wyświetlana. Przykład: 

    [ ![](app-icons-images/000a.png "Ikona uwagi")](app-icons-images/000a-full.png)
- **Ikona ustawień** — Jeśli użytkownik wprowadzi **ustawienia** aplikacji na urządzeniu z systemem iOS, ta ikona pojawi się na końcu **ustawienia** listy aplikacji. Przykład: 

    [ ![](app-icons-images/000b.png "Ikona ustawień")](app-icons-images/000b-full.png)

Następujące rozmiary obrazów zasobów i rozwiązania będzie potrzebnych do obsługi wszystkich typów ikona wymagane przez aplikację platformy Xamarin.iOS przeznaczonych dla systemu iOS 5 za pomocą systemu iOS 9 (lub nowszego):

<table cellpadding="7" cellspacing="0" width="100%">
    <tr valign="top">
        <td width="200" style="border-width: 0px;"></td>
        <td colspan="5" align="center" bgcolor="#F0F0F0"><b>iPhone</b></td>
    </tr>
    <tr valign="center">
        <td width="200" style="border-width: 0px;"></td>
        <td colspan="2" align="center" bgcolor="#F9F9F9"><b>iOS 5 & 6</b></td>
        <td colspan="2" align="center" bgcolor="#F9F9F9"><b>iOS 7 & 8</b></td>
        <td align="center" bgcolor="#F9F9F9"><b>iOS 9 & 10<b><br/><i>(iPhone 6 i 7 Plus)</i></td>
    </tr>
    <tr valign="top" bgcolor="#F0F0F0">
        <td width="200" align="center"><b>Typ ikony</b></td>
        <td align="center"><b>1x</b></td>
        <td align="center"><b>2x</b></td>
        <td align="center"><b>1x</b></td>
        <td align="center"><b>2x</b></td>
        <td align="center"><b>3x</b></td>
    </tr>
    <tr valign="top">
        <td width="200" bgcolor="#F9F9F9" align="right">Ikona aplikacji</td>
        <td align="center">57x57</td>
        <td align="center">114x114</td>
        <td align="center" style="color:#BBBBBB;">60x60<sup>(1)</sup></td>
        <td align="center">120x120</td>
        <td align="center">180x180</td>
    </tr>
    <tr valign="top">
        <td width="200" bgcolor="#F9F9F9" align="right">Uwagi</td>
        <td align="center">29x29</td>
        <td align="center">58x58</td>
        <td align="center" style="color:#BBBBBB;">40x40<sup>(2)</sup></td>
        <td align="center">80x80</td>
        <td align="center">120x120</td>
    </tr>
    <tr valign="top">
        <td width="200" bgcolor="#F9F9F9" align="right">Ustawienia</td>
        <td align="center" style="color:#BBBBBB;">29x29<sup>(3)(4)</sup></td>
        <td align="center" style="color:#BBBBBB;">58x58<sup>(3)(4)</sup></td>
        <td align="center">-</td>
        <td align="center">-</td>
        <td align="center">87x87</td>
    </tr>
</table>

<table cellpadding="7" cellspacing="0" width="100%">
    <tr valign="top">
        <td width="200" style="border-width: 0px;"></td>
        <td colspan="5" align="center" bgcolor="#F0F0F0"><b>iPad</b></td>
    </tr>
    <tr valign="center">
        <td width="200" style="border-width: 0px;"></td>
        <td colspan="2" align="center" bgcolor="#F9F9F9"><b>iOS 5 & 6</b></td>
        <td colspan="2" align="center" bgcolor="#F9F9F9"><b>iOS 7 & 8</b></td>
        <td colspan="1" align="center" bgcolor="#F9F9F9"><b>iOS&nbsp;9 & 10</b></td>
    </tr>
    <tr valign="top" bgcolor="#F0F0F0">
        <td width="200" align="center"><b>Typ ikony</b></td>
        <td align="center"><b>1x</b></td>
        <td align="center"><b>2x</b></td>
        <td align="center"><b>1x</b></td>
        <td align="center"><b>2x</b></td>
        <td align="center"><b>2x<br/>iPad&nbsp;Pro</b></td>
    </tr>
    <tr valign="top">
        <td width="200" bgcolor="#F9F9F9" align="right">Ikona aplikacji</td>
        <td align="center">72x72</td>
        <td align="center">144x144</td>
        <td align="center">76x76</td>
        <td align="center">152x152</td>
        <td align="center">167x167<sup>(6)</sup></td>
    </tr>
    <tr valign="top">
        <td width="200" bgcolor="#F9F9F9" align="right">Uwagi</td>
        <td align="center">50x50</td>
        <td align="center">100x100</td>
        <td align="center">40x40</td>
        <td align="center">80x80</td>
        <td align="center" style="color:#BBBBBB;">120x120<sup>(5)</sup></td>
    </tr>
    <tr valign="top">
        <td width="200" bgcolor="#F9F9F9" align="right">Ustawienia</td>
        <td align="center" style="color:#BBBBBB;">29x29<sup>(3)(5)</sup></td>
        <td align="center" style="color:#BBBBBB;">58x58<sup>(3)(5)</sup></td>
        <td align="center">-</td>
        <td align="center">-</td>
        <td align="center" style="color:#BBBBBB;">58x58<sup>(5)</sup></td>
    </tr>
</table>

1. _Zarówno programu Visual Studio for Mac i Xcode nie obsługuje ustawienie 1 obrazu x dla systemu iOS 7._
2. _Ustawianie obrazu 1 x, dla systemu iOS 7 nie jest obsługiwane podczas korzystania z zawartości katalogów._
3. _iOS 7 i 8 używać tego samego rozmiary obrazów jako iOS 5 i 6._
4. _Używa tego samego obrazów i rozmiary ikona uwagi._
5. _Używa tego samego rozmiaru ikony jako telefonów iPhone._
6. _Obsługiwane tylko z zestawami obrazu katalogu zasobów._

Aby uzyskać więcej informacji na temat ikon, zobacz firmy Apple [ikony, jak i rozmiary obrazów](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/MobileHIG/IconMatrix.html#//apple_ref/doc/uid/TP40006556-CH27-SW1) dokumentacji.

<a name="managing" />

## <a name="managing-icons-with-asset-catalogs"></a>Zarządzanie ikon z katalogami zasobów

Ikony, specjalnego `AppIcons` zestaw obrazu mogą być dodawane do `Assets.xcassets` plik w projekcie aplikacji. Wszystkie wersję obrazu muszą obsługiwać wszystkie rozwiązania są uwzględnione w _xcasset_ i zgrupowane razem. Specjalne Edytor w programie Visual Studio for Mac umożliwia deweloperowi obejmują a graficznie instalację tych obrazów.

Aby użyć katalogu zasobów, wykonaj następujące czynności:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Kliknij dwukrotnie `Info.plist` w pliku **Eksploratora rozwiązań** go otworzyć do edycji.
2. Przewiń w dół do **ikony aplikacji** sekcji.
3. Z **źródła** listy rozwijanej liście, upewnij się, **AppIcons** wybrano: 

    ![](app-icons-images/migrate01.png "Upewnij się, że wybrano AppIcons")
4. Z **Eksploratora rozwiązań**, kliknij dwukrotnie `Assets.xcassets` plik, aby otworzyć do edycji: 

    ![](app-icons-images/asset01.png "Plik Assets.xcassets w Eksploratorze rozwiązań")
5. Wybierz `AppIcons` z listy zasobów, aby wyświetlić `Icon Editor`: 

    ![](app-icons-images/asset02.png "Edytor AppIcons")
6. Albo kliknij dany typ ikony i wybierz plik obrazu dla wymaganego typu/rozmiaru lub przeciągnij obrazu z folderu i upuść ją na żądany rozmiar.
7. Kliknij przycisk **Otwórz** przycisk, aby uwzględnić go w projekcie i ustaw go w xcasset.
8. Powtórz ten krok dla wszystkich potrzebnych obrazów.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Kliknij dwukrotnie **Info.plist** w pliku **Eksploratora rozwiązań**:

    ![](app-icons-images/icon01w.png "Wybierz Info.plist")
2. Polecenie **zasoby Visual** i kliknij polecenie **użyj katalogu zasobów** przycisku w obszarze **ikony aplikacji**: 

    ![](app-icons-images/icon02w.png "Wybierz kartę Zasoby Visual")
4. Z **Eksploratora rozwiązań**, rozwiń węzeł **katalogu zasobów** folderu: 

    ![](app-icons-images/image009.png "Rozwiń folder katalogu zasobów")
5. Kliknij dwukrotnie **nośnika** plik, aby otworzyć go w edytorze: 

    ![](app-icons-images/image010.png "Otwórz plik multimedialny w edytorze")
6. W obszarze **Explorer właściwości** dewelopera można wybrać różne typy i rozmiary ikon wymagane.
7. Kliknij dany typ ikony i wybierz plik obrazu dla wymaganego typu/rozmiaru.
8. Kliknij przycisk **Otwórz** przycisk, aby uwzględnić go w projekcie i ustaw go w xcasset.
9. Powtórz ten krok dla wszystkich potrzebnych obrazów.

-----

Jest to preferowana metoda w tym i zarządzanie zasobami obrazu, które będą służyć do zapewnienia ikony aplikacji, uwagi i ustawień dla aplikacji.

### <a name="migrating-from-infoplist-to-asset-catalogs"></a>Migrowanie z Info.plist do zawartości katalogów

Dla istniejących Xamarin.iOS aplikacji przy użyciu `Info.plist` plików do zarządzania jego ikony, zdecydowanie zalecane jest, że deweloper przełączyć go do użycia `AppIcons` zasób obrazu wewnątrz `Assets.xcassets`.

Wykonaj następujące czynności:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Kliknij dwukrotnie `Info.plist` w pliku **Eksploratora rozwiązań** go otworzyć do edycji.
2. Przewiń w dół do **ikony aplikacji** sekcji.
3. Z **źródła** listy rozwijanej wybierz **migracji do zawartości katalogów**: 

    ![](app-icons-images/migrate02.png "Wybierz opcję migracji do zawartości katalogów")
4. Wszystkie istniejące ikony zdefiniowane w `Info.plist` pliku zostaną zmigrowane do `AppIcons` Ustaw obraz dodany do `Assets.xcassets`: 

     ![](app-icons-images/migrate03.png "Zestaw AppIcons obrazu w Assets.xcassets")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Kliknij dwukrotnie `Info.plist` w pliku **Eksploratora rozwiązań** go otworzyć do edycji.
2. Kliknij na telefonie iPhone sekcji ikon: 

    ![](app-icons-images/image007.png "Edytor ikon iPhone Rhe")
3. Przewiń w dół do **ikony** sekcji.
4. Z **katalogu zasobów** listy rozwijanej wybierz **wykorzystania zasobów katalogi**.
5. Wszystkie istniejące ikony zdefiniowane w `Info.plist` pliku zostaną zmigrowane do `Images` dodane do zestawu `Assets.xcassets`.
6. Zapisać zmiany w `Info.plist` pliku.

-----

<a name="itunes" />

## <a name="itunes-artwork"></a>iTunes kompozycji

Jeśli przy użyciu metody Ad Hoc dostarczania aplikacji, (zarówno dla użytkowników w firmach i testowania wersji beta w rzeczywistych urządzeń), deweloper również musi zawierać 512 x 512 i obraz 1024 x 1024, który będzie używany do reprezentowania aplikacji w programach iTunes.

Aby określić iTunes kompozycji, wykonaj następujące czynności:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Kliknij dwukrotnie `Info.plist` w pliku **Eksploratora rozwiązań** go otworzyć do edycji.
2. Przewiń do **iTunes kompozycji** części edytora: 

    ![](app-icons-images/itunes01.png "Przewiń do sekcji kompozycji edytora iTunes")
3. Wszelkie brakujące obrazu, kliknij miniaturę w edytorze, wybierz plik obrazu dla kompozycji żądaną iTunes w oknie dialogowym Otwórz plik i kliknij przycisk **OK** przycisku.
4. Powtórz ten krok, aż wszystkie wymagane obrazy zostały określone dla aplikacji.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Kliknij dwukrotnie `Info.plist` w pliku **Eksploratora rozwiązań** go otworzyć do edycji.

2. Polecenie **zasoby Visual** karcie i rozwiń **iTunes kompozycji**: 

    ![](app-icons-images/itunes01w.png "Edytowanie iTunes kompozycji w programie Visual Studio")
4. Wszelkie brakujące obrazu, kliknij miniaturę w edytorze, wybierz plik obrazu dla kompozycji żądaną iTunes w oknie dialogowym Otwórz plik i kliknij przycisk **Otwórz** przycisku.
5. Powtórz ten krok, aż wszystkie wymagane obrazy zostały określone dla aplikacji.

-----

## <a name="related-links"></a>Linki pokrewne

- [Praca z obrazami (przykład)](https://developer.xamarin.com/samples/WorkingWithImages/)
- [Witaj, iPhone](~/ios/get-started/hello-ios/index.md)
- [Niestandardowe ikony, jak i wskazówki dotyczące tworzenia obrazu](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html))
