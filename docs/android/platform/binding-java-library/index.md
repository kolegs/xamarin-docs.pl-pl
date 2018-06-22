---
title: Powiązanie biblioteka języka Java
description: Android społeczności ma wiele bibliotek języka Java, których można użyć w aplikacji; w tym przewodniku wyjaśniono, jak zastosować bibliotek języka Java w aplikacji platformy Xamarin.Android przez utworzenie biblioteki powiązania.
ms.prod: xamarin
ms.assetid: B39FF1D5-69C3-8A76-D268-C227A23C9485
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/01/2017
ms.openlocfilehash: 3c2ed92da07519516db94697bbeaac9f328baa22
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
ms.locfileid: "30768824"
---
# <a name="binding-a-java-library"></a>Powiązanie biblioteka języka Java

_Android społeczności ma wiele bibliotek języka Java, których można użyć w aplikacji; w tym przewodniku wyjaśniono, jak zastosować bibliotek języka Java w aplikacji platformy Xamarin.Android przez utworzenie biblioteki powiązania._

## <a name="overview"></a>Omówienie

Duże jest ekosystem firm biblioteki dla systemu Android. W związku z tym często warto użyć istniejącej biblioteki systemu Android niż Aby utworzyć nowy. Xamarin.Android udostępnia dwa sposoby używania biblioteki:

-   Utwórz *biblioteki powiązania* który jest automatycznie zawijany biblioteki z otoki C#, można wywoływać kodu języka Java za pomocą wywołania C#.

-   Użyj *natywnego interfejsu Java* (*JNI*) do wywołania bezpośrednio wywołań w kodzie biblioteka języka Java. JNI jest programowania strukturę, która umożliwia kodu języka Java w celu wywołania i być wywoływany przez natywnych aplikacji lub biblioteki.

W tym przewodniku opisano pierwsza opcja: tworzenie *biblioteki powiązania* który opakowuje co najmniej jeden istniejących bibliotek języka Java w zestawie, do którego można połączyć w aplikacji. Aby uzyskać więcej informacji na temat używania JNI, zobacz [Praca z JNI](~/android/platform/java-integration/working-with-jni.md).

Xamarin.Android implementuje powiązania za pomocą *zarządzane wywoływane otoki* (*MCW*). MCW jest mostek JNI, który jest używany, gdy kod zarządzany musi wywołać kodu języka Java. Wywoływane otoki zarządzanych również zapewniają obsługę dla tworzenie podklas typów języka Java i przesłanianie wirtualnej metody na typach Java. Podobnie gdy kod środowiska uruchomieniowego systemu Android (GRAFIKA) zamierza wywołanie kodu zarządzanego, kopiuje je za pomocą innego Mostek JNI znane jako Android można wywołać otoki (Aktywnej). To [architektura](~/android/internals/architecture.md) przedstawiono na poniższym diagramie:

