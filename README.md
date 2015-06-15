- ###数据格式###

请求都附上sid:  (session)  
服务器返回数据为JSON：

    {
        "info": "",        //String
        "data": "",
        "status":         //Int
    }
    info: 字符串类型，通常是一些提示性信息，如"登录成功"，"字段不能为空"等等；
    data: 有效数据，类型不定
    status: 整型，0通常表示合法或成功的状态，非零一般表示非法或失败

-----

- ###设置###

URL: /api?m=config  
POST参数:

    1. timestamp(时间戳)

返回值:

    1. session_info(session 信息)
        1.1 session_id(session id)
        1.2 city_id(选择城市id)
        1.3 city_name(选择城市)
    2. config(如果为空就不用更新,不为空有内容则更新)
        2.1 app_setting(软件内部的一些设置)
            2.1.1 service_phone(客服电话)
            2.1.2 app_comment_url(app评论链接)
            2.1.3 about_url(关于我们的url链接)
            2.1.4 disclaimer_url(免责声明url链接)
            2.1.5 version
                5.1 ios
                    5.1.1 build_id(本地软件版本1，2，3)
                    5.1.2 force(是否强制更新)
                    5.1.3 update_log(更新文案)
                    5.1.4 version_url(更新版本的地址)
                5.2 android
    3. timestamp(新的时间戳)

-----
#地址部分#

- ###城市列表###

URL: /api?m=city_list  
POST参数:

    1. lon(经度, 空为无效)
    2. lat(纬度, 空为无效)
    3. page(当前请求第几页)

返回值:

    1. gps_city(定位城市, 空为没有)
        1.1 city_id(城市id)
        1.2 city_name(城市)
    2. city_list(城市列表)
        2.1 city_id(城市id)
        2.2 city_name(城市)
        2.3 city_pingyin(拼音)
        
- ###选择城市###

URL: /api?m=city_select  
POST参数:

    1. city_id(城市id)

返回值:

    默认

- ###地址列表###

URL: /api?m=addr_list  
POST参数:

    1. page(当前请求第几页)

返回值:

    1. pages(总页数)
    2. result(结果)
        2.1 city_area(区域地址)
        2.2 addr(详细地址)
        2.3 receiver(收件人)
        2.4 phone(联系电话)
        2.5 is_default(是否默认地址)
        2.6 addr_id(地址id)

- ###删除地址###

URL: /api?m=addr_delete  
POST参数:

    1. addr_id(地址id)

返回值:

    默认

- ###区域地址###

URL: /api?m=addr_area_list  
POST参数:

    1. lon(经度, 空为无效)
    2. lat(纬度, 空为无效)

返回值:

    1. city_list(城市列表)
        1.1 city_id(城市id)
        1.2 city_name(城市)
        1.3 area_list(行政区域列表)
            1.3.1 area_id(行政区id)
            1.3.2 area_name(行政区名)
    2. area_default
        2.1 city_id
        2.2 city_name
        2.3 area_id
        2.4 area_name


- ###添加地址###

URL: /api?m=addr_add  
POST参数:
    
    1. receiver(收件人)
    2. phone(联系电话)
    3. city_area(城市区域)
    4. addr(详细地址)

返回值:

    默认

- ###设置默认地址###

URL: /api?m=addr_set_default  
POST参数:

    1. addr_id

返回值:

    默认
    
-----

#登录部分#

- ###获取验证码接口###

URL: /api?m=get_code  
POST参数:
 
    1. phone（手机号码）

返回值:

    默认

- ###登录接口###

URL: /api?m=signin  
POST参数:

    1. phone(手机号)
    2. code(验证码)

返回值:

    1. status(1为登录错误)
    2. info(错误信息)
    3. data
        3.1 userinfo
            3.1.1 avatar(头像URL)
            3.1.2 username(用户名)
            3.1.3 phone(手机号) 
            3.1.4 user_id(用户id)
        
- ###退出登录###

URL: /api?m=signout  
POST参数:

    默认

返回值:

    默认
    
-----

#个人信息部分#

- ###个人信息###

URL: /api?m=userinfo  
POST参数:

    默认

返回值:

    1. avatar(头像URL)
    2. username(用户名)
    3. phone(手机号)
    4. user_id(用户id)
    
- ###修改个人信息###

