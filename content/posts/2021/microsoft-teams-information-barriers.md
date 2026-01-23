---
title: 'Microsoft Teams: Information Barriers'
date: '2021-08-16T08:59:00-04:00'
author: 'Ryan Robinson'
category: "Microsoft 365"
series: MS-700
---

What if you need to partition your organization so that some members of your tenant cannot communicate with others in Teams? For example, maybe you have interns who you do want to have Teams access but not to any sensitive information.

Information barriers are the solution you need. You can block interaction across the barriers so that the interns cannot see any content from anybody else or vice versa.

Creating the barriers is done by PowerShell. There is not currently a web admin interface to do it.

Create one organization segment for interns:

```powershell
New-OrganizationSegment -Name "Interns" -UserGroupFilter "Title -contains 'Intern'"
```

Repeat for another segment called staff:

```powershell
New-OrganizationSegment -Name "Staff" -UserGroupFilter "Title -not (-contains 'Intern')"
```

Now that you have the two segments, create the barrier between them:

```powershell
New-InformationBarrierPolicy -Name "Interns" -AssignedSegment "Interns" -SegmentsBlocked "Staff" -State Inactive
```

Finally, apply the information barriers:

```powershell
Start-InformationBarrierPoliciesApplication
```

It could take several hours to go into effect.
