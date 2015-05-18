### statreduce

statreduce is a library for using R from mapreduce jobs.
It lets you write map step in java and reduce step in R.

It provides simple abstractions like ListStatReducer and MatrixStatReducer for working with R vector and R matrix.

ListStatReducer takes a list of int/double as input from map step, converts it to R vector, invokes R function and stores the result.

MatrixStatReducer takes a matrix of int/double as input from map step, converts it to R matrix, invokes R function and stores the result.

To use this library, add statreduce as dependency:
```
<dependency>
  <groupId>com.fk</groupId>
  <artifactId>statreduce</artifactId>
  <version>0.0.1</version>
</dependency>
```

If you're doing statistical operation on a vector of int/double, use ListStatReducer
```
public class TemperatureSummaryReducer extends ListStatReducer {
    public TemperatureSummaryReducer(){
        super("tempValues", "avgTemp", "", "avgTemp <- mean(tempValues)", Double.class, Double.class);
    }
}
```

tempValues is the double vector passed to R.

avgTemp <- mean(tempValues) is the function call invoked in R.

avgTemp is the value returned as Reducer output.

If you're doing statistical operation on a matrix of int/double, use MatrixStatReducer:
```
public class MoneyballOnBasePercentageReducer extends MatrixStatReducer {
    public MoneyballOnBasePercentageReducer() {
        super("playerStats", "obp", ScriptUtils.fromFile("/tmp/on_base_percentage.R"),
                "obp <- calculateOBP(playerStats)", Double.class, Double.class);
    }
}
```
This calculates On-base percentage for baseball players by taking playerStats as input and returning obp as output.
R functions can also be loaded from files/hdfs/jar.

Look at [MoneyballOnBasePercentageDriver.java](https://github.com/flipkart-incubator/statreduce/tree/master/src/examples/java/com/statreduce/examples/baseball) or [TemperatureSummaryDriver.java](https://github.com/flipkart-incubator/statreduce/tree/master/src/examples/java/com/statreduce/examples/temperature) for usage examples.
(Sample data and R functions are in data/ and functions/ dir)
