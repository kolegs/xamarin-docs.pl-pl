---
title: "Rozwiązywanie problemów z powiązań"
description: "W tym artykule przedstawiono kilka typowych błędów, które mogą wystąpić podczas generowania powiązań, a także możliwe przyczyny i sugerowane sposoby ich rozwiązywania."
ms.topic: article
ms.prod: xamarin
ms.assetid: BB81FCCF-F7BF-4C78-884E-F02C49AA819A
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 6d31e2a22c63f8d46893dd1928b561e1a06b19b4
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/12/2018
---
# <a name="troubleshooting-bindings"></a>Rozwiązywanie problemów z powiązań

_W tym artykule przedstawiono kilka typowych błędów, które mogą wystąpić podczas generowania powiązań, a także możliwe przyczyny i sugerowane sposoby ich rozwiązywania._


## <a name="overview"></a>Omówienie

Powiązanie biblioteki systemu Android ( **.aar** lub **JAR**) pliku jest rzadko affair prostego; zwykle wymaga dodatkowego nakładu pracy w celu ograniczenia problemów wynikających z różnic między Java i .NET.
Te problemy zostaną zapobiec Xamarin.Android powiązanie biblioteki systemu Android i przedstawiają się jako komunikaty o błędach w dzienniku kompilacji. W tym przewodniku Podaj porady dotyczące rozwiązywania problemów, niektóre z najczęściej problemów/scenariuszy i podaj możliwych rozwiązań pomyślnie powiązanie biblioteki systemu Android.

Powiązanie istniejącej biblioteki systemu Android, należy mieć na uwadze następujące kwestie:

- **Zależności zewnętrzne dla biblioteki** &ndash; Java wszystkich zależności wymagane przez biblioteki systemu Android muszą być zawarte w projekcie platformy Xamarin.Android jako **ReferenceJar** lub jako  **EmbeddedReferenceJar**.

- **Biblioteka systemu Android to wskazuje poziom interfejsu API systemu Android** &ndash; nie jest możliwe "przejścia" poziom interfejsu API systemu Android, upewnij się, projekt platformy Xamarin.Android powiązanie dotyczy tego samego interfejsu API level (lub nowszej) jako biblioteka systemu Android.

- **Wersja systemu Android JDK, użytej do pakietu biblioteki systemu Android** &ndash; błędów powiązań może wystąpić, jeśli biblioteka systemu Android został utworzony za pomocą innej wersji JDK niż używana przez platformy Xamarin.Android. Jeśli to możliwe Skompiluj ponownie biblioteki systemu Android przy użyciu tej samej wersji JDK, używanego przez instalację platformy Xamarin.Android.

Pierwszy krok w celu rozwiązywania problemów z powiązaniem biblioteki platformy Xamarin.Android jest umożliwienie [wyjście diagnostyczne MSBuild](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output).
Po włączeniu diagnostycznych danych wyjściowych, skompiluj ponownie projekt platformy Xamarin.Android powiązania i przeanalizuj dziennik kompilacji można zlokalizować informacji o tym, co jest przyczyną problemu.

Może również być pomocne dekompilować biblioteki systemu Android i sprawdź, czy typy i metody, które próbuje powiązać platformy Xamarin.Android. Jest to opisane bardziej szczegółowo później w tym przewodniku.


## <a name="decompiling-an-android-library"></a>Decompiling biblioteki systemu Android

