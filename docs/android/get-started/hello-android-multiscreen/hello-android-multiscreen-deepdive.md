---
title: Hello Android のマルチスクリーン:詳しく調べる
description: 2 部構成のこのガイドでは、2 つ目の画面を処理するために、(「Hello, Android」ガイドで作成された) 基本的な Phoneword アプリケーションが展開されます。 その過程で、基本的な Android アプリケーションの構成ブロックが紹介されます。 Android アーキテクチャの詳細も含まれます。これは、Android アプリケーションの構造と機能の理解を深めるのに役立ちます。
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: E4150036-7760-4023-BD33-B7BDE7B7AF5B
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 10/05/2018
ms.openlocfilehash: 4acbfe810abefd9a25721ddf59c9f4f197afdf28
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73020976"
---
# <a name="hello-android-multiscreen-deep-dive"></a>Hello Android のマルチスクリーン:詳しく調べる

_2 部構成のこのガイドでは、2 つ目の画面を処理するために、(「Hello, Android」ガイドで作成された) 基本的な Phoneword アプリケーションが展開されます。その過程で、基本的な Android アプリケーションの構成ブロックが紹介されます。Android アーキテクチャの詳細も含まれます。これは、Android アプリケーションの構造と機能の理解を深めるのに役立ちます。_

「[Hello, Android Multiscreen Quickstart](~/android/get-started/hello-android-multiscreen/hello-android-multiscreen-quickstart.md)」 (Hello, Android マルチスクリーン クイック スタート) では、最初のマルチスクリーン Xamarin.Android アプリケーションをビルドして実行しました。

このガイドでは、さらに高度な Android アーキテクチャについて説明します。 *インテント*での Android ナビゲーションについて説明し、Android ハードウェアのナビゲーション オプションを探索します。 あるアプリケーションと、オペレーティング システムおよび他のアプリケーションとの関係をより全体的な視点で見る際に、Phoneword アプリへの新規追加を分析します。

## <a name="android-architecture-basics"></a>Android アーキテクチャの基本

「[Hello, Android Deep Dive](~/android/get-started/hello-android/hello-android-deepdive.md)」 (Hello, Android の詳細) では、Android アプリケーションは単一のエントリ ポイントがないため、固有のプログラムであることを学習しました。 代わりに、オペレーティング システム (または別のアプリケーション) はアプリケーションの登録済みアクティビティのいずれかを開始します。これにより、アプリケーションのプロセスが開始されます。 Android アーキテクチャを詳しく分析し、Android アプリケーションの構成ブロックとその機能について紹介することで、Android アプリケーションのビルド方法の理解を深めます。

### <a name="android-application-building-blocks"></a>Android アプリケーションのビルド ブロック

Android アプリケーションは、イメージ、テーマ、ヘルパー クラスなどのさまざまなアプリ リソース &ndash;(これらは、*Android マニュフェスト* という XML ファイルで調整されます) と共にバンドルされる*アプリケーション ブロック*という特別な Android クラスのコレクションで構成されます。

アプリケーション ブロックは Android アプリケーションのバックボーンを形成します。これらを使用すれば、通常のクラスでは通常、実行できない操作を行うことができます。 最も重要なのは以下の_アクティビティ_と_サービス_の 2 つです。

- **アクティビティ** &ndash; アクティビティはユーザー インターフェイスを持つ画面に対応しており、概念的には Web アプリケーションの Web ページと似ています。 たとえば、ニュースフィード アプリケーションの場合、ログイン画面が最初のアクティビティであり、ニュース項目のスクロール可能なリストがもう 1 つのアクティビティで、各項目の詳細ページが 3 番目になります。 「[Activity Lifecycle](~/android/app-fundamentals/activity-lifecycle/index.md)」 (アクティビティのライフサイクル) ガイドでは、アクティビティについてさらに学習できます。

- **サービス** &ndash; Android サービスでは、実行時間の長いタスクを引き継ぎ、バックグラウンドで実行することで、アクティビティをサポートします。 サービスにはユーザー インターフェイスがありません。サービスは、画面に関連付けられていないタスク (バックグラウンドでの曲の再生やサーバーへの写真のアップロードなど) を処理する場合に使用されます。 サービスの詳細については、[サービスの作成](~/android/app-fundamentals/services/index.md)および[Android サービス](~/android/app-fundamentals/services/index.md)に関するガイドを参照してください。

Android アプリケーションでは一部のタイプのブロックが使用されない場合があり、多くの場合、1 つのタイプにはいくつかのブロックがあります。 たとえば、「[Hello, Android Quickstart](~/android/get-started/hello-android/hello-android-quickstart.md)」 (Hello, Android クイック スタート) の Phoneword アプリケーションが、1 つのアクティビティ (画面) のみと複数のリソース ファイルで構成されているとします。 単純な音楽プレーヤー アプリには、アプリがバックグラウンドにある状態で音楽を再生するために 1 つのサービスと複数のアクティビティが存在する可能性があります。

### <a name="intents"></a>インテント

Android アプリケーションのもう 1 つの基本概念は*インテント*です。
Android は*最小権限の原則*に従って設計されています。アプリケーションは、操作する必要があるブロックにのみアクセスでき、オペレーティング システムや他のアプリケーションを構成するブロックへのアクセスは制限されます。 同様に、ブロックは*疎結合*であり、他のブロックについてほとんど認識せず、また、他のブロック (同じアプリケーションの一部であるブロックであっても) へのアクセスが制限されるように設計されています。

