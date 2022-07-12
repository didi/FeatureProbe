用户手册
# 用户使用手册
欢迎来到FeatureProbe！从这里，您可以了解如何使用FeatureProbe的更多信息。

### 常见的使用场景
1. 功能发版（增加新功能或整体版本升级，在不确定影响的情况下，可以只开放给少量用户进行快速试错，反馈良好的情况下再逐步放量，直到全量放开。
2. 运营活动（不同时期的线上运营活动，模式基本固定，每次只需要修改几个变量）
 - 例：运营人员想在"My First Project"项目下推出商品秒杀活动，需要根据不同城市设置不同的商品价格，以往都是需要通过开发在代码中写好，一旦价格需要变动，开发人员要在代码中修改线上商品价格，在经过审核部署发布等一些列操作，才能生效，使用FeatureProbe的功能开关，只需要运营人员修改一下“价格”，便可一秒发布生效。

   + 流程说明
     * 运营人员在FeatureProbe上新增项目"My First Project"，并在项目的"online"环境下创建一个名叫"Promotional Campaign"的开关，开关配置如下图所示:![This is an image](./pictures/Commodity_spike_activity.png)
     * 开发人员在代码中引用FeatureProbe的sdk，配置"online"的密钥，并关联开关的key（promotion_activity），设置number类型的variations（用户价格分层）对应好定义的参数city
  
    ```java
   FPUser user = new FPUser(user_id);
   user.with("city", city_name);
   double discount = fpClient.numberValue("promotion_activity", user, 1.0);
   discountSetTo(discount);
    ```
 
     * 在一切准备就绪后，运营人员直接把"Promotional Campaign"开关"启用"，配置内容便即刻生效了
     * 价格需要变动的情况下，运营人员只需要直接修改variations中的价格，并分发给对应的人群即可

3. 服务降级预案（如依赖的后端服务访问失败可以切服务切换为从缓存服务中获取历史数据版本）
 - 例：一般依赖的商品库存服务发现故障时，需要开发人员通过修改代码的方式进行降级，于是使用FeatureProbe，一旦依赖的商品库存服务发现故障时，可以快速降级从缓存中获取相当的商品库存数据。
   + 流程说明
     * 开发人员在项目My First Project"的"online"环境下创建一个名叫"Service Degrade"的开关，开关配置如下图所示:![This is an image](./pictures/Store_service_fallback.png)
     * 开发人员在代码中关联开关的key（service_degrade），设置boolean类型的variations（是否打开降级）
  
    ```java
   FPUser user = new FPUser(user_id);
    boolean fallback = fpClient.boolValue("service_degrade", user, false);
    if (fallback) {
    	// Do something.
    } else {
    	// Do normal process.
    }
    ```
 
     * 当依赖的库存服务发生故障后，为避免对用户造成影响，可以快速将库存服务进行降级，使用缓存非实时库存数据。

4. A/B实验（针对同一个板块，设计了几套不同的方案，想看一下哪种方案比较受欢迎）
 - 例1：某平台的支付按钮的颜色想由红色改为了绿色，产品小王不缺定哪个颜色效果更好，于是想使用FeatureProbe的功能开关，针对这两种颜色对巴黎的用户做个实验，看到底哪个颜色购买率更高

   + 流程说明
     * 运营人员在项目My First Project"的"online"环境下创建一个名叫"Button Color AB Test"的开关，开关配置如下图所示:![This is an image](./pictures/Color_ab_test.png)
     * 开发人员在代码中关联开关的key（color_ab_test），设置string类型的variations（颜色分类）对应好定义的参数city
  
    ```java
   FPUser user = new FPUser(user_id);
    user.with("city", city_name);
    String color = fpClient.stringValue("color_ab_test", user, "red");
    setButtonColor(color);
    ```
 
     * 在一切准备就绪后，运营人员直接把"Button Color AB Test"开关"启用"，配置内容便即刻生效了
     * 产品小王通过查看实验数据得出，支付按钮为绿色购买率会更高，于是全量用户开放为绿色

除此之外，我们还准备了一个彩蛋：FeatureProbe的功能开关（header_skin），可以控制平台的皮肤，快去看看吧。


# 功能介绍
## 登录
## 账户管理
## 项目管理
## 使用功能开关
  
    
## 登录
首次登录FeatureProbe平台，需要用“初始账号和密码”【账号：Admin  密码：Pass1234】登录。

1. 在账号处填写Admin
2. 在密码处填写Pass1234
3. 点击登录按钮，即可登录成功

## 账户管理
### 添加账户成员
如果想添加更多的企业成员，来共同使用FeatureProbe平台，则需要Admin到账户管理中添加成员。

1. 点击添加成员按钮
2. 填写成员账号（可批量添加，逗号隔开即可）
3. 默认密码（系统提供了快速密码作为被添加成员的初始密码，如果想自定义密码，请取消勾选）
4. 填写密码（“默认密码”取消勾选后，自定义密码为必填项）
5. 点击创建按钮，即可完成账户成员的创建

- 注：账户成员添加成功后，即可到登录页面登录FeatureProbe平台（账号和密码即为Admin创建成员时的账号和密码）

### 编辑账户成员【常用于成员“忘记密码”】
Admin这个账号任何人都没有编辑权限，Admin有权限编辑其他账户成员信息。

- 注：修改后，成员可重新登录系统（若在成员登录后编辑了成员的密码，该成员在无任何操作30分钟的情况下，系统会让其重新登录）

### 删除账户成员
Admin这个账号任何人都没有权限删除，Admin有权删除其他账户成员。

- 注：账号成员一旦删除，则不可再创建与被删除成员重名的账号

