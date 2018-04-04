---
title: Instalowanie platformy Xamarin.Android jako aplikację systemu
description: W tym przewodniku będzie omawiać różnice między aplikacji systemu i aplikacji przez użytkownika oraz instrukcje dotyczące instalowania aplikacji platformy Xamarin.Android jako aplikacji systemowej. Ten przewodnik dotyczy autorów niestandardowych obrazów ROM systemu Android. Zostanie wyjaśnia sposób tworzenia niestandardowych pamięci ROM.
ms.prod: xamarin
ms.assetid: 0113143B-7D8D-4C4C-B2F5-B966A2E7CE1F
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: 94f2108a55cea520782aa5eac959195be09929b5
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="installing-xamarinandroid-as-a-system-app"></a>Instalowanie platformy Xamarin.Android jako aplikację systemu

_W tym przewodniku będzie omawiać różnice między aplikacji systemu i aplikacji przez użytkownika oraz instrukcje dotyczące instalowania aplikacji platformy Xamarin.Android jako aplikacji systemowej. Ten przewodnik dotyczy autorów niestandardowych obrazów ROM systemu Android. Zostanie wyjaśnia sposób tworzenia niestandardowych pamięci ROM._

## <a name="system-app"></a>Aplikacja systemu

Autorzy niestandardowych obrazów systemu Android ROM lub producentów urządzeń z systemem Android mogą mają zostać uwzględnione aplikacji platformy Xamarin.Android jako _aplikacji systemu_ do rozpowszechniania CD-ROM lub na urządzeniu. Aplikacja systemu to aplikacja, która została uznana za być istotne dla działania urządzenia, lub podaj funkcji niestandardowych pamięci ROM zawsze chce, aby być dostępna.

Aplikacje systemu są instalowane w folderze **/system/app/** (tylko do odczytu katalogu w systemie plików) i nie można usunąć lub przenieść przez użytkownika, chyba że użytkownik ma dostęp do konta root. Z kolei nosi nazwę aplikacji, która jest zainstalowana przez użytkownika (zwykle z witryny Google Play lub ładowania bezpośredniego aplikacji) _aplikacji użytkownika_. Aplikacje użytkownika może zostać usunięta przez użytkownika i w wielu przypadkach mogą zostać przeniesione do innej lokalizacji na urządzenia (na przykład konto magazynu zewnętrznego).

Aplikacje systemu zachowują się tak samo jak aplikacji użytkownika, ale ma następujące godnymi uwagi wyjątkami:

- Aplikacje systemu są możliwości uaktualnienia, podobnie jak w zwykłym _aplikacji użytkownika_. Jednak ponieważ aplikacji zawsze istnieje jego kopia w **/system/app/**, zawsze jest możliwe wycofać aplikację do wersji oryginalnej.

- Aplikacje systemu może być przyznane określone uprawnienia tylko do systemu, które nie są dostępne w aplikacji użytkownika. Na przykład z uprawnieniami tylko do systemu [ `BLUETOOTH_PRIVILEGED` ](https://developer.android.com/reference/android/Manifest.permission.html#BLUETOOTH_PRIVILEGED), który umożliwia aplikacjom sparowane urządzenia Bluetooth bez interakcji użytkownika.

Istnieje możliwość dystrybucji aplikacji platformy Xamarin.Android jako aplikacji systemowej. Oprócz zapewnienia APK do niestandardowych ROM, istnieją dwie biblioteki udostępnionej, **libmonodroid.so** i **libmonosgen 2.0.so** który należy ręcznie skopiować z APK do filesytem pamięci ROM obrazu. W tym przewodniku wyjaśniono, wymagane kroki.

## <a name="restrictions"></a>Ograniczenia

Ten przewodnik dotyczy autorów niestandardowych obrazów ROM systemu Android. Zostanie wyjaśnia sposób tworzenia niestandardowych pamięci ROM.

W tym przewodniku założono znajomość [pakowania zlecenia APK dla platformy Xamarin.Android](~/android/deploy-test/publishing/index.md) i zrozumienia [architektury Procesora](~/android/app-fundamentals/cpu-architectures.md) dla aplikacji systemu Android.

## <a name="install-a-xamarinandroid-app-as-a-system-app"></a>Instalowanie aplikacji platformy Xamarin.Android jako aplikację systemu

W poniższych krokach opisano sposób instalowania aplikacji platformy Xamarin.Android jako aplikację systemu.

1. **Pakiet zlecenia APK aplikacji platformy Xamarin.Android** &ndash; to jest opisane bardziej szczegółowo przez [publikowania aplikacji](~/android/deploy-test/publishing/index.md) przewodnik.

2. **Wyodrębnij bibliotek udostępnionych z APK** &ndash; przy użyciu dowolnego programu narzędziowego ZIP, otwarcie pliku APK i sprawdź, czy zawartość **/lib/** folderu. Ten folder będzie miał podkatalogu dla każdego _binarny interfejsu aplikacji_ (ABI) jest on obsługiwany przez aplikację; zawartość tego folderu będzie zawierać wszystkie współużytkowanych bibliotekach, które są wymagane przez aplikację w tym określonego ABI:

    ![Zrzut ekranu przedstawiający .so pliki w folderze armeabi v7a taskypro.zip](install-system-app-images/install-system-app-01.png)

   W poprzednim zrzucie ekranu jest tylko jeden z obsługiwanych ABI (**armeabi v7a**) zawierający dwa **.so** pliki, które są wymagane przez aplikację. Należy pamiętać, że tylko niezbędne wyodrębnić pliki ABI, które są odpowiednie dla architektury docelowej urządzenia pamięci ROM lub urządzenia, tj. nie należy kopiować **.so** plików ze **x86** folder  **armeabi v7a** urządzenia lub pamięci ROM.

3. **Skopiuj pliki .so do/system/lib** &ndash; kopiowania **.so** pliki, które zostały wyodrębnione APK w poprzednim kroku, aby **/system/lib/** folderu na niestandardowe pamięci ROM.

4. **Skopiuj plik APK do aplikacji/system/** &ndash; ostatnim krokiem jest skopiować plik APK **aplikacji/system/** folderu w pamięci ROM.


## <a name="summary"></a>Podsumowanie

W tym przewodniku opisano różnicę między _aplikacji systemu_ i _aplikacji użytkownika_i wyjaśniono sposób instalowania aplikacji platformy Xamarin.Android jako aplikację systemu.



## <a name="related-links"></a>Linki pokrewne

- [Publikowanie aplikacji](~/android/deploy-test/publishing/index.md)
- [Architektury procesorów](~/android/app-fundamentals/cpu-architectures.md)
- [BLUETOOTH_PRIVILEGED](https://developer.android.com/reference/android/Manifest.permission.html#BLUETOOTH_PRIVILEGED)
- [Zarządzanie ABI](https://developer.android.com/ndk~/abis.html)
