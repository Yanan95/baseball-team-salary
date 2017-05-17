# baseball-team-salary

## purpose  
a. Load in CSV files from the web.  
b. Create functions in python.  
C. Create plots and summary statistics for exploratory data analysis such as histograms, boxplots and scatter plots.  

## problem 1a  
Load in these CSV files from the Sean Lahman's Baseball Database. For this assignment, we will use the 'Salaries.csv' and 'Teams.csv' tables. Read these tables into a pandas DataFrame and show the head of each table.  

## my own solution    
> salary = pd.DataFrame.from_csv('Salaries.csv')  
> team = pd.DataFrame.from_csv('Teams.csv')  
> salary.head()  
> team.head()  

⚠️ head()函数最好加括号，不然会出现格式错乱的情况.  
⚠️ 这个方法是下载csv到本地，直接读取的。答案是直接从网页读取   
## example solution  
Use the requests, StringIO and zipfile modules to get from the web.    
> import requests  
> import StringIO  
> import zipfile  
> def getZIP(zipFileName):  
    r = requests.get(zipFileName).content  
    s = StringIO.StringIO(r)  
    zf = zipfile.ZipFile(s, 'r') # Read in a list of zipped files  
    return zf  
> url = 'http://seanlahman.com/files/database/lahman-csv_2014-02-14.zip'  
zf = getZIP(url)  
print zf.namelist()  
tablenames = zf.namelist()  
tablenames[tablenames.index('Salaries.csv')]  
salaries = pd.read_csv(zf.open(tablenames[tablenames.index('Salaries.csv')]))  
print "Number of rows: %i" % salaries.shape[0]  
salaries.head()  
 
⚠️ 如果我们只对dataframe其中某几个columns感兴趣，则利用中括号[]提取  
> teams=teams[['yearID', 'teamID', 'W']]  
## problem 1b   
Summarize the Salaries DataFrame to show the total salaries for each team for each year. Show the head of the new summarized DataFrame.  
这是一个条件筛选，利用groupby或者pivot table。  
⚠️条件筛选时看清楚，一定有个筛选条件和筛选主体。这里的salaries就是筛选主体，each year和each team就是筛选条件，而且这里是total salary即是grouping+aggregation的形式。 两个筛选条件既可以用groupby也可以用pivot table.  
## solution 1 groupby 
> salaries.groupby(['yearID','teamID']).sum()  
> ⚠️这里是两个筛选条件，所以groupby()里面是一个列表[]，表示两个元素  

## solution 2 pivot table
> salaries.pivot_table(salary, columns= ['yearID','teamID'],aggfunc=sum)  
⚠️pivot_table()里面选择columns还是index是根据你想把它俩放在index上还是column上，而与变化之前在什么位置没有关系。如果不加，默认放在index上  

## probelm 1c  
Merge the new summarized Salaries DataFrame and Teams DataFrame together to create a new DataFrame showing wins and total salaries for each team for each year year. Show the head of the new merged DataFrame.






 
