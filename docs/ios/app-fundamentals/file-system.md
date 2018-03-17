---
title: "ファイル システムでの作業"
description: "Xamarin.iOS は、任意の .NET アプリケーションで使用する iOS でファイルおよびディレクトリを使用する同じ System.IO クラスを使用することができます。 ただし、使い慣れたクラスとメソッドに関係なく、iOS は作成またはアクセスできるファイルのいくつかの制限を実装し、機能も提供特別な特定のディレクトリ。 この記事では、これらの制限およびの機能について説明し、Xamarin.iOS アプリケーションでファイルへのアクセスがどのように動作するかを示します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 37DF2F38-901E-8F8E-269A-5EE0CCD28C08
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: fd6aa66a7e5e788babc0df3e94b8f3677a7625f0
ms.sourcegitcommit: 5fc1c4d17cd9c755604092cf7ff038a6358f8646
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/17/2018
---
# <a name="working-with-the-file-system"></a>ファイル システムでの作業

_Xamarin.iOS は、任意の .NET アプリケーションで使用する iOS でファイルおよびディレクトリを使用する同じ System.IO クラスを使用することができます。ただし、使い慣れたクラスとメソッドに関係なく、iOS は作成またはアクセスできるファイルのいくつかの制限を実装し、機能も提供特別な特定のディレクトリ。この記事では、これらの制限およびの機能について説明し、Xamarin.iOS アプリケーションでファイルへのアクセスがどのように動作するかを示します。_

Xamarin.iOS を使用して、`System.IO`内のクラス、 *.NET 基本クラス ライブラリ (BCL)* iOS ファイル システムにアクセスします。 `File`クラスを使用して、作成、削除、および、ファイルの読み取りおよび`Directory`クラスでは、作成、削除、またはディレクトリの内容を列挙することができます。 使用することも`Stream`サブクラスより多くのファイル操作 (ファイル内の圧縮や位置を検索) などの制御を提供できます。

iOS では、アプリケーションが、アプリケーションのデータのセキュリティを維持して、有害なアプリからユーザーを保護するファイル システムで実行できる操作のいくつかの制限があります。 これらの制限の一部である、*アプリケーション サンド ボックス*– ファイル、設定、ネットワーク リソース、ハードウェアなどに、アプリケーションのアクセスを制限する規則のセット。アプリケーションがそのホーム ディレクトリ (インストールされている場所) 内のファイルの読み書きに制限されます。他のアプリケーションのファイルにアクセスできません。

iOS にもいくつかのファイル システムに固有の機能: 特定のディレクトリのバックアップと、アップグレードに関して特別な処理を必要とし、アプリケーションは、iTunes を使用してファイルを共有もできます。

