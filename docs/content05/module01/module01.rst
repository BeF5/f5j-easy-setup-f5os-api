QKViewの取得
======================================

- 以下のcurlコマンドを実行して、F5OSのQKViewファイルを取得します。QKViewの取得には、POSTメソッドを使用します。この例では、QKViewファイル名を"mytest-qkview.tgz"として設定します。

.. code-block:: bash

   $ curl -sk -X POST -H "X-Auth-Token:$F5OS_TOKEN" -H "Content-Type:application/yang-data+json" https://$APPLIANCE_IP/api/data/openconfig-system:system/f5-system-diagnostics-qkview:diagnostics/f5-system-diagnostics-qkview:qkview/f5-system-diagnostics-qkview:capture -d @- <<EOS
   {
       "f5-system-diagnostics-qkview:filename": "mytest-qkview.tgz"
   }
   EOS


- 上記コマンドの実行結果 (レスポンス)は、以下の通りです。

.. code-block:: bash

   {
     "f5-system-diagnostics-qkview:output": {
       "result": " Warning: Qkview may contain sensitive data such as secrets, passwords and core files. Handle with care. Please send this file to F5 support. \nQkview file mytest-qkview.tgz is being collected.\nreturn code 200\n ",
       "resultint": 0
     }
   }

- 以下のcurlコマンドを実行して、rSeriesアプライアンス上に保存されたQKViewファイルの一覧を取得します。

.. code-block:: bash

   $ curl -sk -H "X-Auth-Token:$F5OS_TOKEN" -H "Content-Type:application/yang-data+json" https://$APPLIANCE_IP/api/data/openconfig-system:system/f5-system-diagnostics-qkview:diagnostics/f5-system-diagnostics-qkview:qkview/f5-system-diagnostics-qkview:state/f5-system-diagnostics-qkview:files/f5-system-diagnostics-qkview:file


- 上記のコマンドの出力例は、以下の通りです。

.. code-block:: bash

   {
     "f5-system-diagnostics-qkview:file": [
       {
         "filename": "mytest-qkview.tgz",
         "size": 622282180,
         "created-on": "2023-08-21T17:55:40.185751329+09:00"
       }
     ]
   }



