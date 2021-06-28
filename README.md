
## Day1

**（１）モバイルアプリを開発する上で、設計上留意すべき点はどこになるか、サーバサイドやフロントエンドとの違いの観点から説明してください。**
（１） Answer:
```
When designing mobile application, it's little different than web app design. Because in the case of web, the pages are not aware of the whole system. Only some information is inherited between pages by cookies and URL parameters. But in the case of Mobile app, Apps need to know all the data, the flow of information, and the actions of users within their own processes. Server side and Mobile app side is different in design sense. Server side is responsible for storing and providing data in the means of API etc. App side is responsible for fetching, modifying, storing data communicating server, and also serving these data to users through UI. So, these points should be kep in mind while designing Mobile application. Also, we have to consider app lifecycle when designing mobile applications. As there are various lifecycle in app, like onStart, onCreate, onPause, onResume, onRestart, onStop etc. in both Activity and Fragments, we have to consider these lifecycle things. And as app can be opened in various ways, like tapping app icon in home screen of mobile, tapping push notifications, from Deep link etc. these entry point should be considered and processing should be handled accordingly. And as there are foreground and background concept in app, due to lifecycle, these things should be kept in mind for UI updating, data updating etc.
```

**（２）Activityへの過度な依存や類似/同一コードの重複を避けるため、コード設計上どのような対策をとることが望ましいか、プレゼンテーション層（View）と処理・ビジネスロジック（Controller）それぞれの観点から説明してください。**
（２） Answer:
```
In code designing, decaoupling of classes shoulld be considered. Architecture design should be robust so that, classes doesn't get much dependent. Separating classes according to single responsibility will make the app more maintenable, testable, reusable. Business logic, presentation logic, data process logic, all these thing should be separated. Presentation layer (View) should be responsible for displaying data and taking user actions. And if there is any possibility of repeating same view more than one place, then Custom view or fragment should be used. Common business logics that will be repeated in several clasees, that should not be in activity or fragment, they can reside in Service/Manager/Helper utils kind classes. Android framework related code will reside in Controllers (Activity or Fragment class).
```

## Day2

**（３）以下のそれぞれのレイアウトを実現するレイアウトXMLを書いてください。**

①　横一列に３つのViewを並べるレイアウト。その際、３つのViewの幅(width)の比率が、`3:2:1`になるようにすること。また、それぞれ左右に`10dp`ずつマージンを取り、高さはすべて`100dp`とすること。

（３）① Answer
```
<LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal">

        <View
            android:layout_width="0dp"
            android:layout_weight="3"
            android:layout_height="100dp"
            android:layout_marginStart="10dp"
            android:layout_marginEnd="5dp"
            android:background="@android:color/holo_red_dark"/>

        <View
            android:layout_width="0dp"
            android:layout_weight="2"
            android:layout_height="100dp"
            android:layout_marginStart="5dp"
            android:layout_marginEnd="5dp"
            android:background="@android:color/holo_green_dark"/>

        <View
            android:layout_width="0dp"
            android:layout_weight="1"
            android:layout_height="100dp"
            android:layout_marginStart="5dp"
            android:layout_marginEnd="10dp"
            android:background="@android:color/holo_blue_dark"/>
        
    </LinearLayout>
```

②　画面いっぱい（上下左右に`10dp`ずつマージン）にViewを置き、それに重ねる形で、width `100dp`、height `100dp`のViewを縦横中央にそろえたレイアウト。色はそれぞれ変えること。

（３）② Answer:
```
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <View
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:layout_margin="10dp"
        android:background="@android:color/holo_green_dark" />

    <View
        android:layout_width="100dp"
        android:layout_height="100dp"
        android:layout_gravity="center"
        android:background="@android:color/holo_red_dark" />

</FrameLayout>
```

③　親ビューの下部に横幅`100%`、高さは可変サイズのTextViewを配置し、その上部にwidth `100dp`、height `100dp`のViewを左右の中央にそろえたレイアウト。

（３）③ Answer
```
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    xmlns:app="http://schemas.android.com/apk/res-auto">

    <TextView
        android:id="@+id/textview"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="This is textView"
        android:gravity="center"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintBottom_toBottomOf="parent"/>
    
    <View
        android:layout_width="100dp"
        android:layout_height="100dp"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintBottom_toTopOf="@id/textview"
        android:background="@android:color/holo_green_light"/>

</androidx.constraintlayout.widget.ConstraintLayout>
```

