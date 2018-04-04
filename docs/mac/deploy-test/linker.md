---
title: Opcje konsolidatora Xamarin.Mac
description: Łączenie to narzędzie optymalizacji zaawansowane, które zmniejsza rozmiar aplikacji przez usunięcie nieużywanych kodu.
ms.prod: xamarin
ms.assetid: F03176C3-F8D4-4DE8-870C-7F27D8CE525A
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 11/10/2017
ms.openlocfilehash: f98953574f33612395500787a09351d2ba451802
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="xamarinmac-linker-options"></a>Opcje konsolidatora Xamarin.Mac

_Łączenie to narzędzie optymalizacji zaawansowane, które zmniejsza rozmiar aplikacji przez usunięcie nieużywanych kodu._

## <a name="overview"></a>Omówienie

Na podstawie [platformy docelowej](~/mac/platform/target-framework.md) używa projektu, dostępne opcje konsolidatora może być ograniczony. Jest to spowodowane faktem, że konsolidacji wymaga utworzenia wykres obiektu typu, co używany przez aplikację i nie jest to możliwe w całości (lub nieobsługiwana) z powodu System.Configuration.

Dostępne są cztery opcje:

- **Brak** — Wyłącz wszystkie konsolidacji. Domyślna konfiguracja debugowania w Modern oraz wszystkich konfiguracji z w pełni.
- **Zestaw SDK** — łączy wszystkie zestawu SDK, z wyłączeniem zestawów użytkownika. Domyślne w konfiguracji wydanie w Modern. Niedostępne w całości.
- **Pełna** — Połącz wszystkie zestawy. Wymaga to kod użytkownika jest bezpieczne, zobacz konsolidatora [uwagi](~/ios/deploy-test/linker.md) Aby uzyskać więcej informacji. Niedostępne w całości.
- **Platform** – Link just Xamarin.Mac.dll. Aby uzyskać więcej informacji, zobacz poniżej.

## <a name="platform-linking"></a>Łączenie platformy

Łączenie aplikacji przy użyciu pełnego [platformy docelowej](~/mac/platform/target-framework.md) jest zazwyczaj niebezpieczne, ale istnieje wiele scenariuszy, w którym wymagany jest bardzo ograniczona formę łączenia.

Na przykład aplikacje przesłane do macOS sklepu z aplikacjami nie musi odwoływać się liczba przestarzałe interfejsy API (np. QTKit), niektóre z nich Xamarin.Mac zawiera powiązania. Nawet jeśli aplikacje nie mogą wywoływać tych powiązań, wywołanie będzie istnieć w zestawie SDK i odrzucane.

Łączenie platformy zakłada aplikacji BCL konsolidatora niebezpieczne i są po prostu usuń nieużywane kodu z Xamarin.Mac.dll. 

Wszystkie aplikacje, które nie odzwierciedlają na typy Xamarin.Mac.dll zobaczą do poprawy pomocnicza uruchamiania usunięcie niepotrzebnych typów.

Łączenie platformy przydaje się zwykle tylko do aplikacji przy użyciu platformy docelowej pełne, zgodnie z bardziej zaawansowanych opcji zestawu SDK można używać aplikacji Modern.

## <a name="setting-the-linker-configuration"></a>Ustawienie konfiguracji konsolidatora

Aby zmienić konfigurację konsolidatora do projektu Xamarin.Mac, wykonaj następujące czynności:

1. Otwórz projekt Xamarin.Mac w programie Visual Studio dla komputerów Mac.
2. W **Eksploratora rozwiązań**, kliknij dwukrotnie plik projektu, aby otworzyć **opcje projektu** okno dialogowe.
3. Z **kompilacji Mac** , a następnie wybierz typ **zachowanie konsolidatora** które odpowiada potrzebom aplikacji:

  ![Wybierz zachowanie które konsolidatora do użycia](linker-images/link-behavior.png "wybierz których zachowanie konsolidatora do użycia")

4. Platforma łączenie dla pełnej struktur docelowych nie będą widoczne w IDE do przyszłych aktualizacji. Do tego czasu, należy dodać `--linkplatform` do **mmp dodatkowe argumenty** zamiast tego.
5. Kliknij przycisk **OK** przycisk, aby zapisać zmiany.


## <a name="related-links"></a>Linki pokrewne

- [Łączenie w systemie iOS](~/ios/deploy-test/linker.md)
