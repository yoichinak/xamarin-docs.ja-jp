---
title: Xamarin.Android Errors Matrix
ms.topic: article
ms.prod: xamarin
ms.assetid: 7EBE4C01-8EFC-4B7E-97BA-D879994F59AB
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 1c9284f3db1c503b5c88f0d310f916f4c9e3363d
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2018
---
# <a name="xamarinandroid-errors-matrix"></a>Xamarin.Android Errors Matrix

## <a name="errors-reference"></a>エラーのリファレンス

このドキュメントでは、Xamarin からさまざまなエラー コードの一部の情報を提供します。

<table>
    <thead>
        <tr>
            <td>カテゴリ</td>
            <td>説明</td>
        </tr>
    </thead>
    <tbody>
        <tr><td>XA0xxx</td><td><code>mandroid</code> エラー</td></tr>
        <tr><td>XA1xxx</td><td>ファイルのコピー/シンボリック リンク (関連するプロジェクト) エラー</td></tr>
        <tr><td>XA2xxx</td><td>リンカー エラー</td></tr>
        <tr><td>XA3xxx</td><td>AOT エラー</td></tr>
        <tr><td>XA4xxx</td><td>コード生成エラー</td></tr>
        <tr><td>XA5xxx</td><td>GCC およびツール チェーン エラー</td></tr>
        <tr><td>XA6xxx</td><td><code>mandroid</code> 内部 Tools エラー</td></tr>
        <tr><td>XA7xxx</td><td>予約されています。</td></tr>
        <tr><td>XA8xxx</td><td>予約されています。</td></tr>
        <tr><td>XA9xxx</td><td>ライセンス エラー</td></tr>
    </tbody>
</table>

## <a name="error-codes"></a>エラー コード

### <a name="xa0xxx-errors"></a>XA0xxx エラー

<table>
    <thead>
        <tr><td>エラー コード</td><td>説明</td></tr>
    </thead>
    <tbody>
        <tr><td>XA0000</td><td>予期しないエラーでバグ報告を入力してください<a href="http://bugzilla.xamarin.com">http://bugzilla.xamarin.com。</a></td></tr>
        <tr><td>XA0001</td><td>'-任意のデバイスに固有の操作なし devname が指定されました</td></tr>
        <tr><td>XA0002</td><td>環境変数 ' 0'} を解析できませんでした。</td></tr>
        <tr><td>XA0003</td><td>アプリケーション名は、SDK または製品のアセンブリ (.dll) 名との競合を {0} .exe です。</td></tr>
        <tr><td>XA0004</td><td>新しいカウント ロジックでは、sgen も有効にする必要があります。</td></tr>
        <tr><td>XA0005</td><td>出力ディレクトリ ' 0'} は存在しません</td></tr>
        <tr><td>XA0006</td><td>' 0'} で devel プラットフォームがない、--プラットフォームを使用して、SDK を指定する PLAT を =</td></tr>
        <tr><td>XA0007</td><td>ルート アセンブリ ' 0'} は存在しません</td></tr>
        <tr><td>XA0008</td><td>1 つのルート アセンブリのみを指定する必要があります。</td></tr>
        <tr><td>XA0009</td><td>アセンブリの読み込み中にエラー: {0}</td></tr>
        <tr><td>XA0010</td><td>コマンドライン引数を解析できませんでした: {0}</td></tr>
        <tr><td>XA0011</td><td>{0} に対して作成されているより新しいランタイム ({1}) より MonoTouch をサポートしています</td></tr>
        <tr><td>XA0012</td><td>完了に不完全なデータが提供される`{0}`です。</td></tr>
        <tr><td>XA0013</td><td>Sgen も有効にするプロファイルのサポートが必要です。</td></tr>
        <tr><td>XA0014</td><td>iOS {0} が ARMv6 を対象とするアプリケーションの構築をサポートしていません</td></tr>
        <tr><td>XA0020</td><td>Mandroid パスを特定できませんでした。</td></tr>
        <tr><td>XA0100</td><td>Android アプリケーション プロジェクトで EmbeddedNativeLibrary ' 0'} が正しくありません。 AndroidNativeLibrary を使用してください。</td></tr>
    </tbody>
