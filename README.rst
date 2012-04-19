nxpgmon
==============


Description
------------

  This a simple script to monitor system and PostgreSQL activity. 

  By default it will also extract some information using the Nuxeo
  database schema, but this script can be used without Nuxeo.


Requirement
------------

  This scripts requires the following packages:

  - atop

  - sysstat

  - logtail

  - postgresql-contrib (for pg_stat_statements and pg_buffers)
  
  Also aspersa summary can be installed::

    wget http://aspersa.googlecode.com/svn/trunk/summary


Configuration
-------------

  1. Edit the nxpgmon and set the default:

     - ``PG_LOG``: the PostgreSQL log file path

     - ``DB_NAME``: the database name

     - ``SUMMARY``: the aspersa summary script

  2. Modify the postgresql.conf file to add logs, theses option can
     stay in production ::

       # pg_stat_statement
       shared_preload_libraries = 'pg_stat_statements'
       custom_variable_classes = 'pg_stat_statements'
       pg_stat_statements.max = 10000
       pg_stat_statements.track = top
       # pgfouine compatible log
       log_min_duration_statement = 200
       log_line_prefix = '%t [%p]: [%l-1] user=%u,db=%d '
       # important logs
       log_checkpoints=on
       log_lock_waits=on
       log_temp_files=0
       log_autovacuum_min_duration=0

  3. Optional configuration, NOT FOR PRODUCTION ::

       # auto explain
       shared_preload_libraries = 'pg_stat_statements, auto_explain'
       custom_variable_classes = 'pg_stat_statements, auto_explain'
       auto_explain.log_min_duration = '1s'
       auto_explain.log_analyze = 'true'


  4. Restart the database

  5. Enable extension in your database as ``postgres`` user::

       psql $DB_NAME -c "CREATE EXTENSION pg_stat_statements; CREATE EXTENSION pg_buffercache;"


Usage
------

  1. Before the action to monitor as ``postgres`` user ::

      nxpgmon start

  2. After the action, gets the logs ::

      nxpgmon stop


  For more information : ``nxpgmon -h``


Todo
-----

- prefix archive file with host name and tag: nxpgmon-TAG-HOST-DATE.tgz
