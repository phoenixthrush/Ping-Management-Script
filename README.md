# Ping Management Script

This script allows you to manage a list of hosts by adding, removing, displaying and pinging them. It also provides the ability to install and uninstall a cron job to automatically ping at a specified interval.

# Installation

## Method 1: Via GitHub Repository

1. Clone the repository:
    ```bash
    git clone https://github.com/phoenixthrush/Ping-Management-Script.git monitor
    ```

2. Copy the script to `/usr/bin/monitor`:
    ```bash
    sudo cp monitor/monitor/usr/bin/monitor /usr/bin/monitor
    ```

3. Make the script executable:
    ```bash
    sudo chmod +x /usr/bin/monitor
    ```

4. Run the script:
    ```bash
    monitor
    ```

## Method 2: Using my apt Repository

1. Install via the apt repository:
    ```bash
    curl -sSL https://phoenixthrush.com/repo/install.sh | bash
    ```

2. Install the monitor package:
    ```bash
    sudo apt install monitor
    ```

## Script Features

-   `addhost`: Add a new host to the list of hosts.
-   `delhost`: Remove an existing host from the list.
-   `showhosts`: Display all the hosts in the list.
-   `pinghosts`: Ping all hosts in the list and log results.
-   `install`: Install a cron job to periodically ping hosts.
-   `uninstall`: Remove the cron job.
-   `help`: Show help section.

## Configuration File

The script uses a configuration file (config.cfg) at ~/.monitor/config.cfg to define the number of ping packets, timeout duration, and interval between pings.

### Default Configuration:

If the configuration file doesn't exist, the script will create one with the following default values:

```
PING_PACKETS=3
PING_TIMEOUT=5
PING_INTERVAL=1
```

Feel free to modify the configuration file (config.cfg) to match your needs.

## Available Arguments (Examples)

You can run the script with the following commands:

Add a host to the list.

```shell
./monitor.sh addhost 192.168.1.1
```

Remove a host from the list.

```shell
./monitor.sh delhost 192.168.1.1
```

Display all the hosts in the list.

```shell
./monitor.sh showhosts
```

Ping all the hosts in the list and log the results.

```shell
./monitor.sh pinghosts
```

Install a cronjob to run the script at a specified interval (in minutes).

```shell
./monitor.sh install 5
```

Remove the installed cronjob.

```shell
./monitor.sh uninstall
```

To display the help section, run the following command:

```shell
./monitor.sh help
```

If installed via package, you can also view the help documentation using:

```shell
man monitor
```

## Script Overview

The script works as follows:

1. `addhost`:

    - Checks if a value (host) is provided.
    - If the host is not already in the `hosts.txt` file, it adds the host.
    - If the host already exists, it returns with exit code `3`.

2. `delhost`:

    - Checks if a value (host) is provided.
    - If the host exists in the `hosts.txt` file, it removes the host.

3. `showhosts`:

    - Displays all the hosts in the `hosts.txt` file.
    - If the file is empty, it notifies the user and returns with exit code `2`.

4. `pinghosts`:

    - Pings all the hosts listed in `hosts.txt` using the parameters defined in the `config.cfg` file (ping packets, timeout, interval).
    - It displays the status of each host (either up or down).
    - If the hosts file is empty, it notifies the user and returns with exit code `2`.
    - Logs failed ping attempts in `error.log`.

5. `install`:

    - Checks if a value (interval in minutes) is provided.
    - If the script is not executable, it makes it executable.
    - Backs up the current cron jobs and installs a new cron job to run the script at the specified interval.

6. `uninstall`:

    - Backs up the current cron jobs and removes the installed cron job for this script.
    - If the configuration is invalid, it notifies the user and returns with exit code `4`.

7. `help`:

    - Shows help section.

## Error Codes

The script uses the following error codes:

-   `0`: Success  
    This code is returned when a command executes successfully without errors.

-   `1`: Missing value or invalid argument  
    This code is returned when an argument is missing or incorrect, such as when a required host value is not provided or an invalid argument is used.

-   `2`: File is empty  
    This code is returned when the `hosts.txt` file is empty, meaning there are no hosts listed to work with.

-   `3`: Host already exists  
    This code is returned when attempting to add a host that is already present in the `hosts.txt` file.

-   `4`: Configuration file not found or invalid  
    This code is returned when the configuration file (`config.cfg`) is missing or contains invalid values (e.g., missing or incorrect configuration variables like `PING_PACKETS`, `PING_TIMEOUT`, `PING_INTERVAL`).
