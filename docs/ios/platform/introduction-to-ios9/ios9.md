---
title: iOS 9 の互換性
description: すぐ iOS 9 の機能をアプリに追加する予定がない場合でも、Xamarin の最新バージョンを使用してアプリケーションを再構築する必要があります。
ms.prod: xamarin
ms.assetid: 69A05B0E-8A0A-489F-8165-B10AC46FAF3C
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/19/2017
ms.openlocfilehash: 6ade1c05c8e1cc64a4d24df1284d86175083ab80
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61293653"
---
# <a name="ios-9-compatibility"></a>iOS 9 の互換性

_すぐ iOS 9 の機能をアプリに追加する予定がない場合でも、Xamarin の最新バージョンを使用してアプリケーションを再構築する必要があります。_

> [!IMPORTANT]
> IOS 8 を対象とするアプリ ストアで既にアプリの使用のお客様向けのこのページの情報は、以前では、ユーザーが既に送信されていない iOS 9 の互換性のための更新プログラム。 -最新バージョンが既に使用している場合は、Xcode 7 と Xamarin.iOS 9 - アプリの開発を参照してください、 [iOS 9 の概要](~/ios/platform/introduction-to-ios9/index.md)します。

最初の iOS 9 ベータが登場したときに以前のバージョンの Xamarin で iOS 9 を開始できない古いアプリとして示されている 2 つの問題を特定しました。

