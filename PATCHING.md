# Package Management and Patching

* What is package management?
* Package management tools
    * yum
    * dnf
    * rpm
    * apt
* Difference between `Upgrade` and `Update` ?
* Rollback the patches and update


## Package management

It's all about packages
* Installing
* Upgrading 
* Deleting 
* View Package Info
* Package config

Package managers:

* Manage software using `YUM/DNF` and `RPM` for your Red Hat-based linux systems like CentOS.
* `APT` for Debian based, Ubuntu, Kali Linux etc.

### 1. YUM (Yellow-Dog Updater Modified)
- YUM is the primary package management tool for redhat.
- YUM performs dependency resolution when installing, updating, and removing software packages.
- YUM can manage packages from installed repositories in the system or from .rpm packages.

#### YUM Commands:

\* To run these command as normal user sudo access is required. For root user, there is no restriction. 

* How to install and remove a package 

`# yum install nginx -y`

`# yum remove nginx`

* How to upgrade or update a package

`# yum upgrade package`

`# yum update package`

* To list down available updates

`# yum list updates`

* Update all packages 

`# yum update -y`



<br>

What is the difference between `yum Update & Upgrade`?

Upgrade -> Will delete the old packages

Update -> Keep the old packages, we can rollback


<br>

* Check all the option

`# yum -options`

* Check the available updates for packages

`# yum check-update`

**Package Rollback**

* To see the past work done related to packages, which will show you the activity with date and time 

`# yum history`

* We can simply undo or redo any action using

`# yum history undo/redo <id>`


### 2. RPM (Redhat package manager)

* Using `RPM`, you can install, uninstall, and query individual software packages.

*Issue: It cannot manage dependency resolution like `YUM`*

* `RPM` maintains a database of installed packages, which enables powerful and fast queries.

* To install, upgrade or delete an .rpm package using `RPM`,

`# rpm -i package-file`

`# rpm -U package-file`

`# rpm -ivh package-file`

`# rpm -ievh package-file`

i - install,
U - upgrade,
v - verbose,
h - hash to show progress


* To query all the installed packages

`# rpm -qa`

* More info about the package

`# rpm -qi <package_name>`

* Info about config files for a package

`# rpm -qc <package_name>`


### 3. DNF (Dandified YUM)

*Updated version of `yum`*

For Redhat/CentOS 8

* `dnf list available`

* `dnf list installed`

* `dnf update/upgrade`

* `dnf install package.name`

* `dnf remove package.name`

* `dnf info package.name`

* `dnf search package`


### 4. APT (Advanced Packaging Tool)

For Debian based system like ubuntu, kali linux etc.

* `apt install package_name`

* `apt remove package_name`

* `apt autoremove` (to remove the dependencies)

*In apt we don't have yum or dnf like functionality where dependencies will be removed automatically once package is removed. Here, were need to run above command to remove dependencies.*

* `apt update` (to update the repo)

* `apt-cache search package_name`



---

### Patching

**What are patches?**

Patches are new or updated lines of code that determine how an operating system, platform, or application behaves. Patches are usually released as-needed to fix mistakes in code, improve the performance of existing features, or add new features to software. Patches are not newly compiled OSs, platforms, or applications—patches are always released as updates to existing software.

Because modifications like these are usually quicker to distribute than minor or major software releases, patches are regularly used as network security tools against cyber attacks, security breaches, and malware—vulnerabilities that are caused by emerging threats, outdated or missing patches, and system misconfigurations.

#### Patch management best practices

* Identify systems that are noncompliant, vulnerable, or unpatched. Scan systems daily/weekly/monthly.

* Prioritize patches based on the potential impact. Calculate risk, performance, and time considerations.

* Patch often. Patches are usually shipped once a month or sooner.

* Test patches before placing them into production.

### OS Patching

Performing RHEL OS patching is a critical task that requires careful planning and execution. Here's a comprehensive guide on prerequisites, different methods, pre-checks, post-checks, and relevant files involved in RHEL OS patching:

**Prerequisites:**

1. Subscription: Ensure your RHEL system is properly subscribed to Red Hat.
2. Backup: Back up critical data and configurations before applying updates.
3. Access: Have SSH access to the systems you want to patch.
4. Ansible Setup: If using Ansible, ensure it's installed and configured with the necessary credentials.

**Methods for Patching:**

There are several methods for patching RHEL, including:

1. **Using Yum**:
   - This is the most common method.
   - Uses the `yum` or `dnf` package manager to install updates.
   - See the previous responses for detailed steps when using Yum.

2. **Using Ansible**:
   - Ansible can be used to automate patching across multiple RHEL systems.
   - Ansible playbooks can be customized to your environment.

### 1. Manual Patching with Yum/DNF:

**Pre-Checks:**

1. **Current System Status**:
   - Verify that the system is running smoothly before patching.
   - Check disk space, active services, and system health.

2. **Inventory**:
   - Maintain an inventory of all systems you plan to patch.

3. **System Backup**:
   - Take full system backups or snapshots before applying updates.

**Patching with Yum (Detailed Steps):**

4. **Clear Yum Cache**:
   ```
   sudo yum clean all
   ```

5. **Check for Available Updates**:
   ```
   sudo yum check-update
   ```
    * Review and note the packages that will be updated.
    * Ensure there's enough disk space.
    * Check system services for any critical processes.

