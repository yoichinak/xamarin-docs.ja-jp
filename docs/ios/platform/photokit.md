---
title: Xamarin.iOS で PhotoKit
description: このドキュメントには、モデル オブジェクトについてのディスカッション PhotoKit がについて説明方法モデル データのクエリ、および写真ライブラリへの変更を保存します。
ms.prod: xamarin
ms.assetid: 7FDEE394-3787-40FA-8372-76A05BF184B3
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/14/2017
ms.openlocfilehash: 4aeeec5b96e24c654407ad672930c0cb78592450
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787898"
---
# <a name="photokit-in-xamarinios"></a>Xamarin.iOS で PhotoKit

PhotoKit は、イメージ ライブラリがシステムのクエリを表示およびその内容を変更するカスタム ユーザー インターフェイスを作成するアプリケーションを使用する新しいフレームワークです。 画像とビデオの資産だけでなくアルバムやフォルダーなどの資産のコレクションを表すクラスの数が含まれています。

## <a name="model-objects"></a>モデル オブジェクト

PhotoKit では、モデル オブジェクトにこれらのアセットを表します。 写真やビデオ自体を表すモデル オブジェクトは型の`PHAsset`です。 A`PHAsset`資産のメディアの種類の作成日などのメタデータが含まれています。
同様に、`PHAssetCollection`と`PHCollectionList`クラスには資産コレクションおよびコレクションのリストについてのメタデータがそれぞれ含まれます。 資産のコレクションは、すべての写真と特定の年のビデオなどの資産のグループです。 同様に、コレクションのリストは、資産のコレクション、写真やビデオの年度別にグループ化などのグループです。

## <a name="querying-model-data"></a>モデル データのクエリ

PhotoKit 簡単モデル データをクエリするさまざまなフェッチ方法を使用できます。 たとえば、すべてのイメージを取得する呼び出して`PFAsset.Fetch`渡す、`PHAssetMediaType.Image`メディアの種類。

    PHFetchResult fetchResults = PHAsset.FetchAssets (PHAssetMediaType.Image, null);

`PHFetchResult`インスタンスが含まれ、すべて、`PFAsset`イメージを表すインスタンス。 イメージ自体を取得するには、使用して、 `PHImageManager` (またはキャッシュのバージョンでは、 `PHCachingImageManager`) を呼び出して、イメージの要求を行い、`RequestImageForAsset`です。 たとえば、次のコードが各資産のイメージを取得、`PHFetchResult`コレクション ビューのセルに表示します。


    public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
    {
              var imageCell = (ImageCell)collectionView.DequeueReusableCell (cellId, indexPath);
            imageMgr.RequestImageForAsset ((PHAsset)fetchResults [(uint)indexPath.Item], thumbnailSize,
        PHImageContentMode.AspectFill, new PHImageRequestOptions (), (img, info) => {
            imageCell.ImageView.Image = img;
        });
        return imageCell;
    }

これは、結果は、次に示すようにイメージのグリッドになります。

![](photokit-images/image4.png "実行中のアプリのイメージのグリッドを表示します。")
 
## <a name="saving-changes-to-the-photo-library"></a>フォト ライブラリに変更を保存

クエリを実行して、データの読み取りを処理する方法です。 ライブラリに、変更を戻す記述することもできます。 複数の対象アプリケーションでシステム フォト ライブラリと対話することがあるため、PhotoLibraryObserver を使用して変更の通知を受信するオブザーバーを登録できます。 次に、変更には、アプリケーションがそれに応じて更新できます。 たとえば、上記のコレクション ビューを再読み込みする単純な実装を次に示します。

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
    
アプリケーションからの変更を戻す書き込む実際には、するには、変更要求を作成します。 モデル クラスのそれぞれで、関連付けられている変更要求クラスがあります。 たとえば、PHAsset を変更するには、PHAssetChangeRequest を作成します。 フォト ライブラリに書き戻さされ、上記のようなオブザーバーに送信される変更を実行する手順は次のとおりです。

-   編集操作を実行します。
-   PHContentEditingOutput インスタンスにフィルター選択された画像データを保存します。
-   変更のフォームの編集の出力を発行する変更要求を行います。

Core イメージ noir フィルターを適用するイメージの変更を書き込む例を次に示します。

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
    
ユーザーがボタンを選択すると、フィルターが適用されます。

![](photokit-images/image5.png "適用されるフィルターの例")
 
PHPhotoLibraryChangeObserver 感謝、変更が反映されますコレクション ビューに戻るときに。

![](photokit-images/image6.png "戻るときに、コレクション ビューに、変更が反映されます。")
