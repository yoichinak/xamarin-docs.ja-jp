---
title: Xamarin.Android でのアクセス許可
ms.prod: xamarin
ms.assetid: 3C440714-43E3-4D31-946F-CA59DAB303E8
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 03/09/2018
ms.openlocfilehash: 3e12aa47404d8ee4e52ddada3d99f91250e6c54d
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242265"
---
# <a name="permissions-in-xamarinandroid"></a>Xamarin.Android でのアクセス許可


## <a name="overview"></a>概要

Android アプリケーションが独自のサンド ボックスで実行し、セキュリティ上の理由からはアクセスできない特定のシステム リソースやハードウェア デバイスでします。 これらのリソースを使用する前に、ユーザーは、アプリへのアクセス許可を明示的に付与する必要があります。 たとえば、アプリケーション、ユーザーから明示的なアクセス許可がないデバイスに GPS にアクセスできません。 Android がスローされます、`Java.Lang.SecurityException`アプリでは、アクセス許可がない保護されたリソースにアクセスしようとするとします。

アクセス許可が宣言されている、 **AndroidManifest.xml**アプリの開発時に、アプリケーション開発者。 Android では、これらのアクセス許可、ユーザーの同意を取得するための 2 つの異なるワークフローがあります。
 
* アプリのインストール時に、Android 5.1 (API レベル 22) または下を対象とするアプリに対して、アクセス許可を要求が発生しました。 ユーザーがアクセス許可を付与しなかった場合、アプリはインストールされません。 アプリがインストールされると、アプリをアンインストールすることによって以外のアクセス権を失効させる方法はありません。
* Android 6.0 (API レベル 23) 以降、ユーザーはさらに制御; のアクセス許可を付与されました。許可またはデバイスにアプリがインストールされている限り、アクセス許可を取り消すことができます。 このスクリーン ショットは、Google の連絡先アプリのアクセス許可の設定を示しています。 さまざまなアクセス許可の一覧し、有効またはアクセス許可を無効にできます。

![サンプルのアクセス許可 画面](permissions-images/01-permissions-check.png) 

Android アプリは、保護されたリソースにアクセスするためのアクセス許可がないか確認する実行時に確認する必要があります。 アプリがアクセス許可を持たない場合、アクセス許可を付与するユーザーの Android SDK で提供される新しい Api を使用して要求を作成する必要があります。 アクセス許可は、2 つのカテゴリに分類されます。

