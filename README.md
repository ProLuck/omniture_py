Omniture_Py
===========
A Python Library To Access Omniture's SiteCatalyst Reporting API
----------------------------------------------------------------------------

-------------------------------
This is a modified fork from https://github.com/RobGoretsky/omniture_py
I only included the ranked, realtime report and clean the code a litle bit.
All credits for Rob Goretsky.
------------------------------

This library simplifies the process of accessing the Omniture SiteCatalyst Reporting API by wrapping the REST API calls in a Python library.

First, get access configured by reading through documentation at [Omniture Developer Connection](http://developer.omniture.com/).  Then, to test authentication/access:  

```python
from omniture import Omniture  
om = Omniture('username:company','shared_secret')     
json_object =  om.request('Company.GetReportSuites', '')  
for suite in json_object["report_suites"]: 
    print "Report Suite ID: %s\nSite Title: %s\n" % (suite["rsid"], suite["site_title"])  
```

To get total number of page views yesterday, just run the following.  It runs an <b>Overtime</b> report in SiteCatalyst, with <b>pageViews</b> as the metric, and returns the numeric total result.  

```python
print om.get_count_from_report('report_suite_name', 'pageViews') 
```

To get number of page views for "Homepage" and "Media Guide" combined, yesterday, run the following.  It runs a <b>Trended</b> report in SiteCatalyst, using <b>pageViews</b> as the metric, and 'page' as the Element, and then selecting for pages titled either "Homepage" or "Media Guide":  

```python
print om.get_count_from_report('report_suite_name', 'pageViews', 'page', ["Homepage","Media Guide"])
```

To get a Report.QueueRanked run the following. If you want pass more than one element you have to pass a list with a dict for every element like this [{"id": "evar1"}, {"product"}].

```python
data = om.ranked_report('report_suite_name', 'visits', element='browser')
```

The get_count_from_report function currently supports requesting one dimension (called an 'element') in addition to the date, which is always implied to be included.  It also only currently supports requesting one metric at a time.  If you wish to have lower-level access to the Omniture API than this, you can just use the run_omtr_queue_and_wait_request, and pass it the type of request and full request body.  This will return a Python object.

```python
obj = om.request_and_wait('Report.QueueTrended', {"reportDescription" : reportDescription})
```
	
Full documentation on each of the parameters available to these methods can be found within the code.
