---
title: Xamarin. iOS の Web ビュー
description: このドキュメントでは、Xamarin iOS アプリで web コンテンツを表示するさまざまな方法について説明します。 WKWebView、SFSafariViewController、Safari、およびアプリトランスポートのセキュリティについて説明します。
ms.prod: xamarin
ms.assetid: 84886CF4-2B2B-4540-AD92-7F0B791952D1
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/22/2017
ms.openlocfilehash: bf8f6a9014192d680b0032e034e34b594ecf193c
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91430540"
---
# <a name="web-views-in-xamarinios"></a>Xamarin. iOS の Web ビュー

IOS Apple の有効期間中は、アプリ開発者がアプリに web ビュー機能を組み込むためのさまざまな方法がリリースされました。 ほとんどのユーザーは iOS デバイスで組み込みの Safari web ブラウザーを利用しているため、他のアプリの web ビュー機能はこのエクスペリエンスと同じであることが期待されます。 同じジェスチャが動作すること、パフォーマンスが同等であること、および機能が同じであることが期待されます。

iOS 11 では、との両方に新しい変更が導入されました `WKWebView` `SFSafariViewController` 。 これらの詳細については、「 [iOS 11 の Web 変更ガイド](~/ios/platform/introduction-to-ios11/web.md) 」ガイドを参照してください。

## <a name="wkwebview"></a>WKWebView

`WKWebView` は、iOS 8 で導入されました。これにより、アプリ開発者は、mobile Safari と同様の web 閲覧インターフェイスを実装できます。 これは、Nitro Javascript エンジンを使用することによるものです。これは、 `WKWebView` モバイル Safari で使用されるのと同じエンジンです。 `WKWebView` は、パフォーマンスの向上、ユーザーにとって便利なジェスチャの構築、web ページとアプリ間の相互作用の容易さによって可能な限り、UIWebView で常に使用する必要があります。

`WKWebView` は、UIWebView とほぼ同じ方法でアプリに追加できます。ただし、開発者は UI/UX と機能をはるかに細かく制御できます。 Web ビューオブジェクトを作成して表示すると、要求されたページが表示されます。ただし、ビューの表示方法、ユーザーが移動する方法、およびユーザーがビューを終了する方法を制御できます。  

次のコードを使用し `WKWebView` て、Xamarin. iOS アプリでを起動できます。

```csharp
WKWebView webView = new WKWebView(View.Frame, new WKWebViewConfiguration());
View.AddSubview(webView);

var url = new NSUrl("https://docs.microsoft.com");
var request = new NSUrlRequest(url);
webView.LoadRequest(request);
```

は名前空間にあることに注意して `WKWebView` `WebKit` ください。したがって、この using ディレクティブをクラスの先頭に追加する必要があります。

`WKWebView` は、Xamarin. Mac アプリでも使用できます。クロスプラットフォームの Mac/iOS アプリを作成する場合は、このアプリを使用する必要があります。

「 [Javascript アラートを処理](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/web_view/handle_javascript_alerts) する」レシピでは、javascript での WKWebView の使用に関する情報も提供します。

## <a name="sfsafariviewcontroller"></a>SFSafariViewController

 `SFSafariViewController` は、アプリから web コンテンツを提供するための最新の方法であり、iOS 9 以降で使用できます。 またはとは異なり `UIWebView` `WKWebView` 、 `SFSafariViewController` はビューコントローラーであるため、他のビューと一緒に使用することはできません。 ビューコントローラーを表示するのと `SFSafariViewController` 同じように、新しいビューコントローラーとして表示する必要があります。

 `SFSafariViewController` は、基本的には ' ミニ safari ' であり、アプリに埋め込むことができます。 WKWebView と同様に、同じ Nitro Javascript エンジンを使用しますが、オートフィル、閲覧者、モバイル Safari で cookie とデータを共有する機能など、さまざまな Safari 機能も用意されています。 ユーザーとの間の対話 `SFSafariViewController` は、アプリにはアクセスできません。 アプリは、既定の Safari 機能のいずれにもアクセスできません。

また、既定では、[ **完了** ] ボタンが実装されています。これにより、ユーザーはアプリに簡単に戻ることができ、ナビゲーションボタンを転送および戻ることができ、ユーザーは web ページのスタック内を移動できます。 また、ユーザーには、予期された web ページ上にあることを安心して提供するアドレスバーを提供します。 アドレスバーでは、ユーザーが url を変更することはできません。

これらの実装は変更できないため、 `SFSafariViewController` アプリがカスタマイズなしで web ページを表示する必要がある場合は、既定のブラウザーとしてを使用することをお勧めします。

