---
title: Obliczenia matematyczne 2D za pomocą narzędzia CocosSharp
description: Ten przewodnik obejmuje 2D matematyki do tworzenia gier. Używa CocosSharp i pokazuje, jak wykonywać typowe zadania tworzenia gier i wyjaśnia matematycznych za te zadania.
ms.prod: xamarin
ms.assetid: 5C241AB4-F97E-4B61-B93C-F5D307BCD517
author: charlespetzold
ms.author: chape
ms.date: 03/27/2017
ms.openlocfilehash: 63c722b74c7dc7e034475e539f38204aca87763e
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242056"
---
# <a name="2d-math-with-cocossharp"></a>Obliczenia matematyczne 2D za pomocą narzędzia CocosSharp

_Ten przewodnik obejmuje 2D matematyki do tworzenia gier. Używa CocosSharp i pokazuje, jak wykonywać typowe zadania tworzenia gier i wyjaśnia matematycznych za te zadania._

To pozycjonowania i przenieść obiekty z kodem jest podstawowym składnikiem opracowywania gier różnej wielkości. Pozycjonowanie i przenoszenie korzystają z matematycznych, czy gra wymaga przenoszenia obiektu wzdłuż linii prostej lub użytkowania trygonometryczne obrotu. W tym dokumencie omówiono następujące tematy:

 - Szybkość pracy
 - Przyspieszanie
 - Obracanie obiektów w narzędziu CocosSharp
 - Obrót przy użyciu szybkości pracy

Deweloperzy, którzy nie mają silne matematyczne w tle lub który long zapomniane tych tematów z szkole, nie musisz martwić się — w tym dokumencie będzie podzielenie pojęcia na części wciągającą i będzie towarzyszyć teoretycznych wyjaśnienia z praktycznymi przykładami. Krótko mówiąc, w tym artykule zostanie znaleźć odpowiedź na pytanie uczniów odwieczny matematyczne: "Kiedy faktycznie muszę użyć różnych?"


## <a name="requirements"></a>Wymagania

Mimo że ten dokument koncentruje się głównie na stronie matematyczne CocosSharp, przykłady kodu założono, pracy z obiektami dziedziczenie formularzy `CCNode`. Ponadto, ponieważ `CCNode` nie zawiera wartości dla prędkości oraz zwiększanie ich szybkości w kodzie założono, Praca z jednostkami, które zawierają wartości, takich jak VelocityX, VelocityY, AccelerationX i AccelerationY. Aby uzyskać więcej informacji na temat jednostek, zobacz nasze wskazówki w [jednostki w narzędziu CocosSharp](~/graphics-games/cocossharp/entities.md).


## <a name="velocity"></a>Szybkość pracy

Deweloperzy gier używany jest termin *prędkości* opisujący, jak obiekt jest przenoszenie — w szczególności jak szybko coś, co jest przenoszenie i kierunek aby znajdował się przenoszenia. 

Prędkość jest definiowana za pomocą dwóch typów jednostek: jednostki czasu i Jednostka pozycji. Na przykład szybkość samochód jest zdefiniowana jako mil na godzinę lub kilometrach na godzinę. Deweloperzy gier przenosi często Użyj pikseli na sekundę do definiowania sposobu szybkiego obiektu. Na przykład punktor może przechodzić z prędkością 300 pikseli na sekundę. Oznacza to w przypadku przejścia w 300 pikseli na sekundę punktor, następnie go znajdzie się 600 jednostek w dwóch sekund i 900 jednostki w trzy sekundy i tak dalej. Ogólnie rzecz biorąc, wartość prędkości *dodaje* do położenia obiektu (jak opisano poniżej).

Mimo, że użyliśmy prędkości, aby wyjaśnić jednostki prędkości, szybkość termin zazwyczaj odwołuje się do wartości, które są niezależne od kierunku, gdy prędkość termin odnosi się do zarówno i kierunku. W związku z tym przypisanie prędkości punktor (przy założeniu, że punktor jest klasą, który zawiera wymagane właściwości) może wyglądać następująco:


```csharp
// This bullet is not moving horizontally, so set VelocityX to 0:
bulletInstance.VelocityX = 0;
// Positive Y is "up" so move the bullet up 300 units per second:
bulletInstance.VelocityY = 300;
```


### <a name="implementing-velocity"></a>Implementowanie szybkości pracy

