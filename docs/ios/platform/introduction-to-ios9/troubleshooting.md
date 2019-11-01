---
title: Xamarin iOS 9 –トラブルシューティング
description: この記事では、Xamarin の ios 9 を操作するためのさまざまなトラブルシューティングのヒントについて説明します。 XML 解析、シミュレーター、レイアウトの制約、ネットワークの問題、およびその他多くのトピックに関するヒントを紹介します。
ms.prod: xamarin
ms.assetid: DCE83E36-CBD9-4D96-8E7F-384CB8A54563
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/20/2017
ms.openlocfilehash: 437698fcda6e85090cd7bdce90959300436e0bc2
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031754"
---
# <a name="xamarinios-9--troubleshooting"></a>Xamarin iOS 9 –トラブルシューティング

_この記事では、Xamarin iOS アプリで iOS 9 を操作するためのトラブルシューティングのヒントをいくつか紹介します。_

## <a name="there-was-a-problem-parsing-the-xml"></a>XML の解析中に問題が発生しました

Xamarin iOS Designer では、Xcode 7 の機能はまだサポートされていません。 StackView などの新しい iOS 9 (Xcode 7) デザイナー要素を使用しようとすると、ストーリーボードがデザイナーに _"XML の解析で問題が発生しました"_ というエラーが表示されます。

iOS Designer による Xcode 7 機能のサポートは、今後のサイクル6機能リリースを対象としています。 サイクル6のプレビューバージョンは現在、アルファチャネルで使用でき、新しい Xcode 7 機能のサポートは制限されています。

Visual Studio for Mac の部分的な回避策: ストーリーボードを右クリックし、[ > **Xcode Interface Builder** **で開く**] を選択します。

## <a name="where-are-the-ios-8-simulators"></a>IOS 8 シミュレータはどこにありますか。

Xcode 7 (またはそれ以降) がインストールされている場合、既定ではすべての iOS 8 シミュレーターが iOS 9 シミュレーターに自動的に置き換えられます。 それでも iOS 8 でテストする必要がある場合は、Xcode を起動し、iOS 8 シミュレーターをダウンロードしてインストールします。

Xcode で、 **[Xcode]** 、 **[Preferences...]**  >  **[ダウンロード]** の順に選択します。

