BIG-IPテナントのデプロイ
======================================


- 以下のようなcurlコマンドを実行して、rSeriesアプライアンス上にBIG-IP (TMOS)テナントを作成します。テナントの作成には、POSTメソッドを使用します。この例で作成するテナントの構成は、以下の通りです。
  
.. csv-table:: :header: "設定項目","値"

   "テナント名","test-tenant01"
   "テナント種別","BIG-IP"
   "利用するテナントイメージ","BIGIP-17.1.0.2-0.0.2.ALL-F5OS.qcow2.zip.bundle"
   "BIG-IPテナントの管理IPアドレス","172.28.15.216"
   "デフォルトゲートウェイ","172.28.15.254"
   "サブネット長 (Prefix-length)","23"
   "テナントに割り当てるVLAN", "103および104"
   "テナントに割り当てる仮想CPU数","4"
   "テナントに割り当てるメモリ (MB)","14848"
   "テナントに割り当てるディスク容量 (GB)","82"
   "テナントの状態","Deployed"
   "アプライアンス・モード","無効"
 

.. code-block:: bash

   $ curl -sk -X POST -H "X-Auth-Token:$F5OS_TOKEN" -H "Content-Type:application/yang-data+json" https://$APPLIANCE_IP/api/data/f5-tenants:tenants -d @- <<EOS
   {
       "tenant": [
           {
               "name": "test-tenant01",
               "config": {
                   "type": "BIG-IP",
                   "image": "BIGIP-17.1.0.2-0.0.2.ALL-F5OS.qcow2.zip.bundle",
                   "nodes": [
                       1
                   ],
                   "mgmt-ip": "172.28.14.216",
                   "gateway": "172.28.15.254",
                   "prefix-length": 23,
                   "vlans": [
                       "103",
                       "104"
                   ],
                   "vcpu-cores-per-node": 4,
                   "memory": 14848,
                   "storage": {
                       "size": 82
                   },
                   "cryptos": "enabled",
                   "running-state": "deployed",
                   "appliance-mode": {
                       "enabled": false
                   }
               }
           }
       ]
   }
   EOS


- 以下のコマンドを実行して、F5OSで稼働するテナントを確認します。

.. code-block:: bash

   $ curl -sk -H "X-Auth-Token:$F5OS_TOKEN" -H "Content-Type:application/yang-data+json" https://$APPLIANCE_IP/api/data/f5-tenants:tenants


- 上記のコマンドの出力例は、以下の通りです。

.. code-block:: bash

   {
     "f5-tenants:tenants": {
       "tenant": [
         {
           "name": "test-tenant01",
           "config": {
             "name": "test-tenant01",
             "type": "BIG-IP",
             "image": "BIGIP-17.1.0.2-0.0.2.ALL-F5OS.qcow2.zip.bundle",
             "nodes": [1],
             "mgmt-ip": "172.28.15.216",
             "prefix-length": 23,
             "dag-ipv6-prefix-length": 128,
             "gateway": "172.28.15.254",
             "vlans": [103, 104],
             "cryptos": "enabled",
             "vcpu-cores-per-node": 4,
             "memory": "14848",
             "storage": {
               "size": 82
             },
             "running-state": "deployed",
             "mac-data": {
               "f5-tenant-l2-inline:mac-block-size": "one"
             },
             "appliance-mode": {
               "enabled": false
             }
           },
           "state": {
             "name": "test-tenant01",
             "unit-key-hash": "CvXv44ROa3LiMjsI4k0mmxiaOZa4rk5iN97edJD2lWYOu0tLgxcBpLC7z9Ubpw4jgaG+D0Xp+hoe6Ffv9HNnXQ==",
             "type": "BIG-IP",
             "image": "BIGIP-17.1.0.2-0.0.2.ALL-F5OS.qcow2.zip.bundle",
             "mgmt-ip": "172.28.15.216",
             "prefix-length": 23,
             "dag-ipv6-prefix-length": 128,
             "gateway": "172.28.15.254",
             "vlans": [103, 104],
             "cryptos": "enabled",
             "vcpu-cores-per-node": 4,
             "memory": "14848",
             "storage": {
               "size": 82
             },
             "running-state": "deployed",
             "mac-data": {
               "base-mac": "14:a9:d0:1a:82:14",
               "mac-pool-size": 1,
               "f5-tenant-l2-inline:mac-block": [
                 {
                   "mac": "14:a9:d0:1a:82:14"
                 }
               ]
             },
             "appliance-mode": {
               "enabled": false
             },
             "cpu-allocations": {
               "cpu-allocation": [
                 {
                   "node": 1
                 }
               ]
             },
             "feature-flags": {
               "stats-stream-capable": true
             },
             "status": "Running",
             "primary-slot": 1,
             "image-version": "BIG-IP 17.1.0.2 0.0.2",
             "instances": {
               "instance": [
                 {
                   "node": 1,
                   "pod-name": "test-tenant01-1",
                   "instance-id": 1,
                   "phase": "Running",
                   "creation-time": "2023-08-21T07:09:32Z",
                   "ready-time": "2023-08-21T07:10:00Z",
                   "status": "Started tenant instance",
                   "mgmt-mac": "14:a9:d0:1a:82:15"
                 }
               ]
             }
           }
         }
       ]
     }
   }

