---
title: Xamarin.iOS でのファイル システムの操作
description: このドキュメントでは、Xamarin.iOS でのファイル システムを操作する方法について説明します。 これには、ディレクトリ、ファイル、XML と JSON のシリアル化、iTunes などを使用してファイルの共有アプリケーションのサンド ボックスの読み取りについて説明します。
ms.prod: xamarin
ms.assetid: 37DF2F38-901E-8F8E-269A-5EE0CCD28C08
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: cf595b57906cf1c47acdcdbcddf04bfbdc963393
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50113431"
---
# <a name="working-with-the-file-system-in-xamarinios"></a>Xamarin.iOS でのファイル システムの操作

Xamarin.iOS を使用して、`System.IO`クラス、 *.NET 基本クラス ライブラリ (BCL)* iOS ファイル システムにアクセスします。 `File`クラスにより、作成、削除、および、ファイルの読み取りおよび`Directory`クラスでは、作成、削除、またはディレクトリの内容を列挙することができます。 使用することも`Stream`サブクラスより詳細なファイル操作 (など、ファイル内の圧縮または位置の検索) 経由での制御を提供することができます。

iOS で、アプリケーションが、アプリケーションのデータのセキュリティを維持して、有害なアプリからユーザーを保護するファイル システムで実行できる操作のいくつかの制限が課せられます。 これらの制限の一部である、*アプリケーションのサンド ボックス*– ファイル、設定、ネットワーク リソース、ハードウェアなどに、アプリケーションのアクセスを制限する一連の規則。アプリケーションがそのホーム ディレクトリ (インストールされている場所) 内のファイルの読み書きに制限されます。別のアプリケーションのファイルにアクセスできません。

iOS はいくつかのファイル システムに固有の機能もあります。 特定のディレクトリのバックアップと、アップグレードに関して特別な処理を必要とすると、アプリケーションは、iTunes 経由でファイルを共有もできます。

