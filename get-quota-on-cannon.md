# Disk quotas

**There is not enough disk space for you to store every output file from
all of your GEOS-Chem runs indefinitely\! You must analyze model output
and remove it at the same rate you generate it over a reasonable
timescale.**

## Disk usage command

While logged into Cannon or AS, type:

``` 
   cd ~
   du -h -s -c *
```

This will list your disk usage in reasonable units (K, M, and G) by
directory and file to help you identify where your usage is greatest.
The `-c` option displays the total usage at the end of the list. The
`du` command can then be issued in subdirectories until you identify
files to remove. Type `man du` for more information about the `du`
command.

## Disk space on Odyssey

| Name           | Path                                       | Disk space            | Backed up?           |
| -------------- | ------------------------------------------ | --------------------- | -------------------- |
| Home directory | `/n/homeXX/USERNAME` or simply `~USERNAME` | 100 GB                | Yes                  |
| Scratch        | `/n/regal/jacob_lab/USERNAME`              | 50 TB limit for group | No, 90-day retention |
| Storage        | `/n/seasasfs02/USERNAME`                   | 500 GB                | Yes                  |

If your folder doesn't exist in `/n/regal/jacob_lab` or `/n/seasasfs02`
you can create one using `mkdir USERNAME`. If you don't have access to
those directories, email [Judit Flo
Gaya](/wiki/geos-chem/personnel#sysadmin) and she can change your
permissions.

For information about disk space on Odyssey see
<https://www.rc.fas.harvard.edu/resources/odyssey-storage/>.

## Return to Menu

  - [Go to Harvard Atmospheric Sciences Wiki Home](/start)
  - [Go to "The Basics" Menu](/wiki/basics)
  - [Go to Network Resources Menu](/wiki/computer_and_network_info)
