---
title: "User Account Management Using shell Scripting"
datePublished: Wed May 14 2025 15:03:03 GMT+0000 (Coordinated Universal Time)
cuid: cmao2lhko000809k19ei93jqt
slug: user-account-management-using-shell-scripting
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1747234881186/b60c2642-cda2-4546-99a9-6706f52ce0b5.png
tags: development, devops, devops-journey, devopscommunity

---

**Task 1: Account Creation**

#### **Steps:**

1. **Check if the username already exists** before creating the account.
    
2. **Prompt for username and password** securely.
    
3. **Create the new account** and set the password.
    
4. **Confirm successful account creation.**
    

#### **Execution:**

```bash
#!/bin/bash

# Function to create a user account
create_user() {
    read -p "Enter new username: " username
    if id "$username" &>/dev/null; then
        echo "Error: User '$username' already exists."
        exit 1
    fi
    read -s -p "Enter password for $username: " password
    echo
    sudo useradd "$username"
    echo "$username:$password" | sudo chpasswd
    echo "User '$username' created successfully."
}

create_user
```

**Explanation:**

* `id "$username"` checks if the user already exists.
    
* `read -s -p` ensures password entry is hidden.
    
* `useradd` creates the account.
    
* `chpasswd` sets the password.
    
* Prints success confirmation.
    

---

### **Task 2: Account Deletion**

#### **Steps:**

1. **Check if the username exists** before deletion.
    
2. **Prompt for username** to be deleted.
    
3. **Delete the account and confirm.**
    

#### **Execution:**

```bash
delete_user() {
    read -p "Enter username to delete: " username
    if ! id "$username" &>/dev/null; then
        echo "Error: User '$username' does not exist."
        exit 1
    fi
    sudo userdel -r "$username"
    echo "User '$username' deleted successfully."
}

delete_user
```

**Explanation:**

* `id "$username"` ensures the user exists before deletion.
    
* `userdel -r` removes the user along with their home directory.
    
* Prints a success message.
    

---

### **Task 3: Password Reset**

#### **Steps:**

1. **Check if the username exists** before resetting password.
    
2. **Prompt for username and new password.**
    
3. **Reset the password securely.**
    
4. **Confirm password change.**
    

#### **Execution:**

```bash
reset_password() {
    read -p "Enter username to reset password: " username
    if ! id "$username" &>/dev/null; then
        echo "Error: User '$username' does not exist."
        exit 1
    fi
    read -s -p "Enter new password: " password
    echo
    echo "$username:$password" | sudo chpasswd
    echo "Password for '$username' reset successfully."
}

reset_password
```

**Explanation:**

* Ensures the account exists before modifying credentials.
    
* Uses `chpasswd` to securely reset passwords.
    
* Prints confirmation.
    

---

### **Task 4: List User Accounts**

#### **Steps:**

1. **Fetch all system users** and their corresponding UIDs.
    
2. **Format the output for readability.**
    

#### **Execution:**

```bash
list_users() {
    echo "User Accounts on the System:"
    cut -d: -f1,3 /etc/passwd | column -t -s:
}

list_users
```

**Explanation:**

* Extracts usernames and their UIDs from `/etc/passwd`.
    
* Formats output neatly using `column -t -s:`.
    

---

### **Task 5: Help and Usage**

#### **Steps:**

1. **Display available command options** and their usage.
    

#### **Execution:**

```bash
usage() {
    echo "Usage: user_management.sh [OPTION]"
    echo "Options:"
    echo "  -c, --create    Create a new user account"
    echo "  -d, --delete    Delete an existing user account"
    echo "  -r, --reset     Reset password for an existing user account"
    echo "  -l, --list      List all user accounts"
    echo "  -h, --help      Display usage information"
}

usage
```

**Explanation:**

* Prints all available command-line options.
    
* Ensures users understand script functionality.
    

---

### **Final Implementation: Running the Full Script**

Now, let's package everything into one script (`user_`[`management.sh`](http://management.sh)) with proper argument handling:

```bash
#!/bin/bash

# Function to display usage
usage() {
    echo "Usage: $0 [-c|--create] [-d|--delete] [-r|--reset] [-l|--list] [-h|--help]"
}

create_user() {
    read -p "Enter new username: " username
    if id "$username" &>/dev/null; then
        echo "Error: User '$username' already exists."
        exit 1
    fi
    read -s -p "Enter password for $username: " password
    echo
    sudo useradd "$username"
    echo "$username:$password" | sudo chpasswd
    echo "User '$username' created successfully."
}

delete_user() {
    read -p "Enter username to delete: " username
    if ! id "$username" &>/dev/null; then
        echo "Error: User '$username' does not exist."
        exit 1
    fi
    sudo userdel -r "$username"
    echo "User '$username' deleted successfully."
}

reset_password() {
    read -p "Enter username to reset password: " username
    if ! id "$username" &>/dev/null; then
        echo "Error: User '$username' does not exist."
        exit 1
    fi
    read -s -p "Enter new password: " password
    echo
    echo "$username:$password" | sudo chpasswd
    echo "Password for '$username' reset successfully."
}

list_users() {
    echo "User Accounts on the System:"
    cut -d: -f1,3 /etc/passwd | column -t -s:
}

case "$1" in
    -c|--create) create_user ;;
    -d|--delete) delete_user ;;
    -r|--reset) reset_password ;;
    -l|--list) list_users ;;
    -h|--help) usage ;;
    *) echo "Invalid option! Use -h or --help for usage."; exit 1 ;;
esac
```

### **How to Use the Script**

1. **Create a User** → `./user_`[`management.sh`](http://management.sh) `-c`
    
2. **Delete a User** → `./user_`[`management.sh`](http://management.sh) `-d`
    
3. **Reset a Password** → `./user_`[`management.sh`](http://management.sh) `-r`
    
4. **List Users** → `./user_`[`management.sh`](http://management.sh) `-l`
    
5. **Show Help** → `./user_`[`management.sh`](http://management.sh) `-h`
    

This script follows **DevOps best practices**, ensuring security, error handling, and user-friendly execution.