</table>

### <a name="xa1xxx-errors"></a>XA1xxx エラー

<table>
    <thead>
        <tr><td>エラー コード</td><td>説明</td></tr>
    </thead>
    <tbody>
        <tr><td>XA1001</td><td>指定されたディレクトリにアプリケーションを見つけることができませんでした。</td></tr>
        <tr><td>XA1002</td><td>シンボリック リンクを作成できませんでした、ファイルがコピーされました。</td></tr>
        <tr><td>XA1003</td><td>アプリケーション ' 0'} を強制終了しませんでした。 アプリケーションを手動で強制終了する必要があります。</td></tr>
        <tr><td>XA1004</td><td>インストールされているアプリケーションの一覧を取得できませんでした。</td></tr>
        <tr><td>XA1005</td><td>デバイス '{1}' 上のアプリケーション ' 0'} を強制終了しませんでした: {2} です。 アプリケーションを手動で強制終了する必要があります。</td></tr>
        <tr><td>XA1006</td><td>デバイス '{1}' を ' 0'} アプリケーションをインストールできませんでした: {2} です。</td></tr>
        <tr><td>XA1007</td><td>デバイス '{1}' の ' 0'} アプリケーションの起動に失敗しました: {2} です。 タップして手動でアプリケーションを起動することができますも。</td></tr>
        <tr><td>XA1008</td><td>シミュレーターの起動に失敗しました: {0}</td></tr>
        <tr><td>XA1009</td><td>アセンブリ ' 0'} を '{1}' にコピーできませんでした: {2}</td></tr>
        <tr><td>XA1010</td><td>アセンブリ ' 0'} を読み込むことができませんでした: {1}</td></tr>
        <tr><td>XA1011</td><td>不足しているリソース ファイルを追加できませんでした: ' 0'}</td></tr>
        <tr><td>XA1101</td><td>アプリを開始できませんでした。</td></tr>
        <tr><td>XA1102</td><td>(強制終了) をアプリにアタッチできませんでした: {0}</td></tr>
        <tr><td>XA1103</td><td>デタッチできませんでした。</td></tr>
        <tr><td>XA1104</td><td>パケットの送信に失敗しました: {0}</td></tr>
        <tr><td>XA1105</td><td>予期しない応答の種類</td></tr>
        <tr><td>XA1106</td><td>デバイスに対するアプリケーションの一覧を取得できませんでした。 要求がタイムアウトしました。</td></tr>
        <tr><td>XA1107</td><td>アプリケーションを起動できませんでした。</td></tr>
        <tr><td>XA1201</td><td>シミュレーターを読み込むことができませんでした: {0}</td></tr>
        <tr><td>XA1301</td><td>ネイティブ ライブラリ`{0}`({1}) は現在のビルド ミラーサイト ({2}) が一致しないために、無視されました</td></tr>
    </tbody>
</table>

### <a name="xa2xxx-errors"></a>XA2xxx エラー

<table>
    <thead>
        <tr><td>エラー コード</td><td>説明</td></tr>
    </thead>
    <tbody>
        <tr><td>XA2001</td><td>アセンブリをリンクできませんでした。</td></tr>
        <tr><td>XA2002</td><td>参照を解決できません: {0}</td></tr>
        <tr><td>XA2003</td><td>リンクが無効になっているために、' 0'} オプションは無視されます。</td></tr>
        <tr><td>XA2004</td><td>余分なリンカー定義ファイル '{0}' が見つかりませんでした。</td></tr>
        <tr><td>XA2005</td><td>' 0'} から定義を解析できませんでした。</td></tr>
        <tr><td>XA2006</td><td>'{2}' からメタデータ項目 '{0}' ('{1}' で定義されている) への参照を解決できませんでした。</td></tr>
    </tbody>
</table>

