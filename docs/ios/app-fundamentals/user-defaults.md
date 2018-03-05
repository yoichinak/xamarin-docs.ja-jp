---
title: "ユーザーの既定値の操作"
description: "この記事では、Xamarin の iOS アプリまたは拡張機能に既定の設定を保存する NSUserDefault の扱いについて説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: DAE7FFC4-B8C9-4D9E-886A-9B2388452EEB
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/07/2016
ms.openlocfilehash: c4b2a103821bb18da4878cd37335faa899e910be
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="working-with-user-defaults"></a>ユーザーの既定値の操作

_この記事では、Xamarin の iOS アプリまたは拡張機能に既定の設定を保存する NSUserDefault の扱いについて説明します。_


`NSUserDefaults`クラスは、ios アプリと拡張機能は、システム全体の既定のシステムとの対話方法を提供します。 既定でシステムを使用すると、ユーザーはアプリの動作または (アプリのデザインに基づく) の設定に応じたスタイル設定を構成できます。 たとえば、ヤード メトリックの vs でのデータを表示または指定した UI のテーマを選択します。

ときにアプリ グループを使用する`NSUserDefaults`も、特定のグループ内でのアプリ (または拡張機能) の間の通信する方法を提供します。

<a name="About-User-Defaults" />

## <a name="about-user-defaults"></a>ユーザーの既定値について

上記のとおり、ユーザーの既定値 (`NSUserDefaults`) アプリ (または拡張機能) に追加され、エンドユーザーは、外観や実行時にアプリの動作を調整する変更できる構成可能なオプションを提供するために使用できます。

アプリが最初に実行されるときに`NSUserDefaults`アプリのユーザーの既定値はデータベースからのキーと値を読み取り、値が必要を開くと、毎回データベースの読み取りを回避するメモリにキャッシュに保持します。 

> [!IMPORTANT]
> **注**: Apple が不要になった、開発者が呼び出すことを推奨、`Synchronize`メソッドを直接データベースにメモリ内キャッシュを同期します。 代わりに、自動的に呼び出されます、メモリ内キャッシュの同期を保つ、ユーザーの既定のデータベースに定期的にします。

