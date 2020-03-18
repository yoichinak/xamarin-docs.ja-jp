---
title: Xamarin. フォームの XAML ホットリロード
description: 実行中のアプリケーションに直ちに XAML ファイルへの変更を再度読み込みます。これにより、XAML を変更するたびに Xamarin. Forms プロジェクトをビルドする必要がなくなります。
ms.prod: xamarin
ms.assetid: E220F054-32EE-424C-A7E5-6156BE271519
ms.technology: xamarin-forms
author: maddyleger1
ms.author: maleger
ms.date: 03/14/2020
ms.openlocfilehash: 225b7dc7dc639031b3198a8fb9e7fe9fb9d7ee7f
ms.sourcegitcommit: 8df67f0d76ff762b517d27b8d4c217d3a3379a18
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/16/2020
ms.locfileid: "79423877"
---
# <a name="xaml-hot-reload-for-xamarinforms"></a>Xamarin. フォームの XAML ホットリロード

XAML ホットリロードは、生産性を向上させ、時間を節約するために、既存のワークフローにプラグインします。 Xaml のホットリロードを使用しない場合は、XAML の変更を確認するたびに、アプリをビルドして配置する必要があります。 ホットリロードでは、XAML ファイルを保存すると、実行中のアプリに変更が反映されます。 また、ナビゲーションの状態とデータが保持されるため、アプリ内の場所を失うことなく、UI をすばやく繰り返すことができます。 そのため、XAML のホットリロードでは、UI の変更を検証するためのアプリの再構築とデプロイにかかる時間が短縮されます。

> [!NOTE]
> WPF または UWP アプリを作成する場合は、「 [uwp と wpf の XAML ホットリロード](/visualstudio/debugger/xaml-hot-reload)」を参照してください。
>
> Xamarin の XAML ホットリロードは、現在、Xamarin. Forms UWP プロジェクトでは使用_できません_。

## <a name="system-requirements"></a>システム要件

| IDE/フレームワーク | バージョンが必要です |
|------|------------------|
|Visual Studio 2019 | 16.4 以上
Visual Studio 2019 for Mac | 8.4 以上
Xamarin.Forms | 4.1 以上

## <a name="enable-xaml-hot-reload-for-xamarinforms"></a>Xamarin. Forms の XAML ホットリロードを有効にする

テンプレートから開始する場合は、XAML ホットリロードが既定で有効になり、プロジェクトは追加設定なしで動作するように構成されます。 Android または iOS エミュレーター、シミュレーター、または物理デバイスでアプリをデバッグし、XAML を変更して、XAML ホットリロードをトリガーするようにファイルを保存します。

既存の Xamarin. Forms ソリューションから作業している場合は、XAML ホットリロードを使用するための追加のインストールは必要ありませんが、最適なエクスペリエンスを得るために構成を再確認する必要がある場合があります。 まず、IDE の設定で有効にします。

* Windows の場合は、**ツール** > **オプション**の  **xamarin > ** **ホットリロード** > **xamarin のホットリロードを有効にする** チェックボックスをオンにします。
* Mac の場合は、 **Visual Studio**の [ > の**設定**] の [xamarin**の** **ホットリロードを有効にする**] チェックボックスをオンにして、xamarin > **XAML のホットリロード**を > ます。
  * 以前のバージョンの Visual Studio for Mac では、メニューは**Visual Studio** > **ユーザー設定** > **プロジェクト** > **Xamarin ホットリロード**) にあります。

次に、Android および iOS のビルド設定で、リンカーが "リンクしない" または "リンクなし" に設定されていることを確認します。 物理 iOS デバイスで XAML ホットリロードを使用するには、 **Mono インタープリターを有効**にする (visual studio 16.4 以上) か、追加の**mtouch 引数**に **--インタープリター**を追加する必要があります (visual studio 16.3 以下)。

次のフローチャートを使用すると、既存のプロジェクトのセットアップで XAML ホットリロードを使用するかどうかを確認できます。

![XAML ホットリロードセットアップ](hot-reload-images/hotreloadflowchart.png "XAML ホットリロードセットアップのフローチャート")

## <a name="resilient-reloading"></a>回復力のある再読み込み

