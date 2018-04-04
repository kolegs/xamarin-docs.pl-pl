---
title: Znajdowanie podpisu Twojego magazynu kluczy
ms.prod: xamarin
ms.assetid: 1b511fec-e6f6-453e-89c8-810aafb02b77
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 46b43e6689f751c4fac1e8668234fce7f953521e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="finding-your-keystores-signature"></a>Znajdowanie podpisu Twojego magazynu kluczy

Zależy od MD5 lub SHA1 podpis aplikacji platformy Xamarin.Android **magazynu kluczy** pliku, który został użyty do podpisania plik APK. Zazwyczaj kompilację debugowania będzie używał innego **magazynu kluczy** pliku niż kompilacji wydania.

## <a name="for-debug--non-custom-signed-builds"></a>Dla debugowania / z systemem innym niż niestandardowe podpisany kompilacji

Xamarin.Android podpisuje wszystkich kompilacjach do debugowania z takimi samymi **debug.keystore** pliku. Ten plik został wygenerowany, jeśli najpierw zainstalowano platformy Xamarin.Android. Poniższe kroki szczegółowo opisano proces znajdowania MD5 lub SHA1 podpis domyślny Xamarin.Android **debug.keystore** pliku.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Zlokalizuj Xamarin **debug.keystore** pliku, który został użyty do podpisania aplikacji. Domyślnie magazynu kluczy, który jest używany do podpisywania wersje do debugowania aplikacji platformy Xamarin.Android można znaleźć w następującej lokalizacji:

**C:\\użytkowników\\*USERNAME*\\AppData\\lokalnego\\Xamarin\\Mono dla systemu Android\\debug.keystore**

Informacje o magazynie kluczy są uzyskiwane przez uruchomienie `keytool.exe` polecenie zestaw JDK. To narzędzie jest zwykle znajdują się w następującej lokalizacji:

**C:\\Program Files (x86)\\Java\\jdk*wersji*\\bin\\keytool.exe**

Dodaj katalog zawierający **keytool.exe** do `PATH` zmiennej środowiskowej.
Otwórz **wiersza polecenia** i uruchom `keytool.exe` za pomocą następującego polecenia:

```cmd
keytool.exe -list -v -keystore "%LocalAppData%\Xamarin\Mono for Android\debug.keystore" -alias androiddebugkey -storepass android -keypass android
```

Gdy zostanie uruchomione, **keytool.exe** powinien output następujący tekst. **MD5:** i **SHA1:** etykiety zidentyfikować odpowiednich podpisów:

```cmd
Alias name: androiddebugkey
Creation date: Aug 19, 2014
Entry type: PrivateKeyEntry
Certificate chain length: 1
Certificate[1]:
Owner: CN=Android Debug, O=Android, C=US
Issuer: CN=Android Debug, O=Android, C=US
Serial number: 53f3b126
Valid from: Tue Aug 19 13:18:46 PDT 2014 until: Sun Nov 15 12:18:46 PST 2043
Certificate fingerprints:
         MD5:  27:78:7C:31:64:C2:79:C6:ED:E5:80:51:33:9C:03:57
         SHA1: 00:E5:8B:DA:29:49:9D:FC:1D:DA:E7:EE:EE:1A:8A:C7:85:E7:31:23
         SHA256: 21:0D:73:90:1D:D6:3D:AB:4C:80:4E:C4:A9:CB:97:FF:34:DD:B4:42:FC:
08:13:E0:49:51:65:A6:7C:7C:90:45
         Signature algorithm name: SHA1withRSA
         Version: 3
```


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Zlokalizuj Xamarin **debug.keystore** pliku, który został użyty do podpisania aplikacji. Domyślnie magazynu kluczy, który jest używany do podpisywania wersje do debugowania aplikacji platformy Xamarin.Android można znaleźć w następującej lokalizacji:

**~/.Local/share/Xamarin/mono dla Android/debug.keystore**


Informacje o magazynie kluczy są uzyskiwane przez uruchomienie **keytool** polecenie zestaw JDK. To narzędzie jest zwykle znajdują się w następującej lokalizacji:

