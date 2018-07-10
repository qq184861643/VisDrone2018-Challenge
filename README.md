# VisDrone2018-Challenge
### Task 1: object detection in images challenge
A conclusion of partipating [ECCV 2018 workshop "Vision Meets Drone: A Challenge"](http://www.aiskyeye.com/).
参加ECCV 2018 workshop "Vision Meets Drone: A Challenge"的总结。<br>

花了两个多月的时间参加了这次比mo赛yu，虽然实际上除了坚持做完了，其实也并没有真的尽到全力，所以最后结果也并不是很好，但是比赛过程中也遇到了一些问题，总结了一些经验，还算是有收获的，所以在这里记录下来。

**1.** 遇到的最大的坑是一共两个月时间，前一个半月为了出结果更快选用了较小的输入大小，这样训练时间更短。结果最后要整合所有的refinement的时候发现，针对小输入所调整的参数大部分在大输入上都没有效果甚至有反效果，所以一个半月的时间起码有一半是白白浪费了。得出的教训是Faster RCNN里很多参数都和输入大小相关，必须要先确定输入大小再调整网络参数。目标检测的其他网络很可能也有相似的性质。<br>
**1.** Hyper parameters of Faster-RCNN are w.r.t the input size, so fix the input size first then tune the parameters otherwise the tuning is useless. Other networks in object detection field may have the similar attribute.<br>

**2.** 试了一些传得很牛逼的网络的结果，但是实际上并不work，比如说SEnet，可能是SEnet的特征层学习对于小目标的检测作用不大。然后还试了将底层特征1×1卷积之后和高层特征相加再做检测，实际上也不work，这至少说明特征层融合还是需要一些严格的设计的，不是随便加就有作用的。<br>
**2.** SEnet is not working for this task. Maybe it is not working for small objects detection. Casually concatenate low feature with higher feature then detecting is not working as well, maybe indicate that feature concatenation need carefully design.
