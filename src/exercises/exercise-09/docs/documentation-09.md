# Enabling Index Based File Search

## Technical Background

Searching through the entire file system with tools like find can be slow because it scans directories in real-time. Index-based search tools like plocate use a pre-built database of file names that allows instant lookups. However, the index only reflects the file system state at the time it was last updated, meaning new files won’t appear and deleted files might still show up until the index is refreshed.

## Related Links

[How to use Nano](https://linuxize.com/post/how-to-use-nano-text-editor/)

[SSH Guide](https://docstore.mik.ua/orelly/networking_2ndEd/ssh/)

[Linux manual](https://man7.org/linux/man-pages/man1/rm.1.html)

## Solution

### Setup

1. Update your packages and install `plocate`:

```
apt update
apt install plocate
```

2. Build the initial file index:    

`updatedb`

!!! Info
    This creates the file index and builds a database at `/var/lib/plocate/plocate.db`

!!! Warning
    `locate` returns stale until this command is run

### Using File Index

1. Search for an existing files:

`locate aptitude`

!!! Note 
    You should see results from already indexed files.

2. Creating a new file:

`touch ~/mylocaltest.txt`

3. Search without updating the index:

`locate localtest`

!!! Note 
    You will see no result, because the index doesn’t know about this new file yet.

4. Update the index:

```
updatedb
locate localtest
```

!!! Note
    Now you should see: `/home/<username>/mylocaltest.txt`

!!! Warning
    Some files get excluded by the index.

5. Delete the file:

`rm ~/mylocaltest.txt`

6. Search without updating the index:

`locate localtest`

!!! Note 
    You will still see the file listed because the index still contains its old entry.

7. Rebuild the index and search again:

```
updatedb
locate localtest
```

!!! Note 
    This time, there is no output, as the deleted file has been removed from the index.

