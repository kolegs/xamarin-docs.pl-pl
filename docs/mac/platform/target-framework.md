---
title: Platforma docelowa
description: "W tym artykule omówiono struktur docelowych (bibliotek klas Base) dostępnych dla Xamarin.Mac i zagadnień dotyczących używania ich w projekcie Xamarin.Mac."
ms.topic: article
ms.prod: xamarin
ms.assetid: AF21BE16-3F92-4121-AB4C-D51AC863D92D
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 11/10/2017
ms.openlocfilehash: ac4644f65486d70fcbb7da1a03574fb238348313
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="target-framework"></a>Platforma docelowa

_W tym artykule omówiono struktur docelowych (bibliotek klas Base) dostępnych dla Xamarin.Mac i zagadnień dotyczących używania ich w projekcie Xamarin.Mac._

![TARGET — opcje framework dla Xamarin.Mac](target-framework-images/select-target.png "Target framework opcje Xamarin.Mac")

## <a name="background"></a>Tło

Co .NET program lub biblioteki zależy od funkcji udostępnianych przez biblioteki klasy podstawowej (BCL). Ta BCL zawiera zestawy takich jak mscorlib, System System.Net.Http i zestawów System.Xml, które zapewniają typowe funkcje wbudowane wszystkich języków .NET.

Całościowo zostały opracowane wiele różnych wersji tego BCL zoptymalizowane pod kątem innych przypadków użycia. "Pulpitu" BCL obejmuje bogatszy zestaw bibliotek, które może być zbyt ciężki dla innych przypadków użycia, podczas gdy mobile koncentruje się na zapewnienie, że interfejsy API są bezpieczne dla połączeń, co spowoduje usunięcie nieużywanych kod, aby ograniczyć wpływ aplikacji.

Co ważniejsze następstwa docelowych tych różnych platform, jest to, że wszystkie zestawy w programie danego *musi* target zestawy BCL zgodne. Jeśli to się nie stało, może mieć dwa zestawy połączonych z różnymi wersjami **System.dll** disagreeing o podpisie danego typu. Biblioteki udostępnionej można miejsca docelowego [.NET Standard 2](https://blog.xamarin.com/share-code-net-standard-2-0/), co jest typowe podzestaw docelowych platform lub platformy docelowej określonej.

Dostępne są trzy opcje platformy docelowej dla Xamarin.Mac, każda z różnych zalety i wady i zalety:

- **Nowoczesne** (nazywane Mobile w dokumentacji starsze) — podzbiór bardzo podobne, jakie uprawnienia Xamarin.iOS, wysoce dostosowanych wydajności i rozmiaru. Ta platforma docelowa jest konsolidatora bezpieczne, więc te projekty mogą się ich rozmiaru końcowego znacząco zredukowane przez usunięcie nieużywanych kodu.

- **Pełna** (nazywane XM 4.5 w dokumentacji starsze) — podzbiór bardzo podobne do "pulpitu" BCL, z kilku małych usuwania. Platforma docelowa jest niemal identyczny net45 (i nowszych), go łatwo korzystać z wielu nugets, które nie udostępniają albo netstandard2 lub Xamarin.Mac określonej kompilacji. Jednak z powodu użycia System.Configuration jest niezgodny z połączeń.

- **Nieobsługiwany** (nazywane systemu w dokumentacji starsze) — zamiast tego łączenia z BCL dostarczonych przez Xamarin.Mac, Użyj bieżącego mono zainstalowanego systemu. Zapewnia to największym zbiór zestawów, w tym niektóre znane problemy (System.Drawing na przykład). Ta opcja istnieje tylko ma "ostateczność" i zdecydowanie zalecane jest w pełni inne opcje przed jego użyciem. Jak wskazuje nazwa, użycia jest nieobsługiwany przez kanały oficjalnego pomocy technicznej.

## <a name="setting-the-target-framework"></a>Ustawienie platformy docelowej

Aby zmienić typ platformy docelowej dla projektu Xamarin.Mac, wykonaj następujące czynności:

1. Otwórz projekt Xamarin.Mac w programie Visual Studio dla komputerów Mac.
2. W **Eksploratora rozwiązań**, kliknij dwukrotnie plik projektu, aby otworzyć **opcje projektu** okno dialogowe.
3. Z **ogólne** , a następnie wybierz typ **platformy docelowej** które odpowiada potrzebom aplikacji:

  [![Za pomocą okna Opcje projektu, aby wybrać platformę docelową](target-framework-images/select-target-full.png "za pomocą okna Opcje projektu, aby wybrać platformy docelowej")](target-framework-images/select-target-full-large.png)

4. Kliknij przycisk **OK** przycisk, aby zapisać zmiany.

Należy **wyczyść** , a następnie **odbudować** Xamarin.Mac projektu po zmianie typu platformy docelowej.

## <a name="summary"></a>Podsumowanie

W tym artykule ma krótko opisano różne typy struktur docelowych (bibliotek klas Base) dostępnych do aplikacji Xamarin.Mac i każdy typ framework powinien być używany.


## <a name="related-links"></a>Linki pokrewne

- [iOS i Mac udostępniania kodu](~/cross-platform/macios/index.md)
- [Ujednolicony interfejs API](~/cross-platform/macios/unified/index.md)
- [Biblioteki klas przenośnych](~/cross-platform/app-fundamentals/pcl.md)
- [Zestawy](~/cross-platform/internals/available-assemblies.md)
- [Aktualizowanie istniejącej aplikacji Mac](~/cross-platform/macios/unified/updating-mac-apps.md)
