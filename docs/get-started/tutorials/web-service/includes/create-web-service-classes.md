---
ms.openlocfilehash: 7a5acca4169b1f763e178e0a05b01548765ff45d
ms.sourcegitcommit: 4d260b655cb52b990dda79c239a9721f2e964625
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2021
ms.locfileid: "98570841"
---
REST 要求は、Web ブラウザーがページの取得やサーバーへのデータ送信に使用するのと同じ HTTP 動詞を使用して HTTP 経由で行われます。 この演習では、GET 動詞を使用して GitHub Web API から .NET リポジトリ データを取得するクラスを作成します。

# <a name="visual-studio"></a>[Visual Studio](#tab/vswin)

1. **ソリューション エクスプローラー** の **[WebServiceTutorial]** プロジェクトで、`Constants` という名前の新しいクラスをプロジェクトに追加します。 次に、**Constants.cs** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```csharp
    namespace WebServiceTutorial
    {
        public static class Constants
        {
            public const string GitHubReposEndpoint = "https://api.github.com/orgs/dotnet/repos";
        }
    }
    ```

    このコードによって、1 つの定数 (Web 要求の対象となるエンドポイント) が定義されます。

1. **ソリューション エクスプローラー** の **[WebServicesTutorial]** プロジェクトで、`Repository` という名前の新しいクラスをプロジェクトに追加します。 次に、**Repository.cs** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```csharp
    using System;
    using Newtonsoft.Json;

    namespace WebServiceTutorial
    {
        public class Repository
        {
            [JsonProperty("name")]
            public string Name { get; set; }

            [JsonProperty("description")]
            public string Description { get; set; }

            [JsonProperty("html_url")]
            public Uri GitHubHomeUrl { get; set; }

            [JsonProperty("homepage")]
            public Uri Homepage { get; set; }

            [JsonProperty("watchers")]
            public int Watchers { get; set; }
        }
    }
    ```

    このコードによって、Web サービスから取得した JSON データをモデル化するために使用される `Repository` クラスが定義されます。 各プロパティは、JSON フィールド名を含む `JsonProperty` 属性で装飾されます。 Newtonsoft.Json は、JSON データをモデル オブジェクトに逆シリアル化するときに、この JSON フィールド名と CLR プロパティとのマッピングを使用します。

    > [!NOTE]
    > 上記のクラス定義は単純化されており、Web サービスから取得した JSON データを完全にはモデル化していません。

