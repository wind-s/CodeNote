github篇：
    
    1. in
        关键字 in:name 项目名包含
        关键字 in:desciption 项目描述中包含
        关键字 in:readme 项目readme文件中包含

        组合使用：
            搜索项目名或者readme中包含秒杀的项目
            seckill in:name,readme
    2.查找
        公式
            xxx关键字 stars通配符。  :>  或者 :>=
            区间关键字   数字1..数字2
        查找star数大于等于5000的springboot项目
            springboot stars:>5000
        查找forks数大于500的springcloud项目
            springcloud forks:>500

        组合使用：
        查找fork在100到200之间并且stars数在80到100之间的springboot项目。
        springboot forks:100..200 stars:80..100

    3.awesome
        awesome redis  直接搜索redis相关的学习资料

    4.高亮显示一行代码
        地址+#L13
        https://github.com/codingXiaxw/seckill/blob/master/src/main/java/cn/codingxiaxw/dao/SeckillDao.java#L13

      高亮显示一段代码：
        地址+#L13-L23
        https://github.com/codingXiaxw/seckill/blob/master/src/main/java/cn/codingxiaxw/dao/SeckillDao.java#L13-L23

    5.项目内搜索
        找到项目，按下 T 就能项目内搜索文件

    6.搜索某个地区内的大佬
        北京地区java 方向的用户
        location:beijing language:java



