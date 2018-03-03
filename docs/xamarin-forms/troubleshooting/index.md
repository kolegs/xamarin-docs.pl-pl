---
title: "Rozwiązywanie problemów"
description: "Typowe warunki błędów i sposobu ich rozwiązania"
ms.topic: article
ms.prod: xamarin
ms.assetid: 63291951-7375-4CBF-BCC3-2E4AD157A2C8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: 23ebefcbd6114b06c39740b3b56f87aeac0b9a00
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="troubleshooting"></a>Rozwiązywanie problemów

_Typowe warunki błędów i sposobu ich rozwiązania_

## <a name="error-unable-to-find-a-version-of-xamarinforms-compatible-with"></a>Błąd: "nie można znaleźć zgodny z wersją platformy Xamarin.Forms..."

Następujące błędy mogą występować w **konsoli pakietów** okno podczas aktualizowania wszystkich pakietów Nuget w rozwiązaniu platformy Xamarin.Forms lub w projekcie aplikacji platformy Xamarin.Forms Android:

```csharp
Attempting to resolve dependency 'Xamarin.Android.Support.v7.AppCompat (= 23.3.0.0)'.
Attempting to resolve dependency 'Xamarin.Android.Support.v4 (= 23.3.0.0)'.
Looking for updates for 'Xamarin.Android.Support.v7.MediaRouter'...
Updating 'Xamarin.Android.Support.v7.MediaRouter' from version '23.3.0.0' to '23.3.1.0' in project 'Todo.Droid'.
Updating 'Xamarin.Android.Support.v7.MediaRouter 23.3.0.0' to 'Xamarin.Android.Support.v7.MediaRouter 23.3.1.0' failed.
Unable to find a version of 'Xamarin.Forms' that is compatible with 'Xamarin.Android.Support.v7.MediaRouter 23.3.0.0'.
```

### <a name="what-causes-this-error"></a>Co powoduje, że ten błąd?

Visual Studio for Mac (lub programu Visual Studio) może wskazywać, że są dostępne aktualizacje dla platformy Xamarin.Forms Nuget packge *i jego zależności*. W programie Xamarin Studio, rozwiązania firmy **pakiety** węzeł może wyglądać następująco (numery wersji mogą się różnić):

![](images/updates-available.png "Folder pakiety aplikacji systemu Android")

Ten błąd może wystąpić, jeśli próba aktualizacji _wszystkie_ pakiety.

Wynika to z faktu z Android ustawienia projektów do wersji docelowej/kompilacji z systemem Android 6.0 (23 interfejsu API) lub poniżej, ma zależność twardych platformy Xamarin.Forms na *określonych* wersji Android obsługuje pakietów. Mimo że zaktualizowane wersje tych pakietów mogą być dostępne, platformy Xamarin.Forms nie jest zawsze zgodny z nimi.

W takim przypadku należy zaktualizować _tylko_ **platformy Xamarin.Forms** pakietu, ponieważ pozwala to zagwarantować pozostawienie zależności w wersjach zgodne. Inne pakiety, które zostały dodane do projektu może również zaktualizować indywidualnie tak długo, jak nie powodują pakietów Obsługa systemu Android do aktualizacji.


> [!NOTE]
> Jeśli używasz platformy Xamarin.Forms 2.3.4 lub nowszej **i** ustawiono wersji docelowej/kompilacji projektu systemu Android 7.0 dla systemu Android (interfejs API 24) lub nowszy, a następnie Zastosuj zależności twardych wymienionych powyżej już i może zaktualizować pomocy technicznej pakiety niezależnie od pakietu platformy Xamarin.Forms.


### <a name="fix-remove-all-packages-and-re-add-xamarinforms"></a>Poprawka: Usuń wszystkie pakiety i ponownie dodać platformy Xamarin.Forms

Jeśli **Xamarin.Android.Support** pakiety zostały uaktualnione do wersji niezgodny, jest najprostsza:

1. Następnie ręcznie usuń wszystkie pakiety Nuget w projekcie systemu Android
2. Dodaj ponownie **platformy Xamarin.Forms** pakietu.

To będzie automatycznie pobierał *poprawne* wersje innych pakietów.

Aby wyświetlić wideo tego procesu, odwoływać się do tego [post fora](https://forums.xamarin.com/discussion/comment/170012/#Comment_170012).
