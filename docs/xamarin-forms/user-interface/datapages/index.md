---
title: DataPages platformy Xamarin.Forms
description: W tym artykule przedstawiono platformy Xamarin.Forms DataPages, które zapewniają interfejs API, aby szybko i łatwo powiązać wbudowanych widoków źródła danych.
ms.prod: xamarin
ms.assetid: DF16EAEE-DB78-42CA-9C59-51D9D6CB6B95
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 2a74b636a41a72b26776157a774f0a33ef45a075
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35243333"
---
# <a name="xamarinforms-datapages"></a>DataPages platformy Xamarin.Forms

![](~/media/shared/preview.png "Ten interfejs API jest obecnie w wersji zapoznawczej")

> [!IMPORTANT]
> Wymaga DataPages [motyw platformy Xamarin.Forms](~/xamarin-forms/user-interface/themes/index.md) odwołania do renderowania.

DataPages platformy Xamarin.Forms zostały ogłoszenia na Evolve 2016 i są dostępne jako podglądu dla klientów i spróbuj przekazać opinię.

DataPages dostarczać interfejs API do szybkiego i łatwego powiązania źródła danych do wbudowanych widoków. Elementy listy i strony szczegółów automatycznie spowoduje, że dane, a można dostosować za pomocą motywów.

Aby zobaczyć, jak działa pokaz prelekcji Evolve, zapoznaj się [Wprowadzenie — przewodnik](get-started.md).

[![](images/demo-sml.png "Aplikacja przykładowa DataPages")](images/demo.png#lightbox "DataPages przykładowej aplikacji")

## <a name="introduction"></a>Wprowadzenie

Źródła danych i skojarzone dane strony umożliwiają deweloperom szybko i łatwo korzystać z obsługiwanego źródła danych i renderować ją przy użyciu interfejsu użytkownika tworzenia szkieletu, który można dostosować wbudowana w usłudze motywów.

DataPages są dodawane do aplikacji platformy Xamarin.Forms przy tym **Xamarin.Forms.Pages** pakietu Nuget.

### <a name="data-sources"></a>Źródła danych

Wersja zapoznawcza ma kilka źródeł danych wbudowane dostępne do użycia:

* **JsonDataSource**
* **AzureDataSource** (Rozdziel Nuget)
* **AzureEasyTableDataSource** (Rozdziel Nuget)

Zobacz [Wprowadzenie — przewodnik](get-started.md) na przykład za pomocą `JsonDataSource`.


### <a name="pages--controls"></a>Strony i kontrolki

Zawarte umożliwia łatwe powiązanie ze źródłami danych są następujące strony i kontrolki:

* **ListDataPage** — zobacz [wprowadzenie przykład](get-started.md).
* **DirectoryPage** — lista grupowanie włączone.
* **PersonDetailPage** — element danych pojedynczego widoku dostosowanych do określonego typu obiektu (wpis kontaktu).
* **Element DataView** — widok do udostępniania danych ze źródła w sposób ogólny.
* **CardView** — styl widoku, który zawiera obraz, tytuł i opis.
* **HeroImage** — widok renderowania obrazu.
* **ListItem** — wbudowana w widoku z układem podobne do natywnej dla systemu iOS i Android listy elementów.

Zobacz [DataPages kontrolki odwołania](controls.md) przykłady.



### <a name="under-the-hood"></a>Kulisy

Źródło danych platformy Xamarin.Forms działa zgodnie z `IDataSource` interfejsu.

Infrastruktura platformy Xamarin.Forms współdziała ze źródłem danych za pośrednictwem następujących właściwości:

* `Data` — listę elementów danych, które mogą być wyświetlane tylko do odczytu.
* `IsLoading` — wartość boolean wskazującą, czy dane są załadowane i dostępne do renderowania.
* `[key]` — indeksator można pobrać elementów.

Istnieją dwie metody `MaskKey` i `UnmaskKey` który może służyć do (lub ukryć) właściwości elementu danych (tj. uniemożliwić ich renderowanego).
Klucz odnosi się do nazwanej właściwości w obiekcie elementu danych.
