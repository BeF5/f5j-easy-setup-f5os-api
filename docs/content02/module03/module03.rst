BIG-IPテナントの削除
======================================

- 以下のcurlコマンドを実行して、作成したテナント"test-tenant01"を削除します。テナントの削除には、DELETEメソッドを使用します。

.. code-block:: bash

   $ curl -sk -X DELETE -H "X-Auth-Token:$F5OS_TOKEN" -H "Content-Type:application/yang-data+json" https://$APPLIANCE_IP/api/data/f5-tenants:tenants/tenant=test-tenant01


- 以下のcurlコマンドを実行して、BIG-IPテナントを確認します。上記で削除したテナントが、存在していないことを確認します。

.. code-block:: bash

   $ curl -sk -H "X-Auth-Token:$F5OS_TOKEN" -H "Content-Type:application/yang-data+json" https://$APPLIANCE_IP/api/data/f5-tenants:tenants


