---
title: Amazon SimpleDB サービスとユーザーの認証
description: Amazon SimpleDB では、独自のリソース ベースのアクセス許可システムを提供していません。 代わりに、ことを確認ユーザーのみが独自のデータへのアクセス、SimpleDB ドメイン内の id プロバイダーに対する認証を使用できます。 この記事では、独自の SimpleDB データへのユーザーのアクセスを制限する方法について説明します。
ms.prod: xamarin
ms.assetid: 797C91A5-9720-4DAC-89D8-5C85996584C8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2016
ms.openlocfilehash: 592e957e0c64e7189d6f01f1ba0f23da074c4bec
ms.sourcegitcommit: 271d3f7ea4abfcf87734d2c747a68cb8114d743c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/12/2018
---
# <a name="authenticating-users-with-an-amazon-simpledb-service"></a>Amazon SimpleDB サービスとユーザーの認証

_Amazon SimpleDB では、独自のリソース ベースのアクセス許可システムを提供していません。代わりに、ことを確認ユーザーのみが独自のデータへのアクセス、SimpleDB ドメイン内の id プロバイダーに対する認証を使用できます。この記事では、独自の SimpleDB データへのユーザーのアクセスを制限する方法について説明します。_

[Xamarin.Auth](https://github.com/xamarin/Xamarin.Auth)サンプル アプリケーションでユーザーの認証プロセスを管理し、デバイスにユーザーのアカウントの詳細を安全に保管するために使用します。 詳細については、次を参照してください。[を Id プロバイダーにユーザーの認証](~/xamarin-forms/data-cloud/authentication/oauth.md)です。

## <a name="allowing-an-authenticated-user-access-to-simpledb-domain-data"></a>SimpleDB ドメインのデータへの認証済みユーザー アクセスを許可します。

サンプル アプリケーションを使用して、`TodoItem`モデル データへのクラスです。 格納する、`TodoItem`に変換する必要がありますまず SimpleDB サービス内のインスタンス、`List`の`ReplaceableAttribute`オブジェクト。 詳細については、次を参照してください。[作成 SimpleDB オブジェクト](~/xamarin-forms/data-cloud/consuming/aws.md)です。

> [!NOTE]
> Ios 9 以降ではは、アプリのトランスポート セキュリティ (ATS) は、機密情報の誤った情報開示を回避をセキュリティで保護された接続 (アプリのバック エンド サーバーなど) のインターネット リソースと、アプリの間に強制します。 ATS が iOS 9 用にビルドされたアプリで既定で有効になるために、すべての接続は ATS セキュリティ要件に応じたされます。 接続はこれらの要件を満たしていない場合は、例外で失敗します。
> 使用することがない場合のうち ATS を選択することができます、`HTTPS`プロトコルし、インターネット リソースのための通信をセキュリティで保護します。 アプリケーションを更新することによってこれを行う**Info.plist**ファイル。 詳細については、次を参照してください。[アプリ トランスポート セキュリティ](~/ios/app-fundamentals/ats.md)です。

アクセスできるようにユーザーが自分のデータのみを SimpleDB ドメイン内に、`ToSimpleDBReplaceableAttributes`メソッドの追加の属性を格納する、`TodoItem`インスタンス、次のコード例に示すようにします。

```csharp
List<ReplaceableAttribute> ToSimpleDBReplaceableAttributes (TodoItem item)
{
  return new List<ReplaceableAttribute> () {
    ...
    new ReplaceableAttribute () {
      Name = "User",
      Value = App.User.Email,
      Replace = true
    },
  };
}
```

この属性では、SimpleDB ドメインに格納されている各項目にデータが属しているユーザーを一意に識別するために使用される、ユーザーの関連付けられた電子メール アドレスがあることを確認します。 呼び出すことによって、ドメインの内容を取得するときに、`AmazonSimpleDBClient.SelectAsync`メソッド、クエリ式により、認証されたユーザーの項目のみが取得されたことの次のコード例に示すようにします。

```csharp
public async Task<List<TodoItem>> RefreshDataAsync ()
{
  ...
  var request = new SelectRequest () {
    SelectExpression = string.Format ("SELECT * from {0} WHERE User = '{1}'", tableName, App.User.Email)
  };
  var response = await client.SelectAsync (request);
  ...
}
```

`SelectAsync`項目およびクエリ式に一致する関連付けられた属性のコレクションを含む応答を返します。 クエリ式では、ユーザーの電子メール アドレスに一致する項目のみを取得することにより、します。 クエリ式の詳細については、次を参照してください。 [Amazon SimpleDB クエリの作成に使用して選択](http://docs.aws.amazon.com/AmazonSimpleDB/latest/DeveloperGuide/UsingSelect.html)Amazon の web サイトです。

> [!NOTE]
> クエリ式を構築するときに引用符の規則に従うように注意します。 詳細については、次を参照してください。 [引用符で囲むルール](http://docs.aws.amazon.com/AmazonSimpleDB/latest/DeveloperGuide/QuotingRulesSelect.html)Amazon の web サイトです。

## <a name="summary"></a>まとめ

この記事では、独自の SimpleDB データへのユーザーのアクセスを制限する方法について説明します。 Amazon SimpleDB では、独自のリソース ベースのアクセス許可システムを提供していません。 代わりに、アクセスできるようにユーザーが自分のデータのみを SimpleDB ドメイン内に id プロバイダーに対する認証を使用できます。


## <a name="related-links"></a>関連リンク

- [TodoAWSAuth (sample)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAWSAuth/)
- [Amazon SimpleDB サービスの使用](~/xamarin-forms/data-cloud/consuming/aws.md)
- [Id プロバイダーとユーザーの認証](~/xamarin-forms/data-cloud/authentication/oauth.md)
- [Amazon Cognito Identity](http://docs.aws.amazon.com/cognito/devguide/identity/)
- [Amazon SimpleDB 開発者向けドキュメント](http://docs.aws.amazon.com/AmazonSimpleDB/latest/DeveloperGuide/Welcome.html)
- [Amazon Web Services SDK for .NET](https://www.nuget.org/packages?q=Tags%3A%22aws-sdk-v3%22)
