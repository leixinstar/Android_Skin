Android_Skin
============

前言

	先来拉下仇恨，看看能不能让让仇恨满级 ...
	使用 ActionBar 的同学，请绕路 ... 我救不了你 ...
	喜欢用 new View 的同学，请谨慎考虑一下 ... 这是一条不归路 ...
	剩下仅使用 layout.xml 的同学 ...

介绍

	Android 安装式的插件 ... 我就不介绍了 ... 个人认为花精力去研究这种东西，毫无意义可言 ...
	本文将插件分为两种：皮肤插件和功能插件。相应的同名同类型资源优先级关系：皮肤插件资源 > 宿主应用资源 > 功能插件资源
   
背景

	虽然本文介绍的是 Android 非安装式的插件，但是，这里的插件都是 apk ... 当然，为了避免用户错误安装了插件 apk，这些插件 apk 发布前，最好嫑签名 ... 
	以下三点，让我一直感叹，Google 为何如此坑爹的原因 ... 
	1）同一 resid，可能是宿主应用包和插件应用包都有，而对应资源却是不同的 ...
	2）Android SDK Package ID 以 0x01 开头 ...
	3）Android App Package ID 以 0x7f 开头 ...

皮肤插件

	寻寻觅觅，冷冷清清，凄凄惨惨戚戚 ... 一路查找资料和查看Android源码，终于找到突破口了 ... 
	Android 获取资源，最终都是通过 Resource 这个类来代理 AssetManager 实现的。
	
	1）接入口
	Activity 也是一个 Context ... 更准确的来说，他是一个 ContextThemeWrapper ...  而真正的 Context 是通过 attachBaseContext 设置进来的 ... 那么，我们以此为入口，通过重写 attachBaseContext  代理一下外部设置进来的 ContextImpl ...
	2）插件资源的管理类的选择
	细究 Android 资源管理机制，我们可以轻易发现，Android 资源的管理是通过 Resource 这个类来管理。而APP Resource 在内存中只有一个备份的，以WeakReference 形式。那么我们以这个思路，设计一个皮肤插件管理类，管理皮肤插件 Resource。
	3）如何换非 inflater View 的资源呢？
	这里的资源不一定是以文件形式存在，R 文件里包含的所有内容都是资源。
	也许，有看过插件式开发的同学，会对此嗤之以鼻，可能会认为只要加载了皮肤插件包资源就 OK了。但这皮肤插件资源必须与宿主应用的资源数量、类型要保持一致，多一个少一个都不行。原因是：同一 resid，可能是宿主应用包和插件应用包都有，而对应资源却是不同的 ... 但，后续开发，不会涉及资源这一模块的变化么？那是不可能的 ... 
	那么，我们要怎样实现换肤功能呢？首先我们要明确，我们换肤要换啥？换的是同名同类型资源（包含drawable、color、colorstatelist、string、text、dimen等。但坚决不能换 id。不建议换 layout，layout 与代码中的逻辑绑定比较深） ...
   	4）如何换 inflater View 的资源呢？
   	看到上面那些内容，喜欢用 new View 的同学应该很开心吧 ... 是啊 ... 我曾经也是喜欢用 new View ... 也为此开心了一阵 ... 但随之而来的是打击 ... 当我欢欢喜喜的拿着研究成功给老大看了一下 ... 他就指出，layout.xml 中的资源能够一键搞定么？一键搞定么？搞定么？么? ...
   	我勒个去了 ... 还真是，这解决方案还真没啥实施价值，毕竟工程中还有很多 layout ... Toast.makeText 中也有 layout ... 擦了 ... 咋整？
   	众里寻她千百度，蓦然回首，那人却在灯火阑珊处 ... 就酱紫 ... 半个月苦苦寻觅，终于找到解决方案了 ... LayoutInflater ... 
   	
功能插件

	得空再写这些文档吧 ...

详细介绍见：http://www.eoeandroid.com/thread-553503-1-1.html

	说多了都是泪，就酱紫大家将就着自己读代码吧 ... 如有不理解的，可以发 Email 问我

												Author: 林恒龙
												Email:v7lin@qq.com
