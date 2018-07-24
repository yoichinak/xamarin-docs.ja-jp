---
title: Xamarin.Forms でファイルの処理
description: .NET 標準ライブラリで、または埋め込みリソースを使用してコードを使用して、ファイル Xamarin.Forms を使用した処理を実現できます。
ms.prod: xamarin
ms.assetid: 9987C3F6-5F04-403B-BBB4-ECB024EA6CC8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/21/2018
ms.openlocfilehash: 0be441a7be9777698212e690aca95fdd75e5050f
ms.sourcegitcommit: eac092f84b603958c761df305f015ff84e0fad44
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/21/2018
ms.locfileid: "36310154"
---
# <a name="file-handling-in-xamarinforms"></a>Xamarin.Forms でファイルの処理

_.NET 標準ライブラリで、または埋め込みリソースを使用してコードを使用して、ファイル Xamarin.Forms を使用した処理を実現できます。_

## <a name="overview"></a>概要

Xamarin.Forms コードは複数のプラットフォームで実行されますが、各プラットフォームには独自のファイルシステムがあります。 以前は、ファイルの読み書きが最も簡単には、各プラットフォームでネイティブのファイル Api を使用してが行われたことが目的ことです。 また、埋め込みリソースは、アプリを含むデータ ファイルを配布する簡単なソリューションです。 ただし、.NET 規格 2.0 で .NET 標準ライブラリでファイルのアクセス コードを共有することができます。

イメージ ファイルを処理する方法についてを参照してください、[イメージを操作](~/xamarin-forms/user-interface/images.md)ページ。

<a name="Loading_and_Saving_Files" />

## <a name="saving-and-loading-files"></a>保存して、ファイルを読み込んでいます

`System.IO`クラスは、各プラットフォームでのファイル システムへのアクセスに使用できます。 `File`クラスを使用して、作成、削除、および、ファイルの読み取りおよび`Directory`クラスでは、作成、削除、またはディレクトリの内容を列挙することができます。 使用することも、`Stream`サブクラスより多くのファイル操作 (ファイル内の圧縮や位置を検索) などの制御を提供できます。

使用して、テキスト ファイルを記述することができます、`File.WriteAllText`メソッド。

```csharp
File.WriteAllText(fileName, text);
```

使用して、テキスト ファイルを読み取ることができます、`File.ReadAllText`メソッド。

```csharp
string text = File.ReadAllText(fileName);
```

さらに、`File.Exists`メソッドでは、指定したファイルが存在するかどうかを判断します。

```csharp
bool doesExist = File.Exists(fileName);
```

値を使用して、標準の .NET ライブラリから各プラットフォーム上のファイルのパスを決定できます、 [ `Environment.SpecialFolder` ](xref:System.Environment.SpecialFolder)列挙体に最初の引数として、`Environment.GetFolderPath`メソッドです。 これは、ことができますと組み合わせて、ファイル名に、`Path.Combine`メソッド。

```csharp
string fileName = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "temp.txt");
```

これらの操作の例は、サンプル アプリを保存し、テキストを読み込むページを含むを示します。

