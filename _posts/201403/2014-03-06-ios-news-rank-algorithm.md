---
layout: post   
title: 基于用户投票的排名算法:好文推荐（news.ios-wiki.com)        
categories: 
- iOS   
tags:     
- iOS
---    

###信息大爆炸

不知到大家有没有这个感受，每天的信息太多太杂，那如何从大量信息中找出最重要的内容呢？

###如何排序
比如 好文推荐([news.ios-wiki.com][1] 这个网站。网站本身有个需求：把信息按重要性依次排列，并且及时更新。上面的内容很可能只是提交者本人感兴趣的。那如何对这个网站的文章排名呢，

如何让那些大家都可能想看的信息排在前面呢？


简单按日期排序是不行的。


可以**基于用户的投票，让用户决定什么样的信息可以排在第一位**。


###基于用户投票的排名算法 

iOS News用的是Hacker News的一个排名算法。基于用户投票，每个帖子前面有个三角形，如果用户觉得文章不错，就可以投一票。同时也考虑了时间因素，新的帖子要比旧帖子更容易排到前面。

因此，投票算法要考虑 1）投票数 2）时间


正如之前的文章[][]中介绍的那样， Hacker News由Paul Graham用Arc实现的，其代码开源，位于：
[arclanguage.org][2]

其代码是这样实现的：

    ; Votes divided by the age in hours to the gravityth power.
    ; Would be interesting to scale gravity in a slider.
    
    (= gravity* 1.8 timebase* 120 front-threshold* 1 
       nourl-factor* .4 lightweight-factor* .3 )
    
    (def frontpage-rank (s (o scorefn realscore) (o gravity gravity*))
      (* (/ (let base (- (scorefn s) 1)
              (if (> base 0) (expt base .8) base))
            (expt (/ (+ (item-age s) timebase*) 60) gravity))
         (if (no (in s!type 'story 'poll))  1
             (blank s!url)                  nourl-factor*
             (lightweight s)                (min lightweight-factor* 
                                                 (contro-factor s))
                                            (contro-factor s))))
                                                     



上面的代码用Python实现如下：

    def calculate_score(votes, item_hour_age, gravity=1.8):
        return (votes - 1) / pow((item_hour_age+2), gravity)



翻译成数学公式就是：

Score = (P-1) / (T+2)^G

其中,
P 代表投票数，减1是为了去掉文章发表人本人的投票。
T 代表文章发表后到现在的时间差（以小时计算）
G 代表“重力因子“，默认值是1.8

####1 分数P的影响

![](http://amix.dk/uploads/score_24_hours.gif)

有三个帖子，得票分别为200票、60票和30票（减1后为199、59和29)，可以看到，投票数越高，分数越高，但是，随着时间的增加，24小时之后，不管他们分数有多高，都只能得到一个降低的分数了。


####2 重力因子G的影响

![](http://amix.dk/uploads/gravity_effects.gif)


三根曲线的其他参数都一样，G的值分别为1.5、1.8和2.0。G值越大，曲线越陡峭，排名下降越快。


<br>
[点此访问好文推荐（news.ios-wiki.com）][1]

<br/>

对这个算法的介绍

参见：[How Hacker News ranking algorithm works][3] 和 [基于用户投票的排名算法（一）：Delicious和Hacker News][4]




[1]:http://news.ios-wiki.com
[2]:arclanguage.org
[3]:http://amix.dk/blog/post/19574
[4]:http://www.ruanyifeng.com/blog/2012/02/ranking_algorithm_hacker_news.html