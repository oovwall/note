# Angular在线竞拍网站学习笔记

Angular 是一个用 HTML 和 JavaScript 或者一个可以编译成 JavaScript 的语言（例如 Dart 或者 TypeScript ），来构建客户端应用的框架。

我们是这样写 Angular 应用的：用 Angular 扩展语法编写 HTML 模板， 用组件类管理这些模板，用服务添加应用逻辑， 用模块打包发布组件与服务。
然后，我们通过引导根模块来启动该应用。 Angular 在浏览器中接管、展现应用的内容，并根据我们提供的操作指令响应用户的交互。

## Angular安装
```powershell
$ npm install @angular/cli -g
```

## Angular新建应用
```powershell
$ ng new auction
$ npm install
```
如果安装依赖包的情况下报以下错误，则运行`npm install @angular-devkit/core@0.0.28 --save-dev
`，安装`@angular-devkit/core@0.0.28`
```powershell
npm WARN @angular-devkit/schematics@0.0.51 requires a peer of @angular-devkit/core@0.0.28 but none was installed.
npm WARN @schematics/angular@0.1.16 requires a peer of @angular-devkit/core@0.0.28 but none was installed.
npm WARN optional SKIPPING OPTIONAL DEPENDENCY: fsevents@1.1.3 (node_modules\fsevents):
npm WARN notsup SKIPPING OPTIONAL DEPENDENCY: Unsupported platform for fsevents@1.1.3: wanted {"os":"darwin","arch":"any"} (current: {"os":"win32","arch":"x64"})
```

