## Room

### 添加依赖
```java
def room_version = "1.1.1"
api  "android.arch.persistence.room:runtime:$room_version"
kapt "android.arch.persistence.room:compiler:$room_version"
api  "android.arch.persistence.room:rxjava2:$room_version"
api  "android.arch.persistence.room:guava:$room_version"
testImplementation  "android.arch.persistence.room:testing:$room_version"
```

### 创建数据库
```kotlin
@Database(entities = [User::class], version = 1)
abstract class AppDatabase : RoomDatabase() {
    
    //数据库操作DAO层
    abstract fun userDao(): UserDao
    
    companion object {
        private var INSTANCE: AppDatabase? = null
		//当数据库版本 1->2
        private val MIGRATION_1_2 = object : Migration(1, 2) {
            override fun migrate(database: SupportSQLiteDatabase) {
                database.execSQL("")
            }
        }
		//数据库版本　2->3
        private val MIGRATION_2_3 = object : Migration(2, 3) {
            override fun migrate(database: SupportSQLiteDatabase) {
                database.execSQL("")
            }
        }
        //获取唯一实例
        fun getInstance(context: Context) : AppDatabase =
            INSTANCE ?: synchronized(this) {
                INSTANCE ?: buildDatabase(context).also {
                    INSTANCE = it
                }
            }
		//创建数据库
        private fun buildDatabase(context: Context) =
            Room.databaseBuilder(context.applicationContext,
                AppDatabase::class.java, "Sample.db")
                .addMigrations(MIGRATION_1_2)
                .addMigrations(MIGRATION_2_3)
                .build()
    }
}
```

### 数据库表操作DAO层
```kotlin
/**
 * 定义数据库操作的方法
 */
@Dao
interface UserDao {

    @Insert
    fun insertUser(user: User)

    @Insert
    fun insertUserList(array: Array<User>)

    @Update
    fun update(user: User)

    @Delete
    fun delete(user: User)

    @Query("SELECT * FROM user")
    fun selectAll(): Array<User>

    @Query("select * from user where userName = :name")
    fun selectUser(name: String): Array<User>

    @Query("SELECT userName,city FROM user")
    fun selectUserCity(): Array<UserTuple>

    @Query("SELECT userName,street FROM user WHERE city IN (:cityArray)")
    fun loadUserInCity(cityArray: Array<String>): Array<UserTuple>

    @Query("SELECT userName,street FROM user WHERE city IN (:cityArray)")
    fun loadUserInCityLive(cityArray: Array<String>): LiveData<Array<UserTuple>>

    @Query("SELECT * FROM user WHERE id = :id LIMIT 1")
    fun loadUserRxJava(id: Int): Flowable<User>
}
```

### 数据实体：数据库中的表
```kotlin
@Entity(tableName = "user")
class User {

    @PrimaryKey(autoGenerate = true)
    var id = 0

    var strength = 0

    @ColumnInfo(name = "userName")
    var name: String? = null
	
    //嵌套对象
    @Embedded
    var address: Address? = null
}

class Address {
    var street: String? = null
    var state: String? = null
    var city: String? = null
    @ColumnInfo(name = "postcode")
    var postCode = 0
}
```