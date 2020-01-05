## 电子合同

### 流程



### 电子合同签章

* 表

  contract_template_data

* 数据生成

```


ApprovalRequest::doApprove

//如果是电子合同，会搜集电子合同信息进行更新
ApprovableContractBase::afterApproved


//合同审批通过后，生成电子合同数据
// 合同数据 + 门店提现资料数据
//
StartStoreContractTemplateData::startStoreContractData()

Base->store()
```