**（４）以下のコードのTODO箇所を埋める形で、ActivityのContentViewの各Viewコンポーネントをプロパティアクセスで参照するコードを、①findViewByIdを使う方法、②ViewBindingを使う方法それぞれで書いてください。
その際、下記の要件に従うこと。**

> - レイアウトファイルの名前は、`main_activity.xml`とする

```kotlin
class MainActivity : Activity(), ViewBindable {
    override lateinit var binding: ViewBinding
    private val findViewByIdView: View?
        get() = findViewById<View>(R.id.view)
    private val viewBindingView: View?
        get() = (binding as? MainActivityBinding)?.view

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        binding = MainActivityBinding.inflate(layoutInflater)
        val view = binding.root
        setContentView(view)
        
        findViewByIdView?.setBackgroundColor(resources.getColor(R.color.red))
        viewBindingView?.setBackgroundColor(resources.getColor(R.color.blue))
    }
}
interface ViewBindable {
    var binding: ViewBinding
}
```

## Day3

**（５）以下のコードのTODO箇所を埋める形で、①Activityが新しい別のActivityを呼び出した際にデータ（Contentデータ）を渡し、②呼び出されたActivityが閉じられる際に古いActivityにデータ(Int型の数値)を渡すコードを書いてください。なお、コード中に使われるIDは、コード内にあるenumを使用すること。**

```kotlin
data class Content(
    var id: Int = 0,
    var name: String = ""
) : Parcelable {
    override fun writeToParcel(parcel: Parcel, flags: Int) {
        parcel.writeInt(id)
        parcel.writeString(name)
    }
    override fun describeContents(): Int {
        return 0
    }
    constructor(parcel: Parcel) : this(
        parcel.readInt() ?: 0,
        parcel.readString() ?: "")
    companion object CREATOR : Parcelable.Creator<Content> {
        override fun createFromParcel(parcel: Parcel): Content {
            return Content(parcel)
        }
        override fun newArray(size: Int): Array<Content?> {
            return arrayOfNulls(size)
        }
    }
}
enum class ActivityRequestCode(val code: Int) {
    TARGET(11111);
}
enum class ActivityResultCode(val code: Int) {
    TARGET(1111);
}
enum class ActivityExtraData(val key: String) {
    NUMBER("number"), CONTENT("content");
}
class MainActivity : Activity() {
    private val button: Button?
        get() = findViewById<Button>(R.id.button) as? Button
    
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        button?.setOnClickListener {
            val contentData = Content()
            val  intent = Intent(this, LoginActivity::class.java)
            intent.putExtra(ActivityExtraData.NUMBER.key, 1)
            intent.putExtra(ActivityExtraData.CONTENT.key, contentData)
            startActivityForResult(intent, ActivityRequestCode.TARGET.code)
        }
    }
    override fun onActivityResult(requestCode: Int, resultCode: Int, data: Intent?) {
        super.onActivityResult(requestCode, resultCode, data)
        if (requestCode == ActivityRequestCode.TARGET.code && resultCode == ActivityResultCode.TARGET.code) {
            Log.d("ReturnedData", "Value: ${data?.extras?.getInt(ActivityExtraData.NUMBER.key)}")
        }
    }
}
class TargetActivity : Activity() {
    private val closeButton: Button?
        get() = findViewById<Button>(R.id.button) as? Button
    var number: Int? = null
    var content: Content? = null
    
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_target)
        content = intent.extras?.get(ActivityExtraData.CONTENT.key) as? Content
        number = intent.extras?.get(ActivityExtraData.NUMBER.key) as? Int
        
        closeButton?.setOnClickListener {
            val data = Intent()
            data.putExtra(ActivityExtraData.NUMBER.key, number)
            setResult(ActivityResultCode.TARGET.code, data)
            finish()
        }
    }
}
```

**（６）以下のコードのTODO箇所を埋めて、Activity（呼び出し側）とカスタムビューとの間での処理のやりとりを実現するコードを書いてください。その際、下記の条件を満たすこと。**

