---
title: パッケージウェアアプリ
ms.prod: xamarin
ms.assetid: E32DD855-78DD-46F8-B234-4EAC0756BDA2
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/02/2018
ms.openlocfilehash: fa35f6fe2388484875180594f18041947963ef7a
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70763976"
---
# <a name="packaging-wear-apps"></a>パッケージウェアアプリ

Android の摩耗アプリは、Google Play で配布するための完全な Android アプリでパッケージ化されます。 

## <a name="automatic-packaging"></a>自動パッケージング

Xamarin Android 5.0 以降では、ハンドヘルドプロジェクトから磨耗プロジェクトへのプロジェクト参照を作成するときに、磨耗アプリが自動的にリソースとしてハンドヘルドアプリにパッケージされます。 この関連付けを作成するには、次の手順を実行します。 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. お使いの摩耗アプリがまだハンドヘルドソリューションに含まれていない場合は、ソリューションノードを右クリックし、[追加]、[**既存のプロジェクトの追加 >** ...] の順に選択します。

2. 磨耗アプリの **.csproj**ファイルに移動して選択し、 **[開く]** をクリックします。 これで、お使いのハンドヘルドソリューションには、磨耗アプリプロジェクトが表示されます。

3. **[参照]** ノードを右クリックし、 **[参照の追加]** を選択します。

4. **[参照マネージャー]** ダイアログで、磨耗プロジェクトを有効にし (クリックしてチェックマークを追加します)、[ **OK]** をクリックします。

5. プロジェクトのパッケージ名を、ハンドヘルドプロジェクトのパッケージ名と一致するように変更します (パッケージ名は プロパティ の  **Android マニフェスト >** で変更できます)。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. お使いの摩耗アプリがまだハンドヘルドソリューションに含まれていない場合は、ソリューションノードを右クリックし、[追加]、[**既存のプロジェクトの追加 >** ...] の順に選択します。

2. 磨耗アプリの **.csproj**ファイルに移動して選択し、 **[開く]** をクリックします。 これで、お使いのハンドヘルドソリューションには、磨耗アプリプロジェクトが表示されます。

3. ソリューションでハンドヘルドプロジェクトノードを右クリックし、 **[参照の編集]** をクリックします。

4. **[参照の編集]** ダイアログで、磨耗プロジェクトを有効にし (クリックしてチェックマークを追加)、[ **OK]** をクリックします。

5. プロジェクトのパッケージ名を、ハンドヘルドプロジェクトのパッケージ名と一致するように変更します (パッケージ名は、 **[プロジェクトオプション > Android アプリケーション]** の下で変更できます)。

-----

磨耗アプリのパッケージ名が、ハンドヘルドアプリのパッケージ名と一致しない場合、 **XA5211**エラーが表示されることに注意してください。 例えば:

```shell
Error XA5211: Embedded wear app package name differs from handheld 
app package name (com.companyname.mywearapp != com.companyname.myapp). (XA5211)
```

このエラーを修正するには、ハンドヘルドアプリのパッケージ名と一致するように、磨耗アプリのパッケージ名を変更します。

**[ビルド > ビルド]** をクリックすると、この関連付けによって、磨耗プロジェクトの自動パッケージングがメインハンドヘルド (電話) プロジェクトにトリガーされます。 磨耗アプリは自動的にビルドされ、ハンドヘルドアプリのリソースとして含まれます。

磨耗アプリプロジェクトによって生成されるアセンブリは、ハンドヘルド (Phone) プロジェクトのアセンブリ参照としては使用されません。 代わりに、ビルドプロセスによって次の処理が行われます。

- パッケージ名が一致することを確認します。 

- は XML を生成し、それをハンドヘルドプロジェクトに追加して、それを磨耗アプリに関連付けます。 例えば: 

    ```xml
    <!-- Handheld (Phone) Project.csproj -->
    <ProjectReference Include="..\MyWearApp\MyWearApp.csproj">
        <Project>{D80E1FEF-653B-448C-B2AA-609C74E88340}</Project>
        <Name>MyWearApp</Name>
        <IsAppExtension>True</IsAppExtension>
    </ProjectReference>
    ```

- ハンドヘルドプロジェクトに**未加工**のリソースとして、磨耗アプリを追加します。 

## <a name="manual-packaging"></a>手動パッケージング

Android の摩耗アプリは、バージョン5.0 より前の Xamarin Android に記述できますが、アプリを配布するには、次の手動のパッケージ化手順に従う必要があります。 

1. ウェアラブルプロジェクトとハンドヘルド (電話) プロジェクトのバージョン番号とパッケージ名が同じであることを確認します。

2. ウェアラブルプロジェクトを**リリース**ビルドとして手動でビルドします。

3. リリースを手動で追加**します。** 手順 (2) から、ハンドヘルド (Phone) プロジェクトの**Resources/raw**ディレクトリに apk を挿入します。

4. 手順 (3) からウェアラブル**Apk**を参照する新しい Xml リソース**リソース/xml/Wearable_app_desc**を、ハンドヘルドプロジェクトに手動で追加します。

    ```xml
    <wearableApp package="wearable.app.package.name">
        <versionCode>1</versionCode>
        <versionName>1.0</versionName>
        <rawPathResId>NAME_OF_APK_FROM_STEP_3</rawPathResId>
    </wearableApp>
    ```

5. 新しい xml リソース`<meta-data />`を参照する要素をハンドヘルドプロジェクトの`<application>` **androidmanifest**要素に手動で追加します。

    ```xml
    <meta-data android:name="com.google.android.wearable.beta.app"
        android:resource="@xml/wearable_app_desc"/>
    ```

Android 開発者サイトの[マニュアルパックの手順](https://developer.android.com/training/wearables/apps/packaging.html#PackageManually)も参照してください。
