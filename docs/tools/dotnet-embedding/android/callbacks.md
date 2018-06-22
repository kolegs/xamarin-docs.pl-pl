---
title: Wywołania zwrotne w systemie Android
ms.prod: xamarin
ms.assetid: F3A7A4E6-41FE-4F12-949C-96090815C5D6
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 3f18516643c00dc67fe533ecab00e1f415eb5c46
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/09/2018
ms.locfileid: "33922825"
---
# <a name="callbacks-on-android"></a>Wywołania zwrotne w systemie Android

Wywołania języka Java w języku C# jest nieco *ryzykowne*. To znaczy istnieje *wzorzec* dla wywołania zwrotne w języku C# Java; Jednakże jest bardziej skomplikowane niż chcielibyśmy.

Trzy opcje do wykonywania wywołań zwrotnych, które są najbardziej odpowiednie dla języka Java omówione zostaną następujące czynności:

- Klasy abstrakcyjne
- Interfejsy
- Metody wirtualne

## <a name="abstract-classes"></a>Klasy abstrakcyjne

Jest najprostszym trasę dla wywołań zwrotnych, dlatego będzie zaleca się `abstract` Jeśli właśnie są próby uzyskania pracy w najprostszej postaci wywołania zwrotnego.

Zacznijmy od klasy C# chcielibyśmy Java implementacji:

```csharp
[Register("mono.embeddinator.android.AbstractClass")]
public abstract class AbstractClass : Java.Lang.Object
{
    public AbstractClass() { }

    public AbstractClass(IntPtr handle, JniHandleOwnership transfer) : base(handle, transfer) { }

    [Export("getText")]
    public abstract string GetText();
}
```

Poniżej przedstawiono szczegóły, aby umożliwić użycie tych wartości:

- `[Register]` generuje nazwę nieuprzywilejowany pakietów w języku Java — otrzymasz nazwą automatycznie generowanej pakietów bez niego.
- Tworzenie podklas `Java.Lang.Object` sygnały do osadzania .NET do uruchamiania klasy przez generator języka Java dla platformy Xamarin.Android.
- Pustego konstruktora: jest należy używać z kodem języka Java.
- `(IntPtr, JniHandleOwnership)` Konstruktor: jest Xamarin.Android użyje do tworzenia C# — równoważne obiektów języka Java.
- `[Export]` sygnalizuje Xamarin.Android do ujawnia metody języka Java. Możemy również zmienić nazwę metody, ponieważ na świecie Java lubi metod małe litery.

Następny upewnijmy metody C# do przetestowania tego scenariusza:

```csharp
[Register("mono.embeddinator.android.JavaCallbacks")]
public class JavaCallbacks : Java.Lang.Object
{
    [Export("abstractCallback")]
    public static string AbstractCallback(AbstractClass callback)
    {
        return callback.GetText();
    }
}
```
`JavaCallbacks` może być dowolną klasę do testowania, tak długo, jak jest `Java.Lang.Object`.

Teraz uruchom osadzanie .NET na Twoje zestawu .NET do generowania AAR. Zobacz [Przewodnik wprowadzający](~/tools/dotnet-embedding/get-started/java/android.md) szczegółowe informacje.

Po zaimportowaniu plików AAR do programu Android Studio, napisz testu jednostkowego:

```java
@Test
public void abstractCallback() throws Throwable {
    AbstractClass callback = new AbstractClass() {
        @Override
        public String getText() {
            return "Java";
        }
    };

    assertEquals("Java", callback.getText());
    assertEquals("Java", JavaCallbacks.abstractCallback(callback));
}
```
Dlatego firma Microsoft:

- Zaimplementowane `AbstractClass` w języku Java z typu anonimowego
- Upewnienie się, zwraca naszych wystąpienie `"Java"` za pomocą języka Java
- Upewnienie się, zwraca naszych wystąpienie `"Java"` w języku C#
- Dodaje `throws Throwable`, ponieważ są obecnie oznaczone konstruktorów C# `throws`

Jeśli wystąpił jednostką przetestuj jako — jest ona nie powiedzie się z powodu błędu takich jak:

```csharp
System.NotSupportedException: Unable to find Invoker for type 'Android.AbstractClass'. Was it linked away?
```

