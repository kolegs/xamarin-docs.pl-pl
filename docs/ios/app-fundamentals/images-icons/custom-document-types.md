---
title: Ikony niestandardowy dokument
description: "Ten artykuł obejmuje tym i zarządzanie zasób obrazu w aplikacji platformy Xamarin.iOS do użycia jako ikona niestandardowy typ dokumentu."
ms.topic: article
ms.prod: xamarin
ms.assetid: 7A3F3C94-2578-4F53-9B8E-25714F48BDD6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/23/2017
ms.openlocfilehash: e14cfb8d3c09d17bdee4b60786f434ff94ef31dc
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="custom-document-icons"></a>Ikony niestandardowy dokument

_Ten artykuł obejmuje tym i zarządzanie zasób obrazu w aplikacji platformy Xamarin.iOS do użycia jako ikona niestandardowy typ dokumentu._

W przypadku aplikacji platformy Xamarin.iOS obsługuje ładowania typu określonego dokumentu, deweloper może zapewnić ikony, które będą używane przez system po napotkaniu tego typu dokumentu, na przykład gdy użytkownik posiada dół załącznik w *poczcie* jako przedstawione tutaj:

 [![](custom-document-types-images/17.png "Przykład ikony typu dokumentu")](custom-document-types-images/17.png#lightbox)

Deweloper można dodać informacji o typie dokumentu formacie pliku aplikacji jest w stanie otwarcia przez dołączenie wpisy słownika dla `CFBundleTypeName` ciągu i `LSItemContentTypes` tablicy w aplikacji `Info.plist`. Ikony typu dokumentu go w programie `CFBundleTypeIconFiles` tablicy. Jeśli ikona dokumentu nie jest dostarczane z systemem iOS będzie pochodzić z ikonę aplikacji.
W przypadku kilku rozmiarów zoptymalizowane dla różnych rozdzielczości urządzenia mogą być dostarczane ikon. 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Aby przypisać te wartości w programie Visual Studio dla komputerów Mac, należy użyć **typów dokumentów** w obszarze **zaawansowane** karcie na `Info.plist` edytora, aby dodać typ dokumentu i przypisać do obrazu ikony. Na przykład poniżej przedstawiono zrzut ekranu przedstawiający rejestracji do obsługi plików PDF:

 [![](custom-document-types-images/18.png "W sekcji typów dokumentów, na karcie Zaawansowane w edytorze "Info.plist"")](custom-document-types-images/18.png#lightbox)
 
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Aby przypisać te wartości w programie Visual Studio, użyj **typów dokumentów** w obszarze **zaawansowane** karcie na `Info.plist`:

 ![](custom-document-types-images/doc01w.png "Otwórz sekcję typów dokumentów na karcie Zaawansowane")

Kliknij przycisk **Dodawanie typu dokumentu** przycisk, a następnie wypełnij wymagane pola:

![](custom-document-types-images/doc02w.png "Formularz Dodawanie typu dokumentu")

-----


Aby uzyskać więcej informacji na temat typów dokumentu, zobacz firmy Apple [Uniform odwołanie do typu identyfikatory](http://developer.apple.com/library/ios/#documentation/Miscellaneous/Reference/UTIRef/Articles/System-DeclaredUniformTypeIdentifiers.html) i [dokumentu interakcji programowania tematy dla systemu iOS](http://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Introduction/Introduction.html).


## <a name="related-links"></a>Linki pokrewne

- [Praca z obrazami (przykład)](https://developer.xamarin.com/samples/WorkingWithImages/)
- [Witaj, iPhone](~/ios/get-started/hello-ios/index.md)
- [Niestandardowe ikony, jak i wskazówki dotyczące tworzenia obrazu](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html)
