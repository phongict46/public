Refer: [link](https://stackoverflow.com/questions/4254389/git-corrupt-loose-object)

![[Git Object corrupt image 20241104124617.png|300]]
The fix
Execute these commands from the parent directory above your repo (replace 'foo' with the
name of your project folder):
1. Create a backup of the corrupt directory:
```bash
cp -R foo foo-backup
```
2. Make a new clone of the remote repository to a new directory:
```bash
git clone git@www.mydomain.de:foo foo-newclone
```
3. Delete the corrupt git subdirectory:
```bash
rm -rf foo/-git
```
4. Move the newly cloned git subdirectory into foo
```bash
mv foo-newclone/.git foo
```
5. Delete the rest of the temporary new clone:
```bash
rm -rf foo-newclone
```
On Windows you will need to use:
• copy instead of cp -R
• rmdir /S instead of rm -rf
• move instead of mv
Now foo has its original git subdirectory back, but all the local changes are still there. git
status, commit, pull, push, etc. work again as they should.
