F5OSのバージョン変更
======================================

- 以下のようなcurlコマンドを実行して、筐体のF5OSバージョンを任意のものに変更します。F5OSのアップグレード/ダウングレードには、POSTメソッドを使用します。この例では、F5OS 1.3.2 Buld 13054 (1.3.2-13054)をインストールします。
  
.. code-block:: bash

   $ curl -sk -X POST -H "X-Auth-Token:$F5OS_TOKEN" -H "Content-Type:application/yang-data+json" https://$APPLIANCE_IP/api/data/openconfig-system:system/f5-system-image:image/set-version -d @- <<EOS
   {
       "input": {
           "iso-version": "1.3.2-13054"
       }
   }
   EOS


- 上記コマンドの実行結果 (レスポンス)は、以下の通りです。
  
.. code-block:: bash

   {
     "f5-system-image:output": {
       "response": "System ISO version has been set"
     }
   }


- F5OS再起動後、:doc:`../../content03/module01/module01` の手順に従ってF5OSイメージを確認し、"Install"以下の部分が指定したF5OSバージョンになっていることを確認します。