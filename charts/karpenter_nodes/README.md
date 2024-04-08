## Fiverr Public Helm Templates - Karpenter Nodes

## Table of contents
- [Table of contents](#table-of-contents)
- [Introduction](#introduction)
- [Working with Helm](#working-with-helm)
  - [Testing Your Changes](#testing-your-changes)
- [Configuration keys](#configuration-keys)

## Introduction

This Helm Template is designed to generate NodeClasses and NodePools using [Karpenter](https://karpenter.sh/) in addition to optional HeadRoom.

The template follows a naming convention which is comprised of the `nodegroup` name and its architecture (amd64, arm64 or multiarch).

For example `nodes-default-amd64`

### UserData
The `UserData` field supports templating and your own values. You can take a look at the `userdata_example_values.yaml` file for an example.

## Working with Helm

### Todo - add helm install command when repo is public and alive with real url

### Testing Your Changes
After making changes you will probably want to see the new output. Run `helm template` with the relevant example files: </br>
`helm template . -f values.yaml`

### Unit Tests
Make sure you have `helm-unittest` plugin installed. [helm-unittest](https://github.com/helm-unittest/helm-unittest)
Unit tests are written in `tests` directory. To run the tests, use the following command: </br>
`helm unittest --helm3 karpenter_nodes -f "tests/$value/*_test.yaml"`


## Configuration keys
Note - Most of the values can be overridden per nodegroup (If not specified, it will use the default (Global) values)


|  Key Name                      | Description | Type | Optional? | Optional Per NodeGroup? |
| ------------------------------ | ----------- | ---- | --------- | ----------------------- |
| `ApiVersion`                   | ApiVersion used in Karpenter's CRD | `String` | × | × |
| `IamRole`                      | The IAM Role which will be attached to the instance <br> via instance-profile (not required if `IamInstanceProfile` is specified) | `String` | x | ✓ |
| `IamInstanceProfile`           | Existing instance profile To set on the instances <br>(not required if `IamRole` is specified)| `String` | x | ✓ |
| `amiFamily`                    | AMIFamily to use (Default to AL2) [Documentation](https://karpenter.sh/docs/concepts/nodeclasses/#specamifamily) | `String` | x | ✓ |
| `amiSelectorTerms`             | AMI Selector Terms (This will override `amiFamily`) [Documentation](https://karpenter.sh/docs/concepts/nodeclasses/#specamiselectorterms) | `List(Map)` | x | ✓ |
| `subnetSelectorTerms`          | Selector for Subnets | `List(Map)` [Documentation](https://karpenter.sh/docs/concepts/nodeclasses/#specsubnetselectorterms) | x | ✓ |
| `securityGroupSelectorTerms`   | Selector for Security Groups | `List(Map)` [Documentation](https://karpenter.sh/docs/concepts/nodeclasses/#specsecuritygroupselectorterms) | x | ✓ |
| `nodeGroupLabelName`           | The Name of the label for each nodegroup (default is `nodegroup`) | `String` | x | ✓ |
| `nodeTags`                     | Tags to add to the instances `<tag_name>`: `<tag_value>` | `Map` | ✓ | ✓ |
| `additionalNodeTags`           | Additional Tags to add to the instances `<tag_name>`: `<tag_value>` | `Map` | ✓ | ✓ |
| `nodegroups.{}`                | each will be used to setup a provisioner and template based on the nodegrup name key | `List[Maps]` | x | ✓ |
| `blockDeviceMappings`          | Block Device Mappings [Documentation](https://karpenter.sh/docs/concepts/nodeclasses/#specblockdevicemappings) | `List(Map)` | x | ✓ |
| `instanceStorePolicy`          | Instance Store Policy [Documentation](https://karpenter.sh/docs/concepts/nodeclasses/#specinstancestorepolicy) | `String` | ✓ | ✓ |
| `metaDataHttpEndpoint`         | Metadata HTTP Endpoint [Documentation](https://karpenter.sh/docs/concepts/nodeclasses/#specmetadataoptions) | `String` | x | ✓ |
| `metaDataHttpProtocolIPv6`     | Metadata HTTP Protocol IPv6 [Documentation](https://karpenter.sh/docs/concepts/nodeclasses/#specmetadataoptions) | `String` | x | ✓ |
| `metaDataHttpPutResponseHopLimit` | Metadata HTTP Put Response Hop Limit [Documentation](https://karpenter.sh/docs/concepts/nodeclasses/#specmetadataoptions) | `String` | x | ✓ |
| `metaDataHttpTokens`           | Metadata HTTP Tokens [Documentation](https://karpenter.sh/docs/concepts/nodeclasses/#specmetadataoptions) | `String` | x | ✓ |
| `userData`                     | User Data (supports templating and your own values) | `MultilineString` | ✓ | ✓ |
| `instances`                    | Instance configurations for node types, families and sizing - see below | `Map` | x | ✓ |
| `instances.minGeneration`      | The minimum instance generation to use (for example 4 = c4,c5,c6 etc) | `Integer` | x | ✓ |
| `instances.architecture`       | `amd64`, `arm64` or `multiarch` for nodegroups which can have combined architectures | `String` | x | ✓ |
| `instances.categories`         | Allowed instance categories (c, m, r) | `List(String)` | x | ✓ |
| `instances.cores`              | Allowed cores per instance (`"4"`, `"8"`) | `List(String(int))` | x | ✓ |
| `instances.capacityType`       | `spot`, `on-demand` (can use both on single provisioner) | `List(String)` | x | ✓ |
| `instances.operatingSystems`   | Allowed operating systems (`"linux"`, `"windows"`) | `List(String)` | x | ✓ |
| `availabilityZones`            | Availability Zones to use | `List(String)` | x | ✓ |
| `expireAfter`                  | Specify how long node should be up before refreshing it [Documentation](https://karpenter.sh/docs/concepts/disruption/#automated-methods) | `String` | x | ✓ |
| `weight`                       | Specify NodeGroup Weight (default is `1`) | `Integer` | x | ✓ |
| `excludeFamilies`              | Exclude specific instance families | `List` | x | ✓ |
| `consolidationPolicy`          | Specify how to consolidate nodes [Documentation](https://karpenter.sh/docs/concepts/nodepools/) | `String` | x | ✓ |
| `consolidateAfter`             | Specify how long to wait before consolidating nodes [Documentation](https://karpenter.sh/docs/concepts/nodepools/) | `String` | ✓ | ✓ |
| `excludeInstanceSize`          | Exclude specific instance sizes | `List` | ✓ | ✓ |
| `headRoom`                     | Generate Ultra Low Priority Class for Headroom (see below) | `String` | ✓ | x |
| `nodegroups.{}.labels`         | Labels to add to nodes `<label_name>`: `<label_value>` | `Map` | ✓ | ✓ |
| `nodegroups.{}.annotations`    | Annotations to add to nodes `<annotation_name>`: `<annotation_value>` | `Map` | ✓ | ✓ |
| `nodegroups.{}.nodeClassRef`   | If you wish to use your own nodeClass, specify it [Documentation](https://karpenter.sh/docs/concepts/nodeclasses/) | `Map` | ✓ | ✓ |
| `nodegroups.{}.taints`         | Taints to add to nodes `- <taint_key>`: `<taint_value>`: `<taint_effect>` | `List(Map)` | ✓ | ✓ |
| `nodegroups.{}.startupTaints`  | startupTaints to add to nodes `- <taint_key>`: `<taint_value>`: `<taint_effect>` | `List(Map)` | ✓ | ✓ |
| `nodegroups.{}.limits`         | Specify Limits [Documentation](https://karpenter.sh/docs/concepts/nodepools/#speclimits) | `Map` | ✓ | ✓ |
| `nodegroups.{}.capacitySpread` | Set range of capacity spread keys (`integers`), set int for `start` and `end` | `Map` | ✓ | ✓ |
| `nodegroups.{}.excludeFamilies`| Exclude specific instance families | `List` | ✓ | ✓ |
| `nodegroups.{}.budgets`        | Specify Disruption Budgets [Documentation](https://karpenter.sh/docs/concepts/disruption/#nodes) | `List` | ✓ | ✓ |
| `nodegroups.{}.*`              | Over-write all above which supports it | `Map` | ✓ | ✓ |
| `nodegroups.{}.instances.*`    | Explicitly specify instances override, if using default specify `instances: {}` | `Map` | ✓ | ✓ |

### Headroom Configuration
Headroom will create `pause` pods with requetss to just keep empty nodes up and ready for scheduling. This is useful for scaling up quickly when needed.
The pods will be configured with ultra-low priority, and will be terminated and recreated on new nodes to free them up for usage if needed.
|  Key Name                      | Description | Type | Optional? | Optional Per NodeGroup? |
| ------------------------------ | ----------- | ---- | --------- | ----------------------- |
| `nodegroups.{}.headRoom`       | List of headroom configurations for the nodePool | `List(Map)` | ✓ | ✓ |
| `nodegroups.{}.headRoom.size`  | `small`, `medium`, `large`, `xlarge` - see below | `String` | ✓ | ✓ |
| `nodegroups.{}.headRoom.count` | Number of headroom pod replicas to schedule | `Integer` | ✓ | ✓ |
| `nodegroups.{}.headRoom.antiAffinitySpec` | Required - set antiaffinity to match against all running workloads | `LabelSelectorSpec` | ✓ | ✓ |
| `nodegroups.{}.headRoom.nameSpaces` | Specify list of namespaces to match again (default `all`) | `List(String)` | ✓ | ✓ |

## Headroom Sizing

|  Size | CPU | Ram |
| ----- | --- | --- |
| `small` | 1 | 4Gi |
| `medium` | 2 | 8Gi |
| `large` | 4 | 16Gi |
| `xlarge` | 8 | 32Gi |
