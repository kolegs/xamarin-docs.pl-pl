---
title: Podstawowe informacje dotyczące zasobów dla systemu android
ms.prod: xamarin
ms.assetid: ED32E7B5-D552-284B-6385-C3EDDCC30A4B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/01/2018
ms.openlocfilehash: f6be1001e5d3455a94e677f1bb5dc52ca574b873
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
ms.locfileid: "30764895"
---
# <a name="android-resource-basics"></a>Podstawowe informacje dotyczące zasobów dla systemu android

Prawie wszystkie aplikacje systemu Android mają jakieś zasobów w nich; co najmniej często mają układów interfejsu użytkownika w postaci plików XML. Podczas tworzenia aplikacji platformy Xamarin.Android domyślnych zasobów są konfiguracji przez szablon projektu platformy Xamarin.Android:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Pliki zasobów](android-resource-basics-images/01-resource-files-vs.png)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Pliki zasobów](android-resource-basics-images/01-resource-files-xs.png)
 
-----

Pięć plików, wchodzące w skład domyślnych zasobów zostały utworzone w folderze zasobów:

-  **Icon.PNG** &ndash; domyślną ikonę dla aplikacji

-  **Main.axml** &ndash; domyślny plik układ interfejsu użytkownika dla aplikacji. Należy pamiętać, że podczas Android używa **.xml** rozszerzenie pliku, używa platformy Xamarin.Android **.axml** rozszerzenia pliku.

-  **Strings.XML** &ndash; tabeli ciągów, aby pomóc w lokalizacji aplikacji

-  **AboutResources.txt** &ndash; to nie jest konieczne i można bezpiecznie usunąć. Zapewnia tylko wysokiego poziomu Omówienie folderu zasobów oraz w nim plików.

-  **Resource.Designer.cs** &ndash; ten plik jest automatycznie generowany i obsługiwana przez platformy Xamarin.Android i blokad unikatowe identyfikatory przypisane do każdego zasobu. To jest bardzo podobne i takie same jak w celu pliku R.java, które byłyby aplikacji systemu Android napisany w języku Java. Automatycznie jest tworzony za pomocą narzędzi platformy Xamarin.Android, a zostaną ponownie wygenerowane od czasu do czasu.


## <a name="creating-and-accessing-resources"></a>Tworzenie i uzyskiwanie dostępu do zasobów

Tworzenie zasobów jest tak proste, jak dodawanie plików do katalogu dla danego typu zasobu. Ekran zrzut poniżej przedstawiono zasoby ciągów dla ustawień regionalnych niemieckim zostały dodane do projektu. Gdy **Strings.xml** zostało dodane do pliku, **Akcja kompilacji** automatycznie ustawiono **AndroidResource** za pomocą narzędzi platformy Xamarin.Android:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Akcja dla Strings.xml ustawioną AndroidResource kompilacji](android-resource-basics-images/02-build-action-vs.png)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Akcja dla Strings.xml ustawioną AndroidResource kompilacji](android-resource-basics-images/02-build-action-xs.png)
 
-----
 

Dzięki temu narzędzia platformy Xamarin.Android prawidłowo gromadzenie i osadzanie zasobów w pliku APK. Jeśli z jakiegoś powodu **Akcja kompilacji** nie jest ustawiony na **Android zasobów**, następnie pliki zostaną wykluczone z APK i próba załadowania lub uzyskania dostępu do zasobów spowoduje błąd wykonania i Aplikacja ulegnie awarii.

Ponadto ważne jest, aby należy pamiętać, że podczas Android obsługuje tylko małe litery w nazwach plików zasobów elementów, Xamarin.Android bardziej forgiving; zostanie obsługuje ona zarówno wielkich i małych nazw plików. Konwencja dla nazwy obrazów jest użycie małe znakami podkreślenia jako separatorów (na przykład **Moje\_obrazu\_name.png**). Należy pamiętać, że nazwy zasobów nie może być przetwarzane, gdy kresek ani spacji, które są używane jako separatorów.

