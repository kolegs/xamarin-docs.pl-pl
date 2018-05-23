---
title: Para Mac
description: W tym przewodniku informacje dotyczące używania pary Mac nawiązywania połączenia z hostem kompilacji Mac Visual Studio 2017 r.
ms.prod: xamarin
ms.assetid: 39DD7B3F-3E69-4E2A-B743-4C26AF613025
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/16/2018
ms.openlocfilehash: e2f9b23bb298b0bb01f7e5491963daed4521ac9c
ms.sourcegitcommit: 9f8e7393019791bbd6af4fefaa24a1602adabb4e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/23/2018
---
# <a name="pair-to-mac"></a>Para Mac

_W tym przewodniku informacje dotyczące używania pary Mac nawiązywania połączenia z hostem kompilacji Mac Visual Studio 2017 r._

## <a name="overview"></a>Omówienie

Tworzenie aplikacji systemu iOS natywnych wymaga dostępu do narzędzi kompilacji firmy Apple, które uruchamiane tylko na komputerach Mac. W związku z tym Visual Studio 2017 musi połączyć się z dostępne w sieci Mac do tworzenia aplikacji platformy Xamarin.iOS.

W programie Visual Studio 2017 pary funkcji Mac odnajduje, łączy się z jest uwierzytelniany w usłudze i pamięta hostów kompilacji Mac, dzięki czemu deweloperzy systemu iOS może efektywnie działać. 

Para Mac umożliwia poniższy przepływ pracy tworzenia:

- Programiści mogą pisać kod platformy Xamarin.iOS w programie Visual Studio 2017 r.

- Visual Studio 2017 otwiera połączenie sieciowe z hosta kompilacji Mac i używa narzędzia kompilacji na tej maszynie do Skompiluj i podpisz aplikację systemu iOS.

- Nie istnieje potrzeba do uruchamiania innej aplikacji dla komputerów Mac — Visual Studio 2017 wywołuje kompilacje Mac bezpieczny sposób za pomocą protokołu SSH.

- Visual Studio 2017 jest powiadamiany o zmianach, natychmiast po ich wprowadzeniu. Na przykład gdy urządzenia z systemem iOS jest podłączone do komputera Mac lub stanie się dostępny w sieci, systemu iOS narzędzi aktualizuje natychmiast.

- Wiele wystąpień programu Visual Studio 2017 mogą łączyć się z Mac jednocześnie.

- Istnieje możliwość używania wiersza polecenia systemu Windows do tworzenia aplikacji systemu iOS.

