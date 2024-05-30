## Log and debug

⬆️ [返回主要選單](README.md#laravel-tips) ⬅️ [返回上一個 (「工廠」)](factories.md) ➡️ [下一個 (「應用開發介面」)](api.md)

- [帶入參數的日誌 - Logging with parameters](#logging-with-parameters)
- [紀錄運行較久的動作 - Log Long Running Laravel Queries](#log-long-running-laravel-queries)
- [壓測class - Benchmark class](#benchmark-class)
- [更多方便的 `dd` - More convenient DD](#more-convenient-dd)
- [日誌與上下文 - Log with context](#log-with-context)
- [快速以 SQL 的形式輸出 Eloquent 的查詢 - Quickly output an Eloquent query in its SQL form](#quickly-output-an-eloquent-query-in-its-sql-form)
- [在開發模式下 Log 所有的 DB 語句 - Log all the database queries during development](#log-all-the-database-queries-during-development)
- [發現在一個請求中觸發所有的事件 - Discover all events fired in one request](#discover-all-events-fired-in-one-request)

<h3 id="logging-with-parameters">帶入參數的日誌</h3>

> 你可以寫 `Log::info()`，或更簡短的 `info()` 訊息加上額外參數，以提供更多發生的上下文。(幫助你更快找到問題)

```php
Log::info('User failed to login.', ['id' => $user->id]);
```

<h3 id="log-long-running-laravel-queries">紀錄運行較久的動作</h3>

```php
DB::enableQueryLog();

DB::whenQueryingForLongerThen(1000, function ($connection) {
     Log::warning(
          'Long running queries have been detected.',
          $connection->getQueryLog()
     );
});
```

源至 [@realstevebauman](https://twitter.com/realstevebauman/status/1576980397552185344)

<h3 id="benchmark-class">壓測class</h3>

> 在 Laravel 9.32 中，我們有一個 Benchmark class 可以測量任務的時間。

這是一個相當有用的幫手:
```php
class OrderController
{
     public function index()
     {
          return Benchmark::measure(fn () => Order::all()),
     }
}
```

源至 [@mmartin_joo](https://twitter.com/mmartin_joo/status/1583096196494553088)

<h3 id="more-convenient-dd">更多方便的 `dd`</h3>

> 你可以在 Eloquent 語句或任何 Collection 的最後加上 `->dd()` 作為方法，而不是使用 `dd($result)`。

```php
// 原本可能寫成這樣
$users = User::where('name', 'Taylor')->get();
dd($users);
// 加入這個屬性後可以這樣寫(更簡潔)
$users = User::where('name', 'Taylor')->get()->dd();
```

<h3 id="log-with-context">日誌與上下文</h3>

> 在 Laravel 8.49 中: `Log::withContext()` 將幫助你區分不同請求之間的 Log 訊息。<br/>
> 如果你建立一個 Middleware 並設置這個上下文，所有 Log 訊息將包含該上下文，你將能更輕鬆地搜尋它們。

```php
public function handle(Request $request, Closure $next)
{
    $requestId = (string) Str::uuid();

    Log::withContext(['request-id' => $requestId]);

    $response = $next($request);

    $response->header('request-id', $requestId);

    return $response;
}
```

<h3 id="quickly-output-an-eloquent-query-in-its-sql-form">快速以 SQL 的形式輸出 Eloquent 的查詢</h3>

> 如果你想快速輸出 Eloquent 查詢的 SQL 形式，你可以像這樣將 `toSql()` 方法加到它上面。

```php
$invoices = Invoice::where('client', 'James pay')->toSql();

dd($invoices)
// select * from `invoices` where `client` = ?
```

源至 [@devThaer](https://twitter.com/devThaer/status/1438816135881822210)

<h3 id="log-all-the-database-queries-during-development">在開發模式下 Log 所有的 DB 語句</h3>

> 如果你想在開發時記錄所有的資料庫查詢，請將此片段添加到你的 AppServiceProvider 中。

```php
public function boot()
{
    if (App::environment('local')) {
        DB::listen(function ($query) {
            logger(Str::replaceArray('?', $query->bindings, $query->sql));
        });
    }
}
```

源至 [@mmartin_joo](https://twitter.com/mmartin_joo/status/1473262634405449730)

<h3 id="discover-all-events-fired-in-one-request">發現在一個請求中觸發所有的事件</h3>

> 如果你想要實現一個新的監聽器來監聽特定事件，但你不知道它的名稱，你可以在請求期間記錄所有觸發的事件。<br/>
> <br/>
> 你可以在 `app/Providers/EventServiceProvider.php` 的 `boot()` 方法中使用 `\Illuminate\Support\Facades\Event::listen()` 方法來捕獲所有觸發的事件。<br/>
> <br/>
> **重要:** 如果你在這個事件監聽器中使用 `Log` facade，那麼你需要排除名為 `Illuminate\Log\Events\MessageLogged` 的事件，以避免無限循環。(例如: `if ($event == 'Illuminate\\Log\\Events\\MessageLogged') return;`)

```php
// Include Event...
use Illuminate\Support\Facades\Event;

// In your EventServiceProvider class...
public function boot()
{
    parent::boot();

    Event::listen('*', function ($event, array $data) {
        // Log the event class
        error_log($event);

        // Log the event data delegated to listener parameters
        error_log(json_encode($data, JSON_PRETTY_PRINT));
    });
}
```

源至 [@MuriloChianfa](https://github.com/MuriloChianfa)
