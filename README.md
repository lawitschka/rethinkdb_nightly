# Node RethinkDB / S3 Backup

*Original work is from [Swift](https://github.com/theycallmeswift/node-mongodb-s3-backup). I just moved a few things around to accommodate RethinkDB*

This is a package that makes backing up your RethinkDB databases to S3 simple.
The binary file is a node cronjob that runs at midnight every day and backs up
the database specified in the config file.

## Installation

    npm install rethinkdb_nightly -g

## Configuration

To configure the backup, you need to pass the binary a JSON configuration file.
There is a sample configuration file supplied in the package (`config.sample.json`).
The file should have the following format:

    {
      "rethinkdb": {
        "host": "localhost",
        "port": 28015,
        "db": "database_to_backup"
      },
      "s3": {
        "key": "your_s3_key",
        "secret": "your_s3_secret",
        "bucket": "s3_bucket_to_upload_to",
        "destination": "/"
      },
      "cron": {
        "time": "11:59",
      }
    }

### Crontabs

You may optionally substitute the cron "time" field with an explicit "crontab"
of the standard format `0 0 * * *`.

      "cron": {
        "crontab": "0 0 * * *"
      }

*Note*: The version of cron that we run supports a sixth digit (which is in seconds) if
you need it.

### Timezones

The optional "timezone" allows you to specify timezone-relative time regardless
of local timezone on the host machine.

      "cron": {
        "time": "00:00",
        "timezone": "America/New_York"
      }

You must first `npm install time` to use "timezone" specification.

## Running

To start a long-running process with scheduled cron job:

  rethinkdb_nightly <path to config file>

To execute a backup immediately and exit:

  rethinkdb_nightly -n <path to config file>
