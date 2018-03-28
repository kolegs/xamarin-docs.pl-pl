---
title: Ograniczenia
ms.topic: article
ms.prod: xamarin
ms.assetid: 5AC28F21-4567-278C-7F63-9C2142C6E06A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: c099797f0687f198ed220c1bd366bd93ab6c6e99
ms.sourcegitcommit: 20ca85ff638dbe3a85e601b5eb09b2f95bda2807
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/28/2018
---
# <a name="limitations"></a>Ograniczenia

Ponieważ aplikacje na telefonie iPhone, przy użyciu platformy Xamarin.iOS są kompilowane do statycznego kodu, nie jest możliwe używać żadnych urządzeń, które wymagają generowanie kodu w czasie wykonywania.

Ograniczenia platformy Xamarin.iOS w porównaniu do pulpitu Mono są to:

 <a name="Limited_Generics_Support" />


## <a name="limited-generics-support"></a>Obsługa ograniczona ogólne

W przeciwieństwie do tradycyjnych Mono/.NET na telefonach iPhone statycznie kompilowania kodu wcześniejsze zamiast kompilowany na żądanie za pomocą kompilatora JIT.

W mono [pełnego drzewa obiektów aplikacji](http://www.mono-project.com/docs/advanced/aot/#full-aot) technologii ma kilka ograniczeń względem ogólne, powodem, ponieważ nie wszystkie możliwe konkretyzacja rodzajowa można określić góry w czasie kompilacji. Nie jest problemem w regularnych środowisk uruchomieniowych .NET lub Mono, ponieważ kod jest zawsze kompilowane w czasie wykonywania za pomocą tylko w kompilatorze czasu. Jednak to stanowi wyzwanie dla statycznych kompilatora, takich jak platformy Xamarin.iOS.

Typowe problemy, które wystąpiły deweloperów, między innymi:

 <a name="Generic_Subclasses_of_NSObjects_are_limited" />


### <a name="generic-subclasses-of-nsobjects-are-limited"></a>Podklasy NSObjects ogólnego są ograniczone

Obecnie Xamarin.iOS ma ograniczoną obsługę tworzenia ogólnego podklasami klasy NSObject, takich jak metody rodzajowe nie są obsługiwane. Począwszy od 7.2.1 przy użyciu ogólnych podklasy NSObjects jest to możliwe, takiego jak to:

```csharp
class Foo<T> : UIView {
    [..]
}
```

> [!NOTE]
> Ogólny podklasy NSObjects są możliwe, istnieje kilka ograniczeń. Odczyt [ogólnego podklasy NSObject](~/ios/internals/api-design/nsobject-generics.md) dokumentu, aby uzyskać więcej informacji



### <a name="pinvokes-in-generic-types"></a>P/Invoke w typach ogólnych

P/Invoke w klasach ogólnych nie są obsługiwane:

```csharp
class GenericType<T> {
    [DllImport ("System")]
    public static extern int getpid ();
}
```

 <a name="Property.SetInfo_on_a_Nullable_Type_is_not_supported" />


### <a name="propertysetinfo-on-a-nullable-type-is-not-supported"></a>Property.SetInfo na typ dopuszczający wartość null nie jest obsługiwane.

Należy ustawić wartość dla typu Nullable przy użyciu odbicia w Property.SetInfo&lt;T&gt; nie jest obecnie obsługiwany.

 <a name="Value_types_as_Dictionary_Keys" />


### <a name="value-types-as-dictionary-keys"></a>Typy wartości jako klucze słownika

Przy użyciu typu wartości w formie słownika&lt;TKey, TValue&gt; klucza jest problematyczne, jako domyślny konstruktor słownika próbuje użyć EqualityComparer&lt;TKey&gt;. Domyślne. EqualityComparer&lt;TKey&gt;. Domyślnie z kolei próbuje użyć odbicia można utworzyć nowy typ, który implementuje IEqualityComparer&lt;TKey&gt; interfejsu.

Działa to w przypadku typów odwołań (jako odbicie + Utwórz nowy typ krok zostanie pominięty), ale dla wartości typów jego awarii (Crash) i zapisze raczej szybkie, gdy próba użycia go na urządzeniu.

 **Obejście**: ręcznie zaimplementować [IEqualityComparer&lt;TKey&gt; ](https://developer.xamarin.com/api/type/System.Collections.Generic.IEqualityComparer%601/) interfejsu w nowym typem i zapewnić wystąpienie tego typu [słownika&lt;TKey, TValue&gt; ](https://developer.xamarin.com/api/type/System.Collections.Generic.Dictionary%3CTKey,TValue%3E/) [(IEqualityComparer&lt;TKey&gt;)](https://developer.xamarin.com/api/type/System.Collections.Generic.IEqualityComparer%601/) konstruktora.


 <a name="No_Dynamic_Code_Generation" />


## <a name="no-dynamic-code-generation"></a>Generowanie kodu dynamiczne nie

Ponieważ jądra iPhone uniemożliwia dynamiczne generowanie kodu Mono na telefonie iPhone nie obsługuje dowolnej formy generowania kodu dynamicznych. Należą do nich następujące elementy:

-  System.Reflection.Emit nie jest dostępna.
-  Brak obsługi System.Runtime.Remoting.
-  Dynamiczne tworzenie typów nie są obsługiwane (nie Type.GetType ("Mojtyp" 1")), chociaż wyszukiwanie istniejących typów (Type.GetType ("System.String") na przykład działa tylko drobne). 
-  Wywołania zwrotne odwrotnej musi być zarejestrowany środowiska uruchomieniowego w czasie kompilacji.


 
 <a name="System.Reflection.Emit" />


### <a name="systemreflectionemit"></a>System.Reflection.Emit

Brak System.Reflection. **Emituj** oznacza, że będzie działać nie kod, który jest zależny od generowanie kodu w czasie wykonywania. Obejmuje to np.:

-  Środowisko uruchomieniowe języka dynamicznego.
-  Wszystkie języki, rozszerzający środowiska Dynamic Language Runtime.
-  TransparentProxy w zdalnych lub jakikolwiek inny spowodowałoby środowiska uruchomieniowego dynamiczne generowanie kodu. 


 **Ważne:** nie należy mylić **Reflection.Emit** z **odbicia**. Reflection.Emit jest o dynamiczne generowanie kodu i ten kod JITed i skompilowanych do natywnego kodu. Ze względu na ograniczenia na telefonie iPhone (nie kompilacji JIT) nie jest to obsługiwane.

Ale cały API odbicia, w tym Type.GetType ("someClass"), listę metod, listę właściwości, pobieranie, atrybuty i wartości działa bez problemu.

 
 <a name="Reverse_Callbacks" />


### <a name="reverse-callbacks"></a>Wywołania zwrotne wstecznego

W standardowe Mono jest możliwe do przekazania do kodu niezarządzanego zamiast wskaźnik funkcji wystąpienia delegata C#. Środowisko wykonawcze będzie zwykle przekształcania tych wskaźników funkcji małych thunk, umożliwiający kodu niezarządzanego do wywołania zwrotnego do kodu zarządzanego.

W Mono mostków te są implementowane przez Just in Time kompilatora. Jeśli przy użyciu kompilatora z wyprzedzeniem o czasie wymagane przez telefonów iPhone się, że na tym etapie istnieją dwie ważne ograniczenia:

-  Należy wszystkie metody wywołania zwrotnego z flagą [MonoPInvokeCallbackAttribute](https://developer.xamarin.com/api/type/ObjCRuntime.MonoPInvokeCallbackAttribute) 
-  Metody musi być metody statyczne, nie jest obsługiwane dla wystąpienia metody. 
 
<a name="No_Remoting" />

## <a name="no-remoting"></a>Nie komunikacji zdalnej

Stos usług zdalnych nie jest dostępna na platformy Xamarin.iOS.


 <a name="Runtime_Disabled_Features" />


## <a name="runtime-disabled-features"></a>Środowisko uruchomieniowe wyłączone funkcje

Następujące funkcje zostały wyłączone w systemie iOS Mono w czasie wykonywania:

-  Profiler
-  Reflection.Emit
-  Funkcje Reflection.Emit.Save
-  Powiązania modelu COM
-  Aparat JIT
-  Metadane weryfikatora (ponieważ nie istnieje żadne JIT)


 <a name=".NET_API_Limitations" />


## <a name="net-api-limitations"></a>Ograniczenia interfejsu API platformy .NET

Interfejs API .NET widoczne jest podzbiorem pełnej framework nie wszystko, co jest dostępne w systemie iOS. Zobacz często zadawane pytania dotyczące [listę aktualnie obsługiwanych zestawów](~/cross-platform/internals/available-assemblies.md).



W szczególności profilu interfejsu API używany przez Xamarin.iOS nie ma System.Configuration, więc nie można używać zewnętrznych plików XML można konfigurować zachowanie środowiska uruchomieniowego.
