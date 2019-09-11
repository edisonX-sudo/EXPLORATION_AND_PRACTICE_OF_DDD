# 领域驱动设计的探索与实践

当系统的业务复杂达到一定的规模后,在不进行任何手段的情况下,系统本身将变得愈发难以理解,扩展和维护.领域驱动设计(ddd)的提出,恰恰针对性了解决了上述问题.如今,越来越多的团队渐渐认识到ddd的强大,逐渐去理解ddd,去使用ddd并享受ddd为其带来的好处.各大网站也有很多关于ddd相关的译文和介绍,然而实践后的感知比较少.因此,今天我将就我本身的实践提出我对ddd的一些理解,希望能对大家有启发.
# 战略上 


## 限界上下文
限界上下文是ddd的战略工具盒内的一个关键的工具,在ddd里扮演着一个不可或缺的角色.即便你不配合使用ddd战术部分工具,他仍然能在战略层次,为业务逻辑分隔,控制业务复杂度上发挥其巨大的作用.在微服务盛行的今日,作为分割服务[边界]的一个重要指导标准,限界上下文的重要程度更是与日俱增.学习ddd时很多人刚开始可能对限界上下文的概念比较模糊,难以把握.其中很大的原因是因为限界上下文的词语本身就非常抽象,对此我将采用类比的方法来阐述我对限界上下文的理解.先理解什么是[上下文],后理解[限界],再谈一些[划分]上下文的建议.


