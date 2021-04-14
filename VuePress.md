## 安装配置

- 创建并进入一个新目录

```sh
mkdir vuepress-starter && cd vuepress-starter
```

- 使用包管理器进行初始化

```sh
yarn init -y
```

- 将 `VuePress` 安装为本地依赖

```sh
yarn add -D vuepress
```

- 在 `package.json` 中添加一些 `scripts`

```json
{
    "scripts": {
        "serve": "vuepress dev docs",
        "build": "vuepress build docs"
    }
}
```

- 创建 `docs` 目录和 `README.md` 文件

```sh
mkdir docs && echo 'Hello VuePress' > docs/README.md 
```

一下为 `docs/README.md ` 文件的配置内容，home页面

```markdown
---
home: true
heroImage: /img/logo.jpg
actionText: 快速上手 →
ationLink: /zh/guide/
features:
- title: 企业定位
  details: 整合医学产学研资源专注功效性类快消品品牌的医疗科技集团。
- title: 企业愿景
  details: 让每一个人都拥有健康的身体和美丽的肌肤。
- title: 企业使命
  details: 以科学方法论为指导，为人类拥有健康的身体和美丽的肌肤而奋斗不止。
footer: 北京安德普泰医疗科技有限公司© Beijing UnderProved medical technology co. LTD
---
```

- 在 `docs` 目录下创建 `.vuepress` 目录

```sh
cd docs && mkdir .vuepress
```

- 在 `.vuepress` 目录下创建 `config.js` 文件

```js
// /docs/.vuepress/config.js
module.exports = {
	title: '安德普泰',
	description: '北京安德普泰医疗科技有限公司',
	base: '/hr/',	// 打包后上传服务器的拼接路径，本地跑打包文件可改成 ./
	dest: './dist',	// 打包目录，默认在 .vuepress 目录下
	port: '3000',	// 端口号
	head: [
		['link', {rel: 'icon', href: '/logo.jpg'}]
	],
	markdown: {
		lineNumbers: true
	},
	themeConfig: {
		nav: require('./nav.js'),	// 将head配置抽离到同级目录下的 nav.js
		sidebar: require('./sidebar.js'),	// 将侧边栏配置抽离到同级目录下的 sidebar.js
		sidebarDepth: 2,
		lastUpdated: 'Last Updated',
		searchMaxSuggestoins: 10,
		serviceWorker: {
			updatePopup: {
				message: '有新的内容',
				buttonText: '更新'
			}
		},
		editLinks: true,
		editLinkText: '在 GitHub 上编辑此页'
	}
}
```

```js
// /docs/.vuepress/nav.js
module.exports = [
	{
		text: '员工手册',link: '/employee_manual/'
	},
	// {
	// 	text: '员工指南',link: '/employee_guide/',
	// 	items: [
	// 		{text: '初级指南',link: '/employee_guide/zero/'},
	// 		{text: '高级指南',link: '/employee_guide/high/'}
	// 	]
	// },{
	// 	text: '百度',link: 'https://www.baidu.com/',
	// 	items: [
	// 		{text: '初级指南',link: '/employee_guide/zero/'},
	// 		{text: '高级指南',link: '/employee_guide/high'}
	// 	]
	// }
]
```

```js
// /docs/.vuepress/sidebar.js
module.exports = {
	// 对多模块进行管控
	'/employee_manual/': require('../employee_manual/sidebar'),
	
	// '/employee_guide/zero/': require('../employee_guide/zero/sidebar'),
	// '/employee_guide/high/': require('../employee_guide/high/sidebar')
}
```

- 静态资源可以放在 `.vuepress/public` 目录中

- 根据 `sidebar.js` 文件中的配置信息创建对应的目录和文件

  + `/docs/employee_manual`

    - `notes` 目录，存放具体内容文件

      * `employee_manual.md`
      * `employee_manual_guide.md`

    - `README.md` 默认展示内容，每个配置文件夹都必须有

    - `sidebar.js` 当前文件夹的配置文件

      ```js
      module.exports = [
      	// {
      	// 		title: '新手指南',
      	// 		collapsable: true,	// 是否折叠
      	// 		children: [
      	// 			'/employee_manual/notes/employee_manual_guide'
      	// 		]
      	// 	},
      		{
      			title: '员工手册',
      			collapsable: true,
      			children: [
      				'/employee_manual/notes/employee_manual'
      			]
      		}
      ]
      ```



