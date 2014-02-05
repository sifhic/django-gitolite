# Django Gitolite App

Provides a basic front-end for Gitolite.

## Configuration

By default the Gitolite rc file is `~/.gitolite.rc`. Follow these steps:

1. Add `LOCAL_CODE => "$ENV{HOME}/local",` to the rc file.
2. Create `~/local/triggers/post-compile/django`, it should be executable and
   call the `gitolitetrigger` management command.
3. Create `~/local/hooks/common/post_receive`, it should be executable and call
   the `gitolitehook` management command.
4. Add `POST_COMPILE => ['post-compile/django'],` to the rc file.
5. Add `POST_CREATE => ['post-compile/django'],` to the rc file.
6. Add `SSH_AUTHKEYS => ['post-compile/ssh-authkeys'],` to the rc file.

### Running as another user

This configuration requires `sudo` and the current user to be in the same group
as the gitolite user's default group.

As the gitolite user, open `~/.gitolite.rc` and add `UMASK =>  0027,`. This is
required to be able to read `gl-creator` and `gl-forked-from` files in the
repositories. Next, ensure that the key directory, `~/.gitolite/keydir` exists
and is writable by the current user. Files in this directory need to be
readable by the gitolite user, to ensure that files are created belong to the
gitolite user's default group use `chmod g+rwxs ~/.gitolite/keydir`.

Next, you need to setup `sudo` so the gitolite user can use it to run Gitolite
triggers. Insert the following line into `/etc/sudoers`:

    %git ALL=(git)NOPASSWD:/usr/bin/gitolite trigger SSH_AUTHKEYS

## License

All code is licensed under GPL v3.
