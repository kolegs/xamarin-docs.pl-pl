---
title: Ręcznie podpisywania plik APK
ms.prod: xamarin
ms.assetid: 08549E1C-7F04-4D20-9E7A-794B9D09FD12
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 9e1b168b7212f093b50a36c40550fba2e7d63e77
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="manually-signing-the-apk"></a>Ręcznie podpisywania plik APK


Po utworzeniu aplikacji w wersji musi być podpisany plik APK przed dystrybucji, dzięki czemu może działać na urządzeniu z systemem Android. Ten proces zwykle odbywa się z IDE, jednak istnieje kilka sytuacji, gdy konieczne jest ręczne, podpisać plik APK w wierszu polecenia. Podpisywania APK obejmuje następujące kroki:

1.   **Utwórz klucz prywatny** &ndash; w tym kroku musi zostać wykonana tylko raz. Klucz prywatny jest niezbędne do cyfrowego podpisywania plik APK.
    Po przygotowaniu klucza prywatnego ten krok można pominąć dla kompilacji w przyszłości.

2.   **Zipalign APK** &ndash; *Zipalign* jest procesem optymalizacji, które jest przeprowadzane w aplikacji. Umożliwia on Android wydajniej interakcję z APK w czasie wykonywania. Xamarin.Android przeprowadza sprawdzenie w czasie wykonywania, a nie zezwoli aplikacji do uruchomienia, jeśli plik APK nie został zipaligned.

3.  **Podpisać plik APK** &ndash; ten krok polega na użyciu **apksigner** narzędzi z zestawu SDK systemu Android i podpisywania plik APK z kluczem prywatnym, który został utworzony w poprzednim kroku. Aplikacje, które są tworzone w starszych wersjach narzędzia kompilacji zestawu SDK systemu Android przed v24.0.3 będzie używać **jarsigner** aplikacji z zestaw JDK. Oba te narzędzia zostanie dokładnie omówione bardziej szczegółowo poniżej. 

Kolejność kroków jest ważna i jest zależna od które narzędzie używane do podpisywania plik APK. Korzystając z **apksigner**, ważne jest, aby pierwszy **zipalign** aplikacji, a następnie zaloguj się za pomocą **apksigner**.  Jeśli zachodzi konieczność użycia **jarsigner** Aby podpisać plik APK, następnie należy najpierw zapisać plik APK, a następnie uruchom **zipalign**. 



## <a name="prerequisites"></a>Wymagania wstępne

Ten przewodnik koncentruje się na temat używania **apksigner** narzędzi, kompilacji z zestawu SDK systemu Android v24.0.3 lub nowszej. Przyjęto założenie, że APK został już utworzony.