Co to jest brak Oto `Invoker` typu. Jest to podklasa `AbstractClass` który przekazuje dalej wywołania C# języka Java. Jeśli obiekt Java wprowadza world C# i odpowiednik typu C# jest abstrakcyjny, a następnie Xamarin.Android automatycznie wyszukuje typu C# z sufiksem `Invoker` do użytku w kodzie języka C#.

Używa tej wartości platformy Xamarin.Android `Invoker` wzorca dla języka Java projektów powiązanie między innymi.

Oto nasze implementacja `AbstractClassInvoker`:
```csharp
class AbstractClassInvoker : AbstractClass
{
    IntPtr class_ref, id_gettext;

    public AbstractClassInvoker(IntPtr handle, JniHandleOwnership transfer) : base(handle, transfer)
    {
        IntPtr lref = JNIEnv.GetObjectClass(Handle);
        class_ref = JNIEnv.NewGlobalRef(lref);
        JNIEnv.DeleteLocalRef(lref);
    }

    protected override Type ThresholdType
    {
        get { return typeof(AbstractClassInvoker); }
    }

    protected override IntPtr ThresholdClass
    {
        get { return class_ref; }
    }

    public override string GetText()
    {
        if (id_gettext == IntPtr.Zero)
            id_gettext = JNIEnv.GetMethodID(class_ref, "getText", "()Ljava/lang/String;");
        IntPtr lref = JNIEnv.CallObjectMethod(Handle, id_gettext);
        return GetObject<Java.Lang.String>(lref, JniHandleOwnership.TransferLocalRef)?.ToString();
    }

    protected override void Dispose(bool disposing)
    {
        if (class_ref != IntPtr.Zero)
            JNIEnv.DeleteGlobalRef(class_ref);
        class_ref = IntPtr.Zero;

        base.Dispose(disposing);
    }
}
```

Występuje dość bit przejściem tutaj, firma Microsoft:

- Dodaje klasę z sufiksem `Invoker` tego podklasy `AbstractClass`
- Dodaje `class_ref` do przechowywania JNI odwołanie do klasy Java podklasy tej klasy Nasze C#
- Dodaje `id_gettext` do przechowywania odwołanie JNI języka Java `getText` — metoda
- Uwzględnione `(IntPtr, JniHandleOwnership)` — Konstruktor
- Zaimplementowane `ThresholdType` i `ThresholdClass` jako wymagania dotyczące platformy Xamarin.Android w celu uzyskania informacji o `Invoker`
- `GetText` potrzebne do wyszukiwania Java `getText` metody z odpowiednim podpisem JNI i nadaj mu
- `Dispose` po prostu są potrzebne, aby wyczyścić odwołanie do `class_ref`

Po dodaniu tej klasy i generowania nowego AAR, przekazuje testu naszych jednostkowego. Jak widać tego wzorca dla wywołania zwrotne nie jest *idealne*, ale doable.

Aby uzyskać szczegółowe informacje na współdziałanie języka Java, zobacz niesamowite [dokumentacji platformy Xamarin.Android](~/android/platform/java-integration/working-with-jni.md) na ten temat.

## <a name="interfaces"></a>Interfejsy

Interfejsy są znacznie takie same, jak klas abstrakcyjnych, z wyjątkiem szczegółów: Xamarin.Android nie generuje Java dla nich. Jest to spowodowane przed osadzanie .NET, nie ma wiele scenariuszy, w którym Java czy implementuje interfejs języka C#.

Załóżmy, że mamy następujący interfejs C#:

```csharp
[Register("mono.embeddinator.android.IJavaCallback")]
public interface IJavaCallback : IJavaObject
{
    [Export("send")]
    void Send(string text);
}
```

`IJavaObject` sygnały do osadzania .NET jest to interfejs platformy Xamarin.Android, że w przeciwnym razie to jest dokładnie taka sama jak `abstract` klasy.

Ponieważ Xamarin.Android aktualnie nie wygeneruje kod języka Java dla tego interfejsu, należy dodać następujące Java do projektu C#:

```java
package mono.embeddinator.android;

public interface IJavaCallback {
    void send(String text);
}
```

Możesz umieścić plik w dowolnym miejscu, ale upewnij się, że jego Akcja kompilacji ustawioną `AndroidJavaSource`. Spowoduje to sygnał osadzanie .NET, aby skopiować go do katalogu właściwego uzyskać skompilowany w pliku AAR.

Następnie `Invoker` jest dość sam implementacji:

```csharp
class IJavaCallbackInvoker : Java.Lang.Object, IJavaCallback
{
    IntPtr class_ref, id_send;

    public IJavaCallbackInvoker(IntPtr handle, JniHandleOwnership transfer) : base(handle, transfer)
    {
        IntPtr lref = JNIEnv.GetObjectClass(Handle);
        class_ref = JNIEnv.NewGlobalRef(lref);
        JNIEnv.DeleteLocalRef(lref);
    }

    protected override Type ThresholdType
    {
        get { return typeof(IJavaCallbackInvoker); }
    }

    protected override IntPtr ThresholdClass
    {
        get { return class_ref; }
    }

    public void Send(string text)
    {
        if (id_send == IntPtr.Zero)
            id_send = JNIEnv.GetMethodID(class_ref, "send", "(Ljava/lang/String;)V");
        JNIEnv.CallVoidMethod(Handle, id_send, new JValue(new Java.Lang.String(text)));
    }

    protected override void Dispose(bool disposing)
    {
        if (class_ref != IntPtr.Zero)
            JNIEnv.DeleteGlobalRef(class_ref);
        class_ref = IntPtr.Zero;

        base.Dispose(disposing);
    }
}
```

Po wygenerowaniu pliku AAR, w programie Android Studio, firma Microsoft może zapisać następujące jednostki przekazywanie testu:

```java
class ConcreteCallback implements IJavaCallback {
    public String text;
    @Override
    public void send(String text) {
        this.text = text;
    }
}

@Test
public void interfaceCallback() {
    ConcreteCallback callback = new ConcreteCallback();
    JavaCallbacks.interfaceCallback(callback, "Java");
    assertEquals("Java", callback.text);
}
```

## <a name="virtual-methods"></a>Metody wirtualne

Zastępowanie `virtual` w języku Java jest to możliwe, ale nie maksymalnego komfortu obsługi.

Załóżmy, że masz następujące klasy C#:

```csharp
[Register("mono.embeddinator.android.VirtualClass")]
public class VirtualClass : Java.Lang.Object
{
    public VirtualClass() { }

    public VirtualClass(IntPtr handle, JniHandleOwnership transfer) : base(handle, transfer) { }

    [Export("getText")]
    public virtual string GetText() { return "C#"; }
}
```

Po wykonaniu `abstract` klasy powyższego przykładu działałoby z wyjątkiem szczegółów: _wyszukiwania nie będzie Xamarin.Android `Invoker`_ .

Aby rozwiązać ten problem, zmodyfikuj klasy C#, która ma być `abstract`:

```csharp
public abstract class VirtualClass : Java.Lang.Object
```

Nie jest to idealne rozwiązanie, ale pobiera pracy tego scenariusza. Przejmą Xamarin.Android `VirtualClassInvoker` i Java można użyć `@Override` w metodzie.

## <a name="callbacks-in-the-future"></a>Wywołania zwrotne w przyszłości

Istnieje kilka rzeczy, firma Microsoft może zwiększyć te scenariusze:

1. `throws Throwable` w języku C# konstruktorów ustala się na tym [PR](https://github.com/xamarin/java.interop/pull/170).
1. Należy generator języka Java w Xamarin.Android obsługuje interfejsy.
    - Spowoduje to usunięcie potrzebę Dodawanie plik źródłowy Java z akcją kompilacji `AndroidJavaSource`.
1. Wprowadź sposób dla platformy Xamarin.Android załadować `Invoker` dla wirtualnych klas.
    - Spowoduje to usunięcie potrzebę Oznacz klasę w naszym `virtual` przykład `abstract`.
1. Generowanie `Invoker` klas .NET osadzania automatycznie
    - Ma to być skomplikowane, ale doable. Xamarin.Android jest sposób podobny do tego dla projektów powiązanie Java.

Wiele zadań do wykonania w tym miejscu, ale możliwe są następujące ulepszenia osadzanie .NET.

## <a name="further-reading"></a>Dalsze informacje

* [Wprowadzenie w systemie Android](~/tools/dotnet-embedding/get-started/java/android.md)
* [Wstępne badanie systemu Android](~/tools/dotnet-embedding/android/index.md)
* [Ograniczenia osadzania .NET](~/tools/dotnet-embedding/limitations.md)
* [Współtworzenie projekt open source](https://github.com/mono/Embeddinator-4000/blob/master/docs/Contributing.md)
* [Kody błędów wraz z opisami](~/tools/dotnet-embedding/errors.md)