機能について説明し、iOS の制限の詳細、ファイル システム Xamarin.iOS を使用して、いくつかの単純なファイル システム操作を実行する方法を示すサンプル アプリケーションが含まれています。

 [![](file-system-images/05-sampleapp.png "一部の単純なファイル システム操作を実行する iOS のサンプル")](file-system-images/05-sampleapp.png#lightbox)

 <a name="General_File_Access" />


## <a name="general-file-access"></a>一般的なファイル アクセス

Xamarin.iOS 使用できるように、.NET `System.IO` iOS でのファイル システム操作のためのクラスです。

次のコード スニペットは、いくつかの一般的なファイル操作を示しています。 できたら、それらすべての下に、`SampleCode.cs`この記事のサンプル アプリケーションでのファイルです。

 <a name="Working_with_directories" />


### <a name="working-with-directories"></a>ディレクトリの操作

このコードは、現在のディレクトリのサブディレクトリを列挙 (で指定した、"./"パラメーター)、これは、アプリケーション実行可能ファイルの場所。
出力は、すべてのファイルとは (デバッグ中は、コンソール ウィンドウに表示されます)、アプリケーションと共に配置されるフォルダーの一覧になります。

```csharp
var directories = Directory.EnumerateDirectories("./");
foreach (var directory in directories) {
      Console.WriteLine(directory);
}
```

 <a name="Reading_files" />


### <a name="reading-files"></a>ファイルの読み取り

テキスト ファイルの読み取り、1 行のコードだけで済みます。 この例では、アプリケーションの出力 ウィンドウでテキスト ファイルの内容が表示されます。

```csharp
var text = File.ReadAllText("TestData/ReadMe.txt");
Console.WriteLine(text);
```

 <a name="XML_Serialization" />


### <a name="xml-serialization"></a>XML シリアル化

完全な操作が`System.Xml`名前空間は、この記事の範囲を超えては、次のように StreamReader を使用して、ファイル システムから XML ドキュメントを簡単に逆ことができます。

```csharp
using (TextReader reader = new StreamReader("./TestData/test.xml")) {
      XmlSerializer serializer = new XmlSerializer(typeof(MyObject));
      var xml = (MyObject)serializer.Deserialize(reader);
}
```

MSDN のドキュメントを参照してください、 [System.Xml](http://msdn.microsoft.com/en-us/library/system.xml.aspx)名前空間の詳細については[シリアル化](http://msdn.microsoft.com/en-us/library/system.xml.serialization.aspx)です。 確認する必要があります、 [Xamarin.iOS ドキュメント](~/ios/deploy-test/linker.md)リンカー – 通常必要がありますを追加する、`[Preserve]`属性をシリアル化するクラス。

 <a name="Creating_Files_and_Directories" />


### <a name="creating-files-and-directories"></a>ファイルとディレクトリを作成します。

このサンプルを使用する方法を示します、`Environment`ファイルとディレクトリを作成できるドキュメント フォルダーにアクセスするクラス。

```csharp
var documents =
 Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments); 
var filename = Path.Combine (documents, "Write.txt");
File.WriteAllText(filename, "Write this text into a file");
```

非常に同様のプロセスは、ディレクトリを作成します。

```csharp
var documents =
 Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments);
var directoryname = Path.Combine (documents, "NewDirectory");
Directory.CreateDirectory(directoryname);
```

System.IO 名前空間の詳細については、次を参照してください。、 [MSDN ドキュメント](http://msdn.microsoft.com/en-us/library/system.io.aspx)です。


### <a name="serializing-json"></a>Json をシリアル化します。

Json を操作する Xamarin.iOS アプリケーション内のデータを非常に簡単を使用して、 [Json.NET](http://www.newtonsoft.com/json) for .NET NuGet パッケージのパフォーマンスの高い JSON フレームワークです。 アプリケーションのプロジェクトに、NuGet パッケージを追加します。 

[![](file-system-images/json01.png "NuGet パッケージのアプリケーション プロジェクトを追加します。")](file-system-images/json01.png#lightbox)

シリアル化または逆シリアル化のデータ モデルとして機能するクラスを次に、追加 (ここでは`Account.cs`)。

```csharp
using System;
using System.Collections.Generic;
using Foundation; // for Preserve attribute, which helps serialization with Linking enabled

namespace FileSystem
{
    [Preserve]
    public class Account
    {
        #region Computed Properties
        public string Email { get; set; }
        public bool Active { get; set; }
        public DateTime CreatedDate { get; set; }
        public List<string> Roles { get; set; }
        #endregion

        #region Constructors
        public Account() {

        }
        #endregion
    }
}
```

インスタンスを最後に、作成、`Account`クラス、json データにシリアル化、およびファイルへの書き込み。

```csharp
// Create a new record
var account = new Account(){
    Email = "monkey@xamarin.com",
    Active = true,
    CreatedDate = new DateTime(2015, 5, 27, 0, 0, 0, DateTimeKind.Utc),
    Roles = new List<string> {"User", "Admin"}
};

// Serialize object
var json = JsonConvert.SerializeObject(account, Newtonsoft.Json.Formatting.Indented);

// Save to file
var documents = Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments);
var filename = Path.Combine (documents, "account.json");
File.WriteAllText(filename, json);
```
.NET を Json を参照してください[ドキュメント](http://www.newtonsoft.com/json/help)詳細については、.NET アプリケーションで json データを操作します。

<a name="Special_Considerations" />

## <a name="special-considerations"></a>特別な考慮事項

類似点、Xamarin.iOS および .NET のファイル操作の間は、iOS や Xamarin.iOS がいくつかの重要な点で .NET と異なります。

 <a name="runtimeaccessible" />


### <a name="making-project-files-accessible-at-runtime"></a>プロジェクト ファイルを実行時にアクセスできるようにします。

既定では、ファイルをプロジェクトに追加する場合は、最終的なアセンブリに含まれませんし、そのため、アプリケーションで使用できません。 アセンブリのファイルを含めるためには、コンテンツと呼ばれる特別なビルド アクションでマークする必要があります。

追加のファイルをマークするには、ファイルを右クリックし、選択**ビルド アクション&gt;コンテンツ**for mac の Visual Studio で 変更することも、**ビルド アクション**ファイルの**プロパティ**シートです。

 <a name="Case_Sensitivity" />


### <a name="case-sensitivity"></a>大文字と小文字の区別

IOS ファイル システムがあることを理解することが重要*大文字小文字を区別*です。 つまり、ファイルおよびディレクトリ名が正確に一致する必要があります – README.txt と readme.txt とみなされます異なるファイル名のことです。

Windows ファイル システムにさらに精通している .NET 開発者にとって複雑になる場合が考えられます*大文字小文字を区別*–"Files"、"FILES"および"files"のすべてが同じディレクトリを参照しています。

そのため、iOS デバイスは大文字小文字を区別であり、コードは、ことに注意を記述する必要があります、iOS シミュレーターはない場合を区別既定でです。 つまり、ファイル自体とコードへの参照の間で、ファイル名の大文字と小文字が異なる場合で、コード可能性があります、シミュレーターでまだ作業が、実際のデバイスで失敗です。 これは、初期および iOS 開発時に多くの場合、実際のデバイスに展開する重要性が理由の 1 つです。

 <a name="Path_Separator" />


### <a name="path-separator"></a>パスの区切り文字

iOS がスラッシュを使用してパスの区切り記号としては、'/' (これとは異なるバック スラッシュを使用する Windows ' \')。

使用することをお勧めはこの混乱を招く違いにより、`System.IO.Path.Combine`メソッドで、特定のパス区切り記号をハードコーディングするのではなく、現在のプラットフォームの調整します。 これは、他のプラットフォームにより、コードの移植性を向上する簡単な手順です。

 <a name="Application_Sandbox" />


## <a name="application-sandbox"></a>アプリケーションのサンド ボックス

ファイル システム (およびその他のリソース、ネットワークとハードウェアの機能など) に、アプリケーションのアクセス セキュリティの理由により制限されます。 この制限と呼ばれる、*アプリケーション サンド ボックス*です。 ファイル システムの観点から、アプリケーションは、作成とそのホーム ディレクトリでファイルおよびディレクトリの削除に制限されます。

ホーム ディレクトリは、アプリケーションとそのすべてのデータが格納されているファイル システム内の一意な場所です。 アプリケーションのホーム ディレクトリの場所を選択 (または変更) にすることはできません。ただし iOS および Xamarin.iOS プロパティとメソッドのファイルおよびディレクトリ内の管理を提供します。

 <a name="The_Application_Bundle" />


## <a name="the-application-bundle"></a>アプリケーション バンドル

*アプリケーション バンドル*は、アプリケーションを含むフォルダーです。
により、ディレクトリ名に追加される .app サフィックスを持つ他のフォルダーから区別されます。 アプリケーション バンドルには、実行可能ファイルとすべてのコンテンツ (ファイル、画像など)、プロジェクトに必要なが含まれています。

その他のディレクトリを参照してください (および .app サフィックスが非表示); よりも、別のアイコンと表示された Mac OS で、アプリケーション バンドルを参照するときただし、通常のオペレーティング システムが異なる方法で表示されているディレクトリのみを勧めします。

Mac と選択の Visual Studio でプロジェクトを右クリックするサンプル コードのアプリケーション バンドルを確認するには、**を含むフォルダーを開く**です。 移動し、 **Bin/debug/**が表示される必要がありますアプリケーション アイコン (次のスクリーン ショットに類似)。

 [![](file-system-images/40-bundle.png "このスクリーン ショットのようなアプリケーション アイコンを検索するには、Bin/debug に移動します。")](file-system-images/40-bundle.png#lightbox)

このアイコンを右クリックして選択**パッケージの内容の表示**アプリケーション バンドルのディレクトリの内容を参照します。 内容は、次に示すように、正規のディレクトリの内容と同じように表示されます。

 [![](file-system-images/45-bundle.png "アプリケーション バンドルの内容")](file-system-images/45-bundle.png#lightbox)

アプリケーション バンドルがインストールされている、シミュレーターまたはデバイスでのテスト中に、App Store に含めることを Apple に送信内容は最終的には.

 <a name="Application_Directories" />


## <a name="application-directories"></a>アプリケーションのディレクトリ

アプリケーションがデバイスにインストールされている場合、オペレーティング システムがそのホーム ディレクトリを作成し、アプリケーション バンドルの内側に配置します。 Nothing に書き込むか、該当のルート ディレクトリと署名されているすべての変更は、アプリケーションを無効にしを起動できなくは、コードがデータを読み取るアプリケーション バンドルにアクセスできます。

Nothing に書き込むか、ルート ディレクトリが<b>iOS 7 およびそれ以前に</b>アプリケーションのルート ディレクトリ内のディレクトリを使用するために使用できるいくつか作成します。 <b>ユーザーがアクセスできるディレクトリは、8、iOS で<a href="https://developer.apple.com/library/ios/technotes/tn2406/_index.html" target="_blank">に存在しない</a>アプリケーション ルート内</b>です。

これらのディレクトリとその用途を以下に示します。

&nbsp;

|ディレクトリ|説明|
|---|---|
|[ApplicationName].app/|**IOS 7 およびそれ以前で**これは、`ApplicationBundle`アプリケーション実行可能ファイルが格納されているディレクトリ。 アプリで作成したディレクトリ構造は、(たとえば、イメージや Mac プロジェクトの Visual Studio でのリソースとしてマークした他のファイルの種類) は、このディレクトリに存在します。<br /><br />このディレクトリへのパスを利用し、アプリケーション バンドル内のコンテンツ ファイルにアクセスする必要がある場合、`NSBundle.MainBundle.BundlePath`プロパティです。|
|ドキュメント/|このディレクトリを使用すると、ユーザーのドキュメントおよびアプリケーション データ ファイルを格納できます。<br /><br />このディレクトリの内容できますで入手できる、ユーザーに iTunes のファイル (ただし、これは既定で無効) を共有します。 追加、`UIFileSharingEnabled`ブール型のキーをユーザーにこれらのファイルのアクセスを許可する Info.plist ファイルにします。<br /><br />場合でも、アプリケーションでは、ファイル共有を有効にしないすぐに、このディレクトリでユーザーから非表示にするファイルを配置することを避ける必要があります (など、データベース ファイルを共有する場合)。 機密性の高いファイルは非表示のまま、限り、これらのファイルいない公開されている (とする可能性のある移動、変更、または iTunes によって削除された) 場合は、今後のバージョンのファイル共有を有効にします。<br /><br /> 使用することができます、`Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments)`アプリケーションの Documents ディレクトリへのパスを取得します。<br /><br />このディレクトリの内容は、iTunes によってバックアップされます。|
|ライブラリ/|ライブラリ ディレクトリは、データベースやその他のアプリケーションによって生成されたファイルなど、ユーザーが直接作成されないファイルを格納する適切な場所です。 このディレクトリの内容は、iTunes を使用してユーザーに決して公開されます。<br /><br />ライブラリで、独自のサブディレクトリを作成することができます。ただし、既にあるいくつかシステムで作成されたディレクトリは、ここの設定、キャッシュなど、注意してください。<br /><br />このディレクトリ (を除くキャッシュ サブディレクトリ) の内容は、iTunes によってバックアップされます。 ライブラリで作成したカスタム ディレクトリのバックアップが作成されます。|
|ライブラリの設定//|アプリケーション固有の基本設定ファイルは、このディレクトリに格納されます。 これらのファイルを直接作成しません。 代わりに、使用、`NSUserDefaults`クラスです。<br /><br />このディレクトリの内容は、iTunes によってバックアップされます。|
|キャッシュ/ライブラリ/|キャッシュ ディレクトリは、アプリケーションに役立つデータ ファイルを保存する場所として適してを実行することが可能に簡単に再作成に必要な場合です。 アプリケーションでは、作成、必要に応じて、これらのファイルを削除し、および必要に応じて、これらのファイルを再作成できる必要があります。 ただし、しません。 そのため、アプリケーションの実行中に、iOS 5 は (状況非常に低い記憶域)、これらのファイルを削除も可能性があります。<br /><br />このディレクトリの内容は、つまり、ユーザー、デバイスを復元する場合はできません、iTunes によってバックアップされませんし、アプリケーションの更新バージョンをインストールした後に存在できない可能性があります、します。<br /><br />インスタンスの場合、アプリケーションは、ネットワークに接続できない、データまたはオフラインの良好なエクスペリエンスを提供するファイルを格納するキャッシュ ディレクトリを使用する可能性があります。 アプリケーションを保存して、ネットワークの応答の待機中にこのデータを迅速に取得できますをバックアップする必要があるとしたりはできます簡単にする回復復元またはバージョンが更新後に再作成します。|
|tmp/|アプリケーションでは、このディレクトリに短い期間にのみ必要な一時ファイルを格納できます。 領域を節約するために必要でなくなったときに、ファイルを削除する必要があります。 アプリケーションが実行されていない場合、オペレーティング システムは、このディレクトリからファイルを削除もできます。<br /><br />このディレクトリの内容は、iTunes のバックアップはバックアップされません。<br /><br />たとえば、tmp ディレクトリされる可能性があります (Twitter アバターや電子メールの添付ファイル)、ユーザーに表示するためダウンロードしたが、されて表示 (をした場合は必須では、後でもう一度ダウンロード後にそれらを削除する可能性があります、一時ファイルを保存するには).|

このスクリーン ショットは、ファインダー ウィンドウで、ディレクトリ構造を示しています。

 [![](file-system-images/08-library-directory.png "このスクリーン ショットは、ファインダー ウィンドウで、ディレクトリ構造を示しています")](file-system-images/08-library-directory.png#lightbox)

 <a name="Accessing_Other_Directories_Programmatically" />

### <a name="accessing-other-directories-programmatically"></a>その他のディレクトリをプログラムでアクセスします。

アクセス以前のディレクトリとファイルの例、`Documents`ディレクトリ。 使用してパスを構築する必要がありますを別のディレクトリに書き込む、".."は、ここに示すような構文。

```csharp
var documents = Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments);
var library = Path.Combine (documents, "..", "Library");
var filename = Path.Combine (library, "WriteToLibrary.txt");
File.WriteAllText(filename, "Write this text into a file in Library");
```

ディレクトリの作成とよく似ています。

```csharp
var documents = Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments);
var library = Path.Combine (documents, "..", "Library");
var directoryname = Path.Combine (library, "NewLibraryDirectory");
Directory.CreateDirectory(directoryname);
```

パス、`Caches`と`tmp`ディレクトリは、次のように構築できます。

```csharp
var documents = Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments);
var cache = Path.Combine (documents, "..", "Library", "Caches");
var tmp = Path.Combine (documents, "..", "tmp");
```

 <a name="Sharing_Files_with_the_User_through_iTunes" />


## <a name="sharing-files-with-the-user-through-itunes"></a>ITunes を通じてユーザーとファイルの共有

ユーザーは編集することによって、アプリケーションの Documents ディレクトリにファイルにアクセスできる`Info.plist`を作成して、**アプリケーションは、iTunes の共有をサポートしている**(`UIFileSharingEnabled`) 内のエントリ、**ソース**ビューとして次に示します。

 [![](file-system-images/09-uifilesharingenabled-plist.png "プロパティを共有 iTunes をサポートしているアプリケーションを追加します。")](file-system-images/09-uifilesharingenabled-plist.png#lightbox)

これらのファイルは、デバイスが接続されているし、ユーザーが選択したときに、iTunes でアクセスできる、`Apps`タブです。たとえば、次のスクリーン ショットでは、選択したアプリが iTunes 経由の共有でファイルを示しています。

 [![](file-system-images/10-itunes-file-sharing.png "このスクリーン ショットでは、選択したアプリが iTunes 経由の共有のファイルを表示します。")](file-system-images/10-itunes-file-sharing.png#lightbox)

ユーザーは、iTunes を経由してこのディレクトリのトップレベルの項目にのみアクセスできます。 (ただし、そのコンピューターにコピーするか、削除) は、すべてのサブディレクトリの内容を参照してください、ことはできません。 たとえば、GoodReader で PDF および epub 形式のファイル共有できます、アプリケーションにできるように、ユーザーが自分の iOS デバイスで読み取ることができます。

[ドキュメント] フォルダーの内容を変更するユーザー問題が発生していない場合は注意してください。 アプリケーションは、これを考慮し、ドキュメント フォルダーの破壊的な更新プログラムに対応する必要があります。

この記事のサンプル コードは ドキュメント フォルダーにファイルとフォルダーの両方を作成します。 ( **SampleCode.cs**) でのファイル共有を有効にし、 **Info.plist**ファイル。 このスクリーン ショットは、iTunes でこれらの表示方法を示しています。

 [![](file-system-images/15-itunes-file-sharing-example.png "このスクリーン ショットは、iTunes のファイルを表示する方法を示しています。")](file-system-images/15-itunes-file-sharing-example.png#lightbox)

参照してください、[イメージを操作](~/ios/app-fundamentals/images-icons/index.md)を作成するカスタム ドキュメントの種類のアプリケーションのアイコンを設定する方法についての情報の記事です。

場合、`UIFileSharingEnabled`キーが false であるか、または存在しない場合、ファイル共有既定では、無効になっているし、ユーザーを Documentsdirectory と対話することにすることはできません。

 <a name="Backup_and_Restore" />

## <a name="backup-and-restore"></a>バックアップと復元

デバイスは、iTunes によってバックアップは、次を除く、アプリケーションのホーム ディレクトリで作成されたすべてのディレクトリが保存されます。

-   **[ApplicationName] .app** – アプリケーション バンドル*は*バンドルが (アプリケーションの更新プログラムをインストールするときなど) に変更されたときにのみ、バックアップを取得します。 署名されているため、インストール後に変更する必要がありますされませんので、このディレクトリを変更するべきではありません。 
-   **ライブラリのキャッシュ/** : 作業ファイルをバックアップする必要がないため、キャッシュ ディレクトリが対象としています。 
-   **tmp** -このディレクトリが作成され、不要になったときに削除する一時ファイルの使用または領域が必要なときにファイルをその iOS を削除します。 


大量のデータをバックアップすると、時間がかかることができます。 アプリケーションではドキュメントおよびライブラリを使用する必要がありますのみの特定のドキュメントやデータをバックアップする必要がある場合は、このフォルダーです。 一時的なデータ、またはネットワークから簡単に取得できるファイルのキャッシュまたは tmp ディレクトリのいずれかを使用します。

IOS は 'クリーン'、ファイル システム ディスクの空き領域不足、デバイスの実行時に注意してください。 ライブラリとキャッシュと tmp からすべてのファイルが削除されますが、現在実行されていないアプリケーションのフォルダーです。

 <a name="Complying_with_iOS5_iCloud_Backup_Restrictions" />

## <a name="complying-with-ios5-icloud-backup-restrictions"></a>IOS5 iCloud のバックアップの制限に準拠します。

導入された Apple *iCloud バックアップ*iOS 5 で機能を使用します。 ICloud のバックアップが有効にすると、アプリケーションのホーム ディレクトリ内のすべてのファイル (通常バックアップされない、アプリ バンドルなどのディレクトリを除外`Caches`と`tmp`) は、バックアップ iCloud サーバーにします。 この機能は、自分のデバイスの紛失、盗難にあったまたは破損している場合に、完全なバックアップを持つユーザーを提供します。

ICloud は、ユーザーごとに 5 Gb の 'free' 領域を提供するだけため、帯域幅が不必要に使用を避けるためには、Apple はバックアップ不可欠なユーザーによって生成されたデータのみをアプリケーションが必要です。 IOS データ記憶域のガイドラインに準拠するには、次の項目に従うことでバックアップされるデータの量を制限する必要があります。

-  ユーザーによって生成されたデータ、またはそれ以外の場合、ドキュメントは、ディレクトリ (にバックアップ)、再作成できないデータを格納だけです。 
-  簡単に再作成または再でダウンロードできるその他のデータを格納`Library/Caches`または`tmp`(がないバックアップ、および 'クリーンアップでした')。 
-  適切な可能性のあるファイルがある場合、`Library/Caches`または`tmp`フォルダーがたくない 'クリーニング' 別の場所に保存する (など`Library/YourData`) を適用し、' バックアップを作成しないを ' ファイルを使用できるように iCloud バックアップ ba を属性ndwidth と記憶域の領域です。 このデータは現在も、デバイス上の領域を使用してため、慎重に管理し、可能であればそれを削除する必要があります。 

'バックアップを作成しないを' を使用して属性を設定、`NSFileManager`クラスです。 クラスが`using Foundation`を呼び出すと`SetSkipBackupAttribute`次のようにします。

```csharp
var documents = Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments);
var filename = Path.Combine (documents, "LocalOnly.txt");
File.WriteAllText(filename, "This file will never get backed-up. It would need to be re-created after a restore or re-install");
NSFileManager.SetSkipBackupAttribute (filename, true); // backup will be skipped for this file
```

ときに`SetSkipBackupAttribute`は`true`ファイルでは、バックアップに格納されているディレクトリに関係なくされません (であっても、`Documents`ディレクトリ)。 属性を使用して、クエリを実行することができます、`GetSkipBackupAttribute`メソッド、およびをリセットできますを呼び出して、`SetSkipBackupAttribute`メソッドを`false`、次のようにします。

```csharp
NSFileManager.SetSkipBackupAttribute (filename, false); // file will be backed-up
```

## <a name="sharing-data-between-ios-apps-and-app-extensions"></a>データの間で iOS アプリおよびアプリの拡張機能を共有

(そのを含むアプリ) ではなく、ホスト アプリケーションの一部として実行されるアプリの拡張機能、のでのデータが共有されていない自動含まれるため、追加の作業が必要です。 アプリ グループは、メカニズム iOS を使用して別のアプリ データを共有することを許可します。 アプリケーションは、適切な権利を正しく構成されているし、プロビジョニングの場合、通常の iOS のサンド ボックスの外部で共有ディレクトリにアクセスします。

### <a name="configure-an-app-group"></a>アプリ グループを構成します。

使用して共有の場所を構成、[アプリ グループ](https://developer.apple.com/library/ios/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19)で構成されて、**証明書、識別子、およびプロファイル**セクションで[iOS Dev Center](https://developer.apple.com/devcenter/ios/)です。 この値は、各プロジェクトの参照も必要があります**Entitlements.plist**です。

作成して、アプリ グループを構成する方法についてを参照してください、[アプリ グループ機能](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md)ガイドです。

### <a name="files"></a>ファイル

IOS アプリ拡張機能では、一般的なファイル パスを使用してファイルを共有できるも (指定された適切な正しい権利に構成されているプロビジョニングとプロビジョニング)。

```csharp
var FileManager = new NSFileManager ();
var appGroupContainer =FileManager.GetContainerUrl ("group.com.xamarin.WatchSettings");
var appGroupContainerPath = appGroupContainer.Path

Console.WriteLine ("Group Path: " + appGroupContainerPath);

// use the path to create and update files
...
```
> [!IMPORTANT]
> **注:**がグループのパスが返される場合は`null`の権利とプロビジョニング プロファイルの構成を確認およびが正しいことを確認してください。


<a name="Application_Version_Updates" />

## <a name="application-version-updates"></a>アプリケーションのバージョンの更新

アプリケーションの新しいバージョンがダウンロードされると、iOS は新しいホーム ディレクトリを作成し、その中に、新しいアプリケーション バンドルを格納します。 iOS では、アプリケーション バンドルの以前のバージョンから、次のフォルダーが、新しいホーム ディレクトリに移動します。

-  **ドキュメント**
-  **Library**


その他のディレクトリがまた間でコピーされ、新しいホーム ディレクトリの下に配置が、するいるとは限りませんコピーされるため、アプリケーションはこのシステムの動作に依存しないようにします。

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事では、ファイル システム操作は他の .NET アプリケーションと同様、Xamarin.iOS ととして単純なことを示しました。 アプリケーションのサンド ボックスを導入され、その原因となるセキュリティへの影響を確認します。 次に、アプリケーション バンドルの概念を説明します。 最後に、アプリケーションで使用できる特殊なディレクトリを列挙し、アプリケーションのアップグレードおよびバックアップ中に、そのロールを説明します。


## <a name="related-links"></a>関連リンク

- [FileSystemSampleCode](https://developer.xamarin.com/samples/FileSystemSampleCode/)
- [ファイル システム プログラミング ガイド](http://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/FileSystemProgrammingGUide/Introduction/Introduction.html)
- [アプリがサポートする型ファイルを登録します。](http://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Articles/RegisteringtheFileTypesYourAppSupports.html#/apple_ref/doc/uid/TP40010411-SW1)