[![保存とテキストを読み込み](files-images/saveandload-sml.png "を保存し、アプリでのファイルの読み込み")](files-images/saveandload.png#lightbox "を保存し、アプリでのファイルの読み込み")

<a name="Loading_Files_Embedded_as_Resources" />

## <a name="loading-files-embedded-as-resources"></a>リソースとして埋め込まれているファイルの読み込み

ファイルを埋め込むには、 **.NET 標準**、アセンブリを作成またはファイルを追加し、いることを確認**ビルド アクション: EmbeddedResource**です。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![リソースのビルド アクションが埋め込まれている構成](files-images/vs-embeddedresource-sml.png "設定 EmbeddedResource\"ビルド アクション\"")](files-images/vs-embeddedresource.png#lightbox "設定埋め込まれたリソースのビルド アクション")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![テキスト ファイルがビルド アクションが埋め込まれたリソースを構成するために、PCL に埋め込まれた](files-images/xs-embeddedresource-sml.png "設定 EmbeddedResource\"ビルド アクション\"")](files-images/xs-embeddedresource.png#lightbox "設定埋め込まれたリソースのビルド アクション")

-----

`GetManifestResourceStream` 埋め込みファイルを使用して、アクセスに使用されるその**リソース ID**です。 既定では、リソース ID で埋め込まれているプロジェクトの既定の名前空間の付いたファイル名は、ここでは、アセンブリは**WorkingWithFiles** 、ファイル名は**PCLTextResource.txt**、リソース ID はように`WorkingWithFiles.PCLTextResource.txt`です。

```csharp
var assembly = IntrospectionExtensions.GetTypeInfo(typeof(LoadResourceText)).Assembly;
Stream stream = assembly.GetManifestResourceStream("WorkingWithFiles.PCLTextResource.txt");
string text = "";
using (var reader = new System.IO.StreamReader (stream)) {
    text = reader.ReadToEnd ();
}
```

`text`し、テキストの表示や、それ以外の場合、コード内で使用する変数を使用できます。 このスクリーン ショット、[サンプル アプリ](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithFiles/)で表示されるテキストを表示、`Label`コントロール。

 [![テキスト ファイルは、PCL に埋め込まれている](files-images/pcltext-sml.png "PCL アプリに表示される埋め込みのテキスト ファイル")](files-images/pcltext.png#lightbox "PCL アプリに表示される埋め込みのテキスト ファイル")

読み込みの XML をシリアル化とは、同様に単純です。 次のコードが読み込まれたし、リソースから逆シリアル化にバインドされている XML ファイルを示します、`ListView`表示用です。 XML ファイルには、配列が含まれています。`Monkey`オブジェクト (クラスは、サンプル コードで定義されます)。

```csharp
var assembly = IntrospectionExtensions.GetTypeInfo(typeof(LoadResourceText)).Assembly;
Stream stream = assembly.GetManifestResourceStream("WorkingWithFiles.PCLXmlResource.xml");
List<Monkey> monkeys;
using (var reader = new System.IO.StreamReader (stream)) {
    var serializer = new XmlSerializer(typeof(List<Monkey>));
    monkeys = (List<Monkey>)serializer.Deserialize(reader);
}
var listView = new ListView ();
listView.ItemsSource = monkeys;
```

 [![Xml ファイルは、ListView に表示される、PCL に埋め込まれている](files-images/pclxml-sml.png "PCL がリスト ビューに表示される埋め込み XML ファイル")](files-images/pclxml.png#lightbox "PCL がリスト ビューに表示される埋め込み XML ファイル")

<a name="Embedding_in_Shared_Projects" />

### <a name="embedding-in-shared-projects"></a>共有プロジェクトに埋め込む

共有プロジェクトにはもファイルが含まれて、埋め込みリソースとしてただし、ファイル リソースの Id を変更することができますが使用されるプレフィックスに埋め込まれている共有プロジェクトの内容は参照元のプロジェクトにコンパイルされているためです。 つまり、各埋め込みファイルのリソース ID は、プラットフォームごとに異なる場合があります。

共有プロジェクトでは、この問題に 2 つの解決があります。

-  **プロジェクトを同期**-使用するには、各プラットフォームのプロジェクト プロパティの編集、**同じ**アセンブリの名前および既定の名前空間。 この値は、埋め込まれたリソース、共有プロジェクト内の Id のプレフィックスとして「ハードコーディング」を指定できます。
-  **#if コンパイラ ディレクティブ**-コンパイラ ディレクティブを使用して正しいリソース ID のプレフィックスを設定し、その値を正しいリソース ID を動的に構築するために使用


コードは、2 番目のオプションは、以下に示します。 コンパイラ ディレクティブを使用して、ハードコードされたリソースのプレフィックス (これは、通常、参照元のプロジェクトの既定の名前空間と同じ) を選択します。 `resourcePrefix`変数は、埋め込みリソース ファイル名を連結して有効なリソース ID の作成に使用されます。

```csharp
#if __IOS__
var resourcePrefix = "WorkingWithFiles.iOS.";
#endif
#if __ANDROID__
var resourcePrefix = "WorkingWithFiles.Droid.";
#endif

Debug.WriteLine("Using this resource prefix: " + resourcePrefix);
// note that the prefix includes the trailing period '.' that is required
var assembly = IntrospectionExtensions.GetTypeInfo(typeof(SharedPage)).Assembly;
Stream stream = assembly.GetManifestResourceStream
    (resourcePrefix + "SharedTextResource.txt");
```

<a name="Organizing_Resources" />

### <a name="organizing-resources"></a>リソースの整理

上記の例では、フォームの場合、リソース ID は、標準的な .NET のライブラリ プロジェクトのルートにファイルが埋め込まれていると仮定**Namespace.Filename.Extension**など`WorkingWithFiles.PCLTextResource.txt`と`WorkingWithFiles.iOS.SharedTextResource.txt`です。

フォルダー内の埋め込みリソースを整理することができます。 フォルダーに埋め込みリソースが配置されると、フォルダー名の一部となります (ピリオドで区切られた) リソース ID、リソース ID の形式になるように**Namespace.Folder.Filename.Extension**です。 フォルダーにサンプル アプリで使用されるファイルを配置する**MyFolder**対応するリソース Id 実行するように`WorkingWithFiles.MyFolder.PCLTextResource.txt`と`WorkingWithFiles.iOS.MyFolder.SharedTextResource.txt`です。

<a name="Debugging_Embedded_Resources" />

### <a name="debugging-embedded-resources"></a>埋め込みリソースのデバッグ

特定のリソースを読み込まれていない理由を理解しにくい場合があります、ため、次のデバッグ コードが、リソースが正しく構成されていることを確認するために、アプリケーションを一時的に追加できます。 特定のアセンブリに埋め込まれた認識されているすべてのリソースを出力は、**エラー**リソース読み込みの問題をデバッグする際のパッド。

```csharp
using System.Reflection;
// ...
// use for debugging, not in released app code!
var assembly = IntrospectionExtensions.GetTypeInfo(typeof(SharedPage)).Assembly;
foreach (var res in assembly.GetManifestResourceNames()) {
    System.Diagnostics.Debug.WriteLine("found resource: " + res);
}
```

## <a name="summary"></a>まとめ

この記事には、保存と読み込み、デバイス上のテキストと埋め込みリソースを読み込むための一部の単純なファイル操作が示されています。 .NET 標準 2.0 は、.NET 標準ライブラリでファイルのアクセス コードを共有できます。

## <a name="related-links"></a>関連リンク

- [FilesSample](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithFiles/)
- [Xamarin.Forms のサンプル](https://github.com/xamarin/xamarin-forms-samples)
- [Xamarin.iOS 内のファイル システムの使用](~/ios/app-fundamentals/file-system.md)
- [ファイルのブック](https://developer.xamarin.com/workbooks/xamarin-forms/application-fundamentals/files/files.workbook)