[![Android JNI Mostek architektury](images/architecture.png)](images/architecture.png#lightbox)

Biblioteka powiązania jest zestawu zawierającego zarządzane wywoływane otoki dla typów języka Java. Na przykład, w tym miejscu jest typem Java `MyClass`, która ma być zawijany w bibliotece powiązania:

```java
package com.xamarin.mycode;

public class MyClass
{
    public String myMethod (int i) { ... }
}
```

Po możemy wygenerować biblioteki powiązania **JAR** zawierający `MyClass`, możemy utworzyć i wywoływać metod w nim w języku C#:

```csharp
var instance = new MyClass ();

string result = instance.MyMethod (42);
```

Aby utworzyć tę bibliotekę powiązań, należy użyć Xamarin.Android *biblioteka języka Java powiązania* szablonu. Projekt wynikowy powiązanie tworzy zestaw .NET z klasami MCW **JAR** pliki i zasoby osadzone w nim projektów biblioteki systemu Android. Można również tworzyć powiązania biblioteki dla systemu Android archiwum (. Pliki AAR) i projektów programu Eclipse biblioteki systemu Android. Za pomocą odwołań do wynikowego zestawu powiązania biblioteki DLL, można ponownie użyć istniejącej biblioteki języka Java w projekcie platformy Xamarin.Android.

Typy referencyjne w bibliotece powiązanie, musisz użyć przestrzeni nazw powiązania biblioteki. Zazwyczaj dodaje się `using` dyrektywy w górnej części źródłowego C# to znaczy pliki programu .NET w wersji przestrzeni nazw nazwa pakietu języka Java. Na przykład, jeśli nazwa użytkownika powiązanych z pakietu Java **JAR** jest następujące:

```csharp
com.company.package
```

A następnie przełączyć następujące `using` instrukcji w górnej części plików źródłowych C# Aby uzyskać dostęp do typów w granicy **JAR** pliku:

```csharp
using Com.Company.Package;
```


Powiązanie istniejącej biblioteki systemu Android, należy mieć na uwadze następujące kwestie:

* **Czy istnieją zależności zewnętrzne dla biblioteki?** &ndash; Wszelkie zależności Java wymagane przez biblioteki systemu Android muszą być zawarte w projekcie platformy Xamarin.Android jako **ReferenceJar** lub jako **EmbeddedReferenceJar**. Wszystkie zestawy natywnego musi zostać dodany do projektu powiązanie jako **EmbeddedNativeLibrary**.  

* **Która wersja interfejsu API systemu Android jest czy docelowej biblioteki systemu Android?** &ndash; Nie jest możliwe "przejścia" poziom interfejsu API systemu Android; Upewnij się, projekt platformy Xamarin.Android powiązanie dotyczy tego samego interfejsu API level (lub nowszej) jako biblioteka systemu Android.

* **Jakiej wersji zestaw JDK, który został użyty do kompilowania biblioteki?** &ndash; Powiązanie błędy mogą wystąpić, jeśli biblioteka systemu Android został utworzony za pomocą innej wersji JDK niż używana przez platformy Xamarin.Android. Jeśli to możliwe Skompiluj ponownie biblioteki systemu Android przy użyciu tej samej wersji JDK, używanego przez instalację platformy Xamarin.Android.


## <a name="build-actions"></a>Tworzenie działania

Podczas tworzenia biblioteki powiązań, ustaw *akcji* na **JAR** lub. Pliki AAR, zawierające do projektu biblioteki powiązania &ndash; każda akcja kompilacji określa sposób **JAR** lub. Plik AAR zostaną osadzone w (lub odwołuje się) biblioteki powiązania. Poniższa lista zawiera podsumowanie tych akcji:

* `EmbeddedJar` &ndash; Osadza **JAR** do wynikowa Biblioteka DLL biblioteki powiązania jako osadzony zasób. Jest to najprostszy i większość akcji najczęściej używanych kompilacji. Użyj tej opcji, jeśli chcesz **JAR** automatycznie skompilowana do kodu bajtowego i umieszczone w bibliotece powiązania.

* `InputJar` &ndash; Nie można osadzić **JAR** do wynikowy biblioteki powiązania. BIBLIOTEKI DLL. Biblioteki powiązania. Biblioteka DLL będzie mieć zależność na tym **JAR** w czasie wykonywania. Użyj tej opcji, jeśli nie chcesz dołączyć **JAR** w bibliotece powiązania (na przykład w przypadku licencjonowania powodów). Jeśli używasz tej opcji upewnij się, że dane wejściowe **JAR** jest dostępny na urządzeniu, który uruchamia aplikację.

* `LibraryProjectZip` &ndash; Osadza. Plik AAR do wynikowy biblioteki powiązania. BIBLIOTEKI DLL. Przypomina EmbeddedJar, z wyjątkiem tego, aby miał dostęp do zasobów (a także kod), w granicy. Plik AAR. Użyj tej opcji, jeśli ma zostać osadzony. AAR do biblioteki powiązania.

* `ReferenceJar` &ndash; Określa odwołania **JAR**: odwołanie **JAR** jest **JAR** co Twoje granica **JAR** lub. Zależy od pliki AAR. To odwołanie **JAR** służy tylko do zaspokojenia zależności kompilacji. Korzystając z tej akcji kompilacji, C# powiązania nie są tworzone dla odwołania **JAR** i nie jest zagnieżdżony w bibliotece wynikowy powiązania. BIBLIOTEKI DLL. Użyj tej opcji, które można utworzyć biblioteki powiązań dla odwołania **JAR** , ale nie zostało zrobione jeszcze. Ta akcja kompilacji jest przydatne w przypadku wielu pakowania **JAR**s (i/lub. AARs) do wielu współzależne bibliotek powiązania.

* `EmbeddedReferenceJar` &ndash; Osadza odwołanie **JAR** do wynikowy biblioteki powiązania. BIBLIOTEKI DLL. Użyj tej akcji kompilacji, jeśli chcesz utworzyć powiązania C# dla obu danych wejściowych **JAR** (lub. AAR) i wszystkie jego odwołania **JAR**(s) w bibliotece powiązania.

* `EmbeddedNativeLibrary` &ndash; Osadza natywny **.so** do powiązania. Ta akcja kompilacji jest używana do **.so** pliki, które są wymagane przez **JAR** plików jest powiązany. Być może trzeba będzie ręcznie załadować **.so** biblioteki przed wykonaniem kodu z biblioteki języka Java. Jest to opisane poniżej.

Kompilacji, te akcje są co omówiono bardziej szczegółowo w następujących przewodnikach.

Ponadto następujące akcje kompilacji służą do importowania dokumentacji interfejsu API języka Java i przekonwertować je na plik dokumentacji XML C#:

* `JavaDocJar` Służy do wskaż Javadoc archiwum Jar biblioteki języka Java, która odpowiada Maven stylu pakietu (zazwyczaj `FOOBAR-javadoc**.jar**`).
* `JavaDocIndex` Służy do wskaż `index.html` pliku w dokumentacji interfejsu API HTML.
* `JavaSourceJar` Służy do uzupełnienia `JavaDocJar`, aby najpierw Generowanie JavaDoc ze źródeł, a następnie traktować jako wyniki `JavaDocIndex`, biblioteki języka Java, która odpowiada Maven pakietu styl (zazwyczaj `FOOBAR-sources**.jar**`).

Dokumentacja interfejsu API powinna być doclet domyślne Java8, Java7 lub Java6 SDK (są one różne format), lub styl DroidDoc.

## <a name="including-a-native-library-in-a-binding"></a>W tym natywnej biblioteki w powiązaniu

Może być konieczne jest stosowanie **.so** biblioteki w projekcie platformy Xamarin.Android powiązania jako część powiązanie biblioteka języka Java. Podczas opakowana kodu języka Java, Xamarin.Android zakończy się niepowodzeniem wywołania JNI i komunikat o błędzie _java.lang.UnsatisfiedLinkError: nie można odnaleźć metody natywnej:_ będą wyświetlane w logcat limit dla aplikacji.

W tym będzie ręcznie załadować **.so** biblioteki z wywołania `Java.Lang.JavaSystem.LoadLibrary`. Na przykład przy założeniu, że projekt platformy Xamarin.Android udostępnił biblioteki **libpocketsphinx_jni.so** zawarty w projekcie powiązania z akcją kompilacji **EmbeddedNativeLibrary**, następujące fragment kodu (wykonywane przed rozpoczęciem korzystania z biblioteki udostępnionej) zostanie załadowany **.so** biblioteki:

```csharp
Java.Lang.JavaSystem.LoadLibrary("pocketsphinx_jni");
```

## <a name="adapting-java-apis-to-ceparsl"></a>Dostosowania interfejsów API języka Java na C&eparsl;

Generator powiązanie Xamarin.Android zmieni niektóre Java idioms i wzorce odpowiadające im wzorce .NET. Na poniższej liście opisano, jak Java jest mapowany na język C# / .NET:

-   _Metody pobierające/ustawiające_ w języku Java są _właściwości_ w .NET.

-   _Pola_ w języku Java są _właściwości_ w .NET.

-   _Interfejsy odbiorników/odbiornika_ w języku Java są _zdarzenia_ w .NET. Parametry metod w interfejsach wywołania zwrotnego będą reprezentowane przez `EventArgs` podklasy.

-   A _zagnieżdżone statycznej klasy_ w języku Java jest _zagnieżdżona klasa_ w .NET.

-   _Klasy wewnętrzny_ w języku Java jest _zagnieżdżona klasa_ z konstruktorem wystąpień w języku C#.



## <a name="binding-scenarios"></a>Scenariusze wiązania

Następujące przewodniki scenariusz wiązania można powiązać biblioteka języka Java (lub biblioteki) do włączenia w swojej aplikacji:

-   [Powiązanie. JAR](~/android/platform/binding-java-library/binding-a-jar.md) jest wskazówki do tworzenia bibliotek powiązań dla **JAR** plików.

-   [Powiązanie. AAR](~/android/platform/binding-java-library/binding-an-aar.md) jest wskazówki do tworzenia bibliotek powiązania. Pliki AAR. Przeczytaj tego przewodnika, aby dowiedzieć się, jak powiązać bibliotek Android Studio.

-   [Powiązanie projektu biblioteki Eclipse](~/android/platform/binding-java-library/binding-a-library-project.md) jest Przewodnik tworzenia bibliotek powiązania z projektów biblioteki systemu Android. Przeczytaj tego przewodnika, aby dowiedzieć się, jak można powiązać projekty biblioteki systemu Android Eclipse.

-   [Dostosowywanie powiązania](~/android/platform/binding-java-library/customizing-bindings/index.md) wyjaśniono, jak dokonać ręcznej modyfikacji powiązania Usuń błędy kompilacji i kształtu wynikowy interfejsu API, aby była ona więcej "C# — takie jak".

-   [Rozwiązywanie problemów z powiązań](~/android/platform/binding-java-library/troubleshooting-bindings.md) zawiera listę typowych scenariuszy Błąd powiązania, opisano możliwe przyczyny i oferuje sugestie dotyczące rozwiązywania tych błędów.


## <a name="related-links"></a>Linki pokrewne

- [Praca z JNI](~/android/platform/java-integration/working-with-jni.md)
- [GAPI Metadata](http://www.mono-project.com/docs/gui/gtksharp/gapi/#metadata)
- [Używanie bibliotek natywnych](~/android/platform/native-libraries.md)
