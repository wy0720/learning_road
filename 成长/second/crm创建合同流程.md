* 创建门店详细信息

  1. Request URL: 

     http://shop-data-mapi-dev.16pinpin.com/approval-request/create?category=contract_shop&include_draft=1&approvable_id=19013&pending=1&is_new_domain=1

* 审核门店

  1. Request URL: 

     http://shop-data-mapi-dev.16pinpin.com/approval-request/33516/approve?is_new_domain=1







* 门店绑定

  1. Request URL: 

     https://shop-data-mapi-dev.16pinpin.com/contract-shop/bind-contract?contract_id=14392&include_draft=1&extra_params=79424&is_change_form=1&is_new_domain=1

     ```
     {"values":{"contract_shop_id":19013,"shop_logo":[],"login_tel":"12345678911","summary":"一句话","goods_picture":[{"url":"https://img.16pinpin.com/Frn4ZKLc2wE3p_SuBk-dtfqaqUCw","value":""}],"goods_price_picture":[{"url":"https://img.16pinpin.com/Frn4ZKLc2wE3p_SuBk-dtfqaqUCw","value":""}],"shop_main_logo":[{"url":"https://img.16pinpin.com/Frn4ZKLc2wE3p_SuBk-dtfqaqUCw","value":""}],"shop_environment_picture":[{"url":"https://img.16pinpin.com/Frn4ZKLc2wE3p_SuBk-dtfqaqUCw","value":""}],"id":26592,"shop_enterprise_id":26592,"group_id":"14399","business_licence":"","enterprise_name":"","bank_card_img":[{"url":"https://img.16pinpin.com/Frn4ZKLc2wE3p_SuBk-dtfqaqUCw","value":""}],"identity_card_img":[{"url":"https://img.16pinpin.com/Frn4ZKLc2wE3p_SuBk-dtfqaqUCw","value":""}],"handheld_identity_card_img":[]}}
     ```

* 添加折扣

  * 预览折扣

    1. Request URL: 

       https://shop-data-mapi-dev.16pinpin.com/approval-request/preview?category=single_shop_vip_rule&contract_id=14392&include_draft=1&extra_params=79424&is_change_form=1&pending=1&is_new_domain=1

  * 提交审核

    1. Request URL: 

       https://shop-data-mapi-dev.16pinpin.com/approval-request/create?category=single_shop_vip_rule&contract_id=14392&include_draft=1&extra_params=79424&is_change_form=1&pending=1&is_new_domain=1&is_new_domain=1

       ```
       {"values":{"id":"","contract_id":"14392","contract_shop_ids":[19013],"expire_date_range":["2019-11-15","2019-12-15"],"discountExplainInfo":{"time_type":"day","rule":{"time":["00:00-23:59"]},"active_time_type":"same_with_shop_time"},"discount_type":"black","ladder_discount":{"discountType":"discount","discount":70},"user_discount":78,"discount_limit":"","highest_discount_type":"none","highest_discount_data":0,"discount_explain":"","discount_file":[]}}
       ```








### 数据流

* 创建商户页面
  * account
  * contract【状态为draft】

* account【创建商户页面创建商户】
* shop_group【新建合同页面，添加合同信息】

* 新建合同页面
  * shop_enterprise
  * 生成contract_base的审批
  * 绑定门店页面【contract表attr会写入门店信息】
  * contract_shop【更新：绑定shop_group和提现资料】
* 合同审批页面
  * contract_template_data
  * 审批通过后【contract：status=approved】
* 电子合同签章
  * 从contract_template_data查询数据展示列表
  * elec_contract数据生成