Aplikacje, które są tworzone przy użyciu starszej wersji narzędzia kompilacji zestawu SDK systemu Android muszą używać **jarsigner** zgodnie z opisem w [podpisać plik APK z jarsigner](#Sign_the_APK_with_jarsigner) poniżej.



## <a name="create-a-private-keystore"></a>Tworzenie magazynu kluczy prywatnych

A *keystore* jest bazy danych certyfikatów zabezpieczeń, która jest tworzona przy użyciu programu [keytool](https://docs.oracle.com/javase/8/docs/technotes/tools/unix/keytool.html) z zestawu Java SDK. Bardzo ważne publikowanie aplikacji platformy Xamarin.Android jest magazynu kluczy, zgodnie z systemem Android nie zostaną uruchomione aplikacje, które nie zostały podpisane cyfrowo.

Podczas tworzenia Xamarin.Android używa keystore debugowania do podpisywania aplikacji, która umożliwia aplikacji można wdrożyć bezpośrednio do emulatora lub urządzeń skonfigurowanych do korzystania z możliwością debugowania aplikacji.
Jednak ta magazynu kluczy nie został rozpoznany jako prawidłowy magazynu kluczy na potrzeby dystrybucji aplikacji.

Z tego powodu prywatnego magazynu kluczy musi utworzony i używany do podpisywania aplikacji. Jest to krok, które powinny być wykonywane jedynie raz, ten sam klucz będzie używany do publikowania aktualizacji, a następnie może być używany do podpisywania innych aplikacji.

Należy chronić tego magazynu kluczy. W przypadku utraty następnie go nie będzie możliwe do publikowania aktualizacji aplikacji ze sklepu Google Play.
Tylko rozwiązanie problem spowodowany przez utracone magazynu kluczy jest tworzenie nowego magazynu kluczy, ponownie podpisać plik APK za pomocą nowego klucza, a następnie przekaż nową aplikację. Następnie starej aplikacji musi być usunięte z witryny Google Play. Podobnie jeśli tego nowego magazynu kluczy naruszenia lub publicznie rozproszonych, następnie istnieje możliwość Nieoficjalny lub złośliwymi wersji aplikacji do dystrybucji.



### <a name="create-a-new-keystore"></a>Tworzenie nowego magazynu kluczy

Tworzenie nowego magazynu kluczy wymaga narzędzia wiersza polecenia [keytool](https://docs.oracle.com/javase/8/docs/technotes/tools/unix/keytool.html) z zestawu Java SDK. Poniższy fragment kodu przedstawiono przykładowy sposób użycia **keytool** (Zastąp `<my-filename>` z nazwą pliku magazynu kluczy i `<key-name>` z nazwą klucza w ramach magazynu kluczy):

```shell
$ keytool -genkeypair -v -keystore <filename>.keystore -alias <key-name> -keyalg RSA \
          -keysize 2048 -validity 10000
```

Najpierw który **keytool** poprosi o hasło magazynu kluczy. Następnie zostanie wyświetlone pytanie niektóre informacje ułatwiające tworzenie klucza. Poniższy fragment jest przykładem tworzenia nowy klucz o nazwie `publishingdoc` przechowywanego w pliku `xample.keystore`:

```shell
$ keytool -genkeypair -v -keystore xample.keystore -alias publishingdoc -keyalg RSA -keysize 2048 -validity 10000
Enter keystore password:
Re-enter new password:
What is your first and last name?
  [Unknown]:  Ham Chimpanze
What is the name of your organizational unit?
  [Unknown]:  NASA
What is the name of your organization?
  [Unknown]:  NASA
What is the name of your City or Locality?
  [Unknown]:  Cape Canaveral
What is the name of your State or Province?
  [Unknown]:  Florida
What is the two-letter country code for this unit?
  [Unknown]:  US
Is CN=Ham Chimpanze, OU=NASA, O=NASA, L=Cape Canaveral, ST=Florida, C=US correct?
  [no]:  yes

Generating 2,048 bit RSA key pair and self-signed certificate (SHA1withRSA) with a validity of 10,000 days
        for: CN=Ham Chimpanze, OU=NASA, O=NASA, L=Cape Canaveral, ST=Florida, C=US
Enter key password for <publishingdoc>
        (RETURN if same as keystore password):
Re-enter new password:
[Storing xample.keystore]
```

Aby wyświetlić listę kluczy, które są przechowywane w magazynie kluczy, należy użyć **keytool** z &ndash; `list` opcji:

```shell
$ keytool -list -keystore xample.keystore
```


## <a name="zipalign-the-apk"></a>Zipalign plik APK

Przed zarejestrowaniem APK z **apksigner**, ważne jest, aby najpierw zoptymalizować plików przy użyciu **zipalign** narzędzi z zestawu SDK systemu Android. **zipalign** będzie restrukturyzacji zasobów w APK wzdłuż 4-bajtowych granicach. Wyrównanie ten umożliwia Android szybko załadować zasobów z APK, co zwiększa wydajność aplikacji i potencjalnie zmniejsza zużycie pamięci. Xamarin.Android przeprowadzi sprawdzenie środowiska wykonawczego w celu określenia, czy plik APK został zipaligned. Jeśli plik APK nie jest zipaligned, aplikacja nie będzie działać.

Wykonaj polecenie użyje podpisem APK i utworzyć zalogowany, nazywany APK zipaligned **helloworld.apk** który jest gotowy do dystrybucji.

```shell
$ zipalign -f -v 4 mono.samples.helloworld-unsigned.apk helloworld.apk
```


## <a name="sign-the-apk"></a>Podpisać plik APK

Po zipaligning plik APK należy podpisać ją przy użyciu magazynu kluczy. Jest to zrobić za pomocą **apksigner** narzędzia w **narzędzia kompilacji** katalogu wersji zestawu SDK narzędzia kompilacji.  Na przykład, jeśli zestaw SDK systemu Android kompilacji narzędzia v25.0.3 jest zainstalowany, następnie **apksigner** znajduje się w katalogu:

```bash
$ ls $ANDROID_HOME/build-tools/25.0.3/apksigner
/Users/tom/android-sdk-macosx/build-tools/25.0.3/apksigner*
```

Poniższy fragment kodu zakłada się, że **apksigner** jest dostępny dla `PATH` zmiennej środowiskowej. Będzie podpisywać APK za pomocą aliasu klucza `publishingdoc` który jest zawarty w pliku **xample.keystore**:

```shell
$ apksigner sign --ks xample.keystore --ks-key-alias publishingdoc mono.samples.helloworld.apk
```

Po uruchomieniu tego polecenia, **apksigner** poprosi o podanie hasła w magazynie kluczy, jeśli to konieczne.

Zobacz [dokumentacji firmy Google](https://developer.android.com/studio/command-line/apksigner.html) więcej szczegółów dotyczących korzystania z **apksigner**.

> [!NOTE]
> Zgodnie z [problem Google 62696222](https://issuetracker.google.com/issues/62696222), **apksigner** "Brak" z zestawu SDK systemu Android. Obejście tego jest zainstalowanie v25.0.3 narzędzia kompilacji zestawu SDK systemu Android i użyć tej wersji **apksigner**.  


<a name="Sign_the_APK_with_jarsigner" />

### <a name="sign-the-apk-with-jarsigner"></a>Zaloguj się APK z jarsigner

> [!WARNING]
> Ta sekcja dotyczy tylko, jeśli jest nececssary do podpisania APK z **jarsigner** narzędzia. Deweloperzy są zachęcani do użycia **apksigner** Aby podpisać plik APK.

Ta metoda obejmuje podpisywanie przy użyciu pliku APK **[jarsigner](https://docs.oracle.com/javase/8/docs/technotes/tools/windows/jarsigner.html)** polecenia z zestawu Java SDK.  **Jarsigner** narzędzie jest dostarczane przez zestaw SDK Java. 

Poniżej pokazano, jak zarejestrować APK przy użyciu **jarsigner** oraz klucz `publishingdoc` który jest zawarty w pliku magazynu kluczy o nazwie **xample.keystore** :

```shell
$ jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 -keystore xample.keystore mono.samples.helloworld.apk publishingdoc
```

> [!NOTE]
> Korzystając z **jarsigner**, ważne jest, aby podpisać plik APK _pierwszy_, a następnie użyć **zipalign**.  



## <a name="related-links"></a>Linki pokrewne

- [Podpisywania aplikacji](https://source.android.com/security/apksigning/)
- [Java JAR podpisywania](https://docs.oracle.com/javase/8/docs/technotes~/jar/jar.html#Signed_JAR_File)
- [jarsigner](https://docs.oracle.com/javase/8/docs/technotes/tools/windows/jarsigner.html)
- [keytool](https://docs.oracle.com/javase/8/docs/technotes/tools/unix/keytool.html)
- [zipalign](https://developer.android.com/studio/command-line/zipalign.html)
- [Narzędzi 26.0.0 — gdy stało apksigner kompilacji?](https://issuetracker.google.com/issues/62696222)
