## 1. API Description

Domain name: monitor.api.qcloud.com


This API (GetMonitorData) is used to query the monitoring data of CPM. The values for input parameters are as follows:
namespace: qce/cpm
dimensions.0.name=instanceId
dimensions.0.value (the instance IP of CPM)

| Name         | Description                                     |
| ---------- | ---------------------------------------- |
| instanceId | The instance ID of CPM, which can be obtained by querying CPM API [DescribeDevice](/document/product/386/6728) |

## 2. Input Parameters

The following request parameter list only provides API request parameters. Common request parameters are also needed when the API is called. For more information, please see <a href="/doc/api/405/公共请求参数" title="Common Request Parameters"> Common Request Parameters</a> page. The Action field for this API is GetMonitorData.

### 2.1 Input Parameters

| Parameter Name               | Required   | Type       | Input Content          | Description                                       |
| ------------------ | ---- | -------- | ------------- | ---------------------------------------- |
| namespace          | Yes    | String   | qce/cpm       | Namespace. Every Tencent Cloud product has a namespace. For more information, please see Input Content column.           |
| metricName         | Yes    | String   | Specific metric name       | Metric name. For more information, please see 2.2                            |
| dimensions.0.name  | Yes    | String   | instanceId    | The input parameter is the instance ID of CPM                            |
| dimensions.0.value | Yes    | String   | The instance ID of CPM | Enter a specific instanceId                           |
| period             | No    | Int      | 60/300        | Collecting duration for monitoring data. Most metrics support a statistical granularity of 60s while some metrics only support a statistical granularity of 300s. Statistical granularity varies with metrics. Enter parameters by referring to the list of metric details in section 2.2. |
| startTime          | No    | Datetime | Start time          | Start time, such as "2016-01-01 10:25:00". Default is "00:00:00" of the current day |
| endTime            | No    | Datetime | End time          | End time. It is the current time by default. endTime cannot be earlier than startTime       |

### 2.2 Metric Name
> For now, the statistical period (the parameter "period") for CPM monitoring dimension is 300 seconds. Data collection with a finer granularity is not supported.

| Metric Name             | Description                 | Unit      | Dimension         |
| ---------------- | ------------------ | ------- | ---------- |
| cpu_usage        | CPU utilization             | %       | instanceId |
| cpu_load_avg     | CPU average load            | Variable. No unit  | instanceId |
| mem_use          | MEM usage           | MByte   | instanceId |
| mem_proc_use     | Application memory usage            | MByte   | instanceId |
| mem_virtual_use  | Virtual memory usage            | MByte   | instanceId |
| mem_usage        | Memory utilization              | %       | instanceId |
| io_read_traffic  | Disk IO read traffic            | KByte/s | instanceId |
| io_write_traffic | Disk IO write traffic            | KByte/s | instanceId |
| io_await         | Disk IO wait time           | ms      | instanceId |
| io_util          | Disk IO CPU utilization        | %       | instanceId |
| io_service_time  | Disk IO service time          | ms      | instanceId |
| lan_outtraffic   | Private network ENI outbound bandwidth (taking the maximum value among multiple ENIs) | Mbps    | instanceId |
| lan_intraffic   | Private network ENI inbound bandwidth (taking the maximum value among multiple ENIs) | Mbps    | instanceId |
| lan_outpkg       | Private network ENI outbound packets (taking the maximum value among multiple ENIs) | pck/sec     | instanceId |
| lan_inpkg        | Private network ENI inbound packets (taking the maximum value among multiple ENIs) | pck/sec     | instanceId |
| wan_outtraffic   | Public network outbound bandwidth              | Mbps    | instanceId |
| wan_intraffic    | Public network inbound bandwidth              | Mbps    | instanceId |
| wan_outpkg       | Public network outbound packets              | pck/sec     | instanceId |
| wan_inpkg        | Public network inbound packets              | pck/sec     | instanceId |
| wan_outflux      | Public network outbound traffic              | GByte   | instanceId |


## 3. Output Parameters

| Parameter Name       | Type       | Description                  |
| ---------- | -------- | ------------------- |
| code       | Int      | Error code. 0: Successful; other values: Failed |
| message    | String   | Error message                |
| startTime  | Datetime | Start time                |
| endTime    | Datetime | End time                |
| metricName | String   | Metric name                |
| period     | Int      | Interval for collecting monitoring data              |
| dataPoints | Array    | Monitoring data list              |


## 4. Error Codes

| Error Code | Error Description    | Error Message                                 |
| ---- | ------- | ------------------------------------ |
| -502 | Resource does not exist   | OperationDenied.SourceNotExists      |
| -503 | Incorrect request parameter  | InvalidParameter                     |
| -505 | Parameter is missing    | InvalidParameter.MissingParameter    |
| -507 | Limit is exceeded    | OperationDenied.ExceedLimit          |
| -509 | Incorrect dimension group | InvalidParameter.DimensionGroupError |
| -513 | DB operation failed  | InternalError.DBoperationFail        |

## 5. Example

Input

<pre>
https://monitor.api.qcloud.com/v2/index.php?
&<a href="/doc/api/405/公共请求参数" title="Common Request Parameters">Common Request Parameters</a>
&namespace=qce/cpm
&metricName=cpu_usage
&dimensions.0.name=instanceId
&dimensions.0.value=cpm-xxxx
&startTime=2016-06-28 14:10:00
&endTime=2016-06-28 14:20:00
&period=300
</pre>

Output

```
{
    "code": 0,
    "message": "",
    "metricName": "inpkg",
    "startTime": "2016-06-28 14:10:00",
    "endTime": "2016-06-28 14:20:00",
    "period": 300,
    "dataPoints": [
        30,
        35,
        40
    ]
}
```
