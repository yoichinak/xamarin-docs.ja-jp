---
title: ファイルを追加または Visual Studio でビルドした後で IPA ファイルからファイルを削除できますか。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 6C3082FB-C3F1-4661-BE45-64570E56DE7C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/03/2018
ms.openlocfilehash: 366308774a7302e54b0d47753256638e89d97b82
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350983"
---
# <a name="can-i-add-files-to-or-remove-files-from-an-ipa-file-after-building-it-in-visual-studio"></a>ファイルを追加または Visual Studio でビルドした後で IPA ファイルからファイルを削除できますか。

はい、できますが、再署名する必要は通常、`.app`変更を行った後にバンドルします。

変更することに注意してください、`.ipa`ファイルは通常の使用に必要ではありません。 この記事では情報提供を目的の純粋に提供します。

## <a name="example-removing-a-file-from-a-ipa-archive"></a>例: からファイルを削除する、`.ipa`アーカイブ

この例には、Xamarin.iOS プロジェクトの名前があると仮定`iPhoneApp1`と`generated session id`は `cc530d20d6b19da63f6f1c6f67a0a254`

1.  ビルド、 `.ipa` Visual Studio からは通常どおりファイルします。

2.  Mac ビルド ホストに切り替えます。

3.  ビルドの検索、`~/Library/Caches/Xamarin/mtbs/builds`フォルダー。 このパスを貼り付けることができます**Finder > 移動 > フォルダーに移動して**Finder でフォルダーを参照します。 プロジェクト名と一致するフォルダーを探します。 、そのフォルダー内に一致するフォルダーを検索、`generated session id`ビルドします。 ほとんどの場合、最新の変更時刻を含むサブフォルダーになります。

4.  新しく開きます`Terminal.app`ウィンドウ。

5.  型`cd `に Terminal.app ウィンドウと、ドラッグ アンド ドロップ、`generated session id`フォルダーに、`Terminal.app`ウィンドウ。

    ![](modify-ipa-images/session-id-folder.png "Finder で生成されるセッション id のフォルダーを検索します。")

6.  ディレクトリを変更する戻り値のキーを入力して、`generated session id`フォルダー。

7.  解凍、`.ipa`に一時ファイル`old/`フォルダーの次のコマンドを使用します。 調整、`Ad-Hoc`と`iPhoneApp1`特定のプロジェクトの必要に応じて名前を付けます。

    > 古い - xk bin/iPhone/Ad-Hoc/iPhoneApp1-1.0.ipa を同じく/

8.  保持、`Terminal.app`ウィンドウを開きます。

9.  目的のファイルから削除、`.ipa`します。 Finder を使用してごみ箱に移動またはを使用して、コマンドラインでそれらを削除することができます`Terminal.app`します。 内容を表示する、 `Payload/iPhone` finder で、コントロールのクリック、ファイルのファイルおよび選択した**パッケージの内容**します。

10.  ログ ファイルを検索して、手順 3. のように、同じ一般的な方法を使用して`~/Library/Logs/Xamarin/MonoTouchVS/`プロジェクト名を持つ、`generated session id`名: ![](modify-ipa-images/build-log.png "Finder で、プロジェクトのビルド ログを見つける")

11.  ダブルクリックしてなど手順 10 からのビルド ログを開きます。

12.  含む行を見つけて`tool /usr/bin/codesign execution started with arguments: -v --force --sign`します。

13.  型`/usr/bin/codesign `手順 8. Terminal.app ウィンドウにします。

14.  以降では、引数をすべてコピー`-v`行では、手順 12 と Terminal.app ウィンドウに貼り付けます。

15.  変更するのには、最後の引数、`.app`バンドルにある、`old/Payload/`フォルダー、および、コマンドを実行します。

```bash
/usr/bin/codesign -v --force --sign SOME_LONG_STRING in/iPhone/Ad-Hoc/iPhoneApp1.app/ResourceRules.plist --entitlements obj/iPhone/Ad-Hoc/Entitlements.xcent old/Payload/iPhoneApp1.app
```

16.  変更、`old/`ターミナルでディレクトリ。

```bash
cd old
```

17.  新しいディレクトリの内容を zip 圧縮`.ipa`ファイルを使用して、`zip`コマンド。 変更することができます、`"$HOME/Desktop/iPhoneApp1-1.0.ipa"`出力への引数、`.ipa`好きな場所にファイルします。

```bash
zip -yr "$HOME/Desktop/iPhoneApp1-1.0.ipa" *
```

## <a name="common-error-messages"></a>一般的なエラー メッセージ

表示された場合`Invalid Signature. A sealed resource is missing or invalid.`、一般に内で何かが変更されたことを意味する、`.app`バンドル、および、`.app`バンドル署名されていない正しく再後。 作成する場合、`.ipa`配布プロファイルの場合は、する_する必要があります_構築元`.ipa`配布プロファイルを使用して。 それ以外の場合、`Entitlements.xcent`は不正確になります。

次を実行する場合、このエラーは発生する方法の具体的な例を提供する`codesign --verify`コマンド手順 9 の後に、ターミナル ウィンドウで、エラーの正確な原因とエラーが表示されます。

```bash
$ codesign -dvvv --no-strict --verify old/Payload/iPhoneApp1.app
old/Payload/iPhoneApp1.app: a sealed resource is missing or invalid
file missing: /Users/macuser/Library/Caches/Xamarin/mtbs/builds/iPhoneApp1/cc530d20d6b19da63f6f1c6f67a0a254/old/Payload/iPhoneApp1.app/MyFile.png
```

アプリ ストアの検証プロセスのようなエラー メッセージが報告されます。

> エラー ITMS-90035:"無効な署名します。 封印されたリソースとは見つからないか無効です。 パス [iPhoneApp1.app/iPhoneApp1] にあるバイナリには、無効な署名が含まれています。 配布証明書、いないアドホックの証明書または開発証明書を使用してアプリケーションに署名を確認します。 (これは、プロジェクト レベルで任意の値を上書きするには)、ターゲット レベルでの Xcode でコード署名の設定が正しいことを確認します。 また、アップロードするバンドルは、Xcode シミュレーター ターゲットではないでリリースのターゲットを使用してビルドされたことを確認してください。 コードの署名の設定が正しい場合は、"クリーン All"を選択して、Xcode で、finder で、「ビルド」ディレクトリを削除およびターゲットのリリースを再構築します。 詳細についてを参照してください[ https://developer.apple.com/library/ios/documentation/Security/Conceptual/CodeSigningGuide/Introduction/Introduction.html ](https://developer.apple.com/library/ios/documentation/Security/Conceptual/CodeSigningGuide/Introduction/Introduction.html)"
