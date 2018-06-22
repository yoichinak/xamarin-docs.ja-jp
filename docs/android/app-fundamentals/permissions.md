---
title: Permissions In Xamarin.Android
ms.prod: xamarin
ms.assetid: 3C440714-43E3-4D31-946F-CA59DAB303E8
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 03/09/2018
ms.openlocfilehash: b8a8005c69c8aaee5d92bdabb3429bd52fc76b4a
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
ms.locfileid: "30770236"
---
# <a name="permissions-in-xamarinandroid"></a>Permissions In Xamarin.Android


## <a name="overview"></a>概要

Android アプリケーションは、独自のサンド ボックスで実行され、セキュリティ上の理由から特定のシステム リソースまたはハードウェアへのアクセスのないデバイス。 これらのリソースで使用される前に、ユーザーはアプリへのアクセス許可を明示的に付与する必要があります。 たとえば、アプリケーション ユーザーの明示的な権限がないデバイスの GPS にアクセスできません。 Android がスローされます、`Java.Lang.SecurityException`アプリの許可なく保護されたリソースにアクセスしようとする場合。

アクセス許可が宣言されている、 **AndroidManifest.xml**アプリの開発時に、アプリケーション開発者によってです。 Android では、これらのアクセス許可、ユーザーの同意を取得するための 2 つの異なるワークフローがあります。
 
* Android 5.1 (API レベル 22) またはそれをターゲットとするアプリの場合、アプリのインストール時に、アクセス許可を要求が発生しました。 ユーザーがアクセス許可を付与しなかった場合、アプリがインストールされていません。 アプリがインストールされると、アプリのアンインストールによって以外のアクセス権を取り消す方法はありません。
* Android 6.0 (API level 23) 以降、ユーザーは詳細な制御権限を付与されました。許可またはアプリがデバイスにインストールされている限り、アクセス許可を取り消すことができます。 このスクリーン ショットは、Google 連絡先アプリのアクセス許可の設定を示します。 これにより、さまざまな権限を一覧表示され、有効にするにまたはアクセス許可を無効にすることができます。

![サンプルのアクセス許可 画面](permissions-images/01-permissions-check.png) 

Android アプリは、実行時に、保護されたリソースにアクセスするアクセス許可があるかどうかを参照してくださいチェックする必要があります。 アプリがアクセス許可を持たない場合、アクセス許可を付与するユーザーの Android SDK で提供される新しい Api を使用して要求を行う必要があります。 アクセス許可は、2 つのカテゴリに分けられます。

