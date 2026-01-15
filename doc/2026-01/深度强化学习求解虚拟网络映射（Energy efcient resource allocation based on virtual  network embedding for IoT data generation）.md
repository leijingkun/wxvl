#  深度强化学习求解虚拟网络映射（Energy efcient resource allocation based on virtual  network embedding for IoT data generation）  
原创 豆豆
                    豆豆  豆豆咨询   2026-01-15 04:17  
  
    最近在复现一篇深度强化学习求解虚拟网络映射的文章，出现了很多的问题，现在对该论文进行分析、复现及主要的问题进行阐述。  
  
   文章如下：Tan L, Aldweesh A, Chen N, Wang J, Zhang J, Zhang Y, Kostromitin K I, and Zhang P. Energy efcient resource allocation based on virtual  network embedding for IoT data generation. Automatedd Software Engineering, 2024, 31:66.  
  
    该文章的创新点在于：  
  
    1）利用虚拟网络映射技术，为  
物联网数据生成提供节能型资源分配决策。物联网建模为多域物理网络，并以  
存储、计算和带宽为研究对象构建资源约束模型。  
  
    2）设计了一个  
四层结构的智能体，用于  
筛选满足数据生成需求的候选物联网节点与链路。该智能体  
基于奖励机制与梯度反向传播算法进行优化，旨在  
提升资源分配的长期收益、长期资源利用率及分配成功率。  
  
（  
有一些疑问：为什么题目是高效节能的虚拟网络映射，但是创新点却一个字都没有提到？而且全文并没有提到能耗的？）  
  
    作为一个深度强化学习的虚拟网络映射论文，还是要读读该篇文章。以下是该篇文章的设计与实验。  
  
一、模型设计  
  
1.1 状态  
  
    环境的状态描述了智能体运行的  
当前  
环境条件，包括不同的变量、特征或者属性。智能体通过观察环境的状态了解环境信息，从而采取行动。具体内容如下：  
  
  1）物联网节点的剩余存储资源：资源越大，则越有可能被使用；  
  
  2）物联网节点的剩余计算资源：  
资源越大，则越有可能被使用；  
  
  3）  
物联网节点相连的链路  
剩余  
带宽资源：  
资源越大越有可能被使用；  
  
  4）物联网节点的距离$Dis^I(n_i^I)$：  
$Dis^I(n_i^I)$  
越小  
，则更少的链路被使用，因此越有可能被使用；  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/lWpCB8WzM57IXJr6zhfeLvicK2QnwL04L78Uhtf9Uo1XXDZ6PNDcibjADficLicD44mibrjXJXwV3NIfFkQ6mxM0R4Q/640?wx_fmt=png&from=appmsg "")  
  
    
计算公式如下所示：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/lWpCB8WzM57IXJr6zhfeLvicK2QnwL04L0WdHm1M9rWTSPEoVo68shRy2FBDGnzrUIBpZkLQ7g0XSiaN55ogiaZlw/640?wx_fmt=png&from=appmsg "")  
  
其中，  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/lWpCB8WzM57IXJr6zhfeLvicK2QnwL04LMATafbjQVaxsA5xrKrtqwIEaH3RMQSLDXNc0lV7ynibJmq5BnYOqIFg/640?wx_fmt=png&from=appmsg "")  
  
该公式应该是$Dis^I{n_i^I}=\sum_{\forall{(i,j)\in{L^I}}}(||D^I{n_i^I}-D^I(n_j^I)||)^2/(1+  
\sum_{\forall{(i,j)\in{L^I}}}h(l_{i,j}^I)  
)$，即如下：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/lWpCB8WzM57IXJr6zhfeLvicK2QnwL04L6IHK6xeKt4RJsNUAJXhjibGEiaZT0Jh5bSTL54N5OAXZyvenzRwiaciaVQ/640?wx_fmt=png&from=appmsg "")  
  