> [!NOTE]
> Przed wykonaniem instrukcji zawartych w tym przewodniku, wykonaj następujące czynności: 
> 
> - Na komputerze z systemem Windows [zainstalować program Visual Studio 2017 r.](~/cross-platform/get-started/installation/windows.md)
> - Na komputerze Mac [Zainstaluj program Xcode](https://itunes.apple.com/us/app/xcode/id497799835?mt=12) i [programu Visual Studio dla komputerów Mac](https://docs.microsoft.com/visualstudio/mac/installation)
>
> Jeśli użytkownik nie chce zainstalować program Visual Studio dla komputerów Mac, Visual Studio 2017 może automatycznie skonfigurować hosta kompilacji Mac z Xamarin.iOS i Mono.
> Aby uzyskać więcej informacji, zobacz [Mac automatycznego inicjowania obsługi administracyjnej](#automatic-mac-provisioning).

## <a name="enable-remote-login-on-the-mac"></a>Włączanie logowania zdalnego dla komputerów Mac

Aby skonfigurować hosta kompilacji Mac, należy najpierw włączyć logowania zdalnego:

1. Dla komputerów Mac, otwórz preferencjach systemowych i przejdź do **udostępniania** okienka.

2. Sprawdź **logowania zdalnego** w **usługi** listy.

    ![Włączanie logowania zdalnego](images/sharing.png "Włączanie logowania zdalnego")

    Upewnij się, że jest skonfigurowana, aby zezwolić na dostęp **wszyscy użytkownicy**, lub że Mac nazwa użytkownika lub grupy znajduje się na liście dozwolone użytkowników.

3. Jeśli zostanie wyświetlony monit, należy skonfigurować zaporę macOS.

    Jeśli ustawisz macOS zapory do blokowania połączeń przychodzących, konieczne może być Zezwalaj `mono-sgen` do odbierania przychodzących połączeń. Monitowanie, jeśli jest to możliwe, zostanie wyświetlony alert.

4. Jeśli jest on w tej samej sieci co maszyny z systemem Windows, Mac teraz powinno być wykrywalny przez Visual Studio 2017 r. Jeśli nadal nie wykrywalny Mac, spróbuj [ręczne dodanie Mac](#manually-add-a-mac) lub Przyjrzyjmy się [przewodnik rozwiązywania problemów](~/ios/get-started/installation/windows/connecting-to-mac/troubleshooting.md).

## <a name="connect-to-the-mac-from-visual-studio-2017"></a>Połącz do komputera Mac z programu Visual Studio 2017 r.

Teraz tego zdalnego logowania jest włączona, Visual Studio 2017 połączyć się z komputerów Mac.

1. W Visual Studio 2017 r, otwórz istniejący projekt dla systemu iOS lub Utwórz nową, wybierając **Plik > Nowy > Projekt** i wybierając szablon projektu systemu iOS.

2. Otwórz **pary Mac** okna dialogowego. 

    - Użyj **pary Mac** przycisk narzędzi dla systemu iOS:

        ![Pasek narzędzi dla systemu iOS z pary podświetlony przycisk Mac](images/ios-toolbar.png "paska narzędzi dla systemu iOS z pary podświetlony przycisk Mac")

    - Lub wybierz **Narzędzia > iOS > pary Mac**.

    - **Pary Mac** okna dialogowego zostanie wyświetlona lista wszystkich wcześniej połączona i aktualnie dostępnych hostów Mac w kompilacji:

        ![Para do okna dialogowego Mac](images/pairtomac.png "pary do okna dialogowego Mac")

3. Wybierz z listy Mac. Kliknij przycisk **Połącz**. 

4. Wprowadź nazwę użytkownika i hasło.
    
    - Po raz pierwszy nawiązywane żadnych szczególnych Mac, zostanie wyświetlony monit o wprowadzenie nazwy użytkownika i hasła dla tego komputera:

        ![Wprowadzanie nazwy użytkownika i hasła dla komputerów Mac](images/auth.png "wprowadzania nazwy użytkownika i hasła dla komputerów Mac")

        > [!TIP]
        > Podczas logowania, użyj Twojej nazwy użytkownika systemu zamiast pełnej nazwy.

    - Para Mac używa tych poświadczeń, aby utworzyć nowe połączenie SSH komputerów Mac. Jeśli próba powiedzie się, klucz zostanie dodany do **authorized_keys** plików na komputerach Mac. Kolejnych połączeń z tym samym adresem MAC zostanie logowania automatycznego.

5. Para Mac automatycznie konfiguruje komputerów Mac.

    [Począwszy od programu Visual Studio 2017 wersji 15,6](https://docs.microsoft.com/visualstudio/releasenotes/vs2017-relnotes#automatic-macos-provisioning)i Visual Studio 2017 instaluje lub aktualizuje Mono Xamarin.iOS na komputerze Mac w podłączonych kompilacji hosta jako potrzebne (należy pamiętać, że Xcode musi nadal należy zainstalować ręcznie). Zobacz [Mac automatycznego inicjowania obsługi administracyjnej](#automatic-mac-provisioning) więcej szczegółów.

6. Poszukaj ikona stanu połączenia.
    
    - Po Visual Studio 2017 jest podłączony do komputera Mac, że Mac elementu w **pary Mac** okno dialogowe wyświetla ikonę wskazującą, czy jest aktualnie połączony:

        ![A połączone Mac](images/connected.png "A połączone Mac")

      Jednocześnie może istnieć tylko jeden Mac w podłączonych.

      > [!TIP]
      > Prawym przyciskiem myszy dowolnego Mac w **pary Mac** listy powoduje wyświetlenie menu kontekstowego, która pozwala na **połączenia...** , **Zapomnij tego Mac**, lub **rozłączyć**:
      >
      > ![Para do menu kontekstowe Mac](images/contextmenu.png "pary Mac menu kontekstowe") 
      >
      > Jeśli wybierzesz **zapomnij Mac to**, poświadczenia dla wybranych komputerów Mac będzie zapomnienia hasła. Ponownie nawiązać połączenie z tym Mac, należy ponownie wprowadzić nazwę użytkownika i hasło.

Jeśli zostały pomyślnie łączyć się z hostem kompilacji Mac, można przystąpić do tworzenia aplikacji platformy Xamarin.iOS w programie Visual Studio 2017 r. Spójrz na [wprowadzenie do platformy Xamarin.iOS dla programu Visual Studio](~/ios/get-started/installation/windows/introduction-to-xamarin-ios-for-visual-studio.md) przewodnik.

Jeśli nie zostały jeszcze można skojarzyć Mac, spróbuj [ręczne dodanie Mac](#manually-add-a-mac) lub Przyjrzyjmy się [przewodnik rozwiązywania problemów](~/ios/get-started/installation/windows/connecting-to-mac/troubleshooting.md).

## <a name="manually-add-a-mac"></a>Ręcznie Dodaj Mac

Jeśli nie widzisz konkretnej Mac, na liście **pary Mac** okna dialogowego, dodaj ją ręcznie:

1. Zlokalizuj komputera Mac, adres IP. 

    - Otwórz **preferencjach systemowych > Udostępnianie > logowania zdalnego** na komputerze Mac:

        [![Adres Mac w preferencjach systemowych > Udostępnianie](images/sharing-ipaddress.png "adres Mac w preferencjach systemowych > udostępniania")](images/sharing.png#lightbox)

    - Można również użyć wiersza polecenia. W terminalu wydaj polecenie: 
   
        ```bash
        $ ipconfig getifaddr en0
        196.168.1.8
        ```

      W zależności od konfiguracji sieci, konieczne może być inny niż Użyj nazwy interfejsu `en0`. Na przykład: `en1`, `en2`itp.

2. W w programie Visual Studio 2017 **pary Mac** okno dialogowe, wybierz opcję **dodać Mac...** :

    [![Przycisk Dodaj Mac w parze do okna dialogowego Mac](images/addtomac.png "przycisku Dodaj Mac w parze do okna dialogowego Mac")](images/addtomac-large.png#lightbox)

3. Wprowadź adres Mac, a następnie kliknij przycisk **Dodaj**:

    ![Wprowadzenie adresu IP Mac](images/enteripaddress.png "wprowadzania adresu Mac")

4. Wprowadź nazwę użytkownika i hasło dla komputerów Mac:

    ![Wprowadź nazwę użytkownika i hasło](images/auth.png "wprowadzania nazwy użytkownika i hasła")

   > [!TIP]
   > Podczas logowania, użyj Twojej nazwy użytkownika systemu zamiast pełnej nazwy.

5. Kliknij przycisk **logowania** nawiązać Visual Studio 2017 Mac za pomocą protokołu SSH i dodaj go do listy znanych maszyn.

## <a name="automatic-mac-provisioning"></a>Automatyczne inicjowanie obsługi komputerów Mac

Począwszy od [programu Visual Studio 2017 wersji 15,6](https://docs.microsoft.com/visualstudio/releasenotes/vs2017-relnotes#automatic-macos-provisioning), pary Mac automatycznie inicjuje Mac z oprogramowanie niezbędne do tworzenia aplikacji platformy Xamarin.iOS: Mono, Xamarin.iOS (oprogramowania framework, nie programu Visual Studio for Mac IDE ) oraz różne narzędzia związane z Xcode (ale nie Xcode, sam).

> [!IMPORTANT]
> - Para Mac nie może zainstalować Xcode; należy ręcznie zainstalować na hoście Mac kompilacji. Jest wymagane do tworzenia aplikacji platformy Xamarin.iOS.
> - Automatyczne udostępnianie Mac wymaga logowania zdalnego jest włączone dla komputerów Mac, czy Mac musi być dostępne w sieci z komputerem z systemem Windows. Zobacz [Włączanie logowania zdalnego dla komputerów Mac](#enable-remote-login-on-the-mac) więcej szczegółów.

Para Mac wykonuje instalacji/aktualizacje oprogramowania niezbędne w przypadku programu Visual Studio 2017 [połączenie z komputerem Mac](#connect-to-the-mac-from-visual-studio-2017).

### <a name="mono"></a>Mono

Para Mac sprawdzi, aby upewnić się, że zainstalowano Mono. Jeśli nie jest zainstalowany, pary Mac pobierze i zainstaluje najnowsza stabilna wersja Mono na komputerach Mac. 

Postęp jest określane przez różnych monitów, jak to przedstawiono na poniższych zrzutach ekranu (kliknij, aby powiększyć):

||Zainstaluj wyboru|Pobieranie|Instalowanie programu
|---|---|---|---|
|Mono|[![Brak Mono instalacji](images/mono-missing.png "Brak Mono instalacji")](images/mono-missing-large.png#lightbox)|[![Pobieranie Mono](images/mono-downloading.png "pobieranie Mono")](images/mono-downloading-large.png#lightbox)|[![Instalowanie Mono](images/mono-installing.png "instalowanie Mono")](images/mono-installing-large.png#lightbox)|

### <a name="xamarinios"></a>Xamarin.iOS 

Para Mac uaktualnia Xamarin.iOS dla komputerów Mac, aby dopasować wersja zainstalowana na komputerze z systemem Windows.

> [!IMPORTANT]
> Para Mac nie będzie starszą wersję platformy Xamarin.iOS na Mac z alfa/wersji beta do stabilnego. Jeśli masz program Visual Studio dla komputerów Mac, Ustaw użytkownika [kanału wersji](https://docs.microsoft.com/visualstudio/mac/update) w następujący sposób:
> - Jeśli używasz programu Visual Studio 2017 wybierz **stabilna** kanału aktualizacji w programie Visual Studio dla komputerów Mac.
> - Jeśli używasz programu Visual Studio 2017 podglądu, wybierz **alfa** kanału aktualizacji w programie Visual Studio dla komputerów Mac.

Postęp jest określane przez różnych monitów, jak to przedstawiono na poniższych zrzutach ekranu (kliknij, aby powiększyć):

||Zainstaluj wyboru|Pobieranie|Instalowanie programu
|---|---|---|---|
|Xamarin.iOS|[![Brak instalacji Xamarin.iOS](images/xamios-missing.png "instalacji brakuje platformy Xamarin.iOS")](images/xamios-missing-large.png#lightbox)|[![Pobieranie Xamarin.iOS](images/xamios-downloading.png "pobieranie platformy Xamarin.iOS")](images/xamios-downloading-large.png#lightbox)|[![Instalowanie platformy Xamarin.iOS](images/xamios-installing.png "instalowanie platformy Xamarin.iOS")](images/xamios-installing-large.png#lightbox)|

### <a name="xcode-tools-and-license"></a>Narzędzia Xcode i licencji

Para Mac sprawdza również określić, czy zainstalowano Xcode i zaakceptowane jej licencji. Gdy pary Mac nie można zainstalować Xcode, jak pokazano na poniższych zrzutach ekranu (kliknij, aby powiększyć) Monituj o akceptacji licencji:

||Zainstaluj wyboru|Akceptacja licencji|
|---|---|---|
|Xcode|[![Brak instalacji Xcode](images/xcode-missing.png "instalacji brak Xcode")](images/xcode-missing-large.png#lightbox)|[![Licencja Xcode](images/xcode-license.png "Xcode licencji")](images/xcode-license-large.png#lightbox)|

Ponadto pary Mac będzie Zainstaluj lub zaktualizuj różnych pakietów rozproszonych w programie Xcode. Na przykład:

- **MobileDeviceDevelopment.pkg**
- **XcodeExtensionSupport.pkg**
- **MobileDevice.pkg**
- **XcodeSystemResources.pkg**

Instalację tych pakietów odbywa się szybko i bez monitu.

> [!NOTE]
> Te narzędzia są różne od Xcode narzędzi wiersza polecenia, które od macOS 10.9 są [zainstalowane z Xcode](https://developer.apple.com/library/content/technotes/tn2339/_index.html).

### <a name="troubleshooting-automatic-mac-provisioning"></a>Rozwiązywanie problemów z automatycznego inicjowania obsługi komputerów Mac

Jeśli wystąpią problemy ze przy użyciu automatycznego inicjowania obsługi komputerów Mac, zapoznaj się w dziennikach programu Visual Studio IDE 2017 przechowywane w **%LOCALAPPDATA%\Xamarin\Logs\15.0**. Te dzienniki mogą zawierać komunikaty o błędach ułatwiających lepsze diagnozowania awarii lub uzyskać pomoc techniczną.

## <a name="build-ios-apps-from-the-windows-command-line"></a>Tworzenie aplikacji systemu iOS z wiersza polecenia systemu Windows 

Para Mac obsługuje tworzenie aplikacji platformy Xamarin.iOS z wiersza polecenia. Na przykład:

```bash
C:\samples\App1>msbuild App1.sln /p:ServerAddress=10.211.55.2 /p:ServerUser=xamUser /p:Platform=iPhoneSimulator /p:ServerPassword=mypassword
```
Parametry przekazywane do `msbuild` w powyższym przykładzie są:

- `ServerAddress` — Adres IP hosta Mac kompilacji.
- `ServerUser` — Nazwa użytkownika używana podczas logowania do hosta kompilacji Mac.
  Użyj nazwy użytkownika systemu nie swoje imię i nazwisko.
- `ServerPassword` — Hasło do użycia podczas logowania się do hosta Mac kompilacji.
 
> [!NOTE]
> Visual Studio 2017 przechowuje `msbuild` w następującym katalogu: **\Microsoft Visual Studio\2017 C:\Program Files (x86)\<wersji > \MSBuild\15.0\Bin**

Para Mac loguje się do określonego hosta kompilacji Mac z programu Visual Studio 2017 lub wiersza polecenia, po raz pierwszy konfiguruje kluczy SSH. Z tych kluczy przyszłych logowania nie wymagają nazwy użytkownika i hasła. Nowo utworzony klucze są przechowywane w **%LOCALAPPDATA%\Xamarin\MonoTouch**.

Jeśli `ServerPassword` parametr zostanie pominięty z wywołania kompilacji wiersza polecenia, pary Mac próbuje zalogować się do hosta kompilacji Mac przy użyciu zapisanych kluczy SSH.

## <a name="summary"></a>Podsumowanie

W tym artykule opisano Visual Studio 2017 nawiązać połączenia z hostem kompilacji Mac włączenie programu Visual Studio 2017 deweloperom tworzenie aplikacji natywnej systemu iOS za pomocą platformy Xamarin.iOS przy użyciu pary Mac.

## <a name="next-steps"></a>Następne kroki

- [Rozwiązywanie problemów z połączeniem](~/ios/get-started/installation/windows/connecting-to-mac/troubleshooting.md)
- [Xamarin Mac wykładu Lightning Xamarin University agenta - kompilacji](https://www.youtube.com/watch?v=MBAPBtxkjFQ)
- [Wprowadzenie do rozwiązania Xamarin.iOS dla programu Visual Studio](~/ios/get-started/installation/windows/introduction-to-xamarin-ios-for-visual-studio.md)
- [Zdalny iOS Simulator dla systemu Windows](~/tools/ios-simulator.md)
- [Wdrażanie bezprzewodowe](~/ios/deploy-test/wireless-deployment.md)

