---
title: Backfilling a CUR file
keywords: reporting, integrations, map, migration, modernization
tags: [map]
sidebar: mydoc_sidebar
permalink: backfilling-a-cur-file.html
folder: UserGuides
series: [Visibility]
weight: 9.3
---

{: .no_toc }

# Backfilling a CUR file for the MAP program #

If you're participating in AWS's Migration Assistance Program, you want to get the maximum credit from the MAP program.

One thing to consider is a longer trail of data within your Cost and Usage Report (CUR).  In order to get the most out of nOps, we recommend filling out a support ticket with AWS to do a CUR backfill.  This will load your CUR file with previous months' data, allowing you to get better insight.


- TOC
{:toc}

## Creating the Ticket for MAP CUR backfill ##

For each Payer account, log into your [Amazon Support Console](https://support.console.aws.amazon.com/support) to create a support ticket.  Please CC your point of contact at nOps in your support ticket, or inform them when the backfill has been completed.

### Ticket Information ###

<table>
<tr>
<td> <b>Item</b> </td><td> <b>What to enter</b> </td>
</tr>
<tr>
<td> <b>Type</b> </td><td> Billing</td>
</tr>
<tr>
<td> <b>Category</b> </td><td> Other Billing Questions</td>
</tr>
<tr>
<td> <b>Severity</b>: </td><td> General question</td>
</tr>
<tr>
<td> <b>Subject</b>: </td><td> Backfill CUR reports</td>
</tr>
<tr>
<td> <b>Description</b>: </td>
<td>

Please Backfill The CUR for the following:<br /><br />

<b>CUR Report name</b>: nopsbilling-hourly-parquet<br />

<b>Time Period</b>: <i>&lt;1 month prior to MAP Start date&gt;</i> to <i>&lt;the Month prior to the data in nOps&gt;</i> <br />

<b>Reason</b>: We moved to a finance reporting engine for tracking MAP credits that requires all CURs to be in parquet format

</td>
</tr>
</table>

{%include note.html content="After the process has completed, please confirm it's been completed with your nOps point of contact."%}



## Creating the Ticket for CUR backfill ##

For each Payer account, log into your [Amazon Support Console](https://support.console.aws.amazon.com/support) to create a support ticket.  Please CC your point of contact at nOps in your support ticket, or inform them when the backfill has been completed.

### Ticket Information ###

<table>
<tr>
<td> <b>Item</b> </td><td> <b>What to enter</b> </td>
</tr>
<tr>
<td> <b>Type</b> </td><td> Billing</td>
</tr>
<tr>
<td> <b>Category</b> </td><td> Other Billing Questions</td>
</tr>
<tr>
<td> <b>Severity</b>: </td><td> General question</td>
</tr>
<tr>
<td> <b>Subject</b>: </td><td> Backfill CUR reports</td>
</tr>
<tr>
<td> <b>Description</b>: </td>
<td>

Please Backfill The CUR for the following:<br /><br />

<b>CUR Report name</b>: nopsbilling-hourly-parquet<br />

<b>Time Period</b>: <i>&lt;Month you want data backfilled from&gt;</i> to <i>&lt;the Month prior to the data in nOps&gt;</i> <br />

<b>Reason</b>: We are leveraging a FinOps solution that requires all CURs to be in parquet format, and would like historical information for the dates listed to be backfilled.

</td>
</tr>
</table>




{%include note.html content="After the process has completed, please confirm it's been completed with your nOps point of contact."%}


<br/><br/>

{% include custom/series_related.html %}