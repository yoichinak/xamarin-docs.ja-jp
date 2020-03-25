---
title: Xamarin.iOS の Web ビュー
description: このドキュメントでは、Xamarin iOS アプリで web コンテンツを表示するさまざまな方法について説明します。 WKWebView、SFSafariViewController、Safari、およびアプリトランスポートのセキュリティについて説明します。
ms.prod: xamarin
ms.assetid: 84886CF4-2B2B-4540-AD92-7F0B791952D1
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/22/2017
ms.openlocfilehash: 7c469a011a70840cfe94a7f87ed77f03968a3525
ms.sourcegitcommit: ec112800a76089ab1db66fe24b8bbcc510e067b4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/24/2020
ms.locfileid: "80159822"
---
# <a name="web-views-in-xamarinios"></a>Xamarin.iOS の Web ビュー

IOS Apple の有効期間中は、アプリ開発者がアプリに web ビュー機能を組み込むためのさまざまな方法がリリースされました。 ほとんどのユーザーは iOS デバイスで組み込みの Safari web ブラウザーを利用しているため、他のアプリの web ビュー機能はこのエクスペリエンスと同じであることが期待されます。 同じジェスチャが動作すること、パフォーマンスが同等であること、および機能が同じであることが期待されます。

iOS 11 では、`WKWebView` と `SFSafariViewController`の両方に新しい変更が導入されました。 これらの詳細については、「 [iOS 11 の Web 変更ガイド](~/ios/platform/introduction-to-ios11/web.md)」ガイドを参照してください。

## <a name="wkwebview"></a>WKWebView

`WKWebView` は、iOS 8 で導入されました。これにより、アプリ開発者は、mobile Safari のような web 閲覧インターフェイスを実装できます。 これは、`WKWebView`、mobile Safari で使用されるのと同じエンジンである Nitro Javascript エンジンを使用していることが原因です。 `WKWebView` は、パフォーマンスの向上、ユーザーにとって便利なジェスチャの構築、web ページとアプリ間の相互作用の容易さによって、可能な限り UIWebView で常に使用する必要があります。

`WKWebView` は、UIWebView とほぼ同じ方法でアプリに追加できます。ただし、開発者は UI/UX と機能をはるかに細かく制御できます。 Web ビューオブジェクトを作成して表示すると、要求されたページが表示されます。ただし、ビューの表示方法、ユーザーが移動する方法、およびユーザーがビューを終了する方法を制御できます。  

次のコードを使用して、Xamarin. iOS アプリで `WKWebView` を起動できます。

```csharp
WKWebView webView = new WKWebView(View.Frame, new WKWebViewConfiguration());
View.AddSubview(webView);

var url = new NSUrl("https://docs.microsoft.com");
var request = new NSUrlRequest(url);
webView.LoadRequest(request);
```

`WKWebView` が `WebKit` 名前空間にあることに注意することが重要です。そのため、この using ディレクティブをクラスの先頭に追加する必要があります。

`WKWebView` は、Xamarin. Mac アプリでも使用できます。クロスプラットフォームの Mac/iOS アプリを作成する場合は、このアプリを使用する必要があります。

「 [Javascript アラートを処理](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/web_view/handle_javascript_alerts)する」レシピでは、javascript での WKWebView の使用に関する情報も提供します。

## <a name="sfsafariviewcontroller"></a>SFSafariViewController

 `SFSafariViewController` は、アプリから web コンテンツを提供する最新の方法であり、iOS 9 以降で使用できます。 `UIWebView` または `WKWebView`とは異なり、`SFSafariViewController` はビューコントローラーであるため、他のビューと一緒に使用することはできません。 ビューコントローラーを表示するのと同じ方法で、`SFSafariViewController` を新しいビューコントローラーとして表示する必要があります。

 `SFSafariViewController` は、基本的には "ミニ safari" で、アプリに埋め込むことができます。 WKWebView と同様に、同じ Nitro Javascript エンジンを使用しますが、オートフィル、リーダー、モバイル Safari で cookie とデータを共有する機能など、さまざまな Safari 機能も用意されています。 ユーザーと `SFSafariViewController` の間の対話は、アプリにはアクセスできません。 アプリは、既定の Safari 機能のいずれにもアクセスできません。

また、既定では、 **[完了]** ボタンが実装されています。これにより、ユーザーはアプリに簡単に戻ることができ、ナビゲーションボタンを転送および戻ることができ、ユーザーは web ページのスタック内を移動できます。 また、ユーザーには、予期された web ページ上にあることを安心して提供するアドレスバーを提供します。 アドレスバーでは、ユーザーが url を変更することはできません。

