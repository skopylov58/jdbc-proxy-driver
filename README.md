## JDBC proxying driver (JDBCMiddleman)

### What for?

JDBCMiddleman driver can be used for:

- performance benchmarking of your JDBC code
- looking what really happens under the hood
- troubleshooting your code
- simulating network latencies
- discovering uncaught exceptions
- intercepting any JDBC call with your custom interceptors
- inspecting 3-rd party JDBC code

### How to use

1. Put this driver into your's application class-path.
2. Prepend your's DB URL with "jdbc:middleman:" prefix, for example "jdbc:middleman:jdbc:mysql://localhost:3306/foo"
3. Done
4. Run your application and observe info in the standard output.

### Custom interceptors

JDBCMiddleman driver is shipped with default `SimpleLoggingInterceptor` which logs to the standard output JDBC invocations and corresponding results with nanoseconds precision:

```
Invoke: prepareStatement with params [INSERT INTO NAMES (name, age) VALUES (?, ?)]
Result prep0: INSERT INTO NAMES (name, age) VALUES (?, ?) took PT0.0019989S
Invoke: setString with params [1, John]
Result null took PT0S
Invoke: setInt with params [2, 40]
Result null took PT0.0010007S
Invoke: executeUpdate with params null
Result 1 took PT0.0019985S
Invoke: executeQuery with params [SELECT * from NAMES]
Result rs3: org.h2.result.LocalResult@543588e6 columns: 2 rows: 1 pos: -1 took PT0.0130346S
Invoke: next with params null
Result true took PT0S
Invoke: getString with params [1]
Result John took PT0S
Invoke: getInt with params [2]
Result 40 took PT0S
John 40
```

You may create your own interceptor by implementing 
[jdbc.Interceptor](src/main/java/com/github/skopylov58/jdbc/Interceptor.java) interface.
To bring your interceptor into action, please:

- put your's interceptor into application class-path 
- specify system property `jdbc.interceptor` with your's interceptor full class name

