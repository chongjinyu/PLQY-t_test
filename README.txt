AIM: Perform T-test to Two Group (Exp and Ctrl) and check if AgNP treatment can help enhancing PLQY.

1.Load value and calculate MEAN and STD
2.perform T-test
3.Find CI(confidence interval)

(Note: this t-test only compares the effect of two groups, however, the samples(same group but different preparation)may also varies. To test the effect between each samples, 'two-factor ANOVA' will be suggested;)


Below is the print results: 
--------------------------------------------------------------
       sample group  value1  value2  value3  value4  value5
1  sample1_exp   exp    25.0    25.4    28.3    27.8    29.4
2  sample2_exp   exp    30.5    21.5    24.3    20.2    25.6
3  sample3_exp   exp    18.1    18.2    17.9    19.2    18.3
5  sample4_ctr  ctrl    20.4    20.2    20.4    19.7    19.3
6  sample5_ctr  ctrl    18.0    19.6    18.2    19.9    19.0
7  sample6_ctr  ctrl    17.9    18.7    18.2    16.8    18.5
exp group data:
        sample group  value1  value2  value3  value4  value5
1  sample1_exp   exp    25.0    25.4    28.3    27.8    29.4
2  sample2_exp   exp    30.5    21.5    24.3    20.2    25.6
3  sample3_exp   exp    18.1    18.2    17.9    19.2    18.3
ctrl group data:
        sample group  value1  value2  value3  value4  value5
5  sample4_ctr  ctrl    20.4    20.2    20.4    19.7    19.3
6  sample5_ctr  ctrl    18.0    19.6    18.2    19.9    19.0
7  sample6_ctr  ctrl    17.9    18.7    18.2    16.8    18.5
E_Mean:23.313±4.510
C_Mean:18.987±1.062
Two sample T-test:
t-statistic = 3.617
p-value = 0.00241
satistically significant (p<0.05) - Reject null hypothesis
Exp Group CI:20.816 to 25.811
Ctrl Group CI:18.399 to 19.575
--------------------------------------------------------------