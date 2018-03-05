---
title: "Amazon SimpleDB サービスの使用"
description: "Amazon SimpleDB は、web サービスを保存し、Amazon のクラウド内のデータをクエリする機能を提供します。 この記事では、AWS SDK for .NET を使用してクエリを実行、作成、置換、および SimpleDB サービスに格納されたデータを削除する方法について説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 823819AA-15F9-4144-B355-78A10AD37513
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2016
ms.openlocfilehash: 590e39deb7972df9e45064bb1a96e533a1fc9856
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="consuming-an-amazon-simpledb-service"></a>Amazon SimpleDB サービスの使用

_Amazon SimpleDB は、web サービスを保存し、Amazon のクラウド内のデータをクエリする機能を提供します。この記事では、AWS SDK for .NET を使用してクエリを実行、作成、置換、および SimpleDB サービスに格納されたデータを削除する方法について説明します。_

SimpleDB services では、REST サービスのコンシューマーになじみ要求と応答のモデルを使用します。 操作は、データを含むことができる、要求を送信して SimpleDB サービスで呼び出されます。 要求を処理した後は、SimpleDB サービスは、すべての結果を含む応答を返します。 SimpleDB サービス プログラムで作成する必要がありでは作成できません、 [AWS コンソール](https://aws.amazon.com)です。 ただし、AWS アカウントは、作成し、Amazon web サービスにアクセスする必要があります。

SimpleDB サービスで、データは、構造体でデータを配置する、データに対してクエリを実行しているドメインに編成されます。 ドメインは、属性の名前と値のペアで説明されている項目で構成されます。 ドメインは、列、および行と同様になるアイテムのようなものの属性を持つテーブル、ようなものと考えることができます。 SimpleDB データ モデルの詳細については、次を参照してください。[データ モデル](http://docs.aws.amazon.com/AmazonSimpleDB/latest/DeveloperGuide/DataModel.html)Amazon の web サイトです。

Amazon の必要なサービスの設定方法は、サンプル アプリケーションに付属している readme ファイルで確認できます。 サンプル アプリケーションの実行時に、次のスクリーン ショットに示すように、SimpleDB サービスへのアクセスを承認するために、Amazon Cognito identity プールに接続します。

![](aws-images/portal.png "サンプル アプリケーション")

> [!NOTE]
> Ios 9 以降ではは、アプリのトランスポート セキュリティ (ATS) は、機密情報の誤った情報開示を回避をセキュリティで保護された接続 (アプリのバック エンド サーバーなど) のインターネット リソースと、アプリの間に強制します。 ATS が iOS 9 用にビルドされたアプリで既定で有効になるために、すべての接続は ATS セキュリティ要件に応じたされます。 接続はこれらの要件を満たしていない場合は、例外で失敗します。
> 使用することがない場合のうち ATS を選択することができます、`HTTPS`プロトコルし、インターネット リソースのための通信をセキュリティで保護します。 アプリケーションを更新することによってこれを行う**Info.plist**ファイル。 詳細については、次を参照してください。[アプリ トランスポート セキュリティ](~/ios/app-fundamentals/ats.md)です。

## <a name="consuming-a-simpledb-service"></a>SimpleDB サービスの使用

Amazon Cognito Identity により、AWS サービス、アプリケーションにハードコーディング AWS 資格情報のないアプリケーションから呼び出される、SimpleDB などです。 一意の id のプールを作成する代わりに、 [Amazon Cognito コンソール](https://console.aws.amazon.com/cognito/home)です。 Identity プールには、id がアクセスできる SimpleDB などのリソースを指定するロールを使用する id が含まれています。

[AWS SDK for .NET](https://www.nuget.org/packages?q=Tags%3A%22aws-sdk-v3%22)提供、`CognitoAWSCredentials`と`AmazonSimpleDBClient`クラスで、アプリケーションで使用される、Xamarin.Forms SimpleDB サービスにアクセスする次のコード例に示すようにします。

```csharp
AmazonSimpleDBClient client;
...

public SimpleDBStorage ()
{
  var credentials = new CognitoAWSCredentials (
                      Constants.CognitoIdentityPoolId,
                      RegionEndpoint.USEast1);
  var config = new AmazonSimpleDBConfig ();
  config.RegionEndpoint = RegionEndpoint.USWest2;
  client = new AmazonSimpleDBClient (credentials, config);
  ...
}
```

新しいインスタンス、`CognitoAWSCredentials`の一意の id のプールの id および Cognito Id アカウントの地域によって提供されるクラスを作成します。 ドキュメントの執筆時に、Cognito Id はのみ使用可能な USEast1 と EUWest1 領域です。 ただし、それらの領域外の Amazon サービスと通信できます。

ときに、`AmazonSimpleDBClient`インスタンスが作成、`CognitoAWSCredentials`インスタンス必要がありますを提供する、と共に、 `AmazonSimpleDBConfig` SimpleDB サービスが存在する地理的地域を指定するインスタンス。 `CognitoAWSCredentials`インスタンスによりアクセスされる SimpleDB サービス AWS アクセス キーと秘密鍵をアプリケーションに埋め込む必要性を回避しながら、identity プールを作成した AWS アカウントに関連付けられているいずれかがします。

呼び出して SimpleDB サービス ドメインを作成、`AmazonSimpleDBClient.CreateDomainAsync`メソッドを次のコード例に示すようにします。

```csharp
string tableName = "Todo";
...

async Task CreateDomain ()
{
  ...
  await client.CreateDomainAsync (new CreateDomainRequest { DomainName = tableName });
  ...
}
```

`CreateDomainAsync`メソッドが必要な`CreateDomainRequest`をパラメーターとしてのインスタンス。 `CreateDomainRequest`インスタンスを初期化します、`DomainName`プロパティをドメインを識別するために使用する値。 ドメインを作成するには、この値は、AWS アカウントに関連付けられているドメインの間で一意にあります。 それ以外の場合、ドメインは作成されませんし、エラー応答を示すために送信されません。 ドメイン名から、操作し、新しく作成されたドメインではなく、既存のドメインに対して発生します。

### <a name="creating-simpledb-objects"></a>SimpleDB オブジェクトを作成します。

サンプル アプリケーションを使用して、`TodoItem`モデル データへのクラスです。 格納する、`TodoItem`に変換する必要がありますまず SimpleDB サービス内のインスタンス、`List`の`ReplaceableAttribute`オブジェクト。 これは、`ToSimpleDBReplaceableAttributes`メソッドを次のコード例に示すようにします。

```csharp
List<ReplaceableAttribute> ToSimpleDBReplaceableAttributes (TodoItem item)
{
  return new List<ReplaceableAttribute> () {
    new ReplaceableAttribute () {
      Name = "Name",
      Value = item.Name,
      Replace = true
    },
    new ReplaceableAttribute () {
      Name = "Notes",
      Value = item.Notes,
      Replace = true
    },
    new ReplaceableAttribute () {
      Name = "Done",
      Value = item.Done.ToString (),
      Replace = true
    }
  };
}
```

このメソッドを作成、`List`の新しい`ReplaceableAttribute`インスタンスで、 `List` 、1 つを表す`TodoItem`インスタンス。 各`ReplaceableAttribute`インスタンスから 1 つのプロパティを表す、`TodoItem`インスタンス。 詳細については、`ReplaceableAttribute`クラスを参照してください[ReplaceableAttribute クラス](http://docs.aws.amazon.com/sdkfornet1/latest/apidocs/html/T_Amazon_SimpleDB_Model_ReplaceableAttribute.htm)Amazon の web サイトです。

同様に、データは SimpleDB サービスから取得された、ときに変換する必要ありますから、`List`の`Attribute`インスタンスを`TodoItem`インスタンス。 これを使用、`FromSimpleDBAttributes`メソッドを次のコード例に示すようにします。

```csharp
TodoItem FromSimpleDBAttributes (List<Amazon.SimpleDB.Model.Attribute> attributeList, string id)
{
  var todoItem = new TodoItem ();
  todoItem.ID = id;
  todoItem.Name = attributeList.Where (attr => attr.Name == "Name").FirstOrDefault ().Value;
  todoItem.Notes = attributeList.Where (attr => attr.Name == "Notes").FirstOrDefault ().Value;
  todoItem.Done = Convert.ToBoolean (attributeList.Where (attr => attr.Name == "Done").FirstOrDefault ().Value);
  return todoItem;
}
```

このメソッドは単に各取得`Attribute`インスタンスから、`List`新しく作成された設定と`TodoItem`インスタンス。

詳細については、`Attribute`クラスを参照してください[属性クラス](http://docs.aws.amazon.com/sdkfornet1/latest/apidocs/html/T_Amazon_SimpleDB_Model_Attribute.htm)Amazon の web サイトです。

### <a name="querying-data"></a>データのクエリ

ドメインの内容を呼び出すことによって取得できます、`AmazonSimpleDBClient.SelectAsync`メソッドを次のコード例に示すようにします。

```csharp
public async Task<List<TodoItem>> RefreshDataAsync ()
{
  ...
  var request = new SelectRequest () {
      SelectExpression = string.Format ("SELECT * from {0}", tableName)
  };
  var response = await client.SelectAsync (request);
  foreach (var item in response.Items) {
    Items.Add (FromSimpleDBAttributes (item.Attributes, item.Name));
  }
  ...
}
```

`SelectAsync`メソッドを受け入れる、`SelectRequest`インスタンスを指定すると、パラメーターとして、`Select`クエリ内の式`SelectExpression`プロパティです。 クエリ式の形式は、標準の SQL の形式に似ています`SELECT`ステートメントです。 クエリ式の詳細については、次を参照してください。 [Amazon SimpleDB クエリの作成に使用して選択](http://docs.aws.amazon.com/AmazonSimpleDB/latest/DeveloperGuide/UsingSelect.html)Amazon の web サイトです。

> [!NOTE]
> **注**: クエリ式を構築するときに引用符の規則に従うように注意してください。 詳細については、次を参照してください。 [引用符で囲むルール](http://docs.aws.amazon.com/AmazonSimpleDB/latest/DeveloperGuide/QuotingRulesSelect.html)Amazon の web サイトです。

`SelectAsync`項目およびクエリ式に一致する関連付けられた属性のコレクションを含む応答を返します。 このコレクションに変換されます、`List`の`TodoItem`表示のインスタンス。

### <a name="creating-and-replacing-data"></a>作成して、データの交換

`AmazonSimpleDBClient.PutAttributesAsync`次のコード例のように、SimpleDB サービス ドメイン内のデータを作成し、置換メソッドを使用します。

```csharp
public async Task SaveTodoItemAsync (TodoItem todoItem)
{
  ...
  var attributeList = ToSimpleDBReplaceableAttributes (todoItem);
  var request = new PutAttributesRequest () {
      DomainName = tableName,
      ItemName = todoItem.ID,
      Attributes = attributeList
  };
  await client.PutAttributesAsync (request);
  ...
}
```

`PutAttributesAsync`メソッドを受け入れる、`PutAttributesRequest`をパラメーターとしてのインスタンス。 `PutAttributesRequest`インスタンスは、新しいアイテムとして作成または既存のアイテムで置換するのには、属性名と値のペアを指定します。 `List`の`ReplaceableAttribute`によってインスタンスが構築された、`ToSimpleDBReplaceableAttributes`メソッドです。 このメソッドも設定、`Replace`の各プロパティ`ReplaceableAttribute`に`true`です。 これによりデータを交換する場合は、既存の属性値を置換する新しい属性の値。 ただし、エラー応答に存在しない属性値の置換を試みるは発生しません。

値、`PutAttributesRequest.ItemName`プロパティは、ドメインに新しい項目を追加するかどうか、または既存の項目が置き換えられますかどうかを制御します。 アプリケーションでは、新しい項目を作成するときに、設定、`TodoItem.ID`プロパティを新しい`Guid`です。 これによって、各`TodoItem`インスタンスには一意の識別子。 したがって場合、`PutAttributesRequest.ItemName`プロパティは、ドメインに存在しない値に設定は、SimpleDB サービスでは、指定した属性の名前と値のペアを含む新しい項目を作成します。 場合、`PutAttributesRequest.ItemName`プロパティが既に存在する値、ドメイン内、SimpleDB サービスは指定した属性の名前と値ペアを持つ項目を更新します。

### <a name="deleting-data"></a>データの削除

`AmazonSimpleDBClient.DeleteAttributesAsync`次のコード例のように、SimpleDB サービス ドメインからデータを削除するメソッドを使用します。

```csharp
public async Task DeleteTodoItemAsync (TodoItem todoItem)
{
  ...
  var attributeList = ToSimpleDBAttributes (todoItem);
  var request = new DeleteAttributesRequest () {
      DomainName = tableName,
      ItemName = todoItem.ID,
      Attributes = attributeList
  };
  await client.DeleteAttributesAsync (request);
  ...
}
```

`DeleteAttributesAsync`メソッドを受け入れる、`DeleteAttributesRequest`をパラメーターとしてのインスタンス。  `DeleteAttributesRequest`インスタンスで、項目から削除される属性を指定する、`List`の`Attribute`を削除するインスタンスによって構築される、`ToSimpleDBAttributes`メソッドです。 項目のすべての属性が削除されますが、項目が削除されます。

## <a name="summary"></a>まとめ

この記事では、AWS SDK for .NET を使用してクエリを実行、作成し、置換、および SimpleDB サービスに格納されたデータを削除する方法について説明します。 この SDK は、`CognitoAWSCredentials`と`AmazonSimpleDBClient`SimpleDB サービスにアクセスする Xamarin.Forms アプリケーションによって使用されるクラスです。


## <a name="related-links"></a>関連リンク

- [TodoAWS (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAWS/)
- [Amazon Web Services SDK Xamarin 開発者ガイド](http://docs.aws.amazon.com/mobile/sdkforxamarin/developerguide/)
- [Amazon Cognito Identity](http://docs.aws.amazon.com/cognito/devguide/identity/)
- [Amazon SimpleDB 開発者向けドキュメント](http://docs.aws.amazon.com/AmazonSimpleDB/latest/DeveloperGuide/Welcome.html)
- [AmazonSimpleDBClient クラス](http://docs.aws.amazon.com/sdkfornet1/latest/apidocs/html/T_Amazon_SimpleDB_AmazonSimpleDBClient.htm)
- [Amazon Web Services SDK for .NET](https://www.nuget.org/packages?q=Tags%3A%22aws-sdk-v3%22)
