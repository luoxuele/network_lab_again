# 实验需求

1. 在 SW1 和 SW2 之间配置动态链路聚合，允许所有 VLAN 通过
2. 要求 SW1 和 SW2 之间的最大传输带宽为 2G



# 实验步骤

## 1. 设备基本配置

	<H3C>sys
	[H3C]sys S1
	[S1]save
	
	<H3C>sys
	[H3C]sysname S2
	[S2]save



## 2.配置动态链路聚合

    [S1]interface Bridge-Aggregation 1
    
    [S1]int g1/0/1
    [S1-GigabitEthernet1/0/1]port link-aggregation group 1
    [S1-GigabitEthernet1/0/1]int g1/0/2
    [S1-GigabitEthernet1/0/2]port link-aggregation group 1
    [S1-GigabitEthernet1/0/2]int g1/0/3
    [S1-GigabitEthernet1/0/3]port link-aggregation group 1
    
    [S2]interface Bridge-Aggregation 1
    
    [S2]int g1/0/1
    [S2-GigabitEthernet1/0/1]port link-aggregation group 1
    [S2-GigabitEthernet1/0/1]int g1/0/2
    [S2-GigabitEthernet1/0/2]port link-aggregation group 1
    [S2-GigabitEthernet1/0/2]int g1/0/3
    [S2-GigabitEthernet1/0/3]port link-aggregation group 1



## 3.在聚合口上配置动态聚合，端口类型为trunk,并放行所有vlan

    [S1-Bridge-Aggregation1]link-aggregation mode dynamic
    [S2-Bridge-Aggregation1]link-aggregation mode dynamic
    
    [S1]interface Bridge-Aggregation 1
    [S1-Bridge-Aggregation1]port link-type trunk
    [S1-Bridge-Aggregation1]port trunk permit vlan all
    
    [S2]interface Bridge-Aggregation 1
    [S2-Bridge-Aggregation1]port link-type trunk
    [S2-Bridge-Aggregation1]port trunk permit vlan all

## 4. 测试
    [S1]display link-aggregation verbose
    
    [S1]interface Bridge-Aggregation 1
    [S1-Bridge-Aggregation1]link-aggregation selected-port maximum 2
    [S2-Bridge-Aggregation1]link-aggregation selected-port maximum 2
    
    [S1]int g1/0/1
    [S1-GigabitEthernet1/0/1]shutdown
    [S1]display link-aggregation verbose
