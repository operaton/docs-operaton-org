---

title: "Update a JBoss/Wildfly Installation from 7.12 to 7.13"

menu:
  main:
    name: "JBoss AS/Wildfly"
    identifier: "migration-guide-713-jboss"
    parent: "migration-guide-713"

---

The following steps describe how to update the Operaton artifacts on a JBoss AS
7 and Wildfly server in a shared process engine scenario. For the entire
procedure, refer to the [update guide][update-guide]. If not
already done, make sure to download the [Operaton JBoss distribution](https://downloads.camunda.cloud/release/camunda-bpm/jboss/7.13/)
or [Operaton Wildfly distribution](https://downloads.camunda.cloud/release/camunda-bpm/wildfly/7.13/). In the following instructions
`$APP_SERVER` should be replaced with either `jboss` or `wildfly`, depending on
the used application server.

The update procedure takes the following steps:

1. Update the OperatonModules
2. Update Optional OperatonModules
3. Update Operaton Web Applications

Whenever the instructions are to *replace* a module, make sure to delete the previous version of the module first to avoid orphan jars.

# 1. Update the OperatonModules

Replace the following modules from the folder `$APP_SERVER_HOME/modules/` with their new versions from the folder `$APP_SERVER_DISTRIBUTION/modules/`:

* `org/camunda/bpm/camunda-engine`
* `org/camunda/bpm/$APP_SERVER/camunda-$APP_SERVER-subsystem`
* `org/camunda/bpm/model/camunda-bpmn-model`
* `org/camunda/bpm/model/camunda-cmmn-model`
* `org/camunda/bpm/model/camunda-dmn-model`
* `org/camunda/bpm/model/camunda-xml-model`
* `org/camunda/bpm/dmn/camunda-engine-dmn`
* `org/camunda/bpm/dmn/camunda-engine-feel-api`
* `org/camunda/bpm/dmn/camunda-engine-feel-juel`
* `org/camunda/bpm/dmn/camunda-engine-feel-scala`
* `org/camunda/template-engines/camunda-template-engines-freemarker`
* `org/camunda/commons/camunda-commons-logging`
* `org/camunda/commons/camunda-commons-typed-values`
* `org/camunda/commons/camunda-commons-utils`
* `org/camunda/feel/feel-engine`
* `org/freemarker/freemarker`
* `org/mybatis/mybatis`

# 2. Update Optional OperatonModules

In addition to the core modules, there may be optional artifacts in `$APP_SERVER_HOME/modules/` for LDAP integration, Operaton Connect, Operaton Spin, and Groovy scripting.
If you use any of these extensions, the following update steps apply:

## LDAP Integration

Replace the following module from the folder `$APP_SERVER_HOME/modules/` with its new version from the folder `$APP_SERVER_DISTRIBUTION/modules/`, if present:

* `org/camunda/bpm/identity/camunda-identity-ldap`

## Operaton Connect

Replace the following modules from the folder `$APP_SERVER_HOME/modules/` with their new versions from the folder `$APP_SERVER_DISTRIBUTION/modules/`, if present:

* `org/camunda/connect/camunda-connect-core`
* `org/camunda/connect/camunda-connect-http`
* `org/camunda/connect/camunda-connect-soap-http-client`
* `org/camunda/bpm/camunda-engine-plugin-connect`

Additionally, also replace the following dependent modules:

* `org/apache/httpcomponents/httpclient`
* `org/apache/httpcomponents/httpcore`
* `commons-codec/commons-codec`

## Operaton Spin

Replace the following modules from the folder `$APP_SERVER_HOME/modules/` with their new versions from the folder `$APP_SERVER_DISTRIBUTION/modules/`, if present:

* `org/camunda/spin/camunda-spin-core`
* `org/camunda/spin/camunda-spin-dataformat-json-jackson`
* `org/camunda/spin/camunda-spin-dataformat-xml-dom`
* `org/camunda/bpm/camunda-engine-plugin-spin`

Additionally, also replace the following dependent modules:

* `com/fasterxml/jackson/core/jackson-annotations`
* `com/fasterxml/jackson/core/jackson-core`
* `com/fasterxml/jackson/core/jackson-databind`
* `com/jayway/jsonpath/json-path`
* `net/minidev/accessors-smart`
* `net/minidev/json-smart`

# 3. Update Operaton Web Applications

## Update REST API

The following steps are required to update the Operaton REST API on a JBoss/Wildfly instance:

1. Undeploy an existing web application with a name like `camunda-engine-rest`
2. Download the REST API web application archive from our [Artifact Repository][engine-rest]. Alternatively, switch to the private repository for
   the enterprise version (credentials from license required). Choose the correct version named `$PLATFORM_VERSION/camunda-engine-rest-$PLATFORM_VERSION-$CLASSIFIER.war`.
3. Deploy the web application archive to your JBoss/Wildfly instance.

## Update Cockpit, Tasklist, and Admin

The following steps are required to update the Operaton web applications Cockpit, Tasklist, and Admin on a JBoss/Wildfly instance:

1. Undeploy an existing web application with a name like `camunda-webapp`
2. Download the Operaton web application archive from our [Artifact Repository][webapp-jboss].
   Alternatively, switch to the private repository for the enterprise version (credentials from license required).
   Choose the correct version named `$PLATFORM_VERSION/camunda-webapp-jboss.war`.
3. Deploy the web application archive to your JBoss/Wildfly instance.


[update-guide]: {{< ref "/update/minor/712-to-713/_index.md" >}}
[engine-rest]: https://artifacts.camunda.com/artifactory/camunda-bpm/org/camunda/bpm/camunda-engine-rest/7.13.0/
[webapp-jboss]: https://artifacts.camunda.com/artifactory/camunda-bpm/org/camunda/bpm/webapp/camunda-webapp-jboss/7.13.0/camunda-webapp-jboss-7.13.0.war
[jackson-update]: {{< ref "/update/minor/712-to-713/_index.md#jackson-version-update" >}}
