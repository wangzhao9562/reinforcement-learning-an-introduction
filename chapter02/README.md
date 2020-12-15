## chapter2代码解读  
# ten_armed_testbed.py主程序:  
if __name__ == '__main__':
    figure_2_1()
    figure_2_2()
    figure_2_3()
    figure_2_4()
    figure_2_5()
    figure_2_6()
# 函数调用:  
main()
  figure2_1
    np::random::randn() 
    pyplot::violinplot()
	pyplot::savefig()
  figure2_2
    Bandit()
	simulate()
	pyplot::plot()
  figure2_3
    Bandit()
	simulate()
  figure2_4
    

# 类解析:  
Bandit: 
__init__：
初始化参数k_arm表示老虎机个数，epsilon为epsilon-greedy算法采用的epsilon值，initial为各action的初
始估计值，step_size为每个时间段采用的步长/学习率，sample_averages为True时表示采用动态步长/学习率，UCB_param不为None时，采用
UCB算法选择动作，gradient为True时，采用基于梯度的bandit算法，gradient_baseline为True时，在基于梯度的bandit算法中采用平均
奖励作为准线（baseline）
reset():
重置每个动作的实际Q值，重置每个动作的估计值，重置每个动作的选择次数，重置最佳动作选择，重置时间步数为0
act():
随机产生一个0-1之间的数，若产生的数小于Bandit的epsilon值，随机选择一个动作的采样（服从标准正态分布）返回
若UCB_param不为None时，计算UCB估计值，UCB_param作为UCB算法中决定估计值置信度的系数c，返回具有最大UCB估计值的动作的采样
若gradient为True时，根据Gibbs/Boltzman分布计算动作概率Pr{At=a}，作为np.random.choice的输入对动作编号进行采样并返回
step()：
输入动作号码（在向量中的下标），按N(实际奖励,1)分布产生一个奖励值，更新时间步长，更新动作选择次数，更新平均奖励
若sample_average为True，采用平均采样更新每个动作的估计值（Q =（R-Q）/ N(a)，学习率为1/N(a)）
若gradient为True，采样基于梯度方法更新动作估计值，若gradient_baseline为True采用奖励平均值作为准线的方法更新动作估计值
若gradient为False，基于固定学习率更新动作的估计值（固定step_size）
返回产生的奖励
simulate()：
执行bandit过程，输入runs为执行轮次，time为每轮次中时间步骤最大次数，bandit为老虎机数量
runs循环中重置当前老虎机状态（Bandit::reset()），执行time循环
time循环中依次调用Bandit::act()选择动作, Bandit::step()更新每个动作估计值，更新奖励总表及均值
  
# 公有函数：  
figure_2_1(): 通过numpy库函数创建服从均值为0，标准差为1正态分布的Action数据集，大小为200x10，绘制violin图显示10种Action
的分布
figure_2_2(): ε可选值分别为0,0.1,0.01，对应whole greedy和ε=0.1与0.01的ε-greedy方法；
figure_2_3(): 创建Bandit列表，添加两个估计值初值不同、epsilon不同的Bandit（初值一个为5.0，另一个为0.0，epsilon一个为0.0
另一个为0.1）
figure_2_4()：创建Bandit列表，添加两个epsilon不同的Bandit，且其中一个输入参数UCB_param为2
figure_2_5()：创建Bandit列表，添加四个固定步长为0.1或0.4，输入gradient_baseline为True或False的Bandit
figure_2_6()：对比固定步长、epsilon-greedy、UCB、greedy四种方法的RL效果


