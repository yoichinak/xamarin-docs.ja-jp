---
title: "ファイル"
description: "埋め込みリソースの使用またはネイティブのファイル システム Api に対して書き込み、ファイル Xamarin.Forms を使用した処理を実行できます。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 9987C3F6-5F04-403B-BBB4-ECB024EA6CC8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/22/2017
ms.openlocfilehash: 605374c0f2bfe656e564e48d14ffe18ce5b7dfe5
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/28/2018
---
# <a name="files"></a>ファイル

_埋め込みリソースの使用またはネイティブのファイル システム Api に対して書き込み、ファイル Xamarin.Forms を使用した処理を実行できます。_

## <a name="overview"></a>概要

Xamarin.Forms コードは複数のプラットフォームで実行されますが、各プラットフォームには独自のファイルシステムがあります。 つまり、ファイルの読み書きにほとんどが簡単に完了した各プラットフォームでネイティブのファイル Api を使用します。 また、埋め込みリソースは、アプリを含むデータ ファイルを配布する簡単なソリューションです。

このドキュメントでは、次の一般的なファイル処理シナリオについて説明します。

-  [ **ファイルがリソースとして埋め込む**](#Loading_Files_Embedded_as_Resources) -ファイルは、アプリケーションでロードを使用して、リフレクション API の一部として出荷することができます。
-  [ **保存してファイルを読み込む**](#Loading_and_Saving_Files) -ユーザー書き込み可能なストレージは、ネイティブに実装され、その後アクセスを使用して、`DependencyService`です。


サードパーティ製のコンポーネントの呼び出し**PCLStorage** PCL コードからのユーザー アクセス可能なストレージにファイルを読み書きするも使用できます。

イメージ ファイルを処理する方法についてを参照してください、[イメージを操作](~/xamarin-forms/user-interface/images.md)ページ。

<a name="Loading_Files_Embedded_as_Resources" />

## <a name="loading-files-embedded-as-resources"></a>リソースとして埋め込まれているファイルの読み込み

ファイルを埋め込むには、 **PCL** 、アセンブリを作成またはファイルを追加し、いることを確認**ビルド アクション: EmbeddedResource**です。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[ ![リソースのビルド アクションが埋め込まれている構成](files-images/vs-embeddedresource-sml.png "設定 EmbeddedResource"ビルド アクション"")](files-images/vs-embeddedresource.png "設定埋め込まれたリソースのビルド アクション")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![テキスト ファイルがビルド アクションが埋め込まれたリソースを構成するために、PCL に埋め込まれた](files-images/xs-embeddedresource-sml.png "設定 EmbeddedResource"ビルド アクション"")](files-images/xs-embeddedresource.png "設定埋め込まれたリソースのビルド アクション")

-----

`GetManifestResourceStream` 埋め込みファイルを使用して、アクセスに使用されるその**リソース ID**です。 既定では、リソース ID で埋め込まれているプロジェクトの既定の名前空間の付いたファイル名は、ここでは、アセンブリは**WorkingWithFiles** 、ファイル名は**PCLTextResource.txt**、リソース ID はように`WorkingWithFiles.PCLTextResource.txt`です。

```csharp
var assembly = typeof(LoadResourceText).GetTypeInfo().Assembly;
Stream stream = assembly.GetManifestResourceStream("WorkingWithFiles.PCLTextResource.txt");
string text = "";
using (var reader = new System.IO.StreamReader (stream)) {
    text = reader.ReadToEnd ();
}
```

`text`し、テキストの表示や、それ以外の場合、コード内で使用する変数を使用できます。 このスクリーン ショット、[サンプル アプリ](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithFiles/)で表示されるテキストを表示、`Label`コントロール。

 [ ![テキスト ファイルは、PCL に埋め込まれている](files-images/pcltext-sml.png "PCL アプリに表示される埋め込みのテキスト ファイル")](files-images/pcltext.png "PCL アプリに表示される埋め込みのテキスト ファイル")

読み込みの XML をシリアル化とは、同様に単純です。 次のコードが読み込まれたし、リソースから逆シリアル化にバインドされている XML ファイルを示します、`ListView`表示用です。 XML ファイルには、配列が含まれています。`Monkey`オブジェクト (クラスは、サンプル コードで定義されます)。

```csharp
var assembly = typeof(LoadResourceText).GetTypeInfo().Assembly;
Stream stream = assembly.GetManifestResourceStream("WorkingWithFiles.PCLXmlResource.xml");
List<Monkey> monkeys;
using (var reader = new System.IO.StreamReader (stream)) {
    var serializer = new XmlSerializer(typeof(List<Monkey>));
    monkeys = (List<Monkey>)serializer.Deserialize(reader);
}
var listView = new ListView ();
listView.ItemsSource = monkeys;
```

 [ ![Xml ファイルは、ListView に表示される、PCL に埋め込まれている](files-images/pclxml-sml.png "PCL がリスト ビューに表示される埋め込み XML ファイル")](files-images/pclxml.png "PCL がリスト ビューに表示される埋め込み XML ファイル")

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
#if WINDOWS_PHONE
var resourcePrefix = "WorkingWithFiles.WinPhone.";
#endif

Debug.WriteLine("Using this resource prefix: " + resourcePrefix);
// note that the prefix includes the trailing period '.' that is required
var assembly = typeof(SharedPage).GetTypeInfo().Assembly;
Stream stream = assembly.GetManifestResourceStream
    (resourcePrefix + "SharedTextResource.txt");
```

<a name="Organizing_Resources" />

### <a name="organizing-resources"></a>リソースの整理

上記の例では、ファイルは、フォームの場合、リソース ID は、PCL プロジェクトのルートに埋め込まれていると仮定**Namespace.Filename.Extension**など`WorkingWithFiles.PCLTextResource.txt`と`WorkingWithFiles.iOS.SharedTextResource.txt`です。

フォルダー内の埋め込みリソースを整理することができます。 フォルダーに埋め込みリソースが配置されると、フォルダー名の一部となります (ピリオドで区切られた) リソース ID、リソース ID の形式になるように**Namespace.Folder.Filename.Extension**です。 フォルダーにサンプル アプリで使用されるファイルを配置する**MyFolder**対応するリソース Id 実行するように`WorkingWithFiles.MyFolder.PCLTextResource.txt`と`WorkingWithFiles.iOS.MyFolder.SharedTextResource.txt`です。

<a name="Debugging_Embedded_Resources" />

### <a name="debugging-embedded-resources"></a>埋め込みリソースのデバッグ

特定のリソースを読み込まれていない理由を理解しにくい場合があります、ため、次のデバッグ コードが、リソースが正しく構成されていることを確認するために、アプリケーションを一時的に追加できます。 特定のアセンブリに埋め込まれた認識されているすべてのリソースを出力は、**エラー**リソース読み込みの問題をデバッグする際のパッド。

```csharp
using System.Reflection;
// ...
// use for debugging, not in released app code!
var assembly = typeof(SharedPage).GetTypeInfo().Assembly;
foreach (var res in assembly.GetManifestResourceNames()) {
    System.Diagnostics.Debug.WriteLine("found resource: " + res);
}
```

<a name="Loading_and_Saving_Files" />

## <a name="saving-and-loading-files"></a>保存して、ファイルを読み込んでいます

Xamarin.Forms は、それぞれ独自のファイル システムに複数のプラットフォームで実行されるため、ユーザーによって作成されたファイルの読み込みと保存の 1 つのアプローチはありません。 保存し、サンプル アプリには、表示する画面保存し、読み込む - 一部のユーザー入力にはが含まれています。 テキスト ファイルを読み込む方法を示すために、完成した画面は次に示します。

 [ ![保存とテキストを読み込み](files-images/saveandload-sml.png "を保存し、アプリでのファイルの読み込み")](files-images/saveandload.png "を保存し、アプリでのファイルの読み込み")

各プラットフォームには、若干異なるディレクトリ構造、および異なる filesystem 機能 - たとえば、Xamarin.iOS および Xamarin.Android ほとんどがサポート`System.IO`のみをサポート機能しますが、Windows Phone`IsolatedStorage`と[`Windows.Storage` ](http://msdn.microsoft.com/library/windowsphone/develop/jj681698(v=vs.105).aspx) Api です。

この問題を回避するには、サンプル アプリは、読み込み、ファイルを保存する Xamarin.Forms PCL にインターフェイスを定義します。 デバイスに格納できる読み込みおよびテキスト ファイルを保存する単純な API を提供します。

```csharp
public interface ISaveAndLoad {
    void SaveText (string filename, string text);
    string LoadText (string filename);
}
```

使用してコードの PCL、 [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md)をインターフェイスのネイティブ実装への参照を取得します。 これにより、移植可能なコードを読み込みと各プラットフォームに固有のプロジェクトで記述されたクラスへのファイルの保存を委任できます。 サンプルでは、**保存**と**ロード**ように、ボタンが書き込まれます。

```csharp
var saveButton = new Button {Text = "Save"};
saveButton.Clicked += (sender, e) => {
    DependencyService.Get<ISaveAndLoad>().SaveText("temp.txt", input.Text);
};
var loadButton = new Button {Text = "Load"};
loadButton.Clicked += (sender, e) => {
    output.Text = DependencyService.Get<ISaveAndLoad>().LoadText("temp.txt");
};
```

実装は、ファイルは実際に保存して読み込む前に、各プラットフォームに固有のプロジェクトに追加する必要があります。

### <a name="ios-and-android"></a>iOS および Android

Xamarin.iOS および Xamarin.Android プロジェクト用のインターフェイスの実装は同じであることができます。 コードを次に、以下を含む、`[assembly: Dependency (typeof (SaveAndLoad))]`に必要な属性、`DependencyService`動作をします。

```csharp
[assembly: Dependency (typeof (SaveAndLoad))]
namespace WorkingWithFiles {
    public class SaveAndLoad : ISaveAndLoad {
        public void SaveText (string filename, string text) {
            var documentsPath = Environment.GetFolderPath (Environment.SpecialFolder.Personal);
            var filePath = Path.Combine (documentsPath, filename);
            System.IO.File.WriteAllText (filePath, text);
        }
        public string LoadText (string filename) {
            var documentsPath = Environment.GetFolderPath (Environment.SpecialFolder.Personal);
            var filePath = Path.Combine (documentsPath, filename);
            return System.IO.File.ReadAllText (filePath);
        }
    }
}
```

### <a name="universal-windows-platform-uwp-windows-81-and-windows-phone-81"></a>ユニバーサル Windows プラットフォーム (UWP)、Windows 8.1 および Windows Phone 8.1

これらのプラットフォームがある別のファイル システム API – [ `Windows.Storage` ](/windows/uwp/files/quickstart-reading-and-writing-files/) – つまりを保存し、ファイルを読み込むために使用します。
`ISaveAndLoad`次に示すように、インターフェイスを実装することができます。

```csharp
using System;
using System.Threading.Tasks;
using Windows.Storage;
using WinApp;
using WorkingWithFiles;
using Xamarin.Forms;

[assembly: Dependency(typeof(SaveAndLoad_WinApp))]

namespace WindowsApp
{
    // https://msdn.microsoft.com/library/windows/apps/xaml/hh758325.aspx
    public class SaveAndLoad_WinApp : ISaveAndLoad
    {
        public async Task SaveTextAsync(string filename, string text)
        {
            StorageFolder localFolder = ApplicationData.Current.LocalFolder;
            StorageFile sampleFile = await localFolder.CreateFileAsync(filename, CreationCollisionOption.ReplaceExisting);
            await FileIO.WriteTextAsync(sampleFile, text);
        }
        public async Task<string> LoadTextAsync(string filename)
        {
            StorageFolder storageFolder = ApplicationData.Current.LocalFolder;
            StorageFile sampleFile = await storageFolder.GetFileAsync(filename);
            string text = await Windows.Storage.FileIO.ReadTextAsync(sampleFile);
            return text;
        }
    }
}
```


<a name="Saving_and_Loading_in_Shared_Projects" />

### <a name="saving-and-loading-in-shared-projects"></a>保存と共有プロジェクトの読み込み

共有プロジェクト コンパイラ ディレクティブをサポートするためになっても、共有プロジェクト内の 1 つのクラス ファイルにすべてのプラットフォーム固有のコードを含める (を使用せず、 `DependencyService`)。
1 つ`SaveAndLoad`のようなコンパイラ ディレクティブを使用して参照しているプロジェクトにコンパイル選択的に、上記の両方の実装を含むクラスを記述する可能性があります`#if WINDOWS_PHONE`、 `#if __IOS__`、および`#if __ANDROID__`です。

## <a name="additional-information"></a>追加情報

Xamarin.Forms プロジェクトの PCL に基づく活用することも、 [PCLStorage NuGet](http://www.nuget.org/packages/pclstorage) ([コード&amp;ドキュメント](https://pclstorage.codeplex.com/)) クロスプラット フォームの方法でファイルの操作の実装に役立ちます。


## <a name="summary"></a>まとめ

このドキュメントでは、埋め込みリソースの読み込みと保存と、デバイス上のテキストを読み込みするための単純なファイル操作を説明します。 開発者が独自ネイティブ Api を使用してファイルを実装することができます、 `DependencyService`、ファイル操作の要件を処理するために必要に応じて複雑になります。


## <a name="related-links"></a>関連リンク

- [FilesSample](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithFiles/)
- [作成、書き込み、およびファイル (UWP) の読み取り](/windows/uwp/files/quickstart-reading-and-writing-files/)
- [Xamarin.Forms のサンプル](https://github.com/xamarin/xamarin-forms-samples)
- [ファイルのブック](https://developer.xamarin.com/workbooks/xamarin-forms/application-fundamentals/files/files.workbook)
