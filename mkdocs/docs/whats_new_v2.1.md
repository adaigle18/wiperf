Title: What's New in version 2.1
Authors: Nigel Bowden

# What's New in version 2.1
<div style="float: right;">
![wiperf_logo](images/wiperf_logo.png)
</div>
Version 2.1 of the wiperf probe code introduces a number of bug-fixes and features. These are outlined below:

## New Features

### SMB Tests
Thanks to the kind contribution of [Mario Gingras](https://twitter.com/gingmar){target=_blank}, wiperf now supports performance testing of SMB/CIFS file transfers.

Up to 5 tests may be defined to be run in each wiperf poll cycle. Each test instance will mount a remote volume and then transfer a file from the remote server to the wiperf probe. The transfer rate and time are then reported to the management platform for reporting.

A new dashboard is also provided to display the SMB data in report format.

If using the SMB tests, note that there are a small number of additional Linux packages that you may need to install to provide the SMB connectivity. Please check out the link provided below for more details.   

Further information: [SMB Tests Reference Document](reference_doc_smb.md)

### Librespeed Speedtest Support
An idea for Librespeed speedtest support by wiperf was submitted by Oscar Leal. This has now been added, meaning that wiperf can now run a speedtest to either Ookla or Librespeed for performance testing.

In addition to testing against public Librespeed servers,it is also possible to test against your own instance of the Librespeed server, allowing testing within your own environment, without hitting the Internet (and associated bottlenecks)

If using the Librespeed speedtest, note that there an additional Linux package that you will need to install to enable the test to work. Please check out the link provided below for more details.

Further information: [Librespeed Reference Document](reference_doc_librespeed.md)

### Results Spooling
Tris Kipling is a keen supporter of the wiperf project and, based on is experience of using wiperf probes, suggested that some mechanism be created to allow a degree of "survivability" for instances when communications between the probe and management platform are down.

To address this request, a spooling mechanism for results data is now provided. Previously, if the probe was unable to communicate with the management server, no tests would be attempted during the current poll cycle. With the new spooling feature, tests will still be performed by the probe and results data will be locally stored. The data will be forwarded by the probe when communications with the management platform is re-established (and any local data spool files removed).

This feature is enabled by default so required no additional configuration. It may be disabled if required.

### Results Data Caching
Due to a couple of requests for local storage of results data, local results caching has been implemented. 

When this is enabled, all results data is stored in either CSV or JSON format in local storage on the wiperf probe. The data files are stored in the local directory `/var/cache/wiperf` for a configurable period of time (3 days by default).

This feature allows for other methods of data inspection or retrieval that users may choose to perform. It is disabled by default.

(Note: This is a completely separate feature from the data spooling feature mentioned previously))

Further information: [Results Caching Reference Document](reference_doc_caching.md)

### Error Reporting
Diagnosing issues with probe tests may be difficult without remote access to a probe for further investigation. To assist with looking at issues, error messages generated by the probe during its test cycle are now available for reporting back to the management platform.

This feature is enabled by default, but may be disabled if there are concerns about volumes of management traffic from the probe. There is also a configuration setting to allow he number of error messages per poll cycle to be capped (5 by default).

A new "Probe Health" dashboard has bene provided to allow viewing of error messages.

## Release Notes

1.  Added fix for bad read of remote config file when downloaded (thanks Ben Roeder)
2.  Fixed bad config variable names for InfluxDB2 in config.py (thanks Konstantin - issue #1)
3.  Added SMB tests provided by Mario Gingras
4.  Improved route injection for tests when required interface not selected by default routing
5.  Fixed issue with incorrect data types in Influx DB for ping tests (some numeric values were created
    as strings, preventing calculations on their values.)

    This fixed isue will cause failures for ping test results on existing data. To correct the issue,
    existing ping test data in the Influx DB must be dropped using the following command in the InfluxDB
    CLI utility:

    sudo influx -username admin -password letmein
    use wiperf
    drop series from "wiperf-ping"

6.  Added support for librespeed to speedtest in addition to existing Ookla support. Also
    made more data points available to reports including latency, jitter & data volume for
    upload/download
7.  Added support for cached data : data may now be cached locally for retrieval and inspection
    by user-defined means (e.g. SSH to probe & inspect data). Data may be stored in VSC or JSON
    format.
8.  Added result spooling support. If connectivity between the probe and reporting platform is
    lost, results may be spooled to a local directory and automatically forwarded when connectivity
    is re-established.
9.  Added support for forwarding of probe error messages to reporting platform to allow diagnosis
    of test related issues on the probe.
10. Fixed DHCP test errors on RPi reported/fixed by Ben Roeder (Issue #39)
11. Fixed re-read of config file from github reported/fixed by Ben Roeder (PR #7)
12. Added new dashboard for SMB tests
13. Updated summary dashboard to support new SMB tests
14. Updated speedtest dashboard to support librespeed and additional data points
15. Added dashboard for probe health, which include probe poll error messages
16. Miscellaneous minor dashboard improvements/fixes



