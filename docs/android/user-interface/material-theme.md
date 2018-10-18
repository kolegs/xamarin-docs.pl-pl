---
title: Motyw materiału
description: Jak motyw aplikacji platformy Xamarin.Android przy użyciu motyw materiału
ms.prod: xamarin
ms.assetid: DC4CDBD0-3DF9-4B7E-B876-29128985E2A7
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: 4b9c39a0ced9a264f501d78142c3bdfd556593ed
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/18/2018
ms.locfileid: "30771323"
---
# <a name="material-theme"></a>Motyw materiału

*Motyw materiału* jest styl interfejsu użytkownika, który określa wygląd i działanie, widoki i działań, począwszy od systemu Android 5.0 (Lollipop). Motyw materiału jest wbudowana w systemie Android 5.0, więc jest używany przez system interfejsu użytkownika, a także przez aplikacje. Motyw materiału, który nie jest "kompozycji" w sensie opcji wyglądu całego systemu, które użytkownik może dynamicznie wybrać z menu ustawień. Przeciwnie motyw materiału można traktować jako zbiór powiązanych wbudowane style podstawowej, które umożliwia dostosowywanie wyglądu i działania aplikacji.

System android oferuje trzy odmian motyw materiału:

-  `Theme.Material` &ndash; Wersja ciemny motyw materiału; jest to wersja domyślna w systemie Android 5.0.

-  `Theme.Material.Light` &ndash; Wersja jasny motyw materiału.

-  `Theme.Material.Light.DarkActionBar` &ndash; Wersja jasny motyw materiału, ale przy użyciu paska akcji ciemny.

Przykłady te odmian motyw materiału są wyświetlane w tym miejscu:

