cometbackup
===========

	cometbackup  version 0.6
	<github.com/boukeversteegh/cometbackup>

	cometbackup is a tool for making (local) differential backups.
	cometcleanup intelligently removes old backups according to a cleanup-schedule.

	Usage: cometbackup SOURCE TARGET [OPTION]...
	  or   cometbackup --help

	Options
	  --log                Write log-messages to TARGET/comet.log
	  --test               For debugging. Don't perform backups, shows settings.
	  --format=FORMAT      Override FORMAT for archive copies of backups.
	  --cleanup            Run cometcleanup after backing up.
	  --schedule=SCHEDULE  Set SCHEDULE for cometcleanup. Def: 'schedules/default'.
	  --help, -h           Show this help.

	  SOURCE       Directory to be backed up.  E.g: '/home/admin'.  Don't add a '/'.
	  TARGET       Directory where all backups will be placed. E.g: '/backup/admin'.
	  FORMAT       Format for naming the backup archive copies. Use parameters that
	               are accepted by 'date +FORMAT' and readable by 'date -d FORMAT'.
	               Default is '%Y-%m-%d %H:%M:%S'.
	  SCHEDULE     Path to schedule file. Default: 'schedules/default'.
	               Refer to 'cometcleanup --help' for more details.

	Details
	  SOURCE directory is synced to TARGET/current. A differential copy is made to
	  TARGET/{TIMESTAMP}. You can safely delete any backup - including 
	  TARGET/current - without affecting any of the copies.

	Examples
	  cometbackup  /home  /backups/home --log
	  Run twice with time interval. Contents of /backups/home:
	    2012-10-23 12:03:10/        # first backup
	    2012-10-23 12:15:43/        # second backup
	    current/                    # identical to the latest backup
	    comet.log

	  cometbackup /home /backups/home --format="%Y/%m/%d %H:00" --cleanup \
	    --schedule=~/home.schedule --log
	
cometcleanup
============
	Tool to cleanup old backups according to cleanup SCHEDULE.

	usage:	cometcleanup TARGET SCHEDULE
		cometcleanup --help

	  TARGET	Directory where backups are placed.
	  SCHEDULE	Path to schedule file. Default: ./schedules/default

			Schedule file format:
			EXPIRYPERIOD UNIT FILTER

			Where:
			  EXPIRYPERIOD	An integer above 1 plus a unit. After this period, only backups matching the filter will be kept.
					  10 seconds
					  1 day
					  3 weeks
			  UNIT		A time unit that will be matched by the filter.
					  %S		Second        00-59
			  		  %M		Minute        00-59
			  		  %H		Hour          00-23
			  		  %w		Day of week   0-6
			  		  %d		Day of month  01-31
			  		  %m		Month         01-12
			  		  %Y		Year          1971-2012
			  FILTER	An expression that matches the unit. Can be one of the following
					VALUE		The unit should be exactly VALUE
					/INTEGER	The filter will match every INTEGER UNIT.

					  %S 00		Matches whenever seconds is 00
					  %M /10	Matches every 10 minutes (00, 10, 20 etc)
					  %m /2		Matches every 2 months (1, 3, 5)
			Example:
			1 week     %w 0	# Suppose you make daily backups. Of backups older than 1 week, only sundays will be kept.
			3 hours    %H /3	# Suppose you make hourly backups. After 3 hours, only one backup in every 3 hours will be kept.