> - `SampleView`に`listener`を設定して、ボタンがタップされた際にActivity側でイベントを受け取るようにすること
> - `SampleView`にデータ（`Content`）を設定したら、`textView`にデータの`text`を表示させること
> - `SampleView`の`update()`関数を呼び出したら、`textView`に表示される情報を更新すること

（６）Answer:
```
//kotlin
data class Content(
    var id: Int = 0,
    var text: String = ""
)
class MainActivity : Activity() {
    private val view: SampleView?
        get() = findViewById<SampleView>(R.id.sample_view) as? SampleView
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        val content = Content(id = 1234, text = "test content")
        view?.listener = object : SampleView.ClickListener {
            override fun onClick() {
                Log.d("Button", "Button clicked in SampleView")
            }
        }
        view?.content = content
    }
}
class SampleView : FrameLayout {
    private val textView: TextView?
        get() = findViewById<TextView>(R.id.text_view) as? TextView
    private val button: Button?
        get() = findViewById<Button>(R.id.text_view) as? Button
    var listener: ClickListener? = null
    var content: Content? by Delegates.observable(null) { _, _, _ ->
        update()
    }
    
    constructor(context: Context) : super(context) {
        button?.setOnClickListener {
            listener?.onClick()
        }
    }
    fun update() {
        textView?.text = content?.name
    }

    interface ClickListener {
        fun onClick()
    }
}
```

**（７）以下のコードのTODO箇所を埋めて、コード中の各ImageViewに、指定された方法で画像を表示させるコードを書いてください。その際、下記の条件を満たすこと。**

> - `imageView1`にresourceのdrawableから "icon" を設定すること
> - `imageView2`にassetから画像ファイル "icon2.png" を読み込み、Bitmapとして設定すること
> - `imageView3`にenum`BrandIcon`を使ってアイコンフォントの画像を設定すること
> - `imageView4`にOSS`Picasso`を使って、画像URL "https://sample.com/sample.jpg" を読み込むこと

