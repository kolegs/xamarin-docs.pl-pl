---
title: Instalowanie platformy Xamarin.iOS w systemie Windows
description: "W tym artykule przedstawiono sposób konfigurowania Xamarin.iOS dla programu Visual Studio. Obejmuje proces instalacji rozszerzenia platformy Xamarin dla Visual Studio i omówiono nawiązywania połączenia SDK Apple zainstalowane na komputerach Mac."
ms.topic: article
ms.prod: xamarin
ms.assetid: abf85d3e-a365-44a2-b1a4-6c572c7f76dd
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/29/2017
ms.openlocfilehash: 08bf8b2b7c56983c43cf1ae080ab112e81851fbb
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/22/2018
---
# <a name="installing-xamarinios-on-windows"></a>Instalowanie platformy Xamarin.iOS w systemie Windows

_W tym artykule przedstawiono sposób konfigurowania Xamarin.iOS dla programu Visual Studio. Obejmuje proces instalacji rozszerzenia platformy Xamarin dla Visual Studio i omówiono nawiązywania połączenia SDK Apple zainstalowane na komputerach Mac._

## <a name="overview"></a>Omówienie

Xamarin.iOS dla programu Visual Studio umożliwia iOS aplikacje mają być zapisywane i przetestowany na komputerach z systemem Windows z sieci Mac świadczenie usług kompilacji i wdrożenia.

Tworzenie aplikacji dla systemu iOS w programie Visual Studio umożliwia:

- Tworzenie rozwiązań i platform dla systemu iOS, Android i Windows aplikacji.
- Za pomocą narzędzi Visual Studio (takich jak *Resharper* i *Team Foundation Server*) dla wszystkich projektów i platform, łącznie z kodu źródłowego z systemem iOS.
- Praca z znanych IDE, wykorzystując powiązania Xamarin.iOS Apple wszystkich interfejsów API

Xamarin.iOS dla programu Visual Studio obsługuje konfiguracje, których Visual Studio działa wewnątrz maszyny wirtualnej systemu Windows na komputerze Mac (przy użyciu rozwiązań równoległych lub VMWare) lub gdy nie znajduje się na osobnym komputerze, na którym jest widoczna w tej samej sieci co Mac. Niezależnie od tego, która Konfiguracja jest najbardziej odpowiedni Visual Studio będzie łączyć się Mac szybko i bezpiecznie przy użyciu protokołu SSH.

W tym artykule opisano kroki, aby zainstalować i skonfigurować narzędzia Xamarin.iOS na komputerze Mac i systemu Windows, jak również kroki, aby połączyć się z hostem Mac, dzięki czemu deweloperzy mogą tworzenia, debugowania i wdrażania aplikacji platformy Xamarin.iOS przy użyciu programu Visual Studio.

Na poniższym diagramie przedstawiono proste omówienie przepływu pracy programowania Xamarin.iOS:

