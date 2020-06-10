---
title: " Xamarin.Forms よく寄せられる質問" ms. トピック: トラブルシューティング ms. 製品: xamarin ms. assetid: 89364175-0ba47 A09-B3E2-44 ac67dd971c ms. テクノロジ: xamarin-forms author: 04/25/2017 davidbritch: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="xamarinforms-frequently-asked-questions"></a>Xamarin.Formsよく寄せられる質問

## <a name="can-i-update-the-xamarinforms-default-template-to-a-newer-nuget-packageupdate-forms-templatemd"></a>[Xamarin.Forms既定のテンプレートを新しい NuGet パッケージに更新することはできますか。](update-forms-template.md)
このガイドでは、 Xamarin.Forms 例として .NET Standard ライブラリテンプレートを使用しますが、共有プロジェクトテンプレートでも同じ一般的な方法を使用でき Xamarin.Forms ます。

## <a name="why-doesnt-the-visual-studio-xaml-designer-work-for-xamarinforms-xaml-filesforms-xaml-designermd"></a>[Xaml ファイルに対して Visual Studio XAML デザイナーが動作しないのはなぜです Xamarin.Forms か。](forms-xaml-designer.md)
Xamarin.Formsでは、XAML ファイルのビジュアルデザイナーは現在サポートされていません。

## <a name="android-build-error-the-linkassemblies-task-failed-unexpectedly"></a>[Android のビルドエラー: "LinkAssemblies" タスクが予期せず失敗しました](android-linkassemblies-error.md)
`The "LinkAssemblies" task failed unexpectedly`フォームを使用する Xamarin Android プロジェクトをビルドすると、エラーメッセージが表示される場合があります。 これは、リンカーがアクティブなときに発生します (通常は*リリース*ビルドで、アプリケーションパッケージのサイズを小さくします)。また、Android ターゲットが最新のフレームワークに更新されないために発生します。 

## <a name="why-does-my-xamarinformsmaps-android-project-fail-with-compiletodalvik--unexpected-top-level-errormaps-compiletodalvik-errormd"></a>["なぜですか Xamarin.Forms 。Maps Android プロジェクトが COMPILETODALVIK で失敗する: 予期しないトップレベルのエラーが発生した場合は、"](maps-compiletodalvik-error.md)
このエラーは、Visual Studio for Mac のエラーパッドまたは Visual Studio のビルド出力ウィンドウに表示されることがあります。Android プロジェクトでは、を使用 Xamarin.Forms します。マップ. 最も一般的に解決されるのは、Xamarin Android プロジェクトの Java ヒープサイズを増やすことによって解決されます。