CocosSharp nie implementuje szybkości, więc obiekty wymagające przenoszenia należy zaimplementować logikę przepływu. Nowych deweloperów gier — trzy często Implementowanie prędkości czuj polegający na wprowadzaniu ich prędkości zależy od szybkości klatek. Oznacza to, że *nieprawidłowa implementacja* będzie wydawać się, aby zapewnić poprawne wyniki, ale będzie zależeć od szybkości klatek gry:

```csharp
// VelocityX and VelocityY will be added every time this code executes
this.PositionX += this.VelocityX;
this.PositionY += this.VelocityY;
```

Jeśli gra działa na wyższe szybkości odtwarzania (na przykład 60 klatek na sekundę zamiast 30 klatek na sekundę), obiekt zostanie wyświetlony jako poruszać się szybciej, niż w przypadku uruchomiona z mniejszą szybkością ramki. Podobnie jeśli gra jest w stanie przetworzyć ramki w jako wysoki wskaźnik ramki (która może być spowodowany przez procesy w tle przy użyciu zasobów urządzenia), gra pojawi się spowolnienia.

To uwzględniać, szybkość pracy jest często implementowany przy użyciu wartości czasu. Na przykład jeśli `seconds` zmienna reprezentuje liczby (lub ułamka) sekund od czasu ostatniego prędkości czas została zastosowana, następnie spowodowałoby następujący kod w obiekcie o ruchu spójne, niezależnie od szybkości klatek:

```csharp
// VelocityX and VelocityY will be added every time this code executes
this.PositionX += this.VelocityX * seconds;
this.PositionY += this.VelocityY * seconds;
```

Należy rozważyć, czy gra, która jest uruchamiana w wolniejszym tempie ramki zostanie zaktualizowana pozycja jej obiektów mniej często. W związku z tym każda aktualizacja spowoduje obiektów przenoszenie dalszego niż w przypadku, jeśli gra były aktualizowane częściej. `seconds` Wartość konta w tym celu raportowanie, ile czasu minęło od ostatniej aktualizacji.

Na przykład sposobu dodawania przepływu na podstawie czasu zobacz [tego przepisu, obejmujących czas na podstawie ruchu](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/game_development/time_based_movement).


### <a name="calculating-positions-using-velocity"></a>Obliczanie stanowisk przy użyciu szybkości pracy

Szybkość pracy może służyć do tworzenia prognoz, o której obiekt będzie po pewien stopień upływu czasu lub dostosować zachowanie obiektów bez konieczności uruchamiania gry. Na przykład deweloper, który implementuje przepływu wyzwolone punktor musi ustawić prędkości punktor, po zostanie uruchomiony. Rozmiar ekranu może służyć do stanowią podstawę do ustawiania szybkość pracy. Oznacza to jeśli deweloper wie, że punktor należy przenieść wysokość ekranu w 2 sekundy, a następnie szybkość pracy powinna być równa wysokość ekranu podzielonej przez 2. Jeśli ekran 800 pikseli wysokości, szybkość punktor, będzie miał ustawienie do 400 (czyli 800/2).

Podobnie logiki w grze może być konieczne do obliczenia, jak długo obiekt spowoduje przejście do miejsca docelowego, biorąc pod uwagę jej szybkość pracy. To może być obliczany przez podzielenie odległości przechodzić przez obiekt podróży prędkości. Na przykład, poniższy kod przedstawia sposób przypisywania tekstu do etykiety, która wyświetla ile aż pocisków osiągnie wartość docelową:


```csharp
// We'll assume only the X axis for this example
float distanceX = target.PositionX - missile.PositionX;

float secondsToReachTarget = distanceX / missile.VelocityX;

label.Text = secondsToReachTarget + " seconds to reach target"; 
```


## <a name="acceleration"></a>Przyspieszanie

*Przyspieszanie* typowych pojęcie się projektowaniem gier i go udostępnia wiele podobieństw szybkość pracy. Przyspieszanie podaje wielkość, czy obiekt jest przyspieszenie spowalniania (jak wartość prędkości zmienia się wraz z upływem czasu). Przyspieszanie *dodaje* szybkość, tak samo, jak szybkość pracy, dodaje do pozycji. Popularne aplikacje przyspieszenie obejmują grawitacji, samochód, przyspieszanie i miejsca na statku wyzwalania jej mechanizmy do wymuszania. 

