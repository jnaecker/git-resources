# Notes on Git on the Server

So you want to use git while sshing into a server.  Supposing git is already installed (likely on any *nix server), you just need to clone in your repo.  This can be done with either SSH or HTTPS.

## Using SSH

Just run

```bash
git clone git://github.com/username/repo.git name
```

This will create a new folder with the given name, download the repo, and add the link as remote.  To pull down future changes, run

```bash
git pull origin master
```

You should be prompted for your Githup email and password.  To avoid having to enter these credentials each time, follow the directions for [adding an SSH key](https://help.github.com/articles/generating-ssh-keys).  If you have issues, see the [trouble-shooting guide](https://help.github.com/articles/error-permission-denied-publickey).  Note that the `ssh-agent` may not be running by default, in which case you have to enter the command

```bash
eval `ssh-agent -s`
```

before you add your ssh key.

## Using HTTPS

If the above does not work, try cloning your repo with HTTPS instead:

```bash
git clone https://github.com/user/repo.git name
```

After the cloning is done, pulls and pushes can be done in the same way.  One downside of using HTTPS is that you can't have permanent keys.  You can, however, cache your password for short times:

```bash
git config --global credential.helper 'cache --timeout=3600'
```

After running this you will have to enter your credentials only once more, after which they will be saved for the next hour.