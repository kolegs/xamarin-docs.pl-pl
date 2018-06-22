---
title: Grafiki i animacji
description: Android zapewnia bardzo zaawansowane i różnych framework do obsługi 2D grafiki i animacji. W tym temacie przedstawiono te struktury i omówiono sposób tworzenia niestandardowych grafiki i animacji do użycia w aplikacji platformy Xamarin.Android.
ms.prod: xamarin
ms.assetid: 80086318-6FE4-4711-9A71-5C8F8C28C754
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 85a7cd2ac5094a4760c5465bd099ce2e3beeae79
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
ms.locfileid: "30773790"
---
# <a name="graphics-and-animation"></a>Grafiki i animacji

_Android zapewnia bardzo zaawansowane i różnych framework do obsługi 2D grafiki i animacji. W tym temacie przedstawiono te struktury i omówiono sposób tworzenia niestandardowych grafiki i animacji do użycia w aplikacji platformy Xamarin.Android._


## <a name="overview"></a>Omówienie

Pomimo działające na urządzeniach, które są tradycyjnie ograniczona zasilania, najwyższy sklasyfikowane aplikacje mobilne często mają zaawansowane użytkownika środowisko (UX), wraz z wysokiej jakości grafiki i animacji, które zapewniają działanie intuicyjny, reakcji, dynamicznej. Jak get aplikacji dla urządzeń przenośnych bardziej zaawansowane opcje, użytkownicy zaczęli bardziej oczekiwać od aplikacji.

Szczęście firmie Microsoft, nowoczesnych platform urządzeń przenośnych mają bardzo zaawansowane struktury do tworzenia zaawansowanych animacji i niestandardowych grafiki przy zachowaniu łatwość użycia. Pozwala to deweloperom dodać rozbudowane środowisko interakcji z bardzo małego wysiłku.

Interfejs API struktur w systemie Android około może zostać podzielony na dwie kategorie: grafiki i animacji.

Grafika dalsze są dzielone na różne podejścia do wykonywania grafiki 2W i 3W. Grafiki 3D są dostępne za pośrednictwem szereg wbudowanych platform, takich jak OpenGL ES (przenośnych określonej wersji OpenGL) i struktur innych firm, takich jak MonoGame (toolkit międzyplatformowego zgodny z zestawu narzędzi platformy XNA). Chociaż grafiki 3D nie są w zakres tego artykułu, omówione wbudowanych 2D techniki rysowania.

Android zapewnia dwa różne interfejsu API do tworzenia 2D grafiki. Jeden jest podejście deklaratywne wysokiego poziomu, a druga programowe API niższego poziomu:

-   **Zasoby obiektów drawable** &ndash; są one używane do tworzenia niestandardowych grafiki programowo lub (więcej zwykle) osadzanie instrukcje rysowania w plikach XML. Obiektów drawable zasoby są zazwyczaj definiowane jako pliki XML, które zawierają instrukcje lub akcji dla systemu Android do renderowania grafiki 2D. 

-   **Kanwy** &ndash; jest niski poziom interfejsu API, która obejmuje rysowanie bezpośrednio na podstawowym mapy bitowej. Zapewnia bardzo precyzyjną kontrolę nad wyświetlanych. 

Oprócz tych metod grafiki 2D Android udostępnia kilka różnych sposobów tworzenia animacji:

-   **Animacje obiektów drawable** &ndash; Android obsługuje również znane jako animacji klatka po *animacji obiektów Drawable*. Jest to najprostsza animacji interfejsu API. Android sekwencyjnie ładuje i wyświetla obiektów Drawable zasoby w sekwencji (podobnie jak kreskówki).

-   **Wyświetl animacje** &ndash; *animacje widoku* są oryginalne animacji interfejsu API w systemie Android, są dostępne we wszystkich wersjach systemu android. Ten interfejs API jest ograniczone, ponieważ działa tylko z obiektami widoku i może wykonywać tylko proste przekształcenia w tych widokach.
    Widok animacje są zazwyczaj definiowane w plikach XML w `/Resources/anim` folderu.

-   **Właściwości animacji** &ndash; Android 3.0 wprowadzono nowy zestaw interfejsów API animacji znany jako *animacji właściwość*. Te nowe API wprowadzono extensible i elastyczny system, który może służyć do animowania właściwości wszystkich obiektów, nie tylko wyświetlanie obiektów. Tego rodzaju elastyczności umożliwia animacji są umieszczane w różnych klas, które spowoduje, że ułatwia udostępnianie kodu.