Przypomina szybkość przyspieszenia jest zdefiniowany w pozycji, a jednostka czasu; jednak firmy przyspieszenie jednostkę czasu jest określany jako *kwadratów* jednostki, która odzwierciedla sposób przyspieszenia zdefiniowano matematycznych. Oznacza to, przyspieszanie gier często jest mierzony w *kwadrat pikseli na sekundę*.

Jeśli obiekt ma przyspieszenie X 10 jednostek na sekundę kwadrat, następnie oznacza jego prędkość wzrosną 10 co sekundę. Jeśli od zera, po jednej sekundy będzie przenoszenia na 10 jednostek na sekundę, po dwóch sekund 20 jednostek na sekundę, i tak dalej.

Przyspieszenie w dwóch wymiarach wymaga składnika X i Y, więc może ona zostać przypisana w następujący sposób:


```csharp
// No horizontal acceleration:
icicle.AccelerationX = 0;
// Simulate gravity with Y acceleration. Negative Y is down, so assign a negative value:
icicle.AccelerationY = -50; 
```


### <a name="acceleration-vs-deceleration"></a>Składniki Akceleracja — a opóźnienia

Mimo że przyspieszenia i opóźnienia są czasami zróżnicowane w mowy — codziennie, nie ma techniczne różnic między nimi. Grawitacji jest wymuszone, co skutkuje przyspieszenie. Jeśli obiekt jest zgłaszany w górę następnie grawitacji spowolni go (spowolnienie), ale gdy obiekt został zatrzymany, Wspinanie się i pozostaje w tym samym kierunku co grawitacji następnie grawitacji przyspiesza jego (przyspieszanie). Jak pokazano poniżej, aplikacja przyspieszenie jest taki sam, czy jest stosowana w tym samym kierunku lub przeciwieństwem kierunek ruchu. 


### <a name="implementing-acceleration"></a>Wykonywania przyspieszania

Przyspieszanie przypomina szybkość pracy podczas implementowania — nie jest automatycznie implementowana przez CocosSharp, a przyspieszanie na podstawie czasu jest żądaną implementację (w przeciwieństwie do opartych na klatkach acceleration). W związku z tym może wyglądać wdrożenie prostego przyspieszenie (wraz z prędkości):

```csharp
this.VelocityX += this.AccelerationX * seconds;
this.VelocityY += this.AccelerationY * seconds;
this.PositionX += this.VelocityX * seconds;
this.PositionY += this.VelocityY * seconds;
```

Powyższy kod to, co jest nazywane *liniowej zbliżenia* przyspieszenie realizacji. Czynność implementuje przyspieszenie dość Zamknij stopień dokładności, ale nie jest modelem doskonale dokładne przyspieszenia. Wymienione powyżej ułatwia zrozumienie pojęcia implementacji przyspieszenia.

Następującą implementacją jest aplikacją ze sobą matematycznie dokładne przyspieszenia i szybkość pracy:


```csharp
float halfSecondsSquared = (seconds * seconds) / 2.0f;

this.PositionX += 
    this.Velocity.X * seconds + this.AccelerationX * halfSecondsSquared;
this.PositionY += 
    this.Velocity.Y * seconds + this.AccelerationY * halfSecondsSquared;

this.VelocityX += this.AccelerationX * seconds;
this.VelocityY += this.AccelerationY * seconds; 
```

