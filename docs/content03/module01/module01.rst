F5OSのバージョン確認
======================================

- rSeries筐体にF5OSのイメージをアップロード後、以下のcurlコマンドを実行して、イメージの状態を確認します。

.. code-block:: bash

   $ curl -sk -H "X-Auth-Token:$F5OS_TOKEN" -H "Content-Type:application/yang-data+json" https://$APPLIANCE_IP/api/data/openconfig-system:system/f5-system-image:image/state


- 上記のコマンドの出力例は、以下の通りです。

.. code-block:: bash

   {
     "f5-system-image:state": {
       "os": {
         "os": [
           {
             "version-os": "1.3.2-13054",
             "status": "ready",
             "date": "2023-01-26",
             "size": "922.16MB",
             "in-use": false,
             "type": ""
           },
           {
             "version-os": "1.5.1-12283",
             "status": "ready",
             "date": "2023-08-14",
             "size": "926.15MB",
             "in-use": true,
             "type": ""
           }
         ]
       },
       "services": {
         "service": [
           {
             "version-service": "1.3.2-13054",
             "status": "ready",
             "date": "2023-01-26",
             "size": "0.38GB",
             "in-use": false,
             "type": ""
           },
           {
             "version-service": "1.3.0-8327",
             "status": "ready",
             "date": "2023-01-26",
             "size": "2.15GB",
             "in-use": false,
             "type": ""
           },
           {
             "version-service": "1.5.1-12283",
             "status": "ready",
             "date": "2023-08-14",
             "size": "2.05GB",
             "in-use": true,
             "type": ""
           },
           {
             "version-service": "1.5.0-5781",
             "status": "ready",
             "date": "2023-08-14",
             "size": "2.49GB",
             "in-use": false,
             "type": ""
           }
         ]
       },
       "iso": {
         "iso": [
           {
             "version-iso": "1.3.2-13054",
             "status": "ready",
             "date": "2023-01-26",
             "size": "4.04GB",
             "in-use": false,
             "type": ""
           },
           {
             "version-iso": "1.5.1-12283",
             "status": "ready",
             "date": "2023-08-14",
             "size": "6.05GB",
             "in-use": false,
             "type": ""
           }
         ]
       },
       "install": {
         "install-os-version": "1.5.1-12283",
         "install-service-version": "1.5.1-12283",
         "install-status": "none"
       }
     }
   }


