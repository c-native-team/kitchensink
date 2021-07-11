# Kitchensink

This is a sample application for EAP 7.3 on OpenShift.

The original of this project is copied from
[kitchensink directory](https://github.com/jboss-developer/jboss-eap-quickstarts/tree/7.3.x-openshift/kitchensink)
on openshift branch of the quickstarts project.

# Usage

How to build:

    mvn package

How to deploy to an already running EAP:

    nvm wildfly:deploy

How to find help documentation for a plugin:

    mvn mvn help:describe -Ddetail -Dplugin=wildfly

## To deploy on OpenShift

Use "openshift" profile to rename the WAR archive to "ROOT.war".

    mnv package -Popenshift

## Run Arquillian test using an already running server

    mvn integration-test -Parq-remote

## Run Arquillian test using with EAP to be started

    JBOSS_HOME=/path/to/eap-home mvn integration-test -Parq-managed

