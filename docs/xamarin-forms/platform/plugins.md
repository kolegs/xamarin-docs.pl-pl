---
title: Wykorzystywanie i tworzenie wtyczki zestawu narzędzi Xamarin.Forms
description: W tym artykule wyjaśniono, jak używanie i tworzenie wtyczki zestawu narzędzi Xamarin.Forms. Wtyczki są zwykle używane do można łatwo udostępnić funkcje platformy natywnej.
ms.prod: xamarin
ms.assetid: 8A06A420-A9D0-4BCB-B9AF-3AEA6A648A8B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/05/2018
ms.openlocfilehash: 4d121c2dfcca380e1735da1a4ca47c42d1957b8a
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37854743"
---
# <a name="consuming-and-creating-xamarinforms-plugins"></a>Wykorzystywanie i tworzenie wtyczki zestawu narzędzi Xamarin.Forms

Istnieje wiele funkcji platformy natywnej znajdujące się na wszystkich platformach, ale mają nieco inne interfejsy API. Jednym ze sposobów deweloperzy mogą korzystać z tych funkcji jest tworzenie abstrakcyjny interfejs dla wielu platform, a następnie implementacji interfejsu w różnych platform. Następnie aplikacja Xamarin.Forms uzyskuje dostęp do tych implementacji platformy przy użyciu [ `DependencyService` ](~/xamarin-forms/app-fundamentals/dependency-service/index.md).

Deweloperzy mogą udostępniać tę pracę, pisząc _wtyczki_ i publikując je w usłudze NuGet.

> [!NOTE]
> Wiele funkcji dla wielu platform wcześniej dostępne tylko za pomocą wtyczki są teraz częścią typu open-source **[Xamarin.Essentials](~/essentials/index.md)** biblioteki. Te funkcje obejmują: stan baterii, compass, czujników ruchu, geolokalizacja, zamiany tekstu na mowę i wiele innych. W przyszłości **Xamarin.Essentials** będzie podstawowym źródłem funkcje dla wielu platform aplikacji platformy Xamarin.Forms. Mimo że deweloperzy nadal tworzyć i publikować wtyczki, należy wziąć pod uwagę Współtworzenie **Xamarin.Essentials**.

## <a name="finding-and-adding-plugins"></a>Znajdowanie i dodawanie wtyczek

Społeczność platformy Xamarin, został utworzony, wiele międzyplatformowych wtyczek zgodne z zestawem narzędzi Xamarin.Forms. Duża kolekcja zasobów można znaleźć w folderze:

[**Xamarin Plugins**](https://github.com/xamarin/XamarinComponents)

Przewodnik dotyczący dodawania pakietów NuGet do projektu, zobacz nasze wskazówki na [dołączanie pakietu NuGet w projekcie](/visualstudio/mac/nuget-walkthrough/).

## <a name="creating-plugins"></a>Tworzenie wtyczek

Użytkownik może również tworzyć i publikować własne wtyczek jako pakiety Nuget (i składników platformy Xamarin). Wiele wtyczek istniejących są typu open source, więc możesz przejrzeć kod, aby zrozumieć, jak są one writtern.

Na przykład lista wtyczek poniżej są wszystkie typu open source i odnoszą się do niektórych przykładów w [ `DependencyService` ](~/xamarin-forms/app-fundamentals/dependency-service/index.md) sekcji:

- **Zamiana tekstu na mowę** przez James Montemagno &ndash; [GitHub](https://github.com/jamesmontemagno/TextToSpeechPlugin) i [NuGet  ](https://www.nuget.org/packages/Xam.Plugins.TextToSpeech)
- **Stan baterii** przez James Montemagno &ndash; [GitHub](https://github.com/jamesmontemagno/BatteryPlugin) i [NuGet](https://www.nuget.org/packages/Xam.Plugin.Battery)

Te projekty Github zapewniają dobry punkt wyjścia do tworzenia własnych międzyplatformowych wtyczek, jak te instrukcje dla [tworzenie wtyczkę dla platformy Xamarin](https://github.com/xamarin/XamarinComponents#create-a-plugin-for-xamarin).

### <a name="structuring-cross-platform-plugin-projects"></a>Tworzenie struktury projektów wtyczki dla wielu Platform

Mimo że nie istnieją żadne szczególne wymagania dotyczące projektowania pakietu NuGet, istnieją pewne wskazówki dotyczące tworzenia pakietu dla aplikacji dla wielu platform.

W przeszłości wtyczki dla wielu platform zazwyczaj składa się z następujących składników:

- PCL przy użyciu interfejsu, który reprezentuje interfejs API dla wtyczki,
- dla systemu iOS, Android i Windows platformy Uniwersalnej klasy biblioteki z implementacją interfejsu.

Odczyt James Montemagno [wpis w blogu](https://blog.xamarin.com/creating-reusable-plugins-for-xamarin-forms/) opisujące proces tworzenia wtyczki dla platformy Xamarin.

Niedawno wtyczek można można utworzyć za pomocą pojedynczej platformy docelowe wielu. To podejście zostało omówione w James Montemagno [wpis w blogu](https://montemagno.com/converting-xamarin-libraries-to-sdk-style-multi-targeted-projects/). Ta metoda jest stosowana w wtyczek James Montemagno linki umieszczono powyżej i jest również format używany w **Xamarin.Essentials**.

Zaleca się unikać odwoływania się do zestawu narzędzi Xamarin.Forms bezpośrednio z poziomu dodatku typu plug-in.
Można utworzyć konfliktu wersji, gdy inni deweloperzy próbują użyć wtyczki. Zamiast tego spróbuj projektowania interfejsu API, dzięki czemu mogą być używane przez dowolną aplikację platformy Xamarin i .NET.

### <a name="publishing-nuget-packages"></a>Publikowanie pakietów NuGet

Pakiety NuGet ma **nuspec** pliku, który jest plikiem xml, który definiuje części projektu, które są publikowane w pakiecie. **Nuspec** plik zawiera także informacje o pakiecie, takie jak identyfikator, tytuł i autorów.

Zobacz [dokumentacji NuGet](/nuget/create-packages/creating-a-package.md) Aby uzyskać więcej informacji na temat tworzenia i publikowania pakietów NuGet.

## <a name="related-links"></a>Linki pokrewne

- [Tworzenie wtyczek wielokrotnego użytku dla zestawu narzędzi Xamarin.Forms](https://blog.xamarin.com/creating-reusable-plugins-for-xamarin-forms)
- [Przy użyciu & opracowywanie wtyczek dla platformy Xamarin (wideo)](https://university.xamarin.com/guestlectures/using-developing-plugins-for-xamarin)
