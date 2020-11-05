---
title: Xamarin. iOS でのファイルシステムアクセス
description: このドキュメントでは、Xamarin. iOS でファイルシステムを操作する方法について説明します。 ディレクトリ、ファイルの読み取り、XML と JSON のシリアル化、アプリケーションサンドボックス、iTunes を使用したファイルの共有などについて説明します。
ms.prod: xamarin
ms.assetid: 37DF2F38-901E-8F8E-269A-5EE0CCD28C08
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 11/12/2018
ms.openlocfilehash: ffb49329b38705d097520b24d53285d5dbf15167
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93371808"
---
# <a name="file-system-access-in-xamarinios"></a>Xamarin. iOS でのファイルシステムアクセス

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/ios-samples/filesystemsamplecode)

`System.IO`Ios ファイルシステムにアクセスするには、 *.Net 基本クラスライブラリ (BCL)* の Xamarin およびクラスを使用します。 `File` クラスでは、ファイルを作成し、削除し、読み込むことができます。`Directory` クラスでは、ディレクトリの内容を作成し、削除し、列挙できます。 また、サブクラスを使用することもできます。これにより、ファイルの `Stream` 操作 (圧縮やファイル内の位置検索など) をより細かく制御できます。

iOS では、アプリケーションのデータのセキュリティを維持し、malignant アプリからユーザーを保護するために、ファイルシステムで実行できる処理についていくつかの制限が課されています。 これらの制限は、アプリケーション *サンドボックス* に含まれています。アプリケーションのファイル、基本設定、ネットワークリソース、ハードウェアなどへのアクセスを制限する一連の規則です。アプリケーションでは、ホームディレクトリ (インストール場所) 内のファイルの読み取りと書き込みが制限されています。別のアプリケーションのファイルにアクセスすることはできません。

iOS には、ファイルシステム固有の機能がいくつかあります。一部のディレクトリでは、バックアップとアップグレードに関して特別な処理が必要であり、アプリケーションはファイル (iOS 11 以降) と iTunes を使用し **て、ファイル** を相互に共有することもできます。

この記事では、iOS ファイルシステムの機能と制限について説明します。また、Xamarin を使用していくつかの単純なファイルシステム操作を実行する方法を示すサンプルアプリケーションを紹介します。

