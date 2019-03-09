---
title: 'Xamarin.iOS 9: トラブルシューティング'
description: この記事では、Xamarin.iOS では、iOS 9 を操作するためのさまざまなトラブルシューティングのヒントを提供します。 ヒントは、XML の解析、シミュレーター、レイアウトの制約、ネットワークの問題、およびその他のトピックについて説明します。
ms.prod: xamarin
ms.assetid: DCE83E36-CBD9-4D96-8E7F-384CB8A54563
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/20/2017
ms.openlocfilehash: f8fae79af654339b54a8df0d2ea32eef38f34adb
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/08/2019
ms.locfileid: "57668453"
---
# <a name="xamarinios-9--troubleshooting"></a>Xamarin.iOS 9: トラブルシューティング

_この記事では、Xamarin.iOS アプリでの iOS 9 を操作するためのトラブルシューティングのヒントを提供します。_

## <a name="there-was-a-problem-parsing-the-xml"></a>XML の解析に問題が発生しました

Xamarin iOS デザイナーは、Xcode 7 の機能をまだサポートしていません。 ストーリー ボードのデザイナーで読み込みに失敗 _「XML の解析に問題が発生しました」_ 新しい iOS 9 (Xcode 7) を使用すると StackView などのデザイナーの要素。

iOS Xcode 7 の機能のデザイナー サポートは、サイクル 6 機能の今後のリリースの対象にします。 サイクル 6 のプレビュー バージョンは、アルファ チャネルで現在使用できる新しい Xcode 7 の機能のサポートが限られています。

Visual Studio for Mac の部分的な回避策:ストーリー ボードを右クリックし、**プログラムから開く** > **Xcode の Interface Builder**します。

## <a name="where-are-the-ios-8-simulators"></a>IOS 8 シミュレーターか。

Xcode 7 (またはそれ以上) がそのは自動的にインストールされている場合は、既定では 9 の iOS シミュレーターで置き換えます iOS 8 シミュレーターのすべて。 IOS 8 でテストする場合は、Xcode を起動ダウンロードし、iOS 8 シミュレーターをインストールしてできます。

Xcode で、選択、 **Xcode**メニュー、**設定しています.**  > **ダウンロード**:

[![](troubleshooting-images/ios8.png "iOS 8 シミュレーターをダウンロードします。")](troubleshooting-images/ios8.png#lightbox)

をクリックして、**チェックし、今すぐインストール**iOS 8 シミュレーターを再インストールするボタンをクリックします。

## <a name="layout-constraint-with-leftright-attribute-errors"></a>左/右の属性エラーのレイアウトの制約

IOS 8 (および以前) では、ストーリー ボード内の UI 要素が両方の組み合わせを使用できます**右** & **左**属性 (`NSLayoutAttributeRight` & `NSLayoutAttributeLeft`) と**先頭** & **トレーリング**属性 (`NSLayoutAttributeLeading` & `NSLayoutAttributeTrailing`) 同じレイアウトにします。

ストーリー ボードと同じで、iOS 9 を実行している場合、例外が次の形式になります。

> キャッチされない例外 'NSInvalidArgumentException' のためのアプリを終了するには、理由: ' * * * + [NSLayoutConstraint constraintWithItem:attribute:relatedBy:toItem:attribute:multiplier:constant:]。制約は、先頭および末尾の属性と左/属性の間にことはできません。 先頭および末尾の両方またはどちらも使用します '。

iOS 9 がいずれかを使用するレイアウトを適用**右** & **左**_または_**先頭** &  **末尾の**属性が*いない*両方。 この問題を解決するには、ストーリー ボード ファイル内で設定された同じ属性を使用するすべてのレイアウトの制約を変更します。

詳細についてを参照してください、 [iOS 9 制約エラー](https://stackoverflow.com/questions/32692841/ios-9-constraint-error)スタック オーバーフローのディスカッション。

## <a name="error-itms-90535-unexpected-cfbundleexecutable-key"></a>エラー ITMS-90535:予期しない CFBundleExecutable キー

IOS 9 への切り替え後からアプリをコンパイルし、iTunes Connect の形式でエラーが発生することができますを新しいビルドを送信しようとするときに、iOS 8 (またはそれ以前) で実行するサード パーティ製コンポーネント (具体的には、既存 Google Maps コンポーネント) を使用します。

> エラー ITMS-90535:予期しない CFBundleExecutable キー。 'Payload/app-name.app/component.bundle' でバンドルにはバンドルの実行可能ファイルが含まれていない.

この問題を通常は、プロジェクト内の名前付きのバンドルの検索で解決して - エラー メッセージの指示のと同様の編集、`Info.plist`削除することで、バンドル内にある、`CFBundleExecutable`キー。 `CFBundlePackageType`にキーを設定する必要があります`BNDL`もします。

これらの変更を行った後は、クリーンし、プロジェクト全体をリビルドします。 これらの変更を行った後、問題なく iTunes Connect に送信する必要があります。

詳細についてを参照してください[Stack Overflow](https://stackoverflow.com/questions/32096130/unexpected-cfbundleexecutable-key)について説明します。

## <a name="cfnetwork-sslhandshake-failed--9824-error"></a>CFNetwork SSLHandshake に失敗しました (-9824) エラー

直接、または iOS 9 の web ビューから、インターネットに接続するときに、フォームでエラーが発生する可能性があります。

```csharp
2015-09-04 14:38:05.757 FormsWebViewiOS[2553:30362] CFNetwork SSLHandshake failed (-9824)
2015-09-04 14:38:05.758 FormsWebViewiOS[2553:30363] NSURLSession/NSURLConnection HTTP load failed (kCFStreamErrorDomainSSL, -9824)
```

または、フォーム。

```csharp
2015-09-04 14:39:17.881 FormsWebViewiOS[2568:30974] App Transport Security has blocked a cleartext HTTP (http://) resource load since it is insecure.
Temporary exceptions can be configured via your app's Info.plist file.
```

Ios 9、App Transport Security (ATS) は、インターネット リソース (アプリのバック エンド サーバーなど) と、アプリの間のセキュリティで保護された接続を強制します。 さらに、ATS でを使用して通信が必要です、`HTTPS`プロトコルと TLS バージョン 1.2 を使用して、前方の機密性を暗号化する高度な API の通信。

ATS が iOS 9 および OS X 10.11 (El Capitan) を使用してすべての接続に開発されたアプリの既定で有効になっているため`NSURLConnection`、`CFURL`または`NSURLSession`ATS セキュリティ要件が適用されます。 接続はこれらの要件を満たしていない場合は、例外で失敗します。

参照してください、 [Opting アウト ATS の](~/ios/app-fundamentals/ats.md)のセクション、[アプリ トランスポート セキュリティ](~/ios/app-fundamentals/ats.md)この問題を解決する方法についてガイドします。

## <a name="my-existing-apps-dont-run-on-ios-9"></a>IOS 9 で既存のマイ アプリは実行しません。

参照してください、 [iOS 9 の互換性情報](~/ios/platform/introduction-to-ios9/ios9.md)iOS 9 上で実行する手順については再構築と、既存のアプリを再デプロイします。

<a name="UICollectionViewCell.ContentView-is-null-in-constructors" />

## <a name="uicollectionviewcellcontentview-is-null-in-constructors"></a>UICollectionViewCell.ContentView はコンス トラクター内の Null

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

<a name="UIView-fails-to-Init-with-Coder-when-Loading-a-View-from-a-Xib/Nib" />

## <a name="uiview-fails-to-init-with-coder-when-loading-a-view-from-a-xibnib"></a>Xib/Nib からビューの読み込み時にエンコーダーを使って UIView が失敗しました。

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

## <a name="dyld-message-no-cache-image-with-name"></a>Dyld メッセージ:名前のキャッシュはイメージがありません.

ログに次の情報でクラッシュが発生する可能性があります。

```csharp
Dyld Error Message:
Dyld Message: no cach image with name (/System/Library/PrivateFrameworks/JavaScriptCore.framework/JavaScriptCore)
```

**理由:** これは、パブリック、プライベートのフレームワークを行うときは、Apple のネイティブ リンカーのバグ (JavaScriptCore がパブリックになった iOS 7 で前に、プライベートのフレームワークが)、アプリのデプロイ ターゲットは、フレームワークがプライベートの場合、iOS 版は。 ここで Apple のリンカーは、パブリック バージョンの代わりに、フレームワークのプライベート バージョンとリンクされます。

**修正:** Ios 9 の場合、これは解決されますが、回避策が簡単であることをそれまでは自分で適用することができます。 (iOS 7 をこの場合に試行することができます)、プロジェクトに後で iOS バージョンを対象にだけです。 他のフレームワークと同様の問題が発生する可能性があります、たとえば、WebKit framework は iOS 8 で公開された (およびため、このエラーの結果は iOS 7 を対象とする iOS アプリで WebKit を使用する 8 をターゲットする必要があります)。

## <a name="untrusted-enterprise-developer"></a>信頼されていないエンタープライズ開発

実際の iOS のハードウェア上で iOS 9 のバージョンの Xamarin.iOS アプリを実行するときを開発者アカウントが、デバイスで信頼されていないことを示すメッセージを取得可能性があります。 例:

[![](troubleshooting-images/untrusted01.png "信頼されていない企業の開発者のアラート")](troubleshooting-images/untrusted01.png#lightbox)

この問題を解決するには、次の操作を行います。

1. 開発ファルダに Xcode (最新のベータ版) を開始します。
2. 選択**デバイス**から、**ウィンドウ** メニューの デバイス ウィンドウを開きます。 

    [![](troubleshooting-images/untrusted02.png "[デバイス] ウィンドウ")](troubleshooting-images/untrusted02.png#lightbox)
3. 下、**デバイス**側のパネルで、デバイス、右クリックして選択を選択します**プロビジョニング プロファイルを表示しています.**: 

    [![](troubleshooting-images/untrusted03.png "SShow プロビジョニング プロファイル")](troubleshooting-images/untrusted03.png#lightbox)
4. 各プロビジョニング プロファイルをクリックして、デバイスで現在の選択、 **-** を削除するボタンをクリックします。 

    [![](troubleshooting-images/untrusted04.png "プロビジョニング プロファイルを削除します。")](troubleshooting-images/untrusted04.png#lightbox)
5. **Xcode**メニューの **設定しています.** と**アカウント**: 

    [![](troubleshooting-images/untrusted05.png "Xcode アカウントの基本設定")](troubleshooting-images/untrusted05.png#lightbox)
6. をクリックして、**の詳細を表示しています.** 、クリックして、**すべてダウンロード**ボタンをクリックします。 

    [![](troubleshooting-images/untrusted06.png "すべてのプロファイルをダウンロードします。")](troubleshooting-images/untrusted06.png#lightbox)
7. 一覧の更新が完了したら、クリックして、**完了**ボタンをクリックし、[preferences] ウィンドウを閉じます。
8. IOS デバイスからテストしようとして Xamarin.iOS アプリの既存のバージョンを削除します。
9. For Mac に Visual Studio に戻り、クリーン ビルドを行うし、デバイスでアプリを再実行しようとしてください。

停止して Xcode によって読み込まれる新しいプロビジョニング プロファイルが表示される前に、Mac に Visual Studio を再起動する必要があります。 調整を設定することも、 **iOS バンドル署名** Xamarin.iOS アプリが新しいプロビジョニング プロファイルを選択するオプションです。

## <a name="launch-screen-issues"></a>起動画面の問題

iOS 9 は、さまざまなインターフェイスの向きをサポートするためには同じ起動イメージが不要に再利用できるようにここで、起動画面の要件を適用します。 Apple を参照してください。 [UILanchImage 参照](https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html#//apple_ref/doc/uid/TP40009252-SW28)詳細についてはします。

セットを使用してではなく、アプリの起動画面を提示するストーリー ボード ファイルを使用できます必要に応じて、 **.png**イメージ ファイル。 これは、Apple の起動画面を表示する方法に優先ようになりました。 参照してください、 [Unified ストーリー ボードの概要](~/ios/user-interface/storyboards/unified-storyboards.md)詳細情報をガイドします。

最後に、アプリは、起動画面のストーリー ボード ファイルを使用してし、(縦、上下縦、横左右ランドス ケープ) のスライドの上パネルまたは分割ビュー モードで実行されていると見なされるすべての 4 つのインターフェイスの向きをサポートする必要があります。 IOS 9 のマルチタス キングの新機能に関する詳細については、次を参照してください、 [iPad のマルチタス キング](~/ios/platform/multitasking.md)ガイド。

## <a name="nsinternalinconsistencyexception-exception"></a>NSInternalInconsistencyException 例外

コンパイルして、iOS 9 の既存の Xamarin.iOS アプリを実行しているときに、フォームでエラーが発生する可能性があります。

> Objective C 例外がスローされます。  名前:NSInternalInconsistencyException 理由:ルート ビュー コント ローラーがあるアプリケーションの最後に起動するアプリケーションの windows が必要です。

これは、エラーがそのアプリの Windows がアプリケーションの起動の最後のルート ビュー コント ローラーを使用して予測され、既存のアプリがあるために発生します。

この問題の少なくとも 2 つの可能な回避策があります。

1. 代わりに、ストーリー ボード ファイルを使用してアプリを更新`xib`ファイルをそのユーザー インターフェイスを定義します。 この 1 つには、ストーリー ボードのレイアウトに非常に多くの時間によっては、アプリのサイズと、iOS デザイナー (または、Xcode の Interface Builder) を使用する方法についての知識が必要です。 詳細については、次を参照してください。 この[Unified ストーリー ボードの概要](~/ios/user-interface/storyboards/unified-storyboards.md)ドキュメント。
2. セットアップ`RootViewController`アプリ ウィンドウのプロパティで`FinishedLaunching`メソッド`AppDelegate`クラスで、アプリケーションの UI のビュー コント ローラーを指すようにします。

## <a name="when-to-initialize-views-and-view-controllers"></a>Initialize ビューとビュー コント ローラーするタイミング

Xamarin.iOS は、マネージ コードに公開されるものが、iOS のデザインを中断するときに呼び出されるコンス トラクター内のビューまたはビュー コント ローラーの初期化を行います。

一般にないから呼び出すことが戻る Objective C コード コンス トラクターは、することを確認することはできませんので何も初期化する必要があるときに呼び出されます。 良い場所 (その他の .ctor) があることも意味または呼び出し (、OBJECTIVE-C では、イベントを持たない) をオーバーライドするこの初期化を行う必要があります。

## <a name="related-links"></a>関連リンク

- [iOS 9 開発者向け](https://developer.apple.com/ios/pre-release/)
- [IOS 9.0 を新します。](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Ios 9 (ビデオ) に、Xamarin.iOS アプリを更新しています](https://university.xamarin.com/lightninglectures/Updating-your-XamariniOS-apps-to-iOS9)
