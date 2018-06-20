---
title: Platformy Xamarin.Android i Java Development Kit 9
description: W tym artykule wyjaśniono, jak rozwiązać błędy Java Development Kit (JDK) 9 platformy Xamarin.Android.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 7DCF0985-F77D-4A68-AC54-10C9846E189A
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/18/2018
ms.openlocfilehash: 529062f820cd682dc6a9c22f706dbceecef1c836
ms.sourcegitcommit: 57f9a9ba2f199697cb75e7be67f1a372c35a861b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36269676"
---
# <a name="xamarinandroid-and-java-development-kit-9"></a>Platformy Xamarin.Android i Java Development Kit 9

_W tym artykule wyjaśniono, jak rozwiązać błędy Java Development Kit (JDK) 9 platformy Xamarin.Android._


## <a name="overview"></a>Omówienie

Xamarin.Android używa języka Java Development Kit (JDK) do integracji z zestawu SDK systemu Android do tworzenia aplikacji systemu Android i systemem Android projektanta. Najnowsze wersje zestawu SDK systemu Android (interfejs API 24 i nowsze) wymagają JDK 8 (1.8). **Ponieważ dostępnej w sklepie Google narzędzia zestawu SDK systemu Android nie są jeszcze zgodne z JDK 9, Xamarin.Android nie działa z JDK 9.**

## <a name="jdk-9-errors"></a>JDK 9 błędów

Jeśli podczas próby skompilowania projektu platformy Xamarin.Android z 9 JDK zainstalowany, zostanie wyświetlona jawne błąd wskazujący, że JDK 9 nie jest obsługiwany.

```shell
Building with JDK Version `9.0.4` is not supported. Please install JDK version `1.8.0`. See https://aka.ms/xamarin/jdk9-errors  
```

Aby usunąć te błędy, należy zainstalować JDK 8 (1.8) zgodnie z objaśnieniem w [jak zaktualizować wersję Java Development Kit (JDK)?](~/android/troubleshooting/questions/update-jdk.md).


## <a name="checking-the-jdk-version"></a>Sprawdzanie wersji JDK

Należy sprawdzić, której wersji programu Java zainstalowano, wprowadzając następujące polecenie (zestaw JDK `bin` katalog musi znajdować się w sieci `PATH`):

```shell
java -version
```

Jeśli zainstalowany JDK 9, zostanie wyświetlony następujący komunikat:

```shell
java version "9.0.4"
Java(TM) SE Runtime Environment (build 9.0.4+11)
Java HotSpot(TM) 64-Bit Server VM (build 9.0.4+11, mixed mode)
```

Jeśli zainstalowano JDK 9, należy zainstalować 8 JDK Java (1.8). Aby uzyskać informacje o sposobie instalowania JDK 8, zobacz [jak zaktualizować wersję Java Development Kit (JDK)?](~/android/troubleshooting/questions/update-jdk.md)

Należy pamiętać, że nie masz odinstalować JDK 9. jednak należy się upewnić używa Xamarin JDK 8 zamiast JDK 9. W programie Visual Studio, kliknij przycisk **Narzędzia > Opcje > Xamarin > Ustawienia systemu Android**. Jeśli **Java Development Kit lokalizacji** nie jest ustawiony na JDK 8 lokalizacji (takie jak **C:\\Program Files\\Java\\jdk1.8.0_111**), kliknij przycisk **zmiany**  i ustaw go do lokalizacji, w którym zainstalowano JDK 8. W programie Visual Studio dla komputerów Mac, przejdź do **Preferencje > Projekty > lokalizacji zestawu SDK > Android > zestaw SDK Java (JDK)** i kliknij przycisk **Przeglądaj** do aktualizacji tej ścieżki.

## <a name="known-issues-with-jdk-9"></a>Znane problemy z JDK 9

### <a name="apksigner"></a>apksigner

Jest to znany problem z apksigner i JDK 9, w którym `apksigner.bat` wywołuje plik `apksigner.jar` z `-Djava.ext.dirs` zamiast `-classpath` oczekiwała JDK 9. Zaleca się używania JDK 8 (1.8). Aby uzyskać informacje o sposobie instalowania JDK 8, zobacz [jak zaktualizować wersję Java Development Kit (JDK)?](~/android/troubleshooting/questions/update-jdk.md)

Jeśli zainstalowano JDK 9, upewnij się, że następująca ścieżka nie jest ustawiona na Twojej `PATH` zmiennej środowiskowej, ponieważ nadal będzie wskazywać JDK 9: `C:\ProgramData\Oracle\Java\javapath`. Po usunięciu, `java-version` w wierszu polecenia powinny być widoczne JDK 8.
