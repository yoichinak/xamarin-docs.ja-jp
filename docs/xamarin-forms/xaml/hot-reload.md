---
title: ' XAML ホットリロード for Xamarin.Forms ' description: ' 実行中のアプリケーションに直ちに xaml ファイルへの変更を再度読み込みます。これにより、すべての xaml の変更後にプロジェクトをビルドする必要がなく Xamarin.Forms なります。 '
ms. 製品: ms. assetid: ms. テクノロジ: 作成者: ms. 作成者: ms. 日付: なし:
- 'Xamarin.Forms'
- 'Xamarin.Essentials'

---

# <a name="xaml-hot-reload-for-xamarinforms"></a>XAML ホットリロード ()Xamarin.Forms

XAML ホットリロードは、生産性を向上させ、時間を節約するために、既存のワークフローにプラグインします。 Xaml のホットリロードを使用しない場合は、XAML の変更を確認するたびに、アプリをビルドして配置する必要があります。 ホットリロードでは、XAML ファイルを保存すると、実行中のアプリに変更が反映されます。 また、ナビゲーションの状態とデータが保持されるため、アプリ内の場所を失うことなく、UI をすばやく繰り返すことができます。 そのため、XAML のホットリロードでは、UI の変更を検証するためのアプリの再構築とデプロイにかかる時間が短縮されます。

> [!NOTE]
> WPF または UWP アプリを作成する場合は、「 [uwp と wpf の XAML ホットリロード](/visualstudio/debugger/xaml-hot-reload)」を参照してください。
>
> の XAML ホットリロード Xamarin.Forms は、現在 UWP プロジェクトに対しては機能し_ません_ Xamarin.Forms 。

## <a name="system-requirements"></a>システム要件

| IDE/フレームワーク | バージョンが必要です |
|---
title: ' XAML ホットリロード for Xamarin.Forms ' description: ' 実行中のアプリケーションに直ちに xaml ファイルへの変更を再度読み込みます。これにより、すべての xaml の変更後にプロジェクトをビルドする必要がなく Xamarin.Forms なります。 '
ms. 製品: ms. assetid: ms. テクノロジ: 作成者: ms. 作成者: ms. 日付: なし:
- 'Xamarin.Forms'
- 'Xamarin.Essentials'

---|---title: ' XAML ホットリロード for Xamarin.Forms ' description: ' 実行中のアプリケーションで xaml ファイルへの変更を直ちに再読み込みするため、xaml の変更が行われるたびにプロジェクトをビルドする必要はありませ Xamarin.Forms ん。 '
ms. 製品: ms. assetid: ms. テクノロジ: 作成者: ms. 作成者: ms. 日付: なし:
- 'Xamarin.Forms'
- 'Xamarin.Essentials'

-
title: ' XAML ホットリロード for Xamarin.Forms ' description: ' 実行中のアプリケーションに直ちに xaml ファイルへの変更を再度読み込みます。これにより、すべての xaml の変更後にプロジェクトをビルドする必要がなく Xamarin.Forms なります。 '
ms. 製品: ms. assetid: ms. テクノロジ: 作成者: ms. 作成者: ms. 日付: なし:
- 'Xamarin.Forms'
- 'Xamarin.Essentials'

-
title: ' XAML ホットリロード for Xamarin.Forms ' description: ' 実行中のアプリケーションに直ちに xaml ファイルへの変更を再度読み込みます。これにより、すべての xaml の変更後にプロジェクトをビルドする必要がなく Xamarin.Forms なります。 '
ms. 製品: ms. assetid: ms. テクノロジ: 作成者: ms. 作成者: ms. 日付: なし:
- 'Xamarin.Forms'
- 'Xamarin.Essentials'

-
title: ' XAML ホットリロード for Xamarin.Forms ' description: ' 実行中のアプリケーションに直ちに xaml ファイルへの変更を再度読み込みます。これにより、すべての xaml の変更後にプロジェクトをビルドする必要がなく Xamarin.Forms なります。 '
ms. 製品: ms. assetid: ms. テクノロジ: 作成者: ms. 作成者: ms. 日付: なし:
- 'Xamarin.Forms'
- 'Xamarin.Essentials'

-
title: ' XAML ホットリロード for Xamarin.Forms ' description: ' 実行中のアプリケーションに直ちに xaml ファイルへの変更を再度読み込みます。これにより、すべての xaml の変更後にプロジェクトをビルドする必要がなく Xamarin.Forms なります。 '
ms. 製品: ms. assetid: ms. テクノロジ: 作成者: ms. 作成者: ms. 日付: なし:
- 'Xamarin.Forms'
- 'Xamarin.Essentials'

-
title: ' XAML ホットリロード for Xamarin.Forms ' description: ' 実行中のアプリケーションに直ちに xaml ファイルへの変更を再度読み込みます。これにより、すべての xaml の変更後にプロジェクトをビルドする必要がなく Xamarin.Forms なります。 '
ms. 製品: ms. assetid: ms. テクノロジ: 作成者: ms. 作成者: ms. 日付: なし:
- 'Xamarin.Forms'
- 'Xamarin.Essentials'

