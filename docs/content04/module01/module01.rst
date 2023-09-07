設定の保存 (バックアップ)
======================================

プライマリキーの設定
--------------------------------------

F5 rSeriesでは、設定データベースに含まれる機密性の高いパスワードやパスフレーズを暗号化/復号化するために、プライマリキーを使用します。
セキュリティを高めるために、プライマリキーは定期的にリセットすることを推奨します。
RMA時等、異なるデバイスに設定をリストアする場合、リストア対象のデバイスに同一のキーを設定する必要があります。

- 以下のcurlコマンドを実行して、rSeriesにプライマリキーを設定します。プライマリキーを新規に設定する際には、POSTメソッドを使用します。この例ではパスフレーズを"myprimarykey"、saltを"mysalt"として設定します。

.. code-block:: bash

   curl -sk -X PATCH -H "X-Auth-Token:$F5OS_TOKEN" -H "Content-Type:application/yang-data+json" https://$APPLIANCE_IP/api/data/openconfig-system:system/aaa/f5-primary-key:primary-key/f5-primary-key:set -d @- <<EOS
   {
       "f5-primary-key:passphrase": "myprimarykey",
       "f5-primary-key:confirm-passphrase": "myprimarykey",
       "f5-primary-key:salt": "mysalt",
       "f5-primary-key:confirm-salt": "mysalt"
   }
   EOS


バックアップファイルの作成
--------------------------------------

- 以下のcurlコマンドを実行して、F5OSの設定を保存 (バックアップ)します。設定のバックアップには、POSTメソッドを使用します。この例では、バックアップファイル名を"F5OS-BACKUP01"として設定します。

.. code-block:: bash

   $ curl -sk -X POST -H "X-Auth-Token:$F5OS_TOKEN" -H "Content-Type:application/yang-data+json" https://$APPLIANCE_IP/api/data/openconfig-system:system/f5-database:database/f5-database:config-backup -d @- <<EOS
   {
       "f5-database:name": "F5OS-BACKUP01"
   }
   EOS


- 上記コマンドの実行結果 (レスポンス)は、以下の通りです。
  
.. code-block:: bash

   {
     "f5-database:output": {
       "result": "Database backup successful. configs/F5OS-BACKUP01 is saved."
     }
   }


- 保存されたバックアップファイルは、ローカル端末等にダウンロードしておきます。