* **通常のアクセス許可**&ndash;これらはアクセス許可をユーザーのセキュリティまたはプライバシーのほとんどのセキュリティ上の問題。 Android 6.0 のインストール時に通常のアクセス許可が自動的に与えられます。 Android のドキュメントを参照してください、[通常のアクセス許可の完全な一覧](https://developer.android.com/guide/topics/permissions/normal-permissions.html)します。
* **危険なアクセス許可**&ndash;危険なアクセス許可では通常のアクセス許可とは異なり、ユーザーのセキュリティやプライバシーを保護します。 明示的には、ユーザーが許可されているこれらする必要があります。 SMS メッセージを送受信する危険なアクセス許可を必要とするアクションの例に示します。

> [!IMPORTANT]
> アクセス許可が属するカテゴリは、時間の経過と共に変更可能性があります。  「標準」のアクセス許可に分類されたがこのアクセス許可は、将来的に API レベルに昇格危険なアクセス許可ことができます。

さらには危険なアクセス許可は分割[_アクセス許可グループ_](https://developer.android.com/guide/topics/permissions/requesting.html#perm-groups)します。 アクセス許可グループでは、論理的に関連するアクセス許可を保持します。 ユーザーは、1 つのアクセス許可グループのメンバーに権限を付与、ときに Android は自動的にそのグループのすべてのメンバーに権限を付与します。 たとえば、 [ `STORAGE` ](https://developer.android.com/reference/android/Manifest.permission_group.html#STORAGE)両方を保持するアクセス許可グループ、`WRITE_EXTERNAL_STORAGE`と`READ_EXTERNAL_STORAGE`アクセス許可。 ユーザーに権限を付与する場合`READ_EXTERNAL_STORAGE`、`WRITE_EXTERNAL_STORAGE`と同時にアクセス許可が自動的に付与します。

1 つ以上のアクセス許可を要求する前にこれは、アプリがアクセス許可を要求する前に、アクセス許可を必要と理由についての根拠を提供することをお勧めします。 ユーザーは、"the rationale"を認識すると、アプリは、ユーザーからアクセス許可を要求できます。 "The rationale"を理解すると、ユーザーはアクセス許可を付与し、一致しない場合、影響を理解する場合に十分な情報に基づいて判断を行うことができます。 

ワークフロー全体をチェックし、アクセス許可の要求と呼ばれる、_実行時のアクセス許可_チェックして、次の図に要約できます。 

[![実行時の権限チェックのフロー チャート](permissions-images/02-permissions-workflow-sml.png)](permissions-images/02-permissions-workflow.png#lightbox)

Android サポート ライブラリ backports 以前のバージョンの Android へのアクセス許可の新しい Api の一部です。 これらのバックポート Api では、API レベル チェックするたびに実行する必要はありませんので、デバイスの Android のバージョンは自動的にチェックします。  

このドキュメントは、Xamarin.Android アプリケーションにアクセス許可を追加する方法を説明する方法と Android 6.0 (API レベル 23) を対象とするアプリ以降、実行時のアクセス許可チェックを実行する必要がありますか。


> [!NOTE]
> ハードウェアのアクセス許可が、アプリが Google Play でフィルター処理する方法に影響する可能性があります。 たとえば、アプリでは、カメラのアクセス許可が必要とする場合、Google Play は表示されませんアプリ カメラがインストールされていないデバイスで Google Play ストアで。


<a name="requirements" />

## <a name="requirements"></a>必要条件

Xamarin.Android プロジェクトが含まれていることを強くお勧め、 [Xamarin.Android.Support.Compat](https://www.nuget.org/packages/Xamarin.Android.Support.Compat/) NuGet パッケージ。 このパッケージはバックポート権限が 1 つの一般的なを提供する Android の古いバージョンを特定の Api インターフェイスは、常にする必要はありませんが、アプリを実行する Android のバージョンを確認します。


## <a name="requesting-system-permissions"></a>システムのアクセス許可を要求します。

Android のアクセス許可を持つ操作するために、まず、アクセス許可を android マニフェスト ファイルを宣言することです。 必要がありますこれ API レベルに関係なく、アプリが対象とします。

Android 6.0 のターゲットまたは高いことはできませんといって、ユーザーを許可するアクセス許可が有効になります次に、過去のある時点でのアクセス許可をアプリ。 Android 6.0 を対象とするアプリでは、実行時のアクセス許可チェックを常に実行する必要があります。 5.1 またはそれ以下の Android を対象とするアプリは、実行時のアクセス許可チェックを実行する必要はありません。

> [!NOTE]
> アプリケーションでは、必要なアクセス許可を要求する必要がありますのみ。


### <a name="declaring-permissions-in-the-manifest"></a>マニフェストにアクセス許可を宣言します。

アクセス許可を追加、 **AndroidManifest.xml**で、`uses-permission`要素。 たとえば、アプリケーションがデバイスの位置を検索する場合に微調整が必要ですし、コースの場所のアクセス許可。 次の 2 つの要素は、マニフェストに追加されます。 

```xml
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
```

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio に組み込まれているツールのサポートを使用してアクセス許可を宣言することができます。

1. ダブルクリック**プロパティ**で、**ソリューション エクスプ ローラー**を選択し、 **Android マニフェスト**プロパティ ウィンドウでタブ。

    [![Android マニフェスト タブで必要なアクセス許可](permissions-images/04-required-permissions-vs-sml.png)](permissions-images/04-required-permissions-vs.png#lightbox)

2. アプリケーションにまだ、AndroidManifest.xml がない場合は、クリックして**いいえ AndroidManifest.xml が見つかりません。1 つ追加する をクリックします。** 次に示すよう。

    [![AndroidManifest.xml メッセージなし](permissions-images/05-no-manifest-vs-sml.png)](permissions-images/05-no-manifest-vs.png#lightbox)

3. アプリケーションが必要なすべてのアクセス許可の選択、**アクセス許可が必要な**を一覧表示し、保存します。

    [![選択の例のカメラのアクセス許可](permissions-images/06-selected-permission-vs-sml.png)](permissions-images/06-selected-permission-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Mac 用 Visual Studio に組み込まれたツールのサポートを使用してアクセス許可を宣言することができます。

1. プロジェクトをダブルクリックして、 **Solution Pad**選択**オプション > ビルド > Android アプリケーション**:

    [![必要なアクセス許可のセクションに表示](permissions-images/04-required-permissions-xs-sml.png)](permissions-images/04-required-permissions-xs.png#lightbox)

2. をクリックして、 **Android マニフェストの追加**ボタン、プロジェクトがまだない場合、 **AndroidManifest.xml**:

    [![プロジェクトの Android マニフェストが見つかりません](permissions-images/05-no-manifest-xs-sml.png)](permissions-images/05-no-manifest-xs.png#lightbox)

3. アプリケーションが必要なすべてのアクセス許可の選択、**アクセス許可が必要な**を一覧表示し、をクリックして **[ok]**:

    [![選択の例のカメラのアクセス許可](permissions-images/03-select-permission-xs-sml.png)](permissions-images/03-select-permission-xs.png#lightbox)
    
-----

Xamarin.Android は一部のアクセス許可をデバッグ ビルドをビルド時に追加自動的には。 これにより、簡単にアプリケーションをデバッグします。 注目すべき 2 つのアクセス許可は、具体的には、`INTERNET`と`READ_EXTERNAL_STORAGE`します。 これらのセットに自動的にアクセス許可で有効にするのには表示されません、**アクセス許可が必要な**一覧。 リリース ビルドただしで明示的に設定されているアクセス許可のみを使用して、**アクセス許可が必要な**一覧。 

Android 5.1 (API レベル 22) またはそれ以下の対象とするアプリで何もない詳細を行う必要があります。 Android 6.0 (API 23 レベル 23) 以降を実行するアプリは、実行時の権限のチェックを実行する方法については、次のセクションに進む必要があります。 


### <a name="runtime-permission-checks-in-android-60"></a>Android 6.0 で実行時アクセス許可を確認します

`ContextCompat.CheckSelfPermission`メソッド (Android サポート ライブラリで使用可能) を使用して特定のアクセス許可が付与されていないかどうかをチェックします。 このメソッドは、 [ `Android.Content.PM.Permission` ](https://developer.xamarin.com/api/type/Android.Content.PM.Permission/) 2 つの値の 1 つを持つ列挙型。

* **`Permission.Granted`** &ndash; 指定した権限を与えられています。
* **`Permission.Denied`** &ndash; 指定したアクセス許可が付与されていません。

このコード スニペットでは、アクティビティでカメラの権限をチェックする方法の例を示します。 

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

アクセス許可が必要な理由、アプリケーションのアクセス許可を与える情報に基づいて判断できるようにしたり、ユーザーに通知することをお勧めします。 写真と geo タグを受け取るアプリの場合、この例にします。 カメラのアクセス許可が必要に応じてが、そのわかりにくい、アプリは、デバイスの場所も必要があります。 そのため、ユーザーには明らかです。 "The rationale"では、ユーザーの場所のアクセス許可は、望ましいことと、カメラのアクセス許可が必要である理由を理解するためにメッセージを表示する必要があります。

`ActivityCompat.ShouldShowRequestPermissionRational` "The rationale"をユーザーに表示するかどうかを判断するメソッドを使用します。 このメソッドが返す`true`場合、指定されたアクセス許可の"the rationale"を表示する必要があります。 このスクリーン ショットでは、アプリがデバイスの場所を知る必要な理由を説明するアプリケーションによって表示される Snackbar の例を示します。

![場所の論理的根拠](permissions-images/07-rationale-snackbar.png) 

ユーザー、アクセス許可を付与する場合、`ActivityCompat.RequestPermissions(Activity activity, string[] permissions, int requestCode)`メソッドを呼び出す必要があります。 このメソッドでは、次のパラメーターが必要です。

* **アクティビティ**&ndash;これは、アクセス許可を要求し、結果の Android で通知するアクティビティ。
* **アクセス許可**&ndash;が、要求されているアクセス許可の一覧。
* **requestCode** &ndash;へのアクセス許可要求の結果を照合に使用される整数値、`RequestPermissions`呼び出します。 この値は、ゼロより大きい値である必要があります。

このコード スニペットでは、説明した 2 つの方法の例を示します。 最初に、アクセス許可"the rationale"を表示するかどうかを判断する確認が実行されます。 "The rationale"を表示する場合は、Snackbar が"the rationale"で表示されます。 ユーザーがクリックした場合**OK** Snackbar にし、アプリは、アクセス許可を要求します。 ユーザーが"the rationale"を受け入れませんアクセス許可を要求するアプリは続行されません。 "The rationale"が表示されない場合、アクティビティは、アクセス許可を要求します。

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

`RequestPermission` ユーザーには、アクセス許可が既に与えられている場合でも呼び出すことができます。 後続の呼び出しは必要ありませんが、ユーザー、アクセス許可を確認します (または取り消す) する機会を提供します。 ときに`RequestPermission`が呼び出されると、コントロールに渡されます、オペレーティング システムのアクセス許可を受け入れるため、UI が表示されます。  

![アクセス許可 ダイアログ](permissions-images/08-location-permission-dialog.png)

Android が、コールバック メソッドを使用して、アクティビティに結果を返すは、ユーザーが完了すると、`OnRequestPermissionResult`します。 このメソッドは、インターフェイスの一部`ActivityCompat.IOnRequestPermissionsResultCallback`アクティビティによって実装されている必要があります。 このインターフェイスは、1 つのメソッドを持って`OnRequestPermissionsResult`ユーザーの選択肢のアクティビティを通知するために Android によって呼び出されます。 ユーザーがアクセス許可を与えられた場合、アプリことができます進んでし、保護されたリソースを使用します。 実装する方法の例`OnRequestPermissionResult`を次に示します。 

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

このガイドでは、追加、および Android デバイスでのアクセス許可を確認する方法について説明します。 (API レベル 23 <)、古い Android アプリと新しい Android アプリの間のアクセス許可の動作の違い (API レベル 22 >)。 これは、Android 6.0 で実行時のアクセス許可のチェックを実行する方法を説明します。


## <a name="related-links"></a>関連リンク

- [通常のアクセス許可の一覧](https://developer.android.com/guide/topics/permissions/normal-permissions.html)
- [ランタイムのアクセス許可のサンプル アプリ](https://github.com/xamarin/monodroid-samples/tree/master/android-m/RuntimePermissions)
- [Xamarin.Android でのアクセス許可の処理](https://github.com/xamarin/recipes/tree/master/Recipes/android/general/projects/add_permissions_to_android_manifest)
