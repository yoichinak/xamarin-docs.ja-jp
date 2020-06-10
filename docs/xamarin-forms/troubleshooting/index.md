---
title: "トラブルシューティング" の説明: "一般的なエラー状態とそれらを解決する方法" ms. トピック: トラブルシューティング ms. 製品: xamarin ms. assetid: 63291951-7375-4CBF-BCC3-2E4AD157A2C8: davidbritch: dabritch ms. date: 04/25/2017 no loc: [ Xamarin.Forms ,]」を参照してください Xamarin.Essentials 。
---

# <a name="troubleshooting"></a>トラブルシューティング

_一般的なエラー状態とその解決方法_

## <a name="error-unable-to-find-a-version-of-xamarinforms-compatible-with"></a>エラー: "互換性のあるバージョンを見つけることができません Xamarin.Forms ..."

ソリューションまたは Android アプリプロジェクト内のすべての NuGet パッケージを更新するときに、**パッケージコンソール**ウィンドウに次のエラーが表示されることが Xamarin.Forms Xamarin.Forms あります。

```csharp
Attempting to resolve dependency 'Xamarin.Android.Support.v7.AppCompat (= 23.3.0.0)'.
Attempting to resolve dependency 'Xamarin.Android.Support.v4 (= 23.3.0.0)'.
Looking for updates for 'Xamarin.Android.Support.v7.MediaRouter'...
Updating 'Xamarin.Android.Support.v7.MediaRouter' from version '23.3.0.0' to '23.3.1.0' in project 'Todo.Droid'.
Updating 'Xamarin.Android.Support.v7.MediaRouter 23.3.0.0' to 'Xamarin.Android.Support.v7.MediaRouter 23.3.1.0' failed.
Unable to find a version of 'Xamarin.Forms' that is compatible with 'Xamarin.Android.Support.v7.MediaRouter 23.3.0.0'.
```

### <a name="what-causes-this-error"></a>このエラーの原因

Visual Studio for Mac (または Visual Studio) は、 Xamarin.Forms NuGet パッケージ*とそのすべての依存関係*の更新プログラムが利用可能であることを示している場合があります。 Xamarin Studio では、ソリューションの [**パッケージ**] ノードは次のようになります (バージョン番号は異なる場合があります)。

![](images/updates-available.png "Android Project Packages Folder")

このエラーは、_すべて_のパッケージを更新しようとした場合に発生する可能性があります。

これは、android プロジェクトが Android 6.0 (API 23) 以降のバージョンに設定されていると、 Xamarin.Forms android サポートパッケージの*特定*のバージョンに対するハードな依存関係があるためです。 これらのパッケージの更新バージョンが使用可能な場合もありますが、これらのパッケージ Xamarin.Forms とは必ずしも互換性がありません。

この場合、 _only_ **Xamarin.Forms** 依存関係が互換性のあるバージョンで維持されるように、パッケージのみを更新する必要があります。 プロジェクトに追加した他のパッケージは、Android サポートパッケージが更新されない限り、個別に更新することもできます。

> [!NOTE]
> 2.3.4 以上を使用していて、 Xamarin.Forms android プロジェクトのターゲット/コンパイルバージョンが android 7.0 (API 24) 以上に設定されている場合は、上記のハード依存関係が適用されなくなり、パッケージとは別にサポートパッケージを更新でき**and** Xamarin.Forms ます。

### <a name="fix-remove-all-packages-and-re-add-xamarinforms"></a>修正: すべてのパッケージを削除し、再度追加します。Xamarin.Forms

互換性の**ない**バージョンに更新された場合、最も簡単な解決策は次のとおりです。

1. Android プロジェクトのすべての NuGet パッケージを手動で削除します。
2. パッケージを再度追加 **Xamarin.Forms** します。

これにより、他のパッケージの*正しい*バージョンが自動的にダウンロードされます。

このプロセスのビデオについては、こちらの[フォーラム投稿](https://forums.xamarin.com/discussion/comment/170012/#Comment_170012)を参照してください。
