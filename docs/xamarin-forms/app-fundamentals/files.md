---
title: Xamarin.Forms でのファイルの処理
description: ファイルの処理を Xamarin.Forms では、.NET Standard ライブラリ、または埋め込みリソースを使用してコードを使用して実現できます。
ms.prod: xamarin
ms.assetid: 9987C3F6-5F04-403B-BBB4-ECB024EA6CC8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/21/2018
ms.openlocfilehash: ddffb45b8cd8d47371e4ab57f30a467cea45b27d
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50117773"
---
# <a name="file-handling-in-xamarinforms"></a>Xamarin.Forms でのファイルの処理

_ファイルの処理を Xamarin.Forms では、.NET Standard ライブラリ、または埋め込みリソースを使用してコードを使用して実現できます。_

## <a name="overview"></a>概要

Xamarin.Forms コードは複数のプラットフォームで実行されますが、各プラットフォームには独自のファイルシステムがあります。 以前は、ことファイルの読み書きが最も簡単に実行されたこと各プラットフォームでネイティブ ファイル Api を使用してこれを意味します。 また、埋め込みリソースは、アプリとデータ ファイルを配布する簡単なソリューションです。 ただし、.NET Standard 2.0 と .NET Standard ライブラリでファイルへのアクセス コードを共有することは。

イメージ ファイルの処理方法の詳細についてを参照してください、[イメージを操作](~/xamarin-forms/user-interface/images.md)ページ。

<a name="Loading_and_Saving_Files" />

## <a name="saving-and-loading-files"></a>保存とファイルの読み込み

`System.IO`クラスは、各プラットフォーム上のファイル システムへのアクセスに使用できます。 `File`クラスにより、作成、削除、および、ファイルの読み取りおよび`Directory`クラスでは、作成、削除、またはディレクトリの内容を列挙することができます。 使用することも、`Stream`サブクラスより詳細なファイル操作 (など、ファイル内の圧縮または位置の検索) 経由での制御を提供することができます。

使用してテキスト ファイルを書き込むことが、`File.WriteAllText`メソッド。

```csharp
File.WriteAllText(fileName, text);
```

使用してテキスト ファイルを読み取ることができます、`File.ReadAllText`メソッド。

```csharp
string text = File.ReadAllText(fileName);
```

さらに、`File.Exists`メソッドは、指定したファイルが存在するかどうかを判断します。

```csharp
bool doesExist = File.Exists(fileName);
```

値を使用して .NET Standard ライブラリから各プラットフォーム上のファイルのパスを確認することができます、 [ `Environment.SpecialFolder` ](xref:System.Environment.SpecialFolder)列挙体には、最初の引数として、`Environment.GetFolderPath`メソッド。 これでファイル名で結合できます、`Path.Combine`メソッド。

```csharp
string fileName = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "temp.txt");
```

これらの操作は、ページを含む、保存、およびテキストを読み込み、サンプル アプリで説明されています。

