## What is this?
A load testing tool for Meteor applications.

This tool utilizes the [Grinder](http://grinder.sourceforge.net/) to manage agents which execute a customized test script capable of speaking DDP with the targe Meteor server.  Each agent simulates 1 or more client connections and records performance metrics.

Users can specify properties such as:
 * Meteor method calls to perform
 * Meteor subscriptions to initiate
 * Number of threads to start
 * Wait time between thread start
 * Process ramp-up (increment)


## Preliminaries
<b>Install pre-req's</b>
  * Java
  * [leiningen](https://github.com/technomancy/leiningen)

<b>Setup the project</b>

```bash
git clone git://github.com/alanning/meteor-load-test.git
cd meteor-load-test
lein deps  # downloads dependencies
```

## The System Under Test (SUT)
<b>To start the server</b>

```bash
cd sut
./run
```

<b>Open app in browser</b>

http://localhost:3000/


## The Grinder
<b>To start an agent</b>

```bash
# in meteor-load-test directory
bin/grinder agent start [optional host url of console - defaults to localhost]
```

<b>Monitor agent log file</b>

```bash
# in separate console window, from meteor-load-test directory
cd log
tail -f agent_1.log
```

<b>To start the console</b>

```bash
# in separate console window, from meteor-load-test directory
bin/grinder console start
```

<b>To run tests</b>

In the Script tab, set the root directory to $PROJECT_HOME/grinder

Select `working.properties` and set it as the properties file to use (the star button)

Open `working.properties` and adjust as appropriate.  Default setup will load test http://localhost:3000/

Click the play button in the top left (tooltip says, "Start the worker processes")

In the results tab, you should see test results


## How to use

Modify `working.properties` as appropriate.  
See the [Grinder documentation](http://grinder.sourceforge.net/g3/properties.html) for more options

Grinder agents should be started on separate boxes (not your webserver).

Note: Currently collection updates are received via Meteor subscriptions but there are no metrics gathered for how long it takes an update to be delivered under load.  The closest we have right now is peak and mean TPS for DDP calls.

Given that, probably the most realistic way to test responsiveness of your app under load is to spin up your agents, kick off the tests, wait for them to saturate your server, and then visit your site via your own browser.


## Future work

Create Chef or Pallet scripts to automate creation of Grinder agent instances in the cloud

Explore recording timing information for length of time to finish receiving all collection updates


## Acknowledgements

Based on [load-testing-with-clojure](https://github.com/locopati/load-testing-with-clojure) by Andy Kriger which load-tests stateless websites.

Uses the [java-ddp-client](https://github.com/kutrumbo/java-ddp-client) by Peter Kutrumbos to communicate with the target Meteor server.

Evolved from discussion on the meteor-talk google group: [Load Testing Meteor](https://groups.google.com/forum/#!topic/meteor-talk/M9waYvcFufs). In particular, major thanks to Andrew Wilcox, Sam Hatoum, Matt DeBergalis, and Tom Coleman for their guidance and suggestions.
