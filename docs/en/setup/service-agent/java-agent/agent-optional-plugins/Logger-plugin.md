# Overview

`logger-plugin` can store the logs generated by the program during the call, such as the content of the error log, into the span log.  Through the configuration file, you can control the log source (log4j2, logback, log4j), package name, and level. Please note that the original `log4j.xml`(or other log framework config files) has a higher priority than the `logconfig.properties`.

# Configuration file

By default, the configuration file is in `apache-skywalking-apm-bin/agent/config/logger-plugin/logconfig.properties`. Of course, **If the file does not exist, the default configurations are as following:**

```properties
log4j.packages=*
log4j.level=error
log4j2.packages=*
log4j2.level=error
logback.packages=*
logback.level=error
```

The meaning of the above configuration is as follows:

1. SkyWalking opened the adaptor(bridge) between tracing kernel and log frameworks, including `log4j`, `log4j2`, `logback`.
2. Only collect logs at the `error` level, others would be ignored, including `trace`, `debug`, `info`, `warn`.
3. Wouldn't filter the logs by the package name.

# Property description

## packages

**package attribute**: Specify the package name of the log that needs log conversion. **The default value is `*`, and it will match all packages.**

packages value:

* the name of package, eg: `org.apache.skywalking`
* `*`:match all 

**Notice:**

When matching multiple packages, the names of different packages should be separated by a comma.

## level

**level attribute** : The level of the log for conversion. **By default it is `error` level**.

The hierarchy order of log levels from low to high is as follows:

`trace` < `debug` < `info` <`warn`< `error` < `fatal`

**Notice:**

Because `logback` does not support the `fatal` log level, the highest level you may set it to is **logback.level=error**.

# Use Cases:
<details>

<summary>Only open the bridge for log4j</summary>

```properties
# only collect the logs from package1 and package2
log4j.packages=package1,package2
# Only collect logs at the `fatal` level, others would be ignored, including `error`,`trace`, `debug`, `info`, `warn`
log4j.level=fatal
```

</details>

<details>

<summary>Open the bridge for log4j and logback</summary>

```properties
# for log4j, only collect the logs from package1 and collect logs at the `error` level and `fatal` level
log4j.packages=package1
log4j.level=error
# for logback, wouldn't filter the logs by the package name. and all level logs except `trace` level 
logback.packages=*
logback.level=debug
```

</details>
