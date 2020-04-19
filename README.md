Asterisk app_amd for FreeSWITCH
===============================

This is an implementation of Asterisk's answering machine detection (voice
activity detection) for FreeSWITCH.

Building
--------

To build this module, all you need to do is type `make`, but because it relies
on `pkg-config` and FreeSWITCH, you need to point `pkg-config` to where
FreeSWITCH is installed before building:

```
$ export PKG_CONFIG_PATH=/usr/local/freeswitch/lib/pkgconfig/
$ make
```

Sample Configuration
--------------------
Just put a file like this in your freeswitch installation, in **conf/autoload_configs/amd.conf.xml**
```xml
<configuration name="amd.conf" description="mod_amd Configuration">
  <settings>
    <param name="silence_threshold" value="256"/>
    <param name="maximum_word_length" value="5000"/>
    <param name="maximum_number_of_words" value="3"/>
    <param name="between_words_silence" value="50"/>
    <param name="min_word_length" value="100"/>
    <param name="total_analysis_time" value="5000"/>
    <param name="after_greeting_silence" value="800"/>
    <param name="greeting" value="1500"/>
    <param name="initial_silence" value="2500"/>
  </settings>
</configuration>
```
Dialplan Example
--------------------
```xml
<extension name="outbound">
   <condition field="destination_number" expression="^(.+)$">
      <action application="set" data="continue_on_fail=false"/>
      <action application="set" data="hangup_after_bridge=true"/>
      <action application="set" data="sip_invite_domain=$${local_ip_v4}"/>
      <action application="set" data="effective_caller_id_number=XXXXXXXXX"/>
      <action application="set" data="execute_on_answer=amd"/>
      <action application="bridge" data="sofia/external/$1@172.16.134.140:5080"/>
      <action application="hangup"/>
   </condition>
</extension>
```