（７）Answer:
```
//kotlin
class MainActivity : Activity() {
    private val imageView1: ImageView?
        get() = findViewById<ImageView>(R.id.image_view_1) as? ImageView
    private val imageView2: ImageView?
        get() = findViewById<ImageView>(R.id.image_view_2) as? ImageView
    private val imageView3: ImageView?
        get() = findViewById<ImageView>(R.id.image_view_3) as? ImageView
    private val imageView4: ImageView?
        get() = findViewById<ImageView>(R.id.image_view_4) as? ImageView

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        
        //`imageView1`にresourceのdrawableから "icon" を設定すること
        imageView1.setImageResource(R.drawable.icon)
        
        `imageView2`にassetから画像ファイル "icon2.png" を読み込み、Bitmapとして設定すること
        try {
            val inputStream = assets.open("icon2.png")
            val bitmap = BitmapFactory.decodeStream(inputStream)
            imageView2?.setImageBitmap(bitmap)
        } catch (e: Exception) {
            imageView2?.setImageResource(R.drawable.arrow_background)
        }
        
        `imageView3`にenum`BrandIcon`を使ってアイコンフォントの画像を設定すること
        imageView3.setImageDrawable(BrandIconDrawable.create(this, BrandIcon.INSTAGRAM, 20.pixel))
        
        `imageView4`にOSS`Picasso`を使って、画像URL "https://sample.com/sample.jpg" を読み込むこと
        Handler().postDelayed({
            imageView4?.let {
                Picasso.get()
                    .load("https://sample.com/sample.jpg")
                    .into(it)
            }
        }, 500L)
    }
}
enum class BrandIcon(val key: Int) {
    TWITTER(1);
    val resourceId: Int
        get() {
            return when (this) {
                TWITTER -> R.string.icon_brand_twitter
            }
        }
    val color: Int
        get() {
            return when (this) {
                TWITTER -> Color.parseColor("#55acee")
            }
        }
}
abstract class FontIconDrawable: Drawable() {
    protected var text: String = ""
    protected val context: Context = App.context
    protected val textPaint: TextPaint = TextPaint()
    protected abstract val typeface: Typeface?
    protected var size: Int = 20
    protected var _alpha: Int = 255
    override fun isStateful(): Boolean {
        return true
    }
    override fun setState(stateSet: IntArray): Boolean {
        val oldValue = textPaint.alpha
        val newValue = if (isEnabled(stateSet)) _alpha else _alpha / 2
        textPaint.alpha = newValue
        return oldValue != newValue
    }
    fun isEnabled(stateSet: IntArray): Boolean {
        for (state in stateSet)
            if (state == android.R.attr.state_enabled)
                return true
        return false
    }
    fun alpha(alpha: Int): FontIconDrawable {
        setAlpha(alpha)
        invalidateSelf()
        return this
    }
    override fun draw(canvas: Canvas) {
        textPaint.textSize = bounds.height().toFloat()
        val textBounds = Rect()
        val textValue = text
        textPaint.getTextBounds(textValue, 0, 1, textBounds)
        val textBottom = (bounds.height() - textBounds.height()) / 2f + textBounds.height() - textBounds.bottom
        canvas.drawText(textValue, bounds.width() / 2f, textBottom, textPaint)
    }
    override fun setAlpha(p0: Int) {
        _alpha = p0
        textPaint.alpha = p0
    }
    override fun getOpacity(): Int {
        return PixelFormat.OPAQUE
    }
    override fun setColorFilter(p0: ColorFilter?) {
        textPaint.colorFilter = p0
    }
    override fun getIntrinsicHeight(): Int {
        return size
    }
    override fun getIntrinsicWidth(): Int {
        return size
    }
    protected fun update() {
        textPaint.typeface = typeface
        textPaint.textSize = size.toFloat()
        textPaint.style = Paint.Style.FILL_AND_STROKE
        textPaint.textAlign = Paint.Align.CENTER
        textPaint.isUnderlineText = false
        textPaint.isAntiAlias = true
        setBounds(0, 0, size, size)
        invalidateSelf()
    }
}
class BrandIconDrawable: FontIconDrawable() {
    override val typeface: Typeface? = CachedTypeFaces.get(context, "fa-brands-400.ttf")
    companion object {
        fun create(context: Context, brand: BrandIcon, size: Int): BrandIconDrawable {
            val drawable = BrandIconDrawable()
            drawable.text = context.getString(brand.resourceId)
            drawable.size = size
            drawable.update()
            drawable.textPaint.color = brand.color
            return drawable
        }
    }
}
object CachedTypeFaces {
    private val cache = HashMap<String, Typeface>()
    fun get(context: Context, assetPath: String): Typeface? {
        synchronized(cache) {
            try {
                if (!cache.containsKey(assetPath)) {
                    val typeface = Typeface.createFromAsset(context.assets, assetPath)
                    cache[assetPath] = typeface
                    return typeface
                }
            } catch (e: Exception) {
                return null
            }
            return cache[assetPath]
        }
    }
}
```

## Day4

**（８）下記の各データを保存するとき、どのような手法を用いて要件を満たせば良いか、その理由も含めて説明してください。**

①　アプリのインストール後チュートリアルの閲覧が完了したかどうかのフラグ

（８）①　Answer:
```
I will save it to app local SharedPreference storage. Because it is related to app install, so when user installs the app, it will be stored only for first time. And will be deleted with app uninstall.
```

②　マスターデータ

（８）②　Answer:
```
I will put master data in files (like json or text) and put them in asset folder. When app first starts or data is not available, then will read from asset. As the master data will not change during app lifetime (at least unless next update), so i will put them as file in asset.
```

③　ユーザーまたはアプリ運営者が継続的に投稿しているコンテンツ

（８）③　Answer:
```
As these data will update continuously, i will prefer them  to store in server and access them through API. Because these data is needed to be saved even after user deletes cache or uninstalls app and reinstall, so they should be keept in a non-losing manner.
```

④　③のコンテンツのキャッシュ

（８）④　Answer:
```
For caching server contents, i would prefer local database like Room, Realm etc. Because there are time lags in fetching data from server also error prone. So, to display UI in a good looking style, we will use those cached data. And i think local database will be better fit for that purpose
```

**（９）以下の要件を満たすデータ群を、enumを用いて実際のコードで書いてください。**

