On a Linux machine, e.g.
```
docker run --rm -it -v /tmp/archie:/outer-tmp archlinux bash -c 'pacman -Sy --noconfirm git watchexec tmux && tmux'
```
run (each in a new shell):

```
cd /tmp && git clone https://github.com/thomasetter/watchexec-parent-ignore-fail.git && cd watchexec-parent-ignore-fail
```

Run a watcher in the main directory:
```
cd /tmp/watchexec-parent-ignore-fail/ && watchexec -vvv --no-global-ignore -w . 'echo changed' | tee >(sed $'s/\033[[][^A-Za-z]*m//g' > /tmp/watchexec-parent-ignore-fail/self-watching-out)
```

Run a watcher for the main directory in the subdirectory:
```
cd /tmp/watchexec-parent-ignore-fail/subdir0/ && watchexec -vvv --no-global-ignore -w .. 'echo changed' | tee >(sed $'s/\033[[][^A-Za-z]*m//g' > /tmp/watchexec-parent-ignore-fail/parent-watching-out)
```

Both watchers output `changed` once.

Creating a directory `ignored-from-child-gitignore` in `subdir0` should do nothing, since it has been ignored in `watchexec-parent-ignore-fail/subdir0/.gitignore`:
```
mkdir /tmp/watchexec-parent-ignore-fail/subdir0/ignored-from-child-gitignore
```

However, the version watching for changes in the parent directory from `subdir0` has two `changed` outputs while the version watching the parent directly only has one (I believe the correct output).

Creating a subdirectory ignored from the parent `subdir0/ignored-from-parent-gitignore` works and does not trigger an extra change:

```
mkdir /tmp/watchexec-parent-ignore-fail/subdir0/ignored-from-parent-gitignore
```
