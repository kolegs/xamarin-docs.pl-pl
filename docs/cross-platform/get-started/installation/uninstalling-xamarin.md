---
title: Odinstalowywanie platformy Xamarin
description: Ten dokument zawiera opis sposobu odinstalowania Xamarin na Mac i systemu Windows. Zapewnia, aby uzyskać szczegółowe instrukcje dotyczące odinstalowywania Mono, Xamarin.Android Xamarin.iOS i inne narzędzia.
ms.prod: xamarin
ms.assetid: b83a85ec-842a-444c-8f82-c2464eda099b
author: asb3993
ms.author: amburns
ms.date: 04/08/2017
ms.openlocfilehash: d5cf15b8ecd225fb75a3cfa0017cb84bc13cce1b
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782024"
---
# <a name="uninstalling-xamarin"></a>Odinstalowywanie platformy Xamarin

W tym przewodniku objaśniono sposób usuwania Xamarin z macOS lub Visual Studio w systemie Windows.

Jeśli jest konieczna ponowna instalacja Xamarin za pomocą Instalatora uniwersalnego, zawsze zaleca się czy komputer jest uruchamiany najpierw.

## <a name="uninstalling-xamarin-on-macos"></a>System macOS odinstalowaniem Xamarin

Ten przewodnik może służyć do odinstalowania każdego produktu pojedynczo, przechodząc do odpowiedniej sekcji. Cały Xamarin zestawu narzędzi, łącznie z wymienionych produktów, można odinstalować po całej sposób za pomocą tego przewodnika:

- [Mono](#uninstallmono)
- [Xamarin.Android](#uninstallandroid)
- [Xamarin.iOS](#uninstallios)
- [Xamarin.Mac](#uninstallmac)
- [Inspektor i skoroszytów](#uninstallworkbooks)
- [Xamarin Profiler](#uninstallprofiler)
- [Installer](#uninstallinstaller)

> [!TIP]
> Firma Microsoft umieściła [odinstalować skryptu](https://raw.githubusercontent.com/MicrosoftDocs/visualstudio-docs/master/mac/resources/uninstall-vsmac.sh) do użycia podczas usuwania z komputera macOS Xamarin. Aby uzyskać więcej informacji na temat używania skryptu, zobacz [przy użyciu skryptu odinstalować](#uninstallscript) sekcji tego przewodnika.

### <a name="uninstalling-visual-studio-for-mac"></a>Odinstalowanie programu Visual Studio dla komputerów Mac

Pierwszym krokiem podczas odinstalowywania Xamarin z Mac jest zlokalizowanie **Visual Studio.app** w **/Applications** katalogu i przeciągnij go do **Kosz**. Alternatywnie kliknij prawym przyciskiem myszy i wybierz **Przenieś do Kosza** jak pokazano na poniższej ilustracji:

![Aplikacji programu Visual Studio Przenieś do Kosza](uninstalling-xamarin-images/uninstall-image1.png)

Usunięcie tego pakietu aplikacji spowoduje usunięcie programu Visual Studio dla komputerów Mac, chociaż może być inne pliki odnoszących się do platformy Xamarin nadal w systemie plików.

Usuń wszystkie ślady programu Visual Studio for Mac, uruchom następujące polecenia w terminalu:

```bash
sudo rm -rf "/Applications/Visual Studio.app"
rm -rf ~/Library/Caches/VisualStudio
rm -rf ~/Library/Preferences/VisualStudio
rm -rf ~/Library/Preferences/Visual\ Studio
rm -rf ~/Library/Logs/VisualStudio
rm -rf ~/Library/VisualStudio
rm -rf ~/Library/Preferences/Xamarin/
rm -rf ~/Library/Developer/Xamarin
rm -rf ~/Library/Application\ Support/VisualStudio
rm -rf ~/Library/Application\ Support/VisualStudio/7.0/LocalInstall/Addins/
```

> [!NOTE]
> Aby uzyskać informacje na temat odinstalowywania programu Visual Studio dla komputerów Mac, zapoznaj się [Odinstaluj](https://docs.microsoft.com/visualstudio/mac/uninstall) przewodnik w witrynie docs.microsoft.com

<a name="uninstallmono" />

### <a name="uninstall-mono-sdk-mdk"></a>Odinstaluj zestaw SDK Mono (MDK)

Mono implementacji open source, platformy .NET i umożliwia Products—Xamarin.iOS platformy Xamarin.IOS, Xamarin.Android i Xamarin.Mac rozwój tych platform w języku C#.

> [!WARNING]
> Istnieją inne aplikacje poza Xamarin wykorzystujących Mono, takich jak Unity. Pamiętaj, że nie ma żadnych innych zależności na Mono przed jego odinstalowaniem.

Aby usunąć Mono Framework, uruchom następujące polecenia w terminalu:

```bash
sudo rm -rf /Library/Frameworks/Mono.framework
sudo pkgutil --forget com.xamarin.mono-MDK.pkg
sudo rm /etc/paths.d/mono-commands
```

<a name="uninstallandroid" />

### <a name="uninstall-xamarinandroid"></a>Uninstall Xamarin.Android

Istnieje wiele elementów, które są wymagane w przypadku korzystania z platformy Xamarin.Android, takich jak zestaw SDK systemu Android i zestawu Java SDK, które muszą zostać usunięte podczas odinstalowywania platformy Xamarin.Android. Ta sekcja przeprowadzi Cię przez odinstalowanie wszystkich niezbędnych części.

Aby usunąć Xamarin.Android, uruchom następujące polecenia w terminalu:

```bash
sudo rm -rf /Developer/MonoDroid
rm -rf ~/Library/MonoAndroid
sudo pkgutil --forget com.xamarin.android.pkg
sudo rm -rf /Library/Frameworks/Xamarin.Android.framework
```

#### <a name="uninstall-android-sdk-and-java-sdk"></a>Odinstaluj zestaw SDK systemu Android SDK i Java

Zestaw SDK systemu Android jest wymagany dla opracowywania aplikacji systemu Android. Aby całkowicie usunąć wszystkie części zestawu SDK systemu Android, zlokalizuj plik na **~/Library/Developer/Xamarin/** i przenieść ją do **Kosza**.

Zestaw SDK Java (JDK) nie musi ma zostać odinstalowany, ponieważ jest ono już wstępnie umieszczone w ramach systemu Mac OS X.

#### <a name="uninstall-android-avd"></a>Odinstaluj AVD systemu Android

> [!WARNING]
> Istnieją inne aplikacje w zakresie poza Visual Studio for Mac, które także używają Android AVD i te dodatkowe składniki systemu android, takie jak Android Studio.
> Usunięcie tego katalogu może spowodować projekty można przerwać w programie Android Studio. 

Aby usunąć wszelkie Android urządzeń Avd i dodatkowe składniki systemu Android, użyj następującego polecenia:

```bash
rm -rf ~/.android
```

Aby usunąć tylko Android urządzeń Avd, użyj następującego polecenia:

```bash
rm -rf ~/.android/avd
```

<a name="uninstallios" />

### <a name="uninstall-xamarinios"></a>Odinstalowywanie platformy Xamarin.iOS

Xamarin.iOS umożliwia iOS projektowanie aplikacji przy użyciu języka C# lub języka F #. Aby odinstalować Xamarin.iOS z komputera, wykonaj następujące czynności:

Aby usunąć wszystkie pliki Xamarin.iOS, użyj następujących poleceń w terminalu:

```bash
rm -rf ~/Library/MonoTouch
sudo rm -rf /Library/Frameworks/Xamarin.iOS.framework
sudo rm -rf /Developer/MonoTouch
sudo pkgutil --forget com.xamarin.monotouch.pkg
sudo pkgutil --forget com.xamarin.xamarin-ios-build-host.pkg
sudo pkgutil --forget com.xamarin.xamarin.ios.pkg
```

<a name="uninstallmac" />

### <a name="uninstall-xamarinmac"></a>Uninstall Xamarin.Mac

Xamarin.Mac można usunąć z komputera przy użyciu odpowiednio likwidacji produktu i licencji z komputera Mac następujące dwa polecenia:

```bash
sudo rm -rf /Library/Frameworks/Xamarin.Mac.framework
rm -rf ~/Library/Xamarin.Mac
```

<a name="uninstallworkbooks" />

### <a name="uninstall-workbooks-and-inspector"></a>Odinstaluj skoroszyty i Inspektora

Usuń wersję inspektora Xamarin i skoroszytów 1.2.2 i nowszym, użyj następujących poleceń w terminalu:

```bash
sudo /Library/Frameworks/Xamarin.Interactive.framework/Versions/Current/uninstall
```

W przypadku wcześniejszych wersji, zobacz [skoroszytów](~/tools/workbooks/install.md#uninstall-macos) odinstalować przewodnik.

<a name="uninstallprofiler" />

### <a name="uninstall-the-xamarin-profiler"></a>Odinstalowanie programu Xamarin

Aby usunąć profilera Xamarin, użyj następujących poleceń w terminalu:

```bash
sudo rm -rf "/Applications/Xamarin Profiler.app"
```

<a name="uninstallinstaller" />

### <a name="uninstall-the-xamarin-installer"></a>Odinstaluj Instalator Xamarin

Aby usunąć wszystkie ślady Instalator uniwersalnych Xamarin, użyj następujących poleceń:

```bash
rm -rf ~/Library/Caches/XamarinInstaller/
rm -rf ~/Library/Caches/VisualStudioInstaller/
rm -rf ~/Library/Logs/XamarinInstaller/
rm -rf ~/Library/Logs/VisualStudioInstaller/
rm -rf ~/Library/Preferences/Xamarin/
rm -rf "~/Library/Preferences/Visual Studio/"
```

<a name="uninstallscript" />

### <a name="using-the-uninstall-script"></a>Przy użyciu skrypt dezinstalacji skryptu

Ten skrypt dezinstalacji skryptu służy do odinstalowania programu Visual Studio for Mac i jego skojarzone składniki platformy Xamarin w jeden z rzeczywistym użyciem.

Skrypt zawiera większość potrzebnych poleceń, które znajdują się w artykule. Istnieją dwa główne pominięć skryptu i nie są uwzględniane z powodu możliwych zależności zewnętrznych:

- Odinstalowywanie Mono
- Odinstalowywanie AVD systemu Android

Aby uruchomić skrypt, wykonaj następujące czynności:

1. Kliknij prawym przyciskiem myszy na skrypt i wybierz polecenie Zapisz jako... Aby zapisać plik opartym na systemie

2.  Otwórz **Terminal** i zmień katalog roboczy, do której pobrano skrypt:

        $ cd /location/of/file

3. Wykonywalny skrypt i uruchom go z **sudo**:

        $ chmod +x ./xamarin_uninstall.sh
        $ sudo ./xamarin_uninstall.sh

4. Na koniec usunąć skrypt dezinstalacji skryptu.

W tym momencie Xamarin należy odinstalować z komputera.

<a name="uninstallwindows" />

## <a name="uninstalling-xamarin-on-windows"></a>Odinstalowywanie platformy Xamarin w systemie Windows

Xamarin obsługiwanego na następujących czynności:

- [Visual Studio 2017](#uninstallvs2017)
- [Visual Studio 2015](#uninstallvs2015)
- [Visual Studio 2013](#uninstallvs2015) [**nieobsługiwany**]
- [Program Xamarin Studio](#uninstallxamarinstudio) [**nieobsługiwany**]

<a name="uninstallvs2017" />

### <a name="visual-studio-2017"></a>Visual Studio 2017

Xamarin zostanie odinstalowana z programu Visual Studio 2017 r przy użyciu aplikacji Instalatora:

1. Użyj **Start menu** otworzyć **Instalator programu Visual Studio**.

2. Naciśnij klawisz **Modyfikuj** przycisk dla tego wystąpienia, które chcesz zmienić.

    [![](uninstalling-xamarin-images/vs2017-02-sml.png "Kliknij przycisk Modyfikuj")](uninstalling-xamarin-images/vs2017-02.png#lightbox)

3. W **obciążeń** karcie, usuń zaznaczenie pola wyboru **programowania aplikacji mobilnych z platformą .NET** opcji (w **Mobile i gier** sekcji).

    [![](uninstalling-xamarin-images/vs2017-03-sml.png "Usuń zaznaczenie pola wyboru obciążenia programowania aplikacji mobilnych")](uninstalling-xamarin-images/vs2017-03.png#lightbox)

4. Kliknij przycisk **Modyfikuj** przycisk w prawym dolnym rogu okna.

5. Instalator usunie cofnąć wybranych składników (Visual Studio 2017 muszą zostać zamknięte przed Instalatora można wprowadzać zmiany).

    [![](uninstalling-xamarin-images/vs2017-04-sml.png "Kliknij przycisk Modyfikuj")](uninstalling-xamarin-images/vs2017-04.png#lightbox)

Poszczególne składniki Xamarin (np. Profiler lub skoroszyty) można odinstalować przełączanie do **pojedynczych składników** kartę w kroku 3 i zaznaczenie pola określone składniki:

[![](uninstalling-xamarin-images/vs2017-components-sml.png "Odinstaluj pojedynczych składników")](uninstalling-xamarin-images/vs2017-components.png#lightbox)

Całkowicie odinstalować program Visual Studio 2017, wybierz **Odinstaluj** z trzech paska menu obok **uruchamianie** przycisku.

[![](uninstalling-xamarin-images/vs2017-uninstall-sml.png "Całkowicie Odinstaluj program Visual Studio")](uninstalling-xamarin-images/vs2017-uninstall.png#lightbox)

> [!IMPORTANT]
> Jeśli masz dwie (lub więcej) wystąpienia programu Visual Studio instalowane side-by-side (SxS) — takie jak zlecenia i wersja zapoznawcza — odinstalowanie jedno wystąpienie może usunąć niektóre funkcje Xamarin z innych wystąpień programu Visual Studio, w tym:
>
> - Xamarin Profiler
> - Xamarin skoroszytów/Inspektora
> - Zdalne Xamarin symulatora systemu iOS
> - Zestaw SDK Bonjour firmy Apple
>
> W niektórych warunkach jedno wystąpienie SxS odinstalowywanie może spowodować niepoprawne usunięcie tych funkcji. Może to spowodować spadek wydajności platformy Xamarin w wystąpienia programu Visual Studio, które pozostaną w systemie po odinstalowaniu programu wystąpienia SxS.
>
>Ta isresolved uruchamiając **naprawy** opcji w Instalatorze programu Visual Studio, który będzie ponownie zainstaluj brakujące składniki.


## <a name="uninstalling-older-and-unsupported-products"></a>Odinstalowywanie produktów starszych i nieobsługiwane

<a name="uninstallvs2015"></a>

### <a name="visual-studio-2015-and-earlier"></a>Visual Studio 2015 i starsze wersje

Aby całkowicie odinstalować program Visual Studio 2015, użyj [odpowiedzi pomocy technicznej w witrynie visualstudio.com](https://www.visualstudio.com/vs/support/vs2015/uninstall-visual-studio-2015/).

Xamarin mogła zostać usunięta z komputera z systemem Windows za pośrednictwem **Panelu sterowania**. Przejdź do **programy i funkcje** lub **programy > Odinstaluj Program** jak przedstawiono poniżej:

 [![](uninstalling-xamarin-images/image3.png "Przejdź do programy i funkcje lub Odinstaluj programy programu, jak pokazano w tym miejscu")](uninstalling-xamarin-images/image3.png#lightbox) 

Z poziomu Panelu sterowania odinstaluj jedną z następujących czynności, które znajdują się:

- Xamarin
- Xamarin dla systemu Windows
- Xamarin.Android
- Xamarin.iOS
- Program Xamarin dla Visual Studio

W Eksploratorze Usuń wszelkie pozostałe pliki w folderach rozszerzenia platformy Xamarin w programie Visual Studio (wszystkie wersje, w tym zarówno pliki programu oraz Program (x86)):

``` 
C:\Program Files*\Microsoft Visual Studio 1*.0\Common7\IDE\Extensions\Xamarin
```

Usuń katalog pamięci podręcznej składników MEF Visual Studio, które powinien znajdować się w następującej lokalizacji:

``` 
%LOCALAPPDATA%\Microsoft\VisualStudio\1*.0\ComponentModelCache
```

Sprawdź w **VirtualStore** katalogu, aby sprawdzić, w przypadku systemu Windows może mieć zapisany żadnego nakładki pliki **Extensions\Xamarin** lub **ComponentModelCache** katalogów:

``` 
%LOCALAPPDATA%\VirtualStore
```

Otwórz Edytor rejestru (regedit) i Wyszukaj następujący klucz:

``` 
HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\SharedDlls
```

Znajdź i Usuń wpisy, które odpowiada tego wzorca:

``` 
C:\Program Files*\Microsoft Visual Studio 1*.0\Common7\IDE\Extensions\Xamarin
```

Wyszukaj ten klucz:

``` 
HKEY_CURRENT_USER\Software\Microsoft\VisualStudio\1*.0\ExtensionManager\PendingDeletions
```

Usuwanie wpisów, które wygląda jak mogą być one związane z Xamarin. Na przykład niczego zawierająca warunki `mono` lub `xamarin`.

Otwórz wiersz polecenia administratora cmd.exe, a następnie uruchom `devenv /setup` i `devenv /updateconfiguration` polecenia dla każdej zainstalowanej wersji programu Visual Studio. Na przykład dla programu Visual Studio 2015:

```cmd
"%ProgramFiles(x86)%\Microsoft Visual Studio 14.0\Common7\IDE\devenv.exe" /setup
"%ProgramFiles(x86)%\Microsoft Visual Studio 14.0\Common7\IDE\devenv.exe" /updateconfiguration
```

<a name="uninstallxamarinstudio"></a>

### <a name="uninstall-xamarin-studio-on-windows"></a>Odinstaluj program Xamarin Studio w systemie Windows

Program Xamarin Studio zostanie odinstalowany z komputera z systemem Windows za pośrednictwem **Panelu sterowania**. Przejdź do **programy i funkcje** lub **programy > Odinstaluj Program** 

Aby odinstalować program Xamarin Studio, Znajdź **Xamarin Studio 5.x.x** na liście programy i kliknij przycisk **Odinstaluj** przycisku. 

### <a name="uninstall-xamarin-studio-on-mac"></a>Odinstaluj program Xamarin Studio dla komputerów Mac

Pierwszym krokiem podczas odinstalowywania Xamarin Studio z Mac jest zlokalizowanie **Xamarin Studio.app** w **/Applications** katalogu i przeciągnij go do **Kosz**. Alternatywnie kliknij prawym przyciskiem myszy i wybierz **Przenieś do Kosza** jak przedstawiono poniżej:

 [![](uninstalling-xamarin-images/image1.png "Alternatywnie kliknij prawym przyciskiem myszy i wybierz polecenie Przenieś do Kosza, zgodnie z opisami w tym miejscu")](uninstalling-xamarin-images/image1.png#lightbox)

Usunięcie tego pakietu aplikacji spowoduje usunięcie Xamarin Studio, istnieją inne pliki odnoszących się do platformy Xamarin nadal w systemie plików.

Aby usunąć wszystkie ślady Xamarin Studio, następujące polecenia powinna być uruchamiana w terminalu:

```bash
sudo rm -rf "/Applications/Xamarin Studio.app"
rm -rf ~/Library/Caches/XamarinStudio-*
rm -rf ~/Library/Preferences/XamarinStudio-*
rm -rf ~/Library/Logs/XamarinStudio-*
rm -rf ~/Library/XamarinStudio-*
```

## <a name="summary"></a>Podsumowanie

W tym artykule podać instrukcji na temat odinstalowywania Xamarin całkowicie z Mac przy użyciu polecenia terminalowych. Również podane instrukcji na temat odinstalowywania z komputera z systemem Windows za pomocą platformy Xamarin **programy i funkcje** opcji (dla programu Visual Studio 2015 i starszych), a za pomocą **Instalator programu Visual Studio** dla Visual Studio 2017 r.


## <a name="related-links"></a>Linki pokrewne

- [Odinstaluj skryptu (przykład)](https://raw.githubusercontent.com/MicrosoftDocs/visualstudio-docs/master/mac/resources/uninstall-vsmac.sh)
