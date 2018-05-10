---
title: パート 4 - 複数のプラットフォームを処理します。
ms.prod: xamarin
ms.assetid: BBE47BA8-78BC-6A2B-63BA-D1A45CB1D3A5
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 0988eef07ead9b2cbb6a447ac54f80bcb80e66bf
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/09/2018
---
# <a name="part-4---dealing-with-multiple-platforms"></a>パート 4 - 複数のプラットフォームを処理します。

## <a name="handling-platform-divergence-amp-features"></a>プラットフォームの相違を処理&amp;機能

相違は 'クロスプラット フォーム' 問題だけです。 はありません。'同じ' プラットフォーム上のデバイスでは、異なる機能 (特に、広範な利用可能な Android デバイス) があります。 最も明白で基本的な画面サイズは他のデバイスの属性が異なるし、アプリケーションで特定の機能を確認し、(存在の有無) に基づいて異なる方法で動作する必要です。

つまり、すべてのアプリケーションが不適切になる、最も一般的な分母の機能セットを表示するか、または機能の正常な低下を扱う必要があります。 各プラットフォームのネイティブの Sdk での Xamarin の緊密な統合は、これらの機能を使用するアプリを設計する方が効率的、プラットフォーム固有の機能を利用するアプリケーションを許可します。

機能で、プラットフォームの違いの概要については、プラットフォームの機能ドキュメントを参照してください。

 <a name="Examples_of_Platform_Divergence" />


### <a name="examples-of-platform-divergence"></a>プラットフォームの相違の例

 <a name="Fundamental_elements_that_exist_across_platforms" />


#### <a name="fundamental-elements-that-exist-across-platforms"></a>プラットフォーム間で存在する基本的な要素

ユニバーサルのモバイル アプリケーションの一部の特性があります。
これらは、上位レベルの概念をすべてのデバイスの場合は true。 一般的には、アプリケーションのデザインの基礎を形成するためです。

-  タブまたはメニューを使用して機能の選択
-  データとスクロールの一覧
-  データの 1 つのビュー
-  データの 1 つのビューの編集
-  バックアップを移動します。


画面の高度なフローをデザインするとき、一般的なユーザー エクスペリエンスをこれらの概念に基づいてできます。

 <a name="platform-specific_attributes" />


#### <a name="platform-specific-attributes"></a>プラットフォーム固有の属性

だけでなくすべてのプラットフォーム上に存在する基本的な要素はアドレス キーのプラットフォームの違い、設計にする必要があります。 これらの相違点を検討してください (および処理専用にコードを記述) する必要があります。

-   **画面のサイズ**– 一部のプラットフォーム (iOS、Windows Phone の以前のバージョンなど) が対象とする比較的単純な画面サイズ標準化します。 Android デバイスがあるさまざまな画面サイズは、アプリケーションでサポートするために多くの労力を必要とします。
-   **ナビゲーションのたとえ**– (プラットフォーム間の違い ハードウェア [戻る] ボタンをクリック、パノラマ UI コントロール) およびプラットフォーム (Android 2 と 4、iPhone と iPad) 内で。
-   **キーボード**– 一部の Android デバイスにが物理キーボード他のユーザーのみ、ソフトウェアのキーボード。 ソフト キーボードが画面の部分を隠すことを検出するコードは、これらの違いに小文字を区別する必要があります。
-   **タッチとジェスチャ**– 各オペレーティング システムの旧バージョンでは特に、ジェスチャ認識のオペレーティング システムのサポートは異なります。 Android の以前のバージョンが非常に古いデバイスのサポートと個別のコードが必要があります、つまり、タッチ操作の制限付きサポート
-   **プッシュ通知**– (各プラットフォームでさまざまな機能と実装があります。 Windows 上のタイルをライブします)。


 <a name="Device-specific_features" />


#### <a name="device-specific-features"></a>デバイス固有の機能

決定最小限の機能をアプリケーションに必要な必要があります。または、ときに活用するために各プラットフォームでどのような追加機能を決定します。 コードは、機能を検出し、機能を無効にするにまたは (代替手段を提供する必要があります。 代わりに地理的な場所にすることにより、ユーザーの場所を入力するか、マップからを選択)。

