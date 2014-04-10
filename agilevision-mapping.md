# Mapping Clarity PPM to VersionOne

All VersionOne references below refer to the [asset type](https://community.versionone.com/Developers/Developer-Library/Concepts/Asset_Type) found in [meta.v1](https://community.versionone.com/Developers/Developer-Library/Documentation/API/Endpoints/meta.v1).

## Agile Vision Mapping

Map directly to the concepts suggested by the Agile Vision Interfaces.

<table border="1" width="100%">
	<tr>
		<th>Clarity PPM</th>
		<th>Agile Vision</th>
		<th>Hierarchy</th>
		<th>VersionOne</th>
	</tr>
	<tr>
		<td>Project</td>
		<td>Release</td>
		<td>root</td>
		<td>Scope</td>
	</tr>
	<tr>
		<td>Task</td>
		<td>Sprint</td>
		<td>child of Release</td>
		<td>Timebox</td>
	</tr>
	<tr>
		<td>Task</td>
		<td>User Story</td>
		<td>child of Sprint</td>
		<td>PrimaryWorkitem</td>
	</tr>
	<tr>
		<td>Task</td>
		<td>Task</td>
		<td>child of User Story</td>
		<td>SecondaryWorkitem</td>
	</tr>
	<tr>
		<td>Timesheet Entry</td>
		<td>Worklog</td>
		<td>child of Task</td>
		<td>Actual</td>
	</tr>
	<tr>
		<td>Resource</td>
		<td>Resource</td>
		<td>root</td>
		<td>Member</td>
	</tr>
</table>

### Pros

* Already familiar to CA folks (sales, services, customers).

### Cons

* Poor information value for the PMO and the business. Both Release and Sprint are just time boxes in agile, thus the "percent complete" for these items are a simple function of time. These provide no insight into project health or progress against business value.
* Fails to empower the PMO and business with advantages of agile. While waterfall projects cannot recognize business value until they reach the final phase (production), agile projects can recognize business value as features are completed. Therefore, the business can decide to stop an agile project and still realize partial business value.
* Misses Epics. Customers have already indicated they want correlation between Clarity PPM Projects and VersionOne Epics.
* No option for course-grained financial categorization. All categorization decisions are pushed to fine-grained, detailed workitems such as stories and tasks.
* Forces VersionOne customers to adopt one-sprint-per-project convention. The Scope to Timebox relationship is many-to-many. Few customers use a one-to-many convention. A one-to-many convention is neither recommended nor enforceable.
* Forces VersionOne customers to adopt effort tracking at Task/Test level. This is one of 4 options for tracking (None, Story/Defect level, Task/Test level, or both).
* Very high coupling. The integration is related to many VersionOne asset types, increasing the complexity of building it. Many difficult corner cases increase the difficulty of coding and testing. Corner cases derive from the reliance on multiple conventions and limited user-interaction in automated jobs.

## Bottom Up Mapping

Ignore the top-level Agile Vision concepts (release and sprint), shoehorning epics into those "slots". Match up on story and task.

<table border="1" width="100%">
	<tr>
		<th>Clarity PPM</th>
		<th>Agile Vision</th>
		<th>Hierarchy</th>
		<th>VersionOne</th>
	</tr>
	<tr>
		<td>Project</td>
		<td>Release</td>
		<td>root</td>
		<td>Epic</td>
	</tr>
	<tr>
		<td>Task</td>
		<td>Sprint</td>
		<td>child of Release</td>
		<td>Epic</td>
	</tr>
	<tr>
		<td>Task</td>
		<td>User Story</td>
		<td>child of Sprint</td>
		<td>PrimaryWorkitem</td>
	</tr>
	<tr>
		<td>Task</td>
		<td>Task</td>
		<td>child of User Story</td>
		<td>SecondaryWorkitem</td>
	</tr>
	<tr>
		<td>Timesheet Entry</td>
		<td>Worklog</td>
		<td>child of Task</td>
		<td>Actual</td>
	</tr>
	<tr>
		<td>Resource</td>
		<td>Resource</td>
		<td>root</td>
		<td>Member</td>
	</tr>
</table>

### Pros

* Provides 2 options for financial categorization: high-level and low-level. With both course-grained (epics) and fine-grained (story/tasks) detail in both products, customers can make option at either level.
* Focus on Epics. Customers have already indicated they want correlation between Clarity PPM Projects and VersionOne Epics.
* Relevant information for the PMO and the business. Progress on epics is more indicative of project health and progress against business value.
* Empowers the PMO and business with advantages of agile. While waterfall projects cannot recognize business value until they reach the final phase (production), agile projects can recognize business value as features are completed. Therefore, the business can decide to stop an agile project and still realize partial business value.
* Avoids data impedance mismatch with project-sprint.

### Cons

* High coupling. The integration is related to multiple VersionOne asset types, increasing the complexity of building it. Some corner cases increase the difficulty of building and testing. Corner cases derive from the reliance on effort tracking convention and limited user-interaction in automated jobs.
* May be difficult to accommodate n-levels of Epics. We believe more than 2 levels of epics is rare.
* Handling VersionOne options for effort entry would introduce more logic paths for integration.

## Top Down Mapping

Ignore the Agile Vision concepts but leverage the structure that it generates in Clarity PPM. Start from the top of the Epic hierarchy and work the way down.

<table border="1" width="100%">
	<tr>
		<th>Clarity PPM</th>
		<th>Agile Vision</th>
		<th>Hierarchy</th>
		<th>VersionOne</th>
	</tr>
	<tr>
		<td>Project</td>
		<td>Release</td>
		<td>root</td>
		<td>Epic (Initiative)</td>
	</tr>
	<tr>
		<td>Task</td>
		<td>Sprint</td>
		<td>child of Release</td>
		<td>Epic (Feature)</td>
	</tr>
	<tr>
		<td>Task</td>
		<td>User Story</td>
		<td>child of Sprint</td>
		<td>Epic (Sub-Feature)</td>
	</tr>
	<tr>
		<td>Task</td>
		<td>Task</td>
		<td>child of User Story</td>
		<td>Epic (Non-Functional)</td>
	</tr>
	<tr>
		<td>Timesheet Entry</td>
		<td>Worklog</td>
		<td>child of Task</td>
		<td>Actual</td>
	</tr>
	<tr>
		<td>Resource</td>
		<td>Resource</td>
		<td>root</td>
		<td>Member</td>
	</tr>
</table>

### Pros

* Moderate coupling. Reduces the coupling to VersionOne asset types.
No need to compensate for VersionOne options about level of time-tracking. The entire Clarity PPM WBS is constructed from one type of VersionOne asset.
* Focus on Epics. Customers have already indicated they want correlation between Clarity PPM Projects and VersionOne Epics.
* Relevant information for the PMO and the business. Progress on epics is more indicative of project health and progress against business value.
* Empowers the PMO and business with advantages of agile. While waterfall projects cannot recognize business value until they reach the final phase (production), agile projects can recognize business value as features are completed. Therefore, the business can decide to stop an agile project and still realize partial business value.
* Avoids data impedance mismatch with project-sprint and effort tracking.

### Cons

* Forces change in use of epics. We believe more than 2 levels of epics is rare. Existing VersionOne customers would have to change their practices.
* Forces a structure of 4 epics in order to have timesheet data. We have been unable to find any plausible story to explain why 4 levels of epics should be required. It seems unlikely that any customer would adopt this convention.
* May not be granular enough for financial categorization. For example, customers may need to distinguish stories from defects.

## Two-Level Mapping

Deviate from the Agile Vision concepts and structure. Map Clarity PPM Project to a VersionOne Feature Epic. Pull back 1 level of children to populate WBS for time entry.

<table border="1" width="100%">
	<tr>
		<th>Clarity PPM</th>
		<th>Agile Vision</th>
		<th>Hierarchy</th>
		<th>VersionOne</th>
	</tr>
	<tr>
		<td>Project</td>
		<td>Release/Project</td>
		<td>root</td>
		<td>Epic</td>
	</tr>
	<tr>
		<td>Task</td>
		<td>Task</td>
		<td>child of Release</td>
		<td>Workitem</td>
	</tr>
	<tr>
		<td>Timesheet Entry</td>
		<td>Worklog</td>
		<td>child of Sprint</td>
		<td>Actual</td>
	</tr>
	<tr>
		<td>Resource</td>
		<td>Resource</td>
		<td>root</td>
		<td>Member</td>
	</tr>
</table>

### Pros

* Simplest approach to explain to both agile team and PMO; hence, the simplest to put into practice.
* Fits well with recommendations for and common use of VersionOne. Allows users greatest flexibility in how they manage their projects in VersionOne because it has the least burden of implied conventions.
* Minimal coupling. Minimizes the coupling to VersionOne asset types.
No need to compensate for VersionOne options about level of time-tracking. The entire Clarity PPM WBS is constructed from one type of VersionOne asset. There are fewer interfaces to implement in code.
* Focus on Epics. Customers have already indicated they want correlation between Clarity PPM Projects and VersionOne Epics.
* Relevant information for the PMO and the business. Progress on epics is more indicative of project health and progress against business value.
* Empowers the PMO and business with advantages of agile. While waterfall projects cannot recognize business value until they reach the final phase (production), agile projects can recognize business value as features are completed. Therefore, the business can decide to stop an agile project and still realize partial business value.
* Avoids data impedance mismatch with project-sprint and effort tracking.

### Cons

* Requires some changes to the way PMO uses Clarity PPM.
* May not be sufficient for financial categorization (Cap/Op). For some orgs, the decision is based on sprint. For example, first 2 sprints are OpEx as risk is removed and the backlog is elaborated, remaining sprints are CapEx as the software asset is built out. For some orgs, the decision is based on the more detailed data. For example, the decision might be made for each task as opposed to each user story/defect.