状态矩阵如下公式(23)所示：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/lWpCB8WzM57IXJr6zhfeLvicK2QnwL04LJUTn4bzoK5CaV0saPNhoertrWzW2VHasBXCyEB5ra1tn5yuJmnTBsg/640?wx_fmt=png&from=appmsg "")  
  
  
1.2 智能体  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/lWpCB8WzM57IXJr6zhfeLvicK2QnwL04L3sExdFhRguKrsdmib3gBgSDP5jsgI907yICFmT9SAbMFz629xWWicUibQ/640?wx_fmt=png&from=appmsg "")  
  
    整合深度强化模型的智能体物联网架构如图3所示，其中智能体模型包括四层：(1)提取层Extraction layer、(2)卷积层Convolution layer、(3)概率层Probability layer、(4)过滤层Filtering layer。  
- **提取层**  
：根据环境信息提取状态矩阵，并将其作为智能体的输入，如公式（23）所示。  
  
- **卷积层**  
：利用卷积因子对状态进行操作，获取可用资源向量。  
  
- **概率层**  
：基于可用向量，通过Softmax函数计算各物联网节点为数据生成提供资源的被选概率。  
  
- **过滤层**  
：根据第3.3节中的约束条件，筛选出不符合相关要求的物联网节点与链路，最终确定候选节点与候选链路。  
  
（  
疑问：其中提取层和过滤层两者并没有可更新的参数，可不用设定为某个层。  
）  
  
      
智能体依据当前状态与数据生成需求预测资源分配行为，并将其转化为具体指令，通过软件定义网络控制器或虚拟基础设施管理器执行资源分配决策。与此同时，相应的物联网资源将被分配给用户，用户使用完毕后将释放资源。此外，在执行决策过程中，系统将收集环境奖励反馈，以实时学习并优化智能体策略。  
  
  
    需要说明的是，  
为鼓励探索行为，通过概率层的Softmax函数为策略引入随机性，将策略输出转换为概率分布。同时，  
通过温度参数控制探索与利用的平衡：较高的参数值可促进更广泛的探索，增加随机性；较低的参数值则倾向于利用已知的最大概率策略，即选择更可能带来高回报的动作，从而鼓励利用行为。  
  
  
      
在前馈过程中，智能体接收当前状态作为输入，并通过神经网络计算各动作的概率分布，该分布将用于选择下一动作；基于所选动作与环境的交互，系统将获取新的状态与奖励值。需要指出的是，由于探索过程中存在随机性，系统并不总是选择概率值最高的策略。  
在反向传播过程中，算法依据获得的奖励与状态转移信息计算梯度，并更新参数。参数更新过程通过梯度反向传播实现，从而调整智能体的动作策略以最大化期望奖励。  
  
  
1.3 动作  
  
    这是智能体对环境的响应。动作为离散动作，即节点资源分配的决策——选择哪些物理节点分配给请求。其具体表示如下：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/lWpCB8WzM57IXJr6zhfeLvicK2QnwL04Lb82bg7sr2Lc5sD1TwHep2AlK9AvMNDIwn8PArshbElZ4M7O0X6JXdg/640?wx_fmt=png&from=appmsg "")  
  
    物理链路的选择是在选定物理节点后，通过广度优先搜索候选物理链路实现的。  
  
1.4 奖励  
  
  
  
  
    奖励是指导智能体学习方向的机制。通过与环境的持续交互，智能体能够学习在不同状态下应采取何种动作以最大化预期奖励。在为物联网数据生成提供节能的资源分配决策，因此我们将资源利用率定义为奖励函数，如公式（19）所示。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/lWpCB8WzM57IXJr6zhfeLvicK2QnwL04L3ww6JInbXLdIQMoQcVJhxQQNU4WqBo2aDsRbfOkaLUtpujjTddD81A/640?wx_fmt=png&from=appmsg "")  
  
    结合梯度反向传播方法与奖励机制，可引导智能体的优化过程。其损失函数按以下形式进行迭代：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/lWpCB8WzM57IXJr6zhfeLvicK2QnwL04LibPuXBBRWURO1yqSBuAh0bQQ08cDhZrdU9iaHAybbuzX7HRpf1zWv3iag/640?wx_fmt=png&from=appmsg "")  
  
