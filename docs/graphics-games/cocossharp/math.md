---
title: 2D matematyczne z CocosSharp
description: "W tym przewodniku dotyczą 2D matematyce do tworzenia gier. Wykorzystuje CocosSharp aby pokazują, jak wykonywać typowe zadania tworzenia gier oraz wyjaśniono matematyczne za te zadania."
ms.topic: article
ms.prod: xamarin
ms.assetid: 5C241AB4-F97E-4B61-B93C-F5D307BCD517
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/27/2017
ms.openlocfilehash: 7573ca423c3d9462d400f117c2116209e7c2a410
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="2d-math-with-cocossharp"></a>2D matematyczne z CocosSharp

_W tym przewodniku dotyczą 2D matematyce do tworzenia gier. Wykorzystuje CocosSharp aby pokazują, jak wykonywać typowe zadania tworzenia gier oraz wyjaśniono matematyczne za te zadania._

Do umieszczenia i przenieść obiekty kod jest częścią core tworzenie gier różnej wielkości. Pozycjonowanie i przeniesienie wymagają użycia matematyczne, czy gier wymaga przenoszenia obiektu wzdłuż linii prostej lub stosowania trygonometryczne obrotu. Tym dokumencie omówiono następujące tematy:

 - Szybkość pracy
 - Akceleracja
 - Obracanie CocosSharp obiektów
 - Przy użyciu Obrót o szybkość pracy

Deweloperzy, którzy nie mają tła silne matematyczne lub który long zapomni tych tematów z służbowe, nie trzeba martwić — ten dokument zostanie rozbicie pojęcia na części o rozmiarze raz i będzie towarzyszyć teoretycznego wyjaśnienia wraz z przykładami praktyczne. Krótko mówiąc, w tym artykule będzie odpowiedzi na pytanie uczniowie age-old matematyczne: "Kiedy zostanie faktycznie należy użyć tego rzeczy?"


# <a name="requirements"></a>Wymagania

Chociaż ten dokument koncentruje się przede wszystkim na stronie matematyczne CocosSharp, przykłady kodu założono pracy z obiektami dziedziczenie formularza `CCNode`. Ponadto, ponieważ `CCNode` nie zawiera wartości szybkość pracy i przyspieszenie kodu zakłada pracy z obiektami, które zawierają wartości, takie jak VelocityX, VelocityY AccelerationX i AccelerationY. Aby uzyskać więcej informacji dotyczących jednostek, zobacz nasze wskazówki w [jednostek CocosSharp](~/graphics-games/cocossharp/entities.md).


# <a name="velocity"></a>Szybkość pracy

Deweloperzy gier używany jest termin *prędkość* opisano, jak obiekt jest przenoszenie — w szczególności szybkość coś porusza się i kierunek jego są przenoszenia. 

Szybkość pracy jest definiowana za pomocą dwa typy jednostek: jednostki czasu i Jednostka pozycji. Na przykład szybkość samochodu jest zdefiniowany jako milach na godzinę lub kilometrach na godzinę. Deweloperzy gier przenosi często Użyj pikseli na sekundę do definiowania sposobu szybkiego obiektu. Na przykład punktor może przenieść z prędkością 300 pikseli na sekundę. Oznacza to, że w przypadku przenoszenia na 300 pikseli na sekundę punktor, następnie go znajdzie się 600 jednostki w dwóch sekund i 900 jednostki trzy sekundy, i tak dalej. Ogólnie rzecz biorąc, wartość prędkość *dodaje* położenie obiektu (jak zajmiemy się poniżej).

Mimo że użyliśmy szybkości wyjaśnić jednostki prędkość szybkości termin zazwyczaj odwołuje się do niezależnie od kierunku, wartość podczas prędkość termin dotyczy zarówno i kierunku. W związku z tym przypisania prędkość punktor (przy założeniu, że punktor jest klasa, która zawiera wymagane właściwości) może wyglądać następująco:


```csharp
// This bullet is not moving horizontally, so set VelocityX to 0:
bulletInstance.VelocityX = 0;
// Positive Y is "up" so move the bullet up 300 units per second:
bulletInstance.VelocityY = 300;
```


## <a name="implementing-velocity"></a>Implementowanie szybkość pracy

