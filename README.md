README for JMXCoquette :: Version 0.8 :: December 2016
======================================================

WHAT IT BE
==========
- JMXCoquette is a New Relic Java Agent extension that provides easy access to MBeans and attribute values by reporting them as custom event data to New Relic Insights.

HOW TO DEPLOY
==========
- Copy the coquette_n.n.jar file to your Java Agent's ./newrelic/extensions/ directory. Ensure the directory and extension have the same owner as all other New Relic agent files.
- Edit the newrelic.yml file found in your Agent's root directory and include a coquette section like below. Be sure to get the spacing right, 2 spaces before "coquette" and four spaces before each property. 
- Place the coquette section directly after the proxy section in newrelic.yml (You can place it anywhere, but I place it after proxy config).

          See readme in src directory for more details - too much funky formatting in this page. 
        
HOW TO CONFIGURE
================
- There are a few options you can configure to change the behavior of JMXCoquette, see below. 
- The service is aware of changes to the newrelic.yml file, so changes will manifest a few minutes after newrelic.yml has been updated, this will depend on the frequency you have set for polling.

        Options
        =======
        - enabled: Either true or false, disables or enables the harvest of mbeans accordingly.
        - mode: One of the following options  (default is "strict").
                - strict: The mbeans listed in the coquette mbean_N section and their attributes are polled.
                - open: All the attributes of the mbeans listed in the coquette mbean_N section are polled. There is no need to define specific attribute values for the mbeans listed.
                - promiscuous: All accessible MBeans and Attributes are polled.
                - disco: Details of all MBeans are written to the newrelic agent logfile with "INFO" severity. This is a good way to discover the mbeans and attributes you wish to poll.   
        - event_name: Determines the custom event name you wish to store the JMX data within Insights (default is JMX).
        - frequency: The number of minutes between runs of the JMX harvest (default and lowest granularity is 1 minute). The service does not have a sleep function it simply skips the number of polling intervals defined herein, 
        - mbean_N: There can be multiple "mbean" options defined in the coquette section, this forms the list of defined MBean polled in strict or open mode.
                - The mbean property is defined in 2 parts, the name of the mbean followed by attributes you wish to poll, comma separated and enclosed in square brackets.
                - e.g. mbean_0: com.mypackage:type=ImportantData [Attribute0,Attribute1,Attribute2].
                - if you wish to use the open mode you must include square brackets within the definition line.

HOW TO USE
==========
- Once the extension is deployed and configured you should start seeing events in the custom event table you defined for Insights. Go about creating dashboards using this new data source. 
- To configure the data elements you may want to override the default formatting New Relic Insights is asserting on the values. To do this select the "Formatter" tab in the "Manage Data" section of your Insights account.

KNOWN LIMITATIONS
=================
- Only reports Attributes from MBeans that return Objects of type: java.lang.Number, java.lang.String, or java.lang.Boolean. Provide feedback to the author if you would like other datatype processing options.
- Only reports MBeans registered to the default platform MBean server.
- Only polls Attributes not Operations.

TODOS
=====
- Provide an ability to alias MBean names.
- Process string values to escape special characters properly.
- Curate a list of useful MBean definitions for popular platforms and frameworks.

CHANGES PER RELEASE
===================
v0.8
- The MBean definition in the coquette section of the newrelic.yml can include wildcards per the MBean lookup options for https://docs.oracle.com/javase/7/docs/api/javax/management/MBeanServer.html#queryMBeans(javax.management.ObjectName,%20javax.management.QueryExp) - this relies on the ObjectName supporting wildcards per constraints defined by https://docs.oracle.com/javase/7/docs/api/javax/management/ObjectName.html. This would then support an MBean name definition of "&#42;:type=BasicDataSource,connectionpool=connections [NumActive,MaxTotal,MinIdle,MaxIdle,NumIdle]" which would match any MBean domain. The problem is that the yml file format does not like to have attribute values start with "&#42;:" so we have to escape such definitions with a character, in our case '\'. 

v0.7
- Display the real name of the MBean Object rather than the query string used to look up the MBean - that was confusing.

v0.6
- Compiled to support the Java 1.6 standard (1.6.0_45)

FEEDBACK
========
- gil@newrelic.com
