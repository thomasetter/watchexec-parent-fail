On a Linux machine, run (each in a new shell):


```
cd /tmp && git clone https://github.com/thomasetter/watchexec-parent-ignore-fail.git && cd watchexec-parent-ignore-fail
```

Run a watcher in the main directory:
```
cd /tmp/watchexec-parent-ignore-fail/ && watchexec -w . -vvv 'echo changed'
```

Run a watcher for the main directory in the subdirectory:
```
cd /tmp/watchexec-parent-ignore-fail/subdir0/ && watchexec -w .. -vvv 'echo changed'
```

Both watchers output `changed` once.

Creating the directory should do nothing, since it has been ignored in `watchexec-parent-ignore-fail/subdir0/.gitignore`:
```
mkdir /tmp/watchexec-parent-ignore-fail/subdir0/dir-should-be-ignored
```

However, the version watching for changes in the parent directory from `subdir0` has two `changed` outputs while the version watching the parent directly only has one (I believe the correct output).
