---
title: Buforowanie tekstury za pomocą CCTextureCache
description: Klasa CCTextureCache w CocosSharp udostępnia standardowy sposób organizowania pamięci podręcznej i zwolnić zawartości. Jest to szczególnie przydatne w przypadku dużych gry, które mogą nie mieści się całkowicie w pamięci RAM, w celu uproszczenia procesu grupowania i usuwanie tekstury.
ms.prod: xamarin
ms.assetid: 1B5F3F85-9E68-42A7-B516-E90E54BA7102
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: c217d8a935ae971aab472b05968c0251366362b2
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783691"
---
# <a name="texture-caching-using-cctexturecache"></a>Buforowanie tekstury za pomocą CCTextureCache

_Klasa CCTextureCache w CocosSharp udostępnia standardowy sposób organizowania pamięci podręcznej i zwolnić zawartości. Jest to szczególnie przydatne w przypadku dużych gry, które mogą nie mieści się całkowicie w pamięci RAM, w celu uproszczenia procesu grupowania i usuwanie tekstury._

`CCTextureCache` Klasa jest integralną część CocosSharp tworzenia gier. Większość CocosSharp gier użyj `CCTextureCache` obiektu, nawet jeśli nie jawnie używany wewnętrznie jako wiele metod CocosSharp *udostępnionego* texture pamięci podręcznej.

W tym przewodniku dotyczą `CCTextureCache` i dlatego jest ważne w przypadku tworzenia gier. W szczególności obejmuje:

 - Dlaczego texture sprawach buforowania
 - Cykl życia tekstury
 - Using SharedTextureCache
 - Powolne ładowanie a wstępne ładowanie z AddImage
 - Usuwanie tekstury


## <a name="why-texture-caching-matters"></a>Dlaczego texture sprawach buforowania

Buforowanie tekstury jest ważną kwestią w opracowywaniu gier, ponieważ podczas ładowania tekstury jest czasochłonna operacja i tekstury wymaga znaczną ilość pamięci RAM w czasie wykonywania.

Podobnie jak w przypadku żadnej operacji pliku ładowania tekstury z dysku może być kosztowna operacja. Podczas ładowania tekstury może zająć dodatkowy czas, jeśli plik ładowany wymaga przetwarzania, takich jak trwa zdekompresować (tak jak w przypadku obrazów png i jpg). Buforowanie tekstury pozwala zmniejszyć liczbę razy, aplikacja musi załadować plików z dysku.

Jak wspomniano powyżej, tekstury zajmować dużą ilość pamięci w czasie wykonywania. Na przykład obraz tła o rozmiarze do rozdzielczości iPhone 6 (1344 x 750) zajmują 4 MB pamięci RAM — nawet jeśli plik PNG jest tylko kilku kilobajtów. Buforowanie tekstury udostępnia sposób współużytkowania tekstury odwołań w aplikacji, a także łatwe zwolnienie całej zawartości podczas przejścia między Stanami gier różne.


## <a name="texture-lifespan"></a>Cykl życia tekstury

Tekstury CocosSharp może być przechowywany w pamięci przez cały czas wykonywania aplikacji lub może być krótko. Aby zminimalizować pamięci użycia aplikacji należy dysponować tekstury, gdy nie są już potrzebne. Oczywiście oznacza to, że tekstury może być usunięty i ponownie załadowany w późniejszym czasie, które mogą zwiększyć czas ładowania lub obniżyć wydajność podczas obciążenia. 

Często podczas ładowania tekstury wymaga zależności między czas użytkowania i obciążenia pamięci / wydajność środowiska uruchomieniowego. Gry, które korzystają z małej ilości pamięci tekstury można zachować wszystkie tekstury w pamięci, zgodnie z potrzebami, ale większych gry może być konieczne zwolnienie tekstury, aby zwolnić miejsce.

Na poniższym diagramie przedstawiono prosty grę, który ładuje tekstury, zgodnie z potrzebami i przechowuje je w pamięci przez cały czas wykonywania:

![](texture-cache-images/image1.png "Ten diagram przedstawia proste grę, który ładuje tekstury, zgodnie z potrzebami i przechowuje je w pamięci przez cały czas wykonywania")

Pierwsze dwa paski reprezentują tekstury, które są potrzebne natychmiast po gry wykonywania. Następujące trzy paski reprezentują tekstury dla każdego poziomu załadować zgodnie z potrzebami.