[![](troubleshooting-images/ios8.png "iOS 8 Simulators Downloads")](troubleshooting-images/ios8.png#lightbox)

[**今すぐ確認してインストール**する] ボタンをクリックして、iOS 8 シミュレーターを再インストールします。

## <a name="layout-constraint-with-leftright-attribute-errors"></a>左/右の属性エラーがあるレイアウトの制約

IOS 8 (およびそれ以前) では、ストーリーボードの UI 要素は、**右** & **左側**の属性 (`NSLayoutAttributeRight` & `NSLayoutAttributeLeft`) と**先頭**の ** & の**属性 (`NSLayoutAttributeLeading` & `NSLayoutAttributeTrailing`) の両方を組み合わせて使用できます。同じレイアウト。

同じストーリーボードが iOS 9 で実行されている場合、次の形式で例外が発生します。

> キャッチされていない例外が発生したため、アプリを終了しています。 ' NSInvalidArgumentException '、理由: ' * * * + [NSLayoutConstraint constraintWithItem: 属性: 関連付け: toItem: 属性: 乗数: 定数:]: 先頭と末尾の間で制約を設定することはできません属性と右/左の属性。 またはのどちらにも先頭と末尾を使用します。

iOS 9 では、右 & **右側**の**属性**また**は** **先頭**の & を使用するようにレイアウトが適用されますが、両方を使用することはでき*ません*。 この問題を解決するには、ストーリーボードファイル内で同じ属性セットを使用するように、すべてのレイアウト制約を変更します。

詳細については、「 [iOS 9 の制約エラー](https://stackoverflow.com/questions/32692841/ios-9-constraint-error) Stack Overflow」を参照してください。

## <a name="error-itms-90535-unexpected-cfbundleexecutable-key"></a>エラー ITMS-90535: 予期しない CFBundleExecutable キー

Ios 9 への切り替え後、アプリから iOS 8 (またはそれ以前) でコンパイルおよび実行されたサードパーティ製のコンポーネント (具体的には、既存の Google Maps コンポーネント) を使用する場合、新しいビルドを iTunes Connect に送信しようとすると、次の形式でエラーが発生する可能性があります。

> エラー ITMS-90535: 予期しない CFBundleExecutable キーです。 ' Payload/app-name/component. バンドル ' のバンドルにバンドル実行可能ファイルが含まれていません...

この問題は、通常、プロジェクト内の名前付きバンドルを検索することによって解決できます。これは、エラーメッセージに示されているように、`CFBundleExecutable` キーを削除することにより、バンドル内の `Info.plist` が編集されたことを示します。 `CFBundlePackageType` キーも `BNDL` に設定する必要があります。

これらの変更を行った後、プロジェクト全体をクリーンにしてリビルドします。 これらの変更を行った後は、問題なく iTunes Connect に送信できます。

詳細については、この[Stack Overflow](https://stackoverflow.com/questions/32096130/unexpected-cfbundleexecutable-key)の説明を参照してください。

## <a name="cfnetwork-sslhandshake-failed--9824-error"></a>CFNetwork SSLHandshake に失敗しました (-9824) エラー

直接、または iOS 9 の web ビューからインターネットに接続しようとすると、次の形式でエラーが表示されることがあります。

```csharp
2015-09-04 14:38:05.757 FormsWebViewiOS[2553:30362] CFNetwork SSLHandshake failed (-9824)
2015-09-04 14:38:05.758 FormsWebViewiOS[2553:30363] NSURLSession/NSURLConnection HTTP load failed (kCFStreamErrorDomainSSL, -9824)
```

またはの形式で、次のようになります。

```csharp
2015-09-04 14:39:17.881 FormsWebViewiOS[2568:30974] App Transport Security has blocked a cleartext HTTP (http://) resource load since it is insecure.
Temporary exceptions can be configured via your app's Info.plist file.
```

IOS9 では、アプリトランスポートセキュリティ (ATS) は、インターネットリソース (アプリのバックエンドサーバーなど) とアプリの間にセキュリティで保護された接続を適用します。 さらに、ATS では、`HTTPS` プロトコルと高レベルの API 通信を使用した通信が、転送機密性を持つ TLS バージョン1.2 を使用して暗号化されている必要があります。

IOS 9 および OS X 10.11 (El Capitan) 用に構築されたアプリでは、ATS が既定で有効になっているため、`NSURLConnection`、`CFURL` または `NSURLSession` を使用するすべての接続は、ATS のセキュリティ要件の対象となります。 接続がこれらの要件を満たしていない場合、例外が発生して失敗します。

この問題を解決する方法の詳細については、[アプリトランスポートセキュリティ](~/ios/app-fundamentals/ats.md)ガイドの「 [ATS のオプトアウト](~/ios/app-fundamentals/ats.md)」セクションを参照してください。

## <a name="my-existing-apps-dont-run-on-ios-9"></a>既存のアプリが iOS 9 で動作しない

IOS 9 で実行する既存のアプリの再構築と再デプロイの手順については、 [ios 9 の互換性に関する情報](~/ios/platform/introduction-to-ios9/ios9.md)を参照してください。

<a name="UICollectionViewCell.ContentView-is-null-in-constructors" />

## <a name="uicollectionviewcellcontentview-is-null-in-constructors"></a>コンストラクターの UICollectionViewCell が Null です

**理由:** IOS 9 では、`initWithFrame:` のコンストラクターが必要になりました。これは、 [UICollectionView ドキュメントの状態](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UICollectionView_class/#//apple_ref/occ/instm/UICollectionView/dequeueReusableCellWithReuseIdentifier:forIndexPath)として、ios 9 の動作が変更されたためです。 指定された識別子に対してクラスを登録し、新しいセルを作成する必要がある場合、そのセルは `initWithFrame:` メソッドを呼び出すことによって初期化されるようになりました。

**修正:** 次のように `initWithFrame:` コンストラクターを追加します。

```csharp
[Export ("initWithFrame:")]
public YourCellClassName (CGRect frame) : base (frame)
{
    Initialize (); // refactor initialize code into a method
}
```

関連サンプル: [Motiongraph](https://github.com/xamarin/monotouch-samples/commit/3c1b7a4170c001e7290db9babb2b7a6dddeb8bcb)、 [TextKitDemo](https://github.com/xamarin/monotouch-samples/commit/23ea01b37326963b5ebf68bbcc1edd51c66a28d6)

<a name="UIView-fails-to-Init-with-Coder-when-Loading-a-View-from-a-Xib/Nib" />

## <a name="uiview-fails-to-init-with-coder-when-loading-a-view-from-a-xibnib"></a>Xib/Nib からビューを読み込むときに、UIView がプログラマによる初期化に失敗する

**理由:** `initWithCoder:` コンストラクターは、Interface Builder Xib ファイルからビューを読み込むときに呼び出されるコンストラクターです。 このコンストラクターがエクスポートされていない場合、アンマネージコードはマネージバージョンのマネージドバージョンを呼び出すことができません。 以前 ( iOS 8 では、`IntPtr` コンストラクターがビューを初期化するために呼び出されました。

**修正:** 次のように `initWithCoder:` コンストラクターを作成してエクスポートします。

```csharp
[Export ("initWithCoder:")]
public YourClassName (NSCoder coder) : base (coder)
{
    Initialize (); // refactor initialize code into a method
}
```

関連サンプル:[チャット](https://github.com/xamarin/monotouch-samples/commit/7b81138d52e5f3f1aa3769fcb08f46122e9b6a88)

## <a name="dyld-message-no-cache-image-with-name"></a>Dyld メッセージ: 名前のキャッシュイメージがありません...

ログに次の情報が表示された場合、クラッシュが発生する可能性があります。

```csharp
Dyld Error Message:
Dyld Message: no cach image with name (/System/Library/PrivateFrameworks/JavaScriptCore.framework/JavaScriptCore)
```

**理由:** これは、Apple のネイティブリンカーのバグであり、プライベートフレームワークをパブリックにすると (JavaScriptCore は iOS 7 で公開されていましたが、プライベートフレームワークではなくなりました)、アプリのデプロイターゲットは、フレームワークがプライベートであるときの iOS バージョン用です。 この場合、Apple のリンカーは、パブリックバージョンではなく、フレームワークのプライベートバージョンとリンクします。

**修正:** これは iOS 9 に対応していますが、その間にすぐに適用できる簡単な回避策があります。 (この場合は、iOS 7 を試すことができます)、後で簡単に適用できます。 他のフレームワークでも同様の問題が発生する可能性があります。たとえば、WebKit フレームワークは iOS 8 で公開されています (したがって、iOS 7 を対象とすると、このエラーが発生します。アプリで WebKit を使用するには、iOS 8 を対象にする必要があります)。

## <a name="untrusted-enterprise-developer"></a>信頼されていないエンタープライズ開発者

Ios 9 バージョンの Xamarin iOS アプリを実際の iOS ハードウェアで実行しようとすると、開発者アカウントがデバイス上で信頼されていないというメッセージが表示されることがあります。 (例:

[![](troubleshooting-images/untrusted01.png "Untrusted Enterprise Developer alert")](troubleshooting-images/untrusted01.png#lightbox)

この問題を解決するには、次の手順を実行します。

1. 開発用 Mac で Xcode (最新のベータ版) を開始します。
2. **ウィンドウ** メニューの **デバイス** を選択して、デバイス ウィンドウを開きます。 

    [![](troubleshooting-images/untrusted02.png "The Devices Window")](troubleshooting-images/untrusted02.png#lightbox)
3. **[デバイス]** サイドパネルで、デバイスを選択し、右クリックして、 **[プロビジョニングプロファイルの表示]** を選択します。 

    [![](troubleshooting-images/untrusted03.png "SShow Provisioning Profiles")](troubleshooting-images/untrusted03.png#lightbox)
4. 現在デバイス上にある各プロビジョニングプロファイルを選択し、[ **-** ] ボタンをクリックして削除します。 

    [![](troubleshooting-images/untrusted04.png "Deleting a provisioning profile")](troubleshooting-images/untrusted04.png#lightbox)
5. **[Xcode]** メニューの **[基本設定...]** を選択し、 **[アカウント]** を選択します。 

    [![](troubleshooting-images/untrusted05.png "Xcode account preferences")](troubleshooting-images/untrusted05.png#lightbox)
6. **[詳細の表示...]** ボタンをクリックし、 **[すべてダウンロード]** ボタンをクリックします。 

    [![](troubleshooting-images/untrusted06.png "Download all profiles")](troubleshooting-images/untrusted06.png#lightbox)
7. 一覧の更新が完了したら、**完了** ボタンをクリックして、設定 ウィンドウを閉じます。
8. IOS デバイスからテストしようとしていた既存のバージョンの Xamarin iOS アプリを削除します。
9. Visual Studio for Mac に戻り、クリーンビルドを実行して、デバイスでアプリを再実行します。

Xcode によって読み込まれた新しいプロビジョニングプロファイルが表示される前に、Visual Studio for Mac を停止して再起動することが必要になる場合があります。 また、Xamarin iOS アプリの**Ios バンドル署名**オプションを調整して、新しいプロビジョニングプロファイルを選択することが必要になる場合もあります。

## <a name="launch-screen-issues"></a>起動画面の問題

iOS 9 では、起動画面の要件が適用されるようになりました。これにより、異なるインターフェイスの向きをサポートするために同じ起動イメージを再利用できなくなります。 詳細については、「Apple の[UILanchImage リファレンス](https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html#//apple_ref/doc/uid/TP40009252-SW28)」を参照してください。

必要に応じて、一連の **.png**イメージファイルを使用するのではなく、ストーリーボードファイルを使用してアプリの起動画面を表示できます。 これは、Apple が起動画面を表示するために推奨する方法です。 詳細については、統合された[ストーリーボードの概要に](~/ios/user-interface/storyboards/unified-storyboards.md)関するガイドを参照してください。

最後に、アプリでは、起動画面にストーリーボードファイルを使用し、4つのインターフェイスの向き (縦、反転、縦、横、横、横) をすべてサポートする必要があります。 IOS 9 の新しいマルチタスク機能の詳細については、「 [iPad 用のマルチタスキング](~/ios/platform/multitasking.md)」ガイドを参照してください。

## <a name="nsinternalinconsistencyexception-exception"></a>NSInternalInconsistencyException 例外

IOS 9 用の既存の Xamarin iOS アプリをコンパイルして実行すると、次の形式でエラーが表示される場合があります。

> 目標-C 例外がスローされました。  名前: NSInternalInconsistencyException Reason: アプリケーションウィンドウには、アプリケーションの起動の最後にルートビューコントローラーが必要です

このエラーが発生しているのは、アプリケーションの起動の最後にアプリウィンドウにルートビューコントローラーがあることが想定されていて、既存のアプリが存在しないためです。

この問題には、少なくとも2つの回避策があります。

1. `xib` ファイルの代わりにストーリーボードファイルを使用してユーザーインターフェイスを定義するようにアプリを更新します。 これには、アプリのサイズによっては非常に多くの時間が必要であり、iOS デザイナー (または Xcode の Interface Builder) を使用してストーリーボードをレイアウトする方法についての知識が必要です。 詳細については、統合されたストーリーボードのドキュメントの[概要に](~/ios/user-interface/storyboards/unified-storyboards.md)関する記事をご覧ください。
2. アプリの UI のビューコントローラーを指すように `AppDelegate` クラスの `FinishedLaunching` メソッドで、アプリウィンドウの `RootViewController` プロパティを設定します。

## <a name="when-to-initialize-views-and-view-controllers"></a>ビューを初期化してコントローラーを表示する場合

Xamarin を使用すると、マネージコードに何かが公開されるときに呼び出されるコンストラクター内で、ビューまたはビューコントローラーの初期化を行うことができますが、iOS の設計は中断されます。

一般に、このコンストラクターが呼び出されるタイミングを確認することはできないため、このコンストラクターから目的の C コードを呼び出すことができるすべてのものを初期化することは避けてください。 また、この初期化を行う必要があることを意味します (他の .ctor)、またはオーバーライドの呼び出し (目標 C にイベントがない場合) があることも意味します。

## <a name="related-links"></a>関連リンク

- [iOS 9 (開発者向け)](https://developer.apple.com/ios/pre-release/)
- [IOS 9.0 の新機能](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Xamarin の iOS アプリを iOS9 に更新する (ビデオ)](https://university.xamarin.com/lightninglectures/Updating-your-XamariniOS-apps-to-iOS9)
