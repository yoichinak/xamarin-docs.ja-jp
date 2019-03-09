---
title: IBTool エラー:操作を完了できませんでした。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: A804EBC4-2BBF-4A98-A4E8-A455DB2E8A17
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 04/03/2018
ms.openlocfilehash: c2f727b55b21dc3bd976f0b41c71b794841cfca4
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/08/2019
ms.locfileid: "57667894"
---
# <a name="ibtool-error-the-operation-couldnt-be-completed"></a>IBTool エラー:操作を完了できませんでした。

## <a name="fixed-in-xcode-611"></a>Xcode 6.1.1 を修正しました

Apple[固定](https://developer.apple.com/library/content/documentation/Xcode/Conceptual/RN-Xcode-Archive/Chapters/xc6_release_notes.html#//apple_ref/doc/uid/TP40016994-CH4-SW1)この`ibtool`Xcode 6.1.1、したがって Xcode 6.1.1 をアップグレードまたはそれ以降のバグが最も簡単な修正します。

* * *

## <a name="description-of-the-problem"></a>問題の説明

`ibtool` Xcode 6.0 でのコマンドは、OS X 10.10 yosemite バグを必要があります。 Xamarin.iOS は Xcode の`ibtool`ストーリー ボードをコンパイルして`XIB`ファイル。

Xcode との関連のバグの詳細については、次のあります Stack Overflow の投稿。 [https://stackoverflow.com/questions/25754763/cant-open-storyboard](https://stackoverflow.com/questions/25754763/cant-open-storyboard)

### <a name="error-message"></a>エラー メッセージ

> "MainStoryboard.storyboard"のドキュメントを開くことができませんでした。 操作を完了できませんでした。 (com.apple.InterfaceBuilder エラー-1 です。)

## <a name="workarounds-for-xcode-60"></a>(Xcode 6.0) の回避策

### <a name="option-1-manage-all-uiimageviewimage-properties-in-code"></a>オプション 1:すべての管理`UIImageView.Image`コード内のプロパティ

設定ではなく、`Image`のプロパティを`UIImageView`でストーリー ボードまたは`.xib`ファイルで設定できるプロパティ、ビューのいずれかのビュー コント ローラーでのオーバーライド メソッドのライフ サイクル (などで`ViewDidLoad()`)。 参照してください[イメージを操作](~/ios/app-fundamentals/images-icons/index.md)使用に関するヒントについて`UIImage.FromBundle()`と`UIImage.FromFile()`します。

### <a name="option-2-move-all-of-the-image-resources-to-the-top-level-resources-folder"></a>オプション 2:最上位レベルに移動するすべてのイメージ リソース`Resources`フォルダー

イメージを最上位レベルに移動した後`Resources`フォルダー、ストーリー ボードを更新する必要と`.xib`新しいイメージのパスを使用するファイル。

### <a name="option-3-set-the-logicalname-for-any-problematic-image-assets-so-they-are-copied-to-the-top-level-of-theapp-bundle"></a>オプション 3:設定、 `LogicalName` 、問題のあるイメージのアセットの最上位レベルにコピーするための`.app`バンドル

たとえば、元`.csproj`ファイルには、次のエントリが含まれています。

`<BundleResource Include="Resources\Images\image.png" />`

この要素を変更し、追加することができます、`LogicalName`イメージの最上位レベルに代わりにコピーされるように、`.app `バンドル。

```xml
<BundleResource Include="Resources\Images\image.png">
    <LogicalName>image.png</LogicalName>
</BundleResource>
```

Visual Studio for Mac で、`LogicalName`を使用して設定することも、`Resource ID`フィールドの下にあるイメージ**ビュー > パッド > プロパティ**します。 (も参照してください: [ https://stackoverflow.com/questions/16938250/xamarin-studio-folder-structure-issue-in-ios-project/16951545#16951545 ](https://stackoverflow.com/questions/16938250/xamarin-studio-folder-structure-issue-in-ios-project/16951545#16951545))

この変更後、ストーリー ボードを更新する必要がありますと`.xib`新しい最上位レベルのイメージのパスを使用するファイル。 Visual Studio for Mac でのオートコンプリートの一覧を自動的に更新、 `Image` iOS Designer のプロパティ。 Visual Studio では、パスを手動で編集する必要があります。 IOS Designer は、このイメージとして表示不足しているが、プロジェクトのビルドおよび正常に実行されます。

### <a name="next-steps"></a>次の手順

問い合わせ、または上記の情報を使用した後でもこの問題が残っている場合を参照してください、詳細については[Xamarin のどのようなサポート オプションを使用しますか?](~/cross-platform/troubleshooting/support-options.md)連絡先オプション、推奨事項は、についてする方法についても必要な場合は、新しいバグをファイルします。 

