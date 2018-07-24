---
title: Odinstalowywanie środowiska Xamarin
description: W tym dokumencie opisano sposób odinstalowanie platformy Xamarin na komputerze Mac i Windows. Umożliwia, aby uzyskać szczegółowe instrukcje dotyczące odinstalowywania środowiska Mono, platformy Xamarin.Android, Xamarin.iOS i innych narzędzi.
ms.prod: xamarin
ms.assetid: b83a85ec-842a-444c-8f82-c2464eda099b
author: asb3993
ms.author: amburns
ms.date: 04/08/2017
ms.openlocfilehash: 87f59e9f0c2150291a43cdfee4fe6c5dfc2058f8
ms.sourcegitcommit: 4c0093ee5d4aeb16c0e6f0c740c4796736971651
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/23/2018
ms.locfileid: "39203127"
---
# <a name="uninstalling-xamarin"></a>Odinstalowywanie środowiska Xamarin

Ten przewodnik wyjaśnia, jak usunąć Xamarin z systemem macOS lub z programu Visual Studio w Windows.

Jeśli zachodzi konieczność ponownej instalacji Xamarin przy użyciu Instalatora uniwersalne, zawsze zaleca się że komputer jest uruchamiany najpierw.

## <a name="uninstalling-xamarin-on-macos"></a>Odinstalowywanie środowiska Xamarin w systemie macOS

Ten przewodnik może służyć do odinstalowania każdego produktu oddzielnie, przechodząc do odpowiedniej sekcji. Może zostać odinstalowany całego środowiska Xamarin zestawu narzędzi, które obejmują produkty uwzględnione na liście, postępując zgodnie z całej sposób za pomocą tego przewodnika:

