---
title: Tekst
description: "Aby wpisać lub wyświetlić tekst przy użyciu platformy Xamarin.Forms."
ms.topic: article
ms.prod: xamarin
ms.assetid: 4DBA7689-E5C8-4583-8FB4-02AB208B4416
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: 55d13589e4241e9f4e29aea9a55346a8f514f208
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/12/2018
---
# <a name="text"></a>Tekst

_Aby wpisać lub wyświetlić tekst przy użyciu platformy Xamarin.Forms._

Platformy Xamarin.Forms ma trzy widoki podstawowe do pracy z tekstem:

- **[Etykieta](#Label)**  &mdash; prezentacji jednego lub wiele wierszy tekstu. Można wyświetlić tekst z wieloma opcjami formatowania w tym samym wierszu.
- **[Wpis](#Entry)**  &mdash; do wprowadzania tekstu, który jest tylko jeden wiersz. Wpis ma tryb hasła.
- **[Edytor](#Editor)**  &mdash; do wprowadzania tekstu, który może zająć więcej niż jeden wiersz.

Można zmienić wygląd tekstu przy użyciu wbudowanych lub niestandardowych [style](#Styles) i niektóre formanty obsługują niestandardowe [czcionki](#Fonts).

<a name="Label" />

## <a name="labellabelmd"></a>[Etykieta](label.md)

`Label` Widoku jest używana do wyświetlania tekstu. Można go wyświetlać wiele wierszy tekstu lub pojedynczy wiersz tekstu. `Label` można przedstawić tekstu z wieloma opcjami formatowania używane w tekście. Wyświetl etykiety można zawijać lub obcinaj tekstu, jeśli nie mieści się w jednym wierszu.

![](images/label.png "Przykład etykiety")

Zobacz [etykiety](label.md) artykuł, aby uzyskać szczegółowe informacje.

Aby uzyskać informacje dotyczące dostosowywania Czcionka używana w etykiecie, zobacz [czcionki](fonts.md).

<a name="Entry" />

## <a name="entryentrymd"></a>[Wpis](entry.md)

`Entry` Służy do przyjmowania danych wejściowych jednego wiersza tekstu. `Entry` oferty kontrolę nad kolorów, ale nie można dostosowywać czcionki. `Entry` trybem hasła i można wyświetlić tekst zastępczy, dopóki wprowadzony tekst.

![](images/entry.png "Przykładowy wpis")

Zobacz [wpis](entry.md) artykułu, aby uzyskać więcej informacji.

Należy zauważyć, że w przeciwieństwie do `Label`, `Entry` nie może mieć niestandardowe ustawienia czcionki.

<a name="Editor" />

## <a name="editoreditormd"></a>[Edytor](editor.md)

`Editor` Służy do wprowadzania tekstu wielowierszowego zaakceptować. `Editor` może być niestandardowy kolor tła, ale kolor tekstu i nie można zmienić czcionkę.

![](images/editor.png "Przykład edytora")

Zobacz [edytora](editor.md) artykuł, aby uzyskać więcej informacji.

<a name="Fonts" />

## <a name="fontsfontsmd"></a>[Czcionki](fonts.md)

`Label` Sterowanie obsługuje ustawienia czcionkę na wszystkich platform lub niestandardowych czcionek dołączone do aplikacji przy użyciu wbudowanych czcionek. Zobacz [czcionki](fonts.md) artykuł, aby uzyskać szczegółowe informacje.

<a name="Styles" />

## <a name="stylesstylesmd"></a>[Style](styles.md)

Zapoznaj się [pracy przy użyciu stylów](~/xamarin-forms/user-interface/styles/index.md) Aby dowiedzieć się, jak skonfigurować czcionkę [kolor](~/xamarin-forms/user-interface/colors.md)i inne właściwości wyświetlania, mające zastosowanie do całej wielu formantów.



## <a name="related-links"></a>Linki pokrewne

- [Tekst (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)
