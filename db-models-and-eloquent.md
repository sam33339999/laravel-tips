## DB Models and Eloquent

⬆️ [返回主要選單](README.md#laravel-tips) ➡️ [下一個 (「DB 關聯」)](models-relations.md)

- [重用或克隆query() - Reuse or clone query()](#reuse-or-clone-query)
- [記得在原始查詢中使用綁定 - Remember to use bindings in your raw queries](#remember-to-use-bindings-in-your-raw-queries)
- [使用 Laravel 在 MySQL 上進行全文搜尋的小抄 - Small cheat-sheet for using Full-Text Search with Laravel on MySQL](#small-cheat-sheet-for-using-full-text-search-with-laravel-on-mysql)
- [合併 Eloquent 搜尋結果 - Merging eloquent collections](#merging-eloquent-collections)
- [不修改 updated_at 字段的情况下執行操作 - Perform operation without modifying updated_at field](#perform-operation-without-modifying-updated_at-field)
- [你可以寫出交易後觸發的程式碼 - You can write transaction-aware code](#you-can-write-transaction-aware-code)
- [在其他關係中的 Eloquent 範疇 - Eloquent scopes inside of other relationships](#eloquent-scopes-inside-of-other-relationships)
- [自 Laravel 9.37 版本起的新 `rawValue()` 方法 - New `rawValue()` method since Laravel 9.37](#new-rawvalue-method-since-laravel-937)
- [當目標值為整數時，加快資料載入速度 - Load data faster when the targeted value is an integer](#load-data-faster-when-the-targeted-value-is-an-integer)
- [載入資料完成於兩個時間戳記之間 - Load data completed between two timestamps](#load-data-completed-between-two-timestamps)
- [傳遞原始查詢以排序你的結果 - Pass a raw query to order your results](#pass-a-raw-query-to-order-your-results)
- [Eloquent 的 where date 方法 - Eloquent where date methods](#eloquent-where-date-methods)
- [遞增和遞減 - Increments and decrements](#increments-and-decrements)
- [沒有時間戳記欄位 - No timestamp columns](#no-timestamp-columns)
- [軟刪除: 多重還原 - Soft-deletes: multiple restore](#soft-deletes-multiple-restore)
- [Model all: 欄位 - Model all: columns](#model-all-columns)
- [要失敗或不要失敗 - To Fail or not to Fail](#to-fail-or-not-to-fail)
- [欄位名稱變更 - Column name change](#column-name-change)
- [映射查詢結果 - Map query results](#map-query-results)
- [變更預設時間戳記欄位 - Change Default Timestamp Fields](#change-default-timestamp-fields)
- [依 created_at 快速排序 - Quick Order by created_at](#quick-order-by-created_at)
- [建立紀錄時自動填入欄位值 - Automatic Column Value When Creating Records](#automatic-column-value-when-creating-records)
- [DB 原生查詢計算運行更快 - DB Raw Query Calculations Run Faster](#db-raw-query-calculations-run-faster)
- [多於一個 scope - More than One Scope](#more-than-one-scope)
- [無需轉換 Carbon - No Need to Convert Carbon](#no-need-to-convert-carbon)
- [依首字母分組 - Grouping by First Letter](#grouping-by-first-letter) <---- here
- [永不更新該欄位 - Never Update the Column](#never-update-the-column)
- [尋找多個 - Find Many](#find-many)
- [尋找多個並返回特定欄位 - Find Many and return specific columns](#find-many-and-return-specific-columns)
- [依鍵尋找 - Find by Key](#find-by-key)
- [使用UUID而非自動遞增 - Use UUID instead of auto-increment](#use-uuid-instead-of-auto-increment)
- [Laravel 方式的子查詢 - Sub-selects in Laravel Way](#sub-selects-in-laravel-way)
- [隱藏部分欄位 - Hide Some Columns](#hide-some-columns)
- [精確的 DB 錯誤 - Exact DB Error](#exact-db-error)
- [使用查詢建構器進行軟刪除 - Soft-Deletes with Query Builder](#soft-deletes-with-query-builder)
- [好老的 SQL 查詢 - Good Old SQL Query](#good-old-sql-query)
- [使用 DB 交易 - Use DB Transactions](#use-db-transactions)
- [更新或建立 - Update or Create](#update-or-create)
- [儲存時刪除快取 - Forget Cache on Save](#forget-cache-on-save)
- [變更 created_at 和 updated_at 的格式 - Change Format Of Created_at and Updated_at](#change-format-of-created_at-and-updated_at)
- [將陣列型別儲存為 JSON - Storing Array Type into JSON](#storing-array-type-into-json)- [將陣列型別儲存為JSON - Storing Array Type into JSON](#storing-array-type-into-json)
- [製作 Model 的副本 - Make a Copy of the Model](#make-a-copy-of-the-model)
- [減少記憶體使用量 - Reduce Memory](#reduce-memory)
- [強制查詢時不使用 $fillable/$guarded - Force query without $fillable/$guarded](#force-query-without-fillableguarded)
- [三層父子結構 - 3-level structure of parent-children](#3-level-structure-of-parent-children)
- [失敗時執行任何操作 - Perform any action on failure](#perform-any-action-on-failure)
- [檢查紀錄是否存在或顯示 404 - Check if record exists or show 404](#check-if-record-exists-or-show-404)
- [如果條件失敗則中止 - Abort if condition failed](#abort-if-condition-failed)
- [在持久化資料到資料庫時自動填入欄位 - Fill a column automatically while you persist data to the database](#fill-a-column-automatically-while-you-persist-data-to-the-database)
- [關於查詢的額外資訊 - Extra information about the query](#extra-information-about-the-query)
- [在Laravel中使用 `doesntExist()` 方法 - Using the doesntExist() method in Laravel](#using-the-doesntexist-method-in-laravel)
- [要新增到少數幾個Model以自動呼叫其 `boot()` 方法的Trait - Trait that you want to add to a few Models to call their boot() method automatically](#trait-that-you-want-to-add-to-a-few-models-to-call-their-boot-method-automatically)
- [在 Laravel 中有兩種常見的方式來確定表格是否為空 - There are two common ways of determining if a table is empty in Laravel](#there-are-two-common-ways-of-determining-if-a-table-is-empty-in-laravel)
- [如何防止*非物件的屬性*錯誤 - How to prevent "property of non-object" error](#how-to-prevent-property-of-non-object-error)
- [在變更 Eloquent 記錄後獲取原始屬性 - Get original attributes after mutating an Eloquent record](#get-original-attributes-after-mutating-an-eloquent-record)
- [簡單的資料庫播種方式 - A simple way to seed a database](#a-simple-way-to-seed-a-database)
- [查詢構造器的crossJoinSub方法 - The crossJoinSub method of the query constructor](#the-crossjoinsub-method-of-the-query-constructor)
- [多對多的樞紐表命名 - Belongs to Many Pivot table naming](#belongs-to-many-pivot-table-naming)
- [根據樞紐欄位排序 - Order by Pivot Fields](#order-by-pivot-fields)
- [從資料庫中尋找單一記錄 - Find a single record from a database](#find-a-single-record-from-a-database)
- [自動分塊記錄 - Automatic records chunking](#automatic-records-chunking)
- [更新模型時不觸發事件 - Updating the model without dispatching events](#updating-the-model-without-dispatching-events)
- [定期從陳舊記錄中清理模型 - Periodic cleaning of models from obsolete records](#periodic-cleaning-of-models-from-obsolete-records)
- [不可變日期和轉換為它們 - Immutable dates and casting to them](#immutable-dates-and-casting-to-them)
- [findOrFail方法也可接受id列表 - The findOrFail method also accepts a list of ids](#the-findorfail-method-also-accepts-a-list-of-ids)
- [可剔除的 Trait 可自動從資料庫中移除模型 - Prunable trait to automatically remove models from your database](#prunable-trait-to-automatically-remove-models-from-your-database)
- [`withAggregate` 方法 - withAggregate method](#withaggregate-method)
- [日期慣例 - Date convention](#date-convention)
- [Eloquent 多重更新/插入 - Eloquent multiple upserts](#eloquent-multiple-upserts)
- [在過濾結果後取得查詢建構器 - Retrieve the Query Builder after filtering the results](#retrieve-the-query-builder-after-filtering-the-results)
- [自訂類型轉換 - Custom casts](#custom-casts)
- [根據相關模型的平均值或計數排序 - Order based on a related model's average or count](#order-based-on-a-related-models-average-or-count)
- [返回交易結果 - Return transactions result](#return-transactions-result)
- [從查詢中移除多個全域範疇 - Remove several global scopes from query](#remove-several-global-scopes-from-query)
- [排序JSON欄位屬性 - Order JSON column attribute](#order-json-column-attribute)
- [從第一個結果獲取單一欄位的值 - Get single column's value from the first result](#get-single-columns-value-from-the-first-result)
- [檢查已更改的值是否更改了鍵 - Check if altered value changed key](#check-if-altered-value-changed-key)
- [定義存取器和修改器的新方式 - New way to define accessor and mutator](#new-way-to-define-accessor-and-mutator)
- [另一種實現存取器和修改器的方式 - Another way to do accessors and mutators](#another-way-to-do-accessors-and-mutators)
- [在搜尋第一筆記錄時，你可以執行一些動作 - When searching for the first record, you can perform some actions](#when-searching-for-the-first-record-you-can-perform-some-actions)
- [直接將 created_at 日期轉換為人類可讀格式 - Directly convert created_at date to human readable format](#directly-convert-created_at-date-to-human-readable-format)
- [按 Eloquent 存取器排序 - Ordering by an Eloquent Accessor](#ordering-by-an-eloquent-accessor)
- [檢查特定模型是否已建立或找到 - Check for specific model was created or found](#check-for-specific-model-was-created-or-found)
- [使用資料庫驅動器的 Laravel Scout - Laravel Scout with database driver](#laravel-scout-with-database-driver)
- [利用查詢建構器上的 value 方法 - Make use of the value method on the query builder](#make-use-of-the-value-method-on-the-query-builder)
- [將陣列傳遞給 where 方法 - Pass array to where method](#pass-array-to-where-method)
- [從模型集合返回主鍵 - Return the primary keys from models collection](#return-the-primary-keys-from-models-collection)
- [強制 Laravel 使用 eager loading - Force Laravel to use eager loading](#force-laravel-to-use-eager-loading)
- [讓所有模型均可大量賦值 - Make all your models mass assignable](#make-all-your-models-mass-assignable)
- [在select all語句中隱藏欄位 - Hiding columns in select all statements](#hiding-columns-in-select-all-statements)
- [JSON Where子句 - JSON Where Clauses](#json-where-clauses)
- [獲取表格的所有欄位名稱 - Get all the column names for a table](#get-all-the-column-names-for-a-table)
- [比較兩個欄位的值 - Compare the values of two columns](#compare-the-values-of-two-columns)
- [存取器快取 - Accessor Caching](#accessor-caching)
- [新的 `scalar()` 方法 - New scalar() method](#new-scalar-method)
- [選擇特定欄位 - Select specific columns](#select-specific-columns)
- [在查詢中鏈結條件子句而無需編寫if-else語句 - Chain conditional clauses to the query without writing if-else statements](#chain-conditional-clauses-to-the-query-without-writing-if-else-statements)
- [在模型中覆寫 Connection 屬性 - Override Connection Attribute in Models](#override-connection-attribute-in-models)
- [在 Where 子句中使用欄位名稱（動態Where子句） - Using Column Names in Where Clauses (Dynamic Where Clauses)](#using-column-names-in-where-clauses-dynamic-where-clauses)
- [使用 `firstOrCreate()` - Using firstOrCreate()](#using-firstorcreate)

<h3 id="reuse-or-clone-query">重用或克隆query()</h3>

> 通常，我們需要從篩選查詢中查詢多次。因此，大多數時候我們使用 `query()` 方法，<br/>
> 來寫一個查詢，以獲取今天創建的活動和非活動產品

```php

$query = Product::query();

$today = request()->q_date ?? today();
if($today){
    $query->where('created_at', $today);
}

// 取得 啟用和未啟用 的產品
$active_products = $query->where('status', 1)->get(); // 這行已經編輯過查詢物件了
$inactive_products = $query->where('status', 0)->get(); // 所以這邊會找不到未啟用的物件
```
> 但是，在獲得 `$active products` 之後，`$query` 將被修改。<br/>
> 因此，`$inactive_products` 將無法從 `$query` 中找到任何未啟用的產品，<br/>
> 這將每次都返回空集合。因為這將嘗試從 `$active_products` 中找到未啟用的產品（`$query` 將僅返回啟用的產品）。<br/><br/>
> 為了解決這個問題，我們可以多次查詢，重複使用這個 `$query` 物件。<br/>
> 因此，在進行任何 `$query` 修改操作之前，我們需要克隆這個 `$query` 物件。

```php
$active_products = $query->clone()->where('status', 1)->get(); // it will not modify the $query
$inactive_products = $query->clone()->where('status', 0)->get(); // so we will get inactive products from $query

```

<h3 id="remember-to-use-bindings-in-your-raw-queries">記得在原始查詢中使用綁定</h3>

> 您可以將大多數原始查詢方法的綁定陣列傳遞給大多數原始查詢方法，以避免 SQL 注入。

```php
// This is vulnerable to SQL injection
$fullname = request('full_name');
User::whereRaw("CONCAT(first_name, last_name) = $fullName")->get();

// Use bindings
User::whereRaw("CONCAT(first_name, last_name) = ?", [request('full_name')])->get();
```

源至 [@cosmeescobedo](https://twitter.com/cosmeescobedo/status/1565806352219328513)

<h3 id="small-cheat-sheet-for-using-full-text-search-with-laravel-on-mysql">使用 Laravel 在 MySQL 上進行全文搜尋的小抄</h3>

Migration
```php
Schema::create('comments', function (Blueprint $table) {
     $table->id();
     $table->string('title');
     $table->text('description');

    // 這將在 `title` 和 `description` 上創建全文索引，以便您可以使用全文搜索。
     $table->fullText(['title', 'description']);
});
```

> 自然語言: <br/>
> 搜尋關於 `something`。
```php
Comment::whereFulltext(['title', 'description'], 'something')->get();
```

> 自然語言使用語句擴展: <br/>
> 搜尋 `something` 並使用結果執行更大的查詢。

```php
Comment::whereFulltext(['title', 'description'], 'something', ['expanded' => true])->get();
```

> 布林模式: <br/>
> 搜尋 `something` 和 `else`。

```php
Comment::whereFulltext(['title', 'description'], '+something -else', ['mode' => 'boolean'])->get();
```

源至 [@w3Nicolas](https://twitter.com/w3Nicolas/status/1566694849767772160/)

<h3 id="merging-eloquent-collections">合併 Eloquent 搜尋結果</h3>

> Eloquent 集合的 `merge` 方法使用 id 來避免重複的模型。<br/>
> 但是，如果合併不同模型的集合，可能會導致意外的結果。<br/>
> 請改用基本集合方法。

```php
$videos = Video::all();
$images = Image::all();

// If there are videos with the same id as images they will get replaced
// You'll end up with missing videos
$allMedia = $videos->merge($images);

// call `toBase()` in your eloquent collection to use the base merge method instead
$allMedia = $videos->toBase()->merge($images);
```

源至 [@cosmeescobedo](https://twitter.com/cosmeescobedo/status/1568392184772296706)

<h3 id="perform-operation-without-modifying-updated_at-field">不修改 updated_at 字段的情况下執行操作</h3>

> 如果您想要在不修改模型的 `updated_at` 时间戳的情况下執行模型操作，<br/>
> 您可以在 `withoutTimestamps` 方法中给定的閉包中對模型進行操作。<br/><br/>
> Laravel 9.31 後可用。

```php
$user = User::first();

// `updated_at` is not changed...

User::withoutTimestamps(
     fn () => $user->update(['reserved_at' => now()])
);
```

源至 [@LaravelEloquent](https://twitter.com/LaravelEloquent/status/1573787406528126976)

<h3 id="you-can-write-transaction-aware-code">你可以寫出交易後觸發的程式碼</h3>

> 使用 `DB::afterCommit` 方法，您可以編寫只有在交易提交成功後才執行的程式碼，<br/>
> 如果交易回滾，則會被丟棄。

```php
DB::transaction(function () {
     $user = User::create([...]);

     $user->teams()->create([...]);
});
```

```php
class User extends Model
{
     protected static function booted()
     {
          static::created(function ($user) {
               // Will send the email only if the
               // transaction is committed
               DB::afterCommit(function () use ($user) {
                    Mail::send(new WelcomeEmail($user));
               });
          });
     }
}
```

源至 [@cosmeescobedo](https://twitter.com/cosmeescobedo/status/1583960872602390528)

<h3 id="eloquent-scopes-inside-of-other-relationships">在其他關係中的 Eloquent 範疇</h3>

> 你知道你可以在定義其他關係時使用 Eloquent 範疇嗎？

```php
// app/Models/Lesson.php - 課堂Model
public function scopePublished($query)
{
     return $query->where('is_published', true);
}
```

```php
// app/Models/Course.php - 課程Model
public function lessons(): HasMany
{
     return $this->hasMany(Lesson::class);
}

public function publishedLessons(): HasMany
{
     return $this->lessons()->published();
}
```

<h3 id="new-rawvalue-method-since-laravel-937">自 Laravel 9.37 版本起的新 `rawValue()` 方法</h3>

> Laravel 9.37 版本有一個新的 `rawValue()` 方法，用於從 SQL 表達式中獲取值。<br/>
```php
$first = TripModel::orderBy('date_at', 'ASC')
     ->rawValue('YEAR(`date_at`)');
$last = TripModel::orderBy('date_at', 'DESC')
     ->rawValue('YEAR(`date_at`)');

$fullname = UserModel::where('id', $id)
     ->rawValue('CONCAT(`first_name`, " ", `last_name`)');
```

源至 [@LoydRG](https://twitter.com/LoydRG/status/1587689148768567298)

<h3 id="load-data-faster-when-the-targeted-value-is-an-integer">當目標值為整數時，加快資料載入速度</h3>

> 當目標值為整數時，不要使用 `whereIn()` 方法來加載大範圍的數據，<br/>
> 而是使用 `whereIntegerInRaw()` 方法，這比 `whereIn()` 更快。

```php
// 不要使用whereIn方法來加載大範圍的數據
Product::whereIn('id', range(1, 50))->get();

// 使用 WhereIntegerInRaw 方法來加載更快
Product::whereIntegerInRaw('id', range(1, 50))->get();
```

源至 [@LaraShout](https://twitter.com/LaraShout)

<h3 id="load-data-completed-between-two-timestamps">載入資料完成於兩個時間戳記之間</h3>

> 使用 `whereBetween` 在兩個時間戳記之間加載記錄，您可以使用空值合併運算符（??）傳遞回退值。

```php
// 讀取 task 的 completed_at 在兩個時間戳記之間的任務   
Task::whereBetween('completed_at', [
    $request->from ?? '2023-01-01',
    $request->to ??  today()->toDateTimeString(),
]);
```

源至 [@LaraShout](https://twitter.com/LaraShout)

<h3 id="pass-a-raw-query-to-order-your-results">傳遞原始查詢以排序你的結果</h3>

> 您可以傳遞原始查詢以排序您的結果。<br/>
> 例如，按照截止日期前完成任務的時間排序任務。

```php
// 依照任務在截止日期前完成的時間排序任務
$tasks = Task::query()
    ->whereNotNull('completed_at')
    ->orderByRaw('due_at - completed_at DESC')
    ->get();
```

源至 [@cosmeescobedo](https://twitter.com/cosmeescobedo)

<h3 id="eloquent-where-date-methods">Eloquent 的 where date 方法</h3>

> 在 Eloquent 中，使用 `whereDay()`、`whereMonth()`、`whereYear()`、`whereDate()` 和 `whereTime()` 函數檢查日期。

```php
$products = Product::whereDate('created_at', '2018-01-31')->get();
$products = Product::whereMonth('created_at', '12')->get();
$products = Product::whereDay('created_at', '31')->get();
$products = Product::whereYear('created_at', date('Y'))->get();
$products = Product::whereTime('created_at', '=', '14:13:58')->get();
```

<h3 id="increments-and-decrements">遞增和遞減</h3>

> 如果要遞增某個表中的某個列，只需使用 `increment()` 函數。<br/>
> 哦，你不僅可以遞增 1，還可以遞增某個數字，例如 50。

```php
Post::find($post_id)->increment('view_count'); // 貼文觀看次數 +1
User::find($user_id)->increment('points', 50); // 使用者積分 +50
```

<h3 id="no-timestamp-columns">沒有時間戳記欄位</h3>

> 如果您的資料庫表不包含時間戳記欄位 `created_at` 和 `updated_at`，<br/>
> 您可以指定 Eloquent 模型不使用它們，使用 `$timestamps = false` 屬性。

```php
class Company extends Model
{
    public $timestamps = false;
}
```

<h3 id="soft-deletes-multiple-restore">軟刪除: 多重還原</h3>

> 使用軟刪除時，您可以在一個語句中還原多個行。

```php
Post::onlyTrashed()->where('author_id', 1)->restore();
```

<h3 id="model-all-columns">Model all: 欄位</h3>

> 當調用 Eloquent 的 `Model::all()` 時，您可以指定要返回的欄位。

```php
$users = User::all(['id', 'name', 'email']);
```

<h3 id="to-fail-or-not-to-fail">要失敗或不要失敗</h3>

> 如果您不想在找不到記錄時引發異常，可以使用 `find()` 方法的 `null` 返回值。<br>
> 除了 `findOrFail()` 之外,Eloquent 還有 `firstOrFail()` 方法,如果查詢找不到任何記錄,它將返回 404 頁面。

```php
$user = User::where('email', 'povilas@laraveldaily.com')->firstOrFail();
```

<h3 id="column-name-change">欄位名稱變更</h3>

> 在 Eloquent 查詢生成器中，您可以指定 "as" 來返回任何具有不同名稱的列，就像在普通 SQL 查詢中一樣。

```php
$users = DB::table('users')->select('name', 'email as user_email')->get();
```

<h3 id="map-query-results">映射查詢結果</h3>

> 在 Eloquent 查詢之後，您可以使用集合中的 `map()` 函數修改行。

```php
$users = User::where('role_id', 1)->get()->map(function (User $user) {
    $user->some_column = some_function($user);
    return $user;
});
```

<h3 id="change-default-timestamp-fields">變更預設時間戳記欄位</h3>

> 如果您的時間戳記列名稱不是 `created_at` 和 `updated_at`，您可以在模型中指定它們。

```php
class Role extends Model
{
    const CREATED_AT = 'create_time';
    const UPDATED_AT = 'update_time';
}
```

<h3 id="quick-order-by-created_at">依 created_at 快速排序</h3>

> 如 1 所述，您可以使用 `orderBy()` 方法來排序結果。<br/>
> 預設情況下，`latest()` 將按 `created_at` 排序。<br/>
> 還有一個相反的方法 `oldest()`，它將按 `created_at` 升序排序。<br/>
> 此外，您可以指定另一個列來排序。例如，如果您想使用 `updated_at`，可以這樣做：

```php
User::orderBy('created_at', 'desc')->get(); // 1

User::latest('created_at')->get(); // desc
User::oldest('created_at')->get(); // asc

$lastUpdatedUser = User::latest('updated_at')->first();
```

<h3 id="automatic-column-value-when-creating-records">建立紀錄時自動填入欄位值</h3>

> 如果您想在創建記錄時生成一些 DB 列值，請將其添加到模型的 `boot()` 方法中。<br/>
> 例如，如果您有一個字段 "position"，並且想要將下一個可用位置分配給新記錄（如 `Country::max('position') + 1`），請執行以下操作：

```php
class Country extends Model {
    protected static function boot()
    {
        parent::boot();

        Country::creating(function($model) {
            $model->position = Country::max('position') + 1;
        });
    }
}
```

<h3 id="db-raw-query-calculations-run-faster">DB 原生查詢計算運行更快</h3>

> 使用 SQL 原始查詢，如 `whereRaw()` 方法，在查詢中進行一些特定於 DB 的計算，而不是在 Laravel 中進行計算，通常結果會更快。<br/>
> 例如，如果您想要獲取在註冊後 30 天以上活動的用戶，這裡是代碼：

```php
User::where('active', 1)
    ->whereRaw('TIMESTAMPDIFF(DAY, created_at, updated_at) > ?', 30)
    ->get();
```

<h3 id="more-than-one-scope">多於一個 scope</h3>

> 您可以組合和鏈接 Eloquent 中的查詢範圍，使用多個範圍在查詢中。<br/>
> Model file (like `\App\Models\User.php`)：

```php
public function scopeActive($query) {
    return $query->where('active', 1);
}

public function scopeRegisteredWithinDays($query, $days) {
    return $query->where('created_at', '>=', now()->subDays($days));
}
```

有些 Controller：

```php
$users = User::registeredWithinDays(30)->active()->get();
```

<h3 id="no-need-to-convert-carbon">無需轉換 Carbon</h3>

> 如果您正在執行 `whereDate()` 並檢查今天的記錄，您可以使用 Carbon 的 `now()`，它將自動轉換為日期。無需執行 `->toDateString()`。

```php
// Instead of
$todayUsers = User::whereDate('created_at', now()->toDateString())->get();
// No need to convert, just use now()
$todayUsers = User::whereDate('created_at', now())->get();
```

<h3 id="Grouping by First Letter">Grouping by First Letter</h3>

You can group Eloquent results by any custom condition, here's how to group by first letter of user's name:

```php
$users = User::all()->groupBy(function($item) {
    return $item->name[0];
});
```

<h3 id="Never Update the Column">Never Update the Column</h3>

If you have DB column which you want to be set only once and never updated again, you can set that restriction on Eloquent Model, with a mutator:

- In version 9 and above:

```php
use Illuminate\Database\Eloquent\Casts\Attribute;

class User extends Model
{
    protected function email(): Attribute
    {
        return Attribute::make(
            set: fn ($value, $attributes) => $attributes['email'] ?? $value,
        );
    }
}
```

- In version 9 and below:

```php
class User extends Model
{
    public function setEmailAttribute($value)
    {
        if (isset($this->attributes['email']) && ! is_null($this->attributes['email'])) {
            return;
        }
        $this->attributes['email'] = $value;
    }
}
```

<h3 id="Find Many">Find Many</h3>

Eloquent method `find()` may accept multiple parameters, and then it returns a Collection of all records found, not just one Model:

```php
// Will return Eloquent Model
$user = User::find(1);
// Will return Eloquent Collection
$users = User::find([1,2,3]);
```

```php
return Product::whereIn('id', $this->productIDs)->get();
// You can do this
return Product::find($this->productIDs)
```

源至 [@tahiriqbalnajam](https://twitter.com/tahiriqbalnajam/status/1436120403655671817)

Incase of integer, use `whereIn` with limited data range only instead use `whereIntegerInRaw` which is faster then `whereIn`.

```php
Product::whereIn('id', range(1, 50))->get();
// You can do this
Product::whereIntegerInRaw('id', range(1, 50))->get();
```

源至 [@sachinkiranti](https://raisachin.com.np)

<h3 id="Find Many and return specific columns">Find Many and return specific columns</h3>

Eloquent method `find()` may accept multiple parameters, and then it returns a Collection of all records found with specified columns, not all columns of model:

```php
// Will return Eloquent Model with first_name and email only
$user = User::find(1, ['first_name', 'email']);
// Will return Eloquent Collection with first_name and email only
$users = User::find([1,2,3], ['first_name', 'email']);
```

源至 [@tahiriqbalnajam](https://github.com/tahiriqbalnajam)

<h3 id="Find by Key">Find by Key</h3>

You can also find multiple records with `whereKey()` method which takes care of which field is exactly your primary key (`id` is the default, but you may override it in Eloquent model):

```php
$users = User::whereKey([1,2,3])->get();
```

<h3 id="Use UUID instead of auto-increment">Use UUID instead of auto-increment</h3>

You don't want to use auto incrementing ID in your model?

Migration:

```php
Schema::create('users', function (Blueprint $table) {
    // $table->increments('id');
    $table->uuid('id')->unique();
});
```

#### Laravel 9 and above:

```php
use Illuminate\Database\Eloquent\Concerns\HasUuids;
use Illuminate\Database\Eloquent\Model;

class Article extends Model
{
    use HasUuids;

    // ...
}

$article = Article::create(['title' => 'Traveling to Europe']);

$article->id; // "8f8e8478-9035-4d23-b9a7-62f4d2612ce5"
```

#### Laravel 8 and below:

Model:

- In PHP 7.4.0 and above:

```php
use Illuminate\Support\Str;
use Illuminate\Database\Eloquent\Model;

class User extends Model
{
    public $incrementing = false;
    protected $keyType = 'string';

    protected static function boot()
    {
        parent::boot();

        self::creating(fn (User $model) => $model->attributes['id'] = Str::uuid());
        self::saving(fn (User $model) => $model->attributes['id'] = Str::uuid());
    }
}
```

- In PHP older than 7.4.0:

```php
use Illuminate\Support\Str;
use Illuminate\Database\Eloquent\Model;

class User extends Model
{
    public $incrementing = false;
    protected $keyType = 'string';

    protected static function boot()
    {
        parent::boot();

        self::creating(function ($model) {
             $model->attributes['id'] = Str::uuid();
        });
        self::saving(function ($model) {
             $model->attributes['id'] = Str::uuid();
        });
    }
}
```

<h3 id="Sub-selects in Laravel Way">Sub-selects in Laravel Way</h3>

From Laravel 6, you can use addSelect() in Eloquent statement, and do some calculation to that added column.

```php
return Destination::addSelect(['last_flight' => Flight::select('name')
    ->whereColumn('destination_id', 'destinations.id')
    ->orderBy('arrived_at', 'desc')
    ->limit(1)
])->get();
```

<h3 id="Hide Some Columns">Hide Some Columns</h3>

When doing Eloquent query, if you want to hide specific field from being returned, one of the quickest ways is to add `->makeHidden()` on Collection result.

```php
$users = User::all()->makeHidden(['email_verified_at', 'deleted_at']);
```

<h3 id="Exact DB Error">Exact DB Error</h3>

If you want to catch Eloquent Query exceptions, use specific `QueryException` instead default Exception class, and you will be able to get the exact SQL code of the error.

```php
try {
    // Some Eloquent/SQL statement
} catch (\Illuminate\Database\QueryException $e) {
    if ($e->getCode() === '23000') { // integrity constraint violation
        return back()->withError('Invalid data');
    }
}
```

<h3 id="Soft-Deletes with Query Builder">Soft-Deletes with Query Builder</h3>

Don't forget that soft-deletes will exclude entries when you use Eloquent, but won't work if you use Query Builder.

```php
// Will exclude soft-deleted entries
$users = User::all();

// Will NOT exclude soft-deleted entries
$users = User::withTrashed()->get();

// Will NOT exclude soft-deleted entries
$users = DB::table('users')->get();
```

<h3 id="Good Old SQL Query">Good Old SQL Query</h3>

If you need to execute a simple SQL query, without getting any results - like changing something in DB schema, you can just do `DB::statement()`.

```php
DB::statement('DROP TABLE users');
DB::statement('ALTER TABLE projects AUTO_INCREMENT=123');
```

<h3 id="Use DB Transactions">Use DB Transactions</h3>

If you have two DB operations performed, and second may get an error, then you should rollback the first one, right?

For that, I suggest to use DB Transactions, it's really easy in Laravel:

```php
DB::transaction(function () {
    DB::table('users')->update(['votes' => 1]);

    DB::table('posts')->delete();
});
```

<h3 id="Update or Create">Update or Create</h3>

If you need to check if the record exists, and then update it, or create a new record otherwise, you can do it in one sentence - use Eloquent method `updateOrCreate()`:

```php
// Instead of this
$flight = Flight::where('departure', 'Oakland')
    ->where('destination', 'San Diego')
    ->first();
if ($flight) {
    $flight->update(['price' => 99, 'discounted' => 1]);
} else {
    $flight = Flight::create([
        'departure' => 'Oakland',
        'destination' => 'San Diego',
        'price' => 99,
        'discounted' => 1
    ]);
}
// Do it in ONE sentence
$flight = Flight::updateOrCreate(
    ['departure' => 'Oakland', 'destination' => 'San Diego'],
    ['price' => 99, 'discounted' => 1]
);
```

<h3 id="Forget Cache on Save">Forget Cache on Save</h3>

源至 [@pratiksh404](https://github.com/pratiksh404)

If you have cache key like `posts` that gives collection, and you want to forget that cache key on new store or update, you can call static `saved` function on your model:

```php
class Post extends Model
{
    // Forget cache key on storing or updating
    public static function boot()
    {
        parent::boot();
        static::saved(function () {
           Cache::forget('posts');
        });
    }
}
```

<h3 id="Change Format Of Created_at and Updated_at">Change Format Of Created_at and Updated_at</h3>

源至 [@syofyanzuhad](https://github.com/syofyanzuhad)

To change the format of `created_at` you can add a method in your model like this:

Since Laravel 9:
```php
protected function createdAtFormatted(): Attribute
{
    return Attribute::make(
        get: fn ($value, $attributes) => $attributes['created_at']->format('H:i d, M Y'),
    );
}
```

Laravel 8 and below:
```php
public function getCreatedAtFormattedAttribute()
{
   return $this->created_at->format('H:i d, M Y');
}
```

So you can use it `$entry->created_at_formatted` when it's needed.
It will return the `created_at` attribute like this: `04:19 23, Aug 2020`.

And also for changing format of `updated_at` attribute, you can add this method :

Since Laravel 9:
```php
protected function updatedAtFormatted(): Attribute
{
    return Attribute::make(
        get: fn ($value, $attributes) => $attributes['updated_at']->format('H:i d, M Y'),
    );
}
```

Laravel 8 and below:
```php
public function getUpdatedAtFormattedAttribute()
{
   return $this->updated_at->format('H:i d, M Y');
}
```

So you can use it `$entry->updated_at_formatted` when it's needed.
It will return the `updated_at` attribute like this: `04:19 23, Aug 2020`.

<h3 id="Storing Array Type into JSON">Storing Array Type into JSON</h3>

源至 [@pratiksh404](https://github.com/pratiksh404)

If you have input field which takes an array and you have to store it as a JSON, you can use `$casts` property in your model. Here `images` is a JSON attribute.

```php
protected $casts = [
    'images' => 'array',
];
```

So you can store it as a JSON, but when retrieved from DB, it can be used as an array.

<h3 id="Make a Copy of the Model">Make a Copy of the Model</h3>

If you have two very similar Models (like shipping address and billing address) and you need to make a copy of one to another, you can use `replicate()` method and change some properties after that.

Example from the [official docs](https://laravel.com/docs/8.x/eloquent#replicating-models):

```php
$shipping = Address::create([
    'type' => 'shipping',
    'line_1' => '123 Example Street',
    'city' => 'Victorville',
    'state' => 'CA',
    'postcode' => '90001',
]);

$billing = $shipping->replicate()->fill([
    'type' => 'billing'
]);

$billing->save();
```

<h3 id="Reduce Memory">Reduce Memory</h3>

Sometimes we need to load a huge amount of data into memory. For example:

```php
$orders = Order::all();
```

But this can be slow if we have really huge data, because Laravel prepares objects of the Model class.
In such cases, Laravel has a handy function `toBase()`

```php
$orders = Order::toBase()->get();
//$orders will contain `Illuminate\Support\Collection` with objects `StdClass`.
```

By calling this method, it will fetch the data from the database, but it will not prepare the Model class.
Keep in mind it is often a good idea to pass an array of fields to the get method, preventing all fields to be fetched from the database.

<h3 id="Force query without $fillable/$guarded">Force query without $fillable/$guarded</h3>

If you create a Laravel boilerplate as a "starter" for other devs, and you're not in control of what THEY would later fill in Model's $fillable/$guarded, you may use forceFill()

```php
$team->update(['name' => $request->name])
```

What if "name" is not in Team model's `$fillable`? Or what if there's no `$fillable/$guarded` at all?

```php
$team->forceFill(['name' => $request->name])
```

This will "ignore" the `$fillable` for that one query and will execute no matter what.

<h3 id="3-level structure of parent-children">3-level structure of parent-children</h3>

If you have a 3-level structure of parent-children, like categories in an e-shop, and you want to show the number of products on the third level, you can use `with('yyy.yyy')` and then add `withCount()` as a condition

```php
class HomeController extend Controller
{
    public function index()
    {
        $categories = Category::query()
            ->whereNull('category_id')
            ->with(['subcategories.subcategories' => function($query) {
                $query->withCount('products');
            }])->get();
    }
}
```

```php
class Category extends Model
{
    public function subcategories()
    {
        return $this->hasMany(Category::class);
    }

    public function products()
    {
        return $this->hasMany(Product::class);
    }
}
```

```php
<ul>
    @foreach($categories as $category)
        <li>
            {{ $category->name }}
            @if ($category->subcategories)
                <ul>
                @foreach($category->subcategories as $subcategory)
                    <li>
                        {{ $subcategory->name }}
                        @if ($subcategory->subcategories)
                            <ul>
                                @foreach ($subcategory->subcategories as $subcategory)
                                    <li>{{ $subcategory->name }} ({{ $subcategory->product_count }})</li>
                                @endforeach
                            </ul>
                        @endif
                    </li>
                @endforeach
                </ul>
            @endif
        </li>
    @endforeach
</ul>
```

<h3 id="Perform any action on failure">Perform any action on failure</h3>

When looking for a record, you may want to perform some actions if it's not found.
In addition to `->firstOrFail()` which just throws 404, you can perform any action on failure, just do `->firstOr(function() { ... })`

```php
$model = Flight::where('legs', '>', 3)->firstOr(function () {
    // ...
})
```

<h3 id="Check if record exists or show 404">Check if record exists or show 404</h3>

Don't use find() and then check if the record exists. Use findOrFail().

```php
$product = Product::find($id);
if (!$product) {
    abort(404);
}
$product->update($productDataArray);
```

Shorter way

```php
$product = Product::findOrFail($id); // shows 404 if not found
$product->update($productDataArray);
```

<h3 id="Abort if condition failed">Abort if condition failed</h3>

`abort_if()` can be used as shorter way to check condition and throw an error page.

```php
$product = Product::findOrFail($id);
if($product->user_id != auth()->user()->id){
    abort(403);
}
```

Shorter way

```php
/* abort_if(CONDITION, ERROR_CODE) */
$product = Product::findOrFail($id);
abort_if ($product->user_id != auth()->user()->id, 403)
```

<h3 id="Fill a column automatically while you persist data to the database">Fill a column automatically while you persist data to the database</h3>

If you want to fill a column automatically while you persist data to the database (e.g: slug) use Model Observer instead of hard code it every time

```php
use Illuminate\Support\Str;

class Article extends Model
{
    ...
    protected static function boot()
    {
        parent:boot();

        static::saving(function ($model) {
            $model->slug = Str::slug($model->title);
        });
    }
}
```

源至 [@sky_0xs](https://twitter.com/sky_0xs/status/1432390722280427521)

<h3 id="Extra information about the query">Extra information about the query</h3>

You can call the `explain()` method on queries to know extra information about the query.

```php
Book::where('name', 'Ruskin Bond')->explain()->dd();
```

```php
Illuminate\Support\Collection {#5344
    all: [
        {#15407
            +"id": 1,
            +"select_type": "SIMPLE",
            +"table": "books",
            +"partitions": null,
            +"type": "ALL",
            +"possible_keys": null,
            +"key": null,
            +"key_len": null,
            +"ref": null,
            +"rows": 9,
            +"filtered": 11.11111164093,
            +"Extra": "Using where",
        },
    ],
}
```

源至 [@amit_merchant](https://twitter.com/amit_merchant/status/1432277631320223744)

<h3 id="Using the doesntExist() method in Laravel">Using the doesntExist() method in Laravel</h3>

```php
// This works
if ( 0 === $model->where('status', 'pending')->count() ) {
}

// But since I don't care about the count, just that there isn't one
// Laravel's exists() method is cleaner.
if ( ! $model->where('status', 'pending')->exists() ) {
}

// But I find the ! in the statement above easily missed. The
// doesntExist() method makes this statement even clearer.
if ( $model->where('status', 'pending')->doesntExist() ) {
}
```

源至 [@ShawnHooper](https://twitter.com/ShawnHooper/status/1435686220542234626)

<h3 id="Trait that you want to add to a few Models to call their boot() method automatically">Trait that you want to add to a few Models to call their boot() method automatically</h3>

If you have a Trait that you want to add to a few Models to call their `boot()` method automatically, you can call Trait's method as boot[TraitName]

```php
class Transaction extends  Model
{
    use MultiTenantModelTrait;
}
```

```php
class Task extends  Model
{
    use MultiTenantModelTrait;
}
```

```php
trait MultiTenantModelTrait
{
    // This method's name is boot[TraitName]
    // It will be auto-called as boot() of Transaction/Task
    public static function bootMultiTenantModelTrait()
    {
        static::creating(function ($model) {
            if (!$isAdmin) {
                $isAdmin->created_by_id = auth()->id();
            }
        })
    }
}
```

<h3 id="There are two common ways of determining if a table is empty in Laravel">There are two common ways of determining if a table is empty in Laravel</h3>

There are two common ways of determining if a table is empty in Laravel. Calling `exists()` or `count()` directly on the model!

One returns a strict true/false boolean, the other returns an integer which you can use as a falsy in conditionals.

```php
public function index()
{
    if (\App\Models\User::exists()) {
        // returns boolean true or false if the table has any saved rows
    }

    if (\App\Models\User::count()) {
        // returns the count of rows in the table
    }
}
```

源至 [@aschmelyun](https://twitter.com/aschmelyun/status/1440641525998764041)

<h3 id="How to prevent “property of non-object” error">How to prevent “property of non-object” error</h3>

```php
// BelongsTo Default Models
// Let's say you have Post belonging to Author and then Blade code:
$post->author->name;

// Of course, you can prevent it like this:
$post->author->name ?? ''
// or
@$post->author->name

// But you can do it on Eloquent relationship level:
// this relation will return an empty App\Author model if no author is attached to the post
public function author() {
    return $this->belongsTo(Author::class)->withDefault();
}
// or
public function author() {
    return $this->belongsTo(Author::class)->withDefault([
        'name' => 'Guest Author'
    ]);
}
```

源至 [@coderahuljat](https://twitter.com/coderahuljat/status/1440556610837876741)

<h3 id="Get original attributes after mutating an Eloquent record">Get original attributes after mutating an Eloquent record</h3>

Get original attributes after mutating an Eloquent record you can get the original attributes by calling getOriginal()

```php
$user = App\User::first();
$user->name; // John
$user->name = "Peter"; // Peter
$user->getOriginal('name'); // John
$user->getOriginal(); // Original $user record
```

源至 [@devThaer](https://twitter.com/devThaer/status/1442133797223403521)

<h3 id="A simple way to seed a database">A simple way to seed a database</h3>

A simple way to seed a database in Laravel with a .sql dump file

```php
DB::unprepared(
    file_get_contents(__DIR__ . './dump.sql')
);
```

源至 [@w3Nicolas](https://twitter.com/w3Nicolas/status/1447902369388249091)

<h3 id="The crossJoinSub method of the query constructor">The crossJoinSub method of the query constructor</h3>

Using the CROSS JOIN subquery

```php
use Illuminate\Support\Facades\DB;

$totalQuery = DB::table('orders')->selectRaw('SUM(price) as total');

DB::table('orders')
    ->select('*')
    ->crossJoinSub($totalQuery, 'overall')
    ->selectRaw('(price / overall.total) * 100 AS percent_of_total')
    ->get();
```

源至 [@PascalBaljet](https://twitter.com/pascalbaljet)

<h3 id="Belongs to Many Pivot table naming">Belongs to Many Pivot table naming</h3>

To determine the table name of the relationship's intermediate table, Eloquent will join the two related model names in alphabetical order.

This would mean a join between `Post` and `Tag` could be added like this:

```php
class Post extends Model
{
    public $table = 'posts';

    public function tags()
    {
        return $this->belongsToMany(Tag::class);
    }
}
```

However, you are free to override this convention, and you would need to specify the join table in the second argument.

```php
class Post extends Model
{
    public $table = 'posts';

    public function tags()
    {
        return $this->belongsToMany(Tag::class, 'posts_tags');
    }
}
```

If you wish to be explicit about the primary keys you can also supply these as third and fourth arguments.

```php
class Post extends Model
{
    public $table = 'posts';

    public function tags()
    {
        return $this->belongsToMany(Tag::class, 'post_tag', 'post_id', 'tag_id');
    }
}
```

源至 [@iammikek](https://twitter.com/iammikek)

<h3 id="Order by Pivot Fields">Order by Pivot Fields</h3>

`BelongsToMany::orderByPivot()` allows you to directly sort the results of a BelongsToMany relationship query.

```php
class Tag extends Model
{
    public $table = 'tags';
}

class Post extends Model
{
    public $table = 'posts';

    public function tags()
    {
        return $this->belongsToMany(Tag::class, 'post_tag', 'post_id', 'tag_id')
            ->using(PostTagPivot::class)
            ->withTimestamps()
            ->withPivot('flag');
    }
}

class PostTagPivot extends Pivot
{
    protected $table = 'post_tag';
}

// Somewhere in the Controller
public function getPostTags($id)
{
    return Post::findOrFail($id)->tags()->orderByPivot('flag', 'desc')->get();
}
```

源至 [@PascalBaljet](https://twitter.com/pascalbaljet)

<h3 id="Find a single record from a database">Find a single record from a database</h3>

The `sole()` method will return only one record that matches the criteria. If no such entry is found, then a `NoRecordsFoundException` will be thrown. If multiple records are found, then a `MultipleRecordsFoundException` will be thrown.

```php
DB::table('products')->where('ref', '#123')->sole();
```

源至 [@PascalBaljet](https://twitter.com/pascalbaljet)

<h3 id="Automatic records chunking">Automatic records chunking</h3>

Similar to `each()` method, but easier to use. Automatically splits the result into parts (chunks).

```php
return User::orderBy('name')->chunkMap(fn ($user) => [
    'id' => $user->id,
    'name' => $user->name,
]), 25);
```

源至 [@PascalBaljet](https://twitter.com/pascalbaljet)

<h3 id="Updating the model without dispatching events">Updating the model without dispatching events</h3>

Sometimes you need to update the model without sending any events. We can now do this with the `updateQuietly()` method, which under the hood uses the `saveQuietly()` method.

```php
$flight->updateQuietly(['departed' => false]);
```

源至 [@PascalBaljet](https://twitter.com/pascalbaljet)

<h3 id="Periodic cleaning of models from obsolete records">Periodic cleaning of models from obsolete records</h3>

To periodically clean models of obsolete records. With this trait, Laravel will do this automatically, only you need to adjust the frequency of the `model:prune` command in the Kernel class.

```php
use Illuminate\Database\Eloquent\Model;
use Illuminate\Database\Eloquent\Prunable;
class Flight extends Model
{
    use Prunable;
    /**
     * Get the prunable model query.
     *
     * @return \Illuminate\Database\Eloquent\Builder
     */
    public function prunable()
    {
        return static::where('created_at', '<=', now()->subMonth());
    }
}
```

Also, in the pruning method, you can set the actions that must be performed before deleting the model:

```php
protected function pruning()
{
    // Removing additional resources,
    // associated with the model. For example, files.

    Storage::disk('s3')->delete($this->filename);
}
```

源至 [@PascalBaljet](https://twitter.com/pascalbaljet)

<h3 id="Immutable dates and casting to them">Immutable dates and casting to them</h3>

Laravel 8.53 introduces the `immutable_date` and `immutable_datetime` castes that convert dates to `Immutable`.

Cast to CarbonImmutable instead of a regular Carbon instance.

```php
class User extends Model
{
    public $casts = [
        'date_field'     => 'immutable_date',
        'datetime_field' => 'immutable_datetime',
    ];
}
```

源至 [@PascalBaljet](https://twitter.com/pascalbaljet)

<h3 id="The findOrFail method also accepts a list of ids">The findOrFail method also accepts a list of ids</h3>

The findOrFail method also accepts a list of ids. If any of these ids are not found, then it "fails".

Nice if you need to retrieve a specific set of models and don't want to have to check that the count you got was the count you expected

```php
User::create(['id' => 1]);
User::create(['id' => 2]);
User::create(['id' => 3]);

// Retrieves the user...
$user = User::findOrFail(1);

// Throws a 404 because the user doesn't exist...
User::findOrFail(99);

// Retrieves all 3 users...
$users = User::findOrFail([1, 2, 3]);

// Throws because it is unable to find *all* of the users
User::findOrFail([1, 2, 3, 99]);
```

源至 [@timacdonald87](https://twitter.com/timacdonald87/status/1457499557684604930)

<h3 id="Prunable trait to automatically remove models from your database">Prunable trait to automatically remove models from your database</h3>

New in Laravel 8.50: You can use the Prunable trait to automatically remove models from your database. For example, you can permanently remove soft deleted models after a few days.

```php
class File extends Model
{
    use SoftDeletes;

    // Add Prunable trait
    use Prunable;

    public function prunable()
    {
        // Files matching this query will be pruned
        return static::query()->where('deleted_at', '<=', now()->subDays(14));
    }

    protected function pruning()
    {
        // Remove the file from s3 before deleting the model
        Storage::disk('s3')->delete($this->filename);
    }
}

// Add PruneCommand to your schedule (app/Console/Kernel.php)
$schedule->command(PruneCommand::class)->daily();
```

Tip by [@Philo01](https://twitter.com/Philo01/status/1457626443782008834)

<h3 id="withAggregate method">withAggregate method</h3>

Under the hood, the withAvg/withCount/withSum and other methods in Eloquent use the 'withAggregate' method. You can use this method to add a subselect based on a relationship

```php
// Eloquent Model
class Post extends Model
{
    public function user()
    {
        return $this->belongsTo(User::class);
    }
}

// Instead of eager loading all users...
$posts = Post::with('user')->get();

// You can add a subselect to only retrieve the user's name...
$posts = Post::withAggregate('user', 'name')->get();

// This will add a 'user_name' attribute to the Post instance:
$posts->first()->user_name;
```

源至 [@pascalbaljet](https://twitter.com/pascalbaljet/status/1457702666352594947)

<h3 id="Date convention">Date convention</h3>

Using the `something_at` convention instead of just a boolean in Laravel models gives you visibility into when a flag was changed – like when a product went live.

```php
// Migration
Schema::table('products', function (Blueprint $table) {
    $table->datetime('live_at')->nullable();
});

// In your model
public function live()
{
    return !is_null($this->live_at);
}

// Also in your model
protected $dates = [
    'live_at'
];
```

源至 [@alexjgarrett](https://twitter.com/alexjgarrett/status/1459174062132019212)

<h3 id="Eloquent multiple upserts">Eloquent multiple upserts</h3>

The upsert() method will insert or update multiple records.

- First array: the values to insert or update
- Second: unique identifier columns used in the select statement
- Third: columns that you want to update if the record exists

```php
Flight::upsert([
    ['departure' => 'Oakland', 'destination' => 'San Diego', 'price' => 99],
    ['departure' => 'Chicago', 'destination' => 'New York', 'price' => 150],
], ['departure', 'destination'], ['price']);
```

源至 [@mmartin_joo](https://twitter.com/mmartin_joo/status/1461591319516647426)

<h3 id="Retrieve the Query Builder after filtering the results">Retrieve the Query Builder after filtering the results</h3>

To retrieve the Query Builder after filtering the results: you can use `->toQuery()`.

The method internally use the first model of the collection and a `whereKey` comparison on the Collection models.

```php
// Retrieve all logged_in users
$loggedInUsers = User::where('logged_in', true)->get();

// Filter them using a Collection method or php filtering
$nthUsers = $loggedInUsers->nth(3);

// You can't do this on the collection
$nthUsers->update(/* ... */);

// But you can retrieve the Builder using ->toQuery()
if ($nthUsers->isNotEmpty()) {
    $nthUsers->toQuery()->update(/* ... */);
}
```

源至 [@RBilloir](https://twitter.com/RBilloir/status/1462529494917566465)

<h3 id="Custom casts">Custom casts</h3>

You can create custom casts to have Laravel automatically format your Eloquent model data. Here's an example that capitalises a user's name when it is retrieved or changed.

```php
class CapitalizeWordsCast implements CastsAttributes
{
    public function get($model, string $key, $value, array $attributes)
    {
        return ucwords($value);
    }

    public function set($model, string $key, $value, array $attributes)
    {
        return ucwords($value);
    }
}

class User extends Model
{
    protected $casts = [
        'name'  => CapitalizeWordsCast::class,
        'email' => 'string',
    ];
}
```

源至 [@mattkingshott](https://twitter.com/mattkingshott/status/1462828232206659586)

<h3 id="Order based on a related model's average or count">Order based on a related model's average or count</h3>

Did you ever need to order based on a related model's average or count?

It's easy with Eloquent!

```php
public function bestBooks()
{
    Book::query()
        ->withAvg('ratings as average_rating', 'rating')
        ->orderByDesc('average_rating');
}
```

源至 [@mmartin_joo](https://twitter.com/mmartin_joo/status/1466769691385335815)

<h3 id="Return transactions result">Return transactions result</h3>

If you have a DB transaction and want to return its result, there are at least two ways, see the example

```php
// 1. You can pass the parameter by reference
$invoice = NULL;
DB::transaction(function () use (&$invoice) {
    $invoice = Invoice::create(...);
    $invoice->items()->attach(...);
})

// 2. Or shorter: just return trasaction result
$invoice = DB::transaction(function () {
    $invoice = Invoice::create(...);
    $invoice->items()->attach(...);

    return $invoice;
});
```

<h3 id="Remove several global scopes from query">Remove several global scopes from query</h3>

When using Eloquent Global Scopes, you not only can use MULTIPLE scopes, but also remove certain scopes when you don't need them, by providing the array to `withoutGlobalScopes()`

[Link to docs](https://laravel.com/docs/8.x/eloquent#removing-global-scopes)

```php
// Remove all of the global scopes...
User::withoutGlobalScopes()->get();

// Remove some of the global scopes...
User::withoutGlobalScopes([
    FirstScope::class, SecondScope::class
])->get();
```

<h3 id="Order JSON column attribute">Order JSON column attribute</h3>

With Eloquent you can order results by a JSON column attribute

```php
// JSON column example:
// bikes.settings = {"is_retired": false}
$bikes = Bike::where('athlete_id', $this->athleteId)
        ->orderBy('name')
        ->orderByDesc('settings->is_retired')
        ->get();
```

源至 [@brbcoding](https://twitter.com/brbcoding/status/1473353537983856643)

<h3 id="Get single column's value from the first result">Get single column's value from the first result</h3>

You can use `value()` method to get single column's value from the first result of a query

```php
// Instead of
Integration::where('name', 'foo')->first()->active;

// You can use
Integration::where('name', 'foo')->value('active');

// or this to throw an exception if no records found
Integration::where('name', 'foo')->valueOrFail('active')';
```

源至 [@justsanjit](https://twitter.com/justsanjit/status/1475572530215796744)

<h3 id="Check if altered value changed key">Check if altered value changed key</h3>

Ever wanted to know if the changes you've made to a model have altered the value for a key? No problem, simply reach for originalIsEquivalent.

```php
$user = User::first(); // ['name' => "John']

$user->name = 'John';

$user->originalIsEquivalent('name'); // true

$user->name = 'David'; // Set directly
$user->fill(['name' => 'David']); // Or set via fill

$user->originalIsEquivalent('name'); // false
```

源至 [@mattkingshott](https://twitter.com/mattkingshott/status/1475843987181379599)

<h3 id="New way to define accessor and mutator">New way to define accessor and mutator</h3>

New way to define attribute accessors and mutators in Laravel 8.77:

```php
// Before, two-method approach
public function setTitleAttribute($value)
{
    $this->attributes['title'] = strtolower($value);
}
public function getTitleAttribute($value)
{
    return strtoupper($value);
}

// New approach
protected function title(): Attribute
{
    return new Attribute(
        get: fn ($value) => strtoupper($value),
        set: fn ($value) => strtolower($value),
    );
}
```

源至 [@Teacoders](https://twitter.com/Teacoders/status/1473697808456851466)

<h3 id="Another way to do accessors and mutators">Another way to do accessors and mutators</h3>

In case you are going to use the same accessors and mutators in many models , You can use custom casts instead.

Just create a `class` that implements `CastsAttributes` interface. The class should have two methods, the first is `get` to specify how models should be retrieved from the database and the second is `set` to specify how the value will be stored in the database.

```php
<?php

namespace App\Casts;

use Carbon\Carbon;
use Illuminate\Contracts\Database\Eloquent\CastsAttributes;

class TimestampsCast implements CastsAttributes
{
    public function get($model, string $key, $value, array $attributes)
    {
        return Carbon::parse($value)->diffForHumans();
    }

    public function set($model, string $key, $value, array $attributes)
    {
        return Carbon::parse($value)->format('Y-m-d h:i:s');
    }
}

```

Then you can implement the cast in the model class.

```php
<?php

namespace App\Models;

use Illuminate\Foundation\Auth\User as Authenticatable;
use App\Casts\TimestampsCast;
use Carbon\Carbon;


class User extends Authenticatable
{

    /**
     * The attributes that should be cast.
     *
     * @var array
     */
    protected $casts = [
        'updated_at' => TimestampsCast::class,
        'created_at' => TimestampsCast::class,
    ];
}

```

源至 [@AhmedRezk](https://github.com/AhmedRezk59)

<h3 id="When searching for the first record, you can perform some actions">When searching for the first record, you can perform some actions</h3>

When searching for the first record, you want to perform some actions, when you don't find it. `firstOrFail()` throws a 404 Exception.

You can use `firstOr(function() {})` instead. Laravel got your covered

```php
$book = Book::whereCount('authors')
            ->orderBy('authors_count', 'DESC')
            ->having('modules_count', '>', 10)
            ->firstOr(function() {
                // The Sky is the Limit ...

                // You can perform any action here
            });
```

源至 [@bhaidar](https://twitter.com/bhaidar/status/1487757487566639113/)

<h3 id="Directly convert created_at date to human readable format">Directly convert created_at date to human readable format</h3>

Did you know you can directly convert created_at date to human readable format like 1 minute ago, 1 month ago using diffForHumans() function. Laravel eloquent by default enables Carbon instance on created_at field.

```php
$post = Post::whereId($id)->first();
$result = $post->created_at->diffForHumans();

/* OUTPUT */
// 1 Minutes ago, 2 Week ago etc..as per created time
```

源至 [@vishal\_\_2931](https://twitter.com/vishal__2931/status/1488369014980038662)

<h3 id="Ordering by an Eloquent Accessor">Ordering by an Eloquent Accessor</h3>

Ordering by an Eloquent Accessor! Yes, that's doable. Instead of ordering by the accessor on the DB level, we order by the accessor on the returned Collection.

```php
class User extends Model
{
    // ...
    protected $appends = ['full_name'];

    // Since Laravel 9
    protected function full_name(): Attribute
    {
        return Attribute::make(
            get: fn ($value, $attributes) => $attributes['first_name'] . ' ' . $attributes['last_name'];),
        );
    }

    // Laravel 8 and lower
    public function getFullNameAttribute()
    {
        return $this->attribute['first_name'] . ' ' . $this->attributes['last_name'];
    }
    // ..
}
```

```php
class UserController extends Controller
{
    // ..
    public function index()
    {
        $users = User::all();

        // order by full_name desc
        $users->sortByDesc('full_name');

        // or

        // order by full_name asc
        $users->sortBy('full_name');

        // ..
    }
    // ..
}
```

`sortByDesc` and `sortBy` are methods on the Collection

源至 [@bhaidar](https://twitter.com/bhaidar/status/1490671693618053123)

<h3 id="Check for specific model was created or found">Check for specific model was created or found</h3>

If you want to check for specific model was created or found, use `wasRecentlyCreated` model attribute.

```php
$user = User::create([
    'name' => 'Oussama',
]);

// return boolean
return $user->wasRecentlyCreated;

// true for recently created
// false for found (already on you db)
```

源至 [@sky_0xs](https://twitter.com/sky_0xs/status/1491141790015320064)

<h3 id="Laravel Scout with database driver">Laravel Scout with database driver</h3>

With laravel v9 you can use Laravel Scout (Search) with database driver. No more where likes!

```php
$companies = Company::search(request()->get('search'))->paginate(15);
```

源至 [@magarrent](https://twitter.com/magarrent/status/1493221422675767302)

<h3 id="Make use of the value method on the query builder">Make use of the value method on the query builder</h3>

Make use of the `value` method on the query builder to execute a more efficient query when you only need to retrieve a single column.

```php
// Before (fetches all columns on the row)
Statistic::where('user_id', 4)->first()->post_count;

// After (fetches only `post_count`)
Statistic::where('user_id', 4)->value('post_count');
```

源至 [@mattkingshott](https://twitter.com/mattkingshott/status/1493583444244410375)

<h3 id="Pass array to where method">Pass array to where method</h3>

Laravel you can pass an array to the where method.

```php
// Instead of this
JobPost::where('company', 'laravel')
        ->where('job_type', 'full time')
        ->get();

// You can pass an array
JobPost::where(['company' => 'laravel',
                'job_type' => 'full time'])
        ->get();
```

源至 [@cosmeescobedo](https://twitter.com/cosmeescobedo/status/1495626752282234881)

<h3 id="Return the primary keys from models collection">Return the primary keys from models collection</h3>

Did you know `modelsKeys()` eloquent collection method? It returns the primary keys from models collection.

```php
$users = User::active()->limit(3)->get();

$users->modelsKeys(); // [1, 2, 3]
```

源至 [@iamharis010](https://twitter.com/iamharis010/status/1495816807910891520)

<h3 id="Force Laravel to use eager loading">Force Laravel to use eager loading</h3>

If you want to prevent a lazy loading in your app, you only need to add following line to the `boot()` method in your `AppServiceProvider`

```php
Model::preventLazyLoading();
```

But, if you want to enable this feature only on your local development you can change above code on that:

```php
Model::preventLazyLoading(!app()->isProduction());
```

源至 [@CatS0up](https://github.com/CatS0up)

<h3 id="Make all your models mass assignable">Make all your models mass assignable</h3>

It is not a recommended approach for security reasons, but it is possible.

When you want do this, you don't need to set an empty `$guarded` array for every model, like this:

```php
protected $guarded = [];
```

You can do it from a single place, just add following line to the `boot()` method in your `AppServiceProvider`:

```php
Model::unguard();
```

Now, all your models are mass assignable.

源至 [@CatS0up](https://github.com/CatS0up)

<h3 id="Hiding columns in select all statements">Hiding columns in select all statements</h3>

If you use Laravel v8.78 and MySQL 8.0.23 and onwards, you can define choosen columns as "invisible". Columns which are define as `invisible` will be hidden from the `select *` statements.

However, to do so, we must use a `invisible()` method in the migration, something like that:

```php
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

Schema::table('table', function (Blueprint $table) {
    $table->string('secret')->nullable()->invisible();
});
```

That's it! This will make chosen column hidden from `select *` statement.

源至 [@CatS0up](https://github.com/CatS0up)

<h3 id="JSON Where Clauses">JSON Where Clauses</h3>

Laravel offers helpers to query JSON columns for databases that support them.

Currently, MySQL 5.7+, PostgreSQL, SQL Server 2016, and SQLite 3.9.0 (using the JSON1 extension)

```php
// To query a json column you can use the -> operator
$users = User::query()
            ->where('preferences->dining->meal', 'salad')
            ->get();
// You can check if a JSON array contains a set of values
$users = User::query()
            ->whereJsonContains('options->languages', [
                'en', 'de'
               ])
            ->get();
// You can also query by the length a JSON array
$users = User::query()
            ->whereJsonLength('options->languages', '>', 1)
            ->get();
```

源至 [@cosmeescobedo](https://twitter.com/cosmeescobedo/status/1509663119311663124)

<h3 id="Get all the column names for a table">Get all the column names for a table</h3>

```php
DB::getSchemaBuilder()->getColumnListing('users');
/*
returns [
    'id',
    'name',
    'email',
    'email_verified_at',
    'password',
    'remember_token',
    'created_at',
    'updated_at',
];
*/
```

源至 [@aaronlumsden](https://twitter.com/aaronlumsden/status/1511014229737881605)

<h3 id="Compare the values of two columns">Compare the values of two columns</h3>

You can use `whereColumn` method to compare the values of two columns.

```php
return Task::whereColumn('created_at', 'updated_at')->get();
// pass a comparison operator
return Task::whereColumn('created_at', '>', 'updated_at')->get();
```

源至 [@iamgurmandeep](https://twitter.com/iamgurmandeep/status/1511673260353548294)

<h3 id="Accessor Caching">Accessor Caching</h3>

As of Laravel 9.6, if you have a computationally intensive accessor, you can use the shouldCache method.

```php
public function hash(): Attribute
{
    return Attribute::make(
        get: fn($value) => bcrypt(gzuncompress($value)),
    )->shouldCache();
}
```

源至 [@cosmeescobedo](https://twitter.com/cosmeescobedo/status/1514304409563402244)

<h3 id="New scalar() method">New scalar() method</h3>

In Laravel 9.8.0, the `scalar()` method was added that allows you to retrieve the first column of the first row from the query result.

```php
// Before
DB::selectOne("SELECT COUNT(CASE WHEN food = 'burger' THEN 1 END) AS burgers FROM menu_items;")->burgers
// Now
DB::scalar("SELECT COUNT(CASE WHEN food = 'burger' THEN 1 END) FROM menu_items;")
```

源至 [@justsanjit](https://twitter.com/justsanjit/status/1514550185837408265)

<h3 id="Select specific columns">Select specific columns</h3>

To select specific columns on a model you can use the select method -- or you can pass an array directly to the get method!

```php
// Select specified columns from all employees
$employees = Employee::select(['name', 'title', 'email'])->get();
// Select specified columns from all employees
$employees = Employee::get(['name', 'title', 'email']);
```

源至 [@ecrmnn](https://twitter.com/ecrmnn/status/1516087672351203332)

<h3 id="Chain conditional clauses to the query without writing if-else statements">Chain conditional clauses to the query without writing if-else statements</h3>

The "when" helper in the query builder is🔥

You can chain conditional clauses to the query without writing if-else statements.

Makes your query very clear:

```php
class RatingSorter extends Sorter
{
    function execute(Builder $query)
    {
        $query
            ->selectRaw('AVG(product_ratings.rating) AS avg_rating')
            ->join('product_ratings', 'products.id', '=', 'product_ratings.product_id')
            ->groupBy('products.id')
            ->when(
                $this->direction === SortDirections::Desc,
                fn () => $query->orderByDesc('avg_rating')
                fn () => $query->orderBy('avg_rating'),
            );

        return $query;
    }
}
```

源至 [@mmartin_joo](https://twitter.com/mmartin_joo/status/1521461317940350976)

<h3 id="Override Connection Attribute in Models">Override Connection Attribute in Models</h3>

Overriding the database connection attribute for individual models in Laravel can be a powerful technique. Here are a few use cases where you might find it especially handy:

#### 1. Multiple Database Connections

If your application uses multiple database connections (e.g., MySQL, PostgreSQL, or different instances of the same database), you may want to specify which connection should be used for a particular model. By overriding the `$connection` property, you can easily manage these connections and ensure your models are interacting with the appropriate databases.

#### 2. Data Sharding

In cases where you're using data sharding to distribute your data across multiple databases, you might have different models that map to different shards. Overriding the connection attribute in each model allows you to define which shard should be used without affecting other models or the default connection.

#### 3. Third-Party Integration

When integrating with third-party services that provide their own database, you may need to use a specific connection for a model representing data from that service. Overriding the connection attribute in that model will ensure it connects to the right database while keeping your application's default settings intact.

#### 4. Multi-Tenancy Applications

In a multi-tenant application, you may have separate databases for each tenant. By overriding the `$connection` property dynamically in your models, you can easily switch between tenant databases based on the current user, ensuring data isolation and proper resource management.

To override the connection attribute in a model, define the `$connection` property within the class:

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Model;

class CustomModel extends Model
{
    protected $connection = 'your_custom_connection';
}
```
<h3 id="Using Column Names in Where Clauses (Dynamic Where Clauses)">Using Column Names in Where Clauses (Dynamic Where Clauses)</h3>

You can use column names in where clause to make dynamic where clauses. In the following example, we use ```whereName('John')``` instead of ```where('name', 'John')```.

```php
<?php

namespace App\Http\Controllers;

use App\Models\User;

class UserController extends Controller
{
    public function example()
    {
        return User::whereName('John')->get();
    }
}
```

源至 [@MNurullahSaglam](https://twitter.com/MNurullahSaglam/status/1699763337586749585)

<h3 id="Using firstOrCreate()">Using firstOrCreate()</h3>

You can use firstOrCreate() to find the first record matching the attributes or create it if it doesn't exist.

#### Example Scenario

Assume that you are importing a CSV file and you want to create a category if it doesn't exist.

```php
<?php

namespace App\Http\Controllers;

use App\Models\Category;
use Illuminate\Http\Request;

class CategoryController extends Controller
{
    public function example(Request $request)
    {
        // instead of
        $category = Category::where('name', $request->name)->first();
        
        if (!$category) {
            $category = Category::create([
                'name' => $request->name,
                'slug' => Str::slug($request->name),
            ]);
        }
        
        // you can use
        $category = Category::firstOrCreate([
            'name' => $request->name,
        ], [
            'slug' => Str::slug($request->name),
        ]);

        return $category;
    }
}
```

源至 [@MNurullahSaglam](https://twitter.com/MNurullahSaglam/status/1699773783748366478)
