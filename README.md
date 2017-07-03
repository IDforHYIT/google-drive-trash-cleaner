# Google Drive Trash Cleaner
Permanently delete files in Google Drive after X days in trash/bin.

Unlike many other cloud storage services, Google Drive doesn't auto delete files in trash/bin even after they've been there for a long time.
There isn't even a way to check when a file was trashed.
This Python script helps you clean your Google Drive's trash.

## Dependencies
To use the Python script directly
* Python 3 (tested with Python 3.5+)
* package *google-api-python-client*  
run `pip install --upgrade google-api-python-client` to install

To use the Windows binary (download on the [releases](https://github.com/cfbao/google-drive-trash-cleaner/releases) page)
* Windows update [KB2999226](https://support.microsoft.com/en-gb/help/2999226/update-for-universal-c-runtime-in-windows "Update for Universal C Runtime in Windows")

## How-to
Download `cleaner`([.py](./cleaner.py) or [.exe](https://github.com/cfbao/google-drive-trash-cleaner/releases)), place it in an empty local folder, and run it from command line.

By default, `cleaner` retrieves a list of all files trashed more than 30 days ago, and prints their info on screen.
You're asked whether you want to delete them.
If confirmed, these files are permanently deleted from Google Drive.

### Google authorization
The first time you run `cleaner`, you will be prompted with a Google authorization page asking you for permission to view and manage your Google Drive files.
Once authorized, a credential file will be saved in `.credentials\google-drive-trash-cleaner.json` under your home directory (`%UserProfile%` on Windows).
You don't need to manually authorize `cleaner` again until you delete this credential file or revoke permission on your Google [account](https://myaccount.google.com/permissions "Apps connected to your account") page.  
You can specify a custom location for the credential file by using the command line option `--credfile`. This is helpful if you're using multiple Google accounts with `cleaner`.

### `page_token` file
`cleaner` finds out when your files were trashed by scanning through your Google Drive activity history.
On first run, it must start from the very beginning to ensure no files are missed, so it might take some time.
After first run, `cleaner` saves a file named `page_token` in its own parent folder.
This file contains a single number indicating an appropriate starting position in your Google Drive activity history for future scans,
so they can be much faster than the first one. Each run of `cleaner` updates `page_token` as appropriate.  
You can specify a custom location or name for the `page_token` file by using the command line option `--ptokenfile`.

### More options
More command line options are available. You can read about them by running `cleaner --help`.
```
usage: cleaner.py [-h] [-a] [-v] [-d #] [-t SECS] [--logfile PATH]
                  [--ptokenfile PATH] [--credfile PATH]

optional arguments:
  -h, --help            show this help message and exit
  -a, --auto            Automatically delete older trashed files in Google
                        Drive without prompting user for confirmation
  -v, --view            Only view which files are to be deleted without
                        deleting them
  -d #, --days #        Number of days files can remain in Google Drive trash
                        before being deleted. Default is 30
  -t SECS, --timeout SECS
                        Specify timeout period in seconds. Default is 300
  --logfile PATH        Path to log file. Default is no logs
```