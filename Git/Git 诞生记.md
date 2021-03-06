Git 诞生记

你可能有过这样的经历：在 debug 的时候这里加一句，那里减一句，顺便改改参数，不一会你的程序就从一个 bug 增加到了无数个 bug 。最重要的是，你完全想不起来自己到底改了几个地方，原来的程序到底长什么样子了。经历过几次这样的痛苦，你学乖了，下次 debug 之前先把原文件备份一下——改成 program.c.bak 什么的，然后放开了胆子改。有时候修改的次数多了，就会出现 program_V1.c.bak， program_V2.c.bak …… 又有时候需要同时修改多个文件。而需要倒回到之前的版本的时候，又完全想不起来这些 V1, V2 到底改了哪些地方。坑爹呐！难道要老子手工查不成？难道老子还要给每个 version 写个描述文件？

程序员们应该都中枪了。

或者你有过这样的经历：写论文的时候这里改动一个词，那里改动一句话。改来改去发现还不如最初的版本……囧之余，你怎么办？Ctrl+Z 能救你几次？甚至，万一是第二天打开 Word 的时候后悔了，又怎么办？

学生党们应该都中枪了。

有没有办法解决问题？当然有。但是别着急，喝杯茶，我们慢慢聊。

1972年的时候，贝尔实验室的程序员们快被这个问题烦死了。可能纯粹出于提升工作舒适度，他们写出了史上第一个版本控制系统（Version Control System A.K.A. VCS)，取了个直白的名字叫 Source Code Control System，简称 SCCS。这个软件用 C 语言改写后，被收录在 AT&T 的系统中。由于太懒了，这群程序员们只写了 Unix 版本。

SCCS 的原理很简单，储存要监控的源文件，**当文件被修改时，它自动地为每次修改创建一个快照（Snapshot）**。你工作的时候，想切换到哪个版本，它就先取出源文件，再依次应用修改直到你要的那个版本为止。

SCCS 好用是好用，但有几个问题。首先，当你对文件作出多次、大量修改后，它的速度明显变慢了——对啊，它要从原文件开始依次应用修改嘛。然后，它内置在 AT&T 生产的系统中，不能跨平台啊，广大 Windows 用户很不开心啊。

这其中 Water.F.Tichy 估计是最不开心的一个。

Water 是一个教计算机的大学教授，学校里的电脑上有 AT&T 的系统，可是到家就没辙了，老爷子很不爽啊，这叫人家在家怎么刻苦呢？1982年，他一气之下，写出了改变历史的第二个被广泛应用的 VCS，取名 Revision Control System，也就是大家熟知的 RCS（谁熟知了？谁？）。

从名字上就能看出，老爷子雄心很大，不想让它仅仅成为程序员们的玩具。**RCS 开源，跨平台，一经推出即在全球……的程序员，和大学中流行。**它相比于 SCCS 更快。为什么？因为老爷子机智地换了一个想法，储存最近的文件作为源文件，对历史修改反向，并且创建快照（Snapshot）。对呀，正常人工作肯定是从最新的文件开始读取，出现问题后再回滚到历史文件，谁没事就从第一版开始改起啊， SCCS 那种做法也太二了吧？

于是 RCS 开始统治江湖。渐渐地，程序员们又不高兴了，说你这个 RCS 也不科学呀，你每次都只能监控单个文件，老爷子你写个论文什么的倒是还行，可是我们是要写整个项目的呀，我项目中每个文件是有关联的呀，你这个单一文件监控不好使呀。被 RCS 折磨了四年之后，程序员们终于忍不了了，不知是哪路豪杰在开源社区吼了一声，一呼百应，不一会儿（哪有那么快！），新的 VCS 诞生了——Concurrent Versions System（A.K.A. CVS）。

CVS 不能不说是革命性的成果，它不但支持了对整个项目进行监控，并且首次提出了**仓库（Repository）**的概念，更加不能忍的是，它明确地分成了**服务端和客户端**，把代码仓库放在服务器上，由客户端向服务端提交修改。这还是 1986 年啊同学们，我真想回滚到那个时候激动地告诉他们：小伙子们（为什么是小伙子们……）你们发明的这种概念叫**「云」**你们造吗？「云」在二十多年以后有多火你们造吗？

CVS 很好地支持了多用户多文件并行操作，按说这样一来所有人应该都满意了吧？当然不是，不然今天讲的就是 CVS 而不是 Git 了。它的问题是监控的对象是文件，而不是目录。乍听起来，觉得好像没什么呀。其实不然，仔细想想，如果在项目目录下再创建目录，这个子目录里面的文件并不会被监控；同理，添加一个新文件，这个新文件也不会被监控。

程序员们又不开心了。

终于到了21世纪，一个跨时代的 VCS 出现了—— Subversion（A.K.A. SVN）。它由 CollabNet 公司开发，并且在后来被纳入了 Apache 软件项目孵化器（Apache Incubator），成为其中 top-level 的产品。它不但**支持监控整个目录，而且首次支持了监控非文本文件。**这两个特性使它在 2001 年在全球范围内取代了 CVS，并且一直流行到今天。没错，现在许多软件公司用的版本控制系统还是 SVN。

咦，说好的 Git 呢？

这就不得不提到同时代的另一款 CVS —— BitKeeper。与 SVN 不同，它是一款商业软件，但提供了免费的社区版本（Community Version）。它最大的优点在于，分布式管理。在 SVN 中，服务端相当于代码中心，所有的代码都提交到这里。它两点不方便的地方在于，客户端要不断地和服务端进行交互以保证自己的代码是最新的，如果自己从一个比较旧的版本开始修改，就会出现问题。而且与服务端的交互要求网络连接，**不适合离线工作。**分布式管理的概念是每个仓库都是主仓库，当两个仓库版本不一致时，可以方便地查看冲突之处并加以修改。

使得 BitKeeper 如此出名的原因还在于，大名鼎鼎的 Linux Kernel 就存放在其免费的社区版本上。

2005年4月，BitMover（拥有 BitKeeper 的那家公司）突然宣布，停止 BitKeeper 的社区版本，你们这群不想交钱就用我们软件人都玩蛋去！好嘛，改变了人类历史的 Linux 就这样无家可归了。

**Linux 之父 Linus 看着自己的儿子被别人赶出了家门，愤怒值立刻飙升。他环绕四周看了看，觉得 SVN 什么的简直是屎，决不能允许亲儿子流落至此。两斤啤酒下肚，他冲到电脑前三天三夜不眠不休。**

##Bang！Git 诞生了。

什么？你说 BitKeeper ？玩蛋去吧。

来源：http://jianshu.io/p/ee89f9385ca4#