Widok animacje są bardziej odpowiednie dla aplikacji, które muszą obsługiwać starszych wstępne Android 3.0 interfejsu API (interfejs API na poziomie 11). W przeciwnym razie aplikacje powinny używać nowszej API animacji właściwość z przyczyn, które zostały wymienione powyżej.

Wszystkie te struktury są działało opcji, jednak w przypadku, gdy to możliwe, należy preferowane właściwości animacji, ponieważ jest bardziej elastyczne interfejsu API do pracy z. Właściwości animacji umożliwiają animacji logikę są umieszczane w różnych klas umożliwia łatwiejsze udostępnianie kodu, który upraszcza konserwację kodu.


## <a name="accessibility"></a>Ułatwienia dostępu

Grafiki i animacji ułatwiają aplikacji systemu Android atrakcyjne i fun użyty. jednak należy pamiętać, że niektóre interakcje wystąpić za pośrednictwem screenreaders, alternatywne urządzenia wejściowe, lub z asystowaną powiększenia.
Ponadto niektóre interakcje mogą wystąpić bez możliwości audio.

Aplikacje są bardziej użyteczne w takich przypadkach, jeśli zostały zaprojektowane z ułatwieniami dostępu pamiętać: wskazówki i pomoc w nawigacji w interfejsie użytkownika i sprawdzeniu zawartości tekstowej lub opisy obrazkami elementów interfejsu użytkownika.