URL: /api?m=userinfo_edit  
POST参数:

    1. avatar(头像base64编码, 空为无效)
    2. username(用户名, 空为无效)

返回值:

    1. avatar(新头像url)
    
- ###得到token上传头像###

URL: /api?m=get_qiniu_token
POST参数:
    
    1. image_name(图片名有MD5生产的32位字符串)
    
返回值:
    
    1. token
    得到token后用七牛的sdk发起请求，
    imageDataBf = 图片的data
    _imageName = 图片名
    QNUploadOption *opt = [[QNUploadOption alloc] initWithMime:@"image/jpeg" progressHandler:nil params:@{ @"x:sid":[[Constant instance] session] } checkCrc:NO cancellationSignal:nil];
                
    QNUploadManager *upManager = [[QNUploadManager alloc] init];
    [upManager putData:imageDataBf key:_imageName token:token
            complete: ^(QNResponseInfo *info, NSString *key, NSDictionary *resp) {
                
                        NSLog(@"%@", info);
                        NSLog(@"%@", resp);（这里返回头像url）
                }
-----

#服务相关部分#

- ###首页服务项目列表###

URL: /api?m=serve_list  
POST参数:

    默认

返回值:

    1. serve_id(服务项目id)
    2. serve_short_name(服务项目名)
    3. serve_name(服务项目的详细名)
    4. image_list(服务的图片url数组)
    5. current_price(现价)
    6. orgion_price(原价)
    7. serve_time(服务需要的时间)
    8. sales_volume(销量,几人做过)
    9. introduce(介绍)
    10.comment_number(评论数)
    11.least_number(最少起订数量)

- ###评论列表###

URL: /api?m=comment_list  
POST参数:

    1. page:(当前请求第几页)
    2. serve_id(服务项目id)

返回值:

    1. pages(总页数)
    2. result(结果)
        2.1 avatar(头像url)
        2.2 user_name(用户名)
        2.3 comment_time(评论时间)
        2.4 comment_content(评论内容)


- ###服务项目下单时的信息###

URL: /api?m=serve_order_info  
POST参数:

    1. serve_id

返回值:

    1. time_list_info(服务可以预约的时间列表)
        1.1 date(年月日)
        1.2 time_list(具体几点数组,10:00,11:00)
    2. addr_default(默认地址信息)
        2.1 addr(地址)
        2.2 receiver(收件人)
        2.3 phone(联系电话)
        2.4 addr_id(地址id)


- ###提交订单###
URL: /api?m=submit_order  
POST参数:

    1. serve_id(服务id)
    2. date_time(预约时间)
    3. addr_id(地址id)
    4. pay_type(支付方式,int 1.微信 2.支付宝)
    5. number(预定的数量)
    6. order_time_id(预约时间的id)
    
返回值:

    如果为支付宝支付
    1. order_string(服务端签名好的一段字符串)

    如果为微信支付
    1. partnerid(商家id)
    2. prepayid(预支付订单id)
    3. package(商家根据财付通文档填写的数据和签名)
    4. noncestr(随机串，防重发)
    5. timestamp(时间戳，防重发)
    6. sign(商家根据微信开放平台文档对数据做的签名)

- ###得到支付信息###

URL: /api?m=get_pay_info
POST参数:

    1. order_id(订单id)

返回值:
    
    跟提交订单返回的一样


- ###订单列表###

URL: /api?m=order_list  
POST参数:

    1. status(1.进行中 2.已完成)
    2. page

返回值:

    1. order_id(订单id)
    2. order_number(订单编号)
    3. date_time(预约时间)
    4. serve_image(服务的图片)
    5. serve_name(服务的详细名)
    6. serve_time(服务需要的时间)
    7. serve_id(服务项目id)
    8. price(总价)
    9. status(订单状态 1.待付款 2.等待服务 3.待评价 4.评价完成 5.申请退款 6.退款成功 7.异常订单)
    10.order_count(购买的份数)
    11.pay_type(支付方式)
    12.addr_info(服务地址信息)
        12.1 city_area(城市区域)
        12.1 addr(详细地址)
        12.2 receiver(收件人)
        12.3 phone(联系电话)
        12.4 addr_id(地址id)

- ###评价订单###

URL: /api?m=order_comment  
POST参数:

    1. star_level(评价几星1-5)
    2. content(评价内容)
    3. order_id(订单id)

返回值:

    默认