> - 名前は`Gender`とすること
> - 「ID」はenumを**定義した時点で**プロパティアクセスできるようにすること
> - `name`プロパティを呼ぶことで「名前」を呼び出せること
> - `convert()`関数を呼ぶことで、「男性」の場合は「女性」、「女性」の場合は「男性」、「不明」の場合は「不明」のenum変数を返すようにすること
> - 値の仕様は以下のテーブルの通り

|  | 男性 | 女性 | 不明 |
|---:|:---|:---:|---:|
|ID|1 |2 |0 |
|名前 |男性 |女性 |不明 |

（９）Answer:
```
//kotlin
enum class Gender(val id: Int) {
    UNKNOWN(0), MALE(1), FEMALE(2);
    
    fun convert(): Gender {
        return when(this) {
            MALE -> {
                FEMALE
            }
            FEMALE -> {
                MALE
            }
            else -> {
                UNKNOWN
            }
        }
    }

    val name: String
        get() {
            return when (this) {
                UNKNOWN -> "Unknown"
                MALE -> "Male"
                FEMALE -> "Female"
            }
        }
}
```

**（１０）以下のURLのJSONをdata classに直したコードを書いてください。data classが複数ある際、それぞれの役割について簡単な説明をコメントで入れてください。**

> http://cs367.xbit.jp/~w065038/app/winas/list-with-error-code.json

（１０）Answer:
```
//kotlin
//This class will hold inner data of json response
data class Employee(
    var id: Int = 0,
    val name: String = "",
    val address: String = "",
    var gender: Int = Gender.UNKNOWN.ID
)

//This class will hold outer data of response json
data class EmployeeResponse(
    val data: List<Employee>,
    @Json(name = "error_code") val errorCode: Int,
    @Json(name = "total_count") val totalCount: Int
)
```

**（１１）データの取得と加工を、それが必要とする箇所（Activity側など）ではなく、仲介クラスやメソッド（講座では`Repository`という名前をつけたクラスを使った）を使って行ったほうが良い理由をわかりやすく説明してください。**

（１１）Answer:
```
Repository class is responsible to provide data (both from remote and local). It is beneficial to use Repository class instead of using data acquiring stuff in Activity/Fragment side. it decaouples the data fetch logic and makes classess more independent, reusable, maintenable, testable. It also encourages the concept of "Single responsibility" of class
```

**（１２）以下の要件を満たすデータをAndroidXのRoomでローカル保存するために必要なコードを「すべて」書いてください。**

> - データ名 `Content`、パラメータは以下を持つものとする
> - データの保存と取得のためのコードも書くこと（呼び出すだけで処理できるように）

| パラメータ名 | 型 | Roomのカラム名 |
|---:|:---:|:---:|
|id|Int|id|
|name|String|name|
|genderId|Int|gender_id|

（１2）Answer:
```
//kotlin
@Entity(
    tableName = "contents",
    indices = []
)
data class Content (
    @PrimaryKey @ColumnInfo(name = "id") var id: Int = 0,
    @ColumnInfo(name = "name") val name: String = "",
    @ColumnInfo(name = "gender_id") var genderId: Int = Gender.UNKNOWN.ID
)

@Dao
interface ContentDao {
    @Query("SELECT * FROM contents")
    fun getAllContents(): Flow<List<Content>>

    @Insert
    fun addAll(data: List<Content>)
}

@Database(entities = [Content::class], version = 1)
@TypeConverters(Converters::class)
abstract class AppDatabase : RoomDatabase() {
    abstract fun contentDao(): ContentDao
    
    companion object {
    
        @Volatile private var INSTANCE: AppDatabase? = null

        fun getInstance(): AppDatabase {
            return INSTANCE ?: synchronized(this) {
                INSTANCE ?: buildDatabase(context).also { INSTANCE = it }
            }
        }

        private fun buildDatabase(context: Context): AppDatabase {
            return Room.databaseBuilder(
                App.context,
                AppDatabase::class.java,
                "sample-database"
            ).build()
        }
    }
}

//Fetch data
AppDatabase.getInstance().contentDao().getAllContents().collect {
    it.forEach { content ->
        Timber.d("Local content data = $content")
    }
}
```