其中，\(\mathcal{L}\) 代表损失函数（需说明的是，采用交叉熵损失函数\textsuperscript{*}），\(\nabla\mathcal{L}\) 表示梯度，\(\mu\) 为学习率。若学习率取值过小，智能体的迭代步长将偏小，导致收敛速度过慢；若取值过大，迭代步长则会偏大，容易无法收敛。因此，本研究将 \(\mu\) 设定为 \(0.01\)。  
  
疑问：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/lWpCB8WzM56uOySyzL6Qsh3ciarIAWXAVaSfp2up5nRo7uA8BoN4ddg2m88HG5b1hVokMO9EAtNJ1ULYaceqlCg/640?wx_fmt=png&from=appmsg "")  
  
在标准的深度学习或强化学习中，这个写法在数学上是错误或不完整的。  
 它更像是**对梯度更新规则的一种描述**  
，而非定义损失函数本身。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/lWpCB8WzM56uOySyzL6Qsh3ciarIAWXAVBcccwbFicSRxbC8eRqjakia5juRrAzbyU0G33wAKQIgx1kyu5CF7HHXQ/640?wx_fmt=png&from=appmsg "")  
  
  
    此外需要特别说明的是，物联网场景中的网络环境与状态处于持续变化中。为此，在每个训练阶段重新提取瞬时的网络环境与状态，以更精确地模拟和预测网络行为，从而更好地优化资源分配策略。算法流程如算法1所示。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/lWpCB8WzM56kZAgCMOts5EINKykOO2mQ8ibFicrMGrZ1mJKIVoXdboiahlXWsG6La8Y58OeGRAOwvYh4OCbu7kVdA/640?wx_fmt=png&from=appmsg "")  
  
（  
有疑问：在动态变化环境中，$G^{V_i}$在batch data中，可能是当前时间窗会没有到来的用户请求，这样就会导致整个系统运行出问题。因此，我们可以设定batch data为一个用户请求  
）。  
  
（  
有疑问：  
前面有说是虚拟请求的所有节点都被映射之后再链路映射，但是算法1，我们再第5行看到有一个节点被映射，然后选择一个链路请求进行映射，同时计算回报、损失和更新参数，两者产生矛盾。  
这是一个非常严重的问题，算法的描述一定是出现问题了  
）。  
  
      
  
     为了解答上述两个疑问，猜测只有两个节点一条链路，静态映射过程才有可能实现上面的算法。  
  
  
二、仿真实验  
  
2.1 仿真环境  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/lWpCB8WzM56kZAgCMOts5EINKykOO2mQ8Xa3w8hM37e3nMFPpxxcqvSKMZDWfvicmGf1nl38KR9lg3IcfSsrmTA/640?wx_fmt=png&from=appmsg "")  
  
在这个参数表里面并没有看到生存时间，因此可以断定是静态映射。  
  
  
2.2 实验训练结果  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/lWpCB8WzM56kZAgCMOts5EINKykOO2mQceMy7fVUiaIAfloEqVURfFl3PRRdpPSZGlPznaYHBBMWwNS593AXwgg/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/lWpCB8WzM56kZAgCMOts5EINKykOO2mQmRY0Ft6XgaWSNibukib9McfvhpbGom5icKnqll726l6rFU7Ap4GLIYSOw/640?wx_fmt=png&from=appmsg "")  
  
2.3 实验测试结果  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/lWpCB8WzM56kZAgCMOts5EINKykOO2mQN9GIqwNhPcictG0SiaN4kMjK3NGneWGrITS3luMMvVX56smibtaWMroVA/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/lWpCB8WzM56kZAgCMOts5EINKykOO2mQ3exegLR61MA0ggr549zrPA37ws50VLJzY3UicTub7fanAGYdUoLW2mg/640?wx_fmt=png&from=appmsg "")  
  
    我们可以看到，接收率和系统收益都趋于下降，而且下降幅度非常大，因此该篇文章应该是静态映射。  
  
3. 总结  
  
    该篇文章阐述了一个深度强化学习的映射方法，虽然有一些缺陷，但是整体还是比较完整，而且是对数据生成请求进行资源分配，整体符合当前AI的发展趋势。  
  
  
  
