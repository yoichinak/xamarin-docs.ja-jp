---
title: ABI 固有の APK のビルド
description: このドキュメントでは、Xamarin.Android を使用する単一の ABI をターゲットとする APK のビルド方法について説明します。
ms.prod: xamarin
ms.assetid: D21B195B-4530-4EB2-8704-5C4349A2CDD8
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/15/2018
ms.openlocfilehash: 4a3ba970f8ca32f0bfa2e5297e8052f3eb572ed0
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2019
ms.locfileid: "69525721"
---
# <a name="building-abi-specific-apks"></a>ABI 固有の APK のビルド

_このドキュメントでは、Xamarin.Android を使用する単一の ABI をターゲットとする APK のビルド方法について説明します。_



## <a name="overview"></a>概要

アプリケーションに複数の APK があると便利な場合があります。各 APK は同じキーストアで署名され、同じパッケージを共有しますが、特定のデバイスや Android 構成用にコンパイルされます。 この方法はお勧めできません。複数のデバイスと構成をサポートできる 1 つの APK を使用するほうが簡単です。 次のような状況では、複数の APK を作成すると便利な場合があります。

- **APK のサイズを小さくする** - Google Play では APK ファイルのサイズが 100 MB に制限されます。 デバイス固有の APK を作成すると、アプリケーションのアセットとリソースのサブセットを提供するだけで済むため、APK のサイズを小さくすることができます。

- **さまざまな CPU アーキテクチャをサポートする** - アプリケーションに特定の CPU の共有ライブラリがある場合、その CPU の共有ライブラリのみを配布できます。


