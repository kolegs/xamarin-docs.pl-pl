---
title: Praca z systemu tvOS, ikony i obrazów w programie Xamarin
description: Ten dokument zawiera opis sposobu pracy z obrazów w systemu tvOS aplikacji skompilowanej za pomocą platformy Xamarin i ikon. Omówiono jej uruchamiania obrazów, obrazy z warstwami, ikona aplikacji i.
ms.prod: xamarin
ms.assetid: A2DA4347-0563-4C72-A8D7-5B9DE9E28712
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 59cbc53acf3ab7da12826b9d3cffb821631a0500
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34788800"
---
# <a name="working-with-tvos-icons-and-images-in-xamarin"></a>Praca z systemu tvOS, ikony i obrazów w programie Xamarin

Tworzenie urzekające ikon i obrazów jest krytyczną częścią zdobywania doświadczenia użytkownika bez ramek do pracy z aplikacjami firmy Apple TV. W tym przewodniku opisano kroki wymagane do tworzenia i niezbędnych zasobów graficznych dla aplikacji Xamarin.tvOS obejmują:

- [Uruchamianie obrazu](#Launch-Image) -obrazu uruchamiania wyświetlany, gdy aplikacja jest uruchomione i zastępuje pierwszy ekran aplikacji, po zakończeniu uruchamiania.
- [Obrazy z warstwami](#Layered-Images) — specyficzne dla Apple TV, firmy Apple nowych obrazów warstwie korzystają z efekt paralaksy w celu utworzenia efekt 3W dla wybranych elementów. Istnieje kilka sposobów [tworzenie obrazów warstwie](#Creating-Layered-Images).
- [Ikona aplikacji](#App-Icons) -ikony są wymagane dla ekranu nie tylko Apple TV Narzędzia główne, ale dla sklepu z aplikacjami. Podaje jako obraz warstwie.
- [Z góry półki obrazu](#Top-Shelf-Image) — Jeśli aplikacja znajduje się w wierszu góry ekranu głównej, konieczne będzie obraz górnej półki przedstawiające funkcje aplikacji. Opcjonalnie możesz podać [dynamicznej górnej półki zawartości](#Dynamic-Top-Shelf-Content) aby wyróżnić zawartości w aplikacji.
- [Obrazy Centrum gier](#Game-Center-Images) — Jeśli aplikacja jest gier i korzysta z programu Game Center, kilka dodatkowych obrazów będzie wymagane.
- [Ustawianie obrazów projektu Xamarin.tvOS](#Setting-Xamarin.tvOS-Project-Images) -omówiono czynności, jakie można ustawić dla aplikacji Xamarin.tvOS ikona aplikacji i uruchamianie obrazów.

> [!IMPORTANT]
> Wszystkie obrazy w Apple TV są rozdzielczością 1 x (`@1x`) i należy _tylko_ obrazy tej wielkości. W tym większy, wyższej rozdzielczości grafiki nie tylko trwać pobranie i użycie więcej pamięci oraz Magazyn, ale muszą być dynamicznie przeskalowywany w ten sposób w czasie wykonywania i negatywnie wpłynąć na wydajność rysowania.

<a name="Launch-Image" />

## <a name="launch-image"></a>Uruchamianie obrazu

Uruchamianie obraz jest najpierw wyświetlany, gdy aplikacja Xamarin.tvOS początkowo jest uruchomiona na Apple TV, a, wszystkie aplikacje systemu tvOS podać do uruchomienia obrazu. 

Uruchamianie obrazu pojawi się szybko i daje wrażenie, że aplikacja jest szybkie i elastyczny. Apple TV spowoduje zamianę uruchamianie obrazu na pierwszym ekranie aplikacji wkrótce po.

Obrazy uruchomienia nie są możliwość reklam lub wyrażenie artystyczny, istnieją one tylko do dają pogląd, że aplikacja uruchamia szybko i jest gotowy do użycia.

|Rozmiar obrazu uruchamiania|Uwagi|
|---|---|
|1920x1080px|Tylko pliki PNG warstwie inne niż|

Apple sprawia, że poniższe sugestie dotyczące projektowania obrazu uruchamianie aplikacji:

- **Niemal identyczny z pierwszym ekranie** -Your ekranu uruchamiania powinny być maksymalnie zbliżony pierwszy ekran aplikacji, jak to możliwe. Łącznie z różnych grafiki lub elementu może spowodować irytujących "flash", gdy zostanie wyświetlony pierwszy ekran.
- **Unikaj przy użyciu tekstu** — uruchamianie obrazów są statyczne i jako taki nie jest lokalizowany przed wyświetleniem.
- **Downplay uruchamiania** — ponieważ Apple TV, użytkownicy często przełączać aplikacji, nie należy zwrócić uwagę na proces uruchamiania aplikacji.
- **Brak reklam lub Branding** -Your obraz Uruchom nie powinna być używana jako ekran informacje lub zawierać żadnych znakowania, chyba że jest statycznej części aplikacji na pierwszym ekranie. Usługa AD są zabronione.

<a name="Setting-the-Launch-Image" />

### <a name="setting-the-launch-image"></a>Ustawienia uruchamiania obrazu

Aby ustawić uruchamianie obrazu dla projektu systemu tvOS, wykonaj następujące czynności:

1. W **Eksploratora rozwiązań**, kliknij dwukrotnie `Assets.xcassets` go otworzyć do edycji: 

    [![](icons-images-images/asset01.png "Plik Assets.xcassets")](icons-images-images/asset01.png#lightbox)
2. W **edytora zasobów**, kliknij `LaunchImages` zasobów: 

    [![](icons-images-images/asset02.png "LaunchImages zasobów")](icons-images-images/asset02.png#lightbox)
3. Polecenie **1 x Apple TV** wejścia i wybierz obraz Uruchom lub opcjonalnie przeciągnij nowy obraz z systemu plików: 

    [![](icons-images-images/asset03.png "Wybierz obraz uruchamiania")](icons-images-images/asset03.png#lightbox)
4. Zapisz zmiany.

<a name="Layered-Images" />

## <a name="layered-images"></a>Obrazy z warstwami

Jesteś nowym użytkownikiem Apple TV, obrazy warstwie korzystają z efekt paralaksy w celu utworzenia efekt 3W, który pomaga chronić użytkownika na kanapie umysłowo podłączone do zawartości ekranu w pomieszczeniu.

Obrazy z warstwami zawierają z dwóch (2) do pięciu (5) osobne warstwy, które są łączone na formularzu pełny obraz. Z wyjątkiem warstwę tła każdej warstwy używa jej porządek wraz z przezroczystość do utworzenia iluzji głębi. Gdy użytkownik wchodzi w interakcję z obrazem z warstwami, wyższych warstw uporządkowane Z są skalowane i nakładają się na ten efekt.

[![](icons-images-images/layered01.png "Uporządkowane obrazów Z diagramu warstwowego")](icons-images-images/layered01.png#lightbox)

> [!IMPORTANT]
> Obrazy z warstwami są wymagane dla ikony aplikacji i są opcjonalne dla innych [Focusable elementów](~/ios/tvos/app-fundamentals/navigation-focus.md#Focus-and-Selection) (takich jak obraz górnej półki). Jednak Apple sugeruje, przy użyciu obrazów warstwie dla żadnego obrazu, który może uzyskać fokusu w aplikacji.




Apple sprawia, że poniższe sugestie dotyczące projektowania warstwie obrazów:

- **Wprowadź tło nieprzezroczyste warstwy** -warstwą tła (Warstwa 1) **musi** być nieprzezroczyste lub zostanie wyświetlony błąd podczas próby użycia warstwie obrazu na Apple TV. Wszystkie inne warstwy może zawierać wiele poziomów przezroczystości w celu zwiększenia efekt 3W.
- **Izolowanie pierwszego planu, drugie i elementy tła** -umieść widocznych elementów (na przykład gier znaków) na pierwszym planie, użyj środka dla elementów dodatkowej lub cieni. Na koniec zawierać neutralne tło zapewnienie etap górny warstw.
- **Zachowaj tekst na pierwszym planie** -chyba że chcesz, aby Twoje tekst, który ma być zasłonięty przez wyższego poziomu, zazwyczaj należy go w najwyższej warstwie.
- **Użyj prostego Układanie warstwowo** -efekt paralaksy został opracowany jako niewielkie, więc Zachowaj warstw do minimalnego zapobiegające jarring, wypadku skutków.
- **Obejmują strefy bezpieczeństwa** — ponieważ górnej warstwy można przycięte podczas efekt paralaksy, należy utworzyć obramowanie strefy bezpieczny do każdej warstwy. Jeśli zawartość za blisko krawędzi warstwy, mogą stać się przycięty poza. Może wystąpić górnej warstwy więcej skalowanie i przycinanie niż niższych warstwach. Zobacz [zmiany rozmiaru obrazu warstwy](#Sizing-Image-Layers) poniższej sekcji.
- **Podgląd często** -warstwowy obrazy powinien można wyświetlić podglądu często, aby upewnić się, że żądany efekt 3W występuje i czy jest brak zasobów w poszczególnych warstwach jest obcinane. Możesz wyświetlić podgląd obrazów warstwowego na rzeczywistą Apple TV sprzętu do upewnij się, że renderują zgodnie z oczekiwaniami.

Jeśli to możliwe, należy zawsze używać wbudowanych `UIKit` formantów na wyświetlanie obrazów z warstwami, jak będą automatycznie otrzymują efekt paralaksy po znalezieniu w fokus.

<a name="Sizing-Image-Layers" />

### <a name="sizing-image-layers"></a>Zmiana rozmiaru obrazu warstwy

Należy pamiętać o uwzględnieniu _strefy bezpieczeństwa_ obramowanie do każdej warstwy, która zostanie utworzenie obrazu warstwie. Ponieważ można skalować i przycięte podczas efekt paralaksy w poszczególnych warstwach, zawartość warstwy można przycięte poza, jeśli jest zbyt blisko krawędzi warstwy:

[![](icons-images-images/layered02.png "Obramowanie 35 pikseli")](icons-images-images/layered02.png#lightbox)

<a name="Creating-Layered-Images" />

### <a name="creating-layered-images"></a>Tworzenie warstwie obrazów

systemu tvOS współpracuje z warstwami obrazów w następujących formatach:

- **Pliki SAMOCHODU** -to jest wewnętrznym formacie katalogu zasobów utworzone przez firmę Apple. Nie twórz plików SAMOCHODU bezpośrednio, są tworzone w czasie kompilacji z żadnych plików LSR i zawartych w pakiecie Twojej aplikacji.
- **Obrazy LSR** — jest to format obrazu zastrzeżonych utworzone przez firmę Apple. Użyj [paralaksy eksportera Adobe Photoshop wtyczki](https://itunespartner.apple.com/assets/downloads/ParallaxExporter_Apps.zip) lub [Podgląd paralaksy](http://itunespartner.apple.com/assets/downloads/Parallax%20Previewer.dmg) do tworzenia i Podgląd warstwie obrazów w formacie LSR.
- **Assets.xcassets** — z dwóch (2) do pięciu (5) standardowe `.png` sformatowany obrazów zawartych w katalogu zasobów, który zostanie skompilowany w SAMOCHODU lub LSR sformatowany warstwie obrazu w czasie kompilacji.
- **Pliki LCR** — jest to format pliku zastrzeżonych utworzone przez firmę Apple. Pliki LCR są przeznaczone do użycia jako dodatkowej zawartość pobraną z jednego z serwerów zawartości. Plik LCR nigdy nie powinien być uwzględniany w Twojego pakietu app. Pliki LCR zostaną wygenerowane na podstawie LSR lub Photoshop plików za pomocą `layerutil` narzędzia wiersza polecenia dołączone do Xcode.

<a name="The-Parallax-Previewer" />

### <a name="the-parallax-previewer"></a>Podgląd paralaksy

Apple utworzony [Podgląd paralaksy](http://itunespartner.apple.com/assets/downloads/Parallax%20Previewer.dmg) utworzony warstwie obrazów, wymagane dla ikony aplikacji i elementów opcjonalnych Focusable i w wersji zapoznawczej. Podglądzie przedstawia każdej warstwie będącej ukończone warstwie obrazu:

[![](icons-images-images/layered03.png "Podgląd paralaksy")](icons-images-images/layered03.png#lightbox)

Podczas wyświetlania podglądu obraz z warstwami, mysz służy do obracania obrazu i Podgląd efektu paralaksy. Użyj **+** (plus) i **-** (minus), aby dodawać i usuwać warstwy.

Podczas tworzenia nowego obrazu z warstwami, można wyeksportować w formacie LSR i zawartych w pakiecie aplikacji.

Aby uzyskać więcej informacji na temat tworzenia i Podgląd warstwie obrazów, zobacz firmy Apple [tworzenie kompozycji paralaksy](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/CreatingParallaxArtwork.html#//apple_ref/doc/uid/TP40015241-CH19-SW1) sekcji [Podręcznik programowania aplikacji dla systemu tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/).

<a name="App-Icons" />

## <a name="app-icons"></a>Ikony aplikacji

Aplikacji Xamarin.tvOS wymaga nie tylko ikony aplikacji dla ekranu Apple TV Narzędzia główne, ale również ikony dla sklepu z aplikacjami. Ikona aplikacji jest pierwszego Zmień aby dużą wrażenie na potencjalne użytkownika i skontaktować się cel aplikacji w jednym rzutem oka.

[![](icons-images-images/icon01.png "Ikona aplikacji")](icons-images-images/icon01.png#lightbox)

Każda aplikacja podać zarówno małych i dużych wersji ikona aplikacji. Małych ikon będzie używany na ekranie Apple TV Narzędzia główne, gdy aplikacja jest zainstalowana. Duża wersja jest używana przez sklepu z aplikacjami. Dużych ikon aplikacji, powinien naśladować wygląd i działanie wersji małych ikon.

|Małe ikony||Dużych ikon||
|---|---|---|---|
|Rzeczywisty rozmiar|400x240px|Rozmiar|1280x768px|
|Rozmiar strefy bezpieczeństwa|370x222px|||
|Rozmiaru bez fokusu|300x180px|||
|Rozmiar mającego fokus|370x222px|||

> [!IMPORTANT]
> Ikony aplikacji musi być dostarczona jako **warstwie obrazów**. Zobacz [warstwie obrazu](#Layered-Images) sekcji powyżej, aby uzyskać więcej informacji.




Apple zawiera poniższe sugestie dotyczące tworzenia ikony aplikacji:

- **Zapewnia jeden punkt fokus** — projekt jako ikony z jednego fokus umieszczone bezpośrednio w Centrum ikony.
- **Użyj prostego tła** — Zachowaj proste tła ikonę, aby wyróżnić górnej warstwy. Należy rozważyć użycie prostego kolor lub gradient niewielkie.
- **Ogranicz ilość tekstu** — ponieważ nazwa aplikacji będzie wyświetlana poniżej ikony, po wybraniu przez użytkownika, użytkownik powinien zawierać tekstu po niezbędne do projektowania ikony.
- **Nie używaj zrzuty ekranu** — zrzuty ekranu są zbyt złożone, ikony i nie Zezwalaj użytkownikowi na zobaczenie celem aplikacji jeden rzut oka.
- **Zachowaj ikony kwadratowe** — systemu tvOS automatycznie stosuje maski pisowni zaokrągla narożników ikony. Nie dołączaj tego zaokrąglania samodzielnie.
- **Oddziel dokładnie Your warstwy** — tekst powinien być w prawym górnym większości warstwy, dodatkowej elementów w środku i neutralne tła, umożliwiający górny warstw zaprezentować.
- **Użyj gradientach i dokładnie Shadows** — gradientach i cieni można mogą powodować konfliktów z efekt paralaksy, powinien być używany dokładnie. Proste góry do dołu, styl gradientu jasny dark najlepiej. Shadows zazwyczaj najlepiej jako tint sharp, hard-edged.
- **Różnią się przezroczystości** — Użyj odpowiedniego poziomu przejrzystości na wyższe poziomy ikonę Twojej aplikacji w taki sposób, aby zwiększyć efekt 3W. Warstwę tła musi być nieprzezroczyste lub spowoduje błąd.

<a name="Setting-the-App-Icons" />

### <a name="setting-the-app-icons"></a>Ustawienie ikony aplikacji

Aby ustawić ikony aplikacji wymagane dla projektu systemu tvOS, wykonaj następujące czynności:

1. W **Eksploratora rozwiązań**, kliknij dwukrotnie `Assets.xcassets` go otworzyć do edycji: 

    [![](icons-images-images/asset01.png "Assets.xcassets fileg")](icons-images-images/asset01.png#lightbox)
2. W **edytora zasobów**, rozwiń węzeł `App Icon & Top Shelf Image` zasobów: 

    [![](icons-images-images/asset04.png "Rozwiń węzeł zasobów górnej półki obrazu")](icons-images-images/asset04.png#lightbox)
3. Następnie rozwiń węzeł `App Icon - Small` zasobów: 

    [![](icons-images-images/asset05.png "Rozwiń ikonę aplikacji - małe zasobów")](icons-images-images/asset05.png#lightbox)
4. Następnie rozwiń węzeł `Back` zasobów i kliknięcie `Contents` wpis: 

    [![](icons-images-images/asset06.png "Rozwiń węzeł zasobów Wstecz")](icons-images-images/asset06.png#lightbox)
5. Polecenie **1 x wpis Apple TV** i wybierz plik obrazu.
6. Powtórz powyższe kroki dla `Front` i `Middle` zasoby.
7. Następnie powtórz te same kroki, aby zdefiniować `App Icon - Large` zasobów.
4. Zapisz zmiany.

<a name="Top-Shelf-Image" />

## <a name="top-shelf-image"></a>Obraz z górnej półki

Jeśli użytkownik ma dotyczącymi aplikacji Xamarin.tvOS górny wiersz na ekranie Apple TV Narzędzia główne, duży obraz górnej półki będą wyświetlane po wybraniu aplikacji przez użytkownika. Ten obraz powinien przedstawiające funkcje aplikacji lub podaj linki bezpośrednie do jego zawartości.

[![](icons-images-images/topshelf01.png "Górny przykład półki obrazu")](icons-images-images/topshelf01.png#lightbox)

Obraz górnej półki albo można podać jako pojedynczy statycznego `.png` lub `.lsr` pliku (zobacz [tworzenia obrazów warstwie](#Creating-Layered-Images)) lub go mogą być tworzone dynamicznie w czasie wykonywania jako pojedynczy wiersz Focusable elementów (zobacz [ Zawartość dynamiczna górnej półki](#Dynamic-Top-Shelf-Content) poniżej).

|Rozmiar obrazu górnej półki|Uwagi|
|---|---|
|1920x720px|Statyczne PNG lub pliku .lsr warstwowej|

Apple zawiera poniższe sugestie dotyczące tworzenia obrazów górnej półki:

- **Użyj obrazów statycznych sformatowanego** — Jeśli aplikacja nie zapewnia zawartość dynamiczną, jego górnej półki obrazu będzie z systemem innym niż focusable. Skorzystaj z tego obrazu, aby wyróżnić funkcji aplikacji lub oznakowanie.
- **Łącze do zawartości aplikacji** — dynamiczne górnej półki układów zapewnić szybkie łącze do zawartości, która znajdzie najważniejszych w aplikacji użytkownika. Użyj tego obszaru, aby zapewnić szybkie łącze, aby uruchomić aplikację i przejść bezpośrednio do danej zawartości.
- **Pokaz najnowsza zawartość** — sformatowanego górnej półki zawartości można narysować użytkownika w aplikacji i będą się więcej. Użyj go jako obszar prezentować najwyższy sklasyfikowane lub najnowsza zawartość.
- **Spersonalizowanych zawartości** — ich najczęściej używane miejsce użytkowników lub ulubionych aplikacji w wierszu Top ekranie głównym. Umożliwia wyświetlanie zawartości, które byłyby największe znaczenie górnej półki.
- **Niedozwolone reklam** — reklam są zabronione będą wyświetlane w górnej półki. Najnowsza wersja płatnej wersji zawartości mogą być wyświetlane, ale powinny być wyświetlane nie informacje o cenach.

### <a name="setting-the-top-shelf-image"></a>Ustawianie obrazów górnej półki

Aby ustawić obraz półki Top wymagane dla projektu systemu tvOS, wykonaj następujące czynności:

1. W **Eksploratora rozwiązań**, kliknij dwukrotnie `Assets.xcassets` go otworzyć do edycji: 

    [![](icons-images-images/asset01.png "Plik Assets.xcassets")](icons-images-images/asset01.png#lightbox)
2. W **edytora zasobów**, rozwiń węzeł `App Icon & Top Shelf Image` zasobów: 

    [![](icons-images-images/asset04.png "Rozwiń węzeł zasobów górnej półki obrazu")](icons-images-images/asset04.png#lightbox)
3. Polecenie `Top Shelf Image` zasobów: 

    [![](icons-images-images/asset07.png "Zasób górnej półki obrazu")](icons-images-images/asset07.png#lightbox)
5. Polecenie **1 x wpis Apple TV** i wybierz plik obrazu.
6. Zapisz zmiany.

<a name="Dynamic-Top-Shelf-Content" />

### <a name="dynamic-top-shelf-content"></a>Zawartość dynamiczna górnej półki

Zamiast statyczny obraz górnej półki górnej półki może zawierać dynamiczne wiersza [Focusable elementy](~/ios/tvos/app-fundamentals/navigation-focus.md#Focus-and-Selection) lub zestawu dynamicznego transparentach przewijania. Oba te style dynamiczne pozwalają Wyróżnij zawartość dostarczone przez aplikację lub skok do jego najczęściej używane funkcje.

<a name="Sectioned-Content-Row" />

#### <a name="sectioned-content-row"></a>Podzielonym zawartości wiersza

Typ zawartości to dynamiczny górnej półki przedstawia pojedynczy wiersz przewijania elementów Focusable opcjonalnie podziale na sekcje. Jego jest zwykle używana do nowych, zaznacz ulubionych lub ostatnio wyświetlane zawartość aplikacji.

Zawartość jest przedstawiany jako pojedynczej, poziomy przewijanej listy zawartości z etykietą znajdujących się w obszarze bieżący element zawartości (który aktualnie ma fokus). Jeśli użytkownik wybierze danego elementu zawartości, aplikacja zostanie uruchomiona i należy podjąć bezpośrednio do tej zawartości.

Wymagane będą następujące wymiary zawartości:

||Plakat (2:3)|(1:1) kwadratu|HDTV (16:9)|
|---|---|---|---|
|Rzeczywisty rozmiar|404x608px|608x608px|908x512px|
|Rozmiar strefy bezpieczeństwa|380x570px|570x570px|852x479px|
|Rozmiaru bez fokusu|333x500px|500x500px|782x440px|
|Rozmiar mającego fokus|380x570px|570x570px|852x479px|

Apple zawiera poniższe sugestie podzielonym zawartości wiersza:

- **Zakończenie wiersza** — należy podać wystarczającej zawartości do span pełnej szerokości ekranu.
- **Skalowanie obrazów mieszane** — podzielonym zawartości wiersza została zaprojektowana w celu przechowywania różnych rozmiary obrazów (z listy powyżej). Jeśli jednak mieszać rozmiary obrazów, należy pamiętać, że dodatkowe skalowania zostanie zastosowana do normalizacji wyświetlanie zawartości.

<a name="Scrolling-Inset-Banners" />

#### <a name="scrolling-inset-banners"></a>Przewijanie transparentach wstawki

Opcjonalnie aplikacji Xamarin.tvOS może stanowić jego zawartość w górnej półki jako automatycznie przewijanie i zapętlenia kolekcji transparentach, które prawie cały ekran. Ten styl jest zwykle używane do pokazują zawartości nowym, takie jak nowe programy telewizyjne.

Oprócz automatycznego przewijania, użytkownik może przejąć kontrolę nad transparentach i przewiń w żadnym kierunku przy użyciu zdalnego Siri. Tworzenie małych, cykliczne gestów na komputerze zdalnym Siri, gdy fokus transparent uaktywni efekt paralaksy w tym transparent.

**Obraz transparentu (bardzo szerokie)**

|   |   |
|---|---|
|Rzeczywisty rozmiar|1940x624px|
|Rozmiar strefy bezpieczeństwa|1740x620px|
|Rozmiaru bez fokusu|1740x560px|
|Rozmiar mającego fokus|1740x620px|

Przewijanie transparentach wstawki albo można podać jako statycznego `.png` lub warstwie `.lsr` pliku.

Apple zawiera poniższe sugestie dotyczące transparentach wstawki przewijanie:

- **Wielkość zawartości** -powinien zapewnić co najmniej trzech (3) transparentach do przewijania uznać fizycznych. Powinna zawierać nie więcej niż osiem (8) transparentach lub rozsądne nawigacji twardych dla użytkownika końcowego.
- **Zawartość tekstu** — Jeśli transparencie wymaga tekstu w powinien być uwzględniony w obrazie transparent. Jeśli używane są obrazy z warstwami, tekst musi należeć do najwyższej warstwy.

Zobacz firmy Apple [odwołania Framework TVServices](https://developer.apple.com/library/prerelease/tvos/documentation/TVServices/Reference/TVServices_Ref/index.html#//apple_ref/doc/uid/TP40016412) Aby uzyskać więcej informacji o dodawaniu rozszerzenie górnej półki do aplikacji w celu zapewnienia zawartości dynamicznej górnej półki.

<a name="Game-Center-Images" />

## <a name="game-center-images"></a>Obrazy Game Center

Jeśli aplikacji Xamarin.tvOS gier oraz obsługa Game Center zostały dołączone, kilka więcej zasobów obrazu będzie wymagane:

- **Ikony osiągnięcia** -przezroczystości obrazu jest wymagany dla każdego osiągnięcia, które zostaną automatycznie przycięte okręgu. Osiągnięć są elementy focusable.
- **Pulpit nawigacyjny kompozycji** — opcjonalnie obrazu można podać wyświetlanego w górnej części pulpitu nawigacyjnego aplikacji w Centrum gier. Te obrazy są z systemem innym niż focusable.
- **Kompozycja Liderzy** -należy podać między jednego (1) do trzech (3) obrazów współczynnik proporcji 16:9 dla każdego Liderzy, która obsługuje aplikację. Może to być statycznych `.png` lub warstwie `.lsr` plików. Kompozycja Liderzy jest focusable.

||Ikony osiągnięcia|Kompozycja pulpitu nawigacyjnego|Liderzy kompozycji|
|---|---|---|---|
|Rozmiar widoczne|200x200px|923x150px|n/d|
|Rzeczywisty rozmiar|320x320px|n/d|659x371px|
|Rozmiar strefy bezpieczeństwa|n/d|n/d|618x348px|
|Rozmiaru bez fokusu|n/d|n/d|548x309px|
|Rozmiar mającego fokus|n/d|n/d|618x348px|

Aby uzyskać więcej informacji na temat pracy z Centrum gier, zobacz firmy Apple [Game Center Programming Guide](https://developer.apple.com/library/prerelease/tvos/documentation/NetworkingInternet/Conceptual/GameKit_Guide/Introduction/Introduction.html).

<a name="Working-with-Images" />

## <a name="working-with-images"></a>Praca z obrazami

Ponieważ systemu tvOS 9 jest podzbiorem systemu iOS 9, te same techniki używane do uwzględnienia, a także wyświetlanie obrazów w aplikacji platformy Xamarin.iOS również działać w przypadku aplikacji Xamarin.tvOS. Zobacz nasze [wyświetlania obrazu](~/ios/app-fundamentals/images-icons/displaying-an-image.md) dokumentacji, aby uzyskać więcej informacji.

<a name="Setting-Xamarin.tvOS-Project-Images" />

## <a name="setting-xamarintvos-project-images"></a>Ustawianie obrazów Xamarin.tvOS projektu

Jak już wspomniano, wszystkie aplikacje systemu tvOS wymagają [uruchomić obrazu](#Launch-Image), i [ikonę aplikacji](#App-Icons). W tej sekcji omówiono, wybierając ikona aplikacji i uruchamianie obrazów Xamarin.tvOS projektu aplikacji po ich ustawieniu w katalogu zasobów.

Wykonaj następujące czynności:

1. W **Eksploratora rozwiązań**, kliknij dwukrotnie `Info.plist` go otworzyć do edycji: 

    [![](icons-images-images/info01.png "Plik Info.plist")](icons-images-images/info01.png#lightbox)
2. W **Edytor Info.Plist**, wybierz opcję katalog zasobów (skonfigurowanych powyżej w [ustawienie ikon aplikacji](#Setting-the-App-Icons) sekcji) dla **ikony aplikacji**: 

    [![](icons-images-images/info02.png "Edytor Info.Plist")](icons-images-images/info02.png#lightbox)
3. Następnie wybierz opcję katalog zasobów (skonfigurowanych powyżej w [ustawienie uruchamiania obrazu](#Setting-the-Launch-Image) sekcji) dla **uruchomić obrazów**.
4. Zapisz zmiany.

<a name="Summary" />

## <a name="summary"></a>Podsumowanie

W tym artykule pokrywającego wszystkie typy obrazów i rozmiary używane w aplikacji Xamarin.tvOS. Po pierwsze objęte uruchomić obrazów, obrazy z warstwami, ikony aplikacji, górnej półki obrazów i Game Center obrazów. Następnie objętych usługą pracy z obrazami w aplikacji Xamarin.tvOS.

## <a name="related-links"></a>Linki pokrewne

- [Przykłady dla systemu tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [systemu tvOS człowieka przewodniki — interfejs](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Przewodnik programowania w języku aplikacji dla systemu tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
