# Users and Groups

## Learning goals

- Understand why and how Linux users are used
- Understand how the root user differs from normal user, and what `sudo` does and how to use it
- Learn what daemon users are and why they exist
- Learn how groups work and why they are used

## Introduction

*Linux*, like most other consumer operating systems, has support for multiple users as well as groups. Each user can have their own set of files, settings, and permissions. In this chapter we'll take a look at how we can leverage multiple users and groups in our *Linux* installations.

## Root user

In *Linux*, there exists what is known as the **root** user, which is a special system account that has complete administrative privileges and can perform almost any operation on the system. The root user has the highest level of access and can modify any file/configuration, as well as install software and manage user accounts.

That being said, due to its all-encompassing nature, using the root user for everyday tasks can be dangerous. It's very easy to accidentally delete a file or modify important configuration files without realizing it, or execute untrustworthy software that could harm the entire installation. As a result, it's recommended to use a regular user account with limited permissions for normal use and only switch to the root user account when necessary.

To make it easier to perform administrative tasks as the root user (installing software, creating users, etc.) many *Linux* distributions ship with the `sudo` command.

The `sudo` command stands for `superuser do` and enables a non-root user to execute a command with administrative privileges by temporarily elevating their permissions to the root user level. Using `sudo` means you don't have to manually login as root, perform tasks, then log back in to your user account every time you want to make an administrative change.

Let's say you want to make a file in the `/etc` (generally used for configuration files) folder, which requires `root` permissions, and you're logged in as a non-root user:

```bash
$ sudo touch /etc/test_config.ini
```

If it's the first time you run `sudo` in that shell session, the terminal will then ask you for the root user password. Once you enter it, the command following `sudo` will be ran.

## Users

As we briefly mentioned, each **user** on a *Linux* installation has their own unique set of files, settings, and permissions. For example, let's say your organization has a shared *Linux* system that gets used by various administrators. For security and management reasons, each of these individuals should have their own user account, to log in and access the system with.

In order to create a user account, you can use the aptly named `adduser` command in the terminal as root:

```bash
$ sudo useradd john
```

After the user has been created, we can set a password for it, using the `passwd` command:

```bash
$ sudo passwd john
```

To modify a user account, you can use the `usermod` command. For example, we can change `john`'s home directory to be in `/home/dev/john` instead of `/home/john` via the following command:

```bash
$ sudo usermod -d /home/dev/john john
```

We can also delete the user using the `deluser` command:

```bash
$ sudo deluser john
```

## Daemon Users

In *Linux*, a **daemon** is a type of background process that runs continuously. These processes can include tasks like monitoring the system, providing services (e.g. hosting a database or web server), or simply managing hardware devices. 

A **daemon user** is a special user that is created to run a daemon on the system. For example, if you're hosting a database using a distributed service, you will frequently find that the database service will create its own user. Or if you're hosting a web server, the server hosting service will probably create its own user as well.

The reason daemons sometimes create their own user can vary, but you can generally summarize them as follows:

- Running with lower privileges than the root user prevents security vulnerabilities and potential system crashes
- Prevents users from accidentally tampering with the daemon's files/resources
- Provides clean separation between itself and other daemons

Worth noting is that these daemon users that get created automatically with software are not intended for human users and you typically won't be logging into them the same way you would a normal user.

That being said, sometimes we create daemon users manually to isolate a specific process from the rest of the system. These kinds of users we can optionally log into.

## Groups

Just like users, **groups** exist to manage permissions and access to system resources. A group is simply a collection of users that share the same permissions and access levels.

As with users, you can create a group using the `groupadd` command:

```bash
$ sudo groupadd students
```

Once your group is created, you can add users to a group using the `usermod` command:

```bash
$ sudo usermod -a -G students john
```

And remove users from a group using the `gpasswd` commant:

```bash
$ sudo gpasswd -d john students
```

You can also use the `deluser` command passing in the groupname:

```bash
$ sudo deluser john students
```

Be careful with this one, however, as accidentally pressing `enter`/`return` before finishing the prompt could delete a user altogether instead of remove a user from the intended group!

## Conclusion

Users are a core component of managing and using *Linux* systems. Even if only one person ends up actually using the computer, there can still be a multitude of users created with specific tasks or services dedicated to them.

In the next chapter, we'll be going over *groups*, which users belong to, and see how they are used and administrated.