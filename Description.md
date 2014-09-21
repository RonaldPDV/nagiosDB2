# Open source

The are many tools or programs in the market that allow you to monitor DB2. There are a few that are really good, and you can understand what is happening inside DB2, and then you can take the best decisions.

However, these software is not free, and something the licenses are expensive. If you took the way to use DB2 Express-C, it is because your current plan is not to pay for the database software (probably you are testing a new feature, you have a small set of data, etc.), and in the same way you are not going to pay for the monitoring software for that database.

With this is mind, this is the reason this project is open source, to allow people to have a basic monitoring for their DB2 databases.

# Bash script

The decision to create the scripts in shell was to not depend in a compiler, such as Java or C, or interpreters like Perl or Python. Bash is independent, and ready to use if bash is available.

Why bash and not korn? well, it was because it is included in all Linux and it is becoming important. It is not probably the best, but many people know how to use it.

# Independent scripts (no architecture)

This set of script was born as an extension ad improve to a set of scripts hosted in dbatodba. It took the same basis, and after that, we see that each script could be modified independently. This provide a lot of flexibility, because a modification does not impact the other scripts.

The lack of architecture could be an issue. But the initial aim was to provide a simple set of scripts to monitor DB2.

The similarity of the scripts is guided by a template that already has the basic structure for all scripts. This template has a specific space for the specific part to be monitored, with many validations done before. You will see that all scripts are based on the template.

We are currently thinking to create a new project / branch to have an architecture. It will be very different from this project, because the scripts will share code, but it will reduce the impact of connection per script, with an asynchronous check. It could probably use jNRPE, and the aim is to keep an active connection to the database, in order to reduce the connection establishment for each check.

# Extensibility

You can modify any script to your own requirements, you just have to edit the file and that is all.

You can also create you own scripts, based on template, just by copying the template.sh file. The benefict of writing a new script based on template, is that your script will be Nagios complaint.

# Support

This is an open source project, and no support is provided. However, you can write an issue here, and we will try to help you.

Also, if you create your own set of plugins or you corrected the existing ones, share them with us. The community will see the benefict of this collaboration.