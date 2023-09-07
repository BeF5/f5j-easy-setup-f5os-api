F5OS初期パスワード変更
======================================

F5OSのadminユーザーの初期パスワードは"admin"になっています。API経由でアクセスするには、初期パスワードを変更する必要があります。以下のようなcurlコマンドを実行して、adminユーザーの初期パスワードを変更します。(<>内の部分は、ご利用の環境に合わせて修正してください。)


.. code-block:: bash

   $ curl -sk -u admin:admin -H "Content-Type:application/yang-data+json" https://$APPLIANCE_IP/api/operations/openconfig-system:system/aaa/authentication/users/user=admin/config/change-password -d @- <<EOS
   {
       "input": [
           {
               "old-password": "admin",
               "new-password": "<F5OSのadminユーザーパスワード>",
               "confirm-password": "<F5OSのadminユーザーパスワード>"
           }
       ]
   }
   EOS


