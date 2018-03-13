---
title: "Łączenie z adresem MAC"
description: "Xamarin.iOS dla programu Visual Studio umożliwia deweloperom tworzenie, kompilacji i debugowanie aplikacji systemu iOS na komputerze z systemem Windows przy użyciu programu Visual Studio IDE. W tym przewodniku opisano dostępne funkcje platformy Xamarin.iOS dla programu Visual Studio i staje się jak połączenia z adresem MAC kompilacji hosta."
ms.topic: article
ms.prod: xamarin
ms.assetid: 39DD7B3F-3E69-4E2A-B743-4C26AF613025
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: c60927593f062c8ac9694d889ffbf581c09bab82
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="connecting-to-the-mac"></a>Łączenie z adresem MAC

_Xamarin.iOS dla programu Visual Studio umożliwia deweloperom tworzenie, kompilacji i debugowanie aplikacji systemu iOS na komputerze z systemem Windows przy użyciu programu Visual Studio IDE. W tym przewodniku opisano dostępne funkcje platformy Xamarin.iOS dla programu Visual Studio i staje się jak połączenia z adresem MAC kompilacji hosta._

Visual Studio łączy się z adresem MAC za pomocą protokołu SSH, co zapewnia kilka korzyści, w tym:

- Visual Studio można uruchomić i bezpośrednio sterowania agenta kompilacji. Brak nie jest już widoczny dla użytkownika aplikacji, która wymaga ręcznego uruchamiania i zatrzymywania.

- Odkrywanie nowych Menedżera połączeń w programie Visual Studio, uwierzytelniania i pamiętaj Mac hosta kompilacji.

- Ponieważ bezpieczny tunel całą komunikację za pośrednictwem protokołu SSH, wymagane jest tylko połączenie jednego portu port 22.

- Visual Studio jest powiadamiany o zmianach, jak wystąpią. Na przykład, gdy urządzenia z systemem iOS jest podłączone na pasku narzędzi spowoduje zaktualizowanie natychmiast.

- Jednocześnie połączyć wiele wystąpień programu Visual Studio.

- Połączenie nie zostanie mającym na programowanie. Go tylko wyświetli monit o połączenie z komputerem Mac podczas wykonywania operacji, dla którego Mac jest wymagane, na przykład debugowania lub przy użyciu projektanta dla systemu iOS.

Połączenie z komputerem Mac składa się z wielu procesów dla różnych części jej funkcji — na przykład agent projektanta systemu iOS i agenta kompilacji —, które są kontrolowane przez brokera. Ta brokera jest kontrolowany i aktualizowane przez program Visual Studio i zostanie automatycznie uruchomiony ponownie żadnego procesów niezależnie od ich awarii.

Na poniższym diagramie przedstawiono proste omówienie przepływu pracy programowania Xamarin.iOS:

[![przepływ pracy programowanie dla systemu iOS](images/xma2.png)](images/xma2.png#lightbox)

> [!IMPORTANT]
>  Visual Studio faktycznie uruchamia oddzielnych procesach MSBuild do tworzenia projektów. Ten proces tworzy nowe połączenie do komputera Mac, co oznacza, że są faktycznie dwóch połączeń SSH z systemu Windows do komputera Mac, w kompilacji programu Visual Studio. Kompilowanie z [wiersza polecenia](#commandline) tworzy tylko jednego procesu programu MSBuild. Dla uproszczenia ten diagram wszystkie połączenia są po prostu reprezentowane przez jedną strzałkę.

## <a name="requirements"></a>Wymagania

Xamarin.iOS dla programu Visual Studio umożliwia zrealizowanie niesamowite feat: umożliwia deweloperom tworzenie, kompilacji i debugowanie aplikacji systemu iOS na komputerze z systemem Windows przy użyciu programu Visual Studio IDE. Nie można wykonać tego samego — nie można utworzyć aplikacji systemu iOS bez kompilatora firmy Apple i nie można wdrożyć bez certyfikaty i narzędzia podpisywania kodu firmy Apple. To oznacza, że Twoje Xamarin.iOS dla instalacji programu Visual Studio wymaga połączenia na komputerach Mac OS X (czyli dalej *hosta* lub *kompilacji hosta*) do wykonywania tych zadań za Ciebie. Po skonfigurowaniu narzędzi dla platformy Xamarin spowoduje, że proces jako bezproblemowe, jak to możliwe.

### <a name="system-requirements"></a>Wymagania systemowe

Wymagania systemowe można znaleźć w [Xamarin.iOS instalowania w systemie Windows](~/ios/get-started/installation/windows/index.md#system-requirements) przewodnik


#### <a name="compatibility"></a>Zgodność

> [!IMPORTANT]
>  Komputer z systemem Windows muszą używać tej samej wersji platformy Xamarin.iOS jako Mac, do którego jest podłączony. Aby to zapewnić jest spełnionych:                                                    
>                                                                                                                 
> - **Visual Studio 2015 lub starszym**: Upewnij się, że jesteś w tym samym [kanału aktualizacji](https://developer.xamarin.com/recipes/cross-platform/ide/change_updates_channel/) co program Visual Studio dla komputerów Mac.
>                                                                                                                 
> - **Visual Studio 2017, wersja wydana**: Upewnij się, że znajdują się na **stabilna** kanału programu Visual Studio dla komputerów Mac.
>                                                                                                                 
> - **Visual Studio 2017, wersja zapoznawcza**: Upewnij się, że znajdują się na **alfa** kanału programu Visual Studio dla komputerów Mac. Program Visual Studio nie będzie sprawdzać, czy zestaw SDK platformy Xamarin.iOS i Xcode istnieje i wersje zgodne.
>   Który będzie sprawdzany przez agenta kompilacji, liczbę błędów kompilacji; i przez agenta projektanta dla systemu iOS, liczbę błędów projektanta.

### <a name="connecting-to-the-mac"></a>Łączenie z adresem MAC

#### <a name="mac-setup"></a>Instalator Mac

Aby skonfigurować hosta Mac, należy włączyć komunikację między rozszerzenie Xamarin dla Visual Studio i z komputerów Mac. Aby to zrobić, należy umożliwić **logowania zdalnego** na komputerze Mac, wykonując następujące czynności:

1. Otwórz *Spotlight* (**miejsca ⌘**) i wyszukaj *logowania zdalnego* , a następnie wybierz *udostępniania* wynik. Spowoduje to otwarcie *preferencjach systemowych* w *udostępniania* panelu:

   [![Zwracanie przez wyszukiwanie Spotlight do logowania zdalnego](images/spotlight.png)](images/spotlight.png#lightbox)

2. Znaczników *logowania zdalnego* opcji *usługi* liście z lewej strony, aby umożliwić Xamarin dla Visual Studio, aby połączyć się z adresem MAC:

   [![Opcja logowania zdalnego na liście usług znaczników](images/sharing.png)](images/sharing.png#lightbox)

3. Upewnij się, że *logowania zdalnego* jest ustawiona, aby umożliwić dostęp do *wszystkie* użytkowników lub że Mac nazwa użytkownika lub grupy znajduje się na liście dozwolonych użytkowników na liście po prawej stronie.

Oprócz tego, jeśli masz zapory OS X, domyślnie ustawione blokowanie podpisanych aplikacji może być konieczne Zezwalaj `mono-sgen` do odbierania przychodzących połączeń. Okna dialogowego alertu pojawi się monit, jeśli jest to możliwe.

Jeśli istnieje sesji bieżącej, Otwórz na komputerze Mac, teraz należy wykrywalny przez program Visual Studio jeśli znajduje się w tej samej sieci.

Visual Studio uruchomi i Zatrzymaj agenta na komputerze Mac, więc nie ma nic, jako użytkownik, potrzebne do uruchomienia.

### <a name="windows-setup"></a>Instalator systemu Windows

Upewnij się, że [zainstalować](~/ios/get-started/installation/windows/index.md) Xamarin narzędzia na komputerze z systemem Windows.

### <a name="connecting-to-the-mac-build-host"></a>Połączenie się z hostem kompilacji Mac

Istnieją dwa sposoby do połączenia z hostem kompilacji Mac:

Na pasku narzędzi z systemem iOS:

[![Na pasku narzędzi z systemem iOS](images/image1.png)](images/image1.png#lightbox)

Lub przechodząc do **Narzędzia > Opcje** w programie Visual Studio, wybierając **Xamarin > Ustawienia systemu iOS** i klikając **znaleźć Xamarin Mac Agent** przycisk:

[![Znajdowanie Xamarin Mac Agent](images/image2.png)](images/image2.png#lightbox)

Nawigacja w obu przypadkach prowadzi do **Mac Agent** okna dialogowego, zostały pokazane poniżej:

[![Okno dialogowe Mac Agent](images/image3.png)](images/image3.png#lightbox)

Spowoduje to wyświetlenie listy wszystkich maszyn, albo został wcześniej podłączany, które są przechowywane jako znanych maszyn lub komputerów, które są dostępne dla *logowania zdalnego*.

Wybierz Mac, klikając dwukrotnie w celu nawiązania połączenia. Przy pierwszym połączeniu Mac, pojawi się monit o podanie poświadczeń użytkownika Mac zezwalały na połączenie zdalne:

[![Wprowadź poświadczenia użytkownika Mac](images/image4.png)](images/image4.png#lightbox)

Agent użyje tych poświadczeń, aby utworzyć nowe połączenie SSH komputerów Mac. Jeśli próba powiedzie się, zostanie utworzony klucz SSH, a musi być [zarejestrowany](#commandline) w `authorized_keys` pliku w tym komputerów Mac. Podczas kolejnych połączeń agent zostanie użyty plik nazwy użytkownika i klucz do nawiązania połączenia najbardziej ostatnio połączony host znane kompilacji.

> [!NOTE]
>  **Uwaga**: należy użyć _username_ i nie _Pełna nazwa_ podczas wprowadzania poświadczeń.  Można to sprawdzić za pomocą `whoami` polecenia w terminalu.  Na przykład w poniższym zrzucie ekranu, nazwa konta będzie **amyb** i nie **oparzenia Amy**:
>
> ![Znajdowanie nazwy użytkownika w terminalu aplikacji](images/image5.png)

Po pomyślnym nawiązaniu połączenia będą wyświetlane w oknie dialogowym Wybór hosta z **połączone** ikona obok niej, jak przedstawiono poniżej:

[![Okno dialogowe wybór hosta z połączonych ikona obok niej](images/image6.png)](images/image6.png#lightbox)

Może istnieć tylko jeden Mac połączonych w dowolnym momencie.

Każdej maszynie, na liście, czy połączenia lub w inny sposób, będą wyświetlane menu kontekstowe na kliknij prawym przyciskiem myszy, umożliwiając **Connect**, **rozłączenia**, lub **zapomnij Mac** jako wymagane:

[![Połącz, odłącz lub zapomnij Mac to menu kontekstowe](images/image7.png)](images/image7.png#lightbox)

Jeśli użytkownik chce **zapomnij Mac to**, musisz ponownie wprowadzić swoje poświadczenia, aby się z nim połączyć ponownie.

<a name="manual-add"/>

### <a name="manually-adding-a-mac"></a>Ręczne dodanie Mac

W pewnych okolicznościach możesz ręcznie dodać Mac, jeśli nie widać nazwy mDNS na liście z okna dialogowego wyboru hosta. Aby to zrobić, wykonaj następujące czynności:

1. Znajdź adres IP komputera Mac przy użyciu albo przeglądania **preferencjach systemowych > Udostępnianie > logowania zdalnego** na komputerze Mac:

   [![Adres Mac w preferencjach systemowych](images/image8.png)](images/image8.png#lightbox)

   Lub, jeśli wolisz korzystać z wiersza polecenia można znaleźć adres IP, wprowadzając `ipconfig getifaddr en0` do terminali (należy pamiętać, że w zależności od typu połączenia zmiennej może być `en1`, `en2` itp.):

   [![Adres IP w terminalu aplikacji](images/image9.png)](images/image9.png#lightbox)

2. Zwracany do programu Visual Studio i w oknie dialogowym Wybór hosta, wybierz opcję **Mac. Dodaj...** :

   [![Okna dialogowego wyboru hosta](images/image10.png)](images/image10.png#lightbox)

3. Wprowadź adres IP należy Mac w oknie dialogowym Dodawanie Mac, a następnie kliknij przycisk **Dodaj**:

   [![Wprowadź adres MAC w oknie dialogowym Dodawanie Mac](images/image11.png)](images/image11.png#lightbox)

4. Na koniec wprowadź nazwę użytkownika (nie pełna nazwa), a odpowiednie hasło konta administratora Mac:

   [![Wprowadź nazwę użytkownika i hasło](images/image12.png)](images/image12.png#lightbox)

Po kliknięciu **logowania**, Visual Studio będzie zalogować się do komputera Mac przy użyciu protokołu SSH i doda to Mac jako znanych maszyny.

<a name="commandline"/>

### <a name="command-line-support"></a>Obsługę wiersza polecenia

Agent obsługuje również tworzenie konfiguracji platformy Xamarin.iOS z wiersza polecenia.  Aby go użyć, należy skonfigurować następujące ustawienia wymagane dla programu MSBuild:

- `ServerAddress` — Adres IP serwera Mac.

- `ServerUser` — Nazwa użytkownika (nie pełna nazwa) używanego do logowania do serwera Mac.

- `ServerPassword` — Hasło używane do logowania do hosta Mac (opcjonalnie).

`ServerPassword` Parametr nie jest wymagany.

Zamiast tego po raz pierwszy przekazano hasła przy użyciu programu Visual Studio lub wiersza polecenia dla tego konkretnego konfiguracji systemu Windows, Mac i użytkownika pary kluczy zostanie wygenerowany i przechowywane na komputerze z systemem Windows do użycia w przyszłości. Będą znajdować się w **%localappdata%\Xamarin\MonoTouch\id_rsa**.  Jeśli nie są przekazywane `ServerPassword` parametru `id_rsa` keyfile będzie używany do uwierzytelniania.

Przykładowe polecenie, aby nawiązać połączenie przy użyciu Mac 10.211.55.2 **xamUser** konta z hasłem **mojehasło** przedstawiono poniżej:

```bash
C:\samples\App1>msbuild App1.sln /p:ServerAddress=10.211.55.2 /p:ServerUser=xamUser /p:Platform=iPhoneSimulator /p:ServerPassword=mypassword
```

### <a name="summary"></a>Podsumowanie

Ten artykuł eksplorowanych połączenie między usługą Visual Studio i narzędzi kompilacji i projektanta dla systemu iOS dla komputerów Mac, co pozwala na tworzenie aplikacji platformy Xamarin.iOS przy użyciu programu Visual Studio.

### <a name="related-links"></a>Linki pokrewne

- [Instalacja](~/ios/get-started/installation/windows/index.md)
- [Rozwiązywanie problemów z połączeniem](~/ios/get-started/installation/windows/connecting-to-mac/troubleshooting.md)
- [Łączenie Mac do środowiska Visual Studio z XMA (klip wideo)](https://university.xamarin.com/lightninglectures/xamarin-mac-agent)
