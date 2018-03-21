---
title: "Metadane powiązania Java"
description: "Kod C# w Xamarin.Android wywołuje bibliotek języka Java za pomocą powiązania, które są mechanizm abstracts szczegółów niskiego poziomu, które są określone w języku Java natywnego interfejsu (JNI). Xamarin.Android udostępnia narzędzia, który generuje tych powiązań. Tego narzędzia umożliwia deweloperowi kontroli, jak powiązania jest tworzona przy użyciu metadanych, dzięki czemu procedury przykład: modyfikacja przestrzenie nazw oraz zmiana nazwy elementów członkowskich. Ten dokument w tym artykule omówiono, jak metadanych działa, znajduje się podsumowanie atrybuty metadane obsługuje i wyjaśniono, jak rozwiązać problemy z powiązaniami, modyfikując te metadane."
ms.topic: article
ms.prod: xamarin
ms.assetid: 27CB3C16-33F3-F580-E2C0-968005A7E02E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/09/2018
ms.openlocfilehash: 70f17b6bc8dc991534cdf4dd065c813aa0e27e96
ms.sourcegitcommit: d450ae06065d8f8c80f3588bc5a614cfd97b5a67
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/21/2018
---
# <a name="java-bindings-metadata"></a>Metadane powiązania Java

_Kod C# w Xamarin.Android wywołuje bibliotek języka Java za pomocą powiązania, które są mechanizm abstracts szczegółów niskiego poziomu, które są określone w języku Java natywnego interfejsu (JNI). Xamarin.Android udostępnia narzędzia, który generuje tych powiązań. Tego narzędzia umożliwia deweloperowi kontroli, jak powiązania jest tworzona przy użyciu metadanych, dzięki czemu procedury przykład: modyfikacja przestrzenie nazw oraz zmiana nazwy elementów członkowskich. Ten dokument w tym artykule omówiono, jak metadanych działa, znajduje się podsumowanie atrybuty metadane obsługuje i wyjaśniono, jak rozwiązać problemy z powiązaniami, modyfikując te metadane._


## <a name="overview"></a>Omówienie

Xamarin.Android **biblioteka języka Java powiązania** spróbuje automatycznie wykonuje sporą część pracy niezbędne do powiązania istniejącej biblioteki systemu Android za pomocą narzędzia, czasami znana jako funkcja _Generator powiązania_. Podczas tworzenia wiązania biblioteka języka Java, Xamarin.Android będzie sprawdzać klas Java i generować listę wszystkich pakietów, typy i elementy Członkowskie które może być powiązane. Wykaz interfejsów API są przechowywane w pliku XML, który można znaleźć w folderze  **\{projektu directory}\obj\Release\api.xml** dla **wersji** kompilacji i  **\{projektu Directory}\obj\Debug\api.XML** dla **debugowania** kompilacji.

![Lokalizacja pliku api.xml w folderze obj./Debug](java-bindings-metadata-images/java-bindings-metadata-01.png)

Użyje Generator powiązania **api.xml** pliku jako wskazówki podczas generowania niezbędne klasy otoki C#. Zawartość tego pliku XML jest odmianą firmy Google _Android Otwórz projekt źródłowy_ format.
Poniższy fragment jest przykładem zawartość **api.xml**:

```xml
<api>
    <package name="android">
        <class abstract="false" deprecated="not deprecated" extends="java.lang.Object"
            extends-generic-aware="java.lang.Object" 
            final="true" 
            name="Manifest" 
            static="false" 
            visibility="public">
            <constructor deprecated="not deprecated" final="false"
                name="Manifest" static="false" type="android.Manifest"
                visibility="public">
            </constructor>
        </class>
...
</api>
```

W tym przykładzie **api.xml** deklaruje klasę w `android` pakiet o nazwie `Manifest` który rozszerza `java.lang.Object`.

