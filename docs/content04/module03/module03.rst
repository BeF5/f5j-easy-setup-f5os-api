設定のリストア
======================================

- 以下のcurlコマンドを実行して、バックアップファイルからF5OSの設定をリストアします。設定のリストアには、POSTメソッドを使用します。この例では、"F5OS-BACKUP01"という名前のバックアップファイルから、リストアを行います。

.. code-block:: bash

   $ curl -sk -X POST -H "X-Auth-Token:$F5OS_TOKEN" -H "Content-Type:application/yang-data+json" https://$APPLIANCE_IP/api/data/openconfig-system:system/f5-database:database/f5-database:config-restore -d @- <<EOS
   {
       "f5-database:name": "F5OS-BACKUP01",
       "f5-database:proceed": "yes"
   }
   EOS


- 上記コマンドの実行結果 (レスポンス)は、以下の通りです。

.. code-block:: bash

   {
     "f5-database:output": {
       "result": "Database config-restore successful."
     }
   }


