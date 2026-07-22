# sailpoint-iiQ

# Guide d'installation de SailPoint IdentityIQ 8.5 sur macOS (Intel)

## Prérequis

-   macOS Intel
-   Homebrew
-   Java 11 (Temurin)
-   Apache Tomcat 9
-   MySQL Server 8.4
-   Package SailPoint IdentityIQ 8.5 (`identityiq.war`)

## 1. Installer Java 11

``` bash
brew install --cask temurin@11

export JAVA_HOME="/Library/Java/JavaVirtualMachines/temurin-11.jdk/Contents/Home"
export PATH="$JAVA_HOME/bin:$PATH"

java -version
javac -version
```

## 2. Installer Tomcat 9

``` bash
brew install tomcat@9
brew services start tomcat@9
```

Créer `setenv.sh` :

``` bash
nano "$(brew --prefix tomcat@9)/libexec/bin/setenv.sh"
```

Contenu :

``` bash
#!/bin/sh
export JAVA_HOME="/Library/Java/JavaVirtualMachines/temurin-11.jdk/Contents/Home"
export CATALINA_OPTS="-Xms1024m -Xmx4096m -Dfile.encoding=UTF-8"
```

Puis :

``` bash
chmod +x "$(brew --prefix tomcat@9)/libexec/bin/setenv.sh"
brew services restart tomcat@9
```

Vérifier :

``` bash
catalina version
```

## 3. Installer MySQL

``` bash
brew install mysql@8.4
brew services start mysql@8.4
```

Activer `mysql_native_password` dans `/usr/local/etc/my.cnf` :

``` ini
[mysqld]
mysql_native_password=ON
```

Redémarrer :

``` bash
brew services restart mysql@8.4
```

## 4. Déployer IdentityIQ

Créer le dossier :

``` bash
cd "$(brew --prefix tomcat@9)/libexec/webapps"
mkdir identityiq
cp /chemin/identityiq.war identityiq/
cd identityiq
jar -xvf identityiq.war
```

Rendre le script exécutable :

``` bash
chmod +x WEB-INF/bin/iiq
```

## 5. Créer les bases

Depuis le répertoire IdentityIQ :

``` bash
mysql -u root -p < WEB-INF/database/create_identityiq_tables-8.5.mysql
```

Bases créées :

-   identityiq
-   identityiqPlugin
-   identityiqah

## 6. Configurer `iiq.properties`

Fichier :

``` text
WEB-INF/classes/iiq.properties
```

Configuration principale :

``` properties
dataSource.username=identityiq
dataSource.password=identityiq
dataSource.url=jdbc:mysql://localhost/identityiq?useServerPrepStmts=true&tinyInt1isBit=true&useSSL=false&characterEncoding=UTF-8&serverTimezone=UTC
dataSource.driverClassName=com.mysql.cj.jdbc.Driver
```

Plugins :

``` properties
pluginsDataSource.username=identityiqPlugin
pluginsDataSource.password=identityiqPlugin
```

Access History :

``` properties
dataSourceAccessHistory.username=identityiqah
dataSourceAccessHistory.password=identityiqah
```

## 7. Vérifier le driver JDBC

``` bash
find WEB-INF/lib -iname "mysql-connector*.jar"
```

Résultat attendu :

``` text
mysql-connector-j-8.4.0.jar
```

## 8. Démarrer Tomcat

``` bash
brew services restart tomcat@9
```

Vérifier :

``` bash
lsof -nP -iTCP:8080 -sTCP:LISTEN
```

Accéder à :

``` text
http://localhost:8080/identityiq
```

## 9. Console IdentityIQ

``` bash
./WEB-INF/bin/iiq
```

Applications disponibles :

-   console
-   upgrade
-   patch
-   encrypt
-   schema
-   integration

## Dépannage

### Java

    UnsupportedClassVersionError

=\> utiliser Java 11.

### MySQL

    mysql_native_password DISABLED

=\> activer le plugin dans `my.cnf` puis redémarrer MySQL.

### Tomcat

    Address already in use

=\> arrêter l'autre instance Tomcat utilisant le port 8080.

## Vérifications finales

``` bash
java -version
catalina version
mysqladmin -u root -p ping
lsof -i :8080
```

Si tout est correct, IdentityIQ est accessible via :

`http://localhost:8080/identityiq`
