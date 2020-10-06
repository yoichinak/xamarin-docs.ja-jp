---
title: Xamarin.Android Environment
ms.prod: xamarin
ms.assetid: 67BFD4E1-276C-4B9F-9BD8-A5218D2BD529
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/15/2018
ms.openlocfilehash: da0e3775f400c965ee59a762884e638e3379c8df
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91454859"
---
# <a name="xamarinandroid-environment"></a>Xamarin.Android Environment

## <a name="execution-environment"></a>実行環境

*実行環境* は、プログラムの実行に影響を与える一連の環境変数と Android のシステム プロパティです。 Android のシステム プロパティは `adb shell setprop` コマンドで設定できますが、環境変数を設定するには `debug.mono.env` システム プロパティを設定します。

```shell
## Enable GREF logging
adb shell setprop debug.mono.log gref

## Set the MONO_LOG_LEVEL and MONO_LOG_MASK environment variables
## so that additional Mono messages will be written to `adb logcat`.
adb shell setprop debug.mono.env "'MONO_LOG_LEVEL=info|MONO_LOG_MASK=asm'"
```

Android システムのプロパティは、ターゲット デバイス上のすべてのプロセスに対して設定されます。

Xamarin.Android 4.6 以降、*環境ファイル* をプロジェクトに追加することで、アプリケーションごとにシステム プロパティと環境変数の両方を設定するか、オーバーライドすることができるようになりました。 環境ファイルは、[`AndroidEnvironment` の**ビルド アクション**](~/android/deploy-test/building-apps/build-process.md)を含む Unix 形式のプレーンテキスト ファイルです。
環境ファイルには、*キー = 値*形式の行が含まれています。
コメントは `#` で始まる行です。 空白行は無視されます。

*キー* が大文字で始まる場合、*キー* は環境変数として扱われます。**setenv** (3) は、プロセスの起動時に環境変数を指定された*値*に設定するために使用されます。

"*キー*" が小文字で始まる場合、"*キー*" は Android のシステム プロパティとして扱われます。"*値*" は "*既定値*" です。Xamarin.Android の実行動作を制御する Android システムのプロパティは、まず Android システムのプロパティ ストアから検索され、値が存在しない場合は、環境ファイルに指定されている値が使用されます。 これは、診断のために、`adb shell setprop` を使用して環境ファイルの値をオーバーライドできるようにするためです。

## <a name="xamarinandroid-environment-variables"></a>Xamarin.Android の環境変数

Xamarin.Android は `XA_HTTP_CLIENT_HANDLER_TYPE` 変数をサポートしています。この変数は `adb shell setprop debug.mono.env` または `$(AndroidEnvironment)` ビルド アクションを介して設定できます。

### `XA_HTTP_CLIENT_HANDLER_TYPE`

