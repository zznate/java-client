---
layout: default
---
## Priming Queries

You build priming requests with the PrimingRequestBuilder. The builder takes the following:

 * **Query** - The only mandatory property e.g "select * from person"
 * **Consistency** - Defaults to all consistencies. Can override to ONE, TWO, LOCAL_QUORUM etc
 * **Result** - Defaults to Success. Other values are Read timeout exception, Write timeout exception and Unavailable exception
 * **ColumnTypes** - All columns default to Varchar. You'll need to override this.

#### Basic Prime - all columns varchar

Priming to return a single row for the query "select * from person" two columns, first\_name and last\_name.

```java
Map<String, String> row = ImmutableMap.of(
        "first_name", "Chris",
        "last_name", "Batey");
PrimingRequest singleRowPrime = PrimingRequest.queryBuilder()
        .withQuery("select * from person")
        .withRows(row)
        .build();
primingClient.primeQuery(singleRowPrime);

```

The type of values in the rows map depend on the type of the column you're stubbing.

#### Prime columns other than varchar

Here is how you would prime a int column. Any columns that are varchar you can omit from the column types map.
When Cassandra returns rows to a driver it specifies the type. Most drivers will throw an exception if you
request a column with the incorrect type.

```java
Map<String, ? extends  Object> row = ImmutableMap.of(
        "first_name", "Chris",
        "last_name", "Batey",
        "age", 29);
Map<String, ColumnTypes> columnTypes = ImmutableMap.of(
        "age", ColumnTypes.Int
);
PrimingRequest singleRowPrime = PrimingRequest.queryBuilder()
        .withQuery("select * from person")
        .withColumnTypes(columnTypes)
        .withRows(row)
        .build();
primingClient.primeQuery(singleRowPrime);
```

Full details of what Java type you should use for each CQL type is [here]({{ site.baseurl }}/column-types).

#### Priming errors

If you want to replicate Cassandra errors you need to override the Result on your prime:

```java
PrimingRequest primeReadRequestTimeout = PrimingRequest.queryBuilder()
        .withQuery("select * from person")
        .withResult(PrimingRequest.Result.read_request_timeout)
        .build();
primingClient.primeQuery(primeReadRequestTimeout);
```

The Result enum contains read request timeout, write request timeout and unavailable. You don't need rows or
column types for this as no rows are returned.


#### Resetting your primes

No need to restart Scassandra. You can either remove all your query primes or all primes (including prepared statement primes).

```java
primingClient.clearQueryPrimes();
primingClient.clearAllPrimes();

```