**/ System/biblioteki/Java/JavaVirtualMachines/*wersji*.jdk/Contents/Home/bin/keytool**

Dodaj katalog zawierający **keytool** do **ścieżki** zmiennej środowiskowej.
Otwórz **terminali** i uruchom **keytool** za pomocą następującego polecenia:

```bash
$ keytool -list -v -keystore ~/.local/share/Xamarin/Mono\ for\ Android/debug.keystore -alias androiddebugkey -storepass android -keypass android
```

Gdy zostanie uruchomione, **keytool** powinien output następujący tekst. **MD5:** i **SHA1:** etykiety zidentyfikować odpowiednich podpisów:

```bash
Alias name: androiddebugkey
Creation date: Apr 16, 2015
Entry type: PrivateKeyEntry
Certificate chain length: 1
Certificate[1]:
Owner: CN=Android Debug, O=Android, C=US
Issuer: CN=Android Debug, O=Android, C=US
Serial number: 76afa120
Valid from: Thu Apr 16 10:52:27 PDT 2015 until: Sat Apr 08 10:52:27 PDT 2045
Certificate fingerprints:
     MD5:  0A:D3:7E:80:3D:40:2A:23:89:B9:AB:9C:4B:B6:63:36
     SHA1: 89:33:8F:F2:C5:0C:91:08:4A:CF:04:A5:EC:4A:31:80:84:18:0D:D4
     SHA256: 91:AC:3E:2F:CB:EF:50:07:2B:E0:D9:8D:8B:C2:42:87:6A:85:02:86:EB:44:84:10:34:02:ED:35:CE:C6:38:47
     Signature algorithm name: SHA256withRSA
     Version: 3

Extensions:

#1: ObjectId: 2.5.29.14 Criticality=false
SubjectKeyIdentifier [
KeyIdentifier [
0000: 65 6C FE 67 8E CD CB 9A   01 CD 9F E8 25 9C A4 A3  el.g........%...
0010: 4F 4E CF 17                                        ON..
]
]
```

-----

## <a name="for-release--custom-signed-builds"></a>Dla wersji / niestandardowe podpisany kompilacji

Proces kompilacji zlecenia, które są podpisane za pomocą niestandardowego **magazynu kluczy** plików są takie same jak powyżej, wraz z wydaniem **magazynu kluczy** zastąpienie pliku **debug.keystore** pliku używany przez platformy Xamarin.Android. Zastąp wartości dla magazynu kluczy hasła i aliasu od utworzenia wersji pliku magazynu kluczy.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Gdy program Visual Studio **dystrybucji** Kreator jest używany do podpisywania aplikacji platformy Xamarin.Android, wynikowy magazynu kluczy znajduje się w następującej lokalizacji:

**C:\\użytkowników\\*USERNAME*\\AppData\\lokalnego\\Xamarin\\Mono dla systemu Android\\alias\\alias.keystore**

Na przykład, jeśli wykonano czynności opisane w [Utwórz nowy certyfikat](~/android/deploy-test/signing/index.md#newcertvs) można utworzyć nowego klucza podpisywania, wynikowy keystore przykład znajduje się w następującej lokalizacji:

**C:\\użytkowników\\*USERNAME*\\AppData\\lokalnego\\Xamarin\\Mono dla systemu Android\\chimp\\chimp.keystore**

Aby uzyskać więcej informacji na temat podpisywania aplikacji platformy Xamarin.Android, zobacz [podpisywanie pakietu aplikacji systemu Android](~/android/deploy-test/signing/index.md).


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Gdy programu Visual Studio for Mac **utworzyć i dystrybuować...**  kreatora do podpisania aplikacji, wynikowy magazynu kluczy znajduje się w następującej lokalizacji:

**~/Library/Developer/Xamarin/KeyStore/*alias*/*alias*magazynu kluczy**

Na przykład, jeśli wykonano czynności opisane w [Utwórz nowy certyfikat](~/android/deploy-test/signing/index.md#newcertxs) można utworzyć nowego klucza podpisywania, wynikowy keystore przykład znajduje się w następującej lokalizacji:

**~/Library/Developer/Xamarin/Keystore/chimp/chimp.keystore**

Aby uzyskać więcej informacji na temat podpisywania aplikacji platformy Xamarin.Android, zobacz [podpisywanie pakietu aplikacji systemu Android](~/android/deploy-test/signing/index.md).


-----
