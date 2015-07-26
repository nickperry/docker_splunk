#

Splunk
----------------------------------------------------------------------
 
This is a Splunk server with proper process control.

http://www.splunk.com/en_us/products/splunk-enterprise.html

The default command for the container is /bin/splunk.sh. This wraps /opt/splunk/bin/splunk to start all Splunk processes and then gracefully shuts them down upon receiving a SIGTERM from a 'docker stop'.

Splunk is configured to run as user splunk.

#

Running
----------------------------------------------------------------------

Run with:

    $ docker run -d -p 8000:8000 -p 8089:8089 -p 7999:7999 nickperry/splunk

If you would like to accept syslog on port 514, add '-p 514:5514/udp -p 514:5514/tcp':

    $ docker run -d -p 8000:8000 -p 8089:8089 -p 7999:7999 -p 514:5514/udp -p 514:5514/tcp nickperry/splunk

Optionally map a volume to /opt/splunk/var to persist your indexed data. /data can be mapped as a path to load data into Splunk from files. Mounting /data from another container is a convenient way to access logs in that container in an ad-hoc fashion - taking Splunk to the data rather than taking the data to Splunk. 

#

Using
----------------------------------------------------------------------

If you've mapped the ports as suggested above you can now:

Access the Splunk web interface at https://your_docker_host:8000

Access the Splunk REST API at https://your_docker_host:8089

Send events to your_docker_host:7999 from a Splunk forwarder.

If you also mapped port 514 you can set up a UDP listener (and optionally a TCP listener) on 5514 and receive syslog events from other hosts:

https://your_docker_host:8000/en-US/manager/search/adddata/selectsource?input_type=udp&input_mode=1

Splunk can take a little while to shut down, so I highly recommend specifying a longer timeout to the docker stop command (docker stop -t 60), otherwise it is likely that docker's default 10s stop timeout will be breached and the splunk processes will get SIGKILLed.

Enjoy your container of Splunk!
