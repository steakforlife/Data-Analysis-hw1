# Data-Analysis-hw1
## 第一小題 算出最多評論的次數與評分的平均值
### code
```
review_count=pd.DataFrame(pd_data,columns=['UserId','ProfileName','Score','Score count'])
review_count=review_count.sort_values(by=["ProfileName"])
review_count=review_count.groupby(['ProfileName','UserId']).agg({'Score count':'sum',
                                          'Score':'sum'})
review_count=review_count.sort_values(by=["Score count"],ascending=False)
review_count['Score mean']=review_count['Score']/review_count['Score count']

count_result=pd.DataFrame(review_count,columns=['Score count','Score mean'])
```
### result
![](https://i.imgur.com/tbKMeqI.png)

## 第二小題 繪出最多筆評論使用者的分數分布
### code
```
#print(pd_data.loc[pd_data['ProfileName']=='c2'])
reviewer_data=pd_data.loc[pd_data['ProfileName']=='c2']
reviewer_data=reviewer_data.groupby(['Score']).agg({'Score count':'sum'}).reset_index()

#plot reviewer data
sns.set(style='whitegrid')
tips=reviewer_data
ax=sns.barplot(x='Score',y='Score count',data=tips)
```
### result
![](https://i.imgur.com/YlmKEhL.png)

## 第三小題 轉換時間至日期並繪圖
### code
```
#No.3
#transfer time to date
#pd.to_datetime(1490195805, unit='s')
list(pd_data)
pd_data['date']=pd.to_datetime(pd_data['Time'],unit='s')
pd_data['year']=pd.DatetimeIndex(pd_data['date']).year
year_count=pd_data.groupby(['year']).agg({'Score count':'sum',}).reset_index()
#year_count

sns.set(style='whitegrid')
tips=year_count
ax=sns.barplot(x='year',y='Score count',data=tips)
```
### result
![](https://i.imgur.com/8IgZhF9.png)

## 第四小題 plot a heat map by applying seaborn
### code
```
heatmap_input=pd_data[['Id','HelpfulnessNumerator',
                    'HelpfulnessDenominator','Score','Time']].corr()

sns.heatmap(heatmap_input,xticklabels=heatmap_input.columns,
            yticklabels=heatmap_input.columns,annot=True)
```
### result 
![](https://i.imgur.com/1nwGFZP.png)

## 第五小題 plot the distribution of helpful percent (hist)
### code
```
help_percent=pd.DataFrame(pd_data,columns=['HelpfulnessNumerator',
                                            'HelpfulnessDenominator'])
#num=help_percent['HelpfulnessNumerator']
#den=help_percent['HelpfulnessDenominator']
help_percent['ratio']=np.where(help_percent['HelpfulnessDenominator']==0,-1,
                            help_percent['HelpfulnessNumerator']/help_percent['HelpfulnessDenominator'])

help_percent['ratio'].hist()
```
### result
![](https://i.imgur.com/nZeEcZI.png)

# Part2 FIFA球員的分析
## 1.分析球員慣用腳的比例
### code
```
prefer_foot=pd.DataFrame(fifa_data,columns=['Preferred Foot'])
prefer_foot['count']=pd.Series(np.ones(len(fifa_data)),index=fifa_data.index)
prefer_foot=prefer_foot.groupby(['Preferred Foot']).agg({'count':'sum'}).reset_index()
prefer_foot.index=['Left','Right']
pie_chart=prefer_foot.plot.pie(y='count',figsize=(5,5),autopct='%1.0f%%')
```
### result
有77%球員的慣用腳為右腳，慣用左腳的球員只有23%

![](https://i.imgur.com/9imAyIE.png)

## 2.各項數據之間的關係
### code
```
heatmap_input=sns.heatmap(fifa_data[['ID','Name','Overall','Wage',
'Preferred Foot','Nationality','Jersey Number']].corr(),annot=True,cmap='Greens')
heatmap_input
```
### result
可以發現Wage(工資)與能力值的相關性只有0.57，表示球員有不小的機會成為薪水小偷XD
![](https://i.imgur.com/P9trVTD.png)

## 3.球員最喜愛的球衣號碼
### code
```
favorite_jersey=pd.DataFrame(fifa_data,columns=['Jersey Number'])
favorite_jersey['count']=pd.Series(np.ones(len(fifa_data)),index=fifa_data.index)
favorite_jersey=favorite_jersey.groupby(['Jersey Number']).agg({'count':'sum'}).reset_index()
sort=favorite_jersey.sort_values(by=['count'],ascending=False).reset_index()
sort=sort[:10]

sns.set(style='whitegrid')
tips=sort
ax=sns.barplot(x='Jersey Number',y='count',data=sort)
```
### result
下表為最多人選擇的號碼前20名，令我意外的是第一名竟然是8號，而不是1號或是0號這種在籃球搶手的號碼

![](https://i.imgur.com/Pcqx4Tk.png)
![](https://i.imgur.com/8cLenVF.png)

## 4.國籍統計
### code
```
nationality=pd.DataFrame(fifa_data,columns=['Nationality'])
nationality['count']=pd.Series(np.ones(len(fifa_data)),index=fifa_data.index)
nationality=nationality.groupby(['Nationality']).agg({'count':'sum'})
nationality=nationality.sort_values(by=['count'],ascending=False)
nationality=nationality[:20]
pie_chart=nationality.plot.pie(x='Nationality',y='count',figsize=(10,10),autopct='%1.0f%%')
```
### result
統計在FIFA中人數前二十多的國家，前三名分別是英格蘭，德國以及西班牙，光是前五名的國家便占了超過總數40%
![](https://i.imgur.com/KF9Gdot.png)



