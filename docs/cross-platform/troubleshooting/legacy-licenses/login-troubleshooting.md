---
title: "なぜことはできませんにログインする Visual Studio または Visual Studio での Xamarin Mac のですか。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 6EF2B553-5DF9-41CC-B68F-77A7F64572FA
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: b50969e4c1d75c0bd79c08223dd959241dcb229a
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="why-cant-i-log-into-xamarin-in-visual-studio-or-visual-studio-for-mac"></a>なぜことはできませんにログインする Visual Studio または Visual Studio での Xamarin Mac のですか。

> [!IMPORTANT]
> 所有またはを使用していない限り、Xamarin アカウントにログインする必要がないために、このガイドはほとんどの MSDN ユーザーに適用されません、 [Xamarin コンポーネント ストア](https://components.xamarin.com/)または[Visual Studio for Mac (Mac)](~/cross-platform/get-started/requirements.md)です。 MSDN ライセンス保持者は、これを参照してください[ライセンス オプションのガイド](~/cross-platform/get-started/requirements.md)代わりにします。



## <a name="overview"></a>概要
できない場合、IDE で Xamarin アカウントへのログイン、いくつかの一般的な原因があります。 既知の問題と修正プログラムは以下のとおりです。

### <a name="finding-the-login-screen"></a>ログイン画面の検索

リファレンスについては、ログイン画面が参照してください。

- Visual Studio for Mac
   - ようこそ画面の右上隅
   - **Visual Studio for Mac > アカウント**(Mac)
   - **ツール > アカウント**(Windows)
- Visual Studio
   - **ツール > Xamarin アカウント**

## <a name="the-ide-is-connecting-but-the-account-screen-isnt-showing-correct-login-information"></a>IDE が接続しているが、アカウントの画面が表示されないログイン情報が正しい

手動でのライセンスは再同期では、この問題は解決されて通常します。
この記事の指示に従ってください:[すれば手動で再同期する Xamarin のライセンスですか?](~/cross-platform/troubleshooting/legacy-licenses/resync-licenses.md)

## <a name="invalid-account-information"></a>無効なアカウント情報

Xamarin の web サイトに移動する場合[ログイン ページ](https://store.xamarin.com/Login?from=%2faccount%2f)現在のアカウントの資格情報でログ記録を試みることができます。
ページには、パスワードをリセットして、新しいアカウントを作成するリンクもあります。

## <a name="account-is-valid-but-the-ide-cant-connect"></a>アカウントは有効ですが、IDE が接続できません。

通常、ファイアウォールまたは他のセキュリティ設定、IDE から必要なエンドポイントへのアクセスをブロックする場合に発生します。
ライセンス認証サーバーは、以下にアクセスする必要があります。

> activation.xamarin.com store.xamarin.com auth.xamarin.com

ただし、他のいくつかのエンドポイントは、NuGet パッケージの管理などの作業の更新などの全般的な開発プロセスにとって重要です。このため、ことをお勧め確認*すべて*から追加されるエンドポイントの[Xamarin ファイアウォールの構成ガイド](~/cross-platform/get-started/installation/firewall.md)です。

### <a name="ios-in-xamarin-studio-windows"></a>iOS in Xamarin Studio Windows
Xamarin Studio for Windows では、iOS 開発はサポートされていません。 ここで、ログイン画面にアクセスするときに、iOS ライセンスは表示されません。

代わりに、従来のライセンスを使用して Xamarin Studio (Mac) または Visual Studio を使用してログインします。 Windows 上の MSDN ユーザーがログインする必要がないことに注意してください。

## <a name="additional-support"></a>その他のサポート

上記のシナリオは、状況を記述またはしていない場合、問題を解決する、これらを参照してください[サポート オプション](https://www.xamarin.com/support)です。