APK が複数あると配布が困難になる場合がある - Google Play で対処されている問題。 Google Play は、アプリケーションのバージョン コードと **AndroidManifest.XML** とともに含まれるその他のメタデータに基づいて、正しい APK が確実にデバイスに配布されるようにします。 Google Play がアプリケーションでの複数の APK の使用をサポートする方法に関する詳細と制限事項については、[複数の APK サポートに関する Google のドキュメント](https://developer.android.com/google/play/publishing/multiple-apks.html)を参照してください。

このガイドでは、Xamarin.Android アプリケーションに対する複数の APK (それぞれ特定の ABI をターゲットとする) のビルドをスクリプト化する方法について説明します。 次のトピックが含まれます。

1. APK に固有の*バージョン コード* を作成する。
1. この APK で使用される一時的なバージョンの **AndroidManifest.XML** を作成する。
1. 前の手順の **AndroidManifest.XML** を使用して、アプリケーションをビルドする。
1. 署名および zipalign を実行して、リリース用に APK を準備する。


このガイドの最後は、[Rake](http://martinfowler.com/articles/rake.html) を使用して、これらの手順をスクリプト化する方法を示すチュートリアルです。



### <a name="creating-the-version-code-for-the-apk"></a>APK のバージョン コードの作成

Google では、7 桁のバージョン コードを使用するバージョン コードの特定のアルゴリズムを推奨しています ([複数の APK サポートに関するドキュメント](https://developer.android.com/google/play/publishing/multiple-apks.html)の*バージョン コード スキームの使用* に関するセクションを参照してください)。
このバージョン コード スキームを 8 桁に拡張することで、Google Play で正しい APK がデバイスに確実に配布されるように、バージョン コードにいくつかの ABI 情報を含めることができます。 以下のリストで、8 桁のバージョン コード形式について説明します (左から右にインデックスが付けられます)。

- **インデックス 0** (下図では赤色で示されている) &ndash; ABI の整数:
    - 1 &ndash; `armeabi`
    - 2 &ndash; `armeabi-v7a`
    - 6 &ndash; `x86`

- **インデックス 1 から 2** (下図ではオレンジ色で示されている) &ndash; アプリケーションでサポートされる最小の API レベル。

- **インデックス 3 から 4** (下図では青色で示されている) &ndash; サポートされる画面サイズ:
    - 1 &ndash; 小
    - 2 &ndash; 標準
    - 3 &ndash; 大
    - 4 &ndash; 特大

- **インデックス 5 から 7** (下図では緑色で示されている) &ndash; バージョン コードに固有の番号。 
    これは開発者によって設定されます。 アプリケーションの一般リリースごとに増えます。

下図に、上のリストで説明した各コードの位置を示します。

[![色分けされた、8 桁のバージョン コード形式の図](abi-specific-apks-images/image00.png)](abi-specific-apks-images/image00.png#lightbox)


Google Play は、`versionCode` と APK 構成に基づいて、正しい APK が確実にデバイスに配布されるようにします。 最高バージョン コードの APK がデバイスに配布されます。 たとえば、アプリケーションに以下のバージョン コードの 3 つの APK があるとします。

- 11413456 - ABI は `armeabi` で、API レベル 14 をターゲットとし、画面サイズは小から大で、バージョン番号は 456 です。
- 21423456 - ABI は `armeabi-v7a` で、API レベル 14 をターゲットとし、画面サイズは標準と大で、バージョン番号は 456 です。
- 61423456 - ABI は `x86` で、API レベル 14 をターゲットとし、画面サイズは標準と大で、バージョン番号は 456 です。

引き続きこの例を使用して、`armeabi-v7a` に固有のバグが修正されたとします。 アプリ バージョンは 457 に増え、`android:versionCode` は 21423457 に設定され、新しい APK がビルドされます。 `armeabi` および `x86` バージョンの versionCode はそのままです。

次に、x86 バージョンが、新しい API (レベル 19) をターゲットとするいくつかの更新プログラムまたはバグ修正を受け取るとします。これでアプリのバージョンが 500 になります。 新しい `versionCode` は 61923500 に変わりますが、armeabi/armeabi-v7a はそのままです。 この時点で、バージョン コードは次のようになります。

- 11413456 - ABI は `armeabi` で、API レベル 14 をターゲットとし、画面サイズは小から大で、バージョン名は 456 です。
- 21423457 - ABI は `armeabi-v7a` で、API レベル 14 をターゲットとし、画面サイズは標準と大で、バージョン名は 457 です。
- 61923500 - ABI は `x86` で、API レベル 19 をターゲットとし、画面サイズは標準と大で、バージョン名は 500 です。


これらのバージョン コードを手動で維持することは、開発者にとってかなりの負担になる場合があります。 正しい `android:versionCode` を計算してから APK をビルドするプロセスを自動化する必要があります。
この実行方法の例は、このドキュメントの最後のチュートリアルで説明します。


### <a name="create-a-temporary-androidmanifestxml"></a>一時的な AndroidManifest.XML を作成する

必ずしも必要ではありませんが、ABI ごとに一時的な **AndroidManifest.XML** を作成しておくと、APK 間での情報漏洩に伴い発生する可能性のある問題を防ぐのに役立つ場合があります。 たとえば、`android:versionCode` 属性が APK ごとに一意であることは非常に重要です。

この実行方法は関連するスクリプト システムによって異なりますが、通常は、開発時に使用される Android マニュフェストのコピーを作成し、それを変更してから、ビルド プロセス時にその変更したマニュフェストを使用します。



### <a name="compiling-the-apk"></a>APK のコンパイル

ABI ごとに APK をビルドする場合、以下のサンプル コマンド ラインに示すように、`xbuild` または `msbuild` を使用するのが最適です。

```bash
/Library/Frameworks/Mono.framework/Commands/xbuild /t:Package /p:AndroidSupportedAbis=<TARGET_ABI> /p:IntermediateOutputPath=obj.<TARGET_ABI>/ /p:AndroidManifest=<PATH_TO_ANDROIDMANIFEST.XML> /p:OutputPath=bin.<TARGET_ABI> /p:Configuration=Release <CSPROJ FILE>
```

以下のリストで、各コマンド ライン パラメーターについて説明します。

- `/t:Package` &ndash; デバッグ キーストアを使用して署名される Android APK を作成します。

- `/p:AndroidSupportedAbis=<TARGET_ABI>` &ndash; これはターゲットとなる ABI です。 `armeabi`、`armeabi-v7a`、`x86` のいずれかである必要があります。

- `/p:IntermediateOutputPath=obj.<TARGET_ABI>/` &ndash; これは、ビルドの一環として作成される中間ファイルを保持するディレクトリです。 必要に応じて、Xamarin.Android は、`obj.armeabi-v7a` など、ABI に基づく名前のディレクトリを作成します。 ビルド間でのファイルの "漏洩" に伴い発生する問題を防ぐため、ABI ごとに 1 つのフォルダーを使用することをお勧めします。 この値がディレクトリの区切り記号 (OS X の場合は `/`) で終わっていることに注目してください。

- `/p:AndroidManifest` &ndash; このプロパティでは、ビルド時に使用される **AndroidManifest.XML** ファイルへのパスを指定します。

- `/p:OutputPath=bin.<TARGET_ABI>` &ndash; これは、最終的な APK を格納するディレクトリです。 Xamarin.Android は、`bin.armeabi-v7a` など、ABI に基づく名前のディレクトリを作成します。

- `/p:Configuration=Release` &ndash; APK のリリース ビルドを実行します。 デバッグ ビルドを Google Play にアップロードすることはできません。

- `<CS_PROJ FILE>` &ndash; これは、Xamarin.Android プロジェクトの `.csproj` ファイルへのパスです。



### <a name="sign-and-zipalign-the-apk"></a>APK に署名して zipalign を実行する

Google Play を使用して配布する前に APK に署名する必要があります。 これは、Java Developer's Kit の一部である `jarsigner` アプリケーションを使用して実行できます。 以下のコマンド ラインでは、コマンド ラインでの `jarsigner` の使用方法を示します。

```shell
jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 -keystore <PATH/TO/KEYSTORE> -storepass <PASSWORD> -signedjar <PATH/FOR/SIGNED_JAR> <PATH/FOR/JAR/TO/SIGN> <NAME_OF_KEY_IN_KEYSTORE>
```

すべての Xamarin.Android アプリケーションは、デバイスで実行する前に zipalign を実行する必要があります。 以下は、使用するコマンド ラインの形式です。

```shell
zipalign -f -v 4 <SIGNED_APK_TO_ZIPALIGN> <PATH/TO/ZIP_ALIGNED.APK>
```


## <a name="automating-apk-creation-with-rake"></a>Rake による APK 作成の自動化

サンプル プロジェクトの [OneABIPerAPK](https://github.com/xamarin/monodroid-samples/tree/master/OneABIPerAPK) は単純な Android プロジェクトであり、ABI 固有のバージョン番号を計算し、以下の ABI ごとに別個の 3 つの APK をビルドする方法を示します。

- armeabi
- armeabi-v7a
- x86


サンプル プロジェクトの [rakefile](https://github.com/xamarin/monodroid-samples/blob/master/OneABIPerAPK/Rakefile.rb) では、前のセクションで説明した各手順を実行します。

1. APK の [android:versionCode を作成する](https://github.com/xamarin/monodroid-samples/blob/master/OneABIPerAPK/Rakefile.rb#L30)。

1. その APK のカスタム **AndroidManifest.XML** に [android:versionCode を書き込む](https://github.com/xamarin/monodroid-samples/blob/master/OneABIPerAPK/Rakefile.rb#L55)。

1. 前の手順で作成された **AndroidManifest.XML** を使用して、単独で ABI をターゲットとする Xamarin.Android プロジェクトの[リリース ビルドをコンパイルする](https://github.com/xamarin/monodroid-samples/blob/master/OneABIPerAPK/Rakefile.rb#L63)。

1. 運用キーストアで [APK に署名する](https://github.com/xamarin/monodroid-samples/blob/master/OneABIPerAPK/Rakefile.rb#L66)。

1. APK に [zipalign を実行する](https://github.com/xamarin/monodroid-samples/blob/master/OneABIPerAPK/Rakefile.rb#L67)。


アプリケーションの APK をすべてビルドするには、次のようにコマンド ラインから `build` Rake タスクを実行します。

```shell
$ rake build
==> Building an APK for ABI armeabi with ./Properties/AndroidManifest.xml.armeabi, android:versionCode = 10814120.
==> Building an APK for ABI x86 with ./Properties/AndroidManifest.xml.x86, android:versionCode = 60814120.
==> Building an APK for ABI armeabi-v7a with ./Properties/AndroidManifest.xml.armeabi-v7a, android:versionCode = 20814120.
```

rake タスクが完了すると、ファイル `xamarin.helloworld.apk` を含む 3 つの `bin` フォルダーが生成されます。 次のスクリーンショットには、これらの各フォルダーとそのコンテンツが示されています。

[![xamarin.helloworld.apk を含むプラットフォーム固有のフォルダーの場所](abi-specific-apks-images/image01.png)](abi-specific-apks-images/image01.png#lightbox)


> [!NOTE]
> このガイドで概説されているビルド プロセスは、さまざまなビルド システムのいずれかに実装できます。 事前に記述した例はありませんが、これは [Powershell](https://technet.microsoft.com/scriptcenter/powershell.aspx) / [psake](https://github.com/psake/psake) または [Fake](http://fsharp.github.io/FAKE/) でも可能です。


## <a name="summary"></a>まとめ

このガイドでは、特定の API をターゲットとする Android APK の作成方法を含め、いくつかの提案事項を提供しました。 また、APK が対象となる CPU アーキテクチャを特定する `android:versionCodes` を作成するための 1 つの考えられるスキーマについて説明しました。 チュートリアルには、Rake を使用してビルドがスクリプト化されたサンプル プロジェクトが含まれていました。



## <a name="related-links"></a>関連リンク

- [OneABIPerAPK (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/oneabiperapk)
- [アプリケーションの発行](~/android/deploy-test/publishing/index.md)
- [Google Play の複数の APK サポート](https://developer.android.com/google/play/publishing/multiple-apks.html)
