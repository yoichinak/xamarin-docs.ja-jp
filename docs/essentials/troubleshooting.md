---
タイトル: "Xamarin.Essentials:トラブルシューティング'' の説明: "このドキュメントでは、Xamarin.Essentials ライブラリを使用した開発時に発生する問題のトラブルシューティング方法について説明します。"
ms.assetid:2E474FAF-F841-4E3C-B815-F7ABD8EE3361 author: jamesmontemagno ms.author: jamont ms.date:01/06/2020 no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# <a name="xamarinessentials-troubleshooting"></a>Xamarin.Essentials:トラブルシューティング

## <a name="error-version-conflict-detected-for-xamarinandroidsupportcompat"></a>エラー :Xamarin.Android.Support.Compat について検出されるバージョンの競合

Xamarin.Forms プロジェクトを含む NuGet パッケージを更新 (または新しいパッケージを追加) するときに、次のエラーが発生する場合があります。
これは Xamarin.Essentials を使用するプロジェクトです。

```error
NU1107: Version conflict detected for Xamarin.Android.Support.Compat. Reference the package directly from the project to resolve this issue.
 MyApp -> Xamarin.Essentials 1.3.1 -> Xamarin.Android.Support.CustomTabs 28.0.0.3 -> Xamarin.Android.Support.Compat (= 28.0.0.3)
 MyApp -> Xamarin.Forms 3.1.0.583944 -> Xamarin.Android.Support.v4 25.4.0.2 -> Xamarin.Android.Support.Compat (= 25.4.0.2).
```

問題は、2 つの NuGet の依存関係の不一致にあります。 これは、両方をサポートできる特定のバージョンの依存関係 (この場合は **Xamarin.Android.Support.Compat**) を手動で追加することで解決できます。

これを行うには、競合の原因となっている NuGet を手動で追加し、**バージョン**の一覧を使って特定のバージョンを選びます。 現在のバージョン 28.0.0.3 の Xamarin.Android.Support.Compat および Xamarin.Android.Support.Core.Util NuGet によってこのエラーが解決されます。

詳細情報と、問題の解決方法に関するビデオについては、[このブログ記事](https://redth.codes/how-to-fix-the-dreaded-version-conflict-nuget-error-in-your-xamarin-android-projects/)を参照してください。

問題が発生したりバグを見つけたりした場合は、[Xamarin.Essentials の GitHub リポジトリ](https://github.com/xamarin/Essentials)でご報告ください。