### <a name="xa3xxx-errors"></a>XA3xxx エラー

これらは、AOT エラーです。

<table>
    <thead>
        <tr><td>エラー コード</td><td>説明</td></tr>
    </thead>
    <tbody>
        <tr><td>XA3001</td><td>AOT ' 0'} アセンブリではない可能性があります。</td></tr>
        <tr><td>XA3002</td><td>AOT 制限: [MonoPInvokeCallback] で装飾されているために、' 0'} メソッドが静的でなければなりません。</td></tr>
        <tr><td>XA3003</td><td>競合している--デバッグおよび--ある llvm オプション。 ソフト デバッグは無効です。</td></tr>
    </tbody>
</table>

### <a name="xa4xxx-errors"></a>XA4xxx エラー

これらは、コード生成エラーです。

<table>
    <thead>
        <tr><td>エラー コード</td><td>説明</td></tr>
    </thead>
    <tbody>
        <tr><td>XA4001</td><td>メインのテンプレートに expansed できませんでした`{0}`です。</td></tr>
        <tr><td>XA4101</td><td>レジストラーが型のシグネチャをビルドできません`{0}`です。</td></tr>
        <tr><td>XA4102</td><td>レジストラーが、無効な型が見つかりました`{0}`メソッドのシグネチャで`{2}`です。 代わりに、`{1}` を使用してください。</td></tr>
        <tr><td>XA4103</td><td>レジストラーが、無効な型が見つかりました`{0}`メソッドのシグネチャで`{2}`: 型は INativeObject を実装しますが、2 つを受け取るコンス トラクターがありません (IntPtr、bool) の引数</td></tr>
        <tr><td>XA4104</td><td>型の戻り値のマーシャ リングできないレジストラー`{0}`メソッドのシグネチャで`{1}`です。</td></tr>
        <tr><td>XA4105</td><td>レジストラーが、型のパラメーターをマーシャ リングできない`{0}`メソッドのシグネチャで`{1}`です。</td></tr>
        <tr><td>XA4106</td><td>構造体の戻り値のマーシャ リングできないレジストラー`{0}`メソッドのシグネチャで`{1}`です。</td></tr>
        <tr><td>XA4107</td><td>レジストラーが、型のパラメーターをマーシャ リングできない`{0}`メソッドのシグネチャで`{1}`です。</td></tr>
        <tr><td>XA4108</td><td>レジストラーがマネージ型にも種類を取得できません`{0}`です。</td></tr>
        <tr><td>XA4109</td><td>レジストラーが生成されたコードをコンパイルできませんでした。 バグ報告を送信してください<a href="http://bugzilla.xamarin.com">http://bugzilla.xamarin.com</a></td></tr>
        <tr><td>XA4110</td><td>型の out パラメーターをマーシャ リングできないレジストラー`{0}`メソッドのシグネチャで`{1}`です。</td></tr>
        <tr><td>XA4111</td><td>レジストラーが型のシグネチャをビルドできません`{0}' in method `{1}' です。</td></tr>
        <tr><td>XA4200</td><td>については 'claas' 型の生成だけです。</td></tr>
        <tr><td>XA4201</td><td>JNI 名 {0} 型を判別できません。</td></tr>
        <tr><td>XA4203</td><td>指定された型名は、完全修飾する必要があります。</td></tr>
        <tr><td>XA4204</td><td>インターフェイス型 ' 0'} を解決できません。 アセンブリ参照が存在することを確認してください。</td></tr>
        <tr><td>XA4205</td><td>[ExportField] は、0 パラメーターを持つメソッドでのみ使用できます。</td></tr>
        <tr><td>XA4206</td><td>[エクスポート] は、ジェネリック型では使用できません。</td></tr>
        <tr><td>XA4207</td><td>[ExportField] は、ジェネリック型では使用できません。</td></tr>
        <tr><td>XA4208</td><td>[Java.Interop.ExportFieldAttribute] は、void を返すメソッドでは使用できません。</td></tr>
        <tr><td>XA4209</td><td>JavaTypeInfo クラスの作成に失敗しました: {0} {1} 原因</td></tr>
        <tr><td>XA4210</td><td>ExportAttribute または ExportFieldAttribute を使用する場合は、Mono.Android.Export.dll への参照を追加する必要があります。</td></tr>
        <tr><td>XA4211</td><td>AndroidManifest.xml //uses-sdk/@android:targetSdkVersion ' 0'} がよりも小さい $(TargetFrameworkVersion) '{1}' です。 API を使用して \  のについてコンパイルします。</td></tr>
    </tbody>
