---
title: 'IBTool エラー: 操作を完了できませんでした。'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: A804EBC4-2BBF-4A98-A4E8-A455DB2E8A17
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 4647227ad208bfa968f8282a966220a09ab7f4a6
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="ibtool-error-the-operation-couldnt-be-completed"></a>IBTool エラー: 操作を完了できませんでした。

## <a name="fixed-in-xcode-611"></a>Xcode 6.1.1 の固定

Apple[固定](https://developer.apple.com/library/content/documentation/Xcode/Conceptual/RN-Xcode-Archive/Chapters/xc6_release_notes.html#//apple_ref/doc/uid/TP40016994-CH4-SW1)この`ibtool`Xcode 6.1.1、したがって Xcode 6.1.1 をアップグレードまたはそれ以降のバグが最も簡単な解決策です。

* * *

## <a name="description-of-the-problem"></a>問題の説明

`ibtool` Xcode 6.0 でのコマンドは OS X 10.10 Yosemite に関するバグを持っています。 Xamarin.iOS は Xcode の`ibtool`ストーリー ボードをコンパイルして`XIB`ファイル。

Xcode に関連して、バグの詳細については、次にあります post のスタック オーバーフローが発生します。 [http://stackoverflow.com/questions/25754763/cant-open-storyboard](http://stackoverflow.com/questions/25754763/cant-open-storyboard)

### <a name="error-message"></a>エラー メッセージ

> "MainStoryboard.storyboard"のドキュメントを開くことができませんでした。 操作を完了できませんでした。 (com.apple.InterfaceBuilder エラー-1 です。)

## <a name="workarounds-for-xcode-60"></a>その回避策 (Xcode 6.0)

### <a name="option-1-manage-all-uiimageviewimage-properties-in-code"></a>オプション 1: 管理すべて`UIImageView.Image`コードでのプロパティ

設定ではなく、`Image`のプロパティ、`UIImageView`ストーリー ボードにまたは`.xib`ファイルできるプロパティを設定するビューのいずれかでビュー コント ローラーのライフ サイクルのオーバーライド メソッド (たとえば、 `ViewDidLoad()`)。 関連項目[イメージを操作](~/ios/app-fundamentals/images-icons/index.md)使用に関するヒントについて`UIImage.FromBundle()`と`UIImage.FromFile()`です。

### <a name="option-2-move-all-of-the-image-resources-to-the-top-level-resources-folder"></a>オプション 2: では、すべてのイメージ リソースを移動、最上位レベルに`Resources`フォルダー

イメージを最上位レベルに移動した後`Resources`フォルダー、ストーリー ボードを更新する必要がおよび`.xib`新しいイメージのパスを使用するファイル。

### <a name="option-3-set-the-logicalname-for-any-problematic-image-assets-so-they-are-copied-to-the-top-level-of-theapp-bundle"></a>オプション 3: 設定、 `LogicalName` 、問題のあるイメージ資産の最上位レベルにコピーするための`.app`バンドル

たとえば、元`.csproj`ファイルには、次のエントリが含まれています。

`<BundleResource Include="Resources\Images\image.png" />`

この要素を変更し、追加することができます、`LogicalName`の最上位レベルに、画像はコピーする代わりにできるように、`.app `バンドルします。

```xml
<BundleResource Include="Resources\Images\image.png">
    <LogicalName>image.png</LogicalName>
</BundleResource>
```

Mac 用の Visual Studio で、`LogicalName`を使用して設定することも、`Resource ID`フィールドの下にあるイメージ**ビュー > パッド > プロパティ**です。 (も参照してください: [ http://stackoverflow.com/questions/16938250/xamarin-studio-folder-structure-issue-in-ios-project/16951545#16951545 ](http://stackoverflow.com/questions/16938250/xamarin-studio-folder-structure-issue-in-ios-project/16951545#16951545))

この変更後に、ストーリー ボードを更新する必要があると`.xib`新しい最上位のイメージのパスを使用するファイル。 Mac 用の visual Studio でのオートコンプリートの一覧を自動的に更新、 `Image` iOS デザイナー内のプロパティです。 Visual Studio では、パスを手動で編集する必要があります。 IOS デザイナーにより、これと表示、不足しているイメージが、プロジェクトのビルドおよび正常に実行されます。

### <a name="next-steps"></a>次の手順

詳細については、問い合わせ先、または、上記の情報を使用した後もこの問題が残っている場合を参照してください[どのようなサポート オプションは、Xamarin を使用しますか?](~/cross-platform/troubleshooting/support-options.md)推奨事項は、の連絡先のオプションについて方法だけでなく必要な場合は、新しいバグをファイルです。 

