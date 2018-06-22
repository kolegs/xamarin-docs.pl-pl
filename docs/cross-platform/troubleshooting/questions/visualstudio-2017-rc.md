---
redirect_url: /xamarin/cross-platform/troubleshooting/questions/
title: Za pomocą platformy Xamarin można użyć programu Visual Studio 2017 Release Candidate?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 8E752F36-F73A-4EFC-9F82-4E18FDE1C9E2
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 4c93653793c7c8cce0d2257107097c89f73fc726
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/10/2018
ms.locfileid: "33946723"
---
# <a name="can-i-use-visual-studio-2017-release-candidate-with-xamarin"></a>Za pomocą platformy Xamarin można użyć programu Visual Studio 2017 Release Candidate?

## <a name="can-i-use-visual-studio-2017-release-candidate-with-xamarin"></a>Za pomocą platformy Xamarin można użyć programu Visual Studio 2017 Release Candidate?

Tak. Jesteś w stanie wykorzystać Xamarin za pomocą programu Visual Studio 2017 Release Candidate. Jednak należy pamiętać, że obecnie, zainstalowanie programu Visual Studio RC 2017 Xamarin odinstaluje wcześniejszą wersję programu Xamarin zainstalowanych w programie Visual Studio 2015/2013. Xamarin dla Visual Studio 2017 nie można zainstalować w tym samym czasie jako Xamarin dla Visual Studio 2015/2013 wyniku Visual Studio 2017 r odchodzi pakiet MSI w kierunku użycie systemu Instalator programu Visual Studio.

Gdy aktualnie poszukuje zespołu w sposób, aby pominąć to oczekiwane zachowanie, zaleca się użytkownikom wybór ich środowisko pracy na podstawie swoich potrzeb. 

> [!IMPORTANT]
> Jeśli tworzysz Xamarin.iOS projektów, upewnij się, że program Visual Studio dla komputerów Mac w systemie Mac sparowanego jest w tej samej wersji platformy Xamarin.iOS kanału.

## <a name="how-do-i-install-xamarin-to-visual-studio-2017-release-candidate"></a>Jak zainstalować program Xamarin do programu Visual Studio 2017 Release Candidate?

### <a name="installing-during-the-visual-studio-2017-rc-installer"></a>Instalowanie podczas Instalator RC 2017 programu Visual Studio

* Wybierz **Xamarin** składnika w ramach nowej **Instalator programu Visual Studio**

  [![](visualstudio-2017-rc-images/install1-sml.png "Ekran Instalatora RC 2017 Visual Studio")](visualstudio-2017-rc-images/install1-orig.png#lightbox)

Spowoduje to zainstalowanie rozszerzenie programu Visual Studio do tworzenia aplikacji platformy Xamarin.iOS i platformy Xamarin.Android.

### <a name="installing-or-reinstalling-xamarin-in-an-existing-installation-of-visual-studio-2017-rc"></a>Instalowanie lub ponowne instalowanie platformy Xamarin w istniejącej instalacji programu Visual Studio 2017 RC

#### <a name="using-the-visual-studio-installer"></a>Za pomocą Instalatora programu Visual Studio:

1. Wyszukiwanie aplikacji Instalator programu Visual Studio

  [![](visualstudio-2017-rc-images/reinstall1-sml.png "Wyniki wyszukiwania dla aplikacji Instalatora programu Visual Studio")](visualstudio-2017-rc-images/reinstall1-orig.png#lightbox)

2. Wybierz:. **Tworzenie przenośnych za pomocą platformy .NET (wersja zapoznawcza)** na karcie obciążeń lub

  [![](visualstudio-2017-rc-images/reinstall2-sml.png "Karta obciążeń Instalator VS") ](visualstudio-2017-rc-images/reinstall2-orig.png#lightbox) b. **Xamarin** w **pojedynczych składników** kartę

  [![](visualstudio-2017-rc-images/reinstall3-sml.png "Karta składników Instalator programu VS")](visualstudio-2017-rc-images/reinstall3-orig.png#lightbox)

#### <a name="using-the-visual-studio-installer-within-visual-studio"></a>Za pomocą Instalatora programu Visual Studio w programie Visual Studio:
1. Przejdź do strony początkowej 2017 Visual Studio
2. Polecenie **więcej szablony projektu** w obszarze **nowy projekt** sekcji

    [![](visualstudio-2017-rc-images/reinstall4-sml.png "Strona początkowa programu Visual Studio")](visualstudio-2017-rc-images/reinstall4-orig.png#lightbox)
3. Polecenie `Open Visual Studio Installer` w okienku po lewej stronie

    [![](visualstudio-2017-rc-images/reinstall5-sml.png "Nowy projekt ekranu")](visualstudio-2017-rc-images/reinstall5-orig.png#lightbox)
4. Wybierz:
    
    a. **Tworzenie przenośnych za pomocą platformy .NET (wersja zapoznawcza)** na karcie obciążeń lub

    [![](visualstudio-2017-rc-images/reinstall2-sml.png "Karta obciążeń Instalator VS") ](visualstudio-2017-rc-images/reinstall2-orig.png#lightbox) b. **Xamarin** w **pojedynczych składników** kartę

    [![](visualstudio-2017-rc-images/reinstall3-sml.png "Karta składników Instalator programu VS")](visualstudio-2017-rc-images/reinstall3-orig.png#lightbox)
