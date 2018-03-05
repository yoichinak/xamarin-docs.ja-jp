---
title: "トラブルシューティング"
description: "一般的なエラー状況と問題を解決する方法"
ms.topic: article
ms.prod: xamarin
ms.assetid: 63291951-7375-4CBF-BCC3-2E4AD157A2C8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: 23ebefcbd6114b06c39740b3b56f87aeac0b9a00
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="troubleshooting"></a>トラブルシューティング

_一般的なエラー状況と問題を解決する方法_

## <a name="error-unable-to-find-a-version-of-xamarinforms-compatible-with"></a>「できません Xamarin.Forms のバージョンと互換性が... 見つかりません」エラー:

次のエラーが表示できる、**パッケージ コンソール**すべての Nuget パッケージ、まず Xamarin.Forms ソリューションまたは Xamarin.Forms Android アプリ プロジェクトを更新するときにウィンドウ。

```csharp
Attempting to resolve dependency 'Xamarin.Android.Support.v7.AppCompat (= 23.3.0.0)'.
Attempting to resolve dependency 'Xamarin.Android.Support.v4 (= 23.3.0.0)'.
Looking for updates for 'Xamarin.Android.Support.v7.MediaRouter'...
Updating 'Xamarin.Android.Support.v7.MediaRouter' from version '23.3.0.0' to '23.3.1.0' in project 'Todo.Droid'.
Updating 'Xamarin.Android.Support.v7.MediaRouter 23.3.0.0' to 'Xamarin.Android.Support.v7.MediaRouter 23.3.1.0' failed.
Unable to find a version of 'Xamarin.Forms' that is compatible with 'Xamarin.Android.Support.v7.MediaRouter 23.3.0.0'.
```

### <a name="what-causes-this-error"></a>このエラーの原因ですか。

Mac (または Visual Studio) 用の visual Studio は、更新プログラムは Xamarin.Forms Nuget パッケージを使用する可能性があります*とそのすべての依存関係*です。 Xamarin Studio で、ソリューションの**パッケージ**ノードは次のようになります (バージョン番号が異なる可能性があります)。

![](images/updates-available.png "Android プロジェクト パッケージ フォルダー")

このエラーは、更新しようとする場合に発生する可能性があります_すべて_パッケージです。

これは、Android プロジェクトが Android 6.0 (API 23) のターゲット/コンパイル バージョンに設定または次に、Xamarin.Forms ハードに依存しているため*特定*Android のバージョンは、パッケージをサポートします。 これらのパッケージの更新バージョンを使用することが Xamarin.Forms は必ずしも互換性にありません。

ここで更新する必要があります_のみ_、 **Xamarin.Forms**パッケージの依存関係は、互換性のあるバージョンに残っていることができます。 他のパッケージをプロジェクトに追加した可能性がありますも個別に更新する、Android のサポート パッケージを更新することがない限り、します。


> [!NOTE]
> Xamarin.Forms 2.3.4 を使用している場合またはそれ以上**と**Android 7.0 (API 24) 以上、Android プロジェクトのターゲット/コンパイル バージョンに設定されている、不要になった上記ハード依存関係を適用し、サポートを更新することがありますXamarin.Forms パッケージとは別にパッケージします。


### <a name="fix-remove-all-packages-and-re-add-xamarinforms"></a>修正: すべてのパッケージを削除してから再度 Xamarin.Forms を追加

場合、 **Xamarin.Android.Support**パッケージは、互換性のないバージョンに更新されましたが、最も簡単な修正プログラムでは。

1. Android のプロジェクト内のすべての Nuget パッケージを手動で削除し、
2. もう一度追加、 **Xamarin.Forms**パッケージです。

これは自動的にダウンロード、*正しい*の他のパッケージのバージョン。

このプロセスのビデオを表示するには、これを参照してください[フォーラムの投稿](https://forums.xamarin.com/discussion/comment/170012/#Comment_170012)です。
