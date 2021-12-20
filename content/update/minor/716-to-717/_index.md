---

title: "Update from 7.16 to 7.17"
weight: 7
layout: "single"

menu:
  main:
    name: "7.16 to 7.17"
    identifier: "migration-guide-717"
    parent: "migration-guide-minor"
    pre: "Update from `7.16.x` to `7.17.0`."

---

This document guides you through the update from Camunda Platform `7.16.x` to `7.17.0` and covers the following use cases:

1. For administrators and developers: [Database updates](#database-updates)
1. For administrators and developers: [Full distribution update](#full-distribution)
1. For administrators: [Standalone web application](#standalone-web-application)
1. For administrators and developers: [Task Worker Metrics](#task-worker-metrics)
1. For developers: [Spin configuration options](#set-variables-on-process-instance-migration)

This guide covers mandatory migration steps as well as optional considerations for the initial configuration of new 
functionality included in Camunda Platform 7.17.

# Database updates

Every Camunda installation requires a database schema update. Check our [database schema update guide]({{< ref "/installation/database-schema.md#update" >}}) 
for further instructions.

# Full distribution

This section is applicable if you installed the 
[Full Distribution]({{< ref "/introduction/downloading-camunda.md#full-distribution" >}}) 
with a **shared process engine**.

The following steps are required:

1. Update the Camunda libraries and applications inside the application server.
2. Migrate custom process applications.

Before starting, ensure you have downloaded the Camunda Platform 7.17 distribution for the application server 
you use. This contains the SQL scripts and libraries required for the update. This guide assumes you have unpacked the 
distribution to a path named `$DISTRIBUTION_PATH`.

## Camunda libraries and applications

Choose the application server you are working with from the following list:

* [JBoss EAP 6 or Wildfly / JBoss EAP 7]({{< ref "/update/minor/716-to-717/jboss.md" >}})
* [Apache Tomcat]({{< ref "/update/minor/716-to-717/tomcat.md" >}})
* [Oracle WebLogic]({{< ref "/update/minor/716-to-717/wls.md" >}})
* [IBM WebSphere]({{< ref "/update/minor/716-to-717/was.md" >}})

## Custom process applications

For every process application, the Camunda dependencies should be updated to the new version. Which dependencies you 
have is application- and server-specific. Typically, the dependencies consist of any of the following:

* `camunda-engine-spring`
* `camunda-engine-cdi`
* `camunda-ejb-client`

There are no new mandatory dependencies for process applications.

# Standalone web application

If you use a standalone web application, replace the current `.war` artifact by its new version. 
Take the following steps to complete the update:

1. Undeploy the current version of the standalone web application.
2. Update the database to the new schema as described in the [database update](#database-updates) section.
3. Configure the database as described in the [installation]({{< ref "/installation/standalone-webapplication.md#database-configuration" >}})
   section.
4. Deploy the new and configured standalone web application to the server.

# Task worker metrics

Starting from version 7.17, the task worker metrics are displayed by default.
If this causes slow page loading, you can turn it off in the [admin webapp configuration.]({{< ref "/webapps/admin/configuration.md#task-worker-metrics" >}}).

# Spin configuration options

Version 7.17 introduces configuration properties for the Spin `DomXmlDataFormat` module. The `DomXmlDataFormat`
configuration properties provide options to toggle **External XML Entity (XXE)** processing, as well as secure processing
for the Spin XML parser.

By default, we disabled XXE processing, and enabled secure processing of XML documents to protect the Spin XML
parser against [XXE attacks](https://en.wikipedia.org/wiki/XML_external_entity_attack) and
[Billion laughs attacks](https://en.wikipedia.org/wiki/Billion_laughs_attack).

You can restore the old behavior by passing the appropriate [configuration properties to the Spin process engine plugin][spin-config].

[spin-config]: {{< ref "/user-guide/data-formats/configuring-spin-integration.md#configuration-properties-of-the-spin-plugin" >}}