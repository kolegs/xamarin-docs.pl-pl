---
title: DataPages zestawu narzędzi Xamarin.Forms
description: W tym artykule przedstawiono Xamarin.Forms DataPages, który dostarczać interfejs API, aby szybko i łatwo powiązać gotowych widoków źródła danych.
ms.prod: xamarin
ms.assetid: DF16EAEE-DB78-42CA-9C59-51D9D6CB6B95
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 2a74b636a41a72b26776157a774f0a33ef45a075
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38815890"
---
# <a name="xamarinforms-datapages"></a>DataPages zestawu narzędzi Xamarin.Forms

![](~/media/shared/preview.png "Ten interfejs API jest obecnie w wersji zapoznawczej")

> [!IMPORTANT]
> Wymaga DataPages [motyw Xamarin.Forms](~/xamarin-forms/user-interface/themes/index.md) odwołania do renderowania.

Xamarin.Forms DataPages zostały ogłoszone w 2016 rozwijających i są dostępne w wersji zapoznawczej dla klientów i spróbuj przekazać opinię.

DataPages dostarczać interfejs API, szybko i łatwo źródła danych można powiązać gotowych widoków. Elementy listy i na stronach szczegółów automatycznie spowoduje, że dane, a następnie można dostosować za pomocą motywów.

Aby zobaczyć, jak działa rozwijających pokaz prelekcji, zapoznaj się z [przewodnik wprowadzenie](get-started.md).

[![](images/demo-sml.png "Aplikacja przykładowa DataPages")](images/demo.png#lightbox "DataPages przykładowej aplikacji")

## <a name="introduction"></a>Wprowadzenie

Źródła danych i powiązane dane strony umożliwiają deweloperom szybko i łatwo korzystać z obsługiwanego źródła danych i renderować ją za pomocą wbudowanych, które można dostosować interfejsu użytkownika tworzenia szkieletów, które z motywami.

DataPages są dodawane do aplikacji platformy Xamarin.Forms, umieszczając **Xamarin.Forms.Pages** pakietu Nuget.

### <a name="data-sources"></a>Źródła danych

Korzystania z wersji zapoznawczej zawiera niektóre źródła danych wbudowanych, dostępna do użycia:

* **JsonDataSource**
* **AzureDataSource** (oddziel Nuget)
* **AzureEasyTableDataSource** (oddziel Nuget)

Zobacz [przewodnik wprowadzenie](get-started.md) dla przykłady dotyczące używania `JsonDataSource`.


### <a name="pages--controls"></a>Stron i kontrolek

Następujące strony i formanty są uwzględniane umożliwia łatwe powiązanie do źródeł danych:

* **ListDataPage** — zobacz [wprowadzenie do przykładu](get-started.md).
* **DirectoryPage** — lista grupowanie włączone.
* **PersonDetailPage** — danych jednego elementu widoku dostosowane do określonego typu obiektu (wpis kontaktu).
* **DataView** — widok, który chcesz uwidocznić dane ze źródła w ogólny sposób.
* **CardView** — różne widok, który zawiera obraz, tytuł i tekst opisu.
* **Duży obraz** — widok renderowanie obrazu.
* **ListItem** — wstępnie skompilowane widok Układ podobny do natywnych dla systemów iOS i Android wyświetlanie listy elementów.

Zobacz [dokumentacja formantów DataPages](controls.md) przykłady.



### <a name="under-the-hood"></a>Kulisy

Źródło danych zestawu narzędzi Xamarin.Forms działa zgodnie z `IDataSource` interfejsu.

Infrastruktura platformy Xamarin.Forms wchodzi w interakcję ze źródłem danych za pomocą następujących właściwości:

* `Data` — tylko do odczytu listę elementów danych, które mogą być wyświetlane.
* `IsLoading` — wartość boolean wskazującą, czy dane są ładowane i dostępne do renderowania.
* `[key]` — indeksator można pobrać elementów.

Istnieją dwie metody `MaskKey` i `UnmaskKey` który może służyć do ukrycia (lub pokazuj) właściwości elementu danych (tj. uniemożliwić ich renderowanego).
Klucz odnosi się do nazwanej właściwości w obiekcie elementu danych.
