---
title: Skoroszyty interakcyjne
description: "Aby utworzyć dokumentów na żywo z kodu C# na eksperymentowanie, użyj skoroszyty nauczania szkolenia lub eksploracji."
ms.topic: article
ms.prod: xamarin
ms.assetid: B79E5DE9-5389-4691-9AA3-FF4336CE294E
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 03/30/2017
ms.openlocfilehash: bd53074fcde25736d37544efc719ecef1110c364
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="interactive-workbooks"></a>Skoroszyty interakcyjne

_Aby utworzyć dokumentów na żywo z kodu C# na eksperymentowanie, użyj skoroszyty nauczania szkolenia lub eksploracji._

Skoroszyty służy jako aplikacja autonomiczna, niezależnie od programu IDE.

Aby rozpocząć tworzenie nowy skoroszyt, uruchom aplikację skoroszytów. Jeśli to nie zainstalowano już, odwiedź stronę [instalacji](~/tools/workbooks/install.md#install) strony. Użytkownik zostanie wyświetlony monit o utworzenie skoroszytu w danej platformy wyboru, które będą automatycznie łączyć się aplikacji agenta, co umożliwia wizualizowanie dokumentu w czasie rzeczywistym.

Jeśli skoroszyty aplikacji jest już uruchomiona, można utworzyć nowy dokument, przechodząc do **Plik > Nowy**.

Skoroszyty można zapisać i otworzyć ponownie później w aplikacji. Następnie można udostępnić osobom przedstawienie pomysły, Poznaj nowe interfejsy API lub uczy nowych pojęć.

## <a name="code-editing"></a>Edytowanie kodu

Okno edycji kodu zawiera uzupełniania kodu, kolorowanie składni wbudowanego live Diagnostyka i obsługi instrukcji wiele wierszy.

[ ![](workbook-images/inspector-0.6.0-repl-small.png "Okno edycji kodu udostępnia uzupełniania kodu, kolorowanie składni wbudowanego live Diagnostyka i obsługi instrukcji wiele wierszy")](workbook-images/inspector-0.6.0-repl.png)

Skoroszyty Xamarin są zapisywane w `.workbook` pliku, który jest plikiem CommonMark z niektóre metadane u góry (zobacz [typów plików skoroszytów](#Workbooks_Files_Types) więcej szczegółów, w jaki sposób można zapisać skoroszyty).

### <a name="nuget-package-support"></a>Obsługa pakietu NuGet

Wielu popularnych pakiety NuGet są obsługiwane bezpośrednio w skoroszytach Xamarin. Można wyszukiwać pakietów, przechodząc do **pliku > Dodaj pakiet**. Dodawanie pakietu zostanie automatycznie wyświetlone `#r` instrukcje odwołujące się do zestawów pakietu, co umożliwia korzystania z nich od razu.

Skoroszyt z odwołaniami do pakietu, te odwołania są także zapisywane. Jeśli współużytkujesz skoroszyt innej osobie, zostaną automatycznie pobrane pakiety, do którego istnieje odwołanie.

Dostępne są niektóre znane ograniczenia z obsługą pakietu NuGet w skoroszytach:

  * Natywnej biblioteki są obsługiwane tylko w systemie iOS i tylko wtedy, gdy są połączone z zarządzanej biblioteki.
  * Pakiety, które są zależne od `.targets` plików lub skryptów programu PowerShell prawdopodobnie nie będą działały zgodnie z oczekiwaniami.
  * Aby usunąć lub zmodyfikować zależności pakietu, należy edytować skoroszytu w manifeście za pomocą edytora tekstu. Zarządzanie prawidłowego pakietu jest w drodze.

### <a name="xamarinforms-support"></a>Obsługa platformy Xamarin.Forms

Jeśli pakiet NuGet platformy Xamarin.Forms odwoływać się w skoroszycie, aplikacji skoroszytu zostanie zmieniona jego głównym widoku na podstawie platformy Xamarin.Forms. Można do niego dostęp za pośrednictwem `Xamarin.Forms.Application.Current.MainPage`.

Na karcie Widok inspektora również ma specjalną obsługę wyświetlania hierarchii widoku platformy Xamarin.Forms ułatwią zrozumienie układów.

## <a name="rich-text-editing"></a>Edytowanie tekstu sformatowanego

Można edytować tekst wokół kod za pomocą edytora tekstu sformatowanego uwzględnione, jak przedstawiono poniżej:

![](workbook-images/inspector-0.6.2-editing.gif "Edytowanie tekstu wokół kodu za pomocą edytora tekstu sformatowanego wbudowane")

### <a name="markdown-authoring"></a>Tworzenie języka znaczników markdown

Skoroszyt autorzy mogą czasami łatwiej bezpośredniego edytowania CommonMark "source" skoroszytu z ulubionego edytora.

Należy pamiętać, że jeśli następnie edytować i zapisać skoroszyt w kliencie skoroszyty tekst CommonMark może ponownie sformatowany.

Należy pamiętać, z powodu rozszerzenia CommonMark używanym do Włącz yaml programu metadane w plikach skoroszytu `---` jest zarezerwowany do tego celu. Jeśli chcesz utworzyć [tematyczna podziały](http://spec.commonmark.org/0.27/#thematic-break) w tekście, należy użyć `***` lub `___` zamiast tego. Należy unikać takich podziały w skoroszytach 1.2 i wcześniej z powodu błędu podczas zapisywania.

### <a name="improvements-in-workbooks-13"></a>Ulepszenia w skoroszytach 1.3

Firma Microsoft również zostały rozszerzone składnia języka znaczników Markdown bloku oferty nieco zwiększające prezentacji. Dodając emoji jako pierwszy znak w ofercie bloku, mogą mieć wpływ kolor tła oferty:

- `> [!NOTE]
>"będzie renderować jako Uwaga na niebieskim tle
- `> [!IMPORTANT]
>"spowoduje, że jako ostrzeżenie żółtym tle
- `> [!WARNING]
>"będzie renderować jako problem z czerwonym tła

Można także połączyć nagłówków w dokumencie skoroszytu. Firma Microsoft Generowanie zakotwiczenia dla każdego nagłówka o identyfikatorze zakotwiczenia jest tekst nagłówka przetwarzana w następujący sposób:

1. Nagłówek jest dolna — z uwzględnieniem wielkości liter.
1. Zostaną usunięte wszystkie znaki oprócz alfanumeryczne i łączniki.
1. Wszystkie spacje są zastępowane kreski.

Oznacza to, że nagłówek takich jak "Ważne nagłówka" pobiera identyfikator `important-header` i można go połączyć z wstawiając łącze do `#important-header` w skoroszycie.

## <a name="document-structure"></a>Struktura dokumentu

### <a name="cell"></a>Komórki

Osobne jednostki zawartości, reprezentująca kodu wykonywalnego lub markdown. Komórki kodu składa się z maksymalnie cztery składniki podrzędne:

- Edytor
  - Bufor
- Diagnostyki kompilatora
- Dane wyjściowe konsoli
- Wyniki wykonania

### <a name="editor"></a>Edytor

Składnik interakcyjne tekstu komórki. Komórki kodu to edytor rzeczywisty kod z wyróżnianie składni itp. W przypadku komórek markdown jest zawartości edytorze tekstu sformatowanego kontekstowej formatowanie i tworzenia paska narzędzi.

### <a name="buffer"></a>Bufor
Zawartość edytora tekstu.

### <a name="compiler-diagnostics"></a>Diagnostyki kompilatora

Wszelkie diagnostyki utworzonego podczas kompilowania kodu renderowana tylko wtedy, gdy wymagane jest wykonanie jawnego. Natychmiast wyświetlone poniżej edytora komórki.

### <a name="console-output"></a>Dane wyjściowe konsoli

Podczas wykonywania komórki zapisywane na wyjście standardowe i błąd standardowy żadnych danych wyjściowych. Wyjście standardowe będzie renderowany w tekście czarne i błąd standardowy będzie odtwarzany na czerwony.

### <a name="execution-results"></a>Wyniki wykonania

Reprezentacje rozbudowanych, interakcyjnych potencjalnie wyników dla komórki są wyświetlane po pomyślnej kompilacji, pod warunkiem wynik faktycznie jest generowany przez wykonanie. Wyjątki są uznawane za wyników w tym kontekście, ponieważ są one tworzone wyniku rzeczywistego wykonania kompilacji.

## <a name="workbooks-files-types"></a>Typy plików skoroszytów

### <a name="plain-files"></a>Zwykłe pliki

Domyślnie skoroszyt jest zapisywany jako zwykły tekst `.workbook` plik zawierający tekst w formacie CommonMark.

### <a name="packages"></a>Pakiety

Pakiet skoroszyt jest katalog o nazwie z `.workbook` rozszerzenia.
Wyszukiwania dla komputerów Mac i okna dialogowego Otwieranie skoroszytów Xamarin i ostatnie menu plików ten katalog zostanie rozpoznany, tak jakby był on pliku.

Katalog musi zawierać `index.workbook` pliku, który jest skoroszytu zwykłego tekstu, który zostanie załadowany w skoroszytach Xamarin. Katalog może również zawierać zasoby wymagane przez `index.workbook`, w tym obrazów lub innych plików.

Jeśli jako zwykły tekst `.workbook` pliku, który odwołuje się do zasobów z jego samego katalogu jest otwarty w skoroszytach 0.99.3 lub później, po jego zapisaniu go zostanie skonwertowana na `.workbook` pakietu. Dotyczy to zarówno komputerów Mac i systemu Windows.

> [!NOTE]
> **Uwaga:** użytkowników systemu Windows zostanie otwarty `package.workbook\index.workbook` pliku bezpośrednio, ale w przeciwnym razie pakietu będą zachowywać się takie same jak na komputerach Mac.

### <a name="archives"></a>Archiwa

Pakiety skoroszytu, trwa katalogów, może być trudne do rozpowszechniania łatwo za pośrednictwem Internetu. Rozwiązanie to archiwa skoroszytu. Archiwum skoroszyt jest pakietem skoroszytu skompresowane zip o nazwie `.workbook` rozszerzenia.

Skoroszyty 1.1, począwszy od podczas zapisywania pakietu skoroszytu, okno dialogowe Zapisz dostępne są Zapisywanie jako archiwum zamiast tego. Skoroszyty 1.0 nie mieli wbudowane możliwości tworzenia lub zapisywania archiwa.

W skoroszytach 1.0, gdy archiwum skoroszyt został otwarty, niewidocznie został skonwertowany do pakietu skoroszytu i plik zip zostało utracone. W skoroszytach 1.1 pozostaje pliku zip. Gdy użytkownik zapisuje archiwum, zastępuje się przy użyciu nowego pliku zip.

Utwórz archiwum skoroszytu ręcznie prawym przyciskiem myszy pakiet skoroszytu i wybierając **Kompresuj** dla komputerów Mac, lub **Wyślij do > skompresowanego folderu (zip)** w systemie Windows. Zmień nazwę pliku zip `.workbook` rozszerzenia pliku. Działa to tylko pakiety skoroszytu, skoroszyt nie zwykłe pliki.

## <a name="related-links"></a>Linki pokrewne

- [Zapraszamy do skoroszytów](https://developer.xamarin.com/workbooks/workbooks/getting-started/welcome.workbook)
