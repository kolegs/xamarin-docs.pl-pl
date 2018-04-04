---
title: Testowanie na urządzeniach czujki
description: Wdrażanie aplikacji do testowania na Twoje Apple Watch
ms.prod: xamarin
ms.assetid: A72A7D38-FAE8-4DD2-843D-54B74C5078D7
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: d1d00a4d561551435e7d2333520dc614a79dcad3
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="testing-on-watch-devices"></a>Testowanie na urządzeniach czujki

Po wykonaniu [kroki wdrażania](~/ios/watchos/deploy-test/index.md) tworzenie identyfikatorów aplikacji i grup aplikacji (jeśli jest to wymagane), postępuj zgodnie z instrukcjami na tej stronie:

- [Konfiguracja urządzenia](#devices) w Centrum deweloperów firmy Apple i
- [Tworzenie profile aprowizacji](#profiles), następnie
- [Wdrażanie i testowanie](#testing) na Apple Watch.

<a name="devices" />

## <a name="devices"></a>Urządzenia

Testowanie aplikacji systemu iOS na rzeczywistą iPhone lub iPad zawsze wymaga urządzenia, które mają być zarejestrowane w Centrum deweloperów. Lista urządzeń wygląda następująco (kliknij znak plus **+** Aby dodać nowe urządzenie):

![](device-images/devices-sml.png "Lista urządzeń wygląda następująco")

Obserwowanie są takie same — trzeba dodać własne urządzenie Apple Watch przed wdrożeniem aplikacji do niego. Znajdź przy użyciu UDID czujki **Xcode** (**Windows > urządzenia** listy). Po podłączeniu sparowanego phone czujki informacje będą również wyświetlane:

[![](device-images/xcode-devices-sml.png "Informacje o sparowanego czujki")](device-images/xcode-devices.png#lightbox)

Jeśli wiesz, obejrzyj UDID, dodaj go do listy urządzeń w Centrum deweloperów:

![](device-images/devices-watch-sml.png "Obejrzyj UDID na liście urządzeń")

Po dodaniu urządzenia czujki upewnij się, że jest zaznaczona w dowolnym programowanie nowych lub istniejących lub tworzone profile ad hoc inicjowania obsługi administracyjnej:

![](device-images/devices-provisioning.png "Lista dostępnych urządzeń")

Nie zapomnij, jeśli edycji istniejącego profilu inicjowania obsługi administracyjnej, Pobierz i zainstaluj go ponownie!

<a name="profiles" />

## <a name="development-provisioning-profiles"></a>Programowanie profile aprowizacji

Do tworzenia, testowania na urządzeniu, należy utworzyć **profilu inicjowania obsługi administracyjnej programowanie** dla każdego Identyfikatora aplikacji w rozwiązaniu.

Jeśli masz identyfikator aplikacji, symbol wieloznaczny *tylko jeden profil inicjowania obsługi będzie wymagane*; ale jeśli masz oddzielne Identyfikatora aplikacji dla każdego projektu, należy profilu inicjowania obsługi administracyjnej dla każdego Identyfikatora aplikacji:

![](device-images/provisioningprofile-development.png "Profil inicjowania obsługi administracyjnej programowanie")

Po utworzeniu wszystkich trzech profilów wyświetlone na liście. Pamiętaj, aby pobrać i zainstalować każdej z nich:

![](device-images/provisioningprofiles.png "Dostępne profile inicjowania obsługi administracyjnej programowanie")

Możesz sprawdzić, profilu inicjowania obsługi administracyjnej w **opcje projektu** wybierając **kompilacji > iOS podpisywania pakietu** ekranu i wybierając **wersji** lub **Debugowania iPhone** konfiguracji.

**Profilu inicjowania obsługi administracyjnej** listy zostaną wyświetlone wszystkie pasujących profilów — powinna być widoczna pasujących profilów utworzone na tej liście rozwijanej:

![](device-images/options-selectprofile.png "Lista profilu inicjowania obsługi administracyjnej")


<a name="testing" />

## <a name="testing-on-a-watch-device"></a>Testowanie na urządzeniu czujki

Po skonfigurowaniu tego urządzenia, identyfikatory aplikacji oraz profile inicjowania obsługi, można przystąpić do testowania.

1. Upewnij się, jest podłączone urządzenia iPhone i wyrażenie kontrolne jest już sparowana z telefonów iPhone.

2. Sprawdź konfigurację jest ustawiony na **wersji** lub **debugowania**.

3. Upewnij się, że urządzenia iPhone połączonych jest zaznaczona na liście.

4. Kliknij prawym przyciskiem myszy projekt aplikacji systemu iOS (nie zegarek lub rozszerzenia), a następnie wybierz pozycję **Ustaw jako projekt startowy**.

5. Kliknij przycisk **Uruchom** przycisk (lub wybierz **Start** opcję **Uruchom** menu).

6. Rozwiązanie zostanie utworzona i aplikacji dla systemu iOS zostanie wdrożone do urządzenia iPhone.
  Jeśli aplikacji dla systemu iOS lub Obejrzyj rozszerzenia obsługi nie jest ustawiony prawidłowo następnie wdrożenia telefonów iPhone zakończy się niepowodzeniem.

7. Jeśli wdrożenie zakończy się pomyślnie, telefonów iPhone automatycznie podejmie próbę przesyłają aplikacji czujki sparowanego czujki. Na ekranie czujki cyklicznej pojawi się ikona Twojej aplikacji *instalowanie* wskaźnik postępu.

8. Jeśli pomyślnie zainstalowano aplikację czujki, ikony pozostaną na ekranie watch - touch, aby rozpocząć testowanie aplikacji!


## <a name="troubleshooting"></a>Rozwiązywanie problemów

W przypadku wystąpienia błędu podczas wdrażania użyj **Widok > konsole > urządzenia dziennika** Aby uzyskać więcej informacji o tym błędzie. Poniżej wymieniono niektóre błędy i ich przyczyn:

### <a name="error-mt3001-could-not-aot-the-assembly"></a>Błąd MT3001: Można drzewa nie obiektów aplikacji zestawu

Może to nastąpić podczas kompilowania w trybie debugowania, aby wdrożyć urządzenie Apple Watch.

Aby *tymczasowo* obejść ten problem, wyłącz **kompilacje przyrostowe** w rozszerzeniu czujki **opcje projektu > kompilacji > watchOS kompilacji** okno:

[![](device-images/disable-incremental-sml.png "Pole wyboru kompilacje przyrostowe")](device-images/disable-incremental.png#lightbox)

Ten problem zostanie rozwiązany w przyszłej wersji, po którym kompilacji przyrostowej można włączyć ponownie przeprowadzać szybsze kompilacji.


### <a name="watch-app-fails-to-start-while-debugging-on-device"></a>Obejrzyj aplikacji nie powiedzie się podczas debugowania na urządzeniu

Podczas próby debugowania aplikacji czujki, na urządzeniu fizycznym, tylko ikona & pokrętła ładowania znajdują się (i ostatecznie limitu czasu). Ten problem zostanie rozwiązany w przyszłej wersji; obejście tego problemu jest uruchomienie kompilacji wydania (który nie zezwala na debugowanie).


### <a name="invalid-application-executable-or-application-verification-failed"></a>Plik wykonywalny aplikacji nieprawidłowy lub nie można zweryfikować aplikacji

```csharp
Failed to install [APPNAME]
Invalid executable/Application Verification Failed
```

![](device-images/invalid-application-executable.png "Nieprawidłowy plik wykonywalny aplikacji alertu")

Jeśli pojawi się *na ekranie czujki* po aplikacja wykonała próbę instalacji, może istnieć kilka problemów:

- Obejrzyj urządzeniu nie została dodana jako urządzenie w Centrum deweloperów firmy Apple. Postępuj zgodnie z instrukcjami, aby [poprawnie skonfigurować urządzenia](#devices).

- Programowanie profilów aprowizacji używane do testowania nie miał urządzenia czujki uwzględnione; lub po czujki został dodany do inicjowania obsługi administracyjnej profilów nie zostały ponownie pobrane i zainstalowane ponownie. Postępuj zgodnie z instrukcjami, aby [poprawnie skonfigurować profile inicjowania obsługi administracyjnej](#profiles).

- Jeśli **dziennika urządzenia iOS** zawiera `The system version is lower than the minimum OS version specified for bundle...Have 8.2; need 8.3` następnie czujki aplikacji **Info.plist** nieprawidłowa **MinimumOSVersion** wartości.
  To pole powinno być **8.2** — Jeśli zainstalowano Xcode 6.3, może być konieczne ręczne, Edytuj źródło wstawiania wartości 8.2.

- Aplikacja czujki **Entitlements.plist** niepoprawnie uprawnienia włączył (np. grupy aplikacji) czy nie powinien zawierać.

- Aplikacja czujki **identyfikator aplikacji** niepoprawnie ma uprawnienie włączone (np. grupy aplikacji) w Centrum deweloperów, który nie powinien on zawierać.



### <a name="install-never-finished"></a>Nie zakończono instalację

```csharp
SPErrorGizmoInstallNeverFinishedErrorMessage
```

Ten błąd może wskazywać klucze niepotrzebnych (i nieprawidłowy) w aplikacji czujki **Info.plist** pliku. Nie powinno być przeznaczone dla systemu iOS aplikacji lub Obejrzyj rozszerzenia w aplikacji czujki kluczy.

<!--eg. NSLocationAlwaysUsageDescription -->


### <a name="waiting-for-debugger-to-connect"></a>"Oczekiwanie debugera połączyć"

Jeśli **danych wyjściowych aplikacji** okna zablokowała przedstawiający

```csharp
waiting for debugger to connect
```

Sprawdź wszelkie NuGets, które zostały uwzględnione w projekcie ma zależności **Microsoft.Bcl.Build**. To jest automatycznie dodawany z niektórych firma Microsoft opublikowała bibliotek, łącznie z popularnych [bibliotek klienta Http Microsoft](http://www.nuget.org/packages/Microsoft.Net.Http/).

**Microsoft.Bcl.Build.targets** pliku, który jest dodawany do **.csproj** może zakłócać opakowanie rozszerzeń systemu iOS podczas wdrażania. Można śledzić [usterki](https://bugzilla.xamarin.com/show_bug.cgi?id=29912).
Możliwym obejściem jest do edytowania pliku .csproj i ręcznie przenieść **Microsoft.Bcl.Build.targets** za ostatnim elementem.

