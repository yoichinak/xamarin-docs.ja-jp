---
title: ファイルを追加または Visual Studio でビルドした後、IPA ファイルからファイルを削除できますか。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 6C3082FB-C3F1-4661-BE45-64570E56DE7C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: b8b61ba38491b2085233dd1b30a82bc57d2baaed
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="can-i-add-files-to-or-remove-files-from-an-ipa-file-after-building-it-in-visual-studio"></a>ファイルを追加または Visual Studio でビルドした後、IPA ファイルからファイルを削除できますか。

はい、できますが、再署名する必要は通常、`.app`変更を行った後にバンドルできます。

変更することに注意してください、`.ipa`ファイルは通常の使用に必要ではありません。 この記事の内容は情報提供を目的にのみ提供されます。

## <a name="example-removing-a-file-from-a-ipa-archive"></a>例: からファイルを削除する、`.ipa`アーカイブ

この例には、Xamarin.iOS プロジェクトの名前があると仮定`iPhoneApp1`と`generated session id`は `cc530d20d6b19da63f6f1c6f67a0a254`

1.  ビルド、 `.ipa` Visual Studio からは通常どおりファイルします。

2.  Mac に切り替えては、ホストを作成します。

3.  ビルドが見つかりません、`~/Library/Caches/Xamarin/mtbs/builds`フォルダーです。 このパスを貼り付けることができます**Finder > 移動 > フォルダーに移動**Finder でフォルダーを参照します。 プロジェクト名に一致するフォルダーを探します。 そのフォルダー内に一致するフォルダーを検索、`generated session id`ビルドのです。 ほとんどの場合、最新の変更時刻を持つサブフォルダーになります。

4.  新しく開きます`Terminal.app`ウィンドウです。

5.  型`cd `に Terminal.app ウィンドウとし、ドラッグ アンド ドロップ、`generated session id`フォルダーに、`Terminal.app`ウィンドウ。

    ![](modify-ipa-images/session-id-folder.png "Finder で生成されるセッション id フォルダーを検索します。")

6.  ディレクトリを変更する戻り値のキーを入力して、`generated session id`フォルダーです。

7.  解凍、`.ipa`に一時ファイル`old/`フォルダーの次のコマンドを使用します。 調整、`Ad-Hoc`と`iPhoneApp1`必要な場合、特定のプロジェクトの名前します。

    > 同じく - xk bin/iPhone/Ad-Hoc/iPhoneApp1-1.0.ipa 古い/

8.  保持、`Terminal.app`ウィンドウを開きます。

9.  目的のファイルから削除、`.ipa`です。 ファインダーを使用してゴミ箱に移動するか、コマンドラインを使用して、それらを削除`Terminal.app`です。 内容を表示する、 `Payload/iPhone` Finder で、コントロールをクリックして、ファイルのファイルおよび選択した**パッケージの内容**です。

10.  手順 3 と同様に同じ手法を使用して、ログ ファイルが見つかりません `~/Library/Logs/Xamarin/MonoTouchVS/`プロジェクト名の両方を持つと`generated session id`名に: ![ ] (modify-ipa-images/build-log.png "Finder で、プロジェクト ビルド ログを検索")

11.  ビルド ログ手順 10 では、たとえばダブルクリックして開きます。

12.  含む行を探します`tool /usr/bin/codesign execution started with arguments: -v --force --sign`です。

13.  型`/usr/bin/codesign `手順 8. Terminal.app ウィンドウに表示します。

14.  以降で引数のすべてをコピー`-v`行では、手順 12、し Terminal.app ウィンドウに貼り付けます。

15.  最後の引数を変更、`.app`バンドルが内にある、`old/Payload/`フォルダー、およびコマンドを実行します。

```bash
/usr/bin/codesign -v --force --sign SOME_LONG_STRING in/iPhone/Ad-Hoc/iPhoneApp1.app/ResourceRules.plist --entitlements obj/iPhone/Ad-Hoc/Entitlements.xcent old/Payload/iPhoneApp1.app
```

16.  変更、`old/`ターミナル ディレクトリ。

```bash
cd old
```

17.  新しいディレクトリの内容を zip 圧縮`.ipa`ツールを使用して、`zip`コマンド。 変更することができます、`"$HOME/Desktop/iPhoneApp1-1.0.ipa"`出力への引数、`.ipa`ファイルの任意の場所には。

```bash
zip -yr "$HOME/Desktop/iPhoneApp1-1.0.ipa" *
```

## <a name="common-error-messages"></a>一般的なエラー メッセージ

表示される場合`Invalid Signature. A sealed resource is missing or invalid.`、通常内で何かが変更されたことを意味する、`.app`バンドル、および、`.app`バンドルが正しく再署名されていなかった後です。 作成する場合、`.ipa`ディストリビューション プロファイルでは、する_必要があります_、元のビルド`.ipa`分布プロファイルを使用しています。 それ以外の場合、`Entitlements.xcent`は不正確になります。

次のコマンドを実行する場合に、このエラーは発生する方法の具体的な例を付与する`codesign --verify`コマンド、ターミナル ウィンドウで手順 9 の後に、エラーの正確な原因がと共にエラーが表示されます。

```bash
$ codesign -dvvv --no-strict --verify old/Payload/iPhoneApp1.app
old/Payload/iPhoneApp1.app: a sealed resource is missing or invalid
file missing: /Users/macuser/Library/Caches/Xamarin/mtbs/builds/iPhoneApp1/cc530d20d6b19da63f6f1c6f67a0a254/old/Payload/iPhoneApp1.app/MyFile.png
```

App Store の検証プロセスは、同様のエラー メッセージを報告します。

> ERROR ITMS-90035: "Invalid Signature. 封印されたリソースは、存在しないか無効です。 パス [iPhoneApp1.app/iPhoneApp1] にあるバイナリには、無効な署名が含まれています。 アドホックの証明書または開発証明書ではない、配布証明書を使用してアプリケーションに署名したことを確認してください。 (これは、プロジェクト レベルでの任意の値を上書き)、ターゲット レベルで、Xcode でコード署名の設定が正しいことを確認します。 また、アップロードするバンドルは、Xcode、シミュレーターのターゲットではないでリリース ターゲットを使用して構築されたことを確認してください。 コードの署名の設定が正しい場合は、Xcode で"クリーン All"を選択して、Finder で、「ビルド」ディレクトリを削除してリビルドしてターゲットのリリースです。 詳細についてを参照してください[ https://developer.apple.com/library/ios/documentation/Security/Conceptual/CodeSigningGuide/Introduction/Introduction.html ](https://developer.apple.com/library/ios/documentation/Security/Conceptual/CodeSigningGuide/Introduction/Introduction.html)"
