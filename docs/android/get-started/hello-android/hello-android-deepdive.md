---
title: Hello, Android:詳しく調べる
description: このガイドは 2 つに分かれています。最初に、Xamarin.Android アプリケーションを作成し、Xamarin を使用する Android アプリケーション開発の基礎について理解を深めます。 その過程で、Xamarin.Android アプリケーションの作成と展開に必要なツール、概念、および手順を紹介します。
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: EF0E110B-20EA-43F6-9476-1A0F41AFD298
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 10/05/2018
ms.openlocfilehash: 10a46c916654f8421dc5a9af93de3abbbae5e934
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/13/2020
ms.locfileid: "79303547"
---
# <a name="hello-android-deep-dive"></a>Hello, Android:詳しく調べる

_このガイドは 2 つに分かれています。最初に、Xamarin.Android アプリケーションを作成し、Xamarin を使用する Android アプリケーション開発の基礎について理解を深めます。その過程で、Xamarin.Android アプリケーションの作成と展開に必要なツール、概念、および手順を紹介します。_

「[Hello, Android Quickstart](~/android/get-started/hello-android-multiscreen/hello-android-multiscreen-quickstart.md)」 (Hello, Android クイック スタート) では、最初の Xamarin.Android アプリケーションをビルドして実行しました。 次は、より複雑なプログラムをビルドできるように、Android アプリケーションのしくみの理解を深めます。 このガイドでは、実行した作業の内容を理解し、Android アプリケーションの開発の基礎について詳しく理解できるように、Hello, Android で実行したステップを確認します。

このガイドでは、次のトピックについて説明します。

::: zone pivot="windows"

- **Visual Studio の概要** &ndash; Visual Studio の概要と新しい Xamarin.Android アプリケーションの作成。

- **Xamarin.Android アプリケーションの構造** Xamarin.Android アプリケーションの重要なパーツの説明。

- **アプリケーションの基本とアーキテクチャの基礎** &ndash; アクティビティ、Android マニフェスト、および Android の開発の一般的なフレーバーの概要。

- **ユーザー インターフェイス (UI)** &ndash; Android Designer を使用したユーザー インターフェイスの作成。

- **アクティビティとアクティビティのライフ サイクル** &ndash; アクティビティのライフ サイクルの概要と、コード内でのユーザー インターフェイスの作成。

- **テスト、展開、しあげ** &ndash; テスト、展開、アートワークの生成などに関するアドバイスを利用したアプリケーションの完成。

::: zone-end
::: zone pivot="macos"

- **Visual Studio for Mac の概要** &ndash; Visual Studio for Mac の概要と新しい Xamarin.Android アプリケーションの作成。

- **Xamarin.Android アプリケーションの構造** &ndash; Xamarin.Android アプリケーションの重要なパーツの説明。

- **アプリケーションの基本とアーキテクチャの基礎** &ndash; アクティビティ、Android マニフェスト、および Android の開発の一般的なフレーバーの概要。

- **ユーザー インターフェイス (UI)** &ndash; Android Designer を使用したユーザー インターフェイスの作成。

- **アクティビティとアクティビティのライフ サイクル** &ndash; アクティビティのライフ サイクルの概要と、コード内でのユーザー インターフェイスの作成。

- **テスト、展開、しあげ** &ndash; テスト、展開、アートワークの生成などに関するアドバイスを利用したアプリケーションの完成。

::: zone-end

このガイドでは、1 つの画面の Android アプリケーションのビルドに必要なスキルと知識を開発できます。 作業した後、Xamarin.Android アプリケーションのさまざまな部分とそれらをまとめる方法を理解できます。

::: zone pivot="windows"

## <a name="introduction-to-visual-studio"></a>Visual Studio の概要

Visual Studio は Microsoft 製の強力な IDE です。 ビジュアル デザイナー、リファクタリング ツール付きのテキスト エディター、アセンブリ ブラウザー、ソース コード統合などの機能が完全に統合されています。 このガイドでは、Xamarin プラグインと基本的な Visual Studio 機能の使用方法について学習します。

