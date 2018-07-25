---
title: Debugowanie aplikacji platformy Xamarin.iOS
description: W tym dokumencie opisano, jak użyć debugera w programie Visual Studio for Mac lub Visual Studio 2017, aby debugować aplikację platformy Xamarin.iOS, w tym ustawianie punktów przerwania, debugowanie sieci bezprzewodowej i innych.
ms.prod: xamarin
ms.assetid: 05460010-99E1-DC38-F855-2D691EF54484
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 4b21a69e49c8c7fd79de8edac9858c4714657f1c
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242319"
---
# <a name="debugging-xamarinios-apps"></a>Debugowanie aplikacji platformy Xamarin.iOS

_Aplikacji platformy Xamarin.iOS mogą być debugowane za pomocą debugera wbudowanego w programie Visual Studio for Mac lub Visual Studio._

Używać programu Visual Studio dla komputerów Mac natywnych debugowania obsługę debugowania języka C# i innego kodu języków zarządzanych i [LLDB](http://lldb.llvm.org/tutorial.html) zaspokajanie debugowania języka C codethat C++ lub języka Objective C można może być konsolidowanie za pomocą projektu Xamarin.iOS.


> [!NOTE]
> Podczas kompilowania aplikacji w trybie debugowania rozszerzenia Xamarin.iOS wygeneruje wolniejszy i znacznie większych aplikacji, zgodnie z wszystkich wierszy kodu, należy go zinstrumentować. Przed wydaniem, upewnij się, że wykonać kompilację wydania.

Debuger platformy Xamarin.iOS jest zintegrowana z używanego środowiska IDE i umożliwia deweloperom debugowanie aplikacji platformy Xamarin.iOS utworzonych za pomocą dowolnego języki zarządzane, obsługiwana przez platformy Xamarin.iOS w symulatorze, a na urządzeniu.

