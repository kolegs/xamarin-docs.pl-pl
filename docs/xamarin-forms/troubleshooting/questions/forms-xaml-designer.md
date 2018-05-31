---
title: Dlaczego projektanta XAML usługi Visual Studio nie działa dla plików XAML platformy Xamarin.Forms
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: cab2eefb-c52f-4d81-866e-8f1feabbdd64
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: 43088beba6c6a86330cac164856be98d88f07fe2
ms.sourcegitcommit: 4f646dc5c51db975b2936169547d625c78a22b30
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/25/2018
ms.locfileid: "34546154"
---
# <a name="why-doesnt-the-visual-studio-xaml-designer-work-for-xamarinforms-xaml-files"></a>Dlaczego projektanta XAML usługi Visual Studio nie działa dla plików XAML platformy Xamarin.Forms

Platformy Xamarin.Forms nie obsługuje obecnie wizualnych projektantów plików XAML. W związku z tym podczas próby otwarcia pliku XAML formularzy w albo program Visual Studio *projektanta interfejsu użytkownika XAML* lub *projektanta interfejsu użytkownika XAML z kodowaniem*, zostanie zgłoszony następujący komunikat o błędzie:

> "Z wybranego edytora nie można otworzyć pliku. Wybierz inny edytor."

To ograniczenie jest opisany w [omówienie](~/xamarin-forms/xaml/xaml-basics/index.md#Overview) sekcji [podstawy XAML platformy Xamarin.Forms](~/xamarin-forms/xaml/xaml-basics/index.md) przewodnika:

> "Nie ma jeszcze wizualnego projektanta generowania XAML w aplikacji platformy Xamarin.Forms, więc cała XAML musi być odręcznego".

Jednak można wyświetlić podgląd XAML platformy Xamarin.Forms wybierając **Widok > inne okna > Podgląd platformy Xamarin.Forms** opcji menu.
