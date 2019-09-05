---
title: 'IBTool エラー: 操作を完了できませんでした。'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: A804EBC4-2BBF-4A98-A4E8-A455DB2E8A17
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 04/03/2018
ms.openlocfilehash: 695410937e82b885e31827d02f5fefd138497523
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70290279"
---
# <a name="ibtool-error-the-operation-couldnt-be-completed"></a>IBTool エラー: 操作を完了できませんでした。

## <a name="fixed-in-xcode-611"></a>Xcode 6.1.1 で修正済み

Apple は Xcode 6.1.1 でこの `ibtool` バグを[修正](https://developer.apple.com/library/content/documentation/Xcode/Conceptual/RN-Xcode-Archive/Chapters/xc6_release_notes.html#//apple_ref/doc/uid/TP40016994-CH4-SW1)したため、Xcode 6.1.1 以降へのアップグレードが最も簡単な修正です。

* * *

## <a name="description-of-the-problem"></a>問題の説明

Xcode `ibtool` 6.0 のコマンドに、OS X 10.10 ヨークのバグがありました。 Xamarin iOS では、Xcode `ibtool`を使用して`XIB` 、ストーリーボードとファイルをコンパイルします。

Xcode に関連するバグの詳細については、次の Stack Overflow 投稿を参照してください。[https://stackoverflow.com/questions/25754763/cant-open-storyboard](https://stackoverflow.com/questions/25754763/cant-open-storyboard)

### <a name="error-message"></a>エラー メッセージ

> ドキュメント "Mainstoryboard.storyboard ファイル" を開けませんでした。 操作を完了できませんでした。 (com. InterfaceBuilder エラー-1)。

## <a name="workarounds-for-xcode-60"></a>回避策 (Xcode 6.0 の場合)

### <a name="option-1-manage-all-uiimageviewimage-properties-in-code"></a>オプション 1:コード内`UIImageView.Image`のすべてのプロパティを管理する

ストーリーボードまたは`Image` `UIImageView` `.xib`ファイルでのプロパティを設定するのではなく、ビューコントローラーのビューライフサイクルオーバーライドメソッドのいずれかでプロパティを設定できます (たとえば、 `ViewDidLoad()`)。 「 `UIImage.FromBundle()` [イメージ](~/ios/app-fundamentals/images-icons/index.md)の操作」も参照してください。`UIImage.FromFile()`

### <a name="option-2-move-all-of-the-image-resources-to-the-top-level-resources-folder"></a>オプション 2:すべてのイメージリソースを最上位`Resources`フォルダーに移動します。

イメージを最上位`Resources`フォルダーに移動した後、新しいイメージパスを使用するようにストーリーボードと`.xib`ファイルを更新する必要があります。

### <a name="option-3-set-the-logicalname-for-any-problematic-image-assets-so-they-are-copied-to-the-top-level-of-theapp-bundle"></a>オプション 3:問題の`LogicalName`あるイメージアセットを`.app`バンドルの最上位レベルにコピーするために、を設定します。

たとえば、元`.csproj`のファイルに次のエントリが含まれているとします。

`<BundleResource Include="Resources\Images\image.png" />`

この要素を変更し、を`LogicalName`追加して、イメージが`.app`バンドルの最上位レベルにコピーされるようにすることができます。

```xml
<BundleResource Include="Resources\Images\image.png">
    <LogicalName>image.png</LogicalName>
</BundleResource>
```

Visual Studio for Mac では`LogicalName` 、[表示] の下`Resource ID`にある画像のフィールドを使用してを設定し **> プロパティ > 埋め**込むこともできます。 (関連項目: [https://stackoverflow.com/questions/16938250/xamarin-studio-folder-structure-issue-in-ios-project/16951545#16951545](https://stackoverflow.com/questions/16938250/xamarin-studio-folder-structure-issue-in-ios-project/16951545#16951545))

この変更を行った後、新しい最上位レベルの`.xib`イメージパスを使用するようにストーリーボードとファイルを更新する必要があります。 Visual Studio for Mac によって、iOS Designer の`Image`プロパティのオートコンプリートの一覧が自動的に更新されます。 Visual Studio では、パスを手動で編集する必要があります。 IOS デザイナーでは、これは見つからないイメージとして表示されますが、プロジェクトは正常にビルドされ、実行されます。

### <a name="next-steps"></a>次の手順

詳細については、お問い合わせください。または、上記の情報を利用した後もこの問題が発生する場合は、「 [Xamarin で使用できるサポートオプション](~/cross-platform/troubleshooting/support-options.md)」を参照してください。連絡先オプション、提案、および必要に応じて新しいバグをファイルに登録する方法については、こちらを参照してください. 