[HttpMessageHandler](/dotnet/api/system.net.http.httpmessagehandler?view=xamarinandroid-7.1) から継承する必要があり、[`HttpClient()` 既定コンストラクター](/dotnet/api/system.net.http.httpclient.-ctor?view=xamarinandroid-7.1#System_Net_Http_HttpClient__ctor)から構築されるアセンブリ修飾型です。

Xamarin.Android 6.1 では、この環境変数は既定では設定されておらず、[HttpClientHandler](/dotnet/api/system.net.http.httpclienthandler?view=xamarinandroid-7.1) が使用されます。

または、値 `Xamarin.Android.Net.AndroidClientHandler` でネットワーク アクセスに [`java.net.URLConnection`](xref:Java.Net.URLConnection) を使用するよう指定できます。
これでは、Android がサポートする場合、TLS 1.2 の使用を許可している*場合があります*。

Xamarin.Android 6.1 で追加されました。

## <a name="xamarinandroid-system-properties"></a>Xamarin.Android のシステム プロパティ

Xamarin.Android は以下のシステム プロパティをサポートしています。これらのシステム プロパティは、`adb shell setprop` または `$(AndroidEnvironment)` のビルド アクションによって設定できます。

- `debug.mono.debug`
- `debug.mono.env`
- `debug.mono.gc`
- `debug.mono.log`
- `debug.mono.max_grefc`
- `debug.mono.profile`
- `debug.mono.runtime_args`
- `debug.mono.trace`
- `debug.mono.wref`
- `XA_HTTP_CLIENT_HANDLER_TYPE`

### `debug.mono.debug`

`debug.mono.debug` システム プロパティの値は整数です。 `1` の場合は、プロセスが `mono --debug` で開始されたかのように動作します。
一般的に、これはスタック トレースなどのファイルと行の情報を示します。デバッガーからアプリケーションを起動する必要はありません。

### `debug.mono.env`

環境変数が `|` で区切られた一覧が含まれています。

### `debug.mono.gc`

`debug.mono.debug` システム プロパティの値は整数です。
`1` の場合は、GC 情報がログに記録されます。

これは、`debug.mono.log` システム プロパティに `gc` が含まれている場合に相当します。

### `debug.mono.log`

Xamarin.Android が `adb logcat` にログを記録する追加情報を制御します。
次のいずれかの値を含むコンマ区切りの文字列 (`,`) です。

- `all`: *すべての* メッセージを出力します。 `lref` メッセージが含まれているので、あまりお勧めしません。
- `assembly`: `.apk` を出力して、解析メッセージをアセンブリします。
- `gc`: GC 関連のメッセージを出力します。
- `gref`: JNI グローバル参照メッセージを出力します。
- `lref`: JNI ローカル参照メッセージを出力します。
  > [!NOTE]
  > これは*実際には*スパム `adb logcat` になります。
  > Xamarin.Android 5.1 では、`.__override__/lrefs.txt` ファイルも作成され、*巨大*なサイズになる可能性があります。
  > そのため、お勧めしません。
- `timing`: いくつかのメソッド タイミング情報を出力します。 この処理で、ファイル `.__override__/methods.txt` と `.__override__/counters.txt` も作成されます。

### `debug.mono.max_grefc`

`debug.mono.max_grefc` システム プロパティの値は整数です。
この値で、ターゲット デバイスの既定の検出された最大 GREF カウントが*オーバーライド*されます。

*注:***environment.txt** ファイルで適時に値を取得できないので、`adb shell setprop
debug.mono.max_grefc` でのみ使用できます。

### `debug.mono.profile`

`debug.mono.profile` システム プロパティでプロファイラーが有効になります。
これは `mono --profile` オプションに相当し、同じ値を使用します (詳細については、[**mono**(1)](http://docs.go-mono.com/?link=man%3amono(1)) のマニュアル ページを参照してください)。

### `debug.mono.runtime_args`

`debug.mono.runtime_args` システム プロパティには、**mono** で解析する必要がある追加オプションがあります。

### `debug.mono.trace`

システム プロパティ `debug.mono.trace` でトレースが有効になります。
これは `mono --trace` オプションに相当し、同じ値を使用します (詳細については、[**mono**(1)](http://docs.go-mono.com/?link=man%3amono(1)) のマニュアル ページを参照してください)。

一般的には*使用しないでください*。 トレースを使用すると、スパム `adb logcat` が出力され、プログラムの動作が大幅に遅くなり、プログラムの動作が変わります (最大の変化でエラー条件の追加など)。

ただし、*場合によっては*、トレースを利用して何らかの追加調査を実行できることがあります。

### `debug.mono.wref`

`debug.mono.wref` システム プロパティを使用すると、既定で検出された JNI の弱い参照メカニズムをオーバーライドすることができます。 サポートされている値は次の 2 つです。

- `jni`: `JNIEnv::NewWeakGlobalRef()` で作成され、`JNIEnv::DeleteWeakGlobalREf()` によって破棄される JNI の弱い参照を使用します。
- `java`: `java.lang.WeakReference` インスタンスを参照する JNI グローバル参照を使用します。

`java` は、既定では API-7 までと、ART が有効な場合は API-19 (Kit Kat) で使用されます (API-8 では `jni` の参照が追加され、ART によって `jni` の参照が "*破棄されました*")。

このシステム プロパティは、テストや特定の形式の調査に役立ちます。
*一般的に*、変更はお勧めしません。

### <a name="xa_http_client_handler_type"></a>XA\_HTTP\_CLIENT\_HANDLER\_TYPE

Xamarin.Android 6.1 で初めて導入されたこの環境変数では、`HttpClient` によって使用される既定の `HttpMessageHandler` 実装を宣言します。 既定では、この変数は設定されておらず、Xamarin.Android は `HttpClientHandler` を使用します。

```shell
XA_HTTP_CLIENT_HANDLER_TYPE=Xamarin.Android.Net.AndroidClientHandler
```

> [!NOTE]
> 基になる Android デバイスが TLS 1.2 をサポートしている必要があります。
Android 5.0 以降では TLS 1.2 をサポートしています。

## <a name="example"></a>例

```shell
## Comments are lines which start with '#'
## Blank lines are ignored.

## Enable GREF messages to `adb logcat`
debug.mono.log=gref

## Clear out a Mono environment variable to decrease logging
MONO_LOG_LEVEL=
```

## <a name="related-links"></a>関連リンク

- [トランスポート層セキュリティ](~/cross-platform/app-fundamentals/transport-layer-security.md)