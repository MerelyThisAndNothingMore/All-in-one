---
tags:
  - AndroidJetPack
aliases:
---

# 简介

[[Jetpack]]’s Room 是一个[[持久化]]库，它为应用程序提供了一个抽象层，用于简化对 SQLite 数据库的访问。
Room 是 Google 官方推出的数据库 **ORM 框架**。[ORM](https://so.csdn.net/so/search?q=ORM&spm=1001.2101.3001.7020)：即 Object Relational Mapping，即对象关系映射，也就是将关系型数据库映射为[[面向对象]]的语言。它通过使用[[注解]]和强类型 API 来管理数据库操作，提供了比直接使用 SQLite 更便捷和安全的方式。

Room中对数据的操作不需要通过SQL语句，而是通过注解后的方法：

```kotlin
@Dao
interface UserDao {
    // 查询操作，@Query 中的 SQL 语句以及返回值类型等会在编译期进行检查，更早的暴露问题
    @Query("SELECT * FROM user")
    fun getAll(): List<User>

    // 查询操作，@Query 中的 SQL 语句可以用参数指定 @Query 中的 where 条件
    @Query("SELECT * FROM user WHERE id IN (:userIds)")
    fun loadAllByIds(userIds: IntArray): List<User>

    // 插入操作，编译期生成的代码会将所有的参数以单独的事务更新到 DB
    @Insert
    fun insertAll(vararg users: User)

    // 更新操作，根据参数对象的主键更新指定 row 的数据
    // onConflict 设置当事务中遇到冲突时的策略
    @Update(onConflict = OnConflictStrategy.REPLACE)
    fun update(vararg user: User)

    // 删除操作，根据参数对象的主键删除指定 row
    @Delete
    fun delete(user: User)
}
```

Room 包含三个组件：Entity、DAO 和 Database。
Entity：实体类，数据库中的表（table），保存数据库并作为应用持久性数据底层连接的主要访问点。
DAO：数据库访问对象，提供访问 DB 的 API，如增删改查等方法。
Database：访问底层数据库的入口，管理着真正的数据库文件。

![](https://img-blog.csdnimg.cn/c98ad91d8f2b45dea5db4608dcd765bf.png)


## Dao

```kotlin
// 该注解代表数据库一张表，tableName为该表名字，不设置则默认类名
// 注解必须有！！tableName可以不设置
@Entity(
    tableName = "User",
    indices = [Index(
        value = ["userName"],
        unique = true
    )]
)
data class User(
    // 该注解指定该字段作为表的主键, 自增长。注解必须有！！
    @PrimaryKey(autoGenerate = true)
    val id: Int? = null,

    // 该注解设置当前属性在数据库表中的列名和类型，注解可以不设置，不设置默认列名和属性名相同
    @ColumnInfo(name = "user_name", typeAffinity = ColumnInfo.TEXT)
    val userName: String?,
    
    // Entity 中的所有属性都会被持久化到数据库，除非使用 @Ignore
    @ColumnInfo(name = "user_gender", typeAffinity = ColumnInfo.TEXT)
    @Ignore val userGender: String?
)

```