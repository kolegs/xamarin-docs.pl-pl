---
title: "Brak błędów pakietów po zaktualizowaniu pakietów Nuget"
ms.topic: article
ms.prod: xamarin
ms.assetid: D61CC966-1D4A-49A5-8A6F-41572E28329B
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: f330590132e4881484eeae3efafe570d991a509a
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="missing-packages-error-after-updating-nuget-packages"></a>Brak błędów pakietów po zaktualizowaniu pakietów Nuget

Ten problem został zgłoszony głównie na platformy Xamarin.Forms przykładowej aplikacji rozwiązania, ale potencjalnych ten problem może się zdarzyć w żadnym projekcie, który używa pakietów NuGet. 

Jeśli po zaktualizowaniu pakietów Nuget w projekcie lub rozwiązaniu, zostanie wyświetlony błąd, który odwołuje się do starego numery wersji pakietu, takich jak:

```csharp
Error: This project references NuGet package(s) that are missing on this computer.
Enable NuGet Package Restore to download them.  
For more information, see http://go.microsoft.com/fwlink/?LinkID=322105

The missing file is ../../packages/Xamarin.Forms.1.3.1.6296/build/portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10/Xamarin.Forms.targets. (FormsGallery)

```

W tym przykładzie *Xamarin.Forms.1.3.1.6296* jest stary numer wersji został usunięty z aktualizacji pakietu Nuget.

Może się to zdarzyć, jeśli ręcznie dodane elementów XML w pliku .csproj, które odwołują się stary numer wersji pakietu lub edytować, Nuget nie usunąć i zaktualizuj je w razie były ręcznie dodać/edytować, więc projekt teraz szuka pakiety, które zostały usunięte. 

Aby rozwiązać ten problem, należy ręcznie edytować pliki .csproj i usunąć wszystkie elementy, które odwołują się stary numer wersji. 

Przykładowe elementy, aby usunąć (jeśli mają stary numer wersji pakietu):

```xml
<Reference Include="Xamarin.Forms.Maps">
    <HintPath>..\..\packages\Xamarin.Forms.Maps.1.3.1.6296\lib\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.Maps.dll</HintPath>
</Reference>

<Import Project="..\..\packages\Xamarin.Forms.1.3.1.6296\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.targets" Condition="Exists('..\..\packages\Xamarin.Forms.1.3.1.6296\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.targets')" />
<Error Condition="!Exists('..\..\packages\Xamarin.Forms.1.3.1.6296\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.targets')" Text="$([System.String]::Format('$(ErrorText)', '..\..\packages\Xamarin.Forms.1.3.1.6296\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.targets'))" />

```

