cometbackup
===========

Tool for making differential backups.

usage:  	cometbackup SOURCE TARGET [--cleanup[=SCHEDULE]]
        	cometbackup --help

options:
  --cleanup	Remove old backups according to cleanup SCHEDULE after backing up.
  --help, -h	Show this help.

  SOURCE	Directory to be backed up.
  TARGET	Directory where all backups will be placed.
  SCHEDULE	Path to schedule file. Default: schedules/default

details:
  Backups are copied to TARGET/current. A differential copy is made to TARGET/{TIMESTAMP}. You can safely delete any backup - including TARGET/current - without affecting any of the copies.