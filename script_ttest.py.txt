import pandas as pd

# load excel file
file_path = r'C:\1_Jinyu\Python\statistics_PLQY\plqy_2024_load.xlsx'
df = pd.read_excel(file_path, header=None)

df = df.dropna(how='all')  #delete empty line
df.columns = ['sample', 'group', 'value1', 'value2', 'value3', 'value4', 'value5']
print(df)

import numpy as np

# classify into two groups
exp_group = df[df['group'] == 'exp']
ctrl_group = df[df['group'] == 'ctrl']

print(f"exp group data:")
print(exp_group)
print(f"ctrl group data:")
print(ctrl_group)

# FOR Experimental group
exp_all_values = []
for i in range(1,6):
	exp_all_values.extend(exp_group[f'value{i}'].values)
exp_all_values = np.array(exp_all_values)

print(f"E_Mean:{exp_all_values.mean():.3f}±{exp_all_values.std(ddof=1):.3f}")

# FOR Control group
ctrl_all_values = []
for i in range(1,6):
	ctrl_all_values.extend(ctrl_group[f'value{i}'].values)
ctrl_all_values = np.array(ctrl_all_values)

print(f"C_Mean:{ctrl_all_values.mean():.3f}±{ctrl_all_values.std(ddof=1):.3f}")


import scipy.stats as stats
t_stat, p_value = stats.ttest_ind(exp_all_values,ctrl_all_values,equal_var=False,nan_policy='omit')
print(f"Two sample T-test:")
print(f"t-statistic = {t_stat:.3f}")
print(f"p-value = {p_value:.5f}")

#Significance Testing
alpha = 0.05
if p_value < alpha:
	print(f"satistically significant (p<{alpha}) - Reject null hypothesis")
else:
	print(f"Not statistically significant(p>{alpha}) - Fail to reject null hypothesis")

#CI function
def calculate_confidence_interval(data,confidence=0.95):
	a = np.array(data)
	n = len(a)
	mean = np.mean(a)
	std_err = stats.sem(a) #std_error is s / √n
	t_critical = stats.t.ppf((1 + confidence) / 2, n - 1) #find critical t-value
	margin_of_error = std_err * t_critical
	return mean, mean-margin_of_error, mean+margin_of_error

exp_CI = calculate_confidence_interval(exp_all_values)
ctrl_CI = calculate_confidence_interval(ctrl_all_values)

print(f"Exp Group CI:{exp_CI[1]:.3f} to {exp_CI[2]:.3f}")
print(f"Ctrl Group CI:{ctrl_CI[1]:.3f} to {ctrl_CI[2]:.3f}")


#Plotting and Visulization
import matplotlib.pyplot as plt

plot_data = [exp_all_values,ctrl_all_values]
x_pos = np.array([0.2,0.8])
fig = plt.figure(figsize=(15,10))

ax1 = plt.subplot(2,2,1)
box = ax1.boxplot(plot_data, positions=x_pos, widths=0.2, labels=['exp','ctrl'])
ax1.set_title('1-All Data Boxplot')
ax1.set_ylabel('PLQY(%)')
ax1.set_ylim(10,35)
ax1.set_xlim(0,1)

exp_mean,exp_std = exp_all_values.mean(), exp_all_values.std(ddof=1)
ctrl_mean,ctrl_std = ctrl_all_values.mean(),ctrl_all_values.std(ddof=1)
ax2 = plt.subplot(2,2,2)
group = ['exp','ctrl']
mean = [exp_mean,ctrl_mean]
std_err2 = [exp_std/np.sqrt(len(exp_all_values)),ctrl_std/np.sqrt(len(ctrl_all_values))]

bars = ax2.bar(x_pos,mean,yerr=std_err2,capsize=5,alpha=1,color=['#FF6B6B','#4ECDC4'],width=0.2)
ax2.set_title('2-Mean ± STD',fontsize=12)
ax2.set_ylim(10,35)
ax2.set_xticks(x_pos)
ax2.set_xticklabels(group)
ax2.set_xlim(0,1)
ax3 = plt.subplot(2,2,3)
ax3.scatter(x_pos, mean, color='#FF6B6B',s=200,marker='*',linewidth=3)
exp_MoE = exp_CI[2]-exp_mean
ctrl_MoE = ctrl_CI[2]-ctrl_mean
MoE = [exp_MoE,ctrl_MoE]
ax3.errorbar(x_pos,mean,yerr=MoE,fmt='none',color='black',linewidth=1)
ax3.set_xticks(x_pos)
ax3.set_xticklabels(group)
ax3.set_title('3-Scatter Plot with 95% CI')
ax3.set_ylabel('PLQY(%)')
ax3.set_ylim(10,35)
ax3.set_xlim(0,1)
ax6 = plt.subplot(2,2,4)
ax6.axis('off')
summary_text = f"""
Exp Group
Mean:{exp_all_values.mean():.3f}±{exp_all_values.std(ddof=1):.3f}
95% CI:[{exp_CI[1]:.3f} to {exp_CI[2]:.3f}]
Min&Max:[{exp_all_values.min():.2f},{exp_all_values.max():.2f}]

Ctrl Group
Mean:{ctrl_all_values.mean():.3f}±{ctrl_all_values.std(ddof=1):.3f}
95% CI:[{ctrl_CI[1]:.3f} to {ctrl_CI[2]:.3f}]
Min&Max:[{ctrl_all_values.min():.2f},{ctrl_all_values.max():.2f}]

t-statistics Results
t-statistic = {t_stat:.3f}
p-value = {p_value:.5f}
{'significant(p<0.05)' if p_value <0.05 else 'insignificant'}
"""
ax6.text(0.1,0.9,summary_text,fontsize=9,verticalalignment='top')
plt.show()