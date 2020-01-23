---
ms.openlocfilehash: 3b1603b6af5ebb5558c3cd764f41fdbe24351b9b
ms.sourcegitcommit: 3f0e4f10e5def19122588bb05f26ab2baa9df6eb
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/23/2020
ms.locfileid: "68669721"
---
REST 要求は、Web ブラウザーがページの取得やサーバーへのデータ送信に使用するのと同じ HTTP 動詞を使用して HTTP 経由で行われます。 この演習では、GET 動詞を使用して [OpenWeatherMap](https://openweathermap.org/) Web API からデータを取得するクラスを作成します。 この Web API は、指定された場所の天気予報データを取得するために使用できます。 この Web API を使用するには、API キーにサインアップする必要があります。

> [!div class="nextstepaction"]
> [API キーにサインアップする](https://home.openweathermap.org/users/sign_up)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. **ソリューション エクスプローラー**の **[WebServiceTutorial]** プロジェクトで、`Constants` という名前の新しいクラスをプロジェクトに追加します。 次に、**Constants.cs** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```csharp
    namespace WebServiceTutorial
    {
        public static class Constants
        {
            public const string OpenWeatherMapEndpoint = "https://api.openweathermap.org/data/2.5/weather";
            public const string OpenWeatherMapAPIKey = "INSERT_API_KEY_HERE";
        }
    }
    ```

    このコードは 2 つの定数を定義します。 `OpenWeatherMapEndpoint` 定数は Web 要求が行われるエンドポイントを定義し、`OpenWeatherMapAPIKey` 定数は [OpenWeatherMap](https://openweathermap.org/) サービスの個人用 API キーを定義します。

    > [!IMPORTANT]
    > 個人用 OpenWeatherMap API キーを `OpenWeatherMapAPIKey` 定数の値として設定する必要があります。

1. **ソリューション エクスプローラー**の **[WebServicesTutorial]** プロジェクトで、`WeatherData` という名前の新しいクラスをプロジェクトに追加します。 次に、**WeatherData.cs** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```csharp
    using Newtonsoft.Json;

    namespace WebServiceTutorial
    {
        public class WeatherData
        {
            [JsonProperty("name")]
            public string Title { get; set; }

            [JsonProperty("weather")]
            public Weather[] Weather { get; set; }

            [JsonProperty("main")]
            public Main Main { get; set; }

            [JsonProperty("visibility")]
            public long Visibility { get; set; }

            [JsonProperty("wind")]
            public Wind Wind { get; set; }
        }

        public class Main
        {
            [JsonProperty("temp")]
            public double Temperature { get; set; }

            [JsonProperty("humidity")]
            public long Humidity { get; set; }
        }

        public class Weather
        {
            [JsonProperty("main")]
            public string Visibility { get; set; }
        }

        public class Wind
        {
            [JsonProperty("speed")]
            public double Speed { get; set; }
        }
    }
    ```

    このコードは、Web サービスから取得した JSON データをモデル化するために使用される 4 つのクラスを定義します。 各プロパティは、JSON フィールド名を含む `JsonProperty` 属性で装飾されます。 Newtonsoft.Json は、JSON データをモデル オブジェクトに逆シリアル化するときに、この JSON フィールド名と CLR プロパティとのマッピングを使用します。

    > [!NOTE]
    > 上記のクラス定義は単純化されており、Web サービスから取得した JSON データを完全にはモデル化していません。 完全なデータ モデルの例については、[Weather App](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/weather/) サンプルをご覧ください。

1. **ソリューション エクスプローラー**の **[WebServiceTutorial]** プロジェクトで、`RestService` という名前の新しいクラスをプロジェクトに追加します。 次に、**RestService.cs** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```csharp
    using System;
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

            public async Task<WeatherData> GetWeatherDataAsync(string uri)
            {
                WeatherData weatherData = null;
                try
                {
                    HttpResponseMessage response = await _client.GetAsync(uri);
                    if (response.IsSuccessStatusCode)
                    {
                        string content = await response.Content.ReadAsStringAsync();
                        weatherData = JsonConvert.DeserializeObject<WeatherData>(content);
                    }
                }
                catch (Exception ex)
                {
                    Debug.WriteLine("\tERROR {0}", ex.Message);
                }

                return weatherData;
            }
        }
    }
    ```

    このコードは、[OpenWeatherMap](https://openweathermap.org/) Web API から指定の場所の天気データを取得する単一のメソッド `GetWeatherDataAsync` を定義します。 このメソッドは `HttpClient.GetAsync` メソッドを使用して、`uri` 引数で指定された Web API に GET 要求を送信します。 その Web API は、`HttpResponseMessage` オブジェクトに保管されている応答を送信します。 応答には、HTTP 要求が成功したか失敗したかを示す HTTP 状態コードが含まれます。 要求が成功した場合、Web API は、HTTP 状態コード 200 (OK)、および `HttpResponseMessage.Content` プロパティ内の JSON 応答で応答します。 この JSON データは、`JsonConvert.DeserializeObject` メソッドを使用して `WeatherData` オブジェクトに逆シリアル化される前に、`HttpContent.ReadAsStringAsync` メソッドを使用して `string` に読み取られます。 このメソッドは、逆シリアル化を実行するために、`WeatherData` クラスで定義されている JSON フィールド名と CLR プロパティとの間のマッピングを使用します。

1. エラーがないようにソリューションを構築してください。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. **Solution Pad** の **[WebServiceTutorial]** プロジェクトで、`Constants` という名前の新しいクラスをプロジェクトに追加します。 次に、**Constants.cs** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```csharp
    namespace WebServiceTutorial
    {
        public static class Constants
        {
            public static string OpenWeatherMapEndpoint = "https://api.openweathermap.org/data/2.5/weather";
            public static string OpenWeatherMapAPIKey = "INSERT_API_KEY_HERE";
        }
    }
    ```

    このコードは 2 つの定数を定義します。 `OpenWeatherMapEndpoint` 定数は Web 要求が行われるエンドポイントを定義し、`OpenWeatherMapAPIKey` 定数は [OpenWeatherMap](https://openweathermap.org/) サービスの個人用 API キーを定義します。

    > [!IMPORTANT]
    > 個人用 OpenWeatherMap API キーを `OpenWeatherMapAPIKey` 定数の値として設定する必要があります。

1. **Solution Pad** の **[WebServicesTutorial]** プロジェクトで、`WeatherData` という名前の新しいクラスをプロジェクトに追加します。 次に、**WeatherData.cs** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```csharp
    using Newtonsoft.Json;

    namespace WebServiceTutorial
    {
        public class WeatherData
        {
            [JsonProperty("name")]
            public string Title { get; set; }

            [JsonProperty("weather")]
            public Weather[] Weather { get; set; }

            [JsonProperty("main")]
            public Main Main { get; set; }

            [JsonProperty("visibility")]
            public long Visibility { get; set; }

            [JsonProperty("wind")]
            public Wind Wind { get; set; }
        }

        public class Main
        {
            [JsonProperty("temp")]
            public double Temperature { get; set; }

            [JsonProperty("humidity")]
            public long Humidity { get; set; }
        }

        public class Weather
        {
            [JsonProperty("main")]
            public string Visibility { get; set; }
        }

        public class Wind
        {
            [JsonProperty("speed")]
            public double Speed { get; set; }
        }
    }
    ```

    このコードは、Web サービスから取得した JSON データをモデル化するために使用される 4 つのクラスを定義します。 各プロパティは、JSON フィールド名を含む `JsonProperty` 属性で装飾されます。 Newtonsoft.Json は、JSON データをモデル オブジェクトに逆シリアル化するときに、この JSON フィールド名と CLR プロパティとのマッピングを使用します。

    > [!NOTE]
    > 上記のクラス定義は単純化されており、Web サービスから取得した JSON データを完全にはモデル化していません。 完全なデータ モデルの例については、[Weather App](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/weather/) サンプルをご覧ください。

1. **Solution Pad** の **[WebServiceTutorial]** プロジェクトで、`RestService` という名前の新しいクラスをプロジェクトに追加します。 次に、**RestService.cs** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```csharp
    using System;
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

            public async Task<WeatherData> GetWeatherDataAsync(string uri)
            {
                WeatherData weatherData = null;
                try
                {
                    HttpResponseMessage response = await _client.GetAsync(uri);
                    if (response.IsSuccessStatusCode)
                    {
                        string content = await response.Content.ReadAsStringAsync();
                        weatherData = JsonConvert.DeserializeObject<WeatherData>(content);
                    }
                }
                catch (Exception ex)
                {
                    Debug.WriteLine("\tERROR {0}", ex.Message);
                }

                return weatherData;
            }
        }
    }
    ```

    このコードは、[OpenWeatherMap](https://openweathermap.org/) Web API から指定の場所の天気データを取得する単一のメソッド `GetWeatherDataAsync` を定義します。 このメソッドは `HttpClient.GetAsync` メソッドを使用して、`uri` 引数で指定された Web API に GET 要求を送信します。 その Web API は、`HttpResponseMessage` オブジェクトに保管されている応答を送信します。 応答には、HTTP 要求が成功したか失敗したかを示す HTTP 状態コードが含まれます。 要求が成功した場合、Web API は、HTTP 状態コード 200 (OK)、および `HttpResponseMessage.Content` プロパティ内の JSON 応答で応答します。 この JSON データは、`JsonConvert.DeserializeObject` メソッドを使用して `WeatherData` オブジェクトに逆シリアル化される前に、`HttpContent.ReadAsStringAsync` メソッドを使用して `string` に読み取られます。 このメソッドは、逆シリアル化を実行するために、`WeatherData` クラスで定義されている JSON フィールド名と CLR プロパティとの間のマッピングを使用します。

1. エラーがないようにソリューションを構築してください。