W wielu przypadkach pomocy człowieka jest wymagana, aby interfejsu API języka Java, możesz więcej ".NET takich jak" lub aby rozwiązać problemy, które uniemożliwiają zestawu powiązania z kompilacji. Na przykład może być konieczna zmiana nazwy pakietów języka Java do przestrzeni nazw .NET, Zmień nazwę klasy lub zmienić zwracany typ metody.

Te zmiany nie zostaną osiągnięte, modyfikując **api.xml** bezpośrednio.
Zamiast tego zmiany są zapisywane w specjalne pliki XML udostępnianych przez szablon biblioteki powiązanie Java. Podczas kompilowania zestawu powiązania Xamarin.Android, Generator powiązania mają wpływ tych plików mapowania podczas tworzenia powiązania zestawu

Pliki mapowania XML można znaleźć w **przekształca** folderu projektu:

-   **MetaData.xml** &ndash; pozwalają na zmiany do końcowego interfejsu API, np. zmiana nazw wygenerowanych powiązania. 

-   **EnumFields.xml** &ndash; zawiera mapowanie między Java `int` stałe i C# `enums` . 

-   **EnumMethods.xml** &ndash; umożliwia zmianę metody parametry i typy zwracane w języku Java `int` stałe dla C# `enums` . 

**MetaData.xml** plik jest najbardziej importowania tych plików, który zezwala ogólnego przeznaczenia zmiany do powiązania, takich jak:

-   Zmiana nazwy przestrzeni nazw, klasy, metody lub pola, więc one zgodne z konwencjami .NET. 

-   Usuwanie przestrzeni nazw, klasy, metody lub pola, które nie są potrzebne. 

-   Przenoszenie klasy w różnych przestrzeniach nazw. 

-   Dodawanie klasy obsługi dodatkowych do projektu powiązania wykonaj .NET framework wzorce. 

Pozwala przejść do omówienia **Metadata.xml** bardziej szczegółowo.


## <a name="metadataxml-transform-file"></a>Plik transformacji METADATA.XML