CocosSharp nie implementuje szybkość pracy, dlatego obiekty wymagające przepływu należy wdrożyć logikę własnych przepływu. Nowych deweloperów gier często Implementowanie prędkość skonfiguruj błąd polegający na wprowadzaniu ich prędkość zależał od szybkość klatek. Oznacza to, że *niepoprawna implementacja* będzie wydawać się zapewnienie poprawnych wyników, ale będą oparte na szybkość klatek gry:

```csharp
// VelocityX and VelocityY will be added every time this code executes
this.PositionX += this.VelocityX;
this.PositionY += this.VelocityY;
```

Jeśli gra działa na poziomie wyższym ramki (na przykład 60 klatek na sekundę, zamiast 30 klatek na sekundę), obiekt zostanie wyświetlony można przenieść szybciej niż w przypadku uruchomiona na wolniejszych szybkość klatek. Podobnie jeśli gry nie może przetworzyć ramek na wysoki szybkość (która może być spowodowany przez procesy w tle przy użyciu zasobów urządzenia), gry będą wyświetlane spowalniać.

Aby to uwzględniać, prędkość często zaimplementowano za pomocą wartości typu time. Na przykład jeśli `seconds` zmiennej reprezentuje numer (lub ułamek) sekund od czasu ostatniego prędkość czasu został zastosowany, a następnie spowoduje następujący kod w obiekcie o ruchu spójne niezależnie od szybkość klatek:

```csharp
// VelocityX and VelocityY will be added every time this code executes
this.PositionX += this.VelocityX * seconds;
this.PositionY += this.VelocityY * seconds;
```

Należy rozważyć gry uruchamianego w dolnym szybkość klatek uaktualnią pozycji obiektów mniej często. W związku z tym każda aktualizacja spowoduje obiektów przenoszenie więcej niż gdyby gry aktualizowany częściej. `seconds` Wartość kont w tym celu raportowanie, czas, jaki upłynął od ostatniej aktualizacji.

