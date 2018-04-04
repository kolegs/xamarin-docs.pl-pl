---
title: Często zadawane pytania
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 0F0FDD2B-FFB1-476F-B674-81DB3A5E1CF3
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/19/2018
ms.openlocfilehash: 2deeb4309be44c1c5f8d78980707336ac8301761
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="frequently-asked-questions"></a>Często zadawane pytania

## <a name="installation--setup"></a>Ustawienia i instalacji

### <a name="which-android-sdk-packages-should-i-installinstall-android-sdk-packagesmd"></a>[Które pakiety zestawu SDK systemu Android należy zainstalować?](install-android-sdk-packages.md)

Instalowanie zestawu SDK systemu Android automatycznie nie zawiera wszystkich minimalnych wymaganych pakietów do opracowywania. Podczas poszczególnych deweloperów musi być inna, w tym przewodniku opisano pakiety, które zazwyczaj jest wymagana w przypadku programowania z użyciem platformy Xamarin.Android.

### <a name="where-can-i-set-my-android-sdk-locationsandroid-sdk-locationmd"></a>[Gdzie mogę ustawić moje lokalizacje zestawu SDK systemu Android?](android-sdk-location.md)

W tym przewodniku opisano ustawienia domyślne SDK systemu Android, która powinna działać w przypadku większości konfiguracji; oraz sposobu zmiany tych wartości domyślnych w programie Visual Studio dla komputerów Mac lub Visual Studio, jeśli to konieczne.

### <a name="how-do-i-update-the-java-development-kit-jdk-versionupdate-jdkmd"></a>[Jak zaktualizować wersję zestawu Java Development Kit (JDK)?](update-jdk.md)

W tym artykule przedstawiono sposób zaktualizuj wersję Java Development Kit (JDK) w systemach Windows i Mac.

### <a name="can-i-use-java-development-kit-jdk-version-9jdk9-errorsmd"></a>[Czy mogę używać zestawu Java Development Kit (JDK) w wersji 9?](jdk9-errors.md)

Xamarin.Android wymaga JDK 8 zamiast JDK 9. W tym artykule wymieniono niektóre typowe komunikaty o błędach, które mogą się pojawić, jeśli JDK 9 jest zainstalowany, wraz z instrukcjami dla Sprawdzanie wersji JDK.


### <a name="how-can-i-manually-install-the-android-support-libraries-required-by-the-xamarinandroidsupport-packagesinstall-android-support-librarymd"></a>[Jak można ręcznie zainstalować biblioteki obsługi systemu Android wymagane przez pakiety Xamarin.Android.Support?](install-android-support-library.md)

Ten przewodnik zawiera przykładowe kroki dotyczące instalowania `Xamarin.Android.Support.v4` Biblioteka obsługi w systemie Windows & Mac.

### <a name="how-do-i-install-google-play-services-in-an-emulatorinstall-gpsmd"></a>[Jak zainstalować usługi Google Play w emulatorze?](install-gps.md)

Ten przewodnik prowadzi do informacji na temat instalowania usług Google Play w emulatorze systemu Android w programie Visual Studio.

### <a name="what-usb-drivers-do-i-need-to-debug-android-on-windowsandroid-drivers-debug-windowsmd"></a>[Jakie sterowniki USB są potrzebne do debugowania systemu Android w systemie Windows?](android-drivers-debug-windows.md)

Debugowanie na urządzeniu z systemem Android, wdrażając w systemie Windows. Musisz zainstalować zgodny sterownik USB. Android SDK Manager obejmuje "Sterownik USB Google" Domyślnie, który dodaje obsługę urządzeń węzła.
Inne urządzenia wymagają sterowników USB, w szczególności opublikowane przez producenta urządzenia. Ten przewodnik zawiera informacje dotyczące znajdowania tych sterowników również jako alternatywne metody prób.

### <a name="is-it-possible-to-connect-to-android-emulators-running-on-a-mac-from-a-windows-vmconnect-android-emulator-mac-windowsmd"></a>[Czy można nawiązać połączenie z emulatorami systemu Android działającymi na komputerze Mac z maszyny wirtualnej z systemem Windows?](connect-android-emulator-mac-windows.md)

W tym przewodniku przedstawiono metody, gdy przy użyciu emulatora systemu Google Android.

## <a name="general-questions"></a>Pytania ogólne

### <a name="how-do-i-automate-an-android-nunit-test-projectautomate-android-nunit-testmd"></a>[Jak zautomatyzować projekt testowy Android NUnit?](automate-android-nunit-test.md)

