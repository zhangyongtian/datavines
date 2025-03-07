# Quick start
## Environment preparation

Before installing `DataVines`, please make sure the following software is installed on your server
- `Git` to ensure smooth execution of `git clone`
- `JDK`, make sure `jdk >= 8`
- `Maven`, to ensure the smooth packaging of the project (of course, you can also upload it to the server after packaging locally)

## Download code
```shell
git clone https://github.com/datavines-ops/datavines.git
cd datavines
````

## Database preparation
The metadata of `DataVines` is stored in a relational database. Currently, `MySQL` and `PostgreSQL` are supported, and `PostgreSQL` is used by default. The following uses `MySQL` as an example to illustrate the installation steps:
- Create database `datavines`
- Execute the `script/sql/datavines-mysql.sql` script to initialize the database

> The following building also uses `MySQL` as an example


### Build Project

Build and unpack

```shell
mvn clean package -Prelease
cd datavines-dist/target
tar -zxvf datavines-1.0.0-SNAPSHOT-bin.tar.gz
````

After the decompression is complete, enter the directory
````
cd datavines-1.0.0-SNAPSHOT-bin
````
Modify configuration information
````
cd conf
vi application.yaml
````
Mainly modify database information
````
spring:
 datasource:
   driver-class-name: com.mysql.cj.jdbc.Driver
   url: jdbc:mysql://127.0.0.1:3306/datavines?useUnicode=true&characterEncoding=UTF-8
   username: root
   password: 123456
````
If you use Spark as the execution engine and submit it to Yarn for execution, you need the yarn-related configuration information in common.properties
- standalone mode
````
yarn.mode=standalone
yarn.application.status.address=http://%s:%s/ws/v1/cluster/apps/%s #The first %s needs to be replaced the ip address of yarn
yarn.resource.manager.http.address.port=8088
````
- ha mode
````
yarn.mode=ha
yarn.application.status.address=http://%s:%s/ws/v1/cluster/apps/%s
yarn.resource.manager.http.address.port=8088
yarn.resource.manager.ha.ids=192.168.0.1,192.168.0.2
````

## Start service

````
cd bin
sh datavines-daemon.sh start mysql
````

Check the log, if there is no error message in the log, and you can see `[INFO] 2022-04-10 12:29:05.447 io.datavines.server.DataVinesServer:[61] - Started DataVinesServer in 3.97 seconds (JVM running for 4.69 )`, it proves that the service has been successfully started

### Start exploring

Enter `localhost:5600` in the browser, and you will see the login interface. Enter the username & password `admin / 123456`

![DataVines架构图](../../img/login.jpg)