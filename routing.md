## Routing

⬆️ [返回主要選單](README.md#laravel-tips) ⬅️ [返回上一個 (「視圖」)](views.md) ➡️ [下一個 (「驗證」)](validation.md)

- [路由群組 - Route group within a group](#route-group-within-a-group)
- [聲明路由與 Model 綁定 - Declare a resolveRouteBinding method in your Model](#declare-a-resolveroutebinding-method-in-your-model)
- [指派 `withTrashed` 到 Route::resource() - assign withTrashed() to Route::resource() method](#assign-withtrashed-to-routeresource-method)
- [略過輸入的規範化 - Skip Input Normalization](#skip-input-normalization)
- [子域名匹配 - Wildcard subdomains](#wildcard-subdomains)
- [路由的背後是什麼？ - What's behind the routes?](#whats-behind-the-routes)
- [路由 Model 綁定: 你可以定義要搜尋的鍵值 - Route Model Binding: You can define a key](#route-model-binding-you-can-define-a-key)
- [路由備用方案: 當沒有路由被匹配時 - Route Fallback: When no Other Route is Matched](#route-fallback-when-no-other-route-is-matched)
- [路由參數使用正則匹配 Route Parameters Validation with RegExp](#route-parameters-validation-with-regexp)
- [路由限流: 全域和針對來賓/使用者 - Rate Limiting: Global and for Guests/Users](#rate-limiting-global-and-for-guestsusers)
- [將查詢字串傳遞給路由 - Query string parameters to Routes](#query-string-parameters-to-routes)
- [按照文件分離路由 - Separate Routes by Files](#separate-routes-by-files)
- [翻譯資源的動詞(FOR SEO) - Translate Resource Verbs](#translate-resource-verbs)
- [自定義資源控制器名稱 - Custom Resource Route Names](#custom-resource-route-names)
- [儘早的讀出關聯 - Eager load relationship](#eager-load-relationship)
- [簡單高亮你的導航菜單 - Easily highlight your navbar menus](#easily-highlight-your-navbar-menus)
- [使用 route() 產生相對路徑 - Generate absolute path using route() helper](#generate-absolute-path-using-route-helper)
- [為每個 Model 覆寫路由綁定解析器 - Override the route binding resolver for each of your models](#override-the-route-binding-resolver-for-each-of-your-models)
- [如果你需要公開URL但同時希望他們是安全的 - If you need public URL, but you want them to be secured](#if-you-need-public-url-but-you-want-them-to-be-secured)
- [使用 Gate 在中介方法 - Using Gate in middleware method](#using-gate-in-middleware-method)
- [簡單 route 使用箭頭函數 - Simple route with arrow function](#simple-route-with-arrow-function)
- [路由直接返回視圖 - Route view](#route-view)
- [路由資料夾加載 - Route directory instead of route file](#route-directory-instead-of-route-file)
- [路由資源群組 - Route resources grouping](#route-resources-grouping)
- [自定義路由綁定 - Custom route bindings](#custom-route-bindings)
- [兩個方法檢查路由名稱 - Two ways to check the route name](#two-ways-to-check-the-route-name)
- [路由綁定Model，並且撈出軟刪除的資訊 - Route model binding soft-deleted models](#route-model-binding-soft-deleted-models)
- [取回 URL 並且不要有請求參數 - Retrieve the URL without query parameters](#retrieve-the-url-without-query-parameters)
- [自定義路由模型綁定中缺失的行為 - Customizing Missing Model Behavior in route model bindings](#customizing-missing-model-behavior-in-route-model-bindings)
- [在某個路由中撇除中介層 - Exclude middleware from a route](#exclude-middleware-from-a-route)

<h3 id="route-group-within-a-group">路由群組</h3>

> 在路由中，你可以建立一個群組，並在「父」群組中的某些 URL 中僅分配某些中介軟體。<br/>
> 如下所示： `/account/login` 和 `/account/register` 不會使用 `auth` 中介層，而 `/account/edit` 會使用。

```php
Route::group(['prefix' => 'account', 'as' => 'account.'], function() {
    Route::get('login', [AccountController::class, 'login']);
    Route::get('register', [AccountController::class, 'register']);
    Route::group(['middleware' => 'auth'], function() {
        Route::get('edit', [AccountController::class, 'edit']);
    });
});
```

<h3 id="declare-a-resolveroutebinding-method-in-your-model">聲明路由與 Model 綁定</h3>

> Laravel 中的路由模型綁定很棒，但有些情況下我們不能讓用戶輕易通過 ID 訪問資源。<br/>
> 我們可能需要驗證他們對資源的所有權。<br/><br/>
> 你可以在 Model 中聲明 `resolveRouteBinding` 方法，並在其中添加自定邏輯。

```php
public function resolveRouteBinding($value, $field = null)
{
     $user = request()->user();

     return $this->where([
          ['user_id' => $user->id],
          ['id' => $value]
     ])->firstOrFail();
}
```

源至 [@notdylanv](https://twitter.com/notdylanv/status/1567296232183447552/)

<h3 id="assign-withtrashed-to-routeresource-method">指派 `withTrashed` 到 Route::resource()</h3>

> Laravel 9.35 之前 - 只適用於 Route::get()
```php
Route::get('/users/{user}', function (User $user) {
     return $user->email;
})->withTrashed();
```

> Laravel 9.35 之後 - 也適用於 Route::resource()
```php
Route::resource('users', UserController::class)
     ->withTrashed();
```

> 更甚者，你可以指定哪些方法應該包含已刪除的資源。
```php
Route::resource('users', UserController::class)
     ->withTrashed(['show']);
```

<h3 id="skip-input-normalization">略過輸入的規範化</h3>

> Laravel 會自動修剪請求中的所有字符串字段。這稱為「輸入規範化」。<br/>
> 但是有的時後，你可能不希望這種行為。<br/>
> 你可以在 TrimStrings 中介軟體上使用 `skipWhen` 方法，並返回 `true` 以跳過它。

```php
public function boot()
{
     TrimStrings::skipWhen(function ($request) {
          return $request->is('admin/*);
     });
}
```

源至 [@Laratips1](https://twitter.com/Laratips1/status/1580210517372596224)

<h3 id="wildcard-subdomains">子域名匹配</h3>

> 你可以通過動態子域名名稱創建路由群組，並將其值傳遞給每個路由。

```php
Route::domain('{username}.workspace.com')->group(function () {
    Route::get('user/{id}', function ($username, $id) {
        //
    });
});
```

<h3 id="whats-behind-the-routes">路由的背後是什麼？</h3>

> 如果你使用 [Laravel UI 套件](https://github.com/laravel/ui)，你可能想知道 `Auth::routes()` 背後實際上是什麼路由？<br/>
> 你可以查看 `/vendor/laravel/ui/src/AuthRouteMethods.php` 文件。

```php
public function auth()
{
    return function ($options = []) {
        // Authentication Routes...
        $this->get('login', 'Auth\LoginController@showLoginForm')->name('login');
        $this->post('login', 'Auth\LoginController@login');
        $this->post('logout', 'Auth\LoginController@logout')->name('logout');
        // Registration Routes...
        if ($options['register'] ?? true) {
            $this->get('register', 'Auth\RegisterController@showRegistrationForm')->name('register');
            $this->post('register', 'Auth\RegisterController@register');
        }
        // Password Reset Routes...
        if ($options['reset'] ?? true) {
            $this->resetPassword();
        }
        // Password Confirmation Routes...
        if ($options['confirm'] ?? class_exists($this->prependGroupNamespace('Auth\ConfirmPasswordController'))) {
            $this->confirmPassword();
        }
        // Email Verification Routes...
        if ($options['verify'] ?? false) {
            $this->emailVerification();
        }
    };
}
```
> 該函數的默認用法僅為：

```php
Auth::routes(); // no parameters
```

> 但是你可以提供參數來啟用或禁用某些路由：

```php
Auth::routes([
    'login'    => true,
    'logout'   => true,
    'register' => true,
    'reset'    => true,  // for resetting passwords 重設密碼
    'confirm'  => false, // for additional password confirmations 進行額外的密碼確認
    'verify'   => false, // for email verification 電子郵件驗證
]);
```

Tip is based on [suggestion](https://github.com/LaravelDaily/laravel-tips/pull/57) by [MimisK13](https://github.com/MimisK13)

<h3 id="route-model-binding-you-can-define-a-key">路由 Model 綁定: 你可以定義要搜尋的鍵值</h3>

> 你可以使用 Route model binding，像這樣 `Route::get('api/users/{user}', function (User $user) { … })` - 但不僅僅是 ID 字段。<br/>
> 如果你想讓 `{user}` 成為 `username` 字段，請在 Model 中添加以下代碼：

```php
public function getRouteKeyName() {
    return 'username';
}
```

<h3 id="route-fallback-when-no-other-route-is-matched">路由備用方案: 當沒有路由被匹配時</h3>

> 如果你想為未找到的路由指定額外邏輯，而不僅僅是拋出默認的 404 頁面，你可以在 Routes 文件的最後創建一個特殊的 Route。

```php
Route::group(['middleware' => ['auth'], 'prefix' => 'admin', 'as' => 'admin.'], function () {
    Route::get('/home', [HomeController::class, 'index']);
    Route::resource('tasks', [Admin\TasksController::class]);
});

// Some more routes....
Route::fallback(function() {
    return 'Hm, why did you land here somehow?';
});
```

<h3 id="route-parameters-validation-with-regexp">路由參數使用正則匹配</h3>

> 我們可以直接在路由中驗證參數，使用 `where` 參數。一個很典型的情況是在路由中添加語言區域前綴，像 `fr/blog` 和 `en/article/333`。我們如何確保這兩個字母不被用於其他目的？

```php
// routes/web.php
Route::group([
    'prefix' => '{locale}',
    'where' => ['locale' => '[a-zA-Z]{2}']
], function () {
    Route::get('/', [HomeController::class, 'index']);
    Route::get('article/{id}', [ArticleController::class, 'show']);;
});
```

<h3 id="rate-limiting-global-and-for-guestsusers">路由限流: 全域和針對來賓/使用者</h3>

> 你可以限制某些 URL 每分鐘最多調用 60 次，使用 `throttle:60,1`：

```php
Route::middleware('auth:api', 'throttle:60,1')->group(function () {
    Route::get('/user', function () {
        //
    });
});
```

> 但是，你也可以為公共用戶和已登錄用戶分別執行：

```php
// maximum of 10 requests for guests, 60 for authenticated users
Route::middleware('throttle:10|60,1')->group(function () {
    //
});
```

> 此外，你可以在 DB 字段 `users.rate_limit` 中設置用戶的限制量：

```php
Route::middleware('auth:api', 'throttle:rate_limit,1')->group(function () {
    Route::get('/user', function () {
        //
    });
});
```

<h3 id="query-string-parameters-to-routes">將查詢字串傳遞給路由</h3>

> 如果你將額外參數傳遞給路由，這些鍵/值對將自動添加到生成的 URL 的查詢字符串中。

```php
Route::get('user/{id}/profile', function ($id) {
    //
})->name('profile');

$url = route('profile', ['id' => 1, 'photos' => 'yes']); // 結果: /user/1/profile?photos=yes
```

<h3 id="separate-routes-by-files">按照文件分離路由</h3>

> 如果你有一組與某個「部分」相關的路由，你可以將它們分開到一個特殊的 `routes/XXXXX.php` 文件中，<br/>
> 然後只需在 `routes/web.php` 中包含它。<br/><br/>
> 例如，你可以將 `routes/auth.php` 文件放在 `routes` 目錄中，然後在 `routes/web.php` 中包含它。<br/>
> 像是： [Laravel Breeze](https://github.com/laravel/breeze/blob/2.x/stubs/api/routes/web.php)

```php
Route::get('/', function () {
    return view('welcome');
});

Route::get('/dashboard', function () {
    return view('dashboard');
})->middleware(['auth'])->name('dashboard');

require __DIR__.'/auth.php';
```

> 然後，在 `routes/auth.php` 中：

```php
use App\Http\Controllers\Auth\AuthenticatedSessionController;
use App\Http\Controllers\Auth\RegisteredUserController;
// ... more controllers

use Illuminate\Support\Facades\Route;

Route::get('/register', [RegisteredUserController::class, 'create'])
                ->middleware('guest')
                ->name('register');

Route::post('/register', [RegisteredUserController::class, 'store'])
                ->middleware('guest');

// ... A dozen more routes
```

> 但是，只有在該獨立路由文件具有相同的 *前綴/中介層* 設置時，才應該使用此 `include()`。
> 否則，最好將它們分組在 `app/Providers/RouteServiceProvider` 中：

```php
public function boot()
{
    $this->configureRateLimiting();

    $this->routes(function () {
        Route::prefix('api')
            ->middleware('api')
            ->namespace($this->namespace)
            ->group(base_path('routes/api.php'));

        Route::middleware('web')
            ->namespace($this->namespace)
            ->group(base_path('routes/web.php'));

        // ... Your routes file listed next here
    });
}
```

<h3 id="translate-resource-verbs">翻譯資源的動詞(FOR SEO)</h3>

> 如果你使用資源控制器，但想為了 SEO 目的將 URL 動詞更改為非英語，<br/>
> 所以你想要西班牙語 `/crear` 而不是 `/create`，你可以使用 `Route::resourceVerbs()` 方法在 `App\Providers\RouteServiceProvider` 中配置：

```php
public function boot()
{
    Route::resourceVerbs([
        'create' => 'crear',
        'edit' => 'editar',
    ]);

    // ...
}
```

<h3 id="custom-resource-route-names">自定義路由名稱</h3>

> 當使用資源控制器時，在 `routes/web.php` 中，你可以指定 `->names()` 參數，<br/>
> 這樣瀏覽器中的 URL 前綴和你在整個 Laravel 項目中使用的路由名稱前綴可能不同。

```php
Route::resource('p', ProductController::class)->names('products');
```

> 這樣將生成像 `/p`, `/p/{id}`, `/p/{id}/edit` 等 URL。<br/>
> 但你在代碼中調用它們時應該是 `route('products.index')`, `route('products.create')`, 等。

<h3 id="eager-load-relationship">儘早的讀出關聯</h3>

> 如果你使用路由模型綁定，並認為不能使用 Eager Loading 來讀取關聯，那麼你錯了。<br/>
> 你實際上可以使用 `->load()` 加載關聯。

```php
public function show(Product $product) {
    //
}
```

> 但是你有一個 belongsTo 關係，不能使用 `$product->with('category')` 預加載嗎？<br/>
> 你實際上可以！使用 `->load()` 加載關聯。

```php
public function show(Product $product) {
    $product->load('category');
    //
}
```

<h3 id="easily-highlight-your-navbar-menus">簡單高亮你的導航菜單</h3>

> 使用 `Route::is('route-name')` 輕鬆高亮你的導航菜單

```php
<ul>
    <li @if(Route::is('home')) class="active" @endif>
        <a href="/">Home</a>
    </li>
    <li @if(Route::is('contact-us')) class="active" @endif>
        <a href="/contact-us">Contact us</a>
    </li>
</ul>
```

源至 [@anwar_nairi](https://twitter.com/anwar_nairi/status/1443893957507747849)

<h3 id="generate-absolute-path-using-route-helper">使用 route() 產生相對路徑</h3>

```php
route('page.show', $page->id);
// http://laravel.test/pages/1

route('page.show', $page->id, false); // 第三個參數設置為 false
// /pages/1
```

源至 [@oliverds\_](https://twitter.com/oliverds_/status/1445796035742240770)

<h3 id="override-the-route-binding-resolver-for-each-of-your-models">為每個 Model 覆寫路由綁定解析器</h3>

> 你可以為每個 Model 覆寫路由綁定解析器。在這個例子中，我無法控制 URL 中的 @ 符號，所以使用 `resolveRouteBinding` 方法，我能夠刪除 @ 符號並解析模型。

```php
// Route
Route::get('{product:slug}', Controller::class);

// Request
// https://nodejs.pub/@unlock/hello-world

// Product Model
public function resolveRouteBinding($value, $field = null)
{
    $value = str_replace('@', '', $value);

    return parent::resolveRouteBinding($value, $field);
}
```

源至 [@Philo01](https://twitter.com/Philo01/status/1447539300397195269)

<h3 id="if-you-need-public-url-but-you-want-them-to-be-secured">如果你需要公開URL但同時希望他們是安全的</h3>

> 如果你需要公開URL但希望它們是安全的，使用 Laravel 簽名URL

```php
class AccountController extends Controller
{
    public function destroy(Request $request)
    {
        $confirmDeleteUrl = URL::signedRoute('confirm-destroy', [
            $user => $request->user()
        ]);
        // Send link by email...
    }

    public function confirmDestroy(Request $request, User $user)
    {
        if (! $request->hasValidSignature()) {
            abort(403);
        }

        // User confirmed by clicking on the email
        $user->delete();

        return redirect()->route('home');
    }
}
```

源至 [@anwar_nairi](https://twitter.com/anwar_nairi/status/1448239591467589633)

<h3 id="Using Gate in middleware method">使用 Gate 在中介方法</h3>

> 你可以在中介方法中使用你在 `App\Providers\AuthServiceProvider` 中指定的權限。
> 要使用這個，你只需要在 `can:` 中指定所需的 `gate` 名稱

```php
Route::put('/post/{post}', function (Post $post) {
    // The current user may update the post...
})->middleware('can:update,post');
```

<h3 id="simple-route-with-arrow-function">簡單 route 使用箭頭函數</h3>

> 你可以在路由中使用 php 箭頭函數，而不必使用匿名函數。

```php
// Instead of
Route::get('/example', function () {
    return User::all();
});

// You can
Route::get('/example', fn () => User::all());
```

<h3 id="route-view">路由直接返回視圖</h3>

> 你可以使用 `Route::view($uri , $bladePage)` 直接返回視圖，而不必使用控制器函數。

```php
//this will return home.blade.php view
Route::view('/home', 'home');
```

<h3 id="route-directory-instead-of-route-file">路由資料夾加載</h3>

> 你可以建立一個 _/routes/web/_ 目錄，並只填充 _/routes/web.php_：

```php
foreach(glob(dirname(__FILE__).'/web/*', GLOB_NOSORT) as $route_file){
    include $route_file;
}
```

> 現在 _/routes/web/_ 中的每個文件都作為 Web 路由文件，你可以將路由組繪製到不同的文件中。

<h3 id="route-resources-grouping">路由資源群組</h3>

> 如果你的路由有很多資源控制器，你可以將它們分組並調用一個 `Route::resources()` 而不是許多單個 `Route::resource()` 聲明。

```php
Route::resources([
    'photos' => PhotoController::class,
    'posts' => PostController::class,
]);
```

<h3 id="custom-route-bindings">自定義路由綁定</h3>

> 你知道你可以在 Laravel 中定義自定義路由綁定嗎？<br/>
> 在這個例子中，我需要通過 slug 解析投資組合。但是 slug 不是唯一的，因為多個用戶可以有名為 'Foo' 的投資組合。<br/>
> 所以我定義了 Laravel 應該如何從路由參數解析它們。

```php
class RouteServiceProvider extends ServiceProvider
{
    public const HOME = '/dashboard';

    public function boot()
    {
        Route::bind('portfolio', function (string $slug) {
            return Portfolio::query()
                ->whereBelongsto(request()->user())
                ->whereSlug($slug)
                ->firstOrFail();
        });
    }
}
```

```php
Route::get('portfolios/{portfolio}', function (Portfolio $portfolio) {
    /*
     * The $portfolio will be the result of the query defined in the RouteServiceProvider
     */
})
```

源至 [@mmartin_joo](https://twitter.com/mmartin_joo/status/1496871240346509312)

<h3 id="Two ways to check the route name">兩個方法檢查路由名稱</h3>

> 這裡有兩種檢查 Laravel 中路由名稱的方法。

```php
// #1: Route::current()->getName() === 'home'
<a
    href="{{ route('home') }}"
    @class="['navbar-link', 'active' => Route::current()->getName() === 'home']"
>
    Home
</a>
// #2: request()->routeIs('home)
<a
    href="{{ route('home') }}"
    @class="['navbar-link', 'active' => request()->routeIs('home)]"
>
    Home
</a>
```

源至 [@AndrewSavetchuk](https://twitter.com/AndrewSavetchuk/status/1510197418909999109)

<h3 id="route-model-binding-soft-deleted-models">路由綁定Model，並且撈出軟刪除的資訊</h3>

> 使用路由模型綁定時，預設情況下不會檢索已軟刪除的模型。<br/>
> 你可以通過在路由中使用 `withTrashed` 來更改該行為。

```php
Route::get('/posts/{post}', function (Post $post) {
    return $post;
})->withTrashed();
```

源至 [@cosmeescobedo](https://twitter.com/cosmeescobedo/status/1511154599255703553)

<h3 id="retrieve-the-url-without-query-parameters">取回 URL 並且不要有請求參數</h3>

> 如果由於某種原因，你的 URL 具有查詢參數，你可以使用請求的 `fullUrlWithoutQuery` 方法來取回不帶查詢參數的 URL。

```php
// Original URL: https://www.amitmerchant.com?search=laravel&lang=en&sort=desc
$urlWithQueryString = $request->fullUrlWithoutQuery([
    'lang',
    'sort'
]);
echo $urlWithQueryString;
// Outputs: https://www.amitmerchant.com?search=laravel
```

源至 [@amit_merchant](https://twitter.com/amit_merchant/status/1510867527962066944)

<h3 id="customizing-missing-model-behavior-in-route-model-bindings">自定義路由模型綁定中缺失的行為</h3>

> 默認情況下，當 Laravel 無法綁定模型時，它會拋出 404 錯誤，但你可以通過向 `missing` 方法傳遞一個閉包來更改該行為。

```php
Route::get('/users/{user}', [UsersController::class, 'show'])
    ->missing(function ($parameters) {
        return Redirect::route('users.index');
    });
```

源至 [@cosmeescobedo](https://twitter.com/cosmeescobedo/status/1511322007576608769)

<h3 id="exclude-middleware-from-a-route">在某個路由中撇除中介層</h3>

> 你可以在 Laravel 中使用 `withoutMiddleware` 方法在某個路由中排除中介層。

```php
Route::post('/some/route', SomeController::class)
    ->withoutMiddleware([VerifyCsrfToken::class]);
```

源至 [@alexjgarrett](https://twitter.com/alexjgarrett/status/1512529798790320129)
