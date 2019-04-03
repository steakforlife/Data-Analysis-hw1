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




