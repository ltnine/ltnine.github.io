# ltnine.github.io
```xml
<?xml version="1.0" encoding="UTF-8"?>
<Configuration status="WARN">
    <properties>
        <!-- 需要引入pfinder支持，隐式打印traceId。Message最大长度1000个字符，异常栈最大打印200行 -->
        <property name="PATTERN_LAYOUT">[%d{yyyy-MM-dd HH:mm:ss.SSS}][%p][%thread][SECURITY][%X{PFTID}][%l] - %.1000m - %ex{200}%n</property>
        <!-- 填写路径，默认使用 /export/Logs/{应用域名} -->
        <property name="LOG_HOME">/export/Logs/r.jdl.com</property>
        <!-- 默认日志级别 -->
        <property name="LOG_LEVEL">INFO</property>
    </properties>

    <Appenders>
        <!-- info -->
        <RollingFile name="infoFile" fileName="${LOG_HOME}/route-ops.log"
                     filePattern="${LOG_HOME}/route-ops.log.%i"
                     bufferSize="8192" immediateFlush="false" append="true">
            <Filters>
                <ThresholdFilter level="error" onMatch="DENY" onMismatch="NEUTRAL"/>
                <ThresholdFilter level="warn" onMatch="ACCEPT" onMismatch="NEUTRAL"/>
                <ThresholdFilter level="info" onMatch="ACCEPT" onMismatch="NEUTRAL"/>
                <ThresholdFilter level="debug" onMatch="ACCEPT" onMismatch="DENY"/>
            </Filters>
            <PatternLayout pattern="${PATTERN_LAYOUT}" charset="UTF-8"/>
            <Policies>
                <SizeBasedTriggeringPolicy size="300MB"/>
            </Policies>
            <DefaultRolloverStrategy max="40"/>
        </RollingFile>
        <!-- error -->
        <RollingFile name="errorFile" fileName="${LOG_HOME}/error.log"
                     filePattern="${LOG_HOME}/error.log.%i"
                     bufferSize="8192" immediateFlush="false" append="true">
            <Filters>
                <ThresholdFilter level="error" onMatch="ACCEPT" onMismatch="DENY"/>
            </Filters>
            <PatternLayout pattern="${PATTERN_LAYOUT}" charset="UTF-8"/>
            <Policies>
                <SizeBasedTriggeringPolicy size="300MB"/>
            </Policies>
            <DefaultRolloverStrategy max="40"/>
        </RollingFile>

        <!-- 全局日志 -->
        <RollingFile name="globalAppender" fileName="${LOG_HOME}/global.log" filePattern="${LOG_HOME}/global.log.%i">
            <PatternLayout pattern="${PATTERN_LAYOUT}" charset="UTF-8"/>
            <Policies>
                <SizeBasedTriggeringPolicy size="300MB"/>
            </Policies>
            <!-- 注意此处磁盘最大占比 -->
            <DefaultRolloverStrategy max="40"/>
        </RollingFile>
    </Appenders>

    <Loggers>
        <!-- 分开打印info和error级别日志至不同的文件中 -->
        <Logger name="com.jdl.fot" level="${output_log_level}" additivity="false" includeLocation="true">
            <AppenderRef ref="infoFile"/>
            <AppenderRef ref="errorFile"/>
        </Logger>
        <!-- jvm参数配置log4j2.contextSelector开启异步logger -->
        <AsyncRoot level="${LOG_LEVEL}" includeLocation="true">
            <AppenderRef ref="globalAppender"/>
        </AsyncRoot>
    </Loggers>
</Configuration>
```
