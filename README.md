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