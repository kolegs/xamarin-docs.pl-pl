---
title: Zasady projektowania interfejsu API platformy Xamarin.Android
ms.prod: xamarin
ms.assetid: 3E52D815-D95D-5510-0D8F-77DAC7E62EDE
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 611046954e8ef359476d2bd12a69f04041d869f1
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/03/2018
---
# <a name="xamarinandroid-api-design-principles"></a>Zasady projektowania interfejsu API platformy Xamarin.Android


## <a name="overview"></a>Omówienie

Oprócz podstawowych podstawowej biblioteki klas, które są częścią Mono Xamarin.Android jest dostarczany z powiązań dla różnych Android interfejsów API umożliwiają deweloperom tworzenie natywnych aplikacji systemu Android z Mono.

Fundament platformy Xamarin.Android jest aparat międzyoperacyjnego tego world mostków C# innym osobom Java i dostarcza deweloperom dostępu do interfejsów API języka Java z języka C# lub innych języków .NET.


## <a name="design-principles"></a>Zasady projektowania

Oto nasze zasady projektowania dla powiązania platformy Xamarin.Android

-  Odpowiadają [zaleceń dotyczących projektowania programu .NET Framework](http://msdn.microsoft.com/en-us/library/ms229042.aspx).

-  Umożliwiają deweloperom podklasą klasy Java.

-  Podklasy powinny współpracować z C# konstrukcje.

-  Pochodzi z istniejącej klasy.

-  Wywołanie konstruktora podstawowego łańcucha.

-  Zastępowanie metody ma się odbywać z C# przez zastąpienie systemu.

-  Umożliwiają łatwe typowych zadań Java i twardych zadania Java.

-  Udostępnianie właściwości JavaBean jako właściwości języka C#.

-  Ujawniać silnie typizowane interfejsu API:

    - Zwiększyć bezpieczeństwo typów.

    - Minimalizowanie błędy podczas wykonywania.

    - Pobierz IDE intellisense w zwracanych typów.

    - Umożliwia dokumentacji podręcznego IDE.

-  Zachęca eksploracji w IDE interfejsów API:

    - Korzystanie z alternatyw Framework narażenia zminimalizować biblioteki klas Java.

    - Udostępnianie C# delegatów (wyrażeń lambda, metody anonimowe i System.Delegate) zamiast pojedynczej metody interfejsów podczas odpowiednie i stosowane.

    - Mechanizm do wywołania dowolnego bibliotek języka Java ( [Android.Runtime.JNIEnv](https://developer.xamarin.com/api/type/Android.Runtime.JNIEnv/)).


## <a name="assemblies"></a>Zestawy

Xamarin.Android obejmuje kilka zestawów, które stanowią *profilu MonoMobile*. [Zestawy](~/cross-platform/internals/available-assemblies.md) strona zawiera więcej informacji.

Powiązania z platformy systemu Android są zawarte w `Mono.Android.dll` zestawu. Ten zestaw zawiera całą powiązanie odbierającą interfejsów API systemu Android i komunikacji z maszyny Wirtualnej w czasie wykonywania dla systemu Android.


## <a name="binding-design"></a>Powiązania projektu


### <a name="collections"></a>Kolekcje

Interfejsy API systemu Android wykorzystywać kolekcje java.util często w celu zapewnienia list, zestawów i map. Uwidaczniamy tych elementów przy użyciu [system.Collections.Generic —](https://developer.xamarin.com/api/namespace/System.Collections.Generic/) interfejsy w naszym powiązania. Podstawowe mapowania są:

-   [java.util.Set<E> ](http://developer.android.com/reference/java/util/Set.html) mapowany na typ systemu [ICollection<T>](http://msdn.microsoft.com/en-us/library/92t2ye13.aspx), klasa pomocy [Android.Runtime.JavaSet<T>](https://developer.xamarin.com/api/type/Android.Runtime.JavaSet%601/).

-   [java.util.List<E> ](http://developer.android.com/reference/java/util/List.html) mapowany na typ systemu [IList<T>](http://msdn.microsoft.com/en-us/library/5y536ey6.aspx), klasa pomocy [Android.Runtime.JavaList<T>](https://developer.xamarin.com/api/type/Android.Runtime.JavaList%601/).

-   [java.util.Map < K, V >](http://developer.android.com/reference/java/util/Map.html) mapowany na typ systemu [IDictionary < TKey, TValue >](http://msdn.microsoft.com/en-us/library/s4ys34ea.aspx), klasa pomocy [Android.Runtime.JavaDictionary < K, V >](https://developer.xamarin.com/api/type/Android.Runtime.JavaDictionary%602/).

-   [java.util.Collection<E> ](http://developer.android.com/reference/java/util/Collection.html) mapowany na typ systemu [ICollection<T>](http://msdn.microsoft.com/en-us/library/92t2ye13.aspx), klasa pomocy [Android.Runtime.JavaCollection<T>](https://developer.xamarin.com/api/type/Android.Runtime.JavaCollection%601/).

Firma Microsoft umieściła klasy pomocy ułatwiające szybciej copyless przekazywanie tych typów. Jeśli to możliwe, zaleca się korzystanie z tych podane kolekcje zamiast wdrożenia struktura dostępna, takich jak [ `List<T>` ](https://developer.xamarin.com/api/type/System.Collections.Generic.List%601/) lub [ `Dictionary<TKey, TValue>` ](https://developer.xamarin.com/api/type/System.Collections.Generic.Dictionary%602/). [Android.Runtime](https://developer.xamarin.com/api/namespace/Android.Runtime/) implementacje wewnętrznie korzystać z natywnych kolekcji Java i w związku z tym nie wymagają kopiowania do i z kolekcji native podczas przekazywania do elementu członkowskiego interfejsu API systemu Android.

Można przekazać do metody Android akceptowanie ten interfejs implementacji interfejsu, np. Przekaż `List<int>` do [ArrayAdapter&lt;int&gt;(kontekstu, int, IList&lt;int&gt;)](https://developer.xamarin.com/api/constructor/Android.Widget.ArrayAdapter%3CT%3E.ArrayAdapter%3CT%3E/p/Android.Content.Context/System.Int32/System.Collections.Generic.IList%7BT%7D/)konstruktora. *Jednak*, dla wszystkich implementacji *z wyjątkiem* implementacji Android.Runtime obejmuje to *kopiowanie* listy z Mono maszyny Wirtualnej w czasie wykonywania dla systemu Android maszyny Wirtualnej. Jeśli lista jest nowsze zmienione w systemie Android runtime (np. przez wywołanie [ArrayAdapter&lt;T&gt;. Add(T)](https://developer.xamarin.com/api/member/Android.Widget.ArrayAdapter%3CT%3E.Add/p/T/) metodę), te zmiany *nie* być widoczne w kodzie zarządzanym. Jeśli `JavaList<int>` zostały użyte, zmiany te będą widoczne.

Rephrased, kolekcje implementacje interfejsu *nie* wymienionych w powyższych **Klasa pomocy**es tylko kierować [In]:

```csharp
// This fails:
var badSource  = new List<int> { 1, 2, 3 };
var badAdapter = new ArrayAdapter<int>(context, textViewResourceId, badSource);
badAdapter.Add (4);
if (badSource.Count != 4) // true
    throw new InvalidOperationException ("this is thrown");

// this works:
var goodSource  = new JavaList<int> { 1, 2, 3 };
var goodAdapter = new ArrayAdapter<int> (context, textViewResourceId, goodSource);
goodAdapter.Add (4);
if (goodSource.Count != 4) // false
    throw new InvalidOperationException ("should not be reached.");
```


### <a name="properties"></a>Właściwości

Metody języka Java przekształceniem właściwości, gdy jest to konieczne:

-  Pary metod Java `T getFoo()` i `void setFoo(T)` przekształceniem `Foo` właściwości. Przykład: [Activity.Intent](https://developer.xamarin.com/api/property/Android.App.Activity.Intent/).

-  Metoda Java `getFoo()` jest przekształcana na Foo właściwości tylko do odczytu. Przykład: [Context.PackageName](https://developer.xamarin.com/api/property/Android.Content.Context.PackageName/).

-  Właściwości tylko do zestawu nie zostały wygenerowane.

-  Właściwości są *nie* generowane, jeśli typ właściwości może być tablicą.



### <a name="events-and-listeners"></a>Zdarzenia i odbiorników

Interfejsy API systemu Android są oparte na języku Java i jej składniki zgodne ze wzorcem Java w celu podłączenia odbiorników zdarzeń. Ten wzorzec zwykle jest trudne, ponieważ wymaga ona użytkownikowi na tworzenie anonimowych klasy ani deklarować metod aby je zastąpić, na przykład jest to, jak elementy będą wykonywane w systemie Android z językiem Java:

```csharp
final android.widget.Button button = new android.widget.Button(context);

button.setText(this.count + " clicks!");
button.setOnClickListener (new View.OnClickListener() {
    public void onClick (View v) {
        button.setText(++this.count + " clicks!");
    }
});
```

Będzie równoważne kodu w języku C# za pomocą zdarzeń:

```csharp
var button = new Android.Widget.Button (context) {
    Text = string.Format ("{0} clicks!", this.count),
};
button.Click += (sender, e) => {
    button.Text = string.Format ("{0} clicks!", ++this.count);
};
```

Należy zauważyć, że oba powyżej mechanizmów dostępne z platformy Xamarin.Android. Można zaimplementować interfejsu odbiornika i dołączenie go z View.SetOnClickListener lub możesz dołączyć utworzone za pomocą dowolnego zwykle wzorcami C# w zdarzeniu kliknij delegata.

Jeśli odbiornik metody wywołania zwrotnego ma typ zwracany typ void, utworzymy elementów interfejsu API na podstawie [EventHandler&lt;TEventArgs&gt; ](https://developer.xamarin.com/api/type/System.EventHandler%601/) delegowanie. Firma Microsoft generują zdarzenie, tak jak w przykładzie powyżej dla tych typów odbiornika. Jednak jeśli wywołanie zwrotne odbiornika zwraca inny niż void i nie- **logiczna** wartości, zdarzeń i EventHandlers nie są używane. Zamiast tego możemy Generowanie określonego delegata dla podpisu wywołania zwrotnego i dodać właściwości zamiast zdarzenia. Dzieje się tak postępowania w przypadku delegata wywołania kolejności i zwracać obsługi. Takie podejście odzwierciedla co odbywa się przy użyciu interfejsu API platformy Xamarin.iOS.

Właściwości lub zdarzenia C# tylko automatycznie są generowane, jeśli metoda Android rejestracja zdarzeń:

1. Ma `set` prefiksu, np. [ *ustawić*OnClickListener](https://developer.xamarin.com/api/member/Android.Views.View.SetOnClickListener/).

1. Ma `void` typ zwracany.

1. Akceptuje tylko jeden parametr, typ parametru jest interfejsem interfejs ma tylko jedną metodę i nazwę interfejsu kończy się `Listener` , np. [View.OnClick *odbiornika*](https://developer.xamarin.com/api/type/Android.Views.View+IOnClickListener/).


Ponadto, jeśli odbiornik interfejsu metody ma typ zwracany **logiczna** zamiast **void**, następnie wygenerowanego *EventArgs* podklasy będzie zawierać *Handled* właściwości. Wartość *Handled* właściwość jest używana jako wartość zwracana *odbiornika* — metoda która domyślnie `true`.

Na przykład Android [View.setOnKeyListener()](https://developer.xamarin.com/api/member/Android.Views.View.SetOnKeyListener/p/Android.Views.View+IOnKeyListener/) metoda przyjmuje [View.OnKeyListener](https://developer.xamarin.com/api/type/Android.Views.View+IOnKeyListener) interfejsu i [View.OnKeyListener.onKey (widoku, int, KeyEvent)](https://developer.xamarin.com/api/member/Android.Views.View+IOnKeyListener.OnKey/p/Android.Views.View/Android.Views.Keycode/Android.Views.KeyEvent/) Metoda ma typ zwracany boolean. Generuje Xamarin.Android odpowiadającego [View.KeyPress](https://developer.xamarin.com/api/event/Android.Views.View.KeyPress/) zdarzenie, które jest [EventHandler&lt;View.KeyEventArgs&gt;](https://developer.xamarin.com/api/type/Android.Views.View+KeyEventArgs/).
*KeyEventArgs* klasy z kolei ma [View.KeyEventArgs.Handled](https://developer.xamarin.com/api/property/Android.Views.View+KeyEventArgs.Handled/) właściwość, która jest używana jako wartość zwracana *View.OnKeyListener.onKey()* metody.

Firma Microsoft planuje dodanie przeciążenia dla innych metod i konstruktorami do udostępnienia połączenia na podstawie obiektu delegowanego. Obiekty nasłuchujące z wielu wywołań zwrotnych wymagają niektóre dodatkowe kontroli w celu ustalenia, czy wykonania poszczególnych wywołania zwrotne jest uzasadnione, dlatego firma Microsoft są one konwertowanie zgodnie z ich rozpoznaniem. Nie ma żadnych odpowiednie zdarzenie, odbiorników musi być używany w języku C#, ale Przełącz które uważasz, że można ustawić właściwość użycia delegata do wymagające uwagi. Niektóre konwersje interfejsy sufiksu "Odbiornika" również ma zostać wykonane, gdy był wyraźny wniosek byłaby korzystna alternatywna delegata.

Wszystkie interfejsy odbiorników zaimplementować [ `Android.Runtime.IJavaObject` ](https://developer.xamarin.com/api/type/Android.Runtime.IJavaObject/) interfejsu z powodu szczegóły implementacji powiązania, więc klasy odbiornika musi zawierać implementację tego interfejsu. Można to zrobić przez zaimplementowanie interfejsu odbiornika w podklasą [Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/) lub inne obiektu języka Java, na przykład działanie systemu Android.


### <a name="runnables"></a>Runnables

Wykorzystuje Java [java.lang.Runnable](https://developer.xamarin.com/api/type/Java.Lang.Runnable/) interfejsu mechanizm delegowania. [Java.lang.Thread](https://developer.xamarin.com/api/type/Java.Lang.Thread/) klasy jest zauważalne konsumenta tego interfejsu. Android wykorzystywania interfejsu w interfejsie API, a także.
[Activity.runOnUiThread()](https://developer.xamarin.com/api/member/Android.App.Activity.RunOnUiThread/p/Java.Lang.IRunnable/) i [View.post()](https://developer.xamarin.com/api/member/Android.Views.View.Post/p/Java.Lang.IRunnable) są zauważalne przykłady.

`Runnable` Interfejs zawiera jedną metodę void [run()](https://developer.xamarin.com/api/member/Java.Lang.Runnable.Run%28%29/). Go w związku z tym pozwala na powiązanie w języku C# jako [elementu System.Action](http://msdn.microsoft.com/en-us/library/system.action.aspx) delegowanie. Firma Microsoft umieściła przeciążenia powiązanie, które akceptują `Action` parametru dla wszystkich członków interfejsu API, które zużywają `Runnable` natywnego interfejsu API, np. [Activity.RunOnUiThread()](https://developer.xamarin.com/api/member/Android.App.Activity.RunOnUiThread/(System.Action)) i [View.Post()](https://developer.xamarin.com/api/member/Android.Views.View.Post/(System.Action)).

Firma Microsoft pozostałych [interfejsu IRunnable](https://developer.xamarin.com/api/type/Java.Lang.IRunnable/) przesłonięć w miejscu zamiast zastąpić je, ponieważ kilka typów implementuje interfejsu i dlatego może być przekazywane jako runnables bezpośrednio.


### <a name="inner-classes"></a>Klasy wewnętrzne

Java ma dwa różne typy [zagnieżdżonych klas](http://download.oracle.com/javase/tutorial/java/javaOO/nested.html): statyczne zagnieżdżone klasy i niestatyczna klasy.

Java statycznej klasy zagnieżdżone są takie same jak typy zagnieżdżone C#.

Niestatyczne zagnieżdżonych klas, nazywany również *wewnętrzny klasy*, różnią się. One zawierać niejawne odwołanie do wystąpienia ich typu otaczającego i nie może zawierać elementy członkowskie static (między innymi różnice poza zakres tego — omówienie).

Jeśli chodzi o powiązania i używanie języka C#, statycznej klasy zagnieżdżone są traktowane jako normalne zagnieżdżone typy. Klasy wewnętrzne, w tym samym czasie mają dwa istotne różnice:

1. Niejawne odwołanie do typu zawierającego musi być jawnie dostarczona jako parametru konstruktora.

1. Podczas dziedziczenia z klasy wewnętrznej klasy wewnętrznej *musi* być zagnieżdżone w obrębie typu który dziedziczy z typu zawierającego wewnętrzny klasy podstawowej i typu pochodnego podać konstruktora taki sam typ jak C# z typem.


Rozważmy na przykład [Android.Service.Wallpaper.WallpaperService.Engine](https://developer.xamarin.com/api/type/Android.Service.Wallpaper.WallpaperService+Engine/) klasy wewnętrznej. Ponieważ jest to wewnętrzny klasa [Konstruktor WallpaperService.Engine()](https://developer.xamarin.com/api/constructor/Android.Service.Wallpaper.WallpaperService+Engine.Engine/p/Android.Service.Wallpaper.WallpaperService/) przyjmuje odwołanie do [WallpaperService](https://developer.xamarin.com/api/type/Android.Service.Wallpaper.WallpaperService/) wystąpienia (porównania i kontrastu języka Java [WallpaperService.Engine ( Konstruktor)](https://developer.xamarin.com/api/type/Android.Service.Wallpaper.WallpaperService+Engine/) który nie przyjmuje żadnych parametrów).

Przykład pochodnym klasy wewnętrznej jest CubeWallpaper.CubeEngine:

```csharp
class CubeWallpaper : WallpaperService {
        public override WallpaperService.Engine OnCreateEngine ()
        {
                return new CubeEngine (this);
        }

        class CubeEngine : WallpaperService.Engine {
                public CubeEngine (CubeWallpaper s)
                        : base (s)
                {
                }
        }
}
```

Uwaga jak `CubeWallpaper.CubeEngine` jest zagnieżdżony w `CubeWallpaper`, `CubeWallpaper` dziedziczy z klasy zawierające `WallpaperService.Engine`, i `CubeWallpaper.CubeEngine` ma konstruktora, który przyjmuje typ deklarujący-- `CubeWallpaper` w takim przypadku — wszystkie jako wymienione powyżej.


### <a name="interfaces"></a>Interfejsy

Interfejsy Java może zawierać trzy zestawy elementów członkowskich, z których dwa spowodować problemy w języku C#:

1. Metody

1. Types

1. Pola


Interfejsy Java są tłumaczone na dwa typy:

1. Interfejs (opcjonalnie) zawierającego deklaracje metody. Ten interfejs ma taką samą nazwę jak interfejsu Java *z wyjątkiem* ma również " *I* " prefiks.

1. (Opcjonalnie) klasy statycznej zawierający wszystkie pola zadeklarowany w interfejsie Java.

Zagnieżdżone typy są "przeniesiony" jako elementy równorzędne otaczającego interfejsu zamiast zagnieżdżone typy z otaczającym nazwy interfejsu jako prefiksu.

Rozważmy na przykład [android.os.Parcelable](https://developer.xamarin.com/api/type/Android.OS.Parcelable/) interfejsu.
*Parcelable* interfejs zawiera metody, zagnieżdżone typy i stałe. *Parcelable* metod interfejsu są umieszczane w [Android.OS.IParcelable](https://developer.xamarin.com/api/type/Android.OS.IParcelable/) interfejsu.
*Parcelable* interfejsu stałe są umieszczane w [Android.OS.ParcelableConsts](https://developer.xamarin.com/api/type/Android.OS.ParcelableConsts/) typu. Zagnieżdżone [android.os.Parcelable.ClassLoaderCreator <t> </t> ](http://developer.android.com/reference/android/os/Parcelable.ClassLoaderCreator.html) i [android.os.Parcelable.Creator <t> </t> ](http://developer.android.com/reference/android/os/Parcelable.Creator.html) typy nie są obecnie powiązany ze względu na ograniczenia w technicznej ogólne; Jeśli są obsługiwane, będą one występować jako *Android.OS.IParcelableClassLoaderCreator* i *Android.OS.IParcelableCreator* interfejsów. Na przykład zagnieżdżonych [android.os.IBinder.DeathRecpient](http://developer.android.com/reference/android/os/IBinder.DeathRecipient.html) interfejs jest powiązany jako [Android.OS.IBinderDeathRecipient](https://developer.xamarin.com/api/type/Android.OS.IBinderDeathRecipient/) interfejsu.


> [!NOTE]
> Począwszy od platformy Xamarin.Android 1.9 są stałe interfejsu Java <em>zduplikowane</em> w celu uproszczenia eksportowanie Java kodu. Pomaga to zapewnić przenoszenia kodu języka Java, która zależy od [android dostawcy](http://developer.android.com/reference/android/provider/package-summary.html) interfejsu stałe.

Oprócz folderów dla powyższych typów istnieją cztery dalszych zmian:

1. Typ o nazwie identycznej z nazwą interfejsu Java jest generowany zawiera stałe.

1. Typy zawierające stałe interfejsu również zawierać wszystkich stałych, które pochodzą od zaimplementowanych interfejsów Java.

1. Wszystkich klas implementujących interfejs Java, zawierające stałe uzyskać nowy zagnieżdżony typ InterfaceConsts, który zawiera stałe z wszystkie zaimplementowane interfejsy.

1. *Stałych* typu jest już nieaktualny.


Aby uzyskać *android.os.Parcelable* interfejs, oznacza to, że teraz będzie [ *Android.OS.Parcelable* ](https://developer.xamarin.com/api/type/Android.OS.Parcelable/) typ zawiera stałe. Na przykład [Parcelable.CONTENTS_FILE_DESCRIPTOR](http://developer.android.com/reference/android/os/Parcelable.html#CONTENTS_FILE_DESCRIPTOR) stała zostanie powiązany jako [ *Parcelable.ContentsFileDescriptor* ](https://developer.xamarin.com/api/field/Android.OS.Parcelable.ContentsFileDescriptor/) stałej, a nie jako  *ParcelableConsts.ContentsFileDescriptor* stałej.

Dla interfejsów zawierających stałe implementujące inne interfejsy zawierający jeszcze więcej stałe generowany jest teraz Unii stałymi. Na przykład [android.provider.MediaStore.Video.VideoColumns](http://developer.android.com/reference/android/provider/MediaStore.Video.VideoColumns.html) implementuje interfejs [android.provider.MediaStore.MediaColumns](https://developer.xamarin.com/api/type/Android.Provider.MediaStore+MediaColumns/) interfejsu. Jednak przed 1.9 [Android.Provider.MediaStore.Video.VideoColumnsConsts](https://developer.xamarin.com/api/type/Android.Provider.MediaStore+Video+VideoColumnsConsts/) typu nie ma możliwości uzyskania dostępu do stałe zadeklarowana w [Android.Provider.MediaStore.MediaColumnsConsts](https://developer.xamarin.com/api/type/Android.Provider.MediaStore+MediaColumnsConsts/).
W wyniku wyrażenia języka Java *MediaStore.Video.VideoColumns.TITLE* musi być powiązana z wyrażeniem C# *MediaStore.Video.MediaColumnsConsts.Title* czyli wykrycie bez odczytu partie dokumentację programu Java. W 1.9, będzie równoważne wyrażenie C# [ *MediaStore.Video.VideoColumns.Title*](https://developer.xamarin.com/api/field/Android.Provider.MediaStore+Video+VideoColumns.Title/).

Ponadto należy wziąć pod uwagę [android.os.Bundle](https://developer.xamarin.com/api/type/Android.OS.Bundle/) typu, który implementuje Java *Parcelable* interfejsu. Ponieważ implementuje interfejs, wszystkich stałych, w tym interfejsie są dostępne "" typ pakietu, np. *Bundle.CONTENTS_FILE_DESCRIPTOR* jest całkowicie prawidłowe wyrażenie języka Java.
Wcześniej, do portu tego wyrażenia na język C# należy przyjrzeć się wszystkich interfejsów, które są wykonywane z jakiego typu *CONTENTS_FILE_DESCRIPTOR* pochodzi z. Począwszy od platformy Xamarin.Android 1.9 klasy implementowanie interfejsów Java, które zawierają stałe będą miały zagnieżdżoną *InterfaceConsts* typu, który będzie zawierać wszystkich stałych dziedziczony interfejs. Umożliwi to tłumaczenia *Bundle.CONTENTS_FILE_DESCRIPTOR* do [ *Bundle.InterfaceConsts.ContentsFileDescriptor*](https://developer.xamarin.com/api/field/Android.OS.Bundle+InterfaceConsts.ContentsFileDescriptor/).

Na koniec typów z *stałych* sufiks, takich jak *Android.OS.ParcelableConsts* są teraz przestarzałe, inne niż nowo wprowadzonych InterfaceConsts zagnieżdżone typy. Zostaną one usunięte 3.0 platformy Xamarin.Android.


## <a name="resources"></a>Zasoby

Obrazy, opisy układu, binarne obiekty BLOB i słowników ciąg może być uwzględniony w aplikacji jako [pliki zasobów](http://developer.android.com/guide/topics/resources/providing-resources.html).
Różne interfejsów API systemu Android mają na celu [działać na identyfikatorów zasobów](http://developer.android.com/guide/topics/resources/accessing-resources.html) zamiast zajmowanie się obrazy, ciągi lub binarne obiektów blob bezpośrednio.

Na przykład przykładowej aplikacji systemu Android zawierającą układ interfejsu użytkownika ( `main.axml`), parametry tabeli przystosowywanie do warunków międzynarodowych ( `strings.xml`) i niektóre ikony ( `drawable-*/icon.png`) zachowa zasobów w katalogu "Zasoby" aplikacji:

    Resources/
        drawable-hdpi/
            icon.png

        drawable-ldpi/
            icon.png

        drawable-mdpi/
            icon.png

        layout/
            main.axml

        values/
            strings.xml

Macierzystych interfejsów API systemu Android nie działają bezpośrednio z nazwami plików, ale zamiast tego działają w identyfikatory zasobów. Podczas kompilowania aplikacji systemu Android, która używa zasobów systemu kompilacji zostanie pakiet zasobów w celu dystrybucji i Generowanie klasy o nazwie `Resource` zawierający tokeny dla każdej z nich uwzględnione zasoby. Na przykład w układzie zasoby powyżej jest to, co może narazić klasy R:

```csharp
public class Resource {
    public class Drawable {
        public const int icon = 0x123;
    }

    public class Layout {
        public const int main = 0x456;
    }

    public class String {
        public const int first_string = 0xabc;
        public const int second_string = 0xbcd;
    }
}
```

Następnie użyje `Resource.Drawable.icon` do odwołania `drawable/icon.png` pliku, lub `Resource.Layout.main` do odwołania `layout/main.xml` pliku, lub `Resource.String.first_string` do odwołania pierwszy ciąg w pliku słownika `values/strings.xml`.


## <a name="constants-and-enumerations"></a>Stałe i wyliczenia

Macierzystych interfejsów API systemu Android ma wiele metod, które przejąć lub zwraca int muszą być zamapowane do pola stałej, aby ustalić, co oznacza int. Aby korzystać z tych metod, użytkownik będzie musiał się z dokumentacją, aby zobaczyć, które stałe są odpowiednie wartości, która jest mniejsza niż nadaje się doskonale.

Rozważmy na przykład [Activity.requestWindowFeature (int featureID)](http://developer.android.com/reference/android/app/Activity.html#requestWindowFeature(int)).

W takich przypadkach firma Microsoft wszelkich starania grupują powiązane stałe w wyliczenie .NET i ponownie zamapować metodę, aby wykonać zamiast tego wyliczenia.
W ten sposób możemy oferować wybór IntelliSense potencjalnych wartości.

Powyższy przykład staje się: [Activity.RequestWindowFeature (WindowFeatures featureId)](https://developer.xamarin.com/api/member/Android.App.Activity.RequestWindowFeature/p/Android.Views.WindowFeatures/)).

Należy pamiętać, że jest to bardzo ręczny proces, aby dowiedzieć się, które stałe należą ze sobą i które interfejsy API korzystać z tych stałych. W interfejsie API, które będą lepiej wyrażona jako wyliczenie pliku usterki na każde użycie stałe.