[![保存とテキストを読み込み](files-images/saveandload-sml.png "保存とアプリ内のファイルの読み込み")](files-images/saveandload.png#lightbox "を保存し、アプリでファイルの読み込み")

<a name="Loading_Files_Embedded_as_Resources" />

## <a name="loading-files-embedded-as-resources"></a>埋め込みリソースファイルの読み込み

ファイルを埋め込むには、 **.NET Standard** 、アセンブリを作成またはファイルを追加し、いることを確認**ビルド アクション: EmbeddedResource**します。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![埋め込まれたリソースのビルド アクションを構成する](files-images/vs-embeddedresource-sml.png "設定 EmbeddedResource\"ビルド アクション\"")](files-images/vs-embeddedresource.png#lightbox "EmbeddedResource ビルド アクションの設定")

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![テキスト ファイルが埋め込まれたリソースのビルド アクションを構成する PCL に埋め込まれた](files-images/xs-embeddedresource-sml.png "設定 EmbeddedResource\"ビルド アクション\"")](files-images/xs-embeddedresource.png#lightbox "EmbeddedResource ビルド アクションの設定")

-----

`GetManifestResourceStream` 使用して、埋め込まれたファイルにアクセスするために使用、**リソース ID**します。 既定では、リソース ID に埋め込まれているプロジェクトの既定の名前空間の付いたファイル名は、この場合、アセンブリは**WorkingWithFiles** 、ファイル名は**PCLTextResource.txt**、リソース ID は`WorkingWithFiles.PCLTextResource.txt`します。

```csharp
var assembly = IntrospectionExtensions.GetTypeInfo(typeof(LoadResourceText)).Assembly;
Stream stream = assembly.GetManifestResourceStream("WorkingWithFiles.PCLTextResource.txt");
string text = "";
using (var reader = new System.IO.StreamReader (stream)) {
    text = reader.ReadToEnd ();
}
```

`text`し、テキストの表示や、それ以外の場合のコードで使用する変数を使用できます。 このスクリーン ショット、[サンプル アプリ](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithFiles/)で表示されるテキストを表示、`Label`コントロール。

 [![テキスト ファイルは、PCL に埋め込まれた](files-images/pcltext-sml.png "アプリに表示される PCL の埋め込みのテキスト ファイル")](files-images/pcltext.png#lightbox "アプリに表示される PCL の埋め込みのテキスト ファイル")

XML を逆シリアル化の読み込みとは、実に単純なのです。 次のコードが読み込まれてし、リソースから逆シリアル化にバインドされている XML ファイルを示します、`ListView`表示にします。 XML ファイルには配列が含まれています`Monkey`オブジェクト (サンプル コードでクラスを定義します)。

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

 [![ListView で表示される PCL に Xml ファイルが埋め込まれた](files-images/pclxml-sml.png "ListView で表示される PCL の埋め込みの XML ファイル")](files-images/pclxml.png#lightbox "ListView で表示される PCL の埋め込みの XML ファイル")

<a name="Embedding_in_Shared_Projects" />

### <a name="embedding-in-shared-projects"></a>Shared プロジェクトでの埋め込み

共有プロジェクトが、ファイル リソースの Id を変更することができますが使用されるプレフィックスに埋め込まれて参照元のプロジェクトには、共有プロジェクトの内容がコンパイルされているために、ファイルを埋め込みリソースとしてこともできます。 つまり、埋め込まれた各ファイルのリソース ID は、プラットフォームごとに異なる可能性があります。

共有プロジェクトでこの問題を 2 つのソリューションがあります。

-  **プロジェクトを同期**-を使用するには、各プラットフォームのプロジェクト プロパティの編集、**同じ**アセンブリの名前と既定の名前空間。 この値は、埋め込みリソース、共有プロジェクト内の Id のプレフィックスとして「ハードコーディング」を指定できます。
-  **#if コンパイラ ディレクティブ**-コンパイラ ディレクティブを使用して、正しいリソース ID のプレフィックスを設定およびその値を使用して、正しいリソース ID を動的に構築するには


コードは、2 番目のオプションは、以下に示します。 コンパイラ ディレクティブを使用して、ハードコーディングされたリソースのプレフィックス (これは、参照元のプロジェクトの既定の名前空間と同じでは通常どおり) を選択します。 `resourcePrefix`変数は、埋め込みリソース ファイル名に連結することによって有効なリソース ID の作成に使用します。

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

上記の例では、フォームの場合、リソース ID は、.NET Standard ライブラリ プロジェクトのルートにファイルが埋め込まれていると仮定**Namespace.Filename.Extension**など`WorkingWithFiles.PCLTextResource.txt`と`WorkingWithFiles.iOS.SharedTextResource.txt`します。

フォルダー内の埋め込みリソースを整理することになります。 埋め込みリソースがフォルダーに配置されると、フォルダー名の一部となります (ピリオドで区切られた) リソース ID、リソース ID の形式になるように**Namespace.Folder.Filename.Extension**します。 フォルダーにサンプル アプリで使用されるファイルを配置する**MyFolder** 、対応するリソース Id になります`WorkingWithFiles.MyFolder.PCLTextResource.txt`と`WorkingWithFiles.iOS.MyFolder.SharedTextResource.txt`します。

<a name="Debugging_Embedded_Resources" />

### <a name="debugging-embedded-resources"></a>埋め込みリソースのデバッグ

されていない、特定のリソースを読み込まれている理由を理解しにくい場合があります、ため、次のコードのデバッグが、リソースが正しく構成されていることを確認するためのアプリケーションを一時的に追加できます。 特定のアセンブリに埋め込まれているすべての既知のリソースの出力には、**エラー**リソース読み込みの問題をデバッグする際に埋め込み。

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

この記事では、保存と読み込み、デバイス上のテキストと埋め込みリソースを読み込むためのいくつか単純なファイル操作を説明しました。 .NET Standard 2.0 は、.NET Standard ライブラリでファイルへのアクセス コードを共有できます。

## <a name="related-links"></a>関連リンク

- [FilesSample](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithFiles/)
- [Xamarin.Forms のサンプル](https://github.com/xamarin/xamarin-forms-samples)
- [Xamarin.iOS でのファイル システムの操作](~/ios/app-fundamentals/file-system.md)

