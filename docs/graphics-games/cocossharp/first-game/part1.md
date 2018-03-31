---
title: Tworzenie projektu CocosSharp wielu Platform
description: 'W tym przewodniku przedstawiono sposób tworzenia nowego rozwiązania CocosSharp wielu platform. Wynik tego przewodnika jest programu Visual Studio for Mac rozwiązania, które zawiera trzy projekty: jednego projektu biblioteki klas przenośnych, jeden projekt specyficzne dla systemu Android i jeden projekt specyficzne dla systemu iOS. Projekt wynikowy wyświetli pusty czarny ekran po wykonaniu.'
ms.topic: article
ms.prod: xamarin
ms.assetid: 37C97693-B0A8-4064-97B6-A6FAB5BA4FB7
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/27/2017
ms.openlocfilehash: 2906035ce9bd44d111b89ccfe7443896775315b7
ms.sourcegitcommit: 7b88081a979381094c771421253d8a388b2afc16
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/30/2018
---
# <a name="creating-a-multi-platform-cocossharp-project"></a>Tworzenie projektu CocosSharp wielu Platform

_W tym przewodniku przedstawiono sposób tworzenia nowego rozwiązania CocosSharp wielu platform. Wynik tego przewodnika jest programu Visual Studio for Mac rozwiązania, które zawiera trzy projekty: jednego projektu biblioteki klas przenośnych, jeden projekt specyficzne dla systemu Android i jeden projekt specyficzne dla systemu iOS. Projekt wynikowy wyświetli pusty czarny ekran po wykonaniu._

Aparat CocosSharp gier 2W umożliwia kodu i zawartość w celu udostępnienia na wielu platformach. W tym przewodniku przedstawiono sposób tworzenia projektu, który może obsługiwać zarówno dla systemu iOS i Android development. W szczególności ten przewodnik obejmuje następujące tematy:

 - Instalowanie CocosSharp
 - Tworzenie nowego rozwiązania
 - `LoadGame` — Metoda

# <a name="installing-cocossharp"></a>Instalowanie CocosSharp

Najpierw dodamy CocosSharp dla programu Visual Studio dla komputerów Mac. Jeśli uruchomiona na komputerze Mac, wybierz **programu Visual Studio for Mac** > **Menedżera dodatków...**  . Jeśli uruchomiona w systemie Windows, zaznacz **narzędzia** > **Menedżera dodatków...**  . Kliknij przycisk **galerii** karcie, rozwiń **elementu CocosSharp**, wybierz pozycję **szablony projektów CocosSharp**, a na koniec kliknij **zainstalować...**  .

![Dodatek CocosSharp](part1-images/xamarinstudioaddinsmac.png "")

# <a name="creating-a-new-solution"></a>Tworzenie nowego rozwiązania

Teraz, gdy jest zainstalowany CocosSharp, utworzymy rozwiązania. W programie Visual Studio dla komputerów Mac, wybierz **pliku** > **nowy** > **rozwiązania...** . Wybierz **aplikacji** opcję w obszarze **i platform** zaznacz **CocosSharp pusty projekt**, a następnie kliknij przycisk **dalej**:

![](part1-images/image1.png "Wybierz opcję aplikacji znajdujących się w sekcji między platformami, wybierz CocosSharp pusty projekt, a następnie kliknij przycisk Dalej")

Wprowadź nazwę **BouncingGame** dla **Nazwa projektu**, następnie kliknij przycisk **Utwórz**:

![](part1-images/image2.png "Wprowadź nazwę BouncingGame dla nazwy projektu, a następnie kliknij przycisk Utwórz")

Po utworzeniu projektu i Visual Studio for Mac, możemy Skompiluj i uruchom go, aby wyświetlić tło: 

![](part1-images/image3.png "Po projekt został utworzony i Visual Studio for Mac, skompiluj i uruchom go, aby wyświetlić tło")


# <a name="loadgame-method"></a>LoadGame — metoda

Domyślny projekt CocosSharp zawiera klasy specyficzne dla systemów iOS i Android konfigurowania `CCGameView`, które są używane do uruchamiania CocosSharp. `CCGameView` Jest tworzone wystąpienie w sposób specyficzne dla platformy: tworzy projekt dla systemu iOS `CCGameView` w `Main.storyboard` plików, gdy tworzy Android `CCGameView` w `Main.axml` pliku. Każdej z platform używa wystąpienia CCGameView w `LoadGame` metodę, która wykonuje niektóre podstawowe ustawienia. Mimo że firma Microsoft nie będzie zmodyfikowanie ten kod, Spójrzmy na niektóre ważne informacje:

 - Ustawia kod `gameView.DesignResolution` do 1024 x 768. Standaryzuje to pozycjonowanie na urządzeniach, niezależnie od bieżącego urządzenia współczynnik proporcji, rozdzielczość fizyczną i orientacji. 
 - Ten kod dodaje kilka ścieżek wyszukiwania. Ścieżki wyszukiwania również umożliwić zawartości bez prefiksy katalogów. Na przykład od `"Sounds"` ścieżka nie zostanie dodany jako ścieżka wyszukiwania, a następnie plik w katalogu `"Content/Sounds/mysound.xnb"` po prostu mógł zostać załadowany jako `"mysound.xnb"`. Ścieżki wyszukiwania są podobne do `using` instrukcje w kodzie — można zmniejszyć kodu, ale może to również wprowadzić niejednoznaczności.
 - Tworzy kod `GameLayer` wystąpienia. `GameLayer` jest `CCLayer`-dziedziczenia klasy gdzie dodamy wszystkich gier logiki. Większe gry może wymagać wielu `CCLayer` wystąpień lub nawet kilka `CCScene` wystąpień (mogącą zawierać wiele `CCLayer` wystąpień), ale firma Microsoft będzie trzymaj pojedynczej `CCLayer` dla tej gry.

#  <a name="summary"></a>Podsumowanie

W tym przewodniku objętych sposobu tworzenia projektu CocosSharp wielu platform przy użyciu programu Visual Studio dla komputerów Mac. Wynik jest pusty ekran, który może służyć jako punkt początkowy dla każdego projektu gier.