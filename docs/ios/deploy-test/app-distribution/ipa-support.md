---
title: Obsługa IPA w Xamarin.iOS
description: W tym artykule omówiono sposób tworzenia pliku IPA, który może służyć do wdrażania aplikacji przy użyciu dystrybucji Ad Hoc do testowania lub wewnętrznych dystrybucji wewnętrznych aplikacji.
ms.prod: xamarin
ms.assetid: D253C2DB-852E-6FC6-C9FD-574730B8DB19
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 288ac813f23f281a1bbed375cadf5faa9d4ff9d0
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34784881"
---
# <a name="ipa-support-in-xamarinios"></a>Obsługa IPA w Xamarin.iOS

_W tym artykule omówiono sposób tworzenia pliku IPA, który może służyć do wdrażania aplikacji przy użyciu dystrybucji Ad Hoc do testowania lub wewnętrznych dystrybucji wewnętrznych aplikacji._

Oprócz wydanie aplikacji do sprzedaży za pomocą programu iTunes App Store, może on zostać wdrożony do następujących celów:

- **Testowanie Ad Hoc** — aplikacji systemu iOS można wdrożyć dla maksymalnie 100 użytkowników (określone przez urządzenie z systemem iOS określonych identyfikatorów UUID) dla Alpha i do celów testowania w wersji Beta. Zobacz nasze [inicjowania obsługi administracyjnej systemu iOS urządzenia na potrzeby programowania](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#provisioning) dokumentacji, aby uzyskać szczegółowe informacje na temat dodawania urządzeń z systemem iOS testu do konta dewelopera firmy Apple i [Ad Hoc](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md) przewodniku, aby uzyskać więcej informacji na temat Aby rozesłać w ten sposób.
- **Wewnętrznie / wdrożenia w przedsiębiorstwie** — aplikacji systemu iOS można wdrożyć wewnętrznie w firmie, która wymaga członkostwa firmy Apple Developer Enterprise program. Więcej informacji na temat dystrybucji w domu została szczegółowo opisana w [dystrybucji w domu](~/ios/deploy-test/app-distribution/in-house-distribution.md) przewodnik.

W obu przypadkach należy utworzony i podpisany cyfrowo za pomocą poprawnego profilu inicjowania obsługi administracyjnej dystrybucji pakiet IPA (specjalny typ pliku zip). W tym artykule opisano kroki wymagane do budowania pakiet IPA i zainstaluj pakiet na urządzeniu z systemem iOS na komputerze z systemem Windows lub Mac za pomocą programu iTunes.

<a name="iTunesMetadata" />

## <a name="the-itunesmetadataplist-file"></a>Plik iTunesMetadata.plist

Podczas tworzenia aplikacji systemu iOS w programach iTunes Connect (albo do sprzedaży lub bezpłatnej wersji ze sklepu iTunes App Store), deweloper można określić informacje, takie jak genre, sub genre aplikacji, o prawach autorskich, urządzeń z systemem iOS obsługiwanych i wymaganych urządzeń możliwości.

aplikacje systemu iOS, które są dostarczane albo za pośrednictwem dystrybucji Ad Hoc lub wewnętrznych, musi być określony sposób obsługuje te informacje, dzięki czemu może być widoczny w programach iTunes i na urządzeniu użytkownika. Domyślnie plik małych iTunesMetadata.plist jest tworzony za każdym razem skompilować projekt i są przechowywane w katalogu projektu.

Niestandardowy **iTunesMetadata.plist** można również tworzyć o podanie dodatkowych informacji do dystrybucji. Aby dowiedzieć się więcej na temat zawartości tego pliku i jak go utworzyć, zobacz nasze [zawartość iTunesMetadata.plist](~/ios/deploy-test/app-distribution/itunesmetadata.md#iTunesMetadata_contents) i [Tworzenie pliku iTunesMetadata.plist](~/ios/deploy-test/app-distribution/itunesmetadata.md#iTunesMetadata_creating) dokumentacji.

<a name="iTunesArtwork" />

## <a name="itunes-artwork"></a>iTunes kompozycji

W celu dostarczenia aplikacji za pośrednictwem środków - App Store, należy to 512 x 512 i obraz 1024 x 1024, który będzie używany do reprezentowania aplikacji w programach iTunes.

Aby określić iTunes kompozycji, wykonaj następujące czynności:

1. Kliknij dwukrotnie **Info.plist** w pliku **Eksploratora rozwiązań** go otworzyć do edycji.
2. Przewiń do **iTunes kompozycji** części edytora.
3. Wszystkie brakujące obrazu, kliknij miniaturę w edytorze, wybierz plik obrazu dla kompozycji żądaną iTunes z **Otwórz plik** okno dialogowe i kliknij przycisk **OK** lub **Otwórz** przycisk.
4. Powtórz ten krok, aż wszystkie wymagane obrazy zostały określone dla aplikacji.

Zobacz [iTunes kompozycji](~/ios/app-fundamentals/images-icons/app-icons.md) dokumentację, aby uzyskać więcej szczegółowych informacji.

<a name="createipa" />

## <a name="creating-an-ipa"></a>Tworzenie IPA

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Tworzenie IPA jest teraz wbudowany w publikacji nowego przepływu pracy. Aby to zrobić, wykonaj instrukcje poniżej, aby zarchiwizować aplikacji, podpisz go i zapisać Twoje IPA.

Przed rozpoczęciem tworzenia IPA dla rozwiązania i platform, upewnij się, że wybrano projektu iOS jako projekt startowy:

![](ipa-support-images/setasstartup.png "Wybrany projekt dla systemu iOS jako projekt startowy")

### <a name="build-your-archive"></a>Tworzenie archiwum

Aby utworzyć IPA _archiwum_ wersji kompilacji naszej aplikacji musi zostać utworzona. To archiwum zawiera naszej aplikacji i informacje identyfikacyjne o nim.


1. Wybierz **wersji | Urządzenie** konfiguracji w programie Visual Studio dla komputerów Mac:!

    ![](ipa-support-images/buildxs01new.png "Wybierz wersji | Konfiguracja urządzenia")

1. Z **kompilacji** menu, wybierz opcję **archiwum publikowania**:

    ![](ipa-support-images/buildxs02new.png "Wybierz archiwum do publikowania")

1. Po utworzeniu archiwum **archiwa** zostanie wyświetlony widok:

    ![](ipa-support-images/buildxs03new.png "Zostanie wyświetlony widok archiwa")


### <a name="sign-and-distribute-your-app"></a>Zaloguj się i dystrybucja aplikacji

Zawsze skompilować aplikację dla archiwum, zostanie on automatycznie otwarty **widoku archiwa**, wyświetlanie wszystkich projektów zarchiwizowane; pogrupowane według rozwiązania. Domyślnie ten widok przedstawia tylko bieżące, otwórz rozwiązanie. Aby wyświetlić wszystkie rozwiązania, których archiwów, kliknij na **Pokaż wszystkie archiwa** opcji.

Zalecane jest aby były przechowywane archiwa wdrażanie u klientów (Ad Hoc lub wewnętrznych wdrożeń), tak, aby informacje o debugowaniu, który jest generowany można symbolized w późniejszym terminie.

Należy pamiętać, że magazyn z systemem innym niż aplikacja tworzy **iTunesMetadata.plist** pliku i ustaw kompozycji iTunes zostaną automatycznie dołączone w Twojej IPA Jeśli występują w archiwum.

Aby zarejestrować aplikację i przygotowania go do dystrybucji:


1. Wybierz **utworzyć i dystrybuować...**  przycisk zostały pokazane poniżej:

    ![](ipa-support-images/buildxs04new.png "Wybierz znak i dystrybucja...")

1. Spowoduje to otwarcie Kreatora publikacji. Wybierz **Ad Hoc** lub **Enterprise**kanału dystrybucji (wewnętrzna), aby utworzyć pakiet:

    ![](ipa-support-images/distribute01.png "Wybierz Ad Hoc lub Enterprise dystrybucji wewnętrznych")

1. Na ekranie profilu inicjowania obsługi administracyjnej Wybierz tożsamość podpisywania i odpowiadający profil inicjowania obsługi administracyjnej lub ponownie podpisać tożsamość:

    ![](ipa-support-images/distribute02.png "Wybierz tożsamość podpisywania i odpowiadający profil inicjowania obsługi administracyjnej")

1. Sprawdź szczegóły pakietu, a następnie kliknij przycisk **publikowania**:

    ![](ipa-support-images/distribute03.png "Sprawdź informacje szczegółowe dotyczące pakietu")

1. Na koniec zapisz IPA użytkownika na komputerze:

    ![](ipa-support-images/distribute04.png "Zapisz plik IPA na komputerze")


### <a name="building-via-the-command-line-on-mac"></a>Korzystając z wiersza polecenia (Mac)

W niektórych przypadkach takich jak w środowisku CI może być konieczne jest skompilowanie możesz IPA przy użyciu wiersza polecenia. Wykonaj poniższe kroki, aby to osiągnąć:


1. Upewnij się, **opcje projektu > iOS opcje IPA > obejmują obrazy iTunesArtwork** jest zaznaczony i **pakietu ad-hoc/enterprise kompilacji (IPA)** zaznaczono:

    ![](ipa-support-images/imagexs04.png "Obejmują obrazy iTunesArtwork i utworzyć pakiet ad-hoc/enterprise, który jest sprawdzany IPA")

    Jeśli wolisz, zamiast tego można edytować **.csproj** plik w edytorze tekstów i ręczne dodanie odpowiednich dwóch właściwości do `PropertyGroup` konfiguracji, który będzie służyć do tworzenia aplikacji:

    ```xml
    <BuildIpa>true</BuildIpa>
    <IpaIncludeArtwork>false</IpaIncludeArtwork>
    ```

1. W przypadku dołączania opcjonalny **iTunesMetadata.plist** plików, kliknij **...**  przycisku, wybierz go z listy i kliknij przycisk **OK** przycisk:

     ![](ipa-support-images/imagexs03.png "Wybierz z listy iTunesMetadata.plist")

1. Wywołanie **xbuild** (lub **mdtool** klasycznego interfejsu API) bezpośrednio, a następnie przekaż tę właściwość w wierszu polecenia:

    ```bash
    /Library/Frameworks/Mono.framework/Commands/xbuild YourSolution.sln /p:Configuration=Ad-Hoc /p:Platform=iPhone /p:BuildIpa=true
    ```

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Po utworzeniu profilu inicjowania obsługi administracyjnej i zaznaczone, opcjonalny **iTunesMetadata.plist** plik został utworzony i iTunes kompozycji ustawiony w programie Visual Studio, możesz utworzyć IPA do dystrybucji. Następnie należy skonfigurować projekt. Wykonaj następujące czynności:

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy nazwę projektu platformy Xamarin.iOS i wybierz **właściwości** aby je otworzyć do edycji:

    ![](ipa-support-images/imagevs01.png "Wybierz polecenie Właściwości")

2. Wybierz **iOS opcje IPA** i wybierz **Ad Hoc** z **konfiguracji** listy rozwijanej:

    ![](ipa-support-images/imagevs02.png "Wybierz z listy rozwijanej konfiguracji Ad Hoc")

    > [!NOTE]
    > Konfiguracja Ad Hoc nie mogą być dostępne dla nowszej platformy Xamarin.iOS projektów. Jeśli nie jest dostępna, zaznacz **wersji** konfiguracji.

3. Jeśli łącznie z opcją **iTunesMetadata.plist** plików, kliknij **...**  przycisku, wybierz go z listy i kliknij przycisk **Otwórz** przycisk:

    ![](ipa-support-images/imagevs03.png "Wybierz z listy iTunesMetadata.plist")

4. Opcjonalnie można określić **nazwy pakietu** dla IPA, jeśli nie określono będzie mieć taką samą nazwę jak projektu platformy Xamarin.iOS.
5. Zapisz zmiany we właściwościach projektu.
6. Wybierz **Ad Hoc** z **kompilacji konfiguracji** listy rozwijanej, jeśli jest dostępna. W przeciwnym razie wybierz **wersji**:

    ![](ipa-support-images/imagevs05.png "Wybierz z listy rozwijanej konfiguracji kompilacji Ad Hoc")

7. Skompiluj projekt, aby utworzyć pakiet IPA.
8. IPA zostanie skompilowany **Bin > iOS Device > Ad Hoc (lub wersji)** folderu:

    ![](ipa-support-images/imagevs06.png "IPA w Eksploratorze plików")

-----

<a name="Customizing-the-IPA-Location" />

## <a name="customizing-the-ipa-location"></a>Dostosowywanie lokalizacji IPA

Nowy **MSBuild** właściwości `IpaPackageDir` został dodany do ułatwiają dostosowywanie **.ipa** pliku lokalizacji wyjściowej. Jeśli `IpaPackageDir` jest ustawiona w lokalizacji niestandardowej, **.ipa** pliku zostaną umieszczone w tej lokalizacji, zamiast domyślnej podkatalogu oznaczony znacznikiem czasowym. Może to być przydatne, gdy tworzenie zautomatyzowanych kompilacje, które korzystają z określonej ścieżki do katalogu działał prawidłowo, takich jak używany dla kompilacji ciągłej integracji (CI).

Istnieje kilka sposobów możliwych do użycia nowej właściwości:

Na przykład, aby dane wyjściowe **.ipa** pliku starego domyślny katalog (tak jak Xamarin.iOS 9.6 i niżej), można ustawić `IpaPackageDir` właściwości `$(OutputPath)` przy użyciu jednej z poniższych metod. W obu przypadkach efekt są zgodne z wszystkich kompilacjach Xamarin.iOS interfejsu API Unified kompilacje IDE w tym również używających kompilacji wiersza polecenia **xbuild**, **msbuild**, lub **mdtool**:

- Pierwsza opcja jest skonfigurowanie `IpaPackageDir` właściwości w `<PropertyGroup>` element **MSBuild** pliku. Na przykład można dodać następujące `<PropertyGroup>` do dołu projekt aplikacji systemu iOS **.csproj** pliku (tylko przed tagiem zamykającym `</Project>` tagu):

    ```xml
    <PropertyGroup>
        <IpaPackageDir>$(OutputPath)</IpaPackageDir>
    </PropertyGroup>
    ```

- Lepszym rozwiązaniem jest dodanie `<IpaPackageDir>` element w dół istniejącej `<PropertyGroup>` odpowiadający konfiguracji, używany do tworzenia **.ipa** pliku. Jest to lepszą, ponieważ przygotuje projektu dla zgodności w przyszłości planowane ustawienia na stronie właściwości projektu iOS IPA opcje. Jeśli obecnie używasz `Release|iPhone` Konfiguracja kompilacji **.ipa** pliku, grupa pełną zaktualizowane właściwości mogą wyglądać podobnie do następujących:

    ```xml
    <PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Release|iPhone' ">
        <Optimize>true</Optimize>
        <OutputPath>bin\iPhone\Release</OutputPath>
        <ErrorReport>prompt</ErrorReport>
        <WarningLevel>4</WarningLevel>
        <ConsolePause>false</ConsolePause>
        <CodesignKey>iPhone Developer</CodesignKey>
        <MtouchUseSGen>true</MtouchUseSGen>
        <MtouchUseRefCounting>true</MtouchUseRefCounting>
        <MtouchFloat32>true</MtouchFloat32>
        <CodesignEntitlements>Entitlements.plist</CodesignEntitlements>
        <MtouchLink>SdkOnly</MtouchLink>
        <MtouchArch>;ARMv7, ARM64</MtouchArch>
        <MtouchHttpClientHandler>HttpClientHandler</MtouchHttpClientHandler>
        <MtouchTlsProvider>Default</MtouchTlsProvider>
        <PlatformTarget>x86&</PlatformTarget>
        <BuildIpa>true</BuildIpa>
        <IpaPackageDir>$(OutputPath</IpaPackageDir>
    </PropertyGroup>
    ```

Alternatywne technika **msbuild** lub **xbuild** kompilacji wiersza polecenia jest dodanie `/p:` argument wiersza polecenia, aby ustawić `IpaPackageDir` właściwości. W takim przypadku należy pamiętać, że **msbuild** nie zwiększa `$()` wyrażenia przekazano w wierszu polecenia, więc nie można użyć `$(OutputPath)` składni. Zamiast tego Podaj pełną nazwę ścieżki. W mono **xbuild** polecenie Rozwiń `$()` wyrażenia, ale jest nadal lepiej używać pełnej nazwy ścieżki, ponieważ **xbuild** po pewnym czasie zostaną wycofane uzyskać [ Wersja i platform **msbuild** ](http://www.mono-project.com/docs/about-mono/releases/4.4.0/#msbuild-preview-for-os-x) w przyszłych wersjach.

Pełny przykład, który korzysta z tej metody może wyglądać podobnie jak poniżej w systemie Windows:


```bash
msbuild /p:Configuration="Release" /p:Platform="iPhone" /p:ServerAddress="192.168.1.3" /p:ServerUser="macuser" /p:IpaPackageDir="%USERPROFILE%\Builds" /t:Build SingleViewIphone1.sln
```

Lub następujące dla komputerów Mac:

```bash
xbuild /p:Configuration="Release" /p:Platform="iPhone" /p:IpaPackageDir="$HOME/Builds" /t:Build SingleViewIphone1.sln
```

<a name="installipa" />

## <a name="installing-an-ipa-using-itunes"></a>Instalowanie IPA za pomocą programu iTunes

Wynikowy pakiet IPA może dostarczyć użytkownikom testu do instalowania na urządzeniach z systemem iOS lub wysłane wdrażania w przedsiębiorstwie. Niezależnie od tego, która metoda zostanie wybrana, użytkownik końcowy zostanie zainstalowany pakiet w aplikacjach iTunes na komputerze z systemem Windows lub Mac klikając dwukrotnie plik IPA pliku (lub przeciągając na oknie Otwórz program iTunes).

Nowa aplikacja systemu iOS jest wyświetlany w **Moje aplikacje** sekcji, w którym kliknij prawym przyciskiem myszy na nim i uzyskiwanie informacji o aplikacji:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

 ![](ipa-support-images/installxs01.png "Nowa aplikacja systemu iOS w sekcji Moje aplikacje")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

 ![](ipa-support-images/installvs01.png "Nowa aplikacja systemu iOS w sekcji Moje aplikacje")

-----

Użytkownik mogą teraz synchronizować iTunes ze swoimi urządzeniami, aby zainstalować nową aplikację systemu iOS.

<a name="Summary" />

## <a name="summary"></a>Podsumowanie

W tym artykule opisano ustawienia wymagane w celu przygotowania aplikacji platformy Xamarin.iOS dla kompilacji - App Store. Pokazano, jak utworzyć pakiet IPA, a ma sposób instalowania wynikowy iOS aplikacji na urządzeniu z systemem iOS przez użytkownika końcowego do testowania lub wewnętrznych dystrybucji.


## <a name="related-links"></a>Linki pokrewne

- [Dystrybucja w sklepie App Store](~/ios/deploy-test/app-distribution/app-store-distribution/index.md)
- [Konfigurowanie aplikacji w iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)
- [Publikowanie w sklepie App Store](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)
- [Dystrybucji wewnętrznych](~/ios/deploy-test/app-distribution/in-house-distribution.md)
- [Dystrybucja Ad Hoc](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md)
- [Plik iTunesMetadata.plist](~/ios/deploy-test/app-distribution/itunesmetadata.md)
- [Rozwiązywanie problemów](~/ios/deploy-test/troubleshooting.md)
- [iTunes kompozycji](~/ios/app-fundamentals/images-icons/app-icons.md#itunes)
- [Dystrybucję aplikacji przedsiębiorstwa dla urządzeń z systemem iOS](http://developer.apple.com/library/ios/#featuredarticles/FA_Wireless_Enterprise_App_Distribution/Introduction/Introduction.html)
