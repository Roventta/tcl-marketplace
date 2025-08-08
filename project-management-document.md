# the Project Management Document for the Private Online Marketplace Website of © Tofu Cat Litter， the MVP.

# © Tofu Cat Litter公司项目管理文件——申请注册制的在线宠物用品购物网站, 最简可行产品阶段


## 项目概览

一个申请制的在线购物网站。用户需要通过在网站上提交个人商业信息并获得管理员批准以注册其网站帐号。网站拥有最基本的在线购物功能：浏览产品，买家下达订单，网站管理员提示已发货。当以上功能全部实现时，添加产品月度订阅功能。

网站前后段技术栈为react或next.js或clojure，前段state management为pinia，数据库为postgreSQL与PrismaORM， 在线交易api使用stripe。

### Stakeholders

1. © Tofu Cat Litter 公司股东
1. © Tofu Cat Litter 网站管理员
1. breeders, 在澳洲育种贩卖宠物的人
1. influencers, 带货网红

### 最基本的申请制用户系统
1. 维持一个网站用户账户信息数据库，数据库容纳所有网站账户的id（unique int）， password (char40), role（foreign key），ABN （char20）。数据库需要维持两种帐号的role： user 和 manager。
1. 每次成功登录后，在用户的cookie中存储JWT。 JWT包含用户role。
1. 服务器端检查每次user request的JWT。当受到任意有过期的JWT的request时，服务器跳转回登录界面。
1. 维持一个网站用户的账户注册申请表数据库，数据库容纳所有网站用户为了申请账户而提交的个人商业信息： first name, given name，ABN, application message（char1000）。申请表的primary key应该是表被数据库收到的时间。
1. 每个ABN只能注册一个账户。当一个注册申请使用的ABN在账户数据库或申请表数据库存在时，拒绝此http request。
1. 网站有一个给manager处理账户注册申请表的前端页面。页面能够按时间顺序显示所有申请表。网站管理员能够阅读每个申请表信息。网站管理员能够选择接受或拒绝某个申请表。此页面只能
1. 网站管理员接受申请表时，用户账户信息数据库根据申请表信息创造一条新账户。网站管理员拒绝申请表时，此条由数据库中删除。

### 最基本的购物功能包括
1. 维持一个产品信息数据库，数据库容纳所有公司可出售的产品信息，如产品id（int），产品名称（char20），产品介绍（char500），以及有无库存（bool）。 不包括产品库存（int）。
1. 产品信息数据库暂时不需要增删该查的前端控制面板。
1. 能够对接工厂库存api，在前段商店页面渲染每个产品的库存量。
1. 不制作云端购物车存储系统，在client side中维持一个pinia state作为其一次购物的购物车。购物车在每次登录间不保存。
1. 维持一个订单信息数据库，每个订单信息包括购买了哪些产品，每个产品购买量，购买者账户，订单目的地地址，订单state：『ordered， shipped』，以及stripe支付信息。
1. 购物者能够使用stripe对自己的购物车内的产品集合进行下单。用户下单前向工厂发送库存检查，若库存不支持此订单，拒绝request。每次用户成功下单后向工厂库存api发送库存减少指令。
1. 购物者能够检视所有自己已经付款的订单。
1. 网站管理员能够检视和通过用户名搜索订单，网站管理员能够更新订单state从ordered至shipped

### 核心user stories
1. 作为一个网站账户持有者，我想要通过输入帐号和密码来登录网站。
1. 作为一个网站管理员，我想要检视每个用户注册申请表。
1. 作为一个网站管理员，我想要能够接受或拒绝用户注册申请表。
1. 作为公司股东，我想要用户注册申请表仅管理员可见
