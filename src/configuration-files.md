# ONLY FOR ANALYSIS OR CHANGING SETTINGS AFTER INSTALLATION!

# Configuration Files for POS Application

This document provides detailed explanations of the configuration files used by the POS application, specifically for RabbitMQ settings.

## 1. RabbitMQ Configuration Files

### 1.1 `jifs-prj.properties`
This file contains RabbitMQ user credentials for various queues.

# project specific use of rootnode RMQ user and password is possible, change following data in this case:

upstreamevent.rabbitmq.user = stduser
upstreamevent.rabbitmq.password = stduser

downstreamdata.rabbitmq.user = storeuser
downstreamdata.rabbitmq.password = CYO0yXjPVKNSxsACZ4Qb

downstreamcontrol.rabbitmq.user = storeuser
downstreamcontrol.rabbitmq.password = CYO0yXjPVKNSxsACZ4Qb

downstreamsoftware.rabbitmq.user = storeuser
downstreamsoftware.rabbitmq.password = CYO0yXjPVKNSxsACZ4Qb

### 1.2 `jifs-prj2_install.properties`

# This file specifies settings for consuming from various RabbitMQ queues, including enabling SSL and trusting all certificates.

# use ssl for rootnode consumer
# settings for consuming from downstream_data queue (updates, installation files, tableau files, initial data files, ...)
downstreamdata.rabbitmq.useTls = true
downstreamdata.rabbitmq.trustAll = true
# settings for event consuming from rootnode
downstreamcontrol.rabbitmq.useTls = true
downstreamcontrol.rabbitmq.trustAll = true
# settings for consuming from downstream_software queue (SOE files)
downstreamsoftware.rabbitmq.useTls = true
downstreamsoftware.rabbitmq.trustAll = true

### 1.3 `jifs-prj2_install.properties`

# This file contains configuration settings for consuming from different RabbitMQ queues and heartbeat endpoints.

##################################################################################
####        Configuration for ZAL TEST euroCONTROL 
####
##################################################################################
# change rootnode and eurocontrol system name with 
#
# zucchetti-eurocontrol-nlb.zstores-test.zalan.do
##################################################################################
# settings for consuming from downstream_data queue (updates, installation files, tableau files, initial data files, ...)
downstreamdata.rabbitmq.host = b-9f5a0fc2-6658-478b-9ab6-c6cc45d8dfc7.mq.eu-central-1.amazonaws.com
# settings for event consuming from rootnode
downstreamcontrol.rabbitmq.host = b-9f5a0fc2-6658-478b-9ab6-c6cc45d8dfc7.mq.eu-central-1.amazonaws.com
# settings for consuming from downstream_software queue (SOE files)
downstreamsoftware.rabbitmq.host = b-9f5a0fc2-6658-478b-9ab6-c6cc45d8dfc7.mq.eu-central-1.amazonaws.com
# heartbeat
crontab.heartbeat.arg.wsEndpoint = https://zucchetti-eurocontrol-nlb.zstores-test.zalan.do:8080/heartbeat-web/HeartBeatListener
cmd.heartbeat.arg.wsEndpoint.def = https://zucchetti-eurocontrol-nlb.zstores-test.zalan.do:8080/heartbeat-web/HeartBeatListener
java.naming.provider.url = http-remoting://zucchetti-eurocontrol-nlb.zstores-test.zalan.do:8080
# Online-Test für euroCONTROL
cmd.headOfficeOnlineCheck.arg.url.def = https://zucchetti-eurocontrol-nlb.zstores-test.zalan.do:8080/euroControl/Login

### 1.4 `rabbitmq.endnode.config`

# This file defines RabbitMQ shovel configurations for transferring messages between different brokers and queues.

[{rabbitmq_shovel,
   [{shovels,
      [{shovel_local,
       [{sources,      [{broker,"amqp://stduser:stduser@localhost/eurosuite"}]},
        {destinations, [{broker, "amqp://stduser:stduser@storenode/eurosuite"}]},
        {queue, <<"upstream_data_store_local">>},
        {ack_mode, on_confirm},
        {publish_properties, [{delivery_mode, 2}]},
        {publish_fields, [{exchange, <<"store_data">>}
                         ]},
       {reconnect_delay, 5}
       ]},
       {trx_shovel_root,
       [{sources,      [{broker,"amqp://stduser:stduser@localhost/eurosuite"}]},
        {destinations, [{broker, "amqps://storeuser:CYO0yXjPVKNSxsACZ4Qb@b-9f5a0fc2-6658-478b-9ab6-c6cc45d8dfc7.mq.eu-central-1.amazonaws.com:5671/eurosuite?verify=verify_none"}]},
        {queue, <<"upstream_data_root">>},
        {ack_mode, on_confirm},
        {publish_properties, [{delivery_mode, 2}]},
        {publish_fields, [{exchange, <<"root_data">>}
                          ]},
       {reconnect_delay, 5}
       ]},
       {event_shovel_root,
       [{sources,      [{broker,"amqp://stduser:stduser@localhost/eurosuite"}]},
        {destinations, [{broker, "amqps://storeuser:CYO0yXjPVKNSxsACZ4Qb@b-9f5a0fc2-6658-478b-9ab6-c6cc45d8dfc7.mq.eu-central-1.amazonaws.com:5671/eurosuite?verify=verify_none"}]},
        {queue, <<"upstream_event_root">>},
        {ack_mode, on_confirm},
        {publish_properties, [{delivery_mode, 2}]},
        {publish_fields, [{exchange, <<"root_event">>}
                          ]},
       {reconnect_delay, 5}
       ]},
       {event_shovel_local,
       [{sources,      [{broker,"amqp://stduser:stduser@localhost/eurosuite"}]},
        {destinations, [{broker, "amqp://stduser:stduser@storenode/eurosuite"}]},
        {queue, <<"upstream_event_store_local">>},
        {ack_mode, on_confirm},
        {publish_properties, [{delivery_mode, 2}]},
        {publish_fields, [{exchange, <<"store_event">>}
                          ]},
       {reconnect_delay, 5}
       ]}
     ]
    }]
 }].