W tym przewodniku przedstawiono kroki konfigurowania projektu testowego Android NUnit _nie_ Xamarin.UITest projektu. Xamarin.UITest znajdują się [tutaj](https://docs.microsoft.com/appcenter/test-cloud/preparing-for-upload/uitest).

### <a name="how-do-i-enable-intellisense-in-android-axml-filesenable-axml-intellisensemd"></a>[Jak włączyć funkcję Intellisense w plikach axml systemu Android?](enable-axml-intellisense.md)

Ten przewodnik opisuje sposób aktywowania Intellisense programu Visual Studio dla systemu Android .axml plików.

### <a name="why-cant-my-android-release-build-connect-to-the-internetandroid-internetmd"></a>[Dlaczego moja kompilacji wydania systemu Android nie może połączyć się z Internetem?](android-internet.md)

Najczęstszą przyczyną tego problemu jest to, że **INTERNET** uprawnienie jest automatycznie uwzględnione w kompilacji debugowania, ale należy ręcznie ustawić dla kompilacji wydania. Ten przewodnik opisuje sposób włączania uprawnień w kompilacjach wydania.

### <a name="smarter-xamarin-android-support-v4--v13-nuget-packagesandroid-support-v4v13-librariesmd"></a>[Inteligentniejsze pakiety NuGet Xamarin Android Support w wersjach 4/13](android-support-v4v13-libraries.md)

`Support-v4` i `Support-v13` nie mogą być używane razem w tej samej aplikacji, oznacza to, że są one wykluczają się wzajemnie. Jest to spowodowane `Support-v13` zawiera wszystkie typy i stosowania `Support-v4`. Jeśli spróbuj i odwołać się zarówno w tym samym projekcie wystąpią błędy zduplikowany typ.

### <a name="how-do-i-resolve-a-pathtoolongexception-errorpath-too-long-exceptionmd"></a>[Jak usunąć błąd pathtoolongexception —?](path-too-long-exception.md)

W tym artykule opisano sposób rozwiązywania **pathtoolongexception —** błąd, który może wystąpić podczas kompilowania projektu platformy Xamarin.Android.



## <a name="deprecated"></a>Przestarzałe

> [!NOTE]
> Artykuły poniżej dotyczą problemów, które zostały rozwiązane w nowszych wersjach programu Xamarin. Jednak jeśli problem wystąpi w najnowszej wersji oprogramowania, pliku [nowych usterek](~/cross-platform/troubleshooting/questions/howto-file-bug.md) z Twojej wersji pełne informacje i pełny dziennik dane wyjściowe kompilacji.

### <a name="what-version-of-xamarinandroid-added-lollipop-supportxa-lollipopmd"></a>[W której wersji platformy Xamarin.Android dodano obsługę wersji Lollipop?](xa-lollipop.md)

Ten przewodnik został pierwotnie przeznaczone dla L systemu Android w wersji zapoznawczej. Xamarin.Android 4.17 dodano obsługę systemu Android w wersji zapoznawczej L & Xamarin.Android 4.20 dodano obsługę systemu Android interfejs typu lizak.

### <a name="androidsupportv7appcompat---no-resource-found-that-matches-the-given-name-attr-androidactionmodesharedrawablemissing-action-mode-share-drawablemd"></a>[Android.Support.v7.AppCompat — nie znaleziono zasobów zgodnych z podaną nazwą: atrybut „android:actionModeShareDrawable”](missing-action-mode-share-drawable.md)

Ten błąd może wystąpić w starszych wersjach programu Xamarin, jeśli brakuje niektórych wymaganych pacakages zestawu SDK systemu Android.

### <a name="adjusting-java-memory-parameters-for-the-android-designerandroid-designer-java-memorymd"></a>[Dostosowywanie parametrów pamięci języka Java do projektanta systemu Android](android-designer-java-memory.md)

Domyślne parametry pamięci, które są używane podczas uruchamiania `java` przetworzyć dla systemu Android projektanta mogą być niezgodne z niektóre konfiguracje systemu. Począwszy od platformy Xamarin Studio 5.7.2.7 i Xamarin dla Visual Studio 3.9.344 te ustawienia można dostosować dla projektu.

### <a name="my-android-resourcedesignercs-file-will-not-updateresource-designer-wont-updatemd"></a>[Nie może zaktualizować pliku Resource.designer.cs systemu Android](resource-designer-wont-update.md)

Błąd w wersji 5.1 Xamarin.Studio wcześniej uszkodzone pliki .csproj usuwając częściowo lub całkowicie kod xml w pliku .csproj. To spowodowałoby ważne części Android kompilacji systemu (np. aktualizowanie Android Resource.designer.cs), aby zakończyć się niepowodzeniem. Począwszy od wersji stabilnej 5.1.4 wersji na 15 lipca, ten problem został rozwiązany; Jednak w wielu przypadkach plik projektu ma zostać naprawiona ręcznie, zgodnie z opisem w tym przewodniku.



