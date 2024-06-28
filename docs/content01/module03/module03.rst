APIアクセストークンの取得
======================================

- 最初に、変数"F5OS_PASSWORD"および"APPLIANCE_IP"を設定します。環境変数として設定する場合には、以下のようなコマンドを実行してください。(<>内の部分は、ご利用の環境に合わせて修正してください。)

.. code-block:: bash

   $ export F5OS_PASSWORD=<F5OSのadminユーザーパスワード>
   $ export APPLIANCE_IP=<rSeriesのManagement IPアドレス>



- 以下のようなコマンドを実行し、Basic認証でAPIアクセストークンを取得します。レスポンス・ヘッダ内に含まれる"X-Auth-Token"の値を取得し、以降のAPIリクエストに利用します。

.. code-block:: bash

   $ curl -sk -u admin:$F5OS_PASSWORD -I https://$APPLIANCE_IP/api/data/openconfig-system:system/aaa


.. note::
   F5OSでは、ポート8888およびポート443でAPIコールを受け付けることができます (いずれもhttps)。ポート443を利用する場合、URIの最初の部分は"/api"となります。ポート8888を利用する場合、URIの最初の部分は"/restconf"となります。本資料では、ポート443で"/api"を用いる場合について、ご紹介していきます。


- 上記のコマンドの出力例は、以下の通りです。

.. code-block:: bash

   HTTP/1.1 200 OK
   Date: Thu, 17 Aug 2023 15:37:35 GMT
   Server: Apache
   Strict-Transport-Security: max-age=63072000; includeSubdomains;
   Cache-Control: private, no-cache, must-revalidate, proxy-revalidate
   Content-Type: application/yang-data+xml
   Pragma: no-cache
   X-Auth-Token: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJTZXNzaW9uIElEIjoiYWRtaW4xNjkyMjg2NjU1IiwiYXV0aGluZm8iOiJhZG1pbiAxMDAwIDkwMDAgXC90bXAiLCJidWZmZXJ0aW1lbGltaXQiOiIzMDAiLCJleHAiOjE2OTIyODc1NTUsImlhdCI6MTY5MjI4NjY1NSwicmVuZXdsaW1pdCI6IjUiLCJ1c2VyaW5mbyI6ImFkbWluIDE3Mi4xOC4yMDIuMTk3In0.WfIWKmKf3ykk3Uorlmurx_YmeialOZbnxzoZDawixy8
   Content-Security-Policy: default-src 'self'; block-all-mixed-content; base-uri 'self'; frame-ancestors 'none';
   Strict-Transport-Security: max-age=15552000; includeSubDomains
   X-Content-Type-Options: nosniff
   X-Frame-Options: DENY
   X-XSS-Protection: 1; mode=block
   Content-Security-Policy: default-src 'self'; upgrade-insecure-requests; frame-ancestors 'none'; script-src  'self'; style-src 'self' 'unsafe-inline'; object-src 'none'; base-uri 'self'; connect-src 'self'; font-src 'self'; frame-src 'self'; img-src 'self' data:; manifest-src 'self'; media-src 'self'; worker-src 'none';


- 以下のコマンドを実行することで、レスポンス・ヘッダ内のX-Auth-Tokenの値を変数"F5OS_TOKEN"に代入できます。

.. code-block:: bash

   $ F5OS_TOKEN=`curl -sk -u admin:$F5OS_PASSWORD -I https://$APPLIANCE_IP/api/data/openconfig-system:system/aaa | grep X-Auth-Token | awk '{print $2}'`


- 以下のコマンドを実行して、変数"F5OS_TOKEN"に値が格納されていることを確認します。

.. code-block:: bash

   $ echo $F5OS_TOKEN


