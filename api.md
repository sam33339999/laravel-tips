## API

⬆️ [返回主要選單](README.md#laravel-tips) ⬅️ [返回上一個 (「日誌、除錯」)](log-and-debug.md) ➡️ [下一個 (「其他」)](other.md)

- [API Resources: 是否有要 "data" 包裝？ - API Resources: With or Without "data"?](#api-resources-with-or-without-data)
- [API Resources 中判斷關聯是否存在，如果存在則正常顯示數量；反之則該屬性不會被包含 - Conditional Relationship Counts on API Resources](#conditional-relationship-counts-on-api-resources)
- [API: 一切順利(無回覆內文) - API Return "Everything went ok"](#api-return-everything-went-ok)
- [在 API Resource 中避免 N+1 的問題 - Avoid N+1 queries in API resources](#avoid-n1-queries-in-api-resources)
- [從請求頭中取得 Authorization 的 Bearer 令牌 - Get Bearer Token from Authorization header](#get-bearer-token-from-authorization-header)
- [排序你的 API 回傳結果 - Sorting Your API Results](#sorting-your-api-results)
- [自定義 API 例外處理 - Customize Exception Handler For API](#customize-exception-handler-for-api)
- [API 回傳強制以 JSON 回應 - Force JSON Response For API Requests](#force-json-response-for-api-requests)
- [API 版本制 - API Versioning](#api-versioning)

<h3><a href="#api-resources-with-or-without-data">API Resources: 是否有要 "data" 包裝？</a></h3>

> 你如果使用 Eloquent API Resources 回傳資料，他會自動的包裝在 'data' 裡面。如果你想要移除它，可以在 `app/Providers/AppServiceProvider.php` 加入 `JsonResource::withoutWrapping();`。

```php
class AppServiceProvider extends ServiceProvider
{
    public function boot()
    {
        JsonResource::withoutWrapping();
    }
}
```

源至 [@phillipmwaniki](https://twitter.com/phillipmwaniki/status/1445230637544321029)

<h3><a href="#conditional-relationship-counts-on-api-resources">API Resources 中判斷關聯是否存在，如果存在則正常顯示數量；反之則該屬性不會被包含</a></h3>

> 你可以按段使用 `whenCounted()` 方法在資源回應中條件性的包含關聯的數量。<br />
> 這樣做，如果關聯的數量缺失，屬性就不會被包含。
```php
public function toArray($request)
{
     return [
          'id' => $this->id,
          'name' => $this->name,
          'email' => $this->email,
          'posts_count' => $this->whenCounted('posts'),
          'created_at' => $this->created_at,
          'updated_at' => $this->updated_at,
     ];
}
```

源至 [@mvpopuk](https://twitter.com/mvpopuk/status/1570480977507504128)

<h3><a href="#api-return-everything-went-ok">API: 一切順利(無回覆內文)</a></h3>

> 如果你的某個 API 是執行一些操作，但沒有回應；因此你只想返回 "一切順利"。<br />
> 你可以返回 `204` 狀態碼 "No content"。在 Laravel 中，這很容易：`return response()->noContent();`。


```php
public function reorder(Request $request)
{
    foreach ($request->input('rows', []) as $row) {
        Country::find($row['id'])->update(['position' => $row['position']]);
    }

    return response()->noContent();
}
```

<h3><a href="#avoid-n1-queries-in-api-resources">在 API Resource 中避免 N+1 的問題</a></h3>

> 你可以避免 N+1 的問題，使用 `whenLoaded()` 方法在 API 資源中。<br/>
> 這樣只有在 Employee 模型中已經加載了部門時，才會附加部門。<br/>
> 沒有 `whenLoaded()` 時，總是會有一個查詢部門的查詢。

```php
class EmployeeResource extends JsonResource
{
    public function toArray($request): array
    {
        return [
            'id' => $this->uuid,
            'fullName' => $this->full_name,
            'email' => $this->email,
            'jobTitle' => $this->job_title,
            'department' => DepartmentResource::make($this->whenLoaded('department')),
        ];
    }
}
```

源至 [@mmartin_joo](https://twitter.com/mmartin_joo/status/1473987501501071362)

<h3><a href="#get-bearer-token-from-authorization-header">從請求頭中取得 Authorization 的 Bearer 令牌</a></h3>

> 當你使用 API 並想要從 Authorization 頭中取得令牌時，`bearerToken()` 函數非常方便。

```php
// 不要手動解析 request header 像是這樣:
$tokenWithBearer = $request->header('Authorization');
$token = substr($tokenWithBearer, 7);

// 這樣做更好:
$token = $request->bearerToken();
```

源至 [@iamharis010](https://twitter.com/iamharis010/status/1488413755826327553)

<h3><a href="#sorting-your-api-results">排序你的 API 回傳結果</a></h3>

> 單個欄位 API 排序，具有方向控制（遞增或是遞減排序）

```php
// Handles /dogs?sort=name and /dogs?sort=-name
Route::get('dogs', function (Request $request) {
    // Get the sort query parameter (or fall back to default sort "name")
    $sortColumn = $request->input('sort', 'name');

    // Set the sort direction based on whether the key starts with -
    // using Laravel's Str::startsWith() helper function
    $sortDirection = Str::startsWith($sortColumn, '-') ? 'desc' : 'asc';
    $sortColumn = ltrim($sortColumn, '-');

    return Dog::orderBy($sortColumn, $sortDirection)
        ->paginate(20);
});
```

> 你也可以對多個欄位進行排序（例如，`?sort=name,-weight`）

```php
// Handles ?sort=name,-weight
Route::get('dogs', function (Request $request) {
    // Grab the query parameter and turn it into an array exploded by ,
    $sorts = explode(',', $request->input('sort', ''));

    // Create a query
    $query = Dog::query();

    // Add the sorts one by one
    foreach ($sorts as $sortColumn) {
        $sortDirection = Str::startsWith($sortColumn, '-') ? 'desc' : 'asc';
        $sortColumn = ltrim($sortColumn, '-');

        $query->orderBy($sortColumn, $sortDirection);
    }

    // Return
    return $query->paginate(20);
});
```
---

<h3><a href="#customize-exception-handler-for-api">自定義 API 例外處理</a></h3>

#### Laravel <= 8 (包含 Laravel 8):

> 有個方法 `render()` 在 `App\Exceptions` 類別中：

```php
   public function render($request, Exception $exception)
    {
        if ($request->wantsJson() || $request->is('api/*')) {
            if ($exception instanceof ModelNotFoundException) {
                return response()->json(['message' => 'Item Not Found'], 404);
            }

            if ($exception instanceof AuthenticationException) {
                return response()->json(['message' => 'unAuthenticated'], 401);
            }

            if ($exception instanceof ValidationException) {
                return response()->json(['message' => 'UnprocessableEntity', 'errors' => []], 422);
            }

            if ($exception instanceof NotFoundHttpException) {
                return response()->json(['message' => 'The requested link does not exist'], 400);
            }
        }

        return parent::render($request, $exception);
    }
```

#### Laravel >= 9:

> 有個方法 `register()` 在 `App\Exceptions` 類別中：

```php
    public function register()
    {
        $this->renderable(function (ModelNotFoundException $e, $request) {
            if ($request->wantsJson() || $request->is('api/*')) {
                return response()->json(['message' => 'Item Not Found'], 404);
            }
        });

        $this->renderable(function (AuthenticationException $e, $request) {
            if ($request->wantsJson() || $request->is('api/*')) {
                return response()->json(['message' => 'unAuthenticated'], 401);
            }
        });
        $this->renderable(function (ValidationException $e, $request) {
            if ($request->wantsJson() || $request->is('api/*')) {
                return response()->json(['message' => 'UnprocessableEntity', 'errors' => []], 422);
            }
        });
        $this->renderable(function (NotFoundHttpException $e, $request) {
            if ($request->wantsJson() || $request->is('api/*')) {
                return response()->json(['message' => 'The requested link does not exist'], 400);
            }
        });
    }
```

源至 [Feras Elsharif](https://github.com/ferasbbm)

---

<h3><a href="#force-json-response-for-api-requests">API 回傳強制以 JSON 回應</a></h3>

> 如果你建立了一個 API，當請求不包含 "Accept: application/JSON " HTTP 標頭時，<br/>
> API 遇到錯誤時將會以 HTML 或重定向回應在 API 路由上，<br/>
> 為了避免這種情況，我們可以強制所有 API 回應為 JSON。

> 第一步是建立中介層，執行以下命令：

```console
php artisan make:middleware ForceJsonResponse
```
> 在 `App/Http/Middleware/ForceJsonResponse.php` 文件的 handle 函數中編寫以下代碼：

```php
public function handle($request, Closure $next)
{
    $request->headers->set('Accept', 'application/json');
    return $next($request);
}
```

> 第二步，在 `app/Http/Kernel.php` 文件中註冊創建的中介層：

```php
protected $middlewareGroups = [        
    'api' => [
        \App\Http\Middleware\ForceJsonResponse::class,
    ],
];
```

源至 [Feras Elsharif](https://github.com/ferasbbm)

---

<h3><a href="#api-versioning">API 版本制</a></h3>

#### 何時版本化？

> 如果你正在進行的項目可能在未來有多個版本，或者你的端點有一個破壞性的變化，<br/>
> 例如響應數據格式的變化，並且你希望確保在代碼發生變化時 API 版本保持功能。


#### 更改默認路由文件

> 第一步是更改 `App\Providers\RouteServiceProvider` 文件中的路由映射，讓我們開始：

#### Laravel <= 8 (包含 Laravel 8):

> 在 `App\Providers\RouteServiceProvider` 文件中添加 `ApiNamespace` 屬性：

```php
/**
 * @var string
 *
 */
protected string $ApiNamespace = 'App\Http\Controllers\Api';
```

> 在 `boot` 方法中添加以下代碼：

```php
$this->routes(function () {
     Route::prefix('api/v1')
        ->middleware('api')
        ->namespace($this->ApiNamespace.'\\V1')
        ->group(base_path('routes/API/v1.php'));
        }
    
    //for v2
     Route::prefix('api/v2')
            ->middleware('api')
            ->namespace($this->ApiNamespace.'\\V2')
            ->group(base_path('routes/API/v2.php'));
});
```


#### Laravel <= 7:

> 在 `App\Providers\RouteServiceProvider` 文件中添加 `ApiNamespace` 屬性：

```php
/**
 * @var string
 *
 */
protected string $ApiNamespace = 'App\Http\Controllers\Api';
```

> 在 `map` 方法中添加以下代碼：

```php
// remove this $this->mapApiRoutes(); 
    $this->mapApiV1Routes();
    $this->mapApiV2Routes();
```

> 添加以下方法：

```php
  protected function mapApiV1Routes()
    {
        Route::prefix('api/v1')
            ->middleware('api')
            ->namespace($this->ApiNamespace.'\\V1')
            ->group(base_path('routes/Api/v1.php'));
    }

  protected function mapApiV2Routes()
    {
        Route::prefix('api/v2')
            ->middleware('api')
            ->namespace($this->ApiNamespace.'\\V2')
            ->group(base_path('routes/Api/v2.php'));
    }
```

#### Controller 文件夾版本化

```
Controllers
└── Api
    ├── V1
    │   └──AuthController.php
    └── V2
        └──AuthController.php
```

#### 路由文件版本化

```
routes
└── Api
   │    └── v1.php     
   │    └── v2.php 
   └── web.php
```

源至 [Feras Elsharif](https://github.com/ferasbbm)
