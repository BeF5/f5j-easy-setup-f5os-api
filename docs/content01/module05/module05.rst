rSeries ネットワーク設定
======================================

VLAN設定
--------------------------------------

- 以下のようなcurlコマンドを実行して、rSeriesアプライアンス上でVLANを設定します。VLANの設定には、PATCHメソッドを使用します。この例では、VLAN ID "103" (VLAN名 "vlan103")とVLAN ID "104" (VLAN名 "vlan104")の2つのVLANを作成します。

.. code-block:: bash

   $ curl -sk -X PATCH -H "X-Auth-Token:$F5OS_TOKEN" -H "Content-Type:application/yang-data+json" https://$APPLIANCE_IP/api/data -d @- <<EOS
   {
       "openconfig-vlan:vlans": {
           "vlan": [
               {
                   "vlan-id": "103",
                   "config": {
                       "vlan-id": 103,
                       "name": "vlan103"
                   }
               },
               {
                 "vlan-id": "104",
                   "config": {
                       "vlan-id": 104,
                       "name": "vlan104"
                   }
               }
           ]
       }
   }
   EOS


- 以下のコマンドを実行して、VLANが適切に設定されているかを確認します。

.. code-block:: bash

   $ curl -sk -H "X-Auth-Token:$F5OS_TOKEN" -H "Content-Type:application/yang-data+json" https://$APPLIANCE_IP/api/data/openconfig-vlan:vlans


- 上記コマンドの実行結果 (レスポンス)は、以下の通りです。

.. code-block:: bash

   {
     "openconfig-vlan:vlans": {
       "vlan": [
         {
           "vlan-id": 103,
           "config": {
             "vlan-id": 103,
             "name": "vlan103"
           }
         },
         {
           "vlan-id": 104,
           "config": {
             "vlan-id": 104,
             "name": "vlan104"
           }
         }
       ]
     }
   }


Interface設定
--------------------------------------

- 以下のようなcurlコマンドを実行して、rSeriesアプライアンスの物理インタフェースにVLANを割り当てます。インタフェースの設定には、PATCHメソッドを使用します。この例では、上記で作成したVLAN 103と104を、Trunk VLAN (Tagged)としてインタフェース3.0に割り当てます。

.. code-block:: bash

   $ curl -sk -X PATCH -H "X-Auth-Token:$F5OS_TOKEN" -H "Content-Type:application/yang-data+json" https://$APPLIANCE_IP/api/data/openconfig-interfaces:interfaces -d @- <<EOS
   {
       "openconfig-interfaces:interfaces": {
           "interface": [
               {
                   "name": "3.0",
                   "openconfig-if-ethernet:ethernet": {
                       "openconfig-vlan:switched-vlan": {
                           "config": {
                               "trunk-vlans": [
                                   103,
                                   104
                               ]
                           }
                       }
                   }
               }
           ]
       }
   }
   EOS

- 以下のコマンドを実行して、インタフェース3.0の設定を確認します。

.. code-block:: bash

   $ curl -sk -H "X-Auth-Token:$F5OS_TOKEN" -H "Content-Type:application/yang-data+json" https://$APPLIANCE_IP/api/data/openconfig-interfaces:interfaces | jq ".[].interface[2]"


- 上記で設定したtrunk-vlans (103および104)が含まれていることを確認します。
  
Link Aggregation (LAG) 設定
--------------------------------------

- 以下のようなcurlコマンドを実行して、LAGインタフェースを作成し、物理インタフェースを割り当てます。インタフェースの設定には、PATCHメソッドを使用します。この例では、"LAG-Data-1"というLAGインタフェースを作成し、インタフェース3.0および4.0を割り当てます。
  
