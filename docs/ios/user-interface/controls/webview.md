---
title: Xamarin.iOS の Web ビュー
description: このドキュメントでは、Xamarin.iOS アプリで Web コンテンツを表示するさまざまな方法について説明します。 WKWebView、SFSafariViewコントローラー、サファリ、およびアプリトランスポートセキュリティについて説明します。
ms.prod: xamarin
ms.assetid: 84886CF4-2B2B-4540-AD92-7F0B791952D1
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/22/2017
ms.openlocfilehash: cc6c87452bf5312d66e33b7c49d3dc216eceb7d6
ms.sourcegitcommit: b93754b220fca3d6e3d131341e3cfbe233d10f84
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/03/2020
ms.locfileid: "80628292"
---
# <a name="web-views-in-xamarinios"></a>Xamarin.iOS の Web ビュー

iOS Apple の存続期間の間に、アプリ開発者が Web ビュー機能をアプリに組み込む方法が数多くリリースされました。 ほとんどのユーザーは、iOS デバイスで組み込みの Safari Web ブラウザーを使用しているため、他のアプリの Web ビュー機能がこのエクスペリエンスと一致することを期待しています。 彼らは、同じジェスチャーが動作し、パフォーマンスが同等であり、機能が同じであることを期待しています。

iOS 11 は、両方`WKWebView`に`SFSafariViewController`新しい変更を導入しました。 詳細については[、iOS 11 ガイドガイドの Web の変更を](~/ios/platform/introduction-to-ios11/web.md)参照してください。

## <a name="wkwebview"></a>WKWeb ビュー

`WKWebView`iOS 8で導入され、アプリ開発者はモバイルSafariと同様のWebブラウジングインターフェイスを実装することができました。 これは、部分的には、Nitro Javascriptエンジン`WKWebView`、モバイルサファリで使用されるのと同じエンジンを使用しているという事実によるものです。 `WKWebView`パフォーマンスの向上、ユーザーフレンドリーなジェスチャの組み込み、および Web ページとアプリ間の操作の容易さにより、可能な限り UIWebView を介して常に使用する必要があります。

`WKWebView`UIWebView とほぼ同じ方法でアプリに追加できますが、開発者は UI/UX と機能を大幅に制御できます。 Web ビュー オブジェクトを作成して表示すると、要求されたページが表示されますが、ビューの表示方法、ユーザーの移動方法、およびユーザーがビューを終了する方法を制御できます。  

以下のコードを使用して、Xamarin.iOS アプリで を`WKWebView`起動できます。

```csharp
WKWebView webView = new WKWebView(View.Frame, new WKWebViewConfiguration());
View.AddSubview(webView);

var url = new NSUrl("https://docs.microsoft.com");
var request = new NSUrlRequest(url);
webView.LoadRequest(request);
```

`WebKit`名前空間にあることに`WKWebView`注意することが重要なので、この using ディレクティブをクラスの先頭に追加する必要があります。

`WKWebView`Xamarin.Mac アプリでも使用でき、クロスプラットフォーム Mac/iOS アプリを作成する場合は、それを使用する必要があります。

[処理 JavaScript アラート](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/web_view/handle_javascript_alerts)のレシピでは、JavaScript での WKWebView の使用に関する情報も提供します。

## <a name="sfsafariviewcontroller"></a>SFSafariViewController

 `SFSafariViewController`は、アプリから Web コンテンツを提供する最新の方法であり、iOS 9 以降で利用できます。 と`UIWebView`は`WKWebView`異`SFSafariViewController`なり、 はビュー コントローラーであるため、他のビューでは使用できません。 ビュー コント`SFSafariViewController`ローラーを表示するのと同じ方法で、新しいビュー コント ローラーとして表示する必要があります。

 `SFSafariViewController`は本質的にアプリに埋め込むことができる「ミニサファリ」です。 WKWebViewと同様に、それは同じニトロJavascriptエンジンを使用していますが、オートフィル、リーダー、モバイルサファリとクッキーやデータを共有する機能などの追加のサファリ機能の範囲を提供します。 ユーザーと の間の`SFSafariViewController`対話は、アプリからアクセスできません。 アプリは、Safari のデフォルトの機能にアクセスできません。

また、既定では **[完了]** ボタンが実装されており、ユーザーがアプリに簡単に戻り、ナビゲーション ボタンを転送および戻り、ユーザーが Web ページのスタック内を移動できるようにします。 さらに、ユーザーに、期待される Web ページに表示される安心感を与えるアドレス バーを提供します。 アドレス バーでは、ユーザーが URL を変更することはできません。