-
title: ' XAML ホットリロード for Xamarin.Forms ' description: ' 実行中のアプリケーションに直ちに xaml ファイルへの変更を再度読み込みます。これにより、すべての xaml の変更後にプロジェクトをビルドする必要がなく Xamarin.Forms なります。 '
ms. 製品: ms. assetid: ms. テクノロジ: 作成者: ms. 作成者: ms. 日付: なし:
- 'Xamarin.Forms'
- 'Xamarin.Essentials'

---------| |Visual Studio 2019 |16.4 以上の Visual Studio 2019 (Mac の場合) |8.4 以上 Xamarin.Forms | 4.1 以上

## <a name="enable-xaml-hot-reload-for-xamarinforms"></a>XAML ホットリロードを有効にするXamarin.Forms

テンプレートから開始する場合は、XAML ホットリロードが既定で有効になり、プロジェクトは追加設定なしで動作するように構成されます。 Android または iOS エミュレーター、シミュレーター、または物理デバイスでアプリをデバッグし、XAML を変更して、XAML ホットリロードをトリガーするようにファイルを保存します。

既存のソリューションから作業している場合は Xamarin.Forms 、XAML ホットリロードを使用するための追加のインストールは必要ありませんが、最適なエクスペリエンスを得るために構成を再確認する必要がある場合があります。 まず、IDE の設定で有効にします。

* Windows の場合は、[**ツール**] [オプション] [xamarin ホットリロード] の [ **xamarin ホットリロードを有効にする**] チェックボックスをオンにし  >  **Options**  >  **Xamarin**  >  **Hot Reload**ます。
* Mac の場合**は、** **Visual Studio**の [  >  **基本設定**] [xamarin  >  XAML**用ツール**] [  >  **ホットリロード**] のチェックボックスをオンにします。
  * 以前のバージョンの Visual Studio for Mac では、メニューは**Visual Studio**の  >  **ユーザー設定**  >  **プロジェクト**  >  **Xamarin のホットリロード**にあります。

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

* UWP や macOS などの他の Xamarin.Forms ターゲットは、まだサポートされて*いません*。 UWP サポートの進行状況は[ここで](https://developercommunity.visualstudio.com/idea/661682/xaml-hot-reload-for-xamarinforms-on-uwp.html)追跡できます。
* XAML ホットリロードセッション中に、ファイルまたは NuGet パッケージを追加、削除、または名前変更することはできません。 ファイルまたは NuGet パッケージを追加または削除する場合は、アプリケーションをリビルドして再デプロイし、引き続き XAML ホットリロードを使用します。
* 最適なエクスペリエンスを得るには、**リンクしない**ようにリンカーを設定するか、または**リンク**しないようにします。 [ **SDK のみをリンク**する] の設定はほとんどの場合に機能しますが、特定の場合には失敗する可能性があります。 リンカーの設定は、Android および iOS のビルドオプションにあります。
* 物理的な iPhone でデバッグを行うには、インタープリターで XAML ホットリロードを使用する必要があります。 これを行うには、プロジェクト設定を開き、[iOS ビルド] タブを選択し、[ **Mono インタープリターを有効にする**] 設定が有効になっていることを確認します。 場合によっては、プロパティページの上部にある**プラットフォーム**オプションを**iPhone**に変更する必要があります。
* 値を使用してコントロールを別のフィールドまたはプロパティに割り当てることによって作成された参照は、再 `x:Name` 読み込みされません。
* AppShell でシェルアプリケーションのビジュアル階層を更新すると、アプリケーションの状態を維持する際に問題が発生する可能性があります。 問題が発生した場合は、アプリをリビルドして再読み込みを続行します。
* XAML ホットリロードでは、イベントハンドラー、カスタムコントロール、ページ分離コード、およびその他のクラスを含む C# コードを再読み込みすることはできません。

## <a name="more-resources"></a>その他のリソース

* [XAML ホットリロードのヒントとコツ](https://devblogs.microsoft.com/xamarin/tips-tricks-xaml-hot-reload/)
* [詳細については、XAML のホットリロード Xamarin.Forms : Xamarin の表示](https://www.youtube.com/watch?v=crhjjPjzknk)

## <a name="troubleshooting"></a>トラブルシューティング

* XAML ホットリロードが初期化に失敗した場合:
  * Xamarin.Formsバージョンを更新します。
  * 最新バージョンの IDE を使用していることを確認します。
  * Android または iOS のリンカー設定をプロジェクトのビルド設定で**リンクしない**ように設定します。
* XAML ファイルの保存時に何も起こらない場合は、IDE で XAML ホットリロードが有効になっていることを確認します。
* 物理 iPhone でデバッグしているときに、アプリが応答しなくなった場合は、インタープリターが有効になっていることを確認します。 有効にするには、iOS のビルド設定で [ **Mono インタープリターを有効に**する (visual studio 16.4/8.4 以上)] または [追加の**mtouch 引数**] フィールド (visual studio 16.3/8.3 以前) に [ **-インタープリター** ] をオンにします。

バグを報告するには、**ヘルプ**を使用して  >  **Send Feedback**  >  Windows で**問題**を報告し、 **Help**  >  Mac で**問題を報告**してください。
