---
title: Materiał motywu
description: Porady kompozycję aplikacji platformy Xamarin.Android materiałów kompozycji
ms.prod: xamarin
ms.assetid: DC4CDBD0-3DF9-4B7E-B876-29128985E2A7
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: a3b5f908330833a38aad9e329835a4a437fc29f0
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
ms.locfileid: "30771323"
---
# <a name="material-theme"></a>Materiał motywu

*Motyw materiału* jest styl interfejsu użytkownika, który określa wygląd i działanie widoków i działania, począwszy od systemu Android 5.0 (interfejs typu lizak). Motyw materiału jest wbudowana w Android 5.0, jest on używany przez system interfejsu użytkownika, a także przez aplikacje. Motyw materiału nie jest "kompozycji" w tym sensie opcji wyglądu całego systemu, które użytkownik może dynamicznie wybierz z menu ustawień. Zamiast motywu materiałów można traktować jako zestaw powiązanych wbudowane style podstawowej, które służy do dostosowywania wyglądu i działania aplikacji.

Android udostępnia trzy odmian materiału motywu:

-  `Theme.Material` &ndash; Wersji ciemnego motywu materiału. jest to wersja domyślna w systemie Android 5.0.

-  `Theme.Material.Light` &ndash; Wersja światła materiałów motywu.

-  `Theme.Material.Light.DarkActionBar` &ndash; Wersja światła motywu materiałów, ale z paskiem ciemny akcji.

Przykłady te odmian motywu materiałów są wyświetlane tutaj:

[![Zrzuty ekranu przykład motywu ciemny, motywu jasny i pasku akcji ciemny motyw](material-theme-images/three-flavors-sml.png)](material-theme-images/three-flavors.png#lightbox)

Z materiałów motywu, aby utworzyć własną kompozycję mogą pochodzić zastępowanie niektórych lub wszystkich atrybutów koloru. Na przykład można utworzyć motywu, która jest pochodną `Theme.Material.Light`, ale zastępuje kolor paska aplikacji, aby dopasować kolor oznakowanie. Styl możesz również poszczególnych widoków; na przykład można utworzyć stylu dla [CardView](~/android/user-interface/controls/card-view.md) ma więcej zaokrąglone narożniki i używa ciemniejszego kolor tła.

Można użyć jednego motyw dla całej aplikacji, lub można użyć różnych motywy dla różnych ekranach (działań) w aplikacji. W powyższym zrzuty ekranu, na przykład tylko jednej aplikacji używa innej kompozycji dla każdego działania do zaprezentowania wbudowanych schematów kolorów. Przyciski radiowe przełączyć aplikację do różnych działań, a w związku z tym wyświetlić kompozycji.

Motyw materiałów jest obsługiwane tylko w systemie Android 5.0 i nowszych, nie umożliwia on (lub niestandardowy motyw pochodną materiału motywu) motywu aplikacji dla we wcześniejszych wersjach systemu android. Jednak należy skonfigurować aplikację, aby użyć motywu materiałów na urządzeniach z systemem Android 5.0 i bezpiecznie wrócił do wcześniejszych motywu po uruchomieniu w starszych wersjach systemu android (zobacz [zgodności](#compatibility) sekcji tego artykułu, aby uzyskać szczegółowe informacje).


## <a name="requirements"></a>Wymagania

Poniżej jest wymagany do korzystania z nowych funkcji systemu Android 5.0 materiałów motyw w aplikacjach opartych na platformie Xamarin:

-  **Xamarin.Android** &ndash; Xamarin.Android 4.20 lub nowszej, musi być zainstalowana i skonfigurowana przy użyciu programu Visual Studio lub Visual Studio dla komputerów Mac. 

-  **Zestaw SDK systemu android** &ndash; Android 5.0 (interfejs API 21) lub nowszym należy zainstalować za pomocą Menedżera zestawu SDK systemu Android.

-  **Java JDK 1.8** &ndash; JDK 1.7 mogą być używane, gdy są w szczególności interfejsu API wskazuje poziom 23 i wcześniejszych. JDK 1.8 jest dostępna z [Oracle](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).

Aby dowiedzieć się, jak skonfigurować projekt aplikacji systemu Android 5.0, zobacz [ustawienie się projekt systemu Android 5.0](~/android/platform/lollipop.md).


## <a name="using-the-built-in-themes"></a>Za pomocą wbudowanych motywów

Najprostszym sposobem Użyj motywu materiałów jest skonfigurować aplikację do użycia wbudowanych motywu bez dostosowania. Jeśli nie chcesz jawnie skonfigurować motyw, domyślnie zostanie ustawiona do aplikacji `Theme.Material` (motyw ciemny). Jeśli aplikacja ma tylko jedno działanie, można skonfigurować motyw na poziomie aplikacji. Jeśli aplikacja ma wiele działań, można skonfigurować motyw na poziomie aplikacji, aby używa tego samego motywu we wszystkich działań, lub można przypisać różne kompozycje do różnych działań. W poniższych sekcjach opisano sposób konfigurowania kompozycje na poziomie aplikacji i na poziomie działania.


### <a name="theming-an-application"></a>Tworzenie motywów aplikacji

Aby skonfigurować całej aplikacji do użycia wersja motywu materiałów, ustaw `android:theme` atrybut węzła aplikacji w **AndroidManifest.xml** do jednej z następujących czynności:

-  `@android:style/Theme.Material` &ndash; Ciemny motyw.

-  `@android:style/Theme.Material.Light` &ndash; Motywu jasny.

-  `@android:style/Theme.Material.Light.DarkActionBar` &ndash; Motywu jasny z paskiem ciemny akcji.

Poniższy przykład umożliwia skonfigurowanie aplikacji *moja_aplikacja* do użycia motywu jasny:

```xml
<application android:label="MyApp" 
             android:theme="@android:style/Theme.Material.Light">
</application>
```

Alternatywnie można skonfigurować aplikację `Theme` atrybutu w **AssemblyInfo.cs** (lub **Properties.cs**). Na przykład:

```C#
[assembly: Application(Theme="@android:style/Theme.Material.Light")]
```

Jeśli ustawiono motywu aplikacji `@android:style/Theme.Material.Light`, każde działanie w *moja_aplikacja* będzie wyświetlany `Theme.Material.Light`.


### <a name="theming-an-activity"></a>Tworzenie motywów działania

Aby motywu działanie, należy dodać `Theme` ustawienie `[Activity]` atrybutu powyżej deklaracji Twojej aktywności i przypisz `Theme` do wersji materiałów motywu, który ma być używany. Następujące tematy przykładowe działanie z `Theme.Material.Light`:

```C#
[Activity(Theme = "@android:style/Theme.Material.Light",
          Label = "MyApp", MainLauncher = true, Icon = "@drawable/icon")]  
```

Inne działania w tej aplikacji zostanie użyta domyślna `Theme.Material` schemat kolorów ciemny (lub, jeśli skonfigurowano ustawienie motywu aplikacji).

<a name="customtheme" />

## <a name="using-custom-themes"></a>Przy użyciu niestandardowych kompozycji

Oznakowanie można zwiększyć, tworząc motyw niestandardowy style aplikacji za pomocą oznakowanie&rsquo;kolory s. Aby Utwórz kompozycję niestandardową, należy zdefiniować nowy styl pochodzącą z wbudowanych wersji motywu materiałów, zastępowanie atrybutów kolorów, które chcesz zmienić. Na przykład można zdefiniować kompozycję niestandardową, która jest pochodną `Theme.Material.Light.DarkActionBar` i zmienia kolor tła ekranu na beżowy zamiast biały.

Motyw materiału uwidacznia następujące atrybuty układu do dostosowania:

-  `colorPrimary` &ndash; Kolor na pasku aplikacji.

-  `colorPrimaryDark` &ndash; Kolor paska stanu i paski kontekstowe aplikacji; jest to zwykle ciemny wersji `colorPrimary`.

-  `colorAccent` &ndash; Kolor kontrolek interfejsu użytkownika, takich jak pola wyboru, przycisków radiowych i pól tekstowych edycji.

-  `windowBackground` &ndash; Kolor tła ekranu.

-  `textColorPrimary` &ndash; Kolor tekstu interfejsu użytkownika na pasku aplikacji.

-  `statusBarColor` &ndash; Kolor na pasku stanu.

-  `navigationBarColor` &ndash; Kolor na pasku nawigacji.

Te obszary ekranu są oznaczone na poniższym diagramie:

[![Diagram atrybutów i ich obszarów skojarzone ekranu](material-theme-images/screen-attributes-sml.png)](material-theme-images/screen-attributes.png#lightbox)

Domyślnie `statusBarColor` ma ustawioną wartość `colorPrimaryDark`. Można ustawić `statusBarColor` do pełnego koloru, lub ustaw ją na `@android:color/transparent` być przezroczysty na pasku stanu. Pasek nawigacyjny może również być przezroczyste przez ustawienie `navigationBarColor` do `@android:color/transparent`.


### <a name="creating-a-custom-app-theme"></a>Tworzenie niestandardowych aplikacji motywu

Można utworzyć motyw niestandardowych aplikacji przy tworzeniu i modyfikowaniu plików w **zasobów** folderu projektu aplikacji. Do określania stylu aplikacji za pomocą niestandardowego motywu, wykonaj następujące kroki:

-   Utwórz **colors.xml** w pliku **zasobów/wartości** &mdash; ten plik służy do definiowania niestandardowego motywu kolorów. Można na przykład Wklej następujący kod do **colors.xml** ułatwiające rozpoczęcie pracy:

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<resources>
    <color name="my_blue">#3498DB</color>
    <color name="my_green">#77D065</color>
    <color name="my_purple">#B455B6</color>
    <color name="my_gray">#738182</color>
</resources>
```

-   Zmodyfikuj ten przykładowy plik do definiowania nazwy i kody kolorów dla zasobów kolorów, które będą używane w niestandardowych kompozycji.

-   Utwórz **zasobów/wartości v21** folderu. W tym folderze utwórz **styles.xml** pliku:

    [![Lokalizacja styles.xml w folderze zasobów/wartości 21.xml](material-theme-images/values-v21-sml.png)](material-theme-images/values-v21.png#lightbox)

    Należy pamiętać, że **zasobów/wartości v21** jest specyficzna dla systemu Android 5.0 &ndash; starszych wersji systemu android nie będzie odczytywać pliki w tym folderze.

-   Dodaj `resources` węzeł **styles.xml** i zdefiniuj `style` węzeł z nazwą niestandardowego motywu. Na przykład, w tym miejscu jest **styles.xml** pliku, który definiuje *MyCustomTheme* (pochodzące z wbudowanych `Theme.Material.Light` stylów motywu):

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<resources>
    <!-- Inherit from the light Material Theme -->
    <style name="MyCustomTheme" parent="android:Theme.Material.Light">
        <!-- Customizations go here -->
    </style>
</resources>
```

-   W tym momencie aplikacji, która używa *MyCustomTheme* wyświetli zasobów `Theme.Material.Light` motyw bez dostosowań:

    [![Wygląd motyw niestandardowy przed dostosowania](material-theme-images/custom-theme-before-sml.png)](material-theme-images/custom-theme-before.png#lightbox)

-   Dodawanie dostosowań kolorów do **styles.xml** przez zdefiniowanie kolorów układu atrybutów, które chcesz zmienić. Na przykład, aby zmienić kolor paska aplikacji, aby `my_blue` i zmienić kolor kontrolek interfejsu użytkownika do `my_purple`, Dodaj zastąpienie kolor **styles.xml** odwołujące się do zasobów kolor skonfigurowane w **colors.xml**:

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<resources>
    <!-- Inherit from the light Material Theme -->
    <style name="MyCustomTheme" parent="android:Theme.Material.Light">
        <!-- Override the app bar color -->
        <item name="android:colorPrimary">@color/my_blue</item>
        <!-- Override the color of UI controls -->
        <item name="android:colorAccent">@color/my_purple</item>
    </style>
</resources>
```

Wprowadzone zmiany w miejscu aplikacji, która używa *MyCustomTheme* wyświetli kolor paska aplikacji w `my_blue` i formantów interfejsu użytkownika w `my_purple`, ale użyj `Theme.Material.Light` wszędzie inny schemat kolorów:

[![Motyw niestandardowy wygląd po dostosowania](material-theme-images/custom-theme-after-sml.png)](material-theme-images/custom-theme-after.png#lightbox)

W tym przykładzie *MyCustomTheme* pożycza kolory z `Theme.Material.Light` tła kolor, pasek stanu i kolory tekstu, ale zmienia kolor na pasku aplikacji, aby `my_blue` i ustawia kolor przycisku radiowego `my_purple`.

<a name="customview" />

### <a name="creating-a-custom-view-style"></a>Tworzenie stylu Widok niestandardowy

Android 5.0 umożliwia także dla Ciebie do nadawania stylu poszczególnych widoku. Po utworzeniu **colors.xml** i **styles.xml** (zgodnie z opisem w poprzedniej sekcji), można dodać styl widoku **styles.xml**.
Do określania stylu poszczególnych widoku, wykonaj następujące kroki:

-   Edytuj **Resources/values-v21/styles.xml** i dodać `style` węzeł z nazwą własnego stylu Widok niestandardowy. Ustawianie atrybutów koloru niestandardowego widoku w ramach tego `style` węzła. Na przykład, aby utworzyć niestandardowy [CardView](~/android/user-interface/controls/card-view.md) styl, który ma więcej zaokrąglone narożniki i używa `my_blue` jako kolor tła karty, Dodaj `style` węzeł **styles.xml** (wewnątrz `resources`węzła) i skonfigurować usługi radius koloru i rogu tła:

```xml
<!-- Theme an individual view: -->
<style name="CardView.MyBlue">

    <!-- Change the background color to Xamarin blue: -->
    <item name="cardBackgroundColor">@color/my_blue</item>

    <!-- Make the corners very round: -->
    <item name="cardCornerRadius">18dp</item>
</style>
```

-   W układzie, ustaw `style` atrybutu dla tego widoku do zgodna z nazwą w poprzednim kroku wybrano opcję Styl niestandardowy. Na przykład:

```xml
<android.support.v7.widget.CardView
    style="@style/CardView.MyBlue"
    android:layout_width="200dp"
    android:layout_height="100dp"
    android:layout_gravity="center_horizontal">
```

Poniższy zrzut ekranu zawiera przykład domyślnych `CardView` (wyświetlana po lewej stronie) w porównaniu do `CardView` który ma zostać stylem niestandardowego `CardView.MyBlue` motywu (wyświetlane po prawej stronie):

[![Przykłady domyślnej CardView i CardView niestandardowe](material-theme-images/custom-cardview-sml.png)](material-theme-images/custom-cardview.png#lightbox)

W tym przykładzie niestandardowego `CardView` jest wyświetlany kolor tła `my_blue` i promień narożnika 18dp.


## <a name="compatibility"></a>Zgodność

Do określania stylu aplikacji tak, aby używa motywu materiału w systemie Android 5.0, ale automatycznie przywróci stylu pobieranej zgodny w starszych wersjach systemu Android, wykonaj następujące kroki:

-   Definiowanie niestandardowego motywu w **Resources/values-v21/styles.xml** który pochodzi ze stylów motywu materiału. Na przykład:

```xml
<resources>
    <style name="MyCustomTheme" parent="android:Theme.Material.Light">
        <!-- Your customizations go here -->
    </style>
</resources>
```

-   Definiowanie niestandardowego motywu w **Resources/values/styles.xml** pochodzi ze starszej motywu, ale używa tej samej nazwie motywu jako powyżej. Na przykład:

```xml
<resources>
    <style name="MyCustomTheme" parent="android:Theme.Holo.Light">
        <!-- Your customizations go here -->
    </style>
</resources>
```

-   W **AndroidManifest.xml**, skonfiguruj aplikację o nazwie motyw niestandardowy. 
    Na przykład:

```xml
<application android:label="MyApp" 
             android:theme="@style/MyCustomTheme">
</application>
```

-   Alternatywnie można stylów określonego działania przy użyciu niestandardowego motywu:

```C#
[Activity(Label = "MyActivity", Theme = "@style/MyCustomTheme")]
```

Jeśli korzysta z motywu kolorów zdefiniowanych w **colors.xml** pliku, umieść ten plik w **zasobów/wartości** (zamiast **zasobów/wartości v21**), aby obie wersje niestandardowych kompozycji można uzyskiwać dostęp do definicji kolorów.

Po uruchomieniu aplikacji na urządzeniu z systemem Android 5.0, zostanie użyty określony w definicji motywu **Resources/values-v21/styles.xml**. Po uruchomieniu tej aplikacji na urządzeniach z systemem Android starszą on automatycznie powróci do określonego w definicji motywu **Resources/values/styles.xml**.

Aby uzyskać więcej informacji o zgodności motywów ze starszymi wersjami systemu Android, zobacz [alternatywnych zasobów](~/android/app-fundamentals/resources-in-android/alternate-resources.md).

## <a name="summary"></a>Podsumowanie

W tym artykule wprowadzono nowy motyw materiałów użytkownika interfejsu styl zawarte w systemie Android 5.0 (interfejs typu lizak). On opisany trzy wbudowane odmian motywu materiałów, używanych do określania stylu aplikacji, go wyjaśniono, jak utworzyć niestandardowy motyw dla znakowania aplikacji i podać przykład do motywu poszczególnych widoku. Ponadto w tym artykule wyjaśniono sposób używania materiałów motywu w aplikacji przy zachowaniu pobieranej zgodność ze starszymi wersjami systemu android.



## <a name="related-links"></a>Linki pokrewne

- [ThemeSwitcher (przykład)](https://developer.xamarin.com/samples/monodroid/android5.0/ThemeSwitcher)
- [Wprowadzenie do interfejs typu lizak](~/android/platform/lollipop.md)
- [CardView](~/android/user-interface/controls/card-view.md)
- [Zasoby alternatywne](~/android/app-fundamentals/resources-in-android/alternate-resources.md)
- [Android L Developer Preview](http://developer.android.com/preview/index.html)
- [Materiał projektu](http://developer.android.com/preview/material/index.html)
- [Zasady materiału projektowania](http://static.googleusercontent.com/media/www.google.com/en/us/design/material-design.pdf)
- [Zachowanie zgodności](http://developer.android.com/preview/material/compatibility.html)
