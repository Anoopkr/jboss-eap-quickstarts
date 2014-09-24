tasks: Testing JPA with Arquillian
==================================
Author: Oliver Kiss, Lukas Fryc  
Level: Intermediate  
Technologies: JPA, Arquillian  
Summary: Demonstrates testing JPA using Arquillian  
Target Product: EAP  
Product Versions: EAP 6.1, EAP 6.2, EAP 6.3  
Source: <https://github.com/jboss-developer/jboss-eap-quickstarts/>  


What is it?
-----------

This project demonstrates how to use JPA 2.0 in Red Hat JBoss Enterprise Application Platform. 

It includes a persistence unit and some sample persistence code to introduce you database access in enterprise Java. 

It does not contain an user interface layer. The purpose of the project is to show you how to test JPA with Arquillian.

_Note: This quickstart uses the H2 database included with JBoss EAP 6. It is a lightweight, relational example datasource that is used for examples only. It is not robust or scalable and should NOT be used in a production environment!_

System requirements
-------------------

The application this project produces is designed to be run on Red Hat JBoss Enterprise Application Platform 6.1 or later. 

All you need to build this project is Java 6.0 (Java SDK 1.6) or later, Maven 3.0 or later.

_Note: This quickstart uses the H2 database included with JBoss EAP 6. It is a lightweight, relational example datasource that is used for examples only. It is not robust or scalable and should NOT be used in a production environment!_
 
Configure Maven
---------------

If you have not yet done so, you must [Configure Maven](https://github.com/jboss-developer/jboss-developer-shared-resources/blob/master/guides/CONFIGURE_MAVEN.md#configure-maven-to-build-and-deploy-the-quickstarts) before testing the quickstarts.


Start the JBoss EAP Server
-------------------------

1. Open a command prompt and navigate to the root of the JBoss EAP directory.
2. The following shows the command line to start the server:

        For Linux:   EAP_HOME/bin/standalone.sh
        For Windows: EAP_HOME\bin\standalone.bat


Run the Arquillian Tests 
-------------------------

This quickstart provides Arquillian tests. By default, these tests are configured to be skipped as Arquillian tests require the use of a container. 

_NOTE: The following commands assume you have configured your Maven user settings. If you have not, you must include Maven setting arguments on the command line. See [Run the Arquillian Tests](https://github.com/jboss-developer/jboss-developer-shared-resources/blob/master/guides/RUN_ARQUILLIAN_TESTS.md#run-the-arquillian-tests) for complete instructions and additional options._

1. Make sure you have started the JBoss EAP server as described above.
2. Open a command prompt and navigate to the root directory of this quickstart.
3. Type the following command to run the test goal with the following profile activated:

        mvn clean test -Parq-jbossas-remote 


Run tests from JBDS
-----------------------

To be able to run the tests from JBDS, first set the active Maven profile in project properties to be either 'arq-jbossas-managed' for running on
managed server or 'arq-jbossas-remote' for running on remote server.

To run the tests, right click on the project or individual classes and select Run As --> JUnit Test in the context menu.


Investigate the Console Output
----------------------------

Maven prints summary of the 8 performed tests to the console. 

    -------------------------------------------------------
     T E S T S
    -------------------------------------------------------
    Running org.jboss.as.quickstarts.tasks.UserDaoTest
    Tests run: 3, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 1.084 sec
    Running org.jboss.as.quickstarts.tasks.TaskDaoTest
    Tests run: 5, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 2.31 sec

    Results :

    Tests run: 8, Failures: 0, Errors: 0, Skipped: 0

### Server log

SQL statements generated by Hibernate are written into the server log.

#### Expected warnings and errors

_Note:_ You will see the following warnings and errors in the server log. Hibernate attempts to drop the table and constraints before they are created because the `hibernate.hbm2ddl.auto` value is set to `create-drop`. You can ignore these warnings and errors.

        HHH000431: Unable to determine H2 database version, certain features may not work

        HHH000389: Unsuccessful: alter table Task drop constraint FK_kxfu633bvt1sptgbnkxkrr3qf
        Table "TASK" not found; SQL statement: alter table Task drop constraint FK_kxfu633bvt1sptgbnkxkrr3qf [42102-168]

#### Example Output from the tests

Creating the database schema:

    10:16:58,770 INFO  [stdout] (MSC service thread 1-2) Hibernate: create table Tasks_task (id bigint not null, title varchar(255), owner_id bigint, primary key (id))
    10:16:58,771 INFO  [stdout] (MSC service thread 1-2) Hibernate: create table Tasks_user (id bigint not null, username varchar(255), primary key (id))
    10:16:58,772 INFO  [stdout] (MSC service thread 1-2) Hibernate: alter table Tasks_task add constraint FKE61757B62CC79EF1 foreign key (owner_id) references Tasks_user

Generating ID for a new entity and inserting the entity into the database:

    10:16:58,956 INFO  [stdout] (http--127.0.0.1-8080-1) Hibernate: select tbl.next_val from hibernate_sequences tbl where tbl.sequence_name=? for update
    10:16:58,957 INFO  [stdout] (http--127.0.0.1-8080-1) Hibernate: insert into hibernate_sequences (sequence_name, next_val)  values (?,?)
    10:16:58,958 INFO  [stdout] (http--127.0.0.1-8080-1) Hibernate: update hibernate_sequences set next_val=?  where next_val=? and sequence_name=?
    10:16:58,960 INFO  [stdout] (http--127.0.0.1-8080-1) Hibernate: insert into Tasks_user (username, id) values (?, ?)


Run the Quickstart in JBoss Developer Studio or Eclipse
-------------------------------------
You can also start the server and deploy the quickstarts or run the Arquillian tests from Eclipse using JBoss tools. For more information, see [Use JBoss Developer Studio or Eclipse to Run the Quickstarts](https://github.com/jboss-developer/jboss-developer-shared-resources/blob/master/guides/USE_JDBS.md#use-jboss-developer-studio-or-eclipse-to-run-the-quickstarts) 


Debug the Application
------------------------------------

If you want to debug the source code or look at the Javadocs of any library in the project, run either of the following commands to pull them into your local repository. The IDE should then detect them.

        mvn dependency:sources
        mvn dependency:resolve -Dclassifier=javadoc