Jeśli długą gry wystarczająco go po pewnym czasie będzie załadować za mało tekstury, aby wypełnić wszystkie pamięci RAM zapewniane przez urządzenia i systemu operacyjnego. Aby rozwiązać ten problem, gier może zwolnić danych tekstury nie jest już potrzebne. Na przykład na poniższym diagramie przedstawiono gry, w którym zwalnia Level1Texture, gdy go nie są już potrzebne, a następnie ładuje Level2Texture następnego poziomu. W rezultacie jest, że tylko trzy tekstury są przechowywane w pamięci w danym momencie: 

![](texture-cache-images/image2.png "W rezultacie jest, że tylko trzy tekstury są przechowywane w pamięci w danym momencie")


Diagram pokazanym powyżej wskazuje przez zwalnianie można zmniejszyć użycie pamięci tekstury, że może to wymagać czasy ładowania dodatkowe, jeśli zdecyduje się on na poziomie powtarzania. Warto również zauważyć, że tekstury UITexture i MainCharacter są załadowane, a nigdy nie został zwolniony. Oznacza to, że te tekstury nie są potrzebne na wszystkich poziomach, dlatego zawsze są przechowywane w pamięci. 


## <a name="using-sharedtexturecache"></a>Using SharedTextureCache

Podczas ładowania ich za pośrednictwem CocosSharp automatycznie buforuje tekstury `CCSprite` konstruktora. Na przykład poniższy kod tworzy tylko jedno wystąpienie tekstury.


```csharp
for (int i = 0; i < 100; i++)
{
    CCSprite starSprite = new CCSprite ("star.png");
    starSprite.PositionX = i * 32;
    this.AddChild (starSprite);
} 
```

Automatycznie buforuje CocosSharp `star.png` tekstury, aby uniknąć kosztownych zamiast tworzenia wiele identycznych `CCTexture2D` wystąpień. Jest to osiągane przez `AddImage` wywoływana w udostępnionej `CCTextureCache` wystąpienia, w szczególności `CCTextureCache.SharedTextureCache.Shared`. Aby zrozumieć sposób `SharedTextureCache` służy firma Microsoft można przyjrzeć się następującym kodem, który jest funkcjonalnie identyczne z wywołaniem `CCSprite` konstruktora z parametru ciągu:


```

CCSprite starSprite = new CCSprite ();
 starSprite.Texture = CCTextureCache.SharedTextureCache.AddImage ("star.png");
```

`AddImage` sprawdza, czy plik argumentu (w tym przypadku `star.png`) został już załadowany. Jeśli tak, jest zwracana buforowane wystąpienie. Jeśli następnie nie został załadowany z systemu plików, a odwołania do tekstury są przechowywane wewnętrznie przez kolejne `AddImage` wywołania. Innymi słowy `star.png` ładowany jest obraz tylko raz i kolejnych wywołań wymagają nie dostęp do dodatkowych dysku lub pamięć dodatkowe tekstury.


## <a name="lazy-loading-vs-pre-loading-with-addimage"></a>Powolne ładowanie a wstępne ładowanie z AddImage

`AddImage` Umożliwia kodu w celu zapisania taka sama czy żądanej tekstury jest już załadowany, czy nie. Oznacza to, że zawartość nie zostanie załadowany, dopóki nie jest wymagana; Jednak to może spowodować problemy z wydajnością w czasie wykonywania, ze względu na zawartość nieprzewidywalne ładowania.

Na przykład rozważmy gry, w którym można uaktualnić broń odtwarzacza. Po uaktualnieniu broni oraz pociski widoczny zmieni się, co nowego tekstury używany. Jeśli zawartość jest powolne załadować następnie tekstury skojarzony z uaktualnionych broni nie zostanie załadowany początkowo, ale raczej w późniejszym czasie, gdy odtwarzacz wymaga uaktualnienia. 

Podczas ładowania tego środek gry może spowodować gry do *pop*, czyli krótki, ale zauważalne blokowanie wykonywania. Aby tego uniknąć, kod można przewidzieć tekstury, które mogą być potrzebne się na początku oraz wstępnie załadować je. Na przykład następujące może służyć do wstępnego ładowania tekstury:


```csharp
void PreLoadImages()
{
    var cache = CCTextureCache.SharedTextureCache;

    cache.AddImage ("powerup1.png");
    cache.AddImage ("powerup2.png");
    cache.AddImage ("powerup3.png");

    cache.AddImage ("enemy1.png");
    cache.AddImage ("enemy2.png");
    cache.AddImage ("enemy3.png");

    // pre-load any additional content here to 
    // prevent pops at runtime
} 
```

