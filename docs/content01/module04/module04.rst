rSeries アプライアンス初期設定
======================================

ライセンス確認
--------------------------------------

- 以下のコマンドを実行して、rSeriesアプライアンスに適用されているライセンス情報を確認します。このコマンドを実行すると、Registration Keyを含むライセンス情報を確認できます。

.. code-block:: bash

   $ curl -sk -H "X-Auth-Token:$F5OS_TOKEN" -H "Content-Type:application/yang-data+json" https://$APPLIANCE_IP/api/data/openconfig-system:system/f5-system-licensing:licensing


ホスト名の設定
--------------------------------------

- 以下のようなcurlコマンドを実行して、rSeriesアプライアンスのホスト名を設定します。ホスト名の設定には、PATCHメソッドを使用します。この例では、ホスト名を"Appliance01"として設定します。

.. code-block:: bash

   $ curl -sk -X PATCH -H "X-Auth-Token:$F5OS_TOKEN" -H "Content-Type:application/yang-data+json" https://$APPLIANCE_IP/api/data/ -d @- <<EOS
   {
       "openconfig-system:system": {
           "config": {
               "hostname": "Appliance01"
           }
       }
   }
   EOS


- 以下のコマンドを実行して、ホスト名が適切に設定されているかを確認します。

.. code-block:: bash

   $ curl -sk -H "X-Auth-Token:$F5OS_TOKEN" -H "Content-Type:application/yang-data+json" https://$APPLIANCE_IP/api/data/openconfig-system:system/config


- 上記コマンドの実行結果 (レスポンス)は、以下の通りです。

.. code-block:: bash

   {
      "openconfig-system:config": {
        "hostname": "Appliance01"
      }
   }


DNSサーバー設定
--------------------------------------

- 以下のようなcurlコマンドを実行して、rSeriesアプライアンスが参照するDNSサーバーを設定します。DNSサーバーの設定には、PATCHメソッドを使用します。この例では、参照するDNSサーバーを"8.8.8.8"として設定します。

.. code-block:: bash

   $ curl -sk -X PATCH -H "X-Auth-Token:$F5OS_TOKEN" -H "Content-Type:application/yang-data+json" https://$APPLIANCE_IP/api/data/openconfig-system:system/dns -d @- <<EOS
   {
       "openconfig-system:dns": {
           "servers": {
               "server": [
                   {
                       "address": "8.8.8.8",
                       "config": {
                           "address": "8.8.8.8",
                           "port": 53
                        }
                    }
                ]
            }
        }
   }
   EOS


- 以下のコマンドを実行して、DNSサーバーが適切に設定されているかを確認します。

.. code-block:: bash

   $ curl -sk -H "X-Auth-Token:$F5OS_TOKEN" -H "Content-Type:application/yang-data+json" https://$APPLIANCE_IP/api/data/openconfig-system:system/dns


- 上記コマンドの実行結果 (レスポンス)は、以下の通りです。

.. code-block:: bash

   {
     "openconfig-system:dns": {
       "servers": {
         "server": [
           {
             "address": "8.8.8.8",
             "config": {
               "address": "8.8.8.8",
               "port": 53
             },
             "state": {
               "port": 53
             }
           }
         ]
       }
     }
   }


時刻設定 (Time ZoneおよびNTPサーバー)
--------------------------------------

- 以下のようなcurlコマンドを実行して、rSeriesアプライアンスのタイムゾーン、および参照するNTPサーバーを設定します。時刻設定には、PATCHメソッドを使用します。この例ではTime Zoneを"Asia/Tokyo"、NTPサーバーを"ntp.nict.jp"として設定します。

.. code-block:: bash

   $ curl -sk -X PATCH -H "X-Auth-Token:$F5OS_TOKEN" -H "Content-Type:application/yang-data+json" https://$APPLIANCE_IP/api/data -d @- <<EOS
   {
        "openconfig-system:system": {
            "clock": {
                "config": {
                    "timezone-name": "Asia/Tokyo"
                }
            },
            "ntp": {
                "config": {
                    "enabled": "true"
                },
                "servers": {
                    "server": [
                        {
                            "address": "ntp.nict.jp",
                            "config": {
                                "address": "ntp.nict.jp"
                            }
                        }
                    ]
                }
            }
        }
   }
   EOS


- 以下のコマンドを実行して、NTPサーバーが適切に設定されているかを確認します。

.. code-block:: bash

   $ curl -sk -H "X-Auth-Token:$F5OS_TOKEN" -H "Content-Type:application/yang-data+json" https://$APPLIANCE_IP/api/data/openconfig-system:system/ntp


- 上記コマンドの実行結果 (レスポンス)は、以下の通りです。

