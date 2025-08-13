Hardening Role
=========

Set of hardening tasks designed to enforce security on Linux systems.

## ⚠️ **Warning**:
Only use this role if you are familiar with its configurations and understand the implications of the changes it will make. Improper use may result in system misconfigurations or reduced accessibility.

**Special Attention Required**:  
Do not enable `HARDENING_SSH_CONFIG_ENABLED` unless there is a privileged user in the system to which the user has access. Enabling this variable without proper user access can lead to loss of control over the system and potentially lock you out.

Take extra care when configuring the firewall settings, particularly with the `HARDENING_FIREWALL_INTERFACE_LIST` and the SSH port (`SSH_PORT`). Incorrect settings in this area could prevent access to the system entirely.

Role Variables
--------------

```yml
#####################
###  Common vars  ###
#####################
# Common variables shared across all roles/playbooks.
USERNAME: user  # Default system user.
SSH_PORT: 22  # SSH port used for access and hardening rules.

##################################
###  Misc hardening role vars  ###
##################################
HARDENING_MISC_CONFIG_ENABLED: false  # Enable or disable miscellaneous hardening tasks.
HARDENING_MISC_YESCRYPT_HOST_FACTOR: 8  # Yescrypt factor for password hashing difficulty.
HARDENING_MISC_NODE_TYPE: desktop  # Specify the type of node (server or desktop).

##################################
###  Auth hardening role vars  ###
##################################
HARDENING_AUTH_FAILLOCK_CONFIG_ENABLED: false  # Enable or disable faillock.
HARDENING_AUTH_FAILLOCK_MAX_ATTEMPTS: 3  # Maximum number of failed login attempts.
HARDENING_AUTH_FAILLOCK_UNLOCK_TIME: 600  # Time (in seconds) to unlock after exceeding attempts.

###########################
###  Network role vars  ###
###########################
HARDENING_NETWORK_CONFIG_ENABLED: false  # Enable or disable network configuration.
HARDENING_NETWORK_DISABLE_IPV6: false  # Enable or disable IPv6 stack disabling.

################################
###  Debian-based role vars  ###
################################
HARDENING_DEBIAN_HARDENING_ENABLED: false  # Enable or disable Debian-based configuration.

#################################
###  RedHat family role vars  ###
#################################
HARDENING_REDHAT_HARDENING_ENABLED: false  # Enable or disable RedHat-based configuration.

#######################
###  SSH role vars  ###
#######################
HARDENING_SSH_CONFIG_ENABLED: false  # Enable or disable SSH configuration.
HARDENING_SSH_REMOVE_ROOT_PASSWORD_ENABLED: false  # Enable or disable root password removal.
HARDENING_SSH_PERMIT_ROOT_LOGIN: true  # Allow or disallow root login via SSH.

############################
###  Fail2Ban role vars  ###
############################
HARDENING_FAIL2BAN_ENABLED: false  # Enable or disable Fail2Ban installation.
HARDENING_FAIL2BAN_BANTIME: 5m  # Duration for which IPs are banned.
HARDENING_FAIL2BAN_FINDTIME: 3m  # Time window to consider failed login attempts.
HARDENING_FAIL2BAN_MAXRETRY: 5  # Number of allowed failed login attempts before banning IP.

##########################
###  Chrony role vars  ###
##########################
HARDENING_CHRONY_ENABLED: false  # Enable or disable Chrony installation.

############################
###  Firewall role vars  ###
############################
HARDENING_FIREWALL_ENABLED: false  # Enable or disable firewall hardening.
# Specify the firewall backend (options: ufw, firewalld, iptables).
# Leave empty to autodetect. Fails if no supported firewall is found.
HARDENING_FIREWALL_BACKEND: ''
HARDENING_FIREWALL_RESET: false  # Enable or disable resetting the firewall rules.
HARDENING_FIREWALL_OPEN_SSH_PORT: true  # Enable or disable opening the SSH port in the firewall.

HARDENING_FIREWALL_INTERFACE_LIST:  # List of network interfaces to apply firewall rules to.
  - eth0
  - wlp2s0

# NOTE: Applies ONLY to iptables.
# Changing this parameter will ignore some of the firewall settings above
# and apply the specified template as-is. Use at your own risk.
# Path to the iptables configuration template file.
# Defaults to the role's built-in template.
HARDENING_FIREWALL_IPTABLES_CONFIG_SRC: firewall_iptables.rules.j2
```

Installing Role
---------------
```bash
ansible-galaxy install git+https://github.com/ggragham/hardening_ansible_role.git
```

Example Playbook
----------------

```yml
  - name: Server configuration
    hosts: servers
    roles:
       - role: hardening_ansible_role
```

License
-------

GPL

Author Information
------------------

[Grell Gragham](https://github.com/ggragham)