6. **Install Updates**:
   ```
   sudo yum update
   ```

7. **Reboot Decision**:
   - Check if a reboot is required using:
     ```
     cat /var/run/reboot-required
     ```
    - Ensure all services are running correctly.
    - Review system logs for any errors.

**Post-Checks:**

8. **Verify Updates**:
    - Check that package versions have been updated:
      ```
      rpm -qa | grep <package-name>
      ```

9. **Check System Services**:
    - Ensure all critical services are running as expected.

**Restart (if needed):**

10. **If a Reboot is Required**: Schedule a system reboot if indicated by the presence of `/var/run/reboot-required`.

11. **Post-Reboot Verification**: After the reboot, perform additional checks to ensure the system is functioning correctly.

**Files and Configuration Files:**

During the patching process, several important files and directories are involved:

- **/etc/yum.conf**: Configuration file for Yum package manager.
- **/etc/yum.repos.d/**: Directory containing Yum repository configuration files.
- **/etc/redhat-release**: File that shows the RHEL version.
- **/var/log/yum.log**: Log file for Yum operations.
- **/var/run/reboot-required**: Indicator file for a required system reboot.
- **/etc/systemd/system**: Location of systemd unit files for system services.


### 2. Automation with Ansible:

**Step 1: Create an Ansible Playbook**

Create an Ansible playbook (e.g., patch_rhel.yml) to automate the patching process. Include the following tasks:

```yaml
---
- name: Update RHEL systems
  hosts: your_target_group
  tasks:
    - name: Check for available updates
      yum:
        list: updates
      register: update_list

    - name: Update packages
      yum:
        name: "*"
        state: latest
      when: update_list.results|length > 0

    - name: Reboot if required
      command: shutdown -r now
      async: 0
      poll: 0
      when: "'Reboot required' in hostvars[inventory_hostname]['ansible_kernel']['stdout_lines']"

    - name: Wait for SSH to become available after reboot
      wait_for_connection:
        connect_timeout: 60
        sleep: 5
        delay: 5
        timeout: 300

```

**Step 2: Pre-Patching Checks**

- Ensure your Ansible inventory file (hosts.ini) includes the target hosts.
- Check the Ansible playbook syntax:

`ansible-playbook --syntax-check patch_rhel.yml`

**Step 3: Run the Ansible Playbook**

Execute the playbook to automate the patching process:

`ansible-playbook -i hosts.ini patch_rhel.yml`

Using Ansible for patch management allows you to automate and standardize the patching process across multiple systems efficiently.

Verify the patching process on each system.
Check system services and logs for any issues.
Confirm the systems have been successfully updated.
Using Ansible for patch management allows you to automate and standardize the patching process across multiple systems efficiently.


Make sure to document your patching process, including any custom configurations or tools used. Regularly review Red Hat's official documentation for the latest best practices and recommendations related to OS patching.


## * KPatch

**Kpatch** is different from traditional package-based OS patching methods, like using `yum` or `dnf`, in how it applies and manages kernel patches in a live Linux system. Kpatch is a technology that allows you to apply live kernel patches without the need to reboot the system. This is particularly useful in situations where system downtime must be minimized, making it ideal for critical, high-availability systems. 

Here are the key differences between Kpatch and traditional patching methods:

1. **Reboot Requirement**:
   - Traditional Patching: When you apply kernel updates using `yum` or `dnf`, a system reboot is usually required for the updates to take effect. During the reboot, the new kernel is loaded, and the system may experience downtime.
   - Kpatch: Kpatch allows you to apply kernel patches to a running system without rebooting. This can significantly reduce or eliminate downtime, making it a valuable tool for critical systems.

2. **Patching Mechanism**:
   - Traditional Patching: Traditional package-based patching updates the kernel by installing new packages. The updates take effect after a system reboot.
   - Kpatch: Kpatch applies patches at the kernel level, allowing for live patching without needing to replace the entire kernel. This is done using dynamic kernel modules.

To use Kpatch, follow these general steps:

1. **Install Kpatch**: You'll need to install Kpatch on your RHEL system. You can do this via `yum`:

   ```
   sudo yum install kpatch
   ```

2. **Load the Kpatch Module**: Load the Kpatch module into the kernel:

   ```
   sudo modprobe kpatch
   ```

3. **Create or Obtain Patches**: Obtain or create the patches you want to apply to the kernel. Kpatch uses patches in a specific format that allows for live patching.

4. **Apply Patches**: Use the `kpatch` command to apply the patches:

   ```
   sudo kpatch create <patch-name>
   ```

   Replace `<patch-name>` with the name of your patch.

5. **Load Patches**: Load the patches into the running kernel:

   ```
   sudo kpatch load <patch-name>
   ```

6. **Verify Patches**: Confirm that the patches have been successfully applied:

   ```
   sudo kpatch list
   ```

7. **Rollback Patches**: If needed, you can roll back patches using the `kpatch unload` command.

Kpatch is a powerful tool for minimizing downtime during kernel updates, but it requires careful management and testing. Be aware that not all kernel updates or security patches may be compatible with Kpatch. It's essential to thoroughly test and validate the patches before applying them to a production system.

Resources:
- https://www.redhat.com/sysadmin/kernel-live-patching-linux
- https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/managing_monitoring_and_updating_the_kernel/applying-patches-with-kernel-live-patching_managing-monitoring-and-updating-the-kernel#doc-wrapper