.. code-block:: bash

   {
     "openconfig-system:ntp": {
       "config": {
         "enabled": true,
         "enable-ntp-auth": false
       },
       "state": {
         "enabled": true,
         "enable-ntp-auth": false
       },
       "servers": {
         "server": [
           {
             "address": "ntp.nict.jp",
             "config": {
               "address": "ntp.nict.jp",
               "port": 123,
               "version": 4,
               "association-type": "SERVER",
               "iburst": false,
               "prefer": false
             },
             "state": {
               "address": "ntp.nict.jp",
               "port": 123,
               "version": 4,
               "association-type": "SERVER",
               "iburst": false,
               "prefer": false,
               "f5-openconfig-system-ntp:authenticated": false
             }
           }
         ]
       }
     }
   }


リモートログサーバー設定
--------------------------------------

- 以下のようなcurlコマンドを実行して、F5OSのログを転送するリモートログサーバー (Syslogサーバー)を設定します。ログサーバーの設定には、PATCHメソッドを使用します。この例では、ログサーバーを"10.10.10.10"として設定します。

.. code-block:: bash

   $ curl -sk -X PATCH -H "X-Auth-Token:$F5OS_TOKEN" -H "Content-Type:application/yang-data+json" https://$APPLIANCE_IP/api/data -d @- <<EOS
   {
       "openconfig-system:system": {
           "logging": {
               "remote-servers": {
                   "remote-server": [
                       {
                           "host": "10.10.10.10",
                           "config": {
                               "host": "10.10.10.10",
                               "remote-port": "514"
                           },
                           "selectors": {
                               "selector": [
                                   {
                                       "facility": "LOCAL0",
                                       "severity": "INFORMATIONAL",
                                       "config": {
                                           "facility": "LOCAL0",
                                           "severity": "INFORMATIONAL"
                                       }
                                   }
                               ]
                           }
                       }
                   ]
               }
           }
       }
   }
   EOS

- 以下のコマンドを実行して、ログサーバーが適切に設定されているかを確認します。

.. code-block:: bash

   $ curl -sk -H "X-Auth-Token:$F5OS_TOKEN" -H "Content-Type:application/yang-data+json" https://$APPLIANCE_IP/api/data/openconfig-system:system/logging/remote-servers


- 上記コマンドの実行結果 (レスポンス)は、以下の通りです。

.. code-block:: bash

   {
     "openconfig-system:remote-servers": {
       "remote-server": [
         {
           "host": "10.10.10.10",
           "config": {
             "host": "10.10.10.10",
             "remote-port": 514,
             "f5-openconfig-system-logging:proto": "udp"
           },
           "selectors": {
             "selector": [
               {
                 "facility": "f5-system-logging-types:LOCAL0",
                 "severity": "INFORMATIONAL",
                 "config": {
                   "facility": "f5-system-logging-types:LOCAL0",
                   "severity": "INFORMATIONAL"
                 }
               }
             ]
           }
         }
       ]
     }
   }


許可リスト (Allow List) 設定
--------------------------------------

- 以下のようなcurlコマンドを実行して、F5OSのOut-of-band管理を許可するIPアドレス、およびポート番号を設定します。Allow Listの設定には、POSTメソッドを使用します。この例では、"10.255.0.0/24"からのSNMP (ポート161)通信を許可するAllow Listを設定します。

.. code-block:: bash

   $ curl -sk -X POST -H "X-Auth-Token:$F5OS_TOKEN" -H "Content-Type:application/yang-data+json" https://$APPLIANCE_IP/api/data/openconfig-system:system/f5-allowed-ips:allowed-ips -d @- <<EOS
   {
       "allowed-ip": [
           {
               "name": "allow-snmp",
               "config": {
                   "ipv4": {
                       "address": "10.255.0.0",
                       "prefix-length": 24,
                       "port": 161
                   }
               }
           }
       ]
   }
   EOS


- 以下のコマンドを実行して、Allow Listが適切に設定されているかを確認します。
  
.. code-block:: bash

   $ curl -sk -H "X-Auth-Token:$F5OS_TOKEN" -H "Content-Type:application/yang-data+json" https://$APPLIANCE_IP/api/data/openconfig-system:system/f5-allowed-ips:allowed-ips


- 上記コマンドの実行結果 (レスポンス)は、以下の通りです。

.. code-block:: bash

   {
     "f5-allowed-ips:allowed-ips": {
       "allowed-ip": [
         {
           "name": "allow-snmp",
           "config": {
             "ipv4": {
               "address": "10.255.0.0",
               "prefix-length": 24,
               "port": 161
             }
           }
         }
       ]
     }
   }

