---
title: Firmy Microsoft OpenJDK dystrybucji (wersja zapoznawcza)
description: Przewodnik krok po kroku do konfigurowania dystrybucji OpenJDK firmy Microsoft.
ms.prod: xamarin
ms.assetid: B5F8503D-F4D1-44CB-8B29-187D1E20C979
ms.technology: xamarin-android
author: vyedin
ms.author: vyedin
ms.date: 07/22/2018
ms.openlocfilehash: 6c1346918ca6881e551f6c5b89ab16ad13d3f804
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242551"
---
# <a name="microsofts-openjdk-distribution-preview"></a>Firmy Microsoft OpenJDK dystrybucji (wersja zapoznawcza)

_W tym przewodniku opisano kroki do przełączania do wersji zapoznawczej dystrybucji OpenJDK firmy Microsoft._

![Funkcja w wersji zapoznawczej](~/media/shared/preview.png)

## <a name="overview"></a>Omówienie

Począwszy od programu Visual Studio 15.9 i programu Visual Studio dla komputerów Mac 7.7, Visual Studio Tools for Xamarin zostaną przeniesione z firmy Oracle JDK uproszczone wersję OpenJDK, które są przeznaczone wyłącznie na potrzeby opracowywania aplikacji systemu Android:

![Nowy przepływ pracy, oferując OpenJDK w programie VS 15.8 wersji zapoznawczej 5 podglądu w sieci web](openjdk-images/openjdk.png)

Dostępne są następujące korzyści wynikające z tego przeniesienia:

- Będziesz zawsze mieć wersję OpenJDK, który działa w przypadku opracowywania aplikacji systemu Android.

- Pobieranie zestawu JDK 9 lub 10 nie będzie mieć wpływ na środowisko programistyczne.

- Znacznie obniżone pobierania rozmiar i zużycia.

- Brak więcej problemów 3 serwerami innych firm i pliki instalacyjne.

Jeśli chcesz przenieść się na poprawie wcześniej, kompilacje dystrybucji OpenJDK firmy Microsoft są dostępne do przetestowania zarówno Windows, jak i komputerów Mac. Poniżej opisano proces instalacji, a użytkownik może przywrócić Oracle JDK w dowolnym momencie.

## <a name="download"></a>Pobieranie

Aby rozpocząć pracę, Pobierz poprawna kompilacja w systemie:

- **Mac** &ndash; https://dl.xamarin.com/OpenJDK/mac/microsoft-dist-openjdk-1.8.0.9.zip
- **Windows x86** &ndash; https://dl.xamarin.com/OpenJDK/win32/microsoft-dist-openjdk-1.8.0.9.zip
- **Windows x64** &ndash; https://dl.xamarin.com/OpenJDK/win64/microsoft-dist-openjdk-1.8.0.9.zip

## <a name="configure"></a>Konfigurowanie

Rozpakuj go do poprawnej lokalizacji:

- **Mac** &ndash; **$HOME/Library/Developer/Xamarin/jdk/microsoft_dist_openjdk_1.8.0.9**
- **Windows** &ndash; **C:\\Program Files\\Android\\jdk\\microsoft_dist_openjdk_1.8.0.9**

> [!IMPORTANT]
> W tym przykładzie użyto kompilacji 1.8.0.9; jednak może być nowsza wersja, którą można pobrać.

Środowisko IDE, wskaż nowy zestaw JDK:

- **Mac** &ndash; kliknij **Narzędzia > Menedżer zestawów SDK > Lokalizacje** i zmień **lokalizację zestawu Java SDK (JDK)** na pełną ścieżkę instalacji OpenJDK. W poniższym przykładzie ustawiono tę ścieżkę **$HOME/Library/Developer/Xamarin/jdk/microsoft_dist_openjdk_1.8.0.9**.

![Ustawianie ścieżki zestawu JDK dystrybucji OpenJDK firmy Microsoft na komputerze Mac](openjdk-images/vsm.png)

- **Windows** &ndash; kliknij **Narzędzia > Opcje > Xamarin > Ustawienia systemu Android** i zmień **lokalizację zestawu Java Development Kit** na pełną ścieżkę instalacji OpenJDK. W poniższym przykładzie ustawiono tę ścieżkę **C:\\Program Files\\Android\\jdk\\microsoft_dist_openjdk_1.8.0.9**:

![Ustawianie ścieżki zestawu JDK dystrybucji Microsoft OpenJDK Windows](openjdk-images/vs.png)

## <a name="revert"></a>Przywróć

Aby przywrócić Oracle JDK, zmień lokalizację zestawu Java SDK do ścieżki zestawu JDK Oracle wcześniej używanych i ponownie skompiluj rozwiązanie. Na komputerze Mac, możesz przywrócić do ścieżki zestawu JDK Oracle, klikając **Resetuj na domyślne**.

Jeśli masz jakiekolwiek problemy z rozkładem OpenJDK firmy Microsoft, przy użyciu narzędzia do udzielania opinii w środowisku IDE, tak aby mogą być śledzone i szybko poprawiane zgłosić problemy.

## <a name="known-issues--planned-fix-dates"></a>Znane problemy i daty planowanego awarii

`JAVA_HOME` Zmienna środowiskowa nie może być poprawnie eksportowany do zestawu SDK oraz Menedżera urządzeń. Jako obejście można ustawić tej zmiennej środowiskowej do lokalizacji OpenJDK na komputerze. Problem ten został rozwiązany w wersji 15.9 zapoznawczych.

## <a name="summary"></a>Podsumowanie

W tym artykule przedstawiono sposób konfigurowania środowiska IDE, aby użyć wersji zapoznawczej dystrybucji OpenJDK firmy Microsoft, który jest slated stabilnej wersji w dalszej części 2018.
