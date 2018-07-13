---
title: Ograniczenia platformy Xamarin.iOS
description: W tym dokumencie opisano ograniczenia dotyczące platformy Xamarin.iOS, omawiając ogólne, ogólne podklasy obiektu NSObjects, P/Invokes w obiektach ogólny i nie tylko.
ms.prod: xamarin
ms.assetid: 5AC28F21-4567-278C-7F63-9C2142C6E06A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/09/2018
ms.openlocfilehash: e154e4e1688b8a3d03459956934409fa9d5aef35
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998088"
---
# <a name="limitations-of-xamarinios"></a>Ograniczenia platformy Xamarin.iOS

Ponieważ aplikacje na urządzeniu iPhone przy użyciu rozszerzenia Xamarin.iOS są kompilowane do kodu statycznego, nie jest możliwe użycie dowolnego urządzenia, które wymagają generowanie kodu w czasie wykonywania.

Oto ograniczenia rozszerzenia Xamarin.iOS w porównaniu do programu desktop Mono:

 <a name="Limited_Generics_Support" />


## <a name="limited-generics-support"></a>Obsługa ograniczonych typów ogólnych

W przeciwieństwie do tradycyjnych Mono/.NET na telefonie iPhone statycznie kompilowania kodu wcześniejsze zamiast kompilowany na żądanie za pomocą kompilatora JIT.

