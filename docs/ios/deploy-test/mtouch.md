---
title: mtouch
ms.prod: xamarin
ms.assetid: BCA491DA-E4C1-8689-3EC9-E4C72495A798
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 933ad24a8778ffbee3a1b6089c6ebcf33d26bf84
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="mtouch"></a>mtouch


iPhone aplikacje są dostarczane jako pakiety aplikacji. Są to katalogi z rozszerzeniem `.app` zawierające kod, dane i pliki konfiguracji manifestu, który używa telefonów iPhone, aby dowiedzieć się więcej na temat aplikacji.

Proces przekształcania wykonywalny .NET do aplikacji przede wszystkim jest wymuszany przez `mtouch` polecenia narzędzia, która integruje się z wielu kroków wymaganych do Włączanie aplikacji do pakietu. Narzędzie to jest również używane do uruchamiania aplikacji w symulatorze i wdrażania oprogramowania do rzeczywistego urządzenia iPhone lub iPod Touch.


## <a name="detailed-instructions"></a>Szczegółowe instrukcje

Sprawdź naszych [mtouch(1)](http://docs.go-mono.com/?link=man%3amtouch(1)) ręczny, biorąc pod uwagę możliwe korzysta z narzędzia mtouch.

## <a name="installation"></a>Instalacja

Na komputerze Mac `mtouch` jest dołączany do platformy Xamarin.iOS. Można je znaleźć w następującym katalogu:

**/Library/Frameworks/Xamarin.iOS.framework/Versions/Current/bin**

Aby `mtouch` wygodny używania, dodaj jej katalog nadrzędny do systemu `PATH` zmiennej środowiskowej.  

Na przykład, aby to zrobić w Bash, Dodaj następujący wiersz na końcu Twojej **~/.bash_profile** pliku:

```bash
export PATH=$PATH:/Library/Frameworks/Xamarin.iOS.framework/Versions/Current/bin
```

> [!WARNING]
> Aby użyć `mtouch`, nie należy polegać na istnienie **/Developer/MonoTouch/usr/bin**, łącza symbolicznego, który wskazuje **/Library/Frameworks/Xamarin.iOS.framework/Versions/Current/bin**. Tego łącza symbolicznego istnieje tylko aby zachować zgodność z starsze MonoTouch zwalnia, które nie zostały zainstalowane w **/Library/Frameworks/...** , i może zniknąć w przyszłej wersji.

## <a name="building"></a>Kompilowanie

`mtouch` Polecenia można skompilować kodu na trzy sposoby:

-  Skompiluj do testowania symulatora.
-  Kompiluj dla wdrożenia urządzenia.
-  Wdrażanie pliku wykonywalnego do urządzenia.


### <a name="building-for-the-simulator"></a>Tworzenie symulator

Po rozpoczęciu można wypróbować tę aplikację w symulatorze, więc będzie używany będzie najbardziej typowym scenariuszem używane `mtouch -sim` skompilować kod w pakiecie symulatora. Odbywa się podobnie do następującej:

```bash
$ mtouch -sim Hello.app hello.exe
```

### <a name="building-for-the-device"></a>Tworzenie dla urządzenia

Tworzenie oprogramowania dla urządzenia zostanie utworzona przy użyciu aplikacji `mtouch -dev` opcji Ponadto należy podać nazwę certyfikatu używanego do podpisywania aplikacji. Poniżej przedstawiono, jak aplikacja jest skompilowany dla urządzenia:

```bash
$ mtouch -dev -c "iPhone Developer: Miguel de Icaza" foo.exe
```

W tym przypadku użyto "iPhone Developer: Miguel de Icaza" certyfikat do podpisania aplikacji. Ten krok jest bardzo ważne, lub urządzenie fizyczne będzie odmawiał załadowania aplikacji.

 <a name="Running_your_Application" />


## <a name="running-your-application"></a>Uruchomienie aplikacji


### <a name="launching-on-the-simulator"></a>Uruchamianie w symulatorze

Uruchamianie w symulatorze jest bardzo prosty, po utworzeniu pakietu aplikacji:

```bash
$ mtouch --sdkroot /Applications/Xcode.app -launchsim Hello.app 
```

Jeśli `--sdkroot` flaga nie jest ustawiona zostanie wartość domyślna to ścieżka wybierz xcode i spowodować następujące ostrzeżenie:

> np: ostrzeżenie MT0061: Xcode.app nr określić (przy użyciu--sdkroot) przy użyciu systemu Xcode, zgodnie z raportem "xcode — wybierz — ścieżkę drukowania": /Applications/Xcode.app/Contents/Developer 

Wiersz polecenia powyżej utworzy niektóre dane wyjściowe podobne to:

```bash
Launching application
Application launched
PID: 98460
Press enter to terminate the application
```



Zdecydowanie zaleca się również przechowywać dziennik standardowe Wyjście i błąd standardowy pliki w Twojej debugowania. Dane wyjściowe `Console.WriteLine` przechodzi do `stdout`, a dane wyjściowe z `Console.Error.WriteLine` i inne środowiska uruchomieniowego komunikaty o błędach przechodzi do `stderr`.

Aby to zrobić, użyj `--stdout` i `--stderr` flag:

```bash
../../tools/mtouch/mtouch --launchsim=Hello.app --stdout=output --stderr=error
```

Jeśli aplikacja nie powiedzie się, widać danych wyjściowych i błędów w celu zdiagnozowania problemu.


### <a name="deploying-to-a-device"></a>Wdrażanie do urządzenia

Aby wdrożyć na urządzeniu należy zainicjować obsługi administracyjnej urządzeniu zgodnie z opisem w firmy Apple [zarządzanie urządzeniami](http://developer.apple.com/library/ios/#documentation/Xcode/Conceptual/ios_development_workflow/00-About_the_iOS_Application_Development_Workflow/introduction.html) dokumentu. Gdy urządzenia zostały prawidłowo udostępnione, służy polecenie mtouch wdrażania skompilowanych "App" do urządzenia. W tym za pomocą tego polecenia:

```bash
$ mtouch —sdkroot /Applications/Xcode.app -installdev=MyApp.app
```

Jeśli `--sdkroot` flaga nie jest ustawiona zostanie wartość domyślna to ścieżka wybierz xcode i spowodować następujące ostrzeżenie:

> np: ostrzeżenie MT0061: Xcode.app nr określić (przy użyciu--sdkroot) przy użyciu systemu Xcode, zgodnie z raportem "xcode — wybierz — ścieżkę drukowania": /Applications/Xcode.app/Contents/Developer 

Te kroki są wykonywane zwykle przez program Visual Studio dla komputerów Mac.

## <a name="reference"></a>Tematy pomocy

Zobacz [mtouch(1)](http://docs.go-mono.com/?link=man%3amtouch(1)) ręczny, aby uzyskać więcej informacji na temat opcji wiersza polecenia.



## <a name="related-links"></a>Linki pokrewne

- [mtouch(1)](http://iosapi.xamarin.com/?link=man%3amtouch(1))
