# Encrypt Home Folder

![elementary OS: 5.1 Hera](https://img.shields.io/badge/elementary%C2%A0OS-5.1%20Hera-007aff)
![Status: Verified](https://img.shields.io/badge/status-verified-58c633)

These steps document how to activate home folder encryption for an existing user account on elementary OS 5.1 Hera.

##  Install the required encryption packages

```
sudo add-apt-repository universe
sudo apt update
sudo apt install ecryptfs-utils cryptsetup
```

## Create a temporary user account which has admin rights

Go to **Applications > System Settings > User Accounts**, click **"+" in the lower left corner** and a new user with the following settings:

- Account Type: **Administrator**
- Full Name: **Temp User for Home Encryption**
- Username: **homecrypt**
- Passwort: **xxxxxx**

Click **Create User**.

## Logout completely and Login with the temporary user

Open a Terminal and execute the following commands:

```
sudo ecryptfs-migrate-home -u <username>
```

## Logout completely and Login with your normal user

Next up we'll login with our regular user. This finishes the home encryption.

## Cleanup

If everything went well, we remove the home directory backup:

```
sudo rm -rf /home/<username>.<hash>
```

We also delete the temporary user in the **System Settings** and make sure its home directory is removed:

```
sudo rm -rf /home/homecrypt
```