---

title: "Update a Glassfish Installation from 7.4 to 7.5"

menu:
  main:
    name: "Glassfish"
    identifier: "migration-guide-74-glassfish"
    parent: "migration-guide-74"

---

{{< note title="ENVIRONMENT OUT OF SUPPORT" class="warning">}}
  From Operaton onwards Glassfish Application Server is not supported anymore. Please have a look at the list of [Supported Environments]({{< ref "/introduction/supported-environments.md#container-application-server-for-runtime-components-excluding-camunda-cycle" >}}).
{{< /note >}}


The following steps describe how to update the Operaton artifacts on a Glassfish 3.1 application server in a shared process engine setting. For the entire procedure, refer to the [update guide][update-guide]. If not already done, make sure to download the [Operaton Glassfish distribution](https://artifacts.camunda.com/artifactory/camunda-bpm/org/camunda/bpm/glassfish/camunda-bpm-glassfish/).

The update procedure takes the following steps:

1. Uninstall the Operaton Applications and Archives
2. Replace Operaton Core Libraries
3. Replace Optional Operaton Libraries
4. Maintain the OperatonConfiguration
5. Maintain Process Applications
6. Install the Operaton Archive
7. Install the Web Applications

In each of the following steps, the identifiers `$*_VERSION` refer to the current version and the new versions of the artifacts.

# 1. Uninstall the Operaton Applications and Archives

First, uninstall the Operaton web applications, namely the Operaton REST API (artifact name like `camunda-engine-rest`) and the Operaton applications Cockpit, Tasklist and Admin (artifact name like `camunda-webapp`).

Uninstall the Operaton EAR. Its name should be `camunda-glassfish-ear-$PLATFORM_VERSION.ear`. Then, uninstall the Operaton job executor adapter, called `camunda-jobexecutor-rar-$PLATFORM_VERSION.rar`.

# 2. Replace Operaton Core Libraries

After shutting down the server, replace the following libraries in `$GLASSFISH_HOME/glassfish/lib` with their equivalents from `$GLASSFISH_DISTRIBUTION/modules/lib`:

* `camunda-engine-$PLATFORM_VERSION.jar`
* `camunda-bpmn-model-$PLATFORM_VERSION.jar`
* `camunda-cmmn-model-$PLATFORM_VERSION.jar`
* `camunda-dmn-model-$PLATFORM_VERSION.jar`
* `camunda-xml-model-$PLATFORM_VERSION.jar`
* `camunda-engine-dmn-$PLATFORM_VERSION.jar`
* `camunda-engine-feel-api-$PLATFORM_VERSION.jar`
* `camunda-engine-feel-juel-$PLATFORM_VERSION.jar`
* `camunda-commons-logging-$COMMONS_VERSION.jar`
* `camunda-commons-typed-values-$COMMONS_VERSION.jar`
* `camunda-commons-utils-$COMMONS_VERSION.jar`

# 3. Replace Optional Operaton Dependencies

In addition to the core libraries, there may be optional artifacts in `$GLASSFISH_HOME/glassfish/lib` for LDAP integration, Operaton Spin, and Groovy scripting. If you use any of these extensions, the following update steps apply:

## LDAP integration

Copy the following library from `$GLASSFISH_DISTRIBUTION/modules/lib` to the folder `$GLASSFISH_HOME/glassfish/lib`, if present:

* `camunda-identity-ldap-$PLATFORM_VERSION.jar`

## Operaton Connect

Copy the following library from `$GLASSFISH_DISTRIBUTION/modules/lib` to the folder `$GLASSFISH_HOME/glassfish/lib`, if present:

* `camunda-connect-core-$CONNECT_VERSION.jar`

## Operaton Spin

Copy the following library from `$GLASSFISH_DISTRIBUTION/modules/lib` to the folder `$GLASSFISH_HOME/glassfish/lib`, if present:

* `camunda-spin-core-$SPIN_VERSION.jar`

## Groovy Scripting

Copy the following library from `$GLASSFISH_DISTRIBUTION/modules/lib` to the folder `$GLASSFISH_HOME/glassfish/lib`, if present:

* `groovy-all-$GROOVY_VERSION.jar`

# 4. Maintain the OperatonConfiguration

If you have previously replaced the default Operatonconfiguration with a custom configuration following any of the ways outlined in the [deployment descriptor reference][configuration-location], it may be necessary to restore this configuration. This can be done by repeating the configuration replacement steps for the updated platform.

# 5. Maintain Process Applications

This section describes changes in the internal API of the engine. If you have implemented one of the APIs and replaced the default implementation then you have to adjust your custom implementation. Otherwise, you can skip this section.

## Incident Handler

The interface of an [Incident Handler]({{< ref "/user-guide/process-engine/incidents.md" >}}) has changed. Instead of a long parameter list, the methods pass a context object which bundles all required information, like process definition id, execution id and tenant id. Since the existing methods have been overridden, custom implementations of an incident handler have to be adjusted.

## Correlation Handler

A new method has been added to the interface of a {{< javadocref page="org/camunda/bpm/engine/impl/runtime/CorrelationHandler.html" text="Correlation Handler" >}}. The new method `correlateStartMessage()` allows to explicitly trigger a message start event of a process definition. If the default implementation is replaced by a custom one then it has to be adjusted.

## Job Handler

The interface of a {{< javadocref page="org/camunda/bpm/engine/impl/jobexecutor/JobHandler.html" text="Job Handler" >}} has changed to support multi-tenancy and separate the parsing of the configuration.

# 6. Install the Operaton Archive

First, install the Operaton job executor resource adapter, namely the file `$GLASSFISH_DISTRIBUTION/modules/camunda-jobexecutor-rar-$PLATFORM_VERSION.rar`. Then install the Operaton EAR, i.e., the file `$GLASSFISH_DISTRIBUTION/modules/camunda-glassfish-ear-$PLATFORM_VERSION.ear`.

# 7. Install the Web Applications

## REST API

The following steps are required to update the Operaton REST API on a Glassfish instance:

1. Download the REST API web application archive from our [Artifact Repository](https://artifacts.camunda.com/artifactory/camunda-bpm/org/camunda/bpm/camunda-engine-rest/). Alternatively, switch to the private repository for the enterprise version (User and password from license required). Choose the correct version named `$PLATFORM_VERSION/camunda-engine-rest-$PLATFORM_VERSION.war`.
2. Deploy the web application archive to your Glassfish instance.

## Cockpit, Tasklist, and Admin

The following steps are required to update the Operaton web applications Cockpit, Tasklist, and Admin on a Glassfish instance:

1. Download the Operaton web application archive from our [Artifact Repository](https://artifacts.camunda.com/artifactory/camunda-bpm/org/camunda/bpm/webapp/camunda-webapp-glassfish/). Alternatively, switch to the private repository for the enterprise version (User and password from license required). Choose the correct version named `$PLATFORM_VERSION/camunda-webapp-glassfish-$PLATFORM_VERSION.war`.
2. Deploy the web application archive to your Glassfish instance.

[configuration-location]: {{< ref "/reference/deployment-descriptors/descriptors/bpm-platform-xml.md" >}}
[update-guide]: {{< ref "/update/minor/74-to-75/_index.md" >}}