Jest najbardziej oczywiste miała wpływ na powyższy kod `halfSecondsSquared` zmiennej i sposób jej użycia do zastosowania przyspieszania do pozycji. Matematyczne przyczyną tego błędu wykracza poza zakres tego samouczka, ale deweloperów zainteresowanych matematycznych za to można znaleźć więcej informacji w [tej dyskusji o integrowaniu przyspieszenia.](http://www.cliffsnotes.com/math/calculus/calculus/integration/distance-velocity-and-acceleration)

Praktyczne wpływ `halfSecondSquare` jest, że przyspieszenie będą zachowywać się ze sobą matematycznie dokładne i przewidywalnego niezależnie od szybkości klatek. Liniowy zbliżenia przyspieszenie podlega szybkość klatek — niższej szybkości klatek spada, staje się zbliżenia mniej dokładne. Za pomocą `halfSecondsSquared` gwarantuje, że kod będzie działa tak samo niezależnie od szybkości klatek.


## <a name="angles-and-rotation"></a>Kąty i obrót

Wizualizacji obiektów, takich jak `CCSprite` obsługuje obracania za pośrednictwem `Rotation` zmiennej. To można przypisać wartość można ustawić jego obrotu w stopniach. Na przykład, poniższy kod pokazuje, jak obrócić `CCSprite` wystąpienie:


```csharp
CCSprite unrotatedSprite = new CCSprite("star.png");
unrotatedSprite.IsAntialiased = false;
unrotatedSprite.PositionX = 100;
unrotatedSprite.PositionY = 100;
this.AddChild (unrotatedSprite);

CCSprite rotatedSprite = new CCSprite("star.png");
rotatedSprite.IsAntialiased = false;
// This sprite is moved to the right so it doesn’t overlap the first
rotatedSprite.PositionX = 130;
rotatedSprite.PositionY = 100;
rotatedSprite.Rotation = 45;
this.AddChild (rotatedSprite); 
```

Wyniki będą następujące:

![](math-images/image1.png "Skutkuje to tego zrzutu ekranu")

Zauważ, że obrót 45 stopni w prawo (które ze względów historycznych jest przeciwieństwem sposób matematycznych zastosowania obrotu):

![](math-images/image2.png "Zauważ, że obrót 45 stopni w prawo który ze względu na historycznych jest przeciwieństwem sposób matematycznych zastosowania obrotu")

Ogólnie rzecz biorąc obrotu CocosSharp ścisłe i matematykę regularne mogą być wizualizowane w następujący sposób:

![](math-images/image3.png "Ogólnie rzecz biorąc obrotu CocosSharp ścisłe i matematykę regularne mogą być wizualizowane w taki sposób, jak to")

![](math-images/image4.png "Ogólnie rzecz biorąc obrotu CocosSharp ścisłe i matematykę regularne mogą być wizualizowane w taki sposób, jak to")

Ważne jest, wykonywania tego rozróżnienia ponieważ `System.Math` klasa używa rotacji do ruchu wskazówek zegara, więc deweloperom zapoznać się z tej klasy muszą Odwróć kąty, pracując z `CCNode` wystąpień.

Należy zauważyć, że powyższe diagramy wyświetlać obrotu w stopniach; Jednak niektóre funkcje matematyczne (takich jak funkcje w `System.Math` przestrzeni nazw) oczekują i zwracają wartości w *radianów* zamiast stopni. Omówimy sposób konwersji między typami dwie jednostki nieco później w tym przewodniku.


### <a name="rotating-to-face-a-direction"></a>Wymiana twarzy na kierunek

Jak wspomniano powyżej, `CCSprite` można obracać przy użyciu `Rotation` właściwości. `Rotation` Właściwości są dostarczane przez `CCNode` (klasę bazową dla `CCSprite`), co oznacza, że obrót mogą być stosowane do jednostek, które dziedziczą z `CCNode` także. 

Niektóre gry wymagają obiektów, aby obracać, więc muszą ponosić obiektu docelowego. Przykładami są kontrolowane przez komputer wroga, rozwiązywania problemów w celu player lub statku miejsca na lot kierunku punktu, w którym użytkownik jest dotykanie ekranu. Jednak wartość obrotu musi najpierw obliczona na podstawie lokalizacji jednostki są obracane i lokalizacji docelowej nie ustąpi.

Ten proces wymaga kilku kroków:

 - Identyfikowanie *przesunięcie* obiektu docelowego. Przesunięcie odnosi się do X i Y odległość między jednostką rotacji, a Jednostka docelowa.
 - Obliczanie kąta przesunięcie przy użyciu funkcji trygonometryczne arcus tangens (opisana szczegółowo poniżej).
 - Dostosowanie różnica pomiędzy 0 stopni skierowana w prawo i kierunek wskazujący rotacji jednostki, podczas odinstalowywania obrócony.
 - Dostosowywanie różnicę między obrotu matematyczne (do ruchu wskazówek zegara) i obrót CocosSharp (prawo).

Następującą funkcję (założono, że ma zostać zapisany w jednostce) obraca się jednostki, aby obiekt docelowy:


```csharp
// This function assumes that it is contained in a CCNode-inheriting object
public void FacePoint(float targetX, float targetY)
{
    // Calculate the offset - the target's position relative to "this"
    float xOffset = targetX - this.PositionX;
    float yOffset = targetY - this.PositionY;

    // Make sure the target isn't the same point as "this". If so,
    // then rotation cannot be calculated.
    if (targetX != this.PositionX || targetY != this.Position.Y)
    {

        // Call Atan2 to get the radians representing the angle from 
        // "this" to the target
        float radiansToTarget = (float)System.Math.Atan2 (yOffset, xOffset);

        // Since CCNode uses degrees for its rotation, we need to convert
        // from radians
        float degreesToTarget = CCMathHelper.ToDegrees (radiansToTarget);

        // The direction that the entity faces when unrotated. In this case
        // the entity is facing "up", which is 90 degrees 
        const float forwardAngle = 90;

        // Adjust the angle we want to rotate by subtracting the
        // forward angle.
        float adjustedForDirecitonFacing = degreesToTarget - forwardAngle;

        // Invert the angle since CocosSharp uses clockwise rotation
        float cocosSharpAngle = adjustedForDirecitonFacing * -1;

        // Finally assign the rotation
        this.Rotation = rotation = cocosSharpAngle;
    }
} 
```

Powyższy kod może służyć do obracania jednostki, więc go twarzy punktu, w którym użytkownik jest dotykanie ekranu, w następujący sposób:


```csharp
private void HandleInput(System.Collections.Generic.List<CCTouch> touches, CCEvent touchEvent)
{
    if(touches.Count > 0)
    {
        CCTouch firstTouch = touches[0];
        FacePoint (firstTouch.Location.X, firstTouch.Location.Y);
    }
} 
```

Ten kod powoduje następujące zachowanie:

![](math-images/image5.gif "Ten kod powoduje to zachowanie")

#### <a name="using-atan2-to-convert-offsets-to-angles"></a>Użycie Atan2 w celu przekonwertowania przesunięcia do kąty

`System.Math.Atan2` może służyć do przekonwertowania przesunięcie do kąta. Nazwa funkcji `Atan2` pochodzi z arcus tangens trygonometryczne funkcji. Ta funkcja pozwala odróżnić sufiks "2" ze standardu `Atan` funkcji, która ściśle odpowiada matematyczne zachowanie tangens. Arcus tangens to funkcja, która zwraca wartość z zakresu od -90 + 90 stopni i (lub równowartość w radianach). Wiele aplikacji, w tym gry komputera, często wymagają pełnego 360 stopni wartości, więc `Math` klasa zawiera `Atan2` spełnić te wymagania.

Należy zauważyć, że powyższy kod przekazuje parametr Y po pierwsze, następnie parametru X podczas wywoływania `Atan2` metody. Jest to wstecz od zwykłych X, Y kolejność współrzędne. Aby uzyskać więcej informacji [zobacz dokumentacja Atan2](https://msdn.microsoft.com/library/system.math.atan2(v=vs.110).aspx).

Warto również zauważyć, że wartość zwracana z `Atan2` znajduje się w radianach, czyli innej jednostki używane do mierzenia kąty. Ten przewodnik nie obejmuje szczegóły wartość w radianach, ale należy pamiętać, że wszystkie funkcje trygonometryczne w `System.Math` przestrzeni nazw użyj wartość w radianach, więc wszystkie wartości muszą zostać skonwertowane do stopni przed ich użyciem w narzędziu CocosSharp obiektów. Można znaleźć więcej informacji na temat radianów [w radianach strony Wikipedii](http://en.wikipedia.org/wiki/Radian).

#### <a name="forward-angle"></a>Kąt do przodu

Gdy `FacePoint` metoda konwertuje wartość kąta na radiany, definiuje `forwardAngle` wartość. Ta wartość reprezentuje kąt, w której jednostka jest skierowany w stronę po jego obrotu wartość jest równa 0. W tym przykładzie przyjęto założenie, że jednostka jest skierowany w stronę w górę, czyli 90 stopni korzystając rotację matematyczne (w przeciwieństwie obracanie CocosSharp). Używamy obrotu matematyczne w tym miejscu, ponieważ firma Microsoft nie zostały jeszcze odwrócony rotacji dla narzędzia CocosSharp.

Poniżej pokazano, jakie jednostki z `forwardAngle` o 90 stopni może wyglądać jak:

![](math-images/image6.png "Pokazuje, jak może wyglądać jednostki z forwardAngle o 90 stopni")


### <a name="angled-velocity"></a>Pod kątem szybkości pracy

Do tej pory Opisaliśmy sposób konwertowania przesunięcie pod kątem. W tej sekcji przechodzi w inny sposób — przyjmuje kąt i konwertuje je do X i Y wartości. Typowe przykłady samochodu przenoszenia w kierunku, w którym jest dostępny z lub rozwiązywania problemów punktor, który przenosi w kierunku, w którym jest skierowany w stronę statku statku miejsca. 

Model można obliczyć prędkości pierwszy Definiowanie odpowiednią szybkość pracy, podczas odinstalowywania obrócony, a następnie Obracanie tego prędkości kąt, którego podmiot jest skierowany w stronę. Aby pomóc w wyjaśnieniu tę koncepcję, szybkość pracy (oraz zwiększanie ich szybkości) mogą być wizualizowane jako 2-wymiarowej *wektor* (który jest zazwyczaj rysowany jako strzałka). Wektor wartości prędkości x = 100 i Y = 0, które mogą być wizualizowane w następujący sposób:

![](math-images/image7.png "Wektor wartości prędkości x = 100 i Y = 0, które mogą być wizualizowane w następujący sposób")

Ta wektora można obracać spowodować nowe szybkość pracy. Na przykład rotacji wektora o 45 stopni (przy użyciu obrotu przeciwnie do ruchu wskazówek zegara) wyniki będą następujące:

![](math-images/image8.png "Obracanie wektora o 45 stopni przy użyciu wyników obrotu przeciwnie do ruchu wskazówek zegara, w tym")

Na szczęście prędkości, przyspieszanie i położenia nawet wszystkie obracać się przy użyciu tego samego kodu, niezależnie od tego, jak wartości zostają zastosowane. Następującą funkcję ogólnego przeznaczenia może służyć do obracania wektora za pomocą wartości obrotu CocosSharp:


```csharp
// Rotates the argument vector by degrees specified by
// cocosSharpDegrees. In other words, the rotation
// value is expected to be clockwise.
// The vector parameter is modified, so it is both an in and out value
void RotateVector(ref CCVector2 vector, float cocosSharpDegrees)
{
    // Invert the rotation to get degrees as is normally
    // used in math (counterclockwise)
    float mathDegrees = -cocosSharpDegrees;

    // Convert the degrees to radians, as the System.Math
    // object expects arguments in radians
    float radians = CCMathHelper.ToRadians (mathDegrees);

    // Calculate the "up" and "right" vectors. This is essentially
    // a 2x2 matrix that we'll use to rotate the vector
    float xAxisXComponent = (float)System.Math.Cos (radians);
    float xAxisYComponent = (float)System.Math.Sin (radians);
    float yAxisXComponent = (float)System.Math.Cos (radians + CCMathHelper.Pi / 2.0f);
    float yAxisYComponent = (float)System.Math.Sin (radians + CCMathHelper.Pi / 2.0f);

    // Store the original vector values which will be used
    // below to perform the final operation of rotation.
    float originalX = vector.X;
    float originalY = vector.Y;

    // Use the axis values calculated above (the matrix values)
    // to rotate and assign the vector.
    vector.X = originalX * xAxisXComponent + originalY * yAxisXComponent;
    vector.Y = originalX * xAxisYComponent + originalY * yAxisYComponent;
} 
```

Pełnego zrozumienia `RotateVector` metoda wymaga, że znasz funkcje trygonometryczne cosinus i sinusa, wraz z niektórych algebry liniowy, który wykracza poza zakres tego artykułu. Jednak gdy zaimplementowane `RotateVector` metoda może służyć do obracania wszelkie wektor, w tym wektora prędkości. Na przykład, poniższy kod może wyzwalać punktor w kierunku określony przez `rotation` wartość:


```csharp
// Create a Bullet instance
Bullet newBullet = new Bullet();

// Define the velocity of the bullet when 
// rotation is 0
CCVector2 velocity = new CCVector2 (0, 100);

// Modify the velocity according to rotation
RotateVector (ref velocity, rotation);

// Assign the newBullet's velocity using the
// rotated vector
newBullet.VelocityX = velocity.X;
newBullet.VelocityY = velocity.Y;

// Set the bullet's rotation so it faces
// the direction that it's flying
newBullet.Rotation = rotation; 
```

Ten kod może powodować mniej więcej tak:

![](math-images/image9.png "Ten kod może generować podobny do tego zrzutu ekranu")


## <a name="summary"></a>Podsumowanie

Ten przewodnik obejmuje typowe pojęcia matematyczne się projektowaniem gier 2D. Pokazuje, jak przypisać i zaimplementować prędkości oraz zwiększanie ich szybkości i opisano, jak obrócić obiekty i metody w przypadku przenoszenia w dowolnym kierunku.
