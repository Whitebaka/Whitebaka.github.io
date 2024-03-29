---
title: 胡言乱语零知识证明Page0
date: 2022-08-04 20:28:31
tags: 
cover: https://whitebaka-1301161068.cos.ap-nanjing.myqcloud.com/image/100174471_p0.jpg/1080
---
本文纯属胡编乱造，角色有ooc请勿代入。关于数学相关的东西也可能不够严谨，请批判阅读。
# 引子
我们先来看一个例子  
>斑鸠路加和七草日花争论谁才是最了解美琴的人。  
>七草日花宣称路加手上关于美琴的三围数据已经过时。路加对这一论述嗤之以鼻，“既然这么说，那告诉我现在的数据，不然你怎么证明呢？”  
>日花陷入沉思，“我要证明我确实知道这件事，但我不能把美琴小姐的数据交给那个人，该该该怎么办……”  
>“我早料到啦！”，不知道从哪里冒出来的制作人，“我给日花准备了一个特殊的证明场地！事务所有一条走廊上有两间房间，分别有一扇通往走廊的门，两个房间之间有一扇带密码锁的门，门的密码已经委托美琴改成了自己的三围，去那里就能证明咯。”  
>“从哪里冒出来的，还有让美琴小姐做那种事你是什么变态制作人……可就算你这么说要怎么做完全不明白啊。”  
>“简单，日花你随便进一间房间，路加就在走廊上看着。路加让你从哪一个房间的门出来，你就从哪一个房间的门出来，如果你确实知道密码，那对你来说这件事就是轻而易举，如果你不知道，那你可没办法伪装。”  
>三十分钟后，响应如流的日花叉着腰对路加，一脸得意。却见路加从房间A进去，过一会从房间B走了出来。“你只是证明了你知道，但我也知道，哪怕两个小时前美琴的身体有什么变化我也会注意到……！”  
>“变态啊……”
# 介绍
零知识证明(Zero-Knowledge Proof)是一套证明系统，证明者(prover)向验证者(verifier)提交一个陈述(Statement)，证明其是真的。  
并满足下列条件：  
1.  完备性。只要Statement正确，prover就能让verifier确信。
2.  可靠性。只要Statement错误，prover无论怎么作弊都无法让verifier相信。
3.  零知识性。证明过程仅揭露Statement是否正确而不泄露其他信息。    
  
对引子来说，我们把数据转换为了另一种表现，隐去了具体数据。事实上，零知识证明更多倾向于一种描述性的概念。理应有许多种构成零知识证明的形式。
# 交互式与非交互式
在引子的例子里，我们可以看到在证明的过程中，路加和日花需要都在场，一个发出验证请求，一个做出证明。我们称这种形式的零知识证明是交互式的，他有一个显而易见的问题，如果七草日花想向其他人也证明这件事的话，她将不得不将这个验证做很多遍！每一遍还要验证者在旁边看着。有没有一种更一劳永逸的办法呢？我们看一个例子。
>大崎甜花最近沉迷在数独游戏。  
>在向制作人宣称要连睡三周之后，甜花终于解出了一个超级难的数独。  
>“甜花，很厉害！”这样想着，甜花想向其他人分享这个喜悦。“但如果把答案告诉别人的话，别人就不能再做了，会损失好多乐趣……噫……な酱，该怎么办啊……”  
>“交给甘奈吧！制作人前段时间正好介绍了一个可以用得上的工具！”  
>“首先做3套数独的卡片吧，在数独的谜面格，正面向上放3张对应的数字卡，在求解格，背面向上放3张对应的数字卡。这样就好了！”  
>“可是这有什么用呢……？甜花，不想别人翻开卡片看答案……”  
>“不会的！我们请千雪来帮忙吧。千雪姐，甜花说她做出了超难的数独哦，就请千雪姐来验证一下吧！请随便从一行、一列、一个九宫格分别取出来一套卡片，分别洗开再展示哦。”  
>“呀，每一套卡都是1-9的一套呢，毕竟数独的规则就是这样吧。”  
>“是正确的数独才是这样的哦。让甜花重新摆好卡，再试一次吧。”  
>“还是正确的呢。甜花好厉害！”
>“にへへ～可是千雪姐，不会知道答案吗……！因为拿出来的时候洗过牌，所以顺序信息被洗掉了，就没法还原……”  
>“没错，甜花真聪明！所有人都知道数独的每一行每一列每一九宫格里都是1-9，但只有解开数独的人可以生成由谜面产生的数独！即便不透露答案，也可以证明甜花做出来了！而且只要提前做好了这样的卡片和取卡规则，无论谁来都可以验证，不再需要甜花的参与啦。”  

如此，我们便提出了一种非交互式的零知识证明。
# 结语
我们对零知识证明做了一个简单的阐述，但零知识证明有什么用，又要如何去构建一个合理的零知识证明系统，其实有许多文章已经涵盖。等未来有兴趣了，再做更多的讨论吧。