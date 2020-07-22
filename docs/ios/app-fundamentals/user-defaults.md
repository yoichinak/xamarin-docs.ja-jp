---
title: Xamarin でのユーザーの既定値の使用
description: この記事では、NSUserDefaults を使用して Xamarin iOS アプリまたは拡張機能に既定の設定を保存する方法について説明します。 ここでは、NSUserDefaults の概要について説明し、値の読み取りと書き込みの方法について説明します。
ms.prod: xamarin
ms.assetid: DAE7FFC4-B8C9-4D9E-886A-9B2388452EEB
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 06/07/2016
ms.openlocfilehash: c65a06b8f2a04eda669b2d741135538fa8a583f2
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/09/2020
ms.locfileid: "84571728"
---
# <a name="working-with-user-defaults-in-xamarinios"></a>Xamarin でのユーザーの既定値の使用

_この記事では、NSUserDefault を使用して Xamarin iOS アプリまたは拡張機能に既定の設定を保存する方法について説明します。_

クラスを使用 `NSUserDefaults` すると、IOS アプリと拡張機能がシステム全体の既定のシステムとプログラムを使用して対話することができます。 ユーザーは、既定のシステムを使用して、アプリの動作またはスタイル設定 (アプリの設計に基づく) を構成できます。 たとえば、メトリックと英国の測定値にデータを表示したり、特定の UI テーマを選択したりすることができます。

アプリグループで使用すると、には、 `NSUserDefaults` 特定のグループ内のアプリ (または拡張機能) 間で通信する手段も用意されています。

<a name="About-User-Defaults"></a>

## <a name="about-user-defaults"></a>ユーザーの既定値について

前述のように、ユーザーの既定値 ( `NSUserDefaults` ) をアプリ (または拡張機能) に追加し、エンドユーザーが実行時にアプリの外観や操作を調整するために変更できる構成可能なオプションを提供するために使用できます。

アプリが初めて実行されるときに、は、 `NSUserDefaults` アプリのユーザーの既定のデータベースからキーと値を読み取り、メモリにキャッシュして、値が必要になるたびにデータベースを開いたり読み取りたりしないようにします。 

> [!IMPORTANT]
> Apple では、開発者がメソッドを呼び出して、 `Synchronize` メモリ内キャッシュをデータベースと直接同期することを推奨していません。 代わりに、メモリ内キャッシュとユーザーの既定のデータベースとの同期を維持するために、定期的に自動的に呼び出されます。

