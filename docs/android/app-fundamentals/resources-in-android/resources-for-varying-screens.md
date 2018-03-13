---
title: "Tworzenie zasobów dla różnych ekranów"
ms.topic: article
ms.prod: xamarin
ms.assetid: 3D17DE45-115C-7192-5685-44F8EEE07DCC
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: fcd77d97d492baee441cfd428e58ea83525f927e
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="creating-resources-for-varying-screens"></a>Tworzenie zasobów dla różnych ekranów

Android sama działa na różnych urządzeniach, każde posiada szerokiego zakresu rozwiązania, rozmiaru ekranu i gęstości ekranu. System android będzie wykonywać skalowanie i zmiany rozmiaru, aby aplikacja działa na tych urządzeniach, ale może to spowodować nieoptymalnych użytkowników. Na przykład obrazy mogą być rozmyte, obrazy mogą zajmować zbyt dużo (lub nie ma wystarczającej ilości) miejsca na ekranie co powoduje, że położenie elementów interfejsu użytkownika w układzie będzie nakładają się lub jest zbyt daleko od siebie.


## <a name="concepts"></a>Pojęcia

Kilka terminy i pojęcia są zrozumieć do obsługi wielu ekranów.

- **Rozmiar ekranu** &ndash; ilość fizycznego miejsca do wyświetlania aplikacji

- **Ekran gęstość** &ndash; liczbę pikseli w dowolnym danym obszarze na ekranie. Typowy jednostka miary jest punkty na cal (dpi).

- **Rozdzielczość** &ndash; całkowitą liczbę pikseli na ekranie. Podczas tworzenia aplikacji, rozdzielczość nie jest ważne co do rozmiaru ekranu i gęstości.

- **Niezależne od gęstości pikseli (dp)** &ndash; to wirtualny jednostki miary umożliwia niezależne od gęstości układów należy planować. Aby przekonwertować dp w pikselach używana jest następująca formuła:

    pikseli &equals; dp &times; dpi &divide; 160

- **Orientacja** &ndash; orientacji ekranu jest traktowany jako pozioma, gdy jest większa niż wysokość. Natomiast w orientacji pionowej jest, gdy ekran jest większa niż szerokość. Orientacja można zmienić okres istnienia aplikacji obracania urządzenia.

Należy zauważyć, że pierwsze trzy tych pojęć między są powiązane &ndash; zwiększenie rozdzielczość bez zwiększenie gęstości spowoduje zwiększenie rozmiaru ekranu. Jednak jeśli zwiększa się gęstości i rozpoznawania następnie rozmiaru ekranu można pozostaną niezmienione. Ta relacja między rozmiaru ekranu, gęstości i rozpoznawania skomplikować obsługę ekranu bardzo szybko.

Ułatwiające biznesowych w radzeniu sobie z tym złożoność platformy systemu Android chce używać *pikselach niezależnych od gęstości (dp)* dla układów ekranu głównego. Za pomocą pikselach niezależnych od gęstości, elementy interfejsu użytkownika będą wyświetlane użytkownikowi, aby mieć taki sam rozmiar fizycznych na ekranach o różnych gęstości.


## <a name="supporting-various-screen-sizes-and-densities"></a>Obsługa różnych rozmiarów ekranu i gęstości

Android obsługuje większość pracy do renderowania układów prawidłowo dla każdej konfiguracji ekranu. Istnieją jednak niektóre akcje, które można podjąć pomagać systemu.

Korzystanie z pikselach niezależnych od gęstości zamiast rzeczywistej pikseli układów jest wystarczające w większości przypadków w celu zapewnienia niezależność gęstości.
Android skalowały drawables w czasie wykonywania do odpowiedniego rozmiaru.
Jednak jest możliwe, że ta skalowanie spowoduje być rozmyte map bitowych. Aby tego uniknąć, może być konieczne podanie alternatywnych zasobów dla różnych gęstości. Podczas projektowania urządzeń dla wielu rozwiązania i gęstości ekranu będzie okazać się łatwiejsze do uruchomienia z wyższej rozdzielczości lub gęstość obrazy i następnie skali. Pozwoli to uniknąć rozmycia lub zakłócenia, które mogą być wynikiem zmiany rozmiaru.


