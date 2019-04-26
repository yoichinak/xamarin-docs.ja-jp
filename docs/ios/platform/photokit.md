---
title: Xamarin.iOS で PhotoKit
description: このドキュメントには、そのモデル オブジェクトについてのディスカッション PhotoKit がについて説明する方法、モデル データのクエリへとフォト ライブラリへの変更を保存します。
ms.prod: xamarin
ms.assetid: 7FDEE394-3787-40FA-8372-76A05BF184B3
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/14/2017
ms.openlocfilehash: d59cac9403a244ce553d84e0590b8a9c3d4d2f30
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61365877"
---
# <a name="photokit-in-xamarinios"></a>Xamarin.iOS で PhotoKit

PhotoKit は、アプリケーションをシステムのイメージのライブラリを照会し、その内容を表示したり、カスタム ユーザー インターフェイスを作成できる新しいフレームワークです。 これには、多数アルバムやフォルダーなどの資産のコレクションだけでなく、イメージおよびビデオ資産を表すクラスにはが含まれます。

## <a name="model-objects"></a>モデル オブジェクト

PhotoKit では、モデル オブジェクトにこれらの資産を表します。 写真やビデオ自体を表すモデル オブジェクトは型`PHAsset`します。 A`PHAsset`資産のメディアの種類とその作成日などのメタデータが含まれています。
同様に、`PHAssetCollection`と`PHCollectionList`クラスには資産のコレクションおよびコレクションのリストについてのメタデータがそれぞれ含まれます。 資産のコレクションは、すべての写真やビデオ、特定の年度などのアセットのグループです。 同様に、コレクションのリストは、写真やビデオの年別にグループ化などの資産のコレクションのグループです。

## <a name="querying-model-data"></a>モデル データのクエリ

PhotoKit 簡単にクエリ モデルのデータのさまざまなフェッチ方法を使用します。 たとえば、すべてのイメージを取得するが呼び出す`PFAsset.Fetch`を渡して、`PHAssetMediaType.Image`メディアの種類。

    PHFetchResult fetchResults = PHAsset.FetchAssets (PHAssetMediaType.Image, null);

`PHFetchResult`インスタンスはすべてを格納し、`PFAsset`イメージを表すインスタンス。 使用するイメージ自体を取得する、 `PHImageManager` (またはキャッシュのバージョンでは、 `PHCachingImageManager`) 呼び出すことによって、イメージの要求を行い、`RequestImageForAsset`します。 次のコードが各資産でのイメージを取得するなど、 `PHFetchResult` collection view cell をで表示します。


    public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
    {
              var imageCell = (ImageCell)collectionView.DequeueReusableCell (cellId, indexPath);
            imageMgr.RequestImageForAsset ((PHAsset)fetchResults [(uint)indexPath.Item], thumbnailSize,
        PHImageContentMode.AspectFill, new PHImageRequestOptions (), (img, info) => {
            imageCell.ImageView.Image = img;
        });
        return imageCell;
    }

これは、次に示すようにイメージのグリッドが得られます。

![](photokit-images/image4.png "実行中のアプリのイメージのグリッドを表示します。")
 
## <a name="saving-changes-to-the-photo-library"></a>フォト ライブラリへの変更の保存

クエリを実行して、データの読み取りを処理する方法です。 変更は、ライブラリに戻る記述することもできます。 複数の対象アプリケーションは、システムのフォト ライブラリを操作することであるため、PhotoLibraryObserver を使用して変更の通知を受け取るオブザーバーを登録できます。 次に、変更には、アプリケーションがそれに応じて更新できます。 たとえば、上記のコレクション ビューを再読み込みする単純な実装を示します。

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
    
アプリケーションからの変更を戻す書き込む実際には、するには、変更要求を作成します。 各モデル クラスには、関連付けられている変更要求クラス。 たとえば、PHAsset を変更するを PHAssetChangeRequest を作成します。 フォト ライブラリに書き戻されるされ、上記のようなオブザーバーに送信される変更を実行する手順は次のとおりです。

-   編集操作を実行します。
-   PHContentEditingOutput インスタンスにフィルター選択された画像データを保存します。
-   変更のフォームの編集の出力を発行する変更要求を行います。

コア イメージ noir フィルターが適用されるイメージへの変更を書き込む例を次に示します。

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
    
ユーザーは、ボタンを選択するときに、フィルターが適用されます。

![](photokit-images/image5.png "適用されるフィルターの例")
 
ユーザーが戻るときは、変更をコレクション ビューに反映するよう、PHPhotoLibraryChangeObserver に協力してくれた。

![](photokit-images/image6.png "ユーザーが戻るときに、コレクション ビューで、変更が反映されます。")
