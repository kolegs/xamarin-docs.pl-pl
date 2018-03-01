---
title: Funkcje platformy systemu android
description: Dodawanie funkcji specyficznych dla systemu Android do aplikacji platformy Xamarin.Forms
ms.topic: article
ms.prod: xamarin
ms.assetid: E24168F3-0138-4814-86EA-B467F6B8A545
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/13/2016
ms.openlocfilehash: d68d84671028ded14b4b885f2c134656fc639f9e
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="android-platform-features"></a>Funkcje platformy systemu android

## <a name="platform-support"></a>Obsługa platformy

Domyślny projekt Android platformy Xamarin.Forms używa starszej styl renderering kontroli, Wspólny przed Android 5.0. Aplikacje utworzone przy użyciu szablonu ma `FormsApplicationActivity` jako klasa bazowa ich działania głównego.

## <a name="material-design-via-appcompat"></a>Projekt materiałów za pośrednictwem AppCompat

Platformy Xamarin.Forms ma również opcjonalne `FormsAppCompatActivity` używającą **AppCompat** funkcje oferowane przez system Android do zaimplementowania kompozycje materiału.

Aby dodać kompozycje materiały do projektu platformy Xamarin.Forms Android, wykonaj [obsługuje instrukcje instalacji AppCompat](appcompat.md)

Oto **Todo** przykładowa przy użyciu domyślnego `FormsApplicationActivity`:

[ ![](images/before-appcompat-sml.png "TODO przykładowej aplikacji bez AppCompat")](images/before-appcompat.png "Todo przykładowej aplikacji bez AppCompat")

I jest to ten sam kod po uaktualnieniu projektu do `FormsAppCompatActivity` (i dodać informacje dodatkowe motywu):

[ ![](images/post-appcompat-sml.png "TODO przykładową aplikację z AppCompat i motywów")](images/post-appcompat.png "Todo przykładową aplikację z AppCompat i motywów")

> [!NOTE]
> **Uwaga**: korzystając z `FormsAppCompatActivity`, [podstawowa klasy dla niektórych Android niestandardowe moduły renderowania](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md) będą inne.


## <a name="related-links"></a>Linki pokrewne

- [Dodaj obsługę projektowania materiału](appcompat.md)