`NSUserDefaults`クラスには読み書きに基本設定の値の一般的なデータ型などのいくつかの便利なメソッドが含まれています: 文字列、整数、浮動小数点、ブール値および Url。 使用して他の種類のデータをアーカイブする`NSData`からの読み取りまたはユーザーの既定値はデータベースに書き込まれます。 詳細については、Apple を参照してください[好みや設定プログラミング ガイド](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/UserDefaults/Introduction/Introduction.html#//apple_ref/doc/uid/10000059i)です。

<a name="Accessing-the-Shared-NSUserDefaults-Instance" />

## <a name="accessing-the-shared-nsuserdefaults-instance"></a>共有 NSUserDefaults インスタンスにアクセスします。 

ユーザーの既定の共有インスタンスは、デバイスの現在のユーザーのユーザーの既定値にアクセスを提供します。 既定値は共有オブジェクトが存在しない場合は、初めてがアクセスして、次の情報を使用して初期化が作成されます。

- `NSArgumentDomain`の現在のアプリから解析の既定値で構成されます。
- アプリのバンドル Id ドメイン。
- `NSGlobalDomain`のすべてのアプリで共有される、既定の設定で構成されます。
- 各ユーザーのドメインを別の言語を優先します。
- `NSRegistationDomain`をアプリに検索は、常に成功したことを確認して変更できる一時的な既定値のセットを使用します。

ユーザーの既定の共有インスタンスにアクセスするには、次のコードを使用します。

```csharp
// Get Shared User Defaults
var plist = NSUserDefaults.StandardUserDefaults;
```

<a name="Accessing-an-App-Group-NSUserDefaults-Instance" />

## <a name="accessing-an-app-group-nsuserdefaults-instance"></a>アプリ グループ NSUserDefaults インスタンスにアクセスします。

アプリ グループを使用して前に、述べたよう`NSUserDefaults`特定のグループ内でのアプリ (または拡張機能) 間の通信に使用できます。 アプリ グループと必要なアプリ Id が構成されている正しくのことを確認する必要が最初に、**証明書、識別子、およびプロファイル**セクションで[iOS Dev Center](https://developer.apple.com/devcenter/ios/)がインストールされていると開発環境でします。

アプリや拡張機能プロジェクトは、上記で作成した有効なアプリ Id のいずれかを指定する必要があります次を`Entitlements.plist`ファイルに有効になっているし、指定されたアプリ グループがあり、アプリ バンドルに含まれるので取得します。

このすべての共有アプリ グループ ユーザーの既定値を次のコードを使用してアクセスできます。

```csharp
// Get App Group User Defaults
var plist = new NSUserDefaults ("group.com.xamarin.todaysharing", NSUserDefaultsType.SuiteName);
```

ここで`group.com.xamarin.todaysharing`でアプリ グループを作成**証明書、識別子、およびプロファイル**にアクセスすることです。 詳細についてを参照してください、[アプリ グループ機能](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md)ドキュメント。

<a name="Reading-Default-Values" />

## <a name="reading-default-values"></a>既定値を読み取る

必要なユーザーの既定のデータベースにアクセスした後は、キーと値のペアと読み取られるデータの種類に基づいて、いくつかの便利なメソッドを使用して既定値から値を読み取ることができます。

- `ArrayForKey` 配列を返します`NSObjects`指定のキー値。
- `BoolForKey` -指定したキーのブール値を返します。
- `DataForKey` 返します、`NSData`指定したキーのオブジェクト。
- `DictionaryForKey` 返します、`NSDictionary`指定したキーのです。
- `DoubleForKey` -指定したキーの double 値を返します。
- `FloatForKey` -指定したキーの浮動小数点値を返します。
- `IntForKey` -指定したキーの整数値を返します。
- `StringArrayForKey` 配列を返します`String`指定されたキー値からのオブジェクト。
- `StringForKey` -指定したキーの文字列値を返します。
- `URLForKey` 返します、`NSUrl`指定したキーの値。

たとえば、次のコードでは、ユーザーの既定値からブール値を読み取るは。

```csharp
// Get Shared User Defaults
var plist = NSUserDefaults.StandardUserDefaults;
...

// Get value
var useHeader = plist.BoolForKey("UseHeader");

```

<a name="Writing-Default-Values" />

## <a name="writing-default-values"></a>既定値を書き込んでいます

上記の値を読み取りと同じように、必要なユーザー既定のデータベースにアクセスした後に記述できます値をキーと値のペアと書き込まれるデータの種類に基づいて、いくつかの便利なメソッドを使用して既定値。

- `SetBool` -指定したキーに指定したブール値を書き込みます。
- `SetDouble` -指定したキーを特定の double 値を書き込みます。
- `SetFloat` -指定したキーに指定された浮動小数点値を書き込みます。
- `SetString` -指定したキーに指定された文字列値を書き込みます。
- `SetURL` -指定された URL を書き込みます (`NSUrl`) 値を指定したキー。

たとえば、次のコードでは、ユーザーの既定値をブール値は記述します。

```csharp
// Get Shared User Defaults
var plist = NSUserDefaults.StandardUserDefaults;
...

// Save value
plist.SetBool(useHeader, "UseHeader");
...

```

> [!IMPORTANT]
> **注:**アプリが最初に実行されるときに`NSUserDefaults`アプリのユーザーの既定値はデータベースからのキーと値を読み取り、値が必要を開くと、毎回データベースの読み取りを回避するメモリにキャッシュに保持します。



<a name="Summary" />

## <a name="summary"></a>まとめ

この記事の内容がカバーされて、`NSUserDefaults`クラスとそのを使用して、エンドユーザーは、Xamarin.iOS アプリの構成に使用できるオプションのセットを提供する方法です。 さらに、アプリのグループを使用して、拡張機能とその親アプリ間、またはグループでのアプリ間の通信について説明します。


## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://developer.xamarin.com/samples/tvos/all/)
- [好みや設定プログラミング ガイド](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/UserDefaults/Introduction/Introduction.html#//apple_ref/doc/uid/10000059i)
- [NSUserDefaults](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/Foundation/Classes/NSUserDefaults_Class/#//apple_ref/doc/constant_group/NSUserDefaults_Domains)
