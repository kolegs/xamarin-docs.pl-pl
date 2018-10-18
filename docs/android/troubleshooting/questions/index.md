---
title: Często zadawane pytania
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 0F0FDD2B-FFB1-476F-B674-81DB3A5E1CF3
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/29/2018
ms.openlocfilehash: ad3fc32245880f6603c63d33aac49309fd61b753
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/18/2018
ms.locfileid: "36935467"
---
# <a name="frequently-asked-questions"></a>Często zadawane pytania

## <a name="installation--setup"></a>Ustawienia i instalacji

### <a name="which-android-sdk-packages-should-i-installinstall-android-sdk-packagesmd"></a>[Które pakiety zestawu SDK systemu Android należy zainstalować?](install-android-sdk-packages.md)

Instalowanie zestawu Android SDK automatycznie nie uwzględnia wszystkie minimalne wymagane pakiety do programowania. Podczas indywidualni deweloperzy muszą się różnić w tym przewodniku omówiono pakiety, które zazwyczaj będą wymagane do programowania z użyciem platformy Xamarin.Android.

### <a name="where-can-i-set-my-android-sdk-locationsandroid-sdk-locationmd"></a>[Gdzie mogę ustawić moje lokalizacje zestawu SDK systemu Android?](android-sdk-location.md)

W tym przewodniku opisano ustawienia domyślne dla systemu Android SDK, która powinna działać dla większości konfiguracji; i jak zmienić te ustawienia domyślne w programie Visual Studio for Mac lub Visual Studio, jeśli to konieczne.

### <a name="how-do-i-update-the-java-development-kit-jdk-versionupdate-jdkmd"></a>[Jak zaktualizować wersję zestawu Java Development Kit (JDK)?](update-jdk.md)

W tym artykule pokazano, jak zaktualizować wersję języka Java Development Kit (JDK) w Windows i Mac.

### <a name="can-i-use-java-development-kit-jdk-version-9-or-laterjdk9-errorsmd"></a>[Czy można używać języka Java Development Kit (JDK) w wersji 9 lub nowszej?](jdk9-errors.md)

Platforma Xamarin.Android wymaga JDK 8 lub OpenJDK Mobile firmy Microsoft. W tym artykule wymieniono niektóre typowe komunikaty o błędach, może zostać wyświetlony, jeśli zestaw JDK, 9 lub nowszy jest zainstalowany, wraz z instrukcjami dla kontroli wersji zestawu JDK.


### <a name="how-can-i-manually-install-the-android-support-libraries-required-by-the-xamarinandroidsupport-packagesinstall-android-support-librarymd"></a>[Jak można ręcznie zainstalować biblioteki obsługi systemu Android wymagane przez pakiety Xamarin.Android.Support?](install-android-support-library.md)

Ten przewodnik zawiera procedury dotyczące instalowania `Xamarin.Android.Support.v4` biblioteki pomocy technicznej na Windows i dla komputerów Mac.

### <a name="what-usb-drivers-do-i-need-to-debug-android-on-windowsandroid-drivers-debug-windowsmd"></a>[Jakie sterowniki USB są potrzebne do debugowania systemu Android w systemie Windows?](android-drivers-debug-windows.md)

Aby debugować na urządzeniu z systemem Android podczas opracowywania w Windows; Musisz zainstalować zgodny sterownik USB. Menedżer zestawów SDK zawiera "Sterownik Google USB" Domyślnie, który dodaje obsługę urządzeń Nexus.
Inne urządzenia wymagają sterowniki USB opublikowane przez producenta urządzenia. Ten przewodnik zawiera informacje dotyczące znajdowania tych sterowników również jako alternatywne metody testowania.

### <a name="is-it-possible-to-connect-to-android-emulators-running-on-a-mac-from-a-windows-vmconnect-android-emulator-mac-windowsmd"></a>[Czy można nawiązać połączenie z emulatorami systemu Android działającymi na komputerze Mac z maszyny wirtualnej z systemem Windows?](connect-android-emulator-mac-windows.md)

Ten przewodnik obejmuje metody, korzystając z emulatora systemu Android.

## <a name="general-questions"></a>Pytania ogólne

### <a name="how-do-i-automate-an-android-nunit-test-projectautomate-android-nunit-testmd"></a>[Jak zautomatyzować projekt testowy Android NUnit?](automate-android-nunit-test.md)

