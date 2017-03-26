# Level 0 (optional) - Setup an Ubuntu VM
Vagrant Mac OS has been choosen to setup a fresh Ubuntu VM to avoid any potential OS or dependency issues.

* Install [VirtualBox](https://www.virtualbox.org/) before start as it is essential for runnig Vagrant.
* Downlaod and install [Vagrant](https://www.vagrantup.com/downloads.html) Mac OS version.
* Run the following command to start a fresh Ubuntu VM
```
$ vagrant init hashicorp/precise64
$ vagrant up
```
* Connect your Ubunutu VM using:
```
$ vagrant ssh
```
* And now you are in!

# Level 1 - Collecting your Data
* First you will need to sign up a new account on [datadoghq.com](http://datadoghq.com).
* Login use you newly created account then you will able to follow the guideline [here](https://app.datadoghq.com/account/settings#agent/ubuntu) to install Agent.
    * **(Bonus question)** What is the Agent? In short, an Agent is a DataDog programme to help clients to send their metrics from their host to DataDog, should you choose not to use API to send DataDog those metrics.
* After install the Agent, navigate to Datadog Agent configration file `/etc/dd-agent/datadog.conf`.
* Use your preferred text editor to add tags to your host:
    * Remember to use `sudo`, e.g. `sudo nano datadog.conf`
    * Find the line start with `# Set the host's tags (optional)`
    * Add the following tags:
    ```
    tags: this, is:an, interesting:challenge
    ```
    * Save your changes.
* Go back to Datadog UI and navigate to "infrastructure -> Host Map", you should be able to see the tags you have just added.
![tags](https://github.com/xpy32/hiring-engineers/blob/542b8f11e02331f904d9eb12a1858a13382eecd1/tags.png?raw=true)
* Now intall MySQL server on your VM:
```
$ sudo apt-get install mysql-server
```
* Install also on Datadog UI integration for MySQL by navigating to "Integrations" tab.
* Finally you are now ready to create your first check. Navigate to Datadog Agent folder `/etc/dd-agent`.
* Here you will be able to find `check.d/` and `conf.d/` for setup you first check:
    * Creat a python file under `check.d/` name it `test.py` to include the following code. By doing so, you have also declared a new metric `test.support.random`
    ```
    from checks import AgentCheck
    import random
    # print(random.random())
    class HelloCheck(AgentCheck):
        def check(self, instance):
            self.gauge('test.support.random', random.random())
    ```
    * Then go to `conf.d/` create a config file with same name as your python file but with yaml as extension `test.yaml` 
* Now you have created your first check `test` with a new metric `test.support.random`.

# Level 2 - Visualizing your Data
* To customize and visualizing your data, first clone your MySQL integration dashboard by going to "Dashboard" tab.
* Click MySQL dashboard and then you will arrive your MySQL integration dashboard.
* Click settings and clone the dashboard as shown on the below screenshot.
![Clone Dashboard](https://github.com/xpy32/hiring-engineers/blob/support-engineer/clone_dashboard.png?raw=true)
* Go to your cloned dashboard and add metric of your check from last step by adding a timeseries graph and choosing metric `test.support.random`.
* Customize this graph by editing it and add a new marker as shown on the screenshot.
![Add Marker](https://github.com/xpy32/hiring-engineers/blob/support-engineer/marker.jpg?raw=true)
* Now you have a graph with a red box at the top indicating when `test.support.random` metric goes up above 0.90
* You can take a snapshot by clicking the "Annotate this graph" button. And an email will be sent to the address you mentioned in the comment.
![Snapshot](https://github.com/xpy32/hiring-engineers/blob/542b8f11e02331f904d9eb12a1858a13382eecd1/snapshot_via_email.png?raw=true)
* **(Bonus question)** The differences between TimeBoard and a ScreenBoard are mainly:
    * TimeBoard always scoped to the same time while ScreenBoard does not.
    * ScreenBoard is more customizable. However TimeBoard is less customizable.
    * ScreenBoard can be shared as read only, but TimeBoard cannot.
* [Link to dashboard](https://app.datadoghq.com/dash/263112/test-cloned). And you can also find a screenshot of it here:
![Dashboard](https://github.com/xpy32/hiring-engineers/blob/542b8f11e02331f904d9eb12a1858a13382eecd1/dashboard_screenshot.png?raw=true)

# Level 3 - Alerting on your Data
* Automatic alert can be created to help to better montor the changes of your metrics.
* First go to "Monitors" tab on Datagod UI.
* Click new "Monitor" link on the top right and select Metric as monitor type with the following setup which will give you a multi-alert triggers when metric goes above 0.9 at least once during the last 5 minutes.
![Create monitor](https://github.com/xpy32/hiring-engineers/blob/support-engineer/monitor_create.png?raw=true)
* You may also want Datadog to send you an email whenever the alert is triggered. You can put your email content and choose who to notify as shown on the screenshot
![Monitor notification](https://github.com/xpy32/hiring-engineers/blob/support-engineer/monitor_send_notification.png?raw=true)
* After click save your alert should be up and running very soon.
* **(Bonus question)** you probably don't want to be alerted when you are out of the office. You can go to manage downtime tab to schedule a downtime for your alert.
![Create downtime](https://github.com/xpy32/hiring-engineers/blob/support-engineer/downtime.png?raw=true)
* Once the downtime is correctly setup. You will be able to receive an email to confirm it.
![Downtime Email](https://github.com/xpy32/hiring-engineers/blob/542b8f11e02331f904d9eb12a1858a13382eecd1/scheduled_downtime.png?raw=true)