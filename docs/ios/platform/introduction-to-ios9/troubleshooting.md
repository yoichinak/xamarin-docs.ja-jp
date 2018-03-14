---
title: "トラブルシューティング"
description: "この記事は、Xamarin.iOS アプリで iOS 9 を操作するためのいくつかのトラブルシューティングのヒントを提供します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: DCE83E36-CBD9-4D96-8E7F-384CB8A54563
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: ca3697b355a45e06f941a6dfd610cd19f922ca75
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2018
---
# <a name="troubleshooting"></a>トラブルシューティング

_この記事は、Xamarin.iOS アプリで iOS 9 を操作するためのいくつかのトラブルシューティングのヒントを提供します。_

## <a name="there-was-a-problem-parsing-the-xml"></a>XML を解析中に問題が発生しました

Xamarin iOS デザイナーは、Xcode 7 の機能をまだサポートしていません。 ストーリー ボードが使用してデザイナーの読み込みに失敗する_「XML を解析中に問題が発生しました」_新しい iOS 9 (Xcode 7) を使用しようとしています。 StackView などのデザイナーの要素。

iOS の Xcode 7 機能のデザイナー サポート サイクル 6 機能の今後のリリースの対象になります。 サイクル 6 のプレビュー バージョンでは、アルファ チャネルで現在使用できるはされ、Xcode 7 の新機能のサポートが制限されます。

Visual Studio for Mac の部分的な回避策: ストーリー ボードを右クリックし、**ファイルを開く** > **Xcode インターフェイス ビルダー**です。

## <a name="where-are-the-ios-8-simulators"></a>ここでは、iOS シミュレーターが 8

Xcode 7 (またはそれ以上) が自動的をインストールしている場合は、既定でシミュレーターが iOS 9 で置き換えます iOS 8 シミュレーターのすべて。 IOS 8 でテストする場合は、Xcode を起動し、ダウンロードしてシミュレーターが iOS 8 をインストールします。

Xcode での選択、 **Xcode**メニュー、**設定しています.**  > **ダウンロード**:

