### 使用maven进行整合
1. 添加maven依赖
(```)
<plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>

            <plugin>
                <artifactId>maven-compiler-plugin</artifactId>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.mybatis.generator</groupId>
                <artifactId>mybatis-generator-maven-plugin</artifactId>
                <version>1.3.2</version>
                <configuration>
                    <configurationFile>
                        ${basedir}/src/main/resources/generator/generatorConfig.xml
                    </configurationFile>
                    <overwrite>true</overwrite>
                    <verbose>true</verbose>
                </configuration>
                <dependencies>
                    <dependency>
                        <groupId>mysql</groupId>
                        <artifactId>mysql-connector-java</artifactId>
                        <version>5.1.29</version>
                    </dependency>
                    <dependency>
                        <groupId>tk.mybatis</groupId>
                        <artifactId>mapper</artifactId>
                        <version>4.0.0</version>
                    </dependency>
                </dependencies>
            </plugin>
        </plugins>
        (```)

2. 添加generatorConfig.xml配置文件
(```)
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">

<generatorConfiguration>
    <!--<properties resource="config.properties"/>-->

    <context id="Mysql" targetRuntime="MyBatis3Simple" defaultModelType="flat">
        <property name="beginningDelimiter" value="`"/>
        <property name="endingDelimiter" value="`"/>

        <plugin type="tk.mybatis.mapper.generator.MapperPlugin">
            <!--通用mapper路径-->
            <property name="mappers" value="tk.mybatis.mapper.common.Mapper"/>
            <!--是否区分大小写，默认false，如果数据库区分大小写，则true-->
            <property name="caseSensitive" value="true"/>
            <!--强制生成注解 @Column @Id等数据库字段映射-->
            <property name="forceAnnotation" value="true"/>
            <!--开始和结束分隔符，如果存在关键字，可以使用-->
            <property name="beginningDelimiter" value="`"/>
            <property name="endingDelimiter" value="`"/>
            <!--生成含lombok注解的model类-->
            <!--<property name="lombok" value="Getter,Setter,ToString,Accessors"/>-->
        </plugin>


        <!--数据库连接-->
        <jdbcConnection driverClass="com.mysql.jdbc.Driver"
                        connectionURL="jdbc:mysql://localhost:3306/test"
                        userId="root"
                        password="root">
        </jdbcConnection>

        <!--model生成目录-->
        <javaModelGenerator targetPackage="com.isea533.mybatis.model"
                            targetProject="src/main/java"/>


        <!--mapper.xml生成目录-->
        <sqlMapGenerator targetPackage="mapper"
                         targetProject="src/main/resources"/>


        <!--mapper的生成目录-->
        <javaClientGenerator targetPackage="com.isea533.mybatis.mapper"
                             targetProject="src/main/java"
                             type="XMLMAPPER"/>

        <table tableName="user">
            <generatedKey column="id" sqlStatement="JDBC"/>
        </table>
    </context>
</generatorConfiguration>
(```)

3. 执行maven命令
`mvn mybatis-generator:generate`

4. 添加MapperScan(mapper接口的目录)
`@MapperScan(basePackages = "com.springboot.mybatis.mapper")`

5. 添加mapper依赖
`<dependency>
  <groupId>tk.mybatis</groupId>
  <artifactId>mapper-spring-boot-starter</artifactId>
  <version>版本号</version>
</dependency>`

