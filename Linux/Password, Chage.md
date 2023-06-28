## Find user account aging details in Linux

To know the account aging information, you will have to append the `-l` flag and username with the chage command:

```
chage -l username
```

And you will get similar output if you are using your system as admin or if it is your personal Linux machine.

Here,

- `Last password change` indicates the date when the password was changed most recently.
- `Password expires` indicates the date when the password will expire.
- `Password inactive` will show how many days the account will remain inactive after the password is expired.
- `Minimum number of days between password change` indicates the minimum day break required between two password changes.
- `Maximum number of days between password change` will show how many days you are left to change your current password.

Once you know the use of all the fields, you can use the following command to use the **chage command in interactive mode** and it will ask you each field one by one:

```
sudo chage username
```

But what if you want to change them individually? Well, here's how you do it.

You require superuser privileges to access all other options besides the -l option, which is needed to find account aging information.

## Lock user account in Linux

The most common use of the chage command is to lock the users.

To lock a specific user, all you need to do is execute a command in the given command syntax:

```
sudo -E 0 username
```

For reference, here, I locked a user account of `sagar`:

```
sudo -E 0 sagar
```

As you can see, once I executed the command to lock the account, it won't allow the user to log in.

And in case you are curious about the use of `-E` and `0` flag, here you have it:

- The `-E` flag is used to set an expiry date of an account
- Whereas `0` is the date when the account should expire. So once you execute the command, the account will be locked immediately.

## Force user to change password at next login

Want to force a specific user to change his password?

It is quite simple and can be achieved by the following command:

```
sudo chage --lastday 0 username
```


And as you can see, when I tried to log in as a `sagar`, it forced me to change my password.

## Set an account expiry date

If you want to set the date of the account expiry, you will have to use a `-E` flag followed by date and username.

For example, here, I have set the account expiry date to 11th Jan 2023:

```
sudo chage -E 2023-01-11 sagar
```


## Remove the account expiry date as a sysadmin

When your account is expired, the system will throw the error saying **"account validation failure, is your account locked?":**


So the first step is to gain root access using the su command:

```
su
```

Now, you will have to use the `--expiredate` flowing by `-1` and username:

```
chage --expiredate -1 username
```

And now you can check the account aging details and it removes the account expiry date:


## Specify the maximum number of days between the password change

To configure the maximum number of days between the password change, you will have to append the number of days to the `-M` flag.

```
sudo chage -M No_of_days username
```

For example, here, I went with 5 days of gap between the password change:

```
sudo chage -M 5 sagar
```


## Set account inactivity time after password expiry

The account inactivity field indicates how many days the account will remain inactive after the password is expired.

To set the account inactivity time, you will have to append the time (in days) and username to the `-I` flag:

```
sudo chage -I No_of_days username
```

For example, here, I have configured my account inactivity period to 12 days:

```
sudo chage -I 12 sagar
```


Here, my password is expiring on 15th Jan 2023, so when I added 12 days of inactivity, it reflected 27th Jan 2023 in the password inactive field.

## Notify the user before changing a password is necessary

You can set the number of days of warnings before the password is expired using the `-W` flag:

```
sudo chage -W No_of_days username
```

For example, if I want to give warnings before 2 days of password expiry, I will be using the following command:

```
sudo chage -W 2 sagar
```


## In the end...

Did you mess up while configuring password aging with the chage command?

Execute the chage command with username only and it gets you in the interactive mode of the chage command enabling you to change every detail:

```
sudo chage username
```