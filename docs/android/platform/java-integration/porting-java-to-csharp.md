---
title: Eksportowanie Java na język C#
description: Trzecia opcja dla przy użyciu języka Java w aplikacji platformy Xamarin.Android jest port kodu źródłowego języka Java na język C#.
ms.prod: xamarin
ms.assetid: 39E528BD-010F-47FC-BE48-8E7848E30454
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/05/2016
ms.openlocfilehash: c2d05b101c627dab42dc1343eab2a408d1bd010f
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
ms.locfileid: "30762600"
---
# <a name="porting-java-to-c"></a>Eksportowanie Java na język C#

_Trzecia opcja dla przy użyciu języka Java w aplikacji platformy Xamarin.Android jest port kodu źródłowego języka Java na język C#._

## <a name="overview"></a>Omówienie

Takie podejście mogą być przydatne do organizacji, które:

-  **Zagadnienia dotyczące stosy technologii Java na język C#.**
-  **Należy zachować C# i Java w wersji tego samego produktu.**
-  **Chcesz, aby .NET w wersji popularnych biblioteka języka Java.**


Istnieją dwa sposoby portu Java kodu dla C#. Pierwszym sposobem jest do portu kod ręcznie. Obejmuje to doświadczona deweloperów, którzy zrozumieć .NET i Java i zapoznaniu się z właściwego idioms dla każdego języka. Takie podejście najwygodniejszy w przypadku małej ilości kodu lub dla organizacji, które chcesz całkowicie przenieść poza Java C#.

Drugi metodologii przenoszenia jest spróbuj i automatyzacji procesu za pomocą konwertera kodu, takie jak [wyostrzania](https://github.com/mono/sharpen). [Wyostrzania](https://github.com/mono/sharpen) jest konwertera typu open source z Versant, która pierwotnie była używana do portu kod *db4o* za pomocą języka Java na język C#. db4o jest zorientowany obiektowo bazę danych Versant opracowane w języku Java, a następnie przenoszone do platformy .NET. Za pomocą konwertera kod może być uzasadniona dla projektów, które musi znajdować się w obu języków i wymagają niektórych parzystości między nimi.

Przykładem po sens narzędzie konwersji automatycznych kodu można wyświetlić w [ngit](https://github.com/mono/ngit) projektu.
Ngit jest port projektu języka Java [jgit](http://eclipse.org/).
Jgit sam jest implementacja Java [Git](http://git-scm.com/) system zarządzania kodu źródłowego. Aby wygenerować kod w języku C# za pomocą języka Java, programistów ngit, należy użyć niestandardowego systemu automatycznych do pobierania kodu Java z jgit, zastosuj niektóre poprawki, aby pomieścić proces konwersji, a następnie uruchom wyostrzania, które generuje kod C#. Dzięki temu projektu ngit, aby korzystać z stałego, trwającej pracy, która jest wykonywana na jgit.

Często jest związane z narzędzie konwersji kodu automatyczne uruchamianie nieuproszczony ilość pracy, a to mogą okazać się bariery do użycia. W wielu przypadkach może być prostszy i bardziej Java portu dla C# ręcznie.



## <a name="related-links"></a>Linki pokrewne

- [Wyostrzania narzędzie konwersji](https://github.com/mono/sharpen)
