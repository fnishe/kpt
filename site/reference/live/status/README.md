---
title: "Status"
linkTitle: "status"
type: docs
description: >
   Status shows the status for the resources in the cluster
---
<!--mdtogo:Short
    Status shows the status for the resources in the cluster
-->

The status command will print the status for all resources in the live state
that belong to the current inventory. It will use the [Inventory Template] to
look up the set of resources in the inventory in the live state and poll all
those resources for their status until either an exit criteria has been met
or the process is cancelled.

### Examples

{{% hide %}}

<!-- @makeWorkplace @verifyExamples-->
```
# Set up workspace for the test.
TEST_HOME=$(mktemp -d)
cd $TEST_HOME
```

<!-- @fetchPackage @verifyExamples-->
```shell
export SRC_REPO=https://github.com/GoogleContainerTools/kpt.git
kpt pkg get $SRC_REPO/package-examples/helloworld-set@next my-app
```

<!-- @createKindCluster @verifyExamples-->
```
kind delete cluster && kind create cluster
```

<!-- @installResourceGroup @verifyExamples-->
```
kpt live install-resource-group
```

<!-- @initCluster @verifyExamples-->
```
kpt live init my-app
kpt live apply my-app
```

{{% /hide %}}

<!--mdtogo:Examples-->
<!-- @liveStatus @verifyExamples-->
```shell
# Monitor status for a set of resources based on manifests. Wait until all
# resources have reconciled.
kpt live status my-app/
```

```shell
# Monitor status for a set of resources based on manifests. Output in table format:
kpt live status my-app/ --poll-until=forever --output=table
```
<!--mdtogo-->

### Synopsis
<!--mdtogo:Long-->
```
kpt live status (DIR | STDIN) [flags]
```

#### Args

```
DIR | STDIN:
  Path to a directory if an argument is provided or reading from stdin if left
  blank. In both situations one of the manifests must contain exactly one
  ConfigMap with the inventory template annotation.
```

#### Flags

```
--poll-period (duration):
  The frequency with which the cluster will be polled to determine the status
  of the applied resources. The default value is every 2 seconds.

--poll-until (string):
  When to stop polling for status and exist. Must be one of the following:
    known:   Exit when the status for all resources have been found.
    current: Exit when the status for all resources have reached the Current status.
    deleted: Exit when the status for all resources have reached the NotFound
             status, i.e. all the resources have been deleted from the live state.
    forever: Keep polling for status until interrupted.
  The default value is ‘known’.

--output (string):
  Determines the output format for the status information. Must be one of the following:
    events: The output will be a list of the status events as they become available.
    table:  The output will be presented as a table that will be updated inline
            as the status of resources become available.
  The default value is ‘events’.

--timeout (duration):
  Determines how long the command should run before exiting. This deadline will
  be enforced regardless of the value of the --poll-until flag. The default is
  to wait forever.
```
<!--mdtogo-->

[Inventory Template]: /reference/live/apply/#prune