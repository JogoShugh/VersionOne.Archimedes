# Archimedes

## Mapping from AgileVision Integration

<table border="1">
<tr><th>AV.V1</th><th>Hierarchy</th><th>Clarity PPM</th><th>VersionOne (Top-Down Option)</th><th>VersionOne (Bottom-Up Option)</th></tr>
<tr><td>Project/Release</td><td>&nbsp;</td><td>Project</td><td>Epic (Initiative)</td><td>Epic (Initiative)</td></tr>
<tr><td>Sprint</td><td>Child</td><td>Task (Summary)</td><td>Epic (Feature)</td><td>Epic (Feature)</td></tr>
<tr><td>User Story</td><td>Child</td><td>Task (Summary)</td><td>Epic (Sub-Feature)</td><td>PrimaryWorkitem (Story, Defect, TestSet)</td></tr>
<tr><td>Task</td><td>Child</td><td>Task</td><td>Epic (Non-Functional?)</td><td>SecondaryWorkitem (Task, Test)</td></tr>
<tr><td>Worklog</td><td>Child</td><td>Timesheet Entry</td><td>Actual</td><td>Actual</td></tr>
<tr><td>Resource</td><td>&nbsp;</td><td>Resource</td><td>Member</td><td>Member</td></tr>
</table>

Epic types given as example, not for implementation. Implementation should probably use relative location in Epic hierarchy.

## Top-Down Option

*Pros*

* Simple implementation for integration. No need to compensate for VersionOne options about level of time-tracking.

*Cons*

* May not be granular enough for CapEx/OpEx tracking, if people need to distinguish User Stories from Defects.

## Bottom-Up Option

*Pros*

* Fine-grained detail available in both tools.

*Cons*

* May be difficult to accommodate n-levels of Epics.
* Handling V1 options for effort entry could introduce more logic paths for integration.
