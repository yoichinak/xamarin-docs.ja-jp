---
title: Xamarin.iOS でのユーザーの既定値の操作
description: この記事では、Xamarin iOS アプリまたは拡張機能で、既定の設定を保存するための NSUserDefaults の使用について説明します。 高レベルのための NSUserDefaults をについて説明し、値を読み書きする方法について説明します。
ms.prod: xamarin
ms.assetid: DAE7FFC4-B8C9-4D9E-886A-9B2388452EEB
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/07/2016
ms.openlocfilehash: 688db534d6c99a8fadb7535f0532f9c1e9564707
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61153388"
---
# <a name="working-with-user-defaults-in-xamarinios"></a>Xamarin.iOS でのユーザーの既定値の操作

_この記事では、Xamarin.iOS アプリまたは拡張機能で、既定の設定を保存する NSUserDefault の操作について説明します。_


`NSUserDefaults`クラスは、ios アプリと拡張機能は、システム全体の既定値は、システムとの対話方法を提供します。 既定でシステムを使用して、ユーザーはアプリの動作や、ユーザー設定 (アプリのデザインに基づく) を満たすためにスタイル設定を構成できます。 たとえば、ヤード メトリックの vs でのデータを表示または指定した UI テーマを選択します。

アプリのグループを使用すると`NSUserDefaults`特定グループ内のアプリ (または拡張機能) の間で通信する方法も提供します。

<a name="About-User-Defaults" />

## <a name="about-user-defaults"></a>ユーザーの既定値について

上記のとおり、ユーザーの既定値 (`NSUserDefaults`) アプリ (または拡張機能) に追加し、エンドユーザーが外観や操作の実行時にアプリを調整する変更できる構成可能なオプションを提供するために使用できます。

アプリが最初に実行されるときに`NSUserDefaults`アプリのユーザーの既定値はデータベースからキーと値を読み取り、値が必要です。 開くと、毎回データベースの読み取りを回避するためにメモリにキャッシュします。 

> [!IMPORTANT]
> Apple 不要になったことをお勧めする開発者の呼び出し、`Synchronize`メソッドを直接データベースにメモリ内キャッシュを同期します。 代わりに、自動的に呼び出されます、メモリ内キャッシュのユーザーの既定のデータベースとの同期を維持するために定期的にします。

