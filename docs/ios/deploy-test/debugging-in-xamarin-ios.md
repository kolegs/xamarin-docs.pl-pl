---
title: Debugowanie
description: Aplikacji platformy Xamarin.iOS może być debugowany z wbudowanych debugera programu Visual Studio dla komputerów Mac lub Visual Studio.
ms.prod: xamarin
ms.assetid: 05460010-99E1-DC38-F855-2D691EF54484
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 8848dd20683163cd42215fe496dd7ff6a9e9f0c7
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="debugging"></a>Debugowanie

_Aplikacji platformy Xamarin.iOS może być debugowany z wbudowanych debugera programu Visual Studio dla komputerów Mac lub Visual Studio._

Użyj programu Visual Studio dla macierzystą obsługę debugowania dla komputerów Mac do debugowania języka C# i innego kodu zarządzanego języków i używać [LLDB](http://lldb.llvm.org/tutorial.html) gdy chcesz debugować C codethat C++ lub języka Objective C można może być połączenie z projektu platformy Xamarin.iOS.


> [!NOTE]
> Podczas kompilowania aplikacji w trybie debugowania Xamarin.iOS wygeneruje wolniejsze i znacznie większe aplikacji zgodnie z każdym wierszu kodu musi być instrumentowany. Przed wydaniem, upewnij się, że wykonać kompilacji wydania.

Debuger platformy Xamarin.iOS jest zintegrowany z IDE i umożliwia deweloperom debugowanie aplikacji platformy Xamarin.iOS utworzonych za pomocą dowolnego z zarządzanych języki obsługiwane przez Xamarin.iOS w symulatorze i na urządzeniu.

