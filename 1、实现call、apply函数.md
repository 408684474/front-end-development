

const color = require('colors');
有一只狗，属性如下
//有一个函数可以获取调用对象的颜色
//那么，如何获取到狗的颜色？


let dog = {
	name: '二哈',
	color: '黑白'
}

function getProps(sex) {
	return this.name + ' , ' + sex + ' , ' + this.color;
}

//方法call,apply
console.log(getProps.call(dog)); //黑白
console.log(getProps.apply(dog)); //黑白

/*具体实现*/
/*1、对上述方法的理解，上述两种方法可以将函数绑定到第一个参数上,即改变函数中this的指向，使其指向第一个参数。
问题来了，我们知道this是函数内的一个变量，产生于函数运行期间，表示函数运行时的执行环境，那么又怎么去改变它呢？
其实改变的想法是不准确的，我们需要实现的就是对与给定变量，我们可以运行函数就是了，那么可以再变量内部去生成对应的函数，
然后在执行完毕后删除即可。
因此，实现call方法需要做如下工作：1) 给变量添加属性 2)执行属性
*/
//实现：

Function.prototype.myCall = function(params = window, ...args) {
	/*这里涉及到一个知识点。
	/定义函数只有一个参数，但实际中给函数传递多个参数,那么实参会作为一个类数组(arguments)传进来
	/其中argument[0]会进行值传递，赋值给变量params
	*/
	//对输入进行验证
	// var params = params || window; /*这里也有一个知识点。使用let params = params || window会报错,可以用params = window来做默认情况*/
	// console.log(params)
	//给对象添加属性
	params.newprop = this; //这里的this指mycall函数
	// let args = Array.prototype.slice.call(arguments, 1);//这里我们也可以使用...rest(剩余参数)类数组转换为数组
	// let args = [...arguments].slice(1);//这里使用
	//执行并返回
	return params.newprop(args);
}

console.log(getProps.myCall(dog, '男').red); //黑白