# kettle-neo4j-integration

Integration tests for the Hop Neo4j plugins

# Configuration example

## Clone this repository

Clone this repository in the /tmp folder (or anywhere else you want):

```
$ cd /tmp
$ git clone git@github.com:mattcasters/hop-neo4j-integration.git
```

## Create a configuration file

Create a configuration file for your environment:

```
$ cp environment-conf.json.sample environment-conf.json
```

Edit the file to match your the Neo4j server on which you want to run tests.
Set the following variables in your environment:

* NEO_HOSTNAME
* NEO_USERNAME
* NEO_PASSWORD
* NEO_BOLT_PORT

Point them to your test-Neo4j instance of choice.
**WARNING** The data in this instance will be removed prior to testing.  These integration tests start from a blank database. Typically we start up Neo4j in a docker instance for this.

## Configure Hop

Create a new project:

```
$ sh hop-conf.sh -pc \
     -p hop-neo4j-integration 
     -ph /tmp/hop-neo4j-integration

Creating project 'hop-neo4j-integration'
Project 'hop-neo4j-integration' was created for home folder : /tmp/hop-neo4j-integration
Configuration file for project 'hop-neo4j-integration' was saved to : /tmp/hop-neo4j-integration/project-config.json
```

Create a new environment using the newly created Project.  
The purpose is "Integration Testing" and we specify the config file we created:

```
$ sh hop-conf.sh -ec \
    -e hop-neo4j-integration \
    -eg /tmp/hop-neo4j-integration/environment-conf.json \
    -ep hop-neo4j-integration \
    -eu "Integration Testing"

Creating environment 'hop-neo4j-integration'
Environment 'hop-neo4j-integration' was created in Hop configuration file /home/matt/tmp/config/hop-config.json
Found existing environment configuration file: /tmp/hop-neo4j-integration/environment-conf.json
  hop-neo4j-integration
    Purpose: Integration Testing
    Configuration files: 
    Project name: hop-neo4j-integration
      Config file: /tmp/hop-neo4j-integration/environment-conf.json
```

# Usage

If you have your environment configured correctly we can execute the daily run of the integration tests:

```
$ sh hop-run.sh -e hop-neo4j-integration -f tests/daily-run.hwf -r local
```

The output results file will be present in /tmp/hop-neo4j-integration/output

# Adding more tests

If you want to add your own tests, create a new folder containing one or more workflows with the filename starting with "main".
If you define a parameter called "TEST_FOLDER" in these workflows you get the current folder name which you can pass to pipeline "shared/validate-tests-in-folder.hpl".
This pipeline will run all the unit tests defined on all the pipelines in the given folder.

Typically the workflows create, merge, match/set data in Neo4j with the various plugins. The data is then read back out in a series of validation pipelines on which we define unit tests to match against golden data.

