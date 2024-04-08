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
Note - Most of the values can be overridden per nodegroup

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
| `instances`                    | Instance provisioners configurations for node types, families and sizing - extended below | `Map` | x | ✓ |
| `instances.minGeneration`      | The minimum instance generation to use (for example 4 = c4,c5,c6 etc) | `Integer` | x | ✓ |
| `instances.architecture`       | `amd64`, `arm64` or `multiarch` for nodegroups which can have combined architectures | `String` | x | ✓ |
| `instances.categories`         | Allowed instance categories (c, m, r) | `List` | x | ✓ |
| `instances.cores`              | Allowed instance with X cores (4, 8) | `List` | x | ✓ |
| `instances.capacityType`       | `spot`, `on-demand` (can use both on single provisioner) | `List` | x | ✓ |
| `nodegroups.{}.labels`         | Labels to add to nodes `<label_name>`: `<label_value>` | `Map` | ✓ | ✓ |
| `nodegroups.{}.annotations`    | Annotations to add to nodes `<annotation_name>`: `<annotation_value>` | `Map` | ✓ | ✓ |
| `nodegroups.{}.nodeClassRef`   | If you wish to use your own nodeClass, specify it [Documentation](https://karpenter.sh/docs/concepts/nodeclasses/) | `Map` | ✓ | ✓ |
| `nodegroups.{}.taints`         | Taints to add to nodes `- <taint_key>`: `<taint_value>`: `<taint_effect>` | `List(Map)` | ✓ | ✓ |
| `nodegroups.{}.startupTaints`  | startupTaints to add to nodes `- <taint_key>`: `<taint_value>`: `<taint_effect>` | `List(Map)` | ✓ | ✓ |
| `nodegroups.{}.instances.*`    | Explicitly specify instances override | `Map` | ✓ | ✓ |
| `nodegroups.{}.limits`         | Specify Limits [Documentation](https://karpenter.sh/docs/concepts/nodepools/#speclimits) | `Map` | ✓ | ✓ |
| `nodegroups.{}.nodeGroupLabel` | Override default generated nodegroup label | `String` | ✓ | ✓ |
| `nodegroups.{}.capacitySpread` | Set range of capacity spread keys (`integers`), set int for `start` and `end` | `Map` | ✓ | ✓ |
| `nodegroups.{}.*`              | Over-write all above which supports it | `Map` | ✓ | ✓ |
| `nodegroups.{}.weight`         | Specify NodeGroup Weight (default is `1`) | `Integer` | ✓ | ✓ |
| `nodegroups.{}.excludeFamilies`| Exclude specific instance families | `List` | ✓ | ✓ |
| `nodegroups.{}.budgets`        | Specify Disruption Budgets [Documentation](https://karpenter.sh/docs/concepts/disruption/#nodes) | `List` | ✓ | ✓ |

### Headroom Configuration
|  Key Name                      | Description | Type | Optional? | Optional Per NodeGroup? |
| ------------------------------ | ----------- | ---- | --------- | ----------------------- |
| `nodegroups.{}.nodeHeadRooms`  | Specify Amount of nodes which will have reserved headroom | `String` | ✓ | ✓ |
| `nodegroups.{}.nodeHeadRooms.size` | `small`, `medium`, `large`, `xlarge` - see below | `String` | ✓ | ✓ |
| `nodegroups.{}.nodeHeadRooms.count` | Number of headroom pods to schedule | `Integer` | ✓ | ✓ |
| `nodegroups.{}.dedicatedNodeHeadRooms` | Specify Amount of empty nodes ready as headroom | `String` | ✓ | ✓ |
| `nodegroups.{}.dedicatedNodeHeadRooms.size` | `small`, `medium`, `large`, `xlarge` - see below | `String` | ✓ | ✓ |
| `nodegroups.{}.dedicatedNodeHeadRooms.count` | Number of headroom nodes to schedule | `Integer` | ✓ | ✓ |



## Headroom Sizing

|  Size | CPU | Ram |
| ----- | --- | --- |
| `small` | 1 | 4Gi |
| `medium` | 2 | 8Gi |
| `large` | 4 | 16Gi |
| `xlarge` | 8 | 32Gi |
