## Splunk Components

Splunk has three main components, namely Forwarder, Indexer, and Search Head. These components are explained below:

![[Untitled 546.png|Untitled 546.png]]

### Splunk Forwarder

Splunk Forwarder is a lightweight agent installed on the endpoint intended to be monitored, and its main task is to collect the data and send it to the Splunk instance. It does not affect the endpoint's performance as it takes very few resources to process. Some of the key data sources are:

- Web server generating web traffic.
- Windows machine generating Windows Event Logs, PowerShell, and Sysmon data.
- Linux host generating host-centric logs.
- Database generating DB connection requests, responses, and errors.

![[Untitled 1 12.png|Untitled 1 12.png]]

### Splunk Indexer

Splunk Indexer plays the main role in processing the data it receives from forwarders. It takes the data, normalizes it into field-value pairs, determines the datatype of the data, and stores them as events. Processed data is easy to search and analyze.

![[Untitled 2 10.png|Untitled 2 10.png]]

### Splunk Search Head

Splunk Search Head is the place within the Search & Reporting App where users can search the indexed logs as shown below. When the user searches for a term or uses a Search language known as Splunk Search Processing Language, the request is sent to the indexer and the relevant events are returned in the form of field-value pairs.

![[Untitled 3 10.png|Untitled 3 10.png]]

Search Head also provides the ability to transform the results into presentable tables, visualizations like pie-chart, bar-chart and column-chart, as shown below:

![[Untitled 4 10.png|Untitled 4 10.png]]

  

---

## Navigating Splunk

### Splunk bar

When you access Splunk, you will see the default home screen identical to the screenshot bellow.

![[Untitled 5 8.png|Untitled 5 8.png]]

Looking at each section of the panel I can see  
  
The  
**TOP PANEL** which is the Splunk Bar

![[Untitled 6 7.png|Untitled 6 7.png]]

In the Splunk Bar, you can see system-level messages (Messages), configure the Splunk instance (Settings), review the progress of jobs (Activity), miscellaneous information such as tutorials (Help), and a search feature (Find).

  

You can change between Splunk installed apps using the Splunk bar instead of the Apps Panel

![[Untitled 7 7.png|Untitled 7 7.png]]

---

### Apps Panel

Next is the Apps Panel. In this panel, you can see the apps installed for the Splunk instance.

The default app for every Splunk instance is Search & Reporting.

![[Untitled 8 7.png|Untitled 8 7.png]]

---

### Explore Splunk

The next section is Explore Splunk. This panel contains quick links to add data to the Splunk instance, add new Splunk apps, and access the Splunk documentation.

![[Untitled 9 6.png|Untitled 9 6.png]]

---

### Splunk Dashboard

The last section is the Home Dashboard. By default, no dashbords are displayed. You can choose from a range of dashboards readily available within your Splunk instance. You can select a dashboard from the dropdown menu or visiting the dashboards listing page.

[![](https://assets.tryhackme.com/additional/splunk-overview/splunk-add-dashboard.gif)](https://assets.tryhackme.com/additional/splunk-overview/splunk-add-dashboard.gif)

You can also create dashboards and add them to the Home Dashboard. The dashboards you can create can be viewed isolated from the other dashboards by clicking on the Yours tab.

[Splunk documentation on Navigating Splunk](https://docs.splunk.com/Documentation/Splunk/8.1.2/SearchTutorial/NavigatingSplunk)

---

## Adding Data to Splunk

Splunk can ingest any data. As per the Splunk documentation, when data is added to Splunk, the data is processed and transformed into a series of individual events.

  

The data sources can be event logs, website logs, firewall logs, etc.

  

Data sources are grouped into categories. Below is a chart listing from the Splunk documentation detailing each data sourec category.

![[Untitled 10 5.png|Untitled 10 5.png]]

### THM ROOM

In this room, we are going to focus on VPN Logs. When we click on the **Add Data** link (from the Splunk home screen), we are presented with the following screen.

![[Untitled 11 5.png|Untitled 11 5.png]]

We will use the upload option to upload the data from your local machine.

As shown below, it has five steps to succesfully upload the data.

1. Select Source → Where we select the log source.
2. Select Source Type → Select what type of logs are being ingested.
3. Input Settings → Select the index where thes logs will be dumped and hostName to be associated with the logs.
4. Review → Review all the configurations.
5. Done → Final step, where the data is uploaded succesfully and ready to be analyzed.

[![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e8dd9a4a45e18443162feab/room-content/c36a6f1c70007602251f331aee914d5c.gif)](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e8dd9a4a45e18443162feab/room-content/c36a6f1c70007602251f331aee914d5c.gif)