# Progress -- Automatic the uploading of binary files using git-annex

11 Jun 2016

This is the second post in the series of post pertaining to the project “Automatic the uploading of binary files using git-annex”. This post aims to show the progress made until the mid term evaluation.

We planned to write abstraction over git-annex to add the files to remote storage server. Git-annex provides support for external special remote, it follows a messaging based service as prescribed here [External Special Remote Protocol](https://git-annex.branchable.com/design/external_special_remote_protocol/) needs to be followed.

But we found some tools [git-annex-remote-rclone](https://github.com/DanielDent/git-annex-remote-rclone), [git-annex-remote-hubic](https://github.com/Schnouki/git-annex-remote-hubic), [dropboxannex](https://github.com/TobiasTheViking/dropboxannex), which were already present , so we decided to test them, debug them and change them according to our requirement. we tried to install and test all of them, and all of them were stuck with some or the other kind of error.

In the case of droppboxannex, git init was taking a lot of time, opened issue. Similarly, for the other two as well ther some errors. We decided to focus our energy on one of them and found that git-annex-remote-rclone was the best out of all of them, as it can support all the remote storage spaces which are supported by rclone, which includes a lot of them from dropbox, hubic to Yandex Disk. So we decided to debug it.

It was showing an error `"Remote origin not usable by git-annex; setting annex-ignore"`. We tried to pinpoint the error, but it looked like the error was occuring out of git-annex, somehow git-anex was not able to use remote rclone, after a lot of struggling, it came to our notice that the message `DIRHASH-LOWER Key` was `First supported by git-annex version 6.20160511` and since we were using the official version supported by ubunutu version. So after cloning the git-annex repo and builiding and installing from the source we were able to run the git-annex-remote-rclone.

Now the tool works and all, but as already present in the problem statment, we need to build a tool which can further ease the process, so if we can create a little bit of more abstraction it will work best. So we decided to built a little bit of more abstraction according to following steps:



```
Installing rlcone [Once only]
1\. Install rclone into your $PATH, e.g. /usr/local/bin
2\. Copy git-annex-remote-rclone into your $PATH
3\. Configure an rclone remote: rclone config

Using git annex remote
1\. git annex init
2\. git annex initremote <remote_name> type=external externaltype=rclone target=<rclone_target_name> prefix=git-annex chunk=50MiB encryption=shared mac=HMACSHA512 rclone_layout=lower
3\. git annex add <binary_files>
4\. git commit -am <message> <binary_files>
5\. git push -u origin master
6\. git annex sync --content
7\. git annex copy <binary_files> --to <remote_name>

Cloning repos
1\. git annex clone <repo address>
2\. cd <cloned_repo_folder_name>
3\. git annex sync
4\. git annex enableremote <remote_name>
5\. git annex get <binary_files> --from <remote_name>

```


The abstraction over the above steps will be as follows:


```
Install Robocomp

Installing rclone [once]
1\. rclone config

Using abstract tool
1\. git_tool initremote <remote_name> type=external externaltype=rclone target=<rclone_target_name> prefix=git-annex chunk=50MiB encryption=shared mac=HMACSHA512 rclone_layout=lower
2\. git_tool add <binary_files> <remote_name>

Cloning repos
1\. git annex clone <repo address>
2\. cd <cloned_repo_folder_name>
3\. git_tool get <binary_files> <remote_name>

```


Swapnil Sharma