- [Mono](#uninstallmono)
- [Xamarin.Android](#uninstallandroid)
- [Xamarin.iOS](#uninstallios)
- [Xamarin.Mac](#uninstallmac)
- [Skoroszyty](#uninstallworkbooks)
- [Xamarin Profiler](#uninstallprofiler)
- [Installer](#uninstallinstaller)

> [!TIP]
> Udostępniliśmy [odinstalować skryptu](https://raw.githubusercontent.com/MicrosoftDocs/visualstudio-docs/master/mac/resources/uninstall-vsmac.sh) do użycia podczas usuwania Xamarin z komputera z systemem macOS. Aby uzyskać więcej informacji na temat używania skryptu, zobacz [przy użyciu skryptu odinstalowania](#uninstallscript) sekcji tego przewodnika.

### <a name="uninstalling-visual-studio-for-mac"></a>Odinstalowywanie programu Visual Studio dla komputerów Mac

Pierwszym etapem odinstalowywanie środowiska Xamarin z poziomu komputera Mac jest zlokalizowanie **Visual Studio.app** w **/Applications** katalogu i przeciągnij go do **Kosza**. Alternatywnie, kliknij prawym przyciskiem myszy i wybierz **przenieść do Kosza** jak pokazano na poniższej ilustracji:

![Przenieś Visual Studio Application do Kosza](uninstalling-xamarin-images/uninstall-image1.png)

Usunięcie tego zbioru aplikacji spowoduje usunięcie programu Visual Studio dla komputerów Mac, jednak mogą istnieć inne pliki odnoszących się do platformy Xamarin, pracując nadal na system plików.

Aby usunąć wszystkie ślady programu Visual Studio dla komputerów Mac, uruchom następujące polecenia w terminalu:

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
> Aby uzyskać informacji na temat odinstalowywania programu Visual Studio dla komputerów Mac, zobacz [Odinstaluj](https://docs.microsoft.com/visualstudio/mac/uninstall) przewodnik w witrynie docs.microsoft.com

<a name="uninstallmono" />

### <a name="uninstall-mono-sdk-mdk"></a>Odinstalowywanie narzędzia Mono zestawu SDK (MDK)

Narzędzie mono jest implementacją open source .NET Framework i jest używany przez wszystkie Xamarin Products—Xamarin.iOS, Xamarin.Android i Xamarin.Mac umożliwia tworzenie tych platform w języku C#.

> [!WARNING]
> Istnieją inne aplikacje poza programem Xamarin, które także używają platformy Mono, takich jak Unity. Pamiętaj, że żadne inne zależności na istnieją Mono przed jego odinstalowaniem.

Aby usunąć platformy Mono, uruchom następujące polecenia w terminalu:

```bash
sudo rm -rf /Library/Frameworks/Mono.framework
sudo pkgutil --forget com.xamarin.mono-MDK.pkg
sudo rm /etc/paths.d/mono-commands
```

<a name="uninstallandroid" />

### <a name="uninstall-xamarinandroid"></a>Uninstall Xamarin.Android

Istnieje kilka elementów, które są wymagane w przypadku korzystania z platformy Xamarin.Android, takich jak zestaw SDK systemu Android i zestawu SDK Java, które muszą zostać usunięte podczas odinstalowywania produktu Xamarin.Android. Ta sekcja przeprowadzi Cię przez odinstalowanie wszystkie elementy niezbędne.

Aby usunąć rozszerzenie Xamarin.Android, uruchom następujące polecenia w terminalu:

```bash
sudo rm -rf /Developer/MonoDroid
rm -rf ~/Library/MonoAndroid
sudo pkgutil --forget com.xamarin.android.pkg
sudo rm -rf /Library/Frameworks/Xamarin.Android.framework
```

#### <a name="uninstall-android-sdk-and-java-sdk"></a>Odinstalowanie zestawu SDK systemu Android SDK i środowiska Java

Zestaw SDK systemu Android jest wymagany do tworzenia aplikacji dla systemu Android. Aby całkowicie usunąć wszystkie części zestawu Android SDK, zlokalizuj plik w rozmiarze **~/Library/Developer/Xamarin/** i przenieść ją do **Kosza**.

Zestaw SDK Java (JDK) nie trzeba dezinstalację, ponieważ jest ono już wstępnie spakowane jako część systemu Mac OS X.

#### <a name="uninstall-android-avd"></a>Odinstaluj AVD systemu Android

> [!WARNING]
> Istnieją inne aplikacje w zakresie poza programem Visual Studio dla komputerów Mac, które także używają urządzeń AVD systemu Android i te dodatkowe składniki systemu android, takich jak Android Studio.
> Usunięcie tego katalogu może spowodować projektów można przerwać w programie Android Studio. 

Aby usunąć AVDs dla systemu Android, a dodatkowe składniki systemu Android, użyj następującego polecenia:

```bash
rm -rf ~/.android
```

Aby usunąć tylko AVDs dla systemu Android, użyj następującego polecenia:

```bash
rm -rf ~/.android/avd
```

<a name="uninstallios" />

### <a name="uninstall-xamarinios"></a>Odinstalowywanie rozszerzenia Xamarin.iOS

Xamarin.iOS umożliwia iOS opracowywanie aplikacji przy użyciu języka C# lub F #. Aby odinstalować rozszerzenia Xamarin.iOS na komputerze, wykonaj następujące czynności:

Aby usunąć wszystkie pliki rozszerzenia Xamarin.iOS, użyj następujących poleceń w terminalu:

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

Rozszerzenia Xamarin.Mac może zostać usunięty z Twojego komputera przy użyciu następujące dwa polecenia odpowiednio likwidacji produktu i licencji na komputerze Mac:

```bash
sudo rm -rf /Library/Frameworks/Xamarin.Mac.framework
rm -rf ~/Library/Xamarin.Mac
```

<a name="uninstallworkbooks" />

### <a name="uninstall-workbooks"></a>Odinstaluj skoroszytów

Aby usunąć Xamarin Workbooks wersji 1.2.2 i nowszym, użyj następujących poleceń w terminalu:

```bash
sudo /Library/Frameworks/Xamarin.Interactive.framework/Versions/Current/uninstall
```

We wcześniejszych wersjach, zobacz [skoroszyty](~/tools/workbooks/install.md#uninstall-macos) odinstalować przewodnik.

<a name="uninstallprofiler" />

### <a name="uninstall-the-xamarin-profiler"></a>Odinstaluj program Xamarin Profiler

Aby usunąć Profiler środowiska Xamarin, użyj następujących poleceń w terminalu:

```bash
sudo rm -rf "/Applications/Xamarin Profiler.app"
```

<a name="uninstallinstaller" />

### <a name="uninstall-the-xamarin-installer"></a>Odinstaluj Instalator platformy Xamarin

Aby usunąć wszystkie ślady Instalatora uniwersalnej platformy Xamarin, użyj następujących poleceń:

```bash
rm -rf ~/Library/Caches/XamarinInstaller/
rm -rf ~/Library/Caches/VisualStudioInstaller/
rm -rf ~/Library/Logs/XamarinInstaller/
rm -rf ~/Library/Logs/VisualStudioInstaller/
rm -rf ~/Library/Preferences/Xamarin/
rm -rf "~/Library/Preferences/Visual Studio/"
```

<a name="uninstallscript" />

### <a name="using-the-uninstall-script"></a>Przy użyciu skryptu dezinstalacji

Ten skrypt dezinstalacji, można odinstalować program Visual Studio dla komputerów Mac i powiązanych z nim składników platformy Xamarin w jednym z rzeczywistym użyciem.

Skrypt zawiera większość poleceń, które znajdują się w artykule. Istnieją dwa główne pominięć ze skryptu i nie są włączone z powodu możliwych zależności zewnętrznych:

- Odinstalowywanie środowiska Mono
- Odinstalowywanie AVD systemu Android

Aby uruchomić skrypt, wykonaj następujące czynności:

1. Kliknij prawym przyciskiem myszy na skrypt, a następnie wybierz polecenie Zapisz jako... Aby zapisać plik na komputerze Mac.

2.  Otwórz **terminalu** i zmień katalog roboczy, do którego został pobrany skrypt:

        $ cd /location/of/file

3. Wykonywalny skrypt i uruchom ją za pomocą **"sudo"**:

        $ chmod +x ./xamarin_uninstall.sh
        $ sudo ./xamarin_uninstall.sh

4. Na koniec usunąć skrypt dezinstalacji.

W tym momencie Xamarin należy odinstalować z komputera.

<a name="uninstallwindows" />

## <a name="uninstalling-xamarin-on-windows"></a>Odinstalowywanie środowiska Xamarin na Windows

Xamarin obsługiwana od następujących czynników:

- [Visual Studio 2017](#uninstallvs2017)
- [Visual Studio 2015](#uninstallvs2015)
- [Visual Studio 2013](#uninstallvs2015) [**nieobsługiwany**]
- [Program Xamarin Studio](#uninstallxamarinstudio) [**nieobsługiwany**]

<a name="uninstallvs2017" />

### <a name="visual-studio-2017"></a>Visual Studio 2017

Xamarin jest odinstalowywana z programu Visual Studio 2017 przy użyciu aplikacji Instalatora:

1. Użyj **Start menu** otworzyć **Instalatora programu Visual Studio**.

2. Naciśnij klawisz **Modyfikuj** przycisku dla tego wystąpienia, które chcesz zmienić.

    [![](uninstalling-xamarin-images/vs2017-02-sml.png "Kliknij przycisk Modyfikuj")](uninstalling-xamarin-images/vs2017-02.png#lightbox)

3. W **obciążeń** karcie, usuń zaznaczenie pola wyboru **programowanie aplikacji mobilnych przy użyciu platformy .NET** opcji (w **urządzenia przenośne i gry** sekcji).

    [![](uninstalling-xamarin-images/vs2017-03-sml.png "Usuń zaznaczenie pola wyboru obciążenie programowanie aplikacji mobilnych")](uninstalling-xamarin-images/vs2017-03.png#lightbox)

4. Kliknij przycisk **Modyfikuj** przycisk w prawym dolnym rogu okna.

5. Instalator usunie anulować wybrane składniki (musi być zamknięty programu Visual Studio 2017, aby Instalator mógł wszelkie zmiany).

    [![](uninstalling-xamarin-images/vs2017-04-sml.png "Kliknij przycisk Modyfikuj")](uninstalling-xamarin-images/vs2017-04.png#lightbox)

Poszczególne składniki platformy Xamarin (na przykład Profiler lub skoroszytów) może zostać odinstalowany, przełączając się **poszczególne składniki** kartę w kroku 3, a następnie usuwając zaznaczenie pola określone składniki:

[![](uninstalling-xamarin-images/vs2017-components-sml.png "Odinstaluj poszczególne składniki")](uninstalling-xamarin-images/vs2017-components.png#lightbox)

Całkowicie odinstalować program Visual Studio 2017, wybierz **Odinstaluj** z trzech paska menu obok **Uruchom** przycisku.

[![](uninstalling-xamarin-images/vs2017-uninstall-sml.png "Całkowicie Odinstaluj program Visual Studio")](uninstalling-xamarin-images/vs2017-uninstall.png#lightbox)

> [!IMPORTANT]
> Jeśli masz dwie (lub więcej) wystąpienia programu Visual Studio instalowane side-by-side (SxS) — takie jak wersja i wersji zapoznawczej — odinstalowanie jedno wystąpienie może usunąć niektóre funkcje platformy Xamarin z innych wystąpień programu Visual Studio, w tym:
>
> - Xamarin Profiler
> - Xamarin skoroszyty/Inspector
> - Xamarin zdalny symulator systemu iOS
> - Zestaw SDK usługi Bonjour firmy Apple
>
> W pewnych okolicznościach odinstalowaniu jednego z wystąpień SxS może spowodować niepoprawne usunięcie tych funkcji. Może to spowodować spadek wydajności platformy Xamarin na wystąpienia programu Visual Studio, które pozostaną w systemie po odinstalowaniu wystąpienia SxS.
>
>Ten problem jest rozwiązany, uruchamiając **naprawy** opcji w oknie Instalatora programu Visual Studio i ponownie zainstaluje brakujące składniki.


## <a name="uninstalling-older-and-unsupported-products"></a>Odinstalowywanie starszej i nieobsługiwanych produktów

<a name="uninstallvs2015"></a>

### <a name="visual-studio-2015-and-earlier"></a>Visual Studio 2015 i starsze

Aby całkowicie odinstalować Visual Studio 2015, użyj [odpowiedzi pomocy technicznej w witrynie visualstudio.com](https://visualstudio.microsoft.com/vs/support/vs2015/uninstall-visual-studio-2015/).

Xamarin może zostać odinstalowany z komputera Windows za pośrednictwem **Panelu sterowania**. Przejdź do **programy i funkcje** lub **programy > Odinstaluj Program** tak jak przedstawiono poniżej:

 [![](uninstalling-xamarin-images/image3.png "Przejdź do programy i funkcje lub programów Odinstaluj Program, jak pokazano tutaj")](uninstalling-xamarin-images/image3.png#lightbox) 

W Panelu sterowania odinstaluj jedną z następujących czynności, które są obecne:

- Xamarin
- Platforma Xamarin dla Windows
- Xamarin.Android
- Xamarin.iOS
- Program Xamarin dla Visual Studio

W Eksploratorze należy usunąć wszelkie pozostałe pliki z folderów rozszerzenie Xamarin Visual Studio (wszystkie wersje, w tym Program Files i pliki programów (x86)):

``` 
C:\Program Files*\Microsoft Visual Studio 1*.0\Common7\IDE\Extensions\Xamarin
```

Usuń katalog pamięci podręcznej składnik MEF programu Visual Studio, który powinien znajdować się w następującej lokalizacji:

``` 
%LOCALAPPDATA%\Microsoft\VisualStudio\1*.0\ComponentModelCache
```

Zaewidencjonuj **VirtualStore** katalogu, aby ustalić, jeśli Windows może być zapisanych dowolne nakładki pliki **Extensions\Xamarin** lub **ComponentModelCache** dostępne katalogi:

``` 
%LOCALAPPDATA%\VirtualStore
```

Otwórz Edytor rejestru (regedit) i Wyszukaj następujący klucz:

``` 
HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\SharedDlls
```

Znajdowanie i usuwanie wpisów, które pasuje do tego wzorca:

``` 
C:\Program Files*\Microsoft Visual Studio 1*.0\Common7\IDE\Extensions\Xamarin
```

Wyszukaj ten klucz:

``` 
HKEY_CURRENT_USER\Software\Microsoft\VisualStudio\1*.0\ExtensionManager\PendingDeletions
```

Usuń wszystkie wpisy, które wyglądają tak, może być powiązany środowiska xamarin. Na przykład niczego zawierający warunki `mono` lub `xamarin`.

Otwórz okno wiersza polecenia cmd.exe administratora, a następnie uruchom `devenv /setup` i `devenv /updateconfiguration` polecenia dla każdej zainstalowanej wersji programu Visual Studio. Na przykład dla programu Visual Studio 2015:

```cmd
"%ProgramFiles(x86)%\Microsoft Visual Studio 14.0\Common7\IDE\devenv.exe" /setup
"%ProgramFiles(x86)%\Microsoft Visual Studio 14.0\Common7\IDE\devenv.exe" /updateconfiguration
```

<a name="uninstallxamarinstudio"></a>

### <a name="uninstall-xamarin-studio-on-windows"></a>Odinstalowywanie programu Xamarin Studio dla Windows

Program Xamarin Studio zostanie odinstalowany z komputera Windows za pośrednictwem **Panelu sterowania**. Przejdź do **programy i funkcje** lub **programy > Odinstaluj Program** 

Aby odinstalować program Xamarin Studio, należy znaleźć **5.x.x Xamarin Studio** na liście programy i kliknij przycisk **Odinstaluj** przycisku. 

### <a name="uninstall-xamarin-studio-on-mac"></a>Odinstaluj program Xamarin Studio dla komputerów Mac

Pierwszym krokiem podczas odinstalowywania programu Xamarin Studio z poziomu komputera Mac jest zlokalizowanie **Xamarin Studio.app** w **/Applications** katalogu i przeciągnij go do **Kosza**. Alternatywnie, kliknij prawym przyciskiem myszy i wybierz **przenieść do Kosza** tak jak przedstawiono poniżej:

 [![](uninstalling-xamarin-images/image1.png "Alternatywnie kliknij prawym przyciskiem myszy i wybierz przenoszenia do Kosza, jak pokazano tutaj")](uninstalling-xamarin-images/image1.png#lightbox)

Usunięcie tego zbioru aplikacji spowoduje usunięcie programu Xamarin Studio, jednak istnieją inne pliki, odnoszące się do platformy Xamarin, pracując nadal na system plików.

Aby usunąć wszystkie ślady programu Xamarin Studio, następujące polecenia można uruchamiać w terminalu:

```bash
sudo rm -rf "/Applications/Xamarin Studio.app"
rm -rf ~/Library/Caches/XamarinStudio-*
rm -rf ~/Library/Preferences/XamarinStudio-*
rm -rf ~/Library/Logs/XamarinStudio-*
rm -rf ~/Library/XamarinStudio-*
```

## <a name="summary"></a>Podsumowanie

W tym artykule podane instrukcji na odinstalowywanie środowiska Xamarin całkowicie z poziomu komputera Mac przy użyciu polecenia w terminalu. Również dostarczane instrukcji na odinstalowywanie środowiska Xamarin na komputerze Windows za pośrednictwem **programy i funkcje** opcji (dla programu Visual Studio 2015 i starszych) i za pomocą **Instalatora programu Visual Studio** dla Program Visual Studio 2017.


## <a name="related-links"></a>Linki pokrewne

- [Odinstaluj skryptu (przykład)](https://raw.githubusercontent.com/MicrosoftDocs/visualstudio-docs/master/mac/resources/uninstall-vsmac.sh)
