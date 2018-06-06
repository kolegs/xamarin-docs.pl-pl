---
title: Dokumentacja Info.plist dla platformy Xamarin.iOS
description: W tym dokumencie opisano różne pary klucz wartość, które można ustawić w pliku Info.plist aplikacji platformy Xamarin.iOS. Klucze te są niezbędne, jeśli aplikacja przeprowadza określonych zadań, takich jak uzyskiwanie dostępu do lokalizacji, zdjęć, mikrofon lub aparatu.
ms.prod: xamarin
ms.assetid: 944DFDB5-ADBA-4D6E-984C-5AEC19A1CC57
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 01/18/2017
ms.openlocfilehash: fa40add67155bd982041b829264a10b9764aa950
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34785479"
---
# <a name="infoplist-reference-for-xamarinios"></a>Dokumentacja Info.plist dla platformy Xamarin.iOS

Aby uzyskać więcej informacji na temat pracy z Info.Plist klucze zapoznaj się [Praca z zabezpieczenia i prywatność](~/ios/app-fundamentals/security-privacy.md) przewodnik. 

## <a name="location"></a>Lokalizacja 

Uzyskiwanie dostępu do lokalizacji użytkownika wymaga również zmiany w pliku Info.plist. Należy ustawić następujące klucze odnoszące się do danych lokalizacji: 

* **NSLocationWhenInUseUsageDescription** — w przypadku gdy uzyskujesz dostęp do lokalizacji użytkownika podczas, gdy są one interakcji z aplikacją. 
* **NSLocationAlwaysUsageDescription** — w przypadku gdy aplikacja uzyskuje dostęp do lokalizacji użytkownika w tle.

## <a name="photos"></a>Zdjęć 

NSPhotoLibraryUsageDescription  

## <a name="contacts"></a>Kontakty 

NSContactsUsageDescription 

## <a name="calendar-data"></a>Dane kalendarza 
    
NSCalendarsUsageDescription 

## <a name="reminder-data"></a>Przypomnienie danych 
    
NSRemindersUsageDescription 

## <a name="bluetooth-peripherals"></a>Urządzenia peryferyjne Bluetooth 
    
NSBluetoothPeripheralUsageDescription 

## <a name="microphone"></a>Mikrofon 

NSMicrophoneUsageDescription 

## <a name="camera"></a>Aparat fotograficzny 
    
NSCameraUsageDescription 

## <a name="reading-healthkit"></a>Odczytywanie HealthKit  

NSHealthShareUsageDescription 

NSHealthUpdateUsageDescription 

## <a name="background-modes"></a>Tryby tła 
    
UIBackgroundModes 

## <a name="homekit"></a>HomeKit 

NSHomeKitUsageDescription 


## <a name="related-links"></a>Linki pokrewne

- [Podręcznik firmy Apple.](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html#//apple_ref/doc/uid/TP40009252-SW10)