この記事では、機能を説明し、iOS の制限の詳細、ファイル システムと Xamarin.iOS を使用して、いくつかの単純なファイル システム操作を実行する方法を示すサンプル アプリケーションが含まれています。

 [![](file-system-images/05-sampleapp.png "一部の単純なファイル システム操作を実行する iOS のサンプル")](file-system-images/05-sampleapp.png#lightbox)

 <a name="General_File_Access" />

## <a name="general-file-access"></a>一般的なファイル アクセス

Xamarin.iOS では、.NET を使用できます。 `System.IO` iOS でのファイル システム操作用のクラス。

次のコード スニペットでは、いくつかの一般的なファイル操作について説明します。 見つかりますそれらすべての下に、`SampleCode.cs`この記事のサンプル アプリケーションでのファイル。

<a name="Working_with_directories" />

### <a name="working-with-directories"></a>ディレクトリの操作

このコードは、現在のディレクトリにサブディレクトリを列挙します (で指定された、"./"パラメーター)、これは、アプリケーションの実行可能ファイルの場所。
出力は、すべてのファイルとは (デバッグ中は、コンソール ウィンドウに表示されます)、アプリケーションと共に配置されるフォルダーの一覧になります。

```csharp
var directories = Directory.EnumerateDirectories("./");
foreach (var directory in directories) {
      Console.WriteLine(directory);
}
```

 <a name="Reading_files" />


### <a name="reading-files"></a>ファイルの読み取り

テキスト ファイルを読み取り、1 行のコードだけでかまいません。 この例では、アプリケーション出力 ウィンドウで、テキスト ファイルの内容が表示されます。

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

MSDN ドキュメントを参照してください、 [System.Xml](http://msdn.microsoft.com/library/system.xml.aspx)名前空間の詳細については[シリアル化](http://msdn.microsoft.com/library/system.xml.serialization.aspx)します。 参照してください、 [Xamarin.iOS ドキュメント](~/ios/deploy-test/linker.md)– リンカーで通常必要がありますを追加する、`[Preserve]`属性をシリアル化するクラス。

 <a name="Creating_Files_and_Directories" />


### <a name="creating-files-and-directories"></a>ファイルとディレクトリを作成します。

このサンプルは、使用する方法を示します、`Environment`ファイルとディレクトリを作成します [ドキュメント] フォルダーにアクセスするクラス。

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

System.IO 名前空間の詳細については、次を参照してください。、 [MSDN ドキュメント](http://msdn.microsoft.com/library/system.io.aspx)します。


### <a name="serializing-json"></a>Json のシリアル化

Json を操作する Xamarin.iOS アプリケーションのデータは非常に簡単を使用して、 [Json.NET](http://www.newtonsoft.com/json) for .NET NuGet パッケージの高性能な JSON フレームワーク。 単に、アプリケーションのプロジェクトに NuGet パッケージを追加します。 

[![](file-system-images/json01.png "アプリケーション プロジェクトに NuGet パッケージを追加します。")](file-system-images/json01.png#lightbox)

次に、シリアル化/逆シリアル化のデータ モデルとして機能するクラスを追加 (この場合`Account.cs`)。

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

最後のインスタンスを作成、`Account`クラス、json データにシリアル化、およびファイルへの書き込み。

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

Xamarin.iOS と .NET のファイル操作、iOS、および Xamarin.iOS 間の類似点もいくつかの重要な点で .NET から異なります。

 <a name="runtimeaccessible" />


### <a name="making-project-files-accessible-at-runtime"></a>プロジェクト ファイルを実行時にアクセスできるようにします。

既定では、ファイルをプロジェクトに追加する場合は、最終的なアセンブリに含まれませんし、そのため、アプリケーションで使用できません。 アセンブリのファイルを含めるためには、コンテンツと呼ばれる特殊なビルド アクションを持つマークする必要があります。

含めるファイルをマークするには、ファイルを右クリックし、選択**ビルド アクション&gt;コンテンツ**Visual studio for mac。 変更することも、**ビルド アクション**ファイルの**プロパティ**シート。

 <a name="Case_Sensitivity" />


### <a name="case-sensitivity"></a>大文字と小文字の区別

IOS のファイル システムがあることを理解することが重要*大文字小文字が区別*します。 これは、ファイルとディレクトリの名前を正確に一致する必要があります – README.txt および readme.txt と思われるさまざまなファイル名のことを意味します。

これは、Windows ファイル システムに精通している .NET 開発者の混乱を招く可能性があります*大文字と小文字を区別しない*–"Files"、"FILES"および"files"のすべてが同じディレクトリを参照しています。

そのためは iOS デバイスは大文字小文字を区別してを考慮するいると、コードを記述する必要があります、iOS シミュレーターは、ない場合区別既定でです。 つまり、ファイル名の大文字と小文字は、ファイルとコードへの参照の間で異なれば場合で、コードがシミュレーターで引き続き動作が、実際のデバイスでコマンドは失敗します。 これは、初期および iOS の開発時に多くの場合、実際のデバイスに展開する重要な理由のいずれかです。

 <a name="Path_Separator" />


### <a name="path-separator"></a>パスの区切り文字

iOS は、スラッシュを使用してパスの区切り文字として '/' (とは異なる Windows で、円記号を使用して ' \')。

使用することをお勧めはこの混乱を招くの違いにより、`System.IO.Path.Combine`メソッドで、特定のパス区切り記号をハードコーディングするのではなく、現在のプラットフォームに調整します。 これは、その他のプラットフォームにより、コードを移植性を高くする簡単な手順です。

 <a name="Application_Sandbox" />


## <a name="application-sandbox"></a>アプリケーションのサンド ボックス

ファイル システム (およびその他のリソース、ネットワークとハードウェアの機能など) へのアプリケーションのアクセスは、セキュリティ上の理由から制限されています。 この制限と呼ばれる、*アプリケーションのサンド ボックス*します。 ファイル システムの観点から、アプリケーションでは、作成およびファイルとそのホーム ディレクトリにディレクトリを削除するのに限定されます。

ホーム ディレクトリでは、一意の場所をアプリケーションとそのすべてのデータが格納されているファイル システムで使用します。 アプリケーションのホーム ディレクトリの場所を選択 (または変更) にすることはできません。ただし iOS と Xamarin.iOS は、プロパティとファイル内のディレクトリを管理するメソッドを提供します。

 <a name="The_Application_Bundle" />


## <a name="the-application-bundle"></a>アプリケーション バンドル

*アプリケーション バンドル*は、アプリケーションを含むフォルダーです。
ディレクトリ名に追加する .app サフィックスにより他のフォルダーから区別されます。 アプリケーション バンドルには、すべてのコンテンツ (ファイル、画像など)、プロジェクトのために必要なし、実行可能ファイルが含まれています。

他のディレクトリ内を参照してください (と .app サフィックスが非表示) よりも、別のアイコンに表示されます、Mac os、アプリケーション バンドルを参照するときただし、オペレーティング システムが異なる方法で表示されている通常のディレクトリだけを勧めします。

Mac と選択 Visual Studio でプロジェクトを右クリック サンプル コードについては、アプリケーション バンドルを表示する**を含むフォルダーを開く**します。 移動し、 **bin/デバッグ/** を検索する場所、アプリケーション アイコン (次のスクリーン ショットに類似)。

 [![](file-system-images/40-bundle.png "このスクリーン ショットのようなアプリケーション アイコンを検索するには、Bin/debug に移動します")](file-system-images/40-bundle.png#lightbox)

このアイコンを右クリックし、選択**パッケージの内容を表示**アプリケーション バンドル ディレクトリの内容を参照します。 次のように通常のディレクトリの内容と同じように内容が表示されます。

 [![](file-system-images/45-bundle.png "アプリ バンドルのコンテンツ")](file-system-images/45-bundle.png#lightbox)

アプリケーション バンドルは、テスト中に、シミュレーターまたはデバイスをインストールされている内容と、何が App Store に含めることを Apple に送信される結局のところは。

 <a name="Application_Directories" />


## <a name="application-directories"></a>アプリケーション ディレクトリ

デバイスをアプリケーションをインストールしているオペレーティング システムはそのホーム ディレクトリを作成し、内で、アプリケーション バンドルを配置します。 Nothing に書き込むか、そのルート ディレクトリが署名されていると、変更は、アプリケーションを無効にして起動することを防ぐことは、コードは、データを読み取るアプリケーション バンドルをアクセスできます。

ルート ディレクトリに何も書き込まれませんする必要がありますが、その<b>iOS 7 およびそれ以前で</b>さまざまな使用可能なアプリケーションのルート ディレクトリ内のディレクトリを作成します。 <b>ユーザーがアクセスできるディレクトリには iOS 8 で<a href="https://developer.apple.com/library/ios/technotes/tn2406/_index.html" target="_blank">に存在しない</a>アプリケーション ルート内</b>します。

これらのディレクトリとその目的は、以下に示します。

&nbsp;

|ディレクトリ|説明|
|---|---|
|[ApplicationName] .app/|**IOS 7 およびそれ以前で**これは、`ApplicationBundle`アプリケーションの実行可能ファイルが格納されているディレクトリ。 アプリで作成するディレクトリ構造は、(たとえば、イメージと、Visual Studio for Mac プロジェクトでリソースとしてマークされているその他のファイルの種類) は、このディレクトリに存在します。<br /><br />このディレクトリへのパスを利用し、アプリケーション バンドル内のコンテンツ ファイルにアクセスする必要がある場合、`NSBundle.MainBundle.BundlePath`プロパティ。|
|ドキュメント/|このディレクトリを使用すると、ユーザーのドキュメントおよびアプリケーション データ ファイルを格納できます。<br /><br />このディレクトリの内容使用できるユーザーに iTunes のファイル (ただし、これは既定で無効です) を共有します。 追加、 `UIFileSharingEnabled` Info.plist ファイルにこれらのファイルへのアクセスを許可するブール値のキー。<br /><br />アプリケーションでは、ファイル共有を有効にするすぐに、場合でも、このディレクトリ内のユーザーから非表示にするファイルを配置することを避ける必要があります (など、データベース ファイルを共有するのでない限り)。 機密性の高いファイルは非表示のまま、限り、これらのファイルいない公開されている (とする可能性のある移動、変更、または iTunes によって削除された) 場合は、今後のバージョンでファイル共有を有効にします。<br /><br /> 使用することができます、`Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments)`アプリケーションの Documents ディレクトリへのパスを取得します。<br /><br />このディレクトリの内容は、iTunes でバックアップされます。|
|ライブラリ/|ライブラリ ディレクトリは、データベースやその他のアプリケーションによって生成されたファイルなど、ユーザーが直接作成されないファイルを格納することをお勧めします。 このディレクトリの内容は iTunes を介してユーザーに公開されることはありません。<br /><br />ライブラリで、独自のサブディレクトリを作成することができます。ただし、あるは既にいくつかシステムで作成されたディレクトリは、ここなどの基本設定とキャッシュの把握しておく必要があります。<br /><br />(キャッシュのサブディレクトリ) を除く、このディレクトリの内容は、iTunes でバックアップされます。 ライブラリで作成したカスタム ディレクトリのバックアップが作成されます。|
|ライブラリの基本/|アプリケーション固有の基本設定ファイルは、このディレクトリに格納されます。 これらのファイルを直接作成しません。 代わりに、使用、`NSUserDefaults`クラス。<br /><br />このディレクトリの内容は、iTunes でバックアップされます。|
|ライブラリ/キャッシュ/|キャッシュ ディレクトリを実行するのに役立つ、アプリケーション データ ファイルを格納する適切な場所が必要な場合は、簡単に再作成をすることができます。 アプリケーションでは、作成、必要に応じて、これらのファイルを削除し、および必要に応じて、これらのファイルを再作成できる必要があります。 iOS 5 は、アプリケーションの実行中に、実行されませんが、非常に低い記憶域の場合)、これらのファイルを削除も可能性があります。<br /><br />このディレクトリの内容は、iTunes、つまり、ユーザー、デバイスを復元する場合はできません、によってバックアップされませんし、アプリケーションの更新バージョンをインストールした後に存在する可能性がありますできません。<br /><br />たとえば、アプリケーションをネットワークに接続できない場合に、データや優れたオフライン エクスペリエンスを提供するファイルを格納するキャッシュ ディレクトリを使用する可能性があります。 アプリケーションは保存でき、ネットワークの応答の待機中にこのデータの取得を迅速にバックアップする必要はありませんとことができます簡単にしたりすることは回復、復元またはバージョンを更新後に再作成します。|
|tmp/|アプリケーションでは、このディレクトリに短い期間にのみ必要な一時ファイルを格納できます。 領域を節約するために必要でなくなったときに、ファイルを削除する必要があります。 オペレーティング システムが、アプリケーションが実行されていない場合、ファイルをこのディレクトリから削除も可能性があります。<br /><br />このディレクトリの内容は、iTunes によってバックアップされません。<br /><br />たとえば、tmp ディレクトリは表示用 (Twitter アバターや電子メールの添付ファイル)、ユーザーにダウンロードしたが、したされて表示 (および将来的に必要な場合、もう一度ダウンロード後にそれらを削除することが、一時ファイルを格納する使用可能性があります。).|

このスクリーン ショットでは、Finder のウィンドウで、ディレクトリ構造を示しています。

 [![](file-system-images/08-library-directory.png "このスクリーン ショットは、Finder のウィンドウで、ディレクトリ構造を示しています")](file-system-images/08-library-directory.png#lightbox)

 <a name="Accessing_Other_Directories_Programmatically" />

### <a name="accessing-other-directories-programmatically"></a>その他のディレクトリにプログラムでアクセスします。

アクセス前のディレクトリとファイルの例、`Documents`ディレクトリ。 使用してパスを構築する必要がありますを別のディレクトリに書き込む、"..."構文を次に示すよう。

```csharp
var documents = Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments);
var library = Path.Combine (documents, "..", "Library");
var filename = Path.Combine (library, "WriteToLibrary.txt");
File.WriteAllText(filename, "Write this text into a file in Library");
```

ディレクトリを作成することはよく似ています。

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


## <a name="sharing-files-with-the-user-through-itunes"></a>ITunes を介してユーザーとファイルを共有

ユーザーを編集して、アプリケーションの Documents ディレクトリ内のファイルにアクセスできます`Info.plist`を作成して、**アプリケーションが iTunes 共有をサポートしている**(`UIFileSharingEnabled`) 内のエントリ、**ソース**ビューとして次に示します。

 [![](file-system-images/09-uifilesharingenabled-plist.png "ITunes 共有プロパティをサポートしているアプリケーションを追加します。")](file-system-images/09-uifilesharingenabled-plist.png#lightbox)

これらのファイルは、デバイスが接続されているし、ユーザーが選択したときに、iTunes でアクセスできる、`Apps`タブ。たとえば、次のスクリーン ショットでは、選択したアプリの iTunes 経由の共有でファイルを示しています。

 [![](file-system-images/10-itunes-file-sharing.png "このスクリーン ショットは、ファイル、選択したアプリの iTunes 経由の共有")](file-system-images/10-itunes-file-sharing.png#lightbox)

ユーザーは、iTunes を使用してこのディレクトリ内の最上位の項目にのみアクセスできます。 (ただし、コンピューターにコピーしたりそれらを削除) は、すべてのサブディレクトリの内容を参照してください、ことはできません。 たとえば、GoodReader で PDF および EPUB ファイルできますように共有するアプリケーションにユーザーが自分の iOS デバイスで読み取ることができます。

ドキュメント フォルダーの内容を変更するユーザーで、注意が必要ない場合、問題が生じる場合があります。 アプリケーションは、これを考慮し、Documents フォルダーの破壊的な更新プログラムを回復できるようにする必要があります。

この記事のサンプル コードは、ドキュメント フォルダーにファイルとフォルダーの両方を作成します。 (で**SampleCode.cs**) ファイルの共有を有効にし、 **Info.plist**ファイル。 このスクリーン ショットは、iTunes でこれらの表示方法を示しています。

 [![](file-system-images/15-itunes-file-sharing-example.png "このスクリーン ショットは、ファイルを iTunes に表示する方法を示しています。")](file-system-images/15-itunes-file-sharing-example.png#lightbox)

参照してください、[イメージを操作](~/ios/app-fundamentals/images-icons/index.md)を作成するカスタム ドキュメントの種類のアプリケーションのアイコンを設定する方法についての情報の記事。

場合、`UIFileSharingEnabled`キーが false または存在しない場合、既定で無効になっている、ファイル共有がし、ユーザーは、Documentsdirectory 対話できません。

 <a name="Backup_and_Restore" />

## <a name="backup-and-restore"></a>バックアップと復元

ITunes でデバイスをバックアップすると、次を除く、アプリケーションのホーム ディレクトリで作成されたすべてのディレクトリが保存されます。

-   **[ApplicationName] .app** – アプリケーション バンドル*は*(アプリケーションの更新プログラムをインストールすると、たとえば) バンドルが変更されたときにのみ、バックアップを取得します。 署名されているため、インストール後に変更する必要がありますされませんので、このディレクトリを変更することはできません。 
-   **ライブラリ/キャッシュ**– 作業ファイルをバックアップする必要がないため、キャッシュ ディレクトリのものです。 
-   **tmp** -このディレクトリが作成され、不要になったときに削除される一時ファイルの使用または領域が必要なときにファイルをその iOS を削除します。 


大量のデータをバックアップすると、長い時間がかかる場合があります。 アプリケーションでは、ドキュメントとライブラリを使用する必要がありますのみ、特定のドキュメントやデータをバックアップする必要がある場合は、このフォルダー。 一時的なデータまたはネットワークから簡単に取得できるファイルの場合、キャッシュまたは tmp ディレクトリのいずれかを使用します。

IOS、'clean'、ファイル システム、デバイスのディスク領域不足実行時に注意してください。 ライブラリ/キャッシュと tmp からすべてのファイルが削除されますが、現在実行されていないアプリケーションのフォルダー。

 <a name="Complying_with_iOS5_iCloud_Backup_Restrictions" />

## <a name="complying-with-ios5-icloud-backup-restrictions"></a>IOS5 iCloud バックアップの制限に準拠します。

導入された Apple *iCloud バックアップ*iOS 5 で機能します。 ICloud バックアップが有効にすると、アプリケーションのホーム ディレクトリ内のすべてのファイル (通常バックアップされないなど、アプリ バンドル ディレクトリを除く`Caches`と`tmp`) はバックアップ サーバーを iCloud にします。 この機能は、自分のデバイスの紛失、盗難、破損している場合に、完全なバックアップを持つユーザーを提供します。

ICloud は、ユーザーごとに 5 Gb の領域の「無償版」を提供するだけでため、帯域幅が不必要に使用を避けるためには、Apple はバックアップ重要なユーザーによって生成されたデータだけにアプリケーションが必要です。 IOS データ ストレージのガイドラインに準拠するには、次の項目に従うことでバックアップされるデータの量を制限する必要があります。

-  ユーザーが生成したデータの場合、またはそれ以外の場合、ドキュメントのディレクトリ (つまりバックアップ) で、再作成できないデータを格納のみです。 
-  簡単に再作成または再ダウンロードその他のデータを格納`Library/Caches`または`tmp`(バックアップがないし、'クリーンアップでした')。 
-  適切な可能性のあるファイルがある場合、`Library/Caches`または`tmp`フォルダーがたくない 'クリーニング' 別の場所に保存 (など`Library/YourData`) を適用し、' バックアップを作成しないを ' ファイルが iCloud バックアップ ba を使用するを防ぐために属性ndwidth とストレージ領域。 慎重に管理し、可能であればそれを削除する必要がありますので、このデータは、デバイス上の領域を引き続き使用します。 

'バックアップを作成しないを' を使用して属性を設定、`NSFileManager`クラス。 クラスのことを確認`using Foundation`を呼び出すと`SetSkipBackupAttribute`次のようにします。

```csharp
var documents = Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments);
var filename = Path.Combine (documents, "LocalOnly.txt");
File.WriteAllText(filename, "This file will never get backed-up. It would need to be re-created after a restore or re-install");
NSFileManager.SetSkipBackupAttribute (filename, true); // backup will be skipped for this file
```

ときに`SetSkipBackupAttribute`は`true`、ファイルはバックアップに格納されているディレクトリに関係なくされません (でも、`Documents`ディレクトリ)。 属性を使用して、クエリを実行することができます、`GetSkipBackupAttribute`メソッド、およびをリセットできますを呼び出して、`SetSkipBackupAttribute`メソッド`false`、次のように。

```csharp
NSFileManager.SetSkipBackupAttribute (filename, false); // file will be backed-up
```

## <a name="sharing-data-between-ios-apps-and-app-extensions"></a>アプリとアプリの拡張機能データの間での iOS の共有

実行されるのでアプリ拡張機能 (包含アプリ) ではなく、ホスト アプリケーションの一部として、データの共有はありません自動含まれるため、余分な作業が必要です。 アプリ グループは、データを共有する別のアプリを許可するメカニズムの iOS を使用です。 アプリケーションは、適切な権利を正しく構成されている、プロビジョニングするには、その標準の iOS のサンド ボックスの外部で共有ディレクトリにアクセスする場合。

### <a name="configure-an-app-group"></a>アプリ グループを構成します。

使用して共有の場所を構成、[アプリ グループ](https://developer.apple.com/library/ios/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19)で構成されており、**証明書, Identifiers & Profiles**セクションで[iOS デベロッパー センター](https://developer.apple.com/devcenter/ios/)します。 この値は、各プロジェクトの参照も必要があります**Entitlements.plist**します。

作成して、アプリ グループを構成するのを参照してください、[アプリ グループ機能](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md)ガイド。

### <a name="files"></a>ファイル

IOS アプリ拡張機能では、一般的なファイル パスを使用してファイルを共有できることも (指定された正しく適切な権限で構成され、プロビジョニングされた)。

```csharp
var FileManager = new NSFileManager ();
var appGroupContainer =FileManager.GetContainerUrl ("group.com.xamarin.WatchSettings");
var appGroupContainerPath = appGroupContainer.Path

Console.WriteLine ("Group Path: " + appGroupContainerPath);

// use the path to create and update files
...
```
> [!IMPORTANT]
> グループのパスが返された場合`null`権利とプロビジョニング プロファイルの構成を確認し、それらが正しいことを確認します。


<a name="Application_Version_Updates" />

## <a name="application-version-updates"></a>アプリケーションのバージョンの更新

アプリケーションの新しいバージョンをダウンロードすると、iOS は新しいホーム ディレクトリを作成し、新しいアプリケーション バンドルを格納します。 iOS では、アプリケーション バンドルの以前のバージョンから、次のフォルダーが、新しいホーム ディレクトリに移動します。

-  **ドキュメント**
-  **Library**


その他のディレクトリも間でコピーし、新しいホーム ディレクトリの下に配置がない保証されているコピーされるため、アプリケーションがこのシステムの動作に依存する必要があります。

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事では、ファイル システム操作が他の任意の .NET アプリケーションと同様の Xamarin.iOS で単純なことを示しました。 また、アプリケーションのサンド ボックスを導入されとセキュリティ上の影響を調べる。 次に、アプリケーション バンドルの概念について説明しました。 最後に、アプリケーションで利用できる特殊なディレクトリを列挙し、アプリケーションのアップグレードおよびバックアップ中にその役割を説明します。


## <a name="related-links"></a>関連リンク

- [FileSystemSampleCode](https://developer.xamarin.com/samples/FileSystemSampleCode/)
- [ファイル システムのプログラミング ガイド](http://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/FileSystemProgrammingGUide/Introduction/Introduction.html)
- [ファイルを登録、アプリケーションがサポートを種類します。](http://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Articles/RegisteringtheFileTypesYourAppSupports.html#/apple_ref/doc/uid/TP40010411-SW1)
