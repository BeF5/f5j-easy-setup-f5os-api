BIG-IPテナントの状態変更
======================================

F5OS上のテナントには以下の3種類の状態があり、F5OSから変更することができます。

.. csv-table:: :header: "状態","説明"

   "Configured","テナント設定のみシステム上に存在"
   "Provisioned","ソフトウェアをインストールし、仮想ディスクを作成"
   "Deployed","リソース (CPU/メモリ)を割り当て、テナントを起動"


- 以下のcurlコマンドを実行して、作成したテナント"test-tenant01"の状態を"Configured"に変更します。テナントの状態設定には、PATCHメソッドを使用します。

.. code-block:: bash

   $ curl -sk -X PATCH -H "X-Auth-Token:$F5OS_TOKEN" -H "Content-Type:application/yang-data+json" https://$APPLIANCE_IP/api/data/f5-tenants:tenants/tenant=test-tenant01/config/running-state -d @- <<EOS
   {
       "running-state": "configured"
   }
   EOS


- 以下のcurlコマンドを実行して、テナント"test-tenant01"の状態が"Configured"に変更されているかを確認します。
  
.. code-block:: bash

   $ curl -sk -H "X-Auth-Token:$F5OS_TOKEN" -H "Content-Type:application/yang-data+json" https://$APPLIANCE_IP/api/data/f5-tenants:tenants/tenant=test-tenant01/config/running-state


- 上記コマンドの実行結果 (レスポンス)は、以下の通りです。
  
.. code-block:: bash

   {
     "f5-tenants:running-state": "configured"
   }

å