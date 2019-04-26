---
title: トラブルシューティング
description: 一般的なエラー状況とその解決方法
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 63291951-7375-4CBF-BCC3-2E4AD157A2C8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: fbe4fb6fce52636b59a9637ee0150c4c19fcc9da
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "60850442"
---
# <a name="troubleshooting"></a>トラブルシューティング

_一般的なエラー状況とその解決方法_

## <a name="error-unable-to-find-a-version-of-xamarinforms-compatible-with"></a>エラー :「Xamarin.Forms のバージョンとの互換性を... 検索できません。」

次のエラーが表示できる、**パッケージ コンソール**ウィンドウのすべての Nuget パッケージで Xamarin.Forms ソリューション、または Xamarin.Forms の Android アプリ プロジェクトで更新する場合。

```csharp
Attempting to resolve dependency 'Xamarin.Android.Support.v7.AppCompat (= 23.3.0.0)'.
Attempting to resolve dependency 'Xamarin.Android.Support.v4 (= 23.3.0.0)'.
Looking for updates for 'Xamarin.Android.Support.v7.MediaRouter'...
Updating 'Xamarin.Android.Support.v7.MediaRouter' from version '23.3.0.0' to '23.3.1.0' in project 'Todo.Droid'.
Updating 'Xamarin.Android.Support.v7.MediaRouter 23.3.0.0' to 'Xamarin.Android.Support.v7.MediaRouter 23.3.1.0' failed.
Unable to find a version of 'Xamarin.Forms' that is compatible with 'Xamarin.Android.Support.v7.MediaRouter 23.3.0.0'.
```

### <a name="what-causes-this-error"></a>このエラーの原因は何ですか。

更新プログラムは、Xamarin.Forms Nuget パッケージを使用する visual Studio for Mac (または Visual Studio) がある可能性があります*とそのすべての依存関係*します。 Xamarin Studio で、ソリューションの**パッケージ**ノードは次のようになります (バージョン番号が異なる可能性があります)。

![](images/updates-available.png "Android プロジェクトのパッケージ フォルダー")

このエラーは、更新しようとした場合に発生する可能性があります_すべて_パッケージ。

これは、Android プロジェクトが Android 6.0 (API 23) のターゲット/コンパイル バージョンに設定または以下では、Xamarin.Forms ハードに依存しているため*特定*Android のバージョンは、パッケージをサポートします。 これらのパッケージの更新バージョンができる場合があります、Xamarin.Forms は必ずしも互換性それらがありません。

このケースで更新する必要があります_のみ_、 **Xamarin.Forms**パッケージの依存関係が互換性のあるバージョンに残っていることができます。 その他のパッケージをプロジェクトに追加した可能性がありますも個別に更新する Android サポート パッケージを更新することがない限り、します。


> [!NOTE]
> Xamarin.Forms 2.3.4 を使用している場合またはそれ以上**と**Android 7.0 (API 24) または以降では、Android プロジェクトのターゲット/コンパイル バージョンに設定、不要になった上記で説明したハードの依存関係を適用し、サポートを更新することがありますXamarin.Forms パッケージとは別にパッケージします。


### <a name="fix-remove-all-packages-and-re-add-xamarinforms"></a>修正:すべてのパッケージを削除して再度 Xamarin.Forms を追加

場合、 **Xamarin.Android.Support**パッケージは、互換性のないバージョンに更新されましたが、最も簡単な修正するには。

1. Android のプロジェクト内のすべての Nuget パッケージを手動で削除し、
2. もう一度追加、 **Xamarin.Forms**パッケージ。

これは自動的にダウンロード、*正しい*の他のパッケージのバージョン。

このプロセスのビデオを参照してください、これを参照してください[フォーラム投稿](https://forums.xamarin.com/discussion/comment/170012/#Comment_170012)します。
