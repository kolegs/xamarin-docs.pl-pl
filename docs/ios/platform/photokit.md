---
title: PhotoKit
description: "Zestaw fotografii umożliwia aplikacjom zapytania Biblioteka obrazów systemu i utworzyć niestandardowego interfejsu użytkownika Służącego do wyświetlania i modyfikowania jego zawartość."
ms.topic: article
ms.prod: xamarin
ms.assetid: 19049ED5-B68E-4A0E-9D57-B7FAE3BB8987
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/14/2017
ms.openlocfilehash: 6f6858f36c30f45225417c78225926906481eb8d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="photokit"></a>PhotoKit

_Zestaw fotografii umożliwia aplikacjom zapytania Biblioteka obrazów systemu i utworzyć niestandardowego interfejsu użytkownika Służącego do wyświetlania i modyfikowania jego zawartość._

Zdjęcie Kit to nowe struktury, która umożliwia aplikacjom zapytania Biblioteka obrazów systemu oraz tworzenie niestandardowych interfejsów użytkownika do wyświetlania i modyfikowania jego zawartość. Zawiera liczbę klas reprezentujących obrazu i zasoby wideo, a także kolekcji zasobów, takich jak albumów i folderów.

## <a name="model-objects"></a>Model obiektów
Zestaw fotografii reprezentuje tych zasobów co wywołuje obiekty modelu. Obiekty modelu, które reprezentują zdjęć i klipów wideo, same są typu `PHAsset`. A `PHAsset` zawiera metadane, takie jak typ nośnika elementu zawartości i jego data utworzenia.
Podobnie `PHAssetCollection` i `PHCollectionList` klasy odpowiednio zawierać metadane dotyczące kolekcji zasobów i kolekcji list. Kolekcje zasobów są grup zasobów, takich jak zdjęć i klipów wideo w danym roku. Podobnie listy kolekcji są grup kolekcji zasobów, takie jak zdjęcia i filmy pogrupowane według roku.

## <a name="querying-model-data"></a>Wykonywanie zapytania na danych modelu
Zestaw fotografii można łatwo zapytania modelu danych przy użyciu różnych metod pobierania. Na przykład, aby pobrać wszystkie obrazy, należy wywołać `PFAsset.Fetch`, przechodzącą `PHAssetMediaType.Image` typu nośnika.

    PHFetchResult fetchResults = PHAsset.FetchAssets (PHAssetMediaType.Image, null);

`PHFetchResult` Wystąpienia następnie zawiera wszystkie `PFAsset` wystąpień reprezentujący obrazów. Aby uzyskać same obrazy, należy użyć `PHImageManager` (lub wersję buforowania, `PHCachingImageManager`) Wyślij żądanie obrazu przez wywołanie metody `RequestImageForAsset`. Na przykład poniższy kod pobiera obraz dla każdego trwałego `PHFetchResult` do wyświetlenia w komórce widoku kolekcji:


    public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
    {
              var imageCell = (ImageCell)collectionView.DequeueReusableCell (cellId, indexPath);
            imageMgr.RequestImageForAsset ((PHAsset)fetchResults [(uint)indexPath.Item], thumbnailSize,
        PHImageContentMode.AspectFill, new PHImageRequestOptions (), (img, info) => {
            imageCell.ImageView.Image = img;
        });
        return imageCell;
    }

Powoduje to siatkę z obrazami, jak pokazano poniżej:

![](photokit-images/image4.png "Uruchomionej aplikacji wyświetlanie siatki obrazów")
 
## <a name="saving-changes-to-the-photo-library"></a>Trwa zapisywanie zmian w bibliotece zdjęcia

To jest sposób obsługi kwerend i odczytywanie danych. Możesz także zapisać zmiany wstecz do biblioteki. Ponieważ wiele aplikacji zainteresowanych mogą wchodzić w interakcje z biblioteki fotografii systemu, należy zarejestrować obserwatora ma być powiadamiany o zmiany przy użyciu PhotoLibraryObserver. Następnie po kto zmian aplikacji można zaktualizować odpowiednio. Na przykład poniżej przedstawiono prosty implementacji ponowne załadowanie widoku kolekcji powyżej:

    class PhotoLibraryObserver : PHPhotoLibraryChangeObserver
    {
        readonly PhotosViewController controller;
        public PhotoLibraryObserver (PhotosViewController controller)
        
        {
            this.controller = controller;
        }
    
        public override void PhotoLibraryDidChange (PHChange changeInstance)
        {
            DispatchQueue.MainQueue.DispatchAsync (() => {
            var changes = changeInstance.GetFetchResultChangeDetails (controller.fetchResults);
            controller.fetchResults = changes.FetchResultAfterChanges;
            controller.CollectionView.ReloadData ();
            });
        }
    }
    
Faktycznie zapisania zmian z aplikacji, podczas tworzenia żądania zmiany. Każda z klas modelu zawiera klasę żądania zmiany skojarzone. Na przykład aby zmienić PHAsset, należy utworzyć PHAssetChangeRequest. Dostępne są następujące kroki, aby wprowadzić zmiany są zapisywane z powrotem w bibliotece zdjęć i wysyłanych do obserwatorów jak powyżej:

-   Wykonaj operację edycji.
-   Zapisz dane obrazu filtrowane do wystąpienia PHContentEditingOutput.
-   Utwórz żądanie zmiany do opublikowania formularza zmiany edycji danych wyjściowych.

Oto przykład, który zapisuje obraz, który stosuje filtr noir obrazu Core zmiany:

    void ApplyNoirFilter (object sender, EventArgs e)
    {
            
            Asset.RequestContentEditingInput (new PHContentEditingInputRequestOptions (), (input, options) => {
            
        // perform the editing operation, which applies a noir filter in this case
            var image = CIImage.FromUrl (input.FullSizeImageUrl);
            image = image.CreateWithOrientation((CIImageOrientation)input.FullSizeImageOrientation);
            var noir = new CIPhotoEffectNoir {
                Image = image
            };
            var ciContext = CIContext.FromOptions (null);
            var output = noir.OutputImage;
            var uiImage = UIImage.FromImage (ciContext.CreateCGImage (output, output.Extent));
            imageView.Image = uiImage;
        //
        // save the filtered image data to a PHContentEditingOutput instance
            var editingOutput = new PHContentEditingOutput(input);
            var adjustmentData = new PHAdjustmentData();
            var data = uiImage.AsJPEG();
            NSError error;
            data.Save(editingOutput.RenderedContentUrl, false, out error);
            editingOutput.AdjustmentData = adjustmentData;
        //
        // make a change request to publish the changes form the editing output
            PHPhotoLibrary.GetSharedPhotoLibrary.PerformChanges (() => {
                PHAssetChangeRequest request = PHAssetChangeRequest.ChangeRequest(Asset);
                request.ContentEditingOutput = editingOutput;
          },
          (ok, err) => Console.WriteLine ("photo updated successfully: {0}", ok));
      });
    }
    
Gdy użytkownik wybierze przycisk, jest stosowany filtr:

![](photokit-images/image5.png "Przykładowy filtr")
 
I dzięki użyciu PHPhotoLibraryChangeObserver, zmiana ta jest uwzględniana w widoku kolekcji gdy użytkownik nawiguje wstecz:

![](photokit-images/image6.png "Zmiany są uwzględniane w widoku kolekcji, gdy użytkownik nawiguje wstecz")
