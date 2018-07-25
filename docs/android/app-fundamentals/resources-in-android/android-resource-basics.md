---
title: Podstawowe informacje dotyczące zasobów dla systemu android
ms.prod: xamarin
ms.assetid: ED32E7B5-D552-284B-6385-C3EDDCC30A4B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/01/2018
ms.openlocfilehash: 207644f5a5d3d346214ba090dcd450e55fde2657
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241321"
---
# <a name="android-resource-basics"></a>Podstawowe informacje dotyczące zasobów dla systemu android

Niemal wszystkich aplikacji dla systemu Android będzie miał jakieś zasoby w nich. co najmniej często mają układy interfejsu użytkownika w postaci plików XML. Podczas tworzenia aplikacji platformy Xamarin.Android zasoby domyślne są konfiguracji przez szablon projektu platformy Xamarin.Android:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Pliki zasobów](android-resource-basics-images/01-resource-files-vs.png)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Pliki zasobów](android-resource-basics-images/01-resource-files-xs.png)
 
-----

Pięć plików, które tworzą domyślne zasoby zostały utworzone w folderze zasobów:

-  **Icon.PNG** &ndash; domyślną ikonę aplikacji

-  **Main.axml** &ndash; domyślny plik układu interfejsu użytkownika dla aplikacji. Należy zauważyć, że podczas Android używa **.xml** używa platformy Xamarin.Android rozszerzenie pliku **axml** rozszerzenie pliku.

-  **Strings.XML** &ndash; tabeli ciągów, aby pomóc w lokalizacji aplikacji

-  **AboutResources.txt** &ndash; to nie jest konieczne i można bezpiecznie usunąć. Ją po prostu Przegląd wysokiego poziomu folderu zasobów i plików.

-  **Resource.Designer.cs** &ndash; ten plik jest automatycznie generowany i utrzymywane przez platformy Xamarin.Android i przechowuje unikatowy identyfikatory przypisane do każdego zasobu. To jest bardzo podobne identyczny z plikiem R.java, którym zostaną nadane aplikacji systemu Android napisaną w języku Java w celu. Ona automatycznie jest tworzony za pomocą narzędzi platformy Xamarin.Android i zostaną wygenerowane od czasu do czasu.


## <a name="creating-and-accessing-resources"></a>Tworzenie i uzyskiwanie dostępu do zasobów

Tworzenie zasobów jest tak proste, jak dodawanie plików do katalogu dla danego typu zasobu. Poniższym zrzucie ekranu przedstawiono zasoby ciągów dla niemieckich ustawień regionalnych zostały dodane do projektu. Gdy **Strings.xml** został dodany do pliku, **Build Action** została automatycznie ustawiona na **AndroidResource** za pomocą narzędzi Xamarin.Android:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Akcja dla Strings.xml równa AndroidResource kompilacji](android-resource-basics-images/02-build-action-vs.png)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Akcja dla Strings.xml równa AndroidResource kompilacji](android-resource-basics-images/02-build-action-xs.png)
 
-----
 

Dzięki temu narzędzi Xamarin.Android prawidłowo kompilowania i osadzanie zasobów w pliku APK. Jeśli z jakiegoś powodu **Build Action** nie jest ustawiony na **zasobów systemu Android**, następnie pliki zostaną wykluczone z zestawu APK i każda próba załadowania lub uzyskać dostęp do zasobów spowoduje błąd w czasie wykonywania i Aplikacja ulegnie awarii.

Ponadto ważne jest, aby pamiętać, że podczas Android obsługuje tylko małe litery w nazwach plików dla elementów zasobów, platformy Xamarin.Android jest nieco więcej forgiving; jej będzie obsługiwać zarówno wielkie i małe litery nazw plików. Konwencja nazw obraz jest za pomocą małe znaki podkreślenia jako separatorów (na przykład **Moje\_obraz\_name.png**). Należy zauważyć, że nazwy zasobów nie można przetworzyć, jeśli kresek ani spacji, które są używane jako separatorów.

Gdy zasoby zostały dodane do projektu, istnieją dwa sposoby użycia ich w aplikacji &ndash; programowo (wewnątrz kodu) lub z plików XML.


## <a name="referencing-resources-programmatically"></a>Programowe odwoływanie się do zasobów

Aby uzyskać dostęp do tych plików programowo, są przypisane identyfikator unikatowy zasób Tego Identyfikatora zasobu jest liczbą całkowitą, zdefiniowane w specjalnych klasę o nazwie `Resource`, który znajduje się w pliku **Resource.designer.cs**, i wygląda następująco:

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

Każdy identyfikator zasobu znajduje się wewnątrz klasy zagnieżdżonej, który odnosi się do typu zasobu. Na przykład, gdy plik **Icon.png** został dodany do projektu, zaktualizowany Xamarin.Android `Resource` klasy, tworząc klasę zagnieżdżoną o nazwie `Drawable` z wewnątrz stałą o nazwie `Icon`.
Dzięki temu plik **Icon.png** można się odwoływać w kodzie jako `Resource.Drawable.Icon`. `Resource` Klasy nie należy ręcznie edytować, ponieważ wszelkie zmiany, które zostały wprowadzone zostaną zastąpione przez rozszerzenie Xamarin.Android.

Podczas odwoływania się do zasobów programowo (kod), są one dostępne za pośrednictwem hierarchii klas zasobów, której używa następującej składni:

```xml
@[<PackageName>.]Resource.<ResourceType>.<ResourceName>
```

-  **Nazwa pakietu** &ndash; wymagany pakiet, który dostarcza zasobu i jest tylko wtedy, gdy są używane zasoby z innymi pakietami.

-  **Typ zasobu** &ndash; to typu zagnieżdżonego zasobu, który jest w ramach klasy zasobów opisanych powyżej.

-  **Nazwa zasobu** &ndash; jest to nazwa pliku zasobu (bez rozszerzenia) lub wartość atrybutu systemu android: name dla zasobów, które znajdują się w elemencie XML.


## <a name="referencing-resources-from-xml"></a>Odwołuje się do zasobów z pliku XML

Zasoby w pliku XML są dostępne dla następujących specjalnej składni:

```xml
@[<PackageName>:]<ResourceType>/<ResourceName>.
```

-  **Nazwa pakietu** &ndash; wymagany pakiet, który dostarcza zasobu i jest tylko wtedy, gdy są używane zasoby z innymi pakietami.

-  **Typ zasobu** &ndash; to typu zagnieżdżonego zasobu, który jest w ramach klasy zasobów.

-  **Nazwa zasobu** &ndash; to nazwa pliku zasobu (*bez* rozszerzenie typu plików) lub wartość `android:name` atrybutu dla zasobów, które znajdują się w elemencie XML.

Na przykład zawartość pliku układu **Main.axml**, są następujące:

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

W tym przykładzie ma [ `ImageView` ](https://github.com/xamarin/recipes/tree/master/Recipes/android/controls/imageview) wymagającym drawable zasób o nazwie **flagi**. `ImageView` Ma jego `src` ustawioną wartość atrybutu **@drawable/flag**. Po uruchomieniu działania systemu Android będzie wyglądać wewnątrz katalogu **zasobów/Drawable** dla pliku o nazwie **flag.png** (rozszerzenie pliku może być inny format obrazu, takie jak **flag.jpg**) ładowanie tego pliku i wyświetlić ją w `ImageView`.
Po uruchomieniu ta aplikacja wyglądałaby podobną do poniższej ilustracji:

![Zlokalizowane ImageView](android-resource-basics-images/03-localized-screenshot.png)

