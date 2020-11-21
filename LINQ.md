# LINQ
LINQ全名為Language Integrated Query，顧名思義就是此程式語言擁有查詢資料的能力，LINQ的出現使得C#
(F#、VB.NET也可以使用)有能力可以在程式中查找資料。

在沒有LINQ技術之前我們在查詢資料時會需要學習另一種查詢語法來達到查詢的需求，例如像是XML、SQL...等，除了要花費學習的精力外，也因為C#並不認識這些語法，所以寫出來的查詢語法在C#中並不會有型別檢查或是自動完成的功能，對於工程師來說這無疑會是一個除錯地獄。

有了LINQ之後，利用標準查詢運算子(Standard Query Operators)，工程師可以用原生的C#語言對資料做處理，選擇資料來源、進行篩選到組合、分組都可以利用標準查詢運算子完成，而且在撰寫的過程中還可以享受到型別檢查及自動完成帶來的便捷。

## Standard Query Operators
標準查詢運算子是應用於集合類別的運算子，它對集合實作了篩選、組合、排序..等等的運算功能，像是Select、Where、OrderBy...等方法，而這些方法就是運作於IEnumerable<T>、IQueryable<T>之上，所以前面的章節說明IEnumerable及泛型就是想要講解LINQ的核心運作方式。

---

## Select
拿出資料，並組成關聯物件
```C#
簽章:
public static IEnumerable<TResult> Select<TSource, TResult>(this IEnumerable<TSource> source, Func<TSource, TResult> selector);

IEnumerable<int> item = t.Select(x => x.Chapter);
var item t.Select(x => x.Chapter);
```

## Where
篩選出特定資料
```C#
簽章:
public static IEnumerable<TSource> Where<TSource>(this IEnumerable<TSource> source, Func<TSource, bool> predicate);

IEnumerable<Class2> item = t.Where(x => x.Chapter==1);
var item = t.Where(x => x.Chapter==1);
```

## OrderBy
把資料排序
```C#
簽章:
public static IOrderedEnumerable<TSource> OrderBy<TSource, TKey>(this IEnumerable<TSource> source, Func<TSource, TKey> keySelector);

IEnumerable<Class2> item = t.OrderBy(x => x.Chapter);
```

## GroupBy
把資料分組
```C#
簽章:
public static IEnumerable<IGrouping<TKey, TSource>> GroupBy<TSource, TKey>(this IEnumerable<TSource> source, Func<TSource, TKey> keySelector);

IEnumerable<IGrouping<int,Class2>> item = t.GroupBy(x => x.Chapter);
```

## First
取第一組符合條件的資料(沒有東西時會丟出例外)

## FirstOrDefault
取第一組符合條件的資料(沒有東西時會傳回default)

## ElementAt
取index位置的資料
---
資料範例:
```C#
List<Class2> t = new List<Class2>
            {
                new Class2 { Chapter = 1, Section = 1 },
                new Class2 { Chapter = 1, Section = 2 },
                new Class2 { Chapter = 1, Section = 3 },
                new Class2 { Chapter = 1, Section = 4 },
                new Class2 { Chapter = 1, Section = 5 },
                new Class2 { Chapter = 2, Section = 1 },
                new Class2 { Chapter = 2, Section = 2 },
                new Class2 { Chapter = 2, Section = 3 },
                new Class2 { Chapter = 2, Section = 4 },
                new Class2 { Chapter = 2, Section = 5 },
                new Class2 { Chapter = 3, Section = 1 },
                new Class2 { Chapter = 3, Section = 2 },
                new Class2 { Chapter = 3, Section = 3 },
                new Class2 { Chapter = 3, Section = 4 },
                new Class2 { Chapter = 3, Section = 5 },
            };
```
