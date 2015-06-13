#[Angular 项目结构最佳实践](https://docs.google.com/document/d/1XXMvReO8-Awi1EZXAXS4PzDzdNvV6pGcuaF4Q9821Es/pub)

##建议:
	把使用angular-seed, yeoman/generator-angular, (google内部案例go/nghellostyle)工具生成的项目，按照一种惯例去开发应用程序会更有效率。

##动机:
	我们的demo应用目前都是按照功能性分组(比如所有的视图放在一块，所有的css放在一块等等)而不是按照结构性分组(比如视图所需要的元素放在一起，把通用的内容放在一起。	
	当在一些真实地较大规模的应用中使用时，通过合理地目录可以提高代码的可读性，并且应用的结构可以跟应用的架构设计保持一致。我们同样希望开发的应用更加简单，管理起来更加容易，并且有一个清晰的通用的结构。
	文章推荐按照功能性分组并且附加一些案例。

##为什么要分层
	我们想要在一个应用中创建一套通用的规则可以应用到每一个逻辑单元，开发者可以思考需要多少个逻辑单元，然后将这些规则应用上去。在大型应用开发中，合理地目录结构可以反应出应用程序的结构(比如可以清楚的看到有哪些模块)，使得开发起来更加容易。

##为什么要约定命名规则
	在angular项目代码中，我们发现了一些问题，比如：
+	不同用处的js文件却拥有相同的名字，比如一个Service和一个Filter，在不同的目录下面却有同样的名字，当我们用IDE开发时候，很难第一眼就能分辨出来，当然我们可以在名字中添加类型信息来解决此问题。
+	有许多种方式来管理命名的规范， 例如在单元测试中，我们可以根据我们写的内容来命名为,\*_test.js,\*.spec.js, 或者test/*.js.



#推荐的目录结构


##可递归的分层次结构
	
	可递归的结构，基本单元的顶层包括一个module(app.js), 一个controller(app-controller.js), 一个单元测试文件(app-controller_test.js),和一个HTML视图(index.html或者app.html), 一些样式文件(app.css),还有一些子目录包括directives, filters, services, protos, e2e tests.
	
##组件和模块
	我们可以把文件(通常是一些通用的，可以被复用的内容)放到"components"目录下面，或者一个有意义的子目录里面（主要是view元素或者route）：

####组件:
+ 	一个组件目录下面包含directives，services， filters和一些相关文件
+ 	通用文件(如图片，model，工具文件等), 可以放到组件下面(如 components/lib), 也可以存在组件外面。
+	组件可以有子组件目录，尽可能包含有意义的名字。
+	在一些情况下，组件可以包含module的定义。（请看"Module定义"章节）

####模块
+	在顶层（或者嵌套的层级）目录下面仅包含模板(html或css文件)，controllers和module的定义。
+	模块中不需要在包含子模块，child之类的，如果有继承关系，在UI使用中嵌套就可以了。
+	对一个简单地应用来说，一个controller就够了，甚至不需要模块目录，因为所有的定义都在顶层。
+	模块经常没有自己的modules，这是根据代码的复杂度来决定的。

####Module定义
+	通常angular.module('foo')'只会调用一次，其他module或者文件可以依赖它，但不能修改它。
+	Module可以在一个主要的module文件中定义，也可以定义在组件或者模块的目录中，这个要根据实际情况来定。

####命名规范

在[Google JavaScript Style Guide naming](http://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml)的基础上面做一些补充

+	包含在组件或者模块的文件，通过文件名应该可以知道它的意图，它的类型也应该作为文件名的一部分，例如一个datepicker的directive路径就应该为components/datepicker/datepicker-directive.js。
+	在目录Controllers，Directives， Services和Filters目录下面的文件的名称应该包含相应地controller,directive, service, filter(如foo-service.js)
+	文件名应该是由小写字母组成，并且要符合JS样式规范。HTML和css文件也应该是小写字母。
+	单元测试名称应该以_test.js结尾，比如foo-controller_test.js 和 bar-directive_test.js
+	我们推荐，局部模板名称应该是以html结尾比如something.html 而不是something.ng。


#例子

####简单地例子

因为这是一个非常简单地应用程序，所以只有一个directive和一个serivce。


``` 

sampleapp/                 此文件夹下面包含客户端app和单元测试文件       
	app.css
	app.js
	app-controller.js
  	app-controller_test.js
   	components/        module定义，services，directives，filters和相关文件
   	foo/                		以下文件为"foo"模块所需要的内容 .
		foo.js     		foo模块定义在这里.
		foo-directive.js
		foo-directive_test.js
		foo-service.js
		Ifoo-service_test.js        
   index.html                        
e2e-tests/                                e2e测试文件在app外面
sampleapp-backend/                        只要在同一级目录下就可以了，也可以使用其他命名，
											如backend/ 开发人员可以自行决定。

```

在这个例子当中，有一个directive和一个service。


```
sampleapp/ 
	app.css
	app.js                        
	app-controller.js
	app-controller_test.js
	components/
		bar/                                service bar 所需要的内容
			bar.js
			bar-service.js
			bar-service_test.js
 		foo/                                directive foo 所需要的内容
    		foo.js
    		foo-directive.js
    		foo-directive_test.js
	index.html  

```

这样的结构是被推荐的，但并不是规定。你也可以有一些其他格式，比如将directives和services提到sampleapp目录下面，我们需要区别对待大型和小型的应用，小型应用目录结构可以简单点。我们的目的是定义一个可以被实施并且针对自动化的规范。

###更加复杂的例子

对一个更加复杂的例子来说，比如像一个订票系统，需要用户和管理员登陆，他们包含相同的ui的片段和router，同时这两种用户又有一些特有的区域，我们想分开这些文件，将他们分给各自的组件中。

```
sampleapp/
    app.css
	app.js                                
	app-controller.js
	app-controller_test.js
	components/
		adminlogin/                                
        	adminlogin.css                styles only used by this component
			adminlogin.js              optional file for module definition
			adminlogin-directive.js                         
			adminlogin-directive_test.js        
        private-export-filter/
			private-export-filter.js
  			private-export-filter_test.js
		userlogin/
			somefilter.js
			somefilter_test.js
			userlogin.js
			userlogin.css                
			userlogin.html                
			userlogin-directive.js
			userlogin-directive_test.js
			userlogin-service.js
			userlogin-service_test.js                
	index.html
	subsection1/
		subsection1.js
		subsection1-controller.js
		subsection1-controller_test.js
		subsection1_test.js                         
		subsection1-1/                        
			subsection1-1.css
			subsection1-1.html
			subsection1-1.js
			subsection1-1-controller.js
			subsection1-1-controller_test.js
		subsection1-2/                        
	subsection2/
		subsection2.css
		subsection2.html
		subsection2.js
		subsection2-controller.js
		subsection2-controller_test.js
	subsection3/
		subsection3-1/
	etc...

```

###应用到public phonecat上

应用到真实地案例上面 [angular-phonecat](https://github.com/angular/angular-phonecat),之前是根据类型分组。

```
app/
	css/
		app.css
		bootstrap.css
	img/
		phones/
			nexus-s.0.jpg
			nexus-s.1.jpg
			nexus-s.2.jpg
			nexus-s.3.jpg
		glyphicons-halflings-white.png
		glyphicons-halflings.png
	js/
		app.js
		controllers.js
		directives.js                this file is empty, artifact from angular-seed
		filters.js
		services.js
	partials/
		phone-detail.html
		phone-list.html
	phones/
		nexus-s.json
		phones.json
	index-async.html
	index.html
test/
	e2e/
		runner.html
		scenarios.js
	unit/
		controllersSpec.js        
		directivesSpec.js
		filtersSpec.js
		servicesSpec.js
```

现在应用我们推荐的结构当中，看起来应该是这样的。

```
app/
	app.css                                        was app/css/app.css
	app.js                                        was app/js/app.js
	bootstrap.css                                was app/css/bootstrap.css
	components/
        checkmark-filter/
           checkmark-filter.js                was app/js/filters.js
           checkmark-filter_test.js                was test/unit/filtersSpec.js
		phonecat/
           	phonecat.js                        new file to contain the module routing
	   		phonecat-service.js                was app/js/services.js
           	phonecat-service_test.js                was test/unit/servicesSpec.js
	detail/
		phone-detail.html                        was app/partials/phone-detail.html
		phone-detail-controller.js        was app/js/controllers.js
		phone-detail-controller_test.js        was test/unit/controllersSpec.js
	index.html                                was app/index.html
	index-async.html                                was app/index-async.html
	list/
		phone-list.html                        was app/partials/phone-list.html
  		phone-list-controller.js                was app/js/controllers.js
	  	phone-list-controller_test.js        was test/unit/controllersSpec.js

app-phonedata/                                outside the scope of the app, just data.
	img/                                        might also be located inside components/
		phones/
			nexus-s.0.jpg
			nexus-s.1.jpg
			nexus-s.2.jpg
			nexus-s.3.jpg
		glyphicons-halflings-white.png
		glyphicons-halflings.png
	phones/
		nexus-s.json
		phones.json
e2e/
	runner.html
	scenarios.js
```