**（１３）サーバAPIからJSONデータを取得して、モデルクラスの形で呼び出し元まで返す過程を、準備のための実装フローも含めて、箇条書きでできるだけ詳しく、ロジックフローで説明してください。なお、ライブラリはRetrofit, OkHttp, Moshiを使うものとします。**

>例：　Step1: XXクラスをXXライブラリの仕様に沿うよう、XXする。  
>　　　Step2: XXデータをXXライブラリのXXメソッドを使ってXXする。

(13) Answer
```
Step1: use Retrofit to get data from remote server. This library is a TYPE-SAFE client library which uses OkHttp for network call. Set API service interface in Retrofit to fetch data. Also set OKHttp client in retrofit.
Step2: Do original network call using OkHttp. OkHttp is another networking library who is responsible to establish connection with server and fetch data stream. We can also add interceptor in OkHttp client such as HeaderInterceptor for adding header info like basic authorization, Response interceptor for doing work depending response like observing HttpResponseCode and showing data or error, Network interceptor to observe networking problems etc.
Step3: Convert JSON string to data class. Use Moshi in Retrofit client builder to map JSON string into corresponding data model class. Moshi library is responsible for parsing/decoding JSON string into model class, encode data class into JSON string, data binding etc. We can set our custom Data Transfer Object for adapting data according to response json. We can use Kotlin's interface and generics.
```

## Day5

**（１４）複数の異なる型を持つデータ群を１つの配列で持ちたいとする。それらのデータをサーバAPIから取得した場合、どのように実装をすればいいのか、MoshiのPolymorphicJsonAdapterFactoryを使って、以下のコードのTODO箇所を埋める形でコードを書いてください。**

>  - APIのURLはこちらから `http://cs367.xbit.jp/~w065038/app/winas/day5/list-adr.json`

（１４）Answer:
```kotlin
val apiInterface = Retrofit
    .Builder()
    .addConverterFactory(
        MoshiConverterFactory.create(
            Moshi.Builder()
                .add(PolymorphicJsonAdapterFactory.of(Feedable::class.java, "content_type")
                            .withSubtype(Restaurant::class.java, FeedContentType.Restraunt.index)
                            .withSubtype(Hospital::class.java, FeedContentType.Hospital.index)
                        )
                .add(KotlinJsonAdapterFactory())
                .build()
        )
    )
    .addCallAdapterFactory(CoroutineCallAdapterFactory())
    .client(createOkHttpClient())
    .baseUrl("http://cs367.xbit.jp/~w065038/app/winas/day5/")
    .build()
    .create(SampleApiService::class.java)
data class Restaurant(
    var id: Int = 0,
    var name: String = ""): Feedable {
    override val feedContentType: FeedContentType
        get() = FeedContentType.Restraunt
}
data class Hospital (
    var id: Int = 0,
    var name: String = ""): Feedable {
    override val feedContentType: FeedContentType
        get() = FeedContentType.Hospital
}
enum class FeedContentType(val index: String) {
    Hospital("hospital"), Restraunt("restaurant")
}
interface Feedable {
    val feedContentType: FeedContentType
}
interface SampleApiService {
    @GET("list-adr.json")
    fun getList(): Deferred<Response<List<FeedableItem>>>
}
fun getList(
    completion: (contents: List<Feedable>) -> Unit,
    failure: () -> Unit
    ) {
    val response = Repository.apiInterface.getList()
        GlobalScope.async {
            response.await()
            Handler(Looper.getMainLooper()).post {
                val response = response.getCompleted()
                if (response.isSuccessful) {
                    val data = response.body()
                    if (data != null) {
                        completion(data)
                    } else {
                        failure()
                    }
                } else {
                    failure()
                }
            }
        }
}
getList(
    completion = { contents ->
        items = contents
    }, failure = {
        // do nothing
    }
)
```

**（１５）あるカスタムビュークラスが、プロパティのデータの型によって異なる表示を行うものとします。その際、ジェネリクスを使った場合の実装、使わない場合の実装のコード例を、以下のコードを埋める形でそれぞれ書いてください。**