</table>

### <a name="xa5xxx-errors"></a>XA5xxx エラー

<table>
    <thead>
        <tr><td>エラー コード</td><td>説明</td></tr>
    </thead>
    <tbody>
        <tr><td>XA5101</td><td>不足している ' 0'} コンパイラです。 Android NDK をインストールしてください。</td></tr>
        <tr><td>XA5102</td><td>アセンブリからネイティブ コードへの変換が失敗しました。 バグ報告を送信してください<a href="http://bugzilla.xamarin.com">http://bugzilla.xamarin.com</a></td></tr>
        <tr><td>XA5103</td><td>ファイル '{0}' のコンパイルに失敗しました。 バグ報告を送信してください<a href="http://bugzilla.com">http://bugzilla.xamarin.com</a></td></tr>
        <tr><td>XA5201</td><td>ネイティブのリンクに失敗しました。 Gcc に提供されるユーザー フラグを確認してください: {0}</td></tr>
        <tr><td>XA5202</td><td>ネイティブのリンクに失敗しました。 ビルド ログを確認してください。</td></tr>
        <tr><td>XA5303</td><td>警告をリンク ネイティブ: {0}</td></tr>
        <tr><td>XA5300</td><td>Android SDK は、見つからないか、完全にインストールされています。</td></tr>
        <tr><td>XA5301</td><td>'削除' のツールがありません。 Xcode コマンド ライン ツール コンポーネントをインストールしてください。</td></tr>
        <tr><td>XA5302</td><td>不足している 'dsymutil' ツールです。 Xcode コマンド ライン ツール コンポーネントをインストールしてください。</td></tr>
        <tr><td>XA5203</td><td>デバッグ シンボル (dSYM ディレクトリ) を生成できませんでした。 ビルド ログを確認してください。</td></tr>
        <tr><td>XA5204</td><td>最終的なバイナリを削除できませんでした。 ビルド ログを確認してください。</td></tr>
        <tr><td>XA5205</td><td>不足している 'aapt' ツールです。 Android SDK Build ツール パッケージをインストールしてください。</td></tr>
        <tr><td>XA5206</td><td>{0}. Android のリソース ディレクトリ {1} が存在しません。</td></tr>
        <tr><td>XA5207</td><td>{0}. Java ライブラリ ファイル {1} が存在しません。</td></tr>
        <tr><td>XA5208</td><td>ダウンロードに失敗しました。 {0} をダウンロードし、{1} ディレクトリに配置してください。</td></tr>
        <tr><td>XA5209</td><td>解凍できませんでした。 {0} をダウンロードし、{1} ディレクトリに抽出してください。</td></tr>
        <tr><td>XA5210</td><td>{0}. ネイティブ ライブラリ ファイル {1} が存在しません。</td></tr>
    </tbody>
</table>

### <a name="xa6xxx-errors"></a>XA6xxx エラー

<table>
    <thead>
        <tr><td>エラー コード</td><td>説明</td></tr>
    </thead>
    <tbody>
        <tr><td>XA6001</td><td>セシルの実行中のバージョンは、アセンブリの削除 () をサポートしていません</td></tr>
        <tr><td>XA6002</td><td>アセンブリを削除できませんでした`{0}`です。</td></tr>
        <tr><td>XA6003</td><td>UnauthorizedAccessException メッセージ]</td></tr>
    </tbody>
</table>

### <a name="xa9xxx-errors"></a>XA9xxx エラー