[![Przykładowe zrzuty ekranów motyw ciemny motyw jasny i motyw ciemny pasek akcji](material-theme-images/three-flavors-sml.png)](material-theme-images/three-flavors.png#lightbox)

Z motywu materiały do tworzenia motywów, może pochodzić przesłanianie niektórych lub wszystkich atrybutów koloru. Na przykład można utworzyć motyw, która pochodzi od klasy `Theme.Material.Light`, lecz zastępuje kolor paska aplikacji, aby dopasować kolor marki. Styl możesz także pojedyncze widoki; na przykład można utworzyć styl dla [CardView](~/android/user-interface/controls/card-view.md) , ma więcej zaokrąglone rogi i używa ciemniejszy kolor tła.

Można użyć pojedynczego motywu dla całej aplikacji, możesz też różne tematy dla różnych ekranów (działalności) w aplikacji. W powyższym zrzuty ekranu, na przykład pojedynczej aplikacji używa innego motywu dla każdego działania do zademonstrowania wbudowanych schematów kolorów. Przyciski radiowe Przełącz aplikacji do różnych działań, a w rezultacie, wyświetlić różne tematy.

Motyw materiału jest obsługiwany tylko w systemie Android 5.0 lub nowszy, nie możesz użyć go (lub niestandardowy motyw pochodną motyw materiału) do motywu aplikację do uruchamiania we wcześniejszych wersjach systemu Android. Jednak należy skonfigurować aplikację do używania motyw materiału na urządzeniach z systemem Android 5.0 i bez problemu zmieniała wrócić do wcześniejszej motywu po uruchomieniu w starszych wersjach systemu Android (zobacz [zgodności](#compatibility) dalszej części tego artykułu, aby uzyskać szczegółowe informacje).


## <a name="requirements"></a>Wymagania

Następujące wymagane jest wprowadzenie nowych funkcji systemu Android 5.0 materiał motywu w aplikacjach opartych na środowisku Xamarin:

-  **Platforma Xamarin.Android** &ndash; Xamarin.Android 4.20 lub nowszy musi być zainstalowane i skonfigurowane za pomocą programu Visual Studio lub Visual Studio dla komputerów Mac. 

-  **Zestaw SDK systemu android** &ndash; Android 5.0 (21 interfejsu API) lub nowszym należy zainstalować za pośrednictwem Menedżera zestawów Android SDK.

-  **Java JDK 1.8** &ndash; zestaw JDK 1.7 może być używany, jeśli są specjalnie poziom odwołujących interfejsu API 23 i starszych. Zestaw JDK 1.8 jest dostępne z [Oracle](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).

Aby dowiedzieć się, jak skonfigurować projekt aplikacji systemu Android 5.0, zobacz [ustawienie zapasowej projektu dla systemu Android 5.0](~/android/platform/lollipop.md).


## <a name="using-the-built-in-themes"></a>Używanie wbudowanych motywów

Najprostszym sposobem użycia motyw materiału jest aby skonfigurować aplikację do używania wbudowanych motyw bez dostosowania. Jeśli nie chcesz jawnie skonfigurować motyw, domyślnie zostanie aplikacji `Theme.Material` (ciemny). Jeśli aplikacja ma tylko jedno działanie, można skonfigurować motyw na poziomie aplikacji. Jeśli aplikacja ma wiele działań, można skonfigurować motyw na poziomie aplikacji, aby ten sam motyw używa we wszystkich działań lub różne tematy można przypisać do różnych działań. Jak skonfigurować kompozycje na poziomie aplikacji i na poziomie działania można znaleźć w poniższych sekcjach.


### <a name="theming-an-application"></a>Motyw aplikacji

Aby skonfigurować całej aplikacji, aby użyć wersji motyw materiału, ustaw `android:theme` atrybutu węzła aplikacji w **AndroidManifest.xml** do jednej z następujących czynności:

-  `@android:style/Theme.Material` &ndash; Motyw ciemny.

-  `@android:style/Theme.Material.Light` &ndash; Motyw jasny.

-  `@android:style/Theme.Material.Light.DarkActionBar` &ndash; Motyw jasny przy użyciu paska akcji ciemny.

Poniższy przykład umożliwia skonfigurowanie aplikacji *MyApp* używać motyw jasny:

```xml
<application android:label="MyApp" 
             android:theme="@android:style/Theme.Material.Light">
</application>
```

Alternatywnie można skonfigurować aplikację `Theme` atrybutu w **AssemblyInfo.cs** (lub **Properties.cs**). Na przykład:

```C#
[assembly: Application(Theme="@android:style/Theme.Material.Light")]
```

Jeśli motyw aplikacji jest równa `@android:style/Theme.Material.Light`, każde działanie w *MyApp* będzie wyświetlany `Theme.Material.Light`.


### <a name="theming-an-activity"></a>Motywy działania

Motyw działanie, należy dodać `Theme` ustawienie `[Activity]` atrybutu powyżej deklaracji swoje działania i przypisać `Theme` do flavor motyw materiału, którego chcesz używać. Następujące tematy przykładowe działanie z `Theme.Material.Light`:

```C#
[Activity(Theme = "@android:style/Theme.Material.Light",
          Label = "MyApp", MainLauncher = true, Icon = "@drawable/icon")]  
```

Inne działania w tej aplikacji użyje domyślnej `Theme.Material` schemat kolorów ciemny (lub, jeśli skonfigurowano ustawienie motyw aplikacji).

<a name="customtheme" />

## <a name="using-custom-themes"></a>Przy użyciu niestandardowych kompozycji

Możesz zwiększyć swoją markę, tworzenie niestandardowego motywu, który stylów aplikacji za pomocą swoją markę&rsquo;s kolorów. Aby utworzyć niestandardowy motyw, zdefiniujesz nowe styl, który pochodzi z wbudowanych smak motyw materiału zastępowanie atrybutów kolorów, które chcesz zmienić. Na przykład można zdefiniować niestandardowy motyw, która pochodzi od klasy `Theme.Material.Light.DarkActionBar` i zmienia kolor tła ekranu beżowy zamiast biały.

Motyw materiału uwidacznia następujące atrybuty układu do dostosowania:

-  `colorPrimary` &ndash; Kolor paska aplikacji.

-  `colorPrimaryDark` &ndash; Kolor paska stanu i paski kontekstowych aplikacji; jest to zwykle ciemny wersję `colorPrimary`.

-  `colorAccent` &ndash; Kolor kontrolek interfejsu użytkownika, takie jak pola tekstowe edycji pola wyboru i przyciski opcji.

-  `windowBackground` &ndash; Kolor tła ekranu.

-  `textColorPrimary` &ndash; Kolor tekstu interfejsu użytkownika, na pasku aplikacji.

-  `statusBarColor` &ndash; Kolor paska stanu.

-  `navigationBarColor` &ndash; Kolor paska nawigacyjnego.

Te obszary ekranu są oznaczone etykietami na poniższym diagramie:

[![Diagram atrybutów i ich obszarów skojarzonym ekranie](material-theme-images/screen-attributes-sml.png)](material-theme-images/screen-attributes.png#lightbox)

Domyślnie `statusBarColor` jest ustawiona na wartość `colorPrimaryDark`. Możesz ustawić `statusBarColor` pełny kolor lub ustawienie go na `@android:color/transparent` być przezroczysty na pasku stanu. Na pasku nawigacyjnym można również przezroczyste, ustawiając `navigationBarColor` do `@android:color/transparent`.


### <a name="creating-a-custom-app-theme"></a>Tworzenie motywów aplikacji niestandardowej

Motyw aplikacji niestandardowej można utworzyć, tworząc i modyfikując pliki w **zasobów** folderu projektu aplikacji. Do określania stylu swoją aplikację przy użyciu niestandardowego motywu, wykonaj następujące kroki:

-   Tworzenie **colors.xml** w pliku **zasobów/wartości** &mdash; definiować kolory motywu niestandardowego przy użyciu tego pliku. Na przykład można Wklej następujący kod do **colors.xml** aby pomóc Ci rozpocząć pracę:

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<resources>
    <color name="my_blue">#3498DB</color>
    <color name="my_green">#77D065</color>
    <color name="my_purple">#B455B6</color>
    <color name="my_gray">#738182</color>
</resources>
```

-   Zmodyfikuj ten plik na przykład, aby definiować nazwy i kodów kolorów dla zasobów kolorów, które będą używane w niestandardowych kompozycji.

-   Tworzenie **zasobów/wartości v21** folderu. W tym folderze utwórz **styles.xml** pliku:

    [![Lokalizacja styles.xml w folderze zasobów/wartości 21.xml](material-theme-images/values-v21-sml.png)](material-theme-images/values-v21.png#lightbox)

    Należy pamiętać, że **zasobów/wartości v21** specyficzne dla systemu Android 5.0 &ndash; starsze wersje systemu android nie odczyta pliki w tym folderze.

-   Dodaj `resources` węzeł **styles.xml** i zdefiniuj `style` węzeł z nazwą Twojego niestandardowego motywu. Na przykład Oto **styles.xml** pliku, który definiuje *MyCustomTheme* (pochodzące z wbudowanej `Theme.Material.Light` style motyw):

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<resources>
    <!-- Inherit from the light Material Theme -->
    <style name="MyCustomTheme" parent="android:Theme.Material.Light">
        <!-- Customizations go here -->
    </style>
</resources>
```

-   W tym momencie aplikację, która używa *MyCustomTheme* wyświetli zasobów `Theme.Material.Light` motyw bez dostosowań:

    [![Wygląd motywu niestandardowego przed dostosowania](material-theme-images/custom-theme-before-sml.png)](material-theme-images/custom-theme-before.png#lightbox)

-   Dodawanie dostosowań kolor **styles.xml** , definiując kolory atrybuty układu, które chcesz zmienić. Na przykład, aby zmienić kolor paska aplikacji, aby `my_blue` i zmienić kolor kontrolki interfejsu użytkownika do `my_purple`, Dodaj kolor zastąpień **styles.xml** odwołujące się do zasobów kolor skonfigurowane w **colors.xml**:

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

Za pomocą tych zmian w miejscu aplikacji, który używa *MyCustomTheme* będzie wyświetlany kolor paska aplikacji w `my_blue` i kontrolek interfejsu użytkownika w `my_purple`, ale `Theme.Material.Light` wszędzie, gdzie inny schemat kolorów:

[![Wygląd motywu niestandardowego po dostosowania](material-theme-images/custom-theme-after-sml.png)](material-theme-images/custom-theme-after.png#lightbox)

W tym przykładzie *MyCustomTheme* pożycza kolory z `Theme.Material.Light` tła kolorów, pasek stanu i kolorów tekstu, ale zmienia kolor na pasku aplikacji, aby `my_blue` i ustawia kolor przycisku radiowego `my_purple`.

<a name="customview" />

### <a name="creating-a-custom-view-style"></a>Tworzenie stylu Widok niestandardowy

Android 5.0 umożliwia także służących do nadawania stylu indywidualnych widoków. Po utworzeniu **colors.xml** i **styles.xml** (zgodnie z opisem w poprzedniej sekcji), możesz dodać styl widoku **styles.xml**.
Do określania stylu indywidualnych widoków, wykonaj następujące kroki:

-   Edytuj **Resources/values-v21/styles.xml** i Dodaj `style` węzeł z nazwą własnego stylu widoku niestandardowego. Ustawianie atrybutów kolor niestandardowy widok w ramach tej `style` węzła. Na przykład, aby utworzyć niestandardową [CardView](~/android/user-interface/controls/card-view.md) styl, który ma więcej zaokrąglone rogi i używa `my_blue` kolor tła kart dodanie `style` węzeł **styles.xml** (wewnątrz `resources`węzła) i skonfiguruj usługi radius koloru i rogu tła:

```xml
<!-- Theme an individual view: -->
<style name="CardView.MyBlue">

    <!-- Change the background color to Xamarin blue: -->
    <item name="cardBackgroundColor">@color/my_blue</item>

    <!-- Make the corners very round: -->
    <item name="cardCornerRadius">18dp</item>
</style>
```

-   W układzie, ustawić `style` atrybutu dla tego widoku dopasować nazwę styl niestandardowy, który został wybrany w poprzednim kroku. Na przykład:

```xml
<android.support.v7.widget.CardView
    style="@style/CardView.MyBlue"
    android:layout_width="200dp"
    android:layout_height="100dp"
    android:layout_gravity="center_horizontal">
```

Poniższy zrzut ekranu przedstawiono przykład domyślnego `CardView` (wyświetlana po lewej stronie) w porównaniu do `CardView` , został wstawiony w z niestandardowym `CardView.MyBlue` motywu (wyświetlane po prawej stronie):

[![Przykłady domyślne CardView i CardView niestandardowe](material-theme-images/custom-cardview-sml.png)](material-theme-images/custom-cardview.png#lightbox)

W tym przykładzie niestandardową `CardView` jest wyświetlany kolor tła `my_blue` i promienia narożnika 18dp.


## <a name="compatibility"></a>Zgodność

Do określania stylu swojej aplikacji, dzięki czemu używa motyw materiału w systemie Android 5.0, ale zostanie automatycznie przywrócona do stylu zniżkową zgodne w starszych wersjach systemu Android, wykonaj następujące kroki:

-   Definiowanie niestandardowego motywu w **Resources/values-v21/styles.xml** pochodząca z style motyw materiału. Na przykład:

```xml
<resources>
    <style name="MyCustomTheme" parent="android:Theme.Material.Light">
        <!-- Your customizations go here -->
    </style>
</resources>
```

-   Definiowanie niestandardowego motywu w **Resources/values/styles.xml** pochodzi od starsze motywu, ale używa tej samej nazwie motyw opisanych powyżej. Na przykład:

```xml
<resources>
    <style name="MyCustomTheme" parent="android:Theme.Holo.Light">
        <!-- Your customizations go here -->
    </style>
</resources>
```

-   W **AndroidManifest.xml**, skonfiguruj aplikację o nazwie niestandardowy motyw. 
    Na przykład:

```xml
<application android:label="MyApp" 
             android:theme="@style/MyCustomTheme">
</application>
```

-   Alternatywnie można stylu określonego działania, za pomocą niestandardowego motywu:

```C#
[Activity(Label = "MyActivity", Theme = "@style/MyCustomTheme")]
```

Jeśli Twój wybrany motyw używa kolorów zdefiniowanych w **colors.xml** plików, należy umieścić ten plik w **zasobów/wartości** (zamiast **zasobów/wartości v21**), aby obie wersje niestandardowego motywu może uzyskiwać dostęp do definicji koloru.

Gdy Twoja aplikacja jest uruchamiana na urządzeniu z systemem Android 5.0, zostanie użyty określony w definicji motywu **Resources/values-v21/styles.xml**. Po uruchomieniu tej aplikacji na starszych urządzeń z systemem Android, jego automatycznie powróci do określonych w definicji motywu **Resources/values/styles.xml**.

Aby uzyskać więcej informacji na temat motyw zgodność ze starszymi wersjami systemu Android, zobacz [zasoby alternatywne](~/android/app-fundamentals/resources-in-android/alternate-resources.md).

## <a name="summary"></a>Podsumowanie

Ten artykuł zawiera wprowadzenie nowego motywu materiały użytkownika interfejsu stylu zawarte w systemie Android 5.0 (Lollipop). On opisany trzy wbudowane odmian motyw materiału, używanych do określania stylu aplikacji, jego wyjaśniono, jak utworzyć niestandardowy motyw do znakowania aplikacji i podano przykładowy sposób motyw na poszczególnych widoku. Na koniec w tym artykule wyjaśniono, jak używać motyw materiału w aplikacji przy zachowaniu zniżkową zgodności ze starszymi wersjami systemu android.



## <a name="related-links"></a>Linki pokrewne

- [ThemeSwitcher (przykład)](https://developer.xamarin.com/samples/monodroid/android5.0/ThemeSwitcher)
- [Wprowadzenie do interfejsu typu lizak](../platform/lollipop.md)
- [CardView](controls/card-view.md)
- [Zasoby alternatywne](../app-fundamentals/resources-in-android/alternate-resources.md)
- [Android interfejsu typu lizak](https://developer.android.com/about/versions/lollipop)
- [Dla deweloperów systemu android kołowy](https://developer.android.com/about/versions/pie/)
- [Projektowania materiałów](https://developer.android.com/guide/topics/ui/look-and-feel/)
- [Zasady projektowania materiału](https://material.io/design/)
- [Zachowanie zgodności](https://developer.android.com/training/backward-compatible-ui/)