[![いくつかの単純なファイルシステム操作を実行する iOS のサンプル](file-system-images/01-sampleapp-sml.png)](file-system-images/01-sampleapp.png#lightbox)

## <a name="general-file-access"></a>一般的なファイルアクセス

Xamarin では、 `System.IO` ios でファイルシステム操作に .net クラスを使用できます。

次のコードスニペットは、一般的なファイル操作を示しています。 これらはすべて、この記事のサンプルアプリケーションの **SampleCode.cs** ファイルに含まれています。

### <a name="working-with-directories"></a>ディレクトリの操作

このコードは、現在のディレクトリ内のサブディレクトリ ("./" パラメーターで指定) を列挙します。これは、アプリケーションの実行可能ファイルの場所です。
出力は、アプリケーションと共に配置されたすべてのファイルとフォルダーの一覧になります (デバッグ中にコンソールウィンドウに表示されます)。

```csharp
var directories = Directory.EnumerateDirectories("./");
foreach (var directory in directories) {
      Console.WriteLine(directory);
}
```

### <a name="reading-files"></a>ファイルの読み取り

テキストファイルを読み取るには、1行のコードだけが必要です。 この例では、[アプリケーション出力] ウィンドウにテキストファイルの内容を表示します。

```csharp
var text = File.ReadAllText("TestData/ReadMe.txt");
Console.WriteLine(text);
```

### <a name="xml-serialization"></a>XML シリアル化

完全な `System.Xml` 名前空間を使用することはこの記事の範囲を超えていますが、次のコードスニペットのような StreamReader を使用して、ファイルシステムから XML ドキュメントを簡単に逆シリアル化できます。

```csharp
using (TextReader reader = new StreamReader("./TestData/test.xml")) {
      XmlSerializer serializer = new XmlSerializer(typeof(MyObject));
      var xml = (MyObject)serializer.Deserialize(reader);
}
```

詳細については、 [System.Xml](xref:System.Xml) と [シリアル化](xref:System.Xml.Serialization)に関するドキュメントを参照してください。 リンカーの [Xamarin. iOS のドキュメント](~/ios/deploy-test/linker.md) を参照してください。多くの場合、 `[Preserve]` シリアル化するクラスに属性を追加する必要があります。

### <a name="creating-files-and-directories"></a>ファイルとディレクトリの作成

このサンプルでは、クラスを使用して `Environment` 、ファイルとディレクトリを作成できる Documents フォルダーにアクセスする方法を示します。

```csharp
var documents =
 Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments); 
var filename = Path.Combine (documents, "Write.txt");
File.WriteAllText(filename, "Write this text into a file");
```

ディレクトリの作成は同様のプロセスです。

```csharp
var documents =
 Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments);
var directoryname = Path.Combine (documents, "NewDirectory");
Directory.CreateDirectory(directoryname);
```

詳細については、 [SYSTEM.IO API リファレンス](xref:System.IO)を参照してください。

### <a name="serializing-json"></a>JSON のシリアル化

[Json.NET](http://www.newtonsoft.com/json) は、Xamarin で動作する高パフォーマンスの Json フレームワークであり、NuGet で利用できます。 Visual Studio for Mac で **nuget を追加** して、nuget パッケージをアプリケーションプロジェクトに追加します。

[![アプリケーションプロジェクトへの NuGet パッケージの追加](file-system-images/json01.png)](file-system-images/json01.png#lightbox)

次に、シリアル化/逆シリアル化のデータモデルとして機能するクラスを追加します (この場合は `Account.cs` )。

```csharp
using System;
using System.Collections.Generic;
using Foundation; // for Preserve attribute, which helps serialization with Linking enabled

namespace FileSystem
{
    [Preserve]
    public class Account
    {
        public string Email { get; set; }
        public bool Active { get; set; }
        public DateTime CreatedDate { get; set; }
        public List<string> Roles { get; set; }

        public Account() {
        }
    }
}
```

最後に、クラスのインスタンスを作成し `Account` 、それを json データにシリアル化してファイルに書き込みます。

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

.NET アプリケーションで json データを操作する方法の詳細については、「Json. NET の [ドキュメント](https://www.newtonsoft.com/json/help)」を参照してください。

## <a name="special-considerations"></a>特別な考慮事項

Xamarin の iOS と .NET のファイル操作 (iOS と Xamarin) の類似性は似ていますが、いくつかの重要な点で .NET とは異なります。

### <a name="making-project-files-accessible-at-runtime"></a>実行時にプロジェクトファイルにアクセスできるようにする

既定では、プロジェクトにファイルを追加しても、最終的なアセンブリには含まれないため、アプリケーションでは使用できません。 アセンブリにファイルを含めるには、コンテンツと呼ばれる特別なビルドアクションを使用してファイルをマークする必要があります。

ファイルを含めるようにマークするには、ファイルを右クリックし、Visual Studio for Mac で [ **アクション &gt; コンテンツのビルド** ] を選択します。 また、ファイルの **プロパティ** シートで **ビルドアクション** を変更することもできます。

### <a name="case-sensitivity"></a>大文字小文字の区別

IOS ファイルシステムでは *大文字と小文字が区別* されることを理解しておくことが重要です。 大文字と小文字の区別とは、ファイル名とディレクトリ名が **README.txt** 完全に一致する必要があることを意味し、 **readme.txt** は異なるファイル名と見なされます。

これは、Windows ファイルシステムに慣れている .NET 開発者が、 **ファイル** 、 **ファイル** 、 **ファイル** がすべて同じディレクトリを参照している *場合* に、混乱を招く可能性があります。

> [!WARNING]
> IOS シミュレーターでは、大文字と小文字は区別されません。
> ファイル名の大文字と小文字の区別がファイル自体とコード内での参照の間で異なる場合は、コードがシミュレーターで動作しても、実際のデバイスでは失敗します。 これは、iOS の開発中に、通常のデバイスでのデプロイとテストが重要な理由の1つです。

### <a name="path-separator"></a>パスの区切り

iOS では、パスの区切り記号としてスラッシュ '/' が使用されます (これは、円記号 ' \ ' を使用する Windows とは異なります)。

このような違いがあるため、 `System.IO.Path.Combine` 特定のパス区切り記号をハードコーディングするのではなく、現在のプラットフォームに合わせて調整するメソッドを使用することをお勧めします。 これは、コードを他のプラットフォームに移植できるようにするための簡単な手順です。

## <a name="application-sandbox"></a>アプリケーションサンドボックス

ファイルシステムへのアプリケーションのアクセス (およびネットワークやハードウェアの機能などの他のリソース) は、セキュリティ上の理由で制限されています。 この制限は、 *アプリケーションサンドボックス* と呼ばれます。 ファイルシステムに関しては、アプリケーションはホームディレクトリ内のファイルとディレクトリの作成と削除に限定されます。

ホームディレクトリは、アプリケーションとそのすべてのデータが格納されるファイルシステム内の一意の場所です。 アプリケーションのホームディレクトリの場所を選択 (または変更) することはできません。ただし、iOS と Xamarin には、内のファイルとディレクトリを管理するためのプロパティとメソッドが用意されています。

## <a name="the-application-bundle"></a>アプリケーションバンドル

アプリケーション *バンドル* は、アプリケーションが含まれているフォルダーです。
他のフォルダーと区別するには、ディレクトリ名に. app サフィックスを追加します。 アプリケーションバンドルには、実行可能ファイルと、プロジェクトに必要なすべてのコンテンツ (ファイルやイメージなど) が含まれています。

Mac OS でアプリケーションバンドルを参照すると、他のディレクトリに表示されるものとは異なるアイコンが表示され **ます** (と、アプリのサフィックスは非表示になります)。ただし、これはオペレーティングシステムの表示が異なる通常のディレクトリにすぎません。

サンプルコードのアプリケーションバンドルを表示するには、 **Visual Studio for Mac** でプロジェクトを右クリックし、[ **Finder で** 表示] を選択します。 次に、アプリケーションアイコンがある **bin/** ディレクトリ (次のスクリーンショットのように) に移動します。

![Bin ディレクトリ内を移動して、次のスクリーンショットのようなアプリケーションアイコンを見つけます。](file-system-images/40-bundle.png)

このアイコンを右クリックし、[ **パッケージの内容の表示** ] を選択して、アプリケーションバンドルディレクトリの内容を参照します。 次に示すように、コンテンツは通常のディレクトリの内容と同様に表示されます。

[![アプリバンドルの内容](file-system-images/45-bundle-sml.png)](file-system-images/45-bundle.png#lightbox)

アプリケーションバンドルは、テスト中にシミュレーターまたはデバイスにインストールされるものです。最終的に、アプリストアに含まれるように Apple に送信されます。

## <a name="application-directories"></a>アプリケーションディレクトリ

アプリケーションがデバイスにインストールされると、オペレーティングシステムによってアプリケーションのホームディレクトリが作成され、アプリケーションのルートディレクトリ内に、使用可能な多数のディレクトリが作成されます。 IOS 8 以降では、ユーザーがアクセスできるディレクトリはアプリケーションルート内に配置されて [いない](https://developer.apple.com/library/ios/technotes/tn2406/_index.html) ため、ユーザーディレクトリからアプリケーションバンドルのパスを派生させることはできません。また、その逆も可能です。

これらのディレクトリ、パスを決定する方法、およびその目的を次に示します。

&nbsp;

|ディレクトリ|説明|
|---|---|
|[ApplicationName]。アプリ/|**IOS 7 以前で** は、これは `ApplicationBundle` アプリケーションの実行可能ファイルが格納されているディレクトリです。 アプリで作成したディレクトリ構造は、このディレクトリに存在します (たとえば、Visual Studio for Mac プロジェクトでリソースとしてマークしたイメージやその他のファイルの種類など)。<br /><br />アプリケーションバンドル内のコンテンツファイルにアクセスする必要がある場合は、プロパティを介してこのディレクトリへのパスを使用でき `NSBundle.MainBundle.BundlePath` ます。|
|ドキュメント|ユーザードキュメントとアプリケーションデータファイルを格納するには、このディレクトリを使用します。<br /><br />このディレクトリの内容は、iTunes ファイル共有を通じてユーザーが使用できるようにすることができます (ただし、既定では無効になっています)。 `UIFileSharingEnabled`ユーザーがこれらのファイルにアクセスできるようにするには、情報 plist ファイルにブールキーを追加します。<br /><br />アプリケーションでファイル共有をすぐに有効にしない場合でも、このディレクトリ (共有する予定がない限り、データベースファイルなど) のユーザーに対して非表示にする必要があるファイルは配置しないようにしてください。 機密ファイルが非表示のままであれば、将来のバージョンでファイル共有が有効になっていると、これらのファイルは公開されません (また、iTunes によって移動、変更、または削除される可能性があります)。<br /><br /> メソッドを使用して、 `Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments)` アプリケーションの Documents ディレクトリへのパスを取得できます。<br /><br />このディレクトリの内容は、iTunes によってバックアップされます。|
|ライブラリ|ライブラリディレクトリは、データベースやその他のアプリケーションで生成されたファイルなど、ユーザーによって直接作成されていないファイルを格納するのに適した場所です。 このディレクトリの内容は、iTunes を介してユーザーに公開されることはありません。<br /><br />独自のサブディレクトリをライブラリに作成することができます。ただし、ここでは、ユーザー設定やキャッシュなど、システムによって作成されたディレクトリがいくつか存在します。<br /><br />このディレクトリの内容 (キャッシュサブディレクトリを除く) は、iTunes によってバックアップされます。 ライブラリに作成したカスタムディレクトリがバックアップされます。|
|ライブラリ/設定/|アプリケーション固有の基本設定ファイルは、このディレクトリに格納されます。 これらのファイルは直接作成しないでください。 代わりに、クラスを使用し `NSUserDefaults` ます。<br /><br />このディレクトリの内容は、iTunes によってバックアップされます。|
|ライブラリ/キャッシュ/|キャッシュディレクトリは、アプリケーションの実行に役立つデータファイルを格納するのに適した場所ですが、簡単に再作成することができます。 アプリケーションでは、必要に応じてこれらのファイルを作成および削除する必要があり、必要に応じてこれらのファイルを再作成することができます。 iOS 5 では、これらのファイル (記憶域が不足している場合) も削除される場合がありますが、アプリケーションの実行中は削除されません。<br /><br />このディレクトリの内容は iTunes によってバックアップされません。つまり、ユーザーがデバイスを復元しても、更新されたバージョンのアプリケーションがインストールされた後には存在しない可能性があることを意味します。<br /><br />たとえば、アプリケーションがネットワークに接続できない場合は、キャッシュディレクトリを使用してデータまたはファイルを格納し、適切なオフラインエクスペリエンスを実現することができます。 このアプリケーションでは、ネットワーク応答の待機中にこのデータをすばやく保存して取得できますが、バックアップする必要はなく、復元やバージョンの更新後に簡単に回復または再作成することができます。|
|tmp|アプリケーションは、このディレクトリ内の短時間のみ必要な一時ファイルを格納できます。 領域を節約するには、不要になったファイルを削除する必要があります。 オペレーティングシステムは、アプリケーションが実行されていない場合に、このディレクトリからファイルを削除することもあります。<br /><br />このディレクトリの内容は、iTunes によってバックアップされません。<br /><br />たとえば、tmp ディレクトリは、ユーザーに表示するためにダウンロードされる一時ファイル (Twitter アバターや電子メールの添付ファイルなど) を格納するために使用されますが、表示された後に削除される可能性があります (後で必要になった場合は、再度ダウンロードされます)。|

このスクリーンショットは、Finder ウィンドウのディレクトリ構造を示しています。

[![このスクリーンショットは、Finder ウィンドウのディレクトリ構造を示しています。](file-system-images/08-library-directory.png)](file-system-images/08-library-directory.png#lightbox)

### <a name="accessing-other-directories-programmatically"></a>プログラムによる他のディレクトリへのアクセス

ディレクトリにアクセスする前のディレクトリとファイルの例を示し `Documents` ます。 別のディレクトリに書き込むには、次に示すように ".." 構文を使用してパスを作成する必要があります。

```csharp
var documents = Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments);
var library = Path.Combine (documents, "..", "Library");
var filename = Path.Combine (library, "WriteToLibrary.txt");
File.WriteAllText(filename, "Write this text into a file in Library");
```

ディレクトリの作成も同様です。

```csharp
var documents = Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments);
var library = Path.Combine (documents, "..", "Library");
var directoryname = Path.Combine (library, "NewLibraryDirectory");
Directory.CreateDirectory(directoryname);
```

ディレクトリとディレクトリへのパスは、次 `Caches` の `tmp` ように構築できます。

```csharp
var documents = Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments);
var cache = Path.Combine (documents, "..", "Library", "Caches");
var tmp = Path.Combine (documents, "..", "tmp");
```

## <a name="sharing-with-the-files-app"></a>ファイルアプリと共有する

iOS 11 **では、ファイルアプリが** 導入されました。これは、ユーザーが iCloud でファイルを表示して操作したり、それをサポートするアプリケーションによって保存したりできるようにする、ios 用のファイルブラウザーです。 ユーザーがアプリ内のファイルに直接アクセスできるようにするには、次のように、ファイルに新しいブール **値のキー** を作成 `LSSupportsOpeningDocumentsInPlace` し、をに設定し `true` ます。

![LSSupportsOpeningDocumentsInPlace の設定](file-system-images/51-supports-opening.png)

これで、アプリの **Documents** ディレクトリが **Files** アプリで閲覧できるようになります。 **ファイル** アプリで、 **[マイ iPhone] の** に移動します。共有ファイルがある各アプリが表示されます。 次のスクリーンショットは、 [FileSystem サンプルアプリ](/samples/xamarin/ios-samples/filesystemsamplecode) の外観を示しています。

![iOS 11 ファイルアプリ](file-system-images/50-files-app-1-sml.png) ![IPhone ファイルを参照する](file-system-images/50-files-app-2-sml.png) ![サンプルアプリファイル](file-system-images/50-files-app-3-sml.png)

## <a name="sharing-files-with-the-user-through-itunes"></a>ITunes を使用してユーザーとファイルを共有する

ユーザーは、 `Info.plist` 次に示すように、ソースビューで iTunes 共有 () エントリを **サポートするアプリケーション** を編集して作成することによって、アプリケーションの Documents ディレクトリ内のファイルにアクセスでき `UIFileSharingEnabled` ます。 **Source**

[![アプリケーションの追加で iTunes 共有プロパティがサポートされる](file-system-images/09-uifilesharingenabled-plist-sml.png)](file-system-images/09-uifilesharingenabled-plist.png#lightbox)

これらのファイルには、デバイスが接続されていて、ユーザーがタブを選択すると、iTunes でアクセスでき `Apps` ます。たとえば、次のスクリーンショットは、選択したアプリ内のファイルが iTunes 経由で共有されていることを示しています。

[![このスクリーンショットは、選択したアプリ内のファイルが iTunes 経由で共有されていることを示します](file-system-images/10-itunes-file-sharing-sml.png)](file-system-images/10-itunes-file-sharing.png#lightbox)

ユーザーは、iTunes を使用して、このディレクトリ内の最上位レベルの項目にのみアクセスできます。 サブディレクトリの内容を表示することはできません (ただし、それらをコンピューターにコピーしたり、削除したりすることはできます)。 たとえば、GoodReader を使用すると、PDF ファイルと EPUB ファイルをアプリケーションと共有して、ユーザーが自分の iOS デバイスで読み取ることができるようにすることができます。

ドキュメントフォルダーの内容を変更すると、ユーザーが注意を払っていない場合、問題が発生する可能性があります。 アプリケーションはこのことを考慮し、ドキュメントフォルダーの破壊的な更新に対して回復力を持つ必要があります。

この記事のサンプルコードでは、Documents フォルダー ( **SampleCode.cs** ) にファイルとフォルダーの両方を作成し、ファイル共有を有効にします **。** このスクリーンショットは、iTunes でこれらがどのように表示されるかを示しています。

[![このスクリーンショットは、iTunes でファイルがどのように表示されるかを示しています。](file-system-images/15-itunes-file-sharing-example-sml.png)](file-system-images/15-itunes-file-sharing-example.png#lightbox)

アプリケーションのアイコンを設定する方法と、作成するカスタムドキュメントの種類については、「 [イメージの操作](~/ios/app-fundamentals/images-icons/index.md) 」を参照してください。

`UIFileSharingEnabled`キーが false の場合、または存在しない場合、ファイル共有は既定で無効になり、ユーザーはドキュメントディレクトリと対話できません。

## <a name="backup-and-restore"></a>バックアップと復元

デバイスが iTunes によってバックアップされると、アプリケーションのホームディレクトリに作成されたすべてのディレクトリが保存されます。ただし、次のディレクトリは除きます。

- **[ApplicationName]。アプリ** –署名されているため、このディレクトリには書き込みません。そのため、インストール後に変更を残しておく必要があります。 このファイルには、コードからアクセスするリソースが含まれている場合がありますが、アプリを再ダウンロードすることによって復元されるため、バックアップは必要ありません。
- **ライブラリ/キャッシュ** -キャッシュディレクトリは、バックアップする必要のない作業ファイルを対象としています。
- **tmp** –このディレクトリは、不要になったときに作成および削除された一時ファイルや、容量が必要なときに iOS が削除するファイルに使用されます。

大量のデータをバックアップすると、長い時間がかかることがあります。 特定のドキュメントまたはデータをバックアップする必要がある場合は、アプリケーションで [ドキュメントとライブラリ] フォルダーのいずれかを使用する必要があります。 ネットワークから簡単に取得できる一時的なデータまたはファイルの場合は、キャッシュまたは tmp ディレクトリを使用します。

> [!NOTE]
> iOS では、デバイスのディスク領域が非常に少なくなったときに、ファイルシステムが "クリーン" されます。
> このプロセスでは、現在実行されていないアプリケーションのライブラリ/キャッシュおよび tmp フォルダーからすべてのファイルが削除されます。

## <a name="complying-with-ios-5-icloud-backup-restrictions"></a>IOS 5 iCloud のバックアップ制限への準拠

> [!NOTE]
> このポリシーは最初に iOS 5 で導入されていましたが (これはかなり前のように見えます)、現在もアプリに関連しています。

Apple では、iOS 5 で *ICloud バックアップ* 機能が導入されました。 ICloud バックアップが有効になっている場合、アプリケーションのホームディレクトリ内のすべてのファイル (通常はバックアップされていないディレクトリ (アプリバンドル、 `Caches` 、など `tmp` ) は icloud サーバーにバックアップされます。 この機能により、デバイスが紛失、盗難、または破損した場合に備えて、完全なバックアップをユーザーに提供できます。

ICloud では、各ユーザーに 5 Gb の空き領域を提供するだけで、帯域幅を不必要に使用しないようにするために、アプリケーションではユーザーが基本的に生成したデータのみをバックアップすることを想定しています。 IOS データストレージのガイドラインに準拠するには、次の項目に従うことで、バックアップされるデータの量を制限する必要があります。

- ユーザーが生成したデータまたは再作成できないデータだけを Documents ディレクトリ (バックアップ対象) に格納します。
- またはで簡単に再作成または再ダウンロードできるその他のデータを格納し `Library/Caches` `tmp` ます (バックアップされていない、"クリーニング済み" の場合があります)。
- フォルダーまたはフォルダーに適切なファイルがあっ `Library/Caches` て `tmp` も、"クリーニング" したくない場合は、ファイルを別の場所 (など) に保存して、 `Library/YourData` "バックアップしない" 属性を適用して、ファイルが iCloud バックアップ帯域幅と記憶域スペースを使用しないようにします。 このデータはデバイスの空き領域を維持するため、慎重に管理し、可能な場合は削除する必要があります。

"バックアップしない" 属性は、クラスを使用して設定され `NSFileManager` ます。 クラスがであること `using Foundation` を確認し、次のようにを呼び出し `SetSkipBackupAttribute` ます。

```csharp
var documents = Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments);
var filename = Path.Combine (documents, "LocalOnly.txt");
File.WriteAllText(filename, "This file will never get backed-up. It would need to be re-created after a restore or re-install");
NSFileManager.SetSkipBackupAttribute (filename, true); // backup will be skipped for this file
```

がの場合、ファイルは、ディレクトリに格納されている `SetSkipBackupAttribute` `true` ディレクトリに関係なく、バックアップされません `Documents` 。 メソッドを使用して属性に対してクエリを実行でき `GetSkipBackupAttribute` ます。また、次のようにを使用してメソッドを呼び出すことによって、この属性をリセットでき `SetSkipBackupAttribute` `false` ます。

```csharp
NSFileManager.SetSkipBackupAttribute (filename, false); // file will be backed-up
```

## <a name="sharing-data-between-ios-apps-and-app-extensions"></a>IOS アプリとアプリ拡張機能間でのデータの共有

アプリ拡張機能はホストアプリケーションの一部として (それを含むアプリではなく) 実行されるため、データの共有は自動的に含まれないため、余分な作業が必要になります。 アプリグループは、さまざまなアプリでデータを共有するために iOS が使用するメカニズムです。 アプリケーションが正しい権利とプロビジョニングで適切に構成されている場合は、通常の iOS サンドボックスの外部にある共有ディレクトリにアクセスできます。

### <a name="configure-an-app-group"></a>アプリグループを構成する

共有の場所は、 [IOS デベロッパーセンター](https://developer.apple.com/devcenter/ios/)の「 **証明書、識別子 & プロファイル** 」セクションで構成されている [アプリグループ](https://developer.apple.com/library/archive/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19)を使用して構成されます。 この値は、各プロジェクトの権利でも参照される必要があります。 **plist** です。

アプリグループの作成と構成の詳細については、 [アプリグループの機能](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) に関するガイドを参照してください。

### <a name="files"></a>Files

IOS アプリと拡張機能は、共通のファイルパスを使用してファイルを共有することもできます (正しい権利とプロビジョニングで適切に構成されている場合)。

```csharp
var FileManager = new NSFileManager ();
var appGroupContainer =FileManager.GetContainerUrl ("group.com.xamarin.WatchSettings");
var appGroupContainerPath = appGroupContainer.Path

Console.WriteLine ("Group Path: " + appGroupContainerPath);

// use the path to create and update files
...
```

> [!IMPORTANT]
> 返されるグループパスがの場合は、 `null` 権利とプロビジョニングプロファイルの構成を確認し、正しいことを確認します。

## <a name="application-version-updates"></a>アプリケーションのバージョンの更新

アプリケーションの新しいバージョンがダウンロードされると、iOS は新しいホームディレクトリを作成し、そのディレクトリに新しいアプリケーションバンドルを格納します。 iOS は、以前のバージョンのアプリケーションバンドルから、次のフォルダーを新しいホームディレクトリに移動します。

- **ドキュメント**
- **Library**

他のディレクトリはコピーして新しいホームディレクトリに配置することもできますが、コピーされることは保証されないため、アプリケーションはこのシステムの動作に依存しないようにする必要があります。

## <a name="summary"></a>まとめ

この記事では、Xamarin を使用したファイルシステム操作が他の .NET アプリケーションと似ていることを示しました。 また、アプリケーションサンドボックスを導入し、その原因となったセキュリティへの影響を調べました。 次に、アプリケーションバンドルの概念について説明します。 最後に、アプリケーションで使用可能な特化されたディレクトリを列挙し、アプリケーションのアップグレード時およびバックアップ時にそのロールを説明しました。

## <a name="related-links"></a>関連リンク

- [FileSystem サンプルコード](/samples/xamarin/ios-samples/filesystemsamplecode)
- [ファイルシステムプログラミングガイド](https://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/FileSystemProgrammingGUide/Introduction/Introduction.html)
- [アプリでサポートされているファイルの種類を登録しています](https://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Articles/RegisteringtheFileTypesYourAppSupports.html#/apple_ref/doc/uid/TP40010411-SW1)