.. code-block:: bash

   $ curl -sk -X PATCH -H "X-Auth-Token:$F5OS_TOKEN" -H "Content-Type:application/yang-data+json" https://$APPLIANCE_IP/api/data -d @- <<EOS
   {
       "openconfig-interfaces:interfaces": {
           "interface": [
               {
                   "name": "LAG-Data-1",
                   "config": {
                       "name": "LAG-Data-1",
                       "type": "iana-if-type:ieee8023adLag",
                       "enabled": true
                   },
                   "openconfig-if-aggregate:aggregation": {
                      "config": {
                           "lag-type": "LACP",
                           "f5-if-aggregate:distribution-hash": "src-dst-ipport"
                       },
                       "openconfig-vlan:switched-vlan": {
                           "config": {
                               "trunk-vlans": [
                                   103,
                                   104
                               ]
                           }
                       }
                   }
               },
               {
                   "name": "3.0",
                   "config": {
                       "name": "3.0"
                   },
                   "openconfig-if-ethernet:ethernet": {
                       "config": {
                           "openconfig-if-aggregate:aggregate-id": "LAG-Data-1"
                       }
                   }
               },
               {
                   "name": "4.0",
                   "config": {
                       "name": "4.0"
                   },
                   "openconfig-if-ethernet:ethernet": {
                       "config": {
                           "openconfig-if-aggregate:aggregate-id": "LAG-Data-1"
                       }
                   }
               }
           ]
       }
   }
   EOS


- 以下のようなcurlコマンドを実行して、上記で作成したLAGインタフェースにLACP (Link Aggregation Control Protocol)の設定を行います。LACPの設定には、PATCHメソッドを使用します。この例では、LACP Intervalを"FAST"、Modeを"Active"として設定します。
  
.. code-block:: bash

   $ curl -sk -X PATCH -H "X-Auth-Token:$F5OS_TOKEN" -H "Content-Type:application/yang-data+json" https://$APPLIANCE_IP/api/data -d @- <<EOS
   {
       "ietf-restconf:data": {
           "openconfig-lacp:lacp": {
               "interfaces": {
                   "interface": [
                       {
                           "name": "LAG-Data-1",
                           "config": {
                               "name": "LAG-Data-1",
                               "interval": "FAST",
                               "lacp-mode": "ACTIVE"
                           }
                       }
                   ]
               }
           }
       }
   }
   EOS


- 以下のコマンドを実行して、LAGおよびLACPが適切に設定されているかを確認します。

.. code-block:: bash

   $ curl -sk -H "X-Auth-Token:$F5OS_TOKEN" -H "Content-Type:application/yang-data+json" https://$APPLIANCE_IP/api/data/openconfig-lacp:lacp


- 上記のコマンドの出力例は、以下の通りです。

.. code-block:: bash

   {
     "openconfig-lacp:lacp": {
       "config": {
         "system-priority": 32768
       },
       "state": {
         "f5-lacp:system-id-mac": "14:a9:d0:1a:82:13"
       },
       "interfaces": {
         "interface": [
           {
             "name": "LAG-Data-1",
             "config": {
               "name": "LAG-Data-1",
               "interval": "FAST",
               "lacp-mode": "ACTIVE"
             },
             "state": {
               "name": "LAG-Data-1",
               "interval": "FAST",
               "lacp-mode": "ACTIVE",
               "system-id-mac": "14:a9:d0:1a:82:13"
             },
             "members": {
               "member": [
                 {
                   "interface": "3.0",
                   "state": {
                     "interface": "3.0",
                     "activity": "ACTIVE",
                     "timeout": "SHORT",
                     "synchronization": "OUT_SYNC",
                     "aggregatable": true,
                     "collecting": false,
                     "distributing": false,
                     "system-id": "14:a9:d0:1a:82:13",
                     "oper-key": 3,
                     "partner-id": "00:00:00:00:00:00",
                     "partner-key": 0,
                     "port-num": 3072,
                     "partner-port-num": 0,
                     "counters": {
                       "lacp-in-pkts": "0",
                       "lacp-out-pkts": "370",
                       "lacp-rx-errors": "0"
                     }
                   }
                 },
                 {
                   "interface": "4.0",
                   "state": {
                     "interface": "4.0",
                     "activity": "ACTIVE",
                     "timeout": "SHORT",
                     "synchronization": "OUT_SYNC",
                     "aggregatable": true,
                     "collecting": false,
                     "distributing": false,
                     "system-id": "14:a9:d0:1a:82:13",
                     "oper-key": 3,
                     "partner-id": "00:00:00:00:00:00",
                     "partner-key": 0,
                     "port-num": 4096,
                     "partner-port-num": 0,
                     "counters": {
                       "lacp-in-pkts": "0",
                       "lacp-out-pkts": "0",
                       "lacp-rx-errors": "0"
                     }
                   }
                 }
               ]
             }
           }
         ]
       }
     }
   }


