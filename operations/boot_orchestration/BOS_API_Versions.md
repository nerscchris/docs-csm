# Boot Orchestration

The Boot Orchestration Service \(BOS\) currently supports two API versions, v1 and v2, that have different apis and underlying mechanisms for performing operations on nodes.  The following is a summary of the changes, and the upgrade path, for users wishing to compare the two.

## BOS v2 improvements

BOS v2 makes significant improvements to boot times, retries and error handling, by allowing nodes to proceed through the boot process at their own pace.  Nodes that require extra time or retries to complete will no longer hold up the rest the nodes, meaning that more nodes will reach a ready state faster.

## API Differences

The v2 endpoints have plural nouns, using `sessions` and `sessiontemplates` rather than `session` and `sessiontemplate`.  This is more consistent with our other apis.

BOS v2 also introduces two new endpoints.  A `/components` endpoint for tracking state at a components level and an `/options` endpoint to allow changing global options on the fly.  See [Components](Components.md) and [Options](Options.md) for more information on these new endpoints.

There are some differences responses in BOS v2 as well.  Listing sessions will now result in full session records rather than just a list of the current session names.  The session status endpoint has also changed significantly.  See [View the Status of a BOS Session](View_the_Status_of_a_BOS_Session.md) for more information on the session status endpoint.

## Upgrading from V1 to V2

To upgrade to BOS v2, users only need to start specifying v2 in the cli or api.  Scripts may also need to be updated to use the plural `sessions` and `sessiontemplates`.  Otherwise 

When BOS is upgraded to a version that supports the v2 endpoints, it will automatically migrate all session templates to a v2 form.  v1 versions of the session templates will also remain with `_deprecated` appended to the name.

## Mechanical Differences

When a session is created in BOS v1, BOS starts a Kubernetes job called the Boot Orchestration Agent \(BOA\), which manages all of the nodes.  BOA moves one phase at a time, ensuring that all nodes have completed the phase before moving on to the next.  This means that all nodes proceed in lock step.

BOS v2 does away with BOA, replacing it with long-running operators that are each responsible for moving nodes through a particular phase transition.  These operators each monitor nodes at the individual level, allowing each node to proceed at it's own pace, improving retry handling and speeding up the overall booting process.

## The CLI

If no version is specified, the BOS cli will currently default to the `v1` endpoints.  However this will not always be the case and the default version will be switched to `v2` in the future.  Scripts should always provide a version to the cli, or bypass the cli and talk straight to the api to avoid compatibility issues in the future.
