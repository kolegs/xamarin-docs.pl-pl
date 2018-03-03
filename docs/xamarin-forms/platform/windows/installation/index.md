---
title: "Instalator Windows projektów"
description: "Dodawanie nowych projektów systemu Windows do istniejącego rozwiązania platformy Xamarin.Forms"
ms.topic: article
ms.prod: xamarin
ms.assetid: A0774D2E-6994-4D91-84E8-DAB66FC92320
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/16/2016
ms.openlocfilehash: 2acabc68c595bfb3bbd5c3516f68cd777ba10327
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
---
# <a name="setup-windows-projects"></a>Instalator Windows projektów

_Dodawanie nowych projektów systemu Windows do istniejącego rozwiązania platformy Xamarin.Forms_

Projekty platformy Xamarin.Forms starsze (lub utworzonych w systemie Mac OS&nbsp;X) nie będą miały te konfiguracji projekty systemu Windows.

Oznacza to, że musisz ręcznie dodać tych typów projektów, aby tworzyć aplikacje Windows 8.1, Windows Phone 8.1 i Windows 10 (UWP).

## <a name="add-a-windows-81-app"></a>Dodawanie aplikacji Windows 8.1

* Jeśli używasz szablonu PCL [zaktualizować profil](#pcl), następnie
* [Dodawanie aplikacji Windows 8.1](~/xamarin-forms/platform/windows/installation/tablet.md) dla obudów typu tablet/pulpitu.

## <a name="add-a-windows-phone-81-app"></a>Dodaj Windows Phone 8.1 aplikacji

* Jeśli używasz szablonu PCL [zaktualizować profil](#pcl), następnie
* [Dodaj Windows Phone 8.1 aplikacji](~/xamarin-forms/platform/windows/installation/phone.md)

## <a name="add-a-universal-windows-platform-uwp-app"></a>Dodaj uniwersalnych systemu Windows platformy Uniwersalnej aplikacji

* Tworzenie [UWP](https://msdn.microsoft.com/library/windows/apps/dn894631.aspx) aplikacji wymaga programu Visual Studio 2015 uruchomiony w systemie Windows 10.
* Jeśli używasz szablonu PCL [zaktualizować profil](#pcl), następnie
* [Dodaj uniwersalnych systemu Windows platformy aplikacji](~/xamarin-forms/platform/windows/installation/universal.md)

<a name="pcl" />

### <a name="update-your-pcl-profile"></a>Aktualizuj profil PCL

Istniejących aplikacji platformy Xamarin.Forms użycie szablonu przenośnej biblioteki klasy (PCL), należy zaktualizować jej profilu.

1. **Kliknij prawym przyciskiem myszy > właściwości** (istniejących ustawień może różnić się)

  ![](images/targets.png "Obiekty docelowe PCL")

2. Polecenie **zmian...**  przycisku

3. Upewnij się, **systemu Windows 8** i **Windows Phone 8.1** są zaznaczone opcje (i **Silveright Windows Phone** jest *cofnąć wybrane*):

  ![](images/pcl.png "Target — opcje PCL")

4. Naciśnij klawisz **OK** i zapisać zmiany.

To jest równa **111 profilu** w przypadku konfigurowania sieci PCL w programie Visual Studio dla komputerów Mac przy użyciu listy rozwijanej.

  ![](images/pcl-xs.png "PCL Profile 111")

**Uwaga:** Jeśli rozwiązania jest nadal projektu Silverlight 8 Windows Phone, PCL powinien być ustawiony na 259 profilu. Obsługa systemu Windows Phone 8 Silverlight jest przestarzałe, dlatego zalecane jest, Zastąp z typami projektu wyświetlane na tej stronie.
