---
layout: default
---
## Priming Collections

As of version ```0.5.0```

Stubbed Cassandra supports priming text maps (support for more types planned) and lists and sets of all types.
Stubbed Cassandra does not support user defined types yet.

### Priming Lists

You need to prime the column type to be of the list type of your schema. For example to prime a list of BigInts:

```java
ImmutableMap<String, ColumnTypes> columnTypes = ImmutableMap.of(
        "list_type_bigint", ColumnTypes.BigintList
);
ImmutableMap<String, List<Long>> rows = ImmutableMap.of(
        "list_type_bigint", Arrays.asList(1l, 2l, 3l)
);
PrimingRequest prime = PrimingRequest.queryBuilder()
        .withQuery(query)
        .withColumnTypes(columnTypes)
        .withRows(rows)
        .build();
```

To see what Java type you need to put in your rows map see [ColumnTypes](http://localhost:4000/java-client/column-types/) page.

The ColumnTypes enum should be self explanatory e.g VarcharList, BigIntList, TimeuuidList

### Priming Sets

The same as for lists but use the Column Type: ```<Type>Set``` e.g ```IntSet```