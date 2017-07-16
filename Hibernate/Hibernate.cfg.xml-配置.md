## Hibernate.cfg.xml 配置文件详解

此配置文件在`hibernate-release-4.3.11.Final\project\etc`可以找到


```
<!DOCTYPE hibernate-configuration PUBLIC
	"-//Hibernate/Hibernate Configuration DTD 3.0//EN"
	"http://www.hibernate.org/dtd/hibernate-configuration-3.0.dtd">

<hibernate-configuration>
	<session-factory>
		<!-- 配置数据库连接信息 -->
		<property name="connection.driver_class">com.mysql.jdbc.Driver</property>
		<property name="connection.url">jdbc:mtsql://node0:3306/oprisk</property>
		<property name="connection.username">root</property>
		<property name="connection.password">123456</property>
		<!-- 		数据库方言 -->
		<property name=""></property>
		<property name="show_sql">true</property>

	</session-factory>
</hibernate-configuration>


```
