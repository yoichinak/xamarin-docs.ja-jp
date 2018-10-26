---
title: Wear アプリのパッケージ化
ms.prod: xamarin
ms.assetid: E32DD855-78DD-46F8-B234-4EAC0756BDA2
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/02/2018
ms.openlocfilehash: 585c276b327a9092bdd13fa633307477017558c5
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50104402"
---
# <a name="packaging-wear-apps"></a>Wear アプリのパッケージ化

Android Wear アプリは、Google Play で配布するための完全な Android アプリをパッケージ化されます。 

## <a name="automatic-packaging"></a>自動のパッケージ化

Xamarin Android 5.0 以降では、Wear アプリが自動的にパッケージ化ハンドヘルド アプリ内のリソースとして Wear プロジェクトに、ハンドヘルド プロジェクトからプロジェクト参照を作成するときにします。 この関連付けを作成するのに、次の手順を使用することができます。 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Wear アプリは、既にハンドヘルド ソリューションの一部ではない場合、は、ソリューション ノードを右クリックして**追加 > 既存のプロジェクトを追加しています.**.

2. 移動し、 **.csproj**ファイル Wear アプリの選択し、をクリックして**オープン**します。 Wear アプリ プロジェクトは、ハンドヘルド ソリューションの表示になります。

3. 右クリックし、**参照**ノード**参照の追加**します。

4. **参照マネージャー**ダイアログ ボックスで、Wear プロジェクト (クリックすると、チェック マークを追加します)、をクリックし、有効にする**OK**します。

5. ハンドヘルド プロジェクトのパッケージ名に一致するように損耗プロジェクトのパッケージ名を変更 (パッケージ名を変更することができます**プロパティ > Android マニフェスト**)。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. Wear アプリは、既にハンドヘルド ソリューションの一部ではない場合、は、ソリューション ノードを右クリックして**追加 > 既存のプロジェクトを追加しています.**.

2. 移動し、 **.csproj**ファイル Wear アプリの選択し、をクリックして**オープン**します。 Wear アプリ プロジェクトは、ハンドヘルド ソリューションの表示になります。

3. クリックして、ソリューションのハンドヘルド プロジェクト ノードを右クリックして**参照の編集.**.

4. **参照の編集**ダイアログ ボックスで、Wear プロジェクト (クリックすると、チェック マークを追加します)、をクリックし、有効にする**OK**します。

5. ハンドヘルド プロジェクトのパッケージ名に一致するように損耗プロジェクトのパッケージ名を変更 (パッケージ名を変更することができます**プロジェクト オプション > Android アプリケーション**)。

-----


Get は、 **XA5211** Wear アプリのパッケージ名がハンドヘルドのアプリのパッケージ名と一致しない場合のエラー。 例えば:

```shell
Error XA5211: Embedded wear app package name differs from handheld 
app package name (com.companyname.mywearapp != com.companyname.myapp). (XA5211)
```

このエラーを修正するには、ハンドヘルドのアプリのパッケージ名に一致するよう Wear アプリのパッケージ名を変更します。

クリックすると**ビルド > すべてビルド**、この関連付けは、メインのハンドヘルド (電話) プロジェクトに Wear プロジェクトの自動パッケージ化をトリガーします。 Wear アプリは自動的に構築し、ハンドヘルド アプリでリソースとして含まれます。

Wear アプリのプロジェクトで生成されるアセンブリは、ハンドヘルド (電話) プロジェクトのアセンブリ参照としては使用されません。 代わりに、ビルド プロセスは、次を行います。

-   パッケージの名前一致することを確認します。 

-   XML を生成し、Wear アプリとの関連付けのハンドヘルド プロジェクトに追加します。 例えば: 

    ```xml
    <!-- Handheld (Phone) Project.csproj -->
    <ProjectReference Include="..\MyWearApp\MyWearApp.csproj">
        <Project>{D80E1FEF-653B-448C-B2AA-609C74E88340}</Project>
        <Name>MyWearApp</Name>
        <IsAppExtension>True</IsAppExtension>
    </ProjectReference>
    ```

-   Wear アプリとしての追加、**生**ハンドヘルド プロジェクトにリソース。 


## <a name="manual-packaging"></a>手動のパッケージ化

前のバージョン 5.0 では、に、Xamarin.Android で Android Wear アプリを記述することができますが、アプリを配布するには、この手動のパッケージ化手順に従う必要があります。 

1. ウェアラブル プロジェクトとハンドヘルド (電話) プロジェクトに同じバージョン番号とパッケージ名があることを確認します。

2. 手動でプロジェクトをビルド、ウェアラブルとして、**リリース**ビルドします。

3. リリースを手動で追加**します。APK**から (2) にステップ イン、 **resources/raw**ハンドヘルド (電話) プロジェクトのディレクトリ。

4. 新しい XML リソースを手動で追加**Resources/xml/wearable_app_desc.xml**ハンドヘルド プロジェクトを参照するウェアラブルで**APK**手順 (3)。

    ```xml
    <wearableApp package="wearable.app.package.name">
        <versionCode>1</versionCode>
        <versionName>1.0</versionName>
        <rawPathResId>NAME_OF_APK_FROM_STEP_3</rawPathResId>
    </wearableApp>
    ```

5. 手動で追加、`<meta-data />`をハンドヘルド プロジェクトの要素**AndroidManifest.xml** `<application>`新しい XML リソースを参照する要素。

    ```xml
    <meta-data android:name="com.google.android.wearable.beta.app"
        android:resource="@xml/wearable_app_desc"/>
    ```

Android Developer サイトのも参照してください。[手動 packging 指示](https://developer.android.com/training/wearables/apps/packaging.html#PackageManually)します。