このエラー コードは、ライセンスとライセンス認証のエラーです。



#### <a name="xa9000"></a>XA9000

 **原因:**ライセンスの有効期限が切れて

 **中にチェック:**ビルド

<table>
    <thead>
        <tr>
            <th>スターター</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>会社住所</th>
            <th>エンタープライズ</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>警告</td>
            <td>警告</td>
            <td>警告</td>
            <td>警告</td>
            <td>警告</td>
        </tr>
    </tbody>
</table>



#### <a name="xa9001"></a>XA9001

 **原因:**試用版の有効期限が切れて

 **中にチェック:**ビルド

<table>
    <thead>
        <tr>
            <th>スターター</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>会社住所</th>
            <th>エンタープライズ</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>警告</td>
            <td>警告</td>
            <td>警告</td>
            <td>警告</td>
            <td>警告</td>
        </tr>
    </tbody>
</table>



#### <a name="xa9002"></a>XA9002

 **原因:** AndroidJavaSource

 **中にチェック:**ビルド

<table>
    <thead>
        <tr>
            <th>スターター</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>会社住所</th>
            <th>エンタープライズ</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>エラー</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
        </tr>
    </tbody>
</table>

 **原因:** AndroidJavaLibrary

 **中にチェック:**ビルド

<table>
    <thead>
        <tr>
            <th>スターター</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>会社住所</th>
            <th>エンタープライズ</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>エラー</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
        </tr>
    </tbody>
</table>

 **バインド アセンブリに埋め込まれている .jar がある場合は、ビルド時間ではなく、パッケージの時にキャッチこれはします。**

 **原因:** AndroidNativeLibrary

 **中にチェック:**ビルド

<table>
    <thead>
        <tr>
            <th>スターター</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>会社住所</th>
            <th>エンタープライズ</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>エラー</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
        </tr>
    </tbody>
</table>



#### <a name="xa9003"></a>XA9003

 **原因:** System.Runtime.Serialization

 **中にチェック:**パッケージ

<table>
    <thead>
        <tr>
            <th>スターター</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>会社住所</th>
            <th>エンタープライズ</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>エラー</td>
            <td>エラー</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
        </tr>
    </tbody>
</table>

 **原因:** System.ServiceModel.Web

 **中にチェック:**パッケージ

<table>
    <thead>
        <tr>
            <th>スターター</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>会社住所</th>
            <th>エンタープライズ</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>エラー</td>
            <td>エラー</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
        </tr>
    </tbody>
</table>

 **原因:** Mono.Data.Tds

 **中にチェック:**ビルド

<table>
    <thead>
        <tr>
            <th>スターター</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>会社住所</th>
            <th>エンタープライズ</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>エラー</td>
            <td>エラー</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
        </tr>
    </tbody>
</table>

 **これは許可されて System.Data.dll によって参照されるこの**

 **原因:** Mono.Android.Export

 **中にチェック:**ビルド

<table>
    <thead>
        <tr>
            <th>スターター</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>会社住所</th>
            <th>エンタープライズ</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>エラー</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
        </tr>
    </tbody>
</table>



#### <a name="xa9004"></a>XA9004

 **原因:** --プロファイリング

 **中にチェック:**パッケージ

<table>
    <thead>
        <tr>
            <th>スターター</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>会社住所</th>
            <th>エンタープライズ</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>エラー</td>
            <td>エラー</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
        </tr>
    </tbody>
</table>



#### <a name="xa9005"></a>XA9005

 **原因:**サイズの制限 (32 kb)

 **中にチェック:**パッケージ

<table>
    <thead>
        <tr>
            <th>スターター</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>会社住所</th>
            <th>エンタープライズ</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>エラー</td>
            <td>OK</td>
            <td>-</td>
            <td>-</td>
            <td>-</td>
        </tr>
    </tbody>
</table>



#### <a name="xa9006"></a>XA9006

 **原因:** System.Data.SqlClient 名前空間

 **中にチェック:**パッケージ

