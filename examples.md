# Archimedes

To use queries below, see documentation on [Server Base URI](http://community.versionone.com/Developers/Developer-Library/Concepts/Server_Base_URI) and [HTTP Clients](http://community.versionone.com/Developers/Developer-Library/Concepts/HTTP_Client).

## VersionOne Initiative Epics

```
<Server Base URI>/rest-1.v1/Data/Epic?where=Category.Name='Initiative'&sel=Name,Number,Reference,Category
```

We want a list of Epics so we use `Epic` as the [from](http://community.versionone.com/Developers/Developer-Library/Documentation/API/Queries/from).

We want to filter on just Initiative Epics so we use `Category.Name='Initiative'` as the [where](http://community.versionone.com/Developers/Developer-Library/Documentation/API/Queries/where). If we only used `Category` we would need to specify an OID for the list value of 'Initiative'.

We want the name and ID for the initiative epic so we specify those attributes in the [select](http://community.versionone.com/Developers/Developer-Library/Documentation/API/Queries/select). The VersionOne OID comes "free" with every query, so we don't need to specify it explicitly. But the ID that people see in the UI is `Number`. And if the ID we really want is a Clarity PPM Project ID, then we would need to store that in another attribute like `Reference`. (I recommend using custom field because other integrations may use and overwrite Reference for other purposes.) In the example, `Category` is added only as a way to confirm the where. If other attributes would be useful, the entire set of Epic attributes can be viewed using meta:

```
<Server Base URI>/meta.v1/Epic?xsl=api.xsl
```

## VersionOne Initiative Epics

```
<Server Base URI>/rest-1.v1/Data/Member?sel=Name,Email
```

We want a list of Members so we use `Member` as the [from](http://community.versionone.com/Developers/Developer-Library/Documentation/API/Queries/from).

We want all of the Members so there is no [where](http://community.versionone.com/Developers/Developer-Library/Documentation/API/Queries/where).

We want the name and email for the Member so we specify those attributes in the [select](http://community.versionone.com/Developers/Developer-Library/Documentation/API/Queries/select). If other attributes would be useful, the entire set of Epic attributes can be viewed using meta:

```
<Server Base URI>/meta.v1/Member?xsl=api.xsl
```

## VersionOne Member Capacities for a Sprint

```
<Server Base URI>/rest-1.v1/Data/Capacity?where=Timebox='Timebox:1285'&sel=Member.Name,Member.Email,Value,Timebox.Name
```

We want capacity values so we use `Capacity` as the [from](http://community.versionone.com/Developers/Developer-Library/Documentation/API/Queries/from).

We want capacity values for a specific sprint so we use the OID `Timebox:1285` as the [where](http://community.versionone.com/Developers/Developer-Library/Documentation/API/Queries/where).

We want the member and the capacity value so we specify those attributes in the [select](http://community.versionone.com/Developers/Developer-Library/Documentation/API/Queries/select). For member, we grab `Name` for UI display and `Email` for linking to Clarity PPM resources. The `Timebox.Name` is selected only to confirm the where. If other attributes would be useful, the entire set of Epic attributes can be viewed using meta:

```
<Server Base URI>/meta.v1/Capacity?xsl=api.xsl
```

## VersionOne Story Points per Epic for a Sprint

```
<Server Base URI>/rest-1.v1/Data/PrimaryWorkitem?where=Timebox='Timebox:1285'&sel=Name,Number,Estimate,SuperAndUp:Epic[Category.Name='Initiative'].Name,SuperAndUp:Epic[Category.Name='Initiative'].Number,SuperAndUp:Epic[Category.Name='Initiative'].Reference,Timebox.Name
```

We want story points for all backlog items (including stories and defects) so we use `PrimaryWorkitem` as the [from](http://community.versionone.com/Developers/Developer-Library/Documentation/API/Queries/from).

We want story points only for a specific sprint so we use the OID `Timebox:1285` as the [where](http://community.versionone.com/Developers/Developer-Library/Documentation/API/Queries/where).

We want the story points and the epic so we specify those attributes in the [select](http://community.versionone.com/Developers/Developer-Library/Documentation/API/Queries/select). For story points, we need `Estimate`. The story `Name` and `Number` are provided for reference.

For epic, we need `Super.Name` for UI, `Super.Number` for UI ID, and `Super.Reference` for Clarity PPM reference. But the parent epic (`Super`) may not be an initiative epic so we need to walk the tree using `SuperAndUp` and filter for `Category.Name='Initiative'`. This is known as an attribute filter. To make sure we can filter on the `Category.Name` for epics, we also have to downcast `SuperAndUp` to `Epic`.

The `Timebox.Name` is selected only to confirm the where. If other attributes would be useful, the entire set of Epic attributes can be viewed using meta:

```
<Server Base URI>/meta.v1/Story?xsl=api.xsl
```
