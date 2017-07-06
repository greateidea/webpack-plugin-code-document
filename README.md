# webpack-plugin-code-document
the document of webpack plugin code, learn how to write webpack plugin (CN).

webpack 描述如何写插件的文档并不完善，并且部分地方解释的并不清晰，同时需要阅读较多的源代码，
导致写一个webpack插件的门槛相当的高，这也是webpack的插件如此至少的重要原因
写这个README的是原因就是记录关于插件源代码的记录和理解，每一段事件记录一点。

根据官方介绍，先从 tapable 这个类开始，官方建议约得源代码中，compler 和 compilation 类扩展自它。
这里也只分部介绍，起一个引导的作用。

Tapable 有四组成员函数:

plugin(name:string, handler:function) 
 -注册一个事件，名字由name参数指定，回调函数由handler指定，如果名字是系统没有的，则这个事件是自定义的，反之是系统定义好的事件。
 当这个指定的事件触发则调用回掉函数，事件的触发可以由下面的 applyPlugins* 来直接触发。
 
apply(…pluginInstances: (AnyPlugin|function)[])
- AnyPlugin 是 AbstractPlugin 的子类，或者是一个有 apply 方法的类（或者，少数情况下是一个对象），或者只是一个有注册代码的函数。(官方原话)
这个函数的作用是直接向 tapable 实例中注册你写的插件类，这些插件类都有一个apply方法，也就是说这个你写的插件类就是官方说的需要在原型上写个apply方法的类。
猜测这些插件类会挨个被 apply(…pluginInstances: (AnyPlugin|function)[]) 这个东西调用一遍，加入实例中。

applyPlugins*(name:string, …)：
- 这个就是发射一个事件，事件名称由参数 name 指定，这个事件也即是系统已有的事件或者上面 plugin(name:string, handler:function) 参数指定的事件，
如果是你自定义的事件则触发对应的 handler。