次のコードを使用し `SFSafariViewController` て、Xamarin. iOS アプリでを起動できます。

```csharp
var sfViewController = new SFSafariViewController(url);

PresentViewController(sfViewController, true, null);
```

これにより、次の web ビューが生成されます。

[![SFSafariViewController を使用した web ビューの例](webview-images/sfsafariviewcontroller.png)](webview-images/sfsafariviewcontroller.png#lightbox)

## <a name="safari"></a>Safari

次のコードを使用して、アプリ内から mobile Safari アプリを開くこともできます。

```csharp
var url = new NSUrl("https://docs.microsoft.com");

UIApplication.SharedApplication.OpenUrl(url);
```

これにより、次の web ビューが生成されます。

[![Safari に表示される web ページ](webview-images/safari.png)](webview-images/safari.png#lightbox)

一般に、アプリから Safari にユーザーを移動することは、常に避ける必要があります。 ほとんどのユーザーはアプリケーションの外部でのナビゲーションを想定していません。そのため、アプリから移動すると、ユーザーはそれを返すことがなく、実質的にはエンゲージメントを終了する可能性があります。

iOS 9 の機能強化により、Safari ページの左上隅にある [戻る] ボタンを使用して、ユーザーが簡単にアプリに戻ることができるようになりました。

## <a name="app-transport-security"></a>アプリケーション トランスポート セキュリティ

IOS 9 では、アプリトランスポートセキュリティ (または *ATS* ) が Apple によって導入され、すべてのインターネット通信がセキュリティで保護された接続のベストプラクティスに準拠するようになりました。

アプリでの実装方法など、ATS の詳細については、「 [アプリトランスポートセキュリティ](~/ios/app-fundamentals/ats.md) ガイド」を参照してください。

## <a name="uiwebview-deprecation"></a>UIWebView の廃止

`UIWebView` は、アプリに web コンテンツを提供する従来の方法です。 IOS 2.0 でリリースされ、8.0 の時点で非推奨とされています。

> [!IMPORTANT]
> `UIWebView` は非推奨とされます。 このコントロールを使用する新しいアプリは [2020 年4月の時点でアプリストアには受け入れられません。このコントロールを使用したアプリの更新は、2020年12月に受け付けられません](https://developer.apple.com/news/?id=12232019b)。
>
> [Apple の `UIWebView` ドキュメント](https://developer.apple.com/documentation/uikit/uiwebview) では、アプリが代わりにを使用することを提案 [`WKWebView`](#wkwebview) します。
>
> Xamarin.Forms の使用時に `UIWebView` の非推奨の警告 (ITMS-90809) についてリソースを探している場合は、[Xamarin.Forms WebView](~/xamarin-forms/user-interface/webview.md#uiwebview-deprecation-and-app-store-rejection-itms-90809) のドキュメントを参照してください。

過去6か月 (またはそれ以降) に iOS アプリケーションを送信した開発者は、アプリストアから警告を受け取った可能性があり、 `UIWebView` 非推奨とされます。

Api の廃止は一般的です。 Xamarin では、カスタム属性を使用してこれらの Api を通知し (利用可能な場合は置換を提案します)、開発者に戻します。 今回はどのような違いがありますが、あまり一般的ではありませんが、提出時に Apple の App Store によって廃止が **適用さ** れることになります。

残念ながら、 `UIWebView` から型を削除することは、バイナリの互換性に `Xamarin.iOS.dll` [影響する変更](/dotnet/core/compatibility/categories#binary-compatibility)です。 この変更により、サポートされていないものや、再コンパイルされていないものも含めて、既存のサードパーティ製のライブラリが破損します (たとえば、クローズドソース)。 これにより、開発者に対してのみ追加の問題が発生します。 そのため、型を *まだ*削除していません。

Xamarin 以降では、からの移行に役立つ新しい検出とツールを使用でき[ます13.16。](/xamarin/ios/release-notes/13/13.16) `UIWebView`

### <a name="detection"></a>検出

Apple App Store に iOS アプリケーションを最近送信したことがある場合は、この状況がアプリケーションに当てはまるかどうかを疑問に思うかもしれません。

確認するには、 `--warn-on-type-ref=UIKit.UIWebView` プロジェクトの**その他の mtouch 引数**にを追加します。 これにより、アプリケーション内の非推奨のへ **の参照** `UIWebView` (およびそのすべての依存関係) が警告されます。 さまざまな警告を使用して、マネージリンカーが実行さ **れる前** と **後** の種類を報告します。

他の警告は、を使用してエラーになることがあり `-warnaserror:` ます。 これは、の新しい依存関係が検証後に追加されないようにする場合に便利です `UIWebView` 。 次に例を示します。

* `-warnaserror:1502` 事前にリンクされたアセンブリで参照が見つかった場合、はエラーを報告します。
* `-warnaserror:1503` リンク後のアセンブリで参照が見つかった場合、はエラーを報告します。

また、送信前/ポストリンクの結果が役に立たない場合にも、警告が表示されないようにすることができます。 次に例を示します。

* `-nowarn:1502` リンク前のアセンブリで参照が見つかった場合、は警告を報告し **ません** 。
* `-nowarn:1503` リンク後のアセンブリで参照が見つかった場合、は警告を報告し **ません** 。

### <a name="removal"></a>削除

すべてのアプリケーションは一意です。 アプリケーションからを削除する場合は `UIWebView` 、使用する方法と場所によって異なる手順が必要になることがあります。 最も一般的なシナリオは次のとおりです。

- アプリケーション内でを使用することはありません `UIWebView` 。 すべて問題ありません。 AppStore への送信時に警告が表示 **されない** ようにする必要があります。 他に何もする必要はありません。
- `UIWebView`アプリケーションによるの直接使用。 まず、の使用を削除し `UIWebView` ます。たとえば、を新しい `WKWebView` (ios 8) または `SFSafariViewController` (ios 9) の種類に置き換えます。 この処理が完了すると、マネージリンカーにはへの参照が表示されなくなり、 `UIWebView` 最終的なアプリバイナリにはトレースがありません。
- 間接的な使用法。 `UIWebView` は、アプリケーションで使用される、マネージまたはネイティブの一部のサードパーティライブラリに存在できます。 この状況は新しいリリースで既に解決されている可能性があるため、外部の依存関係を最新のバージョンに更新することから始めます。 それ以外の場合は、ライブラリのメンテナンスツールに問い合わせて、その更新計画について確認してください。

または、次の方法を試すこともできます。

1. **Xamarin. Forms**を使用している場合は、こちらの[ブログ記事](https://devblogs.microsoft.com/xamarin/uiwebview-deprecation-xamarin-forms/)をお読みください。
1. マネージリンカーを有効にします (プロジェクト全体または少なくともを使用した依存関係で `UIWebView` )。参照されていない場合は、削除される *可能性があり* ます。 これにより問題は解決されますが、コードリンカーセーフにするために追加の作業が必要になる場合があります。
1. マネージリンカーの設定を変更できない場合は、次の特殊なケースを参照してください。

#### <a name="applications-cannot-use-the-linker-or-change-its-settings"></a>アプリケーションがリンカーを使用できない (またはその設定を変更する)

何らかの理由でマネージリンカーを使用して **いない** 場合 ( **リンク**していない場合など)、 `UIWebView` そのシンボルはバイナリアプリに残り、Apple に送信され、拒否される可能性があります。

*強制*解決策とし `--optimize=force-rejected-types-removal` て、プロジェクトの**追加の mtouch 引数**にを追加します。 これにより、アプリケーションからのトレースが削除され `UIWebView` ます。 ただし、型を参照するすべてのコードは正しく機能し **ません** (例外が発生するか、クラッシュすることが予想されます)。 この方法は、実行時にコードに到達できない (静的分析によって到達可能であった場合でも) ことが確実である場合にのみ使用してください。

#### <a name="support-for-ios-7x-or-earlier"></a>IOS 2.x (またはそれ以前) のサポート

`UIWebView` は、v2.0 以降の iOS に含まれています。 最も一般的な置換は、 `WKWebView` (ios 8) と `SFSafariViewController` (ios 9) です。 アプリケーションが以前のバージョンの iOS を引き続きサポートしている場合は、次のオプションを考慮する必要があります。

* IOS 8 のターゲットバージョンの最小値 (ビルド時間の決定) を設定します。
* `WKWebView`アプリが iOS 8 以降で実行されている場合にのみ使用します (ランタイムの決定)。

#### <a name="applications-not-submitted-to-apple"></a>Apple に送信されていないアプリケーション

アプリケーションが Apple に送信されない場合は、今後の iOS リリースでは削除される可能性があるため、非推奨の API から移動することを計画する必要があります。 ただし、独自のタイムアウトを使用して、この移行を行うことができます。

## <a name="related-links"></a>関連リンク

- [Webview (サンプル)](/samples/xamarin/ios-samples/webview)