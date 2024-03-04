# PROJECTNAME

## Overview

Our security tools have alerted us to a malicious actor that is brute forcing accounts for the website. We need you to investigate, find out where the attack is coming from, if they were successful, and if they were, what did they do with their access. This is an extremely time-sensitive investigation, every second is potentially more time the attacker has control over systems on the server. As a starting point, we know the administrator URL is http:// imreallynotbatman. com/joomla/administrator/index.php and that usernames and passwords will be submitted in an HTTP POST request.



### Skills Learned

- Navigating Splunk
- Learn how security analysts analyze and investigate security events within Splunk.

### Tools Used

- Splunk
  
## Steps
### Looking for traffic that represents attempted logins to that url we can come up with this query. Index:* sourcetype:="stream:http" http_method=POST uri="/joomla/administrator/index.php"

![image](https://github.com/CristianFernandez123/Splunk-Lab/assets/161634608/be741506-60bb-493a-9f7c-d2806bddd51e)

### Looking under the src_ip value in interesting fields we can see ip address 23[.]22.63.114 makes up 97% of the traffic.
 
 
![image](https://github.com/CristianFernandez123/Splunk-Lab/assets/161634608/48522abd-3a81-4116-87d3-21b69fcfcea1)

### We can add that IP to our query by left-clicking it.

![image](https://github.com/CristianFernandez123/Splunk-Lab/assets/161634608/4e7f8943-bacb-48aa-a488-a85fdd3f6aec)

### we can add | spath timestamp | search timestamp="2016-08-10T21:46:44.453730Z" to single out one event for analysis.
### scrolling down to the event details we can see the username and password submitted to the web server. username=admin password=baby

![image](https://github.com/CristianFernandez123/Splunk-Lab/assets/161634608/cdf725ae-990b-4c8c-9259-f4c3e6340a1f)

### By editing out query to index="botsv1" sourcetype="stream:http" http_method=POST uri="/joomla/administrator/index.php" src_ip="23.22.63.114" | table timestamp,form_data . Using this query, we can sort out the timestamp column enabling us to view the first password in the brute force attack.

![image](https://github.com/CristianFernandez123/Splunk-Lab/assets/161634608/e830e138-c1ea-4e19-abc2-f2b9d095b1f7)