## 组件相关的概念
![Angular组件相关的概念](https://s1.ax2x.com/2018/01/23/sJPgN.png)

## 开发
### 开发思路（目的）
在Angular框架的设计目标中最主要的目标之一就是：帮助开发人员开发出可重用的组件。

### 将类库引入项目
#### 将类库安装到本地
```powershell
$ npm install jquery --save
$ npm install bootstrap --save
```

#### 引入
在.angular-cli.json的apps的属性中添加相应的值到css和script
**切记先引入jquery，再引入bootstrap**
```json
{
  "styles": [
    "styles.css",
    "../node_modules/bootstrap/dist/css/bootstrap.css"
  ],
  "scripts": [
    "../node_modules/jquery/dist/jquery.min.js",
    "../node_modules/bootstrap/dist/js/bootstrap.js"
  ]
}
```

#### 安装类型描述文件
让typescript认识jquery和bootstrap
```powershell
$ npm install @types/jquery @types/bootstrap --save-dev
```

### 开发
#### 创建组件
```powershell
$ ng g component navbar         # 创建导航组件
$ ng g component footer         # 创建底部组件
$ ng g component search         # 创建搜索组件
$ ng g component carousel       # 创建轮播组件
$ ng g component product        # 创建产品组件
$ ng g component stars          # 创建星级评分组件
```

#### 更改组件
更改组件的html代码

知识点：  
- *ngFor
- [class.***]="*VARIABLE*" 用属性绑定解决是否添加样式
- @Input 
    在父组件上引入组件时，用属性绑定值 <app-stars [rating]="product.rating"></app-stars>
    在组件中@Input，然后声明，即可用this.rating访问
    > FIXME: 此处不太明白@Input，需查官方文档了解清淅
```html
<i *ngFor="let star of stars" class="fas fa-star" [class.far]="star"></i>
```

```typescript
export class StarsComponent implements OnInit {

  @Input()            // 此处要在ts文件开始import Input，import {Component, Input, OnInit} from '@angular/core';
  private rating: Number;

  private stars: Boolean[];

  constructor() { }

  ngOnInit() {
    this.stars = [];

    for (let i = 1; i <= 5; i++) {
      this.stars.push(i > this.rating);
    }
  }

}
```

- 属性绑定 [*ATTRIBUTION*]="*VARIABLE*"
```html
<img class="w-100" [src]="imgUrl" alt="">
```

```typescript
export class ProductComponent implements OnInit {
  
  private imgUrl = 'https://source.unsplash.com/random/265x145';

}
```

#### 路由
**路由对象**

| 名称 | 简介 |
|----|----|
| Routes | 路由配置，保存着哪个URL对应展示哪个组件，以及以哪个RouterOutlet中展示组件 |
| RouterOutlet | 在html中标记路由内容呈现位置的占位符指令。 |
| Router | 负责在运行时执行路由的对象，可以通过调用其navigate()和navigateByUrl()方法来导航到一个指定的路由 |
| RouterLink | 在html中声明路由导航用的指令 |
| ActivatedRoute | 当前激活的路由的对象，保存着当前路由的信息，如路由地址，路由参数等。 |

##### 路由实例 router
新建router项目
```powershell
$ ng new router --routing
$ npm install
$ ng g component home               # 创建导航组件
$ ng g component product            # 创建产品组件
$ ng g component code404            # 创建404组件
```

**app-routing.module.ts**
```typescript
const routes: Routes = [
  {path: '', component: HomeComponent},
  {path: 'product', component: ProductComponent},
  {path: '**', component: Code404Component}
];
```

**app.component.html**
```html
<a [routerLink]="['/']">主页</a>
<a [routerLink]="['/product']">商品详情</a>
<input type="button" value="商品详情" (click)="toProductDetails()">
<router-outlet></router-outlet>
```


**app.component.ts**
```typescript
export class AppComponent {
  title = 'app';

  constructor(private router: Router) {

  }

  toProductDetails() {
    this.router.navigate(['/product']);
  }
}
```

##### 在路由时传递数据

        1. 在查询参数中传递数据
        /product?id=1&name=2   =>   ActivatedRoute.queryParams[id]
        
        2. 在路由路径中传递数据
        { path:/product/:id }   =>   /product/1   =>   ActivatedRoute.params[id]
        
        3. 在路由配置中传递数据
        {
          path: /product,
          component: ProductComponent,
          data: [{ isProd: true }]
        }                      =>   ActivatedRoute.data[0][isProd]

1. 在查询参数中传递数据

**app.component.html**
```html
<a [routerLink]="['/product']" [queryParams]="{ id: 1 }">商品详情</a>
```

商品详情组件里接收参数  
**product.component.ts**
```typescript
export class ProductComponent implements OnInit {

  private productId: Number;

  // 获得ActivatedRoute参数
  constructor(private routeInfo: ActivatedRoute) {  }

  ngOnInit() {
    this.productId = this.routeInfo.snapshot.queryParams['id'];
  }

}
```
**product.component.html**
```html
<p>
  商品id是：{{productId}}
</p>
```

2. 在路由路径(url)中传递数据
 - 修改路由中的path属性  
**app-routing.module.ts**
```typescript
const routes: Routes = [
  {path: '', component: HomeComponent},
  {path: 'product/:id', component: ProductComponent},
  {path: '**', component: Code404Component}
];
```

- 修改路由链接的参数来传递数据
**app.component.html**
```html
<a [routerLink]="['/product', 1]">商品详情</a>
```

商品详情组件里接收参数  
**product.component.ts**
```typescript
export class ProductComponent implements OnInit {
  ...

  ngOnInit() {
    // 参数订阅
    this.routeInfo.params.subscribe((params: Params) => this.productId = params['id']);
      
    // 参数快照（组件内部跳转不会改变组件内部变量）
    // this.productId = this.routeInfo.snapshot.params['id'];
  }

}
```

**app.component.ts**
```typescript
export class AppComponent {
  ...

  toProductDetails() {
    this.router.navigate(['/product', 2]);
  }
}
```

**product.component.html**
```html
<p>
  商品id是：{{productId}}
</p>
```

##### 重定向路由
**app-routing.module.ts**
```typescript
const routes: Routes = [
  {path: '', redirectTo: '/home', pathMatch: 'full'}
];
```

##### 子路由
新建两个新的组件
```powershell
$ ng g component productDesc
$ ng g component sellerInfo
```

配置路由
```typescript
const routes: Routes = [
  {path: 'product/:id', component: ProductComponent, children: [
      {path: '', component: ProductDescComponent},
      {path: 'seller/:id', component: SellerInfoComponent},
    ]}
];
```

##### 辅助路由
一个组件上有多个outlet
**概览**
```html
<a [routerLink]="['/home', { outlets: { aux: 'xxx' }}]">链接一</a>
<a [routerLink]="['/product', { outlets: { aux: 'yyy' }}]">链接二</a>
<router-outlet></router-outlet>
<router-outlet name="aux"></router-outlet>
```

```typescript
const routes: Routes = [
  {path: 'xxx', component: XxxComponent},
  {path: 'yyy', component: YyyComponent}
];
```

**回归到router实例**  
**思路**
1. 在app组件的模板上再定义一个插座来显示聊天面板。
2. 单独开发一个聊天室组件，只显示在定义的插座上。
3. 通过路由参数控制新插座是否显示聊天面板。
