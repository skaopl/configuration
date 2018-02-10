# Configure SSL connection to GitHub

This manual works for Linux operating systems.

## Steps

### Create SSL key

#### Check if key already exists
Check the home directory listing to see if you already have a public SSH key:

    ls -al ~/.ssh

If following error is displayed:
```
ls: nie ma dostępu do '/home/username/.ssh': Nie ma takiego pliku ani katalogu
```  
there are no ssh keys and you need to generete new one.


#### Generate ssh key

GitHub email: @gmail.com

    ssh-keygen -t rsa -b 4096 -C "your_email@example.com"

```
Generating public/private rsa key pair.
Enter file in which to save the key (/home/username/.ssh/id_rsa): 
Created directory '/home/username/.ssh'.
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/username/.ssh/id_rsa.
Your public key has been saved in /home/username/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:xxxxxxxxxxxxxxx your_email@example.com
The key's randomart image is:
+---[RSA 4096]----+
|                 |
|                 |
|                 |
|                 |
|                 |
|                 |
|                 |
|                 |
+----[SHA256]-----+
```


#### SSH Agent 
Start the ssh-agent in the background

    eval $(ssh-agent -s)
Result:

    Agent pid 2682

Add your SSH private key to the ssh-agent

    ssh-add ~/.ssh/id_rsa
    Enter passphrase for /home/username/.ssh/id_rsa: 
    Identity added: /home/username/.ssh/id_rsa (/home/username/.ssh/id_rsa)


Copy the SSH to your clipboard

    cat ~/.ssh/id_rsa.pub
and paste the whole content of the file as new SSH key in GitHub profile.

### Testing SSH connection
Connect to GitHub via SSH

    ssh -T git@github.com
Result:
```
The authenticity of host 'github.com (xxx.xxx.xxx.xxx)' can't be established.
RSA key fingerprint is SHA256:xxxxxxxxxxxxxxx.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'github.com,xxx.xxx.xxx.xxx' (RSA) to the list of known hosts.
Connection to github.com closed by remote host.
```

### Configure github
#### Set your username
It's does not have to be the same as GitHub username.
It will be visible as the author of the commit.
```
git config --global user.name "<username>"
```

#### Set your commit email address in Git
You can use your real email address or use the no reply version provided by GitHub.
For the real address use the one provided while creating SSH keys.
No reply address can be obtained from the profile page on GitHub website.
```
git config --global user.email "email@example.com"
```

#### Set push method
There are two push methods:
* simple - more strict, push to branch which you pull from;
* match - push if branch has the same name

```
git config --global push.default simple
```

### Work with GitHub

#### Clone repository
```
git clone git@github.com:<username>/<repo>.git
```
Example for this repository:
```
git clone git@github.com:skaopl/configuration.git
```

#### Push to the repository
```
git add -A
git commit -am "What was done"
```
Result:
```
[master (root-commit) 22aee3d] GitHub SSH configuration
 1 file changed, 116 insertions(+)
 create mode 100644 GitHub_SSL_Config.md
```
```
git push
```

If following error occurs:
fatal: remote error: 
```
You can't push to git://github.com/<username>/<repo>.git
Use https://github.com/<username>/<repo>.git
```
You need to convert the repository to use a different url for connetion:
```
git remote set-url origin git@github.com:<username>/<repo>.git
```