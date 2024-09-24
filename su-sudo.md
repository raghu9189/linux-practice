In Ubuntu Linux (and most other Linux distributions), both `su` and `sudo` are commands used to gain elevated privileges or switch users, particularly the root user. However, they work in different ways and have distinct use cases. Let’s break down each:

### 1. **`su` (Switch User)**

The `su` command allows you to switch users, and by default, it switches to the root user. When you run `su`, it starts a new shell as the specified user (or root if no user is provided). It requires the password of the user you want to switch to.

- **Syntax**: 
  ```bash
  su [username]
  ```

- **How it works**:
  - **Without a username**: `su` will prompt for the root user's password and, upon successful authentication, will log you into the root shell.
  - **With a username**: `su username` will prompt for the password of the specified user and switch to that user's environment.

- **Example**:
  ```bash
  su root
  ```
  After entering the root password, you'll be in a root shell (indicated by the `#` prompt).
  
  You can also switch to a different user:
  ```bash
  su alice
  ```

- **Exit**:
  To return to your original user, type:
  ```bash
  exit
  ```

**Downside**: Using `su` to switch to root requires knowing the root password, which can be risky as it grants full access to the system.

### 2. **`sudo` (Superuser Do)**

`sudo` is a safer and more flexible alternative to `su`. It allows a permitted user to run a command as the superuser (root) or another user, based on the configuration in the `/etc/sudoers` file. With `sudo`, you don’t need to know the root password; instead, you authenticate with your own password.

- **Syntax**:
  ```bash
  sudo [command]
  ```

- **How it works**:
  - `sudo` runs a single command with elevated privileges. It does not start a new shell like `su` but grants superuser privileges to that command.
  - When you run `sudo`, it asks for **your password** (not root’s password). After successful authentication, the command is executed as root.
  - `sudo` keeps a timestamp of your authentication, so if you run another `sudo` command within a short period, it won’t ask for the password again.

- **Example**:
  ```bash
  sudo apt update
  ```
  This updates the system package lists using root privileges, but it requires **your** password, not the root password.

- **Running commands as another user**: You can also use `sudo` to run commands as a specific user by using the `-u` option.
  ```bash
  sudo -u alice ls /home/alice
  ```

- **Configuration**:
  - The `/etc/sudoers` file controls who can use `sudo` and which commands they can run.
  - To edit the `sudoers` file safely, use:
    ```bash
    sudo visudo
    ```

- **Elevate to root shell**:
  If you want to use `sudo` to get a root shell (similar to `su`), you can run:
  ```bash
  sudo -i
  ```
  This logs you into the root environment using your own password.

### **Key Differences between `su` and `sudo`:**

| Feature               | `su`                                    | `sudo`                                   |
|-----------------------|------------------------------------------|------------------------------------------|
| **Default behavior**   | Switches to root or another user         | Runs a command with root privileges      |
| **Password used**      | The password of the user you're switching to (usually root) | Your own password                        |
| **Scope**              | Starts a new shell as root/user          | Executes a single command as root/user   |
| **Security**           | Requires knowing the root password       | Safer, no need to share the root password|
| **Timestamp**          | N/A                                      | Keeps a temporary timestamp after authentication, so multiple commands can be run without entering the password again |
| **Usage**              | For switching to a different user environment (permanent switch until `exit`) | For temporarily executing commands with elevated privileges |

### **When to Use `su` vs. `sudo`:**
- **`su`**: Use when you need to completely switch to another user, especially if you're going to perform multiple tasks as that user (e.g., root). However, this is less common in Ubuntu, where `sudo` is preferred for admin tasks.
  
- **`sudo`**: Use when you need to run individual commands with superuser (root) privileges or as another user. In Ubuntu, `sudo` is the default way to manage system administration tasks.

### **Why Ubuntu Uses `sudo` by Default**
Ubuntu disables the root account by default for security reasons. Instead, it configures users to use `sudo` to execute administrative tasks. This way:
1. There's no need to directly use the root account.
2. It limits root-level access, making accidental system-wide changes less likely.
3. Logs of all `sudo` commands can be maintained for auditing.

### Example Scenarios:
- **Install a package**:
  ```bash
  sudo apt install nginx
  ```
  You’re running the package installation as root but only using your own password for authentication.

- **Edit a system configuration file**:
  ```bash
  sudo nano /etc/hostname
  ```

- **Switch to another user** (e.g., to root):
  ```bash
  su root
  ```

Or use:
  ```bash
  sudo -i
  ```

In summary, `sudo` is more commonly used in Ubuntu due to its security advantages, while `su` can still be used to switch users or get a root shell in environments where direct root access is necessary.