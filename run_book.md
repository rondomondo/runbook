## All Example Alerts - Group Name: "Example"  
<hr>  


### **example alerts**  
<hr>  

#### **Alert Name:** `folder_disk_usage`
+  *Message*: `"folder {{ $labels.path }} crossed disk usage limit on {{$labels.instance}}"` - [see example of this in pagerduty](https://example.pagerduty.com/incidents/ABC123FRED)

+  *Severity*: **page**
+  *Owners and contacts*:
   +  owner: [Example Team](https://example.pagerduty.com/teams/ABCDEF/users)   
   +  owner: [Example Data Team](https://example.pagerduty.com/teams/TGF123/users)   
   +  slack team: [#dd-proxy](https://example.slack.com/archives/CBA124)  
   +  slack on-call: [#proxy_on_call](https://example.slack.com/archives/J123F456)  
   +  slack data on-call: [#data_platform__on_call](https://example.slack.com/archives/99BALLOONS)  
   +  escalate: [escalation path in pagerduty](https://example.pagerduty.com/escalation_policies#JJKKRR123)  

   There is some multi team ownership here because of the service users of the folder/storage.
+  *Symptoms*:  
Root cause is the amount of disk space (in bytes) exceeds a threshold. We monitor these [folders](#folders) in particular for this alert:  
   +   /opt/proc/visits
   +   /opt/proc/kafka
   +   /tmp/kafka_uploader [see typical usage](https://grafana.example.net/goto/abcdef)
   <br>
<br>

      >  The monitoring is done via cron with a script that generates metrics for prometheus.

   See the [last 5 incidents](https://example.pagerduty.com/incidents/ABCDEF123456/past)

+  *Impact Assessment*: 
This can be very serious. Particuraly because Kafka is involved as both a [consumer](https://grafana.example.net/d/ABCDEF/kafka-reader?orgId=1%20target%3D) and producer.  You can get serious issues from any of these Example types:
   +   service1  
   +   service2
<br>   

   If you have checked most things and you suspect that the Kafka consumer is broken then escalate to the Example Data Platform team.

+  *Remediation Actions*:  
The specific action or steps to remediate this alert depend on various factors. But, most importantly it is related to whether we are alerting on [folders](#folders) or [partitions](#partitions). This alert is triggered on [folders](#folders).<p>See the [Disk Alert Actions](#disk-alert-actions) table below for what to do in most cases.  
+ *Runbook*:
   + [This page you are reading](https://bitbucket/devops/alerting/runbooks/roles/example/run_book.md)
   + [Example Team Cookbooks](https://confluence.internal.example.com/display/IN/CookBooks)  

+  *Useful Links*:
   + [Logs](https://example.applogger.com/#/query-new/logs?id=3Srxk1pyuFA&page=0)
   + [Dashboard](https://grafana.example.net/d/OzRfX7dGz/proxy-disk-usage?orgId=1)
   + [Mega Example Dashboard](https://grafana.example.net/d/11aMLgjGk/proxy-mega-dashboard)
   + [Example Team Knowledge Center](https://confluence.internal.example.com/display/IN/Example+team+knowledge+share)
   + [Data Team Knowledge Center](https://confluence.example.com/display/IN/Example+team+knowledge+share)
   + [Kafka](https://grafana.example.net/d/ABCDEF123/kafka-reader?orgId=1%20target%3D)
<br>

+  *Resolution*:  
Check that the Alert has gone back to the [ok state](https://prometheus-exaample.somewhere.net/graph?g0.range_input=2d&g0.expr=ALERTS%7Balertname%3D%22folder_disk_usage%22%2Calertstate%3D%22ok%22%7D&g0.tab=1)

<hr>

<hr>

## **`Disk Alert Actions`**  
The typical investigations to do depends on whether it was a folder or partition resource type, and what was the absolute path in question.  

Here are the typical investigative steps (either by logging onto a box or checking external logs etc) so...

Check if there are any files growing too fast, logs files too large or artifact files that got dumped (like core files). Use all the usual du/df type utilities.  <br>

[ `du -h --apparent-size`, &nbsp;`df -hk /var/log`, &nbsp; `du -sch /`, &nbsp;`lsof | grep deleted` ]

If you identify any suspects figures out who owns/created it (process).  Can you safely delete it if the process is still running. Do you still need to data so need to export it.
Check the various log files used by any of the Example family of processes. For example:

   + proc/fluentbit.log
   + proc.log
   + syslog
   + ...

Something to keep in mind is some files are sparse files so the real usage is a lot less that reported - see `/var/log/lastlog` for example. Are there too many logs for example in /var/log/incapsula? Check if the cause of the disk increase still an issue. Maybe a Kafka consumer is not running but the producer is? Is there any evidence of too many small files been written. Are we running out of file descriptors. In some of these cases we need to reach out to the Data Platform or NetOps teams.

Some other things to looks out for are code [dumps/crashes](https://confluence.internal.example.com/display/crash+monitor) or flaky network connections causing a backup of failed rebase efforts for revlite. Also the common config resources can take up space.  You can check this by looking at the git state of the directories - and the associated logs.

Additional detailed information can be found in the [Example Disk Monitor page](https://confluence.internal.example.com/display/Example+Disk+Monitor). Also good stuff in the [Example Team Knowledge Center](https://confluence.internal.example.com/display/Example+team+knowledge+share) and the [Example Data Team Knowledge Center](https://confluence.internal.example.com/display/Example+team+knowledge+share)

