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

## wrong solution  
> new=pd.merge(salaries,teams)  
new.groupby(['yearID','teamID']).sum()  
⚠️ 如果是这样的话，后面的sum会把所有能相加的column都相加，但是这里问的是wins and total salaries,但是wins是不能相加的。  

## correct solution  
> totSalaries = salaries.groupby(['yearID','teamID'], as_index=False).sum()  
joined = pd.merge(totSalaries, teams, how="inner", on=['yearID', 'teamID'])  
joined.head()  
⚠️ 这里因为不能在merge之后group然后sum得到total salaries,所以可以先单独的做一个total salaries，即1b得到的表格，和teams表格进行inner合并，合并参考为'on' keyword.  
⚠️如果在合并之前，没有对as_index=False进行设定，或者没有在pivot_table里面注明将yearID和teamID放在columns上，那么这两者将会在index上，则与teams合并的时候就会产生错误。所以需要所有的key都放在columns上进行合并。  
⚠️一半默认的都是inner merge








 
