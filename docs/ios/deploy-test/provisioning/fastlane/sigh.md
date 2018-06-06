---
title: sigh fastlane dla systemu iOS —
description: Opis tego dokumentu przez Odnów przez fastlane sigh polecenia, które służy do tworzenia i naprawy profile inicjowania obsługi administracyjnej dla platformy Xamarin.iOS wszystkie konfiguracje kompilacji.
ms.prod: xamarin
ms.assetid: CD17276F-2C8C-4A46-A54C-DD532EBD5720
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 8eedc86807035887cade48c42868649b362b7cb2
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34785505"
---
# <a name="fastlane-for-ios--sigh"></a>sigh fastlane dla systemu iOS —

> [!IMPORTANT]
> fastlane zaleca użycie [ `match` ](~/ios/deploy-test/provisioning/fastlane/match.md) generowania i utrzymywania profile inicjowania obsługi administracyjnej. Użyj sigh bezpośrednio tylko wtedy, gdy mają pełną kontrolę i zapoznaniu się o podpisywania kodu.

## <a name="overview"></a>Omówienie

Tradycyjnie Inicjowanie obsługi administracyjnej urządzeń jest wykonywane przez każdego członka zespołu deweloperów, za pomocą środowiska Xcode lub w portalu dla deweloperów firmy Apple. Składa się z kilku kroków:

- Żądanie certyfikatu deweloperskiego
- Dodawanie urządzenia do portalu
- Tworzenie Identyfikatora aplikacji
- Tworzenie profilu inicjowania obsługi administracyjnej
- Pobieranie profilów i certyfikatów

Każdy z tych kroków zawiera zmiennych, które należy rozważyć, które są zależne od typu aplikacji, które tworzysz. Więcej informacji na temat czynności wymagane do konfigurowania urządzenia dla rozwoju ręcznie lub za pomocą środowiska Xcode można znaleźć w [Inicjowanie obsługi administracyjnej urządzeń](~/ios/get-started/installation/device-provisioning/index.md) przewodnik.

W tym przewodniku wprowadzono narzędzia fastlane zamiast za pomocą środowiska Xcode i opisano następujące czynności:

