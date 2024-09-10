# Setting up Opensearch Dashboards (and making sure all works)
By default, Opensearch Dashboards comes with no visualizations or index patterns. At this point, you can setup at least index patterns (think of it as the language that helps the web interface understand what the data are about such numbers and text).

- When you first login to the web interface you are likely prompted to add data. Instead click, "Explore on my own."
- In the left hand top area, click on the three dashes and then at the bottom scroll to "Dashboards Management"
- Then, click on "Index patterns."
- Click on "Create index pattern."
- In the index pattern name, type `logstash*` and click "Next step."
- In the time field, select `@timestamp` and click "Create index pattern."
- Depending on how many data were collected you should see a certain number of fields. These come from the data observed but given that some alerts may have additional metadata these fields may not appear in the index pattern. You should come back to this menu (after an hour or two, or even a day) and hit the refresh button on the top right to update the fields.

Now using the same three dashes in the left top area, you can navigate to "Discover" and see the data that are collected. In the top left, you should see the index (logstash*) where the data are ready from and you should also see the count of hits. These are the number json records created by Suricata (includes Netflow, Alerts, etc.)

If you see no data then it means that the network interface is not received data somehow. If you see some but not your network data, then it is a port mirroring issue.