### 修改密码
获得初始密码后，出于安全性考虑，任何人都可以修改自己的密码。
1. 填写原密码
2. 填写新密码
3. 确认新密码（与新密码保持一致）
4. 点击保存按钮，即可完成密码的修改（退出后，可直接用新密码登录，或者该成员在无任何操作30分钟的情况下，系统会让其重新登录）

## 项目管理
项目允许一个FeatureProbe帐户管理多个不同的业务目标。一般一个项目对应一个系统平台。例如，您可以创建一个名为“移动端应用程序”的项目和另一个名为“服务端应用程序”的项目。每个项目都有自己独特的环境和对应的功能开关。您可以在每个项目中创建多个环境，所有项目必须至少有一个环境
系统会有一个初始化项目（My First Project），初始项目下有2个环境（test、online）每个环境都有自己的SDK密钥，用于将FeatureProbe SDK连接到特定环境。
### 添加项目![create project screenshot](./pictures/create_project.png)
1. 点击顶导“Projects”，进入项目列表页
2. 点击添加项目按钮
3. 填写项目名称
4. 填写key（项目的唯一性标识，一旦创建不可编辑）
5. 填写描述
6. 点击创建按钮，即可完成项目的创建（项目一旦被创建，则不能删除。新创建的项目会自带一个online环境）
7. 点击项目下的环境卡片，可直接进入该项目下该环境的开关列表页面

### 编辑项目![create environment screenshot](./pictures/create_environment.png)
1. 点击编辑项目icon
2. 编辑项目信息
3. 点击保存按钮，即可完成项目的编辑

### 添加环境

1. 点击添加环境
2. 填写环境名称
3. 填写key（环境的唯一性标识，同项目下唯一，一旦创建不可编辑）
4. 点击创建按钮，即可完成环境的创建（环境一旦被创建，则不能删除）

- 注：新环境创建后，会共享该项目下的开关列表（开关的模板信息），开关的配置信息则需进入该环境去自主配置。

### 编辑环境
![edit environment screenshot](./pictures/edit_environment.png)
1. 点击编辑环境
2. 编辑环境信息
3. 点击保存按钮，即可完成环境的编辑

## 使用功能开关
FeatureProbe平台提供了强大的功能开关管理模块，功能开关通过选择目标流量，进行功能投放，通过持续观测数据逐步放量直到全量部署。
### 开关仪表盘

![This is an image](./pictures/toggle_list.png)
1. 默认展示My First Project的online环境的开关列表信息
2. 左侧导航栏提供了快速切换环境的入口（点击环境右侧的下拉icon）
3. 通过筛选条件，我们可以根据"evaluated","enabled/disabled","tags","name/key/description"对开关进行快速的筛选

### 添加开关模板
开关的“模板信息”（开关创建成功后，将同步成为已有环境的初始化信息）

![This is an image](./pictures/create_toggle.png)
1. 填写开关名称
2. 填写开关的key（开关的唯一性标识，同项目下唯一，一旦创建不可编辑）
3. 填写描述信息
4. 选择标签（无初始值，可自行创建）
5. 选择sdk类型
6. 选择开关的return type（支持4种：Boolean、String、Number、JSON），一旦创建不可编辑
7. 填写Variations
    - 默认两个variations，value为空（最少2个，可自行增减）【value可更改，name可更改，description可更改】

8. 填写disabled return value（开关禁用时的返回值），默认同步variation1的数据，可更改
9. 点击创建按钮，完成开关的创建

### 编辑开关模板
开关的“模板信息”（编辑成功后，不会影响已有环境中的开关配置信息，仅同步到未来新环境的初始化信息）

### 开关配置
开关的“配置信息”（各环境间不共享，独立拥有,修改配置信息，不会同步到开关的"模板信息"），请切换到目标环境后，再进行配置（配置信息的初始信息，会自动同步开关的“模板信息”）

![This is an image](./pictures/toggle_targeting.png)
1. Status：开关的状态（禁用后生效Disabled return value，启用后开关配置中的Rules及Default Rule生效）
2. Variations：默认同步开关的模板信息（可更改）
3. Rules：多个Rule之间为“或”关系（rule的顺序很重要，一个用户进来，是从上往下依次筛选的，命中了第一个Rule就不会再匹配下面的Rule，没命中的才会继续往下筛）

  - 添加Rule：为“指定人群”设置“返回值”
 
    + 填写rule名称
    + 根据“条件”筛选“指定人群”，条件之间为且的关系（至少有一个条件）
      * 添加条件：选择用户属性（自定义添加，回车生效）、选择关系符、填写具体的值（自定义添加，回车生效）
      * 删除条件：点击条件行右侧的删除icon，即可删除该条件
    + 指定返回值：在variations中选择【可以选择某一个variation（该项占比100%），也可以每个variation指定百分比（所有的variation占比之和必须为100%）】
    + 点击Rule卡片区域并拖动，可以对rule进行自由排序
    + 删除Rule卡片：点击卡片右上角删除icon即可删除整条Rule

4. 设置Default Rule：为“未指定人群”设置默认返回值：在variations中选择【可以选择某一个variation（该项占比100%），也可以每个variation指定百分比（所有的variation占比之和必须为100%）】
5. Disabled return value：默认同步开关的模板信息（可更改）
6. 点击Publish，显示更改前后的diff信息
7. 查看diff后，点击confirm，完成发布

### 查看开关的访问信息
开关的访问信息（各环境间不共享，独立拥有）

![This is an image](./pictures/evaluations.png)
1. 统计信息：展示每个Variation的总访问量
2. 统计粒度：目前默认展示24小时内的访问信息，点击可切换查看7天内的访问信息
