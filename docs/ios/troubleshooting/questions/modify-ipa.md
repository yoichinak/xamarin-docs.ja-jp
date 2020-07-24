---
title: Visual Studio でビルドした後、IPA ファイルにファイルを追加したり、ファイルを削除したりすることはできますか。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 6C3082FB-C3F1-4661-BE45-64570E56DE7C
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 04/03/2018
ms.openlocfilehash: e3b535b8624f03e9c832f1719237409cb9ff26c2
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86939907"
---
# <a name="can-i-add-files-to-or-remove-files-from-an-ipa-file-after-building-it-in-visual-studio"></a>Visual Studio でビルドした後、IPA ファイルにファイルを追加したり、ファイルを削除したりすることはできますか。

はい、できますが、変更を行った後でバンドルに再署名する必要があり `.app` ます。

`.ipa`通常の使用では、ファイルの変更は必要ありません。 この記事は、あくまで情報提供を目的として提供されています。

## <a name="example-removing-a-file-from-a-ipa-archive"></a>例: アーカイブからファイルを削除する `.ipa`

この例では、Xamarin. iOS プロジェクトの名前がで、がであることを前提としています。 `iPhoneApp1` `generated session id``cc530d20d6b19da63f6f1c6f67a0a254`

1. `.ipa`Visual Studio から通常どおりファイルをビルドします。

2. Mac ビルドホストに切り替えます。

3. フォルダー内のビルドを検索し `~/Library/Caches/Xamarin/mtbs/builds` ます。 このパスを Finder に貼り付けることができます **> [フォルダーに移動**] を選択して finder でフォルダーを参照 > ます。 プロジェクト名と一致するフォルダーを探します。 そのフォルダー内で、ビルドのに一致するフォルダーを探し `generated session id` ます。 これは、ほとんどの場合、最新の変更時刻を持つサブフォルダーです。

4. 新しいウィンドウを開き `Terminal.app` ます。

5. `cd`[アプリ] ウィンドウに「」と入力し、フォルダーをドラッグ & て `generated session id` ウィンドウにドロップし `Terminal.app` ます。

    ![Finder で生成されたセッション id フォルダーを検索する](modify-ipa-images/session-id-folder.png)

6. フォルダーにディレクトリを変更するには、return キーを入力し `generated session id` ます。

7. `.ipa` `old/` 次のコマンドを使用して、一時フォルダーにファイルを解凍します。 特定の `Ad-Hoc` `iPhoneApp1` プロジェクトに必要な名前と名前を調整します。

    > ditto-xk bin/iPhone/Ad-Hoc/iPhoneApp1-1.0. ipa old/

8. ウィンドウを開いたままに `Terminal.app` します。

9. から目的のファイルを削除し `.ipa` ます。 Finder を使用してごみ箱に移動するか、を使用してコマンドラインで削除することができ `Terminal.app` ます。 Finder でファイルの内容を表示するには、ファイルを管理し、[ `Payload/iPhone` **パッケージの内容を表示**] を選択します。

10. 手順 3. と同じ一般的な方法を使用して、プロジェクト名とという名前のログファイルを探し `~/Library/Logs/Xamarin/MonoTouchVS/` `generated session id` ます。 ![ Finder でプロジェクトビルドログを検索します。](modify-ipa-images/build-log.png)

11. 手順10のビルドログを開きます。たとえば、ダブルクリックします。

12. に含まれる行を探し `tool /usr/bin/codesign execution started with arguments: -v --force --sign` ます。

13. `/usr/bin/codesign`手順 8. の [アプリ] ウィンドウに「」と入力します。

14. から始まるすべての引数を、 `-v` 手順 12. の行からコピーし、それらをターミナルアプリウィンドウに貼り付けます。

15. 最後の引数をフォルダー内にあるバンドルになるように変更 `.app` `old/Payload/` してから、コマンドを実行します。

    ```bash
    /usr/bin/codesign -v --force --sign SOME_LONG_STRING in/iPhone/Ad-Hoc/iPhoneApp1.app/ResourceRules.plist --entitlements obj/iPhone/Ad-Hoc/Entitlements.xcent old/Payload/iPhoneApp1.app
    ```

16. `old/`ターミナルのディレクトリに移動します。

    ```bash
    cd old
    ```

17. コマンドを使用して、ディレクトリの内容を新しいファイルに圧縮し `.ipa` `zip` ます。 `"$HOME/Desktop/iPhoneApp1-1.0.ipa"`ファイルを出力するように引数を変更するには、次のようにし `.ipa` ます。

    ```bash
    zip -yr "$HOME/Desktop/iPhoneApp1-1.0.ipa" *
    ```

## <a name="common-error-messages"></a>一般的なエラー メッセージ

が表示される場合は `Invalid Signature. A sealed resource is missing or invalid.` 、通常、バンドル内で何らかの変更が行われたこと、 `.app` およびバンドルが後で正しく再署名されていないことを意味し `.app` ます。 配布プロファイルを使用してを作成する場合は、配布プロファイルを使用して元のを作成する必要があることにも注意して `.ipa` _ください_ `.ipa` 。 それ以外の場合、は `Entitlements.xcent` 正しくありません。

このエラーが発生する具体的な例を示すために、手順9の後にターミナルウィンドウで次のコマンドを実行すると、エラー `codesign --verify` の正確な原因と共にエラーが表示されます。

```bash
$ codesign -dvvv --no-strict --verify old/Payload/iPhoneApp1.app
old/Payload/iPhoneApp1.app: a sealed resource is missing or invalid
file missing: /Users/macuser/Library/Caches/Xamarin/mtbs/builds/iPhoneApp1/cc530d20d6b19da63f6f1c6f67a0a254/old/Payload/iPhoneApp1.app/MyFile.png
```

また、アプリストアの検証プロセスでも、次のようなエラーメッセージが報告されます。

> エラー ITMS-90035: "署名が無効です。 封印されたリソースがないか、無効です。 パス [iPhoneApp1/iPhoneApp1] のバイナリに無効な署名が含まれています。 アドホック証明書または開発証明書ではなく、配布証明書を使用してアプリケーションに署名していることを確認します。 Xcode のコード署名設定がターゲットレベルで正しいことを確認します (プロジェクトレベルのすべての値をオーバーライドします)。 また、アップロードするバンドルが、シミュレーターターゲットではなく、Xcode のリリースターゲットを使用してビルドされていることを確認します。 コード署名の設定が正しいことがわかっている場合は、Xcode で [すべてクリーン] を選択し、Finder の "build" ディレクトリを削除して、リリースターゲットをリビルドします。 詳細については、「」を参照してください。 [https://developer.apple.com/library/ios/documentation/Security/Conceptual/CodeSigningGuide/Introduction/Introduction.html](https://developer.apple.com/library/ios/documentation/Security/Conceptual/CodeSigningGuide/Introduction/Introduction.html)