[![Przepływ pracy tworzenia platformy Xamarin.iOS](images/xma2.png)](images/xma2.png#lightbox)

> [!IMPORTANT]
> Visual Studio faktycznie uruchamia oddzielnych procesach MSBuild do tworzenia projektów. Ten proces tworzy nowe połączenie do komputera Mac, co oznacza, że są faktycznie dwóch połączeń SSH z systemu Windows do komputera Mac, w kompilacji programu Visual Studio. Kompilowanie z [wiersza polecenia](~/ios/get-started/installation/windows/connecting-to-mac/index.md) tworzy tylko jednego procesu programu MSBuild. Dla uproszczenia ten diagram wszystkie połączenia są po prostu reprezentowane przez jedną strzałkę.

## <a name="requirements"></a>Wymagania

Xamarin.iOS dla programu Visual Studio umożliwia zrealizowanie niesamowite feat: umożliwia deweloperom tworzenie, kompilacji i debugowanie aplikacji systemu iOS na komputerze z systemem Windows przy użyciu programu Visual Studio IDE. Nie można wykonać tego samego — nie można utworzyć aplikacji systemu iOS bez kompilatora firmy Apple i nie można wdrożyć bez certyfikaty i narzędzia podpisywania kodu firmy Apple. Oznacza to, że Xamarin.iOS dla instalacji programu Visual Studio wymaga połączenia sieciowe komputer Mac OS X, aby wykonać te zadania. Po skonfigurowaniu narzędzi dla platformy Xamarin spowoduje, że proces jako bezproblemowe, jak to możliwe.


<a name="system-requirements"/>

### <a name="system-requirements"></a>Wymagania systemowe

Wymagania systemowe są:

### <a name="windows"></a>Windows

1. Windows 7 lub nowszy.

2. Program Visual Studio Professional 2015 lub nowszego

    a. Jeśli użytkownik ma licencję Enterprise, należy zainstalować program Visual Studio Enterprise.

3. Xamarin dla Visual Studio.

Narzędzia platformy Xamarin nie można z wersji Express programu Visual Studio z powodu braku obsługi wtyczki. Xamarin jest obsługiwane w Visual Studio Community.

### <a name="mac"></a>Mac

1. Mac macOS uruchomionych Sierra (10.12) lub nowszy (choć zalecana jest najnowsza stabilna wersja).

2. Xamarin iOS SDK. Jest instalowana podczas pobierania programu Visual Studio dla komputerów Mac

3. Xcode IDE i iOS SDK (zalecany jest najnowsza stabilna wersja ze sklepu Mac App Store) firmy Apple.

**Komputer z systemem Windows musi być możliwe nawiązanie łączności Mac za pośrednictwem sieci.**

### <a name="apple-developer-account"></a>Konto dewelopera firmy Apple

Aby wdrożyć aplikacje na urządzeniu lub przesyłanie ich do sklepu z aplikacjami, wymagane jest konto deweloperów firmy Apple. Developer odpowiednie certyfikaty i profile inicjowania obsługi administracyjnej należy utworzyć i zainstalowane na sieciowym Mac przed Xamarin.iOS dla programu Visual Studio może współpracować. Zobacz [Inicjowanie obsługi administracyjnej urządzeń](~/ios/get-started/installation/device-provisioning/index.md) artykułu kroki do uzyskania certyfikatu deweloperskiego i udostępnić urządzeniu.

## <a name="features"></a>Funkcje 

Xamarin.iOS dla programu Visual Studio umożliwia tworzenie, edytowania, tworzenia i wdrażania platformy Xamarin.iOS projektów z systemu Windows. Obejmuje to następujące funkcje:

- Utwórz nowe projekty dla systemu iOS.

- Edytuj iOS projektów i rozwiązań i platform, które również obejmować projektów platformy Xamarin.Android i platformy uniwersalnej systemu Windows.

- Kompilowanie projektów dla systemu iOS i rozwiązań i platform, które również obejmować projektów platformy Xamarin.Android i platformy uniwersalnej systemu Windows.

- Scenorysu i .xib obsługuje przy użyciu projektanta dla systemu iOS.

- Wdrażanie i debugowanie aplikacji systemu iOS, w którym aplikacja jest uruchomiona w symulatorze lub na urządzenie podłączone do komputerów Mac.

- symulatora systemu iOS w systemie Windows — Aby uzyskać więcej informacji na temat używania symulatora systemu iOS w systemie Windows, zapoznaj się [w tym przewodniku](~/tools/ios-simulator.md).

<a name="configuring" />

## <a name="configuring-your-mac"></a>Konfigurowanie komputerów Mac

<a name="installation"/>

### <a name="installation"></a>Instalacja

Aby zainstalować narzędzia platformy Xamarin.iOS na hoście z systemem mac należy [zainstalować program Visual Studio for Mac](https://docs.microsoft.com/visualstudio/mac/installation).

Po zainstalowaniu oprogramowania, wykonaj kroki opisane w kolejnych sekcjach, aby skonfigurować Xamarin.iOS na macOS umożliwia Xamarin dla Visual Studio, aby się z nim połączyć.

> [!IMPORTANT]
> Komputer z systemem Windows muszą używać tej samej wersji platformy Xamarin.iOS jako Mac, do którego jest podłączony. Aby to zapewnić jest spełnionych:
>
> - **Visual Studio 2015 lub starszym**: Upewnij się, że jesteś w tym samym [kanału aktualizacji](https://developer.xamarin.com/recipes/cross-platform/ide/change_updates_channel/) co program Visual Studio dla komputerów Mac.
>
> - **Visual Studio 2017, wersja wydana**: Upewnij się, że znajdują się na **stabilna** kanału programu Visual Studio dla komputerów Mac.
>
> - **Visual Studio 2017, wersja zapoznawcza**: Upewnij się, że znajdują się na **alfa** kanału programu Visual Studio dla komputerów Mac.

<a name="configuration" />


### <a name="configuration"></a>Konfiguracja

Aby uzyskać dostęp do komunikacji między rozszerzenie Xamarin dla Visual Studio i komputerów Mac, musisz zezwolić **logowania zdalnego** opartym na systemie Wykonaj poniższe kroki, aby go skonfigurować:

1. Otwórz *Spotlight* (**Cmd miejsca**) i wyszukaj **logowania zdalnego** , a następnie wybierz **udostępniania** wynik. Spowoduje to otwarcie **preferencjach systemowych** w **udostępniania** panelu.

![Zwracanie przez wyszukiwanie Spotlight do logowania zdalnego](images/spotlight.png)

2. Znaczników **logowania zdalnego** opcji **usługi** liście z lewej strony, aby umożliwić Xamarin dla Visual Studio, aby nawiązać połączenie komputerów Mac.

![Opcja logowania zdalnego na liście usług znaczników](images/sharing.png)

3. Upewnij się, że **logowania zdalnego** jest ustawiona, aby umożliwić dostęp do **wszyscy użytkownicy**, lub że Mac nazwa użytkownika lub grupy znajduje się na liście dozwolone użytkowników na liście po prawej stronie.

Mac teraz powinno być wykrywalny przez program Visual Studio, jeśli znajduje się w tej samej sieci.

> [!NOTE]
> Jeśli masz zapory macOS domyślnie ustawiony na blokowanie podpisanych aplikacji, konieczne może być Zezwalaj `mono-sgen` do odbierania przychodzących połączeń. Okna dialogowego alertu pojawi się monit, jeśli jest to możliwe.

<a name="developersetup"/>

### <a name="ios-developer-setup"></a>Konfiguracja dewelopera z systemem iOS

Dla opracowywania aplikacji systemu iOS ważne jest, że komputer Mac konfiguruje się odpowiednie tożsamości podpisywania. Dzięki temu można do prawidłowego podpisania aplikacji, dzięki czemu mogą być dystrybuowane za pośrednictwem sklepu z aplikacjami lub w trybie Ad Hoc. Wykonaj poniższe łącze, aby uzyskać instrukcje dotyczące konfigurowania komputerów Mac na potrzeby opracowywania aplikacji systemu iOS za pomocą platformy Xamarin:

- [Inicjowanie obsługi administracyjnej urządzeń](~/ios/get-started/installation/device-provisioning/index.md?ide=vs)

Po skonfigurowaniu komputera Mac jest czasu na skonfigurowanie komputera z systemem Windows.

<a name="windowsinstallation"/>

## <a name="windows-installation"></a>Instalacja systemu Windows

Xamarin można zainstalować jako część programu Visual Studio 2017 lub 2015 instalacji. Aby zainstalować narzędzia Visual Studio dla platformy Xamarin, zobacz [instalacji systemu Windows](~/cross-platform/get-started/installation/windows.md) przewodnik.

## <a name="installation-complete"></a>Zakończenie instalacji

Po zakończeniu instalacji nadal istnieją kilka więcej czynności wymagane do pracy wszystkie dane:

- [Łączenie programu Visual Studio do komputera Mac](#connectingtomac) — Visual Studio musi być podłączony do hosta kompilacji Mac, zanim można tworzyć projektów platformy Xamarin.iOS.
- [Konfigurowanie narzędzi Visual Studio](#toolbar) — pozwoli to łatwo uzyskiwać dostęp do funkcji Xamarin.iOS w programie Visual Studio.

<a name="connectingtomac" /> 

### <a name="connecting-to-the-mac"></a>Łączenie z adresem MAC

Połączenie jest nawiązywane z platformy Xamarin.iOS dla programu Visual Studio do hosta kompilacji Mac za pomocą połączenia SSH między maszynami. Aby uzyskać więcej informacji na połączenie się [nawiązywania Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md) przewodnik.

Aby połączyć z komputera Mac, wykonaj następujące czynności:

- Przejdź do **Narzędzia > Opcje** i w obszarze **Xamarin** wybierz **ustawienia systemu iOS**:

  [![Ekran ustawień systemu iOS](images/image2.png)](images/image2.png#lightbox)

- Pod warunkiem Mac została prawidłowo [skonfigurowane](#configuration) umożliwia **logowania zdalnego**, powinno być możliwe do komputera Mac wybierz z listy:

  [![Okno dialogowe hosta zdalnego](images/xma3.png)](images/xma3.png#lightbox)

- To wyświetli monit o podanie poświadczeń administracyjnych serwera hosta Mac:

  [![Okno dialogowe logowania](images/xma4.png)](images/xma4.png#lightbox)

- Po połączeniu, wyświetli "Połączenie powiodło się" Ikona obok nazwy komputera:

  [![Okno dialogowe ma zdalnego wyświetlanie pomyślnego połączenia ikony obok nazwy komputera](images/image6.png)](images/image6.png#lightbox)

Zostanie ponownie zawsze po uruchomieniu programu Visual Studio.

<a name="toolbar" />

## <a name="visual-studio-toolbar-configuration"></a>Konfiguracja narzędzi programu Visual Studio

Po otwarciu projektu iOS iOS narzędzi będą widoczne domyślnie i nie musi być skonfigurowany.

Jeśli nie jest wyświetlana na pasku narzędzi z systemem iOS można poniższe kroki.

Do konfigurowania narzędzi pierwszym otwarciu **Widok > Paski narzędzi** menu i upewnij się, że **iOS** wpis jest zaznaczone. Wybierz element menu, jak pokazano w tym zrzucie ekranu pokazano — należy ją zaznaczyć aby wskazać, że jest on widoczny:

[![Wybierz paski narzędzi > iOS](images/image31.png)](images/image31.png#lightbox)

### <a name="visual-studio-2015"></a>Visual Studio 2015

W wersjach starszych niż program Visual Studio 2017 r. **platformy rozwiązania** przycisk może być konieczne do dodania na standardowym pasku narzędzi. Dzięki temu iOS urządzenia lub symulatora systemu iOS, należy wybrać podczas debugowania. Postępuj zgodnie z instrukcjami poniżej, aby go skonfigurować

Kliknij przycisk menu z prawej strony paska standardowe:

- Wybierz **Dodaj lub usuń przyciski**
- Wybierz **platformy rozwiązania**

[![Wybierz platformę rozwiązania](images/image35.png)](images/image35.png#lightbox)

**Standardowe** i **iOS** paski narzędzi powinien teraz wyglądać tego zrzutu ekranu:

[![Paski narzędzi Standardowy i iOS powinien teraz wyglądać tego zrzutu ekranu](images/image36.png)](images/image36.png#lightbox)

Po zakończeniu konfiguracji narzędzi, można przystąpić do rozpoczęcia korzystania z systemu Xamarin iOS dla programu Visual Studio.

## <a name="summary"></a>Podsumowanie

W tym artykule przedstawione przewodnik krok po kroku dotyczący instalowania, konfigurowania i za pomocą platformy Xamarin iOS dla programu Visual Studio.

Obejmuje ona instalowanie i konfigurowanie wymagań wstępnych narzędzia w systemach Windows i Mac OS X.


## <a name="related-links"></a>Linki pokrewne

- [Instalacja](~/cross-platform/get-started/installation/windows.md)
- [Inicjowanie obsługi administracyjnej urządzeń](~/ios/get-started/installation/device-provisioning/index.md)
- [Wprowadzenie do rozwiązania Xamarin.iOS dla programu Visual Studio](~/ios/get-started/installation/windows/introduction-to-xamarin-ios-for-visual-studio.md)
- [Łączenie Mac do środowiska Visual Studio z XMA (klip wideo)](https://university.xamarin.com/lightninglectures/xamarin-mac-agent)
