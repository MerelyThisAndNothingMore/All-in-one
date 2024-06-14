---
tags:
  - AndroidJetPack
aliases:
---

# 简介

Jetpack’s Room 是一个[[持久化]]库，它为应用程序提供了一个抽象层，用于简化对 SQLite 数据库的访问。Room 通过使用[[注解]]和强类型 API 来管理数据库操作，提供了比直接使用 SQLite 更便捷和安全的方式。

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

![](https://img-blog.csdnimg.cn/c98ad91d8f2b45dea5db4608dcd765bf.png)


