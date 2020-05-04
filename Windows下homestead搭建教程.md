#### 前言：很多laravel项目，生成环境一般是Linux，然而很多开发者习惯在windows上进行项目开发，由于开发环境不一致，项目上线后会出现很多问题，而homestead是一个集成了laravel项目所需要的环境的linux虚拟机盒子,例如redis、mysql、php，非常利于进行laravek开发。

### 安装步骤：

1. 安装VirtualBox、vagrant，重启电脑
2. 下载homesetad box盒子,从learnku找资源,新建文件metadata.json（与盒子同一路径）,填写盒子信息

		{
		    "name": "laravel/homestead", # 命名
		    "versions":
		    [
		        {
		            "version": "6.4.1", # 版本
		            "providers": [
		                {
		                  "name": "virtualbox", # 盒子名称
		                  "url": "virtualbox.box" # 盒子路径
		                }
		            ]
		        }
		    ]
		}

3. 执行命令`vagrant box add metadata.json`将盒子新增到vagrant（安装vagrant重启后新增系统变量，所以可执行vagrant命令）
4. 拉取homestead管理工具代码 `git clone https://github.com/laravel/homestead.git` 
5. 在homstead管理工具文件夹中执行`bash init.sh`或者`init.bat`进行初始化
6. 修改homestead.yaml文件夹
7. 配置密钥、共享文件夹、站点（需修改hosts文件）

		---
		ip: "192.168.10.10" # 本地访问地址
		memory: 2048
		cpus: 1
		provider: virtualbox # 虚拟机管理工具，我们选virtualbox
		
		authorize: ~/.ssh/id_rsa.pub # 公钥地址
		
		keys: 
		    - ~/.ssh/id_rsa # 私钥地址
		
		folders: # 配置共享
		    - map: D:\wamp64\www # 本地路径
		      to: /home/vagrant/code # 虚拟机路径
		
		sites: # 配置站点，需同时修改hosts文件
		    - map: api.homestead.com 
		      to: /home/vagrant/code/54qj-api/public
		    - map: hyperf.test
		      to: /home/vagrant/code/hyperf-skeleton/bin
		
		databases:
		    - homestead
		
		# ports:
		#     - send: 50000
		#       to: 5000
		#     - send: 7777
		#       to: 777
		#       protocol: udp
		
		# blackfire:
		#     - id: foo
		#       token: bar
		#       client-id: foo
		#       client-token: bar
		
		# zray:
		#  If you've already freely registered Z-Ray, you can place the token here.
		#     - email: foo@bar.com
		#       token: foo
		#  Don't forget to ensure that you have 'zray: "true"' for your site.

8. 在homstead管理工具文件夹中执行`vagrant up`开启虚拟机（确保homestead管理工具需要的box版本于你下载的box版本一致，否则管理工具会重新下载box盒子,这会是一个漫长的等待)
9. 在homstead管理工具文件夹中执行`vagrant ssh`可以进入虚拟机


### 总结：windows安装homestead对于拥有一定虚拟机知识的同学来说并不复杂，需要注意的是盒子的版本与homestead管理工具的版本需要对应