# Audit Logs

## Enable or disable audit logging for host and Kubernetes APIs

Audit logs are used to monitor the system and search for suspicious behavior.
Host and Kubernetes API audit logging can be enabled to produce extra audit logs for analysis.
Enabling audit logging is optional. If enabled it generates some load and data on the non-compute nodes \(NCNs\).
By default, host and Kubernetes API audit logging are not enabled.

## Procedure

1. Enable or disable host and Kubernetes API audit logging.

   The method for updating the audit log settings will vary depending on the state of the system.
   Select one of the following options based on the installation status of the system:

   * Install

     To update the audit log settings during the installation, use one of the following options:

     * **Option 1**

       During the installation, audit logging is enabled or disabled by modifying the CSI settings.
       To enable or disable audit logging, use the following flags with the `csi config init` command. For more information on using flags, see `csi config init -h`:
       Refer to `csi config init -h` for more information on using flags.

       `ncn-mgmt-node-auditing-enabled`: Set to `true` to enable host logging or to `false` to disable host logging.

       ```console
       ncn# csi config init --ncn-mgmt-node-auditing-enabled=true [other config init options]
       ```

       `k8s-api-auditing-enabled`: Set to `true` to enable Kubernetes API logging or to `false` to disable Kubernetes API logging.

       ```console
       ncn# csi config init --k8s-api-auditing-enabled=true [other config init options]
       ```

     * **Option 2**

       Adjust the audit log settings by editing the `system_config.yaml` file.

       View the current settings with the following command:

       ```bash
       ncn# cd /var/www/ephemeral/prep
       ncn# grep audit system_config.yaml
       ```

       Example output:

       ```text
       k8s-api-auditing-enabled: false
       ncn-mgmt-node-auditing-enabled: false
       ```

   * Post-install

     If a system is installed, audit logging can be enabled through the Boot Script Service (BSS).

     * `ncn-mgmt-node-auditing-enabled`
     * `k8s-api-auditing-enabled`

2. Verify if audit logging is enabled.

   * `ncn-mgmt-node-auditing-enabled`
  
     ```bash
     ncn# cray bss bootparameters list --format json --hosts Global | jq '.[]."cloud-init"."meta-data"."k8s-api-auditing-enabled"'
     ```

   * `k8s-api-auditing-enabled`

     ```bash
     ncn# cray bss bootparameters list --format json --hosts Global | jq '.[]."cloud-init"."meta-data" "ncn-mgmt-node-auditing-enabled"'
     ```

3. Restart each NCN to apply the new settings after the CSI setting is changed.

   Follow the [Reboot NCNs](../node_management/Reboot_NCNs.md) procedure.

Host audit logs are stored in the `/var/log/audit/HostOS` directory on each NCN.
Host audit logging uses a maximum of 60GB on each NCN when using log rotation settings.
The log rotation settings are enabled after editing the CSI settings and rebooting the NCNs.

The Kubernetes API audit logs are stored in the `/var/log/audit/kl8s/apiserver` directory on each master NCN.
Kubernetes API audit logging uses a maximum of 1GB on each master NCN when using log rotation settings.

