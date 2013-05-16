rundrush
========

a bash script allow running drush command against multiple drupal instances on multisite install

Originally developed by: Ed Tremel for Brown WebServices, May 2011

run-drush is a bash script that automates the execution of drush commands against
multiple sites in a multi-site installation.  It can accept a list of sites to
operate on from the location parameter (-l).  The parameter value can be the path
to the given core's sites folder or a text file with a linefeed delimited list of
sites folders.  The first line of the text file must read:
--Drupal Sites List--

A testfile is provided in the directory showing the required format.

A test option (-t) is also available which will allow the operator to run the
command using the auth core.  By default, drush will be run against the production
Drupal core (/www/data/httpd/htdocs/cms/drupal-cms).

Ignore values are hard-coded into the script.  As of now (20120605) site folders will
be ignored by the script if they match the following (* = wildcard):
all*
default*
old*
*backup
*-old
*dev
*sandbox

N.B. currently (20120605) not certain whether ignore values working when supplying path
to sites folder.  Best results using hand edited sites list.


Examples
=========
Executing a SQL command (removing a specific user from a role) across multiple sites:
./run-drush -l chg5.txt sql-connect < changebmcrole.sql --verbose

chg5.txt is the file specifying the list of sites/folders against which to execute command in the format:
brown.edu.sitepath.sitename

sql-connect is the drush command used to create a database connection with the designated site

changebmcrole.sql is the file containing the SQL command to be executed within the sql-connect database connection, in this case:
DELETE FROM {users_roles} WHERE uid=157 AND rid=4 LIMIT 1

--verbose is a drush flag used to provide additional details as each command is executed (--debug would provide even more information)

Alternative SQL Example
-----------------------
./run-drush -l testsites sqlq --file=[path-to-file]/test.sql --verbose

where testsites is another file listing sites/folders against which to execute the sqlq command

sqlq is the short form of the sql-query drush command

--file=[path-to-file]/test.sql specifies location of sql script to run against target sites

test.sql contents:
UPDATE `contact` SET `reply` = 'Thank you for your service inquiry.' WHERE `contact`.`category` LIKE 'Service Inquiries';