通信を行うために、アプリケーション ブロックは*インテント*という非同期メッセージを送受信します。 インテントには、受信側のブロックと、場合によっては一部のデータに関する情報が含まれます。 1 つのアプリ コンポーネントから送信されたインテントが別のアプリ コンポーネントでのアクションをトリガーし、これにより、2 つのアプリ コンポーネントがバインドされ、通信できるようになります。 インテントを送受信することで、写真を撮影して保存するためのカメラ アプリの起動、場所情報の収集、またはある画面から次の画面への移動などの複雑なアクションをブロックで調整することができます。

### <a name="androidmanifestxml"></a>AndroidManifest.XML

ブロックをアプリケーションに追加する際に、**Android マニュフェスト**という特別な XML ファイルに登録されます。 マニフェストはアプリケーションのすべてのアプリケーション ブロックと、バージョンの要件、アクセス許可、およびリンクされたライブラリ (実行するアプリケーションについて、オペレーティング システムで認識する必要があるすべて) を追跡します。 また、**Android マニフェスト**はアクティビティおよびインテントと連動し、特定のアクティビティに適したアクションを制御します。 Android マニフェストのこれらの高度な機能については、[Android マニュフェストの操作](~/android/platform/android-manifest.md)に関するガイドで説明します。

単一画面の Phoneword アプリケーションでは、アイコンなどの追加リソースと共に、1 つのアクティビティのみと、1 つのインテント、および `AndroidManifest.xml` が使用されました。 複数画面の Phoneword では、アクティビティが追加され、インテントを使用して最初のアクティビティから起動されました。 次のセクションでは、インテントが Android アプリケーションのナビゲーションを作成するのにどう役立つかを説明します。

## <a name="android-navigation"></a>Android のナビゲーション

画面間を移動するのにインテントが使用されました。 ここでは、このコードを詳しく調べ、インテントのしくみを確認し、Android のナビゲーションでの役割を理解します。

### <a name="launching-a-second-activity-with-an-intent"></a>インテントによる 2 番目のアクティビティの起動

Phoneword アプリケーションでは、2 番目の画面 (アクティビティ) を起動するためにインテントが使用されました。 ここでは、まず、インテントを作成し、現在の*コンテキスト* (現在の**コンテキスト**を示す `this`) と、参照するアプリケーション ブロックのタイプ (`TranslationHistoryActivity`) を渡します。

```csharp
Intent intent = new Intent(this, typeof(TranslationHistoryActivity));
```

**コンテキスト**は、アプリケーション環境に関するグローバル情報へのインターフェイスです。新しく作成されたオブジェクトに、アプリケーションの状況を知らせます。 インテントをメッセージと考えた場合、メッセージの受信者の名前 (`TranslationHistoryActivity`) と受信者のアドレス (`Context`) を提供することになります。

Android は、インテントに単純なデータを添付するためのオプションを提供します (複雑なデータは別の方法で処理されます)。 Phoneword の例では、`PutStringArrayExtra` を使用して、インテントに電話番号のリストが添付され、`StartActivity` がインテントの受信者に対して呼び出されます。 完成したコードは次のようになります。

```csharp
translationHistoryButton.Click += (sender, e) =>
{
    var intent = new Intent(this, typeof(TranslationHistoryActivity));
    intent.PutStringArrayListExtra("phone_numbers", _phoneNumbers);
    StartActivity(intent);
};
```

## <a name="additional-concepts-introduced-in-phoneword"></a>Phoneword で導入されているその他の概念

Phoneword アプリケーションでは、このガイドでは説明していない概念がいくつか導入されています。 たとえば、次のような概念です。

**文字列リソース** &ndash; Phoneword アプリケーションでは、`TranslationHistoryButton` のテキストが `"@string/translationHistory"` に設定されました。 `@string` 構文は、文字列の値が_文字列リソース ファイル_の **Strings.xml** に格納されることを意味します。 `translationHistory` 文字列の次の値は、**Strings.xml** に追加されました。

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="translationHistory">Call History</string>
</resources>
```

文字列リソースとその他の Android のリソースの詳細については、[Android のリソース ガイド](~/android/app-fundamentals/resources-in-android/index.md)を参照してください。

**ListView と ArrayAdapter** &ndash; _ListView_ は、行のスクロール リストを表示するための簡単な方法を提供する UI コンポーネントです。 `ListView` インスタンスでは、行ビューに含まれているデータをフィードするための _Adapter_ が必要です。 次のコード行は、`TranslationHistoryActivity` のユーザー インターフェイスの設定に使用されました。

```csharp
this.ListAdapter = new ArrayAdapter<string>(this, Android.Resource.Layout.SimpleListItem1, phoneNumbers);
```

ListView と Adapter はこのドキュメントの範囲外ですが、非常に包括的な [ListView と Adapter](~/android/user-interface/layouts/list-view/index.md) に関するガイドで説明されています。
「[ListView にデータを読み込む](~/android/user-interface/layouts/list-view/populating.md)」では、特に組み込みの `ListActivity` と `ArrayAdapter` クラスを使用して、(Phoneword の例で行ったように) カスタム レイアウトを定義せずに `ListView` を作成して設定する方法について説明します。

## <a name="summary"></a>まとめ

これで、最初のマルチスクリーン Android アプリケーションが完成しました。 このガイドでは *Android アプリケーションの構成ブロック*と*インテント*について紹介し、これらを使用して、マルチスクリーン Android アプリケーションをビルドしました。 独自の Xamarin.Android アプリケーションの開発を開始するために必要な、強固な基盤ができました。

次は、[クロスプラットフォーム アプリケーションのビルドに関するガイド](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)で Xamarin でのクロスプラットフォーム アプリケーションのビルドについて学習します。
