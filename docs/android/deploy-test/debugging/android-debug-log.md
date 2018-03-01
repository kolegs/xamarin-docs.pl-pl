---
title: Dziennik debugowania dla systemu android
ms.topic: article
ms.prod: xamarin
ms.assetid: 01A715FE-9E9D-9B85-8A59-6568D8A09CA5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 99b9ed9e3c71766f483f7b00996137aae7a247d1
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="android-debug-log"></a>Dziennik debugowania dla systemu android

## <a name="android-debug-log-overview"></a>Omówienie dziennika debugowania dla systemu android

Jeden często deweloperzy lewy umożliwia debugowanie aplikacji jest do łączenia się `Console.WriteLine`. Jednak na platformie przenośnych, takich jak Android nie konsoli. Urządzenia z systemem android zawiera dziennik używanego podczas pisania aplikacji. Jest to czasami określane jako "logcat" z powodu polecenie wpisz ją pobrać.

## <a name="accessing-from-visual-studio"></a>Uzyskiwanie dostępu w programie Visual Studio

W programie Visual Studio 2015 i nowszych systemów Android i iOS są unified tablety dziennika.

### <a name="visual-studio-2015--2017"></a>Visual Studio 2015 & 2017

Nowe okno narzędzi urządzenia dziennika dla programu Visual Studio zawiera dzienniki dla urządzeń z systemami Android i iOS. Mogą być wyświetlane, wykonując jedną z następujących poleceń: 

-   **Widok > innych okien > dziennika urządzenia**
-   **Narzędzia > Android > dziennika urządzenia**
-   **Android narzędzi > dziennika urządzenia**

Po wyświetleniu okna narzędzia fizyczne urządzenie można wybrać w polu kombi urządzeń. Gdy urządzenie jest zaznaczona, automatycznie uruchamia można dodać wpisy dziennika z tabeli uruchomionej aplikacji. Przełączanie między urządzeniami będzie zatrzymać i uruchomić rejestrowanie urządzenia. Aby urządzeń, które mają być widoczne w polu kombi można załadować projekt systemu Android. Jeśli urządzenie nie ma combobox, sprawdzić w pierwszej kolejności, jeśli jest dostępna w rozpocząć debugowanie listy rozwijanej. 

Udostępnia tego okna narzędzia: tabeli wpisów dziennika, pole kombi wyboru urządzenia, sposób Wyczyść wpisy dziennika, pole wyszukiwania i przyciski odtwarzania/stop/wstrzymania. 


<a name="Accessing_from_the_Command_Line" />

## <a name="accessing-from-the-command-line"></a>Uzyskiwanie dostępu z poziomu wiersza polecenia

Inną opcję, aby wyświetlić dziennik debugowania jest za pomocą wiersza polecenia. Otwórz okno konsoli i przejdź do folderu narzędzi platformy zestawu SDK systemu Android (takich jak **C:\android-sdk-windows\platform-tools**). 

Jeśli tylko jedno urządzenie jest podłączone, dziennik można wyświetlić w programie:

```shell
$ adb logcat
```

Jeśli więcej niż jedno urządzenie jest podłączone, należy zidentyfikować urządzenia. Na przykład `adb -d logcat` przedstawia dziennik tylko fizyczne urządzenie podłączone, podczas gdy `adb -e logcat` przedstawia dziennik tylko uruchomionym emulatorem. 

Kolejne polecenia nie można znaleźć tylko uruchamiając **adb**.

<a name="Writing_to_the_Debug_Log" />


## <a name="writing-to-the-debug-log"></a>Zapisywanie w dzienniku debugowania

Komunikaty mogą zapisywane w dzienniku debugowania przy użyciu metod na [Android.Util.Log](https://developer.xamarin.com/api/type/Android.Util.Log/) klasy: 

```csharp
string tag = "myapp";

Log.Info (tag, "this is an info message");
Log.Warn (tag, "this is a warning message");
Log.Error (tag, "this is an error message");
```

Daje to:

```shell
I/myapp   (11103): this is an info message
W/myapp   (11103): this is a warning message
E/myapp   (11103): this is an error message
```

<a name="Interesting_Messages" />

## <a name="interesting-messages"></a>Interesujące wiadomości

Podczas odczytywania dziennika, a szczególnie, gdy wstawki dziennika do innych użytkowników (jako zapewniające pełny plik dziennika jest zbyt obszerne (1) i (2) nie można go odczytać), zapewniając *najważniejszych* wiersza się rozpoczynać linii przypominające:

```shell
I/ActivityManager(12944): Starting: Intent { act=android.intent.action.MAIN cat=[android.intent.category.LAUNCHER] flg=0x10200000 cmp=GcTest.GcTest/gctest.Activity1 } from pid 24175
```

W szczególności wiersz zgodny z wyrażeniem regularnym:

```shell
^I.*ActivityManager.*Starting: Intent
```

i zawierający nazwę pakietu aplikacji. Jest to wiersz odpowiadający uruchomienia działania i *większość* (ale nie wszystkie) z następujących komunikatów odnoszą się do aplikacji. 

W szczególności każdy komunikat zawiera identyfikator procesu (pid) proces generowania wiadomości. W powyższych `ActivityManager` komunikatu, proces `12944` generowany komunikat. Aby ustalić, który proces jest procesem debugowanej aplikacji, wyszukaj mono. MonoRuntimeProvider komunikat o błędzie: 

```shell
I/ActivityThread(  602): Pub TouchTest.TouchTest.__mono_init__: mono.MonoRuntimeProvider
```

Ten komunikat pochodzi z procesu, które zostało uruchomione, a wszystkie następujące komunikaty, które zawierają tego samego identyfikatora pid pochodzą z tego samego procesu. 
