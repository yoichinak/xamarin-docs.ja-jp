---
title: パート 4 - 複数のプラットフォームを処理します。
description: このドキュメントでは、プラットフォームや機能に基づくアプリケーションの相違を処理する方法について説明します。 これは、画面サイズ、ナビゲーションのメタファ、タッチとジェスチャ、プッシュ通知、およびリストやタブなどのインターフェイスのパラダイムについて説明します。
ms.prod: xamarin
ms.assetid: BBE47BA8-78BC-6A2B-63BA-D1A45CB1D3A5
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 4a60c99cbc9819f07b77bfe9abe046ea92a550a5
ms.sourcegitcommit: 081a2d094774c6f75437d28b71d22607e33aae71
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37403326"
---
# <a name="part-4---dealing-with-multiple-platforms"></a>パート 4 - 複数のプラットフォームを処理します。

## <a name="handling-platform-divergence-amp-features"></a>処理プラットフォーム ダイバージェンス&amp;機能

分岐 'プラットフォーム' 問題ではだけ; ではありません。'同じ' のプラットフォーム上のデバイスでは、さまざまな機能 (特に利用可能な Android デバイスのさまざまな) があります。 最も明白で基本的な画面サイズが、その他のデバイス属性が異なるし、アプリケーションが特定の機能を確認して、(存在の有無) に基づいて異なる方法で動作が必要です。

つまり、すべてのアプリケーションは、魅力的ではありません、最小公分母の機能セットを表示するか、または、機能のグレースフル デグラデーションを処理する必要があります。 Xamarin の密接な統合によるネイティブ Sdk の各プラットフォームでは、これらの機能を使用するアプリを設計するためのプラットフォーム固有の機能を利用するアプリケーションを許可します。

プラットフォームの違いは、機能の概要については、プラットフォームの機能ドキュメントを参照してください。

 <a name="Examples_of_Platform_Divergence" />


### <a name="examples-of-platform-divergence"></a>プラットフォームの相違の例

 <a name="Fundamental_elements_that_exist_across_platforms" />


#### <a name="fundamental-elements-that-exist-across-platforms"></a>プラットフォーム間で存在する基本的な要素

ユニバーサルなモバイル アプリケーションのいくつかの特性があります。
これらより高度な概念であり、すべてのデバイスの場合は true。 一般的には、アプリケーションの設計の基礎を形成できるためです。

-  タブまたはメニューを使用して機能の選択
-  データとスクロールの一覧
-  データの 1 つのビュー
-  データの 1 つのビューの編集
-  戻る


画面の高度なフローを設計するときに、一般的なユーザー エクスペリエンスをこれらの概念に基づいてできます。

 <a name="platform-specific_attributes" />


#### <a name="platform-specific-attributes"></a>プラットフォーム固有の属性

すべてのプラットフォーム上に存在する基本的な要素だけでなく、設計の主要プラットフォーム差異をアドレスに必要になります。 これらの相違点を検討してください (および処理するには、具体的にはコードを記述) する必要があります。

-   **画面サイズ**– 一部のプラットフォーム (iOS、Windows Phone の以前のバージョンなど) が対象とする比較的単純な画面サイズが標準化されています。 Android デバイスがあるさまざまな画面サイズは、アプリケーションでサポートするために多くの労力が必要です。
-   **ナビゲーションのメタファ**-(例: プラットフォーム間での違い ハードウェア [戻る] ボタン、パノラマ UI コントロール) およびプラットフォーム (Android 2、4、iPhone と iPad) 内で。
-   **キーボード**– 一部の Android デバイス物理キーボードにが他のユーザーのみソフトウェア キーボード。 ソフト キーボードを隠ぺいすると、画面の一部を検出するコードは、これらの違いに小文字を区別する必要があります。
-   **タッチとジェスチャ**– 各オペレーティング システムの旧バージョンでは特に、ジェスチャ認識のサポートはオペレーティング システムが異なります。 以前のバージョンの Android が非常に古いデバイスをサポートするいると別のコードが必要があります、つまり、タッチ操作のサポートの制限
-   **プッシュ通知**-(例: 各プラットフォームでさまざまな機能と実装があります。 Windows 上のタイルをライブします)。


 <a name="Device-specific_features" />


#### <a name="device-specific-features"></a>デバイス固有の機能

決定する、アプリケーションに必要な最小限の機能する必要があります。または、各プラットフォームで利用するどのような追加機能を決定する際にします。 コードは、検出機能や機能を無効にする (例: 代替手段を提供する必要があります。 代わりに地理的場所にすることにより、ユーザーの場所を入力するか、マップから選択できます)。

