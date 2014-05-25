---
layout: default
---
## Priming Prepared Statements

You need to prime any prepared statements your application is going to use before your application prepares the statement,
not just before it executes it. So if your application prepares them on startup you should prime Scassandra before you
start your application.

This is because when your application prepares the statement Cassandra responds with the types of each of the variable's (?s)
type information. Drivers use this information to validate your execution of the prepared statement without sending it to
Cassandra. So if you do not prime before your application prepares the statement Scassandra can only guess what the types are,
 and it defaults them all to varchar.

#### Priming variable types, without any rows

This primes a prepared statement with two variables, where the first is a varchar and the second is an int. You specify
the variable types in the order they appear in the prepared statement text.

```java
PrimingRequest preparedStatementPrime = PrimingRequest.preparedStatementBuilder()
    .withQuery("insert into person(first_name, age) values (?,?)")
    .withVariableTypes(ColumnTypes.Varchar, ColumnTypes.Int)
    .build();
primingClient.primePreparedStatement(preparedStatementPrime);
```

As this is an insert then you don't need to specify any rows to return.

#### Priming variable types and rows with column types

This is about as complicated as it gets. You want to prime the variable types of the prepared statement as well as
return rows with column types other than varchat.

```java
Map<String, ? extends  Object> row = ImmutableMap.of(
        "first_name", "Chris",
        "last_name", "Batey",
        "age", 29);
Map<String, ColumnTypes> columnTypes = ImmutableMap.of(
        "age", ColumnTypes.Int
);
PrimingRequest preparedStatementPrime = PrimingRequest.preparedStatementBuilder()
        .withQuery("select * from person where first_name = ?")
        .withVariableTypes(ColumnTypes.Varchar, ColumnTypes.Int)
        .withColumnTypes(columnTypes)
        .withRows(row)
        .build();
primingClient.primePreparedStatement(preparedStatementPrime);
```

#### Priming errors

Works the same as with queries. See [priming queries]({{ site.baseurl }}/priming-query)

#### Resetting your primes

No need to restart Scassandra. You can either remove all your prepared statement primes or all primes (including prepared statement primes).

```java
primingClient.clearAllPrimes();
primingClient.clearPreparedPrimes();

```