<table>
    <thead>
        <tr>
            <th>スターター</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>会社住所</th>
            <th>エンタープライズ</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>エラー</td>
            <td>エラー</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
        </tr>
    </tbody>
</table>



#### <a name="xa9008"></a>XA9008

 **原因:**コマンドラインからのビルド

 **中にチェック:**ビルド

<table>
    <thead>
        <tr>
            <th>スターター</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>会社住所</th>
            <th>エンタープライズ</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>エラー</td>
            <td>エラー</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
        </tr>
    </tbody>
</table>



#### <a name="xa9009"></a>XA9009

 **原因:** Missing シリアル番号

 **中にチェック:**しよう

<table>
    <thead>
        <tr>
            <th>スターター</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>会社住所</th>
            <th>エンタープライズ</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>エラー</td>
            <td>エラー</td>
            <td>エラー</td>
            <td>エラー</td>
            <td>エラー</td>
        </tr>
    </tbody>
</table>



#### <a name="xa9010"></a>XA9010

 **原因:** ProductId が無効です

 **中にチェック:**ビルド

<table>
    <thead>
        <tr>
            <th>スターター</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>会社住所</th>
            <th>エンタープライズ</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>エラー</td>
            <td>エラー</td>
            <td>エラー</td>
            <td>エラー</td>
            <td>エラー</td>
        </tr>
    </tbody>
</table>

XA9018 に相当する機能



#### <a name="xa9011"></a>XA9011

 **原因:**にライセンス ファイル (新しいファイル形式) を更新できませんでした

 **中にチェック:**アクティブ化

<table>
    <thead>
        <tr>
            <th>スターター</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>会社住所</th>
            <th>エンタープライズ</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>エラー</td>
            <td>エラー</td>
            <td>エラー</td>
            <td>エラー</td>
            <td>エラー</td>
        </tr>
    </tbody>
</table>



#### <a name="xa9012"></a>XA9012

 **原因:**いないインターネット

 **中にチェック:**アクティブ化

<table>
    <thead>
        <tr>
            <th>スターター</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>会社住所</th>
            <th>エンタープライズ</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>エラー</td>
            <td>エラー</td>
            <td>エラー</td>
            <td>エラー</td>
            <td>エラー</td>
        </tr>
    </tbody>
</table>



#### <a name="xa9013"></a>XA9013

 **原因:**不明なエラー

 **中にチェック:**アクティブ化

<table>
    <thead>
        <tr>
            <th>スターター</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>会社住所</th>
            <th>エンタープライズ</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>エラー</td>
            <td>エラー</td>
            <td>エラー</td>
            <td>エラー</td>
            <td>エラー</td>
        </tr>
    </tbody>
</table>



#### <a name="xa9014"></a>XA9014

 **原因:**無効なアクティブ化コード

 **中にチェック:**アクティブ化

<table>
    <thead>
        <tr>
            <th>スターター</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>会社住所</th>
            <th>エンタープライズ</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>エラー</td>
            <td>エラー</td>
            <td>エラー</td>
            <td>エラー</td>
            <td>エラー</td>
        </tr>
    </tbody>
</table>



#### <a name="xa9017"></a>XA9017

 **原因:**ライセンス認証サーバーでは、有効なライセンスを返しません

 **中にチェック:**アクティブ化

<table>
    <thead>
        <tr>
            <th>スターター</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>会社住所</th>
            <th>エンタープライズ</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>エラー</td>
            <td>エラー</td>
            <td>エラー</td>
            <td>エラー</td>
            <td>エラー</td>
        </tr>
    </tbody>
</table>


#### <a name="xa9018"></a>XA9018

**原因:**無効なライセンス

**中にチェック:**ビルド

<table>
    <thead>
        <tr>
            <th>スターター</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>会社住所</th>
            <th>エンタープライズ</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>-</td>
            <td>-</td>
            <td>エラー</td>
            <td>-</td>
            <td>-</td>
        </tr>
    </tbody>
</table>
