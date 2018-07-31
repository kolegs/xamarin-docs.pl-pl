---
title: Za pomocą mtouch do pakietu aplikacji platformy Xamarin.iOS
description: W tym dokumencie opisano mtouch, narzędzie dysków wielu kroków wymagane było włączyć aplikacji platformy Xamarin.iOS w pakiecie, uruchom go w symulatorze i wdrożyć ją na urządzeniu fizycznym.
ms.prod: xamarin
ms.assetid: BCA491DA-E4C1-8689-3EC9-E4C72495A798
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/05/2017
ms.openlocfilehash: 49507b6500755c617deacfb197ef089e3debd598
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351359"
---
# <a name="using-mtouch-to-bundle-xamarinios-apps"></a>Za pomocą mtouch do pakietu aplikacji platformy Xamarin.iOS

aplikacje dla telefonu iPhone są dostarczane jako pakiety aplikacji. Są to katalogi z rozszerzeniem `.app` zawierające kod, danych, pliki konfiguracji i manifestu, który używa telefon iPhone, aby dowiedzieć się więcej na temat aplikacji.

Proces przekształcania wykonywalnej .NET do aplikacji, przede wszystkim jest wymuszany przez `mtouch` polecenia narzędzia, która integruje się z wielu kroków wymaganych do Włączanie aplikacji do pakietu. To narzędzie służy także do uruchomienia aplikacji w symulatorze i aby wdrożyć oprogramowanie do rzeczywistego urządzenia iPhone i iPod Touch.

## <a name="detailed-instructions"></a>Szczegółowe instrukcje

Sprawdź nasze [mtouch(1)](http://docs.go-mono.com/?link=man%3amtouch(1)) strony w przypadku wszystkich możliwych używa tego mtouch narzędzia.

## <a name="installation"></a>Instalacja

Na komputerze Mac `mtouch` jest zawarte w pakiecie rozszerzenia Xamarin.iOS. Można je znaleźć w następującym katalogu:

**/Library/Frameworks/Xamarin.iOS.framework/Versions/Current/bin**

Aby `mtouch` wygodne do użycia, dodaj jej katalog nadrzędny do systemu `PATH` zmiennej środowiskowej.  

Na przykład, aby to zrobić w powłoce Bash, Dodaj następujący wiersz na końcu Twojej **~/.bash_profile** pliku:

```bash
export PATH=$PATH:/Library/Frameworks/Xamarin.iOS.framework/Versions/Current/bin
```

> [!WARNING]
> Aby użyć `mtouch`, nie należy polegać na istnienie **/Developer/MonoTouch/usr/bin**, łącza symbolicznego, która wskazuje **/Library/Frameworks/Xamarin.iOS.framework/Versions/Current/bin**. Tego linku symbolicznego istnieje tylko w celu zachowania zgodności z MonoTouch starszej wersji, nie zostały zainstalowane w **/Library/Frameworks/...** , i może zniknąć w przyszłej wersji.

## <a name="building"></a>Kompilowanie

`mtouch` Polecenia można kompilować kodu na trzy różne sposoby:

-  Kompilacji do testowania symulatora.
-  Kompiluj dla wdrażania urządzenia.
-  Plik wykonywalny można wdrożyć na urządzeniu.


### <a name="building-for-the-simulator"></a>Tworzenie aplikacji dla symulatora

Gdy zaczniesz, umożliwiające wypróbowanie aplikacji w symulatorze, dzięki czemu będzie używany będzie najczęściej używany scenariusz `mtouch -sim` skompilować kod w pakiecie symulatora. Odbywa się podobnie do następującego:

```bash
$ mtouch -sim Hello.app hello.exe
```

### <a name="building-for-the-device"></a>Tworzenie aplikacji dla urządzenia

Do tworzenia oprogramowania dla urządzenia zostanie skompilowana swojej aplikacji za pomocą `mtouch -dev` opcji, ponadto należy podać nazwę certyfikatu używanego do podpisywania aplikacji. Poniżej przedstawiono, jak aplikacja jest skompilowana dla urządzenia:

```bash
$ mtouch -dev -c "iPhone Developer: Miguel de Icaza" foo.exe
```

W tym przypadku używamy "iPhone dla deweloperów: Miguel de Icaza" certyfikat do podpisania aplikacji. Ten krok jest bardzo ważne, lub urządzenia fizycznego będzie odmawiał załadowania aplikacji.

 <a name="Running_your_Application" />


## <a name="running-your-application"></a>Uruchamianie aplikacji


### <a name="launching-on-the-simulator"></a>Uruchamianie w symulatorze

Uruchamianie w symulatorze jest bardzo proste, po utworzeniu pakietu aplikacji:

```bash
$ mtouch --sdkroot /Applications/Xcode.app -launchsim Hello.app 
```

Jeśli `--sdkroot` flaga nie jest ustawiona zostanie wartość domyślna to ścieżka polecenia xcode-select i powodować następujące ostrzeżenie:

> na przykład: ostrzeżenie MT0061: Xcode.app nie określić (przy użyciu--sdkroot) przy użyciu systemu Xcode, zgłoszonej przez "xcode-select--print-path": /Applications/Xcode.app/Contents/Developer 

W wierszu polecenia powyżej dadzą pewne dane wyjściowe w następujący sposób:

```bash
Launching application
Application launched
PID: 98460
Press enter to terminate the application
```



Zdecydowanie zaleca się również zachować dziennika standardowych plików Wyjście i standardowy błąd do pomocy w debugowaniu usługi. Dane wyjściowe `Console.WriteLine` przechodzi do `stdout`oraz dane wyjściowe `Console.Error.WriteLine` i inne środowiska uruchomieniowego komunikaty o błędach przechodzi do `stderr`.

Aby to zrobić, należy użyć `--stdout` i `--stderr` flag:

```bash
../../tools/mtouch/mtouch --launchsim=Hello.app --stdout=output --stderr=error
```

Jeśli aplikacja zakończy się niepowodzeniem, spowoduje Wyjście i błąd, aby zdiagnozować problem.


### <a name="deploying-to-a-device"></a>Wdrażanie na urządzeniu

Aby wdrożyć urządzenie należy do aprowizacji urządzenia, zgodnie z opisem w firmy Apple [zarządzanie urządzeniami](http://developer.apple.com/library/ios/#documentation/Xcode/Conceptual/ios_development_workflow/00-About_the_iOS_Application_Development_Workflow/introduction.html) dokumentu. Po poprawnie aprowizowana urządzenia można użyć polecenia mtouch można wdrażać skompilowanej ".app" do urządzenia. W tym za pomocą tego polecenia:

```bash
$ mtouch —sdkroot /Applications/Xcode.app -installdev=MyApp.app
```

Jeśli `--sdkroot` flaga nie jest ustawiona zostanie wartość domyślna to ścieżka polecenia xcode-select i powodować następujące ostrzeżenie:

> na przykład: ostrzeżenie MT0061: Xcode.app nie określić (przy użyciu--sdkroot) przy użyciu systemu Xcode, zgłoszonej przez "xcode-select--print-path": /Applications/Xcode.app/Contents/Developer 

Kroki te są zazwyczaj wykonywane przez program Visual Studio dla komputerów Mac.

## <a name="reference"></a>Tematy pomocy

Zobacz [mtouch(1)](http://docs.go-mono.com/?link=man%3amtouch(1)) ręczne strony, aby uzyskać szczegółowe informacje na temat innych opcji wiersza polecenia.



## <a name="related-links"></a>Linki pokrewne

- [mtouch(1)](http://iosapi.xamarin.com/?link=man%3amtouch(1))