`NSUserDefaults`クラスには読み取りとなど、一般的なデータ型の基本設定の値を書き込み、いくつかの便利なメソッドが含まれています: 文字列、整数、浮動小数点、ブール値、および Url。 使用して他の種類のデータをアーカイブする`NSData`からの読み取りまたはユーザーの既定値はデータベースに書き込まれます。 詳細については、Apple を参照してください[の基本設定と設定のプログラミング ガイド](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/UserDefaults/Introduction/Introduction.html#//apple_ref/doc/uid/10000059i)します。

<a name="Accessing-the-Shared-NSUserDefaults-Instance" />

## <a name="accessing-the-shared-nsuserdefaults-instance"></a>共有 NSUserDefaults インスタンスへのアクセス 

ユーザーの既定の共有インスタンスは、デバイスの現在のユーザーのユーザーの既定値にアクセスを提供します。 共有の既定値のオブジェクトが存在しない場合にアクセスし、次の情報を使用して初期化は、最初に作成されます。

- `NSArgumentDomain`の現在のアプリから解析された既定値で構成されます。
- アプリのバンドル Id のドメイン。
- `NSGlobalDomain`で共有されるすべてのアプリの既定値で構成されます。
- 各ユーザーのドメインを別の言語を優先します。
- `NSRegistrationDomain`検索が成功した常に確認するアプリによって変更可能な一時的な既定のセットを使用します。

ユーザーの既定の共有インスタンスにアクセスするには、次のコードを使用します。

```csharp
// Get Shared User Defaults
var plist = NSUserDefaults.StandardUserDefaults;
```

<a name="Accessing-an-App-Group-NSUserDefaults-Instance" />

## <a name="accessing-an-app-group-nsuserdefaults-instance"></a>アプリ グループのための NSUserDefaults インスタンスへのアクセス

アプリのグループを使用して、前述のよう`NSUserDefaults`特定グループ内のアプリ (または拡張機能) 間の通信に使用できます。 最初に、アプリ グループ id と必要なアプリ Id が適切で構成されていることを確認する必要がありますには、**証明書, Identifiers & Profiles**セクションで[iOS デベロッパー センター](https://developer.apple.com/devcenter/ios/)がインストールされていると開発環境。

次に、アプリや拡張機能プロジェクトを上記で作成した有効なアプリ Id の 1 つの必要があります、`Entitlements.plist`ファイルは、アプリ グループを有効になっているし、指定したアプリ バンドルに含める必要があります。

このすべてを共有アプリ グループ ユーザーの既定値アクセスできる次のコードを使用します。

```csharp
// Get App Group User Defaults
var plist = new NSUserDefaults ("group.com.xamarin.todaysharing", NSUserDefaultsType.SuiteName);
```

場所`group.com.xamarin.todaysharing`でアプリ グループが作成**証明書, Identifiers & Profiles**にアクセスします。 詳細についてを参照してください、[アプリ グループ機能](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md)ドキュメント。

<a name="Reading-Default-Values" />

## <a name="reading-default-values"></a>既定値を読み取る

必要なユーザーの既定のデータベースにアクセスした後は、キー/値のペアと読み取られるデータの種類に基づいて、いくつかの便利なメソッドを使用して既定値から値を読み取ることができます。

- `ArrayForKey` -の配列を返します`NSObjects`の特定のキー値。
- `BoolForKey` -指定されたキーのブール値を返します。
- `DataForKey` -を返します、`NSData`指定されたキーのオブジェクト。
- `DictionaryForKey` -を返します、`NSDictionary`の特定のキー。
- `DoubleForKey` -指定されたキーの double 値を返します。
- `FloatForKey` -指定されたキーの浮動小数点値を返します。
- `IntForKey` -指定されたキーの整数値を返します。
- `StringArrayForKey` -の配列を返します`String`オブジェクト特定のキー値から。
- `StringForKey` -指定されたキーの文字列値を返します。
- `URLForKey` -を返します、`NSUrl`指定されたキーの値。

たとえば、次のコードよう、ユーザーの既定値からブール値になります。

```csharp
// Get Shared User Defaults
var plist = NSUserDefaults.StandardUserDefaults;
...

// Get value
var useHeader = plist.BoolForKey("UseHeader");

```

<a name="Writing-Default-Values" />

## <a name="writing-default-values"></a>既定値を書き込んでいます

上記の値を読み取りと同じよう、必要なユーザー既定のデータベースにアクセスした後の値に書き込めるキー/値のペアと書き込まれるデータの種類に基づいて、いくつかの便利なメソッドを使用して、既定値。

- `SetBool` -指定したキーに指定したブール値を書き込みます。
- `SetDouble` -指定したキーに指定した double 値を書き込みます。
- `SetFloat` -指定したキーに指定された浮動小数点値を書き込みます。
- `SetString` -指定したキーに指定した文字列値を書き込みます。
- `SetURL` -指定された URL を書き込みます (`NSUrl`) 値を指定したキー。

たとえば、次のコードは、ユーザーの既定値をブール値を記述します。

```csharp
// Get Shared User Defaults
var plist = NSUserDefaults.StandardUserDefaults;
...

// Save value
plist.SetBool(useHeader, "UseHeader");
...

```

> [!IMPORTANT]
> アプリが最初に実行されるときに`NSUserDefaults`アプリのユーザーの既定値はデータベースからキーと値を読み取り、値が必要です。 開くと、毎回データベースの読み取りを回避するためにメモリにキャッシュします。



<a name="Summary" />

## <a name="summary"></a>まとめ

この記事ではカバーされて、`NSUserDefaults`クラスとそのを使用して Xamarin.iOS アプリを構成する、エンドユーザーが使用できるオプションのセットを提供する方法。 さらに、アプリ グループを使用して、拡張機能とその親アプリ間、または、グループ内のアプリ間の通信について説明します。


## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://developer.xamarin.com/samples/tvos/all/)
- [基本設定と設定のプログラミング ガイド](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/UserDefaults/Introduction/Introduction.html#//apple_ref/doc/uid/10000059i)
- [NSUserDefaults](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/Foundation/Classes/NSUserDefaults_Class/#//apple_ref/doc/constant_group/NSUserDefaults_Domains)