Ta wstępnego ładowania może spowodować nieużywanego pamięci i może spowodować wydłużenie czasu uruchamiania. Na przykład odtwarzacz nigdy może uzyskać reprezentowany przez włączania zasilania `powerup3.png` tekstury, więc zostanie niepotrzebnie załadowana. Oczywiście może to być konieczne koszt płatności uniknąć potencjalnych pop w gry, więc jest zazwyczaj najlepiej wstępnego ładowania zawartości, jeśli zmieści się w pamięci RAM.


## <a name="disposing-textures"></a>Usuwanie tekstury

Jeśli gry nie wymaga więcej pamięci tekstury niż dostępna w minimalnej specyfikacji urządzenia, a następnie tekstury nie muszą zostać usunięte. Z drugiej strony większe gry może być konieczne zwolnić pamięć tekstury, aby zwolnić miejsce na nową zawartość. Na przykład gry może używać dużej ilości pamięci przechowywania tekstury dla środowiska. Jeśli środowisko jest używane wyłącznie w określony poziom następnie powinno być zwolniony podczas kończenia poziomu.


### <a name="disposing-a-single-texture"></a>Usuwanie jednego tekstury

Usunięcie jednego tekstury najpierw wymaga wywołania `Dispose` metody, a następnie ręczne usunięcie z `CCTextureCache`.

Poniżej przedstawiono sposób całkowicie usunąć sprite tła wraz z jego tekstury:


```csharp
void DisposeBackground()
{
    // Assuming this is called from a CCLayer:
    this.RemoveChild (backgroundSprite);

    CCTextureCache.SharedTextureCache.RemoveTexture (backgroundsprite.Texture);

    backgroundSprite.Texture.Dispose ();
} 
```

Może być skuteczne bezpośrednio disposing tekstury, gdy postępowania z mniejszą liczbą tekstury, ale to może stać się podatne na błędy podczas pracy nad większych zestawów tekstury.

Tekstury można grupować w niestandardowych (z systemem innym niż udostępniana) `CCTextureCache` instancje, aby uprościć oczyszczania tekstury.

Rozważmy na przykład na przykład w przypadku, gdy zawartość jest wstępnie ładowane przy użyciu określonego poziomu `CCTextureCache` wystąpienia. `CCTextureCache` Wystąpienie może być zdefiniowana w klasie Definiowanie poziomu (które mogą być `CCLayer` lub `CCScene`):


```csharp
CCTextureCache levelTextures; 
```

`levelTextures` Wystąpienie może być następnie używane do wstępnego ładowania tekstury specyficzne dla poziomu: 


```

void PreloadLevelTextures(CCApplication application)
{
    levelTextures = new CCTextureCache (application);

    levelTextures.AddImage ("Background.png");
    levelTextures.AddImage ("Foreground.png");
    levelTextures.AddImage ("Enemy1.png");
    levelTextures.AddImage ("Enemy2.png");
    levelTextures.AddImage ("Enemy3.png");

    levelTextures.AddImage ("Powerups.png");
    levelTextures.AddImage ("Particles.png");
} 
```

Na koniec zakończona poziom tekstury może być usunięte wszystkie jednocześnie za pośrednictwem `CCTextureCache`:


```csharp
void EndLevel()
{
    levelTextures.Dispose ();
    // Perform any other end-level cleanup
} 
```

Metodę Dispose zlikwiduje wszystkie wewnętrzny tekstury, czyszcząc limit pamięci używane przez te tekstury. Łączenie `CCTextureCache.Shared` z poziomu lub gier trybu określonym `CCTextureCache` powoduje wystąpienie niektórych tekstury utrwalanie za pośrednictwem całego gry i niektóre zwalnianie jak zakończyć poziomy, podobnie jak diagram przedstawionych na początku tego przewodnika: 

![](texture-cache-images/image2.png "Łączenie CCTextureCache.Shared z poziomu lub gier trybu CCTextureCache wystąpienia wyników w niektórych tekstury utrwalanie za pośrednictwem całego gry i niektóre zwalnianie jak zakończyć poziomy, podobnie jak diagram przedstawionych na początku tego przewodnika")




## <a name="summary"></a>Podsumowanie

Ten przewodnik przedstawia sposób użycia `CCTextureCache` klasy saldo wydajności użycia i obsługi pamięci. `CCTexturCache.SharedTextureCache` można jawnie lub niejawnie używany do ładowania i pamięci podręcznej tekstury przez cały okres istnienia aplikacji, podczas gdy `CCTextureCache` wystąpień może służyć do zwolnienia tekstury, aby zmniejszyć użycie pamięci.

## <a name="related-links"></a>Linki pokrewne

- [https://github.com/mono/CocosSharp](https://github.com/mono/CocosSharp)
- [/api/type/CocosSharp.CCTextureCache/](https://developer.xamarin.com/api/type/CocosSharp.CCTextureCache/)
