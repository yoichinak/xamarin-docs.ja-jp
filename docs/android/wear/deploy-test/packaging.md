---
title: 消耗アプリのパッケージ化
ms.prod: xamarin
ms.assetid: E32DD855-78DD-46F8-B234-4EAC0756BDA2
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/02/2018
ms.openlocfilehash: af96c0f8cf862b7a208beb5b91ecbb30598b09d9
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
ms.locfileid: "30767714"
---
# <a name="packaging-wear-apps"></a>消耗アプリのパッケージ化

Android アプリを損傷は、Google play 内の配布用の完全 Android アプリのパッケージ化されます。 

## <a name="automatic-packaging"></a>自動のパッケージ化

Xamarin Android 5.0 以降では、消耗アプリは自動的にパッケージ化ハンドヘルド アプリのリソースとして消耗プロジェクトに、ハンドヘルド プロジェクトからプロジェクト参照を作成する場合。 次の手順を使用すると、この関連付けを作成します。 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. 消耗アプリが既にに属していない、ハンドヘルド ソリューションの場合、ソリューション ノードを右クリックし、**追加 > 既存のプロジェクトを追加しています.**.

2. 移動し、 **.csproj**ファイル消耗アプリの選択し、をクリックして**開く**です。 消耗アプリ プロジェクトが今すぐハンドヘルド ソリューションで表示されます。

3. 右クリックし、**参照**ノード**参照の追加**です。

4. **参照マネージャー**ダイアログで、有効にする、消耗プロジェクト (クリックしてチェック マークを追加する) をクリックして**OK**です。

5. ハンドヘルド プロジェクトのパッケージ名に一致する消耗プロジェクトのパッケージ名の変更 (パッケージの名前を変更することができます**プロパティ > Android マニフェスト**)。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. 消耗アプリが既にに属していない、ハンドヘルド ソリューションの場合、ソリューション ノードを右クリックし、**追加 > 既存のプロジェクトを追加しています.**.

2. 移動し、 **.csproj**ファイル消耗アプリの選択し、をクリックして**開く**です。 消耗アプリ プロジェクトが今すぐハンドヘルド ソリューションで表示されます。

3. ハンドヘルドでプロジェクト ノードをクリックして、ソリューションを右クリックして**参照の編集.**.

4. **参照の編集**ダイアログで、有効にする、消耗プロジェクト (クリックしてチェック マークを追加する) をクリックして**OK**です。

5. ハンドヘルド プロジェクトのパッケージ名に一致する消耗プロジェクトのパッケージ名の変更 (パッケージの名前を変更することができます**プロジェクトのオプション > Android アプリケーション**)。

-----


表示されることに注意してください、 **XA5211**消耗アプリのパッケージ名が、パッケージ アプリの名前、ハンドヘルドと一致しない場合はエラーです。 例えば:

```shell
Error XA5211: Embedded wear app package name differs from handheld 
app package name (com.companyname.mywearapp != com.companyname.myapp). (XA5211)
```

このエラーを解決するには、ハンドヘルド アプリのパッケージ名に一致する消耗アプリのパッケージ名を変更します。

クリックすると、**ビルド > すべてビルド**、この関連付けが消耗プロジェクトのメイン ハンドヘルド (Phone) プロジェクトに自動のパッケージをトリガーします。 消耗アプリは自動的に構築し、ハンドヘルド アプリでリソースとして含まれます。

消耗アプリ プロジェクトで生成されるアセンブリは、ハンドヘルド (Phone) のプロジェクト内のアセンブリ参照としては使用されません。 代わりに、ビルド プロセスは次のとおり

-   パッケージ名前と一致していることを確認します。 

-   XML を生成し、これをアプリに関連付けて、消耗ハンドヘルド プロジェクトに追加します。 例えば: 

    ```xml
    <!-- Handheld (Phone) Project.csproj -->
    <ProjectReference Include="..\MyWearApp\MyWearApp.csproj">
        <Project>{D80E1FEF-653B-448C-B2AA-609C74E88340}</Project>
        <Name>MyWearApp</Name>
        <IsAppExtension>True</IsAppExtension>
    </ProjectReference>
    ```

-   として消耗アプリを追加、**生**ハンドヘルド プロジェクトにリソース。 


## <a name="manual-packaging"></a>手動のパッケージ化

バージョン 5.0 では、前に Xamarin.Android で着用して Android アプリを記述することができますが、アプリを配布するには、この手動のパッケージ化手順に従う必要があります。 

1. ウェアラブル プロジェクトとハンドヘルド (Phone) プロジェクトに同じバージョン番号とパッケージの名前があることを確認します。

2. 手動でプロジェクトをビルド、ウェアラブルとして、**リリース**を構築します。

3. リリースを手動で追加**です。APK**にステップ イン (2) から、 **/生のリソース**ハンドヘルド (Phone) プロジェクトのディレクトリ。

4. 新しい XML リソースを手動で追加**Resources/xml/wearable_app_desc.xml**ウェアラブルを参照するハンドヘルド プロジェクトで**APK**手順 (3)。

    ```xml
    <wearableApp package="wearable.app.package.name">
        <versionCode>1</versionCode>
        <versionName>1.0</versionName>
        <rawPathResId>NAME_OF_APK_FROM_STEP_3</rawPathResId>
    </wearableApp>
    ```

5. 手動で追加、`<meta-data />`ハンドヘルド プロジェクトの要素**AndroidManifest.xml** `<application>`新しい XML リソースを参照する要素。

    ```xml
    <meta-data android:name="com.google.android.wearable.beta.app"
        android:resource="@xml/wearable_app_desc"/>
    ```

Android Developer サイトのも参照してください。[手動 packging 指示](https://developer.android.com/training/wearables/apps/packaging.html#PackageManually)です。

