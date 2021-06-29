# WechatpayEx

**微信支付APIV3 Elixir SDK**

## Installation

暂无，想用的把代码拷贝过去就行，就一个ex文件
目前我用的是一个GenServer实现，把Config都存在state里面，也可以考虑改写成一个巨大的macro，全凭喜好

## Usage

### 配置

```
config :myapp, :wechat_pay,
  appid: "your wechat appid",
  mchid: "your mchid",
  notify_url: "https://your_webservice_host/api/notify/wechat_pay",
  apiv3_key: "your apiv3 key",
  wx_pubs: [{"wx cert serial_no", File.read!('/path_to/wx_pub.pem')}], # 微信平台证书链
  client_serial_no: "client cert serial_no", # 商户证书序列号
  client_key: File.read!('/path_to/apiclient_key.pem'),  # 商户私钥
  client_cert: File.read!('/path_to/apiclient_cert.pem') # 商户证书(这个似乎没啥用)
```

### otp

```
defmodule Myapp.Application do
  # See https://hexdocs.pm/elixir/Application.html
  # for more information on OTP Applications
  @moduledoc false

  use Application

  def start(_type, _args) do
    children = [
      # {Worker, arg}

      # name想用啥用啥
      {WechatPay, Application.get_env(:myapp, :wechat_pay) ++ [name: :wechat_pay]}
    ]

    # See https://hexdocs.pm/elixir/Supervisor.html
    # for other strategies and supported options
    opts = [strategy: :one_for_one, name: Myapp.Supervisor]
    Supervisor.start_link(children, opts)
  end
end

```

## TODOLIST

- [x] 获取平台证书
- [x] 敏感信息解密
- [x] jspapi下单
- [x] native下单
- [x] 生成微信小程序支付表单
- [x] 返回包/回调包 验签
- [ ] H5支付
- [ ] APP支付
- [ ] 订单查询
- [ ] 发起退款
- [ ] 退款查询
- [ ] 其他(https://pay.weixin.qq.com/wiki/doc/apiv3/index.shtml)
