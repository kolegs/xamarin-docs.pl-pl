---
title: Brak błędów pakietów po zaktualizowaniu pakietów Nuget
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: D61CC966-1D4A-49A5-8A6F-41572E28329B
author: asb3993
ms.author: amburns
ms.openlocfilehash: 4f1c4f51b690e35711efc0fc4fac56565b9cb3c7
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/09/2018
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

