---
title: iOS 9 の互換性
description: IOS 9 の機能をアプリにすぐに追加する予定がない場合でも、最新バージョンの Xamarin を使用してアプリをリビルドする必要があります。
ms.prod: xamarin
ms.assetid: 69A05B0E-8A0A-489F-8165-B10AC46FAF3C
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/19/2017
ms.openlocfilehash: 246653cee7917141ddd0f911a7c4d1b21f945360
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70751971"
---
# <a name="ios-9-compatibility"></a>iOS 9 の互換性

_IOS 9 の機能をアプリにすぐに追加する予定がない場合でも、最新バージョンの Xamarin を使用してアプリをリビルドする必要があります。_

> [!IMPORTANT]
> このページの情報は、ios 8 以前を対象とするアプリストアに既にインストールされているアプリを使用していて、iOS 9 互換性のための更新プログラムをまだ送信していないユーザーを対象としています。 Xcode 7 と Xamarin を既に使用している場合: アプリの開発については、「 [iOS 9 の概要」を](~/ios/platform/introduction-to-ios9/index.md)参照してください。

最初の iOS 9 ベータ版が登場すると、以前のバージョンの Xamarin では2つの問題が検出されました。これは、以前のアプリを iOS 9 で開始できないことを示しています。

次の 2[つの問題](http://forums.xamarin.com/discussion/comment/131529/#Comment_131529)があります。

- IOS 8 以前用のアプリビルドは、32ビットのデバイス ( [Unified API](~/cross-platform/macios/unified/index.md)でビルドされたアプリを含む) で開始できません。
- P/Invoke が完全パスで失敗しました。

Xamarin のインストールを最新の安定したチャネルリリースに更新してから、アプリをリビルドして再デプロイすると、これらの2つの問題が修正されます。

_IOS 9 の機能でアプリをすぐに更新する予定がない場合でも、最新バージョンの Xamarin で再構築し、App Store に再送信することをお勧めし_ます。

これにより、お客様のアップグレード後、アプリが iOS 9 で動作するようになります。
IOS 8 のサポートを継続できます。最新のリリースでの再構築は、アプリケーションのターゲットバージョンには影響しません。

IOS 9 で既存のアプリのテスト中にさらに問題が発生した場合は、以下の「[互換性の向上](#compat)」セクションを参照してください。

### <a name="updating-with-visual-studio"></a>Visual Studio を使用した更新

Visual Studio が最新の安定したバージョンに更新されていることを明示的に確認することをお勧めします。

## <a name="what-about-components-nugets-and-other-libraries"></a>コンポーネント、Nuget、およびその他のライブラリについて

前述の2つの問題に対処するために使用しているコンポーネントまたは Nuget の新しいバージョンを待つ必要はあり**ません**。
これらの問題は、Xamarin. iOS の最新の安定したリリースを使用してアプリを再構築するだけで修正されます。

同様に、コンポーネントベンダーと Nuget の作成者は、前述の2つの問題を修正するためだけに新しいビルドを送信する必要は**ありません**。 ただし、コンポーネントまたは Nuget で**Xib**ファイル`UICollectionView`のビューを使用したり読み込みたりする場合は、以下で説明する iOS 9 の互換性の問題に対処するために更新が必要に*なることがあり*ます。

<a name="compat" />

## <a name="improving-compatibility-in-your-code"></a>コードの互換性の向上

IOS 9 では、以前のバージョンの iOS での動作に*使用さ*れるコードパターンがいくつかあります。 次に、iOS 9 でのテスト時に発生する可能性のある問題 (およびその解決策) を示します。

### <a name="uicollectionviewcellcontentview-is-null-in-constructors"></a>コンストラクターの UICollectionViewCell が null です

**理由**Ios `initWithFrame:` 9 では、 [UICollectionView ドキュメントの状態](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UICollectionView_class/#//apple_ref/occ/instm/UICollectionView/dequeueReusableCellWithReuseIdentifier:forIndexPath)として、ios 9 の動作が変更されたため、コンストラクターが必要になりました。 指定された識別子に対してクラスを登録し、新しいセルを作成する必要がある場合は、その`initWithFrame:`メソッドを呼び出すことによってセルが初期化されるようになりました。

**ファイル**次の`initWithFrame:`ようにコンストラクターを追加します。

```csharp
[Export ("initWithFrame:")]
public YourCellClassName (CGRect frame) : base (frame)
{
    Initialize (); // refactor initialize code into a method
}
```

関連するサンプル:[Motiongraph](https://github.com/xamarin/monotouch-samples/commit/3c1b7a4170c001e7290db9babb2b7a6dddeb8bcb)、 [TextKitDemo](https://github.com/xamarin/monotouch-samples/commit/23ea01b37326963b5ebf68bbcc1edd51c66a28d6)

### <a name="uiview-fails-to-init-with-coder-when-loading-a-view-from-a-xibnib"></a>Xib/Nib からビューを読み込むときに、UIView がプログラマによる初期化に失敗する

**理由**`initWithCoder:`コンストラクターは、Interface Builder Xib ファイルからビューを読み込むときに呼び出されるコンストラクターです。 このコンストラクターがエクスポートされていない場合、アンマネージコードはマネージバージョンのマネージドバージョンを呼び出すことができません。 以前 ( iOS 8 では、 `IntPtr`コンストラクターはビューを初期化するために呼び出されました。

**ファイル**次のように`initWithCoder:`コンストラクターを作成してエクスポートします。

```csharp
[Export ("initWithCoder:")]
public YourClassName (NSCoder coder) : base (coder)
{
    Initialize (); // refactor initialize code into a method
}
```

関連するサンプル:[コミュニティ](https://github.com/xamarin/monotouch-samples/commit/7b81138d52e5f3f1aa3769fcb08f46122e9b6a88)

### <a name="dyld-message-no-cache-image-with-name"></a>Dyld メッセージ: 名前のキャッシュイメージがありません...

ログに次の情報が表示された場合、クラッシュが発生する可能性があります。

```csharp
Dyld Error Message:
Dyld Message: no cache image with name (/System/Library/PrivateFrameworks/JavaScriptCore.framework/JavaScriptCore)
```

**理由**これは、Apple のネイティブリンカーのバグであり、プライベートフレームワークをパブリックにすると (JavaScriptCore は iOS 7 で公開されていましたが、プライベートフレームワークではなくなりました)、アプリのデプロイターゲットは、フレームワークがプライベートであるときの iOS バージョン用です。 この場合、Apple のリンカーは、パブリックバージョンではなく、フレームワークのプライベートバージョンとリンクします。

**ファイル**これは iOS 9 に対応していますが、その間にすぐに適用できる簡単な回避策があります。 (この場合は、iOS 7 を試すことができます)、後で簡単に適用できます。 他のフレームワークでも同様の問題が発生する可能性があります。たとえば、WebKit フレームワークは iOS 8 で公開されています (したがって、iOS 7 を対象とすると、このエラーが発生します。アプリで WebKit を使用するには、iOS 8 を対象にする必要があります)。

## <a name="related-links"></a>関連リンク

- [iOS 9 互換性リリース情報](https://releases.xamarin.com/ios-hotfix-for-ios-9-preview-xcode-6/)
- [iOS 9 の概要](~/ios/platform/introduction-to-ios9/index.md)
- [Xamarin の iOS アプリを iOS9 に更新する (ビデオ)](https://university.xamarin.com/lightninglectures/Updating-your-XamariniOS-apps-to-iOS9)
