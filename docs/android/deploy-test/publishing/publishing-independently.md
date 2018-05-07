---
title: Publikowanie niezależnie
ms.prod: xamarin
ms.assetid: 6FB4DEF2-01AD-C5FE-0950-CE1BF088A9C6
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/21/2017
ms.openlocfilehash: f7ba0620a4639ff62e2d75d7cf8f02fcc01faac5
ms.sourcegitcommit: c9ebf456e1c6924956bedb13f4ea78ff09f7b1a0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/04/2018
---
# <a name="publishing-independently"></a>Publikowanie niezależnie

Istnieje możliwość opublikować aplikację bez przy użyciu dowolnej z istniejących rynków systemu Android. W tej sekcji objaśnia tych innych metod publikowania i licencjonowania poziomów platformy Xamarin.Android.


## <a name="xamarin-licensing"></a>Xamarin licencjonowania

Tworzenie, wdrażanie i dystrybucja aplikacji platformy Xamarin.Android dostępne są cztery licencji:

-   **Visual Studio Community** &ndash; dla uczniów lub studentów, niewielkich zespołów i OSS deweloperów, którzy korzystają z systemu Windows.

-   **Program Visual Studio Professional** &ndash; dla indywidualnych deweloperów i niewielkich zespołów (tylko system Windows). Ta licencja oferuje standard lub subskrypcję chmury, dostęp do dodatkowej zawartości Xamarin University i żadne ograniczenia użycia.

-   **Visual Studio Enterprise** &ndash; dla zespołów o dowolnym rozmiarze (tylko system Windows). Ta licencja zawiera funkcje, enterprise, standard lub w chmurze subskrypcji.

Odwiedź stronę [visualstudio.com](https://www.visualstudio.com/xamarin/) pobierania Community Edition lub aby dowiedzieć się więcej na temat zakupu wersji Professional i Enterprise.


## <a name="allow-installation-from-unknown-sources"></a>Zezwalaj na instalacja z nieznanych źródeł

Domyślnie Android uniemożliwia pobieranie i instalowanie aplikacji z lokalizacji innych niż Google Play. Aby umożliwić instalowane ze źródeł spoza witryny marketplace, użytkownik musi włączyć *nieznane źródła* ustawienia na urządzeniu przed próbą zainstalowania aplikacji. Ustawienie to można znaleźć w obszarze **Ustawienia > Zabezpieczenia**, jak pokazano na poniższym diagramie:

[![Ekran ustawień zabezpieczeń](publishing-independently-images/settings.png)](publishing-independently-images/settings.png#lightbox)


> [!IMPORTANT]
> Niektórzy dostawcy sieci mogą uniemożliwić instalację aplikacji z nieznanych źródeł, niezależnie od tego ustawienia.



## <a name="publishing-by-e-mail"></a>Publikowanie za pośrednictwem poczty E-Mail

Dołączanie wersji APK do wiadomości e-mail jest szybki i łatwy sposób dystrybucji aplikacji dla użytkowników. Gdy użytkownik otwiera wiadomość e-mail na urządzeniu z systemem Android zasilania, Android rozpozna załącznika APK i wyświetlić **zainstalować** przycisku, jak pokazano na poniższej ilustracji:

[![Przycisk dla załącznika Zainstaluj](publishing-independently-images/publishing-via-email.png)](publishing-independently-images/publishing-via-email.png#lightbox)

Mimo że dystrybucji za pośrednictwem poczty e-mail jest proste, udostępnia kilka ochrony przed piractwo lub nieautoryzowanych dystrybucji. Najlepiej jest on zarezerwowany do sytuacji, gdy adresaci aplikacji są kilka i są one zaufane nie Dystrybuuj aplikację.


## <a name="publishing-by-web"></a>Publikowanie w sieci Web

Istnieje możliwość dystrybucji aplikacji przez serwer sieci web. Jest to osiągane przez przekazywania aplikacji do serwera sieci web, a następnie udostępnienie łącza pobierania dla użytkowników. Gdy urządzenia z systemem Android zasilania przejdzie do łącza, a następnie pobiera aplikacji, tej aplikacji zostaną zainstalowane automatycznie po ukończeniu pobierania.


## <a name="manually-installing-an-apk"></a>Ręczne instalowanie APK

Instalacja ręczna jest trzecia opcja instalowania aplikacji. W celu ręcznej instalacji aplikacji:

1.   **Dystrybucji APK użytkownikowi** &ndash; na przykład tej kopii mogą być dystrybuowane na dysku CD lub dysk flash USB.
1.   **(Użytkownik) instaluje aplikację na urządzeniu z systemem Android** &ndash; użyć wiersza polecenia *mostka debugowania Android* (**adb**) narzędzie. **ADB** jest uniwersalny narzędzie wiersza polecenia, które umożliwia komunikację z wystąpieniem emulatora albo urządzenia z systemem Android zasilania. Zawiera zestaw SDK systemu Android **adb**; znajduje się w katalogu  **<sdk>/platform-tools /**.

Android urządzenie musi być połączone za pomocą kabla USB do komputera.
Komputery z systemem Windows może być także wymagane dodatkowe sterowniki USB z dostawcą telefon, aby być rozpoznawane przez **adb**. Instrukcje dotyczące instalacji dla tych dodatkowych sterowników USB wykracza poza zakres tego dokumentu.

Przed wykonaniem dowolnej **adb** poleceń, warto wiedzieć, które wystąpienie emulatora lub urządzenia są połączone, jeśli istnieje. Można wyświetlić listę co zostało dołączone przy użyciu `devices` polecenia, jak pokazano w poniższy fragment kodu:

```shell
$ adb devices
List of devices attached
        0149B2EC03012005device
```

Po zostały potwierdzone połączonych urządzeń, aplikacji mogą być instalowane przez wystawianie `install` z **adb**:

```shell
$ adb install <path-to-apk>
```

Poniższy fragment kodu przedstawia przykład instalowanie aplikacji do podłączonego urządzenia:

```shell
$ adb install helloworld.apk
3772 KB/s (3013594 bytes in 0.780s)
        pkg: /data/local/tmp/helloworld.apk
Success
```

Jeśli aplikacja jest już zainstalowana, `adb install` nie będzie można zainstalować plik APK i będzie raportu awaria, jak pokazano w poniższym przykładzie:

```shell
$ adb install helloworld.apk
4037 KB/s (3013594 bytes in 0.728s)
        pkg: /data/local/tmp/helloworld.apk
Failure [INSTALL_FAILED_ALREADY_EXISTS]
```

Będzie konieczne odinstalowanie aplikacji z urządzenia. Najpierw należy wystawić `adb uninstall` polecenia:

```shell
adb uninstall <package_name>
```

Poniższy fragment jest przykładem odinstalowywanie aplikacji:

```shell
$ adb uninstall mono.samples.helloworld
Success
```
