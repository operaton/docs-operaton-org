---

title: "Update a JBoss/Wildfly Installation from 7.6 to 7.7"

menu:
  main:
    name: "JBoss AS/Wildfly"
    identifier: "migration-guide-76-jboss"
    parent: "migration-guide-76"

---

The following steps describe how to update the Operaton artifacts on a JBoss AS
7, Wildfly 8 and Wildfly 10 server in a shared process engine scenario. For the entire
procedure, refer to the [update guide][update-guide]. If not
already done, make sure to download the [Operaton JBoss distribution](https://downloads.camunda.cloud/release/camunda-bpm/jboss/7.7/), [Operaton Wildfly 8](https://downloads.camunda.cloud/release/camunda-bpm/wildfly8/7.7/)
or [Operaton Wildfly 10 distribution](https://downloads.camunda.cloud/release/camunda-bpm/wildfly10/7.7/). In the following instructions
`$APP_SERVER` should be replaced with either `jboss` or `wildfly`, depending on
the used application server.

The update procedure takes the following steps:

1. Update the OperatonModules
2. Update Optional OperatonModules
3. Update Operaton Web Applications

Whenever the instructions are to *replace* a module, make sure to delete the previous version of the module first to avoid orphan jars.

{{< note title="Updated Wildfly Version" class="info" >}}
The pre-built Operaton distribution ships with Wildfly 8, alternatively with Wildfly 10. In particular, Operaton is supported on Wildfly 8.2 and 10.1 such that a Wildfly update is not required when migrating from 7.6 to 7.7.

See the [Wildfly migration guide](https://docs.jboss.org/author/display/CMTOOL/WildFly+8+to+10) for any Wildfly-specific migration notes and procedures.
{{< /note >}}

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
* `org/camunda/commons/camunda-commons-logging`
* `org/camunda/commons/camunda-commons-typed-values`
* `org/camunda/commons/camunda-commons-utils`

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
* `org/camunda/connect/camunda-connect-soap-http`
* `org/camunda/bpm/camunda-engine-plugin-connect`

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

## Groovy Scripting

Replace the following module from the folder `$APP_SERVER_HOME/modules/` with its new version from the folder `$APP_SERVER_DISTRIBUTION/modules/` if present:

* `org/codehaus/groovy/groovy-all`


# 3. Update Operaton Web Applications

## Update REST API

The following steps are required to update the Operaton REST API on a JBoss/Wildfly instance:

1. Undeploy an existing web application with a name like `camunda-engine-rest`
2. Download the REST API web application archive from our [Artifact Repository][engine-rest]. Alternatively, switch to the private repository for
   the enterprise version (credentials from license required). Choose the correct version named `$PLATFORM_VERSION/camunda-engine-rest-$PLATFORM_VERSION.war`.
3. Deploy the web application archive to your JBoss/Wildfly instance.

## Update Cockpit, Tasklist, and Admin

The following steps are required to update the Operaton web applications Cockpit, Tasklist, and Admin on a JBoss/Wildfly instance:

1. Undeploy an existing web application with a name like `camunda-webapp`
2. Download the Operaton web application archive from our [Artifact Repository][webapp-jboss].
   Alternatively, switch to the private repository for the enterprise version (credentials from license required).
   Choose the correct version named `$PLATFORM_VERSION/camunda-webapp-jboss.war`.
3. Deploy the web application archive to your JBoss/Wildfly instance.


[jboss-threads-to-camunda-mapping-table]: {{< ref "/update/minor/76-to-77/jboss.md#jboss-threads-to-camunda-subsystem-mapping-table" >}}
[update-guide]: {{< ref "/update/minor/76-to-77/_index.md" >}}
[jboss-distro]: https://artifacts.camunda.com/artifactory/camunda-bpm/org/camunda/bpm/jboss/camunda-bpm-jboss/
[wildfly-distro]: https://artifacts.camunda.com/artifactory/camunda-bpm/org/camunda/bpm/wildfly/camunda-bpm-wildfly/
[engine-rest]: https://artifacts.camunda.com/artifactory/camunda-bpm/org/camunda/bpm/camunda-engine-rest/
[webapp-jboss]: https://artifacts.camunda.com/artifactory/camunda-bpm/org/camunda/bpm/webapp/camunda-webapp-jboss/
[jboss-container-integration]: {{< ref "/user-guide/runtime-container-integration/jboss.md" >}}