Debuger platformy Xamarin.iOS używa [nietrwałego debugera Mono](http://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/), co oznacza, że wygenerowany kod i środowiska uruchomieniowego Mono współpracują ze środowiskiem IDE, aby zapewnić środowisko debugowania. Stanowi to odmianę twardych debugery, takich jak LLDB lub MDB, kontrolujące programu bez wiedzy lub współpracy z debugowanego programu.

## <a name="setting-breakpoints"></a>Ustawianie punktów przerwania

Jeśli jesteś gotowy do uruchomienia debugowania aplikacji pierwszym krokiem jest [ustawiania punktów przerwania aplikacja](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/set_a_breakpoint). Można to zrobić przez kliknięcie w obszarze marginesu edytora, obok numer wiersza kodu, nad którym chcesz przerwać:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](debugging-in-xamarin-ios-images/debugging1.png "Ustawianie punktów przerwania")](debugging-in-xamarin-ios-images/debugging1.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](debugging-in-xamarin-ios-images/debugging1a.png "Ustawianie punktów przerwania")](debugging-in-xamarin-ios-images/debugging1a.png#lightbox)

-----

Możesz wyświetlić wszystkie punkty przerwania, które zostały ustawione w kodzie, przechodząc do **Konsola punktów przerwania**:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](debugging-in-xamarin-ios-images/image0a.png "Konsola punktów przerwania")](debugging-in-xamarin-ios-images/image0a.png#lightbox)

 Jeśli konsola punkty przerwania nie są automatycznie wyświetlane, można tworzyć widoczne, wybierając _Widok > Windows Debuguj > punktów przerwania_
 
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](debugging-in-xamarin-ios-images/image0.png "Konsola punktów przerwania")](debugging-in-xamarin-ios-images/image0.png#lightbox)

 Jeśli konsola punkty przerwania nie są automatycznie wyświetlane, można tworzyć widoczne, wybierając _Debuguj > Windows > punktów przerwania_
 
-----

Przed rozpoczęciem debugowania aplikacji zawsze upewnij się, że konfiguracja jest ustawiona na **debugowania**, ponieważ zawiera on przydatne zestaw narzędzi do obsługi debugowania, takie jak punkty przerwania, przy użyciu wizualizatorów danych i wyświetlanie stosu wywołań:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](debugging-in-xamarin-ios-images/debugging7.png "Debugowanie w symulatorze")](debugging-in-xamarin-ios-images/debugging7.png#lightbox)
[![](debugging-in-xamarin-ios-images/debugging7a.png "debugowania na urządzeniu fizycznym")](debugging-in-xamarin-ios-images/debugging7a.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](debugging-in-xamarin-ios-images/debugging7c.png "Debugowanie w symulatorze")](debugging-in-xamarin-ios-images/debugging7c.png#lightbox)
[![](debugging-in-xamarin-ios-images/debugging7d.png "debugowania na urządzeniu fizycznym")](debugging-in-xamarin-ios-images/debugging7d.png#lightbox)

-----

## <a name="start-debugging"></a>Rozpocznij debugowanie
Aby rozpocząć debugowanie, wybierz urządzenie docelowe lub podobne w środowisku IDE:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](debugging-in-xamarin-ios-images/debugging7b.png "Wybierz urządzenie docelowe")](debugging-in-xamarin-ios-images/debugging7b.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](debugging-in-xamarin-ios-images/debugging7e.png "Wybierz urządzenie docelowe")](debugging-in-xamarin-ios-images/debugging7e.png#lightbox)

-----



Następnie Wdróż aplikację, naciskając klawisz **Odtwórz** przycisku.

Po osiągnięciu punktu przerwania kod będzie wyróżniony żółtym:

[![](debugging-in-xamarin-ios-images/image2.png "Kod będzie wyróżniony żółtym")](debugging-in-xamarin-ios-images/image2.png#lightbox)

Debugowania narzędzi, takich jak sprawdzanie wartości obiektów, można w tym momencie uzyskać więcej informacji o tym, co się dzieje w kodzie:

[![](debugging-in-xamarin-ios-images/image3.png "Wyświetlanie wartości koloru")](debugging-in-xamarin-ios-images/image3.png#lightbox)

## <a name="conditional-breakpoints"></a>Warunkowe punkty przerwania

Można również ustawić zasady dyktowanie okoliczności, w których powinny być wykonywane punkt przerwania, jest wiedzieć, jak dodawanie *warunkowego punktu przerwania*.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Aby ustawić warunkowego punktu przerwania, dostęp do **okna Właściwości punktu przerwania**, które można zrobić na dwa sposoby:


- Aby dodać nowe warunkowego punktu przerwania, kliknij prawym przyciskiem myszy na marginesu edytora, numer wiersza dla kodu, który chcesz ustawić punkt przerwania, po lewej stronie, a następnie wybierz nowy punkt przerwania:

    [![](debugging-in-xamarin-ios-images/image4.png "Wybierz nowy punkt przerwania")](debugging-in-xamarin-ios-images/image4.png#lightbox)

- Aby dodać warunek istniejącym punkcie przerwania, kliknij prawym przyciskiem myszy punkt przerwania, a następnie wybierz pozycję **właściwości punktu przerwania** lub **Konsola punktów przerwania** kliknij przycisk Właściwości przedstawione poniżej:

    [![](debugging-in-xamarin-ios-images/image5.png "Konsola punktów przerwania")](debugging-in-xamarin-ios-images/image5.png#lightbox)


Następnie możesz wprowadzić warunek, pod którym ma punkt przerwania, wystąpienia:

[![](debugging-in-xamarin-ios-images/image6.png "Wprowadź warunek punktu przerwania, wystąpienia")](debugging-in-xamarin-ios-images/image6.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Aby ustawić warunkowego punktu przerwania w programie Visual Studio 2015, najpierw [Ustaw punkt przerwania regularne](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/set_a_breakpoint). Kliknij prawym przyciskiem myszy na punkcie przerwania, aby wyświetlić jego menu kontekstowe:

 [![](debugging-in-xamarin-ios-images/image4vs.png "Menu kontekstowe punktu przerwania")](debugging-in-xamarin-ios-images/image4vs.png#lightbox)

Wybierz **warunki...**  do wyświetlenia _ustawienia punktu przerwania_ menu:

 [![](debugging-in-xamarin-ios-images/image6vs.png "W menu Ustawienia punktu przerwania")](debugging-in-xamarin-ios-images/image6vs.png#lightbox)

W tym miejscu można wprowadzić warunków, w których punkt przerwania, wystąpienia

Aby uzyskać więcej informacji na temat korzystania z warunków punktu przerwania we wcześniejszych wersjach programu Visual Studio, zobacz [dokumentację programu Visual Studio](https://docs.microsoft.com/visualstudio/debugger/using-breakpoints) na ten temat.

-----

## <a name="navigating-through-code"></a>Nawigowanie po kodzie za

Po osiągnięciu punktu przerwania narzędzi debugowania pozwalają uzyskać kontrolę nad wykonywania programu. IDE spowoduje wyświetlenie czterech przycisków, co pozwala na uruchamianie i przejść przez kod.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

W programie Visual Studio dla komputerów Mac będzie wyglądać następująco:

 [![](debugging-in-xamarin-ios-images/image7.png "Narzędzia debugowania umożliwiają deweloperowi uzyskać kontrolę nad wykonywania programu")](debugging-in-xamarin-ios-images/image7.png#lightbox)

Są to:

- **Odtwórz/Zatrzymaj** — to spowoduje rozpoczęcie i zakończenie wykonywania kodu, aż do następnego punktu przerwania.
- **Step Over** — spowoduje to wykonanie następnego wiersza kodu. Jeśli następny wiersz jest wywołanie funkcji, krok ponad spowoduje wykonanie funkcji i będzie Zatrzymaj w następnym wierszu kodu _po_ funkcji.
- **Step Into** — spowoduje to również wykonanie następnego wiersza kodu. Jeśli następny wiersz jest wywołanie funkcji, Step Into przestanie w pierwszym wierszu funkcji, co pozwala na dalsze wiersz po wierszu debugowanie funkcji. Jeśli następny wiersz nie jest funkcją, będzie ona działać taka sama jak Step Over.
- **Step Out** — spowoduje to zwrócenie do wiersza gdy wywołana została funkcja bieżąca.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

W programie Visual Studio będzie wyglądać następująco:

[![](debugging-in-xamarin-ios-images/image7vs.png "Narzędzia debugowania umożliwiają deweloperowi uzyskać kontrolę nad wykonywania programu")](debugging-in-xamarin-ios-images/image7vs.png#lightbox)

Są to:

- **Odtwórz/Zatrzymaj** — to spowoduje rozpoczęcie i zakończenie wykonywania kodu, aż do następnego punktu przerwania.
- **Przekrocz (F11)** — spowoduje to wykonanie następnego wiersza kodu. Jeśli następny wiersz jest wywołanie funkcji, krok ponad spowoduje wykonanie funkcji i będzie Zatrzymaj w następnym wierszu kodu _po_ funkcji.
- **Wkrocz (F10)** — spowoduje to również wykonanie następnego wiersza kodu. Jeśli następny wiersz jest wywołanie funkcji, Step Into przestanie w pierwszym wierszu funkcji, co pozwala na dalsze wiersz po wierszu debugowanie funkcji. Jeśli następny wiersz nie jest funkcją, będzie ona działać taka sama jak Step Over.
- **Step Out (Shift + F11)** — spowoduje to zwrócenie do wiersza gdy wywołana została funkcja bieżąca.

Aby uzyskać więcej informacji w dokumentacji głębokości na debugowanie zobacz [Przejdź kodu za pomocą debugera programu Visual Studio](https://docs.microsoft.com/visualstudio/debugger/navigating-through-code-with-the-debugger).

-----

### <a name="breakpoints"></a>Punkty przerwania

Warto wspomnieć, że systemu iOS daje aplikacjom tylko kilka sekund (10) do uruchamiania i ukończyć `FinishedLaunching` metody delegata aplikacji. Jeśli aplikacja nie zostanie ukończone tę metodę w ciągu 10 sekund, z systemem iOS będzie kasowanie procesu.

Oznacza to, że jest prawie niemożliwe ustawić punkty przerwania dla kodu uruchomienia programu. Jeśli chcesz debugować kod startowy, należy opóźnić niektóre inicjowania i umieścić te dane do metody wywoływane przez czasomierz lub w innej postaci metody wywołania zwrotnego, który jest wykonywany po zakończeniu FinishedLaunching.

## <a name="device-diagnostics"></a>Diagnostyka urządzenia

Jeśli występuje błąd podczas konfigurowania debugera, aby umożliwić szczegółowe dane diagnostyczne, dodając "-v - v - v" na dodatkowe argumenty mtouch w opcjach projektu. Spowoduje to wydrukowanie szczegółowe informacje o błędzie w konsoli urządzenia.

 <a name="WiFi_Debugging" />

## <a name="wireless-debugging"></a>Bezprzewodowe debugowania

Domyślnie w rozszerzeniu Xamarin.iOS nie jest debugowanie aplikacji na urządzeniach za pośrednictwem połączenia USB. Czasami urządzenia USB mogą być wymagane do przetestowania podłączyć/odłączyć kabel do tworzenia aplikacji bazujących na ExternalAccessory. W takich przypadkach można użyć, debugowanie za pośrednictwem sieci bezprzewodowej.

Aby uzyskać więcej informacji na temat wdrażania sieci bezprzewodowej i debugowania, zobacz [wdrażanie bezprzewodowe](~/ios/deploy-test/wireless-deployment.md) przewodnik.

<a name="Technical_Details" />

## <a name="technical-details"></a>Szczegóły techniczne

Xamarin.iOS używa nowego nietrwałego debuger środowiska Mono. W przeciwieństwie do standardowych debuger środowiska Mono, czyli program, który kontroluje oddzielny proces przy użyciu interfejsów systemu operacyjnego do kontrolowania oddzielny proces nietrwałe debuger działa przez środowisko uruchomieniowe Mono udostępniają funkcje debugowania za pośrednictwem protokołu o komunikacji sieciowej.

Podczas uruchamiania aplikacji, która ma być debugowany kontaktów debugera i debugera rozpoczyna działanie. W rozszerzeniu Xamarin.iOS dla programu Visual Studio Xamarin Mac Agent działa jako środka man między aplikacją (w programie Visual Studio) i debugera.

Ten debuger nietrwałego wymaga współpracy schemat debugowania podczas uruchamiania na urządzeniu. Oznacza to, że plik binarny kompilacji podczas debugowania jest większy, ponieważ kod został zinstrumentowany zawierać dodatkowego kodu na każdym etapie sekwencji, aby zapewnić obsługę debugowania.

<a name="Accessing_the_Console" />


## <a name="accessing-the-console"></a>Dostęp do konsoli

Dzienniki awarii i dane wyjściowe klasy konsoli będą wysyłane do konsoli dla telefonu iPhone. Można uzyskać dostęp do tej konsoli za pomocą edytora Xcode organizator "", a następnie wybrać urządzenie organizatora.

Alternatywnie, jeśli chcesz uruchomić program Xcode, można użyć firmy Apple [iPhone narzędziu konfiguracji](http://www.apple.com/support/iphone/enterprise/) bezpośrednio dostępu do konsoli. Ma to atutem, czy są dostępne dzienniki konsoli maszyny Windows Jeśli debugujesz problem w tym polu.

Dla użytkowników programu Visual Studio w oknie dane wyjściowe są dostępne kilka dzienników, ale możesz należy przełączyć się do komputer Mac na potrzeby bardziej dokładne i szczegółowe dzienniki.

-----

<a name="Debugging_Mono's_Class_Libraries" />


## <a name="debugging-monos-class-libraries"></a>Debugowanie biblioteki klas platformy Mono firmy

Xamarin.iOS jest dostarczany z kodu źródłowego dla bibliotek klas firmy Mono i służy to do pojedynczego kroku z debugera można zobaczyć, jak wszystko działa pod maską.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Ponieważ ta funkcja zużywa więcej pamięci podczas debugowania, to jest domyślnie wyłączona.


Aby włączyć tę funkcję, upewnij się, **Debuguj tylko kod projektu; nie wkraczaj do kodu struktury** opcja jest wyłączona w ramach _programu Visual Studio dla komputerów Mac > Preferencje > debuger_ menu, zgodnie z opisami poniżej:

[![](debugging-in-xamarin-ios-images/debugging6.png "Debugowanie biblioteki klas platformy Mono firmy")](debugging-in-xamarin-ios-images/debugging6.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Do debugowania biblioteki klas w programie Visual Studio, należy wyłączyć **tylko mój kod** w obszarze _Debuguj > Opcje_ menu. W _debugowanie > Ogólne_ węzła, wyczyść **Włącz tylko mój kod** pola wyboru:

[![](debugging-in-xamarin-ios-images/debugging6vs.png "Debugowanie biblioteki klas platformy Mono firmy")](debugging-in-xamarin-ios-images/debugging6vs.png#lightbox)

-----

Po wykonaniu tej czynności można uruchomić aplikację i jednego kroku do żadnych bibliotek klas podstawowych platformy Mono firmy.


## <a name="related-links"></a>Linki pokrewne

- [Debugowanie za pomocą platformy Xamarin](/visualstudio/mac/debugging/)
- [Wizualizacje danych](/visualstudio/mac/data-visualizations/)
- [Ustaw punkt przerwania](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/set_a_breakpoint)
- [Przechodź przez kod](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/step_through_code)
- [Informacje o danych wyjściowych do okna dziennika](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/output_information_to_log_window)
