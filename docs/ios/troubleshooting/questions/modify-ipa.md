---
title: Visual Studio でビルドした後、IPA ファイルにファイルを追加したり、ファイルを削除したりすることはできますか。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 6C3082FB-C3F1-4661-BE45-64570E56DE7C
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 04/03/2018
ms.openlocfilehash: d1546b83304d8c66f7433bd77c5ebecc9dc95aaa
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70278477"
---
# <a name="can-i-add-files-to-or-remove-files-from-an-ipa-file-after-building-it-in-visual-studio"></a>Visual Studio でビルドした後、IPA ファイルにファイルを追加したり、ファイルを削除したりすることはできますか。

はい、できますが、変更を行った後でバンドルに`.app`再署名する必要があります。

通常の使用で`.ipa`は、ファイルの変更は必要ありません。 この記事は、あくまで情報提供を目的として提供されています。

## <a name="example-removing-a-file-from-a-ipa-archive"></a>例: `.ipa`アーカイブからファイルを削除する

この例では、Xamarin. iOS プロジェクトの名前が`iPhoneApp1` `generated session id`で、がであることを前提としています。`cc530d20d6b19da63f6f1c6f67a0a254`

1. Visual Studio `.ipa`から通常どおりファイルをビルドします。

2. Mac ビルドホストに切り替えます。

3. `~/Library/Caches/Xamarin/mtbs/builds`フォルダー内のビルドを検索します。 このパスを Finder に貼り付けることができます **> [フォルダーに移動**] を選択して finder でフォルダーを参照 > ます。 プロジェクト名と一致するフォルダーを探します。 そのフォルダー内で、ビルドのに`generated session id`一致するフォルダーを探します。 これは、ほとんどの場合、最新の変更時刻を持つサブフォルダーです。

4. 新しい`Terminal.app`ウィンドウを開きます。

5. [ `cd`アプリ] ウィンドウに「」と入力し、 `generated session id`フォルダーをドラッグ & て`Terminal.app`ウィンドウにドロップします。

    ![](modify-ipa-images/session-id-folder.png "Finder で生成されたセッション id フォルダーを検索する")

6. `generated session id`フォルダーにディレクトリを変更するには、return キーを入力します。

7. 次のコマンドを使用し`old/`て、一時フォルダーにファイルを解凍します。`.ipa` 特定のプロジェクト`iPhoneApp1`に必要な名前と名前を調整します。`Ad-Hoc`

    > ditto-xk bin/iPhone/Ad-Hoc/iPhoneApp1-1.0. ipa old/

8. ウィンドウを`Terminal.app`開いたままにします。

9. から目的のファイルを削除`.ipa`します。 Finder を使用してごみ箱に移動するか、を使用して`Terminal.app`コマンドラインで削除することができます。 Finder で`Payload/iPhone`ファイルの内容を表示するには、ファイルを管理し、 **[パッケージの内容を表示]** を選択します。

10. 手順3と同じ一般的な方法を使用して、の下`~/Library/Logs/Xamarin/MonoTouchVS/`にプロジェクト名`generated session id`とという名前のログファイルを見つけます。![](modify-ipa-images/build-log.png "Finder でプロジェクトビルドログを検索する")

11. 手順10のビルドログを開きます。たとえば、ダブルクリックします。

12. に含ま`tool /usr/bin/codesign execution started with arguments: -v --force --sign`れる行を探します。

13. 手順`/usr/bin/codesign` 8. の [アプリ] ウィンドウに「」と入力します。

14. から始まる`-v`すべての引数を、手順 12. の行からコピーし、それらをターミナルアプリウィンドウに貼り付けます。

15. 最後の引数を`.app` `old/Payload/`フォルダー内にあるバンドルになるように変更してから、コマンドを実行します。

    ```bash
    /usr/bin/codesign -v --force --sign SOME_LONG_STRING in/iPhone/Ad-Hoc/iPhoneApp1.app/ResourceRules.plist --entitlements obj/iPhone/Ad-Hoc/Entitlements.xcent old/Payload/iPhoneApp1.app
    ```

16. ターミナルのディレクトリ`old/`に移動します。

    ```bash
    cd old
    ```

17. コマンドを使用して、ディレクトリの内容を`.ipa`新しいファイルに圧縮します。 `zip` ファイル`.ipa`を出力する`"$HOME/Desktop/iPhoneApp1-1.0.ipa"`ように引数を変更するには、次のようにします。

    ```bash
    zip -yr "$HOME/Desktop/iPhoneApp1-1.0.ipa" *
    ```

## <a name="common-error-messages"></a>一般的なエラーメッセージ

が表示`Invalid Signature. A sealed resource is missing or invalid.`される場合は、通常、 `.app` `.app`バンドル内で何らかの変更が行われたこと、およびバンドルが後で正しく再署名されていないことを意味します。 配布プロファイルを`.ipa`使用してを作成する場合は、配布プロファイルを使用して元`.ipa`のを作成する必要があることにも注意してください。 それ以外`Entitlements.xcent`の場合、は正しくありません。

このエラーが発生する具体的な例を示すために、手順9の後`codesign --verify`にターミナルウィンドウで次のコマンドを実行すると、エラーの正確な原因と共にエラーが表示されます。

```bash
$ codesign -dvvv --no-strict --verify old/Payload/iPhoneApp1.app
old/Payload/iPhoneApp1.app: a sealed resource is missing or invalid
file missing: /Users/macuser/Library/Caches/Xamarin/mtbs/builds/iPhoneApp1/cc530d20d6b19da63f6f1c6f67a0a254/old/Payload/iPhoneApp1.app/MyFile.png
```

また、アプリストアの検証プロセスでも、次のようなエラーメッセージが報告されます。

> エラー 90035:"署名が無効です。 封印されたリソースがないか、無効です。 パス [iPhoneApp1/iPhoneApp1] のバイナリに無効な署名が含まれています。 アドホック証明書または開発証明書ではなく、配布証明書を使用してアプリケーションに署名していることを確認します。 Xcode のコード署名設定がターゲットレベルで正しいことを確認します (プロジェクトレベルのすべての値をオーバーライドします)。 また、アップロードするバンドルが、シミュレーターターゲットではなく、Xcode のリリースターゲットを使用してビルドされていることを確認します。 コード署名の設定が正しいことがわかっている場合は、Xcode で [すべてクリーン] を選択し、Finder の "build" ディレクトリを削除して、リリースターゲットをリビルドします。 詳細については、 [https://developer.apple.com/library/ios/documentation/Security/Conceptual/CodeSigningGuide/Introduction/Introduction.html](https://developer.apple.com/library/ios/documentation/Security/Conceptual/CodeSigningGuide/Introduction/Introduction.html)「」を参照してください。
