## chapter1代码解读  
# tic_tac_toe.py主程序:  
if __name__ == '__main__':  
    train(int(1e5))  
    compete(int(1e3))  
    play()  
  
# 函数调用:  
main()  
  train()  
    Player()  
    Judger()  
    loop:  
      Judger::play()   
  	  Judger::alternate()  
  	  Judger::reset()  
  	  State()  
  	  Player::set_state()  
  	  State::print_state()  
  	  loop:  
  	    Player::act()  
  		State::next_state()  
  		State::hash()  
  		Player::set_state()  
  	Player::backup()  
  	Judger::reset()  
  	  Player::reset()  
    Player::save_policy()  
  compete()  
    Player()  
    Judger()  
    Player::load_policy()  
    loop:  
      Judger::play()  
  	Judger::reset()  
  Play()  
    loop:  
      HumanPlayer()  
      Player()  
      Judger()  
      Player::load_policy()  
      Judger::play()  
  
# 类解析:  
State:  
  __init__: State类初始化，创建棋盘矩阵data，初始化winner、结束标识end以及哈希值hash_val  
  hash：遍历矩阵data，为状态生成哈希值  
  is_end：判断游戏是否结束，更新winner及结束标识end  
  next_state：创建新的State，拷贝data到新的State，更新data中落子位置的值  
  print_state：打印棋盘  
Judger：  
  __init__: 拷贝输入的两个Player对象，初始化Player对象落子Symbol（-1,1）,创建当前State对象  
  reset：执行两个Player对象的reset()函数重置Player  
  alternate：以generator的方式封装两个Player，返回这个generator  
  play：调用reset重置两个Player，创建当前State对象并配置Player，交替切换Player，读取Player行为（ACT），更新  
  State（调用State::next_state()）并获取State哈希值，从state字典中提取State及结束标识更新当前状态对象及结束  
标识，更新Player状态，如有需要打印Player状态以及返回winner  
Player：  
  __init__: 创建字典estimations，拷贝输入步长step size，默认学习率0.1，创建状态列表，创建贪心列表，初始化  
  symbol为0  
  reset：重置状态列表及贪心列表为空  
  set_state: 状态列表中添加输入State，贪心列表添加True布尔量  
  set_symbol：拷贝输入symbol，根据哈希值遍历state字典，提取对应State及结束标识，若结束标识为True且State中  
winner与拷贝的symbol相同，设置以该哈希值对应的estimations字典值为1.0，若winner为0（平局），设置为0.5，  
否则设置为0，若结束标识为False，设置为0.5  
  backup：提取State列表中所有State对象的哈希值为新的列表，反序遍历State哈希值列表，计算相邻两个状态在价值  
表（estimation字典）中的差值，并更新到对应状态中V(s) = V(s) + α(V(s+1) - V(s+1))  
  act: 提取State列表中最后一个State对象，创建next_state列表及next_position列表，遍历棋盘，该状态下遍历到的  
位置所标示的值为0，添加该位置到next_positions中，next_state列表添加在该位置落子后的State哈希值，执行随机过程  
。若产生的随机数小于设置阈值，从next_positions中随机选择一个位置作为下一步落子位置，并将持有的Symbol加入到  
action列表中，贪心列表最后一个布尔量设为False，返回行为列表action。若产生的随机数大于设置阈值，组合next_states  
和next_positions为values列表，通过Numpy中随机数shuffle方法打乱values列表，再对values排列，将持有的Symbol加入  
到action列表中，返回行为列表actions。  
  save_policy：以.bin文件格式通过pickle库封装estimations字典  
  load_policy：从.bin文件中通过pickle库加载数据到estimations字典  
HumanPlayer：  
  __init__: 初始化symbol为None，初始化键列表，初始化State为None  
  reset：空实现  
  set_state：拷贝输入state  
  set_symbol：拷贝输入symbol  
  act：将输入键映射为棋盘上落子位置，返回位置及symbol  
  
# 公有函数：  
get_all_states_impl: 遍历矩阵data，弱遍历位置未落子，以输入symbol更新状态并生成新的哈希值，若状态字典中  
没有该哈希值，在state字典中添加该状态和结束标识，若结束标识为False，调用get_all_states_impl并将输入symbol  
取反  
get_all_states: 创建State，创建state字典，以State哈希值为键存储State及State结束标识，调用get_all_states_impl  
train：创建两个Player，以创建的Player对象作为输入创建Judger对象，进行epochs轮次迭代，调用Judger::play()，调  
用Judger::play()，调用Player::backup()更新状态及状态值，调用Judger::reset()重置Judger。迭代结束后，调用  
Player::save_policy()存储策略  
compete：创建两个Player，以创建的Player对象作为输入创建Judger对象，调用Player::load_policy()加载训练的决策  
迭代turns轮次，调用Judger::play()进行游戏，调用Judger::reset()重置Judger  
play：循环中创建一个HumanPlayer和一个Player，以创建的对象作为输入创建Judger对象，调用Player::load_policy()  
加载决策，调用Judger::play()  