Visual Studio は、コードを _ソリューション_ と _プロジェクト_ に分けて整理しています。 ソリューションとは、1 つまたは複数のプロジェクトを保持できるコンテナーです。 プロジェクトは、アプリケーション (iOS、Android など)、サポートするライブラリ、テスト アプリケーションなどの場合があります。 **Phoneword** アプリで、**Android アプリケーション** テンプレートを使用して、[Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md) ガイドで作成した **Phoneword** ソリューションに新しい Android プロジェクトを追加しました。

::: zone-end
::: zone pivot="macos"

## <a name="introduction-to-visual-studio-for-mac"></a>Visual Studio for Mac の概要

Visual Studio for Mac は、Visual Studio と類似した無料のオープン ソース IDE です。 ビジュアル デザイナー、リファクタリング ツール付きのテキスト エディター、アセンブリ ブラウザー、ソース コード統合などの機能が完全に統合されています。 このガイドでは、基本的な Visual Studio for Mac 機能の使用方法について学習します。 Visual Studio for Mac を初めて使用する場合、[Visual Studio for Mac の概要](https://docs.microsoft.com/visualstudio/mac/)について詳しい情報を確認したい場合があります。

Visual Studio for Mac は、コードを _ソリューション_ と _プロジェクト_ に分けて整理するという Visual Studio の方法に従っています。 ソリューションとは、1 つまたは複数のプロジェクトを保持できるコンテナーです。 プロジェクトは、アプリケーション (iOS、Android など)、サポートするライブラリ、テスト アプリケーションなどの場合があります。 **Phoneword** アプリで、**Android アプリケーション** テンプレートを使用して、[Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md) ガイドで作成した **Phoneword** ソリューションに新しい Android プロジェクトを追加しました。

::: zone-end

<a name="anatomy" />

## <a name="anatomy-of-a-xamarinandroid-application"></a>Xamarin.Android アプリケーションの構造

::: zone pivot="windows"

次のスクリーン ショットは、ソリューションの内容を一覧表示します。 これは、ディレクトリ構造とソリューションに関連付けられているすべてのファイルを含むソリューション エクスプローラーです。

[![ソリューション エクスプローラー](hello-android-deepdive-images/vs/02-solution-structure-sml.png)](hello-android-deepdive-images/vs/02-solution-structure.png#lightbox)

::: zone-end
::: zone pivot="macos"

次のスクリーン ショットは、ソリューションの内容を一覧表示します。 これは、ディレクトリ構造とソリューションに関連付けられているすべてのファイルを含む Solution Pad です。

[![Solution Pad](hello-android-deepdive-images/xs/02-solution-structure-sml.png)](hello-android-deepdive-images/xs/02-solution-structure.png#lightbox)

::: zone-end

**Phoneword** と呼ばれるソリューションが作成され、Android プロジェクト **Phoneword** がその内部に配置されました。

プロジェクト内の各アイテムを見て、各フォルダーとその目的を確認します。

- **Properties** &ndash; 名前、バージョン番号、アクセス許可を含む Xamarin.Android アプリケーションのすべての要件を記述する [AndroidManifest.xml](~/android/platform/android-manifest.md) ファイルを含んでいます。 **Properties** フォルダーには、.NET アセンブリ メタデータ ファイルである [AssemblyInfo.cs](xref:Microsoft.VisualBasic.ApplicationServices.AssemblyInfo) も含まれています。 このファイルには、アプリケーションに関する基本的な情報を入力しておくことをお勧めします。

- **References** &ndash; アプリケーションのビルドと実行に必要なアセンブリが含まれています。 References ディレクトリを展開すると、[System](xref:System)、System.Core、[System.Xml](xref:System.Xml) などの .NET アセンブリ、および Xamarin の Mono.Android アセンブリへの参照が表示されます。

- **Assets** &ndash; フォント、ローカル データ ファイル、テキスト ファイルなどのアプリケーションで実行する必要があるファイルが含まれています。 ここに含まれるファイルは、生成された `Assets` クラスを介してアクセスできます。 Android の資産の詳細については、Xamarin の「[Using Android Assets](~/android/app-fundamentals/resources-in-android/android-assets.md)」 (Android 資産の使用) ガイドを参照してください。

- **Resources** &ndash;文字列、イメージ、およびレイアウトなどのアプリケーション リソースが含まれています。 生成された `Resource` クラスを介してコード内でこれらのリソースにアクセスすることができます。 [Android リソース](~/android/app-fundamentals/resources-in-android/index.md) ガイドには、**Resources** ディレクトリに関する詳細が記載されています。 アプリケーション テンプレートには、**AboutResources.txt** ファイル内のリソースに簡潔なガイドも含まれています。

### <a name="resources"></a>リソース

**Resources** ディレクトリには、**drawable**、**layout**、**mipmap**、**values** という 4 つのフォルダーと **Resource.designer.cs** というファイルも含まれています。

下の表に項目をまとめます。

- **drawable** &ndash; drawable ディレクトリには、画像やビットマップなどの[ドローアブル リソース](https://developer.android.com/guide/topics/resources/drawable-resource.html)が含まれています。

- **mipmap** &ndash; mipmap ディレクトリには、密度が異なる起動アイコンのドローアブル ファイルが含まれています。 既定のテンプレートでは、drawable ディレクトリにはアプリケーション アイコン ファイル **Icon.png** が含まれています。

::: zone pivot="windows"

- **layout** &ndash; layout ディレクトリには、各画面またはアクティビティのユーザーインターフェイスを定義する _Android Designer ファイル_ (.axml) が含まれています。 テンプレートは、**activity_main.axml** という名前に既定値のレイアウトを作成します。

::: zone-end
::: zone pivot="macos"

- **layout** &ndash; layout ディレクトリには、各画面またはアクティビティのユーザーインターフェイスを定義する _Android Designer ファイル_ (.axml) が含まれています。 テンプレートは、**Main.axml** という名前に既定値のレイアウトを作成します。

::: zone-end

- **values** &ndash; このディレクトリには、文字列、整数、色などの単純な値を格納する XML ファイルを格納します。 テンプレートは、**Strings.xml** という文字列値を格納するファイルを作成します。

- **Resource.designer.cs** &ndash; `Resource` クラスとも呼ばれるこのファイルは、各リソースに割り当てられている一意の ID を保持する部分クラスです。 Xamarin.Android ツールによって自動的に作成され、必要に応じて再生成されます。 Xamarin.Android が手動で加えられた変更をすべて上書きするので、このファイルを手動で編集しないでください。

## <a name="app-fundamentals-and-architecture-basics"></a>アプリケーションの基本とアーキテクチャの基礎

Android アプリケーションには、1 つのエントリ ポイントはありません。つまり、オペレーティング システムがアプリケーションを起動するために呼び出す 1 行のコードがアプリケーション内にありません。 代わりに、Android がいずれかのクラスをインスタンス化するときにアプリケーションが起動し、そのときに Android がアプリケーションのプロセス全体をメモリ内に読み込みます。

Android のこの固有の機能は、複雑なアプリケーションの設計時や Android オペレーティング システムとの相互作用のときに特に便利です。 ただし、これらのオプションは、**Phoneword** アプリケーションのような基本的なシナリオを扱うときに Android を複雑にすることもあります。 このため、Android のアーキテクチャの探索は 2 つに分割されます。 このガイドは、Android アプリの最も一般的なエントリ ポイントを使用するアプリケーションをあげて詳しく説明します (最初の画面)。 「[Hello Android のマルチスクリーン](~/android/get-started/hello-android-multiscreen/index.md)」では、アプリケーションを起動するさまざまな方法を説明することで、Android アーキテクチャのすべての複雑さについて説明します。

### <a name="phoneword-scenario---starting-with-an-activity"></a>Phoneword シナリオ - アクティビティの開始

**Phoneword** アプリケーションをエミュレーターまたはデバイスで最初に開いたときに、オペレーティング システムで最初の *Activity* が作成されます。 Activity は、1 つのアプリケーションの画面に対応する特殊な Android クラスであり、ユーザー インターフェイスの描画と稼働を担当します。 Android は、アプリケーションの最初の Activity を作成するときに、アプリケーション全体を読み込みます。

[![Activity の読み込み](hello-android-deepdive-images/01-activity-load-sml.png)](hello-android-deepdive-images/01-activity-load.png#lightbox)

Android アプリケーションは直線的に進行しないため (いくつかのポイントからアプリケーションを起動できます)、Android には、アプリケーションを構成するクラスとファイルを追跡するユニークな方法があります。 **Phoneword** の例では、アプリケーションを構成するすべてのパーツが、**Android マニフェスト**と呼ばれる特別な XML ファイルに登録されます。 **Android マニフェスト**の役割は、アプリケーションのコンテンツ、プロパティ、およびアクセス許可を追跡し、Android オペレーティング システムにそれらを開示することです。 下の図に示すように、**Phoneword** アプリケーションは、1 つの Activity (画面) と、Android マニフェスト ファイルによって結合されるリソースとヘルパー ファイルのコレクションとして考えることができます。

[![リソース ヘルパー](hello-android-deepdive-images/02-resources-helpers-sml.png)](hello-android-deepdive-images/02-resources-helpers.png#lightbox)

次のいくつかのセクションでは、**Phoneword** アプリケーションのさまざまなパーツ間の関係について詳しく説明します。これにより、上記の図の理解を深めることができます。 最初に、ユーザー インターフェイスについて説明し、Android デザイナーとレイアウト ファイルについて説明します。

## <a name="user-interface"></a>ユーザー インターフェイス

> [!TIP]
> 新しいリリースの Visual Studio では、Android Designer 内で .xml ファイルを開くことができます。
>
> Android Designer では、.axml ファイルと .xml ファイルの両方がサポートされています。

::: zone pivot="windows"

**activity_main.axml** は、アプリケーションの最初の画面に対応するユーザー インターフェイスのレイアウト ファイルです。 .axml は、これが Android デザイナー ファイルであることを示します (AXML は *Android XML* を意味します)。 *Main* という名前は、Android の観点から見ると任意であり、&ndash;レイアウト ファイルには別の名前を付けることもできます。 IDE で **activity_main.axml** を開くと、*Android Designer* と呼ばれる Android レイアウト ファイルのビジュアル エディターが起動します。

[![Android Designer](hello-android-deepdive-images/vs/03-android-designer-sml.png "Android Designer")](hello-android-deepdive-images/vs/03-android-designer.png#lightbox)

**Phoneword** アプリで、**TranslateButton** の ID は、`@+id/TranslateButton` に設定されます。

[![TranslateButton ID の設定](hello-android-deepdive-images/vs/04-translatebutton-sml.png "TranslateButton ID の設定")](hello-android-deepdive-images/vs/04-translatebutton.png#lightbox)

::: zone-end
::: zone pivot="macos"

**Main.axml** は、アプリケーションの最初の画面に対応するユーザー インターフェイスのレイアウト ファイルです。 .axml は、これが Android デザイナー ファイルであることを示します (AXML は *Android XML* を意味します)。 *Main* という名前は、Android の観点から見ると任意であり、&ndash;レイアウト ファイルには別の名前を付けることもできます。 IDE で **Main.axml** を開くと、*Android Designer* と呼ばれる Android レイアウト ファイルのビジュアル エディターが起動します。

[![Android Designer](hello-android-deepdive-images/xs/03-android-designer-sml.png)](hello-android-deepdive-images/xs/03-android-designer.png#lightbox)

**Phoneword** アプリで、**TranslateButton** の ID は、`@+id/TranslateButton` に設定されます。

[![TranslateButton ID の設定](hello-android-deepdive-images/xs/04-translatebutton-sml.png)](hello-android-deepdive-images/xs/04-translatebutton.png#lightbox)

::: zone-end

**TranslateButton** の `id` プロパティを設定すると、Android Designer によって **TranslateButton** コントロールが `Resource` クラスにマッピングされ、`TranslateButton` の*リソース ID* が割り当てられます。 視覚的なコントロールのクラスへのマッピングによって、アプリのコード内で **TranslateButton** や他のコントロールを見つけて使用できるようになります。 これは、コントロールを稼働させるコードを分解するときに、詳しく説明されます。 ここで知っておく必要があるのは、コントロールのコード表現が、`id` プロパティを介してデザイナーでコントロールの視覚的な表現にリンクされるということです。

### <a name="source-view"></a>ソース ビュー

デザイン サーフェイスで定義されているすべてのものは、Xamarin.Android で使用するための XML に変換されます。 Android Designer では、ビジュアル デザイナーから生成された XML を含むソース ビューを提供します。 この XML を表示するには、下のスクリーンショットに示すように、デザイナー ビューの左下のパネルで **[ソース]** パネルに切り替えます。

::: zone pivot="windows"

[![Designer のソース ビュー](hello-android-deepdive-images/vs/05-source-view-sml.png "Designer のソース ビュー")](hello-android-deepdive-images/vs/05-source-view.png#lightbox)

::: zone-end
::: zone pivot="macos"

[![Designer のソース ビュー](hello-android-deepdive-images/xs/05-source-view-sml.png)](hello-android-deepdive-images/xs/05-source-view.png#lightbox)

::: zone-end

この XML ソースコードには、次の 4 つのコントロール要素を含める必要があります。2 つの**TextView**、1 つの**EditText**、1 つの**Button** 要素です。 Android Designer の詳細については、Xamarin Android の「[Designer Overview](~/android/user-interface/android-designer/index.md)」 (Designer の概要) ガイドを参照してください。

ユーザー インターフェイスの視覚的な部分の背後にあるツールと概念について説明しました。 次に、アクティビティとアクティビティのライフサイクルを調べることで、ユーザー インターフェイスを稼働させるコードについて見てみましょう。

## <a name="activities-and-the-activity-lifecycle"></a>アクティビティとアクティビティのライフサイクル

`Activity` クラスには、ユーザー インターフェイスを稼働させるコードが含まれています。
Activity は、ユーザーとの対話に応答して動的なユーザー エクスペリエンスを作成します。
ここでは、`Activity` クラス、アクティビティのライフ サイクルについて説明し、**Phoneword** アプリケーションでユーザー インターフェイスを稼働させるコードを詳しく説明します。

### <a name="activity-class"></a>Activity クラス

**Phoneword** アプリケーションには、1 つだけの画面 (Activity) があります。 画面を稼働させるクラスは、`MainActivity` と呼ばれ、**MainActivity.cs** ファイルに格納されます。 `MainActivity` という名前は、Android では特別な意味を持ちません &ndash; ただし、規則では、アプリケーションの最初のアクティビティの名前が `MainActivity` ですが、Android では、それが別の名前でもかまいません。

**MainActivity.cs** を開くと、`MainActivity` クラスは、`Activity` クラスの*サブクラス*であること、および Activity に [Activity](xref:Android.App.ActivityAttribute) 属性が指定されていることがわかります。

```csharp
[Activity (Label = "Phone Word", MainLauncher = true)]
public class MainActivity : Activity
{
  ...
}
```

`Activity` 属性は、Activity を Android マニフェストに登録します。これにより、このクラスは、このマニフェストによって管理されている **Phoneword** アプリケーションの一部であることが Android に知らされます。 `Label` プロパティは、画面上部に表示されるテキストを設定します。

`MainLauncher` プロパティは、アプリケーションが起動するときに、この Activity を表示するように Android に通知します。 「[Hello Android のマルチスクリーン](~/android/get-started/hello-android-multiscreen/index.md)」のガイドで説明されているように、アプリケーションにアクティビティ (画面) を追加するときに、このプロパティが重要になります。

`MainActivity` の基本について説明したので、次に、_アクティビティのライフサイクル_ を紹介することでアクティビティのコードについて詳しく説明します。

### <a name="activity-lifecycle"></a>アクティビティのライフサイクル

Android では、アクティビティは、ユーザーとの対話に応じて、ライフ サイクルのさまざまな段階を通過します。 アクティビティの作成、開始、一時停止、再開、破棄などを行うことができます。 `Activity` クラスには、画面のライフサイクルの特定の時点でシステムが呼び出すメソッドが含まれています。 次の図は、アクティビティの一般的な有効期間だけでなく、対応するライフサイクル メソッドの一部を示しています。

[![アクティビティのライフサイクル](hello-android-deepdive-images/04-lifecycle-sml.png)](hello-android-deepdive-images/04-lifecycle.png#lightbox)

`Activity` ライフサイクル メソッドをオーバーライドすることで、アクティビティを読み込む方法、ユーザーに反応する方法、さらにデバイスの画面に表示されなくなった後の動作まで制御できます。 たとえば、上の図でライフサイクル メソッドをオーバーライドして、いくつかの重要なタスクを実行することができます。

- **OnCreate** &ndash; ビューを作成し、変数を初期化し、ユーザーがアクティビティを表示する前に行う必要があるその他の準備作業を実行します。 このメソッドは、アクティビティがメモリに読み込まれるときに 1 回だけ呼び出されます。

- **OnResume** &ndash; アクティビティがデバイスの画面に戻るたびに発生する必要があるタスクを実行します。

- **OnPause** &ndash; アクティビティがデバイスの画面から消えるたびに発生する必要があるタスクを実行します。

`Activity` でライフサイクル メソッドにカスタム コードを追加する場合、そのライフサイクル メソッドの*基本の実装*を*オーバーライド*します。 既存のライフサイクルメソッド (既にいくつかのコードがアタッチされています) にアクセスし、独自のコードでそのメソッドを拡張します。 メソッドの内部から基本の実装を呼び出し、新しいコードの前に元のコードが実行されるようにします。 次のセクションでこの例を示します。

アクティビティのライフサイクルは、Android の重要かつ複雑な部分です。 _作業の開始_ シリーズが終了した後にアクティビティの詳細について学習したい場合は、「[アクティビティのライフ サイクル](~/android/app-fundamentals/activity-lifecycle/index.md)」ガイドを参照してください。 このガイドでは、アクティビティのライフサイクルの最初の段階である `OnCreate` を中心に説明しています。

### <a name="oncreate"></a>OnCreate メソッド

Android は、アクティビティを作成するとき (画面がユーザーに表示される前) に `Activity` の `OnCreate` メソッドを呼び出します。 `OnCreate` ライフサイクル メソッドをオーバーライドして、ビューを作成し、アクティビティをユーザーに合わせて準備することができます。

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    // Set our view from the "main" layout resource
    SetContentView (Resource.Layout.Main);
    // Additional setup code will go here
}
```

::: zone pivot="windows"

**Phoneword** アプリで、`OnCreate` で最初に実行する操作は、Android Designer で作成されたユーザー インターフェイスを読み込むことです。 UI を読み込むには、`SetContentView` を呼び出し、レイアウト ファイル (**activity_main.axml**) の*リソース レイアウト名*を渡します。 レイアウトは `Resource.Layout.activity_main` に置かれています。

```csharp
SetContentView (Resource.Layout.activity_main);
```

`MainActivity` は、起動すると、**activity_main.axml** ファイルのコンテンツに基づいてビューを作成します。

::: zone-end
::: zone pivot="macos"

**Phoneword** アプリで、`OnCreate` で最初に実行する操作は、Android Designer で作成されたユーザー インターフェイスを読み込むことです。 UI を読み込むには、`SetContentView` を呼び出し、次のレイアウト ファイルの "*リソース レイアウト名*" を渡します: **Main.axml** レイアウトは `Resource.Layout.Main` に置かれています。

```csharp
SetContentView (Resource.Layout.Main);
```

`MainActivity` は、起動すると、**Main.axml** ファイルのコンテンツに基づいてビューを作成します。 レイアウト ファイル名は、アクティビティ名に一致することに注意してください &ndash; *Main*.axml は、*Main* アクティビティのレイアウトです。 Android の観点から見るとこれは必須ではありませんが、他の画面をアプリケーションに追加し始めると、この名前付け規則のおかげで、レイアウト ファイルにコード ファイルを簡単に一致させることができることがわかります。

::: zone-end

レイアウト ファイルを準備した後で、コントロールの検索を開始することができます。
コントロールを検索するには、`FindViewById` を呼び出してコントロールのリソース ID を渡します。

```csharp
EditText phoneNumberText = FindViewById<EditText>(Resource.Id.PhoneNumberText);
Button translateButton = FindViewById<Button>(Resource.Id.TranslateButton);
TextView translatedPhoneWord = FindViewById<TextView>(Resource.Id.TranslatedPhoneWord);
```

これでレイアウト ファイルにコントロールへの参照があるので、ユーザーの操作に応答するようにプログラミングを開始できます。

### <a name="responding-to-user-interaction"></a>ユーザー操作に対する応答

Android では、`Click` イベントが、ユーザーの操作をリッスンします。 このアプリでは、`Click` イベントがラムダで処理されますが、デリゲートまたは名前付きイベント ハンドラーを代わりに使用することができます。 最終的な **TranslateButton** ボタンのコードは次のようになります。

```csharp
translateButton.Click += (sender, e) =>
{
    // Translate user's alphanumeric phone number to numeric
    translatedNumber = PhonewordTranslator.ToNumber(phoneNumberText.Text);
    if (string.IsNullOrWhiteSpace(translatedNumber))
    {
        translatedPhoneWord.Text = string.Empty;
    }
    else
    {
        translatedPhoneWord.Text = translatedNumber;
    }
};
```

## <a name="testing-deployment-and-finishing-touches"></a>テスト、展開、および最後の仕上げ

Visual Studio for Mac と Visual Studio のいずれも、アプリケーションをテストおよび展開するためのオプションを多数用意しています。 このセクションでは、デバッグ オプションについて説明し、デバイスでのアプリケーションのテストのデモンストレーションを示し、さまざまな画面密度に対応するカスタム アプリ アイコンを作成するためのツールを紹介します。

### <a name="debugging-tools"></a>デバッグ ツール

アプリケーション コード内の問題の診断は困難なことがあります。 複雑なコードの問題の診断に役立てるために、[ブレークポイントを設定する](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/set_a_breakpoint)、[コードのステップを実行する](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/step_through_code)、または[ログ ウィンドウに情報を出力する](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/output_information_to_log_window)ことができます。

### <a name="deploy-to-a-device"></a>デバイスに展開する

エミュレーターは、アプリケーションの展開およびテストの有効な出発点ですが、ユーザーは、最終的なアプリをエミュレーターで使用できません。 早い段階で頻繁に実際のデバイスでアプリケーションをテストすることをお勧めします。

Android デバイスを使用してアプリケーションをテストできるようにする前に、開発用に構成する必要があります。 「[Set Up Device for Development](~/android/get-started/installation/set-up-device-for-development.md)」(開発用デバイスの設定) ガイドでは、開発用デバイスを準備する詳細な手順を説明しています。

::: zone pivot="windows"

デバイスを構成した後で、デバイスをプラグインし、 **[デバイスの選択]** ダイアログからデバイスを選択し、アプリケーションを開始することでデバイスを展開できます。

![デバッグ デバイスの選択](hello-android-deepdive-images/vs/06-select-device.png "デバッグ デバイスの選択")

::: zone-end
::: zone pivot="macos"

デバイスを構成した後で、デバイスをプラグインし、 **[開始 (再生)]** を押してから、 **[デバイスの選択]** ダイアログからデバイスを選択し、 **[OK]** を押してデバイスを展開できます。

[![デバッグ デバイスの選択](hello-android-deepdive-images/xs/06-select-device-sml.png)](hello-android-deepdive-images/xs/06-select-device.png#lightbox)

::: zone-end

これにより、デバイス上のアプリケーションを起動します。

[![Phoneword の入力](hello-android-deepdive-images/05-enter-phoneword-sml.png)](hello-android-deepdive-images/05-enter-phoneword.png#lightbox)

### <a name="set-icons-for-different-screen-densities"></a>さまざまな画面密度のアイコンを設定する

Android デバイスにはさまざまな画面サイズと解像度があり、すべての画像がすべての画面に適切に表示されるとは限りません。 たとえば、高密度の Nexus 5 上の低密度アイコンのスクリーン ショットを次に示します。 周囲のアイコンと比較してどれだけぼやけているか見てください。

[![ぼやけたアイコン](hello-android-deepdive-images/06-blurry-icon-sml.png)](hello-android-deepdive-images/06-blurry-icon.png#lightbox)

これに対応するには、さまざまな種類の解像度のアイコンを **Resources** フォルダーに追加することをお勧めします。 Android では、異なる密度の起動アイコンを処理するためのさまざまなバージョンの **mipmap** フォルダーが用意されています。これには、中密度の画面用の *mdpi*、高密度用の *hdpi*、非常に高密度用の *xhdpi*、*xxhdpi*、*xxxhdpi* が含まれます。 さまざまなサイズのアイコンが、適切な **mipmap-** フォルダーに格納されます。

::: zone pivot="windows"

![mipmap フォルダー](hello-android-deepdive-images/vs/07-mipmap-folders.png "mipmap フォルダー")

::: zone-end
::: zone pivot="windows"

[![mipmap フォルダー](hello-android-deepdive-images/xs/07-mipmap-folders-sml.png)](hello-android-deepdive-images/xs/07-mipmap-folders.png#lightbox)

::: zone-end

Android では、適切な密度のアイコンを取得します。

[![適切な密度のアイコン](hello-android-deepdive-images/07-appropriate-density-sml.png)](hello-android-deepdive-images/07-appropriate-density.png#lightbox)

### <a name="generate-custom-icons"></a>カスタム アイコンを生成する

誰もが、デザイナーを使用してカスタム アイコンを作成し、アプリで目立つようにする必要がある画像を起動できるわけではありません。カスタム アプリのアートワークを生成するためのいくつかの代替の方法を次に示します。

::: zone pivot="windows"

- [Android Asset Studio](https://romannurik.github.io/AndroidAssetStudio/index.html) &ndash; すべての種類の Android のアイコン用の Web ベースのブラウザー内ジェネレーターで、他の役に立つコミュニティ ツールへのリンクも提供しています。 Google Chrome で最も適切に動作します。

- Visual Studio &ndash; これを使用して、アプリ用の単純なアイコン セットを IDE で直接作成できます。

- [Fiverr](https://www.fiverr.com/) &ndash; 5 ドルから利用でき、さまざまなデザイナーから選択してアイコンのセットを作成できます。 見つかる場合も見つからない場合もありますが、アイコンをすぐにデザインする必要がある場合は有効なリソースです。

::: zone-end
::: zone pivot="macos"

- [Android Asset Studio](https://romannurik.github.io/AndroidAssetStudio/index.html) &ndash; すべての種類の Android のアイコン用の Web ベースのブラウザー内ジェネレーターで、他の役に立つコミュニティ ツールへのリンクも提供しています。 Google Chrome で最も適切に動作します。

- [Pixelmator](https://www.pixelmator.com/) &ndash; 約 30 ドルの Mac 用の多様な画像編集アプリです。

- [Fiverr](https://www.fiverr.com/) &ndash; 5 ドルから利用でき、さまざまなデザイナーから選択してアイコンのセットを作成できます。 見つかる場合も見つからない場合もありますが、アイコンをすぐにデザインする必要がある場合は有効なリソースです。

::: zone-end

アイコンのサイズと要件に関する詳細については、[Android リソース](~/android/app-fundamentals/resources-in-android/index.md) ガイドを参照してください。

::: zone pivot="macos"

### <a name="adding-google-play-services-packages"></a>Google Play 開発者サービス パッケージを追加する

_Google Play Services_ は、アドオン ライブラリのセットであり、これを使用して、Android 開発者が、Google マップ、Google Cloud Messaging、アプリ内の課金サービスなどの Google からの最新の機能を利用することができます。
以前は、Google Play 開発者サービスのすべてのライブラリへのバインドは、Xamarin によって、1 つのパッケージの形式で支給されていました &ndash; Visual Studio for Mac 以降、アプリに含める Google Play 開発者サービス パッケージを選択するための新しいプロジェクト ダイアログを利用できます。

1 つまたは複数の Google Play Service ライブラリを追加するには、プロジェクト ツリーで**パッケージ** ノードを右クリックし、 **[Google Play Services の追加...]** をクリックします。

[![Google Play Service を追加する](hello-android-deepdive-images/xs/08-add-google-play-services-sml.png)](hello-android-deepdive-images/xs/08-add-google-play-services.png#lightbox)

**[Google Play Services の追加]** ダイアログが表示されているときに、プロジェクトに追加するパッケージ (nugets) を選択します。

[![パッケージの選択](hello-android-deepdive-images/xs/09-add-dialog-sml.png)](hello-android-deepdive-images/xs/09-add-dialog.png#lightbox)

サービスを選択して **[パッケージの追加]** をクリックすると、Visual Studio for Mac によって、選択したパッケージ、およびそのために必要な依存 Google Play 開発者サービス パッケージがダウンロードおよびインストールされます。 場合によっては、 **[ライセンスの同意]** ダイアログが表示され、パッケージをインストールする前に **[同意]** をクリックする必要があります。

[![ライセンスの同意](hello-android-deepdive-images/xs/10-license-acceptance-sml.png)](hello-android-deepdive-images/xs/10-license-acceptance.png#lightbox)

::: zone-end

## <a name="summary"></a>まとめ

おめでとうございます! これで、Xamarin.Android アプリケーションのコンポーネント、およびそれを作成するために必要なツールを確実に理解できました。

「_作業の開始_」シリーズの次のチュートリアルでは、複数の画面を処理するようにアプリケーションを拡張し、より高度な Android アーキテクチャと概念を学習します。