Narzędzie mono firmy [pełnej kompilacji AOT](http://www.mono-project.com/docs/advanced/aot/#full-aot) technologii ma kilka ograniczeń w odniesieniu do typów ogólnych, powodem, ponieważ nie wszystkie możliwe podczas tworzenia wystąpienia ogólnego można określić na początku w czasie kompilacji. Nie jest to problem dla środowisk uruchomieniowych regularne .NET lub platformy Mono, ponieważ kod zawsze jest kompilowany w czasie wykonywania przy użyciu tylko w kompilatorze czasu. Ale to stanowi wyzwanie dla statycznych kompilatora, takich jak platformy Xamarin.iOS.

Typowe problemy, które wystąpiły deweloperów, należą:

 <a name="Generic_Subclasses_of_NSObjects_are_limited" />


### <a name="generic-subclasses-of-nsobjects-are-limited"></a>Ogólne podklasy NSObjects są ograniczone

Obecnie Xamarin.iOS ma ograniczoną obsługę tworzenia ogólne podklasy obiektu NSObject klasy, takie jak metody ogólne nie są obsługiwane. Począwszy od 7.2.1 za pomocą ogólne podklasy obiektu NSObjects jest to możliwe, podobny do poniższego:

```csharp
class Foo<T> : UIView {
    [..]
}
```

> [!NOTE]
> Ogólne podklasy obiektu NSObjects są możliwe, istnieją pewne ograniczenia. Odczyt [ogólne podklasy obiektu NSObject](~/ios/internals/api-design/nsobject-generics.md) dokumentu, aby uzyskać więcej informacji



### <a name="pinvokes-in-generic-types"></a>P/Invoke w typach ogólnych

P/Invokes w klasy ogólne nie są obsługiwane:

```csharp
class GenericType<T> {
    [DllImport ("System")]
    public static extern int getpid ();
}
```

 <a name="Property.SetInfo_on_a_Nullable_Type_is_not_supported" />


### <a name="propertysetinfo-on-a-nullable-type-is-not-supported"></a>Property.SetInfo na typ dopuszczający wartość null nie jest obsługiwane.

Przy użyciu odbicia firmy Property.SetInfo można ustawić wartości na Nullable&lt;T&gt; nie jest obecnie obsługiwane.

 <a name="Value_types_as_Dictionary_Keys" />


### <a name="value-types-as-dictionary-keys"></a>Typy wartości jako klucze słownika

Za pomocą typu wartości w formie słownika&lt;TKey, TValue&gt; klucza jest problematyczne, jako domyślny konstruktor słownika podejmują próbę użycia EqualityComparer&lt;TKey&gt;. Domyślnie. EqualityComparer&lt;TKey&gt;. Ustawienie domyślne, z kolei próbuje używać odbicia w celu utworzenia wystąpienia nowego typu, która implementuje IEqualityComparer&lt;TKey&gt; interfejsu.

Działa to w przypadku typów referencyjnych (jako odbicie + Utwórz nowy typ krok zostanie pominięty), ale dla wartości typów awarii i spala dość szybko po użytkownik podejmie próbę użycia go na urządzeniu.

 **Obejście**: ręcznie wdrożyć [IEqualityComparer&lt;TKey&gt; ](xref:System.Collections.Generic.IEqualityComparer`1) interfejs nowego typu i zapewnić wystąpienie tego typu [słownika&lt;TKey, TValue&gt; ](xref:System.Collections.Generic.Dictionary`2) [(IEqualityComparer&lt;TKey&gt;)](xref:System.Collections.Generic.IEqualityComparer`1) konstruktora.


 <a name="No_Dynamic_Code_Generation" />


## <a name="no-dynamic-code-generation"></a>Generowanie kodu dynamicznej nie jest

Ponieważ jądra dla telefonu iPhone zapobiega aplikację dynamiczne generowanie kodu platformy Mono na telefonie iPhone nie obsługuje dowolnej formie generowania kodu dynamicznych. Należą do nich następujące elementy:

-  System.Reflection.Emit nie jest dostępna.
-  Brak obsługi System.Runtime.Remoting.
-  Dynamiczne tworzenie typów nie są obsługiwane (nie Type.GetType ("MyType" 1")), mimo że wyszukiwanie istniejących typów (Type.GetType ("System.String") na przykład działa poprawnie). 
-  Zwrotny wywołania zwrotne musi być zarejestrowana do aparatu plików wykonywalnych w czasie kompilacji.


 
 <a name="System.Reflection.Emit" />


### <a name="systemreflectionemit"></a>System.Reflection.Emit

Brak System.Reflection. **Emituj** oznacza, że będzie działać bez kodu, który zależy od środowiska uruchomieniowego generowania kodu. Obejmuje to np.:

-  Środowisko uruchomieniowe języka dynamicznego.
-  Wszystkie języki, zbudowany na podstawie środowiska Dynamic Language Runtime.
-  TransparentProxy firmy komunikacji zdalnej lub jakichkolwiek innych czynności, które spowodują, że środowisko uruchomieniowe dynamiczne generowanie kodu. 


 **Ważne:** nie należy mylić **Reflection.Emit** z **odbicia**. Reflection.Emit o dynamiczne generowanie kodu oraz że kod JITed i skompilowanych do natywnego kodu. Ze względu na ograniczenia na urządzeniu iPhone (nie kompilacja JIT) nie jest to obsługiwane.

Ale całego interfejsu API odbicia, łącznie z Type.GetType ("someClass"), listę metod, listę właściwości, pobieranie, atrybuty i wartości działa prawidłowo.

### <a name="using-delegates-to-call-native-functions"></a>Używanie delegatów, aby wywoływać funkcje natywne

Aby wywołać funkcji macierzystej za pośrednictwem pełnomocnika C#, deklaracja delegata musi posiadać przy użyciu jednego z następujących atrybutów:

- [UnmanagedFunctionPointerAttribute](xref:System.Runtime.InteropServices.UnmanagedFunctionPointerAttribute) (preferowane, ponieważ jest dla wielu platform i są zgodne z .NET Standard 1.1 +.)
- [MonoNativeFunctionWrapperAttribute](https://developer.xamarin.com/api/type/ObjCRuntime.MonoNativeFunctionWrapperAttribute)

Nie można podać jedną z tych atrybutów spowoduje błąd środowiska uruchomieniowego, takie jak:

```
System.ExecutionEngineException: Attempting to JIT compile method '(wrapper managed-to-native) YourClass/YourDelegate:wrapper_aot_native(object,intptr,intptr)' while running in aot-only mode.
```
 
 <a name="Reverse_Callbacks" />


### <a name="reverse-callbacks"></a>Zwrotny wywołania zwrotne

W rozwiązaniu standard Mono istnieje możliwość przekazania wystąpień delegata w języku C# do kodu niezarządzanego audytów wskaźnika funkcji. Środowisko uruchomieniowe będzie zazwyczaj przekształcający te wskaźniki funkcji thunk mały, umożliwiająca kodu niezarządzanego do wywołania zwrotnego w kodzie zarządzanym.

W rozwiązaniu Mono mostków te są implementowane przez Just-in-Time kompilatora. Podczas używania kompilatora ahead of time wymaganych przez telefon iPhone na tym etapie istnieją dwie ważne ograniczenia:

-  Musi wszystkich metod wywołania zwrotnego z flagą [MonoPInvokeCallbackAttribute](https://developer.xamarin.com/api/type/ObjCRuntime.MonoPInvokeCallbackAttribute) 
-  Metody musi być metody statyczne, nie jest obsługiwane dla wystąpienia metody. 
 
<a name="No_Remoting" />

## <a name="no-remoting"></a>Nie komunikacji zdalnej

Stos komunikacji zdalnej nie jest dostępna na platformy Xamarin.iOS.


 <a name="Runtime_Disabled_Features" />


## <a name="runtime-disabled-features"></a>Środowisko uruchomieniowe wyłączone funkcje

Następujące funkcje zostały wyłączone w systemie iOS Mono w czasie wykonywania:

-  Profiler
-  Reflection.Emit
-  Funkcje Reflection.Emit.Save
-  Wiązania modelu COM
-  Aparatu JIT
-  Weryfikator metadanych (ponieważ nie istnieje żadne JIT)


 <a name=".NET_API_Limitations" />


## <a name="net-api-limitations"></a>Ograniczenia interfejsu API platformy .NET

Interfejs API platformy .NET, ujawnione jest podzbiorem pełnej framework, nie wszystko, co jest dostępna w systemie iOS. Zobacz często zadawane pytania dotyczące [listę aktualnie obsługiwanych zestawów](~/cross-platform/internals/available-assemblies.md).



W szczególności profilu interfejsu API Xamarin.iOS nie ma System.Configuration, więc nie jest możliwe użyć zewnętrznych plików XML do konfiguracji zachowanie aparatu plików wykonywalnych.
