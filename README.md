# kettle-neo4j-integration

Integration tests for the Hop Neo4j plugins

# Configuration

Set the following variables in your environment:

* NEO_HOSTNAME
* NEO_USERNAME
* NEO_PASSWORD
* NEO_BOLT_PORT

Point them to your test-Neo4j instance of choice.
**WARNING** The data in this instance will be removed prior to testing.  These integration tests start from a blank database. Typically we start up Neo4j in a docker instance for this.

# Usage

Run the workflow "daily-run.hwf" in the tests/ folder.
After the execution you can find the results in the file "output/results_<date>.csv"

# Adding more tests

If you want to add your own tests, create a new folder containing one or more workflows with the filename starting with "main".
If you define a parameter called "TEST_FOLDER" in these workflows you get the current folder name which you can pass to pipeline "shared/validate-tests-in-folder.hpl".
This pipeline will run all the unit tests defined on all the pipelines in the given folder.

Typically the workflows create, merge, match/set data in Neo4j with the various plugins. The data is then read back out in a series of validation pipelines on which we define unit tests to match against golden data.