-   **カメラ**– 機能は、デバイス間で異なる: 両方の前面に、背面カメラを他のユーザーが一部のデバイスでカメラを持っていません。 一部のデジタル カメラはビデオ録画できます。
-   **地理的な場所とマップ**– GPS または Wi-fi の場所のサポートはすべてのデバイスに存在しません。 アプリは、各メソッドでサポートされている精度のさまざまなレベルに対応するためにも必要です。
-   **加速度計、ジャイロスコープおよびコンパス**– これらの機能によく見られるだけ選択されている各プラットフォームのデバイス アプリがほとんどの場合、ハードウェアがサポートされていないときに、フォールバックを提供する必要があります。
-   **Twitter および Facebook** iOS5 と iOS6 '組み込み' のみ – それぞれします。 以前のバージョンおよびその他のプラットフォームで、独自の認証機能を提供し、各と直接やり取りする必要があるサービスの API です。
-   **近距離通信 (NFC) を付近**– (一部) でのみ (の執筆時点) での Android 電話します。


 <a name="Dealing_with_Platform_Divergence" />


### <a name="dealing-with-platform-divergence"></a>プラットフォームの相違を処理します。

同じコード ベース、それぞれ長所と短所の独自セットを持つ複数のプラットフォームをサポートする 2 つの方法があります。

-   **プラットフォームの抽象化**– ビジネス ファサード パターンでは、プラットフォーム間で統一されたアクセスを提供し、1 つの統一された API を特定のプラットフォームの実装を抽象化します。
-   **異なる実装**– 特定のプラットフォームの呼び出しは、インターフェイス、継承、条件付きコンパイルなどのアーキテクチャのツールを使用して異なる実装を使用して機能します。


 <a name="Platform_Abstraction" />


## <a name="platform-abstraction"></a>プラットフォームの抽象化

 <a name="Class_Abstraction" />


### <a name="class-abstraction"></a>クラスの抽象化

インターフェイスまたは基本クラスを使用して共有コードで定義し、実装またはプラットフォーム固有のプロジェクトで拡張します。 記述およびクラスの抽象化で共有コードを拡張するは、特にために適していますポータブル クラス ライブラリが使用できるようにするフレームワークの限定されたサブセットがあるし、プラットフォーム固有のコード分岐をサポートするためにコンパイラ ディレクティブを含めることはできません。

 <a name="Interfaces" />


#### <a name="interfaces"></a>インターフェイス

インターフェイスを使用するには、引き続き共通コードを活用するために、共有ライブラリに渡すことができるプラットフォーム固有のクラスを実装することができます。

インターフェイスは、共有コードで定義され、共有ライブラリ パラメーターまたはプロパティとして渡されます。

プラットフォーム固有のアプリケーションでは、インターフェイスを実装でき、それを '処理' 共有コードのも利用することができます。

 **長所**

実装では、プラットフォーム固有のコードを含むでき、でもプラットフォーム固有の外部ライブラリを参照することができます。

 **短所**

作成し、共有コードに実装を渡すことです。 共有コード内で深いインターフェイスを使用する場合、終了されている複数のメソッド パラメーターを通じて渡される、それ以外の場合、呼び出しチェーンを介して押し下げします。 共有コードは、多数の異なるインターフェイスを使用している場合が必要がありますすべて作成して共有コードをどこかに設定します。

 <a name="Inheritance" />


#### <a name="inheritance"></a>継承

共有コードでは、1 つまたは複数のプラットフォーム固有のプロジェクトで拡張することが abstract または virtual のクラスを実装する可能性があります。 これはのようなインターフェイスを使用するのには、既に実装されているいくつかの動作をします。 インターフェイスまたは継承がデザインの方がかどうかに別のビュー ポイントがある。 具体的には c# のみ許可単一継承しているので、が決定できる独自の Api の設計は、今後方法です。 継承を使用して、注意が必要とします。

長所と短所のインターフェイスは、基底クラスがいくつか実装コード (おそらく、プラットフォーム全体に依存しない実装必要に応じて拡張することもできます) を含めることができる追加の利点と、継承に同じように適用します。

<a name="Xamarin.Forms" />

### <a name="xamarinforms"></a>Xamarin.Forms

参照してください、 [Xamarin.Forms](~/xamarin-forms/get-started/index.md)ドキュメント。


### <a name="plug-in-cross-platform-functionality"></a>プラグインのクロス プラットフォームの機能

プラグインを使用して一貫した方法でクロスプラット フォーム アプリを拡張することもできます。