これらの実装は変更できないため、`SFSafariViewController` は、アプリがカスタマイズなしで web ページを表示する場合に、既定のブラウザーとして使用するのが理想的です。

次のコードを使用して、Xamarin. iOS アプリで `SFSafariViewController` を起動できます。

```csharp
var sfViewController = new SFSafariViewController(url);

PresentViewController(sfViewController, true, null);
```

これにより、次の web ビューが生成されます。

[SFSafariViewController を使用した web ビューの例の ![](webview-images/sfsafariviewcontroller.png)](webview-images/sfsafariviewcontroller.png#lightbox)

## <a name="safari"></a>Safari

次のコードを使用して、アプリ内から mobile Safari アプリを開くこともできます。

```csharp
var url = new NSUrl("https://docs.microsoft.com");

UIApplication.SharedApplication.OpenUrl(url);
```

これにより、次の web ビューが生成されます。

[Safari に表示される web ページを ![する](webview-images/safari.png)](webview-images/safari.png#lightbox)

一般に、アプリから Safari にユーザーを移動することは、常に避ける必要があります。 ほとんどのユーザーはアプリケーションの外部でのナビゲーションを想定していません。そのため、アプリから移動すると、ユーザーはそこに戻ることはなく、実質的にはエンゲージメントを終了する可能性があります。

iOS 9 の機能強化により、Safari ページの左上隅にある [戻る] ボタンを使用して、ユーザーが簡単にアプリに戻ることができるようになりました。

## <a name="app-transport-security"></a>アプリケーション トランスポート セキュリティ

IOS 9 では、アプリトランスポートセキュリティ (または*ATS* ) が Apple によって導入され、すべてのインターネット通信がセキュリティで保護された接続のベストプラクティスに準拠するようになりました。

アプリでの実装方法など、ATS の詳細については、「[アプリトランスポートセキュリティ](~/ios/app-fundamentals/ats.md)ガイド」を参照してください。

## <a name="uiwebview-deprecation"></a>UIWebView の廃止

`UIWebView` は、アプリに web コンテンツを提供するための Apple の従来の方法です。 IOS 2.0 でリリースされ、8.0 の時点で非推奨とされています。

> [!IMPORTANT]
> `UIWebView` は使用されなくなりました。 このコントロールを使用する新しいアプリは[2020 年4月の時点でアプリストアには受け入れられません。このコントロールを使用したアプリの更新は、2020年12月に受け付けられません](https://developer.apple.com/news/?id=12232019b)。
>
> [Apple の `UIWebView` ドキュメント](https://developer.apple.com/documentation/uikit/uiwebview)では、アプリで[`WKWebView`](#wkwebview)を使用することを提案します。
>
> Xamarin.Forms の使用時に `UIWebView` の非推奨の警告 (ITMS-90809) についてリソースを探している場合は、[Xamarin.Forms WebView](~/xamarin-forms/user-interface/webview.md#uiwebview-deprecation-and-app-store-rejection-itms-90809) のドキュメントを参照してください。

過去6か月以内に iOS アプリケーションを送信した開発者 (またはその場合) は、アプリストアから警告を受信した可能性があります。これは `UIWebView` 非推奨とされます。

Api の廃止は一般的です。 Xamarin では、カスタム属性を使用してこれらの Api を通知し (利用可能な場合は置換を提案します)、開発者に戻します。 今回はどのような違いがありますが、あまり一般的ではありませんが、提出時に Apple の App Store によって廃止が**適用さ**れることになります。

残念ながら、`Xamarin.iOS.dll` からの `UIWebView` 型の削除は、[バイナリの互換性に影響する変更](https://docs.microsoft.com/dotnet/core/compatibility/categories#binary-compatibility)です。 この変更により、サポートされていないものや、再コンパイルされていないものも含めて、既存のサードパーティ製のライブラリが破損します (たとえば、クローズドソース)。 これにより、開発者に対してのみ追加の問題が発生します。 そのため、型を*まだ*削除していません。

Xamarin 以降、`UIWebView`からの移行に役立つ新しい検出とツールを使用できるように[13.16 なります。](https://docs.microsoft.com/xamarin/ios/release-notes/13/13.16)

### <a name="detection"></a>検出

Apple App Store に iOS アプリケーションを最近送信したことがある場合は、この状況がアプリケーションに当てはまるかどうかを疑問に思うかもしれません。

確認するには、プロジェクトの**その他の mtouch 引数**に `--warn-on-type-ref=UIKit.UIWebView` を追加します。 これにより、アプリケーション内の非推奨の `UIWebView` (およびそのすべての依存関係) へ**の参照が**警告されます。 さまざまな警告を使用して、マネージリンカーが実行さ**れる前**と**後**の種類を報告します。

他の警告は、`-warnaserror:`を使用してエラーになることがあります。 これは、検証後に `UIWebView` に対する新しい依存関係が追加されないようにする場合に便利です。 次に例を示します。

* リンク済みのアセンブリで参照が見つかった場合、`-warnaserror:1502` はエラーを報告します。
* リンク後のアセンブリで参照が見つかった場合、`-warnaserror:1503` はエラーを報告します。

また、送信前/ポストリンクの結果が役に立たない場合にも、警告が表示されないようにすることができます。 次に例を示します。

* リンク済みのアセンブリで参照が見つかった場合、`-nowarn:1502` は警告を報告し**ません**。
* リンク後のアセンブリで参照が検出された場合、`-nowarn:1503` は警告を報告し**ません**。

### <a name="removal"></a>削除

すべてのアプリケーションは一意です。 アプリケーションから `UIWebView` を削除するには、どのように使用するかによって異なる手順が必要になることがあります。 最も一般的なシナリオは次のとおりです。

- アプリケーション内に `UIWebView` は使用されません。 すべて問題ありません。 AppStore への送信時に警告が表示**されない**ようにする必要があります。 他に何もする必要はありません。
- アプリケーションによって `UIWebView` が直接使用されます。 まず、`UIWebView`の使用を削除します。たとえば、新しい `WKWebView` (iOS 8) または `SFSafariViewController` (iOS 9) の種類に置き換えます。 この処理が完了すると、マネージリンカーには `UIWebView` への参照が表示されなくなり、最終的なアプリバイナリにはトレースがありません。
- 間接的な使用法。 `UIWebView` は、アプリケーションで使用される一部のサードパーティ製ライブラリ (マネージまたはネイティブ) に存在できます。 この状況は新しいリリースで既に解決されている可能性があるため、外部の依存関係を最新のバージョンに更新することから始めます。 それ以外の場合は、ライブラリのメンテナンスツールに問い合わせて、その更新計画について確認してください。

または、次の方法を試すこともできます。

1. **Xamarin. Forms**を使用している場合は、こちらの[ブログ記事](https://devblogs.microsoft.com/xamarin/uiwebview-deprecation-xamarin-forms/)をお読みください。
1. (プロジェクト全体または少なくとも `UIWebView`を使用した依存関係で) マネージリンカーを有効にします。参照されていない場合は、削除される*可能性があり*ます。 これにより問題は解決されますが、コードリンカーセーフにするために追加の作業が必要になる場合があります。
1. マネージリンカーの設定を変更できない場合は、次の特殊なケースを参照してください。

#### <a name="applications-cannot-use-the-linker-or-change-its-settings"></a>アプリケーションがリンカーを使用できない (またはその設定を変更する)

何らかの理由でマネージリンカーを使用して**いない**場合 (たとえば、**リンク**していない場合)、`UIWebView` シンボルは Apple に送信するバイナリアプリに残り、拒否される可能性があります。

解決*策として*、プロジェクトの**その他の mtouch 引数**に `--optimization=force-rejected-types-removal` を追加します。 これにより、アプリケーションから `UIWebView` のトレースが削除されます。 ただし、型を参照するすべてのコードは正しく機能し**ません**(例外が発生するか、クラッシュすることが予想されます)。 この方法は、実行時にコードに到達できない (静的分析によって到達可能であった場合でも) ことが確実である場合にのみ使用してください。

#### <a name="support-for-ios-7x-or-earlier"></a>IOS 2.x (またはそれ以前) のサポート

`UIWebView` は、v2.0 以降の iOS に含まれていました。 最も一般的な置換は `WKWebView` (iOS 8) と `SFSafariViewController` (iOS 9) です。 アプリケーションが以前のバージョンの iOS を引き続きサポートしている場合は、次のオプションを考慮する必要があります。

* IOS 8 のターゲットバージョンの最小値 (ビルド時間の決定) を設定します。
* アプリが iOS 8 以降で実行されている場合にのみ `WKWebView` を使用します (ランタイムの決定)。

#### <a name="applications-not-submitted-to-apple"></a>Apple に送信されていないアプリケーション

アプリケーションが Apple に送信されない場合は、今後の iOS リリースでは削除される可能性があるため、非推奨の API から移動することを計画する必要があります。 ただし、独自のタイムアウトを使用して、この移行を行うことができます。

## <a name="related-links"></a>関連リンク

- [Webview (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/webview)