Na przykład sposobu dodawania ruchu na podstawie czasu zobacz [ruchu na podstawie tego przepisu obejmujące czas](https://developer.xamarin.com/recipes/cross-platform/game_development/time_based_movement/).


## <a name="calculating-positions-using-velocity"></a>Obliczanie stanowisk przy użyciu szybkość pracy

Szybkość pracy może służyć do tworzenia prognoz, o której będzie obiektu po niektórych ilość czasu, lub aby dostosować zachowanie obiektów bez konieczności uruchomienia gry. Na przykład deweloper, który implementuje przepływu wypalane punktor musi ustawić prędkość punktor po zostanie on uruchomiony. Rozmiar ekranu może służyć do stanowią podstawę do ustawiania szybkość pracy. Oznacza to czy dewelopera wie, że punktor należy przenosić wysokości ekranu w 2 sekundy, a następnie prędkości powinien być ustawiony na wysokość ekranu podzielona przez 2. Jeśli ekran jest wysokości 800 pikseli, szybkość punktor czy ustawić do 400 (czyli 800/2).

Podobnie logikę w gry może być konieczne obliczenia, jak długo obiektu spowoduje przejście do punktu docelowego podane jego szybkość pracy. To może być obliczany przez podzielenie odległości do podróży prędkość podróży obiektu. Na przykład poniższy kod przedstawia jak przypisać tekst etykiety, który zawiera informacje na jak długo dopóki pocisków osiągnie wartość docelową:


```csharp
// We'll assume only the X axis for this example
float distanceX = target.PositionX - missile.PositionX;

float secondsToReachTarget = distanceX / missile.VelocityX;

label.Text = secondsToReachTarget + " seconds to reach target"; 
```


# <a name="acceleration"></a>Akceleracja

*Akceleracja* jest działaniem typowych w opracowywaniu gier, i go udostępnia wiele podobieństw szybkość pracy. Akceleracja podaje wielkość, czy obiekt jest przyspieszenie spowolnieniem (jak wartość prędkość zmienia się wraz z upływem czasu). Akceleracja *dodaje* do prędkości, podobnie jak prędkość dodaje się do pozycji. Aplikacje przyspieszenia obejmują grawitacji, samochodu przyspieszania i statku miejsca wyzwalania jego mechanizmy do wymuszania. 

Podobnie jak prędkość, przyspieszenie jest zdefiniowany w pozycji, a jednostka czasu; jednak przyspieszenie na jednostkę czasu jest określany jako *kwadratów* jednostki, która odzwierciedla sposób przyspieszenia zdefiniowano ze sobą matematycznie. Oznacza to, gier przyspieszenie często jest mierzony w *kwadrat pikseli na sekundę*.

Jeśli obiekt ma przyspieszenie X 10 jednostek na sekundę kwadrat, następnie oznacza to, że jego prędkość wzrośnie w ciągu sekundy 10. Jeśli od zera, po jednej sekundy będzie przenoszenie 10 jednostek na sekundę, po dwóch sekund 20 jednostek na drugi, i tak dalej.

Przyspieszenie w dwóch wymiarów wymaga składnika X i Y, może być przypisana w następujący sposób:


```csharp
// No horizontal acceleration:
icicle.AccelerationX = 0;
// Simulate gravity with Y acceleration. Negative Y is down, so assign a negative value:
icicle.AccelerationY = -50; 
```


## <a name="acceleration-vs-deceleration"></a>Akceleracja vs. Opóźnienia

Mimo że przyspieszenia i opóźnienia są czasami zróżnicowana w każdym dniu mowy, nie ma różnic technicznych, między nimi. Grawitacji jest force, co prowadzi do przyspieszania. Jeśli obiekt jest generowany w górę następnie grawitacji spowolni go (decelerating), ale po obiektu została zatrzymana, typu i jest objęte w tym samym kierunku co grawitacji następnie grawitacji jest przyspieszenie go (przyspieszania). Jak pokazano poniżej, aplikacja przyspieszenie jest taki sam, czy jest stosowany w tym samym kierunku lub odwrotnie kierunek ruchu. 


## <a name="implementing-acceleration"></a>Implementowanie przyspieszenia

Przyspieszenie przypomina prędkość podczas implementowania — nie jest automatycznie implementowana przez CocosSharp i przyspieszenie na podstawie czasu jest żądanej implementacji (w przeciwieństwie do przyspieszania na podstawie ramki). W związku z tym wykonania prostego acceleration (wraz z prędkość) może wyglądać tak:

```csharp
this.VelocityX += this.AccelerationX * seconds;
this.VelocityY += this.AccelerationY * seconds;
this.PositionX += this.VelocityX * seconds;
this.PositionY += this.VelocityY * seconds;
```

Powyższy kod jest, co jest nazywane *liniowej zbliżenia* acceleration implementację. Ostatecznie implementuje przyspieszenie dość Zamknij stopień dokładności, ale nie jest modelem doskonale dokładne przyspieszenia. Wymienione powyżej w celu lepszego objaśnienia pojęcie implementowania przyspieszenia.

Następująca implementacja jest aplikacją ze sobą matematycznie dokładne przyspieszenia i prędkości:


```csharp
float halfSecondsSquared = (seconds * seconds) / 2.0f;

this.PositionX += 
    this.Velocity.X * seconds + this.AccelerationX * halfSecondsSquared;
this.PositionY += 
    this.Velocity.Y * seconds + this.AccelerationY * halfSecondsSquared;

this.VelocityX += this.AccelerationX * seconds;
this.VelocityY += this.AccelerationY * seconds; 
```

Najbardziej oczywisty różnica w kodzie powyżej jest `halfSecondsSquared` zmienną i jej użycia do przyspieszania pozycja do zastosowania. Matematyczne przyczynę wykracza poza zakres tego samouczka, ale zainteresowana matematyczne za to deweloperzy mogą znaleźć dodatkowe informacje w [rozważania dotyczące integrowania przyspieszenia.](http://www.cliffsnotes.com/math/calculus/calculus/integration/distance-velocity-and-acceleration)

Praktyczne wpływ `halfSecondSquare` jest, że przyspieszenie będą zachowywać się ze sobą matematycznie dokładnie i przewidywalnego niezależnie od szybkość klatek. Liniowy zbliżenia przyspieszenia podlega szybkość klatek — im niższy szybkości klatek spadnie staje się zbliżenia mniej dokładne. Przy użyciu `halfSecondsSquared` gwarantuje, że kod będą zachowywać się taka sama niezależnie od szybkość klatek.


# <a name="angles-and-rotation"></a>Kąty i obrotu

Visual obiekty, takie jak `CCSprite` obsługuje obracania za pośrednictwem `Rotation` zmiennej. Można ustawić jej obrotu w stopniach to można przypisać do wartości. Na przykład poniższy kod przedstawia sposób Obróć `CCSprite` wystąpienie:


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

![](math-images/image1.png "Powoduje to tego zrzutu ekranu")

Zwróć uwagę, że obrót jest 45 stopni w prawo (które przyczyn historycznych jest przeciwieństwem sposób ze sobą matematycznie stosowania obrotu):

![](math-images/image2.png "Zauważ, że obrót wynosi 45 stopni w prawo, który ze względu na historycznych jest przeciwieństwem sposób ze sobą matematycznie stosowania obrotu")

Ogólnie rzecz biorąc obrotu CocosSharp i regularnego matematyce można wizualizowane w następujący sposób:

![](math-images/image3.png "Ogólnie rzecz biorąc obrotu CocosSharp i regularnego matematyce można wizualizowane następująco")

![](math-images/image4.png "Ogólnie rzecz biorąc obrotu CocosSharp i regularnego matematyce można wizualizowane następująco")

Różnica w tym ważne jest, ponieważ `System.Math` klasy używa obrotu zegara, więc konieczne jest odwrócenie kąty podczas pracy z deweloperzy zapoznać się z tą klasą `CCNode` wystąpień.

Firma Microsoft zauważyć, że powyższe diagramy wyświetlenia obrotu w stopniach; Jednak niektóre funkcje matematyczne (takich jak funkcje w `System.Math` przestrzeni nazw) oczekuje i zwracają wartości w *radianach* zamiast stopni. Wyjaśniono, jak przekonwertować między typami jednostek dwóch nieco później w tym przewodniku.


## <a name="rotating-to-face-a-direction"></a>Obracanie na rzecz kierunku

Jak pokazano powyżej, `CCSprite` można obracać przy użyciu `Rotation` właściwości. `Rotation` Właściwość jest udostępniana przez `CCNode` (klasa podstawowa dla `CCSprite`), co oznacza, że obrotu może odnosić się do jednostki, które dziedziczą z `CCNode` również. 

Niektóre gry wymagają obiektów można obracać tak muszą ponosić obiektu docelowego. Przykładami mogą być kontrolowane przez komputer wroga, premia docelowego obiektu pełniącego lub statku miejsce pod kierunku punktu, w którym użytkownik jest dotknięcie ekranu. Jednak wartość obrotu musi najpierw zostać obliczane na podstawie lokalizacji jednostki za i lokalizacji obiektu docelowego na rzecz.

Ten proces wymaga kilku kroków:

 - Identyfikowanie *przesunięcie* obiektu docelowego. Przesunięcie odwołuje się do X i Y odległość między obracania jednostki i obiekt docelowy.
 - Obliczanie kąt przesunięcie za pomocą funkcji trygonometryczne tangens (co omówiono szczegółowo poniżej).
 - Dostosowanie różnicę między 0 stopni w prawo i wskazujący obracania jednostki, podczas odinstalowywania obrócony kierunek wskazuje.
 - Korygowanie różnicę między obracanie matematyczne (zegara) i obrotu CocosSharp (prawo).

Następująca funkcja (założono, że ma zostać zapisany w jednostce) obraca jednostki na rzecz elementu docelowego:


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

Powyższy kod może posłużyć do obracania jednostki, więc zwróconym punktu, w którym użytkownik jest dotknięcie ekranu w następujący sposób:


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

Ten kod powoduje następujące działania:

![](math-images/image5.gif "Ten kod skutkuje to zachowanie")

### <a name="using-atan2-to-convert-offsets-to-angles"></a>Aby przekonwertować przesunięcia kąty przy użyciu Atan2
`System.Math.Atan2` może służyć do przekonwertowania na kąt przesunięcia. Nazwa funkcji `Atan2` pochodzi z tangens trygonometryczne funkcji. Ta funkcja pozwala odróżnić sufiksem "2" zgodne ze standardem `Atan` funkcji, która jest ściśle zgodna z tangens matematyczne zachowaniem. Arcus tangens to funkcja, która zwraca wartość z zakresu od -90 i + 90 stopni (lub odpowiednik w radianach). Wiele aplikacji, w tym gry komputera często wymagają pełnego 360 stopni wartości, więc `Math` klasa zawiera `Atan2` by spełnić te wymagania.

Zwróć uwagę, że powyższy kod przekazuje parametr Y najpierw, następnie parametru X podczas wywoływania metody `Atan2` metody. Jest to wstecz z zwykle X, Y kolejność współrzędne. Aby uzyskać więcej informacji [zobacz dokumentacja Atan2](https://msdn.microsoft.com/en-us/library/system.math.atan2(v=vs.110).aspx).

Warto również zauważyć, że zwracany wartość z `Atan2` jest podany w radianach, czyli innej jednostki używanych do pomiarów kątów. Ten przewodnik nie obejmuje szczegóły radiany, ale należy pamiętać, że wszystkie funkcje trygonometryczne w `System.Math` przestrzeni nazw użyj radiany, więc wartości muszą zostać skonwertowane do stopni przed ich użyciem w obiektach CocosSharp. Można znaleźć więcej informacji na temat radianach [w radianach strony Wikipedia](http://en.wikipedia.org/wiki/Radian).

### <a name="forward-angle"></a>Kąt do przodu
Raz `FacePoint` metoda Konwertuje kąt na radiany, definiuje `forwardAngle` wartość. Ta wartość przedstawia kąt, w którym jednostki jest skierowany, gdy jego wartość obrotu jest równa 0. W tym przykładzie przyjęto założenie, że jednostka jest skierowane w górę, czyli 90 stopni, używając matematyczne obrotu (w przeciwieństwie do obrotu CocosSharp). Używamy matematyczne obrót tutaj ponieważ firma Microsoft nie zostały jeszcze odwrócony obrotu dla CocosSharp.

Poniżej pokazano, jakie jednostki z `forwardAngle` o 90 stopni może wyglądać tak:

![](math-images/image6.png "Oznacza to, co może wyglądać jednostki z forwardAngle 90 stopni.")


## <a name="angled-velocity"></a>Pod kątem szybkość pracy

Do tej pory zostały Analizujemy sposób konwertowania przesunięcia pod kątem. W tej sekcji przechodzi w inny sposób — przyjmuje kąt i konwertuje ją na X i wartości Y. Typowe przykłady samochodu przenoszenie w kierunku, w którym jest on skierowany lub statku miejsca premia punktor, który przenosi kierunku, w którym jest ukierunkowane statku. 

Koncepcyjnie prędkość może zostać obliczona pierwszy Definiowanie żądaną prędkość podczas odinstalowywania obrócony, a następnie Obracanie tego prędkość kąt, którego podmiot jest skierowany. Aby wyjaśniają koncepcji, prędkość (i przyspieszania) można wizualizowane jako 2-wymiarową *wektor* (która jest zazwyczaj rysowana w formie strzałki). Wektor prędkość wartości x = 100 i Y = 0 może zostać zwizualizowany w następujący sposób:

![](math-images/image7.png "Wektor prędkość wartości x = 100 i Y = 0 może zostać zwizualizowany w następujący sposób")

Wektor ten można obracać spowodować prędkość nowe. Na przykład obracanie wektor o 45 stopni (przy użyciu wskazówek obrót w prawo) wyniki będą następujące:

![](math-images/image8.png "Obracanie wektor o 45 stopni przy użyciu wyników przeciwnie obrót w prawo, w tym")

Na szczęście prędkość przyspieszenie i nawet pozycji wszystkich można obracać o tym samym kodzie, niezależnie od tego, jak są stosowane wartości. Następująca funkcja ogólnego przeznaczenia można obracać wektora przez wartość obrotu CocosSharp:


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

Pełnego zrozumienia `RotateVector` metoda wymaga trwa poznać funkcje trygonometryczne cosinus i sinus wraz z niektórych algebraiczną skali liniowej, która wykracza poza zakres tego artykułu. Jednak po zaimplementowana `RotateVector` metody można użyć do obracania każdego wektora, w tym wektora prędkości. Na przykład następujący kod mogą wyzwalać punktor w kierunku określony przez `rotation` wartość:


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

Ten kod może powodować wyglądać mniej więcej tak:

![](math-images/image9.png "Ten kod może powodować ekran podobny do tego zrzutu ekranu")


# <a name="summary"></a>Podsumowanie

W tym przewodniku przedstawiono typowe matematyczne w opracowywaniu gier 2W. Pokazuje, jak przypisać i wdrożenie szybkość pracy i przyspieszenie i opisano sposób Obróć obiektów i wektorów do przenoszenia zawartości w dowolnym kierunku.