---
title: Xamarin.Forms におけるファイル処理
description: Xamarin.Forms でのファイル処理は .NET Standard ライブラリのコード、または埋め込みリソースを使用することによって実現できます。
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
# <a name="file-handling-in-xamarinforms"></a>Xamarin.Forms におけるファイル処理

_Xamarin.Forms でのファイル処理は .NET Standard ライブラリのコード、または埋め込みリソースを使用することによって実現できます。_

## <a name="overview"></a>概要

Xamarin.Forms コードはマルチプラットフォームで実行されますが、各プラットフォームには独自のファイルシステムがあります。 以前までは、ファイルを読み書きするには、各プラットフォームでネイティブのファイル API を使用することがもっとも簡単に実現する方法でした。
また、埋め込みリソースは、アプリと一緒にデータファイルを配布するより簡単な方法です。 しかし、.NET Standard 2.0 を使えば、 .NET Standard ライブラリでファイルへアクセスするコードを共有することができます。

画像ファイルを処理する方法については、[画像の処理](~/xamarin-forms/user-interface/images.md) ページを参照してください。

<a name="Loading_and_Saving_Files" />

## <a name="saving-and-loading-files"></a>ファイルの保存と読み込み

`System.IO` のクラスは、各プラットフォームのファイルシステムへのアクセスに使用できます。 `File` クラスは、ファイルの作成、削除、およびファイルの読み込みが可能で、`Directory` クラスでは、ディレクトリの作成、削除、および内容の列挙が可能です。 また `Stream` サブクラスを使用して、より多くのファイル操作 (圧縮やファイル内の位置の検索など) の制御を提供できます。

`File.WriteAllText` メソッドを使用して、テキストファイルを書き込むことができます。

```csharp
File.WriteAllText(fileName, text);
```

`File.ReadAllText` メソッドを使用して、テキストファイルを読み込むことができます。

```csharp
string text = File.ReadAllText(fileName);
```

さらに、`File.Exists` メソッドでは、指定したファイルが存在するかどうかを判断します。

```csharp
bool doesExist = File.Exists(fileName);
```

各プラットフォームのファイルパスは、`Environment.GetFolderPath` メソッドに [ `Environment.SpecialFolder` ](xref:System.Environment.SpecialFolder) 列挙体の値を最初の引数として使用することで、.NET Standard ライブラリから取得することができます。これは、`Path.Combine` メソッドを使ってファイル名と組み合わせることができます。

```csharp
string fileName = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "temp.txt");
```

これらの操作は、テキストを保存し読み込むページを含むサンプルアプリで示します。