XAML ホットリロードで再読み込みできないように変更すると、IntelliSense を使用してエラーメッセージが表示されます。 これらの変更 (ルード編集と呼ばれます) には、XAML のミスの修正や、存在しないイベントハンドラーへのコントロールの接続などがあります。 ルード編集の場合でも、アプリを再起動せずに再度読み込むことができます。 XAML ファイル内の別の場所で別の変更を行い、保存してください。 ルードエディットは再読み込みされませんが、その他の変更は引き続き適用されます。

## <a name="reload-on-multiple-platforms-at-once"></a>複数のプラットフォームで一度に再読み込みする

XAML ホットリロードは、Visual Studio と Visual Studio for Mac での同時デバッグをサポートしています。 Android と iOS ターゲットを同時にデプロイすると、両方のプラットフォームに同時に反映された変更を確認できます。 複数のプラットフォームでデバッグするには、次を参照してください。
* **Windows** [方法: 複数のスタートアッププロジェクトを設定する](https://docs.microsoft.com/visualstudio/ide/how-to-set-multiple-startup-projects?view=vs-2019)
* **Mac**で[複数のスタートアッププロジェクトを設定](https://docs.microsoft.com/visualstudio/mac/set-startup-projects?view=vsmac-2019)する

## <a name="known-limitations"></a>既知の制限事項

* UWP や MacOS など、その他の Xamarin 形式のターゲットは、まだサポートされて*いません*。 UWP サポートの進行状況は[ここで](https://developercommunity.visualstudio.com/idea/661682/xaml-hot-reload-for-xamarinforms-on-uwp.html)追跡できます。
* XAML ホットリロードセッション中に、ファイルまたは NuGet パッケージを追加、削除、または名前変更することはできません。 ファイルまたは NuGet パッケージを追加または削除する場合は、アプリケーションをリビルドして再デプロイし、引き続き XAML ホットリロードを使用します。
* 最適なエクスペリエンスを得るには、**リンクしない**ようにリンカーを設定するか、または**リンク**しないようにします。 [ **SDK のみをリンク**する] の設定はほとんどの場合に機能しますが、特定の場合には失敗する可能性があります。 リンカーの設定は、Android および iOS のビルドオプションにあります。
* 物理的な iPhone でデバッグを行うには、インタープリターで XAML ホットリロードを使用する必要があります。 これを行うには、プロジェクト設定を開き、iOS ビルド タブを選択し、 **Mono インタープリターを有効にする** 設定が有効になっていることを確認します。 場合によっては、プロパティページの上部にある**プラットフォーム**オプションを**iPhone**に変更する必要があります。
* `x:Name` 値を使用してコントロールを別のフィールドまたはプロパティに割り当てることによって作成された参照は、再読み込みされません。
* AppShell でシェルアプリケーションのビジュアル階層を更新すると、アプリケーションの状態を維持する際に問題が発生する可能性があります。 問題が発生した場合は、アプリをリビルドして再読み込みを続行します。
* XAML ホットリロードではC# 、イベントハンドラー、カスタムコントロール、ページ分離コード、およびその他のクラスを含むコードを再読み込みすることはできません。

## <a name="more-resources"></a>その他のリソース

* [XAML ホットリロードのヒントとコツ](https://devblogs.microsoft.com/xamarin/tips-tricks-xaml-hot-reload/)
* [Xamarin の XAML ホットリロード: Xamarin の表示](https://www.youtube.com/watch?v=crhjjPjzknk)

## <a name="troubleshooting"></a>トラブルシューティング

* XAML ホットリロードが初期化に失敗した場合:
  * Xamarin. Forms バージョンを更新します。
  * 最新バージョンの IDE を使用していることを確認します。
  * Android または iOS のリンカー設定をプロジェクトのビルド設定で**リンクしない**ように設定します。
* XAML ファイルの保存時に何も起こらない場合は、IDE で XAML ホットリロードが有効になっていることを確認します。
* 物理 iPhone でデバッグしているときに、アプリが応答しなくなった場合は、インタープリターが有効になっていることを確認します。 有効にするには、iOS のビルド設定で **[Mono インタープリターを有効に]** する (visual studio 16.4/8.4 以上) または 追加の **[mtouch 引数]** フィールド (visual studio 16.3/8.3 以前) に **[-インタープリター]** をオンにします。

バグを報告するには**Help** 、ヘルプ ** > 使用**して、Windows で**問題を報告**し > 、Mac で**問題を報告** ** > て**ください。
