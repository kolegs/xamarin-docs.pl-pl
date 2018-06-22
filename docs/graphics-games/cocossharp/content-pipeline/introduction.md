---
title: Wprowadzenie do zawartości potoki
description: Zawartości potoki to aplikacje lub części aplikacji, służące do konwersji do formatu, który może zostać załadowany w projektach gier plików. Potok zawartości MonoGame jest implementacją określonych potoku zawartości do konwertowania plików dla projektów CocosSharp i MonoGame.
ms.prod: xamarin
ms.assetid: 40628B5F-FAF7-4FA7-A929-6C3FEA83F8EC
author: charlespetzold
ms.author: chape
ms.date: 03/27/2017
ms.openlocfilehash: a369c5ba61033eb61c0f188c03b21e08c71784fb
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/09/2018
ms.locfileid: "33922838"
---
# <a name="introduction-to-content-pipelines"></a>Wprowadzenie do zawartości potoki

_Zawartości potoki to aplikacje lub części aplikacji, służące do konwersji do formatu, który może zostać załadowany w projektach gier plików. Potok zawartości MonoGame jest implementacją określonych potoku zawartości do konwertowania plików dla projektów CocosSharp i MonoGame._

Ten artykuł zawiera pojęć związanych z zawartością potoki, przede wszystkim koncentrujących się na *MonoGame zawartości potoku*, która jest używana z CocosSharp i MonoGame implementacja potoku zawartości.


## <a name="what-is-a-content-pipeline"></a>Co to jest potoku zawartości?

Termin *zawartości potoku* to ogólny termin do konwertowania pliku z jednego formatu na inny proces. *Wejściowych* zawartości potoku jest zazwyczaj plikiem wyjściowych z narzędzia autorskiego, takich jak pliki obrazów z programu Photoshop. Tworzy zawartości potoku *dane wyjściowe* pliku w formacie, który może zostać załadowany bezpośrednio przez gier projektu. Zazwyczaj pliki wyjściowe są zoptymalizowane pod kątem szybkie ładowanie i zmniejszenie rozmiaru dysku.

Zawartości potoki może być wiersza polecenia pliki wykonywalne, aplikacjami graficznego interfejsu użytkownika lub wtyczek osadzony w innej aplikacji, takiego jak Visual Studio. Potoki zawartości są zazwyczaj uruchamiane przed wykonaniem gry. Jeśli zawartości potoku jest skojarzony z niektórych aplikacji, takimi jak Visual Studio, następnie go może wykonać podczas kompilacji. Jeśli potoku zawartości to aplikacja autonomiczna, może on również uruchomić po jawnie informację, aby to zrobić przez użytkownika. Aplikacja lub logiki odpowiedzialny za konwertowanie określony plik wejściowy (takich jak **.png**) do wyjścia skojarzony plik jest określany jako *konstruktora*. 

Firma Microsoft może wizualizacji ścieżkę pliku pobiera autorstwa do ładowane w czasie wykonywania w następujący sposób:

![](introduction-images/image1.png "Ścieżka pliku pobiera autorstwa do ładowane w czasie wykonywania wizualizacji na tym diagramie")

## <a name="why-use-a-content-pipeline"></a>Dlaczego warto używać potoku zawartości?

Potoki zawartości wprowadzenie dodatkowego kroku między tworzenia aplikacji i gier, które można zwiększyć czas kompilacji i zwiększenia złożoności procesu tworzenia. Pomimo powyższe zagadnienia potoki zawartości, należy wprowadzić pewne korzyści do tworzenia gier:


### <a name="converting-to-a-format-understood-by-the-game"></a>Konwertowanie na format zrozumiałe gry

CocosSharp i MonoGame zapewniają metody ładowania różnych typów zawartości; jednak zawartości muszą być sformatowane prawidłowo przed ładowany. Większość typów zawartości wymagają pewien typ konwersji przed ładowany. Na przykład efekty w **.wav** format muszą zostać przekonwertowane na **.xnb** ma zostać załadowany w czasie wykonywania, ponieważ CocosSharp i MonoGame nie obsługuje ładowania **.wav** format pliku.


### <a name="converting-to-a-format-native-to-the-hardware"></a>Konwertowanie na format macierzysty sprzętu

Sprzętowe mogą traktować zawartości inaczej w czasie wykonywania. Na przykład CocosSharp gry może ładować pliki obrazów podczas tworzenia `CCSprite` wystąpienia. Mimo że tego samego kodu można ładować pliki zarówno dla systemu iOS i Android, każdej z platform inaczej przechowuje załadowanym pliku. W konsekwencji potoku zawartości MonoGame formatuje tekstury **.xnb** pliki inaczej w zależności od platformy docelowej.


### <a name="reducing-size-on-disk"></a>Zmniejszenie rozmiaru na dysku 

Zawartość potoki może zostać użyty do usunięcia informacji, która jest przydatne w czasie autora, ale nie jest konieczna w czasie wykonywania. Oryginalny plik (wejścia) może przechowywać wszystkie informacje, co może pomóc twórcom zawartości Obsługa istniejącej zawartości, ale plik wyjściowy może być stripped-down do zachowywanie małych rozmiarów plików ogólnego gier. Ta kwestia jest szczególnie przydatna w przypadku gry przenośnych, które są pobierane, a nie rozproszone na nośniku instalacyjnym.


### <a name="reducing-load-time"></a>Skrócenie czasu ładowania

Gry może wymagać modyfikacji zawartości, aby poprawić wydajność środowiska uruchomieniowego, zwiększające elementy wizualne lub w celu dodania nowych funkcji. Na przykład wiele gier 3D obliczyć oświetlenia jeden raz, a następnie użyj wynik tego obliczenia podczas renderowania złożonych sceny. Od czasu wykonywania tych obliczeń podczas ładowania zawartości może być zbyt duży zamiast tego można wykonać obliczeń podczas gry jest wbudowana. Wynikowa obliczeń można dołączyć do zawartości, włączanie zawartości do załadowania znacznie szybciej niż w inny sposób. 


## <a name="xnb-file-extension"></a>rozszerzenie pliku xnb

**.Xnb** rozszerzenie pliku jest rozszerzenie dla wszystkich plików wyjściowych przez potok Monogame zawartości. Rozszerzenia plików wyjściowych z Microsoft XNA potoku zawartości jest zgodny.

**.Xnb** rozszerzenie jest używane niezależnie od oryginalnego typu pliku. Innymi słowy, pliki obrazów (**.png**), pliki dźwiękowe (**.wav**), oraz wszelkie niestandardowe typy plików będzie wszystkie wyprodukowania jako **.xnb** pliki, gdy dane są przekazywane za pośrednictwem potoku zawartości. Ponieważ rozszerzenia nie może służyć do rozróżnienia formaty plików różnych następnie zarówno CocosSharp i MonoGame metody, które obciążenia **.xnb** plików nie oczekuje rozszerzeń podczas ładowania pliku.

Pliki .xnb CocosSharp i MonoGame można utworzyć za pomocą narzędzia Monogame potoku, które są objęte [w tym przewodniku](~/graphics-games/cocossharp/content-pipeline/walkthrough.md).


## <a name="summary"></a>Podsumowanie

W tym artykule udostępniane omówienie i zalet zawartości potoki ogólnie rzecz biorąc, oraz wprowadzenie do potoku MonoGame zawartości.

## <a name="related-links"></a>Linki pokrewne

- [Dokumentacja MonoGame potoku](http://www.monogame.net/documentation/?page=Pipeline)
