# Archimedes

To use queries below, see documentation on [Server Base URI](http://community.versionone.com/Developers/Developer-Library/Concepts/Server_Base_URI) and [HTTP Clients](http://community.versionone.com/Developers/Developer-Library/Concepts/HTTP_Client).

## VersionOne Feature Epics

```
<Server Base URI>/rest-1.v1/Data/Epic?where=Category.Name='Feature'&sel=Name,Number,Reference,Category
```

We want a list of Epics so we use `Epic` as the [from](http://community.versionone.com/Developers/Developer-Library/Documentation/API/Queries/from).

We want to filter on just Feature Epics so we use `Category.Name='Feature'` as the [where](http://community.versionone.com/Developers/Developer-Library/Documentation/API/Queries/where). If we only used `Category` we would need to specify an OID for the list value of `'Feature'`.

We want the name and ID for the feature epic so we specify those attributes in the [select](http://community.versionone.com/Developers/Developer-Library/Documentation/API/Queries/select). The VersionOne [OID](http://community.versionone.com/Developers/Developer-Library/Concepts/OID_Token) comes "free" with every query, so we don't need to specify it explicitly. But the ID that people see in the UI is `Number`. And if the ID we really want is a Clarity PPM Project ID, then we would need to store that in another attribute like `Reference`. (I recommend using custom field because other integrations may use and overwrite Reference for other purposes.) In the example, `Category` is added only as a way to confirm the where. If other attributes would be useful, the entire set of `Epic` attributes can be viewed using meta:

```
<Server Base URI>/meta.v1/Epic?xsl=api.xsl
```

## Implied Feature Epics

If users are not diligently marking the category with feature, then we might want to use an implied feature epic. In the implied approach, we assume the direct children of an initiative epic are feature epics.

```
<Server Base URI>/rest-1.v1/Data/Epic?where=Super.Category.Name='Initiative'&sel=Name,Number,Reference,Category,Super.Number,Super.Reference
```

We want a list of Epics so we use `Epic` as the [from](http://community.versionone.com/Developers/Developer-Library/Documentation/API/Queries/from).

We want to filter to the children of initiative epics so we use `Super.Category.Name='Feature'` as the [where](http://community.versionone.com/Developers/Developer-Library/Documentation/API/Queries/where). Actually, the filter is phrased from the perspective of the children. It asks for all the epics where the parent is an initiative epic.

We want the name and ID for the feature epic so we specify those attributes in the [select](http://community.versionone.com/Developers/Developer-Library/Documentation/API/Queries/select). The VersionOne [OID](http://community.versionone.com/Developers/Developer-Library/Concepts/OID_Token) comes "free" with every query, so we don't need to specify it explicitly. But the ID that people see in the UI is `Number`. And if the ID we really want is a Clarity PPM Project ID, then we would need to store that in another attribute like `Reference`. (I recommend using custom field because other integrations may use and overwrite Reference for other purposes.) To connect this information to the initiative epic, we also want the `Super.Number` and the `Super.Reference` (again, this might be a custom field instead).

In the example, `Category` is added only as a way to confirm the where.