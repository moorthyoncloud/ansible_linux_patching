# Patch and Reboot Servers Playbook

This Ansible playbook is designed to update and reboot servers based on their OS family (RedHat or Debian). It automates package updates and checks if a reboot is required, then reboots the server if necessary. The playbook processes servers in batches, ensuring a controlled and rolling update process.

## Features
- **OS-Specific Updates**: Uses `yum` for RedHat-based servers and `apt` for Debian-based servers.
- **Conditional Reboot**: Reboots servers only when required based on OS-specific criteria.
- **Batch Processing**: Updates and reboots servers in batches, with pauses between batches to ensure minimal service interruption.

## Prerequisites
- Ansible installed on the control machine.
- Access to managed servers with appropriate sudo privileges.
- Servers categorized in Ansible inventory with `ansible_os_family` fact set to either `RedHat` or `Debian`. Works even when `ansible_os_family` fact is not set if the fact gathering is not turned Off.

## Usage

1. **Clone the repository** and navigate to the playbook directory.

    ```bash
    git clone https://github.com/moorthyoncloud/ansible_linux_patching.git
    cd ansible_linux_patching
    ```

2. **Run the playbook** to apply updates and perform reboots as needed. The playbook will process 3 servers at a time.

    ```bash
    ansible-playbook -i <inventory-file> patch.yml
    ```

3. **Inventory File**: Ensure your Ansible inventory file includes the servers you wish to update and specifies their `ansible_os_family`.

## Playbook Breakdown

- **Update RPM-based servers**: Runs `yum -y update` on RedHat-based servers.
- **Update APT-based servers**: Updates package cache and performs a distribution upgrade on Debian-based servers.
- **Check for Reboot Requirement**:
  - On Debian: Checks for the presence of `/var/run/reboot-required`.
  - On RedHat: Uses `needs-restarting -r` command to determine if a reboot is necessary.
- **Conditional Reboot**:
  - Debian: Reboots if `/var/run/reboot-required` exists.
  - RedHat: Reboots if `needs-restarting -r` indicates a restart is needed.
- **Batch Processing**: Processes servers in batches of five, with a 1-minute pause between batches.

## Notes

- **Serial Execution**: Servers are updated and rebooted in groups of 3 to avoid simultaneous reboots across all servers.
- **Pause**: The playbook includes a pause of 1 minute between each batch to provide a buffer between server updates and reboots.

## License
This project is licensed under the MIT License.

## Author
- [Shenbagamoorthy](https://github.com/moorthyoncloud)