[![](troubleshooting-images/ios8.png "iOS 8 シミュレータをダウンロードします。")](troubleshooting-images/ios8.png#lightbox)

をクリックして、**チェックとインストール 今すぐ**iOS 8 シミュレータを再インストールするボタンをクリックします。

## <a name="layout-constraint-with-leftright-attribute-errors"></a>エラーの左/右属性レイアウト制約

IOS 8 (および前)、ストーリー ボードでの UI 要素が両方の組み合わせを使用でした**右** & **左**属性 (`NSLayoutAttributeRight` & `NSLayoutAttributeLeft`) および**先頭の** & **Trailing**属性 (`NSLayoutAttributeLeading` & `NSLayoutAttributeTrailing`) 同じレイアウトにします。

IOS 9 内で同じストーリー ボードで実行する場合は、次の形式での例外で決定されます。

> キャッチされない例外 'NSInvalidArgumentException' のためのアプリを終了するには、理由: '*** + [NSLayoutConstraint constraintWithItem:attribute:relatedBy:toItem:attribute:multiplier:constant:]: 制約を先頭または末尾の間にすることはできません属性と右/左属性。 先頭および末尾の両方またはどちらも使用します '。

iOS 9 を適用するかを使用するレイアウト**右** & **左**_または_**先頭** &  **末尾の**属性が、*いない*両方です。 この問題を解決するには、同じ属性、ストーリー ボード ファイル内で設定を使用するすべてのレイアウト制約を変更します。

詳細についてを参照してください、 [iOS 9 制約エラー](http://stackoverflow.com/questions/32692841/ios-9-constraint-error)ディスカッションのスタック オーバーフローが発生します。

## <a name="error-itms-90535-unexpected-cfbundleexecutable-key"></a>エラー ITMS-90535: 予期しない CFBundleExecutable キー

IOS 9 に切り替え後のアプリからサード パーティのコンポーネント (具体的には、既存 Google Maps コンポーネント) にコンパイルと iTunes Connect 形式でエラーが発生したことができますを新しいビルドを送信しようとするときに、iOS 8 (またはそれ以前) で実行を使用します。

> エラー ITMS-90535: 予期しない CFBundleExecutable キー。 'Payload/app-name.app/component.bundle' に、バンドルにバンドルの実行可能ファイルが含まれていない.

この問題でき通常プロジェクトの名前付きのバンドルを検索して解決して、エラー メッセージは、次の提案のと同様の編集、`Info.plist`を削除して、バンドル内にある、`CFBundleExecutable`キー。 `CFBundlePackageType`にキーを設定する必要があります`BNDL`もします。

これらの変更を行った後は、クリーンをプロジェクト全体をリビルドします。 これらの変更を行った後、問題なく iTunes Connect に送信することができます。

詳細についてを参照してください[スタック オーバーフロー](http://stackoverflow.com/questions/32096130/unexpected-cfbundleexecutable-key)説明します。

## <a name="cfnetwork-sslhandshake-failed--9824-error"></a>CFNetwork SSLHandshake に失敗しました (-9824) エラー

直接、または iOS 9 の web ビューから、インターネットに接続するときに、フォームでエラーが発生する可能性があります。

```csharp
2015-09-04 14:38:05.757 FormsWebViewiOS[2553:30362] CFNetwork SSLHandshake failed (-9824)
2015-09-04 14:38:05.758 FormsWebViewiOS[2553:30363] NSURLSession/NSURLConnection HTTP load failed (kCFStreamErrorDomainSSL, -9824)
```

またはの形式で。

```csharp
2015-09-04 14:39:17.881 FormsWebViewiOS[2568:30974] App Transport Security has blocked a cleartext HTTP (http://) resource load since it is insecure.
Temporary exceptions can be configured via your app's Info.plist file.
```

IOS9 では、アプリのトランスポート セキュリティ (ATS) は、インターネット リソースの (アプリのバック エンド サーバーなど) と、アプリ間でセキュリティで保護された接続を強制します。 さらに、ATS が必要な通信、`HTTPS`プロトコルと転送の機密性と TLS バージョン 1.2 を使用して暗号化する高レベルの API 通信します。

使用するすべての接続を iOS 9 および OS X 10.11 (許可されて El) 用に開発されたアプリの既定で有効になって ATS のため`NSURLConnection`、`CFURL`または`NSURLSession`ATS セキュリティ要件に従うことになります。 接続にはこれらの要件を満たしていない場合は、例外で失敗します。

参照してください、 [ATS の Opting アウト](~/ios/app-fundamentals/ats.md)のセクションで、[アプリ トランスポート セキュリティ](~/ios/app-fundamentals/ats.md)この問題を解決する方法についてガイドします。

## <a name="my-existing-apps-dont-run-on-ios-9"></a>IOS 9 で、既存のアプリは実行しません。

参照してください、 [iOS 9 の互換性情報](~/ios/platform/introduction-to-ios9/ios9.md)および手順についての再構築、既存のアプリを再展開を iOS 9 で実行します。

<a name="UICollectionViewCell.ContentView-is-null-in-constructors" />

## <a name="uicollectionviewcellcontentview-is-null-in-constructors"></a>UICollectionViewCell.ContentView はコンス トラクター内の Null

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

<a name="UIView-fails-to-Init-with-Coder-when-Loading-a-View-from-a-Xib/Nib" />

## <a name="uiview-fails-to-init-with-coder-when-loading-a-view-from-a-xibnib"></a>Xib/Nib からビューを読み込むときに、コードの作成者を使って UIView が失敗します。

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

## <a name="dyld-message-no-cache-image-with-name"></a>Dyld メッセージ: キャッシュ イメージがない名前を持つしています.

ログに次の情報でクラッシュが発生する可能性があります。

```csharp
Dyld Error Message:
Dyld Message: no cach image with name (/System/Library/PrivateFrameworks/JavaScriptCore.framework/JavaScriptCore)
```

**理由:**これは、パブリック、プライベートのフレームワークを行うときに発生する Apple のネイティブ リンカーのバグ (JavaScriptCore 公開された iOS 7 で前にプライベート framework した)、あり、アプリの配置ターゲットを iOS 版の場合、フレームワークは、プライベートでした。 ここでは Apple のリンカーは、プライベート framework のバージョンで、公開されているバージョンの代わりにリンクします。

**修正:** ios 9、これに対応する予定しますが、ある、簡単な対応策が当面の間に自分で適用することができます。 (iOS 7 をここではに再試行することができます)、プロジェクトの後で iOS バージョンを対象にだけです。 その他のフレームワークは、類似した問題を示す可能性があります、たとえば WebKit フレームワークが 8、iOS で公開された (および iOS 8 を実行し、アプリ内の WebKit の使用の対象にする必要があります。 このエラーが発生、iOS 7 を対象とするように) します。

## <a name="untrusted-enterprise-developer"></a>信頼されていないエンタープライズ開発者

IOS 9 バージョン Xamarin.iOS アプリの実際の iOS のハードウェアを実行すると、デバイスで、開発者アカウントが信頼されていないことを示すメッセージを取得可能性があります。 例:

[![](troubleshooting-images/untrusted01.png "信頼されていないエンタープライズ開発者アラート")](troubleshooting-images/untrusted01.png#lightbox)

この問題を解決するには、次の操作を行います。

1. 開発ファルダ Xcode (最新のベータ版) を開始します。
2. 選択**デバイス**から、**ウィンドウ** メニューの デバイス ウィンドウを開きます。 

    [![](troubleshooting-images/untrusted02.png "[デバイス] ウィンドウ")](troubleshooting-images/untrusted02.png#lightbox)
3. 下にある、**デバイス**側のパネルで、デバイスを右クリックして選択を選択**プロビジョニング プロファイルを表示しています.**: 

    [![](troubleshooting-images/untrusted03.png "SShow プロビジョニング プロファイル")](troubleshooting-images/untrusted03.png#lightbox)
4. 現在、デバイス上をクリックして各プロビジョニング プロファイルを選択、  **-** 削除ボタンをクリックします。 

    [![](troubleshooting-images/untrusted04.png "プロビジョニング プロファイルの削除")](troubleshooting-images/untrusted04.png#lightbox)
5. **Xcode**メニューの **設定しています.**と**アカウント**: 

    [![](troubleshooting-images/untrusted05.png "Xcode アカウントの基本設定")](troubleshooting-images/untrusted05.png#lightbox)
6. クリックして、**の詳細を表示しています.**クリックして、**すべてダウンロード**ボタンをクリックします。 

    [![](troubleshooting-images/untrusted06.png "すべてのプロファイルをダウンロードします。")](troubleshooting-images/untrusted06.png#lightbox)
7. 一覧の更新が完了したらをクリックして、**完了**ボタンをクリックし、環境設定ウィンドウを閉じます。
8. IOS デバイスからテストしようとした Xamarin.iOS アプリの既存のバージョンを削除します。
9. Mac 用 Visual Studio に戻り、クリーン ビルドを実行およびデバイスでアプリを再実行しようとしてください。

停止し、Xcode によって読み込まれた新しいプロビジョニング プロファイルが表示される前に、Mac を Visual Studio を再起動する必要があります。 調整を設定することも、 **iOS バンドル署名** Xamarin.iOS アプリが新しいプロビジョニング プロファイルを選択するオプションです。

## <a name="launch-screen-issues"></a>起動画面の問題

向きが別のインターフェイスをサポートするためには、同じ起動イメージが不要になったに再利用できるように、iOS 9 は今すぐ起動画面の要件を強制します。 Apple を参照してください[UILanchImage 参照](https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html#//apple_ref/doc/uid/TP40009252-SW28)詳細についてはします。

セットを使用してではなく、アプリの起動画面を提示するストーリー ボード ファイルを使用する必要に応じて、 **.png**イメージ ファイル。 これは、Apple の起動画面を表示する方法優先ようになりました。 参照してください、 [Unified ストーリー ボードの概要](~/ios/user-interface/storyboards/unified-storyboards.md)についてガイドします。

最後に、アプリでは、その起動画面のストーリー ボード ファイルを使用し、すべての 4 つのインターフェイス向き (縦向き、上下縦、横左右ランドス ケープ) のスライド上のパネルで、または分割ビュー モードで実行されていると見なされるをサポートする必要があります。 IOS 9 の新しいマルチタスク機能の詳細についてを参照してください、 [iPad のマルチタスク](~/ios/platform/multitasking.md)ガイドです。

## <a name="nsinternalinconsistencyexception-exception"></a>NSInternalInconsistencyException 例外

コンパイルして、iOS 9 の既存の Xamarin.iOS アプリを実行しているときに、フォームでエラーが発生する可能性があります。

> Objective C の例外がスローされます。  [名前]: NSInternalInconsistencyException 理由: アプリケーションの起動の最後に、ルート ビュー コント ローラーに windows アプリケーションは必要

これは、エラーがアプリの Windows がアプリケーションの起動の最後に、ルート ビュー コント ローラーをいる必要があり、既存のアプリはために発生します。

この問題の考えられる回避策は、少なくとも 2 つあります。

1. 代わりに、ストーリー ボード ファイルを使用してアプリを更新`xib`ファイルをそのユーザー インターフェイスを定義します。 この 1 つには、ストーリー ボードのレイアウトに非常に多くの時間によっては、アプリのサイズと、iOS デザイナー (Xcode のインターフェイスのビルダー) を使用する方法に関する知識が必要です。 詳細については、次を参照してください。 この[Unified ストーリー ボードの概要](~/ios/user-interface/storyboards/unified-storyboards.md)ドキュメント。
2. セットアップ`RootViewController`アプリ ウィンドウのプロパティで`FinishedLaunching`メソッド`AppDelegate`アプリの UI にビュー コント ローラー をポイントするクラス。

## <a name="when-to-initialize-views-and-view-controllers"></a>初期化のビューとコント ローラーの表示するタイミング

Xamarin.iOS とは、マネージ コードに公開されるものが、iOS のデザインが壊れた場合と呼ばれるコンス トラクター内のビューまたはビュー コント ローラーの初期化することができます。

一般にする必要がありますいないを初期化する何も呼び出すことができる戻る Objective C コード コンス トラクターからすることを確認することはできませんのでときに呼び出されます。 改善の場所 (その他の .ctor) があることも意味オーバーライド (Objective C には、イベントがあるない) を呼び出し、またはこの初期化を行う必要があります。



## <a name="related-links"></a>関連リンク

- [開発者向けの iOS 9](https://developer.apple.com/ios/pre-release/)
- [新機能 iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [IOS9 (ビデオ) を Xamarin.iOS アプリの更新](https://university.xamarin.com/lightninglectures/Updating-your-XamariniOS-apps-to-iOS9)
