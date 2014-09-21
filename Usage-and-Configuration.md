Most of the scripts need the instance directory where they are going to be executed.

Let's suppose we are going to use `check_instance_up`. You will need to pass the directory instance with the parameter `-i`. The directory instance is the home directory of the user that executes the instance.

You can get the instances of a server by running `db2greg -dump`

    /opt/ibm/db2/V9.7/bin/db2greg -dump

The output is similar to

    S,DB2,9.7.0.7,/opt/ibm/db2/V9.7,,,7,0,,1361790210,0
    V,DB2GPRF,DB2SYSTEM,DB2META-10,/opt/ibm/db2/V9.7,
    V,DB2GPRF,DB2ADMINSERVER,dasusr1,/opt/ibm/db2/V9.7,
    I,DB2,9.7.0.7,db2inst1,/home/db2inst1,,1,0,/opt/ibm/db2/V9.7,,

The lines starting by `I` indicate an instance in the server.
You can get the home directory of a given user by

    echo ~db2inst1

One you have the home directory

    ./check_instance_up -i /home/db2inst1

If something is wrong, you will get the error message

    ./check_instance_up
    Usage: check_instance_up { -i instanceHomeDirectory # -h # -V } [ -v ]
    Note: The test was not executed.|
    |
