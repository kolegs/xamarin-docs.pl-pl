---
title: Odinstalowywanie platformy Xamarin
description: "Odinstalowywanie Xamarin produktów z komputera"
ms.topic: article
ms.prod: xamarin
ms.assetid: b83a85ec-842a-444c-8f82-c2464eda099b
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 04/08/2017
ms.openlocfilehash: d2db4af7dd13611075bc2b100470b5fb3ba83118
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="uninstalling-xamarin"></a>Odinstalowywanie platformy Xamarin

> [!IMPORTANT]
> W tym artykule wyjaśniono, jak odinstalować program Xamarin Studio lub innych produktów Xamarin z komputera Mac lub Windows. Aby uzyskać informacje na temat odinstalowywania programu Visual Studio dla komputerów Mac, zapoznaj się [Odinstaluj](https://docs.microsoft.com/visualstudio/mac/uninstall) przewodnik w witrynie docs.microsoft.com

# <a name="overview"></a>Omówienie

Istnieje wiele produktów Xamarin, które umożliwiają tworzenie aplikacji wieloplatformowych, łącznie z autonomicznej aplikacji, takich jak program Xamarin Studio i rozszerzenia do innych aplikacji, takich jak Obsługa platformy Xamarin w programie Visual Studio.

W tym przewodniku opisano, jak usunąć funkcji Xamarin macOS, lub z programu Visual Studio w systemie Windows:

1.  [Odinstalowywanie program Xamarin Studio](#uninstallxamarinstudio)
  1.  [Odinstalowywanie Mono](#uninstallmono)
  1.  [Uninstalling Xamarin.Android](#uninstallandroid)
  1.  [Uninstalling Xamarin.iOS](#uninstallios)
  1.  [Uninstalling Xamarin.Mac](#uninstallmac)
  2.  [Odinstalowywanie inspektora i skoroszytów](#uninstallworkbooks)
1.  [Odinstalowywanie Xamarin z systemu Windows](#uninstallwindows)
  1.  [Odinstalowywanie platformy Xamarin w programie Visual Studio 2015 i starszych wersji](#uninstallvs2015)
  1.  [Odinstalowywanie Xamarin z programu Visual Studio 2017 r.](#uninstallvs2017)
1.  [Odinstalowanie programu Visual Studio dla komputerów Mac](#uninstallvsmac)

Jeśli ponownie zainstaluj program Xamarin, za pomocą Instalatora uniwersalnego, zawsze zaleca się czy komputer jest uruchamiany najpierw.

# <a name="uninstalling-xamarin-on-mac"></a>Odinstalowywanie platformy Xamarin dla komputerów Mac

Ten przewodnik może służyć do odinstalowania każdego produktu pojedynczo, przechodząc do odpowiedniej sekcji. Można odinstalować cały zestaw narzędzi platformy Xamarin po całej sposób za pomocą tego przewodnika.

Aby uzyskać pomoc dotyczącą przy użyciu [odinstalować skryptu](http://download.xamarin.com/developer/cross-platform/xamarin_uninstall.sh), przejść do [przy użyciu skryptu odinstalować](#uninstallscript) w dolnej części tego przewodnika.

<a name="uninstallxamarinstudio" />

## <a name="uninstall-xamarin-studio"></a>Odinstaluj program Xamarin Studio

Pierwszym krokiem podczas odinstalowywania Xamarin Studio z Mac jest zlokalizowanie **Xamarin Studio.app** w **/Applications** katalogu i przeciągnij go do **Kosz**. Alternatywnie kliknij prawym przyciskiem myszy i wybierz **Przenieś do Kosza** jak przedstawiono poniżej:

 [ ![](uninstalling-xamarin-images/image1.png "Alternatywnie kliknij prawym przyciskiem myszy i wybierz polecenie Przenieś do Kosza, zgodnie z opisami w tym miejscu")](uninstalling-xamarin-images/image1.png)

Usunięcie tego pakietu aplikacji spowoduje usunięcie Xamarin Studio, istnieją inne pliki odnoszących się do platformy Xamarin nadal w systemie plików.

Aby usunąć wszystkie ślady Xamarin Studio, następujące polecenia powinna być uruchamiana w terminalu:

```bash
sudo rm -rf "/Applications/Xamarin Studio.app"
rm -rf ~/Library/Caches/XamarinStudio-*
rm -rf ~/Library/Preferences/XamarinStudio-*
rm -rf ~/Library/Logs/XamarinStudio-*
rm -rf ~/Library/XamarinStudio-*
```

<a name="uninstallmono" />

## <a name="uninstall-mono-sdk-mdk"></a>Odinstaluj zestaw SDK Mono (MDK)

Mono jest implementacją typu open source firmy Microsoft .NET Framework i umożliwia Products—Xamarin.iOS platformy Xamarin.IOS, Xamarin.Android i Xamarin.Mac rozwój tych platform w języku C#.

> [!IMPORTANT]
> Uwaga: Istnieją inne aplikacje poza Xamarin wykorzystujących Mono, takich jak Unity. Pamiętaj, że nie ma żadnych innych zależności na Mono przed jego odinstalowaniem.

Aby usunąć Mono Framework z komputera, uruchom następujące polecenia w terminalu:

```bash
sudo rm -rf /Library/Frameworks/Mono.framework
sudo pkgutil --forget com.xamarin.mono-MDK.pkg
```

<a name="uninstallandroid" />

## <a name="uninstall-xamarinandroid"></a>Uninstall Xamarin.Android

Istnieje wiele elementów wymaganych do instalacji i korzystania z platformy Xamarin.Android, takich jak zestaw SDK systemu Android i zestawu Java SDK. Więcej informacji na temat tych wymaganych składników jest dostępna w [Instalacja ręczna](https://docs.microsoft.com/visualstudio/mac/installation/) przewodnik.

Aby usunąć Xamarin.Android, użyj następujących poleceń:

```bash
sudo rm -rf /Developer/MonoDroid
rm -rf ~/Library/MonoAndroid
sudo pkgutil --forget com.xamarin.android.pkg
sudo rm -rf /Library/Frameworks/Xamarin.Android.framework
```

### <a name="uninstall-android-sdk-and-java-sdk"></a>Odinstaluj zestaw SDK systemu Android SDK i Java

Zestaw SDK systemu Android jest wymagany dla opracowywania aplikacji systemu Android. Aby całkowicie usunąć wszystkie części zestawu SDK systemu Android, zlokalizuj plik na **~/Library/Developer/Xamarin/** i przenieść ją do **Kosza**, jak pokazano poniżej:

 [ ![](uninstalling-xamarin-images/image2.png "Aby całkowicie usunąć wszystkie części zestawu SDK systemu Android, Znajdź plik i umieści ją w Koszu, jak pokazano w tym miejscu")](uninstalling-xamarin-images/image2.png)

Zestaw SDK Java (JDK) nie musi ma zostać odinstalowany, ponieważ jest ono już wstępnie umieszczone w ramach systemu Mac OS X.

<a name="uninstallios" />

## <a name="uninstall-xamarinios"></a>Odinstalowywanie platformy Xamarin.iOS

Xamarin.iOS umożliwia iOS projektowanie aplikacji przy użyciu języka C# lub języka F # za pomocą platformy Xamarin Studio na komputerach Mac.
Hosta kompilacji Xamarin również był instalowany automatycznie z wcześniejszymi wersjami programu Xamarin.iOS, aby umożliwić programowanie dla systemu iOS w programie Visual Studio. Aby odinstalować zarówno z komputera, wykonaj następujące czynności:

Użyj następujących poleceń w terminalu, aby usunąć wszystkie pliki Xamarin.iOS z systemu plików:

```bash
rm -rf ~/Library/MonoTouch
sudo rm -rf /Library/Frameworks/Xamarin.iOS.framework
sudo rm -rf /Developer/MonoTouch
sudo pkgutil --forget com.xamarin.monotouch.pkg
sudo pkgutil --forget com.xamarin.xamarin-ios-build-host.pkg
```

### <a name="uninstall-the-mac-build-host"></a>Odinstaluj hosta kompilacji Mac

Uwaga: To mógł już zostać usunięty już wprowadzone do platformy Xamarin 4 uruchom następujące polecenie w terminalu, aby usunąć aplikację hosta kompilacji:

```bash
sudo rm -rf "/Applications/Xamarin.iOS Build Host.app"
```

Proces hosta kompilacji lub `launchd` zadanie może nadal uruchomiona lub nasłuchiwania na określonych portach.
Jego stan można sprawdzić, uruchamiając `launchctl list | grep com.xamarin.mtvs.buildserver` w terminalu.

```bash
sudo launchctl unload /Library/LaunchAgents/com.xamarin.mtvs.buildserver.plist
sudo rm -f /Library/LaunchAgents/com.xamarin.mtvs.buildserver.plist
```

<a name="uninstallmac" />

## <a name="uninstall-xamarinmac"></a>Uninstall Xamarin.Mac

Jeśli program Xamarin Studio został pomyślnie odinstalowany, Xamarin.Mac można usunąć z komputera odpowiednio likwidacji produktu i licencji z komputera Mac przy użyciu dwóch następujących poleceń:

```bash
sudo rm -rf /Library/Frameworks/Xamarin.Mac.framework
rm -rf ~/Library/Xamarin.Mac
```

<a name="uninstallworkbooks" />

## <a name="uninstall-workbooks-and-inspector"></a>Odinstaluj skoroszyty i Inspektora

Polecenie Bash usunie Xamarin Inspector i skoroszytów wersję 1.2.2 i powyżej:

```bash
sudo /Library/Frameworks/Xamarin.Interactive.framework/Versions/Current/uninstall
```

W przypadku wcześniejszych wersji, zobacz [skoroszytów](~/tools/workbooks/install.md#uninstall-macos) odinstalować przewodnik.

## <a name="uninstall-the-xamarin-installer"></a>Odinstaluj Instalator Xamarin

Aby usunąć wszystkie ślady Instalator uniwersalnych platformy Xamarin, użyj następujących poleceń:

```bash
rm -rf ~/Library/Caches/XamarinInstaller/
rm -rf ~/Library/Logs/XamarinInstaller/
rm -rf ~/Library/Preferences/Xamarin/
```

<a name="uninstallscript" />

## <a name="using-the-uninstall-script"></a>Przy użyciu skrypt dezinstalacji skryptu

[Odinstalować skryptu](http://download.xamarin.com/developer/cross-platform/xamarin_uninstall.sh) spowoduje usunięcie Xamarin z komputera. Aby użyć skrypt dezinstalacji skryptu:

1.  Pobierz skrypt dezinstalacji skryptu i zanotuj wartość w lokalizacji pobierania. Domyślnie jest to **/pobiera** katalogu.
1.  Otwórz **Terminal** i zmień katalog roboczy, do której pobrano skrypt:

        $ cd /location/of/file

1. Wykonywalny skrypt i uruchom go z **sudo**:

        $ chmod +x ./xamarin_uninstall.sh
        $ sudo ./xamarin_uninstall.sh

1. Na koniec usunąć skrypt dezinstalacji skryptu.

W tym momencie Xamarin należy odinstalować z komputera.

<a name="uninstallwindows" />

# <a name="uninstalling-xamarin-on-windows"></a>Odinstalowywanie platformy Xamarin w systemie Windows

<a name="uninstallvs2015" />

## <a name="visual-studio-2015-and-earlier"></a>Visual Studio 2015 i starsze wersje

Xamarin mogła zostać usunięta z komputera z systemem Windows za pośrednictwem **Panelu sterowania**. Przejdź do **programy i funkcje** lub **programy > Odinstaluj Program** jak przedstawiono poniżej:

 [ ![](uninstalling-xamarin-images/image3.png "Przejdź do programy i funkcje lub Odinstaluj programy programu, jak pokazano tutaj") ](uninstalling-xamarin-images/image3.png) [ ![ ] (uninstalling-xamarin-images/image4.png "przejdź do programy i funkcje lub Odinstaluj programy Program jako przedstawione tutaj")](uninstalling-xamarin-images/image4.png)

Aby odinstalować program Xamarin Studio, Znajdź **Xamarin Studio 5.x.x** na liście programy i kliknij przycisk **Odinstaluj** przycisku. Aby usunąć rozszerzenia platformy Xamarin dla Visual Studio i zestawy SDK, Znajdź **Xamarin** na liście programy i kliknij przycisk **Odinstaluj**. Zostały one przedstawione na poniższym zrzucie ekranu:

 [ ![](uninstalling-xamarin-images/image4a.png "Te zostały przedstawione w tym zrzut ekranu")](uninstalling-xamarin-images/image4a.png)

Te programy mogą zostać usunięte również całkowicie wszystkich składników Xamarin:

-  Android SDK


  [ ![](uninstalling-xamarin-images/image5.png "Te programy mogą zostać usunięte również całkowicie wszystkich składników Xamarin")](uninstalling-xamarin-images/image5.png)
-  GTK#


  [ ![](uninstalling-xamarin-images/image6.png "Te programy mogą zostać usunięte również całkowicie wszystkich składników Xamarin")](uninstalling-xamarin-images/image6.png)
-  Xamarin Universal Installer


 [ ![](uninstalling-xamarin-images/image7.png "Te programy mogą zostać usunięte również całkowicie wszystkich składników Xamarin")](uninstalling-xamarin-images/image7.png)
-  Zestaw SDK Java (należy zachować ostrożność podczas usuwania tego, jakie mogą być inne zależności na nim)


 [ ![](uninstalling-xamarin-images/image8.png "Należy zachować ostrożność, podczas usuwania zestawu Java SDK, jakie na nim mogą być inne zależności")](uninstalling-xamarin-images/image8.png)

Aby całkowicie odinstalować program Visual Studio, wykonaj [instrukcje firmy Microsoft](https://msdn.microsoft.com/library/mt720585.aspx).


<a name="uninstallvs2017" />

# <a name="visual-studio-2017"></a>Visual Studio 2017

Xamarin mogła zostać usunięta z programu Visual Studio 2017 r przy użyciu aplikacji Instalatora:

1. Użyj **Start menu** otworzyć **Instalator programu Visual Studio**.

  [ ![](uninstalling-xamarin-images/vs2017-01-sml.png "Uruchom Instalatora programu Visual Studio")](uninstalling-xamarin-images/vs2017-01.png)

1. Naciśnij klawisz **Modyfikuj** przycisk dla tego wystąpienia, które chcesz zmienić.

  [ ![](uninstalling-xamarin-images/vs2017-02-sml.png "Kliknij przycisk Modyfikuj")](uninstalling-xamarin-images/vs2017-02.png)

1. W **obciążeń** karcie, usuń zaznaczenie pola wyboru **programowania aplikacji mobilnych z platformą .NET** opcji (w **Mobile i gier** sekcji).

  [ ![](uninstalling-xamarin-images/vs2017-03-sml.png "Usuń zaznaczenie pola wyboru obciążenia programowania aplikacji mobilnych")](uninstalling-xamarin-images/vs2017-03.png)

1. Kliknij przycisk **Modyfikuj** przycisk w prawym dolnym rogu okna.
1. Instalator usunie cofnąć wybranych składników (Visual Studio 2017 muszą zostać zamknięte przed Instalatora można wprowadzać zmiany).

  [ ![](uninstalling-xamarin-images/vs2017-04-sml.png "Kliknij przycisk Modyfikuj")](uninstalling-xamarin-images/vs2017-04.png)

Poszczególne składniki Xamarin (np. Profiler lub skoroszyty) można odinstalować przełączanie do **pojedynczych składników** kartę w kroku 3 i usuwając zaznaczenie określone składniki:

[ ![](uninstalling-xamarin-images/vs2017-components-sml.png "Odinstaluj pojedynczych składników")](uninstalling-xamarin-images/vs2017-components.png)

Całkowicie odinstalować program Visual Studio 2017, wybierz **Odinstaluj** z trzech paska menu obok **uruchamianie** przycisku.

[ ![](uninstalling-xamarin-images/vs2017-uninstall-sml.png "Całkowicie Odinstaluj program Visual Studio")](uninstalling-xamarin-images/vs2017-uninstall.png)

> [!IMPORTANT]
> **Ostrzeżenie:** Jeśli masz dwie (lub więcej) wystąpienia zainstalowanego programu Visual Studio side-by-side (SxS) — takie jak zlecenia i wersja zapoznawcza — odinstalowanie jedno wystąpienie może usunąć niektóre funkcje Xamarin z innych wystąpień programu Visual Studio w tym:
>
> - Xamarin Profiler
> - Xamarin skoroszytów/Inspektora
> - Zdalne Xamarin symulatora systemu iOS
> - Zestaw SDK Bonjour firmy Apple
>
> W niektórych warunkach jedno wystąpienie SxS odinstalowywanie może spowodować niepoprawne usunięcie tych funkcji. Może to spowodować spadek wydajności platformy Xamarin w wystąpienia programu Visual Studio, które pozostaną w systemie po odinstalowaniu programu wystąpienia SxS.
>
>Ten problem można rozwiązać, uruchamiając **naprawy** opcji w Instalatorze programu Visual Studio, który będzie ponownie zainstaluj brakujące składniki.

<a name="uninstallvsmac" />

# <a name="uninstalling-visual-studio-for-mac"></a>Odinstalowanie programu Visual Studio dla komputerów Mac

Aby odinstalować program Visual Studio dla komputerów Mac, ale nadal używać Xamarin Studio, zlokalizuj **Visual Studio.app** w **/Applications** katalogu i przeciągnij go do Can. Kosza Alternatywnie kliknij prawym przyciskiem myszy i wybierz **Przenieś do Kosza** jak przedstawiono poniżej:

 [ ![](uninstalling-xamarin-images/image9.png "Kliknij prawym przyciskiem myszy ikonę programu Visual Studio i wybierz polecenie Przenieś do Kosza")](uninstalling-xamarin-images/image9.png)

Całkowicie odinstalować Xamarin z komputera, najpierw usuń Visual Studio dla komputerów Mac, a następnie wykonaj wszystkie czynności opisane w [odinstalować program Xamarin Studio](#uninstallxamarinstudio) sekcji.

# <a name="summary"></a>Podsumowanie

W tym artykule analizujemy odinstalowywania Xamarin całkowicie z Mac przy użyciu polecenia terminalowych, a także odinstalowanie Xamarin z systemu Windows maszyny za pośrednictwem **programy i funkcje** opcji (dla programu Visual Studio 2015 i wcześniej) a za pomocą **Instalator programu Visual Studio** dla programu Visual Studio 2017 r.


## <a name="related-links"></a>Linki pokrewne

- [Odinstaluj skryptu (przykład)](http://download.xamarin.com/developer/cross-platform/xamarin_uninstall.sh)
