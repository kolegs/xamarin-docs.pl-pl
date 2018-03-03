---
title: "Omówienie integracji Java"
description: "Ekosystemu języka Java obejmuje różne i olbrzymie zbiór elementów. Wiele z tych składników można skrócić czas potrzebny do opracowywania aplikacji systemu Android. Ten dokument zostanie wprowadzić i stanowią ogólne omówienie niektórych metod, deweloperzy mogą używać tych istniejące składniki Java zwiększające ich możliwości tworzenia aplikacji platformy Xamarin.Android."
ms.topic: article
ms.prod: xamarin
ms.assetid: 7B5B8695-1C49-19BF-AE99-948CDCBD2A20
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/18/2017
ms.openlocfilehash: 88e8c66d36956649f0a996046f038d89a7267cf5
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="java-integration-overview"></a>Omówienie integracji Java

_Ekosystemu języka Java obejmuje różne i olbrzymie zbiór elementów. Wiele z tych składników można skrócić czas potrzebny do opracowywania aplikacji systemu Android. Ten dokument zostanie wprowadzić i stanowią ogólne omówienie niektórych metod, deweloperzy mogą używać tych istniejące składniki Java zwiększające ich możliwości tworzenia aplikacji platformy Xamarin.Android._

<a name="Overview" />

## <a name="overview"></a>Omówienie

Podana w zakresie ekosystemu języka Java, jest bardzo prawdopodobne, że wszelkie danego funkcje wymagane dla aplikacji platformy Xamarin.Android ma już kodowanych w języku Java. W związku z tym jest atrakcyjne i spróbuj ponownie wykorzystać te istniejącej biblioteki podczas tworzenia aplikacji platformy Xamarin.Android. 

Istnieją trzy sposoby można ponownie użyć bibliotek języka Java w aplikacji platformy Xamarin.Android: 

-   **Utwórz bibliotekę powiązania Java** &ndash; z tej techniki projektu platformy Xamarin.Android służy do tworzenia otoki C# wokół typów języka Java. Aplikacji platformy Xamarin.Android odwoływanie otoki C# utworzone przez tego projektu, a następnie użyj `.jar` pliku. 

-   **Interfejs natywny Java** &ndash; *Java natywnego* *interfejsu* (JNI) to platforma, która pozwala się połączyć lub być wywoływany przez kod języka Java działający wewnątrz maszyny JVM kodu-Java (takie jak C++ lub C#) . 

-   **Port kod** &ndash; ta metoda obejmuje pobieranie kodu źródłowego języka Java, a następnie konwertując go do języka C#. Można to zrobić ręcznie lub za pomocą automatycznego narzędzia, takie jak wyostrzania. 

Fundament pierwszych dwóch metod jest *natywnego interfejsu Java* (JNI). JNI to platforma, która umożliwia aplikacjom nie napisany w języku Java na współdziałanie z kodem języka Java uruchomione w maszynie wirtualnej Java. Xamarin.Android używa JNI w celu utworzenia *powiązania* dla kodu C#. 

Pierwszy technika jest bardziej zautomatyzowaną, deklaratywne podejście do powiązania bibliotek języka Java. Obejmuje ona dla komputerów Mac lub typ projektu programu Visual Studio zapewnianej przez platformy Xamarin.Android przy użyciu albo program Visual Studio &ndash; biblioteka języka Java powiązania. Aby pomyślnie utworzyć tych powiązań, biblioteki powiązania Java nadal może wymagać ręcznego zmodyfikowana, ale nie tyle jako czysty podejście JNI czy. Zobacz [powiązanie biblioteka języka Java](~/android/platform/binding-java-library/index.md) Aby uzyskać więcej informacji o bibliotekach powiązanie Java. 

Drugiej techniki przy użyciu JNI, działa na wiele niższym poziomie, ale można zapewnić bardziej precyzyjną kontrolę i dostęp do metody Java, które zwykle nie jest dostępny za pośrednictwem biblioteka języka Java powiązania. 

Trzeci technika znacząco różni się od poprzedniej dwa: eksportowanie kodu w języku Java na język C#. Eksportowanie kodu z jednego języka do innego może być bardzo pracochłonne proces, ale można zmniejszyć, że wywoływana nakładu pracy za pomocą narzędzia *wyostrzania*. Wyostrzania jest narzędziem typu open source, które jest Java-do-C# konwertera. 


<a name="Summary" />

## <a name="summary"></a>Podsumowanie

Niniejszy dokument jest udostępniany ogólne omówienie niektórych różne sposoby, że biblioteki za pomocą języka Java mogą być ponownie używane w aplikacji platformy Xamarin.Android. Go wprowadzono pojęcia powiązań i zarządzane wywoływane otoki i opisem opcji eksportowanie Java kodu dla C#. 


## <a name="related-links"></a>Linki pokrewne

- [Architektura](~/android/internals/architecture.md)
- [Tworzenie powiązań biblioteki języka Java](~/android/platform/binding-java-library/index.md)
- [Praca z JNI](~/android/platform/java-integration/working-with-jni.md)
- [Sharpen](https://github.com/slluis/sharpen)
- [Interfejs natywny Java](http://docs.oracle.com/javase/7/docs/technotes~/jni/index.html)
