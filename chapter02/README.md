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
  

# 类解析:  

  
# 公有函数：  
figure_2_1(): 通过numpy库函数创建服从均值为0，标准差为1正态分布的Action数据集，数据集大小为200x10
figure_2_2(): ε可选值分别为0,0.1,0.01，对应whole greedy和ε=0.1与0.01的ε-greedy方法；



