---
title: Funkcje platformy systemu Windows
ms.topic: article
ms.prod: xamarin
ms.assetid: F6EA9E49-FB3E-442F-AF13-B7AD0C80D11F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/20/2017
ms.openlocfilehash: f77290a8c780d7dd5c936af576b39228d91687aa
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="windows-platform-features"></a>Funkcje platformy systemu Windows

Tworzenie aplikacji platformy Xamarin.Forms dla platform Windows wymaga programu Visual Studio. [Strony wymagania](~/xamarin-forms/get-started/installation.md) zawiera więcej informacji na temat wymagania wstępne.

![](images/allhanselman.png "Aplikacji platformy Xamarin.Forms działających w systemie Windows")

## <a name="platform-support"></a>Obsługa platformy

Szablony platformy Xamarin.Forms dostępne w programie Visual Studio zawiera jeden projekt Windows domyślnie:

* **Uniwersalnych aplikacji platformy systemu Windows** -aplikacji platformy Xamarin.Forms również mogą być optymalizowane dla systemu Windows 10. Aplikacje uniwersalne (UWP) można uruchamiać na telefon, Tablet PC i urządzeń komputerowych.

Jeśli zainstalowano opcje poprawne programowanie w Visual Studio, jest również możliwe [dodać](installation/index.md) projektu te typy do obsługi starszych wersji systemu Windows:

* **Windows 8.1** — można wdrożyć aplikacji platformy Xamarin.Forms typu tablet i pulpitu rozmiarach jako aplikację Windows 8.1 projektu za pomocą formantów WinRT.
* **Windows Phone 8.1** -platformy Xamarin.Forms ma pełną obsługę dla platformy Windows Phone 8.1 przy użyciu WinRT. Wygląd i działanie aplikacji przy użyciu obsługi systemu Windows Phone 8.1 mogą być inne do starszych aplikacji platformy Xamarin.Forms Windows Phone opartych na technologii Silverlight.


> [!NOTE]
> Obsługa platformy Xamarin.Forms 1.x i 2.x _Windows Phone 8 Silverlight_ projektowanie aplikacji, jednak ten typ projektu jest przestarzała.


## <a name="getting-started"></a>Wprowadzenie

Przejdź do **Plik > Nowy > Projekt** w programie Visual Studio i wybierz jedną z **i Platform > Pusta aplikacja (platformy Xamarin.Forms)** szablony, aby rozpocząć pracę.

Starsze rozwiązań platformy Xamarin.Forms lub utworzone na macOS, nie będą mieć projekty systemu Windows wymienionych powyżej, ale muszą być dodane ręcznie.
Jeśli ma zostać celem platformy Windows nie jest już w rozwiązaniu, odwiedź [instrukcje instalacji](installation/index.md) można dodać żądanego systemu Windows projektu typu/s.


## <a name="samples"></a>Przykłady

[Wszystkie przykłady](https://github.com/xamarin/xamarin-forms-book-preview-2) książki Charlesa Petzold [ *tworzenia aplikacji mobilnych za pomocą platformy Xamarin.Forms* ](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md) obejmują Windows Phone 8.1, Windows 8.1 i projekty platformy uniwersalnej systemu Windows (dla systemu Windows 10).

["Scott Hanselman" demonstracją aplikacji](https://github.com/jamesmontemagno/Hanselman.Forms) jest dostępne oddzielnie, a także projektów Apple Watch i Android nosić (przy użyciu odpowiednio Xamarin.iOS i Xamarin.Android, platformy Xamarin.Forms nie działa w tych platform).


## <a name="related-links"></a>Linki pokrewne

- [Instalator Windows projektów](~/xamarin-forms/platform/windows/installation/index.md)
