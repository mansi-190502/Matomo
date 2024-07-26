## Configuring Loki data source
You have setup everything now it time to link loki to Grafana monitoring tool. Use the given URL to open Grafana login page –
```
http://localhost:3000
```

### Use username and password as admin.
Once logged in successfully you will be able to see the Grafana dashboard.

![image](https://github.com/user-attachments/assets/d6c7f144-ca2f-4b51-8b6f-25de81509c48)

Go to Configuration> Data sources> Add Data Source now find and click on Loki.

Add the following settings to this page –

Name – Loki

URL – http://localhost:3100

As you can see in the image below.

![image](https://github.com/user-attachments/assets/dbdfc3a7-0a0a-476f-bebf-80d2c38aae60)

Scroll down and click on Save & test.

Visualizing logs in Grafana Loki
To visualize logs in Grafana Loki go to Exaplore and then select Loki at the data source. In the log browser enter a Loki query for example to view varlogs enter {job=”varlogs”}.

Now you can scroll and view logs displayed in the dashboard.

![image](https://github.com/user-attachments/assets/1d053b50-4bcb-4f09-832a-4f69ed3da050)

### Conclusion
I hope you have setup Grafana Loki successfully on your Ubuntu system. Now if you have a query then write us in the comments below.
