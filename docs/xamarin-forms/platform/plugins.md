---
title: Dodatki plug-in
description: Łatwe dodawanie natywnej funkcji do aplikacji platformy Xamarin.Forms
ms.prod: xamarin
ms.assetid: 8A06A420-A9D0-4BCB-B9AF-3AEA6A648A8B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/07/2016
ms.openlocfilehash: 5770d13c46998872752820b7a0cbb222a04c3ff8
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="plugins"></a>Dodatki plug-in

Istnieje wiele funkcji natywnego platformy, które istnieją na wszystkich platformach, ale ma nieco inne interfejsy API. Deweloperzy zapisu wtyczek do utworzenia abstrakcyjny międzyplatformowego interfejsu dla tych funkcji, które mogą również współużytkować z innymi osobami.

Te funkcje obejmują: stan baterii, kompas czujników ruchu, geolokalizacja, tekst na mowę i wiele innych. Dodatki plug-in umożliwiają te funkcje można łatwo uzyskać dostępu do aplikacji platformy Xamarin.Forms.

## <a name="finding-and-adding-plugins"></a>Znajdowanie i dodawanie wtyczek

Społeczność Xamarin utworzył wielu wtyczek i platform zgodne z platformy Xamarin.Forms - dużych kolekcji znajduje się w temacie:

[**Xamarin Plugins**](https://github.com/xamarin/plugins)

Przewodnik dotyczący Dodawanie pakietów NuGet do projektu, zobacz nasze wskazówki na [tym pakietu NuGet w projekcie](/visualstudio/mac/nuget-walkthrough/).


## <a name="creating-plugins"></a>Tworzenie wtyczek

Istnieje również możliwość utworzenia i opublikuj własne wtyczek pakiety Nuget (i składników Xamarin). Wiele wtyczek istniejących są open source, możesz przejrzeć swój kod, aby zrozumieć, jak są one writtern.

Na przykład na liście poniżej dodatków plug-in są wszystkie typu open source, a odpowiadają one próbek w [ `DependencyService` ](~/xamarin-forms/app-fundamentals/dependency-service/index.md) sekcji:

- **Tekst na mowę** przez James Montemagno &ndash; [GitHub](https://github.com/jamesmontemagno/Xamarin.Plugins/tree/master/TextToSpeech) i [NuGet](https://www.nuget.org/packages/Xam.Plugin.Battery)
- **Stan baterii** przez James Montemagno &ndash; [GitHub](https://github.com/jamesmontemagno/Xamarin.Plugins/tree/master/Battery) i [NuGet](https://www.nuget.org/packages/Xam.Plugins.TextToSpeech/)

Te projekty Github mogła zapewniać dobry punkt wyjścia do tworzenia własnych wtyczek i platform, wykonaj te instrukcje dotyczące [tworzenie wtyczki dla platformy Xamarin](https://github.com/xamarin/plugins#create-a-plugin-for-xamarin).

### <a name="structuring-cross-platform-plugin-projects"></a>Struktury projektów dodatek obsługujący wiele Platform

Choć nie ma określonego wymagań dotyczących projektowania pakietu NuGet, istnieją pewne wskazówki dotyczące tworzenia pakietu dla aplikacji i platform.

Dodatek obsługujący wiele platform zazwyczaj powinien składać się z następujących składników:

- PCL z interfejs, który reprezentuje interfejsu API dla wtyczki,
- iOS, Android i Windows klasy biblioteki z implementacją interfejsu.

Odczyt Kuba Montemagno [wpis w blogu](https://blog.xamarin.com/creating-reusable-plugins-for-xamarin-forms/) opisujące proces tworzenia wtyczek xamarin.

Zaleca się unikać odwołujące się do platformy Xamarin.Forms bezpośrednio z wtyczki.
Można utworzyć konfliktu wersji, gdy inni deweloperzy próba użycia wtyczki. Zamiast tego spróbuj projekt interfejsu API, aby mogą być używane przez dowolną aplikację platformy Xamarin lub .NET.

### <a name="publishing-nuget-packages"></a>Publikowanie pakietów NuGet

Pakiety NuGet zostały **nuspec** pliku, który jest plik xml, który definiuje, które części projektu są publikowane w pakiecie. **Nuspec** plik zawiera także informacje o pakiecie, takich jak identyfikator, tytuł i autorów.

Zobacz [dokumentacji NuGet](http://docs.nuget.org/create/creating-and-publishing-a-package) Aby uzyskać więcej informacji na temat tworzenia i publikowania pakietów NuGet.


## <a name="related-links"></a>Linki pokrewne

- [Tworzenie wtyczek wielokrotnego użytku dla platformy Xamarin.Forms](https://blog.xamarin.com/creating-reusable-plugins-for-xamarin-forms)
- [Tworząc & Tworzenie wtyczek xamarin (klip wideo)](https://university.xamarin.com/guestlectures/using-developing-plugins-for-xamarin)