[![テキストの保存と読み込み](files-images/saveandload-sml.png "アプリでのファイルの保存と読み込み")](files-images/saveandload.png#lightbox "アプリでのファイルの保存と読み込み")

<a name="Loading_Files_Embedded_as_Resources" />

## <a name="loading-files-embedded-as-resources"></a>埋め込みリソースファイルの読み込み

**.NET Standard** アセンブリにファイルを埋め込むには、ファイルを作成または追加し、**ビルド アクション: EmbeddedResource** を指定します。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![埋め込みリソースのビルドアクションの設定](files-images/vs-embeddedresource-sml.png "ビルドアクション EmbeddedResource を設定")](files-images/vs-embeddedresource.png#lightbox "ビルドアクション EmbeddedResource を設定")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![テキストファイルをPCLに埋め込み、埋め込みリソースのビルドアクションを構成する](files-images/xs-embeddedresource-sml.png "ビルドアクション EmbeddedResource を設定")](files-images/xs-embeddedresource.png#lightbox "ビルドアクション EmbeddedResource を設定")

-----

`GetManifestResourceStream` は、**リソース ID** を使って埋め込みファイルへアクセスするために使用します。 既定では、リソース ID は、埋め込まれているプロジェクトの規定の名前空間が前に付いたファイル名です。アセンブリが **WorkingWithFiles** でファイル名が **PCLTextResource.txt** の場合は、リソース ID は、`WorkingWithFiles.PCLTextResource.txt` になります。

```csharp
var assembly = IntrospectionExtensions.GetTypeInfo(typeof(LoadResourceText)).Assembly;
Stream stream = assembly.GetManifestResourceStream("WorkingWithFiles.PCLTextResource.txt");
string text = "";
using (var reader = new System.IO.StreamReader (stream)) {
    text = reader.ReadToEnd ();
}
```

`text` 変数は、テキストの表示やコード内で使用することができます。この [サンプル アプリ](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithFiles/) のスクリーンショットは、`Label` コントロールでのテキスト表示を示しています。

 [![PCLに埋め込まれたテキストファイル](files-images/pcltext-sml.png "PCLに埋め込まれたテキストファイルをアプリに表示")](files-images/pcltext.png#lightbox "PCLに埋め込まれたテキストファイルをアプリに表示")

XML の読み込みとデシリアライズも、同様に単純です。 次のコードは、XML ファイルが、リソースから読み込まれ、デシリアライズされ、表示するために `ListView` にバインドしていることを示しています。 XML ファイルには、`Monkey`オブジェクト (クラスは、サンプルコードで定義されています) 配列が含まれています。

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

 [![PCLに埋め込んだ Xml ファイル を ListView に表示する](files-images/pclxml-sml.png "PCLに埋め込んだ Xml ファイル を ListView に表示する")](files-images/pclxml.png#lightbox "PCLに埋め込んだ Xml ファイル を ListView に表示する")

<a name="Embedding_in_Shared_Projects" />

### <a name="embedding-in-shared-projects"></a>Shared プロジェクトでの埋め込み

Shared プロジェクトにも、埋め込みリソースとしてファイルを含めることができます。しかしながら、Shared プロジェクトの内容は参照元のプロジェクトにコンパイルされるため、埋め込みファイルのリソース ID に使われるプレフィックスが変化する可能性があります。これは、埋め込みファイルのリソース ID がプラットフォームごとに異なる可能性があることを意味します。

Shared プロジェクトでのこの問題に対する解決方法は 2 つあります。

-  **プロジェクトを同期** - 各プラットフォームのプロジェクトプロパティを編集し、**同じ** アセンブリ名と規定の名前空間を使うようにします。この値は、Shared プロジェクトの埋め込みリソース ID のプレフィックスとしてハードコーディングされます。
-  **#if コンパイラ ディレクティブ** - コンパイラ ディレクティブを使用して、正しいリソース ID のプレフィックスを設定し、その値を使用して正しいリソース ID を動的に構築します。


2 つめのオプションに関するコードをいかに示します。 コンパイラ ディレクティブを使用して、ハードコードされたリソースのプレフィックス (これは、通常、参照元のプロジェクトの既定の名前空間と同じ) を選択します。 `resourcePrefix` 変数は、埋め込みリソース ファイル名を連結して有効なリソース ID を作成するために使用されます。

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

上記の例では、ファイルは .NET Standard ライブラリ プロジェクトのルートに埋め込まれることが想定されており、その場合、リソース ID は、`WorkingWithFiles.PCLTextResource.txt` や`WorkingWithFiles.iOS.SharedTextResource.txt` といった **Namespace.Filename.Extension** の形になります。

埋め込みリソースはフォルダーに整理することが可能です。埋め込みリソースがフォルダー内に配置されている場合、フォルダー名がリソース ID の一部になり（ピリオドで区切られます）、リソース ID のフォーマットは、**Namespace.Folder.Filename.Extension** になります。サンプルアプリで使用されるファイルを **MyFolder** というフォルダーに配置すると、対応するリソース ID は、`WorkingWithFiles.MyFolder.PCLTextResource.txt` や `WorkingWithFiles.iOS.MyFolder.SharedTextResource.txt` になります。

<a name="Debugging_Embedded_Resources" />

### <a name="debugging-embedded-resources"></a>埋め込みリソースのデバッグ

特定のリソースが読み込まれない原因を理解しにくい場合があるため、次のデバッグコードを、リソースが正しく構成されていることを確認するために、アプリケーションに一時的に追加できます。 これは、リソースの読み込みの問題をデバッグするために、**エラー** パッドに与えられたアセンブリに埋め込まれた全てのリソースを出力します。

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

この記事では、デバイス上のテキストの保存と読み込みと、埋め込みリソースの読み込みのためのいくつかのシンプルなファイル操作を紹介しました。.NET Standard 2.0 を使えば、.NET Standard ライブラリで、ファイルにアクセスするコードを共有することが可能です。

## <a name="related-links"></a>関連リンク

- [FilesSample](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithFiles/)
- [Xamarin.Forms のサンプル](https://github.com/xamarin/xamarin-forms-samples)
- [Xamarin.iOS 内のファイル システムの使用](~/ios/app-fundamentals/file-system.md)
- [ファイルのブック](https://developer.xamarin.com/workbooks/xamarin-forms/application-fundamentals/files/files.workbook)