これらの実装は変更できないため、カスタマイズせずに`SFSafariViewController`Web ページを表示する場合は、既定のブラウザーとして使用するのが理想的です。

以下のコードを使用して、Xamarin.iOS アプリで を`SFSafariViewController`起動できます。

```csharp
var sfViewController = new SFSafariViewController(url);

PresentViewController(sfViewController, true, null);
```

これにより、次の Web ビューが作成されます。

[![サンプル Web ビューと SFSafariView コントローラー](webview-images/sfsafariviewcontroller.png)](webview-images/sfsafariviewcontroller.png#lightbox)

## <a name="safari"></a>Safari

次のコードを使用して、アプリ内からモバイル Safari アプリを開くことができます。

```csharp
var url = new NSUrl("https://docs.microsoft.com");

UIApplication.SharedApplication.OpenUrl(url);
```

これにより、次の Web ビューが作成されます。

[![サファリで表示されるウェブページ](webview-images/safari.png)](webview-images/safari.png#lightbox)

アプリから Safari へのユーザーの移動は、通常は常に避けてください。 ほとんどのユーザーはアプリケーションの外部でのナビゲーションを期待しないでしょうから、アプリから移動すると、ユーザーは決してそれを返さない可能性があり、本質的にエンゲージメントが強制終了します。

iOS 9 の改善により、ユーザーは Safari ページの左上隅に表示される [戻る] ボタンを使用してアプリに簡単に戻れるようにしました。

## <a name="app-transport-security"></a>アプリケーション トランスポート セキュリティ

アプリのトランスポートセキュリティ、または*ATS*は、すべてのインターネット通信が安全な接続のベストプラクティスに準拠していることを確認するために、iOS 9でAppleによって導入されました。

ATS の詳細については、アプリで実装する方法など、[アプリのトランスポート セキュリティ](~/ios/app-fundamentals/ats.md)ガイドを参照してください。

## <a name="uiwebview-deprecation"></a>廃止されたビュー

`UIWebView`は、アプリで Web コンテンツを提供する Apple の従来の方法です。 iOS 2.0 でリリースされ、8.0 で廃止されました。

> [!IMPORTANT]
> `UIWebView` は非推奨とされます。 このコントロールを使用する新しいアプリは[、2020 年 4 月の時点では App Store に受け付けられませんし、このコントロールを使用するアプリの更新は 2020 年 12 月までに受け付けられません](https://developer.apple.com/news/?id=12232019b)。
>
> [Appleの`UIWebView`ドキュメントは、](https://developer.apple.com/documentation/uikit/uiwebview)アプリが代[`WKWebView`](#wkwebview)わりに使用する必要があることを示唆しています。
>
> Xamarin.Forms の使用時に `UIWebView` の非推奨の警告 (ITMS-90809) についてリソースを探している場合は、[Xamarin.Forms WebView](~/xamarin-forms/user-interface/webview.md#uiwebview-deprecation-and-app-store-rejection-itms-90809) のドキュメントを参照してください。

過去 6 か月間に iOS アプリケーションを提出した開発者は、App Store から非推奨であることを`UIWebView`示す警告を受け取った可能性があります。

API の廃止は一般的です。 Xamarin.iOS は、カスタム属性を使用して、これらの API に通知 (および利用可能な場合は置換を提案する) を開発者に返します。 今回の違い、そしてあまり一般的ではないのは、提出時にAppleのApp Storeによって廃止**が強制されること**です。

残念ながら、`UIWebView`型を`Xamarin.iOS.dll`取り除くことは[バイナリブレークの変更](https://docs.microsoft.com/dotnet/core/compatibility/categories#binary-compatibility)です。 この変更により、サポートされていない、または再コンパイル可能になるもの (クローズド ソースなど) を含む既存のサード パーティライブラリが破損します。 これは開発者に追加の問題を生み出すだけです。 したがって、まだ型を削除*していません*。

[Xamarin.iOS 13.16](https://docs.microsoft.com/xamarin/ios/release-notes/13/13.16)以降の新しい検出とツールを使用して、`UIWebView`から移行できます。

### <a name="detection"></a>検出

最近 Apple App Store に iOS アプリケーションを提出していない場合は、この状況がアプリケーションに当てはまるかどうか疑問に思うかもしれません。

確認するには、プロジェクトの追加`--warn-on-type-ref=UIKit.UIWebView`**mtouch 引数**に追加します。 これにより、アプリケーション内**any**で非推奨になっており`UIWebView`、そのすべての依存関係への参照が警告されます。 さまざまな警告を使用して、管理対象リンカー**の実行****前後**のタイプを報告します。

警告は、他の警告と同様に、 を`-warnaserror:`使用してエラーに変換できます。 これは、検証後に新しい依存関係`UIWebView`が追加されないようにする場合に便利です。 次に例を示します。

* `-warnaserror:1502`リンク済みのアセンブリに参照が見つかった場合は、エラーが報告されます。
* `-warnaserror:1503`リンク後のアセンブリに参照が見つかった場合は、エラーが報告されます。

また、リンク前/投稿の結果が役に立たない場合は、警告を無音にすることもできます。 次に例を示します。

* `-nowarn:1502`リンク済みのアセンブリに参照が見つかった場合は、警告は報告**されません**。
* `-nowarn:1503`リンク後のアセンブリに参照が見つかった場合は、警告は報告**されません**。

### <a name="removal"></a>削除

すべてのアプリケーションは一意です。 アプリケーション`UIWebView`から削除する方法と場所によって、異なる手順が必要になる場合があります。 最も一般的なシナリオは次のとおりです。

- アプリケーション内では`UIWebView`使用しません。 すべてがうまくいきます。 AppStore に送信するときに警告を表示**しないように**する必要があります。 他に何も必要ありません。
- アプリケーションによる直接`UIWebView`の使用。 まず、新しい`UIWebView``WKWebView`(iOS 8) または`SFSafariViewController`(iOS 9) の種類に置き換えるなど、を削除します。 これが完了すると、マネージ リンカーは参照を見ず`UIWebView`、最終的なアプリ バイナリはトレースを持たないはずです。
- 間接使用法。 `UIWebView`アプリケーションで使用されるマネージ ライブラリまたはネイティブ ライブラリの一部に存在する可能性があります。 この状況は新しいリリースで既に解決される可能性があるため、外部依存関係を最新バージョンに更新して開始します。 ない場合は、ライブラリのメンテナンス担当者に連絡し、更新計画について尋ねます。

または、次の方法を試すことができます。

1. **Xamarin.Forms**を使用している場合は、この[ブログ記事](https://devblogs.microsoft.com/xamarin/uiwebview-deprecation-xamarin-forms/)を読んでください。
1. マネージ リンカー (プロジェクト全体、または少なくとも を使用`UIWebView`する依存関係) を有効にして、参照されていない場合は削除*される可能性があります*。 これにより問題は解決しますが、コード リンカーを安全にするための追加作業が必要になる場合があります。
1. 管理対象リンカーの設定を変更できない場合は、以下の特殊なケースを参照してください。

#### <a name="applications-cannot-use-the-linker-or-change-its-settings"></a>アプリケーションはリンカーを使用できません (または設定を変更する)

何らかの理由で管理対象リンカー (リンク**しない**など) を使用`UIWebView`**していない**場合、シンボルはバイナリアプリに残り、Apple に送信され、拒否される可能性があります。

*強引な*解決策は、プロジェクト`--optimize=force-rejected-types-removal`の**追加 mtouch 引数**に追加することです。 これにより、アプリケーションからの`UIWebView`トレースが削除されます。 ただし、型を参照するコードは正常に動作**しません**(例外またはクラッシュが発生する必要があります)。 この方法は、コードが実行時に到達できないことが確実な場合にのみ使用してください (静的分析を通じて到達可能な場合でも)。

#### <a name="support-for-ios-7x-or-earlier"></a>iOS 7.x (またはそれ以前) のサポート

`UIWebView`v2.0 以降、iOS の一部となっています。 最も一般的な置`WKWebView`き換えは(iOS `SFSafariViewController` 8)と(iOS 9)です。 アプリケーションが以前のバージョンの iOS をサポートしている場合は、次のオプションを検討する必要があります。

* iOS 8 を最小ターゲット バージョン (ビルド時間の決定) にします。
* アプリが`WKWebView`iOS 8 以降で実行されている場合にのみ使用します (実行時の決定)。

#### <a name="applications-not-submitted-to-apple"></a>Apple に提出されないアプリケーション

アプリケーションが Apple に送信されない場合は、今後の iOS リリースで削除できるため、非推奨の API から移行する計画を立てる必要があります。 ただし、独自の時刻表を使用してこの移行を行うことができます。

## <a name="related-links"></a>関連リンク

- [ウェブビュー (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/webview)
