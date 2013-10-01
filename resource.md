# Archimedes

To use queries below, see documentation on [Server Base URI](http://community.versionone.com/Developers/Developer-Library/Concepts/Server_Base_URI) and [HTTP Clients](http://community.versionone.com/Developers/Developer-Library/Concepts/HTTP_Client).

## VersionOne Members

```
<Server Base URI>/rest-1.v1/Data/Member?sel=Name,Email
```

We want a list of Members so we use `Member` as the [from](http://community.versionone.com/Developers/Developer-Library/Documentation/API/Queries/from).

We want all of the Members so there is no [where](http://community.versionone.com/Developers/Developer-Library/Documentation/API/Queries/where).

We want the name and email for the Member so we specify those attributes in the [select](http://community.versionone.com/Developers/Developer-Library/Documentation/API/Queries/select). If other attributes would be useful, the entire set of `Member` attributes can be viewed using meta:

```
<Server Base URI>/meta.v1/Member?xsl=api.xsl
```

## VersionOne Member Capacities for a Sprint

```
<Server Base URI>/rest-1.v1/Data/Capacity?where=Timebox='Timebox:1285'&sel=Member.Name,Member.Email,Value,Timebox.Name
```

We want capacity values so we use `Capacity` as the [from](http://community.versionone.com/Developers/Developer-Library/Documentation/API/Queries/from).

We want capacity values for a specific sprint so we use the OID `Timebox:1285` as the [where](http://community.versionone.com/Developers/Developer-Library/Documentation/API/Queries/where).

We want the member and the capacity value so we specify those attributes in the [select](http://community.versionone.com/Developers/Developer-Library/Documentation/API/Queries/select). For member, we grab `Name` for UI display and `Email` for linking to Clarity PPM resources. The `Timebox.Name` is selected only to confirm the where. If other attributes would be useful, the entire set of `Capacity` attributes can be viewed using meta:

```
<Server Base URI>/meta.v1/Capacity?xsl=api.xsl
```

## VersionOne Story Points per Epic for a Sprint

There are multiple ways to accomplish this goal. The main deciding factor should be which attributes matter more: workitem or epic. If the details of each workitem will be interesting to "show your work", then we use a query where `PrimaryWorkitem` is the `from`. We can expand the list of selected workitem attributes as needed. If the epic details are important, then we use a query where `Epic` is the `from`. We can expand the list of selected epic attributes as needed. If we want both, we would need 2 queries: one for workitem level attributes, and another for epic level attributes.

In any case, I prefer to work out the details of the query for leaf nodes first, in this case workitems. The roll-up to epics is more complex so it is helpful to have the leaf node query as reference.

```
<Server Base URI>/rest-1.v1/Data/PrimaryWorkitem?where=Timebox='Timebox:1285'&sel=Name,Number,Estimate,SuperAndUp:Epic[Category.Name='Initiative'].Name,SuperAndUp:Epic[Category.Name='Initiative'].Number,SuperAndUp:Epic[Category.Name='Initiative'].Reference,Timebox.Name
```

We want story points for all backlog items (including stories and defects) so we use `PrimaryWorkitem` as the [from](http://community.versionone.com/Developers/Developer-Library/Documentation/API/Queries/from).

We want story points only for a specific sprint so we use the OID `Timebox:1285` as the [where](http://community.versionone.com/Developers/Developer-Library/Documentation/API/Queries/where).

We want the story points and the epic so we specify those attributes in the [select](http://community.versionone.com/Developers/Developer-Library/Documentation/API/Queries/select). For story points, we need `Estimate`. The story `Name` and `Number` are provided for reference.

For epic, we need `Super.Name` for UI, `Super.Number` for UI ID, and `Super.Reference` for Clarity PPM reference. But the parent epic (`Super`) may not be an initiative epic so we need to walk the tree using `SuperAndUp` and filter for `Category.Name='Initiative'`. This is known as an attribute filter. To make sure we can filter on the `Category.Name` for epics, we also have to downcast `SuperAndUp` to `Epic`.

The `Timebox.Name` is selected only to confirm the where. If other attributes would be useful, the entire set of `PrimaryWorkitem` attributes can be viewed using meta:

```
<Server Base URI>/meta.v1/PrimaryWorkitem?xsl=api.xsl
```

Now to work out the aggregation for epics.

```
<Server Base URI>/rest-1.v1/Data/Epic?where=Category.Name='Initiative';SubsAndDown.Timebox=$TIMEBOX&sel=Name,Number,Reference,Category,SubsAndDown:PrimaryWorkitem[AssetState!='Dead';Timebox=$TIMEBOX].Estimate.@Sum&with=$TIMEBOX=Timebox:1358
```

We want to roll-up story points by epic so we use `Epic` as the [from](http://community.versionone.com/Developers/Developer-Library/Documentation/API/Queries/from).

We only want initiative epics with stories from a specific sprint. We use `Category.Name='Initiative'` to match initiative epics. We also check whether any of the `SubsAndDown` are in the specified timebox (the variable notation is explained below). The `SubsAndDown` is also downcast to PrimaryWorkitems since those are the only kind of children that we care about (in other words, not epics). Both are joined in the [where](http://community.versionone.com/Developers/Developer-Library/Documentation/API/Queries/where) by `;` for logical AND.

We want the name and ID of the initiative epic so we specify those attributes in the [select](http://community.versionone.com/Developers/Developer-Library/Documentation/API/Queries/select) as we did with the first query. For story points, we aggregate `SubsAndDown`. We downcast to `PrimaryWorkitem` so we get only those children. We filter on `AssetState!='Dead'` so we don't get deleted and template children (a quirk of the aggregation functions). We also filter on only the children that are in the specified `Timebox`. For those items, we sum the `Estimate` values.