### 上下文
![image.png](https://cdn.nlark.com/yuque/0/2019/png/405864/1568171031103-65f6af73-4780-41a8-a365-49318234e310.png#align=left&display=inline&height=492&name=image.png&originHeight=492&originWidth=562&size=338051&status=done&width=562)
> pizza on the table/pizza on the ground(pizza处于不同的上下文,得到了不同的意义)



如图所示,
对于处于餐桌上的pizza而言,他非常的可口,他可以被吃掉.
对于处于马路上的pizza而言,他是垃圾,可以被扔掉一个pizza.
处于不同的环境,让pizza得到了完全不同的意义,同时他也能行使不同的职责(被吃,被扔).上下文可以用图片描述(上图),可以被语言描述(谁在什么地方做了什么),也可以被语言描述(代码也是语言).从图上的角度看待,上图的pizza是处于不同上下文的,其对应的职责也是被恰当的分割到了2个上下文里.上下文内部概念及其职责都与上下文本身相关,概念复杂度被合适的限定到对应的上下文内,而上下文也能更好的被理解.


### 限界
限界,即用边界限定,用城墙围起来.这里的边界是指包围业务概念,保护业务概念的边界.参考下图[阶段2],概念A内聚后若不给其界定边界,外部的资源B将有可能感知到其不愿意暴露的概念,那么外部资源就有可能使用它,最终可能导致概念A处于不一致的或难以扩展(内部概念被依赖而无法自由重构)的状态.所以我们要用边界包围内聚概念.


![image.png](https://cdn.nlark.com/yuque/0/2019/png/405864/1568171283081-d5d56e9b-68e1-490e-8653-e2633302435f.png#align=left&display=inline&height=173&name=image.png&originHeight=173&originWidth=690&size=32185&status=done&width=690)
> 罗列,内聚,限界

边界有若干种的表现形式:

- 命名空间级别：逻辑边界仅仅通过命名空间进行界定，但是所有的限界上下文其实都处于同一个模块中，编译后都属于同一个Jar包。
- 模块级别：在命名空间上是逻辑分离的，而不同限界上下文则属于同一个项目的不同模块，编译后会生成各自的Jar包。若限界上下文之间存在依赖，则在运行时，这些Jar会被同时加载到同一个Java虚拟机中。这里所谓的“模块”，在Java代码中也可以创建为Jigsaw的module。
- 进程级别:直接将不同的上下文用不同的项目实现,之间通过http等手段进行交互

当然并不是使用了这些技术,就一定可以得到安全的边界(不小心暴露内部概念或可能影响业务一致性的功能),利用这些方式进行边界划分的同时也要好好利用暴露出来的对象的封装级别,用封装将一致性的转移和内部概念封锁在对象内部,只暴露出和业务相关的概念.


### 划分限界上下文的建议
划分上下文,和业务关注事物的角度有关,进行划分时是可以将一个客观概念分割的,切莫认为同一个客观概念就一定是都在一个上下文.如上图的pizza所示,一个客观概念可能在多个上下文,有不同的身份(如:某人在儿子面前是爸爸,在妻子面前是丈夫,在父亲面前是儿子,在公司里面是老板,在国家里面是公民).同时,即便是完全不同的客观存在,从合适的角度来看,也是能发现他们可能共享一个相同的上下文(如:老师和父亲职责有一定差异,但都是人).当然是否要抽取这个上下文取决于业务系统是否从业务角度关注.
为了更好的说明,我在此做个虚拟的场景.
假设有:
[原创文学聚集地]webapp是一个关注将优秀作者的原创文学产物第一时间展现给读者的书籍webapp.
那么书籍这个概念在这个系统就可能被这样划分.如此划分是因为业务系统本身关注[作者业务,编辑业务,读者业务]这几个方面,同时书籍在这个几个不同的关注中体现出清晰的不重叠的职责.
![image.png](https://cdn.nlark.com/yuque/0/2019/png/405864/1568171451831-34a6ed34-0e9d-417c-8c77-76229b02f2e3.png#align=left&display=inline&height=192&name=image.png&originHeight=192&originWidth=483&size=40480&status=done&width=483)
> 书(不同的行为&交互)

![image.png](https://cdn.nlark.com/yuque/0/2019/png/405864/1568171475885-3be04ad4-2ea5-44fb-a062-9ff1ac94fb1c.png#align=left&display=inline&height=155&name=image.png&originHeight=155&originWidth=252&size=15871&status=done&width=252)
> 书在不同上下文的职责

在业务系统的关注确定,我们也感知出清晰的客观概念后,我们可以尝试者给所有的客观概念画生命周期图,再给对应的概念画好生命周期图后,尝试去寻找他们交互的地方,分析交互处的ubiquitousLanguage的意义,继而根据分析情况,辅助我们发现清晰的上下文.
![image.png](https://cdn.nlark.com/yuque/0/2019/png/405864/1568171646984-490b90ab-b5b2-416e-ba32-9cf042fcd9eb.png#align=left&display=inline&height=263&name=image.png&originHeight=263&originWidth=549&size=45720&status=done&width=549)
> 概念生命周期

刚刚开始进行上下文划分的时候可能会有这样一个误区,直接在零散的概念中寻找边界,立马给对应上下文划分边界.然而正确的做法应该是先深入的讨论理解所有的概念,把相关的概念联系起来,放在一起,再从全局的视角看待,就会发现边界会自然的显现.
![image.png](https://cdn.nlark.com/yuque/0/2019/png/405864/1568171695662-b15876da-9932-4b43-ad40-63f00b02d1c1.png#align=left&display=inline&height=302&name=image.png&originHeight=302&originWidth=413&size=41167&status=done&width=413)
> 直接开始找概念边界(边界不易寻找,扭曲)

![image.png](https://cdn.nlark.com/yuque/0/2019/png/405864/1568171707295-68585b8b-af3d-46e7-8f80-00474f8bc691.png#align=left&display=inline&height=347&name=image.png&originHeight=347&originWidth=433&size=32504&status=done&width=433)
> 将概念关联(内聚)后找边界(边界易于寻找,清晰)


# 战术上


## 六边形架构
六边形架构,或称端口和适配器架构,是实践领域驱动战术部分的一种非常好的架构方式.它清晰地区分领域模型和输入输出设备之间的界限.使用六边形架构,就能让技术代码与领域模型代码分离,除去了业务代码内有害业务理解的噪音,同时负责技术实现的代码也会更加纯粹,偏向技术本身.利用六边形架构实现ddd,我们将打造出浓缩独立的领域模型,也会造就出易于替换拓展的基础设施.为了更好的说明六边形架构,接下来我将结合传统的3层架构,在前后对比中描述六边形架构的一些特性以及列出其相应的实现.
![image.png](https://cdn.nlark.com/yuque/0/2019/png/405864/1568172245240-92d79a22-030c-47a4-9c29-32d01ec39896.png#align=left&display=inline&height=325&name=image.png&originHeight=254&originWidth=221&size=6934&status=done&width=283)
> 图A:3层架构

![image.png](https://cdn.nlark.com/yuque/0/2019/png/405864/1568181772727-91bf734e-eac9-407e-974a-27010c665fab.png#align=left&display=inline&height=643&name=image.png&originHeight=643&originWidth=695&size=179830&status=done&width=695)
> 图B:六边形架构

### 
### 从关注上来看
传统3层架构中并没有体现出领域的概念,分层的结构体现出其关注的反而是技术组件的类型.
六边形架构直接体现出其对领域部分的关注,并将领域部分显式的添加到架构图上,本身也说明了他和领域驱动设计战术部分的贴合.


### 从模块的依赖流向上来看
以图A为例,面对一个请求,3架构的流向是
controller->service->dao
下层总是为上层所调度,上层总是依赖下层.
以图B为例,面对一个请求,六边形结构的流向是
RestAdapter->core(application->domain)->AdapterF(如:DB)
与3层架构不同,外围(如RestAdapter, AdapterF)总是依赖core(application->domain),左边RestAdapter负责调用core的概念,右边AdapterF(如:DB)负责实现core的概念.core不知道外围的概念,RestAdapter和AdapterF仅仅知道core的概念而不会相互感知,通过这种隔离最大程度的防止危险的复用,防止耦合.
为了对六边形架构有更深入的理解,这里给出一种对该架构的实践方式.
![image.png](https://cdn.nlark.com/yuque/0/2019/png/405864/1568172143100-5e888bbe-cb57-4153-b673-be25541e1f71.png#align=left&display=inline&height=389&name=image.png&originHeight=389&originWidth=465&size=41324&status=done&width=465)
> 图C:项目模块结构

![image.png](https://cdn.nlark.com/yuque/0/2019/png/405864/1568172230707-4097a0d9-b11d-4973-b64d-100a37489709.png#align=left&display=inline&height=515&name=image.png&originHeight=515&originWidth=1286&size=31844&status=done&width=1286)
> 图D:模块依赖关系

![image.png](https://cdn.nlark.com/yuque/0/2019/png/405864/1568181659450-0528d795-0dc6-4edd-bb74-bbff542b0ad6.png#align=left&display=inline&height=601&name=image.png&originHeight=601&originWidth=1332&size=141193&status=done&width=1332)![image.png](https://cdn.nlark.com/yuque/0/2019/png/405864/1568181668030-ca4a08f8-a6ce-4af3-97c5-da8595a2b083.png#align=left&display=inline&height=591&name=image.png&originHeight=591&originWidth=1405&size=129792&status=done&width=1405)
> 相关模块maven的依赖声明

以图C为例,从请求的流向上来看,其流向是http->core(application->domain)->core-impl.左边http负责调用core的概念,右边core-impl(如:DB)负责实现core的概念.
以图D为例,从模块依赖上来看,external作为父pom依赖core内的application,故external下所有模块都会依赖application,同时由于application本身也依赖了domain,因此external也能感知到domain的概念,如此最终体现出六边形架构的性质.


### 3层架构真的一无是处么
3层架构并没有因此退出历史舞台,他仍然保有自己独有的优势,如我们的项目由于工期要求急,后期变动少,项目规模小,人员成本大等因素没有在使用ddd的方式建模,如图E所示,只要项目的业务复杂度在一定数值以内,3层架构+表模型建模的方式甚至可以帮助我们快速的完成项目.
另一个方面,在我采取cqrs的方式来实现我们的项目的时候,承载读模型的项目由于并不关注对应领域的规则(维护一致性),采取3层架构的方式来实现成本更低,相应更快.
![image.png](https://cdn.nlark.com/yuque/0/2019/png/405864/1568181751561-25dbd853-f1bf-4be0-a98e-b6146b85ce4c.png#align=left&display=inline&height=479&name=image.png&originHeight=479&originWidth=638&size=265042&status=done&width=638)
> 图E:付出(纵轴)与业务复杂度(横轴)的关系

## 
## 设计安全&易于理解的聚合
聚合是操作领域模型的门户,聚合若暴露过多的概念,会导致聚合承载过多信息,不易被调用者使用.聚合若不慎暴露了内部概念,可能会导致内部概念被不正确的使用,导致聚合无法处于一个正常的一致的状态.即便不会引发一致性不正确,也可能会使得后期无法自由的重构聚合(如删除或改变内部概念).因此,对于聚合的暴露的功能,要严格的控制.为了能一定程度的解决以上问题,我利用了以下方法来应对.


### 利用模块和包来强化聚合的封装
[利用模块和包来强化聚合的封装]是为了增强聚合的封装性,不暴露出不必要的方法和概念的方法.如图我们在实现core-impl模块下的对应domain模块内某聚合的repository时,可以采取让core-impl模块下repository的实现和domain模块下对应的聚合使用同包名,同时聚合的字段采用包级别封装的方式,来强化聚合封装.
![image.png](https://cdn.nlark.com/yuque/0/2019/png/405864/1568181632504-b38f7203-dde9-4f17-b470-d8967f41fc1b.png#align=left&display=inline&height=641&name=image.png&originHeight=641&originWidth=995&size=141873&status=done&width=995)
> 模块core-impl

![image.png](https://cdn.nlark.com/yuque/0/2019/png/405864/1568181639312-ba31666f-cceb-4328-92f6-8178a79f6c28.png#align=left&display=inline&height=812&name=image.png&originHeight=812&originWidth=1401&size=391698&status=done&width=1401)
> 模块domain

如此就不用暴露不必要的视窗方法和构造方法,让调用者值仅仅感知到聚合必要的面向领域的暴露.



---

结尾:本文的重点是基于我个人对ddd的一些理解，希望能整理出一些自己总结出来的一些感悟和经验，并分享给大家。希望大家能在实践ddd的过程把自己的一些新的发现和心得分享出来,推动ddd在国内的发展,用以打造伟大的软件.
