## 1. Introduction

### 1.1. License

Activiti 采用[the Apache V2 license](http://www.apache.org/licenses/LICENSE-2.0.html).

### 1.2. Download

<http://activiti.org/download.html>

### 1.3. Sources

本发布包含大部分的源码在 jar 文件中. Activiti 源码可以通过<https://github.com/Activiti/Activiti>获得。

### 1.4.  获取软件

#### 1.4.1. JDK 7+

Activiti运行在高于或等于版本7的JDK上。转到[Oracle Java SE下载]（http://www.oracle.com/technetwork/java/javase/downloads/index.html）并单击“下载”按钮JDK”。 该页面上也有安装说明。 要验证安装是否成功，请在命令行上运行`java -version`。 这应该打印已安装的JDK版本。

#### 1.4.2. IDE

Activiti开发可以使用您选择的IDE完成。 如果您想使用Activiti Designer，那么您需要Eclipse Kepler或Luna。 从[Eclipse下载页面]（http://www.eclipse.org/downloads/）下载您选择的eclipse发行版。 解压缩下载的文件然后你应该能够在目录`eclipse`中的eclipse文件启动它。 在本用户指南中，有一节[安装我们的eclipse设计器插件]（https://www.activiti.org/userguide/#eclipseDesignerInstallation）。

### 1.5. 报告问题

 每个开发者都应该阅读[如何提问]（http://www.catb.org/~esr/faqs/smart-questions.html）。您可以在[用户论坛]（http://forums.activiti.org/en/viewforum.php?f=3）上发布问题和评论，并在[我们的JIRA问题跟踪器]中创建问题（https://activiti.atlassian.net/）。  

### 1.6. 实验功能

 标有**[实验] **的章节不应被视为稳定。

包名中包含`.impl`的所有类都是内部实现类，不能被认为是稳定的。 但是，如果用户指南提到这些类作为配置值，则它们受支持并且可以被认为是稳定的。  

### 1.7. 内部实现类

在jar文件中，包含`.impl`。（例如`org.activiti.engine.impl.db`）的包中的所有类都是实现类，应该被视为内部类。 对实现类中的类或接口没有给出稳定性保证。

## 2. Getting Started

### 2.1. 一分钟版本

 从[Activiti网站]（http://www.activiti.org/）下载Activiti UI WAR文件后，请按照以下步骤使用默认设置运行演示设置。 你需要一个有用的[Java运行时]（http://java.sun.com/javase/downloads/index.jsp）和[Apache Tomcat]（http://tomcat.apache.org/download-80.cgi） ）安装（实际上，任何Web容器都可以工作，因为我们只依赖于servlet功能。但我们主要测试Tomcat）。

 \- 将下载的activiti-app.war复制到Tomcat的webapps目录。
 \- 通过在Tomcat的bin文件夹中运行startup.bat或startup.sh脚本来启动Tomcat
 \- 启动Tomcat时打开浏览器并转到<http：// localhost：8080 / activiti-app>。 使用管理员和密码测试登录。

而已！ Activiti UI应用程序默认使用内存中的H2数据库，如果要使用其他数据库配置，请阅读[较长版本]（https://www.activiti.org/userguide/#activiti.setup）。  

### 2.2. Activiti设置

要安装Activiti，您需要一个有效的[Java运行时]（http://java.sun.com/javase/downloads/index.jsp）和[Apache Tomcat]（http://tomcat.apache.org/download- 70.cgi）安装。 还要确保正确设置了* JAVA_HOME *系统变量。 这样做的方式取决于您的操作系统。

要使Activiti UI和REST Web应用程序运行，只需将从Activiti下载页面下载的WAR复制到Tomcat安装目录中的`webapps`文件夹。 默认情况下，UI应用程序使用内存数据库运行。

Demo user:

| UserId | Password | Security roles |
| ------ | -------- | -------------- |
| admin  | test     | admin          |

访问：

| Webapp Name | URL                                  | Description                                                  |
| ----------- | ------------------------------------ | ------------------------------------------------------------ |
| Activiti UI | <http://localhost:8080/activiti-app> | 流程引擎用户控制台。 使用此工具可以启动新流程，分配任务，查看和声明任务等。|

请注意，Activiti UI应用程序演示设置是一种尽可能快速且尽可能快地显示Activiti功能和功能的方法。 但是，这并不意味着它是使用Activiti的唯一方法。 由于Activiti只是一个jar *，它可以嵌入到任何Java环境中：使用swing或Tomcat，JBoss，WebSphere等。或者您可以选择将Activiti作为典型的独立BPM服务器运行。 如果有可能在Java中，可以使用Activiti！

### 2.3. Activiti数据库设置

正如在一分钟的演示设置中所述，Activiti UI应用程序默认运行内存中的H2数据库。 要使用独立H2或其他数据库运行Activiti UI应用程序，应更改Activiti UI Web应用程序的WEB-INF / classes / META-INF / activiti-app中的activiti-app.properties。

### 2.4. 引入Activiti jar 和依赖

要包含Activiti jar及其依赖库，我们建议使用[Maven]（http://maven.apache.org/）（或[Ivy]（http://ant.apache.org/ivy/））， 它简化了我们和您身边的依赖管理。 按照<http://www.activiti.org/community.html#maven.repository>中的说明在您的环境中包含必要的jar。

或者，如果您不想使用Maven，您可以自己在项目中包含jar。 Activiti下载zip包含一个文件夹`libs`，其中包含所有Activiti jar（和源jar）。 依赖项不是以这种方式发送的。 Activiti引擎所需的依赖项是（使用`mvn dependency：tree`生成）：

```
org.activiti:activiti-engine:jar:6.x
+- org.activiti:activiti-bpmn-converter:jar:6.x:compile
|  \- org.activiti:activiti-bpmn-model:jar:6.x:compile
|     +- com.fasterxml.jackson.core:jackson-core:jar:2.2.3:compile
|     \- com.fasterxml.jackson.core:jackson-databind:jar:2.2.3:compile
|        \- com.fasterxml.jackson.core:jackson-annotations:jar:2.2.3:compile
+- org.activiti:activiti-process-validation:jar:6.x:compile
+- org.activiti:activiti-image-generator:jar:6.x:compile
+- org.apache.commons:commons-email:jar:1.2:compile
|  +- javax.mail:mail:jar:1.4.1:compile
|  \- javax.activation:activation:jar:1.1:compile
+- org.apache.commons:commons-lang3:jar:3.3.2:compile
+- org.mybatis:mybatis:jar:3.3.0:compile
+- org.springframework:spring-beans:jar:4.1.6.RELEASE:compile
|  \- org.springframework:spring-core:jar:4.1.6.RELEASE:compile
+- joda-time:joda-time:jar:2.6:compile
+- org.slf4j:slf4j-api:jar:1.7.6:compile
+- org.slf4j:jcl-over-slf4j:jar:1.7.6:compile
```

Note: 只有在使用[邮件服务任务]（https://www.activiti.org/userguide/#bpmnEmailTask）时才需要email jar。

在[Activiti源代码]（https://github.com/Activiti/Activiti）的模块上使用`mvn dependency：copy-dependencies`可以轻松下载所有依赖项。

### 2.5. 下一步

使用[Activiti UI]（https://www.activiti.org/userguide/#activitiUI）Web应用程序是熟悉Activiti概念和功能的好方法。但是，Activiti的主要目的当然是在您自己的应用程序中启用强大的BPM和工作流功能。以下章节将帮助您熟悉如何在您的环境中以编程方式使用Activiti：

 -  [配置章节]（https://www.activiti.org/userguide/#configuration）将教你如何设置Activiti以及如何获取`ProcessEngine`类的实例，它是您的中心访问点Activiti的所有引擎功能。 * [API章节]（https://www.activiti.org/userguide/#chapterApi）将指导您完成Activiti API的服务。这些服务以方便而强大的方式提供Activiti引擎功能，可以在任何Java环境中使用。 *有兴趣深入了解BPMN 2.0，即Activiti引擎的流程编写格式？然后继续[BPMN 2.0部分]（https://www.activiti.org/userguide/#bpmn20）。

## 3. 配置

### 3.1. 创建ProcessEngine

Activiti流程引擎通过名为`activiti.cfg.xml`的XML文件进行配置。 请注意，如果您使用[构建流程引擎的Spring样式]（https://www.activiti.org/userguide/#springintegration），则**不适用**。

获取`ProcessEngine`的最简单方法是使用`org.activiti.engine.ProcessEngines`类：

```
ProcessEngine processEngine = ProcessEngines.getDefaultProcessEngine()
```

这将在类路径中查找`activiti.cfg.xml`文件，并根据该文件中的配置构造引擎。 以下代码段显示了示例配置。 以下部分将详细介绍配置属性。

```
 <beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

  <bean id="processEngineConfiguration" class="org.activiti.engine.impl.cfg.StandaloneProcessEngineConfiguration">

    <property name="jdbcUrl" value="jdbc:h2:mem:activiti;DB_CLOSE_DELAY=1000" />
    <property name="jdbcDriver" value="org.h2.Driver" />
    <property name="jdbcUsername" value="sa" />
    <property name="jdbcPassword" value="" />

    <property name="databaseSchemaUpdate" value="true" />

    <property name="asyncExecutorActivate" value="false" />

    <property name="mailServerHost" value="mail.my-corp.com" />
    <property name="mailServerPort" value="5025" />
  </bean>

</beans>
```

请注意，配置XML实际上是一个Spring配置。 **这并不意味着Activiti只能在Spring环境中使用！**我们只是在内部利用Spring的解析和依赖注入功能来构建引擎。

ProcessEngineConfiguration对象也可以使用配置文件以编程方式创建。 也可以使用不同的bean id（例如，参见第3行）。

```
ProcessEngineConfiguration.createProcessEngineConfigurationFromResourceDefault();
ProcessEngineConfiguration.createProcessEngineConfigurationFromResource(String resource);
ProcessEngineConfiguration.createProcessEngineConfigurationFromResource(String resource, String beanName);
ProcessEngineConfiguration.createProcessEngineConfigurationFromInputStream(InputStream inputStream);
ProcessEngineConfiguration.createProcessEngineConfigurationFromInputStream(InputStream inputStream, String beanName);
```

也可以不使用配置文件，并根据默认值创建配置（有关详细信息，请参阅[不同支持的类]（https://www.activiti.org/userguide/#configurationClasses））。
```
ProcessEngineConfiguration.createStandaloneProcessEngineConfiguration();
ProcessEngineConfiguration.createStandaloneInMemProcessEngineConfiguration();
```

所有这些`ProcessEngineConfiguration.createXXX（）`方法都返回一个`ProcessEngineConfiguration`，如果需要可以进一步调整。 在调用`buildProcessEngine（）`操作之后，创建一个`ProcessEngine`：

```
ProcessEngine processEngine = ProcessEngineConfiguration.createStandaloneInMemProcessEngineConfiguration()
  .setDatabaseSchemaUpdate(ProcessEngineConfiguration.DB_SCHEMA_UPDATE_FALSE)
  .setJdbcUrl("jdbc:h2:mem:my-own-db;DB_CLOSE_DELAY=1000")
  .setAsyncExecutorActivate(false)
  .buildProcessEngine();
```

### 3.2. ProcessEngineConfiguration bean

`activiti.cfg.xml`必须包含一个id为''processEngineConfiguration'`的bean。

```
<bean id="processEngineConfiguration" class="org.activiti.engine.impl.cfg.StandaloneProcessEngineConfiguration">
```

然后使用该bean构造`ProcessEngine`。有多个类可用于定义`processEngineConfiguration`。这些类表示不同的环境，并相应地设置默认值。选择匹配（最多）环境的类是最佳实践，以最小化配置引擎所需的属性数。目前可以使用以下类（将来的版本中将会有更多类）：

 -  ** org.activiti.engine.impl.cfg.StandaloneProcessEngineConfiguration **：流程引擎以独立方式使用。 Activiti将负责交易。默认情况下，仅在引擎引导时检查数据库（如果没有Activiti架构或架构版本不正确，则抛出异常）。
 -  ** org.activiti.engine.impl.cfg.StandaloneInMemProcessEngineConfiguration **：这是一个用于单元测试目的的便利类。 Activiti将负责交易。默认情况下使用H2内存数据库。引擎启动和关闭时将创建和删除数据库。使用此功能时，可能不需要其他配置（例如，使用作业执行程序或邮件功能时除外）。
 -  ** org.activiti.spring.SpringProcessEngineConfiguration **：在Spring环境中使用流程引擎时使用。有关详细信息，请参阅[Spring集成部分]（https://www.activiti.org/userguide/#springintegration）。
 -  ** org.activiti.engine.impl.cfg.JtaProcessEngineConfiguration **：在引擎以独立模式运行时使用JTA事务。

### 3.3. Database configuration

有两种方法可以配置Activiti引擎将使用的数据库。第一个选项是定义数据库的JDBC属性：

 -  ** jdbcUrl **：数据库的JDBC URL。
 -  ** jdbcDriver **：为特定数据库类型实现驱动程序。
 -  ** jdbcUsername **：用于连接数据库的用户名。
 -  ** jdbcPassword **：连接数据库的密码。

基于提供的JDBC属性构造的数据源将具有默认的[MyBatis]（http://www.mybatis.org/）连接池设置。可以选择设置以下属性来调整该连接池（取自MyBatis文档）：

 -  ** jdbcMaxActiveConnections **：任何时候连接池最多可以包含的活动连接数。默认值为10。
 -  ** jdbcMaxIdleConnections **：任何时候连接池最多可以包含的空闲连接数。
 -  ** jdbcMaxCheckoutTime **：在强制返回连接池之前，可以*从连接池中检出连接的时间量（以毫秒为单位）。默认值为20000（20秒）。
 -  ** jdbcMaxWaitTime **：这是一个低级别设置，它使池有机会打印日志状态并在异常长时间内重新尝试获取连接（以避免在以下情况下无声地失败）池配置错误）默认值为20000（20秒）。

示例数据库配置：

```
<property name="jdbcUrl" value="jdbc:h2:mem:activiti;DB_CLOSE_DELAY=1000" />
<property name="jdbcDriver" value="org.h2.Driver" />
<property name="jdbcUsername" value="sa" />
<property name="jdbcPassword" value="" />
```

我们的基准测试表明，在处理大量并发请求时，MyBatis连接池不是最有效或最有弹性的。 因此，建议我们使用`javax.sql.DataSource`实现并将其注入流程引擎配置（例如DBCP，C3P0，Hikari，Tomcat连接池等）：
```
<bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource" >
  <property name="driverClassName" value="com.mysql.jdbc.Driver" />
  <property name="url" value="jdbc:mysql://localhost:3306/activiti" />
  <property name="username" value="activiti" />
  <property name="password" value="activiti" />
  <property name="defaultAutoCommit" value="false" />
</bean>

<bean id="processEngineConfiguration" class="org.activiti.engine.impl.cfg.StandaloneProcessEngineConfiguration">

    <property name="dataSource" ref="dataSource" />
    ...
```

请注意，Activiti不附带允许定义此类数据源的库。所以你必须确保库在你的类路径上。

无论您使用的是JDBC还是数据源方法，都可以设置以下属性：

 -  ** databaseType **：通常不需要指定此属性，因为它是从数据库连接元数据中自动分析的。只应在自动检测失败的情况下指定。可能的值：{h2，mysql，oracle，postgres，mssql，db2}。此设置将确定将使用哪些创建/删除脚本和查询。有关支持哪些类型的概述，请参阅[支持的数据库*部分]（https://www.activiti.org/userguide/#supporteddatabases）。
 -  ** databaseSchemaUpdate **：允许设置策略以在流程引擎启动和关闭时处理数据库模式。
   - `false`（默认值）：在创建流程引擎时检查库模式的版本，如果版本不匹配则抛出异常。
   - `true`：在构建流程引擎时，执行检查并在必要时执行模式更新。如果架构不存在，则创建它。
   - `create-drop`：在创建流程引擎时创建模式，并在流程引擎关闭时删除模式。

### 3.4. JNDI Datasource Configuration

默认情况下，Activiti的数据库配置包含在每个Web应用程序的WEB-INF / classes中的db.properties文件中。 这并不总是理想的，因为它要求用户修改Activiti源中的db.properties并重新编译war文件，或者在每次部署时爆炸war并修改db.properties。

通过使用JNDI（Java命名和目录接口）获取数据库连接，连接完全由Servlet容器管理，并且可以在war部署之外管理配置。 与db.properties文件提供的连接参数相比，这还允许更多地控制连接参数。

#### 3.4.1. Configuration

JNDI数据源的配置将根据您使用的servlet容器应用程序而有所不同。 以下说明适用于Tomcat，但对于其他容器应用程序，请参阅容器应用程序的文档。

如果使用Tomcat，则在$ CATALINA_BASE / conf / [enginename] / [hostname] / [warname] .xml中配置JNDI资源（对于Activiti UI，这通常是$ CATALINA_BASE / conf / Catalina / localhost / activiti-app。XML）。 首次部署应用程序时，将从Activiti war文件中复制默认上下文，因此如果已存在，则需要替换它。 例如，要更改JNDI资源以便应用程序连接到MySQL而不是H2，请将文件更改为以下内容：

```
 <?xml version="1.0" encoding="UTF-8"?>
    <Context antiJARLocking="true" path="/activiti-app">
        <Resource auth="Container"
            name="jdbc/activitiDB"
            type="javax.sql.DataSource"
            description="JDBC DataSource"
            url="jdbc:mysql://localhost:3306/activiti"
            driverClassName="com.mysql.jdbc.Driver"
            username="sa"
            password=""
            defaultAutoCommit="false"
            initialSize="5"
            maxWait="5000"
            maxActive="120"
            maxIdle="5"/>
        </Context>
```

#### 3.4.2. JNDI properties

要配置JNDI数据源，请在Activiti UI的属性文件中使用以下属性：

 -  datasource.jndi.name：数据源的JNDI名称。
 -  datasource.jndi.resourceRef：设置查询是否发生在J2EE容器中，即如果JNDI名称尚未包含它，则需要添加前缀“java：comp / env /”。 默认为“true”。
### 3.5. 支持的 databases

下面列出了Activiti用于引用数据库的类型（区分大小写！）。

| Activiti数据库类型|示例JDBC URL |备注|
| ---------------------- | -------------------------------------------------- ---------- | -------------------------------------------------- ---------- |
| h2 | jdbc：h2：tcp：// localhost / activiti |默认配置数据库|
| mysql | jdbc：mysql：// localhost：3306 / activiti？autoReconnect = true |使用mysql-connector-java数据库驱动程序进行测试
| oracle | jdbc：oracle：thin：@localhost：1521：xe | |
| postgres | jdbc：postgresql：// localhost：5432 / activiti | |
| db2 | jdbc：db2：// localhost：50000 / activiti | |
| mssql | jdbc：sqlserver：// localhost：1433; databaseName = activiti（jdbc.driver = com.microsoft.sqlserver.jdbc.SQLServerDriver）* OR * jdbc：jtds：sqlserver：// localhost：1433 / activiti（jdbc.driver = net .sourceforge.jtds.jdbc.Driver）|使用Microsoft JDBC Driver 4.0（sqljdbc4.jar）和JTDS Driver |进行测试

### 3.6. Creating the database tables

为数据库创建数据库表的最简单方法是：

 - 将activiti-engine jar添加到类路径中
 - 添加合适的数据库驱动程序
 - 在您的类路径中添加一个Activiti配置文件（* activiti.cfg.xml *），指向您的数据库（参见[数据库配置部分]（https://www.activiti.org/userguide/#databaseConfiguration））
 - 执行* DbSchemaCreate *类的main方法

但是，通常只有数据库管理员才能在数据库上执行DDL语句。 在生产系统中，这也是最明智的选择。 SQL DDL语句可以在Activiti下载页面上或Activiti分发文件夹中的`database`subdirectory中找到。 脚本也位于引擎jar（* activiti-engine-x.jar *）中，包* org / activiti / db / create *（* drop *文件夹包含drop语句）。 SQL文件的形式

```
activiti.{db}.{create|drop}.{type}.sql
```

其中* db *是[支持的数据库]（https://www.activiti.org/userguide/#supporteddatabases）和* type *中的任何一个

- **引擎：**引擎执行所需的表。需要。
- ** identity：**包含用户，组和用户到组的成员的表。这些表是可选的，应在使用引擎附带的默认身份管理时使用。
- **历史记录：**包含历史记录和审计信息的表格。可选：历史级别设置为* none *时不需要。请注意，这还将禁用将数据存储在历史数据库中的某些功能（例如对任务进行注释）。

** MySQL用户注意事项：**低于5.6.4的MySQL版本不支持精确到毫秒级的时间戳或日期。更糟糕的是，某些版本在尝试创建此类列时会抛出异常但其他版本则不会。在进行自动创建/升级时，引擎会在执行时更改DDL。使用DDL文件方法时，常规版本和带有* mysql55 *的特殊文件都可用（这适用于低于5.6.4的任何内容）。后一个文件将具有不具有毫秒精度的列类型。

具体地说，以下适用于MySQL版本

- ** <5.6：**无毫秒精度。 DDL文件可用（查找包含* mysql55 *的文件）。自动创建/更新将开箱即用。
- ** 5.6.0 - 5.6.3：**无毫秒精度。自动创建/更新将不起作用。建议无论如何都要升级到更新的数据库版本。如果确实需要，可以使用* mysql 5.5 *的DDL文件。
- ** 5.6.4 +：**毫秒精度可用。 DDL文件可用（默认文件包含* mysql *）。自动创建/更新开箱即用。

请注意，如果稍后升级MySQL数据库并且已经创建/升级了Activiti表，则必须手动完成列类型更改！

### 3.7. Database table names explained

Activiti的数据库名称都以** ACT _ **开头。第二部分是表的用例的双字符标识。此用例也将大致匹配服务API。

- ** ACT_RE _ ***：* RE *代表`repository`。具有此前缀的表包含* static *信息，例如流程定义和流程资源（图像，规则等）。
- ** ACT_RU _ ***：* RU *代表`runtime`。这些是包含流程实例，用户任务，变量，作业等的运行时数据的运行时表.Activiti仅在流程实例执行期间存储运行时数据，并在流程实例结束时删除记录。这使运行时表保持小而快。
- ** ACT_ID _ ***：* ID *代表`identity`。这些表包含身份信息，例如用户，组等。
- ** ACT_HI _ ***：* HI *代表`history`。这些是包含历史数据的表，例如过去的流程实例，变量，任务等。
- ** ACT_GE _ ***：`general`数据，用于各种用例。

### 3.8. Database upgrade

在运行升级之前，请确保备份数据库（使用数据库备份功能）。

默认情况下，每次创建流程引擎时都会执行版本检查。 这通常在您的应用程序或Activiti Web应用程序的启动时发生一次。 如果Activiti库注意到库版本与Activiti数据库表的版本之间存在差异，则会引发异常。

要进行升级，必须首先将以下配置属性放在activiti.cfg.xml配置文件中：

```
<beans >

  <bean id="processEngineConfiguration" class="org.activiti.engine.impl.cfg.StandaloneProcessEngineConfiguration">
    <!-- ... -->
    <property name="databaseSchemaUpdate" value="true" />
    <!-- ... -->
  </bean>

</beans>
```

**另外，在数据路径中包含适用于数据库的数据库驱动程序。**升级应用程序中的Activiti库。 或者启动新版本的Activiti并将其指向包含旧版本的数据库。 将`databaseSchemaUpdate`设置为`true`时，Activiti会在第一次注意到库和数据库模式不同步时自动将数据库模式升级到新版本。

**作为替代方案，您还可以运行升级DDL语句。**还可以运行Activiti下载页面上提供的升级数据库脚本。

### 3.9. Job Executor (since version 6.0.0)

Activiti 5的异步执行程序是Activiti 6中唯一可用的作业执行程序，因为它是一种在Activiti Engine中执行异步作业的性能更高且更加数据库友好的方式。 Activiti 5的旧作业执行者被删除。 可以在用户指南的高级部分中找到更多信息。

此外，如果在Java EE 7下运行，可以使用符合JSR-236的`ManagedAsyncJobExecutor`来让容器管理线程。 为了启用它们，应该在配置中传递线程工厂，如下所示：

```
<bean id="threadFactory" class="org.springframework.jndi.JndiObjectFactoryBean">
   <property name="jndiName" value="java:jboss/ee/concurrency/factory/default" />
</bean>

<bean id="customJobExecutor" class="org.activiti.engine.impl.jobexecutor.ManagedAsyncJobExecutor">
   <!-- ... -->
   <property name="threadFactory" ref="threadFactory" />
   <!-- ... -->
</bean>
```

如果未指定线程工厂，则托管实现将回退到其默认对应项。

### 3.10. Job executor activation

`AsyncExecutor`是一个管理线程池以触发定时器和其他异步任务的组件。 其他实现也是可能的（例如，使用消息队列，请参阅用户指南的高级部分）。

默认情况下，“AsyncExecutor”未激活且未启动。 通过以下配置，可以与Activiti Engine一起启动异步执行程序。

```
<property name =“asyncExecutorActivate”value =“true”/>
```

属性asyncExecutorActivate指示Activiti Engine在启动时启动Async执行程序。

### 3.11. Mail server configuration

配置邮件服务器是可选的。 Activiti支持在业务流程中发送电子邮件。 要实际发送电子邮件，需要有效的SMTP邮件服务器配置。 有关配置选项，请参阅[电子邮件任务]（https://www.activiti.org/userguide/#bpmnEmailTaskServerConfiguration）。

### 3.12. History configuration

自定义历史存储的配置是可选的。 这允许您调整影响引擎的[历史记录功能]（https://www.activiti.org/userguide/#history）的设置。 有关详细信息，请参阅[历史记录配置]（https://www.activiti.org/userguide/#historyConfig）。

```
<property name="history" value="audit" />
```

### 3.13. Exposing configuration beans in expressions and scripts

默认情况下，您在`activiti.cfg.xml`配置或您自己的Spring配置文件中指定的所有bean都可用于表达式和脚本。 如果要限制配置文件中bean的可见性，可以在流程引擎配置中配置名为`beans`的属性。 `ProcessEngineConfiguration`中的beans属性是一个映射。 指定该属性时，表达式和脚本只能看到该映射中指定的bean。 暴露的bean将使用您在该映射中指定的名称公开。

### 3.14. Deployment cache configuration

所有流程定义都被缓存（在解析之后），以避免每次需要流程定义时都会访问数据库，并且流程定义数据不会更改。 默认情况下，此缓存没有限制。 要限制流程定义缓存，请添加以下属性

```
<property name="processDefinitionCacheLimit" value="10" />
```

设置此属性将使默认哈希映射缓存与具有提供的硬限制的LRU缓存交换。 当然，此属性的* best *值取决于所存储的流程定义的总量以及所有运行时流程实例在运行时实际使用的流程定义的数量。

您也可以注入自己的缓存实现。 这必须是实现org.activiti.engine.impl.persistence.deploy.DeploymentCache接口的bean：

```
<property name="processDefinitionCache">
  <bean class="org.activiti.MyCache" />
</property>
```

有一个名为`knowledgeBaseCacheLimit`和`knowledgeBaseCache`的类似属性用于配置规则缓存。 只有在流程中使用规则任务时才需要这样做。

### 3.15. Logging

所有日志记录（activiti，spring，mybatis，...）都通过SLF4J进行路由，并允许选择您选择的日志记录实现。

**默认情况下，在活动引擎依赖项中不存在SFL4J绑定jar，这应该在项目中添加，以便使用您选择的日志框架。**如果没有添加实现jar，SLF4J将使用NOP -logger，不记录任何内容，除了警告不会记录任何内容。 有关这些绑定的更多信息，请访问<http://www.slf4j.org/codes.html#StaticLoggerBinder>。

使用Maven，添加例如这样的依赖（这里使用log4j），请注意您仍然需要添加一个版本：

```
<dependency>
  <groupId>org.slf4j</groupId>
  <artifactId>slf4j-log4j12</artifactId>
</dependency>
```

activiti-ui和activiti-rest webapps配置为使用Log4j绑定。在运行所有activiti- *模块的测试时也使用Log4j。
**在类路径中使用带有commons-logging的容器时的重要注意事项：**为了通过SLF4J路由弹簧日志，使用了一个桥（参见<http://www.slf4j.org/legacy.html#jclOverSLF4J>）。 如果您的容器提供了公共日志记录实现，请按照此页面上的说明进行操作：<http://www.slf4j.org/codes.html#release>以确保稳定性。

使用Maven时的示例（省略版本）：

```
<dependency>
  <groupId>org.slf4j</groupId>
  <artifactId>jcl-over-slf4j</artifactId>
</dependency>
```

### 3.16. Mapped Diagnostic Contexts

Activiti支持SLF4j的映射诊断上下文功能。 这些基本信息将与将要记录的内容一起传递给基础记录器：

 -  processDefinition Id为mdcProcessDefinitionID
 -  processInstance Id为mdcProcessInstanceID
 - 执行ID为mdcExecutionId

默认情况下，不会记录这些信息。 可以将记录器配置为以所需格式显示它们，这是常用记录消息的补充。 例如，在Log4j中，以下示例布局定义使记录器显示上述信息：
```
log4j.appender.consoleAppender.layout.ConversionPattern=ProcessDefinitionId=%X{mdcProcessDefinitionID}
executionId=%X{mdcExecutionId} mdcProcessInstanceID=%X{mdcProcessInstanceID} mdcBusinessKey=%X{mdcBusinessKey} %m%n
```

当日志包含需要通过例如日志分析器实时检查的信息时，这非常有用。

### 3.17. Event handlers

Activiti引擎中的事件机制允许您在引擎内发生各种事件时收到通知。查看[所有支持的事件类型]（https://www.activiti.org/userguide/#eventDispatcherEventTypes），了解可用事件的概述。

可以为某些类型的事件注册侦听器，而不是在调度任何类型的事件时收到通知。您可以[通过配置]添加引擎范围的事件侦听器（https://www.activiti.org/userguide/#eventDispatcherConfiguration），在运行时使用API​​添加引擎范围的事件侦听器（https：// www .activiti.org / userguide / #eventDispatcherConfigurationRuntime）或将事件监听器添加到[BPMN XML中的特定流程定义]（https://www.activiti.org/userguide/#eventDispatcherConfigurationProcessDefinition）。

调度的所有事件都是`org.activiti.engine.delegate.event.ActivitiEvent`的子类型。该事件公开（如果可用）`type`，`executionId`，`processInstanceId`和`processDefinitionId`。某些事件包含与发生的事件相关的其他上下文，有关其他有效负载的其他信息可以在[所有支持的事件类型]列表中找到（https://www.activiti.org/userguide/#eventDispatcherEventTypes）。

#### 3.17.1. Event listener implementation

事件监听器的唯一要求是实现`org.activiti.engine.delegate.event.ActivitiEventListener`。 下面是一个侦听器的示例实现，它将收到的所有事件输出到标准输出，但与作业执行相关的事件除外：

```
public class MyEventListener implements ActivitiEventListener {

  @Override
  public void onEvent(ActivitiEvent event) {
    switch (event.getType()) {

      case JOB_EXECUTION_SUCCESS:
        System.out.println("A job well done!");
        break;

      case JOB_EXECUTION_FAILURE:
        System.out.println("A job has failed...");
        break;

      default:
        System.out.println("Event received: " + event.getType());
    }
  }

  @Override
  public boolean isFailOnException() {
    // The logic in the onEvent method of this listener is not critical, exceptions
    // can be ignored if logging fails...
    return false;
  }
}
```

`isFailOnException（）`方法确定`onEvent（..）`方法在调度事件时抛出异常的行为。如果返回`false`，则忽略该异常。当返回“true”时，不会忽略异常并且冒泡，有效地使当前正在进行的命令失败。如果事件是API调用（或任何其他事务操作，例如作业执行）的一部分，则事务将被回滚。如果事件监听器中的行为不是业务关键型，则建议返回“false”。

Activiti提供了一些基本实现来促进事件监听器的常见用例。这些可以用作基类或示例监听器实现：

- ** org.activiti.engine.delegate.event.BaseEntityEventListener **：一个事件监听器基类，可用于监听特定类型的实体或所有实体的实体相关事件。它隐藏了类型检查并提供了4个应该被覆盖的方法：`onCreate（..）`，`onUpdate（..）`和`onDelete（..）`当创建，更新或删除实体时。对于所有其他与实体相关的事件，调用`onEntityEvent（..）`。

#### 3.17.2. Configuration and setup

如果在流程引擎配置中配置了事件侦听器，则它将在流程引擎启动时处于活动状态，并在后续重新引导引擎后保持活动状态。

属性`eventListeners`需要一个`org.activiti.engine.delegate.event.ActivitiEventListener`实例的列表。 像往常一样，您可以声明内联bean定义或使用`ref`来代替现有bean。 下面的代码段会向调度任何事件时通知的配置添加事件侦听器，而不管其类型如何：

```
<bean id="processEngineConfiguration" class="org.activiti.engine.impl.cfg.StandaloneProcessEngineConfiguration">
    ...
    <property name="eventListeners">
      <list>
         <bean class="org.activiti.engine.example.MyEventListener" />
      </list>
    </property>
</bean>
```

要在调度某些类型的事件时收到通知，请使用`typedEventListeners`属性，该属性需要一个映射。 map-entry的键是以逗号分隔的事件名称列表（或单个事件名称）。 map-entry的值是`org.activiti.engine.delegate.event.ActivitiEventListener`实例的列表。 下面的代码段为配置添加了一个事件监听器，在作业执行成功或失败时通知：

```
<bean id="processEngineConfiguration" class="org.activiti.engine.impl.cfg.StandaloneProcessEngineConfiguration">
    ...
    <property name="typedEventListeners">
      <map>
        <entry key="JOB_EXECUTION_SUCCESS,JOB_EXECUTION_FAILURE" >
          <list>
            <bean class="org.activiti.engine.example.MyJobEventListener" />
          </list>
        </entry>
      </map>
    </property>
</bean>
```

调度事件的顺序取决于添加侦听器的顺序。 首先，按照`list`中定义的顺序调用所有正常的事件监听器（`eventListeners`属性）。 之后，如果调度了正确类型的事件，则调用所有类型化事件侦听器（`typedEventListeners`properties）。

#### 3.17.3. Adding listeners at runtime

可以使用API（“RuntimeService”）向引擎添加和删除其他事件侦听器：

```
/**
 * Adds an event-listener which will be notified of ALL events by the dispatcher.
 * @param listenerToAdd the listener to add
 */
void addEventListener(ActivitiEventListener listenerToAdd);

/**
 * Adds an event-listener which will only be notified when an event occurs, which type is in the given types.
 * @param listenerToAdd the listener to add
 * @param types types of events the listener should be notified for
 */
void addEventListener(ActivitiEventListener listenerToAdd, ActivitiEventType... types);

/**
 * Removes the given listener from this dispatcher. The listener will no longer be notified,
 * regardless of the type(s) it was registered for in the first place.
 * @param listenerToRemove listener to remove
 */
 void removeEventListener(ActivitiEventListener listenerToRemove);
```

请注意，重新启动引擎时，不会保留在运行时添加的侦听器**。**

#### 3.17.4. Adding listeners to process definitions

可以将侦听器添加到特定的流程定义中。 只会调用与流程定义相关的事件以及与使用该特定流程定义启动的流程实例相关的所有事件的侦听器。 可以使用完全限定的类名来定义侦听器实现，该表达式解析为实现侦听器接口的bean，或者可以配置为抛出消息/信号/错误BPMN事件。

##### Listeners executing user-defined logic

下面的代码片段为流程定义添加了2个侦听器。 第一个侦听器将接收任何类型的事件，并使用基于完全限定类名的侦听器实现。 第二个侦听器仅在作业成功执行或失败时通知，使用已在流程引擎配置的`beans`属性中定义的侦听器。

```
<process id="testEventListeners">
  <extensionElements>
    <activiti:eventListener class="org.activiti.engine.test.MyEventListener" />
    <activiti:eventListener delegateExpression="${testEventListener}" events="JOB_EXECUTION_SUCCESS,JOB_EXECUTION_FAILURE" />
  </extensionElements>

  ...

</process>
```

对于与实体相关的事件，还可以将侦听器添加到流程定义中，该流程定义仅在特定实体类型发生实体事件时才会得到通知。 下面的代码段显示了如何实现这一目标。 它可以用于所有实体事件（第一个示例）或仅用于特定事件类型（第二个示例）。

```
<process id="testEventListeners">
  <extensionElements>
    <activiti:eventListener class="org.activiti.engine.test.MyEventListener" entityType="task" />
    <activiti:eventListener delegateExpression="${testEventListener}" events="ENTITY_CREATED" entityType="task" />
  </extensionElements>

  ...

</process>
```

`entityType`的支持值是：`attachment`，`comment`，`execution`，`identity-link`，`job`，`process-instance`，`process-definition`，`task`。

#####听众抛出BPMN事件

[实验\]]（https://www.activiti.org/userguide/#experimental）

处理被调度事件的另一种方法是抛出BPMN事件。 请记住，仅使用某些类型的Activiti事件类型抛出BPMN事件才有意义。 例如，删除流程实例时抛出BPMN事件将导致错误。 下面的代码片段展示了如何在进程实例中抛出信号，向外部进程（全局）抛出信号，在进程实例中抛出消息事件并在进程实例中抛出错误事件。 不使用`class`或`delegateExpression`，而是使用属性`throwEvent`以及一个特定于被抛出事件类型的附加属性。

```
<process id="testEventListeners">
  <extensionElements>
    <activiti:eventListener throwEvent="signal" signalName="My signal" events="TASK_ASSIGNED" />
  </extensionElements>
</process>
<process id="testEventListeners">
  <extensionElements>
    <activiti:eventListener throwEvent="globalSignal" signalName="My signal" events="TASK_ASSIGNED" />
  </extensionElements>
</process>
<process id="testEventListeners">
  <extensionElements>
    <activiti:eventListener throwEvent="message" messageName="My message" events="TASK_ASSIGNED" />
  </extensionElements>
</process>
<process id="testEventListeners">
  <extensionElements>
    <activiti:eventListener throwEvent="error" errorCode="123" events="TASK_ASSIGNED" />
  </extensionElements>
</process>
```

如果需要其他逻辑来决定是否抛出BPMN事件，则可以扩展Activiti提供的侦听器类。通过覆盖子类中的`isValidEvent（ActivitiEvent事件），可以防止BPMN事件抛出。涉及的类是+ org.activiti.engine.test.api.event.SignalThrowingEventListenerTest`，`org.activiti.engine.impl.bpmn.helper.MessageThrowingEventListener`和`org.activiti.engine.impl.bpmn.helper.ErrorThrowingEventListener `。

#####关于流程定义的侦听器的注释

- 事件监听器只能在`process`元素上声明，作为`extensionElements`的子元素。无法在流程中的个别活动中定义监听器。
- `delegateExpression`中使用的表达式无法访问执行上下文，因为其他表达式（例如在网关中）具有。它们只能引用在流程引擎配置的`beans`属性中定义的bean，或者在实现侦听器接口的任何spring-bean中使用spring（并且缺少beans属性）时。
- 当使用侦听器的`class`属性时，只会创建该类的单个实例。确保侦听器实现不依赖于成员字段或确保从多个线程/上下文安全使用。
- 当在`events`属性中使用非法事件类型或使用非法`throwEvent`值时，在部署流程定义时会引发异常（有效地使部署失败）。当提供`class`或`delegateExecution`的非法值（不存在的类，不存在的bean引用或不实现侦听器接口的委托）时，将在启动进程时（或第一个有效事件时）抛出异常为该进程定义被分派给监听器）。确保引用的类位于类路径中，并且表达式解析为有效的实例。

#### 3.17.5. Dispatching events through API

我们通过API打开了事件派发机制，允许您将自定义事件分派给在引擎中注册的任何侦听器。 建议（虽然没有强制执行）只调度类型为“CUSTOM”的`ActivitiEvents`。 可以使用`RuntimeService`来调度事件：

```
/**
 * Dispatches the given event to any listeners that are registered.
 * @param event event to dispatch.
 *
 * @throws ActivitiException if an exception occurs when dispatching the event or when the {@link ActivitiEventDispatcher}
 * is disabled.
 * @throws ActivitiIllegalArgumentException when the given event is not suitable for dispatching.
 */
 void dispatchEvent(ActivitiEvent event);
```

#### 3.17.6. Supported event types

下面列出了引擎中可能出现的所有事件类型。 每种类型对应于`org.activiti.engine.delegate.event.ActivitiEventType`中的枚举值。

| Event name                | Description                                                  | Event classes                                                |
| ------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| ENGINE_CREATED            | The process-engine this listener is attached to, has been created and is ready for API-calls. | `org.activiti…ActivitiEvent`                                 |
| ENGINE_CLOSED             | The process-engine this listener is attached to, has been closed. API-calls to the engine are no longer possible. | `org.activiti…ActivitiEvent`                                 |
| ENTITY_CREATED            | A new entity is created. The new entity is contained in the event. | `org.activiti…ActivitiEntityEvent`                           |
| ENTITY_INITIALIZED        | A new entity has been created and is fully initialized. If any children are created as part of the creation of an entity, this event will be fired AFTER the create/initialisation of the child entities as opposed to the `ENTITY_CREATE`event. | `org.activiti…ActivitiEntityEvent`                           |
| ENTITY_UPDATED            | An existing is updated. The updated entity is contained in the event. | `org.activiti…ActivitiEntityEvent`                           |
| ENTITY_DELETED            | An existing entity is deleted. The deleted entity is contained in the event. | `org.activiti…ActivitiEntityEvent`                           |
| ENTITY_SUSPENDED          | An existing entity is suspended. The suspended entity is contained in the event. Will be dispatched for ProcessDefinitions, ProcessInstances and Tasks. | `org.activiti…ActivitiEntityEvent`                           |
| ENTITY_ACTIVATED          | An existing entity is activated. The activated entity is contained in the event. Will be dispatched for ProcessDefinitions, ProcessInstances and Tasks. | `org.activiti…ActivitiEntityEvent`                           |
| JOB_EXECUTION_SUCCESS     | A job has been executed successfully. The event contains the job that was executed. | `org.activiti…ActivitiEntityEvent`                           |
| JOB_EXECUTION_FAILURE     | The execution of a job has failed. The event contains the job that was executed and the exception. | `org.activiti…ActivitiEntityEvent` and `org.activiti…ActivitiExceptionEvent` |
| JOB_RETRIES_DECREMENTED   | The number of job retries have been decremented due to a failed job. The event contains the job that was updated. | `org.activiti…ActivitiEntityEvent`                           |
| TIMER_FIRED               | A timer has been fired. The event contains the job that was executed? | `org.activiti…ActivitiEntityEvent`                           |
| JOB_CANCELED              | A job has been canceled. The event contains the job that was canceled. Job can be canceled by API call, task was completed and associated boundary timer was canceled, on the new process definition deployment. | `org.activiti…ActivitiEntityEvent`                           |
| ACTIVITY_STARTED          | An activity is starting to execute                           | `org.activiti…ActivitiActivityEvent`                         |
| ACTIVITY_COMPLETED        | An activity is completed successfully                        | `org.activiti…ActivitiActivityEvent`                         |
| ACTIVITY_CANCELLED        | An activity is going to be cancelled. There can be three reasons for activity cancellation (MessageEventSubscriptionEntity, SignalEventSubscriptionEntity, TimerEntity). | `org.activiti…ActivitiActivityCancelledEvent`                |
| ACTIVITY_SIGNALED         | An activity received a signal                                | `org.activiti…ActivitiSignalEvent`                           |
| ACTIVITY_MESSAGE_RECEIVED | An activity received a message. Dispatched before the activity receives the message. When received, a `ACTIVITY_SIGNAL` or `ACTIVITY_STARTED` will be dispatched for this activity, depending on the type (boundary-event or event-subprocess start-event) | `org.activiti…ActivitiMessageEvent`                          |
| ACTIVITY_ERROR_RECEIVED   | An activity has received an error event. Dispatched before the actual error has been handled by the activity. The event’s `activityId` contains a reference to the error-handling activity. This event will be either followed by a `ACTIVITY_SIGNALLED` event or `ACTIVITY_COMPLETE` for the involved activity, if the error was delivered successfully. | `org.activiti…ActivitiErrorEvent`                            |
| UNCAUGHT_BPMN_ERROR       | An uncaught BPMN error has been thrown. The process did not have any handlers for that specific error. The event’s `activityId` will be empty. | `org.activiti…ActivitiErrorEvent`                            |
| ACTIVITY_COMPENSATE       | An activity is about to be compensated. The event contains the id of the activity that is will be executed for compensation. | `org.activiti…ActivitiActivityEvent`                         |
| VARIABLE_CREATED          | A variable has been created. The event contains the variable name, value and related execution and task (if any). | `org.activiti…ActivitiVariableEvent`                         |
| VARIABLE_UPDATED          | An existing variable has been updated. The event contains the variable name, updated value and related execution and task (if any). | `org.activiti…ActivitiVariableEvent`                         |
| VARIABLE_DELETED          | An existing variable has been deleted. The event contains the variable name, last known value and related execution and task (if any). | `org.activiti…ActivitiVariableEvent`                         |
| TASK_ASSIGNED             | A task has been assigned to a user. The event contains the task | `org.activiti…ActivitiEntityEvent`                           |
| TASK_CREATED              | A task has been created. This is dispatched after the `ENTITY_CREATE` event. In case the task is part of a process, this event will be fired before the task listeners are executed. | `org.activiti…ActivitiEntityEvent`                           |
| TASK_COMPLETED            | A task has been completed. This is dispatched before the `ENTITY_DELETE` event. In case the task is part of a process, this event will be fired before the process has moved on and will be followed by a `ACTIVITY_COMPLETE` event, targeting the activity that represents the completed task. | `org.activiti…ActivitiEntityEvent`                           |
| PROCESS_COMPLETED         | A process has been completed. Dispatched after the last activity `ACTIVITY_COMPLETED`event. Process is completed when it reaches state in which process instance does not have any transition to take. | `org.activiti…ActivitiEntityEvent`                           |
| PROCESS_CANCELLED         | A process has been cancelled. Dispatched before the process instance is deleted from runtime. Process instance is cancelled by API call `RuntimeService.deleteProcessInstance` | `org.activiti…ActivitiCancelledEvent`                        |
| MEMBERSHIP_CREATED        | A user has been added to a group. The event contains the ids of the user and group involved. | `org.activiti…ActivitiMembershipEvent`                       |
| MEMBERSHIP_DELETED        | A user has been removed from a group. The event contains the ids of the user and group involved. | `org.activiti…ActivitiMembershipEvent`                       |
| MEMBERSHIPS_DELETED       | All members will be removed from a group. The event is thrown before the members are removed, so they are still accessible. No individual `MEMBERSHIP_DELETED` events will be thrown if all members are deleted at once, for performance reasons. | `org.activiti…ActivitiMembershipEvent`                       |

所有`ENTITY _ \ *`事件都与引擎内的实体相关。 下面的列表显示了为哪些实体分派了哪些实体事件：

 -  ** ENTITY_CREATED，ENTITY_INITIALIZED，ENTITY_DELETED **：附件，评论，部署，执行，组，IdentityLink，作业，模型，ProcessDefinition，ProcessInstance，任务，用户。
 -  ** ENTITY_UPDATED **：附件，部署，执行，组，IdentityLink，作业，模型，ProcessDefinition，ProcessInstance，任务，用户。
 -  ** ENTITY_SUSPENDED，ENTITY_ACTIVATED **：ProcessDefinition，ProcessInstance / Execution，Task。

#### 3.17.7. Additional remarks

**在发动机中只通知侦听器调度事件。**因此，如果您有不同的引擎 - 针对同一数据库运行 - 仅发起侦听器所注册的引擎中发生的事件，则调度到该侦听器。 不会将其他引擎中发生的事件分派给侦听器，无论它们是否在同一JVM中运行。

某些事件类型（与实体相关）公开目标实体。 根据类型或事件，这些实体不再被更新（例如，当删除实体时）。 如果可能，使用事件公开的`EngineServices`以安全的方式与引擎进行交互。 即便如此，您还需要对调度事件中涉及的实体的更新/操作保持谨慎。

没有调度与历史相关的实体事件，因为它们都具有调度事件的运行时对应事件。

## 4. The Activiti API

### 4.1. The Process Engine API and services

引擎API是与Activiti交互的最常用方式。 中心起点是`ProcessEngine`，可以通过[配置部分]（https://www.activiti.org/userguide/#configuration）中描述的几种方式创建。 从ProcessEngine，您可以获得包含工作流/ BPM方法的各种服务。 ProcessEngine和服务对象是线程安全的。 因此，您可以为整个服务器保留对其中一个的引用。

![api.services](https://www.activiti.org/userguide/images/api.services.png)

```
ProcessEngine processEngine = ProcessEngines.getDefaultProcessEngine();

RuntimeService runtimeService = processEngine.getRuntimeService();
RepositoryService repositoryService = processEngine.getRepositoryService();
TaskService taskService = processEngine.getTaskService();
ManagementService managementService = processEngine.getManagementService();
IdentityService identityService = processEngine.getIdentityService();
HistoryService historyService = processEngine.getHistoryService();
FormService formService = processEngine.getFormService();
DynamicBpmnService dynamicBpmnService = processEngine.getDynamicBpmnService();
```

`ProcessEngines.getDefaultProcessEngine（）`将在第一次调用时初始化并构建流程引擎，之后始终返回相同的流程引擎。使用`ProcessEngines.init（）`和`ProcessEngines.destroy（）`可以正确创建和关闭所有流程引擎。

ProcessEngines类将扫描所有`activiti.cfg.xml`和`activiti-context.xml`文件。对于所有`activiti.cfg.xml`文件，流程引擎将以典型的Activiti方式构建：`ProcessEngineConfiguration.createProcessEngineConfigurationFromInputStream（inputStream）.buildProcessEngine（）`。对于所有`activiti-context.xml`文件，流程引擎将以Spring方式构建：首先创建Spring应用程序上下文，然后从该应用程序上下文获取流程引擎。

所有服务都是无国籍的。这意味着您可以轻松地在群集中的多个节点上运行Activiti，每个节点都转到同一个数据库，而不必担心哪台计算机实际执行了以前的调用。任何对任何服务的调用都是幂等的，无论它在何处执行。

** RepositoryService **可能是使用Activiti引擎时所需的第一个服务。该服务提供管理和操作“部署”和“流程定义”的操作。这里没有详细介绍，流程定义是BPMN 2.0流程的Java对应物。它表示过程的每个步骤的结构和行为。 “部署”是Activiti引擎中的包装单位。部署可以包含多个BPMN 2.0 xml文件和任何其他资源。选择一个部署中包含的内容取决于开发人员。它可以从单个流程BPMN 2.0 xml文件到整个流程包和相关资源（例如，部署* hr-processes *可以包含与hr流程相关的所有内容）。 `RepositoryService`允许`部署'这样的包。部署部署意味着将其上载到引擎，在引擎中，所有进程在存储到数据库之前都会被检查和解析。从那时起，系统就知道部署，现在可以启动部署中包含的任何进程。

此外，这项服务允许

- 查询引擎已知的部署和流程定义。
- 暂停和激活整个部署或特定流程定义。暂停意味着不能对它们进行进一步的操作，而激活则是相反的操作。
- 检索各种资源，例如部署中包含的文件或引擎自动生成的流程图。
- 检索流程定义的POJO版本，该版本可用于使用Java而不是xml对流程进行内省。

虽然`RepositoryService`更像是静态信息（即数据没有改变，或者至少不是很多），但** RuntimeService **恰恰相反。它涉及启动流程定义的新流程实例。如上所述，“流程定义”定义了流程中不同步骤的结构和行为。流程实例是这种流程定义的一次执行。对于每个流程定义，通常会有许多实例同时运行。 “RuntimeService”也是用于检索和存储“过程变量”的服务。这是特定于给定流程实例的数据，并且可以由流程中的各种构造使用（例如，专用网关通常使用流程变量来确定选择哪个路径来继续该流程）。 `Runtimeservice`还允许查询流程实例和执行。执行是BPMN 2.0的“令牌”概念的表示。基本上，执行是指向流程实例当前所在位置的指针。最后，只要流程实例等待外部触发器并且需要继续该过程，就会使用`RuntimeService`。流程实例可以具有各种“等待状态”，并且该服务包含各种操作以*信号*接收外部触发器的实例并且可以继续流程实例。

需要由系统的实际人类用户执行的任务是BPM引擎（如Activiti）的核心。围绕任务的所有内容都分组在** TaskService **中，例如

- 查询分配给用户或组的任务
- 创建新的*独立*任务。这些是与流程实例无关的任务。
- 操作分配任务的用户或以某种方式参与任务的用户。
- 声明并完成任务。声明意味着某人决定成为该任务的受让人，这意味着该用户将完成该任务。完成意味着*完成任务的工作*。通常，这是填写各种形式。

** IdentityService **非常简单。它允许组和用户的管理（创建，更新，删除，查询......）。这很重要


### 4.2. Exception strategy

The base exception in Activiti is the `org.activiti.engine.ActivitiException`, an unchecked exception. This exception can be thrown at all times by the API, but *expected* exceptions that happen in specific methods are documented in the [the javadocs](http://activiti.org/javadocs/index.html). For example, an extract from `TaskService`:

```
/**
 * Called when the task is successfully executed.
 * @param taskId the id of the task to complete, cannot be null.
 * @throws ActivitiObjectNotFoundException when no task exists with the given id.
 */
 void complete(String taskId);
```

在上面的示例中，当传递不存在任务的id时，将引发异常。此外，由于javadoc **明确声明taskId不能为null，因此在传递null时将抛出ActivitiIllegalArgumentException。

即使我们想要避免一个大的异常层次结构，也会添加以下子类，这些子类在特定情况下会被抛出。在流程执行或API调用期间发生的所有其他错误都不适合下面可能的异常，它们将作为常规`ActivitiExceptions`抛出。

- `ActivitiWrongDbException`：当Activiti引擎发现数据库模式版本和引擎版本不匹配时抛出。
- `ActivitiOptimisticLockingException`：在由同一数据条目的并发访问引起的数据存储中发生乐观锁定时抛出。
- `ActivitiClassLoadingException`：在未找到请求加载的类或加载时发生错误（例如JavaDelegates，TaskListeners，...）时抛出。
- `ActivitiObjectNotFoundException`：当请求或操作的对象不存在时抛出。
- `ActivitiIllegalArgumentException`：一个异常，表示在Activiti API调用中提供了非法参数，在引擎配置中配置了非法值，或者提供了非法值或在流程定义中使用了非法值。
- `ActivitiTaskAlreadyClaimedException`：当一个任务被声明时，当调用`taskService.claim（...）`时抛出。

### 4.3. Working with the Activiti services

如上所述，与Activiti引擎交互的方式是通过`org.activiti.engine.ProcessEngine`类的实例公开的服务。 以下代码片段假设您有一个有效的Activiti环境，即您可以访问有效的`org.activiti.engine.ProcessEngine`。 如果您只是想尝试下面的代码，可以下载或克隆[Activiti单元测试模板]（https://github.com/Activiti/activiti-unit-test-template），将其导入IDE并添加 `org.activiti.MyUnitTest`单元测试的`testUserguideCode（）`方法。

这个小教程的最终目标是建立一个有效的业务流程，模仿公司的简单假期请求流程：

！[api.vacationRequest（https://www.activiti.org/userguide/images/api.vacationRequest.png）

#### 4.3.1. Deploying the process

通过** RepositoryService **访问与* static * data（例如流程定义）相关的所有内容。 从概念上讲，每个这样的静态数据都是Activiti引擎的* repository *的内容。

在`src / test / resources / org / activiti / test`资源文件夹中创建一个新的xml文件`VacationRequest.bpmn20.xml`（如果你没有使用单元测试模板，则在其他任何地方），并带有以下内容。 请注意，本节不会解释上面示例中使用的xml构造。 如果需要，请阅读[BPMN 2.0章节]（https://www.activiti.org/userguide/#bpmn20）以熟悉这些结构。

```
<?xml version="1.0" encoding="UTF-8" ?>
<definitions id="definitions"
             targetNamespace="http://activiti.org/bpmn20"
             xmlns="http://www.omg.org/spec/BPMN/20100524/MODEL"
             xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xmlns:activiti="http://activiti.org/bpmn">

  <process id="vacationRequest" name="Vacation request">

    <startEvent id="request" activiti:initiator="employeeName">
      <extensionElements>
        <activiti:formProperty id="numberOfDays" name="Number of days" type="long" value="1" required="true"/>
        <activiti:formProperty id="startDate" name="First day of holiday (dd-MM-yyy)" datePattern="dd-MM-yyyy hh:mm" type="date" required="true" />
        <activiti:formProperty id="vacationMotivation" name="Motivation" type="string" />
      </extensionElements>
    </startEvent>
    <sequenceFlow id="flow1" sourceRef="request" targetRef="handleRequest" />

    <userTask id="handleRequest" name="Handle vacation request" >
      <documentation>
        ${employeeName} would like to take ${numberOfDays} day(s) of vacation (Motivation: ${vacationMotivation}).
      </documentation>
      <extensionElements>
         <activiti:formProperty id="vacationApproved" name="Do you approve this vacation" type="enum" required="true">
          <activiti:value id="true" name="Approve" />
          <activiti:value id="false" name="Reject" />
        </activiti:formProperty>
        <activiti:formProperty id="managerMotivation" name="Motivation" type="string" />
      </extensionElements>
      <potentialOwner>
        <resourceAssignmentExpression>
          <formalExpression>management</formalExpression>
        </resourceAssignmentExpression>
      </potentialOwner>
    </userTask>
    <sequenceFlow id="flow2" sourceRef="handleRequest" targetRef="requestApprovedDecision" />

    <exclusiveGateway id="requestApprovedDecision" name="Request approved?" />
    <sequenceFlow id="flow3" sourceRef="requestApprovedDecision" targetRef="sendApprovalMail">
      <conditionExpression xsi:type="tFormalExpression">${vacationApproved == 'true'}</conditionExpression>
    </sequenceFlow>

    <task id="sendApprovalMail" name="Send confirmation e-mail" />
    <sequenceFlow id="flow4" sourceRef="sendApprovalMail" targetRef="theEnd1" />
    <endEvent id="theEnd1" />

    <sequenceFlow id="flow5" sourceRef="requestApprovedDecision" targetRef="adjustVacationRequestTask">
      <conditionExpression xsi:type="tFormalExpression">${vacationApproved == 'false'}</conditionExpression>
    </sequenceFlow>

    <userTask id="adjustVacationRequestTask" name="Adjust vacation request">
      <documentation>
        Your manager has disapproved your vacation request for ${numberOfDays} days.
        Reason: ${managerMotivation}
      </documentation>
      <extensionElements>
        <activiti:formProperty id="numberOfDays" name="Number of days" value="${numberOfDays}" type="long" required="true"/>
        <activiti:formProperty id="startDate" name="First day of holiday (dd-MM-yyy)" value="${startDate}" datePattern="dd-MM-yyyy hh:mm" type="date" required="true" />
        <activiti:formProperty id="vacationMotivation" name="Motivation" value="${vacationMotivation}" type="string" />
        <activiti:formProperty id="resendRequest" name="Resend vacation request to manager?" type="enum" required="true">
          <activiti:value id="true" name="Yes" />
          <activiti:value id="false" name="No" />
        </activiti:formProperty>
      </extensionElements>
      <humanPerformer>
        <resourceAssignmentExpression>
          <formalExpression>${employeeName}</formalExpression>
        </resourceAssignmentExpression>
      </humanPerformer>
    </userTask>
    <sequenceFlow id="flow6" sourceRef="adjustVacationRequestTask" targetRef="resendRequestDecision" />

    <exclusiveGateway id="resendRequestDecision" name="Resend request?" />
    <sequenceFlow id="flow7" sourceRef="resendRequestDecision" targetRef="handleRequest">
      <conditionExpression xsi:type="tFormalExpression">${resendRequest == 'true'}</conditionExpression>
    </sequenceFlow>

     <sequenceFlow id="flow8" sourceRef="resendRequestDecision" targetRef="theEnd2">
      <conditionExpression xsi:type="tFormalExpression">${resendRequest == 'false'}</conditionExpression>
    </sequenceFlow>
    <endEvent id="theEnd2" />

  </process>

</definitions>
```

要使Activiti引擎知道此过程，我们必须先*部署它。 部署意味着引擎将BPMN 2.0 xml解析为可执行文件，并为* deployment *中包含的每个流程定义添加新的数据库记录。 这样，当引擎重新启动时，它仍然会知道所有*部署的*进程：

```
ProcessEngine processEngine = ProcessEngines.getDefaultProcessEngine();
RepositoryService repositoryService = processEngine.getRepositoryService();
repositoryService.createDeployment()
  .addClasspathResource("org/activiti/test/VacationRequest.bpmn20.xml")
  .deploy();

Log.info("Number of process definitions: " + repositoryService.createProcessDefinitionQuery().count());
```

阅读[部署章节]（https://www.activiti.org/userguide/#chDeployment）中有关部署的更多信息。

#### 4.3.2. Starting a process instance

在将流程定义部署到Activiti引擎后，我们可以从中启动新的流程实例。 对于每个流程定义，通常有许多流程实例。 流程定义是* blueprint *，而流程实例是它的运行时执行。

可以在** RuntimeService **中找到与进程的运行时状态相关的所有内容。 有多种方法可以启动新的流程实例。 在下面的代码片段中，我们使用在流程定义xml中定义的密钥来启动流程实例。 我们还在流程实例启动时提供了一些流程变量，因为第一个用户任务的描述将在其表达式中使用这些变量。 通常使用过程变量，因为它们为特定过程定义的过程实例赋予了意义。 通常，流程变量使流程实例彼此不同。

```
Map<String, Object> variables = new HashMap<String, Object>();
variables.put("employeeName", "Kermit");
variables.put("numberOfDays", new Integer(4));
variables.put("vacationMotivation", "I'm really tired!");

RuntimeService runtimeService = processEngine.getRuntimeService();
ProcessInstance processInstance = runtimeService.startProcessInstanceByKey("vacationRequest", variables);

// Verify that we started a new process instance
Log.info("Number of process instances: " + runtimeService.createProcessInstanceQuery().count());
```

#### 4.3.3. Completing tasks

当流程开始时，第一步将是用户任务。 这是必须由系统用户执行的步骤。 通常，这样的用户将具有*任务*收件箱，其列出了该用户需要完成的所有任务。 以下代码段显示了如何执行此类查询：

```
// Fetch all tasks for the management group
TaskService taskService = processEngine.getTaskService();
List<Task> tasks = taskService.createTaskQuery().taskCandidateGroup("management").list();
for (Task task : tasks) {
  Log.info("Task available: " + task.getName());
}
```

要继续流程实例，我们需要完成此任务。 对于Activiti引擎，这意味着您需要“完成”任务。 以下代码段显示了如何完成此操作：

```
Task task = tasks.get(0);

Map<String, Object> taskVariables = new HashMap<String, Object>();
taskVariables.put("vacationApproved", "false");
taskVariables.put("managerMotivation", "We have a tight deadline!");
taskService.complete(task.getId(), taskVariables);
```

流程实例现在将继续下一步。 在此示例中，下一步允许员工填写调整其原始休假请求的表单。 员工可以重新提交休假请求，这将导致进程循环回到启动任务。

#### 4.3.4. Suspending and activating a process

可以暂停流程定义。 暂停流程定义时，无法创建新流程实例（将抛出异常）。 暂停流程定义是通过`RepositoryService`完成的：

```
repositoryService.suspendProcessDefinitionByKey("vacationRequest");
try {
  runtimeService.startProcessInstanceByKey("vacationRequest");
} catch (ActivitiException e) {
  e.printStackTrace();
}
```

要重新激活流程定义，只需调用其中一个`repositoryService.activateProcessDefinitionXXX`方法。

也可以暂停流程实例。 暂停时，该过程无法继续（例如，完成任务会抛出异常）并且不会执行任何作业（例如计时器）。 可以通过调用`runtimeService.suspendProcessInstance`方法来挂起流程实例。 再次激活流程实例是通过调用`runtimeService.activateProcessInstanceXXX`方法完成的。

#### 4.3.5. Further reading

我们在前几节中几乎没有涉及Activiti功能的表面。 我们将在未来进一步扩展这些部分，并对Activiti API进行更多报道。 当然，与任何开源项目一样，最好的学习方法是检查代码并阅读Javadocs！

### 4.4. Query API

有两种方法可以从引擎查询数据：查询API和本机查询。 Query API允许使用流畅的API对完全类型安全的查询进行编程。 您可以为查询添加各种条件（所有这些条件作为逻辑AND一起应用）并且恰好是一个排序。 以下代码显示了一个示例：

```
List<Task> tasks = taskService.createTaskQuery()
    .taskAssignee("kermit")
    .processVariableValueEquals("orderId", "0815")
    .orderByDueDate().asc()
    .list();
```

有时您需要更强大的查询，例如 使用OR运算符的查询或使用Query API无法表达的限制。 对于这些情况，我们引入了本机查询，允许您编写自己的SQL查询。 返回类型由您使用的Query对象定义，数据映射到正确的对象，例如 任务，ProcessInstance，执行等.... 由于将在数据库中触发查询，因此必须使用在数据库中定义的表和列名称; 这需要一些有关内部数据结构的知识，建议小心使用本机查询。 可以通过API检索表名，以使依赖性尽可能小。

```
List<Task> tasks = taskService.createNativeTaskQuery()
  .sql("SELECT count(*) FROM " + managementService.getTableName(Task.class) + " T WHERE T.NAME_ = #{taskName}")
  .parameter("taskName", "gonzoTask")
  .list();

long count = taskService.createNativeTaskQuery()
  .sql("SELECT count(*) FROM " + managementService.getTableName(Task.class) + " T1, "
    + managementService.getTableName(VariableInstanceEntity.class) + " V1 WHERE V1.TASK_ID_ = T1.ID_")
  .count();
```

### 4.5. Variables

每个流程实例都需要并使用数据来执行它所存在的步骤。 在Activiti中，此数据称为*变量*，它们存储在数据库中。 变量可以在表达式中使用（例如，在独占网关中选择正确的传出顺序流），在调用外部服务时的java服务任务中（例如，提供输入或存储服务调用的结果）等。

流程实例可以包含变量（称为*流程变量*），但也包含*执行*（它们是流程处于活动状态的特定指针），用户任务可以包含变量。 流程实例可以包含任意数量的变量。 每个变量都存储在* ACT_RU_VARIABLE *数据库表中的一行中。

任何* startProcessInstanceXXX *方法都有一个可选参数，用于在创建和启动流程实例时提供变量。 例如，来自* RuntimeService *：

```
ProcessInstance startProcessInstanceByKey(String processDefinitionKey, Map<String, Object> variables);
```

可以在流程执行期间添加变量。 例如（* RuntimeService *）：

```
void setVariable(String executionId, String variableName, Object value);
void setVariableLocal(String executionId, String variableName, Object value);
void setVariables(String executionId, Map<String, ? extends Object> variables);
void setVariablesLocal(String executionId, Map<String, ? extends Object> variables);
```

请注意，对于给定的执行，可以为变量设置* local *（请记住，流程实例由执行树组成）。 该变量仅在该执行时可见，而在执行树中不高。 如果数据不应传播到流程实例级别，或者变量在流程实例中具有某个路径的新值（例如，使用并行路径时），则此操作非常有用。

也可以再次获取变量，如下所示。 请注意，* TaskService *上存在类似的方法。 这意味着任务（如执行）可以在任务期间具有* alive *的局部变量。

```
Map<String, Object> getVariables(String executionId);
Map<String, Object> getVariablesLocal(String executionId);
Map<String, Object> getVariables(String executionId, Collection<String> variableNames);
Map<String, Object> getVariablesLocal(String executionId, Collection<String> variableNames);
Object getVariable(String executionId, String variableName);
<T> T getVariable(String executionId, String variableName, Class<T> variableClass);
```

变量通常用于[Java代理]（https://www.activiti.org/userguide/#bpmnJavaServiceTask），[表达式]（https://www.activiti.org/userguide/#apiExpressions），执行或任务执行者 在这些构造中，当前的* execution *或* task *对象是可用的，它可以用于变量设置和/或检索。 最简单的方法是这些：

```
execution.getVariables();
execution.getVariables(Collection<String> variableNames);
execution.getVariable(String variableName);

execution.setVariables(Map<String, object> variables);
execution.setVariable(String variableName, Object value);
```

请注意，带有* local *的变体也可用于上述所有内容。

对于历史（和向后兼容的原因），当进行上述任何调用时，实际上**将从数据库中获取所有**变量。这意味着如果你有10个变量，并且只通过* getVariable（“myVariable”）*获得一个变量，那么在后台将获取并缓存其他9个变量。这不错，因为后续调用不会再次访问数据库。例如，当您的流程定义具有三个顺序服务任务（以及一个数据库事务）时，使用一个调用来获取第一个服务任务中的所有变量可能会更好，然后分别获取每个服务任务中所需的变量。请注意，这适用于**和**以获取和设置变量。

当然，在使用大量变量时或者只是在想要严格控制数据库查询和流量时，这是不合适的。自Activiti 5.17以来，已经引入了新方法来对此进行更严格的控制，通过添加具有可选参数的新方法，该方法告诉引擎在后台是否需要获取和缓存所有变量：

```
Map<String, Object> getVariables(Collection<String> variableNames, boolean fetchAllVariables);
Object getVariable(String variableName, boolean fetchAllVariables);
void setVariable(String variableName, Object value, boolean fetchAllVariables);
```

当对参数* fetchAllVariables *使用* true *时，行为将完全如上所述：获取或设置变量时，将获取并缓存所有其他变量。

但是，当使用* false *作为值时，将使用特定查询，并且不会提取或缓存其他变量。 只有此处所讨论的变量的值才会被缓存以供后续使用。

### 4.6. Transient variables

瞬态变量是行为类似于常规变量的变量，但不是持久变量。通常，瞬态变量用于高级用例（即，当有疑问时，使用常规过程变量）。

以下适用于瞬态变量：

- 瞬态变量根本没有存储历史记录。
- 与*常规*变量一样，瞬态变量在设置时放在*最高父变量*上。这意味着在执行时设置变量时，瞬态变量实际上存储在流程实例执行中。与常规变量一样，如果应在特定执行或任务上设置变量，则存在方法的* local *变体。
- 瞬态变量只能在流程定义中的下一个*等待状态*之前访问。在那之后，他们走了。等待状态在此处表示流程实例中持久存储到数据存储的点。请注意，* async *活动在此定义中也是*等待状态*！
- 瞬态变量只能由* setTransientVariable（name，value）*设置，但调用* getVariable（name）时也会返回瞬态变量*（a * getTransientVariable（name）*也存在，只检查瞬态变量） 。这样做的原因是使表达式的编写变得容易，并且使用变量的现有逻辑适用于这两种类型。
- 瞬态变量* shadow *具有相同名称的持久变量。这意味着当在流程实例上设置持久变量和瞬态变量并且使用* getVariable（“someVariable”）*时，将返回瞬态变量值。

可以在暴露常规变量的大多数地方获取和/或设置瞬态变量：

- 在* JavaDelegate *实现中的* DelegateExecution *
- 在* ExecutionListener *实现中的* DelegateExecution *和在* TaskListener *实现上的* DelegateTask *上
- 在脚本任务中通过* execution *对象
- 通过运行时服务启动流程实例时
- 完成任务时
- 调用* runtimeService.trigger *方法时

这些方法遵循常规流程变量的命名约定：

```
void setTransientVariable(String variableName, Object variableValue);
void setTransientVariableLocal(String variableName, Object variableValue);
void setTransientVariables(Map<String, Object> transientVariables);
void setTransientVariablesLocal(Map<String, Object> transientVariables);

Object getTransientVariable(String variableName);
Object getTransientVariableLocal(String variableName);

Map<String, Object> getTransientVariables();
Map<String, Object> getTransientVariablesLocal();

void removeTransientVariable(String variableName);
void removeTransientVariableLocal(String variableName);
```

以下BPMN图显示了一个典型示例：

！[api.transient.variable.example（https://www.activiti.org/userguide/images/api.transient.variable.example.png）

假设* Fetch Data *服务任务调用一些远程服务（例如使用REST）。 我们还假设需要一些配置参数，并且需要在启动流程实例时提供。 此外，这些配置参数对于历史审计目的并不重要，因此我们将它们作为瞬态变量传递：

```
ProcessInstance processInstance = runtimeService.createProcessInstanceBuilder()
       .processDefinitionKey("someKey")
       .transientVariable("configParam01", "A")
       .transientVariable("configParam02", "B")
       .transientVariable("configParam03", "C")
       .start();
```

请注意，变量将一直可用，直到达到用户任务并持久保存到数据库。 例如，在* Additional Work *用户任务中，它们不再可用。 另请注意，如果* Fetch Data *是异步的，那么在该步骤之后它们也将不可用。

* Fetch Data *（简化）可能是这样的

```
public static class FetchDataServiceTask implements JavaDelegate {
  public void execute(DelegateExecution execution) {
    String configParam01 = (String) execution.getVariable(configParam01);
    // ...

    RestReponse restResponse = executeRestCall();
    execution.setTransientVariable("response", restResponse.getBody());
    execution.setTransientVariable("status", restResponse.getStatus());
  }
}
```

* Process Data *将获得响应瞬态变量，解析它并将相关数据存储在实际过程变量中，以便我们稍后需要它们。

离开专用网关的序列流的条件无关于是使用持久变量还是瞬态变量（在这种情况下是* status * transient变量）：

```
<conditionExpression xsi:type="tFormalExpression">${status == 200}</conditionExpression>
```

### 4.7. Expressions

Activiti使用UEL进行表达式解析。 UEL代表*统一表达语言*，是EE6规范的一部分（有关详细信息，请参阅[EE6规范]（http://docs.oracle.com/javaee/6/tutorial/doc/gjddd.html）。为了支持所有环境中最新UEL规范的所有功能，我们使用JUEL的修改版本。

表达式可用于例如[Java服务任务]（https://www.activiti.org/userguide/#bpmnJavaServiceTaskXML），[执行监听器]（https://www.activiti.org/userguide/#executionListeners）， [任务监听器]（https://www.activiti.org/userguide/#taskListeners）和[条件序列流程]（https://www.activiti.org/userguide/#conditionalSequenceFlowXml）。尽管有两种类型的表达式，值表达式和方法表达式，但Activiti对此进行了抽象，因此它们都可以在需要“表达式”的地方使用。

- **值表达式**：解析为值。默认情况下，可以使用所有过程变量。此外，所有spring-beans（如果使用Spring）都可用于表达式。一些例子：

```
${myVar}
${myBean.myProperty}
```

 -  **方法表达式**：调用带或不带参数的方法。 **在调用不带参数的方法时，请确保在方法名称后添加空括号（因为这会将表达式与值表达式区分开来）。**传递的参数可以是文字值或自己解析的表达式。 例子：
```
${printer.print()}
${myBean.addNewOrder('orderName')}
${myBean.doSomething(myVar, execution)}
```

请注意，这些表达式支持解析基元（包括比较它们），bean，列表，数组和映射。

除了所有流程变量之外，还有一些默认对象可用于表达式：

- `execution`：`DelegateExecution`，它包含有关正在执行的其他信息。
- `task`：`DelegateTask`，它包含有关当前任务的其他信息。 **注意：仅适用于从任务侦听器评估的表达式。**
- `authenticatedUserId`：当前经过身份验证的用户的id。如果没有用户通过身份验证，则该变量不可用。

有关更具体的用法和示例，请查看[Spring中的表达式]（https://www.activiti.org/userguide/#springExpressions），[Java服务任务]（https://www.activiti.org/userguide/# bpmnJavaServiceTaskXML），[执行监听器]（https://www.activiti.org/userguide/#executionListeners），[任务监听器]（https://www.activiti.org/userguide/#taskListeners）或[条件序列流程] （https://www.activiti.org/userguide/#conditionalSequenceFlowXml）。

### 4.8. Unit testing

业务流程是软件项目不可或缺的一部分，它们应该以与测试正常应用程序逻辑相同的方式进行测试：使用单元测试。由于Activiti是一个可嵌入的Java引擎，因此编写业​​务流程的单元测试就像编写常规单元测试一样简单。

Activiti支持JUnit版本3和4样式的单元测试。在JUnit 3样式中，必须扩展`org.activiti.engine.test.ActivitiTestCase`。这将使ProcessEngine和服务通过受保护的成员字段可用。在测试的`setup（）`中，默认情况下会使用类路径上的`activiti.cfg.xml`resource初始化processEngine。要指定其他配置文件，请覆盖* getConfigurationResource（）*方法。当配置资源相同时，进程引擎将在多个单元测试中静态缓存。

通过扩展`ActivitiTestCase`，您可以使用`org.activiti.engine.test.Deployment`注释测试方法。在运行测试之前，将部署与测试类在同一包中的`testClassName.testMethod.bpmn20.xml`形式的资源文件。在测试结束时，将删除部署，包括所有相关的流程实例，任务等。“部署”注释还支持显式设置资源位置。有关更多信息，请参阅类本身。

考虑到所有这些，JUnit 3样式测试如下所示。

```
public class MyBusinessProcessTest extends ActivitiTestCase {

  @Deployment
  public void testSimpleProcess() {
    runtimeService.startProcessInstanceByKey("simpleProcess");

    Task task = taskService.createTaskQuery().singleResult();
    assertEquals("My Task", task.getName());

    taskService.complete(task.getId());
    assertEquals(0, runtimeService.createProcessInstanceQuery().count());
  }
}
```

要在使用JUnit 4样式编写单元测试时获得相同的功能，必须使用`org.activiti.engine.test.ActivitiRule`规则。 通过此规则，流程引擎和服务可通过getter获得。 与`ActivitiTestCase`（见上文）一样，包括这个`Rule`将允许使用`org.activiti.engine.test.Deployment`注释（见上面有关其使用和配置的解释），它将看起来 对于类路径上的默认配置文件。 当使用相同的配置资源时，进程引擎在多个单元测试中静态缓存。

以下代码片段显示了使用JUnit 4测试样式和`ActivitiRule`的使用示例。

```
public class MyBusinessProcessTest {

  @Rule
  public ActivitiRule activitiRule = new ActivitiRule();

  @Test
  @Deployment
  public void ruleUsageExample() {
    RuntimeService runtimeService = activitiRule.getRuntimeService();
    runtimeService.startProcessInstanceByKey("ruleUsage");

    TaskService taskService = activitiRule.getTaskService();
    Task task = taskService.createTaskQuery().singleResult();
    assertEquals("My Task", task.getName());

    taskService.complete(task.getId());
    assertEquals(0, runtimeService.createProcessInstanceQuery().count());
  }
}
```

### 4.9. Debugging unit tests

使用内存H2数据库进行单元测试时，以下说明允许在调试会话期间轻松检查Activiti数据库中的数据。这里的截图是在Eclipse中进行的，但其他IDE的机制应该类似。

假设我们在单元测试中的某处放置了一个*断点*。在Eclipse中，通过双击代码旁边的左边框来完成：

！[api.test.debug.breakpoint（https://www.activiti.org/userguide/images/api.test.debug.breakpoint.png）

如果我们现在以* debug *模式运行单元测试（右键单击测试类，选择* Run as *然后* JUnit test *），测试执行将在我们的断点处停止，现在我们可以检查我们的变量测试如右上图所示。

！[api.test.debug.view（https://www.activiti.org/userguide/images/api.test.debug.view.png）

要检查Activiti数据，请打开*'Display'*窗口（如果此窗口不存在，打开Window→Show View→Other并选择* Display *。）并键入（代码完成可用）`org.h2 .tools.Server.createWebServer。（ “ - 网络”）开始（）`

！[api.test.debug.start.h2.server（https://www.activiti.org/userguide/images/api.test.debug.start.h2.server.png）

选择刚刚输入的行，然后右键单击它。现在选择*显示*（或执行快捷方式而不是右键单击）

！[api.test.debug.start.h2.server.2（https://www.activiti.org/userguide/images/api.test.debug.start.h2.server.2.png）

现在打开浏览器并转到[http：// localhost：8082]（http：// localhost：8082 /），并将JDBC URL填入内存数据库（默认情况下为`jdbc：h2： mem：activiti`），然后点击连接按钮。

！[api.test.debug.h2.login（https://www.activiti.org/userguide/images/api.test.debug.h2.login.png）

您现在可以看到Activiti数据并使用它来了解单元测试以某种方式执行过程的方式和原因。

！[api.test.debug.h2.tables（https://www.activiti.org/userguide/images/api.test.debug.h2.tables.png）

### 4.10. The process engine in a web application

`ProcessEngine`是一个线程安全的类，可以很容易地在多个线程之间共享。 在Web应用程序中，这意味着可以在容器启动时创建一次流程引擎，并在容器停机时关闭引擎。

以下代码片段显示了如何编写一个简单的`ServletContextListener`来初始化和销毁普通Servlet环境中的流程引擎：

```
public class ProcessEnginesServletContextListener implements ServletContextListener {

  public void contextInitialized(ServletContextEvent servletContextEvent) {
    ProcessEngines.init();
  }

  public void contextDestroyed(ServletContextEvent servletContextEvent) {
    ProcessEngines.destroy();
  }

}
```

`contextInitialized`方法将委托给`ProcessEngines.init（）`。 这将在类路径上查找`activiti.cfg.xml`资源文件，并为给定的配置创建一个`ProcessEngine`（例如，带有配置文件的多个jar）。 如果类路径上有多个此类资源文件，请确保它们都具有不同的名称。 当需要流程引擎时，可以使用它来获取它

```
ProcessEngines.getDefaultProcessEngine()
```

或

```
ProcessEngines.getProcessEngine("myName");
```

当然，也可以使用创建流程引擎的任何变体，如[配置部分]（https://www.activiti.org/userguide/#configuration）中所述。

context-listener的`contextDestroyed`方法委托给`ProcessEngines.destroy（）`。 这将正确关闭所有初始化的流程引擎。

## 5. Spring 整合

虽然你绝对可以使用没有Spring的Activiti，但我们提供了一些非常好的集成功能，本章将对此进行解释。

### 5.1. ProcessEngineFactoryBean

可以将`ProcessEngine`配置为常规Spring bean。 集成的起点是类`org.activiti.spring.ProcessEngineFactoryBean`。 该bean采用流程引擎配置并创建流程引擎。 这意味着Spring的属性的创建和配置与[配置部分]（https://www.activiti.org/userguide/#configuration）中记录的相同。 对于Spring集成，配置和引擎bean将如下所示：

```
<bean id="processEngineConfiguration" class="org.activiti.spring.SpringProcessEngineConfiguration">
    ...
</bean>

<bean id="processEngine" class="org.activiti.spring.ProcessEngineFactoryBean">
  <property name="processEngineConfiguration" ref="processEngineConfiguration" />
</bean>
```

请注意，`processEngineConfiguration` bean现在使用`org.activiti.spring.SpringProcessEngineConfiguration`class。

### 5.2. Transactions

我们将逐步解释在Spring的分布示例中找到的`SpringTransactionIntegrationTest`。下面是我们在此示例中使用的Spring配置文件（您可以在SpringTransactionIntegrationTest-context.xml中找到它）。下面显示的部分包含dataSource，transactionManager，processEngine和Activiti Engine服务。

当将DataSource传递给`SpringProcessEngineConfiguration`（使用属性“dataSource”）时，Activiti在内部使用`org.springframework.jdbc.datasource.TransactionAwareDataSourceProxy`，它包装传递的DataSource。这样做是为了确保从DataSource和Spring事务中检索到的SQL连接能够很好地协同工作。这意味着不再需要在Spring配置中自己代理dataSource，尽管它仍然允许将`TransactionAwareDataSourceProxy`传递给`SpringProcessEngineConfiguration`。在这种情况下，不会发生额外的包装。

**确保在自己声明Spring配置中的TransactionAwareDataSourceProxy时，不要将它用于已经知道Spring事务的资源（例如，DataSourceTransactionManager和JPATransactionManager需要未代理的dataSource）。**

```
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans   http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-2.5.xsd
                           http://www.springframework.org/schema/tx      http://www.springframework.org/schema/tx/spring-tx-3.0.xsd">

  <bean id="dataSource" class="org.springframework.jdbc.datasource.SimpleDriverDataSource">
    <property name="driverClass" value="org.h2.Driver" />
    <property name="url" value="jdbc:h2:mem:activiti;DB_CLOSE_DELAY=1000" />
    <property name="username" value="sa" />
    <property name="password" value="" />
  </bean>

  <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <property name="dataSource" ref="dataSource" />
  </bean>

  <bean id="processEngineConfiguration" class="org.activiti.spring.SpringProcessEngineConfiguration">
    <property name="dataSource" ref="dataSource" />
    <property name="transactionManager" ref="transactionManager" />
    <property name="databaseSchemaUpdate" value="true" />
    <property name="asyncExecutorActivate" value="false" />
  </bean>

  <bean id="processEngine" class="org.activiti.spring.ProcessEngineFactoryBean">
    <property name="processEngineConfiguration" ref="processEngineConfiguration" />
  </bean>

  <bean id="repositoryService" factory-bean="processEngine" factory-method="getRepositoryService" />
  <bean id="runtimeService" factory-bean="processEngine" factory-method="getRuntimeService" />
  <bean id="taskService" factory-bean="processEngine" factory-method="getTaskService" />
  <bean id="historyService" factory-bean="processEngine" factory-method="getHistoryService" />
  <bean id="managementService" factory-bean="processEngine" factory-method="getManagementService" />

...
```

该Spring配置文件的其余部分包含我们将在此特定示例中使用的bean和配置：

```
<beans>
  ...
  <tx:annotation-driven transaction-manager="transactionManager"/>

  <bean id="userBean" class="org.activiti.spring.test.UserBean">
    <property name="runtimeService" ref="runtimeService" />
  </bean>

  <bean id="printer" class="org.activiti.spring.test.Printer" />

</beans>
```

首先，使用任何Spring方法创建应用程序上下文。 在此示例中，您可以使用类路径XML资源来配置Spring应用程序上下文：

```
ClassPathXmlApplicationContext applicationContext = new ClassPathXmlApplicationContext(
	"org/activiti/examples/spring/SpringTransactionIntegrationTest-context.xml");
```

或者因为它是一个测试：

```
@ContextConfiguration("classpath:org/activiti/spring/test/transaction/SpringTransactionIntegrationTest-context.xml")
```

然后我们可以获取服务bean并在它们上调用方法。 ProcessEngineFactoryBean将为在Activiti服务方法上应用Propagation.REQUIRED事务语义的服务添加额外的拦截器。 因此，例如，我们可以使用repositoryService来部署这样的进程：

```
RepositoryService repositoryService =
  (RepositoryService) applicationContext.getBean("repositoryService");
String deploymentId = repositoryService
  .createDeployment()
  .addClasspathResource("org/activiti/spring/test/hello.bpmn20.xml")
  .deploy()
  .getId();
```

反过来也有效。 在这种情况下，Spring事务将围绕userBean.hello（）方法，Activiti服务方法调用将加入同一事务。

```
UserBean userBean = (UserBean) applicationContext.getBean("userBean");
userBean.hello();
```

UserBean看起来像这样。 请记住，在Spring bean配置中，我们将repositoryService注入userBean。

```
public class UserBean {

  /** injected by Spring */
  private RuntimeService runtimeService;

  @Transactional
  public void hello() {
    // here you can do transactional stuff in your domain model
    // and it will be combined in the same transaction as
    // the startProcessInstanceByKey to the Activiti RuntimeService
    runtimeService.startProcessInstanceByKey("helloProcess");
  }

  public void setRuntimeService(RuntimeService runtimeService) {
    this.runtimeService = runtimeService;
  }
}
```

### 5.3. Expressions

在使用ProcessEngineFactoryBean时，默认情况下，BPMN流程中的所有[表达式]（https://www.activiti.org/userguide/#apiExpressions）也会*查看*所有Spring bean。 可以使用您可以配置的映射来限制要在表达式中公开的bean，甚至根本不暴露任何bean。 下面的示例公开了一个bean（打印机），可以在“打印机”键下使用。 **要完全暴露NO bean，只需在SpringProcessEngineConfiguration上将空列表作为beans属性传递。 如果未设置beans属性，则上下文中的所有Spring bean都可用。**

```
<bean id="processEngineConfiguration" class="org.activiti.spring.SpringProcessEngineConfiguration">
  ...
  <property name="beans">
    <map>
      <entry key="printer" value-ref="printer" />
    </map>
  </property>
</bean>

<bean id="printer" class="org.activiti.examples.spring.Printer" />
```

现在可以在表达式中使用公开的bean：例如，SpringTransactionIntegrationTest`hello.bpmn20.xml`显示了如何使用UEL方法表达式调用Spring bean上的方法：

```
<definitions id="definitions">

  <process id="helloProcess">

    <startEvent id="start" />
    <sequenceFlow id="flow1" sourceRef="start" targetRef="print" />

    <serviceTask id="print" activiti:expression="#{printer.printMessage()}" />
    <sequenceFlow id="flow2" sourceRef="print" targetRef="end" />

    <endEvent id="end" />

  </process>

</definitions>
```

`Printer`看起来像这样：

```
public class Printer {

  public void printMessage() {
    System.out.println("hello world");
  }
}
```

Spring bean配置（也如上所示）如下所示：

```
<beans>
  ...

  <bean id="printer" class="org.activiti.examples.spring.Printer" />

</beans>
```

### 5.4. Automatic resource deployment

Spring集成还具有部署资源的特殊功能。 在流程引擎配置中，您可以指定一组资源。 创建流程引擎后，将扫描和部署所有这些资源。 有过滤可以防止重复部署。 只有当资源实际发生变化时，才会将新部署部署到Activiti DB。 这在许多用例中很有意义，其中Spring容器经常重启（例如测试）。

这是一个例子：

```
<bean id="processEngineConfiguration" class="org.activiti.spring.SpringProcessEngineConfiguration">
  ...
  <property name="deploymentResources"
    value="classpath*:/org/activiti/spring/test/autodeployment/autodeploy.*.bpmn20.xml" />
</bean>

<bean id="processEngine" class="org.activiti.spring.ProcessEngineFactoryBean">
  <property name="processEngineConfiguration" ref="processEngineConfiguration" />
</bean>
```

默认情况下，上面的配置会将与过滤匹配的所有资源分组到Activiti引擎的单个部署中。防止重新部署未更改资源的重复筛选适用于整个部署。在某些情况下，这可能不是你想要的。例如，如果以这种方式部署一组流程资源，并且这些资源中只有一个流程定义发生了变化，则整个部署将被视为新部署，并且将重新部署该部署中的所有流程定义，从而产生在每个流程定义的新版本中，即使只有一个实际更改了。

为了能够自定义部署的确定方式，您可以在`SpringProcessEngineConfiguration`，`deploymentMode`中指定其他属性。此属性定义从与筛选器匹配的资源集确定部署的方式。默认情况下，此属性支持3个值：

- `default`：将所有资源分组到单个部署中，并对该部署应用重复过滤。这是默认值，如果您未指定值，将使用它。
- `single-resource`：为每个单独的资源创建一个单独的部署，并对该部署应用重复过滤。这是用于单独部署每个流程定义的值，如果已更改，则仅创建新的流程定义版本。
- `resource-parent-folder`：为共享同一父文件夹的资源创建单独的部署，并对该部署应用重复过滤。此值可用于为大多数资源创建单独的部署，但仍可以通过将它们放在共享文件夹中来对其进行分组。这是一个如何为`deploymentMode`指定`single-resource`配置的示例：

```
<bean id="processEngineConfiguration"
    class="org.activiti.spring.SpringProcessEngineConfiguration">
  ...
  <property name="deploymentResources" value="classpath*:/activiti/*.bpmn" />
  <property name="deploymentMode" value="single-resource" />
</bean>
```

除了使用上面列出的`deploymentMode`值之外，您还可能需要自定义行为来确定部署。 如果是这样，您可以创建`SpringProcessEngineConfiguration`的子类并覆盖`getAutoDeploymentStrategy（String deploymentMode）`方法。 此方法确定将哪个部署策略用于“deploymentMode”配置的特定值。

### 5.5. Unit testing

与Spring集成时，可以使用标准[Activiti测试工具]（https://www.activiti.org/userguide/#apiUnitTesting）轻松测试业务流程。 以下示例显示了如何在典型的基于Spring的单元测试中测试业务流程：

```
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("classpath:org/activiti/spring/test/junit4/springTypicalUsageTest-context.xml")
public class MyBusinessProcessTest {

  @Autowired
  private RuntimeService runtimeService;

  @Autowired
  private TaskService taskService;

  @Autowired
  @Rule
  public ActivitiRule activitiSpringRule;

  @Test
  @Deployment
  public void simpleProcessTest() {
    runtimeService.startProcessInstanceByKey("simpleProcess");
    Task task = taskService.createTaskQuery().singleResult();
    assertEquals("My Task", task.getName());

    taskService.complete(task.getId());
    assertEquals(0, runtimeService.createProcessInstanceQuery().count());

  }
}
```

请注意，要使其工作，您需要在Spring配置中定义一个* org.activiti.engine.test.ActivitiRule * bean（在上面的示例中通过自动布线注入）。

```
<bean id="activitiRule" class="org.activiti.engine.test.ActivitiRule">
  <property name="processEngine" ref="processEngine" />
</bean>
```

### 5.6. JPA with Hibernate 4.2.x

在服务任务中使用Hibernate 4.2.x JPA或在Activiti Engine中使用侦听器逻辑时，需要对Spring ORM进行额外的依赖。 Hibernate 4.1.x或更低版本不需要这样做。 应添加以下依赖项：

```
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-orm</artifactId>
  <version>${org.springframework.version}</version>
</dependency>
```

### 5.7. Spring Boot

Spring Boot是一个应用程序框架，根据[其网站]（http://projects.spring.io/spring-boot/），*可以轻松创建独立的，生产级的基于Spring的应用程序，可以 可以“跑”。 它采用了对Spring平台和第三方库的自以为是的观点，因此您可以轻松上手。 大多数Spring Boot应用程序只需要很少的Spring配置*。

有关Spring Boot的更多信息，请参阅<http://projects.spring.io/spring-boot/>

Spring Boot  -  Activiti集成目前正在试验中。 它一直是开发人员和Spring提交者，但它仍处于早期阶段。 我们欢迎所有人尝试并提供反馈。

#### 5.7.1. Compatibility

Spring Boot需要JDK 7运行时。 请检查Spring Boot文档。

#### 5.7.2. Getting started

Spring Boot是关于约定优于配置的。 首先，只需将* spring-boot-starters-basic *依赖项添加到项目中即可。 例如对于Maven：

```
<dependency>
	<groupId>org.activiti</groupId>
	<artifactId>activiti-spring-boot-starter-basic</artifactId>
	<version>${activiti.version}</version>
</dependency>
```

这就是所需要的。 此依赖项将传递性地将正确的Activiti和Spring依赖项添加到类路径中。 您现在可以编写Spring Boot应用程序：

```
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.EnableAutoConfiguration;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;

@Configuration
@ComponentScan
@EnableAutoConfiguration
public class MyApplication {

    public static void main(String[] args) {
        SpringApplication.run(MyApplication.class, args);
    }

}
```

Activiti需要一个数据库来存储其数据。 如果您将运行上面的代码，它将为您提供一个信息性异常消息，您需要将数据库驱动程序依赖项添加到类路径。 现在，添加H2数据库依赖项：

```
<dependency>
	<groupId>com.h2database</groupId>
	<artifactId>h2</artifactId>
	<version>1.4.183</version>
</dependency>
```

现在可以启动该应用程序。 你会看到这样的输出：

```
  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::        (v1.1.6.RELEASE)

MyApplication                            : Starting MyApplication on ...
s.c.a.AnnotationConfigApplicationContext : Refreshing org.springframework.context.annotation.AnnotationConfigApplicationContext@33cb5951: startup date [Wed Dec 17 15:24:34 CET 2014]; root of context hierarchy
a.s.b.AbstractProcessEngineConfiguration : No process definitions were found using the specified path (classpath:/processes/**.bpmn20.xml).
o.activiti.engine.impl.db.DbSqlSession   : performing create on engine with resource org/activiti/db/create/activiti.h2.create.engine.sql
o.activiti.engine.impl.db.DbSqlSession   : performing create on history with resource org/activiti/db/create/activiti.h2.create.history.sql
o.activiti.engine.impl.db.DbSqlSession   : performing create on identity with resource org/activiti/db/create/activiti.h2.create.identity.sql
o.a.engine.impl.ProcessEngineImpl        : ProcessEngine default created
o.a.e.i.a.DefaultAsyncJobExecutor        : Starting up the default async job executor [org.activiti.spring.SpringAsyncExecutor].
o.a.e.i.a.AcquireTimerJobsRunnable       : {} starting to acquire async jobs due
o.a.e.i.a.AcquireAsyncJobsDueRunnable    : {} starting to acquire async jobs due
o.s.j.e.a.AnnotationMBeanExporter        : Registering beans for JMX exposure on startup
MyApplication                            : Started MyApplication in 2.019 seconds (JVM running for 2.294)
```

因此，只需将依赖项添加到类路径并使用* @ EnableAutoConfiguration *注释，就会在幕后发生很多事情：

 - 自动创建内存中数据源（因为H2驱动程序在类路径上）并传递给Activiti流程引擎配置
 - 创建并公开Activiti ProcessEngine bean
 - 所有Activiti服务都以Spring bean的形式公开
 - 创建了Spring Job Executor

此外，* processes *文件夹中的任何BPMN 2.0流程定义都将自动部署。 创建文件夹* processes *并将虚拟流程定义（名为* one-task-process.bpmn20.xml *）添加到此文件夹。

```
<?xml version="1.0" encoding="UTF-8"?>
<definitions
        xmlns="http://www.omg.org/spec/BPMN/20100524/MODEL"
        xmlns:activiti="http://activiti.org/bpmn"
        targetNamespace="Examples">

    <process id="oneTaskProcess" name="The One Task Process">
        <startEvent id="theStart" />
        <sequenceFlow id="flow1" sourceRef="theStart" targetRef="theTask" />
        <userTask id="theTask" name="my task" />
        <sequenceFlow id="flow2" sourceRef="theTask" targetRef="theEnd" />
        <endEvent id="theEnd" />
    </process>

</definitions>
```

还要添加以下代码行以测试部署是否实际工作。 * CommandLineRunner *是一种特殊的Spring bean，在应用程序启动时执行：

```
@Configuration
@ComponentScan
@EnableAutoConfiguration
public class MyApplication {

    public static void main(String[] args) {
        SpringApplication.run(MyApplication.class, args);
    }

    @Bean
    public CommandLineRunner init(final RepositoryService repositoryService,
                                  final RuntimeService runtimeService,
                                  final TaskService taskService) {

        return new CommandLineRunner() {
            @Override
            public void run(String... strings) throws Exception {
                System.out.println("Number of process definitions : "
                	+ repositoryService.createProcessDefinitionQuery().count());
                System.out.println("Number of tasks : " + taskService.createTaskQuery().count());
                runtimeService.startProcessInstanceByKey("oneTaskProcess");
                System.out.println("Number of tasks after process start: " + taskService.createTaskQuery().count());
            }
        };

    }

}
```

输出将如预期：

```
Number of process definitions : 1
Number of tasks : 0
Number of tasks after process start : 1
```

#### 5.7.3. Changing the database and connection pool

如上所述，Spring Boot是关于配置的约定。 默认情况下，通过在类路径上只有H2，它创建了一个内存数据源并将其传递给Activiti流程引擎配置。

要更改数据源，只需通过提供Datasource bean覆盖默认值。 我们在这里使用* DataSourceBuilder *类，它是Spring Boot的助手类。 如果Tomcat，HikariCP或Commons DBCP在类路径中，将选择其中一个（首先是Tomcat的顺序）。 例如，要切换到MySQL数据库：

```
@Bean
public DataSource database() {
    return DataSourceBuilder.create()
        .url("jdbc:mysql://127.0.0.1:3306/activiti-spring-boot?characterEncoding=UTF-8")
        .username("alfresco")
        .password("alfresco")
        .driverClassName("com.mysql.jdbc.Driver")
        .build();
}
```

从Maven依赖项中删除H2并将MySQL驱动程序和Tomcat连接池添加到类路径：

```
<dependency>
	<groupId>mysql</groupId>
	<artifactId>mysql-connector-java</artifactId>
	<version>5.1.34</version>
</dependency>
<dependency>
	<groupId>org.apache.tomcat</groupId>
	<artifactId>tomcat-jdbc</artifactId>
	<version>8.0.15</version>
</dependency>
```

当应用程序现在启动时，您将看到它使用MySQL作为数据库（以及Tomcat连接池框架）：

```
org.activiti.engine.impl.db.DbSqlSession   : performing create on engine with resource org/activiti/db/create/activiti.mysql.create.engine.sql
org.activiti.engine.impl.db.DbSqlSession   : performing create on history with resource org/activiti/db/create/activiti.mysql.create.history.sql
org.activiti.engine.impl.db.DbSqlSession   : performing create on identity with resource org/activiti/db/create/activiti.mysql.create.identity.sql
```

当您多次重启应用程序时，您将看到任务数量上升（H2内存数据库无法在关闭后继续存在，MySQL会这样做）。

#### 5.7.4. REST support

通常，嵌入式Activiti引擎（与公司中的不同服务交互）需要REST API。 Spring Boot让这很容易。 将以下依赖项添加到类路径：

```
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-web</artifactId>
	<version>${spring.boot.version}</version>
</dependency>
```

创建一个新类，一个Spring服务，并创建两个方法：一个用于启动我们的流程，另一个用于获取给定受理人的任务列表。 我们只是在这里包装Activiti调用，但在现实场景中，这显然会更加复杂。

```
@Service
public class MyService {

    @Autowired
    private RuntimeService runtimeService;

    @Autowired
    private TaskService taskService;

	@Transactional
    public void startProcess() {
        runtimeService.startProcessInstanceByKey("oneTaskProcess");
    }

	@Transactional
    public List<Task> getTasks(String assignee) {
        return taskService.createTaskQuery().taskAssignee(assignee).list();
    }

}
```

我们现在可以通过使用* @ RestController *注释类来创建REST端点。 在这里，我们只是委托上面定义的服务。

```
@RestController
public class MyRestController {

    @Autowired
    private MyService myService;

    @RequestMapping(value="/process", method= RequestMethod.POST)
    public void startProcessInstance() {
        myService.startProcess();
    }

    @RequestMapping(value="/tasks", method= RequestMethod.GET, produces=MediaType.APPLICATION_JSON_VALUE)
    public List<TaskRepresentation> getTasks(@RequestParam String assignee) {
        List<Task> tasks = myService.getTasks(assignee);
        List<TaskRepresentation> dtos = new ArrayList<TaskRepresentation>();
        for (Task task : tasks) {
            dtos.add(new TaskRepresentation(task.getId(), task.getName()));
        }
        return dtos;
    }

    static class TaskRepresentation {

        private String id;
        private String name;

        public TaskRepresentation(String id, String name) {
            this.id = id;
            this.name = name;
        }

         public String getId() {
            return id;
        }
        public void setId(String id) {
            this.id = id;
        }
        public String getName() {
            return name;
        }
        public void setName(String name) {
            this.name = name;
        }

    }

}
```

* @Service *和* @ RestController *都可以通过我们添加到应用程序类的自动组件扫描（* @ ComponentScan *）找到。 再次运行应用程序类。 我们现在可以使用例如cURL与REST API进行交互：

```
curl http://localhost:8080/tasks?assignee=kermit
[]

curl -X POST  http://localhost:8080/process
curl http://localhost:8080/tasks?assignee=kermit
[{"id":"10004","name":"my task"}]
```

#### 5.7.5. JPA support

要在Spring Boot中添加对Activiti的JPA支持，请添加以下依赖项：

```
<dependency>
	<groupId>org.activiti</groupId>
	<artifactId>activiti-spring-boot-starter-jpa</artifactId>
	<version>${activiti.version}</version>
</dependency>
```

这将添加Spring配置和bean以使用JPA。 默认情况下，JPA提供程序将是Hibernate。

让我们创建一个简单的Entity类：

```
@Entity
class Person {

    @Id
    @GeneratedValue
    private Long id;

    private String username;

    private String firstName;

    private String lastName;

    private Date birthDate;

    public Person() {
    }

    public Person(String username, String firstName, String lastName, Date birthDate) {
        this.username = username;
        this.firstName = firstName;
        this.lastName = lastName;
        this.birthDate = birthDate;
    }

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getFirstName() {
        return firstName;
    }

    public void setFirstName(String firstName) {
        this.firstName = firstName;
    }

    public String getLastName() {
        return lastName;
    }

    public void setLastName(String lastName) {
        this.lastName = lastName;
    }

    public Date getBirthDate() {
        return birthDate;
    }

    public void setBirthDate(Date birthDate) {
        this.birthDate = birthDate;
    }
}
```

默认情况下，不使用内存数据库时，不会自动创建表。 在类路径上创建文件* application.properties *并添加以下属性：

```
spring.jpa.hibernate.ddl-auto=update
```

添加以下课程：

```
public interface PersonRepository extends JpaRepository<Person, Long> {

    Person findByUsername(String username);

}
```

这是一个Spring存储库，提供开箱即用的CRUD。 我们添加方法以按用户名查找Person。 Spring将根据约定（即使用的属性名称）自动实现它。

我们现在进一步加强服务：

 - 通过向班级添加* @ Transactional *。 请注意，通过添加上面的JPA依赖项，我们之前使用的DataSourceTransactionManager现在由JpaTransactionManager自动换出。
 -  * startProcess *现在获取一个受理人用户名，用于查找Person，并将Person JPA对象作为流程实例中的流程变量。
 - 添加了创建虚拟用户的方法。 这在CommandLineRunner中用于填充数据库。

```
@Service
@Transactional
public class MyService {

    @Autowired
    private RuntimeService runtimeService;

    @Autowired
    private TaskService taskService;

    @Autowired
    private PersonRepository personRepository;

    public void startProcess(String assignee) {

        Person person = personRepository.findByUsername(assignee);

        Map<String, Object> variables = new HashMap<String, Object>();
        variables.put("person", person);
        runtimeService.startProcessInstanceByKey("oneTaskProcess", variables);
    }

    public List<Task> getTasks(String assignee) {
        return taskService.createTaskQuery().taskAssignee(assignee).list();
    }

    public void createDemoUsers() {
		 if (personRepository.findAll().size() == 0) {
            personRepository.save(new Person("jbarrez", "Joram", "Barrez", new Date()));
            personRepository.save(new Person("trademakers", "Tijs", "Rademakers", new Date()));
        }
    }

}
```

CommandLineRunner现在看起来像：

```
@Bean
public CommandLineRunner init(final MyService myService) {

	return new CommandLineRunner() {
    	public void run(String... strings) throws Exception {
        	myService.createDemoUsers();
        }
    };

}
```

RestController也略有改变，以包含上面的更改（仅显示新方法），HTTP POST现在有一个包含受理人用户名的正文：

```
@RestController
public class MyRestController {

    @Autowired
    private MyService myService;

    @RequestMapping(value="/process", method= RequestMethod.POST)
    public void startProcessInstance(@RequestBody StartProcessRepresentation startProcessRepresentation) {
        myService.startProcess(startProcessRepresentation.getAssignee());
    }

   ...

    static class StartProcessRepresentation {

        private String assignee;

        public String getAssignee() {
            return assignee;
        }

        public void setAssignee(String assignee) {
            this.assignee = assignee;
        }
    }
```

最后，为了尝试Spring-JPA-Activiti集成，我们使用流程定义中Person JPA对象的id来分配任务：

```
<userTask id="theTask" name="my task" activiti:assignee="${person.id}"/>
```

我们现在可以启动一个新的流程实例，在POST正文中提供用户名：

```
curl -H "Content-Type: application/json" -d '{"assignee" : "jbarrez"}' http://localhost:8080/process
```

现在使用person id获取任务列表：

```
curl http://localhost:8080/tasks?assignee=1

[{"id":"12505","name":"my task"}]
```

#### 5.7.6. Further Reading

显然有很多关于尚未触及的Spring Boot，比如非常简单的JTA集成或构建可在主要应用程序服务器上运行的war文件。 Spring Boot集成还有很多：

 - 执行器支持
 -  Spring Integration支持
 -  Rest API集成：启动Spring应用程序中嵌入的Activiti Rest API
 -  Spring Security支持

所有这些领域目前都是第一版，但它们将来会进一步发展。

## 6. Deployment

### 6.1. Business archives

要部署流程，必须将它们包装在业务存档中。 业务存档是Activiti Engine的部署单位。 业务存档等同于zip文件。 它可以包含BPMN 2.0流程，任务表单，规则和任何其他类型的文件。 通常，业务归档包含一组命名资源。

部署业务归档时，将扫描具有“.bpmn20.xml”或“.bpmn”扩展名的BPMN文件。 其中每个都将被解析，并可能包含多个流程定义。

|| 业务档案**中存在的Java类将不会添加到类路径**中。 业务归档中的流程定义中使用的所有自定义类（例如Java服务任务或事件侦听器实现）都应存在于Activiti Engine类路径中，以便运行流程。|
| ---- |-------------------------------------------------- ---------- |
|||

#### 6.1.1. Deploying programmatically

从zip文件部署业务存档可以这样做：

```
String barFileName = "path/to/process-one.bar";
ZipInputStream inputStream = new ZipInputStream(new FileInputStream(barFileName));

repositoryService.createDeployment()
    .name("process-one.bar")
    .addZipInputStream(inputStream)
    .deploy();
```

也可以从各个资源构建部署。 有关更多详细信息，请参阅javadocs。

### 6.2. External resources

流程定义存在于Activiti数据库中。 当使用Service Task或执行侦听器或Activiti配置文件中的Spring bean时，这些流程定义可以引用委托类。 这些类和Spring配置文件必须可用于可执行流程定义的所有流程引擎。

#### 6.2.1. Java classes

当进程的实例启动时，进程中使用的所有自定义类（例如，在Service Tasks或事件侦听器中使用的JavaDelegates，TaskListeners，...）都应该出现在引擎的类路径中。

但是，在部署业务归档期间，这些类不必存在于类路径中。 这意味着在使用Ant部署新的业务归档时，您的委派类不必位于类路径中。

当您使用演示设置并且想要添加自定义类时，应该将包含类的jar添加到activiti-explorer或activiti-rest webapp lib。 不要忘记包含自定义类（如果有）的依赖项。 或者，您可以将依赖项包含在Tomcat安装的库目录中，`$ {tomcat.home} / lib`。

#### 6.2.2. Using Spring beans from a process

当表达式或脚本使用Spring bean时，执行流程定义时，这些bean必须可供引擎使用。 如果您正在构建自己的webapp并按照[spring integration section]（https://www.activiti.org/userguide/#springintegration）中的描述在上下文中配置流程引擎，那很简单。 但请记住，如果您使用它，您还应该使用该上下文更新Activiti rest webapp。 您可以通过使用包含Spring上下文配置的`activiti-context.xml`文件替换`activiti-rest / lib / activiti-cfg.jar` JAR文件中的`activiti.cfg.xml`来实现。

#### 6.2.3. Creating a single app

而不是确保所有进程引擎在其类路径上都具有所有委托类并使用正确的Spring配置，您可以考虑在您自己的webapp中包含Activiti rest webapp，以便只有一个`ProcessEngine`。

### 6.3. Versioning of process definitions

BPMN没有版本控制的概念。这实际上很好，因为可执行的BPMN流程文件可能会作为开发项目的一部分存在于版本控制系统存储库（例如Subversion，Git或Mercurial）中。在部署期间创建流程定义的版本。在部署期间，Activiti将在将其存储在Activiti DB中之前为`ProcessDefinition`分配一个版本。

对于业务存档中的每个流程定义，执行以下步骤来初始化属性`key`，`version`，`name`和`id`：

- XML文件中的流程定义`id`属性用作流程定义`key`属性。
- XML文件中的流程定义`name`属性用作流程定义`name`属性。如果未指定name属性，则使用id属性作为名称。
- 第一次部署具有特定密钥的进程时，将分配版本1。对于具有相同密钥的流程定义的所有后续部署，版本将设置为高于当前部署的最大版本1。 key属性用于区分流程定义。
- id属性设置为{processDefinitionKey}：{processDefinitionVersion}：{generated-id}，其中`generated-id`是添加的唯一编号，用于保证集群环境中流程定义缓存的进程ID的唯一性。

以下面的过程为例

```
<definitions id="myDefinitions" >
  <process id="myProcess" name="My important process" >
    ...
```

When deploying this process definition, the process definition in the database will look like this:

| id              | key       | name                 | version |
| --------------- | --------- | -------------------- | ------- |
| myProcess:1:676 | myProcess | My important process | 1       |

Suppose we now deploy an updated version of the same process (e.g. changing some user tasks), but the `id` of the process definition remains the same. The process definition table will now contain the following entries:

| id              | key       | name                 | version |
| --------------- | --------- | -------------------- | ------- |
| myProcess:1:676 | myProcess | My important process | 1       |
| myProcess:2:870 | myProcess | My important process | 2       |

当调用`runtimeService.startProcessInstanceByKey（“myProcess”）`时，它现在将使用版本为“2”的进程定义，因为这是进程定义的最新版本。

我们是否应该创建第二个流程（如下所述）并将其部署到Activiti，第三行将添加到表中。

```
<definitions id="myNewDefinitions" >
  <process id="myNewProcess" name="My important process" >
    ...
```

The table will look like this:

| id                  | key          | name                 | version |
| ------------------- | ------------ | -------------------- | ------- |
| myProcess:1:676     | myProcess    | My important process | 1       |
| myProcess:2:870     | myProcess    | My important process | 2       |
| myNewProcess:1:1033 | myNewProcess | My important process | 1       |

请注意新流程的密钥与第一个流程的不同之处。 即使名称相同（我们也应该改变它），Activiti在区分进程时只考虑`id`属性。 因此，新进程部署在版本1中。

### 6.4. Providing a process diagram

可以将流程图图像添加到部署中。此图像将存储在Activiti存储库中，并可通过API访问。此图像还用于在Activiti Explorer中显示该过程。

假设我们的类路径上有一个进程，`org / activiti / expenseProcess.bpmn20.xml`，它有一个进程密钥* expense *。流程图图像的以下命名约定适用（按此特定顺序）：

- 如果部署中存在具有与进程密钥和图像后缀连接的BPMN 2.0 XML文件名的名称的映像资源，则使用此映像。在我们的例子中，这将是`org / activiti / expenseProcess.expense.png`（或.jpg / gif）。如果您在一个BPMN 2.0 XML文件中定义了多个图像，则此方法最有意义。然后，每个图表图像将在其文件名中包含进程密钥。
- 如果不存在此类映像，则搜索与BPMN 2.0 XML文件的名称匹配的部署中的映像资源。在我们的例子中，这将是`org / activiti / expenseProcess.png`。请注意，这意味着**在同一BPMN 2.0文件中定义的每个流程定义**都具有相同的流程图图像。如果每个BPMN 2.0 XML文件中只有一个流程定义，这显然不是问题。

以编程方式部署的示例：

```
repositoryService.createDeployment()
  .name("expense-process.bar")
  .addClasspathResource("org/activiti/expenseProcess.bpmn20.xml")
  .addClasspathResource("org/activiti/expenseProcess.png")
  .deploy();
```

The image resource can be retrieved through the API afterwards:

```
ProcessDefinition processDefinition = repositoryService.createProcessDefinitionQuery()
  .processDefinitionKey("expense")
  .singleResult();

String diagramResourceName = processDefinition.getDiagramResourceName();
InputStream imageStream = repositoryService.getResourceAsStream(
    processDefinition.getDeploymentId(), diagramResourceName);
```

### 6.5. Generating a process diagram

如果部署中未提供映像，如[上一节]（https://www.activiti.org/userguide/#providingProcessDiagram）中所述，如果流程定义包含必要的，则Activiti引擎将生成图表图像 *图表交换*信息。

可以使用与部署中[提供图像]（https://www.activiti.org/userguide/#providingProcessDiagram）时完全相同的方式检索资源。

！[deployment.image.generation（https://www.activiti.org/userguide/images/deployment.image.generation.png）

如果由于某种原因，在部署期间没有必要或想要生成图表，则可以在流程引擎配置上设置`isCreateDiagramOnDeploy`属性：

```
<property name =“createDiagramOnDeploy”value =“false”/>
```

现在不会生成图表。

### 6.6. Category

部署和流程定义都具有用户定义的类别。 流程定义类别是BPMN文件中属性的初始化值：`<definitions ... targetNamespace =“yourCategory”...`

可以在API中指定部署类别，如下所示：

```
repositoryService
    .createDeployment()
    .category("yourCategory")
    ...
    .deploy();
```

## 7. BPMN 2.0 Introduction

### 7.1. What is BPMN?

请参阅我们的[BPMN 2.0常见问题解答条目]（http://activiti.org/faq.html#WhatIsBpmn20）。

### 7.2。定义一个过程

| |本简介是在您使用[Eclipse IDE]（http://eclipse.org/）创建和编辑文件的假设下编写的。但是，这很少是特定于Eclipse的。您可以使用您喜欢的任何其他工具来创建包含BPMN 2.0的XML文件。 |
| ---- | -------------------------------------------------- ---------- |
| | |

创建一个新的XML文件（*右键单击任何项​​目并选择New→Other→XML-XML File *）并为其命名。确保文件**以.bpmn20.xml或.bpmn **结尾，否则引擎将不会选择此文件进行部署。

！[new.bpmn.procdef（https://www.activiti.org/userguide/images/new.bpmn.procdef.png）

BPMN 2.0模式的根元素是`definitions`元素。在此元素中，可以定义多个流程定义（尽管我们建议每个文件中只有一个流程定义，因为这样可以在开发过程的后期简化维护）。空进程定义如下所示。请注意，最小的`definitions`元素只需要`xmlns`和`targetNamespace`声明。 `targetNamespace`可以是任何东西，对于分类流程定义很有用。
```
<definitions
  xmlns="http://www.omg.org/spec/BPMN/20100524/MODEL"
  xmlns:activiti="http://activiti.org/bpmn"
  targetNamespace="Examples">

  <process id="myProcess" name="My First Process">
    ..
  </process>

</definitions>
```

（可选）您还可以添加BPMN 2.0 XML架构的联机架构位置，以替代Eclipse中的XML目录配置。

```
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://www.omg.org/spec/BPMN/20100524/MODEL
                    http://www.omg.org/spec/BPMN/2.0/20100501/BPMN20.xsd
```

`process`元素有两个属性：

- ** id **：此属性是**必需**并映射到Activiti`ProcessDefinition`对象的** key **属性。然后，可以通过`RuntimeService`上的`startProcessInstanceByKey`方法使用此`id`来启动流程定义的新流程实例。此方法将始终采用流程定义的**最新部署版本**。

```
ProcessInstance processInstance = runtimeService.startProcessInstanceByKey（“myProcess”）;
```

- 这里需要注意的是，这与调用`startProcessInstanceById`方法不同。此方法需要Activiti引擎在部署时生成的String id，并且可以通过调用`processDefinition.getId（）`方法来检索。生成的id的格式为** key：version **，长度为**约束为64个字符**。如果你得到一个`ActivitiException`声明生成的id太长，则限制进程的* key *字段中的文本。
- ** name **：此属性是**可选**并映射到`ProcessDefinition`的* name *属性。引擎本身不使用此属性，因此它可用于在用户界面中显示更人性化的名称。

[[10minutetutorial]]

### 7.3. Getting started: 10 minute tutorial

在本节中，我们将介绍一个（非常简单的）业务流程，我们将使用它来介绍一些基本的Activiti概念和Activiti API。

#### 7.3.1. Prerequisites

本教程假设您正在运行[Activiti演示设置]（https://www.activiti.org/userguide/#demo.setup.one.minute.version），并且您正在使用独立的H2服务器。 编辑`db.properties`并设置`jdbc.url = jdbc：h2：tcp：// localhost / activiti`，然后根据[H2的文档]运行独立服务器（http://www.h2database.com/HTML/ tutorial.html＃using_server）。

#### 7.3.2. Goal

本教程的目标是了解Activiti和一些基本的BPMN 2.0概念。 最终结果将是一个简单的Java SE程序，它部署流程定义，并通过Activiti引擎API与此流程交互。 我们还将触及Activiti周围的一些工具。 当然，在围绕业务流程构建自己的Web应用程序时，也可以使用本教程中学到的内容。

#### 7.3.3. Use case

用例很简单：我们有一家公司，我们称之为BPMCorp。 在BPMCorp，每个月都需要为公司股东撰写财务报告。 这是会计部门的责任。 报告完成后，高层管理人员中的一位成员需要在将文件发送给所有股东之前批准该文件。

#### 7.3.4. Process diagram

可以使用[Activiti Designer]（https://www.activiti.org/userguide/#activitiDesigner）以图形方式显示上述业务流程。 但是，对于本教程，我们将自己键入XML，因为我们在这一点上以这种方式学习最多。 我们流程的图形化BPMN 2.0表示法如下所示：

！[financial.report.example.diagram（https://www.activiti.org/userguide/images/financial.report.example.diagram.png）

我们看到的是[无启动事件]（https://www.activiti.org/userguide/#bpmnNoneStartEvent）（左边的圆圈），然后是两个[用户任务]（https://www.activiti.org / userguide / #bpmnUserTask）：*'写月度财务报告'*和*'验证月度财务报告'*，以[无结束事件]结尾（https://www.activiti.org/userguide/#bpmnNoneEndEvent）（ 右边有粗边框的圆圈）。

#### 7.3.5. XML representation

此业务流程的XML版本（* FinancialReportProcess.bpmn20.xml *）如下所示。很容易识别我们流程的主要元素（点击链接转到该BPMN 2.0构造的详细部分）：

- [（无）启动事件]（https://www.activiti.org/userguide/#bpmnNoneStartEvent）告诉我们该流程的*入口点*是什么。
- [用户任务]（https://www.activiti.org/userguide/#bpmnUserTask）声明是我们流程的人工任务的表示。请注意，第一个任务分配给* accountancy *组，而第二个任务分配给* management *组。有关如何将用户和组分配给用户任务的详细信息，请参阅[有关用户任务分配的部分]（https://www.activiti.org/userguide/#bpmnUserTaskAssignment）。
- 当到达[无结束事件]（https://www.activiti.org/userguide/#bpmnNoneEndEvent）时，该过程结束。
- 元素通过[序列流]（https://www.activiti.org/userguide/#bpmnSequenceFlow）相互连接。这些序列流有一个“source”和“target”，定义了序列流的* direction *。

```
<definitions id="definitions"
  targetNamespace="http://activiti.org/bpmn20"
  xmlns:activiti="http://activiti.org/bpmn"
  xmlns="http://www.omg.org/spec/BPMN/20100524/MODEL">

	<process id="financialReport" name="Monthly financial report reminder process">

	  <startEvent id="theStart" />

	  <sequenceFlow id="flow1" sourceRef="theStart" targetRef="writeReportTask" />

	  <userTask id="writeReportTask" name="Write monthly financial report" >
	    <documentation>
	      Write monthly financial report for publication to shareholders.
	    </documentation>
	    <potentialOwner>
	      <resourceAssignmentExpression>
	        <formalExpression>accountancy</formalExpression>
	      </resourceAssignmentExpression>
	    </potentialOwner>
	  </userTask>

	  <sequenceFlow id="flow2" sourceRef="writeReportTask" targetRef="verifyReportTask" />

	  <userTask id="verifyReportTask" name="Verify monthly financial report" >
	    <documentation>
	      Verify monthly financial report composed by the accountancy department.
	      This financial report is going to be sent to all the company shareholders.
	    </documentation>
	    <potentialOwner>
	      <resourceAssignmentExpression>
	        <formalExpression>management</formalExpression>
	      </resourceAssignmentExpression>
	    </potentialOwner>
	  </userTask>

	  <sequenceFlow id="flow3" sourceRef="verifyReportTask" targetRef="theEnd" />

	  <endEvent id="theEnd" />

	</process>

</definitions>
```

#### 7.3.6. Starting a process instance

我们现在已经创建了业务流程的**流程定义**。从这样的流程定义，我们可以创建**流程实例**。在这种情况下，一个流程实例将与特定月份的单个财务报告的创建和验证相匹配。所有流程实例共享相同的流程定义。

为了能够从给定的流程定义创建流程实例，我们必须首先**部署**此流程定义。部署流程定义意味着两件事：

- 流程定义将存储在为Activiti引擎配置的持久数据存储中。因此，通过部署我们的业务流程，我们确保引擎在引擎重启后找到流程定义。
- BPMN 2.0流程文件将被解析为内存中的对象模型，可以通过Activiti API进行操作。

有关部署的更多信息可以在[部署专用部分]（https://www.activiti.org/userguide/#chDeployment）中找到。

如该部分所述，部署可以通过多种方式进行。一种方法是通过API如下。请注意，与Activiti引擎的所有交互都通过其* services *进行。

```
Deployment deployment = repositoryService.createDeployment()
  .addClasspathResource("FinancialReportProcess.bpmn20.xml")
  .deploy();
```

现在我们可以使用在流程定义中定义的`id`来启动一个新的流程实例（请参阅XML文件中的流程元素）。 请注意，Activiti术语中的“id”称为**键**。

```
ProcessInstance processInstance = runtimeService.startProcessInstanceByKey("financialReport");
```

这将创建一个首先通过start事件的流程实例。在启动事件之后，它遵循所有传出的顺序流（在这种情况下只有一个）并且达到第一个任务（*写月度财务报告*）。 Activiti引擎现在将任务存储在持久数据库中。此时，附加到任务的用户或组分配将被解析并存储在数据库中。值得注意的是，Activiti引擎将继续执行流程执行步骤，直到达到*等待状态*，例如用户任务。在这种等待状态下，流程实例的当前状态存储在数据库中。在用户决定完成任务之前，它仍处于该状态。此时，引擎将继续运行，直到达到新的等待状态或进程结束。当引擎在此期间重新启动或崩溃时，该过程的状态在数据库中是安全的。

创建任务后，将返回`startProcessInstanceByKey`方法，因为用户任务活动是*等待状态*。在这种情况下，任务被分配给一个组，这意味着该组的每个成员都是**候选**来执行该任务。

我们现在可以将所有这些放在一起并创建一个简单的Java程序。创建一个新的Eclipse项目，并将Activiti JAR和依赖项添加到其类路径中（这些可以在Activiti发行版的* libs *文件夹中找到）。在我们调用Activiti服务之前，我们必须首先构建一个`ProcessEngine`，它允许我们访问服务。这里我们使用*'standalone'*配置，它构造一个`ProcessEngine`，它使用在演示设置中也使用的数据库。

您可以下载流程定义XML [此处]（https://www.activiti.org/userguide/images/FinancialReportProcess.bpmn20.xml）。此文件包含如上所示的XML，但还包含必要的BPMN [图表交换信息]（https://www.activiti.org/userguide/#generatingProcessDiagram），以便在Activiti工具中可视化该过程。

```
public static void main(String[] args) {

  // Create Activiti process engine
  ProcessEngine processEngine = ProcessEngineConfiguration
    .createStandaloneProcessEngineConfiguration()
    .buildProcessEngine();

  // Get Activiti services
  RepositoryService repositoryService = processEngine.getRepositoryService();
  RuntimeService runtimeService = processEngine.getRuntimeService();

  // Deploy the process definition
  repositoryService.createDeployment()
    .addClasspathResource("FinancialReportProcess.bpmn20.xml")
    .deploy();

  // Start a process instance
  runtimeService.startProcessInstanceByKey("financialReport");
}
```

#### 7.3.7. Task lists

我们现在可以通过`TaskService`通过添加以下逻辑来检索此任务：

```
List<Task> tasks = taskService.createTaskQuery().taskCandidateUser("kermit").list();
```

请注意，我们传递给此操作的用户需要是* accountancy *组的成员，因为它是在流程定义中声明的：

```
<potentialOwner>
  <resourceAssignmentExpression>
    <formalExpression>accountancy</formalExpression>
  </resourceAssignmentExpression>
</potentialOwner>
```

我们还可以使用任务查询API来使用组的名称获得相同的结果。 我们现在可以在代码中添加以下逻辑：

```
TaskService taskService = processEngine.getTaskService();
List<Task> tasks = taskService.createTaskQuery().taskCandidateGroup("accountancy").list();
```

由于我们已将`ProcessEngine`配置为使用与演示设置正在使用的相同的数据库，因此我们现在可以登录[Activiti Explorer]（http：// localhost：8080 / activiti-explorer /）。默认情况下，* accountancy *组中没有用户。使用kermit / kermit登录，单击“组”，然后单击“创建组”。然后单击“用户”并将该组添加到fozzie。现在使用fozzie / fozzie登录，我们会发现我们可以在选择* Processes *页面并点击与''每月对应的*'Actions'*列中的*'Start Process'*链接后开始我们的业务流程财务报告'*流程。

！[bpmn.financial.report.example.start.process（https://www.activiti.org/userguide/images/bpmn.financial.report.example.start.process.png）

如上所述，该过程将执行第一个用户任务。由于我们以kermit身份登录，因此我们可以看到在我们启动流程实例后，有一个新的候选任务可供他使用。选择* Tasks *页面以查看此新任务。请注意，即使该进程是由其他人启动的，该任务仍然可以作为候选任务显示给会计组中的每个人。

！[bpmn.financial.report.example.task.assigned（https://www.activiti.org/userguide/images/bpmn.financial.report.example.task.assigned.png）

#### 7.3.8. Claiming the task

现在需要**声明任务**。 通过声明任务，特定用户将成为任务的**受让人**，并且任务将从会计组的其他成员的每个任务列表中消失。 声明任务是以编程方式完成的，如下所示：
```
taskService.claim(task.getId(), "fozzie");
```

该任务现在位于声明任务**的**个人任务列表中。

```
List<Task> tasks = taskService.createTaskQuery().taskAssignee("fozzie").list();
```

在Activiti UI App中，单击* claim *按钮将调用相同的操作。 该任务现在将移至登录用户的个人任务列表。 您还会看到任务的受理人已更改为当前登录的用户。

！[bpmn.financial.report.example.claim.task（https://www.activiti.org/userguide/images/bpmn.financial.report.example.claim.task.png）

#### 7.3.9. Completing the task

会计师现在可以开始编制财务报告。 报告完成后，他可以**完成任务**，这意味着完成该任务的所有工作。

```
taskService.complete(task.getId());
```

对于Activiti引擎，这是一个外部信号，必须继续执行流程实例。 任务本身将从运行时数据中删除。 遵循任务的单个传出转换，将执行移至第二个任务（*'验证报告'*）。 现在将使用与第一个任务描述的机制相同的机制来分配第二个任务，但差别很小，即任务将分配给* management *组。

在演示设置中，单击任务列表中的* complete *按钮即可完成任务。 由于Fozzie不是会计师，我们需要退出Activiti Explorer并以* kermit *（谁是经理）登录。 现在，第二个任务在未分配的任务列表中可见。

#### 7.3.10. Ending the process

可以以与以前完全相同的方式检索和声明验证任务。 完成第二个任务会将流程执行移至结束事件，从而完成流程实例。 将从数据存储中删除流程实例和所有相关的运行时执行数据。

当您登录Activiti Explorer时，您可以验证这一点，因为在存储流程执行的表中不会找到任何记录。

！[bpmn.financial.report.example.process.ended（https://www.activiti.org/userguide/images/bpmn.financial.report.example.process.ended.png）

在编程方面，您还可以使用`historyService`验证进程是否已结束

```
HistoryService historyService = processEngine.getHistoryService();
HistoricProcessInstance historicProcessInstance =
historyService.createHistoricProcessInstanceQuery().processInstanceId(procId).singleResult();
System.out.println("Process instance end time: " + historicProcessInstance.getEndTime());
```

#### 7.3.11. Code overview

结合前面部分的所有片段，你应该有这样的东西（这段代码考虑到你可能会通过Activiti Explorer UI启动一些流程实例。因此，它总是检索一个任务列表而不是一个 任务，所以它总是有效）：

```
public class TenMinuteTutorial {

  public static void main(String[] args) {

    // Create Activiti process engine
    ProcessEngine processEngine = ProcessEngineConfiguration
      .createStandaloneProcessEngineConfiguration()
      .buildProcessEngine();

    // Get Activiti services
    RepositoryService repositoryService = processEngine.getRepositoryService();
    RuntimeService runtimeService = processEngine.getRuntimeService();

    // Deploy the process definition
    repositoryService.createDeployment()
      .addClasspathResource("FinancialReportProcess.bpmn20.xml")
      .deploy();

    // Start a process instance
    String procId = runtimeService.startProcessInstanceByKey("financialReport").getId();

    // Get the first task
    TaskService taskService = processEngine.getTaskService();
    List<Task> tasks = taskService.createTaskQuery().taskCandidateGroup("accountancy").list();
    for (Task task : tasks) {
      System.out.println("Following task is available for accountancy group: " + task.getName());

      // claim it
      taskService.claim(task.getId(), "fozzie");
    }

    // Verify Fozzie can now retrieve the task
    tasks = taskService.createTaskQuery().taskAssignee("fozzie").list();
    for (Task task : tasks) {
      System.out.println("Task for fozzie: " + task.getName());

      // Complete the task
      taskService.complete(task.getId());
    }

    System.out.println("Number of tasks for fozzie: "
            + taskService.createTaskQuery().taskAssignee("fozzie").count());

    // Retrieve and claim the second task
    tasks = taskService.createTaskQuery().taskCandidateGroup("management").list();
    for (Task task : tasks) {
      System.out.println("Following task is available for management group: " + task.getName());
      taskService.claim(task.getId(), "kermit");
    }

    // Completing the second task ends the process
    for (Task task : tasks) {
      taskService.complete(task.getId());
    }

    // verify that the process is actually finished
    HistoryService historyService = processEngine.getHistoryService();
    HistoricProcessInstance historicProcessInstance =
      historyService.createHistoricProcessInstanceQuery().processInstanceId(procId).singleResult();
    System.out.println("Process instance end time: " + historicProcessInstance.getEndTime());
  }

}
```

#### 7.3.12. Future enhancements

很容易看出这个业务流程太简单，无法在现实中使用。 但是，当您浏览Activiti中提供的BPMN 2.0构造时，您将能够通过以下方式增强业务流程：

 - 定义作为决策的**网关**。 这样，经理可以拒绝重建会计任务的财务报告。
 - 声明和使用**变量**，以便我们可以存储或引用报告，以便可以在表单中显示它。
 - 在流程结束时定义**服务任务**，将报告发送给每个股东。
 - ...

## 8. BPMN 2.0 Constructs

本章介绍Activiti支持的BPMN 20构造以及BPMN标准的自定义扩展。

### 8.1. Custom extensions

BPMN 2.0标准对所有相关方都是一件好事。最终用户不会受到供应商锁定的影响，具体取决于专有解决方案。框架，特别是像Activiti这样的开源框架，可以实现与大型供应商具有相同（通常更好实现;-)功能的解决方案。由于BPMN 2.0标准，从这样一个大型供应商解决方案向Activiti的过渡是一个简单而平稳的路径。

然而，标准的缺点在于，它始终是不同公司（通常是愿景）之间的许多讨论和妥协的结果。作为开发人员阅读流程定义的BPMN 2.0 XML，有时候感觉某些结构或做事方式太麻烦。由于Activiti将易用性作为首要任务，因此我们推出了名为** Activiti BPMN扩展**的东西。这些*扩展*是新构造或简化某些不在BPMN 2.0规范中的构造的方法。

尽管BPMN 2.0规范明确指出它是为自定义扩展而制定的，但我们确保：

- 这种自定义扩展的先决条件是**总是**必须是**标准的做事方式的简单转换**。因此，当您决定使用自定义扩展时，您不必害怕无法回复。
- 使用自定义扩展时，通过给出新的XML元素，属性等** activiti：**命名空间前缀，可以清楚地表明这一点。

因此，无论您是否想要使用自定义扩展程序，完全取决于您。有几个因素会影响这个决定（图形编辑器使用，公司政策等）。我们只提供它们，因为我们相信标准中的某些要点可以更简单或更有效。您可以随意向我们（积极和/或消极）反馈我们的扩展程序，或发布自定义扩展程序的新想法。谁知道，有一天你的想法可能会出现在规范中！

### 8.2. Events

事件用于模拟在生命周期过程中发生的事情。 事件总是可视化为圆圈。 在BPMN 2.0中，存在两个主要事件类别：* catch *或* throwing * event。

 -  **捕获：**当流程执行到达事件时，它将等待触发发生。 触发器的类型由XML中的内部图标或类型声明定义。 捕捉事件通过未填充的内部图标（即，它是白色）在视觉上区别于投掷事件。
 -  **投掷：**当进程执行到达事件时，触发器被触发。 触发器的类型由XML中的内部图标或类型声明定义。 通过填充黑色的内部图标在视觉上区分投掷事件。

#### 8.2.1. Event Definitions

事件定义定义事件的语义。 没有事件定义，事件“没有什么特别之处”。 例如，没有和事件定义的启动事件没有指定启动过程的确切内容。 如果我们向start事件添加一个事件定义（比如一个timer事件定义），我们声明事件的“type”启动过程（在定时器事件定义的情况下，达到某个时间点的事实）。

#### 8.2.2. Timer Event Definitions

定时器事件是由定义的定时器触发的事件。 它们可以用作[开始事件]（https://www.activiti.org/userguide/#bpmnTimerStartEvent），[中间事件]（https://www.activiti.org/userguide/#bpmnIntermediateCatchingEvent）或[边界事件]]（https://www.activiti.org/userguide/#bpmnTimerBoundaryEvent）。 时间事件的行为取决于使用的业务日历。 每个计时器事件都有一个默认的业务日历，但业务日历也可以在计时器事件定义中定义。

```
<timerEventDefinition activiti:businessCalendarName="custom">
    ...
</timerEventDefinition>
```

businessCalendarName指向流程引擎配置中的业务日历的位置。 省略业务日历时，将使用默认业务日历。

定时器定义必须具有以下一个元素：

 -  ** timeDate **。 当触发器被触发时，此格式以[ISO 8601]（http://en.wikipedia.org/wiki/ISO_8601#Dates）格式指定固定日期。 例：

```
<timerEventDefinition>
    <timeDate>2011-03-11T12:13:14</timeDate>
</timerEventDefinition>
```

 -  ** timeDuration **。 要指定计时器在触发之前应运行多长时间，可以将* timeDuration *指定为* timerEventDefinition *的子元素。 使用的格式是[ISO 8601]（http://en.wikipedia.org/wiki/ISO_8601#Durations）格式（根据BPMN 2.0规范的要求）。 示例（间隔持续10天）：

```
<timerEventDefinition>
    <timeDuration>P10D</timeDuration>
</timerEventDefinition>
```

 -  ** timeCycle **。 指定重复间隔，这对于定期启动进程或为过期用户任务发送多个提醒非常有用。 时间周期元素可以是两种格式。 首先是[ISO 8601]（http://en.wikipedia.org/wiki/ISO_8601#Repeating_intervals）标准规定的经常性持续时间的格式。 示例（3个重复间隔，每个持续10个小时）：

还可以将* endDate *指定为* timeCycle *上的可选属性，或者在时间表达式的末尾指定如下：`R3 / PT10H / $ {EndDate}`。 到达endDate时，应用程序将停止为此任务创建其他作业。 它接受静态值[ISO 8601]（http://en.wikipedia.org/wiki/ISO_8601#Dates）标准，例如*“2015-02-25T16：42：11 + 00:00”*或变量* ${结束日期}*

```
<timerEventDefinition>
    <timeCycle activiti:endDate="2015-02-25T16:42:11+00:00">R3/PT10H</timeCycle>
</timerEventDefinition>

<timerEventDefinition>
    <timeCycle>R3/PT10H/${EndDate}</timeCycle>
</timerEventDefinition>
```

如果同时指定了两者，则系统将使用指定为属性的endDate。

目前只有* BoundaryTimerEvents *和* CatchTimerEvent *支持* EndDate *功能。

此外，您可以使用cron表达式指定时间周期，下面的示例显示每5分钟触发一次，从完整小时开始：

```
0 0/5 * * * ?
```

有关使用cron表达式的信息，请参阅[本教程]（http://www.quartz-scheduler.org/docs/tutorials/crontrigger.html）。

**注意：**第一个符号表示秒，而不是正常Unix cron中的分钟。

经常性持续时间更适合处理相对定时器，相对于某个特定时间点（例如，用户任务启动的时间）计算相对定时器，而cron表达式可以处理绝对定时器 - 这对于[定时器启动事件特别有用]]（https://www.activiti.org/userguide/#timerStartEventDescription）。

您可以将表达式用于计时器事件定义，这样您就可以根据流程变量影响计时器定义。 对于适当的计时器类型，过程变量必须包含ISO 8601（或循环类型的cron）字符串。

```
<boundaryEvent id="escalationTimer" cancelActivity="true" attachedToRef="firstLineSupport">
  <timerEventDefinition>
    <timeDuration>${duration}</timeDuration>
  </timerEventDefinition>
</boundaryEvent>
```

**注意：**定时器仅在启用作业或异步执行程序时触发（即* jobEEecutorActivate *或* asyncExecutorActivate *需要在`activiti.cfg.xml`中设置为`true`，因为作业和异步 执行程序默认禁用）。

#### 8.2.3. Error Event Definitions

**重要说明：** BPMN错误与Java异常不同。 事实上，两者没有任何共同之处。 BPMN错误事件是一种建模*业务异常*的方法。 Java异常以[他们自己的特定方式]处理（https://www.activiti.org/userguide/#serviceTaskExceptionHandling）。

```
<endEvent id="myErrorEndEvent">
  <errorEventDefinition errorRef="myError" />
</endEvent>
```

#### 8.2.4. Signal Event Definitions

信号事件是引用命名信号的事件。 信号是全局范围的事件（广播语义），并传递给所有活动的处理程序（等待进程实例/捕获信号事件）。

使用`signalEventDefinition`元素声明信号事件定义。 属性`signalRef`引用`signal`element，声明为`definitions`根元素的子元素。 以下是一个过程的摘录，其中信号事件被中间事件抛出并捕获。

```
<definitions... >
	<!-- declaration of the signal -->
	<signal id="alertSignal" name="alert" />

	<process id="catchSignal">
		<intermediateThrowEvent id="throwSignalEvent" name="Alert">
			<!-- signal event definition -->
			<signalEventDefinition signalRef="alertSignal" />
		</intermediateThrowEvent>
		...
		<intermediateCatchEvent id="catchSignalEvent" name="On Alert">
			<!-- signal event definition -->
			<signalEventDefinition signalRef="alertSignal" />
		</intermediateCatchEvent>
		...
	</process>
</definitions>
```

`signalEventDefinition`引用相同的`signal`元素。

##### Throwing a Signal Event

信号可以由流程实例使用BPMN构造抛出，也可以使用java API以编程方式抛出。 `org.activiti.engine.RuntimeService`上的以下方法可用于以编程方式抛出信号：

```
RuntimeService.signalEventReceived(String signalName);
RuntimeService.signalEventReceived(String signalName, String executionId);
```

`signalEventReceived（String signalName）;`和`signalEventReceived（String signalName，String executionId）;`之间的区别在于第一种方法将信号全局抛出到所有订阅的处理程序（广播语义），第二种方法将信号传递给特定的 只执行。

##### Catching a Signal Event

信号事件可以被中间捕获信号事件或信号边界事件捕获。

##### Querying for Signal Event subscriptions

可以查询已订阅特定信号事件的所有执行：

```
 List<Execution> executions = runtimeService.createExecutionQuery()
      .signalEventSubscriptionName("alert")
      .list();
```

然后我们可以使用`signalEventReceived（String signalName，String executionId）`方法将信号传递给这些执行。

##### Signal event scope

默认情况下，信号是*广播流程引擎范围*。 这意味着您可以在流程实例中抛出信号事件，而具有不同流程定义的其他流程实例可以对此事件的发生做出反应。

但是，有时需要仅在*相同的流程实例*内对信号事件作出反应。 例如，如果两个或多个活动是互斥的，则用例是流程实例中的同步机制。

要限制信号事件的* scope *，请将（非BPMN 2.0标准！）*范围属性*添加到信号事件定义中：

```
<signal id="alertSignal" name="alert" activiti:scope="processInstance"/>
```

此属性的默认值为*“global”*。

##### Signal Event example(s)

以下是使用信号进行通信的两个独立进程的示例。如果更新或更改保险单，则启动第一个流程。在人类参与者审查了更改后，将抛出信号事件，表明策略已更改：

！[bpmn.signal.event.throw（https://www.activiti.org/userguide/images/bpmn.signal.event.throw.png）

现在，所有感兴趣的流程实例都可以捕获此事件。以下是订阅该事件的流程示例。

！[bpmn.signal.event.catch（https://www.activiti.org/userguide/images/bpmn.signal.event.catch.png）

**注意：**重要的是要了解信号事件是否向**所有**活动处理程序广播。这意味着在上面给出的示例的情况下，捕获信号的过程的所有实例都将接收事件。在这种情况下，这就是我们想要的。但是，也存在无意中广播行为的情况。请考虑以下过程：

！[bpmn.signal.event.warning.1（https://www.activiti.org/userguide/images/bpmn.signal.event.warning.1.png）

BPMN不支持上述过程中描述的模式。这个想法是，执行“执行某事”任务时抛出的错误被边界错误事件捕获，并将使用信号throw事件传播到并行执行路径，然后中断“并行执行”任务。到目前为止，Activiti将按预期执行。信号将传播到捕获边界事件并中断任务。 **但是，由于信号的广播语义，它也会传播到已订阅信号事件的所有其他进程实例。**在这种情况下，这可能不是我们想要的。

**注意：**信号事件不会对特定流程实例执行任何类型的关联。相反，它会广播到所有流程实例。如果您只需要向特定流程实例发送信号，请手动执行关联并使用`signalEventReceived（String signalName，String executionId）和相应的[查询机制]（https://www.activiti.org/userguide/# bpmnSignalEventDefinitionQuery）。

通过将* scope *属性添加到signal事件并将其设置为* processInstance *，Activiti确实有办法解决这个问题。

#### 8.2.5. Message Event Definitions

消息事件是引用命名消息的事件。 消息具有名称和有效负载。 与信号不同，消息事件始终指向单个接收器。

使用`messageEventDefinition`元素声明消息事件定义。 属性`messageRef`引用一个`message`元素，声明为`definitions`根元素的子元素。 以下是一个过程的摘录，其中两个消息事件由start事件和中间捕获消息事件声明和引用。

```
<definitions id="definitions"
  xmlns="http://www.omg.org/spec/BPMN/20100524/MODEL"
  xmlns:activiti="http://activiti.org/bpmn"
  targetNamespace="Examples"
  xmlns:tns="Examples">

  <message id="newInvoice" name="newInvoiceMessage" />
  <message id="payment" name="paymentMessage" />

  <process id="invoiceProcess">

    <startEvent id="messageStart" >
    	<messageEventDefinition messageRef="newInvoice" />
    </startEvent>
    ...
    <intermediateCatchEvent id="paymentEvt" >
    	<messageEventDefinition messageRef="payment" />
    </intermediateCatchEvent>
    ...
  </process>

</definitions>
```

##### Throwing a Message Event

作为一个可嵌入的流程引擎，Activiti并不关心实际接收消息。 这将取决于环境，并且需要特定于平台的活动，例如连接到JMS（Java消息服务）队列/主题或处理Web服务或REST请求。 因此，接收消息是您必须实现的过程引擎嵌入的应用程序或基础结构的一部分。

在应用程序中收到消息后，您必须决定如何处理它。 如果消息应触发新流程实例的启动，请在运行时服务提供的以下方法之间进行选择：

```
ProcessInstance startProcessInstanceByMessage(String messageName);
ProcessInstance startProcessInstanceByMessage(String messageName, Map<String, Object> processVariables);
ProcessInstance startProcessInstanceByMessage(String messageName, String businessKey, Map<String, Object> processVariables);
```

这些方法允许使用引用的消息启动流程实例。

如果消息需要由现有流程实例接收，则首先必须将消息关联到特定流程实例（请参阅下一节），然后触发等待执行的继续。 运行时服务提供以下方法，用于根据消息事件订阅触发执行：

```
void messageEventReceived(String messageName, String executionId);
void messageEventReceived(String messageName, String executionId, HashMap<String, Object> processVariables);
```

#####查询消息事件订阅

 - 在消息启动事件的情况下，消息事件订阅与特定的*进程定义*相关联。 可以使用`ProcessDefinitionQuery`查询此类消息订阅：

```
ProcessDefinition processDefinition = repositoryService.createProcessDefinitionQuery()
      .messageEventSubscription("newCallCenterBooking")
      .singleResult();
```

由于特定邮件订阅只能有一个流程定义，因此查询始终返回零或一个结果。 如果更新了流程定义，则只有最新版本的流程定义才能订阅消息事件。

 - 在中间捕获消息事件的情况下，消息事件订阅与特定的*执行*相关联。 可以使用`ExecutionQuery`查询此类消息事件订阅：

```
Execution execution = runtimeService.createExecutionQuery()
      .messageEventSubscriptionName("paymentReceived")
      .variableValueEquals("orderId", message.getOrderId())
      .singleResult();
```

此类查询称为相关查询，通常需要有关进程的知识（在这种情况下，给定orderId最多只有一个流程实例）。

##### Message Event example(s)

以下是可以使用两个不同消息启动的进程示例：

！[bpmn.start.message.event.example.1（https://www.activiti.org/userguide/images/bpmn.start.message.event.example.1.png）

如果流程需要替代方法来响应不同的启动事件但最终以统一的方式继续，这将非常有用。

#### 8.2.6. Start Events

开始事件表示进程的开始位置。 启动事件的类型（进程在消息到达时开始，在特定时间间隔等开始），定义*如何*启动进程在事件的可视化表示中显示为一个小图标。 在XML表示中，类型由子元素的声明给出。

开始事件**总是捕捉**：从概念上讲，事件是（在任何时候）等待直到某个触发发生。

在启动事件中，可以指定以下特定于Activiti的属性：

 -  ** initiator **：标识进程启动时将在其中存储经过身份验证的用户标识的变量名称。 例：

```
<startEvent id="request" activiti:initiator="initiator" />
```

必须在try-finally块中使用方法`IdentityService.setAuthenticatedUserId（String）`设置经过身份验证的用户，如下所示：

```
try {
  identityService.setAuthenticatedUserId("bono");
  runtimeService.startProcessInstanceByKey("someProcessKey");
} finally {
  identityService.setAuthenticatedUserId(null);
}
```

此代码将附加到Activiti Explorer应用程序中。 因此它与[Forms]（https://www.activiti.org/userguide/#forms）结合使用。

#### 8.2.7. None Start Event

#####说明

A * none * start事件在技术上意味着未指定启动流程实例的触发器。 这意味着引擎无法预测何时必须启动流程实例。 通过调用* startProcessInstanceByXXX *方法之一，通过API启动流程实例时，将使用none start事件。

```
ProcessInstance processInstance = runtimeService.startProcessInstanceByXXX();
```

*注意：*子进程始终具有无启动事件。

#####图形符号

无启动事件可视化为没有内部图标的圆（即没有触发类型）。

！[bpmn.none.start.event（https://www.activiti.org/userguide/images/bpmn.none.start.event.png）

##### XML表示

无启动事件的XML表示是正常的启动事件声明，没有任何子元素（其他启动事件类型都有一个声明该类型的子元素）。

```
<startEvent id =“start”name =“my start event”/>
```

#####无启动事件的自定义扩展

** formKey **：对用户在启动新流程实例时必须填写的表单模板的引用。 更多信息可以在[表格部分]中找到（https://www.activiti.org/userguide/#forms）示例：

```
<startEvent id =“request”activiti：formKey =“org / activiti / examples / taskforms / request.form”/>
```

#### 8.2.8. Timer Start Event

##### Description

计时器启动事件用于在给定时间创建流程实例。它既可以用于应该只启动一次的进程，也可以用于应该以特定时间间隔启动的进程。

*注意：*子进程不能有计时器启动事件。

*注意：*部署进程后立即安排启动计时器事件。不需要调用startProcessInstanceByXXX，虽然调用start process方法不受限制，并且会在startProcessInstanceByXXX Invocation时再引发一个进程。

*注意：*当部署具有启动计时器事件的新版本的进程时，将删除与先前计时器对应的作业。原因是通常不希望自动启动此旧版本流程的新流程实例。

#####图形符号

无启动事件可视化为带有时钟内部图标的圆圈。

！[bpmn.clock.start.event（https://www.activiti.org/userguide/images/bpmn.clock.start.event.png）

##### XML表示

计时器启动事件的XML表示是具有计时器定义子元素的正常启动事件声明。有关配置详细信息，请参阅[计时器定义]（https://www.activiti.org/userguide/#timerEventDefinitions）。

示例：从2011年3月11日12:13开始，过程将以5分钟为间隔开始4次

```
<startEvent id="theStart">
  <timerEventDefinition>
    <timeCycle>R4/2011-03-11T12:13/PT5M</timeCycle>
</timerEventDefinition>
</startEvent>
```

Example: process will start once, on selected date

```
<startEvent id="theStart">
  <timerEventDefinition>
    <timeDate>2011-03-11T12:13:14</timeDate>
  </timerEventDefinition>
</startEvent>
```

#### 8.2.9. Message Start Event

##### Description

可以使用[message]（https://www.activiti.org/userguide/#bpmnMessageEventDefinition）启动事件来使用命名消息启动流程实例。这有效地允许我们使用消息名称从一组备选启动事件中选择*正确的启动事件。

当**使用一个或多个消息启动事件部署**流程定义时，以下注意事项适用：

- 消息启动事件的名称在给定的流程定义中必须是唯一的。流程定义不能具有多个具有相同名称的消息启动事件。 Activiti在部署流程定义时抛出异常，以便两个或多个消息启动事件引用相同的消息，如果两个或多个消息启动事件引用具有相同消息名称的消息。
- 消息启动事件的名称在所有已部署的流程定义中必须是唯一的。 Activiti在部署流程定义时抛出异常，使得一个或多个消息启动事件引用与已由不同流程定义部署的消息启动事件同名的消息。
- 流程版本控制：部署新版本的流程定义后，将取消先前版本的消息订阅。对于新版本中不存在的消息事件也是如此。

当**启动流程实例时，可以使用`RuntimeService`上的以下方法触发消息启动事件：

```
ProcessInstance startProcessInstanceByMessage(String messageName);
ProcessInstance startProcessInstanceByMessage(String messageName, Map<String, Object> processVariables);
ProcessInstance startProcessInstanceByMessage(String messageName, String businessKey, Map<String, Object< processVariables);
```

`messageName`是`messageEventDefinition`的`messageRef`属性引用的`message`元素的`name`属性中给出的名称。 ** **启动流程实例时，以下注意事项适用：

- 仅在顶级进程上支持消息启动事件。嵌入式子进程不支持消息启动事件。
- 如果流程定义有多个消息启动事件，`runtimeService.startProcessInstanceByMessage（...）`允许选择适当的启动事件。
- 如果流程定义有多个消息启动事件和一个无启动事件，`runtimeService.startProcessInstanceByKey（...）`和`runtimeService.startProcessInstanceById（...）`使用none start事件启动流程实例。
- 如果进程定义有多个消息启动事件且没有无启动事件，则`runtimeService.startProcessInstanceByKey（...）`和`runtimeService.startProcessInstanceById（...）`抛出异常。
- 如果流程定义具有单个消息启动事件，则`runtimeService.startProcessInstanceByKey（...）`和`runtimeService.startProcessInstanceById（...）`使用消息启动事件启动新的流程实例。
- 如果从呼叫活动启动进程，则仅支持消息启动事件
   - 除了消息启动事件之外，该进程还有一个无启动事件
   - 该进程具有单个消息启动事件，而没有其他启动事件。
   
##### Graphical notation

消息开始事件可视化为带有消息事件符号的圆圈。 符号未填充，以显示捕获（接收）行为。

！[bpmn.start.message.event（https://www.activiti.org/userguide/images/bpmn.start.message.event.png）

##### XML representation

消息启动事件的XML表示是带有messageEventDefinition子元素的正常启动事件声明：

```
<definitions id="definitions"
  xmlns="http://www.omg.org/spec/BPMN/20100524/MODEL"
  xmlns:activiti="http://activiti.org/bpmn"
  targetNamespace="Examples"
  xmlns:tns="Examples">

  <message id="newInvoice" name="newInvoiceMessage" />

  <process id="invoiceProcess">

    <startEvent id="messageStart" >
    	<messageEventDefinition messageRef="tns:newInvoice" />
    </startEvent>
    ...
  </process>

</definitions>
```

#### 8.2.10. Signal Start Event

##### Description

可以使用[signal]（https://www.activiti.org/userguide/#bpmnSignalEventDefinition）启动事件来使用命名信号启动流程实例。 可以使用中间信号throw事件或通过API（* runtimeService.signalEventReceivedXXX *方法）从流程实例中*发出*信号。 在这两种情况下，将启动具有相同名称的信号启动事件的所有流程定义。

请注意，在这两种情况下，还可以在流程实例的同步和异步启动之间进行选择。

必须在API中传递的`signalName`是`signalEventDefinition`的`signalRef`属性引用的`signal`元素的`name`属性中给出的名称。

##### Graphical notation

信号开始事件可视化为具有信号事件符号的圆圈。 符号未填充，以显示捕获（接收）行为。

！[bpmn.start.signal.event（https://www.activiti.org/userguide/images/bpmn.start.signal.event.png）

##### XML representation

信号启动事件的XML表示是具有signalEventDefinition子元素的正常启动事件声明：

```
<signal id="theSignal" name="The Signal" />

<process id="processWithSignalStart1">
  <startEvent id="theStart">
    <signalEventDefinition id="theSignalEventDefinition" signalRef="theSignal"  />
  </startEvent>
  <sequenceFlow id="flow1" sourceRef="theStart" targetRef="theTask" />
  <userTask id="theTask" name="Task in process A" />
  <sequenceFlow id="flow2" sourceRef="theTask" targetRef="theEnd" />
	  <endEvent id="theEnd" />
</process>
```

#### 8.2.11. Error Start Event

##### Description

[error]（https://www.activiti.org/userguide/#bpmnErrorEventDefinition）启动事件可用于触发事件子流程。 **错误启动事件不能用于启动流程实例**。

错误启动事件总是在中断。

##### Graphical notation

错误开始事件可视化为带有错误事件符号的圆圈。 符号未填充，以显示捕获（接收）行为。

！[bpmn.start.error.event（https://www.activiti.org/userguide/images/bpmn.start.error.event.png）

##### XML representation

错误启动事件的XML表示形式是带有errorEventDefinition子元素的正常启动事件声明：

```
<startEvent id="messageStart" >
	<errorEventDefinition errorRef="someError" />
</startEvent>
```

#### 8.2.12. End Events

结束事件表示（子）进程的结束（路径）。 结束事件**总是投掷**。 这意味着当进程执行到达结束事件时，会抛出* result *。 结果的类型由事件的内部黑色图标描绘。 在XML表示中，类型由子元素的声明给出。

#### 8.2.13. None End Event

##### Description

A * none * end事件表示未指定到达事件时抛出的* result *。 因此，除了结束当前的执行路径之外，引擎不会做任何额外的事情。

##### Graphical notation

无端事件可视化为具有粗边框但没有内部图标（无结果类型）的圆。

！[bpmn.none.end.event（https://www.activiti.org/userguide/images/bpmn.none.end.event.png）

##### XML representation

无结束事件的XML表示是正常的结束事件声明，没有任何子元素（其他结束事件类型都有一个声明该类型的子元素）。

```
<endEvent id="end" name="my end event" />
```

#### 8.2.14. Error End Event

##### Description

当进程执行到达**错误结束事件**时，当前执行路径结束并抛出错误。 此错误可以[由匹配的中间边界错误事件捕获]（https://www.activiti.org/userguide/#bpmnBoundaryErrorEvent）。 如果未找到匹配的边界错误事件，则将引发异常。

##### Graphical notation

错误结束事件可视化为典型的结束事件（具有粗边框的圆圈），其中包含错误图标。 错误图标完全是黑色，表示投掷语义。

！[bpmn.error.end.event（https://www.activiti.org/userguide/images/bpmn.error.end.event.png）

##### XML representation

错误结束事件表示为结束事件，带有* errorEventDefinition *子元素。

```
<endEvent id="myErrorEndEvent">
  <errorEventDefinition errorRef="myError" />
</endEvent>
```

* errorRef *属性可以引用在进程外定义的* error *元素：

```
<error id="myError" errorCode="123" />
...
<process id="myProcess">
...
```

* error *的** errorCode **将用于查找匹配的捕获边界错误事件。 如果* errorRef *与任何定义的* error *不匹配，则* errorRef *用作* errorCode *的快捷方式。 这是Activiti特定的快捷方式。 更具体地说，以下片段在功能上是等同的。

```
<error id="myError" errorCode="error123" />
...
<process id="myProcess">
...
  <endEvent id="myErrorEndEvent">
    <errorEventDefinition errorRef="myError" />
  </endEvent>
...
```

相当于

```
<endEvent id="myErrorEndEvent">
  <errorEventDefinition errorRef="error123" />
</endEvent>
```

请注意，* errorRef *必须符合BPMN 2.0架构，并且必须是有效的QName。

#### 8.2.15. Terminate End Event

##### Description

当达到*终止结束事件*时，将终止当前流程实例或子流程。 从概念上讲，当执行到达终止结束事件时，将确定并结束第一个*范围*（进程或子进程）。 请注意，在BPMN 2.0中，子流程可以是嵌入式子流程，调用活动，事件子流程或事务子流程。 此规则通常适用：例如，当存在多实例调用活动或嵌入式子流程时，仅结束该实例，其他实例和流程实例不受影响。

可以添加可选属性* terminateAll *。 当* true *时，无论在流程定义中是否存在终止结束事件，并且无论是否处于子流程（甚至是嵌套），（根）流程实例都将被终止。

##### Graphical notation

取消结束事件可视化为典型的结束事件（具有粗轮廓的圆圈），内部带有完整的黑色圆圈。

！[bpmn.terminate.end.event（https://www.activiti.org/userguide/images/bpmn.terminate.end.event.png）

##### XML representation

终止事件事件表示为结束事件，带有* terminateEventDefinition *子元素。

请注意，* terminateAll *属性是可选的（默认情况下为* false *）。

```
<endEvent id="myEndEvent >
  <terminateEventDefinition activiti:terminateAll="true"></terminateEventDefinition>
</endEvent>
```

#### 8.2.16. Cancel End Event

[[EXPERIMENTAL\]](https://www.activiti.org/userguide/#experimental)

##### Description

取消结束事件只能与bpmn事务子流程结合使用。 当到达取消结束事件时，抛出取消事件，必须由取消边界事件捕获。 取消边界事件然后取消交易并触发补偿。

##### Graphical notation

取消结束事件可视化为典型的结束事件（具有粗轮廓的圆圈），其中包含取消图标。 取消图标完全是黑色，表示投掷语义。

！[bpmn.cancel.end.event（https://www.activiti.org/userguide/images/bpmn.cancel.end.event.png）


##### XML representation

取消结束事件表示为结束事件，带有* cancelEventDefinition *子元素。

```
<endEvent id="myCancelEndEvent">
  <cancelEventDefinition />
</endEvent>
```

#### 8.2.17. Boundary Events

边界事件是*捕获*附加到活动的事件（边界事件永远不会抛出）。 这意味着当活动正在运行时，事件为* listen *表示某种类型的触发器。 当事件被*捕获*时，活动被中断并且遵循事件之外的序列流。

所有边界事件都以相同的方式定义：

```
<boundaryEvent id="myBoundaryEvent" attachedToRef="theActivity">
      <XXXEventDefinition/>
</boundaryEvent>
```

边界事件定义为

 - 唯一标识符（流程范围）
 - 通过** attachedToRef **属性引用事件的活动的引用。 注意，边界事件定义在与它们所附着的活动相同的级别上（即，不在活动内包含边界事件）。
 - 定义边界事件类型的* XXXEventDefinition *形式的XML子元素（例如* TimerEventDefinition *，* ErrorEventDefinition *等）。 有关详细信息，请参阅特定边界事件类型。

#### 8.2.18. Timer Boundary Event

##### Description

计时器边界事件充当秒表和闹钟。 当执行到达附加边界事件的活动时，启动计时器。 当计时器触发时（例如，在指定的间隔之后），活动被中断边界事件被跟随。

##### Graphical Notation

计时器边界事件可视化为典型的边界事件（即边界上的圆圈），内部有计时器图标。

！[bpmn.boundary.timer.event（https://www.activiti.org/userguide/images/bpmn.boundary.timer.event.png）

##### XML Representation

计时器边界事件被定义为[常规边界事件]（https://www.activiti.org/userguide/#bpmnBoundaryEvent）。 在这种情况下，特定类型子元素是** timerEventDefinition **元素。

```
<boundaryEvent id="escalationTimer" cancelActivity="true" attachedToRef="firstLineSupport">
  <timerEventDefinition>
    <timeDuration>PT4H</timeDuration>
  </timerEventDefinition>
</boundaryEvent>
```

有关计时器配置的详细信息，请参阅[计时器事件定义]（https://www.activiti.org/userguide/#timerEventDefinitions）。

在图形表示中，圆圈的线条是点缀的，如上面的示例所示：

！[bpmn.non.interrupting.boundary.timer.event（https://www.activiti.org/userguide/images/bpmn.non.interrupting.boundary.timer.event.png）

典型的用例是另外发送升级电子邮件但不中断正常的流程。

从BPMN 2.0开始，中断和非中断定时器事件之间存在差异。 中断是默认设置。 不间断导致原始活动**不会被中断，但活动仍然存在。 而是创建其他执行并通过事件的传出转换发送。 在XML表示中，* cancelActivity *属性设置为false：

```
<boundaryEvent id="escalationTimer" cancelActivity="false" attachedToRef="firstLineSupport"/>
```

**注意：**边界计时器事件仅在启用作业或异步执行程序时触发（即* jobEEecutorActivate *或* asyncExecutorActivate *需要在`activiti.cfg.xml`中设置为`true`，因为该作业 默认情况下禁用异步执行程序）。

##### Known issue with boundary events

在使用任何类型的边界事件时，存在关于并发性的已知问题。 目前，不可能将多个传出序列流附加到边界事件（请参阅问题[ACT-47]（https://activiti.atlassian.net/browse/ACT-47））。 该问题的解决方案是使用一个传递到并行网关的传出序列流。

！[bpmn.known.issue.boundary.event（https://www.activiti.org/userguide/images/bpmn.known.issue.boundary.event.png）

#### 8.2.19. Error Boundary Event

##### Description

活动边界上的中间*捕获*错误，或简称**边界错误事件**，捕获在定义它的活动范围内引发的错误。

在[嵌入式子流程]（https://www.activiti.org/userguide/#bpmnSubProcess）或[call activity]（https://www.activiti.org/userguide/）上定义边界错误事件最有意义。 #bpmnCallActivity），作为子进程为子进程内的所有活动创建一个范围。 [错误结束事件]（https://www.activiti.org/userguide/#bpmnErrorEndEvent）引发错误。这样的错误将向上传播其父作用域，直到找到定义了与错误事件定义匹配的边界错误事件的作用域。

当捕获错误事件时，销毁定义边界事件的活动，也破坏其中的所有当前执行（例如，并发活动，嵌套子进程等）。在边界事件的输出序列流之后继续执行流程。

##### Graphical notation

边界错误事件可视化为边界上的典型中间事件（内部具有较小圆圈的圆圈），其中包含错误图标。 错误图标为白色，表示* catch *语义。

！[bpmn.boundary.error.event（https://www.activiti.org/userguide/images/bpmn.boundary.error.event.png）

##### Xml representation

边界错误事件被定义为典型的[边界事件]（https://www.activiti.org/userguide/#bpmnBoundaryEvent）：

```
<boundaryEvent id="catchError" attachedToRef="mySubProcess">
  <errorEventDefinition errorRef="myError"/>
</boundaryEvent>
```

与[错误结束事件]（https://www.activiti.org/userguide/#bpmnErrorEndEvent）一样，* errorRef *引用在process元素之外定义的错误：

```
<error id="myError" errorCode="123" />
...
<process id="myProcess">
...
```

** errorCode **用于匹配捕获的错误：

 - 如果省略* errorRef *，则边界错误事件将捕获**任何错误事件**，无论* error *的errorCode如何。
 - 如果提供了* errorRef *且它引用了现有的* error *，则边界事件将**仅捕获具有相同错误代码**的错误。
 - 如果提供了* errorRef *，但BPMN 2.0文件中没有定义* error *，则** errorRef用作errorCode **（类似于错误结束事件）。


##### Example

以下示例流程显示了如何使用错误结束事件。 当*'Review profitability'*用户任务通过声明提供的信息不足而完成时，将引发错误。 当此错误被捕获到子流程的边界时，*'审核销售线索'*子流程中的所有活动活动都将被销毁（即使*'审核客户评级'*尚未完成），以及*'提供其他详细信息 '*用户任务已创建。

！[bpmn.boundary.error.example（https://www.activiti.org/userguide/images/bpmn.boundary.error.example.png）

此过程在演示设置中作为示例提供。 可以在* org.activiti.examples.bpmn.event.error *包中找到进程XML和单元测试。

#### 8.2.20. Signal Boundary Event

##### Description

在活动边界上附加的中间*捕获* [信号]（https://www.activiti.org/userguide/#bpmnSignalEventDefinition）或简称**边界信号事件**捕获具有相同信号名称的信号 作为参考信号的定义。

**注意：**与边界错误事件等其他事件相反，边界信号事件不仅捕获从其附加的范围抛出的信号事件。 相反，信号事件具有全局范围（广播语义），这意味着信号可以从任何地方抛出，甚至可以从不同的流程实例抛出。

**注意：**与错误事件等其他事件相反，如果信号被捕获，则不会消耗信号。 如果有两个活动信号边界事件捕获相同的信号事件，则两个边界事件都会被触发，即使它们是不同流程实例的一部分。

##### Graphical notation

边界信号事件可视化为边界上的典型中间事件（内部具有较小圆的圆），其中具有信号图标。 信号图标为白色（未填充），表示* catch *语义。

！[bpmn.boundary.signal.event（https://www.activiti.org/userguide/images/bpmn.boundary.signal.event.png）

##### XML representation

边界信号事件被定义为典型的[边界事件]（https://www.activiti.org/userguide/#bpmnBoundaryEvent）：

```
<boundaryEvent id="boundary" attachedToRef="task" cancelActivity="true">
          <signalEventDefinition signalRef="alertSignal"/>
</boundaryEvent>
```

##### Example

请参阅[信号事件定义]部分（https://www.activiti.org/userguide/#bpmnSignalEventDefinition）。

#### 8.2.21. Message Boundary Event

##### Description

在活动边界上附加的中间*捕获* [消息]（https://www.activiti.org/userguide/#bpmnMessageEventDefinition）或简称**边界消息事件**捕获具有相同消息名称的消息 作为引用的消息定义。

##### Graphical notation

边界消息事件可视化为边界上的典型中间事件（内部具有较小圆圈的圆圈），其中包含消息图标。 消息图标为白色（未填充），表示* catch *语义。

！[bpmn.boundary.message.event（https://www.activiti.org/userguide/images/bpmn.boundary.message.event.png）

请注意，边界消息事件可以是中断（右侧）和非中断（左侧）。

##### XML representation

边界消息事件被定义为典型的[边界事件]（https://www.activiti.org/userguide/#bpmnBoundaryEvent）：

```
<boundaryEvent id="boundary" attachedToRef="task" cancelActivity="true">
          <messageEventDefinition messageRef="newCustomerMessage"/>
</boundaryEvent>
```

##### Example

请参阅[消息事件定义]部分（https://www.activiti.org/userguide/#bpmnMessageEventDefinition）。

#### 8.2.22. Cancel Boundary Event

[[EXPERIMENTAL\]](https://www.activiti.org/userguide/#experimental)

##### Description

事务子流程边界上的附加中间*捕获*取消，或简称**边界取消事件**，在取消事务时触发。触发取消边界事件时，它首先中断当前作用域中活动的所有执行。接下来，它开始补偿交易范围内的所有有效补偿边界事件。同步执行补偿，即边界事件在离开交易之前在补偿完成之前等待。当补偿完成时，使用在取消边界事件之外用完的序列流来保留事务子流程。

**注意：**事务子流程只允许一个取消边界事件。

**注意：**如果事务子进程承载嵌套子进程，则仅对已成功完成的子进程触发补偿。

**注意：**如果在具有多实例特征的事务子流程上放置了取消边界事件，则如果一个实例触发取消，则边界事件将取消所有实例。

##### Graphical notation

取消边界事件可视化为边界上的典型中间事件（内部较小圆圈的圆圈），其中包含取消图标。 取消图标为白色（未填充），表示*捕获*语义。

！[bpmn.boundary.cancel.event（https://www.activiti.org/userguide/images/bpmn.boundary.cancel.event.png）

##### XML representation

取消边界事件被定义为典型的[边界事件]（https://www.activiti.org/userguide/#bpmnBoundaryEvent）：

```
<boundaryEvent id="boundary" attachedToRef="transaction" >
          <cancelEventDefinition />
</boundaryEvent>
```

由于取消边界事件总是在中断，因此不需要`cancelActivity`属性。

#### 8.2.23. Compensation Boundary Event

[[EXPERIMENTAL\]](https://www.activiti.org/userguide/#experimental)

##### Description

在活动边界上附加的中间*捕获*补偿或简称**补偿边界事件**可用于将补偿处理程序附加到活动。

补偿边界事件必须使用定向关联引用单个补偿处理程序。

补偿边界事件与其他边界事件具有不同的激活策略。其他边界事件（例如信号边界事件）在启动它们所附加的活动时被激活。当活动离开时，它们被停用并且相应的事件订阅被取消。补偿边界事件是不同的。补偿边界事件在附加到**的活动成功完成时激活**。此时，创建对补偿事件的相应订阅。在触发补偿事件或相应的流程实例结束时，将删除订阅。由此可见：

- 触发补偿时，与补偿边界事件关联的补偿处理程序的调用次数与成功完成的活动的次数相同。
- 如果将补偿边界事件附加到具有多个实例特征的活动，则会为每个实例创建补偿事件订阅。
- 如果补偿边界事件附加到循环内包含的活动，则每次执行活动时都会创建补偿事件订阅。
- 如果流程实例结束，则取消对补偿事件的订阅。

**注意：**嵌入式子流程不支持补偿边界事件。

##### Graphical notation

补偿边界事件可视化为边界上的典型中间事件（内部具有较小圆的圆），其中具有补偿图标。 补偿图标为白色（未填充），表示* catch *语义。 除了补偿边界事件之外，下图还显示了使用单向关联与边界事件关联的补偿处理程序：

！[bpmn.boundary.compensation.event（https://www.activiti.org/userguide/images/bpmn.boundary.compensation.event.png）

##### XML representation

补偿边界事件被定义为典型的[边界事件]（https://www.activiti.org/userguide/#bpmnBoundaryEvent）：

```
<boundaryEvent id="compensateBookHotelEvt" attachedToRef="bookHotel" >
          <compensateEventDefinition />
</boundaryEvent>

<association associationDirection="One" id="a1"  sourceRef="compensateBookHotelEvt" targetRef="undoBookHotel" />

<serviceTask id="undoBookHotel" isForCompensation="true" activiti:class="..." />
```

由于在活动成功完成后激活补偿边界事件，因此不支持“cancelActivity”属性。

#### 8.2.24. Intermediate Catching Events

所有中间捕获事件都以相同的方式定义：

```
<intermediateCatchEvent id="myIntermediateCatchEvent" >
      <XXXEventDefinition/>
</intermediateCatchEvent>
```

中间捕获事件定义为

 - 唯一标识符（流程范围）
 -  * XXXEventDefinition *形式的XML子元素（例如* TimerEventDefinition *等），用于定义中间捕获事件的类型。 有关详细信息，请参阅特定捕获事件类型。

#### 8.2.25. Timer Intermediate Catching Event

##### Description

计时器中间事件充当秒表。 当执行到达捕获事件活动时，启动计时器。 当计时器触发时（例如，在指定的间隔之后），遵循从计时器中间事件发出的顺序流。

##### Graphical Notation

计时器中间事件可视化为中间捕获事件，内部有计时器图标。

！[bpmn.intermediate.timer.event（https://www.activiti.org/userguide/images/bpmn.intermediate.timer.event.png）

##### XML Representation

计时器中间事件被定义为[中间捕获事件]（https://www.activiti.org/userguide/#bpmnIntermediateCatchingEvent）。 在这种情况下，特定类型子元素是** timerEventDefinition **元素。

```
<intermediateCatchEvent id="timer">
  <timerEventDefinition>
    <timeDuration>PT5M</timeDuration>
  </timerEventDefinition>
</intermediateCatchEvent>
```

See [timer event definitions](https://www.activiti.org/userguide/#timerEventDefinitions) for configuration details.

#### 8.2.26. Signal Intermediate Catching Event

##### Description

中间*捕获* [信号]（https://www.activiti.org/userguide/#bpmnSignalEventDefinition）事件捕获与参考信号定义具有相同信号名称的信号。

**注意：**与错误事件等其他事件相反，如果信号被捕获，则不会消耗信号。 如果有两个活动信号边界事件捕获相同的信号事件，则两个边界事件都会被触发，即使它们是不同流程实例的一部分。

##### Graphical notation

中间信号捕获事件可视化为典型的中间事件（内部具有较小圆圈的圆圈），其中包含信号图标。 信号图标为白色（未填充），表示* catch *语义。

![bpmn.intermediate.signal.catch.event](https://www.activiti.org/userguide/images/bpmn.intermediate.signal.catch.event.png)

##### XML representation

信号中间事件被定义为[中间捕获事件]（https://www.activiti.org/userguide/#bpmnIntermediateCatchingEvent）。 在这种情况下，特定类型子元素是** signalEventDefinition **元素。

```
<intermediateCatchEvent id="signal">
  <signalEventDefinition signalRef="newCustomerSignal" />
</intermediateCatchEvent>
```

##### Example

See section on [signal event definitions](https://www.activiti.org/userguide/#bpmnSignalEventDefinition).

#### 8.2.27. Message Intermediate Catching Event

##### Description

中间*捕获* [消息]（https://www.activiti.org/userguide/#bpmnMessageEventDefinition）事件捕获具有指定名称的消息。

##### Graphical notation

中间捕获消息事件可视化为典型的中间事件（内部具有较小圆圈的圆圈），其中包含消息图标。 消息图标为白色（未填充），表示* catch *语义。

![bpmn.intermediate.message.catch.event](https://www.activiti.org/userguide/images/bpmn.intermediate.message.catch.event.png)

##### XML representation

消息中间事件被定义为[中间捕获事件]（https://www.activiti.org/userguide/#bpmnIntermediateCatchingEvent）。 在这种情况下，特定类型的子元素是** messageEventDefinition **元素。

```
<intermediateCatchEvent id="message">
  <messageEventDefinition signalRef="newCustomerMessage" />
</intermediateCatchEvent>
```

##### Example

请参阅[消息事件定义]部分（https://www.activiti.org/userguide/#bpmnMessageEventDefinition）。

#### 8.2.28. Intermediate Throwing Event

所有中间投掷事件都以相同的方式定义：

```
<intermediateThrowEvent id="myIntermediateThrowEvent" >
      <XXXEventDefinition/>
</intermediateThrowEvent>
```

中间投掷事件定义为

 - 唯一标识符（流程范围）
 -  * XXXEventDefinition *形式的XML子元素（例如* signalEventDefinition *等），用于定义中间投掷事件的类型。 有关详细信息，请参阅特定投掷事件类型。

#### 8.2.29. Intermediate Throwing None Event

以下流程图显示了一个中间无事件的简单示例，该事件通常用于指示流程中实现的某些状态。

![bpmn.intermediate.none.event](https://www.activiti.org/userguide/images/bpmn.intermediate.none.event.png)

这可以很好地监控一些KPI，基本上是通过添加[执行监听器]（https://www.activiti.org/userguide/#executionListeners）。

```
<intermediateThrowEvent id="noneEvent">
  <extensionElements>
    <activiti:executionListener class="org.activiti.engine.test.bpmn.event.IntermediateNoneEventTest$MyExecutionListener" event="start" />
  </extensionElements>
</intermediateThrowEvent>
```

在那里你可以添加一些自己的代码，可能会发送一些事件到你的BAM工具或DWH。 在那种情况下，引擎本身不会做任何事情，它只是通过。

#### 8.2.30. Signal Intermediate Throwing Event

##### Description

中间*投掷* [信号]（https://www.activiti.org/userguide/#bpmnSignalEventDefinition）事件会为定义的信号抛出信号事件。

在Activiti中，信号被广播给所有活动的处理程序（即所有捕获信号事件）。 信号可以同步或异步发布。

 - 在默认配置中，信号同步传送** **。 这意味着抛出流程实例会等待信号传递到所有捕获流程实例。 捕获流程实例也在与抛出流程实例相同的事务中得到通知，这意味着如果其中一个通知的实例产生技术错误（抛出异常），则所有涉及的实例都会失败。
 - 信号也可以异步** **传送。 在那种情况下，确定在达到投掷信号事件时哪些处理者是活动的。 对于每个活动的处理程序，JobExecutor存储并传递异步通知消息（Job）。

##### Graphical notation

中间信号投掷事件可视化为典型的中间事件（内部具有较小圆圈的圆圈），其中包含信号图标。 信号图标为黑色（填充），表示* throw *语义。

！[bpmn.intermediate.signal.throw.event（https://www.activiti.org/userguide/images/bpmn.intermediate.signal.throw.event.png）

##### XML representation

信号中间事件被定义为[中间投掷事件]（https://www.activiti.org/userguide/#bpmnIntermediateThrowEvent）。 在这种情况下，特定类型子元素是** signalEventDefinition **元素。

```
<intermediateThrowEvent id="signal">
  <signalEventDefinition signalRef="newCustomerSignal" />
</intermediateThrowEvent>
```

异步信号事件看起来像这样：

```
<intermediateThrowEvent id="signal">
  <signalEventDefinition signalRef="newCustomerSignal" activiti:async="true" />
</intermediateThrowEvent>
```

##### Example

请参阅[信号事件定义]部分（https://www.activiti.org/userguide/#bpmnSignalEventDefinition）。

#### 8.2.31. Compensation Intermediate Throwing Event

[[EXPERIMENTAL\]](https://www.activiti.org/userguide/#experimental)

##### Description

中间*投掷*补偿事件可用于触发补偿。

**触发补偿：**可以针对指定活动或主持补偿事件的范围触发补偿。通过执行与活动相关联的补偿处理程序来执行补偿。

- 当为活动抛出补偿时，相关的补偿处理程序执行的次数与活动成功竞争的次数相同。
- 如果对当前范围进行补偿，则补偿当前范围内的所有活动，包括并发分支上的活动。
- 按层次触发补偿：如果要补偿的活动是子流程，则会对子流程中包含的所有活动触发补偿。如果子进程具有嵌套活动，则会以递归方式抛出补偿。但是，补偿不会传播到流程的“上层”：如果在子流程中触发补偿，则不会将补偿传播到子流程范围之外的活动。 BPMN规范声明对“相同级别的子进程”的活动触发补偿。
- 在Activiti中，补偿按执行顺序执行。这意味着最后完成的任何活动都会首先得到补偿，等等。
- 中间投掷补偿事件可用于补偿成功竞争的交易子过程。

**注意：**如果在包含子流程的范围内抛出补偿且子流程包含具有补偿处理程序的活动，则补偿仅在抛出补偿时成功完成后传播到子流程。如果嵌套在子流程中的某些活动已完成并附加了补偿处理程序，则如果尚未完成包含这些活动的子流程，则不会执行补偿处理程序。请考虑以下示例：

！[bpmn.throw.compensation.example1（https://www.activiti.org/userguide/images/bpmn.throw.compensation.example1.png）

在这个过程中，我们有两个并发执行，一个执行嵌入式子进程，另一个执行“充值信用卡”活动。假设两个执行都已启动，并且第一个并发执行正在等待用户完成“审核预订”任务。第二次执行执行“充值信用卡”活动并抛出错误，这导致“取消预订”事件触发补偿。此时并行子流程尚未完成，这意味着补偿事件不会传播到子流程，因此不会执行“取消酒店预订”补偿处理程序。如果在执行“取消预留”之前用户任务（以及因此嵌入的子过程）完成，则补偿被传播到嵌入的子过程。

**过程变量：**在补偿嵌入式子过程时，用于执行补偿处理程序的执行可以访问子过程完成执行时所处状态的子过程的本地过程变量。为此，需要获取与范围执行相关的过程变量的快照（为执行子过程而创建的执行）。从此形成，有以下几个含义：

- 补偿处理程序无权访问添加到子进程范围内创建的并发执行的变量。
- 与层次结构中较高的执行相关联的流程变量（例如，与流程实例执行相关联的流程变量不包含在快照中：补偿处理程序可以在抛出补偿时处于这些流程变量中的状态。
- 变量快照仅用于嵌入式子进程，而不用于其他活动。

**目前的限制：**

- `waitForCompletion =“false”`目前不受支持。当使用中间投掷补偿事件触发补偿时，仅在补偿成功完成后才会离开事件。
- 补偿本身目前由并发执行执行。并发执行以相反的顺序启动，补偿活动完成。活动的未来版本可能包括按顺序执行补偿的选项。
- 补偿不会传播到由调用活动生成的子流程实例。

##### Graphical notation

中间补偿投掷事件可视化为典型的中间事件（内部具有较小圆圈的圆圈），内部具有补偿图标。 补偿图标为黑色（填充），表示* throw *语义。

![bpmn.intermediate.compensation.throw.event](https://www.activiti.org/userguide/images/bpmn.intermediate.compensation.throw.event.png)

##### Xml representation

补偿中间事件被定义为[中间投掷事件]（https://www.activiti.org/userguide/#bpmnIntermediateThrowEvent）。 在这种情况下，特定类型子元素是** compensateEventDefinition **元素。

```
<intermediateThrowEvent id="throwCompensation">
	<compensateEventDefinition />
</intermediateThrowEvent>
```

此外，可选参数`activityRef`可用于触发特定范围/活动的补偿：

```
<intermediateThrowEvent id="throwCompensation">
	<compensateEventDefinition activityRef="bookHotel" />
</intermediateThrowEvent>
```

### 8.3. Sequence Flow

#### 8.3.1. Description

序列流是进程的两个元素之间的连接器。 在流程执行期间访问元素后，将遵循所有传出序列流。 这意味着BPMN 2.0的默认特性是并行：两个输出序列流将创建两个独立的并行执行路径。

#### 8.3.2. Graphical notation

序列流可视化为从源元素朝向目标元素的箭头。 箭头始终指向目标。

![bpmn.sequence.flow](https://www.activiti.org/userguide/images/bpmn.sequence.flow.png)

#### 8.3.3. XML representation

序列流需要具有进程唯一** id **，以及对现有**源**和**目标**元素的引用。

```
<sequenceFlow id="flow1" sourceRef="theStart" targetRef="theTask" />
```

#### 8.3.4. Conditional sequence flow

##### Description

序列流可以在其上定义条件。 当剩下BPMN 2.0活动时，默认行为是评估传出序列流的条件。 当条件评估为* true *时，选择该传出序列流。 当以这种方式选择多个序列流时，将生成多个*执行*并且该过程将以并行方式继续。

**注意：**上述内容适用于BPMN 2.0活动（和事件），但不适用于网关。 网关将根据网关类型以特定方式处理具有条件的顺序流。

##### Graphical notation

条件序列流可视化为规则序列流，开头有一个小钻石。 条件表达式显示在序列流程旁边。

![bpmn.conditional.sequence.flow](https://www.activiti.org/userguide/images/bpmn.conditional.sequence.flow.png)

##### XML representation

条件序列流在XML中表示为常规序列流，包含** conditionExpression **子元素。 请注意，目前仅支持* tFormalExpressions *，省略* xsi：type =“”*定义将默认为仅支持的表达式类型。

```
<sequenceFlow id="flow" sourceRef="theStart" targetRef="theTask">
  <conditionExpression xsi:type="tFormalExpression">
    <![CDATA[${order.price > 100 && order.price < 250}]]>
  </conditionExpression>
</sequenceFlow>
```

目前，条件表达式**只能与UEL **一起使用，有关这些的详细信息可以在[表达式]部分（https://www.activiti.org/userguide/#apiExpressions）中找到。 使用的表达式应该解析为布尔值，否则在评估条件时会抛出异常。

 - 下面的示例通过getter以典型的JavaBean样式引用流程变量的数据。

```
<conditionExpression xsi:type="tFormalExpression">
  <![CDATA[${order.price > 100 && order.price < 250}]]>
</conditionExpression>
```

 - 此示例调用解析为布尔值的方法。

```
<conditionExpression xsi:type="tFormalExpression">
  <![CDATA[${order.isStandardOrder()}]]>
</conditionExpression>
```

Activiti发行版包含使用值和方法表达式的以下示例流程（请参阅* org.activiti.examples.bpmn.expression）*：

![bpmn.uel expression.on.seq.flow](https://www.activiti.org/userguide/images/bpmn.uel-expression.on.seq.flow.png)

#### 8.3.5. Default sequence flow

##### Description

所有BPMN 2.0任务和网关都可以具有**默认顺序流**。 当且仅当不能选择其他序列流时，才将该序列流选择为该活动的输出序列流。 始终忽略默认顺序流的条件。

##### Graphical notation

默认序列流可视化为常规序列流，开头带有*斜杠*标记。

![bpmn.default.sequence.flow](https://www.activiti.org/userguide/images/bpmn.default.sequence.flow.png)

##### XML representation

某个活动的默认顺序流由该活动上的**默认属性**定义。 以下XML片段显示了一个独占网关，它具有默认顺序流* flow 2 *。 仅当* conditionA *和* conditionB *都评估为false时，才会选择它作为网关的传出顺序流。

```
<exclusiveGateway id="exclusiveGw" name="Exclusive Gateway" default="flow2" />
<sequenceFlow id="flow1" sourceRef="exclusiveGw" targetRef="task1">
  <conditionExpression xsi:type="tFormalExpression">${conditionA}</conditionExpression>
</sequenceFlow>
<sequenceFlow id="flow2" sourceRef="exclusiveGw" targetRef="task2"/>
<sequenceFlow id="flow3" sourceRef="exclusiveGw" targetRef="task3">
  <conditionExpression xsi:type="tFormalExpression">${conditionB}</conditionExpression>
</sequenceFlow>
```

这与以下图形表示相对应：

### 8.4. Gateways

网关用于控制执行流程（或者如BPMN 2.0描述的那样，执行的*令牌*）。 网关能够*消耗*或*生成*令牌。

网关以图形方式显示为菱形，内部带有图标。 该图标显示网关的类型。

![bpmn.gateway](https://www.activiti.org/userguide/images/bpmn.gateway.png)

#### 8.4.1. Exclusive Gateway

##### Description

独占网关（也称为* XOR网关*或更专业的*基于数据的独占网关*）用于对流程中的**决策**进行建模。 当执行到达此网关时，将按照定义它们的顺序评估所有传出顺序流。 选择条件评估为真的序列流（或者没有条件集，在概念上具有在序列流上定义的''真''）以继续该过程。

**请注意，传出序列流的语义与BPMN 2.0中的一般情况不同。 虽然通常选择条件评估为真的所有序列流以并行方式继续，但是在使用专用网关时仅选择一个序列流。 如果多个序列流具有计算结果为true的条件，则选择XML中定义的第一个（并且只有那个！）以继续该过程。 如果不能选择序列流，则会抛出异常。**

##### Graphical notation

专用网关可视化为典型的网关（即菱形），内部带有* X *图标，参考* XOR *语义。 请注意，没有图标的网关默认为独占网关。 BPMN 2.0规范不允许在同一过程定义中混合带有和不带X的钻石。

![bpmn.exclusive.gateway.notation](https://www.activiti.org/userguide/images/bpmn.exclusive.gateway.notation.png)

##### XML representation

专用网关的XML表示是直接的：定义网关的一行和在输出序列流上定义的条件表达式。 请参阅[条件序列流程]（https://www.activiti.org/userguide/#bpmnConditionalSequenceFlow）一节，了解哪些选项可用于此类表达式。

以下面的模型为例：


![bpmn.exclusive.gateway](https://www.activiti.org/userguide/images/bpmn.exclusive.gateway.png)

其中用XML表示如下：

```
<exclusiveGateway id="exclusiveGw" name="Exclusive Gateway" />

<sequenceFlow id="flow2" sourceRef="exclusiveGw" targetRef="theTask1">
  <conditionExpression xsi:type="tFormalExpression">${input == 1}</conditionExpression>
</sequenceFlow>

<sequenceFlow id="flow3" sourceRef="exclusiveGw" targetRef="theTask2">
  <conditionExpression xsi:type="tFormalExpression">${input == 2}</conditionExpression>
</sequenceFlow>

<sequenceFlow id="flow4" sourceRef="exclusiveGw" targetRef="theTask3">
  <conditionExpression xsi:type="tFormalExpression">${input == 3}</conditionExpression>
</sequenceFlow>
```

#### 8.4.2. Parallel Gateway

##### Description

网关还可用于对流程中的并发进行建模。在流程模型中引入并发性的最直接的网关是**并行网关**，它允许* fork *进入多个执行路径或* join *多个传入执行路径。

并行网关的功能基于传入和传出顺序流：

- ** fork：**并行执行所有传出序列流，为每个序列流创建一个并发执行。
- ** join：**到达并行网关的所有并发执行在网关中等待，直到每个传入的序列流都到达执行。然后，该过程继续经过加入网关。

请注意，如果同一并行网关有多个传入和传出顺序流，则并行网关可以具有分支和连接行为**。在这种情况下，网关将首先加入所有传入的序列流，然后再拆分为多个并发的执行路径。

**与其他网关类型的一个重要区别是并行网关不评估条件。如果在与并行网关连接的顺序流上定义条件，则忽略它们。**

##### Graphical Notation

并行网关可视化为网关（菱形），内部带有* plus *符号，引用* AND *语义。

![bpmn.parallel.gateway](https://www.activiti.org/userguide/images/bpmn.parallel.gateway.png)

##### XML representation

定义并行网关需要一行XML：

```
<parallelGateway id="myParallelGateway" />
```

实际行为（fork，join或两者）由连接到并行网关的顺序流定义。

例如，上面的模型归结为以下XML：

```
<startEvent id="theStart" />
<sequenceFlow id="flow1" sourceRef="theStart" targetRef="fork" />

<parallelGateway id="fork" />
<sequenceFlow sourceRef="fork" targetRef="receivePayment" />
<sequenceFlow sourceRef="fork" targetRef="shipOrder" />

<userTask id="receivePayment" name="Receive Payment" />
<sequenceFlow sourceRef="receivePayment" targetRef="join" />

<userTask id="shipOrder" name="Ship Order" />
<sequenceFlow sourceRef="shipOrder" targetRef="join" />

<parallelGateway id="join" />
<sequenceFlow sourceRef="join" targetRef="archiveOrder" />

<userTask id="archiveOrder" name="Archive Order" />
<sequenceFlow sourceRef="archiveOrder" targetRef="theEnd" />

<endEvent id="theEnd" />
```

在上面的示例中，在启动进程后，将创建两个任务：

```
ProcessInstance pi = runtimeService.startProcessInstanceByKey("forkJoin");
TaskQuery query = taskService.createTaskQuery()
                         .processInstanceId(pi.getId())
                         .orderByTaskName()
                         .asc();

List<Task> tasks = query.list();
assertEquals(2, tasks.size());

Task task1 = tasks.get(0);
assertEquals("Receive Payment", task1.getName());
Task task2 = tasks.get(1);
assertEquals("Ship Order", task2.getName());
```

当这两个任务完成后，第二个并行网关将加入两个执行，并且由于只有一个传出序列流，因此不会创建并发执行路径，只有* Archive Order *任务将处于活动状态。

注意，并行网关不需要*平衡*（即，对应的并行网关的输入/输出序列流的匹配数量）。 并行网关将简单地等待所有传入的序列流，并为每个传出的序列流创建并发执行路径，而不受流程模型中其他构造的影响。 因此，以下过程在BPMN 2.0中是合法的：

![bpmn.unbalanced.parallel.gateway](https://www.activiti.org/userguide/images/bpmn.unbalanced.parallel.gateway.png)

#### 8.4.3. Inclusive Gateway

##### Description

**包含网关**可以看作是独占网关和并行网关的组合。与专用网关一样，您可以定义传出顺序流的条件，并且包含网关将对它们进行评估。但主要区别在于包容性网关可以采用多个序列流，如并行网关。

包容性网关的功能基于传入和传出顺序流：

- ** fork：**评估所有传出序列流条件，并且对于评估为true的序列流条件，并行地遵循流，为每个序列流创建一个并发执行。
- ** join：**到达包含网关的所有并发执行在网关中等待，直到每个具有进程令牌的传入序列流都到达为止。这是与并行网关的重要区别。换句话说，包容性网关将只等待将要执行的传入序列流。在加入之后，该过程继续通过加入包含网关。

请注意，如果同一个包含网关有多个传入和传出顺序流，则包含网关可以具有**和加密行为**。在这种情况下，网关将首先连接具有进程令牌的所有传入序列流，然后在具有评估为true的条件的传出序列流拆分为多个并发执行路径。

##### Graphical Notation

包含的网关可视化为网关（菱形），内部带有* circle *符号。

![bpmn.inclusive.gateway](https://www.activiti.org/userguide/images/bpmn.inclusive.gateway.png)

##### XML representation

定义包含网关需要一行XML：

```
<inclusiveGateway id="myInclusiveGateway" />
```

实际行为（fork，join或两者）由连接到包含网关的顺序流定义。

例如，上面的模型归结为以下XML：

```
<startEvent id="theStart" />
<sequenceFlow id="flow1" sourceRef="theStart" targetRef="fork" />

<inclusiveGateway id="fork" />
<sequenceFlow sourceRef="fork" targetRef="receivePayment" >
  <conditionExpression xsi:type="tFormalExpression">${paymentReceived == false}</conditionExpression>
</sequenceFlow>
<sequenceFlow sourceRef="fork" targetRef="shipOrder" >
  <conditionExpression xsi:type="tFormalExpression">${shipOrder == true}</conditionExpression>
</sequenceFlow>

<userTask id="receivePayment" name="Receive Payment" />
<sequenceFlow sourceRef="receivePayment" targetRef="join" />

<userTask id="shipOrder" name="Ship Order" />
<sequenceFlow sourceRef="shipOrder" targetRef="join" />

<inclusiveGateway id="join" />
<sequenceFlow sourceRef="join" targetRef="archiveOrder" />

<userTask id="archiveOrder" name="Archive Order" />
<sequenceFlow sourceRef="archiveOrder" targetRef="theEnd" />

<endEvent id="theEnd" />
```

在上面的示例中，在启动流程后，如果流程变量paymentReceived == false和shipOrder == true，则将创建两个任务。 如果这些过程变量中只有一个等于true，则只创建一个任务。 如果没有条件计算为true并且抛出异常。 这可以通过指定默认的传出顺序流来防止。 在以下示例中，将创建一个任务，即船舶订单任务：

```
HashMap<String, Object> variableMap = new HashMap<String, Object>();
          variableMap.put("receivedPayment", true);
          variableMap.put("shipOrder", true);
          ProcessInstance pi = runtimeService.startProcessInstanceByKey("forkJoin");
TaskQuery query = taskService.createTaskQuery()
                         .processInstanceId(pi.getId())
                         .orderByTaskName()
                         .asc();

List<Task> tasks = query.list();
assertEquals(1, tasks.size());

Task task = tasks.get(0);
assertEquals("Ship Order", task.getName());
```

完成此任务后，第二个包含网关将加入两个执行，并且由于只有一个传出顺序流，因此不会创建并发执行路径，并且只有* Archive Order *任务将处于活动状态。

注意，包含网关不需要*平衡*（即，对应的包含网关的输入/输出序列流的匹配数量）。 包容性网关将简单地等待所有传入的序列流，并为每个传出序列流创建并发执行路径，而不受流程模型中其他构造的影响。

#### 8.4.4. Event-based Gateway

##### Description

基于事件的网关允许基于事件做出决定。网关的每个输出序列流需要连接到中间捕获事件。当进程执行到达基于事件的网关时，网关就像一个等待状态：执行被暂停。此外，对于每个传出序列流，创建事件订阅。

注意，基于事件的网关耗尽的序列流与普通的序列流不同。这些序列流从未实际“执行”。相反，它们允许流程引擎确定到达基于事件的网关的执行需要订阅哪些事件。以下限制适用：

- 基于事件的网关必须具有两个或更多传出序列流。
- 基于事件的网关只能连接到“intermediateCatchEvent”类型的元素。 （Activiti不支持基于事件的网关后接收任务。）
- 连接到基于事件的网关的`intermediateCatchEvent`必须具有单个传入序列流。

##### Graphical notation

基于事件的网关可视化为菱形，与其他BPMN网关一样，内部带有特殊图标。

![bpmn.event.based.gateway.notation](https://www.activiti.org/userguide/images/bpmn.event.based.gateway.notation.png)

##### XML representation

用于定义基于事件的网关的XML元素是`eventBasedGateway`。

##### Example(s)

以下过程是使用基于事件的网关的过程示例。 当执行到达基于事件的网关时，将暂停进程执行。 此外，流程实例订阅警报信号事件并创建一个在10分钟后触发的计时器。 这有效地使过程引擎等待十分钟以进行信号事件。 如果信号在10分钟内发生，则取消定时器并在信号之后继续执行。 如果未触发信号，则在取消定时器和信号订阅后继续执行。

![bpmn.event.based.gateway.example](https://www.activiti.org/userguide/images/bpmn.event.based.gateway.example.png)

```
<definitions id="definitions"
	xmlns="http://www.omg.org/spec/BPMN/20100524/MODEL"
	xmlns:activiti="http://activiti.org/bpmn"
	targetNamespace="Examples">

	<signal id="alertSignal" name="alert" />

	<process id="catchSignal">

		<startEvent id="start" />

		<sequenceFlow sourceRef="start" targetRef="gw1" />

		<eventBasedGateway id="gw1" />

		<sequenceFlow sourceRef="gw1" targetRef="signalEvent" />
		<sequenceFlow sourceRef="gw1" targetRef="timerEvent" />

		<intermediateCatchEvent id="signalEvent" name="Alert">
			<signalEventDefinition signalRef="alertSignal" />
		</intermediateCatchEvent>

		<intermediateCatchEvent id="timerEvent" name="Alert">
			<timerEventDefinition>
				<timeDuration>PT10M</timeDuration>
			</timerEventDefinition>
		</intermediateCatchEvent>

		<sequenceFlow sourceRef="timerEvent" targetRef="exGw1" />
		<sequenceFlow sourceRef="signalEvent" targetRef="task" />

		<userTask id="task" name="Handle alert"/>

		<exclusiveGateway id="exGw1" />

		<sequenceFlow sourceRef="task" targetRef="exGw1" />
		<sequenceFlow sourceRef="exGw1" targetRef="end" />

		<endEvent id="end" />
</process>
</definitions>
```

### 8.5. Tasks

#### 8.5.1. User Task

##### Description

A *用户任务*用于模拟需要由人类演员完成的工作。 当流程执行到达这样的用户任务时，在分配给该任务的用户或组的任务列表中创建新任务。

##### Graphical notation

用户任务可视化为典型任务（圆角矩形），左上角有一个小用户图标。

![bpmn.user.task](https://www.activiti.org/userguide/images/bpmn.user.task.png)

##### XML representation

用户任务在XML中定义如下。 * id *属性是必需的，* name *属性是可选的。

```
<userTask id="theTask" name="Important task" />
```

用户任务也可以具有描述。 事实上，任何BPMN 2.0元素都可以有描述。 通过添加** documentation **元素来定义描述。

```
<userTask id="theTask" name="Schedule meeting" >
  <documentation>
	  Schedule an engineering meeting for next week with the new hire.
  </documentation>
```

可以使用标准Java方式从任务中检索描述文本：

```
task.getDescription()
```

##### Due Date

每个任务都有一个字段，指示该任务的截止日期。 查询API可用于查询在特定日期之前，之后或之后到期的任务。

有一个活动扩展，允许您在任务定义中指定表达式，以设置创建任务时的初始截止日期。 表达式**应始终解析为java.util.Date，java.util.String（ISO8601格式化），ISO8601持续时间（例如PT50M）或null **。 例如，您可以使用在流程中先前表单中输入的日期或在先前的服务任务中计算的日期。 如果使用持续时间，则根据当前时间计算到期日期，并按给定时间段递增。 例如，当“PT30M”用作dueDate时，任务将在30分钟后完成。

```
<userTask id="theTask" name="Important task" activiti:dueDate="${dateVariable}"/>
```

任务的截止日期也可以使用`TaskService`或`TaskListener`使用传递的`DelegateTask`来更改。

##### User assignment

用户任务可以直接分配给用户。 这是通过定义** humanPerformer **子元素来完成的。 这样的* humanPerformer *定义需要一个实际定义用户的** resourceAssignmentExpression **。 目前，仅支持** formalExpressions **。

```
<process >

  ...

  <userTask id='theTask' name='important task' >
    <humanPerformer>
      <resourceAssignmentExpression>
        <formalExpression>kermit</formalExpression>
      </resourceAssignmentExpression>
    </humanPerformer>
  </userTask>
```

**只有一个**用户可以被指定为任务的人类执行者。 在Activiti术语中，该用户被称为**受让人**。 具有受让人的任务在其他人的任务列表中不可见，并且可以在受让人的所谓**个人任务列表**中找到。

可以通过TaskService检索直接分配给用户的任务，如下所示：

```
List<Task> tasks = taskService.createTaskQuery().taskAssignee("kermit").list();
```

任务也可以放在人们所谓的**候选任务列表**中。 在这种情况下，必须使用** potentialOwner **构造。 用法类似于* humanPerformer *构造。 请注意，需要为正式表达式中的每个元素定义，以指定它是用户还是组（引擎无法猜测）。

```
<process >

  ...

  <userTask id='theTask' name='important task' >
    <potentialOwner>
      <resourceAssignmentExpression>
        <formalExpression>user(kermit), group(management)</formalExpression>
      </resourceAssignmentExpression>
    </potentialOwner>
  </userTask>
```

任务使用* potential owner *构造定义，可以按如下方式检索（或者与受让人的任务类似的* TaskQuery *用法）：

```
 List<Task> tasks = taskService.createTaskQuery().taskCandidateUser("kermit");
```

这将检索kermit是**候选用户**的所有任务，即正式表达式包含* user（kermit）*。 这还将检索**分配给kermit是**成员的组的所有任务（例如，* group（management）*，如果kermit是该组的成员并且使用了Activiti标识组件）。 用户组在运行时解析，可以通过[IdentityService]（https://www.activiti.org/userguide/#apiEngine）进行管理。

如果没有给出具体信息，无论给定的文本字符串是用户还是组，引擎默认为group。 因此，以下内容与* group（accountancy）声明*时相同。


```
<formalExpression>accountancy</formalExpression>
```

##### Activiti extensions for task assignment

很明显，对于分配不复杂的用例而言，用户和组分配非常麻烦。 为避免这些复杂性，可以在用户任务上使用[自定义扩展]（https://www.activiti.org/userguide/#bpmnCustomExtensions）。

 -  **assignment属性**：此自定义扩展允许直接将用户任务分配给给定用户。

```
<userTask id="theTask" name="my task" activiti:assignee="kermit" />
```

这与使用上面定义的** humanPerformer **构造完全相同（https://www.activiti.org/userguide/#bpmnUserTaskAssignment）。

- **candidateUsers attribute**: 此自定义扩展允许使用户成为任务的候选者。

```
<userTask id="theTask" name="my task" activiti:candidateUsers="kermit, gonzo" />
```

这与使用上面定义的** potentialOwner **构造完全相同（https://www.activiti.org/userguide/#bpmnUserTaskAssignment）。 请注意，不需要像* potential owner *构造那样使用* user（kermit）*声明，因为该属性只能用于用户。

- **candidateGroups attribute**: 此自定义扩展允许使组成为任务的候选者。

```
<userTask id="theTask" name="my task" activiti:candidateGroups="management, accountancy" />
```

这与使用上面定义的** potentialOwner **构造完全相同（https://www.activiti.org/userguide/#bpmnUserTaskAssignment）。 请注意，与* potential owner *构造一样，不需要使用* group（management）*声明，因为该属性只能用于组。

- *candidateUsers* 和 *candidateGroups* can both be defined on the same user task.

注意：尽管Activiti提供了通过[IdentityService]（https://www.activiti.org/userguide/#apiEngine）公开的身份管理组件，但不会检查身份组件是否知道提供的用户。 这允许Activiti在嵌入到应用程序中时与现有的身份管理解决方案集成。

##### Custom identity link types (Experimental)

[[EXPERIMENTAL\]](https://www.activiti.org/userguide/#experimental)

The BPMN standard supports a single assigned user or **humanPerformer** or a set of users that form a potential pool of **potentialOwners**as defined in [User assignment](https://www.activiti.org/userguide/#bpmnUserTaskAssignment). In addition, Activiti defines [extension attribute elements](https://www.activiti.org/userguide/#bpmnUserTaskUserAssignmentExtension) for the User Task that can represent the task **assignee** or **candidate owner**.

The supported Activiti identity link types are:

```
public class IdentityLinkType {
  /* Activiti native roles */
  public static final String ASSIGNEE = "assignee";
  public static final String CANDIDATE = "candidate";
  public static final String OWNER = "owner";
  public static final String STARTER = "starter";
  public static final String PARTICIPANT = "participant";
}
```

BPMN标准和Activiti示例授权标识是** user **和** group **。 如上一节所述，Activiti身份管理实现不适用于生产用途，但应根据支持的授权方案进行扩展。

如果需要其他链接类型，可以使用以下语法将自定义资源定义为扩展元素：

```
<userTask id="theTask" name="make profit">
  <extensionElements>
    <activiti:customResource activiti:name="businessAdministrator">
      <resourceAssignmentExpression>
        <formalExpression>user(kermit), group(management)</formalExpression>
      </resourceAssignmentExpression>
    </activiti:customResource>
  </extensionElements>
</userTask>
```

自定义链接表达式添加到* TaskDefinition *类：

```
protected Map<String, Set<Expression>> customUserIdentityLinkExpressions =
      new HashMap<String, Set<Expression>>();
protected Map<String, Set<Expression>> customGroupIdentityLinkExpressions =
      new HashMap<String, Set<Expression>>();

public Map<String,
         Set<Expression>> getCustomUserIdentityLinkExpressions() {
  return customUserIdentityLinkExpressions;
}

public void addCustomUserIdentityLinkExpression(String identityLinkType,
      Set<Expression> idList)
  customUserIdentityLinkExpressions.put(identityLinkType, idList);
}

public Map<String,
       Set<Expression>> getCustomGroupIdentityLinkExpressions() {
  return customGroupIdentityLinkExpressions;
}

public void addCustomGroupIdentityLinkExpression(String identityLinkType,
       Set<Expression> idList) {
  customGroupIdentityLinkExpressions.put(identityLinkType, idList);
}
```

它们在运行时由* UserTaskActivityBehavior handleAssignments *方法填充。

最后，必须扩展* IdentityLinkType *类以支持自定义标识链接类型：

```
package com.yourco.engine.task;

public class IdentityLinkType
    extends org.activiti.engine.task.IdentityLinkType
{
    public static final String ADMINISTRATOR = "administrator";

    public static final String EXCLUDED_OWNER = "excludedOwner";
}
```

##### Custom Assignment via task listeners

如果以前的方法不够，可以在create事件上使用[任务监听器]（https://www.activiti.org/userguide/#taskListeners）委托自定义分配逻辑：

```
<userTask id="task1" name="My task" >
  <extensionElements>
    <activiti:taskListener event="create" class="org.activiti.MyAssignmentHandler" />
  </extensionElements>
</userTask>
```

传递给`TaskListener`实现的`DelegateTask`允许设置受理人和候选用户/组：

```
public class MyAssignmentHandler implements TaskListener {

  public void notify(DelegateTask delegateTask) {
    // Execute custom identity lookups here

    // and then for example call following methods:
    delegateTask.setAssignee("kermit");
    delegateTask.addCandidateUser("fozzie");
    delegateTask.addCandidateGroup("management");
    ...
  }

}
```

使用Spring时，可以使用上面部分中描述的自定义赋值属性，并使用[task listener]（https://www.activiti.org/userguide/#taskListeners）委托给Spring bean [ 表达式（https://www.activiti.org/userguide/#springExpressions），用于侦听task * create * events。 在下面的示例中，将通过调用`ldapService` Spring bean上的`findManagerOfEmployee`来设置受理人。 传递的* emp *参数是一个过程变量>。

```
<userTask id="task" name="My Task" activiti:assignee="${ldapService.findManagerForEmployee(emp)}"/>
```

这也适用于候选用户和组：

```
<userTask id="task" name="My Task" activiti:candidateUsers="${ldapService.findAllSales()}"/>
```

请注意，只有在调用方法的返回类型为`String`或`Collection <String>`（对于候选用户和组）时，这才有效：

```
public class FakeLdapService {

  public String findManagerForEmployee(String employee) {
    return "Kermit The Frog";
  }

  public List<String> findAllSales() {
    return Arrays.asList("kermit", "gonzo", "fozzie");
  }

}
```

#### 8.5.2. Script Task

##### Description

脚本任务是一种自动活动。 当进程执行到达脚本任务时，将执行相应的脚本。

##### Graphical Notation

脚本任务可视化为典型的BPMN 2.0任务（圆角矩形），矩形的左上角有一个小*脚本*图标。

![bpmn.scripttask](https://www.activiti.org/userguide/images/bpmn.scripttask.png)

##### XML representation

通过指定**脚本**和** scriptFormat **来定义脚本任务。

```
<scriptTask id="theScriptTask" name="Execute script" scriptFormat="groovy">
  <script>
    sum = 0
    for ( i in inputArray ) {
      sum += i
    }
  </script>
</scriptTask>
```

** scriptFormat **属性的值必须是与[JSR-223]兼容的名称（http://jcp.org/en/jsr/detail?id=223）（Java平台的脚本）。 默认情况下，JavaScript包含在每个JDK中，因此不需要任何其他jar。 如果要使用另一个（JSR-223兼容）脚本引擎，只需将相应的jar添加到类路径并使用适当的名称即可。 例如，Activiti单元测试通常使用Groovy，因为语法与Java非常相似。

请注意，Groovy脚本引擎与groovy-all jar捆绑在一起。 在2.0版之前，脚本引擎是常规Groovy jar的一部分。 因此，现在必须添加以下依赖项：

```
<dependency>
      <groupId>org.codehaus.groovy</groupId>
      <artifactId>groovy-all</artifactId>
      <version>2.x.x<version>
</dependency>
```

##### Variables in scripts

可以在脚本中使用通过执行到达脚本任务的所有过程变量。 在示例中，脚本变量*'inputArray'*实际上是一个过程变量（整数数组）。

```
<script>
    sum = 0
    for ( i in inputArray ) {
      sum += i
    }
</script>
```

也可以通过调用* execution.setVariable（“variableName”，variableValue）*来在脚本中设置流程变量。 默认情况下，不会自动存储任何变量（**注意：在Activiti 5.12之前就是这种情况！**）。 通过将`scriptTask`上的属性`autoStoreVariables`设置为`true`，可以自动存储脚本中定义的任何变量（例如上面示例中的* sum *）。 但是，**最佳做法是不要这样做并使用显式的execution.setVariable（）调用**，因为在某些最新版本的JDK中，自动存储变量对某些脚本语言不起作用。 有关详细信息，请参阅[此链接]（http://www.jorambarrez.be/blog/2013/03/25/bug-on-jdk-1-7-0_17-when-using-scripttask-in-activiti/）。

```
<scriptTask id="script" scriptFormat="JavaScript" activiti:autoStoreVariables="false">
```

此参数的默认值为“false”，这意味着如果从脚本任务定义中省略该参数，则所有声明的变量将仅在脚本持续时间内存在。

有关如何在脚本中设置变量的示例：

```
<script>
    def scriptVar = "test123"
    execution.setVariable("myVar", scriptVar)
</script>
```

注意：以下名称是保留的，**不能用作变量名：** out，out：print，lang：import，context，elcontext **。

##### Script results

通过将流程变量名称指定为脚本任务定义的*'activiti：resultVariable'*属性的文字值，可以将脚本任务的返回值分配给已存在或新的流程变量。 特定流程变量的任何现有值都将被脚本执行的结果值覆盖。 如果未指定结果变量名称，则会忽略脚本结果值。

```
<scriptTask id="theScriptTask" name="Execute script" scriptFormat="juel" activiti:resultVariable="myVar">
  <script>#{echo}</script>
</scriptTask>
```

在上面的示例中，脚本执行的结果（已解析的表达式*'＃{echo}'*的值）在脚本完成后设置为名为*'myVar'*的过程变量。

##### Security

使用* javascript *作为脚本语言也可以使用*安全脚本*。 请参阅[安全脚本部分]（https://www.activiti.org/userguide/#advancedSecureScripting）。

#### 8.5.3. Java Service Task

##### Description

Java服务任务用于调用外部Java类。

##### Graphical Notation

服务任务可视化为圆角矩形，左上角有一个小齿轮图标。

![bpmn.java.service.task](https://www.activiti.org/userguide/images/bpmn.java.service.task.png)

##### XML representation

有4种方法可以声明如何调用Java逻辑：

 - 指定实现JavaDelegate或ActivityBehavior的类
 - 评估解析为委托对象的表达式
 - 调用方法表达式
 - 评估值表达式

要指定在流程执行期间调用的类，需要由** activiti：class **属性提供完全限定的类名。

```
<serviceTask id="javaService"
             name="My Java Service Task"
             activiti:class="org.activiti.MyJavaDelegate" />
```

有关如何使用此类的更多详细信息，请参阅[实施部分]（https://www.activiti.org/userguide/#bpmnJavaServiceTaskImplementation）。

也可以使用解析为对象的表达式。 此对象必须遵循与使用`activiti：class`属性时创建的对象相同的规则（请参阅[更多]（https://www.activiti.org/userguide/#bpmnJavaServiceTaskImplementation））。

```
<serviceTask id="serviceTask" activiti:delegateExpression="${delegateExpressionBean}" />
```

这里，`delegateExpressionBean`是一个实现`JavaDelegate`接口的bean，在例如Spring容器中定义。

要指定应评估的UEL方法表达式，请使用属性** activiti：expression **。

```
<serviceTask id="javaService"
             name="My Java Service Task"
             activiti:expression="#{printer.printMessage()}" />
```

方法`printMessage`（不带参数）将在名为`printer`的命名对象上调用。

也可以使用表达式中使用的方法传递参数。

```
<serviceTask id="javaService"
             name="My Java Service Task"
             activiti:expression="#{printer.printMessage(execution, myVar)}" />
```

方法`printMessage`将在名为`printer`的对象上调用。 传递的第一个参数是`DelegateExecution`，它在表达式上下文中可用，默认情况下可用作`execution`。 传递的第二个参数是当前执行中名为`myVar`的变量的值。

要指定应评估的UEL值表达式，请使用属性** activiti：expression **。

```
<serviceTask id="javaService"
             name="My Java Service Task"
             activiti:expression="#{split.ready}" />
```

属性`ready`，`getReady`（不带参数）的getter方法将在名为`split`的命名bean上调用。 命名对象在执行的过程变量中解析，并在Spring上下文中（如果适用）。

##### Implementation

要实现可在流程执行期间调用的类，此类需要实现* org.activiti.engine.delegate.JavaDelegate *接口，并在* execute *方法中提供所需的逻辑。当流程执行到达此特定步骤时，它将执行该方法中定义的此逻辑，并将活动保留为默认的BPMN 2.0方式。

让我们创建一个Java类，可以用来将进程变量String更改为大写。这个类需要实现* org.activiti.engine.delegate.JavaDelegate *接口，这需要我们实现* execute（DelegateExecution）*方法。这个操作将由引擎调用，并且需要包含业务逻辑。可以通过[DelegateExecution]（http://activiti.org/javadocs/org/activiti/engine/delegate/DelegateExecution.html）界面访问和操作流程实例信息（如流程变量和其他信息）（单击链接以获取其操作的详细Javadoc）。

```
public class ToUppercase implements JavaDelegate {

  public void execute(DelegateExecution execution) throws Exception {
    String var = (String) execution.getVariable("input");
    var = var.toUpperCase();
    execution.setVariable("input", var);
  }

}
```

注意：**将仅为**定义的serviceTask创建该Java类的一个实例。所有流程实例共享将用于调用* execute（DelegateExecution）*的相同类实例。这意味着该类不能使用任何成员变量，并且必须是线程安全的，因为它可以从不同的线程同时执行。这也影响了[Field injection]（https://www.activiti.org/userguide/#serviceTaskFieldInjection）的处理方式。

流程定义中引用的类（即使用`activiti：class`）**在部署期间**不会被实例化。只有当流程执行在使用该类的过程中第一次到达时，才会创建该类的实例。如果找不到类，则抛出`ActivitiException`。这样做的原因是部署时的环境（更具体地说是* classpath *）通常与实际的运行时环境不同。例如，在Activiti Explorer中使用* ant *或业务归档上载来部署进程时，类路径不包含引用的类。

[[内部：非公开实施类\]]（https://www.activiti.org/userguide/#internal）也可以提供实现* org.activiti.engine.impl.pvm的类。 delegate.ActivityBehavior *接口。然后，实现可以访问更强大的* ActivityExecution *，例如也允许影响流程的控制流。但请注意，这不是一个很好的做法，应尽可能避免。因此，建议仅将* ActivityBehavior *接口用于高级用例，如果您确切知道自己在做什么。

##### Field Injection

可以将值注入委托类的字段中。支持以下类型的注射：

- 修正字符串值
- 表达

如果可用，则通过公共setter方法在委托类上注入值，遵循Java Bean命名约定（例如，字段`firstName`具有setter`setFirstName（...）`）。如果该字段没有可用的setter，则将在委托上设置private成员的值。某些环境中的SecurityManagers不允许修改私有字段，因此为要注入的字段公开公共setter方法更安全。

**无论流程定义中声明的值类型如何，注入目标上的setter / private字段的类型应始终为org.activiti.engine.delegate.Expression。解析表达式后，可以将其强制转换为适当的类型。**

使用*'actviiti：class'*属性时，支持字段注入。使用* activiti：delegateExpression *属性时，也可以进行字段注入，但是有关线程安全的特殊规则适用（请参阅下一节）。

以下代码段显示了如何将常量值注入到类中声明的字段中。请注意，我们需要在实际的字段注入声明**之前声明aextensionElements XML元素，这是BPMN 2.0 XML Schema的要求。

```
<serviceTask id="javaService"
    name="Java service invocation"
    activiti:class="org.activiti.examples.bpmn.servicetask.ToUpperCaseFieldInjected">
    <extensionElements>
      <activiti:field name="text" stringValue="Hello World" />
  </extensionElements>
</serviceTask>
```

类`ToUpperCaseFieldInjected`有一个字段`text`，类型为`org.activiti.engine.delegate.Expression`。 当调用`text.getValue（execution）`时，将返回配置的字符串值`Hello World`：

```
public class ToUpperCaseFieldInjected implements JavaDelegate {

  private Expression text;

  public void execute(DelegateExecution execution) {
    execution.setVariable("var", ((String)text.getValue(execution)).toUpperCase());
  }

}
```

或者，对于longs文本（例如内联电子邮件），可以使用*'activiti：string'*子元素：

```
<serviceTask id="javaService"
    name="Java service invocation"
    activiti:class="org.activiti.examples.bpmn.servicetask.ToUpperCaseFieldInjected">
  <extensionElements>
    <activiti:field name="text">
        <activiti:string>
          This is a long string with a lot of words and potentially way longer even!
      </activiti:string>
    </activiti:field>
  </extensionElements>
</serviceTask>
```

要注入在运行时动态解析的值，可以使用表达式。 这些表达式可以使用流程变量或Spring定义的bean（如果使用Spring）。 如[服务任务实现]（https://www.activiti.org/userguide/#bpmnJavaServiceTaskImplementation）中所述，当使用* activiti：class *时，服务任务中的所有流程实例共享Java类的实例 属性。 要在字段中动态注入值，可以在`org.activiti.engine.delegate.Expression`中注入值和方法表达式，可以使用`execute`方法中传递的`DelegateExecution`来计算/调用它们。

下面的示例类使用注入的表达式并使用当前的“DelegateExecution”解析它们。 传递* gender *变量时使用A * generBean *方法调用。 完整的代码和测试可以在`org.activiti.examples.bpmn.servicetask.JavaServiceTaskTest.testExpressionFieldInjection`中找到

```
<serviceTask id="javaService" name="Java service invocation"
  activiti:class="org.activiti.examples.bpmn.servicetask.ReverseStringsFieldInjected">

  <extensionElements>
    <activiti:field name="text1">
      <activiti:expression>${genderBean.getGenderString(gender)}</activiti:expression>
    </activiti:field>
    <activiti:field name="text2">
       <activiti:expression>Hello ${gender == 'male' ? 'Mr.' : 'Mrs.'} ${name}</activiti:expression>
    </activiti:field>
  </ extensionElements>
</ serviceTask>
public class ReverseStringsFieldInjected implements JavaDelegate {

  private Expression text1;
  private Expression text2;

  public void execute(DelegateExecution execution) {
    String value1 = (String) text1.getValue(execution);
    execution.setVariable("var1", new StringBuffer(value1).reverse().toString());

    String value2 = (String) text2.getValue(execution);
    execution.setVariable("var2", new StringBuffer(value2).reverse().toString());
  }
}
```

或者，您也可以将表达式设置为属性而不是子元素，以使XML更简洁。

```
<activiti:field name="text1" expression="${genderBean.getGenderString(gender)}" />
<activiti:field name="text1" expression="Hello ${gender == 'male' ? 'Mr.' : 'Mrs.'} ${name}" />
```

##### Field injection and thread safety

通常，使用Java委托和字段注入的服务任务是线程安全的。但是，有些情况下无法保证线程安全，具体取决于Activiti运行的设置或环境。

使用* activiti：class *属性时，使用字段注入始终是线程安全的。对于引用某个类的每个服务任务，将实例化一个新实例，并在创建实例时注入一次字段。在不同的任务或流程定义中多次重复使用同一个类是没有问题的。

使用* activiti：expression *属性时，无法使用字段注入。参数通过方法调用传递，并且这些参数始终是线程安全的。

使用* activiti：delegateExpression *属性时，委托实例的线程安全性将取决于表达式的解析方式。如果委托表达式在各种任务和/或流程定义中重用，并且表达式始终返回相同的实例，则使用字段注入**不是线程安全的**。让我们看几个例子来澄清。

假设表达式是* $ {factory.createDelegate（someVariable）} *，其中factory是引擎已知的Java bean（例如，使用Spring集成时的Spring bean），每次解析表达式时都会创建一个新实例。在这种情况下使用字段注入时，线程安全性没有问题：每次解析表达式时，都会在这个新实例上注入字段。

但是，假设表达式是* $ {someJavaDelegateBean} *，它解析为JavaDelegate类的实现，并且我们在一个创建每个bean的单例实例的环境中运行（如Spring，但也包括许多其他实例）。在不同的任务和/或流程定义中使用此表达式时，表达式将始终解析为同一实例。在这种情况下，使用字段注入不是线程安全的。例如：

```
<serviceTask id="serviceTask1" activiti:delegateExpression="${someJavaDelegateBean}">
    <extensionElements>
        <activiti:field name="someField" expression="${input * 2}"/>
    </extensionElements>
</serviceTask>

<!-- other process definition elements -->

<serviceTask id="serviceTask2" activiti:delegateExpression="${someJavaDelegateBean}">
    <extensionElements>
        <activiti:field name="someField" expression="${input * 2000}"/>
    </extensionElements>
</serviceTask>
```

此示例代码段有两个使用相同委托表达式的服务任务，但为* Expression *字段注入不同的值。 **如果表达式解析为同一个实例，则在执行进程时注入字段* someField *时，并发场景中可能存在竞争条件**。

解决这个问题的最简单的解决方案是

- 重写Java委托以使用表达式并通过方法参数将所需数据传递给委托。
- 每次解析委托表达式时，返回委托类的新实例。例如，当使用Spring时，这意味着bean的范围必须设置为** prototype **（例如，通过将@Scope（SCOPE_PROTOTYPE）注释添加到委托类）

从Activiti版本5.21开始，可以通过设置* delegateExpressionFieldInjectionMode *属性的值（它采用* org.activiti中的一个值）来配置流程引擎配置，以禁用在委托epxressions上使用字段注入。 .engine.imp.cfg.DelegateExpressionFieldInjectionMode * enum）。

可以进行以下设置：

- ** DISABLED **：使用委托表达式时完全禁用字段注入。不会尝试进行现场注入。在线程安全方面，这是最安全的模式。
- **兼容性**：在此模式下，行为将与5.21之前的行为完全相同：使用委托表达式时可以进行字段注入，并且在委托类中未定义字段时将引发异常。这当然是线程安全方面最不安全的模式，但它可能需要向后兼容，或者当委托表达式仅用于一组进程定义中的一个任务时可以安全使用（因此没有并发竞争）条件可能发生）。
- ** MIXED **：允许在使用delegateExpressions时进行注入，但在委托中未定义字段时不会引发异常。这允许混合行为，其中一些代表注入（例如因为他们不是单身人士）而有些则没有。
- ** Activiti 5.x版的默认模式是兼容性。**
- ** Activiti版本6.x的默认模式为MIXED。**

例如，假设我们正在使用* MIXED *模式，我们正在使用Spring集成。假设我们在Spring配置中有以下bean：

```
<bean id="singletonDelegateExpressionBean"
  class="org.activiti.spring.test.fieldinjection.SingletonDelegateExpressionBean" />

<bean id="prototypeDelegateExpressionBean"
  class="org.activiti.spring.test.fieldinjection.PrototypeDelegateExpressionBean"
  scope="prototype" />
```

第一个bean是一个普通的Spring bean，因此是一个单例。 第二个有* prototype *作为作用域，Spring容器每次请求bean时都会返回一个新实例。

给出以下流程定义：

```
<serviceTask id="serviceTask1" activiti:delegateExpression="${prototypeDelegateExpressionBean}">
  <extensionElements>
    <activiti:field name="fieldA" expression="${input * 2}"/>
    <activiti:field name="fieldB" expression="${1 + 1}"/>
    <activiti:field name="resultVariableName" stringValue="resultServiceTask1"/>
  </extensionElements>
</serviceTask>

<serviceTask id="serviceTask2" activiti:delegateExpression="${prototypeDelegateExpressionBean}">
  <extensionElements>
    <activiti:field name="fieldA" expression="${123}"/>
    <activiti:field name="fieldB" expression="${456}"/>
    <activiti:field name="resultVariableName" stringValue="resultServiceTask2"/>
  </extensionElements>
</serviceTask>

<serviceTask id="serviceTask3" activiti:delegateExpression="${singletonDelegateExpressionBean}">
  <extensionElements>
    <activiti:field name="fieldA" expression="${input * 2}"/>
    <activiti:field name="fieldB" expression="${1 + 1}"/>
    <activiti:field name="resultVariableName" stringValue="resultServiceTask1"/>
  </extensionElements>
</serviceTask>

<serviceTask id="serviceTask4" activiti:delegateExpression="${singletonDelegateExpressionBean}">
  <extensionElements>
    <activiti:field name="fieldA" expression="${123}"/>
    <activiti:field name="fieldB" expression="${456}"/>
    <activiti:field name="resultVariableName" stringValue="resultServiceTask2"/>
  </extensionElements>
</serviceTask>
```

我们有四个服务任务，第一个和第二个使用* $ {prototypeDelegateExpressionBean} *委托表达式，第三个和第四个使用* $ {singletonDelegateExpressionBean} *委托表达式。

让我们先看看原型bean：

```
public class PrototypeDelegateExpressionBean implements JavaDelegate {

  public static AtomicInteger INSTANCE_COUNT = new AtomicInteger(0);

  private Expression fieldA;
  private Expression fieldB;
  private Expression resultVariableName;

  public PrototypeDelegateExpressionBean() {
    INSTANCE_COUNT.incrementAndGet();
  }

  @Override
  public void execute(DelegateExecution execution) throws Exception {

    Number fieldAValue = (Number) fieldA.getValue(execution);
    Number fieldValueB = (Number) fieldB.getValue(execution);

    int result = fieldAValue.intValue() + fieldValueB.intValue();
    execution.setVariable(resultVariableName.getValue(execution).toString(), result);
  }

}
```

当我们在运行上面的流程定义的流程实例后检查* INSTANCE_COUNT *时，我们将得到* 2 *，因为每次解析* $ {prototypeDelegateExpressionBean} *时都会创建一个新实例。 可以在这里注入字段而没有任何问题，我们可以在这里看到三个* Expression *成员字段。

然而，单例bean看起来略有不同：

```
public class SingletonDelegateExpressionBean implements JavaDelegate {

  public static AtomicInteger INSTANCE_COUNT = new AtomicInteger(0);

  public SingletonDelegateExpressionBean() {
    INSTANCE_COUNT.incrementAndGet();
  }

  @Override
  public void execute(DelegateExecution execution) throws Exception {

    Expression fieldAExpression = DelegateHelper.getFieldExpression(execution, "fieldA");
    Number fieldA = (Number) fieldAExpression.getValue(execution);

    Expression fieldBExpression = DelegateHelper.getFieldExpression(execution, "fieldB");
    Number fieldB = (Number) fieldBExpression.getValue(execution);

    int result = fieldA.intValue() + fieldB.intValue();

    String resultVariableName = DelegateHelper.getFieldExpression(execution, "resultVariableName").getValue(execution).toString();
    execution.setVariable(resultVariableName, result);
  }

}
```

* INSTANCE_COUNT *将始终为* * *，因为它是单身。在此委托中，没有* Expression *成员字段。这是可能的，因为我们正在以* MIXED *模式运行。在* COMPATIBILITY *模式下，这将抛出异常，因为它期望成员字段在那里。 * DISABLED *模式也适用于这个bean，但它不允许使用上面的原型bean，它确实使用了字段注入。

在这个委托代码中，使用了** org.activiti.engine.delegate.DelegateHelper **类，它有一些有用的实用程序方法来执行相同的逻辑，但是当委托是单例时以线程安全的方式。它不是注入* Expression *，而是通过* getFieldExpression *方法获取。这意味着当涉及到服务任务xml时，字段的定义与单例bean的定义完全相同。如果你看一下上面的xml片段，你可以看到它们在定义上是相同的，只有实现逻辑不同。

（技术说明：* getFieldExpression *将内省BpmnModel并在执行方法时动态创建Expression，使其成为线程安全的）

- 对于Activiti 5.x版，DelegateHelper不能用于* ExecutionListener *或* TaskListener *（由于架构缺陷）。要创建这些侦听器的线程安全实例，请使用表达式或确保每次解析委托表达式时都会创建新实例。
- 对于Activiti版本6.x，DelegateHelper可以在* ExecutionListener *和* TaskListener *实现中工作。例如，在版本6.x中，可以使用** DelegateHelper **编写以下代码：

```
<extensionElements>
  <activiti:executionListener
      delegateExpression="${testExecutionListener}" event="start">
    <activiti:field name="input" expression="${startValue}" />
    <activiti:field name="resultVar" stringValue="processStartValue" />
  </activiti:executionListener>
</extensionElements>
```

Where *testExecutionListener* resolves to an instance implementing the ExecutionListener interface:

```
@Component("testExecutionListener")
public class TestExecutionListener implements ExecutionListener {

  @Override
  public void notify(DelegateExecution execution) {
    Expression inputExpression = DelegateHelper.getFieldExpression(execution, "input");
    Number input = (Number) inputExpression.getValue(execution);

    int result = input.intValue() * 100;

    Expression resultVarExpression = DelegateHelper.getFieldExpression(execution, "resultVar");
    execution.setVariable(resultVarExpression.getValue(execution).toString(), result);
  }

}
```

##### Service task results

通过将流程变量名称指定为*'activiti：resultVariable'*属性的文字值，可以将服务执行的返回值（对于仅使用表达式的服务任务）分配给已存在或新的流程变量。 服务任务定义。 特定流程变量的任何现有值都将被服务执行的结果值覆盖。 如果未指定结果变量名称，则会忽略服务执行结果值。

```
<serviceTask id="aMethodExpressionServiceTask"
    activiti:expression="#{myService.doSomething()}"
    activiti:resultVariable="myVar" />
```

在上面的示例中，服务执行的结果（*'doSomething（）'*方法调用的返回值对于在过程变量中作为名称*'myService'*提供的对象或作为Spring bean）在服务执行完成后设置为名为*'myVar'*的进程变量。

##### Handling exceptions

执行自定义逻辑时，通常需要捕获某些业务异常并在周围进程内处理它们。 Activiti提供了不同的选项。

###### Throwing BPMN Errors

可以从Service Tasks或Script Tasks中的用户代码中抛出BPMN错误。 为此，可以在JavaDelegates，脚本，表达式和委托表达式中抛出一个名为* BpmnError *的特殊ActivitiException。 引擎将捕获此异常并将其转发到适当的错误处理程序，例如边界错误事件或错误事件子进程。

```
public class ThrowBpmnErrorDelegate implements JavaDelegate {

  public void execute(DelegateExecution execution) throws Exception {
    try {
      executeBusinessLogic();
    } catch (BusinessException e) {
      throw new BpmnError("BusinessExceptionOccurred");
    }
  }

}
```

构造函数参数是一个错误代码，它将用于确定导致错误的错误处理程序。 有关如何捕获BPMN错误的信息，请参阅[边界错误事件]（https://www.activiti.org/userguide/#bpmnBoundaryErrorEvent）。

此机制仅应用于**业务故障**，该故障应由流程定义中建模的边界错误事件或错误事件子流程处理。 技术错误应由其他异常类型表示，通常不在进程内处理。

###### Exception mapping

也可以使用`mapException`扩展名将java异常直接映射到业务异常。 单一映射是最简单的形式：

```
<serviceTask id="servicetask1" name="Service Task" activiti:class="...">
  <extensionElements>
    <activiti:mapException
          errorCode="myErrorCode1">org.activiti.SomeException</activiti:mapException>
  </extensionElements>
</serviceTask>
```

在上面的代码中，如果在服务任务中抛出了“org.activiti.SomeException”的实例，它将被捕获并转换为具有给定errorCode的BPMN异常。 从这一点开始，它将像普通的BPMN异常一样处理。 任何其他异常都将被视为没有映射。 它将传播到API调用者。

可以使用`includeChildExceptions`属性在一行中映射某个异常的所有子异常。

```
<serviceTask id="servicetask1" name="Service Task" activiti:class="...">
  <extensionElements>
    <activiti:mapException errorCode="myErrorCode1"
           includeChildExceptions="true">org.activiti.SomeException</activiti:mapException>
  </extensionElements>
</serviceTask>
```

上面的代码将导致activiti将任何“SomeException”的直接或间接后代转换为具有给定错误代码的BPMN错误。 如果没有给出，`includeChildExceptions`将被视为“false”。

最通用的映射是默认映射。 默认地图是没有类的地图。 它将匹配任何java异常：


```
<serviceTask id="servicetask1" name="Service Task" activiti:class="...">
  <extensionElements>
    <activiti:mapException errorCode="myErrorCode1"/>
  </extensionElements>
</serviceTask>
```

从顶部到底部按顺序检查映射，并且将遵循第一个找到的匹配，默认映射除外。 只有在未成功检查所有地图后才会选择默认地图。 只有没有类的第一个地图才会被视为默认地图。 默认Map会忽略`includeChildExceptions`。

###### Exception Sequence Flow

[[INTERNAL: non-public implementation classes\]](https://www.activiti.org/userguide/#internal)

另一种选择是在发生某些异常时将流程执行路由到另一个路径。 以下示例显示了如何完成此操作。

```
<serviceTask id="javaService"
  name="Java service invocation"
  activiti:class="org.activiti.ThrowsExceptionBehavior">
</serviceTask>

<sequenceFlow id="no-exception" sourceRef="javaService" targetRef="theEnd" />
<sequenceFlow id="exception" sourceRef="javaService" targetRef="fixException" />
```

这里，服务任务有两个输出序列流，称为“异常”和“无异常”。 如果发生异常，此序列流ID将用于指导流程流程：

```
public class ThrowsExceptionBehavior implements ActivityBehavior {

  public void execute(ActivityExecution execution) throws Exception {
    String var = (String) execution.getVariable("var");

    PvmTransition transition = null;
    try {
      executeLogic(var);
      transition = execution.getActivity().findOutgoingTransition("no-exception");
    } catch (Exception e) {
      transition = execution.getActivity().findOutgoingTransition("exception");
    }
    execution.take(transition);
  }

}
```

##### Using an Activiti service from within a JavaDelegate

对于某些用例，可能需要在Java服务任务中使用Activiti服务（例如，如果callActivity不适合您的需要，则通过RuntimeService启动流程实例）。 * org.activiti.engine.delegate.DelegateExecution *允许通过* org.activiti.engine.EngineServices *界面轻松使用这些服务：

```
public class StartProcessInstanceTestDelegate implements JavaDelegate {

  public void execute(DelegateExecution execution) throws Exception {
    RuntimeService runtimeService = execution.getEngineServices().getRuntimeService();
    runtimeService.startProcessInstanceByKey("myProcess");
  }

}
```

所有Activiti服务API都可通过此界面获得。

由于使用这些API调用而发生的所有数据更改都将成为当前事务的一部分。 这也适用于具有依赖注入的环境，如Spring和CDI，有或没有启用JTA的数据源。 例如，下面的代码片段将与上面的代码片段相同，但现在注入RuntimeService而不是通过* org.activiti.engine.EngineServices *接口获取它。

```
@Component("startProcessInstanceDelegate")
public class StartProcessInstanceTestDelegateWithInjection {

    @Autowired
    private RuntimeService runtimeService;

    public void startProcess() {
      runtimeService.startProcessInstanceByKey("oneTaskProcess");
    }

}
```

**重要技术说明：**由于服务调用是作为当前事务的一部分完成的，因此在执行服务任务之前生成或更改的任何数据尚未刷新到数据库。 所有API调用都对数据库数据起作用，这意味着这些未提交的更改在服务任务的api调用中不是*可见*。

#### 8.5.4. Web Service Task

[[EXPERIMENTAL\]](https://www.activiti.org/userguide/#experimental)

##### Description

Web Service任务用于同步调用外部Web服务。

##### Graphical Notation

Web Service任务可视化与Java服务任务相同。

![bpmn.web.service.task](https://www.activiti.org/userguide/images/bpmn.web.service.task.png)

##### XML representation

要使用Web服务，我们需要导入其操作和复杂类型。 这可以通过使用指向Web服务的WSDL的import标记自动完成：

```
<import importType="http://schemas.xmlsoap.org/wsdl/"
	location="http://localhost:63081/counter?wsdl"
	namespace="http://webservice.activiti.org/" />
```

上一个声明告诉Activiti导入定义，但它不会为您创建项目定义和消息。 假设我们想调用一个名为* prettyPrint *的特定方法，因此我们需要为请求和响应消息创建相应的消息和项定义：

```
<message id="prettyPrintCountRequestMessage" itemRef="tns:prettyPrintCountRequestItem" />
<message id="prettyPrintCountResponseMessage" itemRef="tns:prettyPrintCountResponseItem" />

<itemDefinition id="prettyPrintCountRequestItem" structureRef="counter:prettyPrintCount" />
<itemDefinition id="prettyPrintCountResponseItem" structureRef="counter:prettyPrintCountResponse" />
```

在声明服务任务之前，我们必须定义实际引用Web服务的BPMN接口和操作。 基本上，我们定义和* interface *以及所需的* operation *。 对于每个操作，我们重复使用先前定义的消息进入和退出。 例如，以下声明定义* counter *接口和* prettyPrintCountOperation *操作：

```
<interface name="Counter Interface" implementationRef="counter:Counter">
	<operation id="prettyPrintCountOperation" name="prettyPrintCount Operation"
			implementationRef="counter:prettyPrintCount">
		<inMessageRef>tns:prettyPrintCountRequestMessage</inMessageRef>
		<outMessageRef>tns:prettyPrintCountResponseMessage</outMessageRef>
	</operation>
</interface>
```

然后，我们可以使用## WebService实现和对Web服务操作的引用来声明Web服务任务。

```
<serviceTask id="webService"
	name="Web service invocation"
	implementation="##WebService"
	operationRef="tns:prettyPrintCountOperation">
```

##### Web Service Task IO Specification

除非我们使用简单的方法进行数据输入和输出关联（见下文），否则每个Web服务任务都需要声明一个IO规范，该规范说明哪个是任务的输入和输出。 该方法非常简单，BPMN 2.0投诉，对于我们的prettyPrint示例，我们根据先前声明的项目定义定义输入和输出集：

```
<ioSpecification>
	<dataInput itemSubjectRef="tns:prettyPrintCountRequestItem" id="dataInputOfServiceTask" />
	<dataOutput itemSubjectRef="tns:prettyPrintCountResponseItem" id="dataOutputOfServiceTask" />
	<inputSet>
		<dataInputRefs>dataInputOfServiceTask</dataInputRefs>
	</inputSet>
	<outputSet>
		<dataOutputRefs>dataOutputOfServiceTask</dataOutputRefs>
	</outputSet>
</ioSpecification>
```

##### Web Service Task data input associations

有两种指定数据输入关联的方法：

 - 使用表达式
 - 使用简单的方法

要使用表达式指定数据输入关联，我们需要定义源项目和目标项目，并指定每个项目的字段之间的相应分配。 在以下示例中，我们分配项的前缀和后缀字段：

```
<dataInputAssociation>
	<sourceRef>dataInputOfProcess</sourceRef>
	<targetRef>dataInputOfServiceTask</targetRef>
	<assignment>
		<from>${dataInputOfProcess.prefix}</from>
		<to>${dataInputOfServiceTask.prefix}</to>
	</assignment>
	<assignment>
		<from>${dataInputOfProcess.suffix}</from>
		<to>${dataInputOfServiceTask.suffix}</to>
	</assignment>
</dataInputAssociation>
```

另一方面，我们可以使用更简单的简单方法。 * sourceRef *元素是Activiti变量名称，* targetRef *元素是项目定义的属性。 在下面的示例中，我们为*前缀*字段分配变量* PrefixVariable *的值，并为* suffix *字段分配变量* SuffixVariable *的值。

```
<dataInputAssociation>
	<sourceRef>PrefixVariable</sourceRef>
	<targetRef>prefix</targetRef>
</dataInputAssociation>
<dataInputAssociation>
	<sourceRef>SuffixVariable</sourceRef>
	<targetRef>suffix</targetRef>
</dataInputAssociation>
```

##### Web Service Task data output associations

有两种方法可以指定数据输出关联：

 - 使用表达式
 - 使用简单的方法

要使用表达式指定数据输出关联，我们需要定义目标变量和源表达式。 该方法非常简单，类似的数据输入关联：

```
<dataOutputAssociation>
	<targetRef>dataOutputOfProcess</targetRef>
	<transformation>${dataOutputOfServiceTask.prettyPrint}</transformation>
</dataOutputAssociation>
```

另一方面，我们可以使用更简单的简单方法。 * sourceRef *元素是项定义的属性，* targetRef *元素是Activiti变量名。 该方法非常简单，类似的数据输入关联：

```
<dataOutputAssociation>
	<sourceRef>prettyPrint</sourceRef>
	<targetRef>OutputVariable</targetRef>
</dataOutputAssociation>
```

#### 8.5.5. Business Rule Task

[[EXPERIMENTAL\]](https://www.activiti.org/userguide/#experimental)

##### Description

业务规则任务用于同步执行一个或多个规则。 Activiti使用Drools Expert，Drools规则引擎来执行业务规则。 目前，包含业务规则的.drl文件必须与定义业务规则任务的流程定义一起部署，以执行这些规则。 这意味着进程中使用的所有.drl文件都必须打包在进程BAR文件中，例如任务表单。 有关为Drools Expert创建业务规则的更多信息，请参阅[JBoss Drools]上的Drools文档（http://www.jboss.org/dolols/documentation）

如果您想插入规则任务的实现，例如 因为你想以不同的方式使用Drools，或者你想使用完全不同的规则引擎，那么你可以使用BusinessRuleTask上的类或表达式属性，它的行为与[ServiceTask]完全相同（https://www.activiti.org/ userguide/＃bpmnJavaServiceTask）

##### Graphical Notation

使用表格图标可视化业务规则任务。

![bpmn.business.rule.task](https://www.activiti.org/userguide/images/bpmn.business.rule.task.png)

##### XML representation

要执行与流程定义在同一BAR文件中部署的一个或多个业务规则，我们需要定义输入和结果变量。 对于输入变量定义，可以用逗号分隔过程变量列表。 输出变量定义只能包含一个变量名称，该变量名称将用于将已执行业务规则的输出对象存储在流程变量中。 请注意，结果变量将包含对象列表。 如果默认情况下未指定结果变量名，则使用org.activiti.engine.rules.OUTPUT。

以下业务规则任务执行使用流程定义部署的所有业务规则：

```
<process id="simpleBusinessRuleProcess">

  <startEvent id="theStart" />
  <sequenceFlow sourceRef="theStart" targetRef="businessRuleTask" />

  <businessRuleTask id="businessRuleTask" activiti:ruleVariablesInput="${order}"
      activiti:resultVariable="rulesOutput" />

  <sequenceFlow sourceRef="businessRuleTask" targetRef="theEnd" />

  <endEvent id="theEnd" />

</process>
```

业务规则任务还可以配置为仅从部署的.drl文件中执行一组定义的规则。 必须为此指定由逗号分隔的规则名称列表。

```
<businessRuleTask id="businessRuleTask" activiti:ruleVariablesInput="${order}"
      activiti:rules="rule1, rule2" />
```

在这种情况下，只执行rule1和rule2。

您还可以定义应从执行中排除的规则列表。

```
<businessRuleTask id="businessRuleTask" activiti:ruleVariablesInput="${order}"
      activiti:rules="rule1, rule2" exclude="true" />
```

在这种情况下，除了rule1和rule2之外，将执行与流程定义在同一BAR文件中部署的所有规则。

如前所述，另一个选择是自己挂钩BusinessRuleTask的实现：

```
<businessRuleTask id="businessRuleTask" activiti:class="${MyRuleServiceDelegate}" />
```

现在，Business RuleTask的行为与ServiceTask完全相同，但仍保留业务规则任务图标，以显示我们在此处执行业务规则处理。

#### 8.5.6. Email Task

Activiti允许通过向一个或多个收件人发送电子邮件的自动邮件服务任务来增强业务流程，包括支持cc，bcc，HTML内容......等。请注意，邮件任务不是** *官方* BPMN 2.0规范的任务（因此它没有专用的图标）。 因此，在Activiti中，邮件任务被实现为专用服务任务。

##### Mail server configuration

Activiti引擎通过具有SMTP功能的外部邮件服务器发送电子邮件。 要实际发送电子邮件，引擎需要知道如何到达邮件服务器。 可以在* activiti.cfg.xml *配置文件中设置以下属性：

| Property              | Required?                       | Description                                                  |
| --------------------- | ------------------------------- | ------------------------------------------------------------ |
| mailServerHost        | no                              | The hostname of your mail server (e.g. mail.mycorp.com). Default is `localhost` |
| mailServerPort        | yes, if not on the default port | The port for SMTP traffic on the mail server. The default is *25* |
| mailServerDefaultFrom | no                              | The default e-mail address of the sender of e-mails, when none is provided by the user. By default this is *activiti@activiti.org* |
| mailServerUsername    | if applicable for your server   | Some mail servers require credentials for sending e-mail. By default not set. |
| mailServerPassword    | if applicable for your server   | Some mail servers require credentials for sending e-mail. By default not set. |
| mailServerUseSSL      | if applicable for your server   | Some mail servers require ssl communication. By default set to false. |
| mailServerUseTLS      | if applicable for your server   | Some mail servers (for instance gmail) require TLS communication. By default set to false. |

##### Defining an Email Task

电子邮件任务是作为专用的[服务任务]（https://www.activiti.org/userguide/#bpmnJavaServiceTask）实现的，并通过为服务任务的* type *设置*'mail'*来定义。

```
<serviceTask id="sendMail" activiti:type="mail">
```

电子邮件任务由[现场注入]（https://www.activiti.org/userguide/#serviceTaskFieldInjection）配置。 这些属性的所有值都可以包含EL表达式，这些表达式在进程执行期间在运行时解析。 可以设置以下属性：

| Property              | Required? | Description                                                  |
| --------------------- | --------- | ------------------------------------------------------------ |
| to                    | yes       | The recipients if the e-mail. Multiple recipients are defined in a comma-separated list |
| from                  | no        | The sender e-mail address. If not provided, the [default configured](https://www.activiti.org/userguide/#bpmnEmailTaskServerConfiguration) from address is used. |
| subject               | no        | The subject of the e-mail.                                   |
| cc                    | no        | The cc’s of the e-mail. Multiple recipients are defined in a comma-separated list |
| bcc                   | no        | The bcc’s of the e-mail. Multiple recipients are defined in a comma-separated list |
| charset               | no        | Allows to change the charset of the email, which is necessary for many non-English languages. |
| html                  | no        | A piece of HTML that is the content of the e-mail.           |
| text                  | no        | The content of the e-mail, in case one needs to send plain none-rich e-mails. Can be used in combination with *html*, for e-mail clients that don’t support rich content. The client will then fall back to this text-only alternative. |
| htmlVar               | no        | The name of a process variable that holds the HTML that is the content of the e-mail. The key difference between this and html is that this content will have expressions replaced before being sent by the mail task. |
| textVar               | no        | The name of a process variable that holds the plain text content of the e-mail. The key difference between this and html is that this content will have expressions replaced before being sent by the mail task. |
| ignoreException       | no        | Whether an failure when handling the e-mail throws an ActivitiException. By default this is set to false. |
| exceptionVariableName | no        | When email handling does not throw an exception since *ignoreException = true* a variable with the given name is used to hold a failure message |

##### Example usage

以下XML代码段显示了使用电子邮件任务的示例。

```
<serviceTask id="sendMail" activiti:type="mail">
  <extensionElements>
    <activiti:field name="from" stringValue="order-shipping@thecompany.com" />
    <activiti:field name="to" expression="${recipient}" />
    <activiti:field name="subject" expression="Your order ${orderId} has been shipped" />
    <activiti:field name="html">
      <activiti:expression>
        <![CDATA[
          <html>
            <body>
              Hello ${male ? 'Mr.' : 'Mrs.' } ${recipientName},<br/><br/>

              As of ${now}, your order has been <b>processed and shipped</b>.<br/><br/>

              Kind regards,<br/>

              TheCompany.
            </body>
          </html>
        ]]>
      </activiti:expression>
    </activiti:field>
  </extensionElements>
</serviceTask>
```

结果如下：

![email.task.result](https://www.activiti.org/userguide/images/email.task.result.png)

#### 8.5.7. Mule Task

mule任务允许向Mule发送消息，增强Activiti的集成功能。 请注意，mule任务不是BPMN 2.0规范的** * *官方*任务（因此它没有专用图标）。 因此，在Activiti中，mule任务被实现为专用服务任务。

##### Defining an Mule Task

Mule任务是作为专用的[服务任务]（https://www.activiti.org/userguide/#bpmnJavaServiceTask）实现的，并通过为服务任务的* type *设置*'mule'*来定义。

```
<serviceTask id="sendMule" activiti:type="mule">
```

Mule任务由[field injection]（https://www.activiti.org/userguide/#serviceTaskFieldInjection）配置。 这些属性的所有值都可以包含EL表达式，这些表达式在进程执行期间在运行时解析。 可以设置以下属性：

| Property          | Required? | Description                                                  |
| ----------------- | --------- | ------------------------------------------------------------ |
| endpointUrl       | yes       | The Mule endpoint you want to invoke.                        |
| language          | yes       | The language you want to use to evaluate the payloadExpression field. |
| payloadExpression | yes       | An expression that will be the message’s payload.            |
| resultVariable    | no        | The name of the variable which will store the result of the invocation. |

##### Example usage

以下XML片段显示了使用Mule Task的示例。

```
<extensionElements>
  <activiti:field name="endpointUrl">
    <activiti:string>vm://in</activiti:string>
  </activiti:field>
  <activiti:field name="language">
    <activiti:string>juel</activiti:string>
  </activiti:field>
  <activiti:field name="payloadExpression">
    <activiti:string>"hi"</activiti:string>
  </activiti:field>
  <activiti:field name="resultVariable">
    <activiti:string>theVariable</activiti:string>
  </activiti:field>
</extensionElements>
```

#### 8.5.8. Camel Task

Camel任务允许向Camel发送消息和从Camel接收消息，从而增强了Activiti的集成功能。 请注意，Camel任务不是BPMN 2.0规范的** * *官方*任务（因此它没有专用图标）。 因此，在Activiti中，Camel任务被实现为专用服务任务。 另请注意，在项目中包含Activiti Camel模块以使用Camel任务功能。

##### Defining a Camel Task

Camel任务是作为专用的[服务任务]（https://www.activiti.org/userguide/#bpmnJavaServiceTask）实现的，并通过为服务任务的* type *设置*'camel'*来定义。

```
<serviceTask id="sendCamel" activiti:type="camel">
```

流程定义本身不需要服务任务上的camel类型定义。 集成逻辑全部委托给Camel容器。 默认情况下，Activiti Engine在Spring容器中查找camelContext bean。 camelContext bean定义将由Camel容器加载的Camel路由。 在以下示例中，路由是从特定Java包加载的，但您也可以直接在Spring配置中定义路由。

```
<camelContext id="camelContext" xmlns="http://camel.apache.org/schema/spring">
  <packageScan>
    <package>org.activiti.camel.route</package>
  </packageScan>
</camelContext>
```

有关Camel路线的更多文档，您可以在[Camel网站]（http://camel.apache.org/）上查看。 通过本文档中的一些小样本演示了基本概念。 在第一个示例中，我们将从Activiti工作流程中执行最简单的Camel调用。 我们称之为SimpleCamelCall。

如果要定义多个Camel上下文bean和/或想要使用不同的bean名称，可以在Camel任务定义上覆盖它，如下所示：

```
<serviceTask id="serviceTask1" activiti:type="camel">
  <extensionElements>
    <activiti:field name="camelContext" stringValue="customCamelContext" />
  </extensionElements>
</serviceTask>
```

##### Simple Camel Call example

可以在activiti-camel模块的org.activiti.camel.examples.simpleCamelCall包中找到与此示例相关的所有文件。 目标只是激活特定的驼峰路线。 首先，我们需要一个Spring上下文，其中包含前面提到的路由介绍。 这部分文件用于此目的：

```
<camelContext id="camelContext" xmlns="http://camel.apache.org/schema/spring">
  <packageScan>
    <package>org.activiti.camel.examples.simpleCamelCall</package>
  </packageScan>
</camelContext>

public class SimpleCamelCallRoute extends RouteBuilder {

  @Override
  public void configure() throws Exception {
    from("activiti:SimpleCamelCallProcess:simpleCall").to("log:org.activiti.camel.examples.SimpleCamelCall");
  }
}
```

该路由只记录消息正文，仅此而已。 请注意from端点的格式。 它由三部分组成：

| Endpoint Url Part      | Description                              |
| ---------------------- | ---------------------------------------- |
| activiti               | refers to Activiti endpoint              |
| SimpleCamelCallProcess | name of the process                      |
| simpleCall             | name of the Camel service in the process |

好的，我们的路线现在已正确配置并可供Camel访问。 现在是工作流程部分。 工作流程如下：

```
<process id="SimpleCamelCallProcess">
  <startEvent id="start"/>
  <sequenceFlow id="flow1" sourceRef="start" targetRef="simpleCall"/>

  <serviceTask id="simpleCall" activiti:type="camel"/>

  <sequenceFlow id="flow2" sourceRef="simpleCall" targetRef="end"/>
  <endEvent id="end"/>
</process>
```

##### Ping Pong example

Our example worked but nothing is really transferred between Camel and Activiti and there is not much merit in it. In this example we try to send and receive data to and from Camel. We send a string, camel concatenates something to it and returns back the result. The sender part is trivial, we send our message in form of a variable to Camel Task. Here is our caller code:

```
@Deployment
public void testPingPong() {
  Map<String, Object> variables = new HashMap<String, Object>();

  variables.put("input", "Hello");
  Map<String, String> outputMap = new HashMap<String, String>();
  variables.put("outputMap", outputMap);

  runtimeService.startProcessInstanceByKey("PingPongProcess", variables);
  assertEquals(1, outputMap.size());
  assertNotNull(outputMap.get("outputValue"));
  assertEquals("Hello World", outputMap.get("outputValue"));
}
```

变量“input”实际上是Camel路由的输入，outputMap用于从Camel捕获结果。 这个过程应该是这样的：

```
<process id="PingPongProcess">
  <startEvent id="start"/>
  <sequenceFlow id="flow1" sourceRef="start" targetRef="ping"/>
  <serviceTask id="ping" activiti:type="camel"/>
  <sequenceFlow id="flow2" sourceRef="ping" targetRef="saveOutput"/>
  <serviceTask id="saveOutput"  activiti:class="org.activiti.camel.examples.pingPong.SaveOutput" />
  <sequenceFlow id="flow3" sourceRef="saveOutput" targetRef="end"/>
  <endEvent id="end"/>
</process>
```

请注意SaveOutput Service任务，将“Output”变量的值从上下文存储到前面提到的OutputMap。 现在我们必须知道如何将变量发送给Camel并返回。 这里出现了Camel行为的概念。 变量传递给Camel的方式可通过CamelBehavior进行配置。 这里我们在我们的示例中使用Default one，之后会对其他的简短描述。 使用这样的代码，您可以配置所需的camel行为：

```
<serviceTask id="serviceTask1" activiti:type="camel">
  <extensionElements>
    <activiti:field name="camelBehaviorClass" stringValue="org.activiti.camel.impl.CamelBehaviorCamelBodyImpl" />
  </extensionElements>
</serviceTask>
```

如果您未指定和具体行为，则将设置org.activiti.camel.impl.CamelBehaviorDefaultImpl。 此行为将变量复制到同名的Camel属性。 无论选择的行为如何，如果驼峰消息体是一个映射，那么它的每个元素都被复制为一个变量，否则整个对象被复制到一个名为“camelBody”的特定变量中。 知道了，这条骆驼路线结束了我们的第二个例子：

```
@Override
public void configure() throws Exception {
  from("activiti:PingPongProcess:ping").transform().simple("${property.input} World");
}
```

在此路由中，字符串“world”连接到名为“input”的属性的末尾，结果将在消息正文中。 可以通过在java服务任务中检查“camelBody”变量并将其复制到“outputMap”并在测试用例中检查来访问它。 现在关于其默认行为的示例有效，让我们看看其他可能性是什么。 在启动每个camel路由时，Process Instance ID将被复制到具有特定名称“PROCESS_ID_PROPERTY”的camel属性中。 它稍后用于关联流程实例和驼峰路径。 它也可以在Camel路线中被利用。

Activiti中已经有三种不同的行为可供选择。 该行为可以被路由URL中的特定短语覆盖。 以下是覆盖URL中已定义行为的示例：


```
from("activiti:asyncCamelProcess:serviceTaskAsync2?copyVariablesToProperties=true").
```

下表概述了三种可用的camel行为：

| Behaviour                  | In Url                    | Description                                                  |
| -------------------------- | ------------------------- | ------------------------------------------------------------ |
| CamelBehaviorDefaultImpl   | copyVariablesToProperties | Copy Activiti variables as Camel properties                  |
| CamelBehaviorCamelBodyImpl | copyCamelBodyToBody       | Copy only Activiti variable named "camelBody" as camel message body |
| CamelBehaviorBodyAsMapImpl | copyVariablesToBodyAsMap  | Copy all the Activiti variables in a map as Camel message body |

上表解释了如何将Activiti变量转移到Camel。 下表说明了如何将Camel变量返回给Activiti。 这只能在路由URL中配置。

| Url                         | Description                                                  |
| --------------------------- | ------------------------------------------------------------ |
| Default                     | If Camel body is a map, copy each element as Activiti variable, otherwise copy the whole Camel body as "camelBody" Activiti variable |
| copyVariablesFromProperties | Copy Camel properties as Activiti variables of the same name |
| copyCamelBodyToBodyAsString | like default, but if camel Body is not a map, first convert it to String and then copy it in "camelBody" |
| copyVariablesFromHeader     | Additionally copy camel headers to Activiti variables of the same names |

##### Returning back the variables

上面提到的关于传递变量的内容，仅适用于变量传递的起始端，从Camel到Activiti的两个方向，反之亦然。
重要的是要注意，由于Activiti的特殊非阻塞行为，变量不会自动从Activiti返回给Camel。 为此，可以使用特殊语法。 Camel路由URL中可以有一个或多个参数，格式为`var.return.someVariableName`。 名称等于其中一个参数而没有`var.return`部分的所有变量将被视为输出变量，并将作为具有相同名称的camel属性复制回来。
例如，在以下路线中：

```
from("direct:start").to("activiti:process?var.return.exampleVar").to("mock:result");
```

名为“exampleVar”的Activiti变量将被视为输出变量，并将作为camel中的属性以相同名称复制回来。

##### Asynchronous Ping Pong example

以前的例子都是同步的。 工作流程停止，直到骆驼路线结束并返回。 在某些情况下，我们可能需要Activiti工作流程才能继续。 出于此目的，Camel服务任务的异步功能很有用。 您可以通过将Camel服务任务的async属性设置为true来利用此功能。

```
<serviceTask id="serviceAsyncPing" activiti:type="camel" activiti:async="true"/>
```

通过设置此功能，Activiti作业执行程序将异步激活指定的Camel路由。 在Camel路由中定义队列时，Activiti进程将继续执行Camel服务任务之后的活动。 Camel路由将从流程执行中异步执行。 如果要等待流程定义中某处的Camel服务任务响应，则可以使用接收任务。

```
<receiveTask id="receiveAsyncPing" name="Wait State" />
```

流程实例将等待直到收到信号，例如来自Camel。 在Camel中，您可以通过向正确的Activiti端点发送消息来向流程实例发送信号。

```
 from("activiti:asyncPingProcess:serviceAsyncPing").to("activiti:asyncPingProcess:receiveAsyncPing");
```

- constant string "activiti"
- process name
- receive task name

##### Instantiate workflow from Camel route

在我们之前的所有示例中，Activiti工作流程首先启动，Camel路径在工作流程中启动。 从另一方面也可以。 有可能从已经开始的驼峰路线实例化工作流程。 它与信令接收任务非常相似，只是最后一部分不在那里。 这是一个示例路线：

```
from("direct:start").to("activiti:camelProcess");
```

如您所见，url有两个部分，第一个是常量字符串“activiti”，第二个名称是进程的名称。 显然，该过程应该已经由引擎配置部署和启动。

还可以将进程的发起者设置为Camel头中提供的某个经过身份验证的用户标识。 要实现此目的，首先必须在流程定义中指定一个启动器变量：

```
<startEvent id="start" activiti:initiator="initiator" />
```

然后，假定用户标识包含在名为* CamelProcessInitiatorHeader *的Camel标头中，则Camel路由可以定义如下：

```
from("direct:startWithInitiatorHeader")
    .setHeader("CamelProcessInitiatorHeader", constant("kermit"))
    .to("activiti:InitiatorCamelCallProcess?processInitiatorHeaderName=CamelProcessInitiatorHeader");
```

#### 8.5.9. Manual Task

##### Description

A * Manual Task *定义BPM引擎外部的任务。 它用于模拟由某人完成的工作，引擎不需要知道，也没有系统或UI界面。 对于引擎，手动任务作为**传递活动**处理，从进程执行到达它的那一刻起自动继续进程。

##### Graphical Notation

手动任务可视化为圆角矩形，左上角有一个小*手*图标

![bpmn.manual.task](https://www.activiti.org/userguide/images/bpmn.manual.task.png)

##### XML representation

```
<manualTask id="myManualTask" name="Call client for more information" />
```

#### 8.5.10. Java Receive Task

##### Description

接收任务是一个等待某个消息到达的简单任务。 目前，我们只为此任务实现了Java语义。 当进程执行到达接收任务时，进程状态将提交给持久性存储。 这意味着进程将保持此等待状态，直到引擎接收到特定消息，这将触发继续通过接收任务的进程。

##### Graphical notation

接收任务可视化为任务（圆角矩形），左上角带有消息图标。 消息为白色（黑色消息图标将发送语义）

![bpmn.receive.task](https://www.activiti.org/userguide/images/bpmn.receive.task.png)

##### XML representation

```
<receiveTask id="waitState" name="wait" />
```

要继续当前正在此类接收任务中等待的流程实例，必须使用到达接收任务的执行ID来调用* runtimeService.signal（executionId）*。 以下代码段显示了它在实践中的工作原理：

```
ProcessInstance pi = runtimeService.startProcessInstanceByKey("receiveTask");
Execution execution = runtimeService.createExecutionQuery()
  .processInstanceId(pi.getId())
  .activityId("waitState")
  .singleResult();
assertNotNull(execution);

runtimeService.signal(execution.getId());
```

#### 8.5.11. Shell Task

##### Description

shell任务允许运行shell脚本和命令。 请注意，Shell任务不是** * BPMN 2.0规范的*官方*任务（因此它没有专用图标）。

##### Defining a shell task

shell任务是作为专用的[服务任务]（https://www.activiti.org/userguide/#bpmnJavaServiceTask）实现的，并通过为服务任务的* type *设置*'shell'*来定义。

```
<serviceTask id="shellEcho" activiti:type="shell">
```

Shell任务由[field injection]（https://www.activiti.org/userguide/#serviceTaskFieldInjection）配置。 这些属性的所有值都可以包含EL表达式，这些表达式在进程执行期间在运行时解析。 可以设置以下属性：

| Property          | Required? | Type       | Description                                                | Default                        |
| ----------------- | --------- | ---------- | ---------------------------------------------------------- | ------------------------------ |
| command           | yes       | String     | Shell command to execute.                                  |                                |
| arg0-5            | no        | String     | Parameter 0 to Parameter 5                                 |                                |
| wait              | no        | true/false | wait if necessary, until the shell process has terminated. | true                           |
| redirectError     | no        | true/false | Merge standard error with the standard output.             | false                          |
| cleanEnv          | no        | true/false | Shell process does not inherit current environment.        | false                          |
| outputVariable    | no        | String     | Name of variable to contain the output                     | Output is not recorded.        |
| errorCodeVariable | no        | String     | Name of variable to contain result error code              | Error level is not registered. |
| directory         | no        | String     | Default directory of shell process                         | Current directory              |

##### Example usage

以下XML代码段显示了使用shell Task的示例。 它运行shell脚本“cmd / c echo EchoTest”，等待它终止并将结果放入resultVar

```
<serviceTask id="shellEcho" activiti:type="shell" >
  <extensionElements>
    <activiti:field name="command" stringValue="cmd" />
    <activiti:field name="arg1" stringValue="/c" />
    <activiti:field name="arg2" stringValue="echo" />
    <activiti:field name="arg3" stringValue="EchoTest" />
    <activiti:field name="wait" stringValue="true" />
    <activiti:field name="outputVariable" stringValue="resultVar" />
  </extensionElements>
</serviceTask>
```

#### 8.5.12. Execution listener

**兼容性说明**：在发布5.3之后，我们发现执行侦听器和任务侦听器和表达式仍然在非公共API中。这些类在`org.activiti.engine.impl ...`的子包中，其中包含`impl`）。 `org.activiti.engine.impl.pvm.delegate.ExecutionListener`，`org.activiti.engine.impl.pvm.delegate.TaskListener`和`org.activiti.engine.impl.pvm.el.E​​xpression`已被弃用。从现在开始，你应该使用`org.activiti.engine.delegate.ExecutionListener`，`org.activiti.engine.delegate.TaskListener`和`org.activiti.engine.delegate.Expression`。在新的公开API中，已删除对“ExecutionListenerExecution.getEventSource（）”的访问。除了弃用编译器警告之外，现有代码应运行正常。但考虑切换到新的公共API接口（包名中没有.impl。）。

执行侦听器允许您在流程执行期间发生某些事件时执行外部Java代码或计算表达式。可以捕获的事件是：

- 流程实例的开始和结束。
- 进行过渡。
- 活动的开始和结束。
- 网关的开始和结束。
- 中间事件的开始和结束。
- 结束开始活动或开始结束活动。

以下流程定义包含3个执行侦听器：

```
<process id="executionListenersProcess">

  <extensionElements>
    <activiti:executionListener class="org.activiti.examples.bpmn.executionlistener.ExampleExecutionListenerOne" event="start" />
  </extensionElements>

  <startEvent id="theStart" />
  <sequenceFlow sourceRef="theStart" targetRef="firstTask" />

  <userTask id="firstTask" />
  <sequenceFlow sourceRef="firstTask" targetRef="secondTask">
    <extensionElements>
      <activiti:executionListener class="org.activiti.examples.bpmn.executionListener.ExampleExecutionListenerTwo" />
    </extensionElements>
  </sequenceFlow>

  <userTask id="secondTask" >
    <extensionElements>
      <activiti:executionListener expression="${myPojo.myMethod(execution.event)}" event="end" />
    </extensionElements>
  </userTask>
  <sequenceFlow sourceRef="secondTask" targetRef="thirdTask" />

  <userTask id="thirdTask" />
  <sequenceFlow sourceRef="thirdTask" targetRef="theEnd" />

  <endEvent id="theEnd" />

</process>
```

进程启动时会通知第一个执行侦听器。 监听器是一个外部Java类（如`ExampleExecutionListenerOne`），应该实现`org.activiti.engine.delegate.ExecutionListener`接口。 当事件发生时（在这种情况下是`end`事件），调用方法`notify（ExecutionListenerExecution execution）`。

```
public class ExampleExecutionListenerOne implements ExecutionListener {

  public void notify(ExecutionListenerExecution execution) throws Exception {
    execution.setVariable("variableSetInExecutionListener", "firstValue");
    execution.setVariable("eventReceived", execution.getEventName());
  }
}
```

也可以使用实现`org.activiti.engine.delegate.JavaDelegate`接口的委托类。 然后可以在其他构造中重用这些委托类，例如serviceTask的委托。

执行转换时将调用第二个执行侦听器。 请注意，`listener`元素没有定义`event`，因为只有`take`事件在转换时触发。 **在转换上定义侦听器时，将忽略event属性中的值。**

当活动“secondTask”结束时，将调用最后一个执行侦听器。 而不是在侦听器声明中使用`class`，而是定义`expression`，而不是在触发事件时评估/调用它。

```
<activiti:executionListener expression="${myPojo.myMethod(execution.eventName)}" event="end" />
```

与其他表达式一样，执行变量已解析并可以使用。 因为执行实现对象具有公开事件名称的属性，所以可以使用`execution.eventName`将事件名称传递给方法。

执行监听器还支持使用`delegateExpression`，[类似于服务任务]（https://www.activiti.org/userguide/#bpmnJavaServiceTaskXML）。
```
<activiti:executionListener event="start" delegateExpression="${myExecutionListenerBean}" />
```

在Activiti 5.12中，我们还引入了一种新类型的执行监听器，即org.activiti.engine.impl.bpmn.listener.ScriptExecutionListener。 此脚本执行侦听器允许您为执行侦听器事件执行一段脚本逻辑。


```
<activiti:executionListener event="start" class="org.activiti.engine.impl.bpmn.listener.ScriptExecutionListener" >
  <activiti:field name="script">
    <activiti:string>
      def bar = "BAR";  // local variable
      foo = "FOO"; // pushes variable to execution context
      execution.setVariable("var1", "test"); // test access to execution instance
      bar // implicit return value
    </activiti:string>
  </activiti:field>
  <activiti:field name="language" stringValue="groovy" />
  <activiti:field name="resultVariable" stringValue="myVar" />
</activiti:executionListener>
```

##### Field injection on execution listeners

使用配置了`class`属性的执行侦听器时，可以应用字段注入。 这与使用的[服务任务字段注入]（https://www.activiti.org/userguide/#serviceTaskFieldInjection）完全相同，其中包含现场注入提供的可能性的概述。

下面的片段显示了一个简单的示例流程，其中执行侦听器注入了字段。

```
<process id="executionListenersProcess">
  <extensionElements>
    <activiti:executionListener class="org.activiti.examples.bpmn.executionListener.ExampleFieldInjectedExecutionListener" event="start">
      <activiti:field name="fixedValue" stringValue="Yes, I am " />
      <activiti:field name="dynamicValue" expression="${myVar}" />
    </activiti:executionListener>
  </extensionElements>

  <startEvent id="theStart" />
  <sequenceFlow sourceRef="theStart" targetRef="firstTask" />

  <userTask id="firstTask" />
  <sequenceFlow sourceRef="firstTask" targetRef="theEnd" />

  <endEvent id="theEnd" />
</process>

public class ExampleFieldInjectedExecutionListener implements ExecutionListener {

  private Expression fixedValue;

  private Expression dynamicValue;

  public void notify(ExecutionListenerExecution execution) throws Exception {
    execution.setVariable("var", fixedValue.getValue(execution).toString() + dynamicValue.getValue(execution).toString());
  }
}
```

类`ExampleFieldInjectedExecutionListener`连接2个注入的字段（一个固定另一个动态）并将其存储在过程变量* var *中。

```
@Deployment(resources = {"org/activiti/examples/bpmn/executionListener/ExecutionListenersFieldInjectionProcess.bpmn20.xml"})
public void testExecutionListenerFieldInjection() {
  Map<String, Object> variables = new HashMap<String, Object>();
  variables.put("myVar", "listening!");

  ProcessInstance processInstance = runtimeService.startProcessInstanceByKey("executionListenersProcess", variables);

  Object varSetByListener = runtimeService.getVariable(processInstance.getId(), "var");
  assertNotNull(varSetByListener);
  assertTrue(varSetByListener instanceof String);

  // Result is a concatenation of fixed injected field and injected expression
  assertEquals("Yes, I am listening!", varSetByListener);
}
```

请注意，与线程安全相同的规则适用于服务任务。 有关更多信息，请阅读[相关部分]（https://www.activiti.org/userguide/#serviceTaskFieldInjectionThreadSafety）。

#### 8.5.13. Task listener

A * task listener *用于在发生某个与任务相关的事件时执行自定义Java逻辑或表达式。

任务侦听器只能作为[用户任务]的子元素添加到流程定义中（https://www.activiti.org/userguide/#bpmnUserTask）。 请注意，这也必须作为* BPMN 2.0 extensionElements *的子项和* activiti *名称空间中的子项发生，因为任务侦听器是特定于Activiti的构造。

```
<userTask id="myTask" name="My Task" >
  <extensionElements>
    <activiti:taskListener event="create" class="org.activiti.MyTaskCreateListener" />
  </extensionElements>
</userTask>
```

*任务侦听器*支持以下属性：

- ** event **（必需）：将在其上调用任务侦听器的任务事件的类型。可能的事件是
   - ** create **：在创建任务时发生**所有任务属性都设置**。
   - **赋值**：当任务分配给某人时发生。注意：当进程执行到达userTask时，首先会触发* assignment *事件，**之前** * create *事件被触发。这似乎是一种不自然的顺序，但原因是务实的：当收到* create *事件时，我们通常要检查任务的所有属性，包括受让人。
   - **完成**：在任务完成时以及从运行时数据中删除任务之前发生。
   - **删除**：在删除任务之前发生。请注意，当通过completeTask正常完成任务时也会执行它。
- ** class **：必须调用的委托类。该类必须实现`org.activiti.engine.delegate.TaskListener`接口。

```
public class MyTaskCreateListener implements TaskListener {

  public void notify(DelegateTask delegateTask) {
    // Custom logic goes here
  }

}
```

也可以使用[field injection]（https://www.activiti.org/userguide/#serviceTaskFieldInjection）将流程变量或执行传递给委托类。 请注意，在进程部署时会创建委托类的实例（与Activiti中的任何类委派一样），这意味着实例在所有流程实例执行之间共享。

 -  ** expression ** :(不能与* class *属性一起使用）：指定将在事件发生时执行的表达式。 可以将`DelegateTask`对象和事件名称（使用`task.eventName`）作为参数传递给被调用对象。

```
<activiti:taskListener event="create" expression="${myObject.callMethod(task, task.eventName)}" />
```

 - ** delegateExpression **允许指定一个解析为实现`TaskListener`接口的对象的表达式，[类似于服务任务]（https://www.activiti.org/userguide/#bpmnJavaServiceTaskXML）。
```
<activiti:taskListener event="create" delegateExpression="${myTaskListenerBean}" />
```

 - 在Activiti 5.12中，我们还引入了一种新类型的任务监听器，即org.activiti.engine.impl.bpmn.listener.ScriptTaskListener。 此脚本任务侦听器允许您为任务侦听器事件执行一段脚本逻辑。

```
<activiti:taskListener event="complete" class="org.activiti.engine.impl.bpmn.listener.ScriptTaskListener" >
  <activiti:field name="script">
    <activiti:string>
      def bar = "BAR";  // local variable
      foo = "FOO"; // pushes variable to execution context
      task.setOwner("kermit"); // test access to task instance
      bar // implicit return value
    </activiti:string>
  </activiti:field>
  <activiti:field name="language" stringValue="groovy" />
  <activiti:field name="resultVariable" stringValue="myVar" />
</activiti:taskListener>
```

#### 8.5.14. Multi-instance (for each)

##### Description

*多实例活动*是一种为业务流程中的某个步骤定义重复的方法。 在编程概念中，多实例匹配每个**构造的**：它允许为给定集合中的每个项执行某个步骤甚至完整的子流程，**顺序或并行**。

一个*多实例*是一个常规活动，它定义了额外的属性（所谓的''多实例*特征''），这将导致活动在运行时多次执行。 以下活动可以成为*多实例活动：*

- [User Task](https://www.activiti.org/userguide/#bpmnUserTask)
- [Script Task](https://www.activiti.org/userguide/#bpmnScriptTask)
- [Java Service Task](https://www.activiti.org/userguide/#bpmnJavaServiceTask)
- [Web Service Task](https://www.activiti.org/userguide/#bpmnWebserviceTask)
- [Business Rule Task](https://www.activiti.org/userguide/#bpmnBusinessRuleTask)
- [Email Task](https://www.activiti.org/userguide/#bpmnEmailTask)
- [Manual Task](https://www.activiti.org/userguide/#bpmnManualTask)
- [Receive Task](https://www.activiti.org/userguide/#bpmnReceiveTask)
- [(Embedded) Sub-Process](https://www.activiti.org/userguide/#bpmnSubProcess)
- [Call Activity](https://www.activiti.org/userguide/#bpmnCallActivity)

A [Gateway](https://www.activiti.org/userguide/#bpmnGateways) or [Event](https://www.activiti.org/userguide/#bpmnEvents) **cannot** become multi-instance.

As required by the spec, each parent execution of the created executions for each instance will have following variables:

- **nrOfInstances**: the total number of instances
- **nrOfActiveInstances**: the number of currently active, i.e. not yet finished, instances. For a sequential multi-instance, this will always be 1.
- **nrOfCompletedInstances**: the number of already completed instances.

These values can be retrieved by calling the `execution.getVariable(x)` method.

Additionally, each of the created executions will have an execution-local variable (i.e. not visible for the other executions, and not stored on process instance level) :

- **loopCounter**: indicates the *index in the for-each loop* of that particular instance. loopCounter variable can be renamed by Activiti **elementIndexVariable** attribute.

##### Graphical notation

如果活动是多实例，则由该活动底部的三个短行指示。 三个*垂直*行表示实例将并行执行，而三个*水平*行表示顺序执行。

![bpmn.multi.instance](https://www.activiti.org/userguide/images/bpmn.multi.instance.png)

##### Xml representation

要创建一个活动多实例，activity xml元素必须具有`multiInstanceLoopCharacteristics`子元素。

```
<multiInstanceLoopCharacteristics isSequential="false|true">
 ...
</multiInstanceLoopCharacteristics>
```

** isSequential **属性指示该活动的实例是顺序执行还是并行执行。

进入活动**时，实例数量**计算一次。 有几种配置方法。 正在使用** loopCardinality **子元素直接指定一个数字。

```
<multiInstanceLoopCharacteristics isSequential="false|true">
  <loopCardinality>5</loopCardinality>
</multiInstanceLoopCharacteristics>
```

Expressions that resolve to a positive number are also possible:

```
<multiInstanceLoopCharacteristics isSequential="false|true">
  <loopCardinality>${nrOfOrders-nrOfCancellations}</loopCardinality>
</multiInstanceLoopCharacteristics>
```

Another way to define the number of instances, is to specify the name of a process variable which is a collection using the `loopDataInputRef` child element. For each item in the collection, an instance will be created. Optionally, it is possible to set that specific item of the collection for the instance using the `inputDataItem` child element. This is shown in the following XML example:

```
<userTask id="miTasks" name="My Task ${loopCounter}" activiti:assignee="${assignee}">
  <multiInstanceLoopCharacteristics isSequential="false">
    <loopDataInputRef>assigneeList</loopDataInputRef>
    <inputDataItem name="assignee" />
  </multiInstanceLoopCharacteristics>
</userTask>
```

Suppose the variable `assigneeList` contains the values `\[kermit, gonzo, fozzie\]`. In the snippet above, three user tasks will be created in parallel. Each of the executions will have a process variable named `assignee` containing one value of the collection, which is used to assign the user task in this example.

The downside of the `loopDataInputRef` and `inputDataItem` is that 1) the names are pretty hard to remember and 2) due to the BPMN 2.0 schema restrictions they can’t contain expressions. Activiti solves this by offering the **collection** and **elementVariable**attributes on the `multiInstanceCharacteristics`:

```
<userTask id="miTasks" name="My Task" activiti:assignee="${assignee}">
  <multiInstanceLoopCharacteristics isSequential="true"
     activiti:collection="${myService.resolveUsersForTask()}" activiti:elementVariable="assignee" >
  </multiInstanceLoopCharacteristics>
</userTask>
```

A multi-instance activity ends when all instances are finished. However, it is possible to specify an expression that is evaluated every time one instance ends. When this expression evaluates to true, all remaining instances are destroyed and the multi-instance activity ends, continuing the process. Such an expression must be defined in the **completionCondition** child element.

```
<userTask id="miTasks" name="My Task" activiti:assignee="${assignee}">
  <multiInstanceLoopCharacteristics isSequential="false"
     activiti:collection="assigneeList" activiti:elementVariable="assignee" >
    <completionCondition>${nrOfCompletedInstances/nrOfInstances >= 0.6 }</completionCondition>
  </multiInstanceLoopCharacteristics>
</userTask>
```

In this example, there will be parallel instances created for each element of the `assigneeList` collection. However, when 60% of the tasks are completed, the other tasks are deleted and the process continues.

##### Boundary events and multi-instance

Since a multi-instance is a regular activity, it is possible to define a [boundary event](https://www.activiti.org/userguide/#bpmnBoundaryEvent) on its boundary. In case of an interrupting boundary event, when the event is caught, **all instances** that are still active will be destroyed. Take for example following multi-instance subprocess:

![bpmn.multi.instance.boundary.event](https://www.activiti.org/userguide/images/bpmn.multi.instance.boundary.event.png)

Here, all instances of the subprocess will be destroyed when the timer fires, regardless of how many instances there are or which inner activities are currently not yet completed.

##### Multi instance and execution listeners

(Valid for Activiti 5.18 and up)

There is a caveat when using execution listeners in combination with multi instance. Take for example the following snippet of BPMN 2.0 xml, which is defined on the same level as the *multiInstanceLoopCharacteristics* xml element is set:

```
<extensionElements>
    <activiti:executionListener event="start" class="org.activiti.MyStartListener"/>
		<activiti:executionListener event="end" class="org.activiti.MyEndListener"/>
</extensionElements>
```

For a normal BPMN activity, there would be an invocation of these listeners when the activity is started and ended.

However, when the activity is multi instance, the behavior is different:

- When the multi instance activity is entered, before any of the *inner* activities is executed, a start event is thrown. The *loopCounter*variable is not yet set (is null).
- For each of the actual activities visited, a start event is thrown. The *loopCounter* variable is set.

The same reasoning applies for the end event:

- When the actual activity is left, an end even is thrown. The *loopCounter* variable is set.
- When the multi instance activity has finished as a whole, an end event is thrown. The *loopCounter* variable is not set.

For example:

```
<subProcess id="subprocess1" name="Sub Process">
  <extensionElements>
    <activiti:executionListener event="start" class="org.activiti.MyStartListener"/>
    <activiti:executionListener event="end" class="org.activiti.MyEndListener"/>
  </extensionElements>
  <multiInstanceLoopCharacteristics isSequential="false">
    <loopDataInputRef>assignees</loopDataInputRef>
    <inputDataItem name="assignee"></inputDataItem>
  </multiInstanceLoopCharacteristics>
  <startEvent id="startevent2" name="Start"></startEvent>
  <endEvent id="endevent2" name="End"></endEvent>
  <sequenceFlow id="flow3" name="" sourceRef="startevent2" targetRef="endevent2"></sequenceFlow>
</subProcess>
```

In this example, suppose the *assignees* list has three items. The following happens at runtime:

- A start event is thrown for the multi instance as a whole. The *start* execution listener is invoked. The *loopCounter* nor the *assignee*variable will not be set (i.e. they will be null).
- A start event is thrown for each activity instance. The *start* execution listener is invoked three times. The *loopCounter* nor the *assignee*variable will be set (i.e. different from null).
- So in total, the start execution listener is invoked four times.

Note that the same applies when the *multiInstanceLoopCharacteristics* are defined on something else than a subprocess too. For example in case the example above would be a simple userTask, the same reasoning still applies.

#### 8.5.15. Compensation Handlers

##### Description

[[EXPERIMENTAL\]](https://www.activiti.org/userguide/#experimental)

If an activity is used for compensating the effects of another activity, it can be declared to be a compensation handler. Compensation handlers are not contained in normal flow and are only executed when a compensation event is thrown.

Compensation handlers must not have incoming or outgoing sequence flows.

A compensation handler must be associated with a compensation boundary event using a directed association.

##### Graphical notation

If an activity is a compensation handler, the compensation event icon is displayed in the center bottom area. The following excerpt from a process diagram shows a service task with an attached compensation boundary event which is associated to a compensation handler. Notice the compensation handler icon in the bottom canter area of the "cancel hotel reservation" service task.

![bpmn.boundary.compensation.event](https://www.activiti.org/userguide/images/bpmn.boundary.compensation.event.png)

##### XML representation

In order to declare an activity to be a compensation handler, we need to set the attribute isForCompensation to true:

```
1
2<serviceTask id="undoBookHotel" isForCompensation="true" activiti:class="...">
</serviceTask>
```

### 8.6. Sub-Processes and Call Activities

#### 8.6.1. Sub-Process

##### Description

A *Sub-Process* is an activity that contains other activities, gateways, events, etc. which on itself form a process that is part of the bigger process. A *Sub-Process* is completely defined inside a parent process (that’s why it’s often called an *embedded* Sub-Process).

Sub-Processes have two major use cases:

- Sub-Processes allow **hierarchical modeling**. Many modeling tools allow that Sub-Processes can be *collapsed*, hiding all the details of the Sub-Process and displaying a high-level end-to-end overview of the business process.
- A Sub-Process creates a new **scope for events**. Events that are thrown during execution of the Sub-Process, can be caught by [a boundary event](https://www.activiti.org/userguide/#bpmnBoundaryEvent) on the boundary of the Sub-Process, thus creating a scope for that event limited to the Sub-Process.

Using a Sub-Process does impose some constraints:

- A Sub-Process can only have **one none start event**, no other start event types are allowed. A Sub-Process must **at least have one end event**. Note that the BPMN 2.0 specification allows to omit the start and end events in a Sub-Process, but the current Activiti implementation does not support this.
- **Sequence flow cannot cross Sub-Process boundaries.**

##### Graphical Notation

A Sub-Process is visualized as a typical activity, i.e. a rounded rectangle. In case the Sub-Process is *collapsed*, only the name and a plus-sign are displayed, giving a high-level overview of the process:

![bpmn.collapsed.subprocess](https://www.activiti.org/userguide/images/bpmn.collapsed.subprocess.png)

In case the Sub-Process is *expanded*, the steps of the Sub-Process are displayed within the Sub-Process boundaries:

![bpmn.expanded.subprocess](https://www.activiti.org/userguide/images/bpmn.expanded.subprocess.png)

One of the main reasons to use a Sub-Process, is to define a scope for a certain event. The following process model shows this: both the *investigate software/investigate hardware* tasks need to be done in parallel, but both tasks need to be done within a certain time, before *Level 2 support* is consulted. Here, the scope of the timer (i.e. which activities must be done in time) is constrained by the Sub-Process.

![bpmn.subprocess.with.boundary.timer](https://www.activiti.org/userguide/images/bpmn.subprocess.with.boundary.timer.png)

##### XML representation

A Sub-Process is defined by the *subprocess* element. All activities, gateways, events, etc. that are part of the Sub-Process, need to be enclosed within this element.

```
<subProcess id="subProcess">

  <startEvent id="subProcessStart" />

  ... other Sub-Process elements ...

  <endEvent id="subProcessEnd" />

 </subProcess>
```

#### 8.6.2. Event Sub-Process

##### Description

事件子流程是BPMN 2.0中的新增功能。事件子流程是由事件触发的子流程。可以在流程级别或任何子流程级别添加事件子流程。用于触发事件子进程的事件使用start事件进行配置。由此可见，事件子流程不支持任何启动事件。可以使用诸如消息事件，错误事件，信号事件，定时器事件或补偿事件之类的事件来触发事件子过程。在创建托管事件子流程的范围（流程实例或子流程）时，将创建对启动事件的订阅。范围被销毁时删除订阅。

事件子过程可能是中断或不中断。中断子进程取消当前作用域中的所有执行。非中断事件子进程会产生新的并发执行。虽然中断事件子流程只能在每次激活托管它的范围时触发一次，但可以多次触发非中断事件子流程。使用触发事件子流程的启动事件来配置子流程是否正在中断的事实。

事件子流程不得具有任何传入或传出的序列流。由于事件子事件由事件触发，因此传入的序列流没有意义。当事件子进程结束时，当前作用域结束（在中断事件子进程的情况下），或者为非中断子进程生成的并发执行结束。

**Current limitations:**

- Activiti only supports interrupting Event Sub-Processes.
- Activiti only supports Event Sub-Process triggered using an Error Start Event or Message Start Event.

##### Graphical Notation

An Event Sub-Process might be visualized as a an [embedded subprocess](https://www.activiti.org/userguide/#bpmnSubProcessGraphicalNotation) with a dotted outline.

![bpmn.subprocess.eventSubprocess](https://www.activiti.org/userguide/images/bpmn.subprocess.eventSubprocess.png)

##### XML representation

An Event Sub-Process is represented using XML in the same way as a an embedded subprocess. In addition the attribute `triggeredByEvent` must have the value `true`:

```
1
2
3<subProcess id="eventSubProcess" triggeredByEvent="true">
	...
</subProcess>
```

##### Example

The following is an example of an Event Sub-Process triggered using an Error Start Event. The Event Sub-Process is located at the "process level", i.e. is scoped to the process instance:

![bpmn.subprocess.eventSubprocess.example.1](https://www.activiti.org/userguide/images/bpmn.subprocess.eventSubprocess.example.1.png)

This is how the Event Sub-Process would look like in XML:

```
1
2
3
4
5
6
7<subProcess id="eventSubProcess" triggeredByEvent="true">
	<startEvent id="catchError">
		<errorEventDefinition errorRef="error" />
	</startEvent>
	<sequenceFlow id="flow2" sourceRef="catchError" targetRef="taskAfterErrorCatch" />
	<userTask id="taskAfterErrorCatch" name="Provide additional data" />
</subProcess>
```

As already stated, an Event Sub-Process can also be added to an embedded subprocess. If it is added to an embedded subprocess, it becomes an alternative to a boundary event. Consider the two following process diagrams. In both cases the embedded subprocess throws an error event. Both times the error is caught and handled using a user task.

![bpmn.subprocess.eventSubprocess.example.2a](https://www.activiti.org/userguide/images/bpmn.subprocess.eventSubprocess.example.2a.png)

as opposed to:

![bpmn.subprocess.eventSubprocess.example.2b](https://www.activiti.org/userguide/images/bpmn.subprocess.eventSubprocess.example.2b.png)

In both cases the same tasks are executed. However, there are differences between both modelling alternatives:

- The embedded subprocess is executed using the same execution which executed the scope it is hosted in. This means that an embedded subprocess has access to the variables local to it’s scope. When using a boundary event, the execution created for executing the embedded subprocess is deleted by the sequence flow leaving the boundary event. This means that the variables created by the embedded subprocess are not available anymore.
- When using an Event Sub-Process, the event is completely handled by the subprocess it is added to. When using a boundary event, the event is handled by the parent process.

These two differences can help you decide whether a boundary event or an embedded subprocess is better suited for solving a particular process modeling / implementation problem.

#### 8.6.3. Transaction subprocess

[[EXPERIMENTAL\]](https://www.activiti.org/userguide/#experimental)

##### Description

A transaction subprocess is an embedded subprocess, which can be used to group multiple activities to a transaction. A transaction is a logical unit of work which allows to group a set of individual activities, such that they either succeed or fail collectively.

**Possible outcomes of a transaction:** A transaction can have three different outcomes:

- A transaction is *successful*, if it is neither cancelled not terminated by a hazard. If a transaction subprocess is successful, it is left using the outgoing sequenceflow(s). A successful transaction might be compensated if a compensation event is thrown later in the process. *Note:* just as "ordinary" embedded subprocesses, a transaction may be compensated after successful completion using an intermediary throwing compensation event.
- A transaction is *cancelled*, if an execution reaches the cancel end event. In that case, all executions are terminated and removed. A single remaining execution is then set to the cancel boundary event, which triggers compensation. After compensation is completed, the transaction subprocess is left using the outgoing sequence flow(s) of the cancel boundary event.
- A transaction is ended by a *hazard*, if an error event is thrown, that is not caught within the scope of the transaction subprocess. (This also applies if the error is caught on the boundary of the transaction subprocess.) In this case, compensation is not performed.

The following diagram illustrates the three different outcomes:

![bpmn.transaction.subprocess.example.1](https://www.activiti.org/userguide/images/bpmn.transaction.subprocess.example.1.png)

**Relation to ACID transactions:** it is important not to confuse the bpmn transaction subprocess with technical (ACID) transactions. The bpmn transaction subprocess is not a way to scope technical transactions. In order to understand transaction management in Activiti, read the section on [concurrency and transactions](https://www.activiti.org/userguide/#bpmnConcurrencyAndTransactions). A bpmn transaction is different from a technical transaction in the following ways:

- While an ACID transaction is typically short lived, a bpmn transaction may take hours, days or even months to complete. (Consider the case where one of the activities grouped by a transaction is a usertask, typically people have longer response times than applications. Or, in another situation, a bpmn transaction might wait for some business event to occur, like the fact that a particular order has been fulfilled.) Such operations usually take considerably longer to complete than updating a record in a database, or storing a message using a transactional queue.
- Because it is impossible to scope a technical transaction to the duration of a business activity, a bpmn transaction typically spans multiple ACID transactions.
- Since a bpmn transaction spans multiple ACID transactions, we loose ACID properties. For example, consider the example given above. Let’s assume the "book hotel" and the "charge credit card" operations are performed in separate ACID transactions. Let’s also assume that the "book hotel" activity is successful. Now we have an intermediary inconsistent state, because we have performed an hotel booking but have not yet charged the credit card. Now, in an ACID transaction, we would also perform different operations sequentially and thus also have an intermediary inconsistent state. What is different here, is that the inconsistent state is visible outside of the scope of the transaction. For example, if the reservations are made using an external booking service, other parties using the same booking service might already see that the hotel is booked. This means, that when implementing business transactions, we completely loose the isolation property (Granted: we usually also relax isolation when working with ACID transactions to allow for higher levels of concurrency, but there we have fine grained control and intermediary inconsistencies are only present for very short periods of times).
- A bpmn business transaction can also not be rolled back in the traditional sense. Since it spans multiple ACID transactions, some of these ACID transactions might already be committed at the time the bpmn transaction is cancelled. At this point, they cannot be rolled back anymore.

Since bpmn transactions are long-running in nature, the lack of isolation and a rollback mechanism need to be dealt with differently. In practice, there is usually no better solution than to deal with these problems in a domain specific way:

- The rollback is performed using compensation. If a cancel event is thrown in the scope of a transaction, the effects of all activities that executed successfully and have a compensation handler are compensated.
- The lack of isolation is also often dealt with using domain specific solutions. For instance, in the example above, an hotel room might appear to be booked to a second customer, before we have actually made sure that the first customer can pay for it. Since this might be undesirable from a business perspective, a booking service might choose to allow for a certain amount of overbooking.
- In addition, since the transaction can be aborted in case of a hazard, the booking service has to deal with the situation where a hotel room is booked but payment is never attempted (since the transaction was aborted). In that case the booking service might choose a strategy where a hotel room is reserved for a maximum period of time and if payment is not received until then, the booking is cancelled.

To sum it up: while ACID transactions offer a generic solution to such problems (rollback, isolation levels and heuristic outcomes), we need to find domain specific solutions to these problems when implementing business transactions.

**Current limitations:**

- The BPMN specification requires that the process engine reacts to events issued by the underlying transaction protocol and for instance that a transaction is cancelled, if a cancel event occurs in the underlying protocol. As an embeddable engine, Activiti does currently not support this. (For some ramifications of this, see paragraph on consistency below.)

**Consistency on top of ACID transactions and optimistic concurrency:** A bpmn transaction guarantees consistency in the sense that either all activities compete successfully, or if some activity cannot be performed, the effects of all other successful activities are compensated. So either way we end up in a consistent state. However, it is important to recognize that in Activiti, the consistency model for bpmn transactions is superposed on top of the consistency model for process execution. Activiti executes processes in a transactional way. Concurrency is addressed using optimistic locking. In Activiti, bpmn error, cancel and compensation events are built on top of the same acid transactions and optimistic locking. For example, a cancel end event can only trigger compensation if it is actually reached. It is not reached if some undeclared exception is thrown by a service task before. Or, the effects of a compensation handler cannot be committed if some other participant in the underlying ACID transaction sets the transaction to the state rollback-only. Or, when two concurrent executions reach a cancel end event, compensation might be triggered twice and fail with an optimistic locking exception. All of this is to say that when implementing bpmn transactions in Activiti, the same set of rules apply as when implementing "ordinary" processes and subprocesses. So to effectively guarantee consistency, it is important to implement processes in a way that does take the optimistic, transactional execution model into consideration.

##### Graphical Notation

An transaction subprocess might be visualized as a an [embedded subprocess](https://www.activiti.org/userguide/#bpmnSubProcessGraphicalNotation) with a double outline.

![bpmn.transaction.subprocess](https://www.activiti.org/userguide/images/bpmn.transaction.subprocess.png)

##### XML representation

A transaction subprocess is represented using xml using the `transaction` tag:

```
1
2
3<transaction id="myTransaction" >
	...
</transaction>
```

##### Example

The following is an example of a transaction subprocess:

![bpmn.transaction.subprocess.example.2](https://www.activiti.org/userguide/images/bpmn.transaction.subprocess.example.2.png)

#### 8.6.4. Call activity (subprocess)

##### Description

BPMN 2.0 makes a distinction between a regular *subprocess*, often also called *embedded subprocess*, and the call activity, which looks very similar. From a conceptual point of view, both will call a subprocess when process execution arrives at the activity.

The difference is that the call activity references a process that is external to the process definition, whereas the *subprocess* is embedded within the original process definition. The main use case for the call activity is to have a reusable process definition that can be called from multiple other process definitions.

When process execution arrives in the *call activity*, a new execution is created that is a sub-execution of the execution that arrives in the call activity. This sub-execution is then used to execute the subprocess, potentially creating parallel child execution as within a regular process. The super-execution waits until the subprocess is completely ended, and continues the original process afterwards.

##### Graphical Notation

A call activity is visualized the same as a [subprocess](https://www.activiti.org/userguide/#bpmnSubProcessGraphicalNotation), however with a thick border (collapsed and expanded). Depending on the modeling tool, a call activity can also be expanded, but the default visualization is the collapsed subprocess representation.

![bpmn.collapsed.call.activity](https://www.activiti.org/userguide/images/bpmn.collapsed.call.activity.png)

##### XML representation

A call activity is a regular activity, that requires a *calledElement* that references a process definition by its **key**. In practice, this means that the **id of the process** is used in the *calledElement*.

```
1<callActivity id="callCheckCreditProcess" name="Check credit" calledElement="checkCreditProcess" />
```

Note that the process definition of the subprocess is **resolved at runtime**. This means that the subprocess can be deployed independently from the calling process, if needed.

##### Passing variables

You can pass process variables to the sub process and vice versa. The data is copied into the subprocess when it is started and copied back into the main process when it ends.

```
1
2
3
4
5
6<callActivity id="callSubProcess" calledElement="checkCreditProcess" >
  <extensionElements>
	  <activiti:in source="someVariableInMainProcess" target="nameOfVariableInSubProcess" />
	  <activiti:out source="someVariableInSubProcess" target="nameOfVariableInMainProcess" />
  </extensionElements>
</callActivity>
```

We use an Activiti Extension as a shortcut for the BPMN standard elements called *dataInputAssociation* and *dataOutputAssociation*, which only work if you declare process variables in the BPMN 2.0 standard way.

It is possible to use expressions here as well:

```
1
2
3
4
5
6<callActivity id="callSubProcess" calledElement="checkCreditProcess" >
	<extensionElements>
	  <activiti:in sourceExpression="${x+5}" target="y" />
	  <activiti:out source="${y+5}" target="z" />
	</extensionElements>
</callActivity>
```

So in the end z = y+5 = x+5+5

The callActivity element also supports setting the business key on the started subprocess instance using a custom activiti attribute extension. The *businessKey* attribute can be used to set a custom business key value on the subprocess instance.

```
<callActivity id="callSubProcess" calledElement="checkCreditProcess" activiti:businessKey="${myVariable}">
...
</callActivity>
```

Defining the *inheritBusinessKey* attribute with a value of true will set the business key value on the subprocess to the value of the business key as defined in the calling process.

```
<callActivity id="callSubProcess" calledElement="checkCreditProcess" activiti:inheritBusinessKey="true">
...
</callActivity>
```

##### Example

The following process diagram shows a simple handling of an order. Since the checking of the customer’s credit could be common to many other processes, the *check credit step* is modeled here as a call activity.

![bpmn.call.activity.super.process](https://www.activiti.org/userguide/images/bpmn.call.activity.super.process.png)

The process looks as follows:

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13<startEvent id="theStart" />
<sequenceFlow id="flow1" sourceRef="theStart" targetRef="receiveOrder" />

<manualTask id="receiveOrder" name="Receive Order" />
<sequenceFlow id="flow2" sourceRef="receiveOrder" targetRef="callCheckCreditProcess" />

<callActivity id="callCheckCreditProcess" name="Check credit" calledElement="checkCreditProcess" />
<sequenceFlow id="flow3" sourceRef="callCheckCreditProcess" targetRef="prepareAndShipTask" />

<userTask id="prepareAndShipTask" name="Prepare and Ship" />
<sequenceFlow id="flow4" sourceRef="prepareAndShipTask" targetRef="end" />

<endEvent id="end" />
```

The subprocess looks as follows:

![bpmn.call.activity.sub.process](https://www.activiti.org/userguide/images/bpmn.call.activity.sub.process.png)

There is nothing special to the process definition of the subprocess. It could as well be used without being called from another process.

### 8.7. Transactions and Concurrency

#### 8.7.1. Asynchronous Continuations

Activiti executes processes in a transactional way which can be configured to suite your needs. Lets start by looking at how Activiti scopes transactions normally. If you trigger Activiti (i.e. start a process, complete a task, signal an execution), Activiti is going to advance in the process, until it reaches wait states on each active path of execution. More concretely speaking it performs a depth-first search through the process graph and returns if it has reached wait states on every branch of execution. A wait state is a task which is performed "later" which means that Activiti persists the current execution and waits to be triggered again. The trigger can either come from an external source for example if we have a user task or a receive message task, or from Activiti itself, if we have a timer event. This is illustrated in the following picture:

![activiti.async.example.no.async](https://www.activiti.org/userguide/images/activiti.async.example.no.async.PNG)

We see a segment of a BPMN processes with a usertask, a service task and a timer event. Completing the usertask and validating the address is part of the same unit of work, so it should succeed or fail atomically. That means that if the service task throws an exception we want to rollback the current transaction, such that the execution tracks back to the user task and the user task is still present in the database. This is also the default behavior of Activiti. In (1) an application or client thread completes the task. In that same thread Activiti is now executing the service and advances until it reaches a wait state, in this case the timer event (2). Then it returns the control to the caller (3) potentially committing the transaction (if it was started by Activiti).

In some cases this is not what we want. Sometimes we need custom control over transaction boundaries in a process, in order to be able to scope logical units of work. This is where asynchronous continuations come into play. Consider the following process (fragment):

![activiti.async.example.async](https://www.activiti.org/userguide/images/activiti.async.example.async.PNG)

This time we are completing the user task, generating an invoice and then send that invoice to the customer. This time the generation of the invoice is not part of the same unit of work so we do not want to rollback the completion of the usertask if generating an invoice fails. So what we want Activiti to do is complete the user task (1), commit the transaction and return the control to the calling application. Then we want to generate the invoice asynchronously, in a background thread. This background thread is the Activiti job executor (actually a thread pool) which periodically polls the database for jobs. So behind the scenes, when we reach the "generate invoice" task, we are creating a job "message" for Activiti to continue the process later and persisting it into the database. This job is then picked up by the job executor and executed. We are also giving the local job executor a little hint that there is a new job, to improve performance.

In order to use this feature, we can use the *activiti:async="true"* extension. So for example, the service task would look like this:

```
1<serviceTask id="service1" name="Generate Invoice" activiti:class="my.custom.Delegate" activiti:async="true" />
```

*activiti:async* can be specified on the following BPMN task types: task, serviceTask, scriptTask, businessRuleTask, sendTask, receiveTask, userTask, subProcess, callActivity

On a userTask, receiveTask or other wait states, the async continuation allows us to execute the start execution listeners in a separate thread/transaction.

#### 8.7.2. Fail Retry

Activiti, in its default configuration, reruns a job 3 times in case of any exception in execution of a job. This holds also for asynchronous task jobs. In some cases more flexibility is required. There are two parameters to be configured:

- Number of retries
- Delay between retries These parameters can be configured by `activiti:failedJobRetryTimeCycle` element. Here is a sample usage:

```
1
2
3
4
5<serviceTask id="failingServiceTask" activiti:async="true" activiti:class="org.activiti.engine.test.jobexecutor.RetryFailingDelegate">
	<extensionElements>
		<activiti:failedJobRetryTimeCycle>R5/PT7M</activiti:failedJobRetryTimeCycle>
	</extensionElements>
</serviceTask>
```

Time cycle expression follows ISO 8601 standard, just like timer event expressions. The above example, makes the job executor to retry the job 5 times and wait 7 minutes between before each retry.

#### 8.7.3. Exclusive Jobs

Since Activiti 5.9, the JobExecutor makes sure that jobs from a single process instance are never executed concurrently. Why is this?

##### Why exclusive Jobs?

Consider the following process definition:

![bpmn.why.exclusive.jobs](https://www.activiti.org/userguide/images/bpmn.why.exclusive.jobs.png)

We have a parallel gateway followed by three service tasks which all perform an asynchronous continuation. As a result of this, three jobs are added to the database. Once such a job is present in the database it can be processes by the JobExecutor. The JobExecutor acquires the jobs and delegates them to a thread pool of worker threads which actually process the jobs. This means that using an asynchronous continuation, you can distribute the work to this thread pool (and in a clustered scenario even across multiple thread pools in the cluster). This is usually a good thing. However it also bears an inherent problem: consistency. Consider the parallel join after the service tasks. When execution of a service tasks is completed, we arrive at the parallel join and need to decide whether to wait for the other executions or whether we can move forward. That means, for each branch arriving at the parallel join, we need to take a decision whether we can continue or whether we need to wait for one or more other executions on the other branches.

Why is this a problem? Since the service tasks are configured using an asynchronous continuation, it is possible that the corresponding jobs are all acquired at the same time and delegated to different worker threads by the JobExecutor. The consequence is that the transactions in which the services are executed and in which the 3 individual executions arrive at the parallel join can overlap. And if they do so, each individual transaction will not "see", that another transaction is arriving at the same parallel join concurrently and thus assume that it has to wait for the others. However, if each transaction assumes that it has to wait for the other ones, none will continue the process after the parallel join and the process instance will remain in that state forever.

How does Activiti address this problem? Activiti performs optimistic locking. Whenever we take a decision based on data that might not be current (because another transaction might modify it before we commit, we make sure to increment the version of the same database row in both transactions). This way, whichever transaction commits first wins and the other ones fail with an optimistic locking exception. This solves the problem in the case of the process discussed above: if multiple executions arrive at the parallel join concurrently, they all assume that they have to wait, increment the version of their parent execution (the process instance) and then try to commit. Whichever execution is first will be able to commit and the other ones will fail with an optimistic locking exception. Since the executions are triggered by a job, Activiti will retry to perform the same job after waiting for a certain amount of time and hopefully this time pass the synchronizing gateway.

Is this a good solution? As we have seen, optimistic locking allows Activiti to prevent inconsistencies. It makes sure that we do not "keep stuck at the joining gateway", meaning: either all executions have passed the gateway or, there are jobs in the database making sure that we retry passing it. However, while this is a perfectly fine solution from the point of view of persistence and consistency, this might not always be desirable behavior at an higher level:

- Activiti will retry the same job for a fixed maximum number of times only (*3* in the default configuration). After that, the job will still be present in the database but not be retried actively anymore. That means that an operator would need to trigger the job manually.
- If a job has non-transactional side effects, those will not be rolled back by the failing transaction. For instance, if the "book concert tickets" service does not share the same transaction as Activiti, we might book multiple tickets if we retry the job.

##### What are exclusive jobs?

An exclusive job cannot be performed at the same time as another exclusive job from the same process instance. Consider the process shown above: if we declare the service tasks to be exclusive, the JobExecutor will make sure that the corresponding jobs are not executed concurrently. Instead, it will make sure that whenever it acquires an exclusive job from a certain process instance, it acquires all other exclusive jobs from the same process instance and delegates them to the same worker thread. This ensures sequential execution execution of the jobs.

How can I enable this feature? Since Activiti 5.9, exclusive jobs are the default configuration. All asynchronous continuations and timer events are thus exclusive by default. In addition, if you want a job to be non-exclusive, you can configure it as such using `activiti:exclusive="false"`. For example, the following servicetask would be asynchronous but non-exclusive.

```
1<serviceTask id="service" activiti:expression="${myService.performBooking(hotel, dates)}" activiti:async="true" activiti:exclusive="false" />
```

Is this a good solution? We had some people asking whether this was a good solution. Their concern was that this would to prevent you from "doing things" in parallel and would thus be a performance problem. Again, two things have to be taken into consideration:

- It can be turned off if you are an expert and know what you are doing (and have understood the section named "Why exclusive Jobs?"). Other than that, it is more intuitive for most users if things like asynchronous continuations and timers just work.
- It is actually not a performance issue. Performance is an issue under heavy load. Heavy load means that all worker threads of the job executor are busy all the time. With exclusive jobs, Activiti will simply distribute the load differently. Exclusive jobs means that jobs from a single process instance are performed by the same thread sequentially. But consider: you have more than one single process instance. And jobs from other process instances are delegated to other threads and executed concurrently. This means that with exclusive jobs Activiti will not execute jobs from the same process instance concurrently, but it will still execute multiple instances concurrently. From an overall throughput perspective this is desirable in most scenarios as it usually leads to individual instances being done more quickly. Furthermore, data that is required for executing subsequent jobs of the same process instance will already be in the cache of the executing cluster node. If the jobs do not have this node affinity, that data might need to be fetched from the database again.

### 8.8. Process Initiation Authorization

By default everyone is allowed to start a new process instance of deployed process definitions. The process initiation authorization functionality allows to define users and groups so that web clients can optionally restrict users to start a new process instance. NOTE that the authorization definition is NOT validated by the Activiti Engine in any way. This functionality is only meant for developers to ease the implementation of authorization rules in a web client. The syntax is similar to the syntax of user assignment for a user task. A user or group can be assigned as potential initiator of a process using <activiti:potentialStarter> tag. Here is an example:

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11<process id="potentialStarter">
  <extensionElements>
    <activiti:potentialStarter>
       <resourceAssignmentExpression>
         <formalExpression>group2, group(group3), user(user3)</formalExpression>
       </resourceAssignmentExpression>
    </activiti:potentialStarter>
  </extensionElements>

  <startEvent id="theStart"/>
  ...
```

In the above xml excerpt user(user3) refers directly to user user3 and group(group3) to group group3. No indicator will default to a group type. It is also possible to use attributes of the <process> tag, namely <activiti:candidateStarterUsers> and <activiti:candidateStarterGroups>. Here is an example:

```
1
2
3<process id="potentialStarter" activiti:candidateStarterUsers="user1, user2"
                               activiti:candidateStarterGroups="group1">
      ...
```

It is possible to use both attributes simultaneously.

After the process initiation authorizations are defined, a developer can retrieve the authorization definition using the following methods. This code retrieves the list of process definitions which can be initiated by the given user:

```
1processDefinitions = repositoryService.createProcessDefinitionQuery().startableByUser("userxxx").list();
```

It’s also possible to retrieve all identity links that are defined as potential starter for a specific process definition

```
1identityLinks = repositoryService.getIdentityLinksForProcessDefinition("processDefinitionId");
```

The following example shows how to get list of users who can initiate the given process:

```
1List<User> authorizedUsers =  identityService().createUserQuery().potentialStarter("processDefinitionId").list();
```

Exactly the same way, the list of groups that is configured as a potential starter to a given process definition can be retrieved:

```
1List<Group> authorizedGroups =  identityService().createGroupQuery().potentialStarter("processDefinitionId").list();
```

### 8.9. Data objects

[[EXPERIMENTAL\]](https://www.activiti.org/userguide/#experimental)

BPMN provides the possibility to define data objects as part of a process or sub process element. According to the BPMN specification it’s possible to include complex XML structures that might be imported from XSD definitions. As a first start to support data objects in Activiti the following XSD types are supported:

```
1
2
3
4
5
6<dataObject id="dObj1" name="StringTest" itemSubjectRef="xsd:string"/>
<dataObject id="dObj2" name="BooleanTest" itemSubjectRef="xsd:boolean"/>
<dataObject id="dObj3" name="DateTest" itemSubjectRef="xsd:datetime"/>
<dataObject id="dObj4" name="DoubleTest" itemSubjectRef="xsd:double"/>
<dataObject id="dObj5" name="IntegerTest" itemSubjectRef="xsd:int"/>
<dataObject id="dObj6" name="LongTest" itemSubjectRef="xsd:long"/>
```

The data object definitions will be automatically converted to process variables using the *name* attribute value as the name for the new variable. In addition to the definition of the data object Activiti also provides an extension element to assign a default value to the variable. The following BPMN snippet provides an example:

```
1
2
3
4
5
6
7<process id="dataObjectScope" name="Data Object Scope" isExecutable="true">
  <dataObject id="dObj123" name="StringTest123" itemSubjectRef="xsd:string">
    <extensionElements>
      <activiti:value>Testing123</activiti:value>
    </extensionElements>
  </dataObject>
  ...
```

## 9. Forms

Activiti provides a convenient and flexible way to add forms for the manual steps of your business processes. We support two strategies to work with forms: Build-in form rendering with form properties and external form rendering.

### 9.1. Form properties

All information relevant to a business process is either included in the process variables themselves or referenced through the process variables. Activiti supports complex Java objects to be stored as process variables like `Serializable` objects, JPA entities or whole XML documents as `String`s.

Starting a process and completing user tasks is where people are involved into a process. Communicating with people requires forms to be rendered in some UI technology. In order to facilitate multiple UI technologies easy, the process definition can include the logic of transforming of the complex Java typed objects in the process variables to a `Map<String,String>` of **properties**.

Any UI technology can then build a form on top of those properties, using the Activiti API methods that expose the property information. The properties can provide a dedicated (and more limited) view on the process variables. The properties needed to display a form are available in the **FormData** return values of for example

```
1StartFormData FormService.getStartFormData(String processDefinitionId)
```

or

```
1TaskFormdata FormService.getTaskFormData(String taskId)
```

By default, the build-in form engines, *sees* the properties as well as the process variables. So there is no need to declare task form properties if they match 1-1 with the process variables. For example, with the following declaration:

```
1<startEvent id="start" />
```

All process variables are available when execution arrives in the startEvent, but

```
1formService.getStartFormData(String processDefinitionId).getFormProperties()
```

will be empty since no specific mapping was defined.

In the above case, all the submitted properties will be stored as process variables. This means that by simply adding a new input field in the form, a new variable can be stored.

Properties are derived from process variables, but they don’t have to be stored as process variables. For example, a process variable could be a JPA entity of class Address. And a form property `StreetName` used by the UI technology could be linked with an expression `#{address.street}`

Analogue, the properties that a user is supposed to submit in a form can be stored as a process variable or as a nested property in one of the process variables with a UEL value expression like e.g. `#{address.street}` .

Analogue the default behavior of properties that are submitted is that they will be stored as process variables unless a `formProperty`declaration specifies otherwise.

Also type conversions can be applied as part of the processing between form properties and process variables.

For example:

```
1
2
3
4
5
6
7
8<userTask id="task">
  <extensionElements>
    <activiti:formProperty id="room" />
    <activiti:formProperty id="duration" type="long"/>
    <activiti:formProperty id="speaker" variable="SpeakerName" writable="false" />
    <activiti:formProperty id="street" expression="#{address.street}" required="true" />
  </extensionElements>
</userTask>
```

- Form property `room` will be mapped to process variable `room` as a String
- Form property `duration` will be mapped to process variable `duration` as a java.lang.Long
- Form property `speaker` will be mapped to process variable `SpeakerName`. It will only be available in the TaskFormData object. If property speaker is submitted, an ActivitiException will be thrown. Analogue, with attribute `readable="false"`, a property can be excluded from the FormData, but still be processed in the submit.
- Form property `street` will be mapped to Java bean property `street` in process variable `address` as a String. And required="true" will throw an exception during the submit if the property is not provided.

It’s also possible to provide type metadata as part of the FormData that is returned from methods `StartFormData FormService.getStartFormData(String processDefinitionId)` and `TaskFormdata FormService.getTaskFormData(String taskId)`

We support the following form property types:

- `string` (org.activiti.engine.impl.form.StringFormType
- `long` (org.activiti.engine.impl.form.LongFormType)
- `enum` (org.activiti.engine.impl.form.EnumFormType)
- `date` (org.activiti.engine.impl.form.DateFormType)
- `boolean` (org.activiti.engine.impl.form.BooleanFormType)

For each form property declared, the following `FormProperty` information will be made available through `List<FormProperty> formService.getStartFormData(String processDefinitionId).getFormProperties()` and `List<FormProperty> formService.getTaskFormData(String taskId).getFormProperties()`

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16
17
18public interface FormProperty {
  /** the key used to submit the property in {@link FormService#submitStartFormData(String, java.util.Map)}
   * or {@link FormService#submitTaskFormData(String, java.util.Map)} */
  String getId();
  /** the display label */
  String getName();
  /** one of the types defined in this interface like e.g. {@link #TYPE_STRING} */
  FormType getType();
  /** optional value that should be used to display in this property */
  String getValue();
  /** is this property read to be displayed in the form and made accessible with the methods
   * {@link FormService#getStartFormData(String)} and {@link FormService#getTaskFormData(String)}. */
  boolean isReadable();
  /** is this property expected when a user submits the form? */
  boolean isWritable();
  /** is this property a required input field */
  boolean isRequired();
}
```

For example:

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16
17
18
19
20<startEvent id="start">
  <extensionElements>
    <activiti:formProperty id="speaker"
      name="Speaker"
      variable="SpeakerName"
      type="string" />

    <activiti:formProperty id="start"
      type="date"
      datePattern="dd-MMM-yyyy" />

    <activiti:formProperty id="direction" type="enum">
      <activiti:value id="left" name="Go Left" />
      <activiti:value id="right" name="Go Right" />
      <activiti:value id="up" name="Go Up" />
      <activiti:value id="down" name="Go Down" />
    </activiti:formProperty>

  </extensionElements>
</startEvent>
```

All that information is accessible through the API. The type names can be obtained with `formProperty.getType().getName()`. And even the date pattern is available with `formProperty.getType().getInformation("datePattern")` and the enumeration values are accessible with `formProperty.getType().getInformation("values")`

Activiti explorer supports the form properties and will render the form accordingly to the form definition. The following XML snippet

```
1
2
3
4
5
6
7<startEvent>
  <extensionElements>
    <activiti:formProperty id="numberOfDays" name="Number of days" value="${numberOfDays}" type="long" required="true"/>
    <activiti:formProperty id="startDate" name="First day of holiday (dd-MM-yyy)" value="${startDate}" datePattern="dd-MM-yyyy hh:mm" type="date" required="true" />
    <activiti:formProperty id="vacationMotivation" name="Motivation" value="${vacationMotivation}" type="string" />
  </extensionElements>
</userTask>
```

will render to a process start form when used in Activiti Explorer

![forms.explorer](https://www.activiti.org/userguide/images/forms.explorer.png)

### 9.2. External form rendering

The API also allows for you to perform your own task form rendering outside of the Activiti Engine. These steps explain the hooks that you can use to render your task forms yourself.

Essentially, all the data that’s needed to render a form is assembled in one of these two service methods: `StartFormData FormService.getStartFormData(String processDefinitionId)` and `TaskFormdata FormService.getTaskFormData(String taskId)`.

Submitting form properties can be done with `ProcessInstance FormService.submitStartFormData(String processDefinitionId, Map<String,String> properties)` and `void FormService.submitTaskFormData(String taskId, Map<String,String> properties)`

To learn about how form properties map to process variables, see [Form properties](https://www.activiti.org/userguide/#formProperties)

You can place any form template resource inside the business archives that you deploy (in case you want to store them versioned with the process). It will be available as a resource in the deployment, which you can retrieve using: `String ProcessDefinition.getDeploymentId()` and `InputStream RepositoryService.getResourceAsStream(String deploymentId, String resourceName);` This could be your template definition file, which you can use to render/show the form in your own application.

You can use this capability of accessing the deployment resources beyond task forms for any other purposes as well.

The attribute `<userTask activiti:formKey="…"` is exposed by the API through `String FormService.getStartFormData(String processDefinitionId).getFormKey()` and `String FormService.getTaskFormData(String taskId).getFormKey()`. You could use this to store the full name of the template within your deployment (e.g. `org/activiti/example/form/my-custom-form.xml`), but this is not required at all. For instance, you could also store a generic key in the form attribute and apply an algorithm or transformation to get to the actual template that needs to be used. This might be handy when you want to render different forms for different UI technologies like e.g. one form for usage in a web app of normal screen size, one form for mobile phone’s small screens and maybe even a template for an IM form or an email form.

## 10. JPA

You can use JPA-Entities as process variables, allowing you to:

- Updating existing JPA-entities based on process variables that can be filled in on a form in a userTask or generated in a serviceTask.
- Reusing existing domain model without having to write explicit services to fetch the entities and update the values
- Make decisions (gateways) based on properties of existing entities.
- …

### 10.1. Requirements

Only entities that comply with the following are supported:

- Entities should be configured using JPA-annotations, we support both field and property-access. Mapped super classes can also be used.
- Entity should have a primary key annotated with `@Id`, compound primary keys are not supported (`@EmbeddedId` and `@IdClass`). The Id field/property can be of any type supported in the JPA-spec: Primitive types and their wrappers (excluding boolean), `String`, `BigInteger`, `BigDecimal`, `java.util.Date` and `java.sql.Date`.

### 10.2. Configuration

To be able to use JPA-entities, the engine must have a reference to an `EntityManagerFactory`. This can be done by configuring a reference or by supplying a persistence-unit name. JPA-entities used as variables will be detected automatically and will be handled accordingly.

The example configuration below uses the jpaPersistenceUnitName:

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16
17<bean id="processEngineConfiguration"
  class="org.activiti.engine.impl.cfg.StandaloneInMemProcessEngineConfiguration">

<!-- Database configurations -->
<property name="databaseSchemaUpdate" value="true" />
<property name="jdbcUrl" value="jdbc:h2:mem:JpaVariableTest;DB_CLOSE_DELAY=1000" />

<property name="jpaPersistenceUnitName" value="activiti-jpa-pu" />
<property name="jpaHandleTransaction" value="true" />
<property name="jpaCloseEntityManager" value="true" />

<!-- job executor configurations -->
<property name="jobExecutorActivate" value="false" />

<!-- mail server configurations -->
<property name="mailServerPort" value="5025" />
</bean>
```

The next example configuration below provides a `EntityManagerFactory` which we define ourselves (in this case, an open-jpa entity manager). Note that the snippet only contains the beans that are relevant for the example, the others are omitted. Full working example with open-jpa entity manager can be found in the activiti-spring-examples (`/activiti-spring/src/test/java/org/activiti/spring/test/jpa/JPASpringTest.java`)

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16
17
18<bean id="entityManagerFactory" class="org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean">
  <property name="persistenceUnitManager" ref="pum"/>
  <property name="jpaVendorAdapter">
    <bean class="org.springframework.orm.jpa.vendor.OpenJpaVendorAdapter">
      <property name="databasePlatform" value="org.apache.openjpa.jdbc.sql.H2Dictionary" />
    </bean>
  </property>
</bean>

<bean id="processEngineConfiguration" class="org.activiti.spring.SpringProcessEngineConfiguration">
  <property name="dataSource" ref="dataSource" />
  <property name="transactionManager" ref="transactionManager" />
  <property name="databaseSchemaUpdate" value="true" />
  <property name="jpaEntityManagerFactory" ref="entityManagerFactory" />
  <property name="jpaHandleTransaction" value="true" />
  <property name="jpaCloseEntityManager" value="true" />
  <property name="jobExecutorActivate" value="false" />
</bean>
```

The same configurations can also be done when building an engine programmatically, example:

```
1
2
3
4ProcessEngine processEngine = ProcessEngineConfiguration
.createProcessEngineConfigurationFromResourceDefault()
.setJpaPersistenceUnitName("activiti-pu")
.buildProcessEngine();
```

Configuration properties:

- `jpaPersistenceUnitName`: The name of the persistence-unit to use. (Make sure the persistence-unit is available on the classpath. According to the spec, the default location is `/META-INF/persistence.xml`). Use either `jpaEntityManagerFactory` or `jpaPersistenceUnitName`.
- `jpaEntityManagerFactory`: An reference to a bean implementing `javax.persistence.EntityManagerFactory` that will be used to load the Entities and flushing the updates. Use either *jpaEntityManagerFactory* or *jpaPersistenceUnitName*.
- `jpaHandleTransaction`: Flag indicating that the engine should begin and commit/rollback the transaction on the used *EntityManager* instances. Set to false when *Java Transaction API (JTA)* is used.
- `jpaCloseEntityManager`: Flag indicating that the engine should close the `EntityManager` instance that was obtained from the `EntityManagerFactory`. Set to false when the *EntityManager* is container-managed (e.g. when using an Extended Persistence Context which isn’t scoped to a single transaction').

### 10.3. Usage

#### 10.3.1. Simple Example

Examples for using JPA variables can be found in JPAVariableTest in the Activiti source code. We’ll explain `JPAVariableTest.testUpdateJPAEntityValues` step by step.

First of all, we create an *EntityManagerFactory* for our persistence-unit, which is based on `META-INF/persistence.xml`. This contains classes which should be included in the persistence unit and some vendor-specific configuration.

We are using a simple entity in the test, having an id and `String` value property, which is also persisted. Before running the test, we create an entity and save this.

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29@Entity(name = "JPA_ENTITY_FIELD")
public class FieldAccessJPAEntity {

  @Id
  @Column(name = "ID_")
  private Long id;

  private String value;

  public FieldAccessJPAEntity() {
    // Empty constructor needed for JPA
  }

  public Long getId() {
    return id;
  }

  public void setId(Long id) {
    this.id = id;
  }

  public String getValue() {
    return value;
  }

  public void setValue(String value) {
    this.value = value;
  }
}
```

We start a new process instance, adding the entity as a variable. As with other variables, they are stored in the persistent storage of the engine. When the variable is requested the next time, it will be loaded from the `EntityManager` based on the class and Id stored.

```
1
2
3
4Map<String, Object> variables = new HashMap<String, Object>();
variables.put("entityToUpdate", entityToUpdate);

ProcessInstance processInstance = runtimeService.startProcessInstanceByKey("UpdateJPAValuesProcess", variables);
```

The first node in our process definition contains a `serviceTask` that will invoke the method `setValue` on `entityToUpdate`, which resolves to the JPA variable we set earlier when starting the process instance and will be loaded from the `EntityManager` associated with the current engine’s context'.

```
1
2<serviceTask id='theTask' name='updateJPAEntityTask'
  activiti:expression="${entityToUpdate.setValue('updatedValue')}" />
```

When the service-task is finished, the process instance waits in a userTask defined in the process definition, which allows us to inspect the process instance. At this point, the `EntityManager` has been flushed and the changes to the entity have been pushed to the database. When we get the value of the variable `entityToUpdate`, it’s loaded again and we get the entity with its `value` property set to `updatedValue`.

```
1
2
3
4// Servicetask in process 'UpdateJPAValuesProcess' should have set value on entityToUpdate.
Object updatedEntity = runtimeService.getVariable(processInstance.getId(), "entityToUpdate");
assertTrue(updatedEntity instanceof FieldAccessJPAEntity);
assertEquals("updatedValue", ((FieldAccessJPAEntity)updatedEntity).getValue());
```

#### 10.3.2. Query JPA process variables

You can query for `ProcessInstance`s and `Execution`s that have a certain JPA-entity as variable value. **Note that onlyvariableValueEquals(name, entity) is supported for JPA-Entities on ProcessInstanceQuery and ExecutionQuery**. Methods `variableValueNotEquals`, `variableValueGreaterThan`, `variableValueGreaterThanOrEqual`, `variableValueLessThan` and `variableValueLessThanOrEqual` are unsupported and will throw an `ActivitiException` when a JPA-Entity is passed as value.

```
1
2 ProcessInstance result = runtimeService.createProcessInstanceQuery()
    .variableValueEquals("entityToQuery", entityToQuery).singleResult();
```

#### 10.3.3. Advanced example using Spring beans and JPA

A more advanced example, `JPASpringTest`, can be found in `activiti-spring-examples`. It describes the following simple use case:

- An existing Spring-bean which uses JPA entities already exists which allows for Loan Requests to be stored.
- Using Activiti, we can use the existing entities, obtained through the existing bean, and use them as variable in our process. Process is defined in the following steps:
  - Service task that creates a new LoanRequest, using the existing `LoanRequestBean` using variables received when starting the process (e.g. could come from a start form). The created entity is stored as a variable, using `activiti:resultVariable` which stores the expression result as a variable.
  - UserTask that allows a manager to review the request and approve/disapprove, which is stored as a boolean variable `approvedByManager`
  - ServiceTask that updates the loan request entity so the entity is in sync with the process.
  - Depending on the value of the entity property `approved`, an exclusive gateway is used to make a decision about what path to take next: When the request is approved, process ends, otherwise, an extra task will become available (Send rejection letter), so the customer can be notified manually by a rejection letter.

Please note that the process doesn’t contain any forms, since it is only used in a unit test.

![jpa.spring.example.process](https://www.activiti.org/userguide/images/jpa.spring.example.process.png)

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34
35
36
37
38
39<?xml version="1.0" encoding="UTF-8"?>
<definitions id="taskAssigneeExample"
  xmlns="http://www.omg.org/spec/BPMN/20100524/MODEL"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns:activiti="http://activiti.org/bpmn"
  targetNamespace="org.activiti.examples">

  <process id="LoanRequestProcess" name="Process creating and handling loan request">
    <startEvent id='theStart' />
    <sequenceFlow id='flow1' sourceRef='theStart' targetRef='createLoanRequest' />

    <serviceTask id='createLoanRequest' name='Create loan request'
      activiti:expression="${loanRequestBean.newLoanRequest(customerName, amount)}"
      activiti:resultVariable="loanRequest"/>
    <sequenceFlow id='flow2' sourceRef='createLoanRequest' targetRef='approveTask' />

    <userTask id="approveTask" name="Approve request" />
    <sequenceFlow id='flow3' sourceRef='approveTask' targetRef='approveOrDissaprove' />

    <serviceTask id='approveOrDissaprove' name='Store decision'
      activiti:expression="${loanRequest.setApproved(approvedByManager)}" />
    <sequenceFlow id='flow4' sourceRef='approveOrDissaprove' targetRef='exclusiveGw' />

    <exclusiveGateway id="exclusiveGw" name="Exclusive Gateway approval" />
    <sequenceFlow id="endFlow1" sourceRef="exclusiveGw" targetRef="theEnd">
      <conditionExpression xsi:type="tFormalExpression">${loanRequest.approved}</conditionExpression>
    </sequenceFlow>
    <sequenceFlow id="endFlow2" sourceRef="exclusiveGw" targetRef="sendRejectionLetter">
      <conditionExpression xsi:type="tFormalExpression">${!loanRequest.approved}</conditionExpression>
    </sequenceFlow>

    <userTask id="sendRejectionLetter" name="Send rejection letter" />
    <sequenceFlow id='flow5' sourceRef='sendRejectionLetter' targetRef='theOtherEnd' />

    <endEvent id='theEnd' />
    <endEvent id='theOtherEnd' />
  </process>

</definitions>
```

Although the example above is quite simple, it shows the power of using JPA combined with Spring and parametrized method-expressions. The process requires no custom java-code at all (except for the Spring-bean off course) and speeds up development drastically.

## 11. History

History is the component that captures what happened during process execution and stores it permanently. In contrast to the runtime data, the history data will remain present in the DB also after process instances have completed.

There are 5 history entities:

- `HistoricProcessInstance`s containing information about current and past process instances.
- `HistoricVariableInstance`s containing the latest value of a process variable or task variable.
- `HistoricActivityInstance`s containing information about a single execution of an activity (node in the process).
- `HistoricTaskInstance`s containing information about current and past (completed and deleted) task instances.
- `HistoricDetail`s containing various kinds of information related to either a historic process instances, an activity instance or a task instance.

Since the DB contains historic entities for past as well as ongoing instances, you might want to consider querying these tables in order to minimize access to the runtime process instance data and that way keeping the runtime execution performant.

Later on, this information will be exposed in Activiti Explorer. Also, it will be the information from which the reports will be generated.

### 11.1. Querying history

In the API, it’s possible to query all 5 of the History entities. The HistoryService exposes the methods `createHistoricProcessInstanceQuery()`, `createHistoricVariableInstanceQuery()`, `createHistoricActivityInstanceQuery()`, `createHistoricDetailQuery()` and `createHistoricTaskInstanceQuery()`.

Below are a couple of examples that show some of the possibilities of the query API for history. Full description of the possibilities can be found in the [javadocs](http://activiti.org/javadocs/index.html), in the `org.activiti.engine.history` package.

#### 11.1.1. HistoricProcessInstanceQuery

Get 10 `HistoricProcessInstances` that are finished and which took the most time to complete (the longest duration) of all finished processes with definition *XXX*.

```
1
2
3
4
5historyService.createHistoricProcessInstanceQuery()
  .finished()
  .processDefinitionId("XXX")
  .orderByProcessInstanceDuration().desc()
  .listPage(0, 10);
```

#### 11.1.2. HistoricVariableInstanceQuery

Get all `HistoricVariableInstances` from a finished process instance with id *xxx* ordered by variable name.

```
1
2
3
4historyService.createHistoricVariableInstanceQuery()
  .processInstanceId("XXX")
  .orderByVariableName.desc()
  .list();
```

#### 11.1.3. HistoricActivityInstanceQuery

Get the last `HistoricActivityInstance` of type *serviceTask* that has been finished in any process that uses the processDefinition with id XXX.

```
1
2
3
4
5
6historyService.createHistoricActivityInstanceQuery()
  .activityType("serviceTask")
  .processDefinitionId("XXX")
  .finished()
  .orderByHistoricActivityInstanceEndTime().desc()
  .listPage(0, 1);
```

#### 11.1.4. HistoricDetailQuery

The next example, gets all variable-updates that have been done in process with id 123. Only `HistoricVariableUpdate`s will be returned by this query. Note that it’s possible that a certain variable name has multiple `HistoricVariableUpdate` entries, for each time the variable was updated in the process. You can use `orderByTime` (the time the variable update was done) or `orderByVariableRevision` (revision of runtime variable at the time of updating) to find out in what order they occurred.

```
1
2
3
4
5historyService.createHistoricDetailQuery()
  .variableUpdates()
  .processInstanceId("123")
  .orderByVariableName().asc()
  .list()
```

This example gets all [form-properties](https://www.activiti.org/userguide/#formProperties) that were submitted in any task or when starting the process with id "123". Only `HistoricFormProperties`s will be returned by this query.

```
1
2
3
4
5historyService.createHistoricDetailQuery()
  .formProperties()
  .processInstanceId("123")
  .orderByVariableName().asc()
  .list()
```

The last example gets all variable updates that were performed on the task with id "123". This returns all `HistoricVariableUpdates` for variables that were set on the task (task local variables), and NOT on the process instance.

```
1
2
3
4
5historyService.createHistoricDetailQuery()
  .variableUpdates()
  .taskId("123")
  .orderByVariableName().asc()
  .list()
```

Task local variables can be set using the `TaskService` or on a `DelegateTask`, inside `TaskListener`:

```
1taskService.setVariableLocal("123", "myVariable", "Variable value");
1
2
3public void notify(DelegateTask delegateTask) {
  delegateTask.setVariableLocal("myVariable", "Variable value");
}
```

#### 11.1.5. HistoricTaskInstanceQuery

Get 10 `HistoricTaskInstance`s that are finished and which took the most time to complete (the longest duration) of all tasks.

```
1
2
3
4historyService.createHistoricTaskInstanceQuery()
  .finished()
  .orderByHistoricTaskInstanceDuration().desc()
  .listPage(0, 10);
```

Get `HistoricTaskInstance`s that are deleted with a delete reason that contains "invalid", which were last assigned to user *kermit*.

```
1
2
3
4
5historyService.createHistoricTaskInstanceQuery()
  .finished()
  .taskDeleteReasonLike("%invalid%")
  .taskAssignee("kermit")
  .listPage(0, 10);
```

### 11.2. History configuration

The history level can be configured programmatically, using the enum org.activiti.engine.impl.history.HistoryLevel (or *HISTORY* constants defined on `ProcessEngineConfiguration` for versions prior to 5.11):

```
1
2
3
4ProcessEngine processEngine = ProcessEngineConfiguration
  .createProcessEngineConfigurationFromResourceDefault()
  .setHistory(HistoryLevel.AUDIT.getKey())
  .buildProcessEngine();
```

The level can also be configured in activiti.cfg.xml or in a spring-context:

```
1
2
3
4<bean id="processEngineConfiguration" class="org.activiti.engine.impl.cfg.StandaloneInMemProcessEngineConfiguration">
  <property name="history" value="audit" />
  ...
</bean>
```

Following history levels can be configured:

- `none`: skips all history archiving. This is the most performant for runtime process execution, but no historical information will be available.
- `activity`: archives all process instances and activity instances. At the end of the process instance, the latest values of the top level process instance variables will be copied to historic variable instances. No details will be archived.
- `audit`: This is the default. It archives all process instances, activity instances, keeps variable values continuously in sync and all form properties that are submitted so that all user interaction through forms is traceable and can be audited.
- `full`: This is the highest level of history archiving and hence the slowest. This level stores all information as in the `audit` level plus all other possible details, mostly this are process variable updates.

**Prior to Activiti 5.11, the history level was stored in the database (table ACT_GE_PROPERTY, property with namehistoryLevel). Starting from 5.11, this value is not used anymore and is ignored/deleted from the database. The history can now be changed between 2 boots of the engine, without an exception being thrown in case the level changed from the previous engine-boot.**

### 11.3. History for audit purposes

When [configuring](https://www.activiti.org/userguide/#historyConfig) at least `audit` level for configuration. Then all properties submitted through methods`FormService.submitStartFormData(String processDefinitionId, Map<String, String> properties)` and `FormService.submitTaskFormData(String taskId, Map<String, String> properties)` are recorded.

Form properties can be retrieved with the query API like this:

```
1
2
3
4
5historyService
      .createHistoricDetailQuery()
      .formProperties()
      ...
      .list();
```

In that case only historic details of type `HistoricFormProperty` are returned.

If you’ve set the authenticated user before calling the submit methods with `IdentityService.setAuthenticatedUserId(String)`then that authenticated user who submitted the form will be accessible in the history as well with `HistoricProcessInstance.getStartUserId()` for start forms and `HistoricActivityInstance.getAssignee()` for task forms.

## 12. Eclipse Designer

Activiti comes with an Eclipse plugin, the Activiti Eclipse Designer, that can be used to graphically model, test and deploy BPMN 2.0 processes.

### 12.1. Installation

The following installation instructions are verified on [Eclipse Kepler and Indigo](http://www.eclipse.org/downloads/). Note that Eclipse Helios is **NOT** supported.

Go to **Help → Install New Software**. In the following panel, click on *Add* button and fill in the following fields:

- *Name:*Activiti BPMN 2.0 designer
- *Location:*http://activiti.org/designer/update/

![designer.add.update.site](https://www.activiti.org/userguide/images/designer.add.update.site.png)

Make sure the **"Contact all updates sites.."** checkbox is **checked**, because all the necessary plugins will then be downloaded by Eclipse.

### 12.2. Activiti Designer editor features

- Create Activiti projects and diagrams.

![designer.create.activiti.project](https://www.activiti.org/userguide/images/designer.create.activiti.project.png)

- The Activiti Designer creates a .bpmn file when creating a new Activiti diagram. When opened with the Activiti Diagram Editor view this will provide a graphical modeling canvas and palette. The same file can however be opened with an XML editor and it then shows the BPMN 2.0 XML elements of the process definition. So the Activiti Designer works with only one file for both the graphical diagram as well as the BPMN 2.0 XML. Note that in Activiti 5.9 the .bpmn extension is not yet supported as deployment artifact for a process definition. Therefore the "create deployment artifacts" feature of the Activiti Designer generates a BAR file with a .bpmn20.xml file that contains the content of the .bpmn file. You can also do a quick file rename yourself. Also note that you can open a .bpmn20.xml file with the Activiti Diagram Editor view as well.

![designer.bpmn.file](https://www.activiti.org/userguide/images/designer.bpmn.file.png)

- BPMN 2.0 XML files can be imported into the Activiti Designer and a diagram will be created. Just copy the BPMN 2.0 XML file to your project and open the file with the Activiti Diagram Editor view. The Activiti Designer uses the BPMN DI information of the file to create the diagram. If you have a BPMN 2.0 XML file without BPMN DI information, no diagram can be created.

![designer.open.importedfile](https://www.activiti.org/userguide/images/designer.open.importedfile.png)

- For deployment a BAR file and optionally a JAR file is created by the Activiti Designer by right-clicking on an Activiti project in the package explorer and choosing the *Create deployment artifacts* option at the bottom of the popup menu. For more information about the deployment functionality of the Designer look at the [deployment](https://www.activiti.org/userguide/#eclipseDesignerDeployment) section.

![designer.create.deployment](https://www.activiti.org/userguide/images/designer.create.deployment.png)

- Generate a unit test (right click on a BPMN 2.0 XML file in the package explorer and select *generate unit test*) A unit test is generated with an Activiti configuration that runs on an embedded H2 database. You can now run the unit test to test your process definition.

![designer.unittest.generate](https://www.activiti.org/userguide/images/designer.unittest.generate.png)

- The Activiti project is generated as a Maven project. To configure the dependencies you need to run *mvn eclipse:eclipse* and the Maven dependencies will be configured as expected. Note that for process design Maven dependencies are not needed. They are only needed to run unit tests.

![designer.project.maven](https://www.activiti.org/userguide/images/designer.project.maven.png)

### 12.3. Activiti Designer BPMN features

- Support for start none event, start error event, timer start event, end none event, end error event, sequence flow, parallel gateway, exclusive gateway, inclusive gateway, event gateway, embedded subprocess, event sub process, call activity, pool, lane, script task, user task, service task, mail task, manual task, business rule task, receive task, timer boundary event, error boundary event, signal boundary event, timer catching event, signal catching event, signal throwing event, none throwing event and four Alfresco specific elements (user, script, mail tasks and start event).

![designer.model.process](https://www.activiti.org/userguide/images/designer.model.process.png)

- You can quickly change the type of a task by hovering over the element and choosing the new task type.

![designer.model.quick.change](https://www.activiti.org/userguide/images/designer.model.quick.change.png)

- You can quickly add new elements hovering over an element and choosing a new element type.

![designer.model.quick.new](https://www.activiti.org/userguide/images/designer.model.quick.new.png)

- Java class, expression or delegate expression configuration is supported for the Java service task. In addition field extensions can be configured.

![designer.servicetask.property](https://www.activiti.org/userguide/images/designer.servicetask.property.png)

- Support for pools and lanes. Because Activiti reads different pools as different process definition, it makes the most sense to use only one pool. If you use multiple pools, be aware that drawing sequence flows between the pools will result in problems when deploying the process in the Activiti Engine. You can add as much lanes to a pool as you want.

![designer.model.poolandlanes](https://www.activiti.org/userguide/images/designer.model.poolandlanes.png)

- You can add labels to sequence flows by filling the name property. You can position the labels yourself as the position is saved as part of the BPMN 2.0 XML DI information.

![designer.model.labels](https://www.activiti.org/userguide/images/designer.model.labels.png)

- Support for event sub processes.

![designer.model.eventsubprocess](https://www.activiti.org/userguide/images/designer.model.eventsubprocess.png)

- Support for expanded embedded sub processes. You can also add an embedded sub process in another embedded sub process.

![designer.embeddedprocess.canvas](https://www.activiti.org/userguide/images/designer.embeddedprocess.canvas.png)

- Support for timer boundary events on tasks and embedded sub processes. Although, the timer boundary event makes the most sense when using it on a user task or an embedded sub process in the Activiti Designer.

![designer.timerboundary.canvas](https://www.activiti.org/userguide/images/designer.timerboundary.canvas.png)

- Support for additional Activiti extensions like the Mail task, the candidate configuration of User tasks and Script task configuration.

![designer.mailtask.property](https://www.activiti.org/userguide/images/designer.mailtask.property.png)

- Support for the Activiti execution and task listeners. You can also add field extensions for execution listeners.

![designer.listener.configuration](https://www.activiti.org/userguide/images/designer.listener.configuration.png)

- Support for conditions on sequence flows.

![designer.sequence.condition](https://www.activiti.org/userguide/images/designer.sequence.condition.png)

### 12.4. Activiti Designer deployment features

Deploying process definitions and task forms on the Activiti Engine is not hard. You need a BAR file containing the process definition BPMN 2.0 XML file and optionally task forms and an image of the process that can be viewed in the Activiti Explorer. In the Activiti Designer it’s made very easy to create a BAR file. When you’ve finished your process implementation just right-click on your Activiti project in the package explorer and choose for the **Create deployment artifacts** option at the bottom of the popup menu.

![designer.create.deployment](https://www.activiti.org/userguide/images/designer.create.deployment.png)

Then a deployment directory is created containing the BAR file and optionally a JAR file with the Java classes of your Activiti project.

![designer.deployment.dir](https://www.activiti.org/userguide/images/designer.deployment.dir.png)

This file can now be uploaded to the Activiti Engine using the deployments tab in Activiti Explorer, and you are ready to go.

When your project contains Java classes, the deployment is a bit more work. In that case the **Create deployment artifacts** step in the Activiti Designer will also generate a JAR file containing the compiled classes. This JAR file must be deployed to the activiti-XXX/WEB-INF/lib directory in your Activiti Tomcat installation directory. This makes the classes available on the classpath of the Activiti Engine.

### 12.5. Extending Activiti Designer

You can extend the default functionality offered by Activiti Designer. This section documents which extensions are available, how they can be used and provides some usage examples. Extending Activiti Designer is useful in cases where the default functionality doesn’t suit your needs, you require additional capabilities or have domain specific requirements when modeling business processes. Extension of Activiti Designer falls into two distinct categories, extending the palette and extending output formats. Each of these extension ways requires a specific approach and different technical expertise.

|      | Extending Activiti Designer requires technical knowledge and more specifically, knowledge of programming in Java. Depending on the type of extension you want to create, you might also need to be familiar with Maven, Eclipse, OSGi, Eclipse extensions and SWT. |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

#### 12.5.1. Customizing the palette

You can customize the palette that is offered to users when modeling processes. The palette is the collection of shapes that can be dragged onto the canvas in a process diagram and is displayed to the right hand side of the canvas. As you can see in the default palette, the default shapes are grouped into compartments (these are called "drawers") for Events, Gateways and so on. There are two options built-in to Activiti Designer to customize the drawers and shapes in the palette:

- Adding your own shapes / nodes to existing or new drawers
- Disabling any or all of the default BPMN 2.0 shapes offered by Activiti Designer, with the exception of the connection and selection tools

In order to customize the palette, you create a JAR file that is added to a specific installation of Activiti Designer (more on [how to do that](https://www.activiti.org/userguide/#eclipseDesignerApplyingExtension)later). Such a JAR file is called an *extension*. By writing classes that are included in your extension, Activiti Designer understands which customizations you wish to make. In order for this to work, your classes should implement certain interfaces. There is an integration library available with those interfaces and base classes to extend which you should add to your project’s classpath.

You can find the code examples listed below in source control with Activiti Designer. Take a look in the `examples/money-tasks` directory in the `projects/designer` directory of Activiti’s source code.

|      | You can setup your project in whichever tool you prefer and build the JAR with your build tool of choice. For the instructions below, a setup is assumed with Eclipse Kepler or Indigo, using Maven (3.x) as build tool, but any setup should enable you to create the same results. |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

##### Extension setup (Eclipse/Maven)

Download and extract [Eclipse](http://www.eclipse.org/downloads) (most recent versions should work) and a recent version (3.x) of [Apache Maven](http://maven.apache.org/download.html). If you use a 2.x version of Maven, you will run into problems when building your project, so make sure your version is up to date. We assume you are familiar with using basic features and the Java editor in Eclipse. It’s up to you whether you prefer to use Eclipse’s features for Maven or run Maven commands from a command prompt.

Create a new project in Eclipse. This can be a general project type. Create a `pom.xml` file at the root of the project to contain the Maven project setup. Also create folders for the `src/main/java` and `src/main/resources` folders, which are Maven conventions for your Java source files and resources respectively. Open the `pom.xml` file and add the following lines:

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14<project
  xmlns="http://maven.apache.org/POM/4.0.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">

  <modelVersion>4.0.0</modelVersion>

  <groupId>org.acme</groupId>
  <artifactId>money-tasks</artifactId>
  <version>1.0.0</version>
  <packaging>jar</packaging>
  <name>Acme Corporation Money Tasks</name>
...
</project>
```

As you can see, this is just a basic pom.xml file that defines a `groupId`, `artifactId` and `version` for the project. We will create a customization that includes a single custom node for our money business.

Add the integration library to your project’s dependencies by including this dependency in your `pom.xml` file:

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15<dependencies>
  <dependency>
    <groupId>org.activiti.designer</groupId>
    <artifactId>org.activiti.designer.integration</artifactId>
    <version>5.12.0</version> <!-- Use the current Activiti Designer version -->
    <scope>compile</scope>
  </dependency>
</dependencies>
...
<repositories>
  <repository>
      <id>Activiti</id>
      <url>https://maven.alfresco.com/nexus/content/groups/public/</url>
   </repository>
</repositories>
```

Finally, in the` pom.xml` file, add the configuration for the `maven-compiler-plugin` so the Java source level is at least 1.5 (see snippet below). You will need this in order to use annotations. You can also include instructions for Maven to generate the JAR’s `MANIFEST.MF` file. This is not required, but you can use a specific property in the manifest to provide a name for your extension (this name may be shown at certain places in the designer and is primarily intended for future use if you have several extensions in the designer). If you wish to do so, include the following snippet in `pom.xml`:

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31<build>
  <plugins>
        <plugin>
      <artifactId>maven-compiler-plugin</artifactId>
      <configuration>
        <source>1.5</source>
        <target>1.5</target>
        <showDeprecation>true</showDeprecation>
        <showWarnings>true</showWarnings>
        <optimize>true</optimize>
      </configuration>
    </plugin>
    <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-jar-plugin</artifactId>
      <version>2.3.1</version>
      <configuration>
        <archive>
          <index>true</index>
          <manifest>
            <addClasspath>false</addClasspath>
            <addDefaultImplementationEntries>true</addDefaultImplementationEntries>
          </manifest>
          <manifestEntries>
            <ActivitiDesigner-Extension-Name>Acme Money</ActivitiDesigner-Extension-Name>
          </manifestEntries>
        </archive>
      </configuration>
    </plugin>
  </plugins>
</build>
```

The name for the extension is described by the `ActivitiDesigner-Extension-Name` property. The only thing left to do now is tell Eclipse to setup the project according to the instructions in `pom.xml`. So open up a command shell and go to the root folder of your project in the Eclipse workspace. Then execute the following Maven command:

```
mvn eclipse:eclipse
```

Wait until the build is successful. Refresh the project (use the project’s context menu (right-click) and select `Refresh`). You should now have the `src/main/java` and `src/main/resources` folders as source folders in the Eclipse project.

|      | You can of course also use the [m2eclipse](http://www.eclipse.org/m2e) plugin and simply enable Maven dependency management from the context menu (right-click) of the project. Then choose `Maven` > `Update project configuration` from the project’s context menu. That should setup the source folders as well. |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

That’s it for the setup. Now you’re ready to start creating customizations to Activiti Designer!

##### Applying your extension to Activiti Designer

You might be wondering how you can add your extension to Activiti Designer so your customizations are applied. These are the steps to do just that: * Once you’ve created your extension JAR (for instance, by performing a mvn install in your project to build it with Maven), you need to transfer the extension to the computer where Activiti Designer is installed; * Store the extension somewhere on the hard drive where it will be able to remain and remember the location. *Note:* the location must be outside the Eclipse workspace of Activiti Designer - storing the extension inside the workspace will lead to the user getting a popup error message and the extensions being unavailable; * Start Activiti Designer and from the menu, select `Window` > `Preferences` * In the preferences screen, type `user` as keyword. You should see an option to access the `User Libraries` in Eclipse in the `Java` section.

![designer.preferences.userlibraries](https://www.activiti.org/userguide/images/designer.preferences.userlibraries.png)

- Select the User Libraries item and a tree view shows up to the right where you can add libraries. You should see the default group where you can add extensions to Activiti Designer (depending on your Eclipse installation, you might see several others as well).

![designer.preferences.userlibraries.activiti.empty](https://www.activiti.org/userguide/images/designer.preferences.userlibraries.activiti.empty.png)

- Select the `Activiti Designer Extensions` group and click the `Add JARs…` button. Navigate to the folder where your extension is stored and select the extension file you want to add. After completing this, your preferences screen should show the extension as part of the `Activiti Designer Extensions` group, as shown below.

![designer.preferences.userlibraries.activiti.moneytasks](https://www.activiti.org/userguide/images/designer.preferences.userlibraries.activiti.moneytasks.png)

- Click the `OK` button to save and close the preferences dialog. The `Activiti Designer Extensions` group is automatically added to new Activiti projects you create. You can see the user library as entry in the project’s tree in the Navigator or Package Explorer. If you already had Activiti projects in the workspace, you should also see the new extensions show up in the group. An example is shown below.

![designer.userlibraries.project](https://www.activiti.org/userguide/images/designer.userlibraries.project.png)

Diagrams you open will now have the shapes from the new extension in their palette (or shapes disabled, depending on the customizations in your extension). If you already had a diagram opened, close and reopen it to see the changes in the palette.

##### Adding shapes to the palette

With your project set up, you can now easily add shapes to the palette. Each shape you wish to add is represented by a class in your JAR. Take note that these classes are not the classes that will be used by the Activiti engine during runtime. In your extension you describe the properties that can be set in Activiti Designer for each shape. From these shapes, you can also define the runtime characteristics that should be used by the engine when a process instance reaches the node in the process. The runtime characteristics can use any of the options that Activiti supports for regular `ServiceTask`s. See [this section](https://www.activiti.org/userguide/#eclipseDesignerConfiguringRuntime) for more details.

A shape’s class is a simple Java class, to which a number of annotations are added. The class should implement the `CustomServiceTask` interface, but you shouldn’t implement this interface yourself. Extend the `AbstractCustomServiceTask` base class instead (at the moment you MUST extend this class directly, so no abstract classes in between). In the Javadoc for that class you can find instructions on the defaults it provides and when you should override any of the methods it already implements. Overrides allow you to do things such as providing icons for the palette and in the shape on the canvas (these can be different) and specifying the base shape you want the node to have (activity, event, gateway).

```
1
2
3
4
5
6
7
8/**
 * @author John Doe
 * @version 1
 * @since 1.0.0
 */
public class AcmeMoneyTask extends AbstractCustomServiceTask {
...
}
```

You will need to implement the `getName()` method to determine the name the node will have in the palette. You can also put the nodes in their own drawer and provide an icon. Override the appropriate methods from `AbstractCustomServiceTask`. If you want to provide an icon, make sure it’s in the `src/main/resources` package in your JAR and is about 16x16 pixels and a JPEG or PNG format. The path you supply is relative to that folder.

You can add properties to the shape by adding members to the class and annotating them with the `@Property` annotation like this:

```
1
2
3@Property(type = PropertyType.TEXT, displayName = "Account Number")
@Help(displayHelpShort = "Provide an account number", displayHelpLong = HELP_ACCOUNT_NUMBER_LONG)
private String accountNumber;
```

There are several `PropertyType` values you can use, which are described in more detail in [this section](https://www.activiti.org/userguide/#eclipseDesignerPropertyTypes). You can make a field required by setting the required attribute to true. A message and red background will appear if the user doesn’t fill out the field.

If you want to ensure the order of the various properties in your class as they appear in the property screen, you should specify the order attribute of the `@Property` annotation.

As you can see, there’s also a `@Help` annotation that’s used to provide the user some guidance when filling out the field. You can also use the `@Help` annotation on the class itself - this information is shown at the top of the property sheet presented to the user.

Below is the listing for a further elaboration of the `MoneyTask`. A comment field has been added and you can see an icon is included for the node.

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34
35
36
37
38
39
40
41
42
43
44/**
 * @author John Doe
 * @version 1
 * @since 1.0.0
 */
@Runtime(javaDelegateClass = "org.acme.runtime.AcmeMoneyJavaDelegation")
@Help(displayHelpShort = "Creates a new account", displayHelpLong = "Creates a new account using the account number specified")
public class AcmeMoneyTask extends AbstractCustomServiceTask {

  private static final String HELP_ACCOUNT_NUMBER_LONG = "Provide a number that is suitable as an account number.";

  @Property(type = PropertyType.TEXT, displayName = "Account Number", required = true)
  @Help(displayHelpShort = "Provide an account number", displayHelpLong = HELP_ACCOUNT_NUMBER_LONG)
  private String accountNumber;

  @Property(type = PropertyType.MULTILINE_TEXT, displayName = "Comments")
  @Help(displayHelpShort = "Provide comments", displayHelpLong = "You can add comments to the node to provide a brief description.")
  private String comments;

  /*
   * (non-Javadoc)
   *
   * @see org.activiti.designer.integration.servicetask.AbstractCustomServiceTask #contributeToPaletteDrawer()
   */
  @Override
  public String contributeToPaletteDrawer() {
    return "Acme Corporation";
  }

  @Override
  public String getName() {
    return "Money node";
  }

  /*
   * (non-Javadoc)
   *
   * @see org.activiti.designer.integration.servicetask.AbstractCustomServiceTask #getSmallIconPath()
   */
  @Override
  public String getSmallIconPath() {
    return "icons/coins.png";
  }
}
```

If you extend Activiti Designer with this shape, The palette and corresponding node will look like this:

![designer.palette.add.money](https://www.activiti.org/userguide/images/designer.palette.add.money.png)

The properties screen for the money task is shown below. Note the required message for the `accountNumber` field.

![designer.palette.add.money.properties.required](https://www.activiti.org/userguide/images/designer.palette.add.money.properties.required.png)

Users can enter static text or use expressions that use process variables in the property fields when creating diagrams (e.g. "This little piggy went to ${piggyLocation}"). Generally, this applies to text fields where users are free to enter any text. If you expect users to want to use expressions and you apply runtime behavior to your `CustomServiceTask` (using `@Runtime`), make sure to use `Expression` fields in the delegate class so the expressions are correctly resolved at runtime. More information on runtime behavior can be found in [this section](https://www.activiti.org/userguide/#eclipseDesignerConfiguringRuntime).

The help for fields is offered by the buttons to the right of each property. Clicking on the button shows a popup as displayed below.

![designer.palette.add.money.help](https://www.activiti.org/userguide/images/designer.palette.add.money.help.png)

###### Configuring runtime execution of Custom Service Tasks

With your fields setup and your extension applied to Designer, users can configure the properties of the service task when modelling a process. In most cases, you will want to use these user-configured properties when the process is executed by Activiti. To do this, you must instruct Activiti which class to instantiate when the process reaches your `CustomServiceTask`.

There is a special annotation for specifying the runtime characteristics of your `CustomServiceTask`, the `@Runtime` annotation. Here’s an example of how to use it:

```
1@Runtime(javaDelegateClass = "org.acme.runtime.AcmeMoneyJavaDelegation")
```

Your `CustomServiceTask` will result in a normal `ServiceTask` in the BPMN output of processes modelled with it. Activiti enables [several ways](https://www.activiti.org/userguide/#bpmnJavaServiceTask) to define the runtime characteristics of `ServiceTask`s. Therefore, the `@Runtime` annotation can take one of three attributes, which match directly to the options Activiti provides, like this:

- `javaDelegateClass` maps to `activiti:class` in the BPMN output. Specify the fully qualified classname of a class that implements `JavaDelegate`.
- `expression` maps to `activiti:expression` in the BPMN output. Specify an expression to a method to be executed, such as a method in a Spring Bean. You should *not* specify any `@Property` annotations on fields when using this option. For more information, see below.
- `javaDelegateExpression` maps to `activiti:delegateExpression` in the BPMN output. Specify an expression to a class that implements `JavaDelegate`.

The user’s property values will be injected into the runtime class if you provide members in the class for Activiti to inject into. The names should match the names of the members in your `CustomServiceTask`. For more information, consult [this part](https://www.activiti.org/userguide/#serviceTaskFieldInjection) of the userguide. Note that since version 5.11.0 of the Designer you can use the `Expression` interface for dynamic field values. This means that the value of the property in the Activiti Designer must contain an expression and this expression will then be injected into an `Expression` property in the `JavaDelegate` implementation class.

|      | You can use `@Property` annotations on members of your `CustomServiceTask`, but this will not work if you use `@Runtime`*s expression attribute. The reason for this is that the expression you specify will be attempted to be resolved to a method by Activiti, not to a class. Therefore, no injection into a class will be performed. Any members marked with @Property will be ignored by Designer if you use expression in your @Runtime annotation. Designer will not render them as editable fields in the node’s property pane and will produce no output for the properties in the process* BPMN. |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

|      | Note that the runtime class shouldn’t be in your extension JAR, as it’s dependent on the Activiti libraries. Activiti needs to be able to find it at runtime, so it needs to be on the Activiti engine’s classpath. |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

The examples project in Designer’s source tree contains examples of the different options for configuring `@Runtime`. Take a look in the money-tasks project for some starting points. The examples refer to delegate class examples that are in the money-delegates project.

##### Property types

This section describes the property types you can use for a `CustomServiceTask` by setting its type to a `PropertyType` value.

###### PropertyType.TEXT

Creates a single line text field as shown below. Can be a required field and shows validation messages as a tooltip. Validation failures are displayed by changing the background of the field to a light red color.

![designer.property.text.invalid](https://www.activiti.org/userguide/images/designer.property.text.invalid.png)

###### PropertyType.MULTILINE_TEXT

Creates a multiline text field as shown below (height is fixed at 80 pixels). Can be a required field and shows validation messages as a tooltip. Validation failures are displayed by changing the background of the field to a light red color.

![designer.property.multiline.text.invalid](https://www.activiti.org/userguide/images/designer.property.multiline.text.invalid.png)

###### PropertyType.PERIOD

Creates a structured editor for specifying a period of time by editing amounts of each unit with a spinner control. The result is shown below. Can be a required field (which is interpreted such that not all values may be 0, so at least 1 part of the period must have a non-zero value) and shows validation messages as a tooltip. Validation failures are displayed by changing the background of the entire field to a light red color. The value of the field is stored as a string of the form 1y 2mo 3w 4d 5h 6m 7s, which represents 1 year, 2 months, 3 weeks, 4 days, 6 minutes and 7 seconds. The entire string is always stored, even if parts are 0.

![designer.property.period](https://www.activiti.org/userguide/images/designer.property.period.png)

###### PropertyType.BOOLEAN_CHOICE

Creates a single checkbox control for boolean or toggle choices. Note that you can specify the `required` attribute on the `Property`annotation, but it will not be evaluated because that would leave the user without a choice whether to check the box or not. The value stored in the diagram is java.lang.Boolean.toString(boolean), which results in "true" or "false".

![designer.property.boolean.choice](https://www.activiti.org/userguide/images/designer.property.boolean.choice.png)

###### PropertyType.RADIO_CHOICE

Creates a group of radio buttons as shown below. Selection of any of the radio buttons is mutually exclusive with selection of any of the others (i.e., only one selection allowed). Can be a required field and shows validation messages as a tooltip. Validation failures are displayed by changing the background of the group to a light red color.

This property type expects the class member you have annotated to also have an accompanying `@PropertyItems` annotation (for an example, see below). Using this additional annotation, you can specify the list of items that should be offered in an array of Strings. Specify the items by adding two array entries for each item: first, the label to be shown; second, the value to be stored.

```
1
2
3
4@Property(type = PropertyType.RADIO_CHOICE, displayName = "Withdrawl limit", required = true)
@Help(displayHelpShort = "The maximum daily withdrawl amount ", displayHelpLong = "Choose the maximum daily amount that can be withdrawn from the account.")
@PropertyItems({ LIMIT_LOW_LABEL, LIMIT_LOW_VALUE, LIMIT_MEDIUM_LABEL, LIMIT_MEDIUM_VALUE, LIMIT_HIGH_LABEL, LIMIT_HIGH_VALUE })
private String withdrawlLimit;
```

![designer.property.radio.choice](https://www.activiti.org/userguide/images/designer.property.radio.choice.png)

![designer.property.radio.choice.invalid](https://www.activiti.org/userguide/images/designer.property.radio.choice.invalid.png)

###### PropertyType.COMBOBOX_CHOICE

Creates a combobox with fixed options as shown below. Can be a required field and shows validation messages as a tooltip. Validation failures are displayed by changing the background of the combobox to a light red color.

This property type expects the class member you have annotated to also have an accompanying `@PropertyItems` annotation (for an example, see below). Using this additional annotation, you can specify the list of items that should be offered in an array of Strings. Specify the items by adding two array entries for each item: first, the label to be shown; second, the value to be stored.

```
1
2
3
4
5
6@Property(type = PropertyType.COMBOBOX_CHOICE, displayName = "Account type", required = true)
@Help(displayHelpShort = "The type of account", displayHelpLong = "Choose a type of account from the list of options")
@PropertyItems({ ACCOUNT_TYPE_SAVINGS_LABEL, ACCOUNT_TYPE_SAVINGS_VALUE, ACCOUNT_TYPE_JUNIOR_LABEL, ACCOUNT_TYPE_JUNIOR_VALUE, ACCOUNT_TYPE_JOINT_LABEL,
  ACCOUNT_TYPE_JOINT_VALUE, ACCOUNT_TYPE_TRANSACTIONAL_LABEL, ACCOUNT_TYPE_TRANSACTIONAL_VALUE, ACCOUNT_TYPE_STUDENT_LABEL, ACCOUNT_TYPE_STUDENT_VALUE,
  ACCOUNT_TYPE_SENIOR_LABEL, ACCOUNT_TYPE_SENIOR_VALUE })
private String accountType;
```

![designer.property.combobox.choice](https://www.activiti.org/userguide/images/designer.property.combobox.choice.png)

![designer.property.combobox.choice.invalid](https://www.activiti.org/userguide/images/designer.property.combobox.choice.invalid.png)

###### PropertyType.DATE_PICKER

Creates a date selection control as shown below. Can be a required field and shows validation messages as a tooltip (note, that the control used will auto-set the selection to the date on the system, so the value is seldom empty). Validation failures are displayed by changing the background of the control to a light red color.

This property type expects the class member you have annotated to also have an accompanying `@DatePickerProperty` annotation (for an example, see below). Using this additional annotation, you can specify the date time pattern to be used to store dates in the diagram and the type of datepicker you would like to be shown. Both attributes are optional and have default values that will be used if you don’t specify them (these are static variables in the `DatePickerProperty` annotation). The `dateTimePattern` attribute should be used to supply a pattern to the `SimpleDateFormat` class. When using the `swtStyle` attribute, you should specify an integer value that is supported by `SWT`'s `DateTime` control, because this is the control that is used to render this type of property.

```
1
2
3
4@Property(type = PropertyType.DATE_PICKER, displayName = "Expiry date", required = true)
@Help(displayHelpShort = "The date the account expires ", displayHelpLong = "Choose the date when the account will expire if no extended before the date.")
@DatePickerProperty(dateTimePattern = "MM-dd-yyyy", swtStyle = 32)
private String expiryDate;
```

![designer.property.date.picker](https://www.activiti.org/userguide/images/designer.property.date.picker.png)

###### PropertyType.DATA_GRID

Creates a data grid control as shown below. A data grid can be used to allow the user to enter an arbitrary amount of rows of data and enter values for a fixed set of columns in each of those rows (each individual combination of row and column is referred to as a cell). Rows can be added and removed as the user sees fit.

This property type expects the class member you have annotated to also have an accompanying `@DataGridProperty` annotation (for an example, see below). Using this additional annotation, you can specify some specific attributes of the data grid. You are required to reference a different class to determine which columns go into the grid with the `itemClass` attribute. Activiti Designer expects the member type to be a `List`. By convention, you can use the class of the `itemClass` attribute as its generic type. If, for example, you have a grocery list that you edit in the grid, you would define the columns of the grid in the `GroceryListItem` class. From your `CustomServiceTask`, you would refer to it like this:

```
1
2
3@Property(type = PropertyType.DATA_GRID, displayName = "Grocery List")
@DataGridProperty(itemClass = GroceryListItem.class)
private List<GroceryListItem> groceryList;
```

The "itemClass" class uses the same annotations you would otherwise use to specify fields of a `CustomServiceTask`, with the exception of using a data grid. Specifically, `TEXT`, `MULTILINE_TEXT` and `PERIOD` are currently supported. You’ll notice the grid will create single line text controls for each field, regardless of the `PropertyType`. This is done on purpose to keep the grid graphically appealing and readable. If you consider the regular display mode for a `PERIOD` `PropertyType` for instance, you can imagine it would never properly fit in a grid cell without cluttering the screen. For `MULTILINE_TEXT` and `PERIOD`, a double-click mechanism is added to each field which pops up a larger editor for the `PropertyType`. The value is stored to the field after the user clicks OK and is therefore readable within the grid.

Required attributes are handled in a similar manner to regular fields of type `TEXT` and the entire grid is validated as soon as any field loses focus. The background color of the text control in a specific cell of the data grid is changed to light red if there are validation failures.

By default, the component allows the user to add rows, but not to determine the order of those rows. If you wish to allow this, you should set the `orderable` attribute to true, which enables buttons at the end of each row to move it up or down in the grid.

|      | At the moment, this property type is not correctly injected into your runtime class. |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

![designer.property.datagrid](https://www.activiti.org/userguide/images/designer.property.datagrid.png)

##### Disabling default shapes in the palette

This customization requires you to include a class in your extension that implements the `DefaultPaletteCustomizer` interface. You should not implement this interface directly, but subclass the `AbstractDefaultPaletteCustomizer` base class. Currently, this class provides no functionality, but future versions of the `DefaultPaletteCustomizer` interface will offer more capabilities for which this base class will provide some sensible defaults so it’s best to subclass so your extension will be compatible with future releases.

Extending the `AbstractDefaultPaletteCustomizer` class requires you to implement one method, `disablePaletteEntries()`, from which you must return a list of `PaletteEntry` values. For each of the default shapes, you can disable it by adding its corresponding `PaletteEntry` value to your list. Note that if you remove shapes from the default set and there are no remaining shapes in a particular drawer, that drawer will be removed from the palette in its entirety. If you wish to disable all of the default shapes, you only need to add `PaletteEntry.ALL` to your result. As an example, the code below disables the Manual task and Script task shapes in the palette.

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16public class MyPaletteCustomizer extends AbstractDefaultPaletteCustomizer {

  /*
   * (non-Javadoc)
   *
   * @see org.activiti.designer.integration.palette.DefaultPaletteCustomizer#disablePaletteEntries()
   */
  @Override
  public List<PaletteEntry> disablePaletteEntries() {
    List<PaletteEntry> result = new ArrayList<PaletteEntry>();
    result.add(PaletteEntry.MANUAL_TASK);
    result.add(PaletteEntry.SCRIPT_TASK);
    return result;
  }

}
```

The result of applying this extension is shown in the picture below. As you can see, the manual task and script task shapes are no longer available in the `Tasks` drawer.

![designer.palette.disable.manual.and.script](https://www.activiti.org/userguide/images/designer.palette.disable.manual.and.script.png)

To disable all of the default shapes, you could use something similar to the code below.

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15public class MyPaletteCustomizer extends AbstractDefaultPaletteCustomizer {

  /*
   * (non-Javadoc)
   *
   * @see org.activiti.designer.integration.palette.DefaultPaletteCustomizer#disablePaletteEntries()
   */
  @Override
  public List<PaletteEntry> disablePaletteEntries() {
    List<PaletteEntry> result = new ArrayList<PaletteEntry>();
    result.add(PaletteEntry.ALL);
    return result;
  }

}
```

The result will look like this (notice that the drawers the default shapes were in are no longer in the palette):

![designer.palette.disable.all](https://www.activiti.org/userguide/images/designer.palette.disable.all.png)

#### 12.5.2. Validating diagrams and exporting to custom output formats

Besides customizing the palette, you can also create extensions to Activiti Designer that can perform validations and save information from the diagram to custom resources in the Eclipse workspace. There are built-in extension points for doing this and this section explains how to use them.

|      | The ExportMarshaller functions were reintroduced recently. We are still working on the validation functionality. The documentation below details the old situation and will be updated when the new functionality is available. |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

Activiti Designer allows you to write extensions that validate diagrams. There are already validations of BPMN constructs in the tool by default, but you can add your own if you want to validate additional items such as modeling conventions or the values in properties of `CustomServiceTask`s. These extensions are known as `Process Validators`.

You can also Activiti Designer to publish to additional formats when saving diagrams. These extensions are called `Export Marshallers`and are invoked automatically by Activiti Designer on each save action by the user. This behavior can be enabled or disabled by setting a preference in Eclipse’s preferences dialog for each format for which there is an extension detected. Designer will make sure your `ExportMarshaller` is invoked when saving the diagram, depending on the user’s preference.

Often, you will want to combine a `ProcessValidator` and an `ExportMarshaller`. Let’s say you have a number of `CustomServiceTask`s in use that have properties you would like to use in the process that gets generated. However, before the process is generated, you want to validate some of those values first. Combining a `ProcessValidator` and `ExportMarshaller` is the best way to accomplish this and Activiti Designer enables you to plug your extensions into the tool seamlessly.

To create a `ProcessValidator` or an `ExportMarshaller`, you need to create a different kind of extension than for extending the palette. The reason for this is simple: from your code you will need access to more APIs than those that are offered by the integration library. In particular, you will need classes that are available in Eclipse itself. So to get started, you should create an Eclipse plugin (which you can do by using Eclipse’s PDE support) and package it in a custom Eclipse product or feature. It’s beyond the scope of this user guide to explain all the details involved in developing Eclipse plugins, so the instructions below are limited to the functionality for extending Activiti Designer.

Your bundle should be dependent on the following libraries:

- org.eclipse.core.runtime
- org.eclipse.core.resources
- org.activiti.designer.eclipse
- org.activiti.designer.libs
- org.activiti.designer.util

Optionally, the org.apache.commons.lang bundle is available through Designer if you’d like to use that in your extension.

Both `ProcessValidator`s and `ExportMarshaller`s are created by extending a base class. These base classes inherit some useful methods from their superclass, the `AbstractDiagramWorker` class. Using these methods you can create information, warning and error markers that show up in Eclipse’s problems view for the user to figure out what’s wrong or important. You can get to information about the diagram in the form of `Resources` and `InputStreams`. This information is provided from the `DiagramWorkerContext`, which is available from the `AbstractDiagramWorker` class.

It’s probably a good idea to invoke `clearMarkers()` as one of the first things you do in either a `ProcessValidator` or an `ExportMarshaller`; this will clear any previous markers for your worker (markers are automatically linked to the worker and clearing markers for one worker leaves other markers untouched). For example:

```
1
2// Clear markers for this diagram first
clearMarkersForDiagram();
```

You should also use the progress monitor provided (in the `DiagramWorkerContext`) to report your progress back to the user because validations and/or marshalling actions can take up some time during which the user is forced to wait. Reporting progress requires some knowledge of how you should use Eclipse’s features. Take a look at [this article](http://www.eclipse.org/articles/Article-Progress-Monitors/article.html) for a thorough explanation of the concepts and usage.

##### Creating a ProcessValidator extension

|      | Under review! |
| ---- | ------------- |
|      |               |

Create an extension to the `org.activiti.designer.eclipse.extension.validation.ProcessValidator` extension point in your `plugin.xml` file. For this extension point, you are required to subclass the `AbstractProcessValidator` class.

```
1
2
3
4
5
6
7
8
9<?eclipse version="3.6"?>
<plugin>
  <extension
    point="org.activiti.designer.eclipse.extension.validation.ProcessValidator">
    <ProcessValidator
      class="org.acme.validation.AcmeProcessValidator">
    </ProcessValidator>
  </extension>
</plugin>
1
2public class AcmeProcessValidator extends AbstractProcessValidator {
}
```

You have to implement a number of methods. Most importantly, implement `getValidatorId()` so you return a globally unique ID for your validator. This will enable you to invoke it from and `ExportMarshaller`, or event let someone *else* invoke your validator from their `ExportMarshaller`. Implement `getValidatorName()` and return a logical name for your validator. This name is shown to the user in dialogs. In `getFormatName()`, you can return the type of diagram the validator typically validates.

The validation work itself is done in the `validateDiagram()` method. From this point on, it’s up to your specific functionality what you code here. Typically, however, you will want to start by getting hold of the nodes in the diagram’s process, so you can iterate through them, collect, compare and validate data. This snippet shows you how to do this:

```
1
2
3
4
5
6
7final EList<EObject> contents = getResourceForDiagram(diagram).getContents();
for (final EObject object : contents) {
  if (object instanceof StartEvent ) {
  // Perform some validations for StartEvents
  }
  // Other node types and validations
}
```

Don’t forget to invoke `addProblemToDiagram()` and/or `addWarningToDiagram()`, etc as you go through your validations. Make sure you return a correct boolean result at the end to indicate whether you consider the validation as succeeded or failed. This can be used by and invoking `ExportMarshaller` to determine the next course of action.

##### Creating an ExportMarshaller extension

Create an extension to the `org.activiti.designer.eclipse.extension.ExportMarshaller` extension point in your `plugin.xml`file. For this extension point, you are required to subclass the `AbstractExportMarshaller` class. This abstract base class provides you with a number of useful methods when marshalling to your own format, but most importantly it allows you to save resources to the workspace and to invoke validators.

An example implementation is available in Designer’s examples folder. This example shows how to use the methods in the base class to get the basics done, such as accessing the diagram’s `InputStream`, using its `BpmnModel` and saving resources to the workspace.

```
1
2
3
4
5
6
7
8
9<?eclipse version="3.6"?>
<plugin>
  <extension
    point="org.activiti.designer.eclipse.extension.ExportMarshaller">
    <ExportMarshaller
      class="org.acme.export.AcmeExportMarshaller">
    </ExportMarshaller>
  </extension>
  </plugin>
1
2public class AcmeExportMarshaller extends AbstractExportMarshaller {
}
```

You are required to implement some methods, such as `getMarshallerName()` and `getFormatName()`. These methods are used to display options to the user and to show information in progress dialogs, so make sure the descriptions you return reflect the functionality you are implementing.

The bulk of your work is performed in the `doMarshallDiagram()` method.

If you want to perform a certain validation first, you can invoke the validator directly from your marshaller. You receive a boolean result from the validator, so you know whether validation succeeded. In most cases you won’t want to proceed with marshalling the diagram if it’s not valid, but you might choose to go ahead anyway or even create a different resource if validation fails.

Once you have all the data you need, you should invoke the `saveResource()` method to create a file containing your data. You can invoke `saveResource()` as many times as you wish from a single ExportMarshaller; a marshaller can therefore be used to create more than one output file.

You can construct a filename for your output resource(s) by using the `saveResource()` method in the `AbstractDiagramWorker` class. There are a couple of useful variables you can have parsed, allowing you to create filenames such as _original-filename__my-format-name.xml. These variables are described in the Javadocs and defined by the `ExportMarshaller` interface. You can also use `resolvePlaceholders()` on a string (e.g. a path) if you want to parse the placeholders yourself. `getURIRelativeToDiagram()` will invoke this for you.

You should use the progress monitor provided to report your progress back to the user. How to do this is described in [this article](http://www.eclipse.org/articles/Article-Progress-Monitors/article.html).

## 13. REST API

### 13.1. General Activiti REST principles

#### 13.1.1. Installation and Authentication

Activiti includes a REST API to the Activiti Engine that can be installed by deploying the activiti-rest.war file to a servlet container like Apache Tomcat. However, it can also be used in another web-application by including the servlet and it’s mapping in your application and add all activiti-rest dependencies to the classpath.

By default the Activiti Engine will connect to an in-memory H2 database. You can change the database settings in the db.properties file in the WEB-INF/classes folder. The REST API uses JSON format ([http://www.json.org](http://www.json.org/)) and is built upon the Spring MVC (<http://docs.spring.io/spring/docs/current/spring-framework-reference/html/mvc.html>).

All REST-resources require a valid Activiti-user to be authenticated by default. Basic HTTP access authentication is used, so you should always include a `Authorization: Basic …==` HTTP-header when performing requests or include the username and password in the request-url (e.g. `http://username:password@localhost…;`).

**It’s recommended to use Basic Authentication in combination with HTTPS.**

#### 13.1.2. Configuration

The Activiti REST web application is using Spring Java Configuration for starting the Activiti Engine, defining the basic authentication security using Spring security and to define the variable converters for specific variable handling. A small amount of properties can be defined by changing the engine.properties file you can find in the WEB-INF/classes folder. If you need more advanced configuration options there’s the possibility to override the default Spring beans in XML in the activiti-custom-context.xml file you can also find in the WEB-INF/classes folder. An example configuration is already in comments in this file. This is also the place to override the default RestResponseFactory by defining a new Spring bean with the name restResponsefactory and use your custom implementation class for it.

#### 13.1.3. Usage in Tomcat

Due to [default security properties on Tomcat](http://tomcat.apache.org/tomcat-7.0-doc/security-howto.html), **escaped forward slashes (%2F and %5C) are not allowed by default (400-result is returned).** This may have an impact on the deployment resources and their data-URL, as the URL can potentially contain escaped forward slashes.

**When issues are experienced with unexpected 400-results, set the following system-property: -Dorg.apache.tomcat.util.buf.UDecoder.ALLOW_ENCODED_SLASH=true.**

It’s a best practice to always set the **Accept** and **Content-Type** (in case of posting/putting JSON) headers to **application/json** on the HTTP requests described below.

#### 13.1.4. Methods and return-codes

| Method   | Operations                                                   |
| -------- | ------------------------------------------------------------ |
| `GET`    | Get a single resource or get a collection of resources.      |
| `POST`   | Create a new resource. Also used for executing resource-queries which have a too complex request-structure to fit in the query-URL of a GET-request. |
| `PUT`    | Update properties of an existing resource. Also used for invoking actions on an existing resource. |
| `DELETE` | Delete an existing resource.                                 |

| Response                       | Description                                                  |
| ------------------------------ | ------------------------------------------------------------ |
| `200 - Ok`                     | The operation was successful and a response has been returned (`GET` and `PUT` requests). |
| `201 - Created`                | The operation was successful and the entity has been created and is returned in the response-body (`POST` request). |
| `204 - No content`             | The operation was successful and entity has been deleted and therefore there is no response-body returned (`DELETE` request). |
| `401 - Unauthorized`           | The operation failed. The operation requires an Authentication header to be set. If this was present in the request, the supplied credentials are not valid or the user is not authorized to perform this operation. |
| `403 - Forbidden`              | The operation is forbidden and should not be re-attempted. This does not imply an issue with authentication not authorization, it’s an operation that is not allowed. Example: deleting a task that is part of a running process is not allowed and will never be allowed, regardless of the user or process/task state. |
| `404 - Not found`              | The operation failed.The requested resource was not found.   |
| `405 - Method not allowed`     | The operation failed. The used method is not allowed for this resource. E.g. trying to update (PUT) a deployment-resource will result in a `405` status. |
| `409 - Conflict`               | The operation failed. The operation causes an update of a resource that has been updated by another operation, which makes the update no longer valid. Can also indicate a resource that is being created in a collection where a resource with that identifier already exists. |
| `415 - Unsupported Media Type` | The operation failed. The request body contains an unsupported media type. Also occurs when the request-body JSON contains an unknown attribute or value that doesn’t have the right format/type to be accepted. |
| `500 - Internal server error`  | The operation failed. An unexpected exception occurred while executing the operation. The response-body contains details about the error. |

The media-type of the HTTP-responses is always `application/json` unless binary content is requested (e.g. deployment resource data), the media-type of the content is used.

#### 13.1.5. Error response body

When an error occurs (both client and server, 4XX and 5XX status-codes) the response body contains an object describing the error that occurred. An example for a 404-status when a task is not found:

```
1
2
3
4{
  "statusCode" : 404,
  "errorMessage" : "Could not find a task with id '444'."
}
```

#### 13.1.6. Request parameters

##### URL fragments

Parameters that are part of the url (e.g. the deploymentId parameter in `http://host/actviti-rest/service/repository/deployments/{deploymentId}`) need to be properly escaped (see [URL-encoding or Percent-encoding](https://en.wikipedia.org/wiki/Percent-encoding)) in case the segment contains special characters. Most frameworks have this functionality built in, but it should be taken into account. Especially for segments that can contains forward-slashes (e.g. deployment resource), this is required.

##### Rest URL query parameters

Parameters added as query-string in the URL (e.g. the name parameter used in `http://host/activiti-rest/service/deployments?name=Deployment`) can have the following types and are mentioned in the corresponding REST-API documentation:

| Type    | Format                                                       |
| ------- | ------------------------------------------------------------ |
| String  | Plain text parameters. Can contain any valid characters that are allowed in URL’s. In case of a `XXXLike` parameter, the string should contain the wildcard character `%` (properly url-encoded). This allows to specify the intent of the like-search. E.g. *Tas%*matches all values, starting with *Tas*. |
| Integer | Parameter representing an integer value. Can only contain numeric non-decimal values, between -2.147.483.648 and 2.147.483.647. |
| Long    | Parameter representing a long value. Can only contain numeric non-decimal values, between -9.223.372.036.854.775.808 and 9.223.372.036.854.775.807. |
| Boolean | Parameter representing a boolean value. Can be either `true` or `false`. All other values other than these two, will cause a *405 - Bad request* response. |
| Date    | Parameter representing a date value. Use the ISO-8601 date-format (see [ISO-8601 on wikipedia](http://en.wikipedia.org/wiki/ISO_8601)) using both time and date-components (e.g. `2013-04-03T23:45Z`). |

##### JSON body parameters

| Type    | Format                                                       |
| ------- | ------------------------------------------------------------ |
| String  | Plain text parameters. In case of a `XXXLike` parameter, the string should contain the wildcard character `%`. This allows to specify the intent of the like-search. E.g. *Tas%* matches all values, starting with *Tas*. |
| Integer | Parameter representing an integer value, using a JSON number. Can only contain numeric non-decimal values, between -2.147.483.648 and 2.147.483.647. |
| Long    | Parameter representing a long value, using a JSON number. Can only contain numeric non-decimal values, between -9.223.372.036.854.775.808 and 9.223.372.036.854.775.807. |
| Date    | Parameter representing a date value, using a JSON text. Use the ISO-8601 date-format (see [ISO-8601 on wikipedia](http://en.wikipedia.org/wiki/ISO_8601)) using both time and date-components (e.g. `2013-04-03T23:45Z`). |

##### Paging and sorting

Paging and order parameters can be added as query-string in the URL (e.g. the name parameter used in `http://host/activiti-rest/service/deployments?sort=name`).

| Parameter | Default value                      | Description                                                  |
| --------- | ---------------------------------- | ------------------------------------------------------------ |
| sort      | different per query implementation | Name of the sort key, for which the default value and the allowed values are different per query implementation. |
| order     | asc                                | Sorting order which can be *asc* or *desc*.                  |
| start     | 0                                  | Parameter to allow for paging of the result. By default the result will start at 0. |
| size      | 10                                 | Parameter to allow for paging of the result. By default the size will be 10. |

##### JSON query variable format

```
1
2
3
4
5
6{
  "name" : "variableName",
  "value" : "variableValue",
  "operation" : "equals",
  "type" : "string"
}
```

| Parameter | Required | Description                                                  |
| --------- | -------- | ------------------------------------------------------------ |
| name      | No       | Name of the variable to include in a query. Can be empty in case *equals* is used in some queries to query for resources that have **any variable name** with the given value. |
| value     | Yes      | Value of the variable included in the query, should include a correct format for the given type. |
| operator  | Yes      | Operator to use in query, can have the following values: `equals, notEquals, equalsIgnoreCase, notEqualsIgnoreCase, lessThan, greaterThan, lessThanOrEquals, greaterThanOrEquals` and `like`. |
| type      | No       | Type of variable to use. When omitted, the type will be deducted from the `value`parameter. Any JSON text-values will be considered of type `string`, JSON booleans of type `boolean`, JSON numbers of type `long` or `integer`depending on the size of the number. It’s recommended to include an explicit type when in doubt. Types supported out of the box are listed below. |

| Type name | Description                                                  |
| --------- | ------------------------------------------------------------ |
| string    | Value is threaded as and converted to a `java.lang.String`.  |
| short     | Value is threaded as and converted to a `java.lang.Integer`. |
| integer   | Value is threaded as and converted to a `java.lang.Integer`. |
| long      | Value is threaded as and converted to a `java.lang.Long`.    |
| double    | Value is threaded as and converted to a `java.lang.Double`.  |
| boolean   | Value is threaded as and converted to a `java.lang.Boolean`. |
| date      | Value is treated as and converted to a `java.util.Date`. The JSON string will be converted using ISO-8601 date format. |

##### Variable representation

When working with variables (execution/process and task), the REST-api uses some common principles and JSON-format for both reading and writing. The JSON representation of a variable looks like this:

```
1
2
3
4
5
6
7{
  "name" : "variableName",
  "value" : "variableValue",
  "valueUrl" : "http://...",
  "scope" : "local",
  "type" : "string"
}
```

| Parameter | Required | Description                                                  |
| --------- | -------- | ------------------------------------------------------------ |
| name      | Yes      | Name of the variable.                                        |
| value     | No       | Value of the variable. When writing a variable and `value` is omitted, `null` will be used as value. |
| valueUrl  | No       | When reading a variable of type `binary`or `serializable`, this attribute will point to the URL where the raw binary data can be fetched from. |
| scope     | No       | Scope of the variable. If *local*, the variable is explicitly defined on the resource it’s requested from. When *global*, the variable is defined on the parent (or any parent in the parent-tree) of the resource it’s requested from. When writing a variable and the scope is omitted, `global` is assumed. |
| type      | No       | Type of the variable. See table below for additional information on types. When writing a variable and this value is omitted, the type will be deducted from the raw JSON-attribute request type and is limited to either `string`, `double`, `integer` and `boolean`. It’s advised to always include a type to make sure no wrong assumption about the type can be done. |

| Type name    | Description                                                  |
| ------------ | ------------------------------------------------------------ |
| string       | Value is threaded as a `java.lang.String`. Raw JSON-text value is used when writing a variable. |
| integer      | Value is threaded as a `java.lang.Integer`. When writing, JSON number value is used as base for conversion, falls back to JSON text. |
| short        | Value is threaded as a `java.lang.Short`. When writing, JSON number value is used as base for conversion, falls back to JSON text. |
| long         | Value is threaded as a `java.lang.Long`. When writing, JSON number value is used as base for conversion, falls back to JSON text. |
| double       | Value is threaded as a `java.lang.Double`. When writing, JSON number value is used as base for conversion, falls back to JSON text. |
| boolean      | Value is threaded as a `java.lang.Boolean`. When writing, JSON boolean value is used for conversion. |
| date         | Value is treated as a `java.util.Date`. When writing, the JSON text will be converted using ISO-8601 date format. |
| binary       | Binary variable, treated as an array of bytes. The `value` attribute is null, the `valueUrl` contains an URL pointing to the raw binary stream. |
| serializable | Serialized representation of a Serializable Java-object. As with the `binary` type, the `value` attribute is null, the `valueUrl`contains an URL pointing to the raw binary stream. All serializable variables (which are not of any of the above types) will be exposed as a variable of this type. |

It’s possible to support additional variable-types with a custom JSON representation (either simple value or complex/nested JSON object). By extending the `initializeVariableConverters()` method on `org.activiti.rest.service.api.RestResponseFactory`, you can add additional `org.activiti.rest.service.api.engine.variable.RestVariableConverter` classes to support converting your POJO’s to a format suitable for transferring through REST and converting the REST-value back to your POJO. The actual transformation to JSON is done by Jackson.

### 13.2. Deployment

**When using tomcat, please read Usage in Tomcat.**

#### 13.2.1. List of Deployments

```
GET repository/deployments
```

| Parameter         | Required | Value                                             | Description                                                  |
| ----------------- | -------- | ------------------------------------------------- | ------------------------------------------------------------ |
| name              | No       | String                                            | Only return deployments with the given name.                 |
| nameLike          | No       | String                                            | Only return deployments with a name like the given name.     |
| category          | No       | String                                            | Only return deployments with the given category.             |
| categoryNotEquals | No       | String                                            | Only return deployments which don’t have the given category. |
| tenantId          | No       | String                                            | Only return deployments with the given tenantId.             |
| tenantIdLike      | No       | String                                            | Only return deployments with a tenantId like the given value. |
| withoutTenantId   | No       | Boolean                                           | If `true`, only returns deployments without a tenantId set. If `false`, the `withoutTenantId` parameter is ignored. |
| sort              | No       | *id* (default), *name*, *deploytime*or *tenantId* | Property to sort on, to be used together with the *order*.   |

| Response code | Description                           |
| ------------- | ------------------------------------- |
| 200           | Indicates the request was successful. |

**Success response body:**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16
17{
  "data": [
    {
      "id": "10",
      "name": "activiti-examples.bar",
      "deploymentTime": "2010-10-13T14:54:26.750+02:00",
      "category": "examples",
      "url": "http://localhost:8081/service/repository/deployments/10",
      "tenantId": null
    }
  ],
  "total": 1,
  "start": 0,
  "sort": "id",
  "order": "asc",
  "size": 1
}
```

#### 13.2.2. Get a deployment

```
GET repository/deployments/{deploymentId}
```

| Parameter    | Required | Value  | Description                      |
| ------------ | -------- | ------ | -------------------------------- |
| deploymentId | Yes      | String | The id of the deployment to get. |

| Response code | Description                                       |
| ------------- | ------------------------------------------------- |
| 200           | Indicates the deployment was found and returned.  |
| 404           | Indicates the requested deployment was not found. |

**Success response body:**

```
1
2
3
4
5
6
7
8{
  "id": "10",
  "name": "activiti-examples.bar",
  "deploymentTime": "2010-10-13T14:54:26.750+02:00",
  "category": "examples",
  "url": "http://localhost:8081/service/repository/deployments/10",
  "tenantId" : null
}
```

#### 13.2.3. Create a new deployment

```
POST repository/deployments
```

**Request body:**

The request body should contain data of type *multipart/form-data*. There should be exactly one file in the request, any additional files will be ignored. The deployment name is the name of the file-field passed in. If multiple resources need to be deployed in a single deployment, compress the resources in a zip and make sure the file-name ends with `.bar` or `.zip`.

An additional parameter (form-field) can be passed in the request body with name `tenantId`. The value of this field will be used as the id of the tenant this deployment is done in.

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 201           | Indicates the deployment was created.                        |
| 400           | Indicates there was no content present in the request body or the content mime-type is not supported for deployment. The status-description contains additional information. |

**Success response body:**

```
1
2
3
4
5
6
7
8{
  "id": "10",
  "name": "activiti-examples.bar",
  "deploymentTime": "2010-10-13T14:54:26.750+02:00",
  "category": null,
  "url": "http://localhost:8081/service/repository/deployments/10",
  "tenantId" : "myTenant"
}
```

#### 13.2.4. Delete a deployment

```
DELETE repository/deployments/{deploymentId}
```

| Parameter    | Required | Value  | Description                         |
| ------------ | -------- | ------ | ----------------------------------- |
| deploymentId | Yes      | String | The id of the deployment to delete. |

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 204           | Indicates the deployment was found and has been deleted. Response-body is intentionally empty. |
| 404           | Indicates the requested deployment was not found.            |

#### 13.2.5. List resources in a deployment

```
GET repository/deployments/{deploymentId}/resources
```

| Parameter    | Required | Value  | Description                                        |
| ------------ | -------- | ------ | -------------------------------------------------- |
| deploymentId | Yes      | String | The id of the deployment to get the resources for. |

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates the deployment was found and the resource list has been returned. |
| 404           | Indicates the requested deployment was not found.            |

**Success response body:**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16[
  {
    "id": "diagrams/my-process.bpmn20.xml",
    "url": "http://localhost:8081/activiti-rest/service/repository/deployments/10/resources/diagrams%2Fmy-process.bpmn20.xml",
    "contentUrl": "http://localhost:8081/activiti-rest/service/repository/deployments/10/resourcedata/diagrams%2Fmy-process.bpmn20.xml",
    "mediaType": "text/xml",
    "type": "processDefinition"
  },
  {
    "id": "image.png",
    "url": "http://localhost:8081/activiti-rest/service/repository/deployments/10/resources/image.png",
    "contentUrl": "http://localhost:8081/activiti-rest/service/repository/deployments/10/resourcedata/image.png",
    "mediaType": "image/png",
    "type": "resource"
  }
]
```

- `mediaType`: Contains the media-type the resource has. This is resolved using a (pluggable) `MediaTypeResolver` and contains, by default, a limited number of mime-type mappings.
- `type`: Type of resource, possible values:
- `resource`: Plain old resource.
- `processDefinition`: Resource that contains one or more process-definitions. This resource is picked up by the deployer.
- `processImage`: Resource that represents a deployed process definition’s graphical layout.

*The contentUrl property in the resulting JSON for a single resource contains the actual URL to use for retrieving the binary resource.*

#### 13.2.6. Get a deployment resource

```
GET repository/deployments/{deploymentId}/resources/{resourceId}
```

| Parameter    | Required | Value  | Description                                                  |
| ------------ | -------- | ------ | ------------------------------------------------------------ |
| deploymentId | Yes      | String | The id of the deployment the requested resource is part of.  |
| resourceId   | Yes      | String | The id of the resource to get. **Make sure you URL-encode the resourceId in case it contains forward slashes. Eg: use diagrams%2Fmy-process.bpmn20.xml instead of diagrams/Fmy-process.bpmn20.xml.** |

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates both deployment and resource have been found and the resource has been returned. |
| 404           | Indicates the requested deployment was not found or there is no resource with the given id present in the deployment. The status-description contains additional information. |

**Success response body:**

```
1
2
3
4
5
6
7{
  "id": "diagrams/my-process.bpmn20.xml",
  "url": "http://localhost:8081/activiti-rest/service/repository/deployments/10/resources/diagrams%2Fmy-process.bpmn20.xml",
  "contentUrl": "http://localhost:8081/activiti-rest/service/repository/deployments/10/resourcedata/diagrams%2Fmy-process.bpmn20.xml",
  "mediaType": "text/xml",
  "type": "processDefinition"
}
```

- `mediaType`: Contains the media-type the resource has. This is resolved using a (pluggable) `MediaTypeResolver` and contains, by default, a limited number of mime-type mappings.
- `type`: Type of resource, possible values:
- `resource`: Plain old resource.
- `processDefinition`: Resource that contains one or more process-definitions. This resource is picked up by the deployer.
- `processImage`: Resource that represents a deployed process definition’s graphical layout.

#### 13.2.7. Get a deployment resource content

```
GET repository/deployments/{deploymentId}/resourcedata/{resourceId}
```

| Parameter    | Required | Value  | Description                                                  |
| ------------ | -------- | ------ | ------------------------------------------------------------ |
| deploymentId | Yes      | String | The id of the deployment the requested resource is part of.  |
| resourceId   | Yes      | String | The id of the resource to get the data for. **Make sure you URL-encode the resourceId in case it contains forward slashes. Eg: usediagrams%2Fmy-process.bpmn20.xml instead of diagrams/Fmy-process.bpmn20.xml.** |

```
.Get a deployment resource content - Response codes
```

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates both deployment and resource have been found and the resource data has been returned. |
| 404           | Indicates the requested deployment was not found or there is no resource with the given id present in the deployment. The status-description contains additional information. |

**Success response body:**

The response body will contain the binary resource-content for the requested resource. The response content-type will be the same as the type returned in the resources *mimeType* property. Also, a content-disposition header is set, allowing browsers to download the file instead of displaying it.

### 13.3. Process Definitions

#### 13.3.1. List of process definitions

```
GET repository/process-definitions
```

| Parameter         | Required | Value                                                        | Description                                                  |
| ----------------- | -------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| version           | No       | integer                                                      | Only return process definitions with the given version.      |
| name              | No       | String                                                       | Only return process definitions with the given name.         |
| nameLike          | No       | String                                                       | Only return process definitions with a name like the given name. |
| key               | No       | String                                                       | Only return process definitions with the given key.          |
| keyLike           | No       | String                                                       | Only return process definitions with a name like the given key. |
| resourceName      | No       | String                                                       | Only return process definitions with the given resource name. |
| resourceNameLike  | No       | String                                                       | Only return process definitions with a name like the given resource name. |
| category          | No       | String                                                       | Only return process definitions with the given category.     |
| categoryLike      | No       | String                                                       | Only return process definitions with a category like the given name. |
| categoryNotEquals | No       | String                                                       | Only return process definitions which don’t have the given category. |
| deploymentId      | No       | String                                                       | Only return process definitions which are part of a deployment with the given id. |
| startableByUser   | No       | String                                                       | Only return process definitions which can be started by the given user. |
| latest            | No       | Boolean                                                      | Only return the latest process definition versions. Can only be used together with *key* and *keyLike* parameters, using any other parameter will result in a 400-response. |
| suspended         | No       | Boolean                                                      | If `true`, only returns process definitions which are suspended. If `false`, only active process definitions (which are not suspended) are returned. |
| sort              | No       | *name* (default), *id*, *key*, *category*, *deploymentId* and *version* | Property to sort on, to be used together with the *order*.   |

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates request was successful and the process-definitions are returned |
| 400           | Indicates a parameter was passed in the wrong format or that *latest* is used with other parameters other than *key* and *keyLike*. The status-message contains additional information. |

**Success response body:**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25{
  "data": [
    {
      "id" : "oneTaskProcess:1:4",
      "url" : "http://localhost:8182/repository/process-definitions/oneTaskProcess%3A1%3A4",
      "version" : 1,
      "key" : "oneTaskProcess",
      "category" : "Examples",
      "suspended" : false,
      "name" : "The One Task Process",
      "description" : "This is a process for testing purposes",
      "deploymentId" : "2",
      "deploymentUrl" : "http://localhost:8081/repository/deployments/2",
      "graphicalNotationDefined" : true,
      "resource" : "http://localhost:8182/repository/deployments/2/resources/testProcess.xml",
      "diagramResource" : "http://localhost:8182/repository/deployments/2/resources/testProcess.png",
      "startFormDefined" : false
    }
  ],
  "total": 1,
  "start": 0,
  "sort": "name",
  "order": "asc",
  "size": 1
}
```

- `graphicalNotationDefined`: Indicates the process definition contains graphical information (BPMN DI).
- `resource`: Contains the actual deployed BPMN 2.0 xml.
- `diagramResource`: Contains a graphical representation of the process, null when no diagram is available.

#### 13.3.2. Get a process definition

```
GET repository/process-definitions/{processDefinitionId}
```

| Parameter           | Required | Value  | Description                              |
| ------------------- | -------- | ------ | ---------------------------------------- |
| processDefinitionId | Yes      | String | The id of the process definition to get. |

| Response code | Description                                               |
| ------------- | --------------------------------------------------------- |
| 200           | Indicates the process definition was found and returned.  |
| 404           | Indicates the requested process definition was not found. |

**Success response body:**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16{
  "id" : "oneTaskProcess:1:4",
  "url" : "http://localhost:8182/repository/process-definitions/oneTaskProcess%3A1%3A4",
  "version" : 1,
  "key" : "oneTaskProcess",
  "category" : "Examples",
  "suspended" : false,
  "name" : "The One Task Process",
  "description" : "This is a process for testing purposes",
  "deploymentId" : "2",
  "deploymentUrl" : "http://localhost:8081/repository/deployments/2",
  "graphicalNotationDefined" : true,
  "resource" : "http://localhost:8182/repository/deployments/2/resources/testProcess.xml",
  "diagramResource" : "http://localhost:8182/repository/deployments/2/resources/testProcess.png",
  "startFormDefined" : false
}
```

- `graphicalNotationDefined`: Indicates the process definition contains graphical information (BPMN DI).
- `resource`: Contains the actual deployed BPMN 2.0 xml.
- `diagramResource`: Contains a graphical representation of the process, null when no diagram is available.

#### 13.3.3. Update category for a process definition

```
PUT repository/process-definitions/{processDefinitionId}
```

**Body JSON:**

```
1
2
3{
  "category" : "updatedcategory"
}
```

| Response code | Description                                               |
| ------------- | --------------------------------------------------------- |
| 200           | Indicates the process was category was altered.           |
| 400           | Indicates no category was defined in the request body.    |
| 404           | Indicates the requested process definition was not found. |

**Success response body:** see response for `repository/process-definitions/{processDefinitionId}`.

#### 13.3.4. Get a process definition resource content

```
GET repository/process-definitions/{processDefinitionId}/resourcedata
```

| Parameter           | Required | Value  | Description                                                  |
| ------------------- | -------- | ------ | ------------------------------------------------------------ |
| processDefinitionId | Yes      | String | The id of the process definition to get the resource data for. |

**Response:**

Exactly the same response codes/boy as `GET repository/deployment/{deploymentId}/resourcedata/{resourceId}`.

#### 13.3.5. Get a process definition BPMN model

```
GET repository/process-definitions/{processDefinitionId}/model
```

| Parameter           | Required | Value  | Description                                            |
| ------------------- | -------- | ------ | ------------------------------------------------------ |
| processDefinitionId | Yes      | String | The id of the process definition to get the model for. |

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates the process definition was found and the model is returned. |
| 404           | Indicates the requested process definition was not found.    |

**Response body:** The response body is a JSON representation of the `org.activiti.bpmn.model.BpmnModel` and contains the full process definition model.

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15{
   "processes":[
      {
         "id":"oneTaskProcess",
         "xmlRowNumber":7,
         "xmlColumnNumber":60,
         "extensionElements":{

         },
         "name":"The One Task Process",
         "executable":true,
         "documentation":"One task process description",

    ]
}
```

#### 13.3.6. Suspend a process definition

```
PUT repository/process-definitions/{processDefinitionId}
```

**Body JSON:**

```
1
2
3
4
5{
  "action" : "suspend",
  "includeProcessInstances" : "false",
  "date" : "2013-04-15T00:42:12Z"
}
```

| Parameter               | Description                                                  | Required |
| ----------------------- | ------------------------------------------------------------ | -------- |
| action                  | Action to perform. Either `activate` or `suspend`.           | Yes      |
| includeProcessInstances | Whether or not to suspend/activate running process-instances for this process-definition. If omitted, the process-instances are left in the state they are. | No       |
| date                    | Date (ISO-8601) when the suspension/activation should be executed. If omitted, the suspend/activation is effective immediately. | No       |

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates the process was suspended.                         |
| 404           | Indicates the requested process definition was not found.    |
| 409           | Indicates the requested process definition is already suspended. |

**Success response body:** see response for `repository/process-definitions/{processDefinitionId}`.

#### 13.3.7. Activate a process definition

```
PUT repository/process-definitions/{processDefinitionId}
```

**Body JSON:**

```
1
2
3
4
5{
  "action" : "activate",
  "includeProcessInstances" : "true",
  "date" : "2013-04-15T00:42:12Z"
}
```

See suspend process definition [JSON Body parameters](https://www.activiti.org/userguide/#processDefinitionActionBodyParameters).

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates the process was activated.                         |
| 404           | Indicates the requested process definition was not found.    |
| 409           | Indicates the requested process definition is already active. |

**Success response body:** see response for `repository/process-definitions/{processDefinitionId}`.

#### 13.3.8. Get all candidate starters for a process-definition

```
GET repository/process-definitions/{processDefinitionId}/identitylinks
```

| Parameter           | Required | Value  | Description                                                  |
| ------------------- | -------- | ------ | ------------------------------------------------------------ |
| processDefinitionId | Yes      | String | The id of the process definition to get the identity links for. |

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates the process definition was found and the requested identity links are returned. |
| 404           | Indicates the requested process definition was not found.    |

**Success response body:**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14[
   {
      "url":"http://localhost:8182/repository/process-definitions/oneTaskProcess%3A1%3A4/identitylinks/groups/admin",
      "user":null,
      "group":"admin",
      "type":"candidate"
   },
   {
      "url":"http://localhost:8182/repository/process-definitions/oneTaskProcess%3A1%3A4/identitylinks/users/kermit",
      "user":"kermit",
      "group":null,
      "type":"candidate"
   }
]
```

#### 13.3.9. Add a candidate starter to a process definition

```
POST repository/process-definitions/{processDefinitionId}/identitylinks
```

| Parameter           | Required | Value  | Description                       |
| ------------------- | -------- | ------ | --------------------------------- |
| processDefinitionId | Yes      | String | The id of the process definition. |

**Request body (user):**

```
1
2
3{
  "user" : "kermit"
}
```

**Request body (group):**

```
1
2
3{
  "groupId" : "sales"
}
```

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 201           | Indicates the process definition was found and the identity link was created. |
| 404           | Indicates the requested process definition was not found.    |

**Success response body:**

```
1
2
3
4
5
6{
  "url":"http://localhost:8182/repository/process-definitions/oneTaskProcess%3A1%3A4/identitylinks/users/kermit",
  "user":"kermit",
  "group":null,
  "type":"candidate"
}
```

#### 13.3.10. Delete a candidate starter from a process definition

```
DELETE repository/process-definitions/{processDefinitionId}/identitylinks/{family}/{identityId}
```

| Parameter           | Required | Value  | Description                                                  |
| ------------------- | -------- | ------ | ------------------------------------------------------------ |
| processDefinitionId | Yes      | String | The id of the process definition.                            |
| family              | Yes      | String | Either `users` or `groups`, depending on the type of identity link. |
| identityId          | Yes      | String | Either the userId or groupId of the identity to remove as candidate starter. |

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 204           | Indicates the process definition was found and the identity link was removed. The response body is intentionally empty. |
| 404           | Indicates the requested process definition was not found or the process definition doesn’t have an identity-link that matches the url. |

**Success response body:**

```
1
2
3
4
5
6{
  "url":"http://localhost:8182/repository/process-definitions/oneTaskProcess%3A1%3A4/identitylinks/users/kermit",
  "user":"kermit",
  "group":null,
  "type":"candidate"
}
```

#### 13.3.11. Get a candidate starter from a process definition

```
GET repository/process-definitions/{processDefinitionId}/identitylinks/{family}/{identityId}
```

| Parameter           | Required | Value  | Description                                                  |
| ------------------- | -------- | ------ | ------------------------------------------------------------ |
| processDefinitionId | Yes      | String | The id of the process definition.                            |
| family              | Yes      | String | Either `users` or `groups`, depending on the type of identity link. |
| identityId          | Yes      | String | Either the userId or groupId of the identity to get as candidate starter. |

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates the process definition was found and the identity link was returned. |
| 404           | Indicates the requested process definition was not found or the process definition doesn’t have an identity-link that matches the url. |

**Success response body:**

```
1
2
3
4
5
6{
  "url":"http://localhost:8182/repository/process-definitions/oneTaskProcess%3A1%3A4/identitylinks/users/kermit",
  "user":"kermit",
  "group":null,
  "type":"candidate"
}
```

### 13.4. Models

#### 13.4.1. Get a list of models

```
GET repository/models
```

| Parameter         | Required | Value                                                        | Description                                                  |
| ----------------- | -------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| id                | No       | String                                                       | Only return models with the given id.                        |
| category          | No       | String                                                       | Only return models with the given category.                  |
| categoryLike      | No       | String                                                       | Only return models with a category like the given value. Use the `%` character as wildcard. |
| categoryNotEquals | No       | String                                                       | Only return models without the given category.               |
| name              | No       | String                                                       | Only return models with the given name.                      |
| nameLike          | No       | String                                                       | Only return models with a name like the given value. Use the `%`character as wildcard. |
| key               | No       | String                                                       | Only return models with the given key.                       |
| deploymentId      | No       | String                                                       | Only return models which are deployed in the given deployment. |
| version           | No       | Integer                                                      | Only return models with the given version.                   |
| latestVersion     | No       | Boolean                                                      | If `true`, only return models which are the latest version. Best used in combination with `key`. If `false` is passed in as value, this is ignored and all versions are returned. |
| deployed          | No       | Boolean                                                      | If `true`, only deployed models are returned. If `false`, only undeployed models are returned (deploymentId is null). |
| tenantId          | No       | String                                                       | Only return models with the given tenantId.                  |
| tenantIdLike      | No       | String                                                       | Only return models with a tenantId like the given value.     |
| withoutTenantId   | No       | Boolean                                                      | If `true`, only returns models without a tenantId set. If `false`, the `withoutTenantId` parameter is ignored. |
| sort              | No       | *id* (default), *category*, *createTime*, *key*, *lastUpdateTime*, *name*, *version* or *tenantId* | Property to sort on, to be used together with the *order*.   |

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates request was successful and the models are returned |
| 400           | Indicates a parameter was passed in the wrong format. The status-message contains additional information. |

**Success response body:**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26{
   "data":[
      {
         "name":"Model name",
         "key":"Model key",
         "category":"Model category",
         "version":2,
         "metaInfo":"Model metainfo",
         "deploymentId":"7",
         "id":"10",
         "url":"http://localhost:8182/repository/models/10",
         "createTime":"2013-06-12T14:31:08.612+0000",
         "lastUpdateTime":"2013-06-12T14:31:08.612+0000",
         "deploymentUrl":"http://localhost:8182/repository/deployments/7",
         "tenantId":null
      },

      ...

   ],
   "total":2,
   "start":0,
   "sort":"id",
   "order":"asc",
   "size":2
}
```

#### 13.4.2. Get a model

```
GET repository/models/{modelId}
```

| Parameter | Required | Value  | Description                 |
| --------- | -------- | ------ | --------------------------- |
| modelId   | Yes      | String | The id of the model to get. |

| Response code | Description                                  |
| ------------- | -------------------------------------------- |
| 200           | Indicates the model was found and returned.  |
| 404           | Indicates the requested model was not found. |

**Success response body:**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14{
   "id":"5",
   "url":"http://localhost:8182/repository/models/5",
   "name":"Model name",
   "key":"Model key",
   "category":"Model category",
   "version":2,
   "metaInfo":"Model metainfo",
   "deploymentId":"2",
   "deploymentUrl":"http://localhost:8182/repository/deployments/2",
   "createTime":"2013-06-12T12:31:19.861+0000",
   "lastUpdateTime":"2013-06-12T12:31:19.861+0000",
   "tenantId":null
}
```

#### 13.4.3. Update a model

```
PUT repository/models/{modelId}
```

**Request body:**

```
1
2
3
4
5
6
7
8
9{
   "name":"Model name",
   "key":"Model key",
   "category":"Model category",
   "version":2,
   "metaInfo":"Model metainfo",
   "deploymentId":"2",
   "tenantId":"updatedTenant"
}
```

All request values are optional. For example, you can only include the *name* attribute in the request body JSON-object, only updating the name of the model, leaving all other fields unaffected. When an attribute is explicitly included and is set to null, the model-value will be updated to null. Example: `{"metaInfo" : null}` will clear the metaInfo of the model).

| Response code | Description                                  |
| ------------- | -------------------------------------------- |
| 200           | Indicates the model was found and updated.   |
| 404           | Indicates the requested model was not found. |

**Success response body:**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14{
   "id":"5",
   "url":"http://localhost:8182/repository/models/5",
   "name":"Model name",
   "key":"Model key",
   "category":"Model category",
   "version":2,
   "metaInfo":"Model metainfo",
   "deploymentId":"2",
   "deploymentUrl":"http://localhost:8182/repository/deployments/2",
   "createTime":"2013-06-12T12:31:19.861+0000",
   "lastUpdateTime":"2013-06-12T12:31:19.861+0000",
   "tenantId":""updatedTenant"
}
```

#### 13.4.4. Create a model

```
POST repository/models
```

**Request body:**

```
1
2
3
4
5
6
7
8
9{
   "name":"Model name",
   "key":"Model key",
   "category":"Model category",
   "version":1,
   "metaInfo":"Model metainfo",
   "deploymentId":"2",
   "tenantId":"tenant"
}
```

All request values are optional. For example, you can only include the *name* attribute in the request body JSON-object, only setting the name of the model, leaving all other fields null.

| Response code | Description                      |
| ------------- | -------------------------------- |
| 201           | Indicates the model was created. |

**Success response body:**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14{
   "id":"5",
   "url":"http://localhost:8182/repository/models/5",
   "name":"Model name",
   "key":"Model key",
   "category":"Model category",
   "version":1,
   "metaInfo":"Model metainfo",
   "deploymentId":"2",
   "deploymentUrl":"http://localhost:8182/repository/deployments/2",
   "createTime":"2013-06-12T12:31:19.861+0000",
   "lastUpdateTime":"2013-06-12T12:31:19.861+0000",
   "tenantId":"tenant"
}
```

#### 13.4.5. Delete a model

```
DELETE repository/models/{modelId}
```

| Parameter | Required | Value  | Description                    |
| --------- | -------- | ------ | ------------------------------ |
| modelId   | Yes      | String | The id of the model to delete. |

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 204           | Indicates the model was found and has been deleted. Response-body is intentionally empty. |
| 404           | Indicates the requested model was not found.                 |

#### 13.4.6. Get the editor source for a model

```
GET repository/models/{modelId}/source
```

| Parameter | Required | Value  | Description          |
| --------- | -------- | ------ | -------------------- |
| modelId   | Yes      | String | The id of the model. |

| Response code | Description                                           |
| ------------- | ----------------------------------------------------- |
| 200           | Indicates the model was found and source is returned. |
| 404           | Indicates the requested model was not found.          |

**Success response body:**

Response body contains the model’s raw editor source. The response’s content-type is set to `application/octet-stream`, regardless of the content of the source.

#### 13.4.7. Set the editor source for a model

```
PUT repository/models/{modelId}/source
```

| Parameter | Required | Value  | Description          |
| --------- | -------- | ------ | -------------------- |
| modelId   | Yes      | String | The id of the model. |

**Request body:**

The request should be of type `multipart/form-data`. There should be a single file-part included with the binary value of the source.

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates the model was found and the source has been updated. |
| 404           | Indicates the requested model was not found.                 |

**Success response body:**

Response body contains the model’s raw editor source. The response’s content-type is set to `application/octet-stream`, regardless of the content of the source.

#### 13.4.8. Get the extra editor source for a model

```
GET repository/models/{modelId}/source-extra
```

| Parameter | Required | Value  | Description          |
| --------- | -------- | ------ | -------------------- |
| modelId   | Yes      | String | The id of the model. |

| Response code | Description                                           |
| ------------- | ----------------------------------------------------- |
| 200           | Indicates the model was found and source is returned. |
| 404           | Indicates the requested model was not found.          |

**Success response body:**

Response body contains the model’s raw extra editor source. The response’s content-type is set to `application/octet-stream`, regardless of the content of the extra source.

#### 13.4.9. Set the extra editor source for a model

```
PUT repository/models/{modelId}/source-extra
```

| Parameter | Required | Value  | Description          |
| --------- | -------- | ------ | -------------------- |
| modelId   | Yes      | String | The id of the model. |

**Request body:**

The request should be of type `multipart/form-data`. There should be a single file-part included with the binary value of the extra source.

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates the model was found and the extra source has been updated. |
| 404           | Indicates the requested model was not found.                 |

**Success response body:**

Response body contains the model’s raw editor source. The response’s content-type is set to `application/octet-stream`, regardless of the content of the source.

### 13.5. Process Instances

#### 13.5.1. Get a process instance

```
GET runtime/process-instances/{processInstanceId}
```

| Parameter         | Required | Value  | Description                            |
| ----------------- | -------- | ------ | -------------------------------------- |
| processInstanceId | Yes      | String | The id of the process instance to get. |

| Response code | Description                                             |
| ------------- | ------------------------------------------------------- |
| 200           | Indicates the process instance was found and returned.  |
| 404           | Indicates the requested process instance was not found. |

**Success response body:**

```
1
2
3
4
5
6
7
8
9{
   "id":"7",
   "url":"http://localhost:8182/runtime/process-instances/7",
   "businessKey":"myBusinessKey",
   "suspended":false,
   "processDefinitionUrl":"http://localhost:8182/repository/process-definitions/processOne%3A1%3A4",
   "activityId":"processTask",
   "tenantId": null
}
```

#### 13.5.2. Delete a process instance

```
DELETE runtime/process-instances/{processInstanceId}
```

| Parameter         | Required | Value  | Description                               |
| ----------------- | -------- | ------ | ----------------------------------------- |
| processInstanceId | Yes      | String | The id of the process instance to delete. |

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 204           | Indicates the process instance was found and deleted. Response body is left empty intentionally. |
| 404           | Indicates the requested process instance was not found.      |

#### 13.5.3. Activate or suspend a process instance

```
PUT runtime/process-instances/{processInstanceId}
```

| Parameter         | Required | Value  | Description                                         |
| ----------------- | -------- | ------ | --------------------------------------------------- |
| processInstanceId | Yes      | String | The id of the process instance to activate/suspend. |

**Request response body (suspend):**

```
1
2
3{
   "action":"suspend"
}
```

**Request response body (activate):**

```
1
2
3{
   "action":"activate"
}
```

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates the process instance was found and action was executed. |
| 400           | Indicates an invalid action was supplied.                    |
| 404           | Indicates the requested process instance was not found.      |
| 409           | Indicates the requested process instance action cannot be executed since the process-instance is already activated/suspended. |

#### 13.5.4. Start a process instance

```
POST runtime/process-instances
```

**Request body (start by process definition id):**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10{
   "processDefinitionId":"oneTaskProcess:1:158",
   "businessKey":"myBusinessKey",
   "variables": [
      {
        "name":"myVar",
        "value":"This is a variable",
      }
   ]
}
```

**Request body (start by process definition key):**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11{
   "processDefinitionKey":"oneTaskProcess",
   "businessKey":"myBusinessKey",
   "tenantId": "tenant1",
   "variables": [
      {
        "name":"myVar",
        "value":"This is a variable",
      }
   ]
}
```

**Request body (start by message):**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11{
   "message":"newOrderMessage",
   "businessKey":"myBusinessKey",
   "tenantId": "tenant1",
   "variables": [
      {
        "name":"myVar",
        "value":"This is a variable",
      }
   ]
}
```

Note that also a *transientVariables* property is accepted as part of this json, that follows the same structure as the *variables* property.

Only one of `processDefinitionId`, `processDefinitionKey` or `message` can be used in the request body. Parameters `businessKey`, `variables` and `tenantId` are optional. If `tenantId` is omitted, the default tenant will be used. More information about the variable format can be found in [the REST variables section](https://www.activiti.org/userguide/#restVariables). Note that the variable-scope that is supplied is ignored, process-variables are always `local`.

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 201           | Indicates the process instance was created.                  |
| 400           | Indicates either the process-definition was not found (based on id or key), no process is started by sending the given message or an invalid variable has been passed. Status description contains additional information about the error. |

**Success response body:**

```
1
2
3
4
5
6
7
8
9{
   "id":"7",
   "url":"http://localhost:8182/runtime/process-instances/7",
   "businessKey":"myBusinessKey",
   "suspended":false,
   "processDefinitionUrl":"http://localhost:8182/repository/process-definitions/processOne%3A1%3A4",
   "activityId":"processTask",
   "tenantId" : null
}
```

#### 13.5.5. List of process instances

```
GET runtime/process-instances
```

| Parameter               | Required | Value   | Description                                                  |
| ----------------------- | -------- | ------- | ------------------------------------------------------------ |
| id                      | No       | String  | Only return process instance with the given id.              |
| processDefinitionKey    | No       | String  | Only return process instances with the given process definition key. |
| processDefinitionId     | No       | String  | Only return process instances with the given process definition id. |
| businessKey             | No       | String  | Only return process instances with the given businessKey.    |
| involvedUser            | No       | String  | Only return process instances in which the given user is involved. |
| suspended               | No       | Boolean | If `true`, only return process instance which are suspended. If `false`, only return process instances which are not suspended (active). |
| superProcessInstanceId  | No       | String  | Only return process instances which have the given super process-instance id (for processes that have a call-activities). |
| subProcessInstanceId    | No       | String  | Only return process instances which have the given sub process-instance id (for processes started as a call-activity). |
| excludeSubprocesses     | No       | Boolean | Return only process instances which aren’t sub processes.    |
| includeProcessVariables | No       | Boolean | Indication to include process variables in the result.       |
| tenantId                | No       | String  | Only return process instances with the given tenantId.       |
| tenantIdLike            | No       | String  | Only return process instances with a tenantId like the given value. |
| withoutTenantId         | No       | Boolean | If `true`, only returns process instances without a tenantId set. If `false`, the `withoutTenantId` parameter is ignored. |
| sort                    | No       | String  | Sort field, should be either one of `id` (default), `processDefinitionId`, `tenantId` or `processDefinitionKey`. |

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates request was successful and the process-instances are returned |
| 400           | Indicates a parameter was passed in the wrong format . The status-message contains additional information. |

**Success response body:**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16
17
18
19
20{
   "data":[
      {
         "id":"7",
         "url":"http://localhost:8182/runtime/process-instances/7",
         "businessKey":"myBusinessKey",
         "suspended":false,
         "processDefinitionUrl":"http://localhost:8182/repository/process-definitions/processOne%3A1%3A4",
         "activityId":"processTask",
         "tenantId" : null
      }


   ],
   "total":2,
   "start":0,
   "sort":"id",
   "order":"asc",
   "size":2
}
```

#### 13.5.6. Query process instances

```
POST query/process-instances
```

**Request body:**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12{
  "processDefinitionKey":"oneTaskProcess",
  "variables":
  [
    {
        "name" : "myVariable",
        "value" : 1234,
        "operation" : "equals",
        "type" : "long"
    }
  ]
}
```

The request body can contain all possible filters that can be used in the [List process instances](https://www.activiti.org/userguide/#restProcessInstancesGet) URL query. On top of these, it’s possible to provide an array of variables to include in the query, with their format [described here](https://www.activiti.org/userguide/#restQueryVariable).

The general [paging and sorting query-parameters](https://www.activiti.org/userguide/#restPagingAndSort) can be used for this URL.

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates request was successful and the process-instances are returned |
| 400           | Indicates a parameter was passed in the wrong format . The status-message contains additional information. |

**Success response body:**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16
17
18
19
20{
   "data":[
      {
         "id":"7",
         "url":"http://localhost:8182/runtime/process-instances/7",
         "businessKey":"myBusinessKey",
         "suspended":false,
         "processDefinitionUrl":"http://localhost:8182/repository/process-definitions/processOne%3A1%3A4",
         "activityId":"processTask",
         "tenantId" : null
      }


   ],
   "total":2,
   "start":0,
   "sort":"id",
   "order":"asc",
   "size":2
}
```

#### 13.5.7. Get diagram for a process instance

```
GET runtime/process-instances/{processInstanceId}/diagram
```

| Parameter         | Required | Value  | Description                                            |
| ----------------- | -------- | ------ | ------------------------------------------------------ |
| processInstanceId | Yes      | String | The id of the process instance to get the diagram for. |

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates the process instance was found and the diagram was returned. |
| 400           | Indicates the requested process instance was not found but the process doesn’t contain any graphical information (BPMN:DI) and no diagram can be created. |
| 404           | Indicates the requested process instance was not found.      |

**Success response body:**

```
1
2
3
4
5
6
7
8{
   "id":"7",
   "url":"http://localhost:8182/runtime/process-instances/7",
   "businessKey":"myBusinessKey",
   "suspended":false,
   "processDefinitionUrl":"http://localhost:8182/repository/process-definitions/processOne%3A1%3A4",
   "activityId":"processTask"
}
```

#### 13.5.8. Get involved people for process instance

```
GET runtime/process-instances/{processInstanceId}/identitylinks
```

| Parameter         | Required | Value  | Description                                      |
| ----------------- | -------- | ------ | ------------------------------------------------ |
| processInstanceId | Yes      | String | The id of the process instance to the links for. |

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates the process instance was found and links are returned. |
| 404           | Indicates the requested process instance was not found.      |

**Success response body:**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14[
   {
      "url":"http://localhost:8182/runtime/process-instances/5/identitylinks/users/john/customType",
      "user":"john",
      "group":null,
      "type":"customType"
   },
   {
      "url":"http://localhost:8182/runtime/process-instances/5/identitylinks/users/paul/candidate",
      "user":"paul",
      "group":null,
      "type":"candidate"
   }
]
```

Note that the `groupId` will always be null, as it’s only possible to involve users with a process-instance.

#### 13.5.9. Add an involved user to a process instance

```
POST runtime/process-instances/{processInstanceId}/identitylinks
```

| Parameter         | Required | Value  | Description                                      |
| ----------------- | -------- | ------ | ------------------------------------------------ |
| processInstanceId | Yes      | String | The id of the process instance to the links for. |

**Request body:**

```
1
2
3
4{
  "userId":"kermit",
  "type":"participant"
}
```

Both `userId` and `type` are required.

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 201           | Indicates the process instance was found and the link is created. |
| 400           | Indicates the requested body did not contain a userId or a type. |
| 404           | Indicates the requested process instance was not found.      |

**Success response body:**

```
1
2
3
4
5
6{
   "url":"http://localhost:8182/runtime/process-instances/5/identitylinks/users/john/customType",
   "user":"john",
   "group":null,
   "type":"customType"
}
```

Note that the `groupId` will always be null, as it’s only possible to involve users with a process-instance.

#### 13.5.10. Remove an involved user to from process instance

```
DELETE runtime/process-instances/{processInstanceId}/identitylinks/users/{userId}/{type}
```

| Parameter         | Required | Value  | Description                            |
| ----------------- | -------- | ------ | -------------------------------------- |
| processInstanceId | Yes      | String | The id of the process instance.        |
| userId            | Yes      | String | The id of the user to delete link for. |
| type              | Yes      | String | Type of link to delete.                |

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 204           | Indicates the process instance was found and the link has been deleted. Response body is left empty intentionally. |
| 404           | Indicates the requested process instance was not found or the link to delete doesn’t exist. The response status contains additional information about the error. |

**Success response body:**

```
1
2
3
4
5
6{
   "url":"http://localhost:8182/runtime/process-instances/5/identitylinks/users/john/customType",
   "user":"john",
   "group":null,
   "type":"customType"
}
```

Note that the `groupId` will always be null, as it’s only possible to involve users with a process-instance.

#### 13.5.11. List of variables for a process instance

```
GET runtime/process-instances/{processInstanceId}/variables
```

| Parameter         | Required | Value  | Description                                          |
| ----------------- | -------- | ------ | ---------------------------------------------------- |
| processInstanceId | Yes      | String | The id of the process instance to the variables for. |

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates the process instance was found and variables are returned. |
| 404           | Indicates the requested process instance was not found.      |

**Success response body:**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15[
   {
      "name":"intProcVar",
      "type":"integer",
      "value":123,
      "scope":"local"
   },
   {
      "name":"byteArrayProcVar",
      "type":"binary",
      "value":null,
      "valueUrl":"http://localhost:8182/runtime/process-instances/5/variables/byteArrayProcVar/data",
      "scope":"local"
   }
]
```

In case the variable is a binary variable or serializable, the `valueUrl` points to an URL to fetch the raw value. If it’s a plain variable, the value is present in the response. Note that only `local` scoped variables are returned, as there is no `global` scope for process-instance variables.

#### 13.5.12. Get a variable for a process instance

```
GET runtime/process-instances/{processInstanceId}/variables/{variableName}
```

| Parameter         | Required | Value  | Description                                          |
| ----------------- | -------- | ------ | ---------------------------------------------------- |
| processInstanceId | Yes      | String | The id of the process instance to the variables for. |
| variableName      | Yes      | String | Name of the variable to get.                         |

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates both the process instance and variable were found and variable is returned. |
| 400           | Indicates the request body is incomplete or contains illegal values. The status description contains additional information about the error. |
| 404           | Indicates the requested process instance was not found or the process instance does not have a variable with the given name. Status description contains additional information about the error. |

**Success response body:**

```
1
2
3
4
5
6   {
      "name":"intProcVar",
      "type":"integer",
      "value":123,
      "scope":"local"
   }
```

In case the variable is a binary variable or serializable, the `valueUrl` points to an URL to fetch the raw value. If it’s a plain variable, the value is present in the response. Note that only `local` scoped variables are returned, as there is no `global` scope for process-instance variables.

#### 13.5.13. Create (or update) variables on a process instance

```
POST runtime/process-instances/{processInstanceId}/variables
PUT runtime/process-instances/{processInstanceId}/variables
```

When using `POST`, all variables that are passed are created. In case one of the variables already exists on the process instance, the request results in an error (409 - CONFLICT). When `PUT` is used, nonexistent variables are created on the process-instance and existing ones are overridden without any error.

| Parameter         | Required | Value  | Description                                          |
| ----------------- | -------- | ------ | ---------------------------------------------------- |
| processInstanceId | Yes      | String | The id of the process instance to the variables for. |

**Request body:**

```
[
   {
      "name":"intProcVar"
      "type":"integer"
      "value":123
   },

   ...
]
```

Any number of variables can be passed into the request body array. More information about the variable format can be found in [the REST variables section](https://www.activiti.org/userguide/#restVariables). Note that scope is ignored, only `local` variables can be set in a process instance.

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 201           | Indicates the process instance was found and variable is created. |
| 400           | Indicates the request body is incomplete or contains illegal values. The status description contains additional information about the error. |
| 404           | Indicates the requested process instance was not found.      |
| 409           | Indicates the process instance was found but already contains a variable with the given name (only thrown when POST method is used). Use the update-method instead. |

**Success response body:**

```
[
   {
      "name":"intProcVar",
      "type":"integer",
      "value":123,
      "scope":"local"
   },

   ...

]
```

#### 13.5.14. Update a single variable on a process instance

```
PUT runtime/process-instances/{processInstanceId}/variables/{variableName}
```

| Parameter         | Required | Value  | Description                                          |
| ----------------- | -------- | ------ | ---------------------------------------------------- |
| processInstanceId | Yes      | String | The id of the process instance to the variables for. |
| variableName      | Yes      | String | Name of the variable to get.                         |

**Request body:**

```
1
2
3
4
5 {
    "name":"intProcVar"
    "type":"integer"
    "value":123
 }
```

More information about the variable format can be found in [the REST variables section](https://www.activiti.org/userguide/#restVariables). Note that scope is ignored, only `local` variables can be set in a process instance.

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates both the process instance and variable were found and variable is updated. |
| 404           | Indicates the requested process instance was not found or the process instance does not have a variable with the given name. Status description contains additional information about the error. |

**Success response body:**

```
1
2
3
4
5
6{
  "name":"intProcVar",
  "type":"integer",
  "value":123,
  "scope":"local"
}
```

In case the variable is a binary variable or serializable, the `valueUrl` points to an URL to fetch the raw value. If it’s a plain variable, the value is present in the response. Note that only `local` scoped variables are returned, as there is no `global` scope for process-instance variables.

#### 13.5.15. Create a new binary variable on a process-instance

```
POST runtime/process-instances/{processInstanceId}/variables
```

| Parameter         | Required | Value  | Description                                                  |
| ----------------- | -------- | ------ | ------------------------------------------------------------ |
| processInstanceId | Yes      | String | The id of the process instance to create the new variable for. |

**Request body:**

The request should be of type `multipart/form-data`. There should be a single file-part included with the binary value of the variable. On top of that, the following additional form-fields can be present:

- `name`: Required name of the variable.
- `type`: Type of variable that is created. If omitted, `binary` is assumed and the binary data in the request will be stored as an array of bytes.

**Success response body:**

```
1
2
3
4
5
6
7{
  "name" : "binaryVariable",
  "scope" : "local",
  "type" : "binary",
  "value" : null,
  "valueUrl" : "http://.../runtime/process-instances/123/variables/binaryVariable/data"
}
```

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 201           | Indicates the variable was created and the result is returned. |
| 400           | Indicates the name of the variable to create was missing. Status message provides additional information. |
| 404           | Indicates the requested process instance was not found.      |
| 409           | Indicates the process instance already has a variable with the given name. Use the PUT method to update the task variable instead. |
| 415           | Indicates the serializable data contains an object for which no class is present in the JVM running the Activiti engine and therefore cannot be deserialized. |

#### 13.5.16. Update an existing binary variable on a process-instance

```
PUT runtime/process-instances/{processInstanceId}/variables
```

| Parameter         | Required | Value  | Description                                                  |
| ----------------- | -------- | ------ | ------------------------------------------------------------ |
| processInstanceId | Yes      | String | The id of the process instance to create the new variable for. |

**Request body:** The request should be of type `multipart/form-data`. There should be a single file-part included with the binary value of the variable. On top of that, the following additional form-fields can be present:

- `name`: Required name of the variable.
- `type`: Type of variable that is created. If omitted, `binary` is assumed and the binary data in the request will be stored as an array of bytes.

**Success response body:**

```
1
2
3
4
5
6
7{
  "name" : "binaryVariable",
  "scope" : "local",
  "type" : "binary",
  "value" : null,
  "valueUrl" : "http://.../runtime/process-instances/123/variables/binaryVariable/data"
}
```

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates the variable was updated and the result is returned. |
| 400           | Indicates the name of the variable to update was missing. Status message provides additional information. |
| 404           | Indicates the requested process instance was not found or the process instance does not have a variable with the given name. |
| 415           | Indicates the serializable data contains an object for which no class is present in the JVM running the Activiti engine and therefore cannot be deserialized. |

### 13.6. Executions

#### 13.6.1. Get an execution

```
GET runtime/executions/{executionId}
```

| Parameter   | Required | Value  | Description                     |
| ----------- | -------- | ------ | ------------------------------- |
| executionId | Yes      | String | The id of the execution to get. |

| Response code | Description                                     |
| ------------- | ----------------------------------------------- |
| 200           | Indicates the execution was found and returned. |
| 404           | Indicates the execution was not found.          |

**Success response body:**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11{
   "id":"5",
   "url":"http://localhost:8182/runtime/executions/5",
   "parentId":null,
   "parentUrl":null,
   "processInstanceId":"5",
   "processInstanceUrl":"http://localhost:8182/runtime/process-instances/5",
   "suspended":false,
   "activityId":null,
   "tenantId": null
}
```

#### 13.6.2. Execute an action on an execution

```
PUT runtime/executions/{executionId}
```

| Parameter   | Required | Value  | Description                                   |
| ----------- | -------- | ------ | --------------------------------------------- |
| executionId | Yes      | String | The id of the execution to execute action on. |

**Request body (signal an execution):**

```
1
2
3{
  "action":"signal"
}
```

Both a *variables* and *transientVariables* property is accepted with following structure:

```
1
2
3
4
5
6
7
8
9{
  "action":"signal",
  "variables" : [
    {
      "name": "myVar",
      "value": "someValue"
    }
  ]
}
```

**Request body (signal event received for execution):**

```
1
2
3
4
5{
  "action":"signalEventReceived",
  "signalName":"mySignal"
  "variables": [  ]
}
```

Notifies the execution that a signal event has been received, requires a `signalName` parameter. Optional `variables` can be passed that are set on the execution before the action is executed.

**Request body (signal event received for execution):**

```
1
2
3
4
5{
  "action":"messageEventReceived",
  "messageName":"myMessage"
  "variables": [  ]
}
```

Notifies the execution that a message event has been received, requires a `messageName` parameter. Optional `variables` can be passed that are set on the execution before the action is executed.

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates the execution was found and the action is performed. |
| 204           | Indicates the execution was found, the action was performed and the action caused the execution to end. |
| 400           | Indicates an illegal action was requested, required parameters are missing in the request body or illegal variables are passed in. Status description contains additional information about the error. |
| 404           | Indicates the execution was not found.                       |

**Success response body (in case execution is not ended due to action):**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11{
   "id":"5",
   "url":"http://localhost:8182/runtime/executions/5",
   "parentId":null,
   "parentUrl":null,
   "processInstanceId":"5",
   "processInstanceUrl":"http://localhost:8182/runtime/process-instances/5",
   "suspended":false,
   "activityId":null,
   "tenantId" : null
}
```

#### 13.6.3. Get active activities in an execution

```
GET runtime/executions/{executionId}/activities
```

Returns all activities which are active in the execution and in all child-executions (and their children, recursively), if any.

| Parameter   | Required | Value  | Description                                    |
| ----------- | -------- | ------ | ---------------------------------------------- |
| executionId | Yes      | String | The id of the execution to get activities for. |

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates the execution was found and activities are returned. |
| 404           | Indicates the execution was not found.                       |

**Success response body:**

```
1
2
3
4[
  "userTaskForManager",
  "receiveTask"
]
```

#### 13.6.4. List of executions

```
GET runtime/executions
```

| Parameter                    | Required | Value   | Description                                                  |
| ---------------------------- | -------- | ------- | ------------------------------------------------------------ |
| id                           | No       | String  | Only return executions with the given id.                    |
| activityId                   | No       | String  | Only return executions with the given activity id.           |
| processDefinitionKey         | No       | String  | Only return executions with the given process definition key. |
| processDefinitionId          | No       | String  | Only return executions with the given process definition id. |
| processInstanceId            | No       | String  | Only return executions which are part of the process instance with the given id. |
| messageEventSubscriptionName | No       | String  | Only return executions which are subscribed to a message with the given name. |
| signalEventSubscriptionName  | No       | String  | Only return executions which are subscribed to a signal with the given name. |
| parentId                     | No       | String  | Only return executions which are a direct child of the given execution. |
| tenantId                     | No       | String  | Only return executions with the given tenantId.              |
| tenantIdLike                 | No       | String  | Only return executions with a tenantId like the given value. |
| withoutTenantId              | No       | Boolean | If `true`, only returns executions without a tenantId set. If `false`, the `withoutTenantId`parameter is ignored. |
| sort                         | No       | String  | Sort field, should be either one of `processInstanceId`(default), `processDefinitionId`, `processDefinitionKey` or `tenantId`. |

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates request was successful and the executions are returned |
| 400           | Indicates a parameter was passed in the wrong format . The status-message contains additional information. |

**Success response body:**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31{
   "data":[
      {
         "id":"5",
         "url":"http://localhost:8182/runtime/executions/5",
         "parentId":null,
         "parentUrl":null,
         "processInstanceId":"5",
         "processInstanceUrl":"http://localhost:8182/runtime/process-instances/5",
         "suspended":false,
         "activityId":null,
         "tenantId":null
      },
      {
         "id":"7",
         "url":"http://localhost:8182/runtime/executions/7",
         "parentId":"5",
         "parentUrl":"http://localhost:8182/runtime/executions/5",
         "processInstanceId":"5",
         "processInstanceUrl":"http://localhost:8182/runtime/process-instances/5",
         "suspended":false,
         "activityId":"processTask",
         "tenantId":null
      }
   ],
   "total":2,
   "start":0,
   "sort":"processInstanceId",
   "order":"asc",
   "size":2
}
```

#### 13.6.5. Query executions

```
POST query/executions
```

**Request body:**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16
17
18
19
20
21{
  "processDefinitionKey":"oneTaskProcess",
  "variables":
  [
    {
        "name" : "myVariable",
        "value" : 1234,
        "operation" : "equals",
        "type" : "long"
    }
  ],
  "processInstanceVariables":
  [
    {
        "name" : "processVariable",
        "value" : "some string",
        "operation" : "equals",
        "type" : "string"
    }
  ]
}
```

The request body can contain all possible filters that can be used in the [List executions](https://www.activiti.org/userguide/#restExecutionsGet) URL query. On top of these, it’s possible to provide an array of `variables` and `processInstanceVariables` to include in the query, with their format [described here](https://www.activiti.org/userguide/#restQueryVariable).

The general [paging and sorting query-parameters](https://www.activiti.org/userguide/#restPagingAndSort) can be used for this URL.

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates request was successful and the executions are returned |
| 400           | Indicates a parameter was passed in the wrong format . The status-message contains additional information. |

**Success response body:**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31{
   "data":[
      {
         "id":"5",
         "url":"http://localhost:8182/runtime/executions/5",
         "parentId":null,
         "parentUrl":null,
         "processInstanceId":"5",
         "processInstanceUrl":"http://localhost:8182/runtime/process-instances/5",
         "suspended":false,
         "activityId":null,
         "tenantId":null
      },
      {
         "id":"7",
         "url":"http://localhost:8182/runtime/executions/7",
         "parentId":"5",
         "parentUrl":"http://localhost:8182/runtime/executions/5",
         "processInstanceId":"5",
         "processInstanceUrl":"http://localhost:8182/runtime/process-instances/5",
         "suspended":false,
         "activityId":"processTask",
         "tenantId":null
      }
   ],
   "total":2,
   "start":0,
   "sort":"processInstanceId",
   "order":"asc",
   "size":2
}
```

#### 13.6.6. List of variables for an execution

```
GET runtime/executions/{executionId}/variables?scope={scope}
```

| Parameter   | Required | Value  | Description                                                  |
| ----------- | -------- | ------ | ------------------------------------------------------------ |
| executionId | Yes      | String | The id of the execution to the variables for.                |
| scope       | No       | String | Either `local` or `global`. If omitted, both local and global scoped variables are returned. |

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates the execution was found and variables are returned. |
| 404           | Indicates the requested execution was not found.             |

**Success response body:**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16
17[
   {
      "name":"intProcVar",
      "type":"integer",
      "value":123,
      "scope":"global"
   },
   {
      "name":"byteArrayProcVar",
      "type":"binary",
      "value":null,
      "valueUrl":"http://localhost:8182/runtime/process-instances/5/variables/byteArrayProcVar/data",
      "scope":"local"
   }


]
```

In case the variable is a binary variable or serializable, the `valueUrl` points to an URL to fetch the raw value. If it’s a plain variable, the value is present in the response.

#### 13.6.7. Get a variable for an execution

```
GET runtime/executions/{executionId}/variables/{variableName}?scope={scope}
```

| Parameter    | Required | Value  | Description                                                  |
| ------------ | -------- | ------ | ------------------------------------------------------------ |
| executionId  | Yes      | String | The id of the execution to the variables for.                |
| variableName | Yes      | String | Name of the variable to get.                                 |
| scope        | No       | String | Either `local` or `global`. If omitted, local variable is returned (if exists). If not, a global variable is returned (if exists). |

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates both the execution and variable were found and variable is returned. |
| 400           | Indicates the request body is incomplete or contains illegal values. The status description contains additional information about the error. |
| 404           | Indicates the requested execution was not found or the execution does not have a variable with the given name in the requested scope (in case scope-query parameter was omitted, variable doesn’t exist in local and global scope). Status description contains additional information about the error. |

**Success response body:**

```
1
2
3
4
5
6   {
      "name":"intProcVar",
      "type":"integer",
      "value":123,
      "scope":"local"
   }
```

In case the variable is a binary variable or serializable, the `valueUrl` points to an URL to fetch the raw value. If it’s a plain variable, the value is present in the response.

#### 13.6.8. Create (or update) variables on an execution

```
POST runtime/executions/{executionId}/variables
PUT runtime/executions/{executionId}/variables
```

When using `POST`, all variables that are passed are created. In case one of the variables already exists on the execution in the requested scope, the request results in an error (409 - CONFLICT). When `PUT` is used, nonexistent variables are created on the execution and existing ones are overridden without any error.

| Parameter   | Required | Value  | Description                                   |
| ----------- | -------- | ------ | --------------------------------------------- |
| executionId | Yes      | String | The id of the execution to the variables for. |

**Request body:**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11[
   {
      "name":"intProcVar"
      "type":"integer"
      "value":123,
      "scope":"local"
   }



]
```

*Note that you can only provide variables that have the same scope. If the request-body array contains variables from mixed scopes, the request results in an error (400 - BAD REQUEST).*Any number of variables can be passed into the request body array. More information about the variable format can be found in [the REST variables section](https://www.activiti.org/userguide/#restVariables). Note that scope is ignored, only `local` variables can be set in a process instance.

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 201           | Indicates the execution was found and variable is created.   |
| 400           | Indicates the request body is incomplete or contains illegal values. The status description contains additional information about the error. |
| 404           | Indicates the requested execution was not found.             |
| 409           | Indicates the execution was found but already contains a variable with the given name (only thrown when POST method is used). Use the update-method instead. |

**Success response body:**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11[
   {
      "name":"intProcVar",
      "type":"integer",
      "value":123,
      "scope":"local"
   }



]
```

#### 13.6.9. Update a variable on an execution

```
PUT runtime/executions/{executionId}/variables/{variableName}
```

| Parameter    | Required | Value  | Description                                          |
| ------------ | -------- | ------ | ---------------------------------------------------- |
| executionId  | Yes      | String | The id of the execution to update the variables for. |
| variableName | Yes      | String | Name of the variable to update.                      |

**Request body:**

```
1
2
3
4
5
6 {
    "name":"intProcVar"
    "type":"integer"
    "value":123,
    "scope":"global"
 }
```

More information about the variable format can be found in [the REST variables section](https://www.activiti.org/userguide/#restVariables).

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates both the process instance and variable were found and variable is updated. |
| 404           | Indicates the requested process instance was not found or the process instance does not have a variable with the given name. Status description contains additional information about the error. |

**Success response body:**

```
1
2
3
4
5
6   {
      "name":"intProcVar",
      "type":"integer",
      "value":123,
      "scope":"global"
   }
```

In case the variable is a binary variable or serializable, the `valueUrl` points to an URL to fetch the raw value. If it’s a plain variable, the value is present in the response.

#### 13.6.10. Create a new binary variable on an execution

```
POST runtime/executions/{executionId}/variables
```

| Parameter   | Required | Value  | Description                                             |
| ----------- | -------- | ------ | ------------------------------------------------------- |
| executionId | Yes      | String | The id of the execution to create the new variable for. |

**Request body:**

The request should be of type `multipart/form-data`. There should be a single file-part included with the binary value of the variable. On top of that, the following additional form-fields can be present:

- `name`: Required name of the variable.
- `type`: Type of variable that is created. If omitted, `binary` is assumed and the binary data in the request will be stored as an array of bytes.
- `scope`: Scope of variable that is created. If omitted, `local` is assumed.

**Success response body:**

```
1
2
3
4
5
6
7{
  "name" : "binaryVariable",
  "scope" : "local",
  "type" : "binary",
  "value" : null,
  "valueUrl" : "http://.../runtime/executions/123/variables/binaryVariable/data"
}
```

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 201           | Indicates the variable was created and the result is returned. |
| 400           | Indicates the name of the variable to create was missing. Status message provides additional information. |
| 404           | Indicates the requested execution was not found.             |
| 409           | Indicates the execution already has a variable with the given name. Use the PUT method to update the task variable instead. |
| 415           | Indicates the serializable data contains an object for which no class is present in the JVM running the Activiti engine and therefore cannot be deserialized. |

#### 13.6.11. Update an existing binary variable on a process-instance

```
PUT runtime/executions/{executionId}/variables/{variableName}
```

| Parameter    | Required | Value  | Description                                             |
| ------------ | -------- | ------ | ------------------------------------------------------- |
| executionId  | Yes      | String | The id of the execution to create the new variable for. |
| variableName | Yes      | String | The name of the variable to update.                     |

**Request body:** The request should be of type `multipart/form-data`. There should be a single file-part included with the binary value of the variable. On top of that, the following additional form-fields can be present:

- `name`: Required name of the variable.
- `type`: Type of variable that is created. If omitted, `binary` is assumed and the binary data in the request will be stored as an array of bytes.
- `scope`: Scope of variable that is created. If omitted, `local` is assumed.

**Success response body:**

```
1
2
3
4
5
6
7{
  "name" : "binaryVariable",
  "scope" : "local",
  "type" : "binary",
  "value" : null,
  "valueUrl" : "http://.../runtime/executions/123/variables/binaryVariable/data"
}
```

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates the variable was updated and the result is returned. |
| 400           | Indicates the name of the variable to update was missing. Status message provides additional information. |
| 404           | Indicates the requested execution was not found or the execution does not have a variable with the given name. |
| 415           | Indicates the serializable data contains an object for which no class is present in the JVM running the Activiti engine and therefore cannot be deserialized. |

### 13.7. Tasks

#### 13.7.1. Get a task

```
GET runtime/tasks/{taskId}
```

| Parameter | Required | Value  | Description                |
| --------- | -------- | ------ | -------------------------- |
| taskId    | Yes      | String | The id of the task to get. |

| Response code | Description                                 |
| ------------- | ------------------------------------------- |
| 200           | Indicates the task was found and returned.  |
| 404           | Indicates the requested task was not found. |

**Success response body:**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16
17
18
19{
  "assignee" : "kermit",
  "createTime" : "2013-04-17T10:17:43.902+0000",
  "delegationState" : "pending",
  "description" : "Task description",
  "dueDate" : "2013-04-17T10:17:43.902+0000",
  "execution" : "http://localhost:8182/runtime/executions/5",
  "id" : "8",
  "name" : "My task",
  "owner" : "owner",
  "parentTask" : "http://localhost:8182/runtime/tasks/9",
  "priority" : 50,
  "processDefinition" : "http://localhost:8182/repository/process-definitions/oneTaskProcess%3A1%3A4",
  "processInstance" : "http://localhost:8182/runtime/process-instances/5",
  "suspended" : false,
  "taskDefinitionKey" : "theTask",
  "url" : "http://localhost:8182/runtime/tasks/8",
  "tenantId" : null
}
```

- `delegationState`: Delegation-state of the task, can be `null`, `"pending"` or `"resolved".`

#### 13.7.2. List of tasks

```
GET runtime/tasks
```

| Parameter                      | Required | Value    | Description                                                  |
| ------------------------------ | -------- | -------- | ------------------------------------------------------------ |
| name                           | No       | String   | Only return tasks with the given name.                       |
| nameLike                       | No       | String   | Only return tasks with a name like the given name.           |
| description                    | No       | String   | Only return tasks with the given description.                |
| priority                       | No       | Integer  | Only return tasks with the given priority.                   |
| minimumPriority                | No       | Integer  | Only return tasks with a priority greater than the given value. |
| maximumPriority                | No       | Integer  | Only return tasks with a priority lower than the given value. |
| assignee                       | No       | String   | Only return tasks assigned to the given user.                |
| assigneeLike                   | No       | String   | Only return tasks assigned with an assignee like the given value. |
| owner                          | No       | String   | Only return tasks owned by the given user.                   |
| ownerLike                      | No       | String   | Only return tasks assigned with an owner like the given value. |
| unassigned                     | No       | Boolean  | Only return tasks that are not assigned to anyone. If `false`is passed, the value is ignored. |
| delegationState                | No       | String   | Only return tasks that have the given delegation state. Possible values are `pending`and `resolved`. |
| candidateUser                  | No       | String   | Only return tasks that can be claimed by the given user. This includes both tasks where the user is an explicit candidate for and task that are claimable by a group that the user is a member of. |
| candidateGroup                 | No       | String   | Only return tasks that can be claimed by a user in the given group. |
| candidateGroups                | No       | String   | Only return tasks that can be claimed by a user in the given groups. Values split by comma. |
| involvedUser                   | No       | String   | Only return tasks in which the given user is involved.       |
| taskDefinitionKey              | No       | String   | Only return tasks with the given task definition id.         |
| taskDefinitionKeyLike          | No       | String   | Only return tasks with a given task definition id like the given value. |
| processInstanceId              | No       | String   | Only return tasks which are part of the process instance with the given id. |
| processInstanceBusinessKey     | No       | String   | Only return tasks which are part of the process instance with the given business key. |
| processInstanceBusinessKeyLike | No       | String   | Only return tasks which are part of the process instance which has a business key like the given value. |
| processDefinitionId            | No       | String   | Only return tasks which are part of a process instance which has a process definition with the given id. |
| processDefinitionKey           | No       | String   | Only return tasks which are part of a process instance which has a process definition with the given key. |
| processDefinitionKeyLike       | No       | String   | Only return tasks which are part of a process instance which has a process definition with a key like the given value. |
| processDefinitionName          | No       | String   | Only return tasks which are part of a process instance which has a process definition with the given name. |
| processDefinitionNameLike      | No       | String   | Only return tasks which are part of a process instance which has a process definition with a name like the given value. |
| executionId                    | No       | String   | Only return tasks which are part of the execution with the given id. |
| createdOn                      | No       | ISO Date | Only return tasks which are created on the given date.       |
| createdBefore                  | No       | ISO Date | Only return tasks which are created before the given date.   |
| createdAfter                   | No       | ISO Date | Only return tasks which are created after the given date.    |
| dueOn                          | No       | ISO Date | Only return tasks which are due on the given date.           |
| dueBefore                      | No       | ISO Date | Only return tasks which are due before the given date.       |
| dueAfter                       | No       | ISO Date | Only return tasks which are due after the given date.        |
| withoutDueDate                 | No       | boolean  | Only return tasks which don’t have a due date. The property is ignored if the value is `false`. |
| withoutDueDate                 | No       | boolean  | Only return tasks which don’t have a due date. The property is ignored if the value is `false`. |
| withoutDueDate                 | No       | boolean  | Only return tasks which don’t have a due date. The property is ignored if the value is `false`. |
| excludeSubTasks                | No       | Boolean  | Only return tasks that are not a subtask of another task.    |
| active                         | No       | Boolean  | If `true`, only return tasks that are not suspended (either part of a process that is not suspended or not part of a process at all). If false, only tasks that are part of suspended process instances are returned. |
| includeTaskLocalVariables      | No       | Boolean  | Indication to include task local variables in the result.    |
| includeProcessVariables        | No       | Boolean  | Indication to include process variables in the result.       |
| tenantId                       | No       | String   | Only return tasks with the given tenantId.                   |
| tenantIdLike                   | No       | String   | Only return tasks with a tenantId like the given value.      |
| withoutTenantId                | No       | Boolean  | If `true`, only returns tasks without a tenantId set. If `false`, the `withoutTenantId`parameter is ignored. |
| candidateOrAssigned            | No       | String   | Select tasks that has been claimed or assigned to user or waiting to claim by user (candidate user or groups). |
| category                       | No       | string   | Select tasks with the given category. Note that this is the task category, not the category of the process definition (namespace within the BPMN Xml). |

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates request was successful and the tasks are returned  |
| 400           | Indicates a parameter was passed in the wrong format or that *delegationState* has an invalid value (other than *pending* and *resolved*). The status-message contains additional information. |

**Success response body:**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28{
  "data": [
    {
      "assignee" : "kermit",
      "createTime" : "2013-04-17T10:17:43.902+0000",
      "delegationState" : "pending",
      "description" : "Task description",
      "dueDate" : "2013-04-17T10:17:43.902+0000",
      "execution" : "http://localhost:8182/runtime/executions/5",
      "id" : "8",
      "name" : "My task",
      "owner" : "owner",
      "parentTask" : "http://localhost:8182/runtime/tasks/9",
      "priority" : 50,
      "processDefinition" : "http://localhost:8182/repository/process-definitions/oneTaskProcess%3A1%3A4",
      "processInstance" : "http://localhost:8182/runtime/process-instances/5",
      "suspended" : false,
      "taskDefinitionKey" : "theTask",
      "url" : "http://localhost:8182/runtime/tasks/8",
      "tenantId" : null
    }
  ],
  "total": 1,
  "start": 0,
  "sort": "name",
  "order": "asc",
  "size": 1
}
```

#### 13.7.3. Query for tasks

```
POST query/tasks
```

**Request body:**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16
17
18
19
20
21
22{
  "name" : "My task",
  "description" : "The task description",

  ...

  "taskVariables" : [
    {
      "name" : "myVariable",
      "value" : 1234,
      "operation" : "equals",
      "type" : "long"
    }
  ],

    "processInstanceVariables" : [
      {
         ...
      }
    ]
  ]
}
```

All supported JSON parameter fields allowed are exactly the same as the parameters found for [getting a collection of tasks](https://www.activiti.org/userguide/#restTasksGet) (except for candidateGroupIn which is only available in this POST task query REST service), but passed in as JSON-body arguments rather than URL-parameters to allow for more advanced querying and preventing errors with request-uri’s that are too long. On top of that, the query allows for filtering based on task and process variables. The `taskVariables` and `processInstanceVariables` are both JSON-arrays containing objects with the format [as described here.](https://www.activiti.org/userguide/#restQueryVariable)

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates request was successful and the tasks are returned  |
| 400           | Indicates a parameter was passed in the wrong format or that *delegationState* has an invalid value (other than *pending* and *resolved*). The status-message contains additional information. |

**Success response body:**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28{
  "data": [
    {
      "assignee" : "kermit",
      "createTime" : "2013-04-17T10:17:43.902+0000",
      "delegationState" : "pending",
      "description" : "Task description",
      "dueDate" : "2013-04-17T10:17:43.902+0000",
      "execution" : "http://localhost:8182/runtime/executions/5",
      "id" : "8",
      "name" : "My task",
      "owner" : "owner",
      "parentTask" : "http://localhost:8182/runtime/tasks/9",
      "priority" : 50,
      "processDefinition" : "http://localhost:8182/repository/process-definitions/oneTaskProcess%3A1%3A4",
      "processInstance" : "http://localhost:8182/runtime/process-instances/5",
      "suspended" : false,
      "taskDefinitionKey" : "theTask",
      "url" : "http://localhost:8182/runtime/tasks/8",
      "tenantId" : null
    }
  ],
  "total": 1,
  "start": 0,
  "sort": "name",
  "order": "asc",
  "size": 1
}
```

#### 13.7.4. Update a task

```
PUT runtime/tasks/{taskId}
```

**Body JSON:**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10{
  "assignee" : "assignee",
  "delegationState" : "resolved",
  "description" : "New task description",
  "dueDate" : "2013-04-17T13:06:02.438+02:00",
  "name" : "New task name",
  "owner" : "owner",
  "parentTaskId" : "3",
  "priority" : 20
}
```

All request values are optional. For example, you can only include the *assignee* attribute in the request body JSON-object, only updating the assignee of the task, leaving all other fields unaffected. When an attribute is explicitly included and is set to null, the task-value will be updated to null. Example: `{"dueDate" : null}` will clear the duedate of the task).

| Response code | Description                                              |
| ------------- | -------------------------------------------------------- |
| 200           | Indicates the task was updated.                          |
| 404           | Indicates the requested task was not found.              |
| 409           | Indicates the requested task was updated simultaneously. |

**Success response body:** see response for `runtime/tasks/{taskId}`.

#### 13.7.5. Task actions

```
POST runtime/tasks/{taskId}
```

**Complete a task - Body JSON:**

```
1
2
3
4{
  "action" : "complete",
  "variables" : []
}
```

Completes the task. Optional variable array can be passed in using the `variables` property. More information about the variable format can be found in [the REST variables section](https://www.activiti.org/userguide/#restVariables). Note that the variable-scope that is supplied is ignored and the variables are set on the parent-scope unless a variable exists in a local scope, which is overridden in this case. This is the same behavior as the `TaskService.completeTask(taskId, variables)` invocation.

Note that also a *transientVariables* property is accepted as part of this json, that follows the same structure as the *variables* property.

**Claim a task - Body JSON:**

```
1
2
3
4{
  "action" : "claim",
  "assignee" : "userWhoClaims"
}
```

Claims the task by the given assignee. If the assignee is `null`, the task is assigned to no-one, claimable again.

**Delegate a task - Body JSON:**

```
1
2
3
4{
  "action" : "delegate",
  "assignee" : "userToDelegateTo"
}
```

Delegates the task to the given assignee. The assignee is required.

**Resolve a task - Body JSON:**

```
1
2
3{
  "action" : "resolve"
}
```

Resolves the task delegation. The task is assigned back to the task owner (if any).

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates the action was executed.                           |
| 400           | When the body contains an invalid value or when the assignee is missing when the action requires it. |
| 404           | Indicates the requested task was not found.                  |
| 409           | Indicates the action cannot be performed due to a conflict. Either the task was updates simultaneously or the task was claimed by another user, in case of the *claim* action. |

**Success response body:** see response for `runtime/tasks/{taskId}`.

#### 13.7.6. Delete a task

```
DELETE runtime/tasks/{taskId}?cascadeHistory={cascadeHistory}&deleteReason={deleteReason}
```

| Parameter      | Required | Value   | Description                                                  |
| -------------- | -------- | ------- | ------------------------------------------------------------ |
| taskId         | Yes      | String  | The id of the task to delete.                                |
| cascadeHistory | False    | Boolean | Whether or not to delete the HistoricTask instance when deleting the task (if applicable). If not provided, this value defaults to false. |
| deleteReason   | False    | String  | Reason why the task is deleted. This value is ignored when `cascadeHistory` is true. |

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 204           | Indicates the task was found and has been deleted. Response-body is intentionally empty. |
| 403           | Indicates the requested task cannot be deleted because it’s part of a workflow. |
| 404           | Indicates the requested task was not found.                  |

#### 13.7.7. Get all variables for a task

```
GET runtime/tasks/{taskId}/variables?scope={scope}
```

| Parameter | Required | Value  | Description                                                  |
| --------- | -------- | ------ | ------------------------------------------------------------ |
| taskId    | Yes      | String | The id of the task to get variables for.                     |
| scope     | False    | String | Scope of variables to be returned. When *local*, only task-local variables are returned. When *global*, only variables from the task’s parent execution-hierarchy are returned. When the parameter is omitted, both local and global variables are returned. |

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates the task was found and the requested variables are returned. |
| 404           | Indicates the requested task was not found.                  |

**Success response body:**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16
17[
  {
    "name" : "doubleTaskVar",
    "scope" : "local",
    "type" : "double",
    "value" : 99.99
  },
  {
    "name" : "stringProcVar",
    "scope" : "global",
    "type" : "string",
    "value" : "This is a ProcVariable"
  }



]
```

The variables are returned as a JSON array. Full response description can be found in the general [REST-variables section](https://www.activiti.org/userguide/#restVariables).

#### 13.7.8. Get a variable from a task

```
GET runtime/tasks/{taskId}/variables/{variableName}?scope={scope}
```

| Parameter    | Required | Value  | Description                                                  |
| ------------ | -------- | ------ | ------------------------------------------------------------ |
| taskId       | Yes      | String | The id of the task to get a variable for.                    |
| variableName | Yes      | String | The name of the variable to get.                             |
| scope        | False    | String | Scope of variable to be returned. When *local*, only task-local variable value is returned. When *global*, only variable value from the task’s parent execution-hierarchy are returned. When the parameter is omitted, a local variable will be returned if it exists, otherwise a global variable. |

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates the task was found and the requested variables are returned. |
| 404           | Indicates the requested task was not found or the task doesn’t have a variable with the given name (in the given scope). Status message provides additional information. |

**Success response body:**

```
1
2
3
4
5
6{
  "name" : "myTaskVariable",
  "scope" : "local",
  "type" : "string",
  "value" : "Hello my friend"
}
```

Full response body description can be found in the general [REST-variables section](https://www.activiti.org/userguide/#restVariables).

#### 13.7.9. Get the binary data for a variable

```
GET runtime/tasks/{taskId}/variables/{variableName}/data?scope={scope}
```

| Parameter    | Required | Value  | Description                                                  |
| ------------ | -------- | ------ | ------------------------------------------------------------ |
| taskId       | Yes      | String | The id of the task to get a variable data for.               |
| variableName | Yes      | String | The name of the variable to get data for. Only variables of type `binary` and `serializable`can be used. If any other type of variable is used, a `404` is returned. |
| scope        | False    | String | Scope of variable to be returned. When *local*, only task-local variable value is returned. When *global*, only variable value from the task’s parent execution-hierarchy are returned. When the parameter is omitted, a local variable will be returned if it exists, otherwise a global variable. |

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates the task was found and the requested variables are returned. |
| 404           | Indicates the requested task was not found or the task doesn’t have a variable with the given name (in the given scope) or the variable doesn’t have a binary stream available. Status message provides additional information. |

**Success response body:**

The response body contains the binary value of the variable. When the variable is of type `binary`, the content-type of the response is set to `application/octet-stream`, regardless of the content of the variable or the request accept-type header. In case of `serializable`, `application/x-java-serialized-object` is used as content-type.

#### 13.7.10. Create new variables on a task

```
POST runtime/tasks/{taskId}/variables
```

| Parameter | Required | Value  | Description                                        |
| --------- | -------- | ------ | -------------------------------------------------- |
| taskId    | Yes      | String | The id of the task to create the new variable for. |

**Request body for creating simple (non-binary) variables:**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11[
  {
    "name" : "myTaskVariable",
    "scope" : "local",
    "type" : "string",
    "value" : "Hello my friend"
  },
  {

  }
]
```

The request body should be an array containing one or more JSON-objects representing the variables that should be created.

- `name`: Required name of the variable
- `scope`: Scope of variable that is created. If omitted, `local` is assumed.
- `type`: Type of variable that is created. If omitted, reverts to raw JSON-value type (string, boolean, integer or double).
- `value`: Variable value.

More information about the variable format can be found in [the REST variables section](https://www.activiti.org/userguide/#restVariables).

**Success response body:**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11[
  {
    "name" : "myTaskVariable",
    "scope" : "local",
    "type" : "string",
    "value" : "Hello my friend"
  },
  {

  }
]
```

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 201           | Indicates the variables were created and the result is returned. |
| 400           | Indicates the name of a variable to create was missing or that an attempt is done to create a variable on a standalone task (without a process associated) with scope `global` or an empty array of variables was included in the request or request did not contain an array of variables. Status message provides additional information. |
| 404           | Indicates the requested task was not found.                  |
| 409           | Indicates the task already has a variable with the given name. Use the PUT method to update the task variable instead. |

#### 13.7.11. Create a new binary variable on a task

```
POST runtime/tasks/{taskId}/variables
```

| Parameter | Required | Value  | Description                                        |
| --------- | -------- | ------ | -------------------------------------------------- |
| taskId    | Yes      | String | The id of the task to create the new variable for. |

**Request body:**

The request should be of type `multipart/form-data`. There should be a single file-part included with the binary value of the variable. On top of that, the following additional form-fields can be present:

- `name`: Required name of the variable.
- `scope`: Scope of variable that is created. If omitted, `local` is assumed.
- `type`: Type of variable that is created. If omitted, `binary` is assumed and the binary data in the request will be stored as an array of bytes.

**Success response body:**

```
1
2
3
4
5
6
7{
  "name" : "binaryVariable",
  "scope" : "local",
  "type" : "binary",
  "value" : null,
  "valueUrl" : "http://.../runtime/tasks/123/variables/binaryVariable/data"
}
```

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 201           | Indicates the variable was created and the result is returned. |
| 400           | Indicates the name of the variable to create was missing or that an attempt is done to create a variable on a standalone task (without a process associated) with scope `global`. Status message provides additional information. |
| 404           | Indicates the requested task was not found.                  |
| 409           | Indicates the task already has a variable with the given name. Use the PUT method to update the task variable instead. |
| 415           | Indicates the serializable data contains an object for which no class is present in the JVM running the Activiti engine and therefore cannot be deserialized. |

#### 13.7.12. Update an existing variable on a task

```
PUT runtime/tasks/{taskId}/variables/{variableName}
```

| Parameter    | Required | Value  | Description                                    |
| ------------ | -------- | ------ | ---------------------------------------------- |
| taskId       | Yes      | String | The id of the task to update the variable for. |
| variableName | Yes      | String | The name of the variable to update.            |

**Request body for updating simple (non-binary) variables:**

```
1
2
3
4
5
6{
  "name" : "myTaskVariable",
  "scope" : "local",
  "type" : "string",
  "value" : "Hello my friend"
}
```

- `name`: Required name of the variable
- `scope`: Scope of variable that is updated. If omitted, `local` is assumed.
- `type`: Type of variable that is updated. If omitted, reverts to raw JSON-value type (string, boolean, integer or double).
- `value`: Variable value.

More information about the variable format can be found in [the REST variables section](https://www.activiti.org/userguide/#restVariables).

**Success response body:**

```
1
2
3
4
5
6{
  "name" : "myTaskVariable",
  "scope" : "local",
  "type" : "string",
  "value" : "Hello my friend"
}
```

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates the variables was updated and the result is returned. |
| 400           | Indicates the name of a variable to update was missing or that an attempt is done to update a variable on a standalone task (without a process associated) with scope `global`. Status message provides additional information. |
| 404           | Indicates the requested task was not found or the task doesn’t have a variable with the given name in the given scope. Status message contains additional information about the error. |

#### 13.7.13. Updating a binary variable on a task

```
PUT runtime/tasks/{taskId}/variables/{variableName}
```

| Parameter    | Required | Value  | Description                                    |
| ------------ | -------- | ------ | ---------------------------------------------- |
| taskId       | Yes      | String | The id of the task to update the variable for. |
| variableName | Yes      | String | The name of the variable to update.            |

**Request body:**

The request should be of type `multipart/form-data`. There should be a single file-part included with the binary value of the variable. On top of that, the following additional form-fields can be present:

- `name`: Required name of the variable.
- `scope`: Scope of variable that is updated. If omitted, `local` is assumed.
- `type`: Type of variable that is updated. If omitted, `binary` is assumed and the binary data in the request will be stored as an array of bytes.

**Success response body:**

```
1
2
3
4
5
6
7{
  "name" : "binaryVariable",
  "scope" : "local",
  "type" : "binary",
  "value" : null,
  "valueUrl" : "http://.../runtime/tasks/123/variables/binaryVariable/data"
}
```

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates the variable was updated and the result is returned. |
| 400           | Indicates the name of the variable to update was missing or that an attempt is done to update a variable on a standalone task (without a process associated) with scope `global`. Status message provides additional information. |
| 404           | Indicates the requested task was not found or the variable to update doesn’t exist for the given task in the given scope. |
| 415           | Indicates the serializable data contains an object for which no class is present in the JVM running the Activiti engine and therefore cannot be deserialized. |

#### 13.7.14. Delete a variable on a task

```
DELETE runtime/tasks/{taskId}/variables/{variableName}?scope={scope}
```

| Parameter    | Required | Value  | Description                                                  |
| ------------ | -------- | ------ | ------------------------------------------------------------ |
| taskId       | Yes      | String | The id of the task the variable to delete belongs to.        |
| variableName | Yes      | String | The name of the variable to delete.                          |
| scope        | No       | String | Scope of variable to delete in. Can be either `local` or `global`. If omitted, `local` is assumed. |

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 204           | Indicates the task variable was found and has been deleted. Response-body is intentionally empty. |
| 404           | Indicates the requested task was not found or the task doesn’t have a variable with the given name. Status message contains additional information about the error. |

#### 13.7.15. Delete all local variables on a task

```
DELETE runtime/tasks/{taskId}/variables
```

| Parameter | Required | Value  | Description                                           |
| --------- | -------- | ------ | ----------------------------------------------------- |
| taskId    | Yes      | String | The id of the task the variable to delete belongs to. |

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 204           | Indicates all local task variables have been deleted. Response-body is intentionally empty. |
| 404           | Indicates the requested task was not found.                  |

#### 13.7.16. Get all identity links for a task

```
GET runtime/tasks/{taskId}/identitylinks
```

| Parameter | Required | Value  | Description                                       |
| --------- | -------- | ------ | ------------------------------------------------- |
| taskId    | Yes      | String | The id of the task to get the identity links for. |

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates the task was found and the requested identity links are returned. |
| 404           | Indicates the requested task was not found.                  |

**Success response body:**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16[
  {
    "userId" : "kermit",
    "groupId" : null,
    "type" : "candidate",
    "url" : "http://localhost:8081/activiti-rest/service/runtime/tasks/100/identitylinks/users/kermit/candidate"
  },
  {
    "userId" : null,
    "groupId" : "sales",
    "type" : "candidate",
    "url" : "http://localhost:8081/activiti-rest/service/runtime/tasks/100/identitylinks/groups/sales/candidate"
  },

  ...
]
```

#### 13.7.17. Get all identitylinks for a task for either groups or users

```
GET runtime/tasks/{taskId}/identitylinks/users
GET runtime/tasks/{taskId}/identitylinks/groups
```

Returns only identity links targetting either users or groups. Response body and status-codes are exactly the same as when getting the full list of identity links for a task.

#### 13.7.18. Get a single identity link on a task

```
GET runtime/tasks/{taskId}/identitylinks/{family}/{identityId}/{type}
```

| Parameter  | Required | Value  | Description                                                  |
| ---------- | -------- | ------ | ------------------------------------------------------------ |
| taskId     | Yes      | String | The id of the task .                                         |
| family     | Yes      | String | Either `groups` or `users`, depending on what kind of identity is targeted. |
| identityId | Yes      | String | The id of the identity.                                      |
| type       | Yes      | String | The type of identity link.                                   |

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates the task and identity link was found and returned. |
| 404           | Indicates the requested task was not found or the task doesn’t have the requested identityLink. The status contains additional information about this error. |

**Success response body:**

```
1
2
3
4
5
6{
  "userId" : null,
  "groupId" : "sales",
  "type" : "candidate",
  "url" : "http://localhost:8081/activiti-rest/service/runtime/tasks/100/identitylinks/groups/sales/candidate"
}
```

#### 13.7.19. Create an identity link on a task

```
POST runtime/tasks/{taskId}/identitylinks
```

| Parameter | Required | Value  | Description          |
| --------- | -------- | ------ | -------------------- |
| taskId    | Yes      | String | The id of the task . |

**Request body (user):**

```
1
2
3
4{
  "userId" : "kermit",
  "type" : "candidate",
}
```

**Request body (group):**

```
1
2
3
4{
  "groupId" : "sales",
  "type" : "candidate",
}
```

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 201           | Indicates the task was found and the identity link was created. |
| 404           | Indicates the requested task was not found or the task doesn’t have the requested identityLink. The status contains additional information about this error. |

**Success response body:**

```
1
2
3
4
5
6{
  "userId" : null,
  "groupId" : "sales",
  "type" : "candidate",
  "url" : "http://localhost:8081/activiti-rest/service/runtime/tasks/100/identitylinks/groups/sales/candidate"
}
```

#### 13.7.20. Delete an identity link on a task

```
DELETE runtime/tasks/{taskId}/identitylinks/{family}/{identityId}/{type}
```

| Parameter  | Required | Value  | Description                                                  |
| ---------- | -------- | ------ | ------------------------------------------------------------ |
| taskId     | Yes      | String | The id of the task.                                          |
| family     | Yes      | String | Either `groups` or `users`, depending on what kind of identity is targeted. |
| identityId | Yes      | String | The id of the identity.                                      |
| type       | Yes      | String | The type of identity link.                                   |

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 204           | Indicates the task and identity link were found and the link has been deleted. Response-body is intentionally empty. |
| 404           | Indicates the requested task was not found or the task doesn’t have the requested identityLink. The status contains additional information about this error. |

#### 13.7.21. Create a new comment on a task

```
POST runtime/tasks/{taskId}/comments
```

| Parameter | Required | Value  | Description                                   |
| --------- | -------- | ------ | --------------------------------------------- |
| taskId    | Yes      | String | The id of the task to create the comment for. |

**Request body:**

```
1
2
3
4{
  "message" : "This is a comment on the task.",
  "saveProcessInstanceId" : true
}
```

Parameter `saveProcessInstanceId` is optional, if `true` save process instance id of task with comment.

**Success response body:**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10{
  "id" : "123",
  "taskUrl" : "http://localhost:8081/activiti-rest/service/runtime/tasks/101/comments/123",
  "processInstanceUrl" : "http://localhost:8081/activiti-rest/service/history/historic-process-instances/100/comments/123",
  "message" : "This is a comment on the task.",
  "author" : "kermit",
  "time" : "2014-07-13T13:13:52.232+08:00"
  "taskId" : "101",
  "processInstanceId" : "100"
}
```

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 201           | Indicates the comment was created and the result is returned. |
| 400           | Indicates the comment is missing from the request.           |
| 404           | Indicates the requested task was not found.                  |

#### 13.7.22. Get all comments on a task

```
GET runtime/tasks/{taskId}/comments
```

| Parameter | Required | Value  | Description                                 |
| --------- | -------- | ------ | ------------------------------------------- |
| taskId    | Yes      | String | The id of the task to get the comments for. |

**Success response body:**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16
17
18
19
20
21
22[
  {
    "id" : "123",
    "taskUrl" : "http://localhost:8081/activiti-rest/service/runtime/tasks/101/comments/123",
    "processInstanceUrl" : "http://localhost:8081/activiti-rest/service/history/historic-process-instances/100/comments/123",
    "message" : "This is a comment on the task.",
    "author" : "kermit"
    "time" : "2014-07-13T13:13:52.232+08:00"
    "taskId" : "101",
    "processInstanceId" : "100"
  },
  {
    "id" : "456",
    "taskUrl" : "http://localhost:8081/activiti-rest/service/runtime/tasks/101/comments/456",
    "processInstanceUrl" : "http://localhost:8081/activiti-rest/service/history/historic-process-instances/100/comments/456",
    "message" : "This is another comment on the task.",
    "author" : "gonzo",
    "time" : "2014-07-13T13:13:52.232+08:00"
    "taskId" : "101",
    "processInstanceId" : "100"
  }
]
```

| Response code | Description                                                 |
| ------------- | ----------------------------------------------------------- |
| 200           | Indicates the task was found and the comments are returned. |
| 404           | Indicates the requested task was not found.                 |

#### 13.7.23. Get a comment on a task

```
GET runtime/tasks/{taskId}/comments/{commentId}
```

| Parameter | Required | Value  | Description                                |
| --------- | -------- | ------ | ------------------------------------------ |
| taskId    | Yes      | String | The id of the task to get the comment for. |
| commentId | Yes      | String | The id of the comment.                     |

**Success response body:**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10{
  "id" : "123",
  "taskUrl" : "http://localhost:8081/activiti-rest/service/runtime/tasks/101/comments/123",
  "processInstanceUrl" : "http://localhost:8081/activiti-rest/service/history/historic-process-instances/100/comments/123",
  "message" : "This is a comment on the task.",
  "author" : "kermit",
  "time" : "2014-07-13T13:13:52.232+08:00"
  "taskId" : "101",
  "processInstanceId" : "100"
}
```

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates the task and comment were found and the comment is returned. |
| 404           | Indicates the requested task was not found or the tasks doesn’t have a comment with the given ID. |

#### 13.7.24. Delete a comment on a task

```
DELETE runtime/tasks/{taskId}/comments/{commentId}
```

| Parameter | Required | Value  | Description                                   |
| --------- | -------- | ------ | --------------------------------------------- |
| taskId    | Yes      | String | The id of the task to delete the comment for. |
| commentId | Yes      | String | The id of the comment.                        |

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 204           | Indicates the task and comment were found and the comment is deleted. Response body is left empty intentionally. |
| 404           | Indicates the requested task was not found or the tasks doesn’t have a comment with the given ID. |

#### 13.7.25. Get all events for a task

```
GET runtime/tasks/{taskId}/events
```

| Parameter | Required | Value  | Description                               |
| --------- | -------- | ------ | ----------------------------------------- |
| taskId    | Yes      | String | The id of the task to get the events for. |

**Success response body:**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12[
  {
    "action" : "AddUserLink",
    "id" : "4",
    "message" : [ "gonzo", "contributor" ],
    "taskUrl" : "http://localhost:8182/runtime/tasks/2",
    "time" : "2013-05-17T11:50:50.000+0000",
    "url" : "http://localhost:8182/runtime/tasks/2/events/4",
    "userId" : null
  }

]
```

| Response code | Description                                               |
| ------------- | --------------------------------------------------------- |
| 200           | Indicates the task was found and the events are returned. |
| 404           | Indicates the requested task was not found.               |

#### 13.7.26. Get an event on a task

```
GET runtime/tasks/{taskId}/events/{eventId}
```

| Parameter | Required | Value  | Description                              |
| --------- | -------- | ------ | ---------------------------------------- |
| taskId    | Yes      | String | The id of the task to get the event for. |
| eventId   | Yes      | String | The id of the event.                     |

**Success response body:**

```
1
2
3
4
5
6
7
8
9{
  "action" : "AddUserLink",
  "id" : "4",
  "message" : [ "gonzo", "contributor" ],
  "taskUrl" : "http://localhost:8182/runtime/tasks/2",
  "time" : "2013-05-17T11:50:50.000+0000",
  "url" : "http://localhost:8182/runtime/tasks/2/events/4",
  "userId" : null
}
```

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates the task and event were found and the event is returned. |
| 404           | Indicates the requested task was not found or the tasks doesn’t have an event with the given ID. |

#### 13.7.27. Create a new attachment on a task, containing a link to an external resource

```
POST runtime/tasks/{taskId}/attachments
```

| Parameter | Required | Value  | Description                                      |
| --------- | -------- | ------ | ------------------------------------------------ |
| taskId    | Yes      | String | The id of the task to create the attachment for. |

**Request body:**

```
1
2
3
4
5
6{
  "name":"Simple attachment",
  "description":"Simple attachment description",
  "type":"simpleType",
  "externalUrl":"http://activiti.org"
}
```

Only the attachment name is required to create a new attachment.

**Success response body:**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11{
  "id":"3",
  "url":"http://localhost:8182/runtime/tasks/2/attachments/3",
  "name":"Simple attachment",
  "description":"Simple attachment description",
  "type":"simpleType",
  "taskUrl":"http://localhost:8182/runtime/tasks/2",
  "processInstanceUrl":null,
  "externalUrl":"http://activiti.org",
  "contentUrl":null
}
```

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 201           | Indicates the attachment was created and the result is returned. |
| 400           | Indicates the attachment name is missing from the request.   |
| 404           | Indicates the requested task was not found.                  |

#### 13.7.28. Create a new attachment on a task, with an attached file

```
POST runtime/tasks/{taskId}/attachments
```

| Parameter | Required | Value  | Description                                      |
| --------- | -------- | ------ | ------------------------------------------------ |
| taskId    | Yes      | String | The id of the task to create the attachment for. |

**Request body:**

The request should be of type `multipart/form-data`. There should be a single file-part included with the binary value of the variable. On top of that, the following additional form-fields can be present:

- `name`: Required name of the variable.
- `description`: Description of the attachment, optional.
- `type`: Type of attachment, optional. Supports any arbitrary string or a valid HTTP content-type.

**Success response body:**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11{
	"id":"5",
	"url":"http://localhost:8182/runtime/tasks/2/attachments/5",
    "name":"Binary attachment",
    "description":"Binary attachment description",
    "type":"binaryType",
    "taskUrl":"http://localhost:8182/runtime/tasks/2",
    "processInstanceUrl":null,
    "externalUrl":null,
    "contentUrl":"http://localhost:8182/runtime/tasks/2/attachments/5/content"
}
```

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 201           | Indicates the attachment was created and the result is returned. |
| 400           | Indicates the attachment name is missing from the request or no file was present in the request. The error-message contains additional information. |
| 404           | Indicates the requested task was not found.                  |

#### 13.7.29. Get all attachments on a task

```
GET runtime/tasks/{taskId}/attachments
```

| Parameter | Required | Value  | Description                                    |
| --------- | -------- | ------ | ---------------------------------------------- |
| taskId    | Yes      | String | The id of the task to get the attachments for. |

**Success response body:**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24[
  {
    "id":"3",
    "url":"http://localhost:8182/runtime/tasks/2/attachments/3",
    "name":"Simple attachment",
    "description":"Simple attachment description",
    "type":"simpleType",
    "taskUrl":"http://localhost:8182/runtime/tasks/2",
    "processInstanceUrl":null,
    "externalUrl":"http://activiti.org",
    "contentUrl":null
  },
  {
    "id":"5",
    "url":"http://localhost:8182/runtime/tasks/2/attachments/5",
    "name":"Binary attachment",
    "description":"Binary attachment description",
    "type":"binaryType",
    "taskUrl":"http://localhost:8182/runtime/tasks/2",
    "processInstanceUrl":null,
    "externalUrl":null,
    "contentUrl":"http://localhost:8182/runtime/tasks/2/attachments/5/content"
  }
]
```

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates the task was found and the attachments are returned. |
| 404           | Indicates the requested task was not found.                  |

#### 13.7.30. Get an attachment on a task

```
GET runtime/tasks/{taskId}/attachments/{attachmentId}
```

| Parameter    | Required | Value  | Description                                   |
| ------------ | -------- | ------ | --------------------------------------------- |
| taskId       | Yes      | String | The id of the task to get the attachment for. |
| attachmentId | Yes      | String | The id of the attachment.                     |

**Success response body:**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11{
  "id":"5",
  "url":"http://localhost:8182/runtime/tasks/2/attachments/5",
  "name":"Binary attachment",
  "description":"Binary attachment description",
  "type":"binaryType",
  "taskUrl":"http://localhost:8182/runtime/tasks/2",
  "processInstanceUrl":null,
  "externalUrl":null,
  "contentUrl":"http://localhost:8182/runtime/tasks/2/attachments/5/content"
}
```

- `externalUrl - contentUrl:`In case the attachment is a link to an external resource, the `externalUrl` contains the URL to the external content. If the attachment content is present in the Activiti engine, the `contentUrl` will contain an URL where the binary content can be streamed from.
- `type:`Can be any arbitrary value. When a valid formatted media-type (e.g. application/xml, text/plain) is included, the binary content HTTP response content-type will be set the the given value.

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates the task and attachment were found and the attachment is returned. |
| 404           | Indicates the requested task was not found or the tasks doesn’t have a attachment with the given ID. |

#### 13.7.31. Get the content for an attachment

```
GET runtime/tasks/{taskId}/attachment/{attachmentId}/content
```

| Parameter    | Required | Value  | Description                                                  |
| ------------ | -------- | ------ | ------------------------------------------------------------ |
| taskId       | Yes      | String | The id of the task to get a variable data for.               |
| attachmentId | Yes      | String | The id of the attachment, a `404`is returned when the attachment points to an external URL rather than content attached in Activiti. |

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates the task and attachment was found and the requested content is returned. |
| 404           | Indicates the requested task was not found or the task doesn’t have an attachment with the given id or the attachment doesn’t have a binary stream available. Status message provides additional information. |

**Success response body:**

The response body contains the binary content. By default, the content-type of the response is set to `application/octet-stream`unless the attachment type contains a valid Content-type.

#### 13.7.32. Delete an attachment on a task

```
DELETE runtime/tasks/{taskId}/attachments/{attachmentId}
```

| Parameter    | Required | Value  | Description                                      |
| ------------ | -------- | ------ | ------------------------------------------------ |
| taskId       | Yes      | String | The id of the task to delete the attachment for. |
| attachmentId | Yes      | String | The id of the attachment.                        |

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 204           | Indicates the task and attachment were found and the attachment is deleted. Response body is left empty intentionally. |
| 404           | Indicates the requested task was not found or the tasks doesn’t have a attachment with the given ID. |

### 13.8. History

#### 13.8.1. Get a historic process instance

```
GET history/historic-process-instances/{processInstanceId}
```

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates that the historic process instances could be found. |
| 404           | Indicates that the historic process instances could not be found. |

**Success response body:**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26{
  "data": [
    {
      "id" : "5",
      "businessKey" : "myKey",
      "processDefinitionId" : "oneTaskProcess%3A1%3A4",
      "processDefinitionUrl" : "http://localhost:8182/repository/process-definitions/oneTaskProcess%3A1%3A4",
      "startTime" : "2013-04-17T10:17:43.902+0000",
      "endTime" : "2013-04-18T14:06:32.715+0000",
      "durationInMillis" : 86400056,
      "startUserId" : "kermit",
      "startActivityId" : "startEvent",
      "endActivityId" : "endEvent",
      "deleteReason" : null,
      "superProcessInstanceId" : "3",
      "url" : "http://localhost:8182/history/historic-process-instances/5",
      "variables": null,
      "tenantId":null
    }
  ],
  "total": 1,
  "start": 0,
  "sort": "name",
  "order": "asc",
  "size": 1
}
```

#### 13.8.2. List of historic process instances

```
GET history/historic-process-instances
```

| Parameter               | Required | Value   | Description                                                  |
| ----------------------- | -------- | ------- | ------------------------------------------------------------ |
| processInstanceId       | No       | String  | An id of the historic process instance.                      |
| processDefinitionKey    | No       | String  | The process definition key of the historic process instance. |
| processDefinitionId     | No       | String  | The process definition id of the historic process instance.  |
| businessKey             | No       | String  | The business key of the historic process instance.           |
| involvedUser            | No       | String  | An involved user of the historic process instance.           |
| finished                | No       | Boolean | Indication if the historic process instance is finished.     |
| superProcessInstanceId  | No       | String  | An optional parent process id of the historic process instance. |
| excludeSubprocesses     | No       | Boolean | Return only historic process instances which aren’t sub processes. |
| finishedAfter           | No       | Date    | Return only historic process instances that were finished after this date. |
| finishedBefore          | No       | Date    | Return only historic process instances that were finished before this date. |
| startedAfter            | No       | Date    | Return only historic process instances that were started after this date. |
| startedBefore           | No       | Date    | Return only historic process instances that were started before this date. |
| startedBy               | No       | String  | Return only historic process instances that were started by this user. |
| includeProcessVariables | No       | Boolean | An indication if the historic process instance variables should be returned as well. |
| tenantId                | No       | String  | Only return instances with the given tenantId.               |
| tenantIdLike            | No       | String  | Only return instances with a tenantId like the given value.  |
| withoutTenantId         | No       | Boolean | If `true`, only returns instances without a tenantId set. If `false`, the `withoutTenantId` parameter is ignored. |

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates that historic process instances could be queried.  |
| 400           | Indicates an parameter was passed in the wrong format. The status-message contains additional information. |

**Success response body:**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32{
  "data": [
    {
      "id" : "5",
      "businessKey" : "myKey",
      "processDefinitionId" : "oneTaskProcess%3A1%3A4",
      "processDefinitionUrl" : "http://localhost:8182/repository/process-definitions/oneTaskProcess%3A1%3A4",
      "startTime" : "2013-04-17T10:17:43.902+0000",
      "endTime" : "2013-04-18T14:06:32.715+0000",
      "durationInMillis" : 86400056,
      "startUserId" : "kermit",
      "startActivityId" : "startEvent",
      "endActivityId" : "endEvent",
      "deleteReason" : null,
      "superProcessInstanceId" : "3",
      "url" : "http://localhost:8182/history/historic-process-instances/5",
      "variables": [
        {
          "name": "test",
          "variableScope": "local",
          "value": "myTest"
        }
      ],
      "tenantId":null
    }
  ],
  "total": 1,
  "start": 0,
  "sort": "name",
  "order": "asc",
  "size": 1
}
```

#### 13.8.3. Query for historic process instances

```
POST query/historic-process-instances
```

**Request body:**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13{
  "processDefinitionId" : "oneTaskProcess%3A1%3A4",


  "variables" : [
    {
      "name" : "myVariable",
      "value" : 1234,
      "operation" : "equals",
      "type" : "long"
    }
  ]
}
```

All supported JSON parameter fields allowed are exactly the same as the parameters found for [getting a collection of historic process instances](https://www.activiti.org/userguide/#restHistoricProcessInstancesGet), but passed in as JSON-body arguments rather than URL-parameters to allow for more advanced querying and preventing errors with request-uri’s that are too long. On top of that, the query allows for filtering based on process variables. The `variables` property is a JSON-array containing objects with the format [as described here.](https://www.activiti.org/userguide/#restQueryVariable)

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates request was successful and the tasks are returned  |
| 400           | Indicates an parameter was passed in the wrong format. The status-message contains additional information. |

**Success response body:**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32{
  "data": [
    {
      "id" : "5",
      "businessKey" : "myKey",
      "processDefinitionId" : "oneTaskProcess%3A1%3A4",
      "processDefinitionUrl" : "http://localhost:8182/repository/process-definitions/oneTaskProcess%3A1%3A4",
      "startTime" : "2013-04-17T10:17:43.902+0000",
      "endTime" : "2013-04-18T14:06:32.715+0000",
      "durationInMillis" : 86400056,
      "startUserId" : "kermit",
      "startActivityId" : "startEvent",
      "endActivityId" : "endEvent",
      "deleteReason" : null,
      "superProcessInstanceId" : "3",
      "url" : "http://localhost:8182/history/historic-process-instances/5",
      "variables": [
        {
          "name": "test",
          "variableScope": "local",
          "value": "myTest"
        }
      ],
      "tenantId":null
    }
  ],
  "total": 1,
  "start": 0,
  "sort": "name",
  "order": "asc",
  "size": 1
}
```

#### 13.8.4. Delete a historic process instance

```
DELETE history/historic-process-instances/{processInstanceId}
```

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates that the historic process instance was deleted.    |
| 404           | Indicates that the historic process instance could not be found. |

#### 13.8.5. Get the identity links of a historic process instance

```
GET history/historic-process-instance/{processInstanceId}/identitylinks
```

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates request was successful and the identity links are returned |
| 404           | Indicates the process instance could not be found.           |

**Success response body:**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11[
 {
  "type" : "participant",
  "userId" : "kermit",
  "groupId" : null,
  "taskId" : null,
  "taskUrl" : null,
  "processInstanceId" : "5",
  "processInstanceUrl" : "http://localhost:8182/history/historic-process-instances/5"
 }
]
```

#### 13.8.6. Get the binary data for a historic process instance variable

```
GET history/historic-process-instances/{processInstanceId}/variables/{variableName}/data
```

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates the process instance was found and the requested variable data is returned. |
| 404           | Indicates the requested process instance was not found or the process instance doesn’t have a variable with the given name or the variable doesn’t have a binary stream available. Status message provides additional information. |

**Success response body:**

The response body contains the binary value of the variable. When the variable is of type `binary`, the content-type of the response is set to `application/octet-stream`, regardless of the content of the variable or the request accept-type header. In case of `serializable`, `application/x-java-serialized-object` is used as content-type.

#### 13.8.7. Create a new comment on a historic process instance

```
POST history/historic-process-instances/{processInstanceId}/comments
```

| Parameter         | Required | Value  | Description                                               |
| ----------------- | -------- | ------ | --------------------------------------------------------- |
| processInstanceId | Yes      | String | The id of the process instance to create the comment for. |

**Request body:**

```
1
2
3
4{
  "message" : "This is a comment.",
  "saveProcessInstanceId" : true
}
```

Parameter `saveProcessInstanceId` is optional, if `true` save process instance id of task with comment.

**Success response body:**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10{
  "id" : "123",
  "taskUrl" : "http://localhost:8081/activiti-rest/service/runtime/tasks/101/comments/123",
  "processInstanceUrl" : "http://localhost:8081/activiti-rest/service/history/historic-process-instances/100/comments/123",
  "message" : "This is a comment on the task.",
  "author" : "kermit",
  "time" : "2014-07-13T13:13:52.232+08:00",
  "taskId" : "101",
  "processInstanceId" : "100"
}
```

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 201           | Indicates the comment was created and the result is returned. |
| 400           | Indicates the comment is missing from the request.           |
| 404           | Indicates the requested historic process instance was not found. |

#### 13.8.8. Get all comments on a historic process instance

```
GET history/historic-process-instances/{processInstanceId}/comments
```

| Parameter         | Required | Value  | Description                                             |
| ----------------- | -------- | ------ | ------------------------------------------------------- |
| processInstanceId | Yes      | String | The id of the process instance to get the comments for. |

**Success response body:**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16
17
18[
  {
    "id" : "123",
    "processInstanceUrl" : "http://localhost:8081/activiti-rest/service/history/historic-process-instances/100/comments/123",
    "message" : "This is a comment on the task.",
    "author" : "kermit",
    "time" : "2014-07-13T13:13:52.232+08:00",
    "processInstanceId" : "100"
  },
  {
    "id" : "456",
    "processInstanceUrl" : "http://localhost:8081/activiti-rest/service/history/historic-process-instances/100/comments/456",
    "message" : "This is another comment.",
    "author" : "gonzo",
    "time" : "2014-07-14T15:16:52.232+08:00",
    "processInstanceId" : "100"
  }
]
```

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates the process instance was found and the comments are returned. |
| 404           | Indicates the requested task was not found.                  |

#### 13.8.9. Get a comment on a historic process instance

```
GET history/historic-process-instances/{processInstanceId}/comments/{commentId}
```

| Parameter         | Required | Value  | Description                                                  |
| ----------------- | -------- | ------ | ------------------------------------------------------------ |
| processInstanceId | Yes      | String | The id of the historic process instance to get the comment for. |
| commentId         | Yes      | String | The id of the comment.                                       |

**Success response body:**

```
1
2
3
4
5
6
7
8{
  "id" : "123",
  "processInstanceUrl" : "http://localhost:8081/activiti-rest/service/history/historic-process-instances/100/comments/456",
  "message" : "This is another comment.",
  "author" : "gonzo",
  "time" : "2014-07-14T15:16:52.232+08:00",
  "processInstanceId" : "100"
}
```

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates the historic process instance and comment were found and the comment is returned. |
| 404           | Indicates the requested historic process instance was not found or the historic process instance doesn’t have a comment with the given ID. |

#### 13.8.10. Delete a comment on a historic process instance

```
DELETE history/historic-process-instances/{processInstanceId}/comments/{commentId}
```

| Parameter         | Required | Value  | Description                                                  |
| ----------------- | -------- | ------ | ------------------------------------------------------------ |
| processInstanceId | Yes      | String | The id of the historic process instance to delete the comment for. |
| commentId         | Yes      | String | The id of the comment.                                       |

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 204           | Indicates the historic process instance and comment were found and the comment is deleted. Response body is left empty intentionally. |
| 404           | Indicates the requested task was not found or the historic process instance doesn’t have a comment with the given ID. |

#### 13.8.11. Get a single historic task instance

```
GET history/historic-task-instances/{taskId}
```

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates that the historic task instances could be found.   |
| 404           | Indicates that the historic task instances could not be found. |

**Success response body:**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26{
  "id" : "5",
  "processDefinitionId" : "oneTaskProcess%3A1%3A4",
  "processDefinitionUrl" : "http://localhost:8182/repository/process-definitions/oneTaskProcess%3A1%3A4",
  "processInstanceId" : "3",
  "processInstanceUrl" : "http://localhost:8182/history/historic-process-instances/3",
  "executionId" : "4",
  "name" : "My task name",
  "description" : "My task description",
  "deleteReason" : null,
  "owner" : "kermit",
  "assignee" : "fozzie",
  "startTime" : "2013-04-17T10:17:43.902+0000",
  "endTime" : "2013-04-18T14:06:32.715+0000",
  "durationInMillis" : 86400056,
  "workTimeInMillis" : 234890,
  "claimTime" : "2013-04-18T11:01:54.715+0000",
  "taskDefinitionKey" : "taskKey",
  "formKey" : null,
  "priority" : 50,
  "dueDate" : "2013-04-20T12:11:13.134+0000",
  "parentTaskId" : null,
  "url" : "http://localhost:8182/history/historic-task-instances/5",
  "variables": null,
  "tenantId":null
}
```

#### 13.8.12. Get historic task instances

```
GET history/historic-task-instances
```

| Parameter                 | Required | Value   | Description                                                  |
| ------------------------- | -------- | ------- | ------------------------------------------------------------ |
| taskId                    | No       | String  | An id of the historic task instance.                         |
| processInstanceId         | No       | String  | The process instance id of the historic task instance.       |
| processDefinitionKey      | No       | String  | The process definition key of the historic task instance.    |
| processDefinitionKeyLike  | No       | String  | The process definition key of the historic task instance, which matches the given value. |
| processDefinitionId       | No       | String  | The process definition id of the historic task instance.     |
| processDefinitionName     | No       | String  | The process definition name of the historic task instance.   |
| processDefinitionNameLike | No       | String  | The process definition name of the historic task instance, which matches the given value. |
| processBusinessKey        | No       | String  | The process instance business key of the historic task instance. |
| processBusinessKeyLike    | No       | String  | The process instance business key of the historic task instance that matches the given value. |
| executionId               | No       | String  | The execution id of the historic task instance.              |
| taskDefinitionKey         | No       | String  | The task definition key for tasks part of a process          |
| taskName                  | No       | String  | The task name of the historic task instance.                 |
| taskNameLike              | No       | String  | The task name with *like*operator for the historic task instance. |
| taskDescription           | No       | String  | The task description of the historic task instance.          |
| taskDescriptionLike       | No       | String  | The task description with *like*operator for the historic task instance. |
| taskDefinitionKey         | No       | String  | The task identifier from the process definition for the historic task instance. |
| taskCategory              | No       | String  | Select tasks with the given category. Note that this is the task category, not the category of the process definition (namespace within the BPMN Xml). |
| taskDeleteReason          | No       | String  | The task delete reason of the historic task instance.        |
| taskDeleteReasonLike      | No       | String  | The task delete reason with *like* operator for the historic task instance. |
| taskAssignee              | No       | String  | The assignee of the historic task instance.                  |
| taskAssigneeLike          | No       | String  | The assignee with *like* operator for the historic task instance. |
| taskOwner                 | No       | String  | The owner of the historic task instance.                     |
| taskOwnerLike             | No       | String  | The owner with *like* operator for the historic task instance. |
| taskInvolvedUser          | No       | String  | An involved user of the historic task instance.              |
| taskPriority              | No       | String  | The priority of the historic task instance.                  |
| finished                  | No       | Boolean | Indication if the historic task instance is finished.        |
| processFinished           | No       | Boolean | Indication if the process instance of the historic task instance is finished. |
| parentTaskId              | No       | String  | An optional parent task id of the historic task instance.    |
| dueDate                   | No       | Date    | Return only historic task instances that have a due date equal this date. |
| dueDateAfter              | No       | Date    | Return only historic task instances that have a due date after this date. |
| dueDateBefore             | No       | Date    | Return only historic task instances that have a due date before this date. |
| withoutDueDate            | No       | Boolean | Return only historic task instances that have no due-date. When `false` is provided as value, this parameter is ignored. |
| taskCompletedOn           | No       | Date    | Return only historic task instances that have been completed on this date. |
| taskCompletedAfter        | No       | Date    | Return only historic task instances that have been completed after this date. |
| taskCompletedBefore       | No       | Date    | Return only historic task instances that have been completed before this date. |
| taskCreatedOn             | No       | Date    | Return only historic task instances that were created on this date. |
| taskCreatedBefore         | No       | Date    | Return only historic task instances that were created before this date. |
| taskCreatedAfter          | No       | Date    | Return only historic task instances that were created after this date. |
| includeTaskLocalVariables | No       | Boolean | An indication if the historic task instance local variables should be returned as well. |
| includeProcessVariables   | No       | Boolean | An indication if the historic task instance global variables should be returned as well. |
| tenantId                  | No       | String  | Only return historic task instances with the given tenantId. |
| tenantIdLike              | No       | String  | Only return historic task instances with a tenantId like the given value. |
| withoutTenantId           | No       | Boolean | If `true`, only returns historic task instances without a tenantId set. If `false`, the `withoutTenantId` parameter is ignored. |

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates that historic process instances could be queried.  |
| 400           | Indicates an parameter was passed in the wrong format. The status-message contains additional information. |

**Success response body:**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34
35
36
37
38
39
40
41
42
43
44
45
46
47
48{
  "data": [
    {
      "id" : "5",
      "processDefinitionId" : "oneTaskProcess%3A1%3A4",
      "processDefinitionUrl" : "http://localhost:8182/repository/process-definitions/oneTaskProcess%3A1%3A4",
      "processInstanceId" : "3",
      "processInstanceUrl" : "http://localhost:8182/history/historic-process-instances/3",
      "executionId" : "4",
      "name" : "My task name",
      "description" : "My task description",
      "deleteReason" : null,
      "owner" : "kermit",
      "assignee" : "fozzie",
      "startTime" : "2013-04-17T10:17:43.902+0000",
      "endTime" : "2013-04-18T14:06:32.715+0000",
      "durationInMillis" : 86400056,
      "workTimeInMillis" : 234890,
      "claimTime" : "2013-04-18T11:01:54.715+0000",
      "taskDefinitionKey" : "taskKey",
      "formKey" : null,
      "priority" : 50,
      "dueDate" : "2013-04-20T12:11:13.134+0000",
      "parentTaskId" : null,
      "url" : "http://localhost:8182/history/historic-task-instances/5",
      "taskVariables": [
        {
          "name": "test",
          "variableScope": "local",
          "value": "myTest"
        }
      ],
      "processVariables": [
        {
          "name": "processTest",
          "variableScope": "global",
          "value": "myProcessTest"
        }
      ],
      "tenantId":null
    }
  ],
  "total": 1,
  "start": 0,
  "sort": "name",
  "order": "asc",
  "size": 1
}
```

#### 13.8.13. Query for historic task instances

```
POST query/historic-task-instances
```

**Query for historic task instances - Request body:**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13{
  "processDefinitionId" : "oneTaskProcess%3A1%3A4",
  ...

  "variables" : [
    {
      "name" : "myVariable",
      "value" : 1234,
      "operation" : "equals",
      "type" : "long"
    }
  ]
}
```

All supported JSON parameter fields allowed are exactly the same as the parameters found for [getting a collection of historic task instances](https://www.activiti.org/userguide/#restHistoricTaskInstancesGet), but passed in as JSON-body arguments rather than URL-parameters to allow for more advanced querying and preventing errors with request-uri’s that are too long. On top of that, the query allows for filtering based on process variables. The `taskVariables` and `processVariables` properties are JSON-arrays containing objects with the format [as described here.](https://www.activiti.org/userguide/#restQueryVariable)

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates request was successful and the tasks are returned  |
| 400           | Indicates an parameter was passed in the wrong format. The status-message contains additional information. |

**Success response body:**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34
35
36
37
38
39
40
41
42
43
44
45
46
47
48{
  "data": [
    {
      "id" : "5",
      "processDefinitionId" : "oneTaskProcess%3A1%3A4",
      "processDefinitionUrl" : "http://localhost:8182/repository/process-definitions/oneTaskProcess%3A1%3A4",
      "processInstanceId" : "3",
      "processInstanceUrl" : "http://localhost:8182/history/historic-process-instances/3",
      "executionId" : "4",
      "name" : "My task name",
      "description" : "My task description",
      "deleteReason" : null,
      "owner" : "kermit",
      "assignee" : "fozzie",
      "startTime" : "2013-04-17T10:17:43.902+0000",
      "endTime" : "2013-04-18T14:06:32.715+0000",
      "durationInMillis" : 86400056,
      "workTimeInMillis" : 234890,
      "claimTime" : "2013-04-18T11:01:54.715+0000",
      "taskDefinitionKey" : "taskKey",
      "formKey" : null,
      "priority" : 50,
      "dueDate" : "2013-04-20T12:11:13.134+0000",
      "parentTaskId" : null,
      "url" : "http://localhost:8182/history/historic-task-instances/5",
      "taskVariables": [
        {
          "name": "test",
          "variableScope": "local",
          "value": "myTest"
        }
      ],
      "processVariables": [
        {
          "name": "processTest",
          "variableScope": "global",
          "value": "myProcessTest"
        }
      ],
      "tenantId":null
    }
  ],
  "total": 1,
  "start": 0,
  "sort": "name",
  "order": "asc",
  "size": 1
}
```

#### 13.8.14. Delete a historic task instance

```
DELETE history/historic-task-instances/{taskId}
```

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates that the historic task instance was deleted.       |
| 404           | Indicates that the historic task instance could not be found. |

#### 13.8.15. Get the identity links of a historic task instance

```
GET history/historic-task-instance/{taskId}/identitylinks
```

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates request was successful and the identity links are returned |
| 404           | Indicates the task instance could not be found.              |

**Success response body:**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11[
 {
  "type" : "assignee",
  "userId" : "kermit",
  "groupId" : null,
  "taskId" : "6",
  "taskUrl" : "http://localhost:8182/history/historic-task-instances/5",
  "processInstanceId" : null,
  "processInstanceUrl" : null
 }
]
```

#### 13.8.16. Get the binary data for a historic task instance variable

```
GET history/historic-task-instances/{taskId}/variables/{variableName}/data
```

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates the task instance was found and the requested variable data is returned. |
| 404           | Indicates the requested task instance was not found or the process instance doesn’t have a variable with the given name or the variable doesn’t have a binary stream available. Status message provides additional information. |

**Success response body:**

The response body contains the binary value of the variable. When the variable is of type `binary`, the content-type of the response is set to `application/octet-stream`, regardless of the content of the variable or the request accept-type header. In case of `serializable`, `application/x-java-serialized-object` is used as content-type.

#### 13.8.17. Get historic activity instances

```
GET history/historic-activity-instances
```

| Parameter           | Required | Value   | Description                                                  |
| ------------------- | -------- | ------- | ------------------------------------------------------------ |
| activityId          | No       | String  | An id of the activity instance.                              |
| activityInstanceId  | No       | String  | An id of the historic activity instance.                     |
| activityName        | No       | String  | The name of the historic activity instance.                  |
| activityType        | No       | String  | The element type of the historic activity instance.          |
| executionId         | No       | String  | The execution id of the historic activity instance.          |
| finished            | No       | Boolean | Indication if the historic activity instance is finished.    |
| taskAssignee        | No       | String  | The assignee of the historic activity instance.              |
| processInstanceId   | No       | String  | The process instance id of the historic activity instance.   |
| processDefinitionId | No       | String  | The process definition id of the historic activity instance. |
| tenantId            | No       | String  | Only return instances with the given tenantId.               |
| tenantIdLike        | No       | String  | Only return instances with a tenantId like the given value.  |
| withoutTenantId     | No       | Boolean | If `true`, only returns instances without a tenantId set. If `false`, the `withoutTenantId` parameter is ignored. |

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates that historic activity instances could be queried. |
| 400           | Indicates an parameter was passed in the wrong format. The status-message contains additional information. |

**Success response body:**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27{
  "data": [
    {
      "id" : "5",
      "activityId" : "4",
      "activityName" : "My user task",
      "activityType" : "userTask",
      "processDefinitionId" : "oneTaskProcess%3A1%3A4",
      "processDefinitionUrl" : "http://localhost:8182/repository/process-definitions/oneTaskProcess%3A1%3A4",
      "processInstanceId" : "3",
      "processInstanceUrl" : "http://localhost:8182/history/historic-process-instances/3",
      "executionId" : "4",
      "taskId" : "4",
      "calledProcessInstanceId" : null,
      "assignee" : "fozzie",
      "startTime" : "2013-04-17T10:17:43.902+0000",
      "endTime" : "2013-04-18T14:06:32.715+0000",
      "durationInMillis" : 86400056,
      "tenantId":null
    }
  ],
  "total": 1,
  "start": 0,
  "sort": "name",
  "order": "asc",
  "size": 1
}
```

#### 13.8.18. Query for historic activity instances

```
POST query/historic-activity-instances
```

**Request body:**

```
1
2
3{
  "processDefinitionId" : "oneTaskProcess%3A1%3A4"
}
```

All supported JSON parameter fields allowed are exactly the same as the parameters found for [getting a collection of historic task instances](https://www.activiti.org/userguide/#restHistoricTaskInstancesGet), but passed in as JSON-body arguments rather than URL-parameters to allow for more advanced querying and preventing errors with request-uri’s that are too long.

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates request was successful and the activities are returned |
| 400           | Indicates an parameter was passed in the wrong format. The status-message contains additional information. |

**Success response body:**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27{
  "data": [
    {
      "id" : "5",
      "activityId" : "4",
      "activityName" : "My user task",
      "activityType" : "userTask",
      "processDefinitionId" : "oneTaskProcess%3A1%3A4",
      "processDefinitionUrl" : "http://localhost:8182/repository/process-definitions/oneTaskProcess%3A1%3A4",
      "processInstanceId" : "3",
      "processInstanceUrl" : "http://localhost:8182/history/historic-process-instances/3",
      "executionId" : "4",
      "taskId" : "4",
      "calledProcessInstanceId" : null,
      "assignee" : "fozzie",
      "startTime" : "2013-04-17T10:17:43.902+0000",
      "endTime" : "2013-04-18T14:06:32.715+0000",
      "durationInMillis" : 86400056,
      "tenantId":null
    }
  ],
  "total": 1,
  "start": 0,
  "sort": "name",
  "order": "asc",
  "size": 1
}
```

#### 13.8.19. List of historic variable instances

```
GET history/historic-variable-instances
```

| Parameter            | Required | Value   | Description                                                  |
| -------------------- | -------- | ------- | ------------------------------------------------------------ |
| processInstanceId    | No       | String  | The process instance id of the historic variable instance.   |
| taskId               | No       | String  | The task id of the historic variable instance.               |
| excludeTaskVariables | No       | Boolean | Indication to exclude the task variables from the result.    |
| variableName         | No       | String  | The variable name of the historic variable instance.         |
| variableNameLike     | No       | String  | The variable name using the *like* operator for the historic variable instance. |

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates that historic variable instances could be queried. |
| 400           | Indicates an parameter was passed in the wrong format. The status-message contains additional information. |

**Success response body:**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16
17
18
19
20{
  "data": [
    {
      "id" : "14",
      "processInstanceId" : "5",
      "processInstanceUrl" : "http://localhost:8182/history/historic-process-instances/5",
      "taskId" : "6",
      "variable" : {
        "name" : "myVariable",
        "variableScope", "global",
        "value" : "test"
      }
    }
  ],
  "total": 1,
  "start": 0,
  "sort": "name",
  "order": "asc",
  "size": 1
}
```

#### 13.8.20. Query for historic variable instances

```
POST query/historic-variable-instances
```

**Request body:**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13{
  "processDefinitionId" : "oneTaskProcess%3A1%3A4",
  ...

  "variables" : [
    {
      "name" : "myVariable",
      "value" : 1234,
      "operation" : "equals",
      "type" : "long"
    }
  ]
}
```

All supported JSON parameter fields allowed are exactly the same as the parameters found for [getting a collection of historic process instances](https://www.activiti.org/userguide/#restHistoricVariableInstancesGet), but passed in as JSON-body arguments rather than URL-parameters to allow for more advanced querying and preventing errors with request-uri’s that are too long. On top of that, the query allows for filtering based on process variables. The `variables` property is a JSON-array containing objects with the format [as described here.](https://www.activiti.org/userguide/#restQueryVariable)

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates request was successful and the tasks are returned  |
| 400           | Indicates an parameter was passed in the wrong format. The status-message contains additional information. |

**Success response body:**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16
17
18
19
20{
  "data": [
    {
      "id" : "14",
      "processInstanceId" : "5",
      "processInstanceUrl" : "http://localhost:8182/history/historic-process-instances/5",
      "taskId" : "6",
      "variable" : {
        "name" : "myVariable",
        "variableScope", "global",
        "value" : "test"
      }
    }
  ],
  "total": 1,
  "start": 0,
  "sort": "name",
  "order": "asc",
  "size": 1
}
```

====Get the binary data for a historic task instance variable

```
GET history/historic-variable-instances/{varInstanceId}/data
```

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates the variable instance was found and the requested variable data is returned. |
| 404           | Indicates the requested variable instance was not found or the variable instance doesn’t have a variable with the given name or the variable doesn’t have a binary stream available. Status message provides additional information. |

**Success response body:**

The response body contains the binary value of the variable. When the variable is of type `binary`, the content-type of the response is set to `application/octet-stream`, regardless of the content of the variable or the request accept-type header. In case of `serializable`, `application/x-java-serialized-object` is used as content-type.

#### 13.8.21. Get historic detail

```
GET history/historic-detail
```

| Parameter                 | Required | Value   | Description                                               |
| ------------------------- | -------- | ------- | --------------------------------------------------------- |
| id                        | No       | String  | The id of the historic detail.                            |
| processInstanceId         | No       | String  | The process instance id of the historic detail.           |
| executionId               | No       | String  | The execution id of the historic detail.                  |
| activityInstanceId        | No       | String  | The activity instance id of the historic detail.          |
| taskId                    | No       | String  | The task id of the historic detail.                       |
| selectOnlyFormProperties  | No       | Boolean | Indication to only return form properties in the result.  |
| selectOnlyVariableUpdates | No       | Boolean | Indication to only return variable updates in the result. |

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates that historic detail could be queried.             |
| 400           | Indicates an parameter was passed in the wrong format. The status-message contains additional information. |

**Success response body:**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28{
  "data": [
    {
      "id" : "26",
      "processInstanceId" : "5",
      "processInstanceUrl" : "http://localhost:8182/history/historic-process-instances/5",
      "executionId" : "6",
      "activityInstanceId", "10",
      "taskId" : "6",
      "taskUrl" : "http://localhost:8182/history/historic-task-instances/6",
      "time" : "2013-04-17T10:17:43.902+0000",
      "detailType" : "variableUpdate",
      "revision" : 2,
      "variable" : {
        "name" : "myVariable",
        "variableScope", "global",
        "value" : "test"
      },
      "propertyId": null,
      "propertyValue": null
    }
  ],
  "total": 1,
  "start": 0,
  "sort": "name",
  "order": "asc",
  "size": 1
}
```

#### 13.8.22. Query for historic details

```
POST query/historic-detail
```

**Request body:**

```
{
  "processInstanceId" : "5",
}
```

All supported JSON parameter fields allowed are exactly the same as the parameters found for [getting a collection of historic process instances](https://www.activiti.org/userguide/#restHistoricDetailGet), but passed in as JSON-body arguments rather than URL-parameters to allow for more advanced querying and preventing errors with request-uri’s that are too long.

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates request was successful and the historic details are returned |
| 400           | Indicates an parameter was passed in the wrong format. The status-message contains additional information. |

**Success response body:**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28{
  "data": [
    {
      "id" : "26",
      "processInstanceId" : "5",
      "processInstanceUrl" : "http://localhost:8182/history/historic-process-instances/5",
      "executionId" : "6",
      "activityInstanceId", "10",
      "taskId" : "6",
      "taskUrl" : "http://localhost:8182/history/historic-task-instances/6",
      "time" : "2013-04-17T10:17:43.902+0000",
      "detailType" : "variableUpdate",
      "revision" : 2,
      "variable" : {
        "name" : "myVariable",
        "variableScope", "global",
        "value" : "test"
      },
      "propertyId" : null,
      "propertyValue" : null
    }
  ],
  "total": 1,
  "start": 0,
  "sort": "name",
  "order": "asc",
  "size": 1
}
```

#### 13.8.23. Get the binary data for a historic detail variable

```
GET history/historic-detail/{detailId}/data
```

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates the historic detail instance was found and the requested variable data is returned. |
| 404           | Indicates the requested historic detail instance was not found or the historic detail instance doesn’t have a variable with the given name or the variable doesn’t have a binary stream available. Status message provides additional information. |

**Success response body:**

The response body contains the binary value of the variable. When the variable is of type `binary`, the content-type of the response is set to `application/octet-stream`, regardless of the content of the variable or the request accept-type header. In case of `serializable`, `application/x-java-serialized-object` is used as content-type.

### 13.9. Forms

#### 13.9.1. Get form data

```
GET form/form-data
```

| Parameter           | Required                        | Value  | Description                                                  |
| ------------------- | ------------------------------- | ------ | ------------------------------------------------------------ |
| taskId              | Yes (if no processDefinitionId) | String | The task id corresponding to the form data that needs to be retrieved. |
| processDefinitionId | Yes (if no taskId)              | String | The process definition id corresponding to the start event form data that needs to be retrieved. |

| Response code | Description                                  |
| ------------- | -------------------------------------------- |
| 200           | Indicates that form data could be queried.   |
| 404           | Indicates that form data could not be found. |

**Success response body:**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34
35
36
37
38
39{
  "data": [
    {
      "formKey" : null,
      "deploymentId" : "2",
      "processDefinitionId" : "3",
      "processDefinitionUrl" : "http://localhost:8182/repository/process-definition/3",
      "taskId" : "6",
      "taskUrl" : "http://localhost:8182/runtime/task/6",
      "formProperties" : [
        {
          "id" : "room",
          "name" : "Room",
          "type" : "string",
          "value" : null,
          "readable" : true,
          "writable" : true,
          "required" : true,
          "datePattern" : null,
          "enumValues" : [
            {
              "id" : "normal",
              "name" : "Normal bed"
            },
            {
              "id" : "kingsize",
              "name" : "Kingsize bed"
            },
          ]
        }
      ]
    }
  ],
  "total": 1,
  "start": 0,
  "sort": "name",
  "order": "asc",
  "size": 1
}
```

#### 13.9.2. Submit task form data

```
POST form/form-data
```

**Request body for task form:**

```
1
2
3
4
5
6
7
8
9{
  "taskId" : "5",
  "properties" : [
    {
      "id" : "room",
      "value" : "normal"
    }
  ]
}
```

**Request body for start event form:**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10{
  "processDefinitionId" : "5",
  "businessKey" : "myKey",
  "properties" : [
    {
      "id" : "room",
      "value" : "normal"
    }
  ]
}
```

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates request was successful and the form data was submitted |
| 400           | Indicates an parameter was passed in the wrong format. The status-message contains additional information. |

**Success response body for start event form data (no response for task form data):**

```
1
2
3
4
5
6
7
8
9{
  "id" : "5",
  "url" : "http://localhost:8182/history/historic-process-instances/5",
  "businessKey" : "myKey",
  "suspended": false,
  "processDefinitionId" : "3",
  "processDefinitionUrl" : "http://localhost:8182/repository/process-definition/3",
  "activityId" : "myTask"
}
```

### 13.10. Database tables

#### 13.10.1. List of tables

```
GET management/tables
```

| Response code | Description                           |
| ------------- | ------------------------------------- |
| 200           | Indicates the request was successful. |

**Success response body:**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13[
   {
      "name":"ACT_RU_VARIABLE",
      "url":"http://localhost:8182/management/tables/ACT_RU_VARIABLE",
      "count":4528
   },
   {
      "name":"ACT_RU_EVENT_SUBSCR",
      "url":"http://localhost:8182/management/tables/ACT_RU_EVENT_SUBSCR",
      "count":3
   }

]
```

#### 13.10.2. Get a single table

```
GET management/tables/{tableName}
```

| Parameter | Required | Value  | Description                   |
| --------- | -------- | ------ | ----------------------------- |
| tableName | Yes      | String | The name of the table to get. |

**Success response body:**

```
1
2
3
4
5{
      "name":"ACT_RE_PROCDEF",
      "url":"http://localhost:8182/management/tables/ACT_RE_PROCDEF",
      "count":60
}
```

| Response code | Description                                                 |
| ------------- | ----------------------------------------------------------- |
| 200           | Indicates the table exists and the table count is returned. |
| 404           | Indicates the requested table does not exist.               |

#### 13.10.3. Get column info for a single table

```
GET management/tables/{tableName}/columns
```

| Parameter | Required | Value  | Description                   |
| --------- | -------- | ------ | ----------------------------- |
| tableName | Yes      | String | The name of the table to get. |

**Success response body:**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16
17
18
19{
   "tableName":"ACT_RU_VARIABLE",
   "columnNames":[
      "ID_",
      "REV_",
      "TYPE_",
      "NAME_"


   ],
   "columnTypes":[
      "VARCHAR",
      "INTEGER",
      "VARCHAR",
      "VARCHAR"


   ]
}
```

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates the table exists and the table column info is returned. |
| 404           | Indicates the requested table does not exist.                |

#### 13.10.4. Get row data for a single table

```
GET management/tables/{tableName}/data
```

| Parameter | Required | Value  | Description                   |
| --------- | -------- | ------ | ----------------------------- |
| tableName | Yes      | String | The name of the table to get. |

| Parameter             | Required | Value   | Description                                                  |
| --------------------- | -------- | ------- | ------------------------------------------------------------ |
| start                 | No       | Integer | Index of the first row to fetch. Defaults to 0.              |
| size                  | No       | Integer | Number of rows to fetch, starting from `start`. Defaults to 10. |
| orderAscendingColumn  | No       | String  | Name of the column to sort the resulting rows on, ascending. |
| orderDescendingColumn | No       | String  | Name of the column to sort the resulting rows on, descending. |

**Success response body:**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16
17
18
19
20
21
22
23{
  "total":3,
   "start":0,
   "sort":null,
   "order":null,
   "size":3,

   "data":[
      {
         "TASK_ID_":"2",
         "NAME_":"var1",
         "REV_":1,
         "TEXT_":"123",
         "LONG_":123,
         "ID_":"3",
         "TYPE_":"integer"
      }



   ]

}
```

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates the table exists and the table row data is returned. |
| 404           | Indicates the requested table does not exist.                |

### 13.11. Engine

#### 13.11.1. Get engine properties

```
GET management/properties
```

Returns a read-only view of the properties used internally in the engine.

**Success response body:**

```
1
2
3
4
5{
   "next.dbid":"101",
   "schema.history":"create(5.15)",
   "schema.version":"5.15"
}
```

| Response code | Description                            |
| ------------- | -------------------------------------- |
| 200           | Indicates the properties are returned. |

#### 13.11.2. Get engine info

```
GET management/engine
```

Returns a read-only view of the engine that is used in this REST-service.

**Success response body:**

```
1
2
3
4
5
6{
   "name":"default",
   "version":"5.15",
   "resourceUrl":"file://activiti/activiti.cfg.xml",
   "exception":null
}
```

| Response code | Description                            |
| ------------- | -------------------------------------- |
| 200           | Indicates the engine info is returned. |

### 13.12. Runtime

#### 13.12.1. Signal event received

```
POST runtime/signals
```

Notifies the engine that a signal event has been received, not explicitly related to a specific execution.

**Body JSON:**

```
1
2
3
4
5
6
7
8
9{
  "signalName": "My Signal",
  "tenantId" : "execute",
  "async": true,
  "variables": [
      {"name": "testVar", "value": "This is a string"}

  ]
}
```

| Parameter  | Description                                                  | Required |
| ---------- | ------------------------------------------------------------ | -------- |
| signalName | Name of the signal                                           | Yes      |
| tenantId   | ID of the tenant that the signal event should be processed in | No       |
| async      | If `true`, handling of the signal will happen asynchronously. Return code will be `202 - Accepted` to indicate the request is accepted but not yet executed. If `false`, handling the signal will be done immediately and result (`200 - OK`) will only return after this completed successfully. Defaults to `false` if omitted. | No       |
| variables  | Array of variables (in the general variables format) to use as payload to pass along with the signal. Cannot be used in case `async` is set to `true`, this will result in an error. | No       |

**Success response body:**

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicated signal has been processed and no errors occurred.  |
| 202           | Indicated signal processing is queued as a job, ready to be executed. |
| 400           | Signal not processed. The signal name is missing or variables are used together with async, which is not allowed. Response body contains additional information about the error. |

### 13.13. Jobs

#### 13.13.1. Get a single job

```
GET management/jobs/{jobId}
```

| Parameter | Required | Value  | Description               |
| --------- | -------- | ------ | ------------------------- |
| jobId     | Yes      | String | The id of the job to get. |

**Success response body:**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14{
   "id":"8",
   "url":"http://localhost:8182/management/jobs/8",
   "processInstanceId":"5",
   "processInstanceUrl":"http://localhost:8182/runtime/process-instances/5",
   "processDefinitionId":"timerProcess:1:4",
   "processDefinitionUrl":"http://localhost:8182/repository/process-definitions/timerProcess%3A1%3A4",
   "executionId":"7",
   "executionUrl":"http://localhost:8182/runtime/executions/7",
   "retries":3,
   "exceptionMessage":null,
   "dueDate":"2013-06-04T22:05:05.474+0000",
   "tenantId":null
}
```

| Response code | Description                                 |
| ------------- | ------------------------------------------- |
| 200           | Indicates the job exists and is returned.   |
| 404           | Indicates the requested job does not exist. |

#### 13.13.2. Delete a job

```
DELETE management/jobs/{jobId}
```

| Parameter | Required | Value  | Description                  |
| --------- | -------- | ------ | ---------------------------- |
| jobId     | Yes      | String | The id of the job to delete. |

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 204           | Indicates the job was found and has been deleted. Response-body is intentionally empty. |
| 404           | Indicates the requested job was not found.                   |

#### 13.13.3. Execute a single job

```
POST management/jobs/{jobId}
```

**Body JSON:**

```
1
2
3{
  "action" : "execute"
}
```

| Parameter | Description                                     | Required |
| --------- | ----------------------------------------------- | -------- |
| action    | Action to perform. Only `execute` is supported. | Yes      |

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 204           | Indicates the job was executed. Response-body is intentionally empty. |
| 404           | Indicates the requested job was not found.                   |
| 500           | Indicates the an exception occurred while executing the job. The status-description contains additional detail about the error. The full error-stacktrace can be fetched later on if needed. |

#### 13.13.4. Get the exception stacktrace for a job

```
GET management/jobs/{jobId}/exception-stacktrace
```

| Parameter | Description                              | Required |
| --------- | ---------------------------------------- | -------- |
| jobId     | Id of the job to get the stacktrace for. | Yes      |

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates the requested job was not found and the stacktrace has been returned. The response contains the raw stacktrace and always has a Content-type of `text/plain`. |
| 404           | Indicates the requested job was not found or the job doesn’t have an exception stacktrace. Status-description contains additional information about the error. |

#### 13.13.5. Get a list of jobs

```
GET management/jobs
```

| Parameter                                 | Description                                                  | Type                                                         |
| ----------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| id                                        | Only return job with the given id                            | String                                                       |
| processInstanceId                         | Only return jobs part of a process with the given id         | String                                                       |
| executionId                               | Only return jobs part of an execution with the given id      | String                                                       |
| processDefinitionId                       | Only return jobs with the given process definition id        | String                                                       |
| withRetriesLeft                           | If `true`, only return jobs with retries left. If false, this parameter is ignored. | Boolean                                                      |
| executable                                | If `true`, only return jobs which are executable. If false, this parameter is ignored. | Boolean                                                      |
| timersOnly                                | If `true`, only return jobs which are timers. If false, this parameter is ignored. Cannot be used together with `'messagesOnly'`. | Boolean                                                      |
| messagesOnly                              | If `true`, only return jobs which are messages. If false, this parameter is ignored. Cannot be used together with `'timersOnly'` | Boolean                                                      |
| withException                             | If `true`, only return jobs for which an exception occurred while executing it. If false, this parameter is ignored. | Boolean                                                      |
| dueBefore                                 | Only return jobs which are due to be executed before the given date. Jobs without duedate are never returned using this parameter. | Date                                                         |
| dueAfter                                  | Only return jobs which are due to be executed after the given date. Jobs without duedate are never returned using this parameter. | Date                                                         |
| exceptionMessage                          | Only return jobs with the given exception message            | String                                                       |
| tenantId                                  | No                                                           | String                                                       |
| Only return jobs with the given tenantId. | tenantIdLike                                                 | No                                                           |
| String                                    | Only return jobs with a tenantId like the given value.       | withoutTenantId                                              |
| No                                        | Boolean                                                      | If `true`, only returns jobs without a tenantId set. If `false`, the `withoutTenantId`parameter is ignored. |
| sort                                      | Field to sort results on, should be one of `id`, `dueDate`, `executionId`, `processInstanceId`, `retries` or `tenantId`. | String                                                       |

**Success response body:**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26{
   "data":[
      {
         "id":"13",
         "url":"http://localhost:8182/management/jobs/13",
         "processInstanceId":"5",
         "processInstanceUrl":"http://localhost:8182/runtime/process-instances/5",
         "processDefinitionId":"timerProcess:1:4",
         "processDefinitionUrl":"http://localhost:8182/repository/process-definitions/timerProcess%3A1%3A4",
         "executionId":"12",
         "executionUrl":"http://localhost:8182/runtime/executions/12",
         "retries":0,
         "exceptionMessage":"Can't find scripting engine for 'unexistinglanguage'",
         "dueDate":"2013-06-07T10:00:24.653+0000",
         "tenantId":null
      }



   ],
   "total":2,
   "start":0,
   "sort":"id",
   "order":"asc",
   "size":2
}
```

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates the requested jobs were returned.                  |
| 400           | Indicates an illegal value has been used in a url query parameter or the both `'messagesOnly'` and `'timersOnly'` are used as parameters. Status description contains additional details about the error. |

### 13.14. Users

#### 13.14.1. Get a single user

```
GET identity/users/{userId}
```

| Parameter | Required | Value  | Description                |
| --------- | -------- | ------ | -------------------------- |
| userId    | Yes      | String | The id of the user to get. |

**Success response body:**

```
1
2
3
4
5
6
7{
   "id":"testuser",
   "firstName":"Fred",
   "lastName":"McDonald",
   "url":"http://localhost:8182/identity/users/testuser",
   "email":"no-reply@activiti.org"
}
```

| Response code | Description                                  |
| ------------- | -------------------------------------------- |
| 200           | Indicates the user exists and is returned.   |
| 404           | Indicates the requested user does not exist. |

#### 13.14.2. Get a list of users

```
GET identity/users
```

| Parameter        | Description                                                  | Type   |
| ---------------- | ------------------------------------------------------------ | ------ |
| id               | Only return user with the given id                           | String |
| firstName        | Only return users with the given firstname                   | String |
| lastName         | Only return users with the given lastname                    | String |
| email            | Only return users with the given email                       | String |
| firstNameLike    | Only return userswith a firstname like the given value. Use `%` as wildcard-character. | String |
| lastNameLike     | Only return users with a lastname like the given value. Use `%` as wildcard-character. | String |
| emailLike        | Only return users with an email like the given value. Use `%` as wildcard-character. | String |
| memberOfGroup    | Only return users which are a member of the given group.     | String |
| potentialStarter | Only return users which are potential starters for a process-definition with the given id. | String |
| sort             | Field to sort results on, should be one of `id`, `firstName`, `lastname` or `email`. | String |

**Success response body:**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30{
   "data":[
      {
         "id":"anotherUser",
         "firstName":"Tijs",
         "lastName":"Barrez",
         "url":"http://localhost:8182/identity/users/anotherUser",
         "email":"no-reply@alfresco.org"
      },
      {
         "id":"kermit",
         "firstName":"Kermit",
         "lastName":"the Frog",
         "url":"http://localhost:8182/identity/users/kermit",
         "email":null
      },
      {
         "id":"testuser",
         "firstName":"Fred",
         "lastName":"McDonald",
         "url":"http://localhost:8182/identity/users/testuser",
         "email":"no-reply@activiti.org"
      }
   ],
   "total":3,
   "start":0,
   "sort":"id",
   "order":"asc",
   "size":3
}
```

| Response code | Description                                  |
| ------------- | -------------------------------------------- |
| 200           | Indicates the requested users were returned. |

#### 13.14.3. Update a user

```
PUT identity/users/{userId}
```

**Body JSON:**

```
1
2
3
4
5
6{
  "firstName":"Tijs",
  "lastName":"Barrez",
  "email":"no-reply@alfresco.org",
  "password":"pass123"
}
```

All request values are optional. For example, you can only include the *firstName* attribute in the request body JSON-object, only updating the firstName of the user, leaving all other fields unaffected. When an attribute is explicitly included and is set to null, the user-value will be updated to null. Example: `{"firstName" : null}` will clear the firstName of the user).

| Response code | Description                                              |
| ------------- | -------------------------------------------------------- |
| 200           | Indicates the user was updated.                          |
| 404           | Indicates the requested user was not found.              |
| 409           | Indicates the requested user was updated simultaneously. |

**Success response body:** see response for `identity/users/{userId}`.

#### 13.14.4. Create a user

```
POST identity/users
```

**Body JSON:**

```
{
  "id":"tijs",
  "firstName":"Tijs",
  "lastName":"Barrez",
  "email":"no-reply@alfresco.org",
  "password":"pass123"
}
```

| Response code | Description                               |
| ------------- | ----------------------------------------- |
| 201           | Indicates the user was created.           |
| 400           | Indicates the id of the user was missing. |

**Success response body:** see response for `identity/users/{userId}`.

#### 13.14.5. Delete a user

```
DELETE identity/users/{userId}
```

| Parameter | Required | Value  | Description                   |
| --------- | -------- | ------ | ----------------------------- |
| userId    | Yes      | String | The id of the user to delete. |

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 204           | Indicates the user was found and has been deleted. Response-body is intentionally empty. |
| 404           | Indicates the requested user was not found.                  |

#### 13.14.6. Get a user’s picture

```
GET identity/users/{userId}/picture
```

| Parameter | Required | Value  | Description                                |
| --------- | -------- | ------ | ------------------------------------------ |
| userId    | Yes      | String | The id of the user to get the picture for. |

**Response Body:**

The response body contains the raw picture data, representing the user’s picture. The Content-type of the response corresponds to the mimeType that was set when creating the picture.

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates the user was found and has a picture, which is returned in the body. |
| 404           | Indicates the requested user was not found or the user does not have a profile picture. Status-description contains additional information about the error. |

#### 13.14.7. Updating a user’s picture

```
GET identity/users/{userId}/picture
```

| Parameter | Required | Value  | Description                                |
| --------- | -------- | ------ | ------------------------------------------ |
| userId    | Yes      | String | The id of the user to get the picture for. |

**Request body:**

The request should be of type `multipart/form-data`. There should be a single file-part included with the binary value of the picture. On top of that, the following additional form-fields can be present:

- `mimeType`: Optional mime-type for the uploaded picture. If omitted, the default of `image/jpeg` is used as a mime-type for the picture.

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates the user was found and the picture has been updated. The response-body is left empty intentionally. |
| 404           | Indicates the requested user was not found.                  |

#### 13.14.8. List a user’s info

```
PUT identity/users/{userId}/info
```

| Parameter | Required | Value  | Description                             |
| --------- | -------- | ------ | --------------------------------------- |
| userId    | Yes      | String | The id of the user to get the info for. |

**Response Body:**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10[
   {
      "key":"key1",
      "url":"http://localhost:8182/identity/users/testuser/info/key1"
   },
   {
      "key":"key2",
      "url":"http://localhost:8182/identity/users/testuser/info/key2"
   }
]
```

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates the user was found and list of info (key and url) is returned. |
| 404           | Indicates the requested user was not found.                  |

#### 13.14.9. Get a user’s info

```
GET identity/users/{userId}/info/{key}
```

| Parameter | Required | Value  | Description                             |
| --------- | -------- | ------ | --------------------------------------- |
| userId    | Yes      | String | The id of the user to get the info for. |
| key       | Yes      | String | The key of the user info to get.        |

**Response Body:**

```
1
2
3
4
5{
   "key":"key1",
   "value":"Value 1",
   "url":"http://localhost:8182/identity/users/testuser/info/key1"
}
```

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates the user was found and the user has info for the given key.. |
| 404           | Indicates the requested user was not found or the user doesn’t have info for the given key. Status description contains additional information about the error. |

#### 13.14.10. Update a user’s info

```
PUT identity/users/{userId}/info/{key}
```

| Parameter | Required | Value  | Description                                |
| --------- | -------- | ------ | ------------------------------------------ |
| userId    | Yes      | String | The id of the user to update the info for. |
| key       | Yes      | String | The key of the user info to update.        |

**Request Body:**

```
1
2
3{
   "value":"The updated value"
}
```

**Response Body:**

```
1
2
3
4
5{
   "key":"key1",
   "value":"The updated value",
   "url":"http://localhost:8182/identity/users/testuser/info/key1"
}
```

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 200           | Indicates the user was found and the info has been updated.  |
| 400           | Indicates the value was missing from the request body.       |
| 404           | Indicates the requested user was not found or the user doesn’t have info for the given key. Status description contains additional information about the error. |

#### 13.14.11. Create a new user’s info entry

```
POST identity/users/{userId}/info
```

| Parameter | Required | Value  | Description                                |
| --------- | -------- | ------ | ------------------------------------------ |
| userId    | Yes      | String | The id of the user to create the info for. |

**Request Body:**

```
1
2
3
4{
   "key":"key1",
   "value":"The value"
}
```

**Response Body:**

```
1
2
3
4
5{
   "key":"key1",
   "value":"The value",
   "url":"http://localhost:8182/identity/users/testuser/info/key1"
}
```

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 201           | Indicates the user was found and the info has been created.  |
| 400           | Indicates the key or value was missing from the request body. Status description contains additional information about the error. |
| 404           | Indicates the requested user was not found.                  |
| 409           | Indicates there is already an info-entry with the given key for the user, update the resource instance (`PUT`). |

#### 13.14.12. Delete a user’s info

```
DELETE identity/users/{userId}/info/{key}
```

| Parameter | Required | Value  | Description                                |
| --------- | -------- | ------ | ------------------------------------------ |
| userId    | Yes      | String | The id of the user to delete the info for. |
| key       | Yes      | String | The key of the user info to delete.        |

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 204           | Indicates the user was found and the info for the given key has been deleted. Response body is left empty intentionally. |
| 404           | Indicates the requested user was not found or the user doesn’t have info for the given key. Status description contains additional information about the error. |

### 13.15. Groups

#### 13.15.1. Get a single group

```
GET identity/groups/{groupId}
```

| Parameter | Required | Value  | Description                 |
| --------- | -------- | ------ | --------------------------- |
| groupId   | Yes      | String | The id of the group to get. |

**Success response body:**

```
1
2
3
4
5
6{
   "id":"testgroup",
   "url":"http://localhost:8182/identity/groups/testgroup",
   "name":"Test group",
   "type":"Test type"
}
```

| Response code | Description                                   |
| ------------- | --------------------------------------------- |
| 200           | Indicates the group exists and is returned.   |
| 404           | Indicates the requested group does not exist. |

#### 13.15.2. Get a list of groups

```
GET identity/groups
```

| Parameter        | Description                                                  | Type   |
| ---------------- | ------------------------------------------------------------ | ------ |
| id               | Only return group with the given id                          | String |
| name             | Only return groups with the given name                       | String |
| type             | Only return groups with the given type                       | String |
| nameLike         | Only return groups with a name like the given value. Use `%` as wildcard-character. | String |
| member           | Only return groups which have a member with the given username. | String |
| potentialStarter | Only return groups which members are potential starters for a process-definition with the given id. | String |
| sort             | Field to sort results on, should be one of `id`, `name` or `type`. | String |

**Success response body:**

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15{
   "data":[
     {
        "id":"testgroup",
        "url":"http://localhost:8182/identity/groups/testgroup",
        "name":"Test group",
        "type":"Test type"
     }
   ],
   "total":3,
   "start":0,
   "sort":"id",
   "order":"asc",
   "size":3
}
```

| Response code | Description                                   |
| ------------- | --------------------------------------------- |
| 200           | Indicates the requested groups were returned. |

#### 13.15.3. Update a group

```
PUT identity/groups/{groupId}
```

**Body JSON:**

```
1
2
3
4{
   "name":"Test group",
   "type":"Test type"
}
```

All request values are optional. For example, you can only include the *name* attribute in the request body JSON-object, only updating the name of the group, leaving all other fields unaffected. When an attribute is explicitly included and is set to null, the group-value will be updated to null.

| Response code | Description                                               |
| ------------- | --------------------------------------------------------- |
| 200           | Indicates the group was updated.                          |
| 404           | Indicates the requested group was not found.              |
| 409           | Indicates the requested group was updated simultaneously. |

**Success response body:** see response for `identity/groups/{groupId}`.

#### 13.15.4. Create a group

```
POST identity/groups
```

**Body JSON:**

```
1
2
3
4
5{
   "id":"testgroup",
   "name":"Test group",
   "type":"Test type"
}
```

| Response code | Description                                |
| ------------- | ------------------------------------------ |
| 201           | Indicates the group was created.           |
| 400           | Indicates the id of the group was missing. |

**Success response body:** see response for `identity/groups/{groupId}`.

#### 13.15.5. Delete a group

```
DELETE identity/groups/{groupId}
```

| Parameter | Required | Value  | Description                    |
| --------- | -------- | ------ | ------------------------------ |
| groupId   | Yes      | String | The id of the group to delete. |

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 204           | Indicates the group was found and has been deleted. Response-body is intentionally empty. |
| 404           | Indicates the requested group was not found.                 |

#### 13.15.6. Get members in a group

There is no GET allowed on `identity/groups/members`. Use the `identity/users?memberOfGroup=sales` URL to get all users that are part of a particular group.

#### 13.15.7. Add a member to a group

```
POST identity/groups/{groupId}/members
```

| Parameter | Required | Value  | Description                             |
| --------- | -------- | ------ | --------------------------------------- |
| groupId   | Yes      | String | The id of the group to add a member to. |

**Body JSON:**

```
1
2
3{
   "userId":"kermit"
}
```

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 201           | Indicates the group was found and the member has been added. |
| 404           | Indicates the userId was not included in the request body.   |
| 404           | Indicates the requested group was not found.                 |
| 409           | Indicates the requested user is already a member of the group. |

**Response Body:**

```
1
2
3
4
5{
   "userId":"kermit",
   "groupId":"sales",
    "url":"http://localhost:8182/identity/groups/sales/members/kermit"
}
```

#### 13.15.8. Delete a member from a group

```
DELETE identity/groups/{groupId}/members/{userId}
```

| Parameter | Required | Value  | Description                                  |
| --------- | -------- | ------ | -------------------------------------------- |
| groupId   | Yes      | String | The id of the group to remove a member from. |
| userId    | Yes      | String | The id of the user to remove.                |

| Response code | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| 204           | Indicates the group was found and the member has been deleted. The response body is left empty intentionally. |
| 404           | Indicates the requested group was not found or that the user is not a member of the group. The status description contains additional information about the error. |

**Response Body:**

```
1
2
3
4
5{
   "userId":"kermit",
   "groupId":"sales",
    "url":"http://localhost:8182/identity/groups/sales/members/kermit"
}
```

## 14. CDI integration

The activiti-cdi modules leverages both the configurability of Activiti and the extensibility of cdi. The most prominent features of activiti-cdi are:

- Support for @BusinessProcessScoped beans (Cdi beans the lifecycle of which is bound to a process instance),
- A custom El-Resolver for resolving Cdi beans (including EJBs) from the process,
- Declarative control over a process instance using annotations,
- Activiti is hooked-up to the cdi event bus,
- Works with both Java EE and Java SE, works with Spring,
- Support for unit testing.

```
1
2
3
4
5<dependency>
	<groupId>org.activiti</groupId>
	<artifactId>activiti-cdi</artifactId>
	<version>5.x</version>
</dependency>
```

### 14.1. Setting up activiti-cdi

Activiti cdi can be setup in different environments. In this section we briefly walk through the configuration options.

#### 14.1.1. Looking up a Process Engine

The cdi extension needs to get access to a ProcessEngine. To achieve this, an implementation of the interface `org.activiti.cdi.spi.ProcessEngineLookup` is looked up at runtime. The cdi module ships with a default implementation named `org.activiti.cdi.impl.LocalProcessEngineLookup`, which uses the `ProcessEngines`-Utility class for looking up the ProcessEngine. In the default configuration `ProcessEngines#NAME_DEFAULT` is used to lookup the ProcessEngine. This class might be subclassed to set a custom name. NOTE: needs an `activiti.cfg.xml` configuration on the classpath.

Activiti cdi uses a java.util.ServiceLoader SPI for resolving an instance of `org.activiti.cdi.spi.ProcessEngineLookup`. In order to provide a custom implementation of the interface, we need to add a plain text file named `META-INF/services/org.activiti.cdi.spi.ProcessEngineLookup` to our deployment, in which we specify the fully qualified classname of the implementation.

|      | If you do not provide a custom `org.activiti.cdi.spi.ProcessEngineLookup` implementation, Activiti will use the default `LocalProcessEngineLookup` implementation. In that case, all you need to do is providing a activiti.cfg.xml file on the classpath (see next section). |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

#### 14.1.2. Configuring the Process Engine

Configuration depends on the selected ProcessEngineLookup-Strategy (cf. previous section). Here, we focus on the configuration options available in combination with the LocalProcessEngineLookup, which requires us to provide a Spring activiti.cfg.xml file on the classpath.

Activiti offers different ProcessEngineConfiguration implementations mostly dependent on the underlying transaction management strategy. The activiti-cdi module is not concerned with transactions, which means that potentially any transaction management strategy can be used (even the Spring transaction abstraction). As a convenience, the cdi-module provides two custom ProcessEngineConfiguration implementations:

- `org.activiti.cdi.CdiJtaProcessEngineConfiguration`: a subclass of the activiti JtaProcessEngineConfiguration, can be used if JTA-managed transactions should be used for Activiti
- `org.activiti.cdi.CdiStandaloneProcessEngineConfiguration`: a subclass of the activiti StandaloneProcessEngineConfiguration, can be used if plain JDBC transactions should be used for Activiti. The following is an example activiti.cfg.xml file for JBoss 7:

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16
17
18
19
20
21
22
23<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

	<!-- lookup the JTA-Transaction manager -->
	<bean id="transactionManager" class="org.springframework.jndi.JndiObjectFactoryBean">
		<property name="jndiName" value="java:jboss/TransactionManager"></property>
		<property name="resourceRef" value="true" />
	</bean>

	<!-- process engine configuration -->
	<bean id="processEngineConfiguration"
		class="org.activiti.cdi.CdiJtaProcessEngineConfiguration">
		<!-- lookup the default Jboss datasource -->
		<property name="dataSourceJndiName" value="java:jboss/datasources/ExampleDS" />
		<property name="databaseType" value="h2" />
		<property name="transactionManager" ref="transactionManager" />
		<!-- using externally managed transactions -->
		<property name="transactionsExternallyManaged" value="true" />
		<property name="databaseSchemaUpdate" value="true" />
	</bean>
</beans>
```

And this is how it would look like for Glassfish 3.1.1 (assuming a datasource named jdbc/activiti is properly configured):

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16
17
18
19
20
21<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

	<!-- lookup the JTA-Transaction manager -->
	<bean id="transactionManager" class="org.springframework.jndi.JndiObjectFactoryBean">
		<property name="jndiName" value="java:appserver/TransactionManager"></property>
		<property name="resourceRef" value="true" />
	</bean>

	<!-- process engine configuration -->
	<bean id="processEngineConfiguration"
		class="org.activiti.cdi.CdiJtaProcessEngineConfiguration">
		<property name="dataSourceJndiName" value="jdbc/activiti" />
		<property name="transactionManager" ref="transactionManager" />
		<!-- using externally managed transactions -->
		<property name="transactionsExternallyManaged" value="true" />
		<property name="databaseSchemaUpdate" value="true" />
	</bean>
</beans>
```

Note that the above configuration requires the "spring-context" module:

```
1
2
3
4
5<dependency>
	<groupId>org.springframework</groupId>
	<artifactId>spring-context</artifactId>
	<version>3.0.3.RELEASE</version>
</dependency>
```

The configuration in a Java SE environment looks exactly like the examples provided in section [Creating a ProcessEngine](https://www.activiti.org/userguide/#configuration), substitute "CdiStandaloneProcessEngineConfiguration" for "StandaloneProcessEngineConfiguration".

#### 14.1.3. Deploying Processes

Processes can be deployed using standard activiti-api (`RepositoryService`). In addition, activiti-cdi offers the possibility to auto-deploy processes listed in a file named `processes.xml` located top-level in the classpath. This is an example processes.xml file:

```
1
2
3
4
5
6<?xml version="1.0" encoding="utf-8" ?>
<!-- list the processes to be deployed -->
<processes>
	<process resource="diagrams/myProcess.bpmn20.xml" />
	<process resource="diagrams/myOtherProcess.bpmn20.xml" />
</processes>
```

===Contextual Process Execution with CDI

In this section we briefly look at the contextual process execution model used by the Activiti cdi extension. A BPMN business process is typically a long-running interaction, comprised of both user and system tasks. At runtime, a process is split-up into a set of individual units of work, performed by users and/or application logic. In activiti-cdi, a process instance can be associated with a cdi scope, the association representing a unit of work. This is particularly useful, if a unit of work is complex, for instance if the implementation of a UserTask is a complex sequence of different forms and "non-process-scoped" state needs to be kept during this interaction.

In the default configuration, process instances are associated with the "broadest" active scope, starting with the conversation and falling back to the request if the conversation context is not active.

#### 14.1.4. Associating a Conversation with a Process Instance

When resolving @BusinessProcessScoped beans, or injecting process variables, we rely on an existing association between an active cdi scope and a process instance. Activiti-cdi provides the `org.activiti.cdi.BusinessProcess` bean for controlling the association, most prominently:

- the *startProcessBy(…)* methods, mirroring the respective methods exposed by the Activiti `RuntimeService` allowing to start and subsequently associating a business process,
- `resumeProcessById(String processInstanceId)`, allowing to associate the process instance with the provided id,
- `resumeTaskById(String taskId)`, allowing to associate the task with the provided id (and by extension, the corresponding process instance),

Once a unit of work (for example a UserTask) is completed, the `completeTask()` method can be called to disassociate the conversation/request from the process instance. This signals Activiti that the current task is completed and makes the process instance proceed.

Note that the `BusinessProcess`-bean is a `@Named` bean, which means that the exposed methods can be invoked using expression language, for example from a JSF page. The following JSF2 snippet begins a new conversation and associates it with a user task instance, the id of which is passed as a request parameter (e.g. `pageName.jsf?taskId=XX`):

```
1
2
3
4<f:metadata>
<f:viewParam name="taskId" />
<f:event type="preRenderView" listener="#{businessProcess.startTask(taskId, true)}" />
</f:metadata>
```

#### 14.1.5. Declaratively controlling the Process

Activiti-cdi allows declaratively starting process instances and completing tasks using annotations. The`@org.activiti.cdi.annotation.StartProcess` annotation allows to start a process instance either by "key" or by "name". Note that the process instance is started *after* the annotated method returns. Example:

```
1
2
3
4
5@StartProcess("authorizeBusinessTripRequest")
public String submitRequest(BusinessTripRequest request) {
	// do some work
	return "success";
}
```

Depending on the configuration of Activiti, the code of the annotated method and the starting of the process instance will be combined in the same transaction. The `@org.activiti.cdi.annotation.CompleteTask`-annotation works in the same way:

```
1
2
3
4
5@CompleteTask(endConversation=false)
public String authorizeBusinessTrip() {
	// do some work
	return "success";
}
```

The `@CompleteTask` annotation offers the possibility to end the current conversation. The default behavior is to end the conversation after the call to Activiti returns. Ending the conversation can be disabled, as shown in the example above.

#### 14.1.6. Referencing Beans from the Process

Activiti-cdi exposes CDI beans to Activiti El, using a custom resolver. This makes it possible to reference beans from the process:

```
1
2<userTask id="authorizeBusinessTrip" name="Authorize Business Trip"
			activiti:assignee="#{authorizingManager.account.username}" />
```

Where "authorizingManager" could be a bean provided by a producer method:

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10@Inject	@ProcessVariable Object businessTripRequesterUsername;

@Produces
@Named
public Employee authorizingManager() {
	TypedQuery<Employee> query = entityManager.createQuery("SELECT e FROM Employee e WHERE e.account.username='"
		+ businessTripRequesterUsername + "'", Employee.class);
	Employee employee = query.getSingleResult();
	return employee.getManager();
}
```

We can use the same feature to call a business method of an EJB in a service task, using the `activiti:expression="myEjb.method()"`-extension. Note that this requires a `@Named`-annotation on the `MyEjb`-class.

#### 14.1.7. Working with @BusinessProcessScoped beans

Using activiti-cdi, the lifecycle of a bean can be bound to a process instance. To this extend, a custom context implementation is provided, namely the BusinessProcessContext. Instances of BusinessProcessScoped beans are stored as process variables in the current process instance. BusinessProcessScoped beans need to be PassivationCapable (for example Serializable). The following is an example of a process scoped bean:

```
1
2
3
4
5
6
7
8@Named
@BusinessProcessScoped
public class BusinessTripRequest implements Serializable {
	private static final long serialVersionUID = 1L;
	private String startDate;
	private String endDate;
	// ...
}
```

Sometimes, we want to work with process scoped beans, in the absence of an association with a process instance, for example before starting a process. If no process instance is currently active, instances of BusinessProcessScoped beans are temporarily stored in a local scope (I.e. the Conversation or the Request, depending on the context. If this scope is later associated with a business process instance, the bean instances are flushed to the process instance.

#### 14.1.8. Injecting Process Variables

Process variables are available for injection. Activiti-CDI supports

- type-safe injection of `@BusinessProcessScoped` beans using `@Inject \[additional qualifiers\] Type fieldName`
- unsafe injection of other process variables using the `@ProcessVariable(name?)` qualifier:

```
1
2@Inject @ProcessVariable Object accountNumber;
@Inject @ProcessVariable("accountNumber") Object account
```

In order to reference process variables using EL, we have similar options:

- `@Named @BusinessProcessScoped` beans can be referenced directly,
- other process variables can be referenced using the `ProcessVariables`-bean:

```
#{processVariables['accountNumber']}
```

#### 14.1.9. Receiving Process Events

[[EXPERIMENTAL\]](https://www.activiti.org/userguide/#experimental)

Activiti can be hooked-up to the CDI event-bus. This allows us to be notified of process events using standard CDI event mechanisms. In order to enable CDI event support for Activiti, enable the corresponding parse listener in the configuration:

```
1
2
3
4
5<property name="postBpmnParseHandlers">
	<list>
		<bean class="org.activiti.cdi.impl.event.CdiEventSupportBpmnParseHandler" />
	</list>
</property>
```

Now Activiti is configured for publishing events using the CDI event bus. The following gives an overview of how process events can be received in CDI beans. In CDI, we can declaratively specify event observers using the `@Observes`-annotation. Event notification is type-safe. The type of process events is `org.activiti.cdi.BusinessProcessEvent`. The following is an example of a simple event observer method:

```
1
2
3public void onProcessEvent(@Observes BusinessProcessEvent businessProcessEvent) {
	// handle event
}
```

This observer would be notified of all events. If we want to restrict the set of events the observer receives, we can add qualifier annotations:

- `@BusinessProcess`: restricts the set of events to a certain process definition. Example: `@Observes @BusinessProcess("billingProcess") BusinessProcessEvent evt`
- `@StartActivity`: restricts the set of events by a certain activity. For example: `@Observes @StartActivity("shipGoods") BusinessProcessEvent evt` is invoke whenever an activity with the id "shipGoods" is entered.
- `@EndActivity`: restricts the set of events by a certain activity. For example: `@Observes @EndActivity("shipGoods") BusinessProcessEvent evt` is invoke whenever an activity with the id "shipGoods" is left.
- `@TakeTransition`: restricts the set of events by a certain transition.
- `@CreateTask`: restricts the set of events by a certain task’s creation.
- `@DeleteTask`: restricts the set of events by a certain task’s deletion.
- `@AssignTask`: restricts the set of events by a certain task’s assignment.
- `@CompleteTask`: restricts the set of events by a certain task’s completion.

The qualifiers named above can be combined freely. For example, in order to receive all events generated when leaving the "shipGoods" activity in the "shipmentProcess", we could write the following observer method:

```
1
2
3public void beforeShippingGoods(@Observes @BusinessProcess("shippingProcess") @EndActivity("shipGoods") BusinessProcessEvent evt) {
	// handle event
}
```

In the default configuration, event listeners are invoked synchronously and in the context of the same transaction. CDI transactional observers (only available in combination with JavaEE / EJB), allow to control when the event is handed to the observer method. Using transactional observers, we can for example assure that an observer is only notified if the transaction in which the event is fired succeeds:

```
1
2
3public void onShipmentSuceeded(@Observes(during=TransactionPhase.AFTER_SUCCESS) @BusinessProcess("shippingProcess") @EndActivity("shipGoods") BusinessProcessEvent evt) {
	// send email to customer.
}
```

#### 14.1.10. Additional Features

- The ProcessEngine as well as the services are available for injection: `@Inject ProcessEngine, RepositoryService, TaskService`, …
- The current process instance and task can be injected: `@Inject ProcessInstance, Task`,
- The current business key can be injected: `@Inject @BusinessKey String businessKey`,
- The current process instance id be injected: `@Inject @ProcessInstanceId String pid`,

### 14.2. Known Limitations

Although activiti-cdi is implemented against the SPI and designed to be a "portable-extension" it is only tested using Weld.

## 15. LDAP integration

Companies often already have a user and group store in the form of an LDAP system. Since version 5.14, Activiti offers an out-of-the-box solution for easily configuring how Activiti should connect with an LDAP system.

Before Activiti 5.14, it was already possible to integrate LDAP with Activiti. However, as of 5.14, the configuration has been simplified a lot. However, the *old* way of configuring LDAP still works. More specifically, the simplified configuration is just a wrapper on top of the *old*infrastructure.

### 15.1. Usage

To add the LDAP integration code to your project, simply add the following dependency to your pom.xml:

```
1
2
3
4
5<dependency>
  <groupId>org.activiti</groupId>
  <artifactId>activiti-ldap</artifactId>
  <version>latest.version</version>
</dependency>
```

### 15.2. Use cases

The LDAP integration has currently two main use cases:

- Allow for authentication through the IdentityService. This could be useful when doing everything through the IdentityService.
- Fetching the groups of a user. This is important when for example querying tasks to see which tasks a certain user can see (i.e. tasks with a candidate group).

### 15.3. Configuration

Integrating the LDAP system with Activiti is done by injecting an instance of `org.activiti.ldap.LDAPConfigurator` in the `configurators` section of the process engine configuration. This class is highly extensible: methods can be easily overridden and many dependent beans are pluggable if the default implementation would not fit the use case.

This is an example configuration (note: of course, when creating the engine programmatically this is completely similar). Don’t worry about all the properties for now, we’ll look at them in detail in a next section.

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32    <bean id="processEngineConfiguration" class="...SomeProcessEngineConfigurationClass">
        ...
        <property name="configurators">
          <list>
              <bean class="org.activiti.ldap.LDAPConfigurator">

                <!-- Server connection params -->
                <property name="server" value="ldap://localhost" />
                <property name="port" value="33389" />
                <property name="user" value="uid=admin, ou=users, o=activiti" />
                <property name="password" value="pass" />

                <!-- Query params -->
                <property name="baseDn" value="o=activiti" />
                <property name="queryUserByUserId" value="(&(objectClass=inetOrgPerson)(uid={0}))" />
                <property name="queryUserByFullNameLike" value="(&(objectClass=inetOrgPerson)(|({0}=*{1}*)({2}=*{3}*)))" />
                <property name="queryGroupsForUser" value="(&(objectClass=groupOfUniqueNames)(uniqueMember={0}))" />

                <!-- Attribute config -->
                <property name="userIdAttribute" value="uid" />
                <property name="userFirstNameAttribute" value="cn" />
                <property name="userLastNameAttribute" value="sn" />
                <property name="userEmailAttribute" value="mail" />


                <property name="groupIdAttribute" value="cn" />
                <property name="groupNameAttribute" value="cn" />

              </bean>
          </list>
        </property>
    </bean>
```

### 15.4. Properties

Following properties can be set on `org.activiti.ldap.LDAPConfigurator`:

```
.LDAP configuration properties
```

| Property name              | Description                                                  | Type                                                         | Default value                    |
| -------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | -------------------------------- |
| server                     | The server on which the LDAP system can be reached. For example *ldap://localhost:33389* | String                                                       |                                  |
| port                       | The port on which the LDAP system is running                 | int                                                          |                                  |
| user                       | The user id that is used to connect to the LDAP system       | String                                                       |                                  |
| password                   | The password that is used to connect to the LDAP system      | String                                                       |                                  |
| initialContextFactory      | The InitialContextFactory name used to connect to the LDAP system | String                                                       | com.sun.jndi.ldap.LdapCtxFactory |
| securityAuthentication     | The value that is used for the *java.naming.security.authentication* property used to connect to the LDAP system | String                                                       | simple                           |
| customConnectionParameters | Allows to set all LDAP connection parameters which do not have a dedicated setter. See for example <http://docs.oracle.com/javase/tutorial/jndi/ldap/jndi.html>for custom properties. Such properties are for example to configure connection pooling, specific security settings, etc. All the provided parameters will be provided when creating a connection to the LDAP system. | Map<String, String>                                          |                                  |
| baseDn                     | The base *distinguished name* (DN) from which the searches for users and groups are started | String                                                       |                                  |
| userBaseDn                 | The base *distinguished name* (DN) from which the searches for users are started. If not provided, baseDn (see above) will be used | String                                                       |                                  |
| groupBaseDn                | The base *distinguished name* (DN) from which the searches for groups are started. If not provided, baseDn (see above) will be used | String                                                       |                                  |
| searchTimeLimit            | The timeout that is used when doing a search in LDAP in milliseconds | long                                                         | one hour                         |
| queryUserByUserId          | The query that is executed when searching for a user by userId. For example: (&(objectClass=inetOrgPerson)(uid={0})) Here, all the objects in LDAP with the class *inetOrgPerson* and who have the matching *uid* attribute value will be returned. As shown in the example, the user id is injected by using {0}. If setting the query alone is insufficient for your specific LDAP setup, you can alternatively plug in a different LDAPQueryBuilder, which allows for more customization than only the query. | string                                                       |                                  |
| queryUserByFullNameLike    | The query that is executed when searching for a user by full name. For example: (& (objectClass=inetOrgPerson) ( | ({0}=**{1}**)({2}=**{3}**)) ) Here, all the objects in LDAP with the class *inetOrgPerson* and who have the matching first name and last name values will be returned. Note that {0} injects the firstNameAttribute (as defined above), {1} and {3} the search text and {2} the lastNameAttribute. If setting the query alone is insufficient for your specific LDAP setup, you can alternatively plug in a different LDAPQueryBuilder, which allows for more customization than only the query. | string                           |
|                            | queryGroupsForUser                                           | The query that is executed when searching for the groups of a specific user. For example: (&(objectClass=groupOfUniqueNames)(uniqueMember={0})) Here, all the objects in LDAP with the class *groupOfUniqueNames* and where the provided DN (matching a DN for a user) is a *uniqueMember* are returned. As shown in the example, the user id is injected by using {0} If setting the query alone is insufficient for your specific LDAP setup, you can alternatively plug in a different LDAPQueryBuilder, which allows for more customization than only the query. | string                           |
|                            | userIdAttribute                                              | Name of the attribute that matches the user id. This property is used when looking for a User object and the mapping between the LDAP object and the Activiti User object is done. | string                           |
|                            | userFirstNameAttribute                                       | Name of the attribute that matches the user first name. This property is used when looking for a User object and the mapping between the LDAP object and the Activiti User object is done. | string                           |
|                            | userLastNameAttribute                                        | Name of the attribute that matches the user last name. This property is used when looking for a User object and the mapping between the LDAP object and the Activiti User object is done. | string                           |
|                            | groupIdAttribute                                             | Name of the attribute that matches the group id. This property is used when looking for a Group object and the mapping between the LDAP object and the Activiti Group object is done. | string                           |
|                            | groupNameAttribute                                           | Name of the attribute that matches the group name. This property is used when looking for a Group object and the mapping between the LDAP object and the Activiti Group object is done. | String                           |
|                            | groupTypeAttribute                                           | Name of the attribute that matches the group type. This property is used when looking for a Group object and the mapping between the LDAP object and the Activiti Group object is done. | String                           |

Following properties are when one wants to customize default behavior or introduced group caching:

| Property name                | Description                                                  | Type                                              | Default value |
| ---------------------------- | ------------------------------------------------------------ | ------------------------------------------------- | ------------- |
| ldapUserManagerFactory       | Set a custom implementation of the LDAPUserManagerFactory if the default implementation is not suitable. | instance of LDAPUserManagerFactory                |               |
| ldapGroupManagerFactory      | Set a custom implementation of the LDAPGroupManagerFactory if the default implementation is not suitable. | instance of LDAPGroupManagerFactory               |               |
| ldapMemberShipManagerFactory | Set a custom implementation of the LDAPMembershipManagerFactory if the default implementation is not suitable. Note that this is very unlikely, as membership are managed in the LDAP system itself normally. | An instance of LDAPMembershipManagerFactory       |               |
| ldapQueryBuilder             | Set a custom query builder if the default implementation is not suitable. The LDAPQueryBuilder instance is used when the LDAPUserManager or LDAPGroupManage} does an actual query against the LDAP system. The default implementation uses the properties as set on this instance such as queryGroupsForUser and queryUserById | An instance of org.activiti.ldap.LDAPQueryBuilder |               |
| groupCacheSize               | Allows to set the size of the group cache. This is an LRU cache that caches groups for users and thus avoids hitting the LDAP system each time the groups of a user needs to be known.The cache will not be instantiated if the value is less than zero. By default set to -1, so no caching is done. | int                                               | -1            |
| groupCacheExpirationTime     | Sets the expiration time of the group cache in milliseconds. When groups for a specific user are fetched, and if the group cache exists, the groups will be stored in this cache for the time set in this property. I.e. when the groups were fetched at 00:00 and the expiration time is 30 minutes, any fetch of the groups for that user after 00:30 will not come from the cache, but do a fetch again from the LDAP system. Likewise, everything group fetch for that user done between 00:00 - 00:30 will come from the cache. | long                                              | one hour      |

Note when using Active Directory: people in the Activiti forum have reported that for Activiti Directory, the *InitialDirContext* needs to be set to Context.REFERRAL. This can be passed through the customConnectionParameters map as described above.

## 16. Advanced

The following sections cover advanced use cases of Activiti, that go beyond typical execution of BPMN 2.0 processes. As such, a certain proficiency and experience with Activiti is advised to understand the topics described here.

### 16.1. Async Executor

In Activiti version 5 (starting from version 5.17.0), the Async executor was added in addition to the existing job executor. The Async Executor has proved to be more performant than the old job executor by many users of Activiti and our benchmarks.

In Activiti 6 (and later), the async executor is the only one available. For version 6, the async executor was completely refactored for optimal performance and pluggability (while still being compatible with existing API’s).

#### 16.1.1. Async Executor design

Two types of jobs exist: timers (like those belonging to a boundary event on a user task) and async continuations (belonging to a service task with the *activiti:async="true"* attribute).

**Timers** are the easiest to explain: they are persisted in the ACT_RU_TIMER_JOB table with a certain due date. There is a thread in the async executor that periodically checks if there are new timers that fire (i.e. the due date is *before* the current time). When that happens, the timer is removed and an async job is created and inserted.

An **async job** is inserted in the database during the execution of process instance steps (which means *during some API call that was made*). If the async executor is active for the current Activiti engine, the async job is actually already *locked*. This means that the job entry is inserted in the ACT_RU_JOB table and will have a *lock owner* and a *lock expiration time* set. A transaction listener that fires on a successful commit of the API call triggers the async executor of the same engine to execute the job (so the data is guaranteed to be in the database). To do this, the async executor has a (configurable) thread pool from which a thread will execute the job and continue the process asynchronously. If the Activiti engine does not have the async executor enabled, the async job is inserted in the ACT_RU_JOB table without being locked.

Similar to the thread that checks for new timers, the async executor has a thread that *acquires* new async jobs. These are jobs that are present in the table and are not locked. This thread will lock these jobs for the current Activiti engine and pass it to the async executor.

The thread pool executing the jobs uses an in-memory queue to take jobs from. When this queue is full (this is configurable), the job will be unlocked and re-inserted into its table. This way, other async executors can pick it up instead.

In case an exception happens during job execution, the async job will be transformed to a timer job with a due date. It will be picked up like a regular timer job and become an async job again, to be retried soon. When a job has been retried for a (configurable) number of times and continues to fail, the job is assumed to be *dead* and moved to the ACT_RU_DEADLETTER_JOB. The *deadletter* concept is widely used in various other systems. An admin will now need to inspect the exception for the failed job and decide what the best course of action is.

Process definitions and process instances can be suspended. Suspended jobs related to these definitions or instances are put in the ACT_RU_SUSPENDED_JOB table, to make sure the query to acquire jobs has a few as possible conditions in its where clause.

One thing that is clear from the above, for people familiar with the old implementations of the job/async executor: the main goal is to allow the *acquire queries* to be as simple as possible. In the past (before version 6), one table was used for all job types/states, which made the *where* condition large as it catered for all the use cases. This problem is now solved and our benchmarks have proved that this new design delivers better performance and is more scalable.

#### 16.1.2. Async executor configuration

The async executor is a highly configurable component. It’s always recommended to look into the default settings of the async executor and validate if they match the requirements of your processes.

Alternatively, it’s possible to extend the default implementation or implement the *org.activiti.engine.impl.asyncexecutor.AsyncExecutor*interface with your own implementation.

The following properties are available on the process engine configuration via setters:

| Name                                        | Default value | Description                                                  |
| ------------------------------------------- | ------------- | ------------------------------------------------------------ |
| asyncExecutorThreadPoolQueueSize            | 100           | The size of the queue on which jobs to be executed are placed after being acquired, before they are actually executed by a thread from the thread pool |
| asyncExecutorCorePoolSize                   | 2             | The minimal number of threads that are kept alive in the thread pool for job execution. |
| asyncExecutorMaxPoolSize                    | 10            | The maximum number of threads that are created in the thread pool for job execution. |
| asyncExecutorThreadKeepAliveTime            | 5000          | The time (in milliseconds) a thread used for job execution must be kept alive before it is destroyed. Having a setting > 0 takes resources, but in the case of many job executions it avoids creating new threads all the time. If 0, threads will be destroyed after they’ve been used for job execution. |
| asyncExecutorNumberOfRetries                | 3             | The number of times a job will be retried before it is moved to the *deadletter* table. |
| asyncExecutorMaxTimerJobsPerAcquisition     | 1             | The number of timer jobs that are acquired during one acquirement query. Default value is 1, as this lowers the potential for optimistic locking exceptions. Larger values can perform better, but the chance of optimistic locking exceptions occurring between different engines becomes larger too. |
| asyncExecutorMaxAsyncJobsDuePerAcquisition  | 1             | The number of async jobs that are acquired during one acquirement query. Default value is 1, as this lowers the potential for optimistic locking exceptions. Larger values can perform better, but the chance of optimistic locking exceptions occurring between different engines becomes larger too. |
| asyncExecutorDefaultTimerJobAcquireWaitTime | 10000         | The time (in milliseconds) the timer acquisition thread will wait to execute the next acquirement query. This happens when no new timer jobs were found or when less timer jobs have been fetched than set in *asyncExecutorMaxTimerJobsPerAcquisition*. |
| asyncExecutorDefaultAsyncJobAcquireWaitTime | 10000         | The time (in milliseconds) the async job acquisition thread will wait to execute the next acquirement query. This happens when no new async jobs were found or when less async jobs have been fetched than set in *asyncExecutorMaxAsyncJobsDuePerAcquisition*. |
| asyncExecutorDefaultQueueSizeFullWaitTime   | 0             | The time (in milliseconds) the async job (both timer and async continuations) acquisition thread will wait when the internal job queue is full to execute the next query. By default set to 0 (for backwards compatibility). Setting this property to a higher value allows the async executor to hopefully clear its queue a bit. |
| asyncExecutorTimerLockTimeInMillis          | 5 minutes     | The amount of time (in milliseconds) a timer job is locked when acquired by the async executor. During this period of time, no other async executor will try to acquire and lock this job. |
| asyncExecutorAsyncJobLockTimeInMillis       | 5 minutes     | The amount of time (in milliseconds) an async job is locked when acquired by the async executor. During this period of time, no other async executor will try to acquire and lock this job. |
| asyncExecutorSecondsToWaitOnShutdown        | 60            | The time (in seconds) that is waited to gracefully shut down the thread pool used for job execution when the a shutdown on the executor (or process engine) is requested. |
| asyncExecutorResetExpiredJobsInterval       | 60 seconds    | The amount of time (in milliseconds) that is between two consecutive checks of *expired jobs*. Expired jobs are jobs that were locked (a lock owner + time was written by some executor, but the job was never completed). During such a check, jobs that are expired are made available again, meaning the lock owner and lock time will be removed. Other executors will now be able to pick it up. A job is deemed expired if the lock time is before the current date. |
| asyncExecutorResetExpiredJobsPageSize       | 3             | The amount of jobs that are fetched at once by the *reset expired* thread of the async executor. |

#### 16.1.3. Message Queue based Async Executor

When reading the [async executor design section](https://www.activiti.org/userguide/#async_executor_design), it becomes clear that the architecture is inspired by message queues. The async executor is designed in such a way that a message queue can easily be used to take over the job of the thread pool and the handling of async jobs.

Benchmarks have shown that using a message queue is superior, throughput-wise, to the thread pool-backed async executor. However, it does come with an extra architectural component, which of course makes setup, maintenance and monitoring more complex. For many users, the performance of the thread pool-backed async executor is more than sufficient. It is nice to know however, that there is an alternative if the required performance grows.

Currently, the only option that is supported out-of-the-box is JMS and Spring. The reason for supporting Spring before anything else is because Spring has some very nice features that ease a lot of the pain when it comes to threading and dealing with multiple message consumers. However, the integration is so simple, that it can easily be ported to any message queue implementation and/or protocol (Stomp, AMPQ, etc.). Feedback is appreciated for what should be the next implementation.

When a new async job is created by the engine, a message is put on a message queue (in a transaction committed transaction listener, so we’re sure the job entry is in the database) containing the job identifier. A message consumer then takes this job identifier to fetch the job, and execute the job. The async executor will not create a thread pool anymore. It will insert and query for timers from a separate thread. When a timer fires, it is moved to the async job table, which now means a message is sent to the message queue too. The *reset expired*thread will also unlock jobs as usual, as message queues can fail too. Instead of *unlocking* a job, a message will now be resent. The async executor will not poll for async jobs anymore.

The implementation consists of two classes:

- An implementation of the *org.activiti.engine.impl.asyncexecutor.JobManager* interface that puts a message on a message queue instead of passing it to the thread pool.
- A *javax.jms.MessageListener* implementation that consumes a message from the message queue, using the job identifier in the message to fetch and execute the job.

First of all, add the *activiti-jms-spring-executor* dependency to your project:

```
1
2
3
4
5<dependency>
  <groupId>org.activiti</groupId>
  <artifactId>activiti-jms-spring-executor</artifactId>
  <version>${activiti.version}</version>
</dependency>
```

To enable the message queue based async executor, in the process engine configuration, the following needs to be done:

- *asyncExecutorActivate* must be set to *true*, as usual
- *asyncExecutorMessageQueueMode* needs to be set to *true*
- The *org.activiti.spring.executor.jms.MessageBasedJobManager* must be injected as *JobManager*

Below is a complete example of a Java based configuration, using *ActiveMQ* as message queue broker.

Some things to note:

- The *MessageBasedJobManager* expects a *JMSTemplate* to be injected that is configured with a correct *connectionFactory*.
- We’re using the *MessageListenerContainer* concept from Spring, as this simplifies threading and multiple consumers a lot.

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34
35
36
37
38
39
40
41
42
43
44
45
46
47
48
49
50
51
52
53
54
55
56
57
58
59
60
61
62
63
64
65
66
67
68
69
70
71
72
73
74@Configuration
public class SpringJmsConfig {

  @Bean
  public DataSource dataSource() {
    // Omitted
  }

  @Bean(name = "transactionManager")
  public PlatformTransactionManager transactionManager() {
    DataSourceTransactionManager transactionManager = new DataSourceTransactionManager();
    transactionManager.setDataSource(dataSource());
    return transactionManager;
  }

  @Bean
  public SpringProcessEngineConfiguration processEngineConfiguration() {
    SpringProcessEngineConfiguration configuration = new SpringProcessEngineConfiguration();
    configuration.setDataSource(dataSource());
    configuration.setTransactionManager(transactionManager());
    configuration.setDatabaseSchemaUpdate(SpringProcessEngineConfiguration.DB_SCHEMA_UPDATE_TRUE);
    configuration.setAsyncExecutorMessageQueueMode(true);
    configuration.setAsyncExecutorActivate(true);
    configuration.setJobManager(jobManager());
    return configuration;
  }

  @Bean
  public ProcessEngine processEngine() {
    return processEngineConfiguration().buildProcessEngine();
  }

  @Bean
  public MessageBasedJobManager jobManager() {
    MessageBasedJobManager jobManager = new MessageBasedJobManager();
    jobManager.setJmsTemplate(jmsTemplate());
    return jobManager;
  }

  @Bean
  public ConnectionFactory connectionFactory() {
      ActiveMQConnectionFactory activeMQConnectionFactory = new ActiveMQConnectionFactory("tcp://localhost:61616");
      activeMQConnectionFactory.setUseAsyncSend(true);
      activeMQConnectionFactory.setAlwaysSessionAsync(true);
      return new CachingConnectionFactory(activeMQConnectionFactory);
  }

  @Bean
  public JmsTemplate jmsTemplate() {
      JmsTemplate jmsTemplate = new JmsTemplate();
      jmsTemplate.setDefaultDestination(new ActiveMQQueue("activiti-jobs"));
      jmsTemplate.setConnectionFactory(connectionFactory());
      return jmsTemplate;
  }

  @Bean
  public MessageListenerContainer messageListenerContainer() {
      DefaultMessageListenerContainer messageListenerContainer = new DefaultMessageListenerContainer();
      messageListenerContainer.setConnectionFactory(connectionFactory());
      messageListenerContainer.setDestinationName("activiti-jobs");
      messageListenerContainer.setMessageListener(jobMessageListener());
      messageListenerContainer.setConcurrentConsumers(2);
      messageListenerContainer.start();
      return messageListenerContainer;
  }

  @Bean
  public JobMessageListener jobMessageListener() {
    JobMessageListener jobMessageListener = new JobMessageListener();
    jobMessageListener.setProcessEngineConfiguration(processEngineConfiguration());
    return jobMessageListener;
  }

}
```

In the code above, the *JobMessageListener* and *MessageBasedJobManager* are the only classes from the *activiti-jms-spring-executor*module. All the other code is from Spring. As such, when wanting to port this to other queues/protocols, these classes must be ported.

### 16.2. Hooking into process parsing

A BPMN 2.0 xml needs to be parsed to the Activiti internal model to be executed on the Activiti engine. This parsing happens during a deployment of the process or when a process is not found in memory, and the xml is fetched from the database.

For each of these processes, the `BpmnParser` class creates a new `BpmnParse` instance. This instance will be used as container for all things that are done during parsing. The parsing on itself is very simple: for each BPMN 2.0 element, there is a matching instance of the `org.activiti.engine.parse.BpmnParseHandler` available in the engine. As such, the parser has a map which basically maps a BPMN 2.0 element class to an instance of `BpmnParseHandler`. By default, Activiti has `BpmnParseHandler` instances to handle all supported elements and also uses it to attach execution listeners to steps of the process for creating the history.

It is possible to add custom instances of `org.activiti.engine.parse.BpmnParseHandler` to the Activiti engine. An often seen use case is for example to add execution listeners to certain steps that fire events to some queue for event processing. The history handling is done in such a way internally in Activiti. To add such custom handlers, the Activiti configuration needs to be tweaked:

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12<property name="preBpmnParseHandlers">
  <list>
    <bean class="org.activiti.parsing.MyFirstBpmnParseHandler" />
  </list>
</property>

<property name="postBpmnParseHandlers">
  <list>
    <bean class="org.activiti.parsing.MySecondBpmnParseHandler" />
    <bean class="org.activiti.parsing.MyThirdBpmnParseHandler" />
  </list>
</property>
```

The list of `BpmnParseHandler` instances that is configured in the `preBpmnParseHandlers` property are added before any of the default handlers. Likewise, the `postBpmnParseHandlers` are added after those. This can be important if the order of things matter for the logic contained in the custom parse handlers.

`org.activiti.engine.parse.BpmnParseHandler` is a simple interface:

```
1
2
3
4
5
6
7public interface BpmnParseHandler {

  Collection<Class>? extends BaseElement>> getHandledTypes();

  void parse(BpmnParse bpmnParse, BaseElement element);

}
```

The `getHandledTypes()` method returns a collection of all the types handled by this parser. The possible types are a subclass of `BaseElement`, as directed by the generic type of the collection. You can also extend the `AbstractBpmnParseHandler` class and override the `getHandledType()` method, which only returns one Class and not a collection. This class contains also some helper methods shared by many of the default parse handlers. The `BpmnParseHandler` instance will be called when the parser encounters any of the returned types by this method. In the following example, whenever a process contained in a BPMN 2.0 xml is encountered, it will execute the logic in the `executeParse` method (which is a typecasted method that replaces the regular `parse` method on the `BpmnParseHandler` interface).

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11public class TestBPMNParseHandler extends AbstractBpmnParseHandler<Process> {

  protected Class<? extends BaseElement> getHandledType() {
    return Process.class;
  }

  protected void executeParse(BpmnParse bpmnParse, Process element) {
     ..
  }

}
```

**Important note:** when writing custom parse handler, do not use any of the internal classes that are used to parse the BPMN 2.0 constructs. This will cause difficult to find bugs. The safe way to implement a custom handler is to implement the *BpmnParseHandler*interface or extends the internal abstract class *org.activiti.engine.impl.bpmn.parser.handler.AbstractBpmnParseHandler*.

It is possible (but less common) to replace the default `BpmnParseHandler` instances that are responsible for the parsing of the BPMN 2.0 elements to the internal Activiti model. This can be done by following snippet of logic:

```
1
2
3
4
5<property name="customDefaultBpmnParseHandlers">
  <list>
    ...
  </list>
</property>
```

A simple example could for example be to force all of the service tasks to be asynchronous:

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13public class CustomUserTaskBpmnParseHandler extends ServiceTaskParseHandler {

  protected void executeParse(BpmnParse bpmnParse, ServiceTask serviceTask) {

    // Do the regular stuff
    super.executeParse(bpmnParse, serviceTask);

    // Make always async
    ActivityImpl activity = findActivity(bpmnParse, serviceTask.getId());
    activity.setAsync(true);
  }

}
```

### 16.3. UUID id generator for high concurrency

In some (very) high concurrency load cases, the default id generator may cause exceptions due to not being able to fetch new id blocks quickly enough. Every process engine has one id generator. The default id generator reserves a block of ids in the database, such that no other engine will be able to use id’s from the same block. During engine operations, when the default id generator notices that the id block is used up, a new transaction is started to fetch a new block. In (very) limited use cases this can cause problems when there is a real high load. For most use cases the default id generator is more than sufficient. The default `org.activiti.engine.impl.db.DbIdGenerator` also has a property `idBlockSize` which can be configured to set the size of the reserved block of ids and to tweak the behavior of the id fetching.

The alternative to the default id generator is the `org.activiti.engine.impl.persistence.StrongUuidGenerator`, which generates a unique [UUID](http://en.wikipedia.org/wiki/Universally_unique_identifier) locally and uses that as identifier for all entities. Since the UUID is generated without the need for database access, it copes better with very high concurrency use cases. Do note that performance may differ from the default id generator (both positive and negative) depending on the machine.

The UUID generator can be configured in the activiti configuration as follows:

```
1
2
3<property name="idGenerator">
    <bean class="org.activiti.engine.impl.persistence.StrongUuidGenerator" />
</property>
```

The use of the UUID id generator depends on the following extra dependency:

```
1
2
3
4
5 <dependency>
    <groupId>com.fasterxml.uuid</groupId>
    <artifactId>java-uuid-generator</artifactId>
    <version>3.1.3</version>
</dependency>
```

### 16.4. Multitenancy

Multitenancy in general is a concept where the software is capable of serving multiple different organizations. Key is that the data is partitioned and no organization can see the data of other ones. In this context, such an organization (or a department, or a team or …) is called a *tenant*.

Note that this is fundamentally different from a multi-instance setup, where an Activiti Process Engine instance is running for each organization separately (and with a different database schema). Although Activiti is lightweight, and running a Process Engine instance doesn’t take much resources, it does add complexity and more maintenance. But, for some use cases it might be the right solution.

Multitenancy in Activiti is mainly implemented around partitioning the data. It is important to note that *Activiti does not enforce multi tenancy rules*. This means it will not verify when querying and using data whether the user doing the operation is belonging to the correct tenant. This should be done in the layer calling the Activiti engine. Activiti does make sure that tenant information can be stored and used when retrieving process data.

When deploying process definition to the Activiti Process Engine it is possible to pass a *tenant identifier*. This is a string (e.g. a UUID, department id, etc.), limited to 256 characters which is uniquely identifies the tenant:

```
1
2
3
4repositoryService.createDeployment()
            .addClassPathResource(...)
            .tenantId("myTenantId")
            .deploy();
```

Passing a tenant id during a deployment has following implications:

- All the process definitions contained in the deployment inherit the tenant identifier from this deployment.
- All process instances started from those process definitions inherit this tenant identifier from the process definition.
- All tasks created at runtime when executing the process instance inherit this tenant identifier from the process instance. Standalone tasks can have a tenant identifier too.
- All executions created during process instance execution inherit this tenant identifier from the process instance.
- Firing a signal throw event (in the process itself or through the API) can be done whilst providing a tenant identifier. The signal will only be executed in the tenant context: i.e. if there are multiple signal catch events with the same name, only the one with the correct tenant identifier will actually be called.
- All jobs (timers and async continuations) inherit the tenant identifier from either the process definition (e.g. timer start event) or the process instance (when a job is created at runtime, e.g. an async continuation). This could potentially be used for giving priority to some tenants in a custom job executor.
- All the historic entities (historic process instance, task and activities) inherit the tenant identifier from their runtime counterparts.
- As a side note, models can have a tenant identifier too (models are used e.g. by the Activiti Modeler to store BPMN 2.0 models).

To actually make use of the tenant identifier on the process data, all the query API’s have the capability to filter on tenant. For example (and can be replaced by the relevant query implementation of the other entities):

```
1
2
3
4
5runtimeService.createProcessInstanceQuery()
    .processInstanceTenantId("myTenantId")
    .processDefinitionKey("myProcessDefinitionKey")
    .variableValueEquals("myVar", "someValue")
    .list()
```

The query API’s also allow to filter on the tenant identifier with *like* semantics and also to filter out entities without tenant id.

**Important implementation detail:** due to database quirks (more specifically: null handling in unique constraints) the *default* tenant identifier value indicating *no tenant* is the **empty string**. The combination of (process definition key, process definition version, tenant identifier) needs to be unique (and there is a database constraint checking this). Also note that the tenant identifier shouldn’t be set to null, as this will affect the queries since certain databases (Oracle) treat empty string as a null value (that’s why the query *.withoutTenantId* does a check against the empty string or null). This means that the same process definition (with same process definition key) can be deployed for multiple tenants, each with their own versioning. This does not affect the usage when tenancy is not used.

**Do note that all of the above does not conflict with running multiple Activiti instances in a cluster.**

[Experimental] It is possible to change the tenant identifier by calling the *changeDeploymentTenantId(String deploymentId, String newTenantId)* method on the *repositoryService*. This will change the tenant identifier everywhere it was inherited before. This can be useful when going from a non-multitenant setup to a multitenant configuration. See the Javadoc on the method for more detailed information.

### 16.5. Execute custom SQL

The Activiti API allows for interacting with the database using a high level API. For example, for retrieving data the Query API and the Native Query API are powerful in its usage. However, for some use cases they might not be flexible enough. The following section describes how a completely custom SQL statement (select, insert, update and delete are possible) can be executed against the Activiti data store, but completely within the configured Process Engine (and thus levering the transaction setup for example).

To define custom SQL statements, the Activiti engine leverages the capabilities of its underlying framework, MyBatis. More info can be read [in the MyBatis user guide](http://mybatis.github.io/mybatis-3/java-api.html).

#### 16.5.1. Annotation based Mapped Statements

The first thing to do when using Annotation based Mapped Statements, is to create a MyBatis mapper class. For example, suppose that for some use case not the whole task data is needed, but only a small subset of it. A Mapper that could do this, looks as follows:

```
1
2
3
4
5
6public interface MyTestMapper {

    @Select("SELECT ID_ as id, NAME_ as name, CREATE_TIME_ as createTime FROM ACT_RU_TASK")
    List<Map<String, Object>> selectTasks();

}
```

This mapper must be provided to the Process Engine configuration as follows:

```
1
2
3
4
5
6
7...
<property name="customMybatisMappers">
  <set>
    <value>org.activiti.standalone.cfg.MyTestMapper</value>
  </set>
</property>
...
```

Notice that this is an interface. The underlying MyBatis framework will make an instance of it that can be used at runtime. Also notice that the return value of the method is not typed, but a list of maps (which corresponds to the list of rows with column values). Typing is possible with the MyBatis mappers if wanted.

To execute the query above, the *managementService.executeCustomSql* method must be used. This method takes in a *CustomSqlExecution* instance. This is a wrapper that hides the internal bits of the engine otherwise needed to make it work.

Unfortunately, Java generics make it a bit less readable than it could have been. The two generic types below are the mapper class and the return type class. However, the actual logic is simply to call the mapper method and return its results (if applicable).

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10CustomSqlExecution<MyTestMapper, List<Map<String, Object>>> customSqlExecution =
          new AbstractCustomSqlExecution<MyTestMapper, List<Map<String, Object>>>(MyTestMapper.class) {

  public List<Map<String, Object>> execute(MyTestMapper customMapper) {
    return customMapper.selectTasks();
  }

};

List<Map<String, Object>> results = managementService.executeCustomSql(customSqlExecution);
```

The Map entries in the list above will only contain *id, name and create time* in this case and not the full task object.

Any SQL is possible when using the approach above. Another more complex example:

```
1
2
3
4
5
6    @Select({
        "SELECT task.ID_ as taskId, variable.LONG_ as variableValue FROM ACT_RU_VARIABLE variable",
        "inner join ACT_RU_TASK task on variable.TASK_ID_ = task.ID_",
        "where variable.NAME_ = #{variableName}"
    })
    List<Map<String, Object>> selectTaskWithSpecificVariable(String variableName);
```

Using this method, the task table will be joined with the variables table. Only where the variable has a certain name is retained, and the task id and the corresponding numerical value is returned.

For a working example on using Annotation based Mapped Statements check the unit test *org.activiti.standalone.cfg.CustomMybatisMapperTest* and other classes and resources in folders src/test/java/org/activiti/standalone/cfg/ and src/test/resources/org/activiti/standalone/cfg/

#### 16.5.2. XML based Mapped Statements

When using XML based Mapped Statements, statements are defined in XML files. For the use case where not the whole task data is needed, but only a small subset of it. The XML file can look as follows:

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13<mapper namespace="org.activiti.standalone.cfg.TaskMapper">

  <resultMap id="customTaskResultMap" type="org.activiti.standalone.cfg.CustomTask">
    <id property="id" column="ID_" jdbcType="VARCHAR"/>
    <result property="name" column="NAME_" jdbcType="VARCHAR"/>
    <result property="createTime" column="CREATE_TIME_" jdbcType="TIMESTAMP" />
  </resultMap>

  <select id="selectCustomTaskList" resultMap="customTaskResultMap">
    select RES.ID_, RES.NAME_, RES.CREATE_TIME_ from ACT_RU_TASK RES
  </select>

</mapper>
```

Results are mapped to instances of *org.activiti.standalone.cfg.CustomTask* class which can look as follows:

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16public class CustomTask {

  protected String id;
  protected String name;
  protected Date createTime;

  public String getId() {
    return id;
  }
  public String getName() {
    return name;
  }
  public Date getCreateTime() {
    return createTime;
  }
}
```

Mapper XML files must be provided to the Process Engine configuration as follows:

```
1
2
3
4
5
6
7...
<property name="customMybatisXMLMappers">
  <set>
    <value>org/activiti/standalone/cfg/custom-mappers/CustomTaskMapper.xml</value>
  </set>
</property>
...
```

The statement can be executed as follows:

```
1
2
3
4
5
6
7
8List<CustomTask> tasks = managementService.executeCommand(new Command<List<CustomTask>>() {

      @SuppressWarnings("unchecked")
      @Override
      public List<CustomTask> execute(CommandContext commandContext) {
        return (List<CustomTask>) commandContext.getDbSqlSession().selectList("selectCustomTaskList");
      }
    });
```

For uses cases that require more complicated statements, XML Mapped Statements can be helpful. Since Activiti uses XML Mapped Statements internally, it’s possible to make use of the underlying capabilities.

Suppose that for some use case the ability to query attachments data is required based on id, name, type, userId, etc! To fulfill the use case a query class *AttachmentQuery* that extends *org.activiti.engine.impl.AbstractQuery* can be created as follows:

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34
35
36
37
38
39
40
41
42public class AttachmentQuery extends AbstractQuery<AttachmentQuery, Attachment> {

  protected String attachmentId;
  protected String attachmentName;
  protected String attachmentType;
  protected String userId;

  public AttachmentQuery(ManagementService managementService) {
    super(managementService);
  }

  public AttachmentQuery attachmentId(String attachmentId){
    this.attachmentId = attachmentId;
    return this;
  }

  public AttachmentQuery attachmentName(String attachmentName){
    this.attachmentName = attachmentName;
    return this;
  }

  public AttachmentQuery attachmentType(String attachmentType){
    this.attachmentType = attachmentType;
    return this;
  }

  public AttachmentQuery userId(String userId){
    this.userId = userId;
    return this;
  }

  @Override
  public long executeCount(CommandContext commandContext) {
    return (Long) commandContext.getDbSqlSession()
                   .selectOne("selectAttachmentCountByQueryCriteria", this);
  }

  @Override
  public List<Attachment> executeList(CommandContext commandContext, Page page) {
    return commandContext.getDbSqlSession()
            .selectList("selectAttachmentByQueryCriteria", this);
  }
```

Note that when extending *AbstractQuery* extended classes should pass an instance of *ManagementService* to super constructor and methods *executeCount* and *executeList* need to be implemented to call the mapped statements.

The XML file containing the mapped statements can look as follows:

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33<mapper namespace="org.activiti.standalone.cfg.AttachmentMapper">

  <select id="selectAttachmentCountByQueryCriteria" parameterType="org.activiti.standalone.cfg.AttachmentQuery" resultType="long">
    select count(distinct RES.ID_)
    <include refid="selectAttachmentByQueryCriteriaSql"/>
  </select>

  <select id="selectAttachmentByQueryCriteria" parameterType="org.activiti.standalone.cfg.AttachmentQuery" resultMap="org.activiti.engine.impl.persistence.entity.AttachmentEntity.attachmentResultMap">
    ${limitBefore}
    select distinct RES.* ${limitBetween}
    <include refid="selectAttachmentByQueryCriteriaSql"/>
    ${orderBy}
    ${limitAfter}
  </select>

  <sql id="selectAttachmentByQueryCriteriaSql">
  from ${prefix}ACT_HI_ATTACHMENT RES
  <where>
   <if test="attachmentId != null">
     RES.ID_ = #{attachmentId}
   </if>
   <if test="attachmentName != null">
     and RES.NAME_ = #{attachmentName}
   </if>
   <if test="attachmentType != null">
     and RES.TYPE_ = #{attachmentType}
   </if>
   <if test="userId != null">
     and RES.USER_ID_ = #{userId}
   </if>
  </where>
  </sql>
</mapper>
```

Capabilities such as pagination, ordering, table name prefixing are available and can be used in the statements (since the parameterType is a subclass of *AbstractQuery*). Note that to map results the predefined *org.activiti.engine.impl.persistence.entity.AttachmentEntity.attachmentResultMap* resultMap can be used.

Finally, the *AttachmentQuery* can be used as follows:

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13....
// Get the total number of attachments
long count = new AttachmentQuery(managementService).count();

// Get attachment with id 10025
Attachment attachment = new AttachmentQuery(managementService).attachmentId("10025").singleResult();

// Get first 10 attachments
List<Attachment> attachments = new AttachmentQuery(managementService).listPage(0, 10);

// Get all attachments uploaded by user kermit
attachments = new AttachmentQuery(managementService).userId("kermit").list();
....
```

For working examples on using XML Mapped Statements check the unit test *org.activiti.standalone.cfg.CustomMybatisXMLMapperTest*and other classes and resources in folders src/test/java/org/activiti/standalone/cfg/ and src/test/resources/org/activiti/standalone/cfg/

### 16.6. Advanced Process Engine configuration with a ProcessEngineConfigurator

An advanced way of hooking into the process engine configuration is through the use of a *ProcessEngineConfigurator*. The idea is that an implementation of the *org.activiti.engine.cfg.ProcessEngineConfigurator* interface is created and injected into the process engine configuration:

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15<bean id="processEngineConfiguration" class="...SomeProcessEngineConfigurationClass">

    ...

    <property name="configurators">
        <list>
            <bean class="com.mycompany.MyConfigurator">
                ...
            </bean>
        </list>
    </property>

    ...

</bean>
```

There are two methods required to implement this interface. The *configure* method, which gets a *ProcessEngineConfiguration* instance as parameter. The custom configuration can be added this way, and this method will guaranteed be called **before the process engine is created, but after all default configuration has been done**. The other method is the *getPriority* method, which allows for ordering the configurators in the case where some configurators are dependent on each other.

An example of such a configurator is the [LDAP integration](https://www.activiti.org/userguide/#chapter_ldap), where the configurator is used to replace the default user and group manager classes with one that is capable of handling an LDAP user store.	So basically a configurator allows to change or tweak the process engine quite heavily and is meant for very advanced use cases. Another example is to swap the process definition cache with a customized version:

```
1
2
3
4
5
6
7
8public class ProcessDefinitionCacheConfigurator extends AbstractProcessEngineConfigurator {

    public void configure(ProcessEngineConfigurationImpl processEngineConfiguration) {
            MyCache myCache = new MyCache();
            processEngineConfiguration.setProcessDefinitionCache(enterpriseProcessDefinitionCache);
    }

}
```

Process Engine configurators can also be auto discovered from the classpath using the [ServiceLoader](http://docs.oracle.com/javase/7/docs/api/java/util/ServiceLoader.html) approach. This means that a jar with the configurator implementation must be put on the classpath, containing a file in the *META-INF/services* folder in the jar called **org.activiti.engine.cfg.ProcessEngineConfigurator**. The content of the file needs to be the fully qualified classname of the custom implementation. When the process engine is booted, the logging will show that these configurators are found:

```
INFO  org.activiti.engine.impl.cfg.ProcessEngineConfigurationImpl  - Found 1 auto-discoverable Process Engine Configurators
INFO  org.activiti.engine.impl.cfg.ProcessEngineConfigurationImpl  - Found 1 Process Engine Configurators in total:
INFO  org.activiti.engine.impl.cfg.ProcessEngineConfigurationImpl  - class org.activiti.MyCustomConfigurator
```

Note that this ServiceLoader approach might not work in certain environments. It can be explicitly disabled using the *enableConfiguratorServiceLoader* property of the ProcessEngineConfiguration (true by default).

### 16.7. Advanced query API: seamless switching between runtime and historic task querying

One core component of any BPM user interface is the task list. Typically, end users work on open, runtime tasks, filtering their inbox with various setting. Often also the historic tasks need to be displayed in those lists, with similar filtering. To make that code-wise easier, the *TaskQuery* and *HistoricTaskInstanceQuery* both have a shared parent interface, which contains all common operations (and most of the operations are common).

This common interface is the *org.activiti.engine.task.TaskInfoQuery* class. Both *org.activiti.engine.task.Task* and *org.activiti.engine.task.HistoricTaskInstance* have a common superclass *org.activiti.engine.task.TaskInfo* (with common properties) which is returned from e.g. the *list()* method. However, Java generics are sometimes more harming than helping: if you want to use the *TaskInfoQuery* type directly, it would look like this:

```
1TaskInfoQuery<? extends TaskInfoQuery<?,?>, ? extends TaskInfo> taskInfoQuery
```

Ugh, Right. To *solve* this, a *org.activiti.engine.task.TaskInfoQueryWrapper* class that can be used to avoid the generics (the following code could come from REST code that returns a task list where the user can switch between open and completed tasks):

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12TaskInfoQueryWrapper taskInfoQueryWrapper = null;
if (runtimeQuery) {
	taskInfoQueryWrapper = new TaskInfoQueryWrapper(taskService.createTaskQuery());
} else {
	taskInfoQueryWrapper = new TaskInfoQueryWrapper(historyService.createHistoricTaskInstanceQuery());
}

List<? extends TaskInfo> taskInfos = taskInfoQueryWrapper.getTaskInfoQuery().or()
	.taskNameLike("%k1%")
	.taskDueAfter(new Date(now.getTime() + (3 * 24L * 60L * 60L * 1000L)))
.endOr()
.list();
```

### 16.8. Custom identity management by overriding standard SessionFactory

If you do not want to use a full *ProcessEngineConfigurator* implementation like in the [LDAP integration](https://www.activiti.org/userguide/#chapter_ldap), but still want to plug in your custom identity management framework, then you can also override the *SessionFactory* classes directly in the *ProcessEngineConfiguration*. In Spring this can be easily done by adding the following to the *ProcessEngineConfiguration* bean definition:

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14<bean id="processEngineConfiguration" class="...SomeProcessEngineConfigurationClass">

    ...

    <property name="customSessionFactories">
        <list>
            <bean class="com.mycompany.MyGroupManagerFactory"/>
            <bean class="com.mycompany.MyUserManagerFactory"/>
        </list>
    </property>

    ...

</bean>
```

The *MyGroupManagerFactory* and *MyUserManagerFactory* need to implement the *org.activiti.engine.impl.interceptor.SessionFactory*interface. The call to *openSession()* returns the custom class implementation that does the actual identity management. For groups this is a class that inherits from *org.activiti.engine.impl.persistence.entity.GroupEntityManager* and for managing users it must inherit from *org.activiti.engine.impl.persistence.entity.UserEntityManager*. The following code sample contains a custom manager factory for groups:

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16
17
18
19package com.mycompany;

import org.activiti.engine.impl.interceptor.Session;
import org.activiti.engine.impl.interceptor.SessionFactory;
import org.activiti.engine.impl.persistence.entity.GroupIdentityManager;

public class MyGroupManagerFactory implements SessionFactory {

	@Override
	public Class<?> getSessionType() {
		return GroupIdentityManager.class;
	}

	@Override
	public Session openSession() {
		return new MyCompanyGroupManager();
	}

}
```

The *MyCompanyGroupManager* created by the factory is doing the actual work. You do not need to override all members of *GroupEntityManager* though, just the ones required for your use case. The following sample provides an indication of how this may look like (only a selection of members are shown):

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32public class MyCompanyGroupManager extends GroupEntityManager {

    private static Logger log = LoggerFactory.getLogger(MyCompanyGroupManager.class);

    @Override
    public List<Group> findGroupsByUser(String userId) {
        log.debug("findGroupByUser called with userId: " + userId);
        return super.findGroupsByUser(userId);
    }

    @Override
    public List<Group> findGroupByQueryCriteria(GroupQueryImpl query, Page page) {
        log.debug("findGroupByQueryCriteria called, query: " + query + " page: " + page);
        return super.findGroupByQueryCriteria(query, page);
    }

    @Override
    public long findGroupCountByQueryCriteria(GroupQueryImpl query) {
        log.debug("findGroupCountByQueryCriteria called, query: " + query);
        return super.findGroupCountByQueryCriteria(query);
    }

    @Override
    public Group createNewGroup(String groupId) {
        throw new UnsupportedOperationException();
    }

    @Override
    public void deleteGroup(String groupId) {
        throw new UnsupportedOperationException();
    }
}
```

Add your own implementation in the appropriate methods to plugin your own identity management solution. You have to figure out which member of the base class must be overridden. For example the following call:

```
1long potentialOwners = identityService.createUserQuery().memberOfGroup("management").count();
```

leads to a call on the following member of the *UserIdentityManager* interface:

```
1List<User> findUserByQueryCriteria(UserQueryImpl query, Page page);
```

The code for the [LDAP integration](https://www.activiti.org/userguide/#chapter_ldap) contains full examples of how to implement this. Check out the code on Github, specifically the following classes [LDAPGroupManager](https://github.com/Activiti/Activiti/blob/master/modules/activiti-ldap/src/main/java/org/activiti/ldap/LDAPGroupManager.java) and [LDAPUserManager](https://github.com/Activiti/Activiti/blob/master/modules/activiti-ldap/src/main/java/org/activiti/ldap/LDAPUserManager.java).

### 16.9. Enable safe BPMN 2.0 xml

In most cases the BPMN 2.0 processes that are being deployed to the Activiti engine are under tight control of e.g. the development team. However, in some use cases it might be desirable to upload arbitrary BPMN 2.0 xml to the engine. In that case, take into consideration that a user with bad intentions can bring the server down as described [here](http://www.jorambarrez.be/blog/2013/02/19/uploading-a-funny-xml-can-bring-down-your-server/).

To avoid the attacks described in the link above, a property *enableSafeBpmnXml* can be set on the process engine configuration:

```
1<property name="enableSafeBpmnXml" value="true"/>
```

**By default this feature is disabled!** The reason for this is that it relies on the availability of the [StaxSource](http://download.java.net/jdk7/archive/b123/docs/api/javax/xml/transform/stax/StAXSource.html) class. Unfortunately, on some platforms (e.g. JDK 6, JBoss, etc.) this class is unavailable (due to older xml parser implementation) and thus the safe BPMN 2.0 xml feature cannot be enabled.

If the platform on which Activiti runs does support it, do enable this feature.

### 16.10. Event logging (Experimental)

As of Activiti version 5.16, an (experimental) event logging mechanism has been introduced. The logging mechanism builds upon the general-purpose [event mechanism of the Activiti engine](https://www.activiti.org/userguide/#eventDispatcher) and is disabled by default. The idea is that the events originating from the engine are caught, and a map containing all the event data (and some more) is created and provided to an *org.activiti.engine.impl.event.logger.EventFlusher* which will flush this data to somewhere else. By default, simple database-backed event handlers/flusher is used, which serializes the said map to JSON using Jackson and stores it in the database as an *EventLogEntryEntity*instance. The table required for this database logging is created by default (called *ACT_EVT_LOG*). This table can be deleted if the event logging is not used.

To enable the database logger:

```
1processEngineConfiguration.setEnableDatabaseEventLogging(true);
```

or at runtime:

```
1
2databaseEventLogger = new EventLogger(processEngineConfiguration.getClock());
runtimeService.addEventListener(databaseEventLogger);
```

The EventLogger class can be subclassed. In particular, the *createEventFlusher()* method needs to return an instance of the *org.activiti.engine.impl.event.logger.EventFlusher* interface if the default database logging is not wanted. The *managementService.getEventLogEntries(startLogNr, size);* can be used to retrieve the *EventLogEntryEntity* instances through Activiti.

It is easy to see how this table data can now be used to feed the JSON into a big data NoSQL store such as MongoDB, Elastic Search, etc. It is also easy to see that the classes used here (org.activiti.engine.impl.event.logger.EventLogger/EventFlusher and many EventHandler classes) are pluggable and can be tweaked to your own use case (eg not storing the JSON in the database, but firing it straight onto a queue or big data store).

Note that this event logging mechanism is additional to the *traditional* history manager of Activiti. Although all the data is in the database tables, it is not optimized for querying nor for easy retrieval. The real use case is audit trailing and feeding it into a big data store.

### 16.11. Disabling bulk inserts

By default, the engine will group multiple insert statements for the same database table together in a *bulk insert*, thus improving performance. This has been tested and implemented for all supported databases.

However, it could be a specific version of a supported and tested database does not allow bulk inserts (we have for example a report for DB2 on z/OS, although DB2 in general works), the bulk insert can be disabled on the process engine configuration:

```
1<property name="bulkInsertEnabled" value="false" />
```

### 16.12. Secure Scripting

**Experimental**: the secure scripting feature has been added as part of the Activiti 5.21 release.

By default, when using a [script task](https://www.activiti.org/userguide/#bpmnScriptTask), the script that is executed has similar capabilities as a Java delegate. It has full access to the JVM, can run forever (due to infinite loops) or use up a lot of memory. However, Java delegates need to be written and put on the classpath in a jar and they have a different lifecyle from a process definitions. End-users generally will not write Java delegates, as this is a typical the job of a developer.

Scripts on the other hand are part of the process definition and its lifecycle is the same. Script tasks don’t need the extra step of a jar deployment, but can be executed from the moment the process definition is deployed. Sometimes, scripts for script tasks are not written by developers. Yet, this poses a problem as stated above: a script has full access to the JVM and it is possible to block many system resources when executing the script. Allowing scripts from just about anyone is thus not a good idea.

To solve this problem, the *secure scripting* feature can be enabled. Currently, this feature is implemented for *javascript* scripting only. To enable it, add the *activiti-secure-javascript* dependency to your project. When using maven:

```
1
2
3
4
5<dependency>
    <groupId>org.activiti</groupId>
    <artifactId>activiti-secure-javascript</artifactId>
    <version>${activiti.version}</version>
</dependency>
```

Adding this dependency will transitively bring in the Rhino dependency (see <https://developer.mozilla.org/en-US/docs/Mozilla/Projects/Rhino>). Rhino is a javascript engine for the JDK. It used to be included in JDK version 6 and 7 and was superseded by the Nashorn engine. However, the Rhino project continued development after it was included in the JDK. Many features (including the ones Activiti uses to implement the secure scripting) were added afterwards. At the time of writing, the Nashorn engine **does not** have the features that are needed to implement the secure scripting feature.

This does mean that there could be (typically small) differences between scripts (for example, *importPackage* works on Rhino, but *load()*has to be used on Nashorn). The changes would be similar to switching from JDK 7 to 8 scripts.

Configuring the secure scripting is done through a dedicated *Configurator* object that is passed to the process engine configuration before the process engine is instantiated:

```
1
2
3
4
5
6
7
8SecureJavascriptConfigurator configurator = new SecureJavascriptConfigurator()
  .setWhiteListedClasses(new HashSet<String>(Arrays.asList("java.util.ArrayList")))
  .setMaxStackDepth(10)
  .setMaxScriptExecutionTime(3000L)
  .setMaxMemoryUsed(3145728L)
  .setNrOfInstructionsBeforeStateCheckCallback(10);

processEngineConfig.addConfigurator(configurator);
```

Following settings are possible:

- **enableClassWhiteListing**: When true, all classes will be blacklisted and all classes that want to be used will need to be whitelisted individually. This gives tight control over what is exposed to scripts. By default *false*.
- **whiteListedClasses**: a Set of Strings corresponding with fully qualified classnames of the classes that are allowed to be used in the script. For example, to expose the *execution* object in a script, the *org.activiti.engine.impl.persistence.entity.ExecutionEntityImpl*String needs to be added to this Set. By default *empty*.
- **maxStackDepth**: Limits the stack depth while calling functions within a script. This can be used to avoid stackoverflow exceptions that occur when recursively calling a method defined in the script. By default *-1* (disabled).
- **maxScriptExecutionTime**: The maximum time a script is allowed to run. By default *-1* (disabled).
- **maxMemoryUsed**: The maximum memory, in bytes, that the script is allowed to use. Note that the script engine itself takes a a certain amount of memory that is counted here too. By default *-1* (disabled).
- **nrOfInstructionsBeforeStateCheckCallback**: The maximum script execution time and memory usage is implemented using a callback that is called every x instructions of the script. Note that these are not script instructions, but java byte code instructions (which means one script line could be hundreds of byte code instructions). By default 100.

*Note:* the *maxMemoryUsed* setting can only be used by a JVM that supports the com.sun.management.ThreadMXBean#getThreadAllocatedBytes() method. The Oracle JDK has this.

There is also a secure variant of the ScriptExecutionListener and ScriptTaskListener: *org.activiti.scripting.secure.listener.SecureJavascriptExecutionListener* and *org.activiti.scripting.secure.listener.SecureJavascriptTaskListener*.

It’s used as follows:

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10<activiti:executionListener event="start" class="org.activiti.scripting.secure.listener.SecureJavascriptExecutionListener">
  <activiti:field name="script">
	  <activiti:string>
		  <![CDATA[
        execution.setVariable('test');
			]]>
	  </activiti:string>
	</activiti:field>
  <activiti:field name="language" stringValue="javascript" />
</activiti:executionListener>
```

For examples that demonstrate unsecure scripts and how they are made secure by the *secure scripting* feature, please check the [unit tests on Github](https://github.com/Activiti/Activiti/tree/master/modules/activiti-secure-javascript/src/test/resources)

## 17. Tooling

### 17.1. JMX

#### 17.1.1. Introduction

It is possible to connect to Activiti engine using standard Java Management Extensions (JMX) technology in order to get information or to change its behavior. Any standard JMX client can be used for that purpose. Enabling and disabling Job Executor, deploy new process definition files and deleting them are just samples of what could be done using JMX without writing a single line of code.

#### 17.1.2. Quick Start

By default JMX is not enabled. To enable JMX in its default configuration, it is enough to add the activiti-jmx jar file to your classpath, using Maven or by any other means. In case you are using Maven, you can add proper dependency by adding the following lines in your pom.xml:

```
1
2
3
4
5<dependency>
  <groupId>org.activiti</groupId>
  <artifactId>activiti-jmx</artifactId>
  <version>latest.version</version>
</dependency>
```

After adding the dependency and building process engine, the JMX connection is ready to be used. Just run jconsole available in a standard JDK distribution. In the list of local processes, you will see the JVM containing Activiti. If for any reason the proper JVM is not listed in "local processes" section, try connecting to it using this URL in "Remote Process" section:

```
service:jmx:rmi:///jndi/rmi://localhost:1099/jmxrmi/activiti
```

You can find the exact local URL in your log files. After connecting, you can see the standard JVM statistics and MBeans. You can view Activiti specific MBeans by selecting MBeans tab and select "org.activiti.jmx.Mbeans" on the right panel. By selecting any MBean, you can query information or change configuration. This snapshot shows how the jconsole looks like:

Any JMX client not limited to jconsole can be used to access MBeans. Most of data center monitoring tools have some connectors which enables them to connect to JMX MBeans.

#### 17.1.3. Attributes and operations

Here is a list available attributes and operations at this moment. This list may extend in future releases based on needs.

| MBean                   | Type      | Name                                                         | Description                                                  |
| ----------------------- | --------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| ProcessDefinitionsMBean | Attribute | processDefinitions                                           | `Id`, `Name`, `Version`, `IsSuspended` properties of deployed process definitions as a list of list of Strings |
|                         | Attribute | deployments                                                  | `Id`, `Name`, `TenantId`properties of current deployments    |
|                         | method    | getProcessDefinitionById(String id)                          | `Id`, `Name`, `Version` and `IsSuspended` properties of the process definition with given id |
|                         | method    | deleteDeployment(String id)                                  | Deletes deployment with the given `Id`                       |
|                         | method    | suspendProcessDefinitionById(String id)                      | Suspends the process definition with the given `Id`          |
|                         | method    | activatedProcessDefinitionById(String id)                    | Activates the process definition with the given `Id`         |
|                         | method    | suspendProcessDefinitionByKey(String id)                     | Suspends the process definition with the given `key`         |
|                         | method    | activatedProcessDefinitionByKey(String id)                   | Activates the process definition with the given `key`        |
|                         | method    | deployProcessDefinition(String resourceName, String processDefinitionFile) | Deploys the process definition file                          |
| JobExecutorMBean        | attribute | isJobExecutorActivated                                       | Returns true if job executor is activated, false otherwise   |
|                         | method    | setJobExecutorActivate(Boolean active)                       | Activates and Deactivates Job executor based on the given boolean |

#### 17.1.4. Configuration

JMX uses default configuration to make it easy to deploy with the most used configuration. However it is easy to change the default configuration. You can do it programmatically or via configuration file. The following code excerpt shows how this could be done in the configuration file:

```
 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14<bean id="processEngineConfiguration" class="...SomeProcessEngineConfigurationClass">
  ...
  <property name="configurators">
    <list>
	  <bean class="org.activiti.management.jmx.JMXConfigurator">

	    <property name="connectorPort" value="1912" />
        <property name="serviceUrlPath" value="/jmxrmi/activiti" />

		...
      </bean>
    </list>
  </property>
</bean>
```

This table shows what parameters you can configure along with their default values:

| Name            | Default value           | Description                                                  |
| --------------- | ----------------------- | ------------------------------------------------------------ |
| disabled        | false                   | if set, JMX will not be started even in presence of the dependency |
| domain          | org.activiti.jmx.Mbeans | Domain of MBean                                              |
| createConnector | true                    | if true, creates a connector for the started MbeanServer     |
| MBeanDomain     | DefaultDomain           | domain of MBean server                                       |
| registryPort    | 1099                    | appears in the service URL as registry port                  |
| serviceUrlPath  | /jmxrmi/activiti        | appears in the service URL                                   |
| connectorPort   | -1                      | if greater than zero, will appear in service URL as connector port |

#### 17.1.5. JMX Service URL

The JMX service URL has the following format:

```
service:jmx:rmi://<hostName>:<connectorPort>/jndi/rmi://<hostName>:<registryPort>/<serviceUrlPath>
```

`hostName` will be automatically set to the network name of the machine. `connectorPort`, `registryPort` and `serviceUrlPath` can be configured.

If `connectionPort` is less than zero, the corresponding part of service URL will be dropped and it will be simplified to:

```
service:jmx:rmi:///jndi/rmi://:<hostname>:<registryPort>/<serviceUrlPath>
```

### 17.2. Maven archetypes

#### 17.2.1. Create Test Case

During development, sometimes it is helpful to create a small test case to test an idea or a feature, before implementing it in the real application. This helps to isolate the subject under test. JUnit test cases are also the preferred tools for communicating bug reports and feature requests. Having a test case attached to a bug report or feature request jira issue, considerably reduces its fixing time.

To facilitate creation of a test case, a maven archetype is available. By use of this archetype, one can rapidly create a standard test case. The archetype should be already available in the standard repository. If not, you can easily install it in your local maven repository folder by just typing **mvn install** in **tooling/archtypes** folder.

The following command creates the unit test project:

```
mvn archetype:generate \
-DarchetypeGroupId=org.activiti \
-DarchetypeArtifactId=activiti-archetype-unittest \
-DarchetypeVersion=<current version> \
-DgroupId=org.myGroup \
-DartifactId=myArtifact
```

The effect of each parameter is explained in the following table:

| Row  | Parameter           | Explanation                                                  |
| ---- | ------------------- | ------------------------------------------------------------ |
| 1    | archetypeGroupId    | Group id of the archetype. should be **org.activiti**        |
| 2    | archetypeArtifactId | Artifact if of the archetype. should be **activiti-archetype-unittest** |
| 3aa  | archetypeVersion    | Activiti version used in the generated test project          |
| 4    | groupId             | Group id of the generated test project                       |
| 5    | artifactId          | Artifact id of the generated test project                    |

The directory structure of the generated project would be like this:

```
.
├── pom.xml
└── src
    └── test
        ├── java
        │   └── org
        │       └── myGroup
        │           └── MyUnitTest.java
        └── resources
            ├── activiti.cfg.xml
            ├── log4j.properties
            └── org
                └── myGroup
                    └── my-process.bpmn20.xml
```

You can modify java unit test case and its corresponding process model, or add new test cases and process models. If you are using the project to articulate a bug or a feature, test case should fail initially. It should then pass after the desired bug is fixed or the desired feature is implemented. Please make sure to clean the project by typing **mvn clean** before sending it.

Last updated 2017-05-25 10:52:43 EDT