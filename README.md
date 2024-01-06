# Examples of Business Process Observability with GRAIL
---

This is a sample for educational purposes only and not officially part of the product.  
If you have question, leave a comment on the github repo.  

For more information about Dynatrace please visit [dynatrace.com](https://www.dynatrace.com).

---
## Upload Notebook
Upload the Notbook in your Dynatrace Saas tenant or in a [free trial tenant](https://www.dynatrace.com/trial) 
![Upload](https://github.com/dynatrace-ace-services/business-process-observability/blob/main/assets/upload_notebook.png?raw=true)

    https://raw.githubusercontent.com/dynatrace-ace-services/business-process-observability/main/notebook/Examples_of_Business_Process_Observability_with_GRAIL.json  

In this notebook, you will find : 

1- Business Process Delivery Chain Observability (simulation)

![dql1](https://github.com/dynatrace-ace-services/business-process-observability/blob/main/assets/dql_request1.png?raw=true)

 <details open>
  <summary>DQL request 1</summary>
```` data 
    record(timestamp=now()-45m, log.source="step1", content="order_ID=1095684, start_time=20231019160000986, status_step1=OK"),
    record(timestamp=now()-30m, log.source="step1", content="order_ID=1095685, start_time=20231019161501986, status_step1=OK"),
    record(timestamp=now()-15m, log.source="step1", content="order_ID=1095686, start_time=20231019163101986, status_step1=OK"),   
    record(timestamp=now()-45m, log.source="step2", content="order_ID=1095684, status_step2=OK"),
    record(timestamp=now()-30m, log.source="step2", content="order_ID=1095685, status_step2=OK"),
    record(timestamp=now()-15m, log.source="step2", content="order_ID=1095686, status_step2=OK"),
    record(timestamp=now()-45m, log.source="step3", content="order_ID=1095684, status_step3=OK"),
    record(timestamp=now()-30m, log.source="step3", content="order_ID=1095685, status_step3=OK"),
    record(timestamp=now()-15m, log.source="step3", content="order_ID=1095686, status_step3=warning"),
    record(timestamp=now()-45m, log.source="step4", content="order_ID=1095684, status_step4=OK"),
    record(timestamp=now()-30m, log.source="step4", content="order_ID=1095685, status_step4=OK"),
    record(timestamp=now()-15m, log.source="step4", content="order_ID=1095686, status_step4=failed"),
    record(timestamp=now()-45m, log.source="step5", content="order_ID=1095684, end_time=2023/10/20 16:01:30, status_step5=OK, "),
    record(timestamp=now()-30m, log.source="step5", content="order_ID=1095685, end_time=2023/10/21 16:31:30, status_step5=OK")
| parse content, """'order_ID=' WORD:order_ID 
      ((', start_time='TIMESTAMP('yyyyMMddHHmmssSSS'):start_time ', status_step1=' WORD:status_step1 ) |
      (', status_step2=' WORD:status_step2) |
      (', status_step3=' WORD:status_step3) |
      (', status_step4=' WORD:status_step4) |
      (', end_time='TIMESTAMP('yyyy/MM/d HH:mm:ss'):end_time ', status_step5=' WORD:status_step5))"""
| summarize{  startTime = takeFirst(start_time), endTime = takeFirst(end_time),
      status_step1 = takeFirst(status_step1),
      status_step2 = takeFirst(status_step2),
      status_step3 = takeFirst(status_step3),
      status_step4 = takeFirst(status_step4),
      status_step5 = takeFirst(status_step5)
      },  by: {order_ID}
```
</details>
    
2- DQL request 2 : Payment delay performance (simulation)
