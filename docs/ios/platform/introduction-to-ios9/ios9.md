---
title: "iOS 9 の互換性"
description: "すぐアプリを iOS 9 の機能を追加する予定がない場合でも、最新バージョンの Xamarin を使用してアプリケーションを再構築する必要があります。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 69A05B0E-8A0A-489F-8165-B10AC46FAF3C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: bfdf0c73226eec472eb671d5543f5ce124919ab8
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="ios-9-compatibility"></a>iOS 9 の互換性

_すぐアプリを iOS 9 の機能を追加する予定がない場合でも、最新バージョンの Xamarin を使用してアプリケーションを再構築する必要があります。_

> [!IMPORTANT]
> **注:**このページでは、この情報は、既に iOS 8 以降を対象とするアプリ ストアにアプリを使用して iOS 9 の互換性のための更新プログラムをまだ送信していない顧客にします。 最新バージョンが既に使用している場合は、Xcode 7 および Xamarin.iOS 9 - アプリケーションの開発を参照してください、 [iOS 9 の概要](~/ios/platform/introduction-to-ios9/index.md)です。

最初の iOS 9 のベータ版が表示されたときに、2 つの問題を古いアプリを iOS 9 で開始することができませんとして示されている Xamarin の以前のバージョンではわかりました。

これら 2 つの問題 (として[フォーラムに詳細な](http://forums.xamarin.com/discussion/comment/131529/#Comment_131529)) でした。

- アプリは iOS 8 または 32 ビットのデバイスで開始することはできません前のビルド (ビルドされたアプリを含む、 [Unified API](~/cross-platform/macios/unified/index.md))。
- P/invoke の完全なパスが発生したが指定されていません。

最新の安定したチャネルのリリースでは、再構築して、アプリを再配置、Xamarin のインストールを更新すると、これら 2 つの問題が修正されます。

_Xamarin の最新バージョンを再構築して、アプリ ストアに再送信するをお勧めすぐ、アプリを iOS 9 の機能と更新を計画していない場合でも_です。



お客様のアップグレード後に iOS 9 で、アプリを実行するようになります。
IOS 8 - をサポートするために続行できる最新のリリースでは再構築しても、アプリケーションのターゲット バージョンは影響しません。

IOS 9 で、既存のアプリのテスト中に更なる問題があれば、読み取り、[向上互換性](#compat)以下のセクションです。


### <a name="updating-with-visual-studio"></a>Visual Studio での更新

明示的に確認して Visual Studio が最新の安定したバージョンに更新されたことをお勧めします。

## <a name="what-about-components-nugets-and-other-libraries"></a>コンポーネント、Nugets、およびその他のライブラリについて説明します。

操作を行う**いない**コンポーネントや Nugets 上記で説明した 2 つの問題に対処するを使用する新しいバージョンを待機する必要があります。
Xamarin.iOS の安定した最新のリリースでは、アプリを再構築するだけでは、この問題を修正します。

同様に、コンポーネントのベンダーと Nuget の作成者は**いない**上記で説明した 2 つの問題を修正するだけの新しいビルドを送信するために必要です。 ただし、存在する場合を使用するコンポーネントまたは Nuget`UICollectionView`からビューを読み込むまたは**Xib**ファイルは、更新プログラム*可能性があります*以下に言及 iOS 9 の互換性の問題に対処する必要あります。


<a name="compat" />

## <a name="improving-compatibility-in-your-code"></a>コードの互換性を向上させる

コードのいくつかのケースがされるパターンがある*使用*iOS 9 の重大な iOS の旧バージョンで動作します。 ここではいくつか考えられる問題 (とその解決策) iOS 9 でテストするときに発生する可能性です。

### <a name="uicollectionviewcellcontentview-is-null-in-constructors"></a>UICollectionViewCell.ContentView がコンス トラクターに null です。

**理由:** iOS 9 に、`initWithFrame:`コンス トラクターは、今すぐとして iOS 9 の動作の変更により、必要な[UICollectionView ドキュメント状態](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UICollectionView_class/#//apple_ref/occ/instm/UICollectionView/dequeueReusableCellWithReuseIdentifier:forIndexPath)です。 セルが呼び出すことによって初期化これで、指定した識別子のクラスを登録すると、別のセルを作成する必要があります、その`initWithFrame:`メソッドです。

**修正:**追加、`initWithFrame:`次のようにコンス トラクター。

```csharp
[Export ("initWithFrame:")]
public YourCellClassName (CGRect frame) : base (frame)
{
    Initialize (); // refactor initialize code into a method
}
```

関連するサンプル: [MotionGraph](https://github.com/xamarin/monotouch-samples/commit/3c1b7a4170c001e7290db9babb2b7a6dddeb8bcb)、 [TextKitDemo](https://github.com/xamarin/monotouch-samples/commit/23ea01b37326963b5ebf68bbcc1edd51c66a28d6)



### <a name="uiview-fails-to-init-with-coder-when-loading-a-view-from-a-xibnib"></a>Xib/Nib からビューを読み込むときに、コードの作成者を使って UIView が失敗します。

**理由:** 、`initWithCoder:`コンス トラクターは、1 つのインターフェイスのビルダー Xib ファイルからビューを読み込むときに呼び出されます。 このコンス トラクターがエクスポートされていない場合、アンマネージ コードは、管理されているバージョンを呼び出すことはできません。 以前 (リフレッシュ レート。 iOS 8) で、`IntPtr`ビューを初期化するコンス トラクターが呼び出されました。

**修正:**の作成とエクスポート、`initWithCoder:`次のようにコンス トラクター。

```csharp
[Export ("initWithCoder:")]
public YourClassName (NSCoder coder) : base (coder)
{
    Initialize (); // refactor initialize code into a method
}
```

関連のサンプル:[チャット](https://github.com/xamarin/monotouch-samples/commit/7b81138d52e5f3f1aa3769fcb08f46122e9b6a88)


### <a name="dyld-message-no-cache-image-with-name"></a>Dyld メッセージ: キャッシュ イメージがない名前を持つしています.

ログに次の情報でクラッシュが発生する可能性があります。

```csharp
Dyld Error Message:
Dyld Message: no cache image with name (/System/Library/PrivateFrameworks/JavaScriptCore.framework/JavaScriptCore)
```

**理由:**これは、パブリック、プライベートのフレームワークを行うときに発生する Apple のネイティブ リンカーのバグ (JavaScriptCore 公開された iOS 7 で前にプライベート framework した)、あり、アプリの配置ターゲットを iOS 版の場合、フレームワークは、プライベートでした。 ここでは Apple のリンカーは、プライベート framework のバージョンで、公開されているバージョンの代わりにリンクします。

**修正:** ios 9、これに対応する予定しますが、ある、簡単な対応策が当面の間に自分で適用することができます。 (iOS 7 をここではに再試行することができます)、プロジェクトの後で iOS バージョンを対象にだけです。 その他のフレームワークは、類似した問題を示す可能性があります、たとえば WebKit フレームワークが 8、iOS で公開された (および iOS 8 を実行し、アプリ内の WebKit の使用の対象にする必要があります。 このエラーが発生、iOS 7 を対象とするように) します。



## <a name="related-links"></a>関連リンク

- [iOS 9 の互換性リリース情報](https://releases.xamarin.com/ios-hotfix-for-ios-9-preview-xcode-6/)
- [iOS 9 の概要](~/ios/platform/introduction-to-ios9/index.md)
- [IOS9 (ビデオ) を Xamarin.iOS アプリの更新](https://university.xamarin.com/lightninglectures/Updating-your-XamariniOS-apps-to-iOS9)