Jak zostało już dowiedzieliśmy się, plik **Metadata.xml** jest używany przez Generator powiązania wpływ tworzenia zestawu powiązania.
Używa formatu metadanych [XPath](https://www.w3.org/TR/xpath/) składni i jest niemal identyczna z *metadanych GAPI* opisanego w [metadanych GAPI](http://www.mono-project.com/docs/gui/gtksharp/gapi/#metadata) przewodnik. Ta implementacja jest prawie pełna implementacja XPath 1.0 i w związku z tym obsługuje elementów w standardowe 1.0. Ten plik jest zaawansowanym mechanizmem XPath na podstawie go zmienić, dodawanie, Ukryj ani przenieść dowolny element lub atrybut w pliku interfejsu API. Wszystkie elementy reguły w specyfikacji metadanych obejmują atrybut ścieżki, aby zidentyfikować węzeł, do którego reguła ma być stosowany. Reguły są stosowane w następującej kolejności:

* **Dodaj węzeł** &ndash; dołącza elementem podrzędnym węzła określony przez atrybut ścieżki.
* **attr** &ndash; ustawia wartość atrybutu element określony przez atrybut ścieżki.
* **Usuń węzeł** &ndash; usuwa węzłów pasujących do określonego języka XPath.

Poniżej przedstawiono przykład **Metadata.xml** pliku:

```xml
<metadata>
    <!-- Normalize the namespace for .NET -->
    <attr path="/api/package[@name='com.evernote.android.job']" 
        name="managedName">Evernote.AndroidJob</attr>

    <!-- Don't  need these packages for the Xamarin binding/public API --> 
    <remove-node path="/api/package[@name='com.evernote.android.job.v14']" />
    <remove-node path="/api/package[@name='com.evernote.android.job.v21']" />

    <!-- Change a parameter name from the generic p0 to a more meaningful one. -->
    <attr path="/api/package[@name='com.evernote.android.job']/class[@name='JobManager']/method[@name='forceApi']/parameter[@name='p0']" 
        name="name">api</attr>
</metadata>
```

Poniższa lista zawiera niektóre z najczęściej używanych elementów XPath dla interfejsu API języka Java:

-   `interface` &ndash; Używana do lokalizowania interfejsu Java. np. `/interface[@name='AuthListener']`.

-   `class` &ndash; Używana do lokalizowania klasy. np. `/class[@name='MapView']`.

-   `method` &ndash; Używana do lokalizowania metodę w klasie Java lub interfejsu. np. `/class[@name='MapView']/method[@name='setTitleSource']`.

-   `parameter` &ndash; Określenie parametru metody. Np. `/parameter[@name='p0']`



### <a name="adding-types"></a>Dodawanie typów

`add-node` Element poinformuje projektu powiązania platformy Xamarin.Android, aby dodać nową klasę otoki do **api.xml**. Na przykład poniższy fragment kodu będzie bezpośrednie powiązanie generatora, aby utworzyć klasę z konstruktorem i jedno pole:

```xml
<add-node path="/api/package[@name='org.alljoyn.bus']">
    <class abstract="false" deprecated="not deprecated" final="false" name="AuthListener.AuthRequest" static="true" visibility="public" extends="java.lang.Object">
        <constructor deprecated="not deprecated" final="false" name="AuthListener.AuthRequest" static="false" type="org.alljoyn.bus.AuthListener.AuthRequest" visibility="public" />
        <field name="p0" type="org.alljoyn.bus.AuthListener.Credentials" />
    </class>
</add-node>
```


### <a name="removing-types"></a>Usuwanie typów

Istnieje możliwość nakazać generatora powiązania platformy Xamarin.Android, aby zignorować typu Java i nie można go powiązać. Jest to realizowane przez dodanie `remove-node` — element XML do **metadata.xml** pliku:

```xml
<remove-node path="/api/package[@name='{package_name}']/class[@name='{name}']" />
```

### <a name="renaming-members"></a>Zmiana nazwy elementów członkowskich

Zmiana nazwy elementów członkowskich nie można przeprowadzić bezpośrednio edytując **api.xml** pliku, ponieważ oryginalne nazwy natywnego interfejsu Java (JNI) wymaga platformy Xamarin.Android. W związku z tym `//class/@name` nie można zmienić atrybutu; Jeśli tak jest, powiązania nie będą działać.

Należy wziąć pod uwagę w przypadku, gdy chcemy, aby zmienić nazwę typu, `android.Manifest`.
W tym celu firma Microsoft może podjąć próbę bezpośredniego edytowania **api.xml** i Zmień nazwę klasy w następujący sposób:

```xml
<attr path="/api/package[@name='android']/class[@name='Manifest']" 
    name="name">NewName</attr>
```

Spowoduje to Generator powiązania tworzenie następujący kod C# dla klasy otoki:

```csharp
[Register ("android/NewName")]
public class NewName : Java.Lang.Object { ... }
```

Należy zauważyć, że klasa otoki została zmieniona na `NewName`, podczas gdy oryginalny typ Java jest nadal `Manifest`. Nie jest już możliwe dla klasy powiązania Xamarin.Android dostępu do żadnych metod na `android.Manifest`; klasy otoki jest powiązany z typem języka Java nie istnieje.

Aby poprawnie zmienić nazwę zarządzanego opakowanej typu (lub metody), należy ustawić `managedName` jak w poniższym przykładzie:

```xml
<attr path="/api/package[@name='android']/class[@name='Manifest']" 
    name="managedName">NewName</attr>
```

<a name="Renaming_EventArg_Wrapper_Classes" />

#### <a name="renaming-eventarg-wrapper-classes"></a>Zmiana nazwy `EventArg` klasy otoki

Gdy identyfikuje generator powiązania Xamarin.Android `onXXX` metody ustawiającej _typu odbiornika_, zdarzenia C# i `EventArgs` podklasy zostanie wygenerowany do obsługi .NET o interfejsu API dla odbiornika opartych na języku Java wzorzec. Na przykład należy wziąć pod uwagę następujące klasy Java i metody:

```xml
com.someapp.android.mpa.guidance.NavigationManager.on2DSignNextManuever(NextManueverListener listener);
```

Xamarin.Android spowoduje porzucenie prefiks `on` z metody ustawiającej — zamiast tego użyj `2DSignNextManuever` jako podstawa dla nazwy `EventArgs` podklasy. Podklasy będą nazwane polecenie podobne do następującego:

```csharp
NavigationManager.2DSignNextManueverEventArgs
```

Nie jest dozwoloną nazwą klasy C#. Aby rozwiązać ten problem, autora powiązania musi używać `argsType` atrybutu i podaj prawidłową nazwę C# dla `EventArgs` podklasy:
 
```xml
<attr path="/api/package[@name='com.someapp.android.mpa.guidance']/
    interface[@name='NavigationManager.Listener']/
    method[@name='on2DSignNextManeuver']" 
    name="argsType">NavigationManager.TwoDSignNextManueverEventArgs</attr>
```

 

## <a name="supported-attributes"></a>Obsługiwanych atrybutów

W poniższych sekcjach opisano niektóre atrybuty do przekształcania interfejsów API języka Java.

### <a name="argstype"></a>argsType

Ten atrybut jest umieszczona na metod ustawiających nazwę `EventArg` podklasy generowany do obsługi odbiorników Java. To jest opisane bardziej szczegółowo poniżej w sekcji [zmiana nazwy klasy otoki EventArg](#Renaming_EventArg_Wrapper_Classes) później w tym przewodniku.

### <a name="eventname"></a>eventName

Określa nazwę zdarzenia. W przypadku braku powstrzymuje generowania zdarzeń.
To jest opisany bardziej szczegółowo w tytułach sekcji [zmiana nazwy klasy otoki EventArg](#Renaming_EventArg_Wrapper_Classes).

### <a name="managedname"></a>managedName

Służy do zmiany nazwy pakietu, klasy, metody lub parametru. Na przykład zmienić nazwę klasy Java `MyClass` do `NewClassName`:

```xml
<attr path="/api/package[@name='com.my.application']/class[@name='MyClass']" 
    name="managedName">NewClassName</attr>
```

Wyrażenie XPath zmianę metody pokazano w przykładzie dalej `java.lang.object.toString` do `Java.Lang.Object.NewManagedName`:

```xml
<attr path="/api/package[@name='java.lang']/class[@name='Object']/method[@name='toString']" 
    name="managedName">NewMethodName</attr>
```

### <a name="managedtype"></a>managedtype —

`managedType` Służy do zmienić zwracany typ metody. W niektórych sytuacjach Generator powiązania niepoprawnie wywnioskuje zwracany typ metody Java, co spowoduje błąd kompilacji. Możliwe rozwiązanie w tej sytuacji jest zmienić zwracany typ metody.

Na przykład Generator powiązania uznaje, że metoda Java `de.neom.neoreadersdk.resolution.compareTo()` powinien zwrócić `int`, które powoduje w komunikacie o błędzie **CS0535 błąd: "DE. Neom.Neoreadersdk.Resolution "nie implementuje elementu członkowskiego interfejsu"Java.Lang.IComparable.CompareTo(Java.Lang.Object)"**. Poniższy fragment kodu pokazano, jak zmienić zwracany typ metody wygenerowanego C# z `int` do `Java.Lang.Object`: 

```xml
<attr path="/api/package[@name='de.neom.neoreadersdk']/
    class[@name='Resolution']/
    method[@name='compareTo' and count(parameter)=1 and
    parameter[1][@type='de.neom.neoreadersdk.Resolution']]/
    parameter[1]"name="managedType">Java.Lang.Object</attr> 
```

### <a name="managedreturn"></a>managedReturn

Zmienia typ zwracany metody. Nie ma to wpływu zwracany atrybutu (jako zmiany, aby zwrócić atrybutów może spowodować zmianę niezgodny podpis JNI). W poniższym przykładzie zwracany typ funkcji `append` metodę zmieniła się z `SpannableStringBuilder` do `IAppendable` (odwołania, że C# nie obsługuje kowariantne typy zwracane):

```xml
<attr path="/api/package[@name='android.text']/
    class[@name='SpannableStringBuilder']/
    method[@name='append']" 
    name="managedReturn">Java.Lang.IAppendable</attr>
```

### <a name="obfuscated"></a>obfuscated

Narzędzia, które zasłaniają bibliotek języka Java może zakłócać Generator powiązanie platformy Xamarin.Android i jego zdolność do generowania klasy otoki C#. Cechy zaciemnionego klas: * Nazwa klasy zawiera  **$** , tj. **$.class** * Nazwa klasy jest całkowicie naruszenia zabezpieczeń z małych liter, tj.  **a.class**

Ta Wstawka kodu przedstawiono przykładowy sposób generowania "bez zaciemnionego" typ C#:

```xml
<attr path="/api/package[@name='{package_name}']/class[@name='{name}']" 
    name="obfuscated">false</attr>
```

### <a name="propertyname"></a>propertyName

Ten atrybut można zmienić nazwę właściwości zarządzanej.

Specjalne przypadek użycia `propertyName` dotyczy sytuacji, gdy klasa Java ma tylko metoda pobierająca dla pola. W takiej sytuacji Generator powiązanie zechce utworzyć właściwości tylko do zapisu, coś, co nie jest zalecane w środowisku .NET. Poniższy fragment kodu przedstawia sposób "Usuń" właściwości platformy .NET przez ustawienie `propertyName` na pusty ciąg:

```xml
<attr path="/api/package[@name='org.java_websocket.handshake']/class[@name='HandshakeImpl1Client']/method[@name='setResourceDescriptor' 
    and count(parameter)=1 
    and parameter[1][@type='java.lang.String']]" 
    name="propertyName"></attr>
<attr path="/api/package[@name='org.java_websocket.handshake']/class[@name='HandshakeImpl1Client']/method[@name='getResourceDescriptor' 
    and count(parameter)=0]" 
    name="propertyName"></attr>
```

Należy pamiętać, że metody ustawiającej i metody pobierającej nadal będzie tworzone przez Generator powiązania.

### <a name="sender"></a>Nadawca

Określa, które parametr metody powinny być `sender` parametru, gdy metoda jest mapowany na zdarzenie. Wartość może być `true` lub `false`. Na przykład:

```xml
<attr path="/api/package[@name='android.app']/
    interface[@name='TimePickerDialog.OnTimeSetListener']/
    method[@name='onTimeSet']/
    parameter[@name='view']" 
    name="sender">true</ attr>
```

### <a name="visibility"></a>widoczność

Ten atrybut służy do zmiany widoczność klasy, metody lub właściwości. Na przykład może być konieczne promować `protected` metoda Java, dzięki czemu jest odpowiadającego C# otoki jest `public`:

```xml
<!-- Change the visibility of a class -->
<attr path="/api/package[@name='namespace']/class[@name='ClassName']" name="visibility">public</attr>

<!-- Change the visibility of a method --> 
<attr path="/api/package[@name='namespace']/class[@name='ClassName']/method[@name='MethodName']" name="visibility">public</attr>
```

## <a name="enumfieldsxml-and-enummethodsxml"></a>EnumFields.xml i EnumMethods.xml

Istnieją przypadki, w którym korzystać z biblioteki systemu Android stałe całkowite do reprezentowania stanów, które są przekazywane do właściwości lub metody bibliotek. W wielu przypadkach warto powiązać te stałe całkowite wyliczeń w języku C#. Aby ułatwić to mapowanie, użyj **EnumFields.xml** i **EnumMethods.xml** pliki w projekcie powiązania. 

### <a name="defining-an-enum-using-enumfieldsxml"></a>Definiowanie wyliczenia za pomocą EnumFields.xml

**EnumFields.xml** plik zawiera mapowania między Java `int` stałe i C# `enums`. Spójrzmy poniższy przykład wyliczenia C# tworzona dla zestawu `int` stałe: 

```xml 
<mapping jni-class="com/skobbler/ngx/map/realreach/SKRealReachSettings" clr-enum-type="Skobbler.Ngx.Map.RealReach.SKMeasurementUnit">
    <field jni-name="UNIT_SECOND" clr-name="Second" value="0" />
    <field jni-name="UNIT_METER" clr-name="Meter" value="1" />
    <field jni-name="UNIT_MILIWATT_HOURS" clr-name="MilliwattHour" value="2" />
</mapping>
```

W tym miejscu przekierowaliśmy Klasa Java `SKRealReachSettings` i definicja wyliczenia C# o nazwie `SKMeasurementUnit` w przestrzeni nazw `Skobbler.Ngx.Map.RealReach`. `field` Wpisy definiuje nazwę stała Java (przykład `UNIT_SECOND`), nazwę wpisu wyliczenia (przykład `Second`) i wartość całkowita reprezentowany przez oba podmioty (przykład `0`). 

### <a name="defining-gettersetter-methods-using-enummethodsxml"></a>Definiowanie metody Getter i Setter za pomocą EnumMethods.xml

**EnumMethods.xml** pliku umożliwia zmianę metody parametry i typy zwracane w języku Java `int` stałe dla C# `enums`. Innymi słowy, mapy odczytu i zapisu wyliczenia C# (zdefiniowany w **EnumFields.xml** pliku) języka Java `int` stałej `get` i `set` metody.

Podane `SKRealReachSettings` wyliczenia zdefiniowanych powyżej, następujące **EnumMethods.xml** pliku zdefiniowane metody pobierającej/ustawiającej dla tego wyliczenia:

```xml
<mapping jni-class="com/skobbler/ngx/map/realreach/SKRealReachSettings">
    <method jni-name="getMeasurementUnit" parameter="return" clr-enum-type="Skobbler.Ngx.Map.RealReach.SKMeasurementUnit" />
    <method jni-name="setMeasurementUnit" parameter="measurementUnit" clr-enum-type="Skobbler.Ngx.Map.RealReach.SKMeasurementUnit" />
</mapping>
```

Pierwszy `method` linii mapy wartość zwracaną Java `getMeasurementUnit` metodę `SKMeasurementUnit` wyliczenia. Drugi `method` linii mapy pierwszy parametr `setMeasurementUnit` do tego samego wyliczenia.

Biorąc pod uwagę te zmiany w miejscu, można użyć kodu wykonaj platformie Xamarin.Android można ustawić `MeasurementUnit`: 

```csharp
realReachSettings.MeasurementUnit = SKMeasurementUnit.Second;
```


## <a name="summary"></a>Podsumowanie

W tym artykule opisano, jak Xamarin.Android używa metadanych do przekształcania definicji interfejsu API z *Google* *AOSP format*. Po obejmujące zmiany, które są możliwe przy użyciu *Metadata.xml*, ją zbadać ograniczenia napotkał podczas zmiany nazwy elementów członkowskich i składane ono listę obsługiwanych atrybutów XML, opisujące, kiedy należy użyć każdego atrybutu.



## <a name="related-links"></a>Linki pokrewne

- [Praca z JNI](~/android/platform/java-integration/working-with-jni.md)
- [Tworzenie powiązań biblioteki języka Java](~/android/platform/binding-java-library/index.md)
- [GAPI Metadata](http://www.mono-project.com/docs/gui/gtksharp/gapi/#metadata)
