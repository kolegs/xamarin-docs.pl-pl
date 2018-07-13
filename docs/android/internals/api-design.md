---
title: Zasady projektowania interfejsu API platformy Xamarin.Android
ms.prod: xamarin
ms.assetid: 3E52D815-D95D-5510-0D8F-77DAC7E62EDE
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 8abb78f335b159223e9394b7845eccbba8d124da
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996350"
---
# <a name="xamarinandroid-api-design-principles"></a>Zasady projektowania interfejsu API platformy Xamarin.Android


## <a name="overview"></a>Omówienie

Oprócz podstawowych podstawowej biblioteki klas, które są częścią platformy Mono platformy Xamarin.Android jest dostarczany z powiązania różne interfejsy API systemu Android umożliwić deweloperom tworzenie natywnych aplikacji dla systemu Android za pomocą platformy Mono.

W samym sercu Xamarin.Android jest aparat międzyoperacyjny dzisiejszym świecie mostków języka C# ze światem Java i zapewnia deweloperom dostęp do interfejsów API języka Java w języku C# lub innymi językami .NET.


## <a name="design-principles"></a>Zasady projektowania

Poniżej przedstawiono niektóre z naszych zasad projektowania dla powiązań platformy Xamarin.Android

-  Odpowiadają [.NET Framework — zalecenia dotyczące projektowania](https://docs.microsoft.com/dotnet/standard/design-guidelines/).

-  Umożliwia deweloperom podklas klas Java.

-  Podklasy powinny współpracować z C# konstrukcje.

-  Pochodzi z istniejącej klasy.

-  Wywołanie konstruktora podstawowego do tworzenia łańcucha.

-  Zastępowanie metody powinna być podejmowana przy użyciu języka C# w zastąpienie systemu.

-  Umożliwiają łatwe typowych zadań języka Java i twardych zadania języka Java.

-  Udostępnianie właściwości JavaBean jako właściwości języka C#.

-  Udostępnić silnie typizowaną interfejsu API:

    - Zwiększ bezpieczeństwo typów.

    - Zminimalizować błędy w czasie wykonywania.

    - Uzyskaj IDE intellisense zwracanych typów.

    - Umożliwia dokumentacji okno podręczne IDE.

-  Zachęć eksploracji w IDE, interfejsów API:

    - Korzystaj z alternatyw Framework narażenia zminimalizować Classlib języka Java.

    - Udostępnianie C# delegatów (wyrażenia lambda, metody anonimowe i System.Delegate) zamiast pojedynczej metody interfejsów, gdy odpowiednie i ma zastosowanie.

    - Mechanizm do wywołania dowolnego biblioteki języka Java ( [Android.Runtime.JNIEnv](https://developer.xamarin.com/api/type/Android.Runtime.JNIEnv/)).


## <a name="assemblies"></a>Zestawy

Platforma Xamarin.Android obejmuje pewną liczbę zestawów, które stanowią *profilu MonoMobile*. [Zestawy](~/cross-platform/internals/available-assemblies.md) strona zawiera więcej informacji.

Powiązania do platformy systemu Android są zawarte w `Mono.Android.dll` zestawu. Ten zestaw zawiera całą powiązania dla konsumencki interfejsy API systemu Android i komunikacji z maszyn wirtualnych w środowisku uruchomieniowym systemu Android.


## <a name="binding-design"></a>Projekt powiązania


### <a name="collections"></a>Kolekcje

Interfejsy API systemu Android korzystać z kolekcji java.util często zapewnienie listy, zestawy oraz maps. Możemy ujawnić te elementy przy użyciu [System.Collections.Generic](xref:System.Collections.Generic) interfejsy w naszym powiązania. Podstawowe mapowania są:

-   [java.util.Set<E> ](http://developer.android.com/reference/java/util/Set.html) mapuje do typu systemu [ICollection<T>](xref:System.Collections.Generic.ICollection`1), klasa pomocy [Android.Runtime.JavaSet<T>](https://developer.xamarin.com/api/type/Android.Runtime.JavaSet%601/).

-   [java.util.List<E> ](http://developer.android.com/reference/java/util/List.html) mapuje do typu systemu [IList<T>](xref:System.Collections.Generic.IList`1), klasa pomocy [Android.Runtime.JavaList<T>](https://developer.xamarin.com/api/type/Android.Runtime.JavaList%601/).

-   [java.util.Map < K, V >](http://developer.android.com/reference/java/util/Map.html) mapuje do typu systemu [IDictionary < TKey, TValue >](xref:System.Collections.Generic.IDictionary`2), klasa pomocy [Android.Runtime.JavaDictionary < K, V >](https://developer.xamarin.com/api/type/Android.Runtime.JavaDictionary%602/).

-   [java.util.Collection<E> ](http://developer.android.com/reference/java/util/Collection.html) mapuje do typu systemu [ICollection<T>](xref:System.Collections.Generic.ICollection`1), klasa pomocy [Android.Runtime.JavaCollection<T>](https://developer.xamarin.com/api/type/Android.Runtime.JavaCollection%601/).

Udostępniliśmy klasy pomocnika, aby ułatwić szybsze copyless kierowania tych typów. Jeśli to możliwe, zaleca się używanie tych podane kolekcji zamiast wdrożenia struktura dostępna, takich jak [ `List<T>` ](xref:System.Collections.Generic.List`1) lub [ `Dictionary<TKey, TValue>` ](xref:System.Collections.Generic.Dictionary`2). [Android.Runtime](https://developer.xamarin.com/api/namespace/Android.Runtime/) implementacje wewnętrznie wykorzystywać natywne kolekcji języka Java i w związku z tym nie wymagają kopiowania z natywnych kolekcji podczas przekazywania do elementu członkowskiego interfejsu API systemu Android.

Można przekazać implementacji interfejsu z metodą dla systemu Android, przyjmuje ten interfejs, np. przekazywać `List<int>` do [ArrayAdapter&lt;int&gt;(kontekst, int, IList&lt;int&gt;)](https://developer.xamarin.com/api/constructor/Android.Widget.ArrayAdapter%3CT%3E.ArrayAdapter%3CT%3E/p/Android.Content.Context/System.Int32/System.Collections.Generic.IList%7BT%7D/)konstruktora. *Jednak*, dla wszystkich implementacjach *z wyjątkiem* w implementacji Android.Runtime wiąże się to *kopiowanie* listy z maszyny Wirtualnej platformy Mono do maszyny Wirtualnej w środowisku uruchomieniowym systemu Android. Jeśli lista zostanie później zmienione w środowisku uruchomieniowym systemu Android (np. wywołując [ArrayAdapter&lt;T&gt;. Add(T)](https://developer.xamarin.com/api/member/Android.Widget.ArrayAdapter%3CT%3E.Add/p/T/) metoda), te zmiany *nie będzie* być widoczne w kodzie zarządzanym. Jeśli `JavaList<int>` był używany, te zmiany będą widoczne.

Sformułowano, kolekcji interfejs implementacji, które są *nie* wymienione w powyższych **klasy Pomocnika**es tylko kierować [In]:

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

Metody Java są przekształcane na właściwości, gdy jest to konieczne:

-  Pary metod języka Java `T getFoo()` i `void setFoo(T)` są przekształcane w `Foo` właściwości. Przykład: [Activity.Intent](https://developer.xamarin.com/api/property/Android.App.Activity.Intent/).

-  Metoda Java `getFoo()` jest przekształcana na Foo właściwość tylko do odczytu. Przykład: [Context.PackageName](https://developer.xamarin.com/api/property/Android.Content.Context.PackageName/).

-  Właściwości tylko do zestawu nie są generowane.

-  Właściwości są *nie* generowane, jeśli typ właściwości może być tablicą.



### <a name="events-and-listeners"></a>Zdarzenia i odbiorników

Interfejsy API systemu Android są zbudowane na podstawie języka Java i jego składniki zgodne ze wzorcem Java w celu podłączenia detektorów zdarzeń. Zwykle jest trudne, ponieważ wymaga ona użytkownikowi na tworzenie klasa anonimowa i metody, aby zastąpić ten wzorzec, na przykład, jest to, jak elementy będą wykonywane w systemie Android przy użyciu języka Java:

```csharp
final android.widget.Button button = new android.widget.Button(context);

button.setText(this.count + " clicks!");
button.setOnClickListener (new View.OnClickListener() {
    public void onClick (View v) {
        button.setText(++this.count + " clicks!");
    }
});
```

Równoważny kod w języku C# przy użyciu zdarzeń będzie:

```csharp
var button = new Android.Widget.Button (context) {
    Text = string.Format ("{0} clicks!", this.count),
};
button.Click += (sender, e) => {
    button.Text = string.Format ("{0} clicks!", ++this.count);
};
```

Należy zauważyć, że oba powyższe mechanizmów dostępne z rozszerzeniem Xamarin.Android. Można zaimplementować interfejsu odbiornik i dołączyć go za pomocą View.SetOnClickListener lub możesz dołączyć delegata utworzone za pomocą dowolnego zwykle paradygmatów C# zdarzenie kliknij.

Gdy metoda wywołania zwrotnego odbiornik ma zwracany typ void, możemy utworzyć elementy interfejsu API na podstawie [EventHandler&lt;TEventArgs&gt; ](xref:System.EventHandler`1) delegować. Firma Microsoft generują zdarzenie, podobnie jak w przykładzie powyżej dla tych typów odbiornika. Jednak jeśli odbiornik wywołania zwrotnego zwraca niż void i innych — **logiczna** wartości, zdarzeń i EventHandlers nie są używane. Zamiast tego będziemy Generowanie określonych delegatów dla sygnatury wywołania zwrotnego i Dodaj właściwości zamiast zdarzenia. Przyczyną jest do przeciwdziałania delegata wywołania zamówienia, a następnie wróć obsługi. To podejście odzwierciedla, co jest przeprowadzane za pomocą interfejsu API Xamarin.iOS.

Zdarzenia języka C# lub właściwości tylko automatycznie są generowane, jeśli metoda rejestracji zdarzeń dla systemu Android:

1. Ma `set` prefiksu, np. [ *ustaw*OnClickListener](https://developer.xamarin.com/api/member/Android.Views.View.SetOnClickListener/).

1. Ma `void` typ zwracany.

1. Akceptuje tylko jeden parametr, typ parametru jest interfejsem interfejs ma tylko jedną metodę, i nazwa interfejsu, który kończy się na `Listener` , np. [View.OnClick *odbiornika*](https://developer.xamarin.com/api/type/Android.Views.View+IOnClickListener/).


Ponadto, jeśli odbiornik interfejsu metoda ma typ zwracany z **logiczna** zamiast **void**, następnie wygenerowanego *EventArgs* podklasy będzie zawierać *Handled* właściwości. Wartość *Handled* właściwość jest używana jako wartość zwracaną dla *odbiornika* metody, a wartość domyślna to `true`.

Na przykład systemu Android [View.setOnKeyListener()](https://developer.xamarin.com/api/member/Android.Views.View.SetOnKeyListener/p/Android.Views.View+IOnKeyListener/) metoda przyjmuje [View.OnKeyListener](https://developer.xamarin.com/api/type/Android.Views.View+IOnKeyListener) interfejsu, a [View.OnKeyListener.onKey (widok, int, KeyEvent)](https://developer.xamarin.com/api/member/Android.Views.View+IOnKeyListener.OnKey/p/Android.Views.View/Android.Views.Keycode/Android.Views.KeyEvent/) Metoda ma typ zwracany boolean. Platforma Xamarin.Android generuje odpowiedni [View.KeyPress](https://developer.xamarin.com/api/event/Android.Views.View.KeyPress/) zdarzenie, które jest [EventHandler&lt;View.KeyEventArgs&gt;](https://developer.xamarin.com/api/type/Android.Views.View+KeyEventArgs/).
*KeyEventArgs* klasa z kolei ma [View.KeyEventArgs.Handled](https://developer.xamarin.com/api/property/Android.Views.View+KeyEventArgs.Handled/) właściwość, która jest używana jako wartość zwracaną dla *View.OnKeyListener.onKey()* metody.

Firma Microsoft planuje dodanie przeciążenia dla innych metod i konstruktory do udostępnienia połączeń opartych na delegacie. Ponadto odbiorniki z wielu wywołań zwrotnych wymagają niektóre dodatkowe kontroli w celu ustalenia, czy wykonywania poszczególnych wywołań zwrotnych jest uzasadnione, dzięki czemu możemy są one konwertowanie po ich zidentyfikowaniu. W przypadku nie odpowiednie zdarzenie odbiorników musi być używany w języku C#, ale. Połącz z nich, które Twoim zdaniem, może być użycie delegata do naszej uwagi. Niektóre konwersje interfejsy bez sufiksu "Odbiornik" również ma zostać wykonane, gdy stało się jasne, że ich używającym zamiast delegata.

Wszystkie interfejsy odbiorników implementują [ `Android.Runtime.IJavaObject` ](https://developer.xamarin.com/api/type/Android.Runtime.IJavaObject/) interfejsu, ze względu na szczegóły implementacji powiązań, więc odbiornik klasy należy zaimplementować ten interfejs. Można to zrobić poprzez implementację interfejsu odbiornika na podklasą [Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/) lub inne opakowaną obiektu języka Java, na przykład działanie systemu Android.


### <a name="runnables"></a>Runnables

Dzięki narzędziom języka Java [java.lang.Runnable](https://developer.xamarin.com/api/type/Java.Lang.Runnable/) interfejsu mechanizm delegowania. [Java.lang.Thread](https://developer.xamarin.com/api/type/Java.Lang.Thread/) klasa jest istotne odbiorcę tego interfejsu. Android wykorzystywania interfejsu API, jak również.
[Activity.runOnUiThread()](https://developer.xamarin.com/api/member/Android.App.Activity.RunOnUiThread/p/Java.Lang.IRunnable/) i [View.post()](https://developer.xamarin.com/api/member/Android.Views.View.Post/p/Java.Lang.IRunnable) przedstawiono istotne.

`Runnable` Interfejs zawiera jedną metodę void [run()](https://developer.xamarin.com/api/member/Java.Lang.Runnable.Run%28%29/). Go w związku z tym pozwala na powiązanie w języku C# jako [elementu System.Action](xref:System.Action) delegować. Udostępniliśmy przeciążenia powiązanie, które akceptują `Action` parametr dla wszystkich elementów członkowskich interfejsu API, które zużywają `Runnable` w natywnych interfejsów API, np. [Activity.RunOnUiThread()](https://developer.xamarin.com/api/member/Android.App.Activity.RunOnUiThread/(System.Action)) i [View.Post()](https://developer.xamarin.com/api/member/Android.Views.View.Post/(System.Action)).

Pozostawiliśmy [interfejsu IRunnable](https://developer.xamarin.com/api/type/Java.Lang.IRunnable/) przeciążeń w miejscu zamiast zastępowania ich, ponieważ różne typy implementują interfejs i w związku z tym może być przekazywane jako runnables bezpośrednio.


### <a name="inner-classes"></a>Klasy wewnętrzne

Java ma dwa różne typy [klas zagnieżdżonych](http://download.oracle.com/javase/tutorial/java/javaOO/nested.html): statyczne zagnieżdżonych klas i niestatycznych klas.

Java statyczne klasy zagnieżdżone są takie same jak typy zagnieżdżone C#.

Niestatycznych zagnieżdżonych klas, nazywany również *klasy wewnętrzny*, znacznie się różnią. One zawierają niejawne odwołanie do wystąpienia ich typ otaczający i nie może zawierać elementy statyczne (oraz inne różnice poza zakres tego omówienia).

Jeśli chodzi o powiązania i używania języka C#, statycznej klasy zagnieżdżone są traktowane jako normalnych typów zagnieżdżonych. Wewnętrzny klasy, w tym samym czasie mają dwie istotne różnice:

1. Niejawne odwołanie do typu zawierającego muszą być wyraźnie oznaczony jako parametr konstruktora.

1. Gdy dziedziczy z klasy wewnętrznej klasy wewnętrznej *musi* być zagnieżdżone wewnątrz typu która dziedziczy z typu zawierającego wewnętrzny klasy bazowej, a typ pochodny musi dostarczyć konstruktora tego samego typu jak język C# z typem.


Na przykład, rozważmy [Android.Service.Wallpaper.WallpaperService.Engine](https://developer.xamarin.com/api/type/Android.Service.Wallpaper.WallpaperService+Engine/) klasy wewnętrznej. Ponieważ jest klasą wewnętrzny [Konstruktor WallpaperService.Engine()](https://developer.xamarin.com/api/constructor/Android.Service.Wallpaper.WallpaperService+Engine.Engine/p/Android.Service.Wallpaper.WallpaperService/) przyjmuje odwołanie do [WallpaperService](https://developer.xamarin.com/api/type/Android.Service.Wallpaper.WallpaperService/) wystąpienia (porównaj i zestaw do języka Java [WallpaperService.Engine ( Konstruktor)](https://developer.xamarin.com/api/type/Android.Service.Wallpaper.WallpaperService+Engine/) która nie przyjmuje żadnych parametrów).

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

Uwaga jak `CubeWallpaper.CubeEngine` jest zagnieżdżony w `CubeWallpaper`, `CubeWallpaper` dziedziczy z klasy zawierającej `WallpaperService.Engine`, i `CubeWallpaper.CubeEngine` ma konstruktora, która przyjmuje typ deklarujący — `CubeWallpaper` w tym przypadku — wszystkie jako określonym powyżej.


### <a name="interfaces"></a>Interfejsy

Java interfejsy mogą zawierać trzy rodzaje członków, z których dwa powodować problemy z kodu C#:

1. Metody

1. Types

1. Pola


Java interfejsy są tłumaczone na dwa typy:

1. Interfejs (opcjonalnie) zawierające deklaracje metody. Ten interfejs ma taką samą nazwę jak interfejsu języka Java *z wyjątkiem* ma również " *I* " prefiks.

1. (Opcjonalnie) klasy statycznej, zawierający wszystkie pola są zadeklarowane w obrębie interfejsu języka Java.

Zagnieżdżone typy są "przemieszczane" jako elementy równorzędne otaczającej interfejsu zamiast zagnieżdżone typy nazwą interfejsu otaczającej jako prefiksu.

Na przykład, rozważmy [android.os.Parcelable](https://developer.xamarin.com/api/type/Android.OS.Parcelable/) interfejsu.
*Parcelable* interfejs zawiera metody, typy zagnieżdżone i stałe. *Parcelable* metod interfejsu są umieszczane w [Android.OS.IParcelable](https://developer.xamarin.com/api/type/Android.OS.IParcelable/) interfejsu.
*Parcelable* interfejsu stałe są umieszczane w [Android.OS.ParcelableConsts](https://developer.xamarin.com/api/type/Android.OS.ParcelableConsts/) typu. Zagnieżdżony [android.os.Parcelable.ClassLoaderCreator <t> </t> ](http://developer.android.com/reference/android/os/Parcelable.ClassLoaderCreator.html) i [android.os.Parcelable.Creator <t> </t> ](http://developer.android.com/reference/android/os/Parcelable.Creator.html) typy nie są obecnie powiązana ze względu na ograniczenia w obsłudze ogólne; Jeśli są obsługiwane, może być obecne jako *Android.OS.IParcelableClassLoaderCreator* i *Android.OS.IParcelableCreator* interfejsów. Na przykład zagnieżdżonych [android.os.IBinder.DeathRecpient](http://developer.android.com/reference/android/os/IBinder.DeathRecipient.html) interfejsu jest powiązany jako [Android.OS.IBinderDeathRecipient](https://developer.xamarin.com/api/type/Android.OS.IBinderDeathRecipient/) interfejsu.


> [!NOTE]
> Począwszy od platformy Xamarin.Android 1.9 są stałe interfejsu Java <em>zduplikowane</em> w ramach działań zmierzających do upraszczają przenoszenie kodu języka Java kodu. Pomaga to poprawić przenoszenia kodu języka Java, która opiera się na [dostawcy systemu android](http://developer.android.com/reference/android/provider/package-summary.html) interfejsu stałe.

Oprócz powyższych typów istnieją cztery dalsze zmiany:

1. Typ o nazwie identycznej z nazwą interfejsu języka Java jest generowany zawiera stałe.

1. Typy zawierające stałe interfejsu również zawierać wszystkich stałych, które pochodzą z implementowane interfejsy języka Java.

1. Wszystkie klasy, które implementują interfejs języka Java, zawierające stałe Uzyskaj nowy typ InterfaceConsts zagnieżdżonych zawierającą stałe ze wszystkich interfejsów zaimplementowane.

1. *Consts* typu jest obecnie przestarzała.


Dla *android.os.Parcelable* interfejs, oznacza to, że teraz będzie [ *Android.OS.Parcelable* ](https://developer.xamarin.com/api/type/Android.OS.Parcelable/) typu zawiera stałe. Na przykład [Parcelable.CONTENTS_FILE_DESCRIPTOR](http://developer.android.com/reference/android/os/Parcelable.html#CONTENTS_FILE_DESCRIPTOR) stała zostanie powiązany jako [ *Parcelable.ContentsFileDescriptor* ](https://developer.xamarin.com/api/field/Android.OS.Parcelable.ContentsFileDescriptor/) wartością stałą, a nie jako  *ParcelableConsts.ContentsFileDescriptor* stałej.

Dla interfejsów, zawierająca stałe implementujących innych interfejsów, zawierająca jeszcze więcej stałych sumę wszystkich stałych jest teraz generowany. Na przykład [android.provider.MediaStore.Video.VideoColumns](http://developer.android.com/reference/android/provider/MediaStore.Video.VideoColumns.html) implementuje interfejs [android.provider.MediaStore.MediaColumns](https://developer.xamarin.com/api/type/Android.Provider.MediaStore+MediaColumns/) interfejsu. Jednak przed 1.9 [Android.Provider.MediaStore.Video.VideoColumnsConsts](https://developer.xamarin.com/api/type/Android.Provider.MediaStore+Video+VideoColumnsConsts/) typu nie ma możliwości uzyskania dostępu do stałych zadeklarowanych w [Android.Provider.MediaStore.MediaColumnsConsts](https://developer.xamarin.com/api/type/Android.Provider.MediaStore+MediaColumnsConsts/).
W wyniku wyrażenia języka Java *MediaStore.Video.VideoColumns.TITLE* musi być powiązana wyrażenie języka C# *MediaStore.Video.MediaColumnsConsts.Title* który jest trudny do odnajdywania bez odczytu wiele dokumentację języka Java. 1.9, będą równoważne wyrażenie języka C# [ *MediaStore.Video.VideoColumns.Title*](https://developer.xamarin.com/api/field/Android.Provider.MediaStore+Video+VideoColumns.Title/).

Ponadto należy wziąć pod uwagę [android.os.Bundle](https://developer.xamarin.com/api/type/Android.OS.Bundle/) typ, który implementuje Java *Parcelable* interfejsu. Ponieważ implementuje interfejs, wszystkich stałych w tym interfejsie są dostępne "przez" typ pakietu, np. *Bundle.CONTENTS_FILE_DESCRIPTOR* jest idealnie prawidłowym wyrażeniem języka Java.
Wcześniej do portu to wyrażenie do języka C# należy spojrzeć na wszystkie interfejsy zaimplementowane w taki sposób, aby zobaczyć, z jakiego typu *CONTENTS_FILE_DESCRIPTOR* pochodzi. Począwszy od platformy Xamarin.Android 1.9 klasy, implementowanie interfejsów Java, które zawierają stałe będą miały zagnieżdżoną *InterfaceConsts* typ, który będzie zawierać stałe dziedziczony interfejs. Umożliwi to tłumaczenie *Bundle.CONTENTS_FILE_DESCRIPTOR* do [ *Bundle.InterfaceConsts.ContentsFileDescriptor*](https://developer.xamarin.com/api/field/Android.OS.Bundle+InterfaceConsts.ContentsFileDescriptor/).

Na koniec typów z *Consts* przyrostka, takich jak *Android.OS.ParcelableConsts* obecnie przestarzałe, inne niż nowo wprowadzonych InterfaceConsts zagnieżdżone typy. Zostaną one usunięte w 3.0 platformy Xamarin.Android.


## <a name="resources"></a>Resources

Obrazy, opisy układu, binarne obiektów blob i słowników ciągu, które mogą być zawarte w Twojej aplikacji w postaci [pliki zasobów](http://developer.android.com/guide/topics/resources/providing-resources.html).
Różne interfejsy API systemu Android są przeznaczone do [działają na identyfikatory zasobów](http://developer.android.com/guide/topics/resources/accessing-resources.html) zamiast radzenia sobie z obrazami, ciągi lub dane binarne w obiektach blob bezpośrednio.

Na przykład przykładowej aplikacji dla systemu Android, który zawiera układ interfejsu użytkownika ( `main.axml`), ciągiem funkcji wielojęzycznych tabeli ( `strings.xml`) i niektóre ikony ( `drawable-*/icon.png`) zachowa jej zasoby w katalogu "Zasoby" aplikacji:

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

Natywne interfejsy API systemu Android nie działają bezpośrednio z nazwami plików, ale zamiast operować na identyfikatory zasobów. Podczas kompilowania aplikacji dla systemu Android, która korzysta z zasobów, system kompilacji będzie pakiet zasobów w celu dystrybucji i wygenerować klasę o nazwie `Resource` zawierający tokeny dla każdej z nich uwzględnione zasoby. Na przykład dla powyższych zasobów układu, to co może narazić klasy R:

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

Następnie należy użyć `Resource.Drawable.icon` do odwołania `drawable/icon.png` pliku, lub `Resource.Layout.main` do odwołania `layout/main.xml` pliku, lub `Resource.String.first_string` można odwoływać się do pierwszego ciągu w pliku słownika `values/strings.xml`.


## <a name="constants-and-enumerations"></a>Stałe i wyliczenia

Natywne interfejsy API systemu Android mają wiele metod, które dopiero po lub zwraca całkowitą, która musi być zamapowany na pole stałe, aby ustalić, co oznacza int. Aby korzystać z tych metod, użytkownik jest wymagane, zapoznaj się z dokumentacją, aby zobaczyć, stałe, które są odpowiednie wartości, mniej niż idealne rozwiązanie.

Na przykład, rozważmy [Activity.requestWindowFeature (int featureID)](http://developer.android.com/reference/android/app/Activity.html#requestWindowFeature(int)).

W takich przypadkach firma Microsoft wszelkich starania, aby zgrupować pokrewnych stałych do wyliczenia .NET i ponowne mapowanie metody podjęcie zamiast tego wyliczenia.
Dzięki temu możemy oferować wybór technologii IntelliSense potencjalnych wartości.

Powyższy przykład staje się: [Activity.RequestWindowFeature (WindowFeatures featureId)](https://developer.xamarin.com/api/member/Android.App.Activity.RequestWindowFeature/p/Android.Views.WindowFeatures/)).

Należy pamiętać, że jest to bardzo ręczny proces, aby ustalić, które stałe należą ze sobą i które interfejsów API korzystać te stałe. Zgłoś usterki za jakiekolwiek wykorzystanie stałych w interfejsie API, która będzie lepiej wyrażona jako wyliczenie.