* **標準のアクセス許可**&ndash;権限をユーザーのセキュリティまたはプライバシーのほとんどのセキュリティ上の問題です。 Android 6.0 のインストール時に通常のアクセス許可が自動的に与えられます。 Android のドキュメントを参照してください、[通常のアクセス許可の完全な一覧](https://developer.android.com/guide/topics/permissions/normal-permissions.html)です。
* **危険なアクセス許可**&ndash;通常のアクセス許可と対照的に危険なアクセス許可は、ユーザーのセキュリティやプライバシーを保護しています。 明示的には、ユーザーが許可されたこれらでなければなりません。 SMS メッセージを送受信する危険なアクセス許可を必要とするアクションの例を示します。

> [!IMPORTANT]
> アクセス許可が属するカテゴリは、時間の経過と共に変更可能性があります。  「通常」のアクセス権がある可能性がありますに分類されたアクセス許可に上げること今後の API レベル危険なアクセス許可ことができます。

危険なアクセス許可がさらにサブ分割[_アクセス許可グループ_](https://developer.android.com/guide/topics/permissions/requesting.html#perm-groups)です。 アクセス許可グループでは、論理的に関連するアクセス許可を保持します。 ユーザーは、1 つのアクセス許可グループのメンバーに権限を付与、ときに Android は自動的にそのグループのすべてのメンバーに権限を付与します。 たとえば、 [ `STORAGE` ](https://developer.android.com/reference/android/Manifest.permission_group.html#STORAGE)アクセス許可グループでは、両方を保持して、`WRITE_EXTERNAL_STORAGE`と`READ_EXTERNAL_STORAGE`アクセス許可。 ユーザーに権限を付与する場合`READ_EXTERNAL_STORAGE`、続いて、`WRITE_EXTERNAL_STORAGE`権限は、同時に自動的に付与します。

1 つ以上のアクセス許可を要求する前に、アプリがアクセス許可を要求する前に、アクセス許可を必要な理由について根拠を提供することをお勧めします。 ユーザーは、理論的根拠を理解して後、アプリは、ユーザーからのアクセス許可を要求できます。 理論的根拠を理解すると、ユーザーできる情報に基づいて判断が存在しない場合への影響を理解している、権限を許可する場合。 

チェックとアクセス許可の要求のワークフロー全体と呼ばれる、_実行時の権限_を確認して、次の図で集計されることができます。 

[![実行時アクセス権チェックのフロー チャート](permissions-images/02-permissions-workflow-sml.png)](permissions-images/02-permissions-workflow.png#lightbox)

Android のサポート ライブラリ backports Android の以前のバージョンへのアクセス許可用の新しい Api の一部です。 API レベル チェックを毎回実行する必要はありませんので、これら移植された Api は自動的にデバイスで Android のバージョンに確認します。  

このドキュメントは Xamarin.Android アプリケーションにアクセス許可を追加する方法については説明、どのように Android 6.0 (API level 23) を対象とするアプリまたはそれ以上実行時アクセス許可チェックを実行する必要があります。


> [!NOTE]
> アプリは Google Play でフィルター処理する方法にハードウェアのアクセス許可が変わる可能性があります。 たとえば、アプリでは、カメラのアクセス許可が必要とする場合、Google Play は表示されませんアプリ カメラがインストールされていないデバイスで Google Play ストアで。


<a name="requirements" />

## <a name="requirements"></a>要件

Xamarin.Android プロジェクトが含まれていることを強くお勧め、 [Xamarin.Android.Support.Compat](https://www.nuget.org/packages/Xamarin.Android.Support.Compat/) NuGet パッケージです。 このパッケージは移植権限が提供する 1 つの一般的な Android の古いバージョンを特定の Api インターフェイスは、常にする必要はありませんは、Android でアプリが実行されているのバージョンを確認します。


## <a name="requesting-system-permissions"></a>システムのアクセス許可を要求します。

Android アクセス許可の操作の最初の手順では、マニフェスト ファイルを Android でのアクセス許可を宣言です。 これ行う必要があります API レベルに関係なく、アプリが対象とします。

またはアプリを対象に Android 6.0 以上できないために受け継がれることをユーザーは、アクセス許可が有効であること、次回までは、ある時点で権限を取得します。 Android 6.0 を対象とするアプリでは、ランタイム アクセス許可のチェックを常に実行する必要があります。 Android 5.1 または下限を対象とするアプリは、実行時アクセス許可チェックを実行する必要はありません。

> [!NOTE]
> アプリケーションは、必要なアクセスのみを要求する必要があります。


### <a name="declaring-permissions-in-the-manifest"></a>マニフェストにアクセス許可を宣言します。

追加の権限、 **AndroidManifest.xml**で、`uses-permission`要素。 たとえば、アプリケーションがデバイスの位置を検索する場合は、その微調整が必要ですし、コースの場所のアクセス許可。 次の 2 つの要素は、マニフェストに追加されます。 

```xml
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
```

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio に組み込まれているツールのサポートを使用してアクセス許可を宣言するには。

1. ダブルクリックして**プロパティ**で、**ソリューション エクスプ ローラー**を選択し、 **Android マニフェスト** タブで、プロパティ ウィンドウ。

    [![Android マニフェスト タブで必要なアクセス許可](permissions-images/04-required-permissions-vs-sml.png)](permissions-images/04-required-permissions-vs.png#lightbox)

2. クリックして、アプリケーションがまだない場合、AndroidManifest.xml、**いいえ AndroidManifest.xml が見つかりました。いずれかの追加 をクリックします。** 次のようにします。

    [![AndroidManifest.xml メッセージはありません。](permissions-images/05-no-manifest-vs-sml.png)](permissions-images/05-no-manifest-vs.png#lightbox)

3. アプリケーションが必要なアクセス許可を選択して、**アクセス許可を必要な**を一覧表示し、保存します。

    [![選択されているカメラ アクセス許可の例](permissions-images/06-selected-permission-vs-sml.png)](permissions-images/06-selected-permission-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Mac 用 Visual Studio に組み込まれているツールのサポートを使用してアクセス許可を宣言するには。

1. プロジェクトをダブルクリックして、**ソリューション パッド**選択**オプション > ビルド > Android アプリケーション**:

    [![必要なアクセス許可のセクションに表示](permissions-images/04-required-permissions-xs-sml.png)](permissions-images/04-required-permissions-xs.png#lightbox)

2. クリックして、 **Android マニフェストの追加**ボタン プロジェクトがまだない場合、 **AndroidManifest.xml**:

    [![プロジェクトの Android マニフェストが見つかりません](permissions-images/05-no-manifest-xs-sml.png)](permissions-images/05-no-manifest-xs.png#lightbox)

3. アプリケーションが必要なアクセス許可を選択して、**アクセス許可を必要な**を一覧表示し、をクリックして**OK**:

    [![選択されているカメラ アクセス許可の例](permissions-images/03-select-permission-xs-sml.png)](permissions-images/03-select-permission-xs.png#lightbox)
    
-----

Xamarin.Android が自動的に追加のアクセス許可ビルド時にデバッグ ビルドします。 これにより、簡単にアプリケーションをデバッグします。 2 つの主なアクセス許可は、具体的には、`INTERNET`と`READ_EXTERNAL_STORAGE`です。 これらに自動的にセットのアクセス許可で有効にするのには表示されません、**アクセス許可を必要な** ボックスの一覧です。 リリース ビルドをただしで明示的に設定されているアクセス許可のみを使用して、**アクセス許可を必要な** ボックスの一覧です。 

Android 5.1 (API レベル 22) または小文字を対象とするアプリの場合は詳細行う必要があります。 Android 6.0 (API 23 level 23) 以降を実行するアプリは、実行時の権限の確認を実行する方法には、次のセクションに進みます必要があります。 


### <a name="runtime-permission-checks-in-android-60"></a>Android 6.0 で実行時の権限を確認します。

`ContextCompat.CheckSelfPermission`メソッド (Android のサポート ライブラリで使用可能) を使用して特定の権限が与えられたかどうかをチェックします。 このメソッドは、 [ `Android.Content.PM.Permission` ](https://developer.xamarin.com/api/type/Android.Content.PM.Permission/) 2 つの値の 1 つを持つ列挙型。

* **`Permission.Granted`** &ndash; 指定したアクセス許可が与えられています。
* **`Permission.Denied`** &ndash; 指定した権限が許可されていません。

このコード スニペットは、アクティビティでカメラの権限をチェックする方法の例を示します。 

```csharp
if (ContextCompat.CheckSelfPermission(this, Manifest.Permission.Camera) == (int)Permission.Granted) 
{
    // We have permission, go ahead and use the camera.
} 
else 
{
    // Camera permission is not granted. If necessary display rationale & request.
}
```

アクセス許可が必要な理由、アプリケーションのアクセス許可を与える情報に基づいて判断できるようにしたり、ユーザーが通知をお勧めします。 この例は写真と geo タグを受け取るアプリ使用することにします。 カメラのアクセス許可が必要に応じて、ですが、分かりにくい、アプリでは、デバイスの場所も必要な理由は、ユーザーには明らかです。 論理的根拠は、ユーザーの場所のアクセス許可は、適切であったと、カメラのアクセス許可が必要である理由を理解するためにメッセージが表示する必要があります。

`ActivityCompat.ShouldShowRequestPermissionRational`論理的根拠をユーザーに表示するかどうかを判断するメソッドを使用します。 このメソッドは`true`した特定の権限の論理的根拠を表示する場合。 このスクリーン ショットは、アプリをデバイスの場所を知る必要がある理由を説明するアプリケーションによって表示される Snackbar の例を示します。

![場所の論理的根拠](permissions-images/07-rationale-snackbar.png) 

ユーザー、アクセス許可を付与する場合、`ActivityCompat.RequestPermissions(Activity activity, string[] permissions, int requestCode)`メソッドを呼び出す必要があります。 このメソッドには、次のパラメーターが必要です。

* **アクティビティ**&ndash;これは、アクセス許可を要求して、結果の Android によって通知するのには、するアクティビティ。
* **アクセス許可**&ndash;が要求されている、権限の一覧です。
* **requestCode** &ndash;へのアクセス許可の要求の結果を照合に使用される整数値、`RequestPermissions`呼び出します。 この値は、ゼロより大きい値である必要があります。

このコード スニペットは、説明した 2 つの方法の例を示します。 最初に、チェックを行うして、権限の論理的根拠を表示するかどうかを判断します。 表示される、論理的根拠がある場合、論理的根拠は、使用、Snackbar が表示されます。 ユーザーがクリックした場合**OK** Snackbar にし、アプリは、アクセス許可を要求します。 ユーザーは、論理的根拠を受け入れませんアクセス許可を要求するアプリは続行できません。 理論的根拠が表示されない場合、アクティビティは、アクセス許可が要求されます。

```csharp
if (ActivityCompat.ShouldShowRequestPermissionRationale(this, Manifest.Permission.AccessFineLocation)) 
{
    // Provide an additional rationale to the user if the permission was not granted
    // and the user would benefit from additional context for the use of the permission.
    // For example if the user has previously denied the permission.
    Log.Info(TAG, "Displaying camera permission rationale to provide additional context.");

    var requiredPermissions = new String[] { Manifest.Permission.AccessFineLocation };
    Snackbar.Make(layout, 
                   Resource.String.permission_location_rationale,
                   Snackbar.LengthIndefinite)
            .SetAction(Resource.String.ok, 
                       new Action<View>(delegate(View obj) {
                           ActivityCompat.RequestPermissions(this, requiredPermissions, REQUEST_LOCATION);
                       }    
            )
    ).Show();
}
else 
{
    ActivityCompat.RequestPermissions(this, new String[] { Manifest.Permission.Camera }, REQUEST_LOCATION);
}
```

`RequestPermission` ユーザーには、アクセス許可が既に与えられている場合でも呼び出すことができます。 後続の呼び出しは、必要ではありませんが、アクセス許可を確認する (または取り消す) する機会をユーザーに提供します。 ときに`RequestPermission`が呼び出されると、コントロールに渡されます、オペレーティング システム、アクセス許可を許可するための UI が表示されます。  

![アクセス許可 ダイアログ](permissions-images/08-location-permission-dialog.png)

Android がコールバック メソッドを使用して、アクティビティに結果を返すは、ユーザーが完了したら、`OnRequestPermissionResult`です。 このメソッドは、インターフェイスの一部分`ActivityCompat.IOnRequestPermissionsResultCallback`アクティビティで実装する必要がありますか。 このインターフェイスは 1 つのメソッド、`OnRequestPermissionsResult`ユーザーの選択肢のアクティビティを通知するために Android によって呼び出されます。 ユーザーがアクセス許可を与えられた場合、アプリできますさあ、保護されたリソースを使用しています。 実装する方法の例`OnRequestPermissionResult`を次に示します。 

```csharp
public override void OnRequestPermissionsResult(int requestCode, string[] permissions, Permission[] grantResults)
{
    if (requestCode == REQUEST_LOCATION) 
    {
        // Received permission result for camera permission.
        Log.Info(TAG, "Received response for Location permission request.");

        // Check if the only required permission has been granted
        if ((grantResults.Length == 1) && (grantResults[0] == Permission.Granted)) {
            // Location permission has been granted, okay to retrieve the location of the device.
            Log.Info(TAG, "Location permission has now been granted.");
            Snackbar.Make(layout, Resource.String.permision_available_camera, Snackbar.LengthShort).Show();            
        } 
        else 
        {
            Log.Info(TAG, "Location permission was NOT granted.");
            Snackbar.Make(layout, Resource.String.permissions_not_granted, Snackbar.LengthShort).Show();
        }
    } 
    else 
    {
        base.OnRequestPermissionsResult(requestCode, permissions, grantResults);
    }
}
```  


## <a name="summary"></a>まとめ

このガイドでは、追加し、Android デバイスでアクセス許可を確認する方法について説明します。 (API レベル < 23) 以前の Android アプリと Android アプリを新しい権限の動作の違い (API レベル 22 >)。 これは、Android 6.0 で実行時アクセス許可のチェックを実行する方法について説明します。


## <a name="related-links"></a>関連リンク

- [通常のアクセス許可の一覧](https://developer.android.com/guide/topics/permissions/normal-permissions.html)
- [ランタイムのアクセス許可のサンプル アプリ](https://github.com/xamarin/monodroid-samples/tree/master/android-m/RuntimePermissions)
- [Xamarin.Android でアクセス許可の処理](https://developer.xamarin.com/recipes/android/general/projects/add_permissions_to_android_manifest)