### <a name="declare-the-screen-size-the-application-supports"></a>Deklarowanie aplikacja obsługuje rozmiaru ekranu

Deklarowanie rozmiaru ekranu zapewnia tylko obsługiwane urządzenia pobrać aplikację. Jest to osiągane przez ustawienie [obsługuje ekrany](http://developer.android.com/guide/topics/manifest/supports-screens-element.html) element **AndroidManifest.xml** pliku. Ten element służy do określania, jakie rozmiaru ekranu są obsługiwane przez aplikację. Danego ekranu jest uznawany za obsługiwany, jeśli aplikację można prawidłowo tego układów do rozmiaru ekranu. Korzystając z tego elementu manifestu aplikacji nie będą widoczne w [ *Google Play* ](https://play.google.com/) dla urządzeń, które nie spełnia wymagań ekranu. Jednak nadal uruchamiania aplikacji na urządzeniach za pomocą nieobsługiwanego ekrany, ale układów może być rozmyte i podzielony na piksele.

Aby to zrobić w Xamarin.Android, należy najpierw dodać **AndroidManifest.xml** plik do projektu, jeśli jeszcze nie istnieje:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Manifestu systemu android](resources-for-varying-screens-images/01-android-manifest-vs-sml.png)](resources-for-varying-screens-images/01-android-manifest-vs.png#lightbox)

**AndroidManifest.xml** jest dodawany do **właściwości** katalogu. Aby uwzględnić zostanie zmieniony plik [obsługuje ekrany](http://developer.android.com/guide/topics/manifest/supports-screens-element.html):

[![Dodawanie ekranów obsługuje](resources-for-varying-screens-images/02-adding-supports-screens-vs-sml.png)](resources-for-varying-screens-images/02-adding-supports-screens-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Manifestu systemu android](resources-for-varying-screens-images/01-android-manifest-xs-sml.png)](resources-for-varying-screens-images/01-android-manifest-xs.png#lightbox)

**AndroidManifest.xml** jest dodawany do **właściwości** katalogu. Aby uwzględnić zostanie zmieniony plik [obsługuje ekrany](http://developer.android.com/guide/topics/manifest/supports-screens-element.html):

[![Dodawanie ekranów obsługuje](resources-for-varying-screens-images/02-adding-supports-screens-xs-sml.png)](resources-for-varying-screens-images/02-adding-supports-screens-xs.png#lightbox)

-----

### <a name="provide-alternate-layouts-for-different-screen-sizes"></a>Podanie alternatywnych układów różnych rozmiarów ekranu

Mimo że Android zmieni się rozmiar według rozmiaru ekranu, może to nie być wystarczające w niektórych przypadkach. Może być pożądane, aby zwiększyć rozmiar niektórych elementów interfejsu użytkownika na większym ekranie lub zmienić położenie elementów interfejsu użytkownika dla mniejszego ekranu.

Począwszy od 13 poziom interfejsu API (Android 3.2), rozmiaru ekranu są zastąpiona przy użyciu sw*N*kwalifikator punktu dystrybucji. Ten nowy kwalifikator deklaruje się, że ilość miejsca na danym układu musi. Zdecydowanie zaleca się powinien być używany nowszej Kwalifikatory aplikacji, które są przeznaczone do uruchamiania na Android 3.2 lub nowszej.

Na przykład w razie potrzeby minimalną 700dp szerokości ekranu układ alternatywny układu mógłby się znaleźć w folderze **sw700dp układu**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Folderu układu 700dp szerokości ekranu](resources-for-varying-screens-images/03-layout-sw700dp-vs.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Folderu układu 700dp szerokości ekranu](resources-for-varying-screens-images/03-layout-sw700dp-xs.png)

-----


Przyjąć poniżej przedstawiono niektóre numery dla różnych urządzeń:

- **Typowy phone** &ndash; 320dp: typowe telefonu

- **Tablet 5"/"tweener"urządzenie** &ndash; 480dp: takie jak Uwaga Samsung

- **Tablet 7"** &ndash; 600dp: takie jak Barnes &amp; Noble Nook

- **Tablet 10"** &ndash; 720dp: takie jak Xoom firmie

Dla aplikacji, że docelowy API poziomy do 12 (3.1 dla systemu Android), układów powinien go w programie katalogi, które używają kwalifikatory **małych**/**normalne**/**duże**  / **xlarge** jako generalizacji różnych rozmiarów ekranu, które są dostępne w większości urządzeń. Na przykład na poniższej ilustracji, brak alternatywnych zasobów dla czterech różnych rozmiarów ekranu:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Alternatywne zasobów dla czterech rozmiaru ekranu](resources-for-varying-screens-images/04-layout-large-vs.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Alternatywne zasobów dla czterech rozmiaru ekranu](resources-for-varying-screens-images/04-layout-large-xs.png)

-----

Poniżej przedstawiono porównanie porównanie starszej wersji pre-API poziom 13 ekranu rozmiar kwalifikatory pikselach niezależnych od gęstość:

- jest 426dp x 320dp **małe**

- jest 470dp x 320dp **normalne**

- jest 640dp x 480dp **duże**

- jest 960dp x 720dp **xlarge**

Nowsze ekranu rozmiar kwalifikatory w poziom interfejsu API 13 i zapasowej mają wyższy priorytet niż starsze kwalifikatory ekranu poziomy interfejsu API 12 i niżej.
Dla aplikacji obejmujących stary i nowy poziom interfejsu API może być konieczne utworzenie alternatywnych zasobów przy użyciu oba zestawy kwalifikatory, jak pokazano na poniższym zrzucie ekranu:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Alternatywne zasoby z obu kwalifikatorów](resources-for-varying-screens-images/05-both-qualifiers-vs.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Alternatywne zasoby z obu kwalifikatorów](resources-for-varying-screens-images/05-both-qualifiers-xs.png)

-----



### <a name="provide-different-bitmaps-for-different-screen-densities"></a>Podaj inną map bitowych gęstości inny ekran

Chociaż Android są skalowane map bitowych jako niezbędne dla urządzenia, mapy bitowe, same nie może trwającej skalować w górę lub w dół: może zostać rozmytego lub rozmyty. Dostarczanie mapy bitowe odpowiednie dla ekranu w pikselach będzie uniknąć tego problemu.

Na przykład na poniższym obrazie przedstawiono przykładowy układu i wyglądu problemów, które mogą wystąpić podczas gęstość Określ zasobów nie są dostępne.

![Zrzuty ekranu bez gęstość zasobów](resources-for-varying-screens-images/06-density-not-provided.png)

Porównaj te układ stworzonego z dużą gęstością określonych zasobów:

![Zrzuty ekranu przy użyciu zasobów specyficznych dla gęstość](resources-for-varying-screens-images/07-density-specific-resources.png)


### <a name="create-varying-density-resources-with-android-asset-studio"></a>Utwórz różne zasoby gęstość z zasobów Android Studio

Tworzenie tych map bitowych różnych gęstości mogą być nieco nużące. W efekcie Google utworzył narzędzia online, co może zmniejszyć niektóre konieczność powtarzania związane z tworzeniem tych map bitowych o nazwie [ **Android Studio zasobów**](https://romannurik.github.io/AndroidAssetStudio/).

[![Zasobów android Studio](resources-for-varying-screens-images/08-android-asset-studio-sml.png)](resources-for-varying-screens-images/08-android-asset-studio.png#lightbox)

Tworzenie map bitowych target cztery typowe gęstości ekranu zapewniając jednego obrazu pomoże tej witryny sieci Web. Android Studio zasobów utworzeniu map bitowych o pewne dostosowania, a następnie zezwolić im na można pobrać jako plik zip.


## <a name="tips-for-multiple-screens"></a>Porady dotyczące wielu ekranów

Android działa na bewildering liczbę urządzeń, i kombinacja rozmiarów ekranu i gęstości ekranu może wydawać się utrudnione. Poniższe porady mogą pomóc zminimalizować ilość pracy niezbędne do obsługi różnych urządzeń:

- **Tylko projektowanie i opracowanie, jakie można potrzeby** &ndash; istnieje wiele różnych urządzeń tam, ale niektóre istnieją w rzadkich rozmiarach, które może skutkować zajęciem dużego nakładu pracy do projektowania i opracowywania dla. [ **Rozmiaru ekranu i z dużą gęstością** ](http://developer.android.com/resources/dashboard/screens.html) pulpit nawigacyjny jest strona dostarczony przez firmę Google dostarczający dane podział macierzy gęstość rozmiar/ekran ekranu. Ten podział zapewnia wgląd na temat nakład pracy na obsłudze ekranów.

- **Użycie punktu dystrybucji zamiast pikseli** -pikseli stają się powodującymi jako ekran zmieni się gęstości. Nie umieszczaj wartości pikseli. Unikaj pikseli na rzecz punktu dystrybucji (w pikselach niezależnych od gęstość).

- **Unikaj** [AbsoluteLayout](https://developer.xamarin.com/api/type/Android.Widget.AbsoluteLayout/)
  **wszędzie tam, gdzie to możliwe** &ndash; jest przestarzała w poziom interfejsu API 3 (z systemem Android w wersji 1.5) i spowoduje łamliwa układów. Nie można używać go. Zamiast tego spróbuj użyć takich jak elastyczny układ elementów widget [ **LinearLayout**](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/), [ **RelativeLayout**](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/), lub nowa [ **GridLayout**](https://developer.xamarin.com/api/type/Android.Widget.GridLayout/).

- **Wybierz jedną orientację układu jako domyślnej** &ndash; na przykład zamiast zapewnianie zasobów alternatywny **ziemi układu** i **portu układu**, umieść zasoby dla poziomą w **układu**i zasoby dla orientacji pionowej do **układu portu**.

- **Użyj LayoutParams wysokość i szerokość** — podczas definiowania elementów interfejsu użytkownika w pliku XML układu aplikację systemu Android przy użyciu **wrap_content** i **fill_parent** wartości będą mieć więcej Powodzenie zapewnienia prawidłowego wyglądu wielu różnych urządzeniach niż przy użyciu pikseli lub gęstość niezależne jednostki. Te wartości powodują systemu Android do skali mapy bitowej zasobów zgodnie z potrzebami. Z tego powodu tej samej jednostki miary niezależne gęstość są najlepiej zarezerwowane dla podczas określania marginesy i dopełnienia elementów interfejsu użytkownika.


## <a name="testing-multiple-screens"></a>Testowanie wielu ekranów

Aplikacja systemu Android musi być testowana wszystkie konfiguracje, które będą obsługiwane. W idealnym przypadku urządzenia powinny być testowane na z samymi urządzeniami rzeczywiste, ale w wielu przypadkach to nie jest możliwe lub praktyczne.
W takim przypadku będzie przydatna używanie emulatora i konfiguracji urządzeń wirtualnych systemu Android dla każdej konfiguracji urządzenia.

Zestaw Android SDK zawiera niektóre emulatora karnacji mogą być używane do tworzenia w AVD zreplikuje rozmiar, gęstość i rozpoznawania wiele urządzeń.
Podobnie wielu dostawców sprzętu Podaj karnacji dla swoich urządzeń.

Innym rozwiązaniem jest używanie usług innych firm testowania usługi.
Te usługi otrzymuje APK, uruchom go na różnych urządzeniach i następnie wyrazić swoją opinię, w jaki sposób działał aplikacji.
