---
title: Implementowanie odtwarzacza wideo
description: W tym artykule wyjaśniono, jak wdrożyć aplikację odtwarzacza wideo przy użyciu platformy Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 0CE9BEE7-4F81-4A00-B9B3-5E2535CD3050
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: 00697ca0adf3a34abec90c2f96d9fd9c273d06bb
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35239787"
---
# <a name="implementing-a-video-player"></a>Implementowanie odtwarzacza wideo

Czasami jest to pożądane do odtwarzania plików wideo w aplikacji platformy Xamarin.Forms. Ta seria artykuły omówiono sposób zapisu niestandardowe moduły renderowania dla systemu iOS, Android i Windows platformy Uniwersalnej o nazwie klasy platformy Xamarin.Forms `VideoPlayer`.

W [ **VideoPlayerDemos** ](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/) przykładowe, wszystkie pliki, które implementuje i obsługuje `VideoPlayer` znajdują się w folderach o nazwie `FormsVideoLibrary` i z przestrzenią nazw `FormsVideoLibrary` lub przestrzeni nazw się od `FormsVideoLibrary`. Ta organizacja i nazw powinna ułatwić skopiuj pliki odtwarzacza wideo do rozwiązania platformy Xamarin.Forms.

`VideoPlayer` odtwarzanie plików wideo z trzech typów źródeł:

- Z Internetem przy użyciu adresu URL
- Zasobu osadzonego w aplikacji platformy
- Urządzenia biblioteki wideo

Wymagaj odtwarzaczy wideo *transportu formanty*, które służą do odtwarzania i wstrzymanie wideo i rozmieszczania pasek, który będzie wyświetlany postęp za pośrednictwem wideo i umożliwia użytkownikowi szybko przejść do innej lokalizacji. `VideoPlayer` za pomocą formantów transportu i pozycjonowania pasek dostarczane przez platformę (jak pokazano poniżej), lub podać kontrolek niestandardowych transportu i pozycjonowania paska. Oto program do uruchamiania z systemem iOS, Android i platformy uniwersalnej systemu Windows:

[![Odtwarzanie wideo w sieci Web](web-videos-images/playwebvideo-small.png "odtwarzania wideo w sieci Web")](web-videos-images/playwebvideo-large.png#lightbox "odtwarzania wideo w sieci Web")

Oczywiście można włączyć phone bok większa w widoku.

Bardziej złożone odtwarzacza wideo musi dodatkowe funkcje, takie jak Regulacja głośności, mechanizmu przerwań wideo, gdy za pośrednictwem połączenia telefonicznego i nimi ekranu active podczas odtwarzania.

Serie następujące artykuły stopniowo przedstawia sposób renderowania platformy i klasy pomocnicze są wbudowane:

## <a name="creating-the-platform-video-playersplayer-creationmd"></a>[Tworzenie odtwarzaczy wideo platformy](player-creation.md)

Każdej z platform wymaga `VideoPlayerRenderer` klasy, która tworzy i obsługuje formantu odtwarzacza wideo obsługiwanego przez platformę. W tym artykule przedstawiono struktury programu renderującego klasy i tworzenia zawodników.

## <a name="playing-a-web-videoweb-videosmd"></a>[Odtwarzanie wideo w sieci Web](web-videos.md)

Najbardziej typowe źródła wideo dla odtwarzacza wideo jest prawdopodobnie Internet. W tym artykule opisano, jak można odwołania i używany jako źródło dla odtwarzacza wideo wideo w sieci Web.

## <a name="binding-video-sources-to-the-playersource-bindingsmd"></a>[Powiązania źródła wideo do odtwarzacza](source-bindings.md)

W tym artykule wykorzystano `ListView` do prezentowania kolekcji filmów wideo do odtwarzania. Jeden program pokazuje, jak plik CodeBehind można ustawić źródła wideo odtwarzacza wideo, ale drugiego programu pokazuje, jak używasz powiązanie między danych `ListView` i odtwarzacza wideo.

## <a name="loading-application-resource-videosloading-resourcesmd"></a>[Ładowanie aplikacji zasobów wideo](loading-resources.md)

Filmy wideo, można ją osadzić jako zasoby w projektach platformy. W tym artykule przedstawiono sposób przechowywania tych zasobów i później załadować je do programu do odtworzenia przez odtwarzacza wideo.

## <a name="accessing-the-devices-video-libraryaccessing-librarymd"></a>[Uzyskiwanie dostępu do biblioteki wideo urządzenia](accessing-library.md)

Po utworzeniu wideo przy użyciu aparatu fotograficznego urządzenia wideo plik jest przechowywany w bibliotece obrazów urządzenia. W tym artykule przedstawiono sposób dostępu selektora obrazu urządzenia wideo, a następnie odtwórz go przy użyciu odtwarzacza wideo.

## <a name="custom-video-transport-controlscustom-transportmd"></a>[Formanty niestandardowe transportu wideo](custom-transport.md)

Mimo że odtwarzaczy wideo na każdej platformie podać własne kontrolki transportu w formie przyciski **odtwarzanie** i **Wstrzymaj**, można zapobiec wyświetlaniu tych przycisków i podać własne. W tym artykule opisano sposób.

## <a name="custom-video-positioningcustom-positioningmd"></a>[Niestandardowe pozycjonowanie wideo](custom-positioning.md)

Każda odtwarzaczy wideo platformy ma pasek pozycji, który będzie wyświetlany postęp wideo i można przejść do przodu lub wstecz do określonej pozycji. W tym artykule przedstawiono, jak można zastąpić ten pasek pozycji kontrolkę niestandardową.





## <a name="related-links"></a>Linki pokrewne

- [Pokazy odtwarzacza wideo (przykład)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
