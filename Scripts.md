# check_connection_qty

## Purpose

This script checks the quantity of established connections on the database. The quantity can be filtered by a given criterion, and inverted if wanted.

The connection state is based on the `list application` output. However, it is unknown how all states are expressed. For this reason, unknown states are sent to a file in /tmp directory in order to catch the text. Please send us the content of that file in order to improve this script. Currently, those unknown states are listed as 'other'.

## Requirements

The user that executes this script should have an authority at instance level (http://www-01.ibm.com/support/knowledgecenter/SSEPGG_10.5.0/com.ibm.db2.luw.admin.cmd.doc/doc/r0001958.html?lang=en):

 * SYSADM
 * SYSCTRL
 * SYSMAINT
 * SYSMON

## Usage

There are two ways to call this script:

-> Group by status

    ./check_connection_qty -i /home/db2inst1/ -d sample -s

-> Group by user

    ./check_connection_qty -i /home/db2inst1/ -d sample

## Output

-> Group by status

    OK. Normal quantity of connections (6).|'Connections'=6;10;15
    |'Connect_Completed'=6
    'UOW_Executing'=0
    'UOW_Waiting'=0
    'Lock_Wait'=0
    'Commit_Active'=0
    'Rollback'=0
    'Rollback_to_savepoint'=0
    'Compiling'=0
    'Disconnecting'=0
    'Backup'=0
    'Restore'=0
    'Recompiling'=0
    'Other'=0

![Check_connections_qty per status](https://angoca.github.io/monitor-db2-with-nagios/check_connection_qty_status.png)

-> Group by user

    OK. Normal quantity of connections (6).|'Connections'=6;10;15
    DB2INST1 has 6,|'User_DB2INST1'=6

![Check_connections_qty per user](https://angoca.github.io/monitor-db2-with-nagios/check_connection_qty_users.png)

## Extra

 * This script is ready to use with Check_MK. Option `-K`. The output is different.
 * If the script does not work, or the output is not correct, try to run it with the `-vvv` option, and then check the messages.
 * This script does not take into account subagents, just quantity of connections.


# check_database_connection

## Purpose

This script checks the connectivity to a database.

## Requirements

It requires the connection authority to the database.

## Usage

This is the way to call the script:

    ./check_database_connection -i /home/db2inst1/ -d sample 

## Output

The output shows the status of the connection:

    OK Connection to database sample. The database is active. |'Connectable_Database'=0.9;0.6;0.3
The database is active. |'Database_Active'=0.8;0.5 

The values 0.9 and 0.8 are indicative, there are just to draw a line in the graph.

![check_database_connection](https://angoca.github.io/monitor-db2-with-nagios/check_database_connection.png)

The graph show some perturbations in the blue line. It means that there were unavailability during that time.


# check_database_size

## Purpose

This script checks the database size. Retrieves a critical alarm when the size is bigger than the given size in critical, or retrieves a warning alarm when the size percentage is bigger than the provided, comparing with the allocated size.

Be careful when using with a standby database in an HADR environment. The values should be updated in the primary database. The standby should only retrieve these values. If the primary database does not update the values, the standby will generate errors.

## Requirements

The user that executes this script needs some special permission in the tables associated with storage management.

For example, if the user that executes the script is nagios, you should execute this.

    db2 grant execute on procedure sysproc.GET_DBSIZE_INFO to user nagios
    db2 grant execute on package NULLID.SYSSH200 to user nagios
    db2 grant select,update on table SYSTOOLS.STMG_DBSIZE_INFO to user nagios

You can also be executing the commands from `root`, in the case you are using Check_MK.

The user should be in the group of `SYSMON` authority.

It could also possible to require a rebind in some packages. For that, you can do this:

    db2rbind sample -l /tmp/log.bnd

## Usage

This is the way to call the script:

    ./check_database_size -i /home/db2inst1/ -d sample 

## Output

The output shows the status of the database:

    OK. Size is 17630625792B allocated in 38769844224B|'Database_size'=17630625792B
|'Database_allocation'=38769844224B 

![Check_database_size](https://angoca.github.io/monitor-db2-with-nagios/check_database_size.png)

## Extra

 * This script never generates an alert. It is just for informative purposes, and have an evolution graph of the database size in Nagios. If something like Cacty is used, this kind of graph could be not used.


# check_db2diag

## Purpose

Check the quantity of messages written in the db2diag.log file. It allows to detect an abnormal behaviour in the instance, because most of the time, when an recurrent error happens in the database/instance, it is described in the diagnostic file.

## Requirements

Access to the db2diag.log file.

## Usage

This is the way to call the script:

    ./check_db2diag -i /home/db2inst1/ 

## Output

The output shows the quantity of messages in the db2diag.log file:

    OK, quantity is normal (0) since 2014-09-21-09.38.08.|'logs'=0;20;40
    Messages by levels. Severe: 0, Critical: 0, Error: 0, Warning: 0, Info: 0, Event: 0|'Severe'=0
    'Critical'=0
    'Error'=0
    'Warning'=0
    'Info'=0
    'Event'=0

![Check_db2diag](https://angoca.github.io/monitor-db2-with-nagios/check_db2diag.png)

## Extra

 * The quantity of messages (blank lines) and lines can be retrieved. This could give you an idea of how big is the file.
 * If you get a message like this `File permissions are wrong '/tmp/last_date_check_db2diag__home_db2inst1_'` in the output, then you should go to the /tmp directory and delete the file, or change its permissions (write to all).


# check_hadr_status

## Purpose

Checks the HADR status by looking if both databases are connected. This script shows the difference in replication between the two machines.

## Requirements

At least SYSMON authority.

## Usage

This is the way to call the script:

    ./check_hadr_status -i /home/db2inst1/ -d sample

## Output

The output shows the status of the HADR and its synchronization:

    TODO output

If HADR is not configured:

    Database is not in HADR|
    |

TODO image


# check_instance_memory

## Purpose

Checks the memory usage by an instance. It does not take into account the bufferpools, or other memory dumps, it just takes the result of the query. The query is executed against the first connectable database of an instance.

## Requirements

The user used to execute the script should have a special privileges in at least one of the database of the instance analyzed; the privilege are:

    db2 grant execute on package NULLID.SQLC2H21 to user nagios
    db2 grant execute on specific function SYSPROC.ADMIN_GET_DBP_MEM_USAGE to user nagios
    db2 grant execute on specific function SYSPROC.ADMIN_GET_DBP_MEM_USAGE_AP to user nagios
    db2 db2 grant usage on workload SYSDEFAULTUSERWORKLOAD to user nagios

## Usage

This is the way to call the script:

    ./check_instance_memory -i /home/db2inst1/

## Output

The output shows the used memory:

    OK, memory usage is automatically.| 'Current_memory_at_node_0'=999MB
    At node 0 the max usage memory is 744MB and the current usage memory 999MB.| 'Max_memory_at_node_0'=744MB

![Check_instance_memory](https://angoca.github.io/monitor-db2-with-nagios/check_instance_memory.png)


# check_instance_up

## Purpose

This script queries the status of the instance by retrieving the port number or service name from the configuration, and then querying netstat to check if there is an associated service with that port.

## Requirements

It is necessary to execute this script with a user with at least SYSMON authority, in order to retrieve all the necessary information. If not, the script will produce a critical alert indicating that the PID of the instance cannot be retrieved.

## Usage

This is the way to call the script:

    ./check_instance_up -i /home/db2inst1/

## Output

The output shows the quantity of messages in the db2diag.log file:

    Instance at /home/db2inst1/ is up (PID 21560).|'Started_Instance'=2;;0.4
    There are 1 active databases|'Active_Databases'=1

![Check_instance_up](https://angoca.github.io/monitor-db2-with-nagios/check_instance_up.png)

As we can see in the graph, there are some periods where the instance was down.