- [Co to jest sigh?](#whatissigh)
- [Tworzenie Identyfikatora aplikacji](#appid)
- [Dodawanie nowych urządzeń](#newdevices)
- [Przy użyciu sigh](#using)
- [Dodatkowe opcje](#options)

> [!NOTE]
> Jeśli istnieje istniejący identyfikator aplikacji odpowiadający identyfikator pakietu aplikacji, a istnieje urządzenia w portalu dla deweloperów, możesz zignorować kroki na [tworzenie identyfikator aplikacji](#appid) i [Dodawanie urządzenia](#newdevices). W takim przypadku przejść bezpośrednio do [Using sigh](#using) rozpocząć pracę.

## <a name="installation"></a>Instalacja

Aby uzyskać informacje na temat instalowania fastlane, zapoznaj się wprowadzenie do [fastlane](~/ios/deploy-test/provisioning/fastlane/index.md#Installation) przewodnik.

<a name="whatissigh" />

## <a name="what-is-sigh"></a>Co to jest sigh

sigh zapewnia terminali interfejs, który służy do tworzenia i odnawiania profile inicjowania obsługi administracyjnej dla wszystkich konfiguracji: Programowanie, dystrybucja aplikacji sklepu, Ad Hoc dystrybucji i dystrybucji przedsiębiorstwa. Jego dodawania umożliwia łatwe pobieranie i napraw profile aprowizacji.

<a name="appid" />

## <a name="creating-an-app-id"></a>Tworzenie Identyfikatora aplikacji

Identyfikator aplikacji można utworzyć przy użyciu następującego polecenia:

    fastlane produce -u your@appleid.com -a com.company.appname --skip_itc

Gdzie `com.company.appname` jest identyfikator pakietu aplikacji, która znajduje się w pliku Info.plist aplikacji platformy Xamarin.iOS, jak przedstawiono poniżej:

[![](sigh-images/fastlane-image5.png "Pliku Info.plist aplikacji platformy Xamarin.iOS")](sigh-images/fastlane-image5.png#lightbox)

Unikatowy identyfikator aplikacji musi być ciągiem styl wstecznego DNS. Po jej utworzeniu, Zachowaj Uwaga, jak należy użyć, jeśli przy użyciu sigh w dalszej części tego przewodnika.

Jeśli aplikacja potrzebuje do utworzenia na [iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md), Usuń `--skip_itc` flagi polecenia powyżej.

<a name="newdevices" />

## <a name="adding-new-devices"></a>Dodawanie nowych urządzeń

Aby dodać pojedynczego urządzenia do portalu dla deweloperów w wierszu polecenia, wpisz następujące polecenie:

```bash
fastlane run register_device name:"Adam iPhone" udid:"abcdeg1234567"
```

Aby dodać więcej niż jednego urządzenia należy użyć `register_devices` polecenia:

```bash
    register_devices(
        devices: {
            "iPhone 6" => "1234567890123456789012345678901234567890",
            "iPad Air 2" => "abcdefghijklmnopqrstvuwxyzabcdefghijklmn"
         }
    )
```

<a name="using" />

## <a name="using-sigh"></a>Przy użyciu sigh

Aby rozpocząć korzystanie z narzędzia sigh, wprowadź następujące polecenie w terminalu:

```bash
fastlane sigh
```

Domyślnie spowoduje to utworzenie [dystrybucji sklepu z aplikacjami](~/ios/deploy-test/app-distribution/app-store-distribution/index.md) profil inicjowania obsługi administracyjnej. Aby skonfigurować urządzenie do tworzenia aplikacji, należy przekazać `--development` flagi:

```bash
fastlane sigh --development
```

Wprowadź identyfikator Apple ID nazwy użytkownika po wyświetleniu monitu przez fastlane. Możesz również zostać poproszony o podanie hasła Jeśli po raz pierwszy używasz fastlane. Jeśli nie będzie pobierać zmiennej środowiskowej haseł w łańcuchu kluczy.

Jeśli identyfikator Apple ID jest podłączona do wielu zespołów będą wyświetlane w tym miejscu. Wybierz numer, który odpowiada zespół, który chcesz użyć:

[![](sigh-images/fastlane-image2.png "Wybierz zespół, który chcesz użyć")](sigh-images/fastlane-image2.png#lightbox)

Identyfikator zespołu mogą zostać przekazane do interfejsu wiersza polecenia w następujący sposób:

```bash
fastlane sigh -l 2TU993NY9J
```

Wprowadź [identyfikator aplikacji](#appid)) aplikacji. Należy pamiętać, że identyfikator pakietu w pliku Info.plist aplikacji powinna odpowiadać.

Wszystkie urządzenia podłączone do konta zostanie dodany do profilu inicjowania obsługi administracyjnej.

fastlane zostanie następnie utworzyć, Pobierz i zainstaluj profil inicjowania obsługi administracyjnej dla Ciebie.

Podczas przeglądania Centrum deweloperów, można wyświetlić nowo utworzony profil inicjowania obsługi administracyjnej, jak przedstawiono poniżej:

[![](sigh-images/fastlane-image10.png "Widok nowo utworzony profil inicjowania obsługi administracyjnej")](sigh-images/fastlane-image10.png#lightbox)

Domyślnie sigh będzie przechowywać profilów aprowizacji w bieżącym folderze. Aby zmienić katalog wyjściowy `output_path`, lub wykonaj następujące czynności:

```bash
fastlane sigh -o "~/Library/MobileDevice/Provisioning Profiles"
```

<a name="options" />

## <a name="sigh-additional-options"></a>Dodatkowe opcje sigh

Następujące opcje można zapewnić dodatkową pomoc przy użyciu sigh:

- Aby pobrać wszystkie profile inicjowania obsługi administracyjnej używać:

    ```bash
    fastlane sigh download_all
    ```

- Aby użyć określonej tożsamości podpisywania do wykorzystania w profilu inicjowania obsługi administracyjnej:

    ```bash
    fastlane sigh -c "Amy cert"
    ```
    
    Gdzie `Amy cert` jest nazwą tożsamość podpisywania kodu.


## <a name="related-links"></a>Linki pokrewne

- [fastlane - sigh](https://github.com/fastlane/fastlane/tree/master/sigh#readme)