これら 2 つの問題 (として[弊社のフォーラムに詳細な](http://forums.xamarin.com/discussion/comment/131529/#Comment_131529)) でした。

- 8 またはそれ以前の 32 ビットのデバイスでを起動できない iOS 用アプリのビルド (ビルドされたアプリを含む、 [Unified API](~/cross-platform/macios/unified/index.md))。
- P/invoke の完全なパスが発生したが指定されていません。

最新の安定したチャネルのリリースでは、再構築して、アプリを再デプロイする Xamarin のインストールを更新すると、これら 2 つの問題が修正されます。

_Xamarin の最新バージョンを再構築し、App Store に再送信を勧め場合でも、iOS 9 の機能を使用してアプリをすぐに更新する予定がない、_ します。



お客様のアップグレード後に iOS 9 で、アプリを実行するようになります。
IOS 8 - のサポートを継続することができます、最新のリリースでの再構築では、アプリケーションのターゲット バージョンは影響しません。

IOS 9 で既存のアプリのテスト中にさらに問題があれば、読み取り、[互換性を向上させる](#compat)以下のセクション。


### <a name="updating-with-visual-studio"></a>Visual Studio での更新

明示的にチェックして最新の安定バージョンを Visual Studio が更新されたことをお勧めします。

## <a name="what-about-components-nugets-and-other-libraries"></a>コンポーネント、Nuget、およびその他のライブラリについて説明します。

操作を行う**いない**コンポーネントや、上記で説明した 2 つの問題に対処している Nuget の新しいバージョンを待機する必要があります。
Xamarin.iOS の最新の安定版リリースを使用してアプリを再構築するだけでは、これらの問題が修正されました。

同様に、コンポーネントのベンダーと Nuget の作成者は**いない**上記で説明した 2 つの問題を修正するだけの新しいビルドを送信するために必要です。 ただし、存在する場合、コンポーネントまたは Nuget を使用して`UICollectionView`またはビューからの読み込み**Xib**ファイル、更新プログラム*可能性があります*下記 iOS 9 の互換性の問題に対処する必要があります。


<a name="compat" />

## <a name="improving-compatibility-in-your-code"></a>コードの互換性を向上させる

コードのいくつかのケースをパターンには*使用*iOS 9 の重大な Io の古いバージョンで動作します。 ここではいくつか考えられる問題 (とその解決策) iOS 9 でテストするときに発生します。

### <a name="uicollectionviewcellcontentview-is-null-in-constructors"></a>UICollectionViewCell.ContentView は null では、コンス トラクターです。

**理由:** IOS 9 で、`initWithFrame:`コンス トラクターは、今すぐとして iOS 9 の動作の変更により、必要な[UICollectionView マニュアルの記載](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UICollectionView_class/#//apple_ref/occ/instm/UICollectionView/dequeueReusableCellWithReuseIdentifier:forIndexPath)します。 セルが現在呼び出すことによって初期化されて場合、指定した識別子のクラスを登録して、新しいセルを作成する必要があります、その`initWithFrame:`メソッド。

**修正:** 追加、`initWithFrame:`このようなコンス トラクター。

```csharp
[Export ("initWithFrame:")]
public YourCellClassName (CGRect frame) : base (frame)
{
    Initialize (); // refactor initialize code into a method
}
```

関連サンプル:[MotionGraph](https://github.com/xamarin/monotouch-samples/commit/3c1b7a4170c001e7290db9babb2b7a6dddeb8bcb)、 [TextKitDemo](https://github.com/xamarin/monotouch-samples/commit/23ea01b37326963b5ebf68bbcc1edd51c66a28d6)



### <a name="uiview-fails-to-init-with-coder-when-loading-a-view-from-a-xibnib"></a>Xib/Nib からビューの読み込み時にエンコーダーを使って UIView が失敗しました。

**理由:**`initWithCoder:`コンス トラクターは 1 つのインターフェイス ビルダー Xib ファイルからビューを読み込むときに呼び出されます。 このコンス トラクターがエクスポートされない場合、アンマネージ コードは、管理対象バージョンを呼び出すことはできません。 以前 (例。 iOS 8) で、`IntPtr`ビューを初期化するコンス トラクターが呼び出されます。

**修正:** 作成し、エクスポート、`initWithCoder:`このようなコンス トラクター。

```csharp
[Export ("initWithCoder:")]
public YourClassName (NSCoder coder) : base (coder)
{
    Initialize (); // refactor initialize code into a method
}
```

関連サンプル:[チャット](https://github.com/xamarin/monotouch-samples/commit/7b81138d52e5f3f1aa3769fcb08f46122e9b6a88)


### <a name="dyld-message-no-cache-image-with-name"></a>Dyld メッセージ: 名前を持つキャッシュ イメージしています.

ログに次の情報でクラッシュが発生する可能性があります。

```csharp
Dyld Error Message:
Dyld Message: no cache image with name (/System/Library/PrivateFrameworks/JavaScriptCore.framework/JavaScriptCore)
```

**理由:** これは、パブリック、プライベートのフレームワークを行うときは、Apple のネイティブ リンカーのバグ (JavaScriptCore がパブリックになった iOS 7 で前に、プライベートのフレームワークが)、アプリのデプロイ ターゲットは、フレームワークがプライベートの場合、iOS 版は。 ここで Apple のリンカーは、パブリック バージョンの代わりに、フレームワークのプライベート バージョンとリンクされます。

**修正:** Ios 9 の場合、これは解決されますが、回避策が簡単であることをそれまでは自分で適用することができます。 (iOS 7 をこの場合に試行することができます)、プロジェクトに後で iOS バージョンを対象にだけです。 他のフレームワークと同様の問題が発生する可能性があります、たとえば、WebKit framework は iOS 8 で公開された (およびため、このエラーの結果は iOS 7 を対象とする iOS アプリで WebKit を使用する 8 をターゲットする必要があります)。



## <a name="related-links"></a>関連リンク

- [iOS 9 の互換性のリリース情報](https://releases.xamarin.com/ios-hotfix-for-ios-9-preview-xcode-6/)
- [iOS 9 の概要](~/ios/platform/introduction-to-ios9/index.md)
- [Ios 9 (ビデオ) に、Xamarin.iOS アプリを更新しています](https://university.xamarin.com/lightninglectures/Updating-your-XamariniOS-apps-to-iOS9)
