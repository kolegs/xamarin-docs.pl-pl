---
title: Łączenie w systemie Android
ms.topic: article
ms.prod: xamarin
ms.assetid: 3528E195-AA74-90AF-B5F3-3B65FB4F0BB8
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/30/2018
ms.openlocfilehash: 1354636c5190362e0782e5d7a42c4c2f8a9ef9cb
ms.sourcegitcommit: 4f1b508caa8e7b6ccf85d167ea700a5d28b0347e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/03/2018
---
# <a name="linking-on-android"></a>Łączenie w systemie Android

Użyj aplikacji platformy Xamarin.Android *konsolidatora* Aby zmniejszyć rozmiar aplikacji. Analizy statycznej employes konsolidatora aplikacji w celu określenia zestawy, które są rzeczywiście używane typy, które są rzeczywiście używane i elementów członkowskich, które są rzeczywiście używane. Następnie konsolidator zachowuje się jak *modułu zbierającego elementy bezużyteczne*stale poszukujący zestawy, typy i elementy członkowskie, które odwołują się do całego zamknięcia zestawów występujących w odwołaniach, typów, i znajduje się elementy członkowskie. To wszystko poza tym zamknięcia *odrzucone*.

Na przykład [Hello, Android](https://developer.xamarin.com/samples/HelloM4A/) próbki:

|Konfiguracja|1.2.0 rozmiar|4.0.1 rozmiar|
|---|---|---|
|Wersja bez konsolidacji:|14.0 MB|16.0 MB|
|Zlecenia przy użyciu połączeń:|4.2 MB|2.9 MB|

Łączenie wyniki w pakiecie, który wynosi 30% rozmiaru oryginalnego pakietu (odłączyć) w wersji 1.2.0 i 18% odłączyć pakietu w 4.0.1.



## <a name="control"></a>Formant

Łączenia jest oparta na *analizy statycznej*. W rezultacie wszystkie elementy, które zależy od środowiska uruchomieniowego nie zostanie wykryty:

```csharp
// To play along at home, Example must be in a different assembly from MyActivity.
public class Example {
    // Compiler provides default constructor...
}

[Activity (Label="Linker Example", MainLauncher=true)]
public class MyActivity {
    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        // Will this work?
        var o = Activator.CreateInstance (typeof (ExampleLibrary.Example));
    }
}
```


### <a name="linker-behavior"></a>Linker Behavior

Podstawowy mechanizm kontroli konsolidator **zachowanie konsolidatora** (*konsolidację* w programie Visual Studio) listy rozwijanej w **opcje projektu** okno dialogowe. Dostępne są trzy opcje:

1.  **Nie zawierają łącza** (*Brak* w programie Visual Studio)
1.  **Łączenie zestawów SDK** (*tylko zestawy Sdk*)
1.  **Połącz wszystkie zestawy** (*zestawu Sdk i zestawów użytkownika*)


**Łącze nie** opcja powoduje wyłączenie konsolidator; powyższy przykład rozmiar "Wersji bez konsolidacji" aplikacji używać tego zachowania. Jest to przydatne podczas rozwiązywania problemów błędy czasu wykonywania, czy konsolidator jest odpowiedzialny. To ustawienie nie jest zwykle zalecane dla kompilacji w środowisku produkcyjnym.

**Zestawy SDK łącze** opcję tylko w łączach [zestawy, które pochodzą z platformy Xamarin.Android](~/cross-platform/internals/available-assemblies.md).
Wszystkie zestawy (na przykład kodu) nie są połączone.

**Łączy wszystkie zestawy** opcja łączy wszystkie zestawy, co oznacza, że kod również mogą zostać usunięte, jeśli istnieją żadnych statycznych odwołań.

Powyższy przykład będzie działać z *łącze nie* i *zestawy SDK łącze* opcje i zakończy się niepowodzeniem z *łączy wszystkie zestawy* zachowanie generowania następujący błąd :

```shell
E/mono    (17755): [0xafd4d440:] EXCEPTION handling: System.MissingMethodException: Default constructor not found for type ExampleLibrary.Example.
I/MonoDroid(17755): UNHANDLED EXCEPTION: System.MissingMethodException: Default constructor not found for type ExampleLibrary.Example.
I/MonoDroid(17755): at System.Activator.CreateInstance (System.Type,bool) <0x00180>
I/MonoDroid(17755): at System.Activator.CreateInstance (System.Type) <0x00017>
I/MonoDroid(17755): at LinkerScratch2.Activity1.OnCreate (Android.OS.Bundle) <0x00027>
I/MonoDroid(17755): at Android.App.Activity.n_OnCreate_Landroid_os_Bundle_ (intptr,intptr,intptr) <0x00057>
I/MonoDroid(17755): at (wrapper dynamic-method) object.95bb4fbe-bef8-4e5b-8e99-ca83a5d7a124 (intptr,intptr,intptr) <0x00033>
E/mono    (17755): [0xafd4d440:] EXCEPTION handling: System.MissingMethodException: Default constructor not found for type ExampleLibrary.Example.
E/mono    (17755):
E/mono    (17755): Unhandled Exception: System.MissingMethodException: Default constructor not found for type ExampleLibrary.Example.
E/mono    (17755):   at System.Activator.CreateInstance (System.Type type, Boolean nonPublic) [0x00000] in <filename unknown>:0
E/mono    (17755):   at System.Activator.CreateInstance (System.Type type) [0x00000] in <filename unknown>:0
E/mono    (17755):   at LinkerScratch2.Activity1.OnCreate (Android.OS.Bundle bundle) [0x00000] in <filename unknown>:0
E/mono    (17755):   at Android.App.Activity.n_OnCreate_Landroid_os_Bundle_ (IntPtr jnienv, IntPtr native__this, IntPtr native_savedInstanceState) [0x00000] in <filename unknown>:0
E/mono    (17755):   at (wrapper dynamic-method) object:95bb4fbe-bef8-4e5b-8e99-ca83a5d7a124 (intptr,intptr,intptr)
```


### <a name="preserving-code"></a>Zachowywanie kodu

Konsolidator czasami spowoduje usunięcie kod, który chcesz zachować. Na przykład:

-   Masz kod, który należy wywołać dynamicznie za pośrednictwem `System.Reflection.MemberInfo.Invoke`.

-   Można utworzyć wystąpienia dynamicznie typy, można zachować domyślnego konstruktora dla typów.

-   Jeśli używasz serializacji XML, można zachować właściwości typów.

W takich przypadkach można użyć [Android.Runtime.Preserve](https://developer.xamarin.com/api/type/Android.Runtime.PreserveAttribute/) atrybutu. Każdy członek, który nie jest połączony statycznie przez aplikację jest warunkiem usunięte, więc ten atrybut może służyć do oznaczania elementów członkowskich, które nie są wywoływane statycznie, ale są nadal wymagane przez aplikację. Ten atrybut można stosować do każdego członka typu lub samego typu.

W poniższym przykładzie ten atrybut służy do zachowania konstruktora `Example` klasy:

```csharp
public class Example
{
    [Android.Runtime.Preserve]
    public Example ()
    {
    }
}
```

Jeśli chcesz zachować całą typu za pomocą następujących składni atrybutu:

```csharp
[Android.Runtime.Preserve (AllMembers = true)]
```

Na przykład w poniższym kodzie fragmentu całą `Example` klasy są zachowywane na potrzeby serializacji XML:

```csharp
[Android.Runtime.Preserve (AllMembers = true)]
class Example
{
    // Compiler provides default constructor...
}
```

Czasami chcesz zachować niektóre elementy członkowskie, ale tylko wtedy, jeśli typ zawierający została zachowana. W takim przypadku należy użyć następującej składni atrybutu:

```csharp
[Android.Runtime.Preserve (Conditional = true)]
```

Jeśli nie chcesz przełączyć zależności w bibliotekach Xamarin &ndash; na przykład tworzysz Biblioteka klas przenośnych dla wielu platform (PCL) &ndash; można nadal używać `Android.Runtime.Preserve` atrybutu. Aby to zrobić, należy zadeklarować `PreserveAttribute` klas w ramach `Android.Runtime` przestrzeni nazw w następujący sposób:

```csharp
namespace Android.Runtime
{
    public sealed class PreserveAttribute : System.Attribute
    {
        public bool AllMembers;
        public bool Conditional;
    }
}
```

W powyższych przykładach `Preserve` atrybut jest zadeklarowany w `Android.Runtime` przestrzeni nazw; można jednak użyć `Preserve` atrybutu w przestrzeni nazw, ponieważ konsolidator odwołuje się do tego atrybutu o nazwie typu.



### <a name="falseflag"></a>falseflag

Jeśli nie można użyć atrybutu [Preserve], często jest przydatne zapewnienie bloku kodu, tak aby konsolidator uznaje się, że typ jest używany podczas uniemożliwia bloku kodu wykonywana w czasie wykonywania. Aby użyć tej techniki, firma Microsoft może wykonać:

```csharp
[Activity (Label="Linker Example", MainLauncher=true)]
class MyActivity {

#pragma warning disable 0219, 0649
    static bool falseflag = false;
    static MyActivity ()
    {
        if (falseflag) {
            var ignore = new Example ();
        }
    }
#pragma warning restore 0219, 0649

    // ...
}
```



### <a name="linkskip"></a>linkskip

Istnieje możliwość określenia, czy zestaw zestawy dostarczane przez użytkownika nie powinna być łączona, podczas gdy inne zestawy użytkownika do pominięcia z *zestawy SDK łącze* zachowanie przy użyciu [AndroidLinkSkip Właściwości MSBuild](~/android/deploy-test/building-apps/build-process.md):

```xml
<PropertyGroup>
    <AndroidLinkSkip>Assembly1;Assembly2</AndroidLinkSkip>
</PropertyGroup>
```


### <a name="linkdescription"></a>LinkDescription

[ `@(LinkDescription)` ](~/android/deploy-test/building-apps/build-process.md) 
 **Akcja kompilacji** może być używany dla plików, które mogą zawierać [pliku konfiguracyjnego konsolidatora niestandardowy](~/cross-platform/deploy-test/linker.md).
plik. Pliki konfiguracji niestandardowej konsolidator może być konieczne przeprowadzenie zachować `internal` lub `private` elementów członkowskich, które muszą zostać zachowane.



### <a name="custom-attributes"></a>Atrybuty niestandardowe

Połączonego zestawu następujące typy atrybutów niestandardowych zostaną usunięte ze wszystkich członków:

-  System.ObsoleteAttribute
-  System.MonoDocumentationNoteAttribute
-  System.MonoExtensionAttribute
-  System.MonoInternalNoteAttribute
-  System.MonoLimitationAttribute
-  System.MonoNotSupportedAttribute
-  System.MonoTODOAttribute
-  System.Xml.MonoFIXAttribute


Jeśli zestaw jest połączony, następującym atrybutem niestandardowym typy zostaną usunięte ze wszystkich elementów członkowskich w wersji kompilacji:

-  System.Diagnostics.DebuggableAttribute
-  System.Diagnostics.DebuggerBrowsableAttribute
-  System.Diagnostics.DebuggerDisplayAttribute
-  System.Diagnostics.DebuggerHiddenAttribute
-  System.Diagnostics.DebuggerNonUserCodeAttribute
-  System.Diagnostics.DebuggerStepperBoundaryAttribute
-  System.Diagnostics.DebuggerStepThroughAttribute
-  System.Diagnostics.DebuggerTypeProxyAttribute
-  System.Diagnostics.DebuggerVisualizerAttribute


## <a name="related-links"></a>Linki pokrewne

- [Konfiguracja konsolidatora niestandardowego](~/cross-platform/deploy-test/linker.md)
- [Łączenie w systemie iOS](~/ios/deploy-test/linker.md)
