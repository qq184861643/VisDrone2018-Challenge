# VisDrone2018-Challenge
### Task 1: object detection in images challenge
A conclusion of partipating [ECCV 2018 workshop "Vision Meets Drone: A Challenge"](http://www.aiskyeye.com/).<br>
参加ECCV 2018 workshop "Vision Meets Drone: A Challenge"的总结。<br>

花了两个多月的时间参加了这次比mo赛yu，虽然实际上除了坚持做完了，其实也并没有真的尽到全力，所以最后结果也并不是很好，但是比赛过程中也遇到了一些问题，总结了一些经验，还算是有收获的，所以在这里记录下来。代码几乎照搬[jwyang的Faster R-CNN](https://github.com/jwyang/faster-rcnn.pytorch)<br>

**1.** 遇到的最大的坑是一共两个月时间，前一个半月为了出结果更快选用了较小的输入大小，这样训练时间更短。结果最后要整合所有的refinement的时候发现，针对小输入所调整的参数大部分在大输入上都没有效果甚至有反效果，所以一个半月的时间起码有一半是白白浪费了。得出的教训是Faster RCNN里很多参数都和输入大小相关，必须要先确定输入大小再调整网络参数。目标检测的其他网络很可能也有相似的性质。<br>
**1.** Hyper parameters of Faster-RCNN are w.r.t the input size, so fix the input size first then tune the parameters otherwise the tuning is useless. Other networks in object detection field may have the similar attribute.<br>

**2.** 试了一些传得很牛逼的网络的结果，但是实际上并不work，比如说SEnet，可能是SEnet的特征层学习对于小目标的检测作用不大。然后还试了将底层特征1×1卷积之后和高层特征相加再做检测，实际上也不work，这至少说明特征层融合还是需要一些严格的设计的，不是随便加就有作用的。<br>
**2.** SEnet is not working for this task. Maybe it is not working for small objects detection. Casually concatenate low feature with higher feature then detecting is not working as well, maybe indicate that feature concatenation need carefully design.<br>

**3.** 因为pytorch没有找到合用的FPN代码，自己的coding水平又不够不能从头撸个fpn出来，代码基本就用了素的Faster R-CNN，但是在实际测试中发现对anchor的大小和乘性放大系数进行调整之后对小目标的检测也能得到不错的结果。说明什么网络结构就要对应什么问题也不是绝对的，还是要具体问题具体测试具体分析。<br>
**3.** ‘this network architecture is good for this kind of problem so you should use it’ is not absolutely right, you should combine the solution with the task in practice and there is often not only one solution. On the other hand, no silver bullet right now.<br>

**4.** 调参调了一个半月的提升比不上一天数据扩增的效果。但是数据扩增也不能盲目进行，你还是需要实现对数据的分布做一定研究。我在对一批训练数据加入了高斯噪声之后，结果反而下降了。分析是高斯噪声对小目标的检测影响较大；或者这是我在实际操作中首次遇到了对抗攻击的实例。<br>
**4.** Data augmentation is useful but better not abuse it. I tried add some guassian noise to a small part of training data and the performace deceased dramatically. either the noise is too heavy for small object or I just made some adversarial examples that attacked the network by adding noise.<br>

**5.** 之前实验的时候发现训练时扩大训练图片的输入可以有效提高结果，到了提交的当天突发奇想：要是测试的时候把输入图片扩大到比输入图片还大会怎么样呢？然后一开始扩大了50%，结果直接从AP50--38.0+提升到了44.0+，之后扩大到一倍，AP50直接提到了48.0+，当然测试时间也翻了一倍多。这基本就是忽略了算法的精巧性靠运算力在莽了。这也是目前学术界很多学者对深度学习反思的一个点，这对于算法来说是一条健康的发展道路吗? 另外，高校的研究机构也就别想着跟外面的企业竞争了，人家专门搞算法的部分随便就是人均2块卡以上，学校的实验室好多还是几个人分一块卡，开发效率不可同日而语。而在学校就读的研究生，如果缺少设备的话，要么自己买，要么就得自己思考怎么去弥补这方面的劣势，或者换一条稍微曲折一点的路去走。<br>
**5.** Now deep learining in practice is abandoning the carefully designed way but head on the enormous computational power way as it can promote the result much more easily. I don't think this is good for the development of the industry.<br>

**6.** ddl的前半个小时才发现测试图片没有标注会导致测试代码运行不了，手忙脚乱的补假标注然后测试然后提交，搞完都超出ddl半个多小时了，还好侥幸提交上了不然两个月的成mo果yu一下付诸东流。。。实际上在调参过程中也发生过类似的事，有几组参数跑完没有详细记录后来自己忘了效果怎么样了，又得重新跑一次。我还是在比赛过程中太懈怠了，对于这种ddl要求明确的任务，还是务必得规划好时间，做好记录，提前一些时间就得把每个环节都试运行一遍看看是否有问题，以防临到头了又出问题。<br>
**6.** because I don't plan a explicit schedule at the first place and rehearse all the stages along to submitting the results ahead of deadline, I came across many unexpected problems and almost miss the submitting deadling, which taught me a very heavy lesson.