1. **ソリューション エクスプローラー** の **[WebServiceTutorial]** プロジェクトで、`RestService` という名前の新しいクラスをプロジェクトに追加します。 次に、**RestService.cs** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Diagnostics;
    using System.Net.Http;
    using System.Threading.Tasks;
    using Newtonsoft.Json;

    namespace WebServiceTutorial
    {
        public class RestService
        {
            HttpClient _client;

            public RestService()
            {
                _client = new HttpClient();
            }

            public async Task<List<Repository>> GetRepositoriesAsync(string uri)
            {
                List<Repository> repositories = null;
                try
                {
                    HttpResponseMessage response = await _client.GetAsync(uri);
                    if (response.IsSuccessStatusCode)
                    {
                        string content = await response.Content.ReadAsStringAsync();
                        repositories = JsonConvert.DeserializeObject<List<Repository>>(content);
                    }
                }
                catch (Exception ex)
                {
                    Debug.WriteLine("\tERROR {0}", ex.Message);
                }

                return repositories;
            }
        }
    }
    ```

    このコードによって、GitHub Web API から .NET リポジトリ データを取得する 1 つのメソッド `GetRepositoriesAsync` が定義されます。 このメソッドは `HttpClient.GetAsync` メソッドを使用して、`uri` 引数で指定された Web API に GET 要求を送信します。 その Web API は、`HttpResponseMessage` オブジェクトに保管されている応答を送信します。 応答には、HTTP 要求が成功したか失敗したかを示す HTTP 状態コードが含まれます。 要求が成功した場合、Web API は、HTTP 状態コード 200 (OK)、および `HttpResponseMessage.Content` プロパティ内の JSON 応答で応答します。 この JSON データは、`JsonConvert.DeserializeObject` メソッドを使用して `List<Repository>` オブジェクトに逆シリアル化される前に、`HttpContent.ReadAsStringAsync` メソッドを使用して `string` に読み取られます。 このメソッドは、逆シリアル化を実行するために、`Repository` クラスで定義されている JSON フィールド名と CLR プロパティとの間のマッピングを使用します。

1. エラーがないようにソリューションを構築してください。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/vsmac)

1. **Solution Pad** の **[WebServiceTutorial]** プロジェクトで、`Constants` という名前の新しいクラスをプロジェクトに追加します。 次に、**Constants.cs** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```csharp
    namespace WebServiceTutorial
    {
        public static class Constants
        {
            public const string GitHubReposEndpoint = "https://api.github.com/orgs/dotnet/repos";
        }
    }
    ```

    このコードによって、1 つの定数 (Web 要求の対象となるエンドポイント) が定義されます。

1. **Solution Pad** の **[WebServicesTutorial]** プロジェクトで、`Repository` という名前の新しいクラスをプロジェクトに追加します。 次に、**Repository.cs** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```csharp
    using System;
    using Newtonsoft.Json;

    namespace WebServiceTutorial
    {
        public class Repository
        {
            [JsonProperty("name")]
            public string Name { get; set; }

            [JsonProperty("description")]
            public string Description { get; set; }

            [JsonProperty("html_url")]
            public Uri GitHubHomeUrl { get; set; }

            [JsonProperty("homepage")]
            public Uri Homepage { get; set; }

            [JsonProperty("watchers")]
            public int Watchers { get; set; }
        }
    }
    ```

    このコードによって、Web サービスから取得した JSON データをモデル化するために使用される `Repository` クラスが定義されます。 各プロパティは、JSON フィールド名を含む `JsonProperty` 属性で装飾されます。 Newtonsoft.Json は、JSON データをモデル オブジェクトに逆シリアル化するときに、この JSON フィールド名と CLR プロパティとのマッピングを使用します。

    > [!NOTE]
    > 上記のクラス定義は単純化されており、Web サービスから取得した JSON データを完全にはモデル化していません。

1. **Solution Pad** の **[WebServiceTutorial]** プロジェクトで、`RestService` という名前の新しいクラスをプロジェクトに追加します。 次に、**RestService.cs** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Diagnostics;
    using System.Net.Http;
    using System.Threading.Tasks;
    using Newtonsoft.Json;

    namespace WebServiceTutorial
    {
        public class RestService
        {
            HttpClient _client;

            public RestService()
            {
                _client = new HttpClient();
            }

            public async Task<List<Repository>> GetRepositoriesAsync(string uri)
            {
                List<Repository> repositories = null;
                try
                {
                    HttpResponseMessage response = await _client.GetAsync(uri);
                    if (response.IsSuccessStatusCode)
                    {
                        string content = await response.Content.ReadAsStringAsync();
                        repositories = JsonConvert.DeserializeObject<List<Repository>>(content);
                    }
                }
                catch (Exception ex)
                {
                    Debug.WriteLine("\tERROR {0}", ex.Message);
                }

                return repositories;
            }
        }
    }
    ```

    このコードによって、GitHub Web API から .NET リポジトリ データを取得する 1 つのメソッド `GetRepositoriesAsync` が定義されます。 このメソッドは `HttpClient.GetAsync` メソッドを使用して、`uri` 引数で指定された Web API に GET 要求を送信します。 その Web API は、`HttpResponseMessage` オブジェクトに保管されている応答を送信します。 応答には、HTTP 要求が成功したか失敗したかを示す HTTP 状態コードが含まれます。 要求が成功した場合、Web API は、HTTP 状態コード 200 (OK)、および `HttpResponseMessage.Content` プロパティ内の JSON 応答で応答します。 この JSON データは、`JsonConvert.DeserializeObject` メソッドを使用して `List<Repository>` オブジェクトに逆シリアル化される前に、`HttpContent.ReadAsStringAsync` メソッドを使用して `string` に読み取られます。 このメソッドは、逆シリアル化を実行するために、`Repository` クラスで定義されている JSON フィールド名と CLR プロパティとの間のマッピングを使用します。

1. エラーがないようにソリューションを構築してください。