Debuger platformy Xamarin.iOS używa [debugera nietrwałego Mono](http://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/), co oznacza, że wygenerowany kod i środowiska uruchomieniowego Mono współpracować z IDE w celu zapewnienia obsługi debugowania. To jest inny niż twardych debugery LLDB lub MDB które kontrolują programu bez wiedzy lub współpracy z debugowanego programu.

## <a name="setting-breakpoints"></a>Ustawianie punktów przerwania

Gdy wszystko będzie gotowe można rozpocząć debugowania aplikacji pierwszym krokiem jest [Ustaw punkty przerwania aplikacji](https://developer.xamarin.com/recipes/cross-platform/ide/debugging/set_a_breakpoint/). Można to zrobić, klikając w obszarze marginesu edytora, obok numer wiersza kodu, który chcesz podzielić na:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](debugging-in-xamarin-ios-images/debugging1.png "Ustawianie punktów przerwania")](debugging-in-xamarin-ios-images/debugging1.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](debugging-in-xamarin-ios-images/debugging1a.png "Ustawianie punktów przerwania")](debugging-in-xamarin-ios-images/debugging1a.png#lightbox)

-----

Można wyświetlić wszystkie punkty przerwania, które zostały ustawione w kodzie, przechodząc do **konsoli punktów przerwania**:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](debugging-in-xamarin-ios-images/image0a.png "Konsola punkty przerwania")](debugging-in-xamarin-ios-images/image0a.png#lightbox)

 Jeśli konsola punkty przerwania nie są automatycznie wyświetlane, można tworzyć i widoczne po wybraniu _Widok > debugowanie Windows > punkty przerwania_
 
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](debugging-in-xamarin-ios-images/image0.png "Konsola punkty przerwania")](debugging-in-xamarin-ios-images/image0.png#lightbox)

 Jeśli konsola punkty przerwania nie są automatycznie wyświetlane, można tworzyć i widoczne po wybraniu _Debuguj > Windows > punkty przerwania_
 
-----

Przed rozpoczęciem debugowania dowolnej aplikacji, upewnij się, że konfiguracja jest ustawiona na **debugowania**, ponieważ zawiera przydatne zestaw narzędzi do obsługi debugowania, takich jak punkty przerwania, przy użyciu danych wizualizatorów i wyświetlając stosu wywołań:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](debugging-in-xamarin-ios-images/debugging7.png "Debugowanie w symulatorze")](debugging-in-xamarin-ios-images/debugging7.png#lightbox)
[![](debugging-in-xamarin-ios-images/debugging7a.png "debugowania na urządzeniu fizycznym")](debugging-in-xamarin-ios-images/debugging7a.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](debugging-in-xamarin-ios-images/debugging7c.png "Debugowanie w symulatorze")](debugging-in-xamarin-ios-images/debugging7c.png#lightbox)
[![](debugging-in-xamarin-ios-images/debugging7d.png "debugowania na urządzeniu fizycznym")](debugging-in-xamarin-ios-images/debugging7d.png#lightbox)

-----

## <a name="start-debugging"></a>Rozpocznij debugowanie
Aby rozpocząć debugowanie, wybierz urządzenie docelowe lub podobne środowiskiem IDE:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](debugging-in-xamarin-ios-images/debugging7b.png "Wybierz urządzenie docelowe")](debugging-in-xamarin-ios-images/debugging7b.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](debugging-in-xamarin-ios-images/debugging7e.png "Wybierz urządzenie docelowe")](debugging-in-xamarin-ios-images/debugging7e.png#lightbox)

-----



Następnie Wdróż aplikację, naciskając klawisz **odtwarzanie** przycisku.

Po trafieniu punktu przerwania kod będzie wyróżniony żółty:

[![](debugging-in-xamarin-ios-images/image2.png "Kod będzie wyróżniony żółty")](debugging-in-xamarin-ios-images/image2.png#lightbox)

Debugowanie narzędzi, takich jak sprawdzanie wartości obiektów, można w tym momencie uzyskać więcej informacji o tym, co się dzieje w kodzie:

[![](debugging-in-xamarin-ios-images/image3.png "Wyświetlanie wartości koloru")](debugging-in-xamarin-ios-images/image3.png#lightbox)

## <a name="conditional-breakpoints"></a>Warunkowe punkty przerwania

Można również ustawić reguły, dyktowania okoliczności, w których punkt przerwania wystąpi, jest to nazywane Dodawanie *warunkowych punktów przerwania*.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Aby ustawić warunkowe punktu przerwania, Uzyskaj dostęp do **okna Właściwości punktu przerwania**, które można wykonać na dwa sposoby:


- Aby dodać nowego punktu przerwania warunkowego, kliknij prawym przyciskiem myszy na marginesie edytor z lewej strony numer wiersza dla kodu, który chcesz ustawić punkt przerwania i wybierz nowego punktu przerwania:

    [![](debugging-in-xamarin-ios-images/image4.png "Wybierz nowego punktu przerwania")](debugging-in-xamarin-ios-images/image4.png#lightbox)

- Aby dodać warunek do istniejącego punktu przerwania, kliknij prawym przyciskiem myszy punkt przerwania i wybierz **właściwości punktu przerwania** lub **konsoli punktów przerwania** kliknij przycisk Właściwości zostały pokazane poniżej:

    [![](debugging-in-xamarin-ios-images/image5.png "Konsola punkty przerwania")](debugging-in-xamarin-ios-images/image5.png#lightbox)


Warunek, pod którym można wprowadzić do punktu przerwania występuje:

[![](debugging-in-xamarin-ios-images/image6.png "Wprowadź warunku punktu przerwania występuje")](debugging-in-xamarin-ios-images/image6.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Aby ustawić warunkowe punktu przerwania w programie Visual Studio 2015, najpierw [Ustaw punkt przerwania regularne](https://developer.xamarin.com/recipes/cross-platform/ide/debugging/set_a_breakpoint/). Kliknij prawym przyciskiem myszy punkt przerwania, aby wyświetlić jego menu kontekstowe:

 [![](debugging-in-xamarin-ios-images/image4vs.png "Menu kontekstowe punktu przerwania")](debugging-in-xamarin-ios-images/image4vs.png#lightbox)

Wybierz **warunki...**  do wyświetlenia _ustawienia punktów przerwania_ menu:

 [![](debugging-in-xamarin-ios-images/image6vs.png "W menu ustawienia punktów przerwania")](debugging-in-xamarin-ios-images/image6vs.png#lightbox)

W tym miejscu można wprowadzić warunki, w których punkt przerwania występuje

Aby uzyskać więcej informacji na temat używania warunków punktu przerwania we wcześniejszych wersjach programu Visual Studio, zapoznaj się [dokumentacji programu Visual Studio](https://docs.microsoft.com/visualstudio/debugger/using-breakpoints) w tym temacie.

-----

## <a name="navigating-through-code"></a>Nawigowanie po kodzie za

Po osiągnięciu punktu przerwania, narzędzi debugowania umożliwiają uzyskanie kontrolę nad wykonywania programu. IDE wyświetli czterech przycisków, umożliwiając uruchamianie i wykonywać krokowo kodu.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

W programie Visual Studio for Mac będzie wyglądać następująco:

 [![](debugging-in-xamarin-ios-images/image7.png "Narzędzia debugowania umożliwić deweloperowi Pełna kontrola wykonywania programu")](debugging-in-xamarin-ios-images/image7.png#lightbox)

Są to:

- **Odtwórz/Zatrzymaj** — to spowoduje rozpoczęcie i zakończenie wykonywania kodu, aż do następnego punktu przerwania.
- **Step Over** — powoduje uruchomienie następnego wiersza kodu. Następnego wiersza po wywołaniu funkcji krok ponad będzie wykonywania funkcji i będzie zatrzymano w następnym wierszu kodu _po_ funkcji.
- **Step Into** — powoduje również uruchomienie następnego wiersza kodu. Następnego wiersza po wywołaniu funkcji Step Into przestanie w pierwszym wierszu funkcji, co pozwala na kontynuowanie wiersz po wierszu debugowanie funkcji. Jeśli następnego wiersza nie jest funkcją, działają taka sama jak Step Over.
- **Step Out** — to nastąpi powrót do wiersza której wywołano bieżącej funkcji.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

W programie Visual Studio będzie wyglądać następująco:

[![](debugging-in-xamarin-ios-images/image7vs.png "Narzędzia debugowania umożliwić deweloperowi Pełna kontrola wykonywania programu")](debugging-in-xamarin-ios-images/image7vs.png#lightbox)

Są to:

- **Odtwórz/Zatrzymaj** — to spowoduje rozpoczęcie i zakończenie wykonywania kodu, aż do następnego punktu przerwania.
- **Step Over (F11)** — powoduje uruchomienie następnego wiersza kodu. Następnego wiersza po wywołaniu funkcji krok ponad będzie wykonywania funkcji i będzie zatrzymano w następnym wierszu kodu _po_ funkcji.
- **Step Into (F10)** — powoduje również uruchomienie następnego wiersza kodu. Następnego wiersza po wywołaniu funkcji Step Into przestanie w pierwszym wierszu funkcji, co pozwala na kontynuowanie wiersz po wierszu debugowanie funkcji. Jeśli następnego wiersza nie jest funkcją, działają taka sama jak Step Over.
- **Step Out (Shift + F11)** — to nastąpi powrót do wiersza której wywołano bieżącej funkcji.

Aby uzyskać więcej informacji, zobacz dokumentacją głębokość na debugowanie, zobacz [Przejdź kodu za pomocą programu Visual Studio Debugger](https://docs.microsoft.com/en-us/visualstudio/debugger/navigating-through-code-with-the-debugger).

-----

### <a name="breakpoints"></a>Punkty przerwania

Ważne jest, aby zwrócić uwagę że iOS daje aplikacji tylko kilka sekund (10) do uruchamiania i ukończyć `FinishedLaunching` metody w elemencie delegowanym aplikacji. Jeśli aplikacja nie została zakończona tej metody za 10 sekund, iOS będzie kasowanie procesu.

Oznacza to, czy jest niemożliwe, można ustawić punktów przerwania w kodzie uruchomienia programu. Jeśli chcesz debugować kodu uruchamiania, należy opóźnienie niektóre inicjowania i put, które do metody wywoływane przez czasomierz lub w innej formie metody wywołania zwrotnego, która jest wykonywana po zakończeniu FinishedLaunching.

## <a name="device-diagnostics"></a>Diagnostyka urządzenia

Jeśli występuje błąd podczas konfigurowania debugera, można włączyć diagnostyki szczegółowe, dodając "-v - v - v" do argumentów mtouch dodatkowe opcje projektu. Spowoduje to wydrukowanie szczegółowe informacje o błędzie w konsoli urządzenia.

 <a name="WiFi_Debugging" />

## <a name="wireless-debugging"></a>Bezprzewodowe debugowania

Domyślnie Xamarin.iOS jest do debugowania aplikacji na urządzeniach za pośrednictwem połączenia USB. Czasami urządzenia USB mogą być wymagane do przetestowania podłączyć/odłączyć kabel do tworzenia aplikacji obsługiwane ExternalAccessory. W takim wypadku można użyć debugowania za pośrednictwem sieci bezprzewodowej.

Więcej informacji o sieci bezprzewodowej, wdrażanie i debugowanie, można znaleźć w [Wdrażanie bezprzewodowej](~/ios/deploy-test/wireless-deployment.md) przewodnik.

<a name="Technical_Details" />

## <a name="technical-details"></a>Szczegóły techniczne

Xamarin.iOS używa nowego Mono debugera nietrwałego. W przeciwieństwie do standardowych debugera Mono, czyli program, który kontroluje oddzielnych procesach za pomocą interfejsów systemu operacyjnego do kontroli procesów odrębnych nietrwałego debuger działa dzięki użyciu Mono środowiska uruchomieniowego udostępniają funkcje debugowania za pośrednictwem protokołu danych przesyłanych w sieci.

Uruchamianie aplikacji, która ma być debugowany kontaktów debugera i debuger uruchamia działanie. W Xamarin.iOS dla programu Visual Studio Xamarin Mac Agent działa jako środek man między debugera i aplikacji (w programie Visual Studio).

Ten debuger nietrwałego wymaga współpracy schemat debugowania podczas uruchamiania na urządzeniu. Oznacza to, że z danych binarnych kompilacji podczas debugowania będzie większy, ponieważ kod jest instrumentowany zawiera dodatkowy kod w każdym punkcie sekwencji do obsługi debugowania.

<a name="Accessing_the_Console" />


## <a name="accessing-the-console"></a>Dostęp do konsoli

Dzienniki awarii i dane wyjściowe konsoli klasy zostanie wysłane do konsoli iPhone. Można uzyskać dostępu do tej konsoli z Xcode organizator "", a następnie wybrać urządzenie organizatora.

Alternatywnie, jeśli nie chcesz uruchamiać Xcode, możesz użyć firmy Apple [iPhone narzędzia konfiguracji](http://www.apple.com/support/iphone/enterprise/) bezpośredni dostęp do konsoli. Dodatkowo, że masz dostęp dzienniki konsoli z komputera z systemem Windows przypadku debugowania problem w tym polu to ustawienie.

Dla użytkowników programu Visual Studio w oknie dane wyjściowe są dostępne kilka dzienników, ale należy przełączyć się do komputerów Mac na potrzeby bardziej kompleksowy i szczegółowe dzienniki.

-----

<a name="Debugging_Mono's_Class_Libraries" />


## <a name="debugging-monos-class-libraries"></a>Debugowanie bibliotek klas w jedno

Xamarin.iOS jest dostarczany z kodu źródłowego dla bibliotek klas w jedno i służy do pojedynczego kroku z debugera aby zobaczyć, jak działają pod maską rzeczy.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Ponieważ ta funkcja wymaga większej ilości pamięci podczas debugowania, to jest domyślnie wyłączona.


Aby włączyć tę funkcję, upewnij się, **debugowania tylko kodu projektu; nie wykonywanie kodu framework** opcja jest wyłączona w obszarze _programu Visual Studio for Mac > Preferencje > debugera_ menu zgodnie z opisami poniżej:

[![](debugging-in-xamarin-ios-images/debugging6.png "Debugowanie bibliotek klas w jedno")](debugging-in-xamarin-ios-images/debugging6.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Aby debugować biblioteki klas w programie Visual Studio, należy wyłączyć **tylko mój kod** w obszarze _Debuguj > Opcje_ menu. W _debugowanie > Ogólne_ węzła, wyczyść **Włącz opcję tylko mój kod** wyboru:

[![](debugging-in-xamarin-ios-images/debugging6vs.png "Debugowanie bibliotek klas w jedno")](debugging-in-xamarin-ios-images/debugging6vs.png#lightbox)

-----

Po wykonaniu tej czynności można uruchomić aplikacji i pojedynczy krok do żadnych bibliotek klas w jedno core.


## <a name="related-links"></a>Linki pokrewne

- [Debugowanie za pomocą platformy Xamarin](/visualstudio/mac/debugging/)
- [Wizualizacje danych](/visualstudio/mac/data-visualizations/)
- [Ustaw punkt przerwania](https://developer.xamarin.com/recipes/cross-platform/ide/debugging/set_a_breakpoint/)
- [Krok przy użyciu kodu](https://developer.xamarin.com/recipes/cross-platform/ide/debugging/step_through_code/)
- [Dane wyjściowe do okna dziennika](https://developer.xamarin.com/recipes/cross-platform/ide/debugging/output_information_to_log_window/)