-   **カメラ**– 機能は、デバイス間で異なる: 両方の前面と背面向きカメラを他のユーザーが一部のデバイスは、カメラがないです。 一部のカメラはビデオ記録できます。
-   **マップ (&)、Geo ロケーション**– GPS または Wi-fi の場所のサポートはすべてのデバイスに存在しません。 アプリは、各メソッドでサポートされている精度のさまざまなレベルの対応するためにも必要です。
-   **加速度計、ジャイロスコープおよび compass** – これらの機能は、各プラットフォームのデバイスの選択部分だけで見つかった多くの場合、アプリはほぼ常にハードウェアがサポートされていないときに、フォールバックを提供する必要があるため、します。
-   **Twitter や Facebook** iOS5 と iOS6 '組み込み' のみ – それぞれします。 以前のバージョンとその他のプラットフォームで独自の認証機能を提供し、各と直接やり取りする必要がありますサービスの API。
-   **近距離通信 (NFC) のほぼ**– (一部) でのみ (の執筆時点) での Android フォンです。


 <a name="Dealing_with_Platform_Divergence" />


### <a name="dealing-with-platform-divergence"></a>プラットフォームの相違を処理します。

同じコード ベース、それぞれ長所と短所の独自セットが複数のプラットフォームをサポートする 2 つの異なるアプローチがあります。

-   **プラットフォームの抽象化**– ビジネス ファサード パターンでは、プラットフォーム間で統一されたアクセスを提供し、単一で統一された API に特定のプラットフォームの実装を抽象化します。
-   **異なる実装**– 特定のプラットフォームの呼び出しは、インターフェイス、継承、条件付きコンパイルなどのアーキテクチャ ツールを使用して異なる実装を使用して機能します。


 <a name="Platform_Abstraction" />


## <a name="platform-abstraction"></a>プラットフォームの抽象化

 <a name="Class_Abstraction" />


### <a name="class-abstraction"></a>クラスの抽象化

インターフェイスまたは基本クラスを使用して、共有コードで定義されているし、実装またはプラットフォームに固有のプロジェクトで拡張します。 拡張クラスの抽象化で共有コードの記述とは特にために適していますポータブル クラス ライブラリが使用できるようにするフレームワークの限定されたサブセットがあるし、プラットフォーム固有のコード分岐をサポートするためにコンパイラ ディレクティブを含めることはできません。

 <a name="Interfaces" />


#### <a name="interfaces"></a>インターフェイス

インターフェイスを使用して一般的なコードを活用するために、共有ライブラリに渡すことができますもプラットフォームに固有のクラスを実装することができます。

インターフェイスは、共有されるコードで定義されている、共有ライブラリにパラメーターまたはプロパティとして渡されます。

プラットフォーム固有のアプリケーションでは、インターフェイスを実装でき、これを '処理' 共有コードも利用することができます。

 **長所**

実装では、プラットフォーム固有のコードを含めることができ、プラットフォーム固有の外部ライブラリを参照していることができます。

 **短所**

作成し、共有コードに実装を渡すことです。 共有コード内の深いインターフェイスを使用する場合されている複数のメソッド パラメーターを通じて渡されるまたは終了呼び出しチェーンによってそれ以外の場合にプッシュ ダウンします。 共有コードは、多数の異なるインターフェイスを使用している場合する必要がありますすべてが作成し、共有コードをどこかに設定します。

 <a name="Inheritance" />


#### <a name="inheritance"></a>継承

共有コードは、1 つまたは複数のプラットフォーム固有プロジェクトで拡張することが抽象または仮想のクラスを実装できます。 これは似ていますが既に実装されているいくつかの動作インターフェイスを使用します。 インターフェイスまたは継承がより優れた設計の選択がかどうかに別のビュー ポイントがある。 具体的には c# のみで継承を 1 つあるため、これが決定できる今後 Api を設計する方法。 継承を慎重に使用します。

長所と短所のインターフェイスの基本クラスが実装コードの一部 (おそらく、プラットフォーム全体に依存しない実装必要に応じて拡張することもできます) を含む追加の利点と、継承に等しく適用されます。

<a name="Xamarin.Forms" />

### <a name="xamarinforms"></a>Xamarin.Forms

参照してください、 [Xamarin.Forms](~/xamarin-forms/get-started/index.md)ドキュメント。


### <a name="plug-in-cross-platform-functionality"></a>プラグインのクロス プラットフォーム機能

プラグインを使用して一貫した方法でクロス プラットフォーム アプリを拡張することもできます。

