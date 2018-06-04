---
title: Zmiana parametrów pamięci Java projektanta dla systemu Android
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 62FAF21C-8090-4AF3-9D88-05A4CFCAFFDC
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/02/2018
ms.openlocfilehash: 691be280b80e379863cc09d0f1bba0ff5882cf21
ms.sourcegitcommit: a7febc19102209b21e0696256c324f366faa444e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34732924"
---
# <a name="adjusting-java-memory-parameters-for-the-android-designer"></a>Zmiana parametrów pamięci Java projektanta dla systemu Android

Domyślne parametry pamięci, które są używane podczas uruchamiania `java` przetworzyć dla systemu Android projektanta mogą być niezgodne z niektóre konfiguracje systemu.

W programie Xamarin Studio 5.7.2.7 (i nowszym, w programie Visual Studio for Mac) i Visual Studio Tools dla platformy Xamarin 3.9.344, te ustawienia można dostosować dla projektu.

## <a name="new-android-designer-properties-and-corresponding-java-options"></a>Nowe właściwości projektanta dla systemu Android i odpowiednie opcje języka Java

Następujące nazwy właściwości odpowiadają wskazanych java [opcji wiersza polecenia](http://docs.oracle.com/javase/7/docs/technotes/tools/windows/java.html)

- **AndroidDesignerJavaRendererMinMemory** - Xms

- **AndroidDesignerJavaRendererMaxMemory** - Xmx

- **AndroidDesignerJavaRendererPermSize** - XX: MaxPermSize


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  Otwórz rozwiązanie w programie Visual Studio.

2.  Wybierz każdego projektu systemu Android po kolei w Eksploratorze rozwiązań i kliknij przycisk [Pokaż wszystkie pliki](https://msdn.microsoft.com/en-us/library/4afxey9h.aspx) dwukrotnie dla każdego projektu. Można pominąć projektów, które nie zawierają żadnego `.axml` plików układu. Ten krok zostanie upewnij się, że każdy katalog projektu zawiera `.csproj.user` pliku.

3.  Zamknij program Visual Studio.

4.  Zlokalizuj `.csproj.user` pliku dla poszczególnych projektów z kroku 2.

5.  Edytuj każdego `.csproj.user` plik w edytorze tekstów.

6.  Dodaj wybrane lub wszystkie nowe właściwości Android projektanta pamięci w `<PropertyGroup>` elementu. Można użyć istniejącej `<PropertyGroup>` lub Utwórz nową. Oto przykład pełną `.csproj.user` pliku, który zawiera wszystkie atrybuty 3 ustawiane na wartości domyślne:

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <Project ToolsVersion="12.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
       <PropertyGroup>
         <ProjectView>ProjectFiles</ProjectView>
       </PropertyGroup>
       <PropertyGroup>
         <AndroidDesignerJavaRendererMinMemory>128m</AndroidDesignerJavaRendererMinMemory>
         <AndroidDesignerJavaRendererMaxMemory>750m</AndroidDesignerJavaRendererMaxMemory>
         <AndroidDesignerJavaRendererPermSize>350m</AndroidDesignerJavaRendererPermSize>
       </PropertyGroup>
    </Project>
    ```

7.  Zapisz i zamknij wszystkie zaktualizowanego `.csproj.user` plików.

8.  Uruchom ponownie program Visual Studio i ponownie otwórz rozwiązanie.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1.  Otwórz rozwiązanie w programie Visual Studio dla komputerów Mac upewnić się, katalog rozwiązania zawiera `.userprefs` pliku.

2.  Zamknij program Visual Studio dla komputerów Mac.

3.  Zlokalizuj `.userprefs` pliku w katalogu rozwiązania.

4.  Edytuj `.userprefs` plik w edytorze tekstów.

5.  Zlokalizuj istniejący element XML o następującym formacie. Ostatnia część tej nazwy elementu będzie zgodna z nazwą projektu: "AndroidApplication1" w tym przykładzie:

    ```xml
    <MonoDevelop.Ide.ItemProperties.AndroidApplication1 ... >
    ```

6.  Jeśli `<MonoDevelop.Ide.ItemProperties.AndroidApplication1 ... >` element nie istnieje, utwórz go w dowolnym miejscu w otaczającej `<Properties>` elementu. Koniecznie Zastąp "AndroidApplication1" z nazwą projektu.

7.  Dodaj wybrane lub wszystkie nowe właściwości Android pamięci projektanta jako atrybuty w elemencie. Oto przykład pełną `.userprefs` pliku, który zawiera wszystkie atrybuty 3 ustawiane na wartości domyślne:

    ```xml
    <Properties StartupItem="AndroidApplication1\AndroidApplication1.csproj">
      <MonoDevelop.Ide.Workspace ActiveConfiguration="Debug" PreferredExecutionTarget="Android.SelectDevice" />
      <MonoDevelop.Ide.Workbench />
      <MonoDevelop.Ide.DebuggingService.Breakpoints>
        <BreakpointStore />
      </MonoDevelop.Ide.DebuggingService.Breakpoints>
      <MonoDevelop.Ide.DebuggingService.PinnedWatches />
      <MonoDevelop.Ide.ItemProperties.AndroidApplication1 AndroidDesignerJavaRendererMinMemory="128m" AndroidDesignerJavaRendererMaxMemory="750m" AndroidDesignerJavaRendererPermSize="350m" />
    </Properties>
    ```

8.  Powtórz kroki od 5 do 7 dla każdego projektu systemu Android w rozwiązaniu, który zawiera wszelkie `.axml` plików układu. (To znaczy, dodaj je `<MonoDevelop.Ide.ItemProperties.ProjectName>` elementu dla każdego projektu.)

9.  Zapisz i Zamknij `.userprefs` pliku.

10. Uruchom ponownie program Visual Studio dla komputerów Mac i ponownie otwórz rozwiązanie.

-----