Zapoznanie się klasy i metody klas Java zapewniają cenne informacje, które pomogą w wiązaniu biblioteki.
[Graficzny interfejs użytkownika JD](http://jd.benow.ca/) to graficzne narzędzie, które można wyświetlić kodu źródłowego języka Java z **klasy** plików znajdujących się w JAR. Uruchomieniem jako aplikacja autonomiczny lub jako wtyczkę IntelliJ lub Eclipse.

Dekompilować otwarty biblioteki systemu Android **. JAR** pliku z Java decompiler. Jeśli biblioteka jest **. AAR** pliku, jest niezbędne wyodrębnić plik **classes.jar** z pliku archiwum. Poniżej przedstawiono przykładowe zrzut ekranu przy użyciu graficznego interfejsu użytkownika JD do analizowania [Picasso](http://square.github.io/picasso/) JAR:

![Analizowanie picasso 2.5.2.jar za pomocą Java Decompiler](troubleshooting-bindings-images/troubleshoot-bindings-01.png)

Gdy ma decompiled biblioteki systemu Android, zbadanie kodu źródłowego. Ogólnie rzecz biorąc Wyszukaj:

- **Klasy, które mają właściwości zaciemnienie** &ndash; cechy zaciemnionego klasy:

    - Nazwa klasy zawiera  **$** , tj. **$.class**
    - Nazwa klasy jest całkowicie naruszenia zabezpieczeń z małych liter, tj. **a.class**      

- **`import` instrukcje dla bibliotek nieużywane** &ndash; zidentyfikować nieużywane biblioteki i Dodaj do projektu platformy Xamarin.Android powiązania z tych zależności **Akcja kompilacji** z **ReferenceJar**  lub **EmbedddedReferenceJar**.

> [!NOTE]
> Decompiling biblioteka języka Java może być zabroniony lub może ulec ograniczenia prawne na podstawie lokalnych przepisów lub licencji, w którym została opublikowana biblioteka języka Java. Jeśli to konieczne, należy zarejestrować usługi prawne professional przed podjęciem próby dekompilować biblioteka języka Java i kontroli kodu źródłowego.


## <a name="inspect-apixml"></a>Sprawdź, czy interfejsu API. XML

Podczas tworzenia projektu powiązania Xamarin.Android wygeneruje nazwę pliku XML **obj/Debug/api.xml**:

![Wygenerowany api.xml w obszarze obj/Debug](troubleshooting-bindings-images/troubleshoot-bindings-02.png)

Ten plik zawiera listę wszystkich interfejsów API języka Java, że Xamarin.Android próbuje powiązania. Zawartość tego pliku może pomóc zidentyfikować wszelkie brakujące typach lub metodach, zduplikowane powiązanie. Mimo że kontroli tego pliku jest niewygodny i czasochłonne, zapewniają dla operacji na jakie mogą być przyczyną problemów powiązania. Na przykład **api.xml** może ujawnić, czy właściwość zwraca typ nieodpowiednie, lub że istnieją dwa typy korzystających z tej samej nazwie zarządzanych.


## <a name="known-issues"></a>Znane problemy

Ta sekcja Wyświetla niektóre typowe komunikaty o błędach lub symptomów który Moje wystąpić podczas próby utworzenia powiązania biblioteki systemu Android.


### <a name="problem-java-version-mismatch"></a>Problemu: Niezgodność wersji języka Java

Czasami typów nie zostanie wygenerowany lub nieoczekiwane awarie mogą wystąpić, ponieważ używasz albo nowszej lub starsza wersja programu Java w porównaniu do biblioteki skompilowanego z. Skompiluj ponownie biblioteki systemu Android przy użyciu tej samej wersji JDK, który używa projektu platformy Xamarin.Android.


### <a name="problem-at-least-one-java-library-is-required"></a>Problem: wymagana jest co najmniej jeden biblioteka języka Java

Zostanie wyświetlony błąd "wymagane jest co najmniej jedną bibliotekę Java" mimo że. JAR został dodany.

#### <a name="possible-causes"></a>Możliwe przyczyny:

Upewnij się, że akcja kompilowania została ustawiona `EmbeddedJar`. Ponieważ istnieje wiele operacji kompilacji. Pliki JAR (takich jak `InputJar`, `EmbeddedJar`, `ReferenceJar` i `EmbeddedReferenceJar`), generator powiązania nie można odgadnąć automatycznie co ma być używany domyślnie. Aby uzyskać więcej informacji na temat akcji kompilacji, zobacz [akcje kompilacji](~/android/platform/binding-java-library/index.md).


### <a name="problem-binding-tools-cannot-load-the-jar-library"></a>Problem: Powiązanie narzędzia nie można załadować. Biblioteka JAR

Generator bibliotek powiązania nie udało się załadować. Biblioteka JAR.

#### <a name="possible-causes"></a>Możliwe przyczyny

Niektóre. Nie można załadować biblioteki JAR, które używają zaciemnienie kodu (za pomocą narzędzi, takich jak narzędzia Proguard) za pomocą narzędzi języka Java. Ponieważ naszego narzędzia sprawia, że użycie odbicia Java i kod bajtowy ASM inżynierii biblioteki, te narzędzia zależnych może odrzucić zaciemnionego biblioteki podczas może przekazywać narzędzi obsługi systemu Android. Obejście tego jest ręcznie — wiązanie te biblioteki zamiast generatora powiązania.



### <a name="problem-missing-c-types-in-generated-output"></a>Problem: Brak typów C# w wygenerowanych danych wyjściowych.

Powiązanie **.dll** kompilacji, ale chybień niektóre typy Java lub wygenerowanego źródła C# nie kompiluje się z powodu komunikat o błędzie informujący, typy Brak.

#### <a name="possible-causes"></a>Możliwe przyczyny:

Ten błąd może wystąpić z kilku powodów wymienionych poniżej:

-   Biblioteka związany może odwoływać się drugi biblioteka języka Java. Jeśli publiczny interfejs API powiązane biblioteki używane typy z drugiej biblioteki, musi odwoływać się zarządzanych powiązanie również drugi biblioteki.

-   Istnieje możliwość, że biblioteki zostało spowodowane z powodu odbicia Java, podobnie jak przyczyna błąd ładowania biblioteki powyżej, powodując nieoczekiwany podczas ładowania metadanych. Narzędzia dla platformy Xamarin.Android obecnie nie można rozpoznać tej sytuacji. W takim przypadku biblioteki musi być ręcznie powiązana.

-   Wystąpił błąd środowiska uruchomieniowego .NET 4.0, którego nie można załadować zestawów, gdy powinien mieć. Ten problem został rozwiązany w środowisku wykonawczym .NET 4.5.

-   Umożliwia Java pochodny Klasa publiczna klasy niepublicznej, ale to nie jest obsługiwana w środowisku .NET. Ponieważ generator powiązania nie generuje powiązań dla niepublicznych klas, należy klas pochodnych, takich jak te nie można prawidłowo wygenerować. Aby rozwiązać ten problem, Usuń wpis metadanych dla tych klas pochodnych przy użyciu Usuń węzeł w **Metadata.xml**, lub usuń jest ujawnianie klasy niepublicznej metadane. Mimo że drugie rozwiązanie będzie utworzyć powiązanie, tak aby utworzy źródłowym C#, nie można użyć klasy niepublicznej.

    Na przykład:

    ```xml
    <attr path="/api/package[@name='com.some.package']/class[@name='SomeClass']"
        name="visibility">public</attr>
    ```

-   Narzędzia, które zasłaniają bibliotek języka Java może zakłócać Generator powiązanie platformy Xamarin.Android i jego zdolność do generowania klasy otoki C#. Poniższy fragment kodu pokazano, jak zaktualizować **Metadata.xml** do unobfuscate nazwy klasy:

    ```xml
    <attr path="/api/package[@name='{package_name}']/class[@name='{name}']"
        name="obfuscated">false</attr>
    ```

### <a name="problem-generated-c-source-does-not-build-due-to-parameter-type-mismatch"></a>Problem: Źródła wygenerowanego C# nie kompilować się z powodu niezgodności typów parametru

Wygenerowanego źródła C# nie kompilacji. Przesłonięcia parametru metody, których typy są niezgodne.

#### <a name="possible-causes"></a>Możliwe przyczyny:

Xamarin.Android zawiera wiele pól Java, które są mapowane do wyliczenia w powiązaniach C#. Mogą być przyczyną niezgodności typu w wygenerowanym powiązania. Aby rozwiązać ten problem, podpisy metod utworzone na podstawie generator powiązanie konieczne modyfikacji w celu użycia wyliczenia. Aby uzyskać więcej imformation, zobacz [korygowanie wyliczenia](~/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata.md).

### <a name="problem-noclassdeffounderror-in-packaging"></a>Problem: NoClassDefFoundError w pakiecie

`java.lang.NoClassDefFoundError` jest generowany w kroku opakowania.

#### <a name="possible-causes"></a>Możliwe przyczyny:

Najbardziej prawdopodobną przyczyną tego błędu jest obowiązkowy biblioteka języka Java musi być dodany do projektu aplikacji (**.csproj**). . Pliki JAR nie są rozpoznawane automatycznie. Powiązanie biblioteka języka Java nie jest zawsze generowana względem zestawu użytkownika, który nie istnieje w docelowym urządzenia lub emulatora (takich jak Google Maps **maps.jar**). To nie jest w przypadku obsługi projektu biblioteki systemu Android, co biblioteki. JAR jest osadzony w bibliotece dll. Na przykład: [4288 usterki](https://bugzilla.xamarin.com/show_bug.cgi?id=4288)

### <a name="problem-duplicate-custom-eventargs-types"></a>Problem: Zduplikowane typy EventArgs niestandardowe

Kompilacja kończy się niepowodzeniem z powodu zduplikowanego niestandardowych typów EventArgs. Następujący błąd:

```shell
error CS0102: The type `Com.Google.Ads.Mediation.DismissScreenEventArgs' already contains a definition for `p0'
```

#### <a name="possible-causes"></a>Możliwe przyczyny:

Jest tak, ponieważ istnieje konflikt niektórych między typy zdarzeń, które pochodzą z więcej niż jednego typu "odbiornika" interfejsu, który udostępnia metody o identycznej nazwie. Na przykład, jeśli istnieją dwa interfejsy Java, jak pokazano w poniższym przykładzie, tworzy generator `DismissScreenEventArgs` dla obu `MediationBannerListener` i `MediationInterstitialListener`, który znajduje się kod błędu.

```java
// Java:
public interface MediationBannerListener {
    void onDismissScreen(MediationBannerAdapter p0);
}
public interface MediationInterstitialListener {
    void onDismissScreen(MediationInterstitialAdapter p0);
}
```

To jest celowe tak, aby uniknąć długich nazw na typy argumentów zdarzenia. Aby uniknąć tych konfliktów, niektóre metadane transformacji jest wymagana. Edytuj [ **Transforms\Metadata.xml** ](https://github.com/xamarin/monodroid-samples/blob/master/AdMob/AdMob/Transforms/Metadata.xml) i Dodaj `argsType` atrybutów w zależności od interfejsów (lub dla metody interfejsu):

```xml
<attr path="/api/package[@name='com.google.ads.mediation']/
        interface[@name='MediationBannerListener']/method[@name='onDismissScreen']"
        name="argsType">BannerDismissScreenEventArgs</attr>

<attr path="/api/package[@name='com.google.ads.mediation']/
        interface[@name='MediationInterstitialListener']/method[@name='onDismissScreen']"
        name="argsType">IntersitionalDismissScreenEventArgs</attr>

<attr path="/api/package[@name='android.content']/
        interface[@name='DialogInterface.OnClickListener']"
        name="argsType">DialogClickEventArgs</attr>
```

### <a name="problem-class-does-not-implement-interface-method"></a>Problem: Klasa nie implementuje metody interfejsu

Komunikat o błędzie jest generowany, wskazującą, czy wygenerowana klasa nie implementuje metody, która jest wymagana dla interfejsu, który implementuje wygenerowanej klasy. Jednak patrzeć wygenerowany kod, spowoduje wykonanie metody.

Oto przykład błędu:

```shell
obj\Debug\generated\src\Oauth.Signpost.Basic.HttpURLConnectionRequestAdapter.cs(8,23):
error CS0738: 'Oauth.Signpost.Basic.HttpURLConnectionRequestAdapter' does not
implement interface member 'Oauth.Signpost.Http.IHttpRequest.Unwrap()'.
'Oauth.Signpost.Basic.HttpURLConnectionRequestAdapter.Unwrap()' cannot implement
'Oauth.Signpost.Http.IHttpRequest.Unwrap()' because it does not have the matching
return type of 'Java.Lang.Object'
```

#### <a name="possible-causes"></a>Możliwe przyczyny:

Jest to problem występujący z powiązaniem metody Java z kowariantne typy zwracane. W tym przykładzie metoda `Oauth.Signpost.Http.IHttpRequest.UnWrap()` musi zwracać `Java.Lang.Object`. Jednak metoda `Oauth.Signpost.Basic.HttpURLConnectionRequestAdapter.UnWrap()` ma typ zwracany `HttpURLConnection`. Istnieją dwa sposoby rozwiązania tego problemu:

-   Dodaj deklarację klasy częściowej dla `HttpURLConnectionRequestAdapter` i jawne Implementowanie `IHttpRequest.Unwrap()`:

    ```csharp
    namespace Oauth.Signpost.Basic {
        partial class HttpURLConnectionRequestAdapter {
            Java.Lang.Object OauthSignpost.Http.IHttpRequest.Unwrap() {
                return Unwrap();
            }
        }
    }
    ```

-   Usuń kowariancję z wygenerowanego kodu C#. Obejmuje to następujące przekształcenie do dodawania **Transforms\Metadata.xml** który spowoduje, że wygenerowany kod C# ma typ zwracany `Java.Lang.Object`:

    ```xml
    <attr
        path="/api/package[@name='oauth.signpost.basic']/class[@name='HttpURLConnectionRequestAdapter']/method[@name='unwrap']"
        name="managedReturn">Java.Lang.Object
    </attr>
    ```

### <a name="problem-name-collisions-on-inner-classes--properties"></a>Problem: Nazwa kolizji w wewnętrznym klasy / właściwości

Konflikt widoczność dziedziczonych obiektów.

W języku Java nie jest to wymagane czy klasy pochodnej mają taką samą widoczność jak jego obiekt nadrzędny. Java naprawi który tylko dla Ciebie. W języku C#, który musi być określone jawnie, dlatego należy upewnić się, że wszystkie klasy w hierarchii mają odpowiednie widoczność. Poniższy przykład pokazuje, jak można zmienić nazwy pakietów języka Java z `com.evernote.android.job` do `Evernote.AndroidJob`:

```xml
<!-- Change the visibility of a class -->
<attr path="/api/package[@name='namespace']/class[@name='ClassName']" name="visibility">public</attr>

<!-- Change the visibility of a method -->
<attr path="/api/package[@name='namespace']/class[@name='ClassName']/method[@name='MethodName']" name="visibility">public</attr>
```

### <a name="problem-a-so-library-required-by-the-binding-is-not-loading"></a>Problem: A **.so** biblioteki wymaganej przez powiązanie jest nie ładowania

Niektóre projekty powiązania może też zależą od funkcji w **.so** biblioteki. Istnieje możliwość, że Xamarin.Android nie są ładowane automatycznie **.so** biblioteki. Podczas opakowana kodu języka Java, Xamarin.Android zakończy się niepowodzeniem wywołania JNI i komunikat o błędzie _java.lang.UnsatisfiedLinkError: nie można odnaleźć metody natywnej:_ będą wyświetlane w logcat limit dla aplikacji.

W tym będzie ręcznie załadować **.so** biblioteki z wywołania `Java.Lang.JavaSystem.LoadLibrary`. Na przykład przy założeniu, że projekt platformy Xamarin.Android udostępnił biblioteki **libpocketsphinx_jni.so** zawarty w projekcie powiązania z akcją kompilacji **EmbeddedNativeLibrary**, następujące fragment kodu (wykonywane przed rozpoczęciem korzystania z biblioteki udostępnionej) zostanie załadowany **.so** biblioteki:

```csharp
Java.Lang.JavaSystem.LoadLibrary("pocketsphinx_jni");
```

## <a name="summary"></a>Podsumowanie

W tym artykule firma Microsoft wymienionych wspólnej rozwiązywania problemów związanych z powiązań Java i wyjaśniono ich rozwiązania.


## <a name="related-links"></a>Linki pokrewne

- [Projekty bibliotek](http://developer.android.com/tools/projects/index.html#LibraryProjects)
- [Praca z JNI](~/android/platform/java-integration/working-with-jni.md)
- [Włącz dane wyjściowe diagnostyki](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output)
- [Xamarin dla deweloperów systemu Android](~/android/get-started/java-developers.md)
- [JD-GUI](http://jd.benow.ca/)