（１５）Answer:
```kotlin
data class Dog(
    override var id: Int = 0,
    override var name: String = "いぬ🐶"): Animal {}
data class Cat(
    override var id: Int = 0,
    override var name: String = "ねこ🐱"): Animal {}
interface Animal {
    val id: Int
    val name: String
}

class GenericsAnimal<T: Animal>(var animal: T)

class SampleCustomView: FrameLayout {
    ジェネリクスを使った場合
    var data: GenericsAnimal<Animal>? by Delegates.observable(null) {_, _, _ ->
        textView?.text = data?.animal?.name
    }

    ジェネリクスを使わない場合
    var data: Animal? by Delegates.observable(null) {_, _, _ ->
        textView?.text = data?.name
    }
    
    private val textView: TextView?
        get() = findViewById<TextView>(R.id.text_view) as? TextView
}
```
**（１６）あるActivityが、プロパティに指定されたモデルデータに基づいてUIを構築・表示する役割を持っていた場合について考えます。**

①　その「モデルデータ」がnullになるケースとしてどのようなケースが考えられるか。わかりやすく説明してください。

（１６）① Answer:
```
モーデルデータがnullになるケースであれば、その時IDを使ってサーバからデータを持ってきて表示します。サーバからもデータがもらうできなければエラーメッセージとかを表示します。あとは、モデルデータとIDどっちもnullであればデータ表示できないのでエラーメッセージを表示します。
```

②　モデルデータがnullであった場合、どのようにして代替処理を行うか、以下のコードのTODO箇所を埋める形でコードを書いてください。

（１６）② Answer:
```kotlin
data class Content(
    var id: Int = 0,
    var text: String = ""
): Parcelable {
    override fun describeContents(): Int {
        return 0
    }

    override fun writeToParcel(dest: Parcel?, flags: Int) {
        dest?.writeInt(id)
        dest?.writeString(text)
    }

    constructor(parcel: Parcel) : this (
        parcel.readInt(),
        parcel.readString() ?: ""
    )

    companion object CREATOR : Parcelable.Creator<Content> {
        override fun createFromParcel(parcel: Parcel): Content {
            return Content(parcel)
        }
        override fun newArray(size: Int): Array<Content?> {
            return arrayOfNulls(size)
        }
    }
}
class ContentActivity : Activity() {
    companion object {
        private const val EXTRA_CONTENT = "EXTRA_CONTENT"
        private const val EXTRA_CONTENT_ID = "EXTRA_CONTENT_ID"
    }
    private var content: Content by Delegates.observable(Content()) { _, _, _ ->
        updateView()
    }

    private val textView: TextView?
        get() = findViewById<TextView>(R.id.text_view) as? TextView
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_content)

        if (intent.hasExtra(EXTRA_CONTENT)) {
            val data = intent.getParcelableExtra(EXTRA_CONTENT) as? Content
            if (data != null) {
                content = data
            } else {
                fetchContent()
            }
        } else {
            fetchContent()
        }
    }

    private fun updateView() {
        textView?.text = content.text
    }

    private fun fetchContent() {
        val contentId = intent.getIntExtra(EXTRA_CONTENT_ID, 0)
        if (contentId != 0) {
            ContentRepository().getContent(
                contentId,
                completion = { content ->
                    this.content = content
                },
                failure = {
                    Toast.makeText(this, "データの取得エラーです。詳細は表示できません。", Toast.LENGTH_SHORT)
                }
            )
        } else {
            Toast.makeText(this, "データの取得エラーです。詳細は表示できません。", Toast.LENGTH_SHORT)
        }
    }
}
class ContentRepository {
    fun getContent(
        contentId: Int,
        completion: (content: Content) -> Unit,
        failure: () -> Unit
    ) {
        // API通信でのデータ取得処理は省略
        let content = Content()
        content.id = contentId
        content.name = "テストコンテンツ"
        completion(content)
    }
}
```

## Day6

**（１７）Activityが破棄されてから復元された際に、プロパティ「counter」のデータも復元されるよう、以下のコードに追記する形で実装してください。**

（１７）Answer:
```kotlin
class SampleActivity : Activity() {
    private var counter: Int = 0
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        counter = savedInstanceState?.getInt(PROP_COUNTER) ?: 0
    }

    companion object {
        private const val PROP_COUNTER = "PROP_COUNTER"
    }

    override fun onSaveInstanceState(outState: Bundle) {
        super.onSaveInstanceState(outState)
        outState.putInt(PROP_COUNTER, counter)
    }
}
```
