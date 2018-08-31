---
title : oc notes
Time  : 2018-05-10
author: sunflowerseat

---

# Realm记录

## 基本配置

```Swift
var config = Realm.Configuration(
	deleteRealmIfMigrationNeeded: true
)
```



## 主键声明

```swift
	override static func primaryKey() -> String {
		return "projectItpFieldValueId"
	}
```

## 忽略属性

```swift
    override static func ignoredProperties() -> [String] {
        return ["tmpID"]
    }
```

## 属性备忘单
| 类型           | 非可空值形式                                                 | 可空值形式                               |
| -------------- | ------------------------------------------------------------ | ---------------------------------------- |
| Bool           | `@objc dynamic var value = false`                            | `let value = RealmOptional<Bool>()`      |
| Int            | `@objc dynamic var value = 0`                                | `let value = RealmOptional<Int>()`       |
| Float          | `@objc dynamic var value: Float = 0.0`                       | `let value = RealmOptional<Float>()`     |
| Double         | `@objc dynamic var value: Double = 0.0`                      | `let value = RealmOptional<Double>()`    |
| String         | `@objc dynamic var value = ""`                               | `@objc dynamic var value: String? = nil` |
| Data           | `@objc dynamic var value = Data()`                           | `@objc dynamic var value: Data? = nil`   |
| Date           | `@objc dynamic var value = Date()`                           | `@objc dynamic var value: Date? = nil`   |
| Object         | 不存在：必须是可空值                                         | `@objc dynamic var value: Class?`        |
| List           | `let value = List<Type>()`                                   | 不存在：必须是非可空值                   |
| LinkingObjects | `let value = LinkingObjects(fromType: Class.self, property: "property")` | 不存在：必须是非可空值                   |

##Tip
- 除了RealmList 可以使用var来声明以外， 其他需要保存到数据库中属性都必须用

  `@objc dynamic var  `

## 查询语句

- 比较操作数可以是属性名，也可以是常量。但至少要有一个操作数是属性名；
- 比较操作符 ==、<=、<、>=、>、!= 和 BETWEEN 支持 Int、Int8、Int16、Int32、Int64、Float、Double 以及 Date 这几种属性类型，例如 age == 45；
- 比较是否相同：== 和 !=，例如，Results<Employee>().filter("company == %@", company)；
- 比较操作符 == 和 != 支持布尔属性；
- 对于 String 和 Data 属性而言，支持使用 ==、!=、BEGINSWITH、CONTAINS 和 ENDSWITH 操作符，例如 name CONTAINS 'Ja'；
- 对于 String 属性而言，LIKE 操作符可以用来比较左端属性和右端表达式：? 和 * 可用作通配符，其中 ? 可以匹配任意一个字符，* 匹配 0 个及其以上的字符。例如：value LIKE '?bc*' 可以匹配到诸如 “abcde” 和 “cbc” 之类的字符串；
- 字符串的比较忽略大小写，例如 name CONTAINS[c] 'Ja'。请注意，只有 “A-Z” 和 “a-z” 之间的字符大小写会被忽略。[c] 修饰符可以与 [d] 修饰符结合使用；
- 字符串的比较忽略变音符号，例如 name BEGINSWITH[d] 'e' 能够匹配到 étoile。这个修饰符可以与 [c] 修饰符结合使用。（这个修饰符只能够用于 Realm 所支持的字符串子集：参见当前的限制一节来了解详细信息。）
- Realm 支持以下组合操作符：“AND”、“OR” 和 “NOT”，例如 name BEGINSWITH 'J' AND age >= 32；
- 包含操作符：IN，例如 name IN {'Lisa', 'Spike', 'Hachi'}；
- 空值比较：==、!=，例如 Results<Company>().filter("ceo == nil")。请注意，Realm 将 nil 视为一种特殊值，而不是某种缺失值；这与 SQL 不同，nil 等同于自身；
- ANY 比较，例如 ANY student.age < 21；
- List 和 Results 属性支持聚集表达式：@count、@min、@max、@sum 和 @avg，例如 realm.objects(Company.self).filter("employees.@count > 5") 可用以检索所有拥有 5 名以上雇员的公司。
- 支持子查询，不过存在以下限制：
	- @count 是唯一一个能在 SUBQUERY 表达式当中使用的操作符；
	- SUBQUERY(…).@count 表达式只能与常量相比较；
	- 目前仍不支持关联子查询。