---
title: Clean HikariCP Resource Configured in the Application Context
date: 2017-06-19 12:31:53
tags:
- tomcat
- resource
- hikaricp
- context
---

I've been dealing with a problem for over a week with the connection pooling {% link HikariCP https://brettwooldridge.github.io/HikariCP/ %}. The problem was that whenever we redeploy the app, the pool left opened connections and that lead to connectivity issues as the connections reached a certain threshold, so after 10 deploys, our app could not connect to the database anymore.

This was our original configuration : 

```xml META-INF\Context.xml

<Resource name="jdbc/PostgresHikari" auth="Container"
    factory="com.zaxxer.hikari.HikariJNDIFactory"
    type="javax.sql.DataSource"
    minimumIdle="5" 
    maximumPoolSize="50"
    connectionTimeout="3000"
    jdbcUrl="jdbc:postgresql://path-to-database/test"
    driverClassName="org.postgresql.Driver"
    dataSource.implicitCachingEnabled="true" 
    dataSource.user="user"
    dataSource.password="password"
/>


```

And it was located under *META-INF* of the project. After a lot of research we came to the conclusion that tomcat somehow was not cleaning the pool and after reading the docs, we came to find this little guy hidden away from us : 

```java
closeMethod="close"
```

Turns out that *HikariCP* does have a cleaning method and so does tomcat, so when you define a resource, you can hint tomcat as to which method to call in order to clean resources after they are no longer need ( i.e. when you redeploy or reload the app). So the working Context that worked flawlessly at the end is : 

```xml 
<Resource name="jdbc/PostgresHikari" auth="Container"
    factory="com.zaxxer.hikari.HikariJNDIFactory"
    type="javax.sql.DataSource"
    minimumIdle="5" 
    maximumPoolSize="50"
    connectionTimeout="3000"
    jdbcUrl="jdbc:postgresql://path-to-database/test"
    driverClassName="org.postgresql.Driver"
    dataSource.implicitCachingEnabled="true" 
    dataSource.user="user"
    dataSource.password="password"
    closeMethod="close"
/>
``` 