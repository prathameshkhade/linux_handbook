# **Chapter 6: User and Group Management**  

## 6.1 **Understanding Users and Groups**  
### 6.1.1 **What are Users and Groups in Linux?**  
  - Explanation of users and why they are necessary for multi-user environments.  
  - Role of groups in managing permissions and access control.  
### 6.1.2 **User Types and Configuration in Both Ubuntu and Red Hat**  
  - System users vs. regular users: Differences and purposes.  
  - Default users in Linux (root, daemon users, service accounts).  
  - Understanding the structure of `/etc/passwd` and `/etc/group` files:  
    - Format and fields of `/etc/passwd` (username, UID, GID, home directory, shell).  
    - Role of `/etc/group` in defining group memberships.  
  - Checking user information using `id`, `whoami`, and `groups` commands.

## 6.2 **Managing User Accounts**  
### 6.2.1 **Creating Users**  
  - Using `useradd` command:  
    - Basic syntax and options (`-m`, `-s`, `-u`, `-d`, etc.).  
    - Assigning home directories and shells during user creation.  
    - Setting default configurations using `/etc/skel`.  
  - Setting passwords for new users (`passwd` command).  
### 6.2.2 Modifying Users**  
  - Changing user details with `usermod`:  
    - Updating home directory, shell, UID, and primary group.  
    - Locking and unlocking user accounts.  
### 6.2.3 Deleting Users**  
  - Using `userdel` safely:  
    - Removing user accounts while preserving or deleting home directories.  
    - Considerations before deleting a user (ownership of files, active processes).  

## 6.3 **Group Management**  
### 6.3.1 Understanding Groups and Their Types**  
  - Primary vs. secondary groups.  
  - System groups and their purposes.  
### 6.3.2 Creating and Managing Groups**  
  - Using `groupadd` to create new groups.  
  - Modifying groups with `groupmod`.  
  - Deleting groups with `groupdel`.  
### 6.3.3 Assigning Users to Groups**  
  - Adding a user to a group (`usermod -aG`).  
  - Viewing group memberships (`groups`, `id`).  
### 6.3.4 Managing Permissions with Groups**  
  - Understanding file and directory permissions (`ls -l`).  
  - Assigning ownership with `chown`.  
  - Changing group ownership using `chgrp`.  
  - Setting default group permissions with `umask`.  

---

## 6.4 **Password Policies and Security**  
### 6.4.1 Password Policies and Management**  
  - Setting and enforcing strong passwords.  
  - Understanding `/etc/shadow` and password hashing.  
  - Changing passwords with `passwd`.  
### 6.4.2 Password Aging and Expiry**  
  - Configuring password aging policies using `chage`:  
    - Setting password expiry, warning periods, and inactivity locks.  
    - Viewing user password aging details (`chage -l`).  
### 6.4.3 Enhancing Security**  
  - Locking and unlocking user accounts (`passwd -l` and `passwd -u`).  
  - Disabling logins for a user.  
  - Using `faillog` to track failed login attempts.  
  - Best practices for securing user accounts (limiting root access, sudo usage). 