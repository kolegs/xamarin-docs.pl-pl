---
title: "Do nowszej pakiet NuGet można zaktualizować domyślnego szablonu platformy Xamarin.Forms?"
ms.topic: article
ms.prod: xamarin
ms.assetid: 160FBE13-26EB-4B4F-9248-A5CBE58FDD7F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: c626e8b4a01a55fac5d2c07f0c511241056c2774
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
---
# <a name="can-i-update-the-xamarinforms-default-template-to-a-newer-nuget-package"></a>Do nowszej pakiet NuGet można zaktualizować domyślnego szablonu platformy Xamarin.Forms?

W tym przewodniku używa szablonu platformy Xamarin.Forms PCL, na przykład, ale tej samej metody ogólne również będzie działać dla szablonu projektu udostępnionego platformy Xamarin.Forms. W tym przewodniku są zapisywane z przykładem aktualizacji z platformy Xamarin.Forms 1.5.1.6471 do 2.1.0.6529, ale można ustawić inne wersje domyślnie zamiast tego są te same kroki.

1.  Skopiuj oryginalnego szablonu `.zip` od:

    > `C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Xamarin\Xamarin\[Xamarin Version]\T\PT\Cross-Platform\Xamarin.Forms.PCL.zip`

2.  Rozpakuj `.zip` do tymczasowej lokalizacji.

3.  Zmień wszystkie wystąpienia starą wersję pakietu formularzy do nowej wersji, którego chcesz użyć.
    *   `FormsTemplate\FormsTemplate.vstemplate`
    *   `FormsTemplate.Android\FormsTemplate.Android.vstemplate`
    *   `FormsTemplate.WinPhone\FormsTemplate.WinPhone.vstemplate`
    *   `FormsTemplate.iOS\FormsTemplate.iOS.vstemplate`

    Przykład: `<package id="Xamarin.Forms" version="1.5.1.6471" />` -> `<package id="Xamarin.Forms" version="2.1.0.6529" />`

4.  Zmienianie elementu "name" głównego [pliku szablonu wielu projektów](http://msdn.microsoft.com/library/ms185308.aspx) (`Xamarin.Forms.PCL.vstemplate`) go. Na przykład:
    > <Name>Pusta aplikacja (przenośna platformy Xamarin.Forms) - 2.1.0.6529</Name>

5.  Ponownie zip szablonu całego folderu. Upewnij się, że odpowiada oryginalnej struktury plików `.zip` pliku. `Xamarin.Forms.PCL.vstemplate` Plik powinien być w górnej części `.zip` pliku w foldery.

6.  Utwórz podkatalogu "Mobile Apps" w folderze Szablony programu Visual Studio na użytkownika:
    > `%USERPROFILE%\Documents\Visual Studio 2013\Templates\ProjectTemplates\Visual C#\Mobile Apps`

7.  Skopiuj folder szablonów zip w górę do nowego katalogu "Mobile Apps".

8.  Pobierz pakiet NuGet, która jest zgodna z wersją w kroku 3. Na przykład [http://nuget.org/api/v2/package/Xamarin.Forms/2.1.0.6529](http://nuget.org/api/v2/package/Xamarin.Forms/2.1.0.6529) (Zobacz też [http://stackoverflow.com/questions/8597375/how-to-get-the-url-of-a-nupkg-file](http://stackoverflow.com/questions/8597375/how-to-get-the-url-of-a-nupkg-file)) i skopiuj go do odpowiednim podfolderze folderu rozszerzeń programu Visual Studio Xamarin:
    > `C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Xamarin\Xamarin\[Xamarin Version]\Packages`
