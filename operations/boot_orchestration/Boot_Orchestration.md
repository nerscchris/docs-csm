# Boot Orchestration

The Boot Orchestration Service \(BOS\) is responsible for booting, configuring, and shutting down collections of nodes.

There are currently two supported api versions for BOS.  BOS v1 is strictly session based and launches a Boot Orchestration Agent \(BOA\) that fulfills boot requests.  BOS v2 takes a more flexible approach and relies on a number of permanent operators to guide components through state transitions in an independent manner.  For more information see ________TODO________

BOS users create BOS session templates via the REST API. A session template is a collection of metadata for a group of nodes and their desired boot artifacts and configuration. A BOS session can then be created by applying an action to a session template. The available actions are boot, reboot, shutdown. BOS coordinates with the underlying subsystems to complete the action requested, and session can be monitored to determine the status of the request.

BOS depends on each of the following services to complete its tasks:

-   Boot Script Service \(BSS\) - Stores the configuration information that is used to boot each hardware component. Nodes consult BSS for their boot artifacts and boot parameters when nodes boot or reboot.
-   Configuration Framework Service \(CFS\) - BOA launches CFS to apply configuration to the nodes in its boot sets \(node personalization\).
-   Cray Advanced Platform Monitoring and Control \(CAPMC\) - Used to power on and off the nodes.
-   Hardware State Manager \(HSM\) - Tracks the state of each node and what groups and roles nodes are included in.

### Use the BOS Cray CLI Commands

BOS commands are available using the Cray CLI commands. For ease of use, the latest version of BOS can be used without specifying the version, but it is recommended that users still specify the version in scripts or documentation, as un-versioned cli command behavior may change.  API information, including the default api version can be found with the following command:

```bash
cray bos list
```

Example output:

```
[[results]]
major = "2"
minor = "0"
patch = "0"
[[links]]
href = "https://api-gw-service-nmn.local/apis/bos/"
rel = "self"

[[links]]
href = "https://api-gw-service-nmn.local/apis/bos/v2"
rel = "versions"
```
