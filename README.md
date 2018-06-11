# Runloop
性能优化,runloop解决tableView加载图片过大,滑动卡顿的问题
runloop:叫做运行循环,是一种机制
目的:
1.保证程序不退出,(原因是执行main函数之后通过runloop开启了一个主线程)
2.负责监听所有的事件,底层是runloop,监听之后包装成event分发给控制器
3.runloop还负责渲染UI,在一次循环中渲染UI;
4.runloop非常懒,做完一件事就睡觉,,处理事件时候再醒过来

runloop处理三种事情:source,Observer,Timer

runloop常用三种模式:
NSdefaultRunloopMode(runloop默认的模式)
UITrackRunloopMode(当UI交互时候runloop切换的模式,优先执行)
NSRunloopCommonModes  不是一种模式只是一种占位符,(NSdefaultRunloopMode模式和UItrack模式都执行,没有优先级了)

例如:如果在主线程中加入NStimer定时器,重复一秒执行,,,但是在界面中有一个滚动视图,滚动视图的时候定时器不会被执行,
这就是runloop优先执行了UITrackRunloopMode模式.


****************runloop解决tableview加载图片过大滑动卡顿的问题********************
如果tableView每个cell都加载很多并且图片过大的图片
会产生两种问题:
1,滑动内存过大
解决:   
        [[cell.contentView viewWithTag:i] removeFromSuperview]
        
        在cell的代理方法中删除view控件,，节约内存

2.由于渲染图片过多切过大,滑动时会发生卡顿的问题,这样可以用runloop来解决,来优化性能,,,,
具体方法: 手动的让一次runloop循环只渲染一张图片
 
         监听runloop循环，每次循环，加载一张！！