Ten przewodnik obejmuje kroki konfigurowania projekt testowy Android NUnit, _nie_ projektu Xamarin.UITest. Xamarin.UITest znajdują się [tutaj](https://docs.microsoft.com/appcenter/test-cloud/preparing-for-upload/uitest).

### <a name="how-do-i-enable-intellisense-in-android-axml-filesenable-axml-intellisensemd"></a>[Jak włączyć funkcję Intellisense w plikach axml systemu Android?](enable-axml-intellisense.md)

Ten przewodnik opisuje sposób aktywowania funkcja Intellisense programu Visual Studio, w plikach axml systemu Android.

### <a name="why-cant-my-android-release-build-connect-to-the-internetandroid-internetmd"></a>[Dlaczego moja kompilacji wydania systemu Android nie może połączyć się z Internetem?](android-internet.md)

Najczęstszą przyczyną tego problemu jest to, że **INTERNET** uprawnienie jest automatycznie dołączany do kompilacji debugowanej, ale należy ręcznie ustawić dla kompilacji wydania. Ten przewodnik opisuje sposób włączania uprawnień w przypadku kompilacji wydania.

### <a name="smarter-xamarin-android-support-v4--v13-nuget-packagesandroid-support-v4v13-librariesmd"></a>[Inteligentniejsze pakiety NuGet Xamarin Android Support w wersjach 4/13](android-support-v4v13-libraries.md)

`Support-v4` i `Support-v13` nie mogą być używane razem w ramach tej samej aplikacji, oznacza to, są one wzajemnie się wykluczają. Jest to spowodowane `Support-v13` faktycznie zawiera wszystkie typy i implementację `Support-v4`. Jeśli spróbujesz teraz odwoływać się w tym samym projekcie, wystąpią błędy zduplikowany typ.

### <a name="how-do-i-resolve-a-pathtoolongexception-errorpath-too-long-exceptionmd"></a>[Jak rozwiązać błąd pathtoolongexception —?](path-too-long-exception.md)

W tym artykule wyjaśniono, jak rozwiązać **pathtoolongexception —** błędów, które mogą wystąpić podczas kompilowania projektu Xamarin.Android.



## <a name="deprecated"></a>przestarzałe

> [!NOTE]
> Poniższe artykuły dotyczą problemów, które zostały rozwiązane w nowszych wersjach platformy Xamarin. Jednak jeśli ten problem występuje w najnowszej wersji oprogramowania, sprawdź plik [nową usterkę](~/cross-platform/troubleshooting/questions/howto-file-bug.md) za pomocą swojej wersji pełne informacje i pełny dziennik dane wyjściowe kompilacji.

### <a name="what-version-of-xamarinandroid-added-lollipop-supportxa-lollipopmd"></a>[W której wersji platformy Xamarin.Android dodano obsługę wersji Lollipop?](xa-lollipop.md)

Ten przewodnik pierwotnie został napisany dla L dla systemu Android w wersji zapoznawczej. Platforma Xamarin.Android 4.17 dodana obsługa systemu Android w wersji zapoznawczej L & 4.20 platformy Xamarin.Android dodano Android obsługę wersji Lollipop.

### <a name="androidsupportv7appcompat---no-resource-found-that-matches-the-given-name-attr-androidactionmodesharedrawablemissing-action-mode-share-drawablemd"></a>[Android.Support.v7.AppCompat — nie znaleziono zasobów zgodnych z podaną nazwą: atrybut „android:actionModeShareDrawable”](missing-action-mode-share-drawable.md)

Ten błąd może wystąpić w starszych wersjach programu Xamarin, jeśli brakuje niektórych wymaganych pakietów zestawu Android SDK.

### <a name="adjusting-java-memory-parameters-for-the-android-designerandroid-designer-java-memorymd"></a>[Dostosowywanie parametrów pamięci języka Java do projektanta systemu Android](android-designer-java-memory.md)

Domyślne parametry pamięci, które są używane podczas uruchamiania `java` przetwarzania dla projektanta systemu Android mogą być niezgodne z niektórych konfiguracji systemu. Począwszy od programu Xamarin Studio 5.7.2.7 i platformy Xamarin dla programu Visual Studio 3.9.344 te ustawienia można dostosować w poszczególnych projektów.

### <a name="my-android-resourcedesignercs-file-will-not-updateresource-designer-wont-updatemd"></a>[Nie może zaktualizować pliku Resource.designer.cs systemu Android](resource-designer-wont-update.md)

Usterkę w Xamarin.Studio 5.1 wcześniej uszkodzone pliki .csproj, usuwając całkowicie lub częściowo kod xml w pliku csproj. To spowodowałoby istotne części Android stworzenie systemu (np. aktualizacja Resource.designer.cs systemu Android) nie powiedzie się. Począwszy od 5.1.4 stabilnej wersji na 15 lipca, ten błąd został naprawiony; ale w wielu przypadkach pliku projektu musi zostać naprawiony ręcznie, zgodnie z opisem w tym przewodniku.



