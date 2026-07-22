# Guide d'installation et d'exploitation

# SailPoint IdentityIQ 8.5 sur macOS (Intel)

**Version :** 1.0\
**Auteur :** ChatGPT & Borel Saffo\
**Plateforme :** macOS + Tomcat 9 + MySQL 8.4 + Java 11

------------------------------------------------------------------------

# Architecture de SailPoint IdentityIQ

``` text
                    +----------------------+
                    |     Navigateur       |
                    +----------+-----------+
                               |
                         HTTP/HTTPS :8080
                               |
                     +---------v----------+
                     | Apache Tomcat 9    |
                     | IdentityIQ (WAR)   |
                     +---------+----------+
                               |
                     JDBC (MySQL Connector/J)
                               |
        +----------------------+----------------------+
        |                                             |
+-------v---------+                        +-----------v-----------+
| identityiq      |                        | identityiqPlugin      |
| Base principale |                        | Plugins              |
+-----------------+                        +----------------------+
        |
+-------v---------+
| identityiqah    |
| Access History  |
+-----------------+

Java 11 (Temurin) exécute Tomcat.
```

# Sommaire

1.  Présentation de SailPoint IdentityIQ
2.  Architecture
3.  Prérequis
4.  Installation Java
5.  Installation Tomcat
6.  Installation MySQL
7.  Déploiement IdentityIQ
8.  Configuration de la base
9.  Configuration Tomcat
10. Premier démarrage
11. Vérifications
12. Démarrage quotidien
13. Dépannage
14. Documentation officielle

# Présentation

SailPoint IdentityIQ est une plateforme IGA (Identity Governance &
Administration) permettant : - Gouvernance des identités -
Provisioning - Certification des accès - Segregation of Duties (SoD) -
Password Management - Connecteurs AD, LDAP, SAP, Azure AD, etc. -
Reporting - Workflows - Lifecycle Management

# Installation

> Reprendre les étapes déjà réalisées (Java 11, Tomcat 9, MySQL 8.4,
> mysql_native_password, déploiement du WAR, création des bases,
> configuration de iiq.properties, etc.).

(Le contenu détaillé est identique au guide précédent.)

# Démarrage quotidien

## Vérifier Java

``` bash
java -version
```

## Démarrer MySQL

``` bash
brew services start mysql@8.4
mysqladmin -u root -p ping
```

## Démarrer Tomcat

``` bash
brew services start tomcat@9
```

## Vérifier Tomcat

``` bash
lsof -i :8080
```

## Accéder à IdentityIQ

    http://localhost:8080/identityiq

## Arrêter les services

``` bash
brew services stop tomcat@9
brew services stop mysql@8.4
```

## Redémarrer

``` bash
brew services restart mysql@8.4
brew services restart tomcat@9
```

# Dépannage

-   UnsupportedClassVersionError → utiliser Java 11.
-   mysql_native_password DISABLED → activer dans my.cnf.
-   Address already in use → vérifier le port 8080.
-   Driver JDBC absent → vérifier mysql-connector-j-8.4.0.jar.

# Documentation officielle

## SailPoint

-   https://documentation.sailpoint.com/
-   https://developer.sailpoint.com/
-   https://community.sailpoint.com/

## Apache Tomcat

-   https://tomcat.apache.org/

## MySQL

-   https://dev.mysql.com/doc/

## Java Temurin

-   https://adoptium.net/

# Fonctionnalités IdentityIQ à explorer

-   Dashboard
-   Identities
-   Applications
-   Roles
-   Policies
-   Certifications
-   Access Requests
-   Lifecycle Manager
-   Tasks
-   Workflows
-   Reports
-   Debug
-   Global Settings
-   Audit
-   Password Management
-   Plugins
