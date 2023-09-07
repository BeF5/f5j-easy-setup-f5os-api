F5OSの初期化
======================================

- 以下のcurlコマンドを実行して、F5OSを初期化します。F5OSの初期化には、POSTメソッドを使用します。
  
.. code-block:: bash

   $ curl -sk -X POST -H "X-Auth-Token:$F5OS_TOKEN" -H "Content-Type:application/yang-data+json" https://$APPLIANCE_IP/api/data/openconfig-system:system/f5-database:database/f5-database:reset-to-default -d @- <<EOS
   {
     "f5-database:proceed": "yes"
   }
   EOS


- 上記コマンドの実行結果 (レスポンス)は、以下の通りです。

.. code-block:: bash

   {
     "f5-database:output": {
       "result": "Database reset-to-default successful."
     }
   }

- 初期化完了後、:doc:`../../content01/module02/module02` の手順に従って、adminユーザーの初期パスワードを変更します。