Gdy zasoby zostały dodane do projektu, istnieją dwa sposoby używania ich w aplikacji &ndash; programowo (wewnątrz kodu) lub z plików XML.


## <a name="referencing-resources-programmatically"></a>Programowo odwołujące się do zasobów

Aby uzyskać dostęp do tych plików programowo, są przypisane identyfikator unikatowy zasób Ten identyfikator zasobu jest liczbą całkowitą zdefiniowana w klasie specjalne o nazwie `Resource`, który znajduje się w pliku **Resource.designer.cs**, i wygląda następująco:

```csharp
public partial class Resource
{
    public partial class Attribute
    {
    }
    public partial class Drawable {
        public const int Icon=0x7f020000;
    }
    public partial class Id
    {
        public const int Textview=0x7f050000;
    }
    public partial class Layout
    {
        public const int Main=0x7f030000;
    }
    public partial class String
    {
        public const int App_Name=0x7f040001;
        public const int Hello=0x7f040000;
    }
}
```

Każdy identyfikator zasobu znajduje się wewnątrz klasy zagnieżdżonej odpowiadający typowi zasobu. Na przykład, jeśli plik **Icon.png** został dodany do projektu platformy Xamarin.Android zaktualizowane `Resource` klasy, tworzenie zagnieżdżona klasa o nazwie `Drawable` w stałej wewnątrz o nazwie `Icon`.
Dzięki temu plik **Icon.png** określano w kodzie jako `Resource.Drawable.Icon`. `Resource` Klasa nie należy ręcznie edytować, ponieważ wszelkie zmiany, które zostały wprowadzone zostaną zastąpione przez platformy Xamarin.Android.

Podczas odwoływania się do zasobów (w kodzie), są one dostępne za pośrednictwem hierarchii klas zasobów, której używa następującej składni:

```xml
@[<PackageName>.]Resource.<ResourceType>.<ResourceName>
```

-  **Nazwa pakietu** &ndash; wymagany pakiet, który dostarcza zasobu i jest tylko wtedy, gdy są używane zasoby z innymi pakietami.

-  **Typ zasobu** &ndash; jest typ zagnieżdżony zasobu, który jest w ramach klasy zasobów opisane powyżej.

-  **Nazwa zasobu** &ndash; to nazwa pliku zasobu (bez rozszerzenia) lub wartość atrybutu android: name dla zasobów, które znajdują się w elemencie XML.


## <a name="referencing-resources-from-xml"></a>Odwołania do zasobów z pliku XML

Zasoby w pliku XML są dostępne następujące specjalnej składni:

```xml
@[<PackageName>:]<ResourceType>/<ResourceName>.
```

-  **Nazwa pakietu** &ndash; wymagany pakiet, który dostarcza zasobu i jest tylko wtedy, gdy są używane zasoby z innymi pakietami.

-  **Typ zasobu** &ndash; to typ zagnieżdżony zasobu, który znajduje się w klasie zasobu.

-  **Nazwa zasobu** &ndash; to nazwa pliku zasobu (*bez* rozszerzenia typu plików) lub wartość `android:name` atrybutu dla zasobów, które znajdują się w elemencie XML.

Na przykład zawartości pliku układu, **Main.axml**, są następujące:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:orientation="vertical"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent">
    <ImageView android:id="@+id/myImage"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:src="@drawable/flag" />
</LinearLayout>
```

W tym przykładzie ma [ `ImageView` ](https://developer.xamarin.com/recipes/android/controls/imageview) wymagający obiektów drawable zasób o nazwie **flagi**. `ImageView` Ma jego `src` ustawić atrybutu **@drawable/flag**. Po uruchomieniu działania Android będzie wyglądać w katalogu **zasobów/Drawable** dla pliku o nazwie **flag.png** (rozszerzenie pliku może być inny format obrazu, takie jak **flag.jpg**) i załadowania tego pliku i wyświetl ją w `ImageView`.
Po uruchomieniu tej aplikacji, jego może wyglądać poniższej ilustracji:

![ImageView zlokalizowanych](android-resource-basics-images/03-localized-screenshot.png)