Zapoznaj się [przewodnik ułatwień dostępu firmy Google](http://developer.android.com/guide/topics/ui/accessibility/) Aby uzyskać więcej informacji na temat sposobu wykorzystywać ułatwień dostępu systemu Android dla interfejsów API.



## <a name="2d-graphics"></a>2D grafiki

Zasoby obiektów drawable to technika popularnych w aplikacjach systemu Android. Zgodnie z innymi zasobami obiektów Drawable zasoby są deklaratywne &ndash; definiowania w plikach XML. Takie podejście umożliwia czyste rozdzielenie kodu z zasobów. Może to uprościć projektowanie i utrzymanie, ponieważ nie jest konieczne zmienić kod, aby zaktualizować lub zmienić grafiki w aplikacji systemu Android. Jednak podczas obiektów Drawable zasoby są przydatne w przypadku wielu proste i typowych wymagań grafiki, mają zasilania i kontrolę nad API kanwy.

Inne techniki przy użyciu [kanwy](https://developer.xamarin.com/api/type/Android.Graphics.Canvas/) obiektu, jest bardzo podobny do innych tradycyjnych struktur interfejsu API, takich jak System.Drawing lub rysunku Core dla systemu iOS. Przy użyciu obiektu kanwy zawiera większość kontrolę nad jak 2D grafiki są tworzone. Jest to przydatne w sytuacjach, w którym obiektów Drawable zasobów nie działa lub jest trudne do pracy z. Na przykład może być konieczne do rysowania niestandardowych suwak kontroli którego wygląd zmieni się na podstawie obliczeń względem wartości suwaka.

Przeanalizujmy najpierw obiektów Drawable zasobów. Są łatwiejsze i obejmuje najbardziej typowe przypadki rysowania niestandardowych.


### <a name="drawable-resources"></a>Obiektów drawable zasobów

Obiektów drawable zasoby są zdefiniowane w pliku XML w katalogu `/Resources/drawable`. W przeciwieństwie do osadzania PNG lub JPEG firmy, nie jest niezbędne do zapewnienia wersji gęstość konkretnych obiektów Drawable zasobów.
W czasie wykonywania aplikacji systemu Android obciążenia tych zasobów i umożliwia tworzenie grafiki 2D instrukcji zawartych w tych plikach XML.
Android definiuje kilka różnych typów obiektów Drawable zasobów:

-   [ShapeDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#Shape) &ndash; jest obiektów Drawable obiekt, który rysuje kształt geometrycznych pierwotne i stosuje ograniczony zestaw graficznego wpływ na tego kształtu. Są one przydatne dla elementów, takich jak dostosowywanie przycisków lub ustawienie tła TextViews. Zostanie wyświetlone przykład sposobu użycia `ShapeDrawable` dalszej części tego artykułu.

-   [StateListDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#StateList) &ndash; jest zasobem obiektów Drawable, która zmieni się wygląd na podstawie stanu elementu widget i kontrola. Na przykład przycisk może zmienić wygląd w zależności od tego, czy zostanie naciśnięty, czy nie.

-   [LayerDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#LayerList) &ndash; ten zasób obiektów Drawable będą umieszczane innych drawables jedną na inny. Przykład *LayerDrawable* przedstawiono na poniższym zrzucie ekranu:

    ![Przykład LayerDrawable](graphics-and-animation-images/image1.png)

-   [TransitionDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#Transition) &ndash; jest *LayerDrawable* , ale z jedną różnicą. A *TransitionDrawable* jest w stanie jedną warstwę wyświetlane na górze innego animowania.

-   [LevelListDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#LevelList) &ndash; jest bardzo podobny do *StateListDrawable* w tym będzie wyświetlać obrazu na podstawie określonych warunków. Jednak w przeciwieństwie do *StateListDrawable*, *LevelListDrawable* Wyświetla obraz oparty na wartość całkowitą. Przykład *LevelListDrawable* będzie można wyświetlić siła sygnału sieci Wi-Fi. Jak siła sygnału zmiany sieci Wi-Fi obiektów drawable, która jest wyświetlana zostanie zmieniona.

-   [ScaleDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#Scale)/[ClipDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#Clip) &ndash; jak nazwa ich wskazuje, te Drawables Podaj skalowanie i wycinka funkcji. *ScaleDrawable* mogą być skalowane innego podczas obiektów Drawable, *ClipDrawable* będzie obiektów Drawable innego.

-   [InsetDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#Inset) &ndash; ten obiektów Drawable zastosuje marginesy na stronach inny zasób obiektów Drawable. Jest używany, gdy widok musi tła, która jest mniejsza niż granice rzeczywisty dla widoku.

-   XML [BitmapDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#Bitmap) &ndash; ten plik jest zestawem instrukcje w formacie XML, które mają być wykonane na rzeczywiste mapy bitowej. Niektóre akcje, które mogą wykonywać Android są dzielenie na mniejsze strony roztrząsania i wygładzanie. Jednym z bardziej powszechne zastosowania to jest kafelków mapy bitowej w tle układ.


#### <a name="drawable-example"></a>Przykład obiektów drawable

Oto prosty przykład sposobu tworzenia ilustracja 2D używanie `ShapeDrawable`. A `ShapeDrawable` można określić jedną z czterech podstawowych kształtów: prostokąt, oval wiersza i pierścień. Użytkownik może również stosować podstawowe, takie jak gradientu, kolor i rozmiar. Następujący kod XML jest `ShapeDrawable` który można znaleźć w *AnimationsDemo* pomocnika projektu (w pliku `Resources/drawable/shape_rounded_blue_rect.xml`).
Definiuje prostokąt z purpurowa gradientu tła i zaokrąglone narożniki:

```xml
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android" android:shape="rectangle">
<!-- Specify a gradient for the background -->
<gradient android:angle="45"
          android:startColor="#55000066"
          android:centerColor="#00000000"
          android:endColor="#00000000"
          android:centerX="0.75" />

<padding android:left="5dp"
          android:right="5dp"
          android:top="5dp"
          android:bottom="5dp" />

<corners android:topLeftRadius="10dp"
          android:topRightRadius="10dp"
          android:bottomLeftRadius="10dp"
          android:bottomRightRadius="10dp" />
</shape>
```

Firma Microsoft może odwoływać się tego zasobu obiektów Drawable deklaratywnie w układzie lub innych Drawable, jak pokazano w poniższych XML:

```xml
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:background="#33000000">
    <TextView android:layout_width="wrap_content"
              android:layout_height="wrap_content"
              android:layout_centerInParent="true"
              android:background="@drawable/shape_rounded_blue_rect"
              android:text="@string/message_shapedrawable" />
</RelativeLayout>
```

Obiektów drawable zasoby mogą być stosowane również programowo. Poniższy fragment kodu przedstawia sposób programowane Ustawianie tła element TextView:

```csharp
TextView tv = FindViewById<TextView>(Resource.Id.shapeDrawableTextView);
tv.SetBackgroundResource(Resource.Drawable.shape_rounded_blue_rect);
```

Aby sprawdzić, jak to będzie wyglądać, uruchom *AnimationsDemo* projektu i wybierz element obiektów Drawable kształt z poziomu menu głównego. Firma Microsoft powinny zostać wyświetlone informacje podobne do następującego zrzutu ekranu:

![Element TextView na tle niestandardowych obiektów drawable z gradientu i zaokrąglone narożniki](graphics-and-animation-images/image1.png)

Aby uzyskać więcej informacji na temat elementów XML oraz składni obiektów Drawable zasobów, zapoznaj się [dokumentacji firmy Google](http://developer.android.com/guide/topics/resources/drawable-resource.html#Shape).


### <a name="using-the-canvas-drawing-api"></a>Przy użyciu interfejsu API rysowania kanwy

Drawables zaawansowanych, którzy mają ich ograniczenia. Pewne działania, które są bardzo skomplikowane, albo nie jest możliwe (na przykład: stosowanie filtru do obrazu, która została wykonana przez aparat w urządzeniu). Może być bardzo trudne do stosowania redukcji efektu czerwonych przy użyciu obiektów Drawable zasobów.
Zamiast tego interfejsu API kanwy umożliwia aplikacji ma bardzo precyzyjną kontrolę, aby selektywnie zmienić kolory określoną część obrazu.

Jedną klasę, która jest najczęściej używany z obszaru roboczego jest [Paint](https://developer.xamarin.com/api/type/Android.Graphics.Paint/) klasy. Ta klasa zawiera kolor, styl i informacji o sposobie rysowania. Służy do zapewnienia rzeczy koloru i przezroczystości.

Interfejs API kanwy wykorzystuje *modelu dla malarza* do rysowania 2D grafiki.
Operacje są stosowane w kolejnych warstw na siebie. Każda operacja obejmują niektóre obszar podstawowej mapy bitowej. Obszar nakłada się na wcześniej malowanego obszaru, nowe paint będzie częściowo lub całkowicie przesłaniać stary. Jest to tak samo jak działa wiele innych rysowania interfejsów API, takich jak System.Drawing i grafiki Core dla systemu iOS.

Istnieją dwa sposoby uzyskania `Canvas` obiektu. Obejmuje pierwszy sposób definiowania [mapy bitowej](https://developer.xamarin.com/api/type/Android.Graphics.Bitmap/) obiektu, a następnie utworzenie wystąpienia `Canvas` obiektu z nim. Na przykład poniższy fragment kodu tworzy nowy obszar roboczy z podstawowej mapy bitowej:

```csharp
Bitmap bitmap = Bitmap.CreateBitmap(100, 100, Bitmap.Config.Argb8888);
Canvas canvas = new Canvas(b);
```

Innym sposobem uzyskania `Canvas` obiektu jest [OnDraw](https://developer.xamarin.com/api/member/Android.Views.View.OnDraw/) metody wywołania zwrotnego, który został dostarczony [widoku](https://developer.xamarin.com/api/type/Android.Views.View/) klasy podstawowej. Android wywołuje tę metodę, gdy uzna widok powinien być rysowany sam i przekazuje w `Canvas` obiekt do pracy z widoku.

Klasa kanwy udostępnia metody programowo zapewniające instrukcje wystawiania. Na przykład:

-   [Canvas.DrawPaint](https://developer.xamarin.com/api/member/Android.Graphics.Canvas.DrawPaint/p/Android.Graphics.Paint/) &ndash; wypełnia całą kanwę mapy bitowej przy użyciu określonego malowania.

-   [Canvas.DrawPath](https://developer.xamarin.com/api/member/Android.Graphics.Canvas.DrawPath/p/Android.Graphics.Path/Android.Graphics.Paint/) &ndash; rysuje określonego kształtu geometrycznych przy użyciu określonego malowania.

-   [Canvas.DrawText](https://developer.xamarin.com/api/member/Android.Graphics.Canvas.DrawText/p/System.String/System.Single/System.Single/Android.Graphics.Paint/) &ndash; rysuje tekst w obszarze roboczym z określonym kolorem. Tekstu jest rysowana w lokalizacji `x,y` .



#### <a name="drawing-with-the-canvas-api"></a>Rysowanie za pomocą obszaru roboczego interfejsu API

Zobaczmy kanwy interfejsu API w akcji. Poniższy fragment kodu przedstawia sposób rysowania widoku:

```csharp
public class MyView : View
{
    protected override void OnDraw(Canvas canvas)
    {
        base.OnDraw(canvas);
        Paint green = new Paint {
            AntiAlias = true,
            Color = Color.Rgb(0x99, 0xcc, 0),
        };
        green.SetStyle(Paint.Style.FillAndStroke);

        Paint red = new Paint {
            AntiAlias = true,
            Color = Color.Rgb(0xff, 0x44, 0x44)
        };
        red.SetStyle(Paint.Style.FillAndStroke);

        float middle = canvas.Width * 0.25f;
        canvas.DrawPaint(red);
        canvas.DrawRect(0, 0, middle, canvas.Height, green);
    }
}
```

Powyższy kod najpierw tworzy red malowania i obiekt zielony malowania. Wstawia zawartość obszaru roboczego z czerwonym, a następnie instruuje kanwę, aby narysować zielony prostokąt o 25% szerokości obszaru roboczego. Na przykład można wyświetlić przez w `AnimationsDemo` projektu, który jest dołączony do kodu źródłowego dla tego artykułu. Uruchamianie aplikacji i wybierając rysowania elementu w menu głównym, możemy powinien ekran podobny do następującego:

![Ekran z czerwonym malowania i obiektami zielony malowania](graphics-and-animation-images/image3.png)


## <a name="animation"></a>Animacja

Użytkownicy, takich jak elementy łączące się zarówno w swoich aplikacjach. Animacje są to dobry sposób na ulepszyć środowisko użytkownika aplikacji i pomóc Wyróżnij. Najlepsze animacje są te, które użytkownicy nie zauważyć, ponieważ uważają fizycznych. Android zawiera następujące API trzy animacji:

-   **Wyświetlanie animacji** &ndash; jest oryginalnym interfejsu API. Te animacje są powiązane z określonym widoku i mogą wykonywać proste przekształcenia na zawartość widoku. Ze względu na prostotę jego ten interfejs API nadal przydatne dla elementów, takich jak alfa animacji, obrotów i tak dalej.

-   **Właściwości animacji** &ndash; animacji właściwość wprowadzono w programie Android 3.0. Umożliwiają one aplikacji w celu animowania prawie wszystko. Właściwości animacji można zmienić dowolną właściwość wszystkich obiektów, nawet jeśli ten obiekt nie jest widoczny na ekranie.

-   **Animacja obiektów drawable** &ndash; to specjalne zasób obiektów Drawable, który jest używany do zastosowania animacji bardzo proste mają wpływ na układów.

Ogólnie rzecz biorąc właściwości animacji jest preferowanym systemu do użycia, ponieważ jest bardziej elastyczna i oferuje więcej funkcji.


### <a name="view-animations"></a>Widok animacji

Widok animacje są ograniczone do widoków i może wykonywać tylko animacji na wartości, takie jak start i punktów końcowych, rozmiaru, obracanie i przezroczystość.
Tego rodzaju animacje są zwykle nazywane *animacji*. Widok animacji można zdefiniować dwa sposoby &ndash; programowo w kodzie lub przy użyciu plików XML. Pliki XML są preferowany sposób, aby zadeklarować animacje widoku, ponieważ są one bardziej czytelny i łatwiejsze w obsłudze.

Będą przechowywane pliki XML animacji w `/Resources/anim` katalogu projektu platformy Xamarin.Android. Ten plik musi mieć jeden z następujących elementów jako element główny:

-   `alpha` &ndash; Twierania lub fade-out animacji.

-   `rotate` &ndash; Animacja obrotu.

-   `scale` &ndash; Zmiany rozmiaru animacji.

-   `translate` &ndash; Ruchu poziomej lub pionowej.

-   `set` &ndash; Kontener, który może utrzymywać co najmniej jednego elementu animacji.

Domyślnie wszystkie animacje w pliku XML zostaną zastosowane jednocześnie. Aby animacje występują kolejno, ustaw `android:startOffset` atrybutu na jeden z elementów zdefiniowanych powyżej.

Użytkownik może wpływać na szybkość zmian w animacji przy użyciu *interpolatora*. Interpolatora sprawiają, że efekty animacji przyspieszony, powtarzane lub decelerated. Platformy systemu Android udostępnia kilka interpolatorów map fabrycznej, takich jak (między innymi):

-   `AccelerateInterpolator` / `DecelerateInterpolator` &ndash; te interpolatorów map zwiększyć lub zmniejszyć szybkość zmian w animacji.

-   `BounceInterpolator` &ndash; Zmiana odrzuceń na końcu.

-   `LinearInterpolator` &ndash; Liczba zmian jest stały.


Następujący kod XML przedstawiono przykład pliku animacji, który łączy niektóre z tych elementów:

```xml
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android=http://schemas.android.com/apk/res/android
     android:shareInterpolator="false">

    <scale android:interpolator="@android:anim/accelerate_decelerate_interpolator"
           android:fromXScale="1.0"
           android:toXScale="1.4"
           android:fromYScale="1.0"
           android:toYScale="0.6"
           android:pivotX="50%"
           android:pivotY="50%"
           android:fillEnabled="true"
           android:fillAfter="false"
           android:duration="700" />

    <set android:interpolator="@android:anim/accelerate_interpolator">
        <scale android:fromXScale="1.4"
               android:toXScale="0.0"
               android:fromYScale="0.6"
               android:toYScale="0.0"
               android:pivotX="50%"
               android:pivotY="50%"
               android:fillEnabled="true"
               android:fillBefore="false"
               android:fillAfter="true"
               android:startOffset="700"
               android:duration="400" />

        <rotate android:fromDegrees="0"
                android:toDegrees="-45"
                android:toYScale="0.0"
                android:pivotX="50%"
                android:pivotY="50%"
                android:fillEnabled="true"
                android:fillBefore="false"
                android:fillAfter="true"
                android:startOffset="700"
                android:duration="400" />
    </set>
</set>
```

Ta animacja będzie wykonywać wszystkie animacji jednocześnie. Pierwszy animacji skali zostanie obraz jest rozciągany w poziomie i pionie zmniejszyć rozmiar, a następnie obrazu zostanie jednocześnie być obracany 45 stopni w lewo i zmniejszyć znika z ekranu.

Animacji można programowo odnosić się do widoku pompowania animacji i zastosowaniu go do widoku. Android udostępnia klasę Pomocnika `Android.Views.Animations.AnimationUtils` które zwiększyć zasobów animacji i zwrócić wystąpienia `Android.Views.Animations.Animation`. Ten obiekt jest stosowany do widoku, wywołując `StartAnimation` i przekazywanie `Animation` obiektu. Poniższy fragment kodu przedstawia przykład:

```csharp
Animation myAnimation = AnimationUtils.LoadAnimation(Resource.Animation.MyAnimation);
ImageView myImage = FindViewById<ImageView>(Resource.Id.imageView1);
myImage.StartAnimation(myAnimation);
```

Teraz, gdy będziemy mieć podstawową wiedzę na temat działania animacji widoku, umożliwia przenoszenie właściwości animacji.


### <a name="property-animations"></a>Właściwości animacji

Twórcy właściwości animacji są nowy interfejs API, który został wprowadzony w systemie Android 3.0.
Zapewniają więcej rozszerzonego interfejsu API, który może służyć do animowania wszystkich właściwości dowolnego obiektu.

Wszystkie właściwości animacji zostały utworzone przy użyciu wystąpienia [zresztą](https://developer.xamarin.com/api/type/Android.Animation.Animator/) podklasy. Aplikacje nie bezpośrednio przy użyciu tej klasy, zamiast niego korzystają z jednej z jego podklas:

-   [ValueAnimator](https://developer.xamarin.com/api/type/Android.Animation.ValueAnimator/) &ndash; ta klasa jest klasą najważniejszych animacji właściwość całego interfejsu API. Obliczania wartości właściwości, które muszą zostać zmienione. `ViewAnimator` Nie bezpośrednio zaktualizować te wartości; zamiast tego zgłasza zdarzenia, które mogą służyć do aktualizowania obiektów animowany.

-   [ObjectAnimator](https://developer.xamarin.com/api/type/Android.Animation.ObjectAnimator/) &ndash; ta klasa jest podklasą `ValueAnimator` . Ten może uprościć proces Animowanie obiektów akceptując obiektu docelowego i właściwości do zaktualizowania.

-   [AnimationSet](https://developer.xamarin.com/api/type/Android.Animation.AnimatorSet/) &ndash; tej klasy jest odpowiedzialny za organizowanie sposób uruchamiania animacji w odniesieniu do siebie. Animacje może działać jednocześnie, sekwencyjnie lub z opóźnieniem określony między nimi.


*Ewaluatory* są specjalne klasy, które są używane przez twórcy animacji obliczania nowej wartości w animacji. Fabrycznej Android zapewnia następujące oceniających:

-   [IntEvaluator](https://developer.xamarin.com/api/type/Android.Animation.IntEvaluator/) &ndash; oblicza wartości dla właściwości, liczbą całkowitą.

-   [FloatEvaluator](https://developer.xamarin.com/api/type/Android.Animation.FloatEvaluator/) &ndash; oblicza wartości dla właściwości typu float.

-   [ArgbEvaluator](https://developer.xamarin.com/api/type/Android.Animation.ArgbEvaluator/) &ndash; oblicza wartości dla właściwości koloru.

Jeśli nie jest właściwość, która jest animowanej `float`, `int` lub kolor, aplikacje mogą tworzyć własne ewaluatora zaimplementowanie `ITypeEvaluator` interfejsu. (Implementowanie niestandardowych oceniających wykracza poza zakres tego tematu.)

#### <a name="using-the-valueanimator"></a>Przy użyciu ValueAnimator

Wszelkie animacji, składa się z dwóch części: obliczanie wartości animowany, a następnie ustawienie tych wartości właściwości w obiekcie niektórych. 
[ValueAnimator](https://developer.xamarin.com/api/type/Android.Animation.ValueAnimator/) obliczy tylko wartości, ale go nie będzie działać na obiektach bezpośrednio. Zamiast tego zostanie zaktualizowany obiektów wewnątrz obsługi zdarzeń, które zostanie wywołana podczas żywotność animacji. Ten projekt umożliwia kilka właściwości do zaktualizowania z jedną wartość animowany.

Uzyskać wystąpienia `ValueAnimator` wywołując jedną z następujących metod fabryki:

-  `ValueAnimator.OfInt`
-  `ValueAnimator.OfFloat`
-  `ValueAnimator.OfObject`

Po wykonaniu tej czynności, `ValueAnimator` wystąpienie musi mieć ustaw jego czas trwania, a następnie można go uruchomić. Poniższy przykład przedstawia sposób animować wartość z zakresu od 0 do 1 w okresie 1000 milisekund:

```csharp
ValueAnimator animator = ValueAnimator.OfInt(0, 100);
animator.SetDuration(1000);
animator.Start();
```

Ale, powyżej fragmentu kodu nie jest bardzo przydatny &ndash; zresztą będą działać, ale brak elementu docelowego dla zaktualizowanej wartości. `Animator` Klasa zostanie podniesiony zdarzenie aktualizacji, gdy decyduje o tym, że należy poinformować użytkowników o nową wartość. Aplikacje mogą dostarczać program obsługi zdarzeń, aby odpowiedzieć na to zdarzenie, jak pokazano w poniższy fragment kodu.

```csharp
MyCustomObject myObj = new MyCustomObject();
myObj.SomeIntegerValue = -1;

animator.Update += (object sender, ValueAnimator.AnimatorUpdateEventArgs e) =>
{
    int newValue = (int) e.Animation.AnimatedValue;
    // Apply this new value to the object being animated.
    myObj.SomeIntegerValue = newValue;
};
```

Teraz, gdy mamy zrozumienia `ValueAnimator`, umożliwia Dowiedz się więcej o `ObjectAnimator`.

#### <a name="using-the-objectanimator"></a>Przy użyciu ObjectAnimator

[ObjectAnimator](https://developer.xamarin.com/api/type/Android.Animation.ObjectAnimator/) jest podklasą `ViewAnimator` łączącą obliczenia aparat i wartość chronometrażu z `ValueAnimator` z logiką wymagane do okablować się procedury obsługi zdarzeń. `ValueAnimator` Wymaga aplikacjom jawnie okablować się program obsługi zdarzeń &ndash; `ObjectAnimator` zajmie się tego kroku firmie Microsoft.

Interfejs API dla `ObjectAnimator` jest bardzo podobny do interfejsu API dla `ViewAnimator`, ale wymaga podania obiektu i nazwę właściwości do zaktualizowania. W poniższym przykładzie przedstawiono przykład użycia `ObjectAnimator`:

```csharp
MyCustomObject myObj = new MyCustomObject();
myObj.SomeIntegerValue = -1;

ObjectAnimator animator = ObjectAnimator.OfFloat(myObj, "SomeIntegerValue", 0, 100);
animator.SetDuration(1000);
animator.Start();
```

Jak widać z poprzedniego fragmentu kodu, `ObjectAnimator` może zmniejszyć i ułatwić kodu, które są niezbędne do animowania obiektu.


### <a name="drawable-animations"></a>Animacje obiektów drawable

Animacja końcowe interfejsu API jest obiektów Drawable API animacji. Animacje obiektów drawable załadować szeregu zasobów obiektów Drawable jeden po drugim i wyświetlić je sekwencyjnie, podobnie jak kreskówki Przerzucanie it.

Obiektów drawable zasoby są zdefiniowane w pliku XML, który ma `<animation-list>` element jako element główny i szereg `<item>` elementów, które określają każdej ramki animacji. Plik XML znajduje się w `/Resource/drawable` folderu aplikacji. Następujący kod XML jest przykładem obiektów drawable animacji:

```xml
<animation-list xmlns:android="http://schemas.android.com/apk/res/android">
  <item android:drawable="@drawable/asteroid01" android:duration="100" />
  <item android:drawable="@drawable/asteroid02" android:duration="100" />
  <item android:drawable="@drawable/asteroid03" android:duration="100" />
  <item android:drawable="@drawable/asteroid04" android:duration="100" />
  <item android:drawable="@drawable/asteroid05" android:duration="100" />
  <item android:drawable="@drawable/asteroid06" android:duration="100" />
</animation-list>
```

Ta animacja zostanie uruchomiony przy użyciu sześciu ramki. `android:duration` Atrybut deklaruje, jak długo będą wyświetlane każdej ramce. Następny fragment kodu przedstawia przykład tworzenia obiektów Drawable animacji i jej uruchamianie, gdy użytkownik kliknie przycisk na ekranie:

```csharp
AnimationDrawable _asteroidDrawable;

protected override void OnCreate(Bundle bundle)
{
    base.OnCreate(bundle);
    SetContentView(Resource.Layout.Main);

    _asteroidDrawable = (Android.Graphics.Drawables.AnimationDrawable)
    Resources.GetDrawable(Resource.Drawable.spinning_asteroid);

    ImageView asteroidImage = FindViewById<ImageView>(Resource.Id.imageView2);
    asteroidImage.SetImageDrawable((Android.Graphics.Drawables.Drawable) _asteroidDrawable);

    Button asteroidButton = FindViewById<Button>(Resource.Id.spinAsteroid);
    asteroidButton.Click += (sender, e) =>
    {
        _asteroidDrawable.Start();
    };
}
```

Na tym etapie firma Microsoft ma omówione podstawy animacji interfejsami API dostępnymi w aplikacji systemu Android.


## <a name="summary"></a>Podsumowanie

W tym artykule wprowadzono wiele nowych pojęć i interfejsu API użytkownika, aby dodać niektóre elementy graficzne do aplikacji systemu Android. Najpierw opisano różne grafiki 2D interfejsu API i przedstawiono, jak Android umożliwia aplikacjom rysowanie bezpośrednio na ekranie przy użyciu obiektu kanwy. Widzieliśmy także niektóre alternatywne techniki, które umożliwia grafiki deklaratywnie należy utworzyć przy użyciu plików XML. Następnie poszliśmy celu omówienia starych i nowych interfejsów API do tworzenia animacji w systemie Android.



## <a name="related-links"></a>Linki pokrewne

- [Wersja demonstracyjna animacji (przykład)](https://developer.xamarin.com/samples/monodroid/AnimationDemo)
- [Animacja i grafiki](http://developer.android.com/guide/topics/graphics/index.html)
- [Można wyświetlić aplikacje mobilne do życia za pomocą animacji](http://youtu.be/ikSk_ILg3d0)
- [AnimationDrawable](https://developer.xamarin.com/api/type/Android.Graphics.Drawables.AnimationDrawable/)
- [Kanwa](https://developer.xamarin.com/api/type/Android.Graphics.Canvas/)
- [Zresztą obiektu](https://developer.xamarin.com/api/type/Android.Animation.ObjectAnimator/)
- [Wartość zresztą](https://developer.xamarin.com/api/type/Android.Animation.ValueAnimator/)