リンクから、[プラグイン github](https://github.com/xamarin/plugins)、ほとんどのプラグインはオープン ソース プロジェクト (通常は Nuget 経由でのインストールの使用可能な) する際に役立つさまざまな設定、バッテリの状態から、プラットフォーム固有の機能を実装する、簡単に Xamarin プラットフォームおよび Xamarin.Forms アプリで使用できるは一般的な API です。


<a name="Other_Cross-Platform_Libraries" />

### <a name="other-cross-platform-libraries"></a>その他のクロス プラットフォーム ライブラリ

クロス プラットフォームの機能を提供する利用可能なサード パーティ製のライブラリを数多くあります。

-   **MvvmCross** -  [https://github.com/slodge/MvvmCross/](https://github.com/slodge/MvvmCross/)
-   **専門用語**(のローカリゼーション) -  [https://github.com/rdio/vernacular/](https://github.com/rdio/vernacular/)
-   **MonoGame** (の XNA ゲーム) -  [http://www.monogame.net](http://www.monogame.net)
-   **NGraphics** - [NGraphics](https://github.com/praeclarum/NGraphics)とその前段階 [https://github.com/praeclarum/CrossGraphics](https://github.com/praeclarum/CrossGraphics)


 <a name="Divergent_Implementation" />


### <a name="divergent-implementation"></a>分岐の実装

 <a name="Conditional_Compilation" />


#### <a name="conditional-compilation"></a>条件付きコンパイル

これは、状況によっては、共有コードは、可能性があるクラスまたは動作が異なる機能にアクセスする、各プラットフォームで異なる方法で動作する必要があります。 条件付きコンパイルは、共有資産プロジェクトを同じソース ファイルを異なるシンボルが定義されている複数のプロジェクトで参照されているで最適に動作します。

Xamarin プロジェクトは常に定義`__MOBILE__`は iOS と Android アプリケーションのプロジェクト (二重下線前と後の修正プログラムでこれらのシンボルに注意してください) の両方に当てはまります。

```csharp
#if __MOBILE__
// Xamarin iOS or Android-specific code
#endif
```

<a name="iOS" />

##### <a name="ios"></a>iOS

Xamarin.iOS 定義`__IOS__`iOS デバイスを検出するために使用することができます。

```csharp
#if __IOS__
// iOS-specific code
#endif
```

ウォッチおよびテレビに固有のシンボルもあります。

```csharp
#if __TVOS__
// tv-specific stuff
#endif

#if __WATCHOS__
// watch-specific stuff
#endif
```

<a name="Android" />

##### <a name="android"></a>Android

Xamarin.Android アプリケーションにのみコンパイルするコードを次に使用できます。

```csharp
#if __ANDROID__
// Android-specific code
#endif
```

各 API のバージョンは、新しいコンパイラ ディレクティブも定義されています。 の機能を追加する新しい API が対象とする場合、このようなコードを使用します。 API レベルごとには、レベル '低い' のすべてのシンボルが含まれています。 この機能は複数のプラットフォームをサポートするため、本当に便利です。通常、`__ANDROID__`シンボルが十分になります。

```csharp
#if __ANDROID_11__
// code that should only run on Android 3.0 Honeycomb or newer
#endif
```

##### <a name="mac"></a>Mac

現在、Xamarin.Mac 向けの組み込みのシンボルはありませんが、Mac で独自のアプリ プロジェクトを追加する**オプション > ビルド > コンパイラ**で、**シンボル定義**ボックス、または編集、 **.csproj**ファイルし、追加があります (たとえば`__MAC__`)

```xml
<PropertyGroup><DefineConstants>__MAC__;$(DefineConstants)</DefineConstants></PropertyGroup>
```

<a name="Windows_Phone" />

##### <a name="windows-phone"></a>Windows Phone

Windows Phone アプリ – 2 つのシンボルを定義する`WINDOWS_PHONE`と`SILVERLIGHT`– コードがプラットフォームを対象にできます。 アンダー スコアなど、Xamarin プラットフォームの記号で囲まれたは必要はありません。


<a name="Using_Conditional_Compilation" />

##### <a name="using-conditional-compilation"></a>条件付きコンパイルを使用します。

条件付きコンパイルの簡単なケース スタディの例では、SQLite データベース ファイルのファイルの場所を設定します。 3 つのプラットフォームでは、ファイルの場所を指定するための若干異なる要件があります。

-   **iOS** – Apple が非ユーザー データ (ライブラリのディレクトリ) の特定の場所に配置することが優先されますが、このディレクトリのシステムの定数はありません。 正しいパスをビルドするプラットフォーム固有のコードが必要です。
-   **Android** – によって返されるシステム パス`Environment.SpecialFolder.Personal`はデータベース ファイルを格納する、許容される場所です。
-   **Windows Phone** – 分離ストレージ メカニズムに完全なパスを指定するには、許可しない相対パスとファイル名だけです。
-   **ユニバーサル Windows プラットフォーム**– 使用`Windows.Storage`Api。

次のコードでは、条件付きコンパイルを使用して、`DatabaseFilePath`が各プラットフォームの正しい。

```csharp
public static string DatabaseFilePath {
        get {
    var filename = "TodoDatabase.db3";
#if SILVERLIGHT
    // Windows Phone 8
    var path = filename;
#else

#if __ANDROID__
    string libraryPath = Environment.GetFolderPath(Environment.SpecialFolder.Personal); ;
#else
#if __IOS__
        // we need to put in /Library/ on iOS5.1 to meet Apple's iCloud terms
        // (they don't want non-user-generated data in Documents)
        string documentsPath = Environment.GetFolderPath (Environment.SpecialFolder.Personal); // Documents folder
        string libraryPath = Path.Combine (documentsPath, "..", "Library");
#else
        // UWP
        string libraryPath = Windows.Storage.ApplicationData.Current.LocalFolder.Path;
#endif
#endif
        var path = Path.Combine (libraryPath, filename);
#endif
        return path;
}
```

結果は、ビルドおよび SQLite データベースのファイルを配置する各プラットフォームで別の場所ですべてのプラットフォームで使用できるクラスです。