リンク、[プラグイン github](https://github.com/xamarin/plugins)、ほとんどのプラグインはオープン ソースできます (通常インストール可能な Nuget 経由で) プロジェクトが、さまざまな設定、バッテリの状態から、プラットフォーム固有の機能を実装します。Xamarin プラットフォームと Xamarin.Forms のアプリでライブラリを利用しやすい一般的な API。


<a name="Other_Cross-Platform_Libraries" />

### <a name="other-cross-platform-libraries"></a>その他のクロスプラット フォーム ライブラリ

クロス プラットフォームの機能を提供する使用可能なのサード パーティ製ライブラリの数があります。

-   **MvvmCross** -  [https://github.com/slodge/MvvmCross/](https://github.com/slodge/MvvmCross/)
-   **専門用語**(ローカライズ用) -  [https://github.com/rdio/vernacular/](https://github.com/rdio/vernacular/)
-   **MonoGame** (XNA ゲーム) - 用  [http://monogame.codeplex.com/](http://monogame.codeplex.com/)
-   **NGraphics** - [NGraphics](https://github.com/praeclarum/NGraphics)し、その前身 [https://github.com/praeclarum/CrossGraphics](https://github.com/praeclarum/CrossGraphics)


 <a name="Divergent_Implementation" />


### <a name="divergent-implementation"></a>分岐の実装

 <a name="Conditional_Compilation" />


#### <a name="conditional-compilation"></a>条件付きコンパイル

状況によっては、共有コードは、可能性のあるクラスまたは動作が異なる機能にアクセスする各プラットフォームの異なる方法で作業する必要があります。 条件付きコンパイルは、共有アセット プロジェクトでは、別のシンボルが定義されている複数のプロジェクトで、同じソース ファイルを参照されている場合に適しています。

Xamarin プロジェクトが常に定義`__MOBILE__`iOS と Android アプリケーション プロジェクトの両方 (、二重アンダー スコア前およびこれらのシンボルの後の修正プログラムに注意してください) の場合は true であります。

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

Xamarin.Android アプリケーションにコンパイルする必要がありますのみコードを次に使用できます。

```csharp
#if __ANDROID__
// Android-specific code
#endif
```

新しい API が対象とする場合は、機能を追加するため、このようなコードを使用できますが、各 API バージョンも新しいコンパイラ ディレクティブを定義します。 各 API レベルには、レベル '低い' のすべてのシンボルが含まれています。 この機能は複数のプラットフォームをサポートするため便利できません。通常、`__ANDROID__`シンボルのでは十分です。

```csharp
#if __ANDROID_11__
// code that should only run on Android 3.0 Honeycomb or newer
#endif
```

##### <a name="mac"></a>Mac

現在の Xamarin.Mac、組み込みシンボルはありませんが、Mac で独自のアプリ プロジェクトを追加することができます**オプション > ビルド > コンパイラ**で、**シンボルを定義する**ボックス、または編集、 **.csproj**ファイルし、追加があります (たとえば`__MAC__`)

```xml
<PropertyGroup><DefineConstants>__MAC__;$(DefineConstants)</DefineConstants></PropertyGroup>
```

<a name="Windows_Phone" />

##### <a name="windows-phone"></a>Windows Phone

Windows Phone アプリ – 2 つのシンボルを定義する`WINDOWS_PHONE`と`SILVERLIGHT`– コードをプラットフォームを対象に使用できます。 これらはありません、アンダー スコア囲んで Xamarin プラットフォーム シンボルのような操作を行います。


<a name="Using_Conditional_Compilation" />

##### <a name="using-conditional-compilation"></a>条件付きコンパイルを使用します。

条件付きコンパイルの単純なケース スタディの例では、SQLite データベース ファイルのファイルの場所を設定します。 3 つのプラットフォームでは、ファイルの場所を指定するための若干異なる要件があります。

-   **iOS** – Apple が非ユーザーのデータ (ライブラリ ディレクトリ) の特定の場所に配置することが推奨が、このディレクトリの定数はシステムではありません。 正しいパスを構築するには、プラットフォーム固有のコードが必要です。
-   **Android** – によって返されるシステム パス`Environment.SpecialFolder.Personal`許容可能なデータベース ファイルを保存する場所です。
-   **Windows Phone** – 分離ストレージのメカニズムでは、完全なパスを指定することはできません、相対パスとファイル名だけです。
-   **ユニバーサル Windows プラットフォーム**– 使用`Windows.Storage`Api です。

次のコードでは、条件付きコンパイルを使用して、`DatabaseFilePath`プラットフォームごとに適切な。

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

結果は、構築および各プラットフォームで別の場所に SQLite データベース ファイルを配置するすべてのプラットフォームで使用できるクラスです。