クラスには、 `NSUserDefaults` string、integer、float、boolean、url などの一般的なデータ型の設定値の読み取りと書き込みを行うための便利なメソッドがいくつか含まれています。 他の種類のデータは、を使用してアーカイブし `NSData` た後、ユーザーの既定のデータベースに対して読み取りまたは書き込みを行うことができます。 詳細については、Apple の「[基本設定と設定のプログラミングガイド](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/UserDefaults/Introduction/Introduction.html#//apple_ref/doc/uid/10000059i)」を参照してください。

<a name="Accessing-the-Shared-NSUserDefaults-Instance"></a>

## <a name="accessing-the-shared-nsuserdefaults-instance"></a>共有 NSUserDefaults インスタンスへのアクセス 

共有ユーザーの既定のインスタンスは、デバイスの現在のユーザーの既定のアクセスを提供します。 共有の既定オブジェクトが存在しない場合は、最初にアクセスされ、次の情報で初期化されるときに、そのオブジェクトが作成されます。

- `NSArgumentDomain`現在のアプリから解析された既定値で構成される。
- アプリのバンドル識別子ドメイン。
- `NSGlobalDomain`すべてのアプリで共有される既定値で構成される。
- ユーザーの優先言語ごとに個別のドメイン。
- `NSRegistrationDomain`検索が常に成功するようにアプリによって変更できる一時的な既定のセットを持つ。

共有ユーザーの既定のインスタンスにアクセスするには、次のコードを使用します。

```csharp
// Get Shared User Defaults
var plist = NSUserDefaults.StandardUserDefaults;
```

<a name="Accessing-an-App-Group-NSUserDefaults-Instance"></a>

## <a name="accessing-an-app-group-nsuserdefaults-instance"></a>アプリグループの NSUserDefaults インスタンスにアクセスしています

前述のように、アプリグループを使用すると、を使用して、 `NSUserDefaults` 特定のグループ内のアプリ (または拡張機能) 間の通信を行うことができます。 最初に、 [IOS デベロッパーセンター](https://developer.apple.com/devcenter/ios/)の [ **Certificates, identifier & Profiles** ] セクションで、アプリグループと必要なアプリ id が適切に構成されていること、および開発環境にインストールされていることを確認する必要があります。

次に、アプリまたは拡張機能プロジェクトに、上で作成した有効なアプリ Id の1つが必要です。また、アプリグループが有効になっていて、 `Entitlements.plist` 指定されたアプリバンドルにファイルが含まれている必要があります。

このすべてが整ったら、次のコードを使用して、共有アプリグループのユーザーの既定値にアクセスできます。

```csharp
// Get App Group User Defaults
var plist = new NSUserDefaults ("group.com.xamarin.todaysharing", NSUserDefaultsType.SuiteName);
```

ここ `group.com.xamarin.todaysharing` で、は、[証明書] で作成されたアプリグループです。 **[識別子] は、アクセスする & プロファイル**です。 詳細については、[アプリグループ機能](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md)のドキュメントを参照してください。

<a name="Reading-Default-Values"></a>

## <a name="reading-default-values"></a>既定値の読み取り

目的のユーザーの既定のデータベースにアクセスした後、キーと値のペアを使用して既定値の値を読み取ることができます。また、読み取るデータの種類に基づいていくつかの便利な方法を使用することもできます。

- `ArrayForKey`-指定されたキー値のの配列を返し `NSObjects` ます。
- `BoolForKey`-指定されたキーのブール値を返します。
- `DataForKey`- `NSData` 指定されたキーのオブジェクトを返します。
- `DictionaryForKey`- `NSDictionary` 指定されたキーのを返します。
- `DoubleForKey`-指定されたキーの double 型の値を返します。
- `FloatForKey`-指定されたキーの float 値を返します。
- `IntForKey`-指定されたキーの整数値を返します。
- `StringArrayForKey`-指定された `String` キー値からオブジェクトの配列を返します。
- `StringForKey`-指定されたキーの文字列値を返します。
- `URLForKey`- `NSUrl` 指定されたキーの値を返します。

たとえば、次のコードは、ユーザーの既定値からブール値を読み取ります。

```csharp
// Get Shared User Defaults
var plist = NSUserDefaults.StandardUserDefaults;
...

// Get value
var useHeader = plist.BoolForKey("UseHeader");

```

<a name="Writing-Default-Values"></a>

## <a name="writing-default-values"></a>既定値の書き込み

上記の値を読み取るのと同様に、目的のユーザーの既定のデータベースにアクセスした後、キーと値のペアを使用して既定値に値を書き込むことができ、記述されるデータの種類に基づいていくつかの便利な方法があります。

- `SetBool`-指定されたキーに指定されたブール値を書き込みます。
- `SetDouble`-指定されたキーに指定された double 値を書き込みます。
- `SetFloat`-指定された float 値を指定されたキーに書き込みます。
- `SetString`-指定されたキーに指定された文字列値を書き込みます。
- `SetURL`-指定された URL ( `NSUrl` ) 値を指定されたキーに書き込みます。

たとえば、次のコードは、ユーザーの既定値にブール値を書き込みます。

```csharp
// Get Shared User Defaults
var plist = NSUserDefaults.StandardUserDefaults;
...

// Save value
plist.SetBool(useHeader, "UseHeader");
...

```

> [!IMPORTANT]
> アプリが初めて実行されるときに、は、 `NSUserDefaults` アプリのユーザーの既定のデータベースからキーと値を読み取り、メモリにキャッシュして、値が必要になるたびにデータベースを開いたり読み取りたりしないようにします。

<a name="Summary"></a>

## <a name="summary"></a>まとめ

この記事では、 `NSUserDefaults` クラスについて説明し、エンドユーザーが Xamarin IOS アプリを構成するために使用できる一連のオプションを提供する方法について説明しました。 さらに、アプリグループを使用して、拡張機能とその親アプリ間、またはグループ内のアプリ間で通信を行います。

## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [基本設定と設定のプログラミングガイド](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/UserDefaults/Introduction/Introduction.html#//apple_ref/doc/uid/10000059i)
- [NSUserDefaults](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/Foundation/Classes/NSUserDefaults_Class/#//apple_ref/doc/constant_group/NSUserDefaults_Domains)
