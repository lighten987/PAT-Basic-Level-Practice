# PAT-Basic-Level-Practice
/*
**1001**
任意自然数，偶数砍半，奇数*3-1再砍半，最后为一，求步数 

***思路：while 数不为1，判断奇偶，对应处理，计数count从0开始++，最后输出count***  
>#include<iostream>    
using namespace std; 	
int main()    
{  
	int n;  
	cin>>n;  
	int count = 0;  
	while(n!=1){  
		if(n%2==0)n/=2;  
		else n=(3*n+1)/2;  
		count++;  
	}   
	cout<<count;   
	return 0;  
}  
*/     
  
/*  
1002  
读入自然数，计算各数字之和，拼音写出每一位  

***思路：数字太大，首先考虑用char数组接收，再去计算和  每一位字符减去‘0’再自加  得到的数按低位放入新数组 ***  
      对于拼音写出，定义字符串数组即可char pinyin[][9]={"ling","yi","er","san","si","wu","liu","qi","ba","jiu"};  
>#include<iostream>  
using namespace std;  	
int main()  
{  
	string c;  
	cin>>c;  
	string pinyin[10] = {"ling","yi","er","san","si","wu","liu","qi","ba","jiu"};  
	int count = 0;  
	//计算总和   
	for(int i=0;i<c.size();i++)  
	{  
		count += c[i]-'0';  
	}  
	//从低位依次放入新数组  
	int a[100];  
	int i = 0;  
	while(count!=0)  
	{  
		a[i] = count%10;  
		count /= 10;  
		i++;    	
	}  
	for(int j = i-1;j>0;j--)  
	{  
		cout<<pinyin[a[j]]<<" ";  
	}  
	cout<<pinyin[a[0]];   
	return 0;  
}  
*/  
  
/*  
1003  
我要通过  字符串满足特定格式就通过，否则不通过  

***思路：p之前的A的个数*中间的A的个数=最后的A的个数!!!!!!!!!***  
>#include<iostream>  
#include<cstring>  
using namespace std;  
int main()  
{  
	int n;  
	cin>>n;    
	for(int i=0;i<n;i++)      
	{    
		char str[100];  
		scanf("%s",str);  
//		cin.getline(str,100);  
		int nump=0,numt=0,other=0,localp,localt;  
		int len=strlen(str);  
		for(int j=0;j<len;j++)  
		{  
			if(str[j]=='P')  //第一个p出现时，意味着前面都是A，且刚好个数即为下标数   
			{  
				nump++;  
				localp=j;  
			}  
			else if(str[j]=='T')  
			{  
				numt++;  
				localt=j;  
			}  
			else if(str[j]!='A')  
			other++;  
		}  
		if((nump!=1)||(numt!=1)||(other!=0)||(localt-localp<=1))//P和T的个数必须为一，没有其他字母，P和T中间至少有一个A    
		{  
			cout<<"NO"<<endl;  
			continue;  //此次循环结束，否则会输出两次NO     
		}  
		if(localp*(localt-localp-1)==(len-localt-1))  
		{  
			cout<<"YES"<<endl;  
		}  
		else cout<<"NO"<<endl;  
	}  	
	return 0;  
}  
*/  

/*
1004
读入n个学生的姓名学号成绩，输出成绩最高的和最低的对应姓名学号  

***思路：定义一个学生类，开辟存储空间并存入值，比较输出   
      类中函数变量 有初始化函数，读取成绩函数，成绩值变量为私有变量，只有函数调用 显示打印函数*** 

>#include<iostream>  
#include<cstring>  
using namespace std;  
int main()  
{  
class Student  
{  
	char Name[11];  
	char Number[11];  
	int Grade;  
public:  
	void Init(char *name,char *number,int grade)  
	{  
		strcpy(Name,name);  
		strcpy(Number,number);  
		Grade=grade;  	
	}  
	//比较成绩时调用此函数     
	int Show_Grade()  
	{  
		return Grade;  
	}  
	//显示信息函数     
	void Show()  
	{  
		cout<<Name<<" "<<Number<<endl;  	
	}   
};   
	int n;  
	cin>>n;  
	char name[11];  
	char number[11];  
	int grade;  
	Student student[n],max,min;  //不需要动态开辟内存空间     
	for(int i=0;i<n;i++)  
	{  
		cin>>name>>number>>grade;  
		student[i].Init(name,number,grade);   
	}  
	max=student[0];  
	for(int i=1;i<n;i++)  
	{  
		if(student[i].Show_Grade()>max.Show_Grade())  
		{  
			max=student[i];  
		}  
	}  
	max.Show();  
	min=student[0];  
	for(int i=1;i<n;i++)  
	{  
		if(student[i].Show_Grade()<min.Show_Grade())  
		{  
			min=student[i];  
		}  
	}  
	min.Show();   
	return 0;  
}  

//第二次代码  用vector做   
>#include<iostream>  
#include<vector>  
#include<algorithm>  
using namespace std;  
struct student  
{  
	string name;  
	string id;  
	int score;  
};  
bool cmp(student a,student b)  //想用sort()函数达到降序效果，cmp返回值为0 所以如果前面一个成绩低，就该返回0   
{  
	return a.score<b.score?0:1;   
}  
vector<student> v;  
int main()  
{  
	int n;  
	cin>>n;  
	for(int i=0; i<n; i++){  
	//	cin>>v[i].name>>v[i].id>>v[i].score;  //第一种读入方式  第二种，分别定义后再读入再赋值     
	    string name;  
	    string id;  
	    int score;  
	    cin>>name>>id>>score;  
	    v.push_back(student{name,id,score});  
	}  
	sort(v.begin(),v.end(),cmp);  
	vector<student>::iterator it;  
	it=v.begin();  
	cout<<(*it).name<<" "<<(*it).id<<endl;//这里用不用c_str()都可以  神奇  但是（*it）括号是必须的   
	it=v.end()-1;  
	cout<<(*it).name<<" "<<(*it).id<<endl;  
	return 0;  
}     
*/  
  
/*  
1005  
继续（3n+1）猜想 找出给出数中关键数，包含于其他的数  

***思路：每个关键数将路径放入路径数组，输入数与对应数在数组中的值比较，将路径数组中为一的对应值输出，即为关键数***  

//继续(3n+1)猜想 参看答案用vector做的，我想直接用数组做，一个数组存放输入数据，另一个数组带下标记录路径  
//第三个数组将数据作为第二数组下标查看只出现一次的，放入，降序排序输出即可    
//第二次代码     
>#include<iostream>  
#include<algorithm>  
using namespace std;  
bool cmp(int i,int j)  
{  
	return i>j;  
}  
int main()  
{  
	int n;  
	int shuzu[101] = {0};     
	int xiabiao[101] = {0};  
	int out[101] = {0};  
	cin>>n;  
	for(int i = 0; i< n; i++){  
		int temp;  
		cin>>temp;  
		shuzu[i]=temp;  
		xiabiao[temp]++;  
		while(temp != 1){  
			if(temp%2 == 0){  
				temp/=2;  
				if(temp<=100)  
				xiabiao[temp]++;  
			}  
			else{  
				temp=(temp*3+1)/2;  
				if(temp<=100)  
				xiabiao[temp]++;  
			}  
		}  
	}  
	int count=0;  
	for(int j = 0; j< n; j++){  
		if(xiabiao[shuzu[j]]==1){  
			out[count++] = shuzu[j];  
		}  
	}  
	sort(out,out+count,cmp);  
	cout<<out[0];  
	for(int j = 1;j<count;j++)cout<<" "<<out[j];  
	return 0;  
}  

//参考答案  
>#include <iostream>  
#include <vector>  
#include <algorithm>   
using namespace std;  
bool compare(int i, int j){   //sort()降序排列       
	return j<i;  
}  
int main() {  
	int n(0); //待验证整数个数  
	cin >> n;  
	vector<int> a(100);   //记录（3n+1)过程中出现每个数的次数   
	vector<int> b(n);   //保存每个待验证整数  
	vector<int>::iterator it;  
	for(it=b.begin();it!=b.end();it++){  
		int temp;  
		cin >> *it;  
		temp = *it;  
		a[temp]++;  
		if(temp==1){  
			a[temp]++;  
		}else{  
			while(temp!=1){  
				if(temp%2==0){  
					temp/=2;  
					if(temp<=100)    //超过100无意义，不需记录   
					a[temp]++;  
				}else{  
					temp=(3*temp+1)/2;  
					if(temp<=100)  
					a[temp]++;  
				}  
			}  
		}  
	}  
	sort(b.begin(),b.end(),compare);  //按降序排序  
	int count(0);  //记录关键数个数        
	vector<int> key(1);  // 保存关键数   
	for(it=b.begin();it!=b.end();it++){  
		if(a[*it]==1){  
			key[count++]=*it;  
			key.resize(count+1);  
		}   
	}   
	//按要求输出结果   
	for(it=key.begin();it!=key.end()-1;it++){  
		if(it!=key.end()-2){     
			cout << *it << " ";  
		}  
		else{  
			cout << *it;  
		}  
	}  
	return 0;  
}     
*/  
 
/*
1006  
换格式输出整数  

***思路：分别读取百十个位，用循环去显示对应字母***

>#include<iostream>  
using namespace std;  
int main()  
{  
	int n;  
	cin>>n;  
	int b,s,g;  
	b=n/100;  
	s=(n-b*100)/10;  
	g=n%10;   
	for(int i=0;i<b;i++)cout<<'B';  
	for(int i=0;i<s;i++)cout<<'S';  
	for(int i=1;i<=g;i++)cout<<i;  
	return 0;  
}  
*/  

/*  
1007  
素数对猜想  给定整数 在范围求出相邻奇数对的对数  

***思路：bool判定素数函数 在整数范围内用循环i遍历，i与i+2都为素数 count计数    
      为了实现程序时间较快  判断素数程序用cmath头文件包含的sqrt()开根号函数***
>#include<iostream>   
#include<math.h>   
using namespace std;   
bool IsLeap(int n)   //判断素数程序     
{  		
	for(int i=2;i<=sqrt(n);i++)  
	{  
		if(n%i==0)return false;	   		
	}  
	return true;  
}  
int main()  
{  
	int n,j=0;  
	cin>>n;  
	for(int i=3;i<=n-2;i++){  
		if(IsLeap(i)&&IsLeap(i+2))  
		{  
			j++;  
		}  
	}  
	cout<<j;       
	return 0;  
}   
*/  

/*  
1008  
数组元素循环右移问题  

***思路：将整体右移m位（初始定义数组长度保证足够）将多位移至开头空位   
      注意，开头空位获得溢出位值时，用取余去等值   
      感觉上只能用指针操作啊！！！ ***   
>#include<iostream>  
using namespace std;  
int main()  
{  
	int a[200];  
	int m;  
	int count;  
	cin>>count>>m;    
	//数组封装完成   
	for(int i=0;i<count;i++)  
	{  
		cin>>a[i];  
	}  
	int *p=a;  
	if(m==0)  
	{  
	  for(int i=0;i<count-1;i++)  
	{  
		cout<<a[i]<<" ";  
	}  
	cout<<a[count-1];  
	}  
	else{  
	//共count个整体后移m位   
	int k=m,v=count;  
	for(;v>=0;v--)  
	{  
		*(p+v+m)=*(p+v);  			 
	}  	
	//前m个   
	for(int j=count;j<count+m;j++)  
	{  
		*(p+j%count)=*(p+j);  			 
	}  
	for(int i=0;i<count-1;i++)  
	{  
		cout<<a[i]<<" ";  
	}  
    cout<<a[count-1];  
	}  
	return 0;  
}   
*/   

/*  
1009   
说反话  给出一串单词 逆序输出单词，单词顺序不变  

***思路：字符数组输入，遇到" "(空格)切分成字符串，用vector读取单词，逆序输出***   
>#include<iostream>    
#include<vector>   
#include<cstring>  
using namespace std;  
int main()  
{  
	vector<string>v;  
	char s[81];  //长度设为80会部分报错！！   
	cin.getline(s,81);  
	int len=strlen(s);  
	string danci="";  
	for(int i=0;i<len;i++)  
	{  
		if(s[i]=='\0')  
		{  
			break;  
		}  
		if(s[i]!=' ')  
		{  
			danci+=s[i];   //全场最骚！！！   
		}  
		else{  
			v.push_back(danci);  
			danci="";  
		}  	
	}  
	v.push_back(danci);  //break出来时还有最后一个单词  
	for(int i=v.size()-1;i>=0;i--)   
    {   
        if(i!=0)  
            cout<<v[i]<<" ";  
        else  
            cout<<v[i];  
    }   
	return 0;  
}  

//第二次代码  
>#include<iostream>    
#include<vector>  
#include<cstring>  
using namespace std;  
vector<string>v;  
int main()  
{  
	char c[81];  
	cin.getline(c,81);  
	int len = strlen(c);  
	string danci = "";  
	for(int i=0;i<len;i++){  
		if(c[i] == '\0'){  
			break;  
		}  
		else if(c[i] != ' '){  //字符 ''   
			danci+=c[i];  
		}  
		else{  
			v.push_back(danci);  
			danci = "";  
		}  
	}  
	v.push_back(danci);  
	for(int j= v.size()-1;j>0;j--)cout<<v[j]<<" ";  
	cout<<v[0];  
	return 0;  
}     
*/  

/*  
1010  
一元多项式求导  将系数与指数告知，输出求导后系数与指数  

***思路：对于一串数字的读取，未知长度，用vector向量***   

//我的答案  部分正确   
>#include<iostream>  
#include<vector>  
using namespace std;  
int main()  
{  
	vector<int>v;  
	int n;  
	while(cin>>n)  
	{  
		v.push_back(n);  
	}   
	if(!v.empty())  
	{  
		for(int i=1;i<=v.size();i+=2)  
		{  
			if(v[i]>1)  
			{  
				v[i-1]*=v[i];  
				v[i]--;  
				cout<<v[i-1]<<" "<<v[i]<<" ";  
			}  
			if(v[i]==1)  
			{  
				cout<<v[i-1]<<" "<<"0";  
			}  
			if(v[i]==0)  
			{  
				cout<<"0 0";  
			}  
		}  	
	}  
	else  
	{  
		cout<<"0 0";  
	}  
	return 0;  
}  
//参考答案  
>#include<iostream>    
#include<vector>    
using namespace std;    
int main()  
{  
	vector<int> v;  
	int n;  
	while(cin>>n)  
		v.push_back(n);  
	if(!v.empty())  
	{  
		for(int i = 0;i<v.size();i=i+2)  
		{  
			v[i] *= v[i+1];  
			v[i+1]--;  
			if(v[i]!=0)  
			{  
				if(i !=0 )  
					cout<< " ";  
				cout<<v[i]<<" "<<v[i+1];  
			}  
			else  
			{  
				if(v.size()==2)  
					cout<<"0 0";  
			}  
		}  
	}  
	else  
		cout<<"0 0";  
	return 0;  
}  
*/  

/*  
1011  
A+B与C判断大小  

***思路：long double试试 放入二维数组***   

>#include<iostream>    
using namespace std;  
int main()  
{  
	long double a[10][4];  
	int n;   
	cin>>n;   
	for(int i=0;i<n;i++)  
	{  
		for(int k=0;k<3;k++)  
		{  
			cin>>a[i][k];  
		}  
	}  
	for(int i=0;i<n;i++)  
	{  
		if(a[i][0]+a[i][1]>a[i][2])  
		{  
			cout<<"Case #"<<i+1<<": true"<<endl;  
		}  
		else cout<<"Case #"<<i+1<<": false"<<endl;  
	}  
	return 0;  
}  
*/  
  
/*  
1012   有一个答案不对  最后一个测试点是A2返回值为计算出来的0，却被判否   
数字分类 将一连串数字按要求分别得出结果并对应显示  

***思路：调用函数完成各个模块，如时间不够再思考***  
>#include<iostream>  
#include<cstring>  
#include<iomanip>  //控制输出位数 cout<<"a="<<fixed<<setprecision(n)<<a<<endl;  小数点后n位输出，且不够会补零   
#include<math.h>   //用到pow()，实现交叉求和   
using namespace std;  
//A1输出函数   
int A1(int *a,int n)  
{  
	int sum=0;  
	for(int i=0;i<n;i++)  
	{  
		if(a[i]%5==0&&a[i]%2==0)  
		{  
		sum+=a[i];  	
		}  
	}  
	return sum;  
}    
//A2输出函数  
int A2(int *a,int n)  
{  
	int sum=0,k=1;  
	for(int i=0;i<n;i++)  
	{  
		if(a[i]%5==1)  
		{  
		sum+=a[i]*pow(-1,k+1);  
		k++;	  
		}  
	}  
	return sum;  	
}  
//A3输出函数  
int A3(int *a,int n)  
{  
	int sum=0;  
		for(int i=0;i<n;i++)  
	{  
		if(a[i]%5==2)  
		{  
		sum++;	  
		}  
	}  
	return sum;  	
}   
//A4输出函数  
float A4(int *a,int n)  
{  
	float sum=0.0;int k=0;  
		for(int i=0;i<n;i++)  
	{  
		if(a[i]%5==3)  
		{  
		sum+=a[i];  
		k++;  	
		}  
	}  
	if(k!=0)  
	return sum/k;  
	else return 0;  	
}  
//A5输出函数  
int A5(int *a,int n)  
{  
	int sum=0,max=0;  
		for(int i=0;i<n;i++)  
	{  
		if(a[i]%5==4)  
		{  
		sum=a[i];  
		if(max<sum)  
		{  
			max=sum;  
		}  	
		}  
	}  
	return max;  	 
}    
int main()  
{  
	int n;  
	cin>>n;  
	int shuzu[1002];  
	for(int i=0;i<n;i++)  
	{  
		cin>>shuzu[i];  
	}  
	//读数并放入数组完成   
	if(A1(shuzu,n)!=0)  
	{  
		cout<<A1(shuzu,n)<<" ";  
	}  
	else cout<<"N"<<" ";  
		cout<<A2(shuzu,n)<<" ";  
	if(A3(shuzu,n)!=0)  
	{  
		cout<<A3(shuzu,n)<<" ";  
	}  
	else cout<<"N"<<" ";   
	if(A4(shuzu,n)!=0)  
	{  
	    printf("%.1f ",A4(shuzu,n));  
//		cout<<fixed<<setprecision(1)<<A4(shuzu,n)<<" ";	  	
	}  
	else cout<<"N"<<" ";  
	if(A5(shuzu,n)!=0)  
	{  
		cout<<A5(shuzu,n);  
	}  
	else cout<<"N";   
	return 0;  
}  
//标准答案  
>#include<iostream>  
int main()  
{  
    int i,n,a[1001],sum1,sum2,count1,count2;  
    scanf("%d",&n);  
    sum1=sum2=count1=count2=0;  
    double avg,sum3=0;  
    int flag=1,max=0,t=0;  
  for(i=1;i<=n;i++)  
  {  
      scanf("%d",&a[i]);  
      if(a[i]%5==0&&a[i]%2==0)  
        sum1+=a[i];  
      if(a[i]%5==1){  
        sum2+=a[i]*flag;  
        flag=flag*-1;  
        t++;  
      }  
      if(a[i]%5==2)  
        count1++;  
      if(a[i]%5==3){  
        sum3+=a[i];  
        count2++;  
        }  
      if(a[i]%5==4)  
      {  
          if(a[i]>max)  
            max=a[i];  
      }  
  }  
  avg=sum3/count2;  
  if(sum1==0)  
    printf("N ");  
  else  
    printf("%d ",sum1);  
  if(t==0)  
    printf("N ");  
  else  
    printf("%d ",sum2);  
  if(count1==0)   
    printf("N ");  
  else  
    printf("%d ",count1);  
  if(count2==0)  
    printf("N ");  
  else  
    printf("%.1lf ",avg);  
  if(max==0)  
    printf("N");  
  else  
    printf("%d",max);  
  return 0;  
}  
 
//第二次代码  
>#include<iostream>  
using namespace std;  
int main()  
{  
 	int N;  
	cin>>N;  
	int flag1 = 0, flag2 = 0, flag3 = 0, flag4 = 0, flag5 = 0;  //标志位  
	int out1 = 0,out2 = 0, count3 = 0, count4 = 0, max = 0;  
	int fflag = 1;  
	double out4=0.0;  
	for(int i=0; i<N; i++){  
		int temp;  
		cin>>temp;  
		if(temp%5 == 0){  
			if(temp%2 == 0){  
				flag1 = 1;  
				out1 += temp;  
			}  
		}  
		else if(temp%5 == 1){  
				flag2 = 1;  
				out2  += temp*fflag;   // 函数pow(-1,k)  
				fflag = fflag*(-1);  
		}  
		else if(temp%5 == 2){  
				flag3 = 1;  
				count3++;  
		}  
		else if(temp%5 == 3){  
				flag4 = 1;  
				count4++;  
				out4 += temp;   
		}  
		else if(temp%5 == 4){  
				flag5 = 1;  
				if(temp>max) max = temp;  
		}  	
	}  
	if(flag1 == 0)cout<<"N ";   //if条件语句中一定记得 ==   
	else cout<<out1<<" ";  
	if(flag2 == 0)cout<<"N ";  
	else cout<<out2<<" ";  
	if(flag3 == 0)cout<<"N ";  
	else cout<<count3<<" ";
	if(flag4 == 0)cout<<"N ";  
	else printf("%.1lf ",out4/count4);  
	if(flag5 == 0)cout<<"N";  
	else cout<<max;  
	return 0;  
}     
*/   

/*
1013  
数素数  给定素数个数的上下限范围，按格式输出中间的素数值  

***思路：在范围内循环遍历，用判断素数函数决定是否输出，编号取余决定输出空格还是换行***
//我的答案，输出答案部分格式错误，找不到问题   
>#include<iostream>  
#include<math.h>  
using namespace std;  
bool IsLeap(int n)   //判断素数程序   
{		  
	for(int i=2;i<=sqrt(n);i++)  
	{  
		if(n%i==0)return false;	  		
	}  
	return true;  
}  
int main()  
{  
	int low,high;  
	cin>>low>>high;  
	int k=0;  
	if(low<=high){  
	int sum=2;  
	for(int i=1;i<=high;sum++)  
	{  
		if(IsLeap(sum))  
		{  
			if(i>=low&&i<=high)  
			{  
				k++;  
				cout<<sum;  
				if(k%10!=0)  
				{  
					cout<<" ";  
				}  
				else cout<<endl;  
			}  
			i++;  
		}  
	 }   
	}  
	else cout<<"0";  
	return 0;  
}  

//参考答案  
>#include<stdio.h>  
int isprime(int n);  
int main() 
{  
  int i,a,b,x=1,sum=2;  
  scanf("%d %d",&a,&b);  
  if (a<=b)  
 {  
  for (;x<=b;sum++)  
  {  
    if (isprime(sum))  
    {  
      if (x>=a&&x<=b)   
      {  
      printf("%d",sum);  
      if ((x-a+1)%10==0&&x!=a&&x!=b)  
        printf("\n");  
        else  
        {  
          if (x!=b&&(x-a+1)%10!=0)  
          printf(" ");  
        }  
        }  
        x++;  
    }  
  }  
  }  
  else  
  {  
    printf("0");  
  }  
  return 0;  
}  
int isprime(int n)  
{  
  int i;  
    for(i=2;i<=sqrt(n);i++)  
    {  
      if(n%i == 0)  
      return 0;  
    }  
  return 1;  
}   
//第二次代码  
>#include<iostream>  
#include<cmath>  
using namespace std;  
bool IsLeap(int n)  
{  
	for(int i=2; i<=sqrt(n); i++){  
		if(n%i == 0)return 0;  
	}  
	return 1;  
}   
int main()  
{  
	int start,end;  
	cin>>start>>end;  
	int count = 0;  
	int k = 1;  
	int temp = 2;  
	while(count<end-1){	  
		if(IsLeap(temp)){  
			count++;  
			if(count>=start){  
				if(k%10 != 0){  
					cout<<temp<<" ";  
					k++;  
				}    
				else{  
					cout<<temp<<endl;  
					k++;  
				}  
			}  		
		}  
		temp++;  
	}  
	for(int i=temp;;i++){  
		if(IsLeap(i)){  
			cout<<i;  
			break;  
		}  
	}  
	return 0;  
}    
*/

/*
1014  
福尔摩斯的约会 给定四个字符串，找到第一对第二对字符，对应输出具体含义  

***思路：用四个字符串读取字符串，遍历并找到相同的大写字母，对应输出  
读题错误，字符串对应位置是相同的，不用两个循环，一个循环就够了   
对于字符串的长度，用s.length()函数***   

>#include <iostream>  
#include <cstring>  
using namespace std;  
int main()  
{  
	string s1, s2, s3, s4;  
	cin>>s1>>s2>>s3>>s4;  
	int i, j, count = 0;  
	for(i = 0; i < s1.length() && i < s2.length(); ++i)  
	{  
		if(s1[i] >= 'A' && s1[i] <= 'G' && s1[i] == s2[i] && count == 0)  
		{  
			switch(s1[i] - 'A')  
			{  
				case 0:cout<<"MON ";break;  
				case 1:cout<<"TUE ";break;  
				case 2:cout<<"WED ";break;  
				case 3:cout<<"THU ";break;  
				case 4:cout<<"FRI ";break;  
				case 5:cout<<"SAT ";break;  
				case 6:cout<<"SUN ";break;  
			}  
			count++;  
		}  
		else if((s1[i] >= '0' && s1[i] <= '9' && s1[i] == s2[i] && count == 1) ||
		   (s1[i] >= 'A' && s1[i] <= 'N' && s1[i] == s2[i] && count == 1)   )  
		{  
			if(s1[i] >= '0' && s1[i] <= '9')  
				cout<<"0"<<s1[i]-'0'<<":";  
			else if(s1[i] >= 'A' && s1[i] <= 'N')  
				cout<<s1[i]-'A'+10<<":";  
			count++;  
		}  
	}  
	for(i = 0; i < s3.length() && i < s4.length(); ++i)  
	{  
		if( (s3[i] >= 'a' && s3[i] <= 'z' && s3[i] == s4[i]) ||
			(s3[i] >= 'A' && s3[i] <= 'Z' && s3[i] == s4[i]) )  
		{   
			if(i < 9)  
				cout<<"0"<<i<<endl;  
			else  
				cout<<i<<endl;  
			break;  
		}  
	}  
	return 0;  
}  
*/

/*    
1015   有三个运行超时  用scanf和printf函数输入输出会好点   
德才论  按规则输出考生成绩信息  

***思路：定义一个类，包含考号，德才成绩，初始化函数，读取德与才的函数，总分函数，显示函数***  

>#include<iostream>  
#include<cstring>  
using namespace std;  
class Kaosheng  
{  
	char Num[9];  
	int De_Score;  
	int Cai_Score;  
public:  
	void Init(char *num,int defen,int caifen)  
	{  
		strcpy(Num,num);  
		De_Score=defen;  
		Cai_Score=caifen;  
	}  
	int Get_DS()  
	{  
		return De_Score;  
	}  
	int Get_CS()  
	{  
		return Cai_Score;  
	}  
	int total()  
	{  
		return De_Score+Cai_Score;  
	}  
	void show()  
	{  
		cout<<Num<<" "<<De_Score<<" "<<Cai_Score<<endl;  
	}  
};   
int main()  
{  
	int n;  
	int low_level;  
	int youxian_level;  
	cin>>n>>low_level>>youxian_level;  
	Kaosheng kaosheng[n],*p1[n],*p2[n],*p3[n],*p4[n],temp;  
	char num[9];   
	int df;  
	int cf;  
	for(int i=0;i<n;i++)  
	{  
		cin>>num>>df>>cf;  
		kaosheng[i].Init(num,df,cf);  
	}  
	int k=0,m=0,q=0,j=0;  
	for(int i=0;i<n;i++)  
	{   
	if((kaosheng[i].Get_DS()>=low_level)&&(kaosheng[i].Get_CS()>=low_level))  
		{  
		if((kaosheng[i].Get_DS()>=youxian_level)&&(kaosheng[i].Get_CS()>=youxian_level))  
		{  
			p1[k]=&kaosheng[i];  
			k++;  
		}  
		else if((kaosheng[i].Get_DS()>=youxian_level)&&(low_level<=kaosheng[i].Get_CS()<youxian_level))  
		{  
			p2[m]=&kaosheng[i];  
			m++;  
		}  
		else if((low_level<=kaosheng[i].Get_CS()<youxian_level)&&(low_level<=kaosheng[i].Get_DS()<youxian_level)&&(kaosheng[i].Get_DS()>=kaosheng[i].Get_CS()))  
		{  
			p3[q]=&kaosheng[i];  
			q++;  
		}  
		else if((low_level<=kaosheng[i].Get_CS()<youxian_level)&&(low_level<=kaosheng[i].Get_DS()<youxian_level)&&(kaosheng[i].Get_DS()<kaosheng[i].Get_CS()))  
		{  
			p4[j]=&kaosheng[i];  
			j++;  
		}  
		}  
     else continue;  
	}  
	cout<<k+m+q+j<<endl;   
	for(int i=0;i<k-1;i++)  
	{  
		for(int w=i+1;w<k;w++)  
		{  
			if(p1[w]->total()>p1[i]->total())  
			{  
				temp=*p1[w];  
				*p1[w]=*p1[i];  
				*p1[i]=temp;  
			}  
		}  
	}  
	for(int i=0;i<k-1;i++)  
	{  
		for(int w=i+1;w<k;w++)  
		{  
			if((p1[w]->total()==p1[i]->total())&&(p1[w]->Get_DS()>=p1[i]->Get_DS()))  
			{  
				temp=*p1[w];  
				*p1[w]=*p1[i];  
				*p1[i]=temp;  
			}  
		}  
	}  
	for(int i=0;i<k;i++)  
	{  
		p1[i]->show();    
	}    
	for(int i=0;i<m-1;i++)  
	{  
		for(int w=i+1;w<m;w++)  
		{  
			if(p2[w]->total()>=p2[i]->total())  
			{  
				temp=*p2[w];  
				*p2[w]=*p2[i];  
				*p2[i]=temp;  
			}  
		}  
	}  
	for(int i=0;i<m;i++)  
	{  
		p2[i]->show();  
	}  
	for(int i=0;i<q-1;i++)  
	{  
		for(int w=i+1;w<q;w++)  
		{   
			if(p3[w]->total()>=p3[i]->total())  
			{  
				temp=*p3[w];  
				*p3[w]=*p3[i];  
				*p3[i]=temp;  
			}  
		}  
	}  
	for(int i=0;i<q;i++)  
	{  
		p3[i]->show();  
	}  
	for(int i=0;i<j-1;i++)  
	{  
		for(int w=i+1;w<j;w++)  
		{  
			if(p4[w]->total()>=p4[i]->total())  
			{  
				temp=*p4[w];  
				*p4[w]=*p4[i];  
				*p4[i]=temp;  
			}  
		}  
	}  
	for(int i=0;i<j;i++)  
	{  
		p4[i]->show();   
	}  
	return 0;  
}  
//标准答案  
>#include <cstdio>  
#include <algorithm>  
#include <vector>  
#include <string>  
using namespace std;    
typedef struct Student  
{  
	string  ID;  
	int  De;  
	int Cai;  
}Student;  
inline bool compare(Student a,Student b)  
{  
	if ((a.Cai+a.De)!=(b.Cai+b.De))  
    return a.Cai+a.De>b.Cai+b.De;  
	else if (a.De!=b.De)   
	return a.De>b.De;  
	else  
	return a.ID<b.ID;  
}  
inline void print(Student s)  
{  
	printf("%s %d %d\n",s.ID.c_str(),s.De,s.Cai);  
}   
int main()  
{  
     int N,L,H;  
	 char id[10];  
	 Student  S;  
	 string  ID;  
	 vector<Student>    vec1;  
	 vector<Student>    vec2;  
	 vector<Student>    vec3;  
	 vector<Student>    vec4;  
	 scanf("%d %d %d",&N,&L,&H);  
	 while(N--)  
	 {   
		 scanf("%s%d%d",id,&S.De,&S.Cai);  
		 S.ID.assign(id);  
		if (S.De>=L&&S.Cai>=L)  
		{  
			if (S.De>=H&&S.Cai>=H)  
				vec1.push_back(S);  
			else  
			if (S.De>=H&&S.Cai<H)  
			    vec2.push_back(S);  
			else  
		   if (S.De<H&&S.Cai<H&&S.De>=S.Cai)  
		        vec3.push_back(S);  
			else  
				vec4.push_back(S);  	
		}  
	 }  
	 sort(vec1.begin(),vec1.end(),compare);  
	 sort(vec2.begin(),vec2.end(),compare);  
     sort(vec3.begin(),vec3.end(),compare);  
     sort(vec4.begin(),vec4.end(),compare);  
	 printf("%d\n",vec1.size()+vec2.size()+vec3.size()+vec4.size());  
	 for_each(vec1.begin(),vec1.end(),print);  
	 for_each(vec2.begin(),vec2.end(),print);  
	 for_each(vec3.begin(),vec3.end(),print);  
	 for_each(vec4.begin(),vec4.end(),print);  
	//  system("PAUSE");  
	return 0;  
}  
//第二次代码  
//两个地方需要注意 有两个测试点会运行超时，用printf进行输出 第二个 结构体的string不能直接输出，需要转字符串 c_str()   
>#include<iostream>  
#include<vector>  
#include<algorithm>  
#include<cstring>  
using namespace std;  
struct node  
{  
	string id;  
	int s_de;  
	int s_cai;  
	int all;  	
};   
bool cmp(node a, node b)  
{  
	return (a.all!=b.all)?a.all>b.all:((a.s_de!=b.s_de)?a.s_de>b.s_de: a.id<b.id);  
}  
vector<node> v1,v2,v3,v4;//v1为才德全尽 v2德分到线 v3德不低于才 v4仅都及格   
int main()  
{  
	int num,low,high;  
	cin>>num>>low>>high;  	
	for(int i = 0; i<num; i++){  
		node temp;  
		cin>>temp.id>>temp.s_de>>temp.s_cai;  
		temp.all = temp.s_de + temp.s_cai;  
		if(temp.s_de>=high && temp.s_cai>=high){  
			v1.push_back(temp);  
		}  
		else if(temp.s_de>=high && temp.s_cai>=low){  
			v2.push_back(temp);  
		}  
		else if(temp.s_de>=low && temp.s_cai>= low && temp.s_de>=temp.s_cai){  
			v3.push_back(temp);  
		}  
		else if(temp.s_de>=low && temp.s_cai>= low){  
			v4.push_back(temp);  
		}  
	}   
	sort(v1.begin(),v1.end(),cmp);  
	sort(v2.begin(),v2.end(),cmp);  
	sort(v3.begin(),v3.end(),cmp);  
	sort(v4.begin(),v4.end(),cmp);  
	int sum = v1.size()+v2.size()+v3.size()+v4.size();  
	cout<<sum<<endl;  
	for(int i = 0; i<v1.size(); i++){  
		printf("%s %d %d\n",v1[i].id.c_str(), v1[i].s_de, v1[i].s_cai);  
//		cout<<v1[i].id<<" "<<v1[i].s_de<<" "<<v1[i].s_cai<<endl;  
	}  
	for(int i = 0; i<v2.size(); i++){   
		printf("%s %d %d\n",v2[i].id.c_str(), v2[i].s_de, v2[i].s_cai);  
//		cout<<v2[i].id<<" "<<v2[i].s_de<<" "<<v2[i].s_cai<<endl;   
	}  
	for(int i = 0; i<v3.size(); i++){  
		printf("%s %d %d\n",v3[i].id.c_str(), v3[i].s_de, v3[i].s_cai);  
//		cout<<v3[i].id<<" "<<v3[i].s_de<<" "<<v3[i].s_cai<<endl;  
	}  
	for(int i = 0; i<v4.size(); i++){  
		printf("%s %d %d\n",v4[i].id.c_str(), v4[i].s_de, v4[i].s_cai);  
//		cout<<v4[i].id<<" "<<v4[i].s_de<<" "<<v4[i].s_cai<<endl;  
	}  
	return 0;  
}    
*/   
  
/*  
1016    
部分A+B 给定两个整数，将对应数字出现次数按规律成新的数字，求和  

***思路：用char数组或者字符串去存入两个char型整数，然后比较字符出现次数，字符转为数字  
      用公式计算出对应结果，用到pow(a,b)函数 a的b次方***  
>#include<iostream>   
#include<cmath>    
using namespace std;  
int main()  
{  
    string A,B;  
    char Da,Db;  
    int a=0,b=0,sum=0;  
    cin>>A>>Da>>B>>Db;  
    for(int i=0; i<A.size(); i++)   
    {  
        if(A[i]==Da)  
            a++;  
    }  
    for(int i=0; i<B.size(); i++)  
    {  
        if(B[i]==Db)  
            b++;  
    }  
    for(int i=0; i<a; i++)  
        sum+=(Da-'0')*pow(10,i);   //10的i次方   
    for(int i=0; i<b; i++)  
        sum+=(Db-'0')*pow(10,i);  
    cout<<sum<<endl;  
}        
*/   

/*  
1017  有一个答案错误   
A除以B  不超过1000位的A读入，算商和余数  

***思路：放入字符型数组，再放入整型数组***   

>#include<iostream>   
#include<cstring>  
using namespace std;  
int main()  
{	  
	char c[1001];  
	int a[1001];  
	cin>>c;	  
	int m;  
	cin>>m;  
	for(int i=0;i<strlen(c);i++)    
	{  
		a[i]=c[i]-'0';  
	}  
	int yu=0,shang;  
	for(int i=0;i<strlen(c);i++)  
	{  
		shang=(yu*10+a[i])/m;	  	
		yu=(yu*10+a[i])%m;      
		if(shang==0&&i==0){    
		}  
		else cout<<shang;  
	}  
	cout<<" "<<yu;  
	return 0;  
}  
//标准答案  
>#include<iostream>  
#include<string.h>  
#include<math.h>  
#include<stdlib.h>  
int main(){  
    char a[2000];  
    scanf("%s",a);  
    int b;  
    scanf("%d",&b);  
    int n = strlen(a);  
    int temp=0;  
    int flag = 0;  
    for(int i=0;i<n;i++){  
        temp = (a[i]-'0')+temp*10;  
        if(temp>=b){  
            printf("%d",temp/b);  
            flag = 1;  
        }  
        else if(flag){  
            printf("0");  
        }  
        temp = temp%b;  
    }  
    if(flag==0)  
        printf("0");  
    printf(" %d",temp);  
}   
*/  

/*    运算符重载只能跟类一起玩   
1018    部分正确   
锤子剪刀布 输出甲乙胜平负的次数和胜最多的手势  

***思路：读取次数后循环遍历，每次计数并对应胜利手势结果数组位置+1***   
 
>#include<iostream>  
using namespace std;  
bool judgeda(char c1,char c2)  
{  
	if(((c1=='C')&&(c2=='J'))||((c1=='J')&&(c2=='B'))||((c1=='B')&&(c2=='C')))  
	return true;  
	else return false;  
}   
bool judgedeng(char c1,char c2)  
{  
	if(((c1=='C')&&(c2=='C'))||((c1=='J')&&(c2=='J'))||((c1=='B')&&(c2=='B')))  
	return true;  
	else return false;  
}  
bool judgexiao(char c1,char c2)  
{  
	if(((c1=='J')&&(c2=='C'))||((c1=='B')&&(c2=='J'))||((c1=='C')&&(c2=='B')))  
	return true;  
	else return false;  
}  
int main()  
{  
	int n;  
	cin>>n;  
	char c1,c2;   //用于读取字符（手势）    
	int jia[3]={0};   //用于放入胜平负次数    多定义一个空位   
	int yi[3]={0};  
	int jiashoushi[3]={0};//用于记录胜利时手势结果 C J B  
	int yishoushi[3]={0};   
	for(int i=0;i<n;i++)  
	{  
		cin>>c1>>c2;  
		if(judgeda(c1,c2))  
		{  
			jia[0]++;  
			yi[2]++;  
			if(c1=='C')  
			{  
				jiashoushi[0]++;  
			}  
			else if(c1=='J')  
			{  
				jiashoushi[1]++;  
			}  
			else if(c1=='B')  
			{  
				jiashoushi[2]++;  
			}  
		}  
		else if(judgexiao(c1,c2))   
		{  
			jia[2]++;  
			yi[0]++;  
			if(c2=='C')  
			{  
				yishoushi[0]++;  
			}  
			else if(c2=='J')  
			{  
				yishoushi[1]++;  
			}  
			else if(c2=='B')  
			{  
				yishoushi[2]++;  
			}  
		}  
		else if(judgedeng(c1,c2))   
		{  
			jia[1]++;  
			yi[1]++;  
		}  
	}   
	cout<<jia[0]<<" "<<jia[1]<<" "<<jia[2]<<endl;  
	cout<<yi[0]<<" "<<yi[1]<<" "<<yi[2]<<endl;  
	//遍历甲获胜手势中最多的  C J B  
	int max1=0;  
	for(int i=0;i<2;i++)  
	{  
		if(jiashoushi[i]<=jiashoushi[i+1])  
		{  
			max1=i+1;  
		}  	 
	}   
	if(max1==0)  
	{  
		cout<<"C"<<" ";  
	}  
	else if(max1==1)  
	{  
		cout<<"J"<<" ";  
	}  
	else if(max1==2)  
	{  
		cout<<"B"<<" ";  
	}  
	int max2=0;  
	for(int i=0;i<2;i++)  
	{  
		if(yishoushi[i]<=yishoushi[i+1])  
		{  
			max2=i+1;  
		}  	
	}  
	if(max2==0)   
	{  
		cout<<"C";  
	}  
	else if(max2==1)  
	{  
		cout<<"J";  
	}  
	else if(max2==2)  
	{  
		cout<<"B";  
	}  
	return 0;  
}  
//标准答案   
>#include <iostream>    
#include <stdio.h>   
using namespace std;  
int main()  
{  
    int n;  
    scanf("%d", &n);  
    int jiaWin = 0, jiaLose = 0, draw = 0;  
    int jiaJWin = 0, jiaCWin = 0, jiaBWin = 0;  
    int yiJWin = 0, yiCWin = 0, yiBWin = 0;  
    char jia, yi;  
    for(int i = 0; i < n; i++){  
        scanf(" %c %c", &jia, &yi);  
        if(jia == 'C'){  
            if(yi == 'C'){  
                draw++;  
            }  
            else if(yi == 'J'){  
                jiaWin++;  
                jiaCWin++;  
            }  
            else if(yi == 'B'){  
                jiaLose++;  
                yiBWin++;  
            }  
        }  
        else if(jia == 'J'){  
            if(yi == 'C'){  
                jiaLose++;  
                yiCWin++;  
            }  
            else if(yi == 'J'){  
                draw++;  
            }  
            else if(yi == 'B'){  
                jiaWin++;  
                jiaJWin++;  
            }  
        }  
        else if(jia == 'B'){  
            if(yi == 'C'){  
                jiaWin++;  
                jiaBWin++;  
            }  
            else if(yi == 'J'){  
                jiaLose++;  
                yiJWin++;  
            }  
            else if(yi == 'B'){  
                draw++;  
            }  
        }  
    }  
    printf("%d %d %d\n", jiaWin, draw, jiaLose);  
    printf("%d %d %d\n", jiaLose, draw, jiaWin);  
    printf("%c ", jiaBWin >= jiaCWin ? (jiaBWin >= jiaJWin ? 'B' : 'J') : (jiaCWin >= jiaJWin ? 'C' : 'J'));  
    printf("%c\n", yiBWin >= yiCWin ? (yiBWin >= yiJWin ? 'B' : 'J') : (yiCWin >= yiJWin ? 'C' : 'J'));  
    return 0;    
}   
//第二次代码  
>#include<iostream>  
#include<vector>   
#include<algorithm>  
#include<cstring>  
using namespace std;  
void out(int a[])  
{  
	int max1 = -1;  
	int num1 = 0;  
	for(int i = 0; i<3 ;i++){   //直接用3   
		if(a[i]>max1){  
			max1=a[i];  
			num1 = i;  
		}   
	}  
	if(num1 == 0) cout<<"B";  
	else if(num1 == 1) cout<<"C";  
	else if(num1 == 2) cout<<"J";	  
}   
int main()  
{  
	int N;  
	cin>>N;    
	int count1 = 0, count2 = 0, count3 = 0;  
	int j[3] = {0}, y[3] = {0};  
	char jia, yi;  
	for(int i = 0; i<N; i++){  
		cin>>jia>>yi;  
		if((jia == 'C' && yi == 'J') || (jia == 'J' && yi == 'B') || (jia == 'B' && yi == 'C')){  
			count1++;  
			if(jia == 'B') j[0]++;  
			else if(jia == 'C') j[1]++;  
			else if(jia == 'J') j[2]++;	  
		}  
		else if((jia == 'C' && yi == 'C') || (jia == 'J' && yi == 'J') || (jia == 'B' && yi == 'B')){  
			count2++;  
		}  
		else if((jia == 'C' && yi == 'B') || (jia == 'J' && yi == 'C') || (jia == 'B' && yi == 'J')){  
			count3++;  
			if(yi == 'B') y[0]++;  
			else if(yi == 'C') y[1]++;  
			else if(yi == 'J') y[2]++;  
		}  
	}   
	printf("%d %d %d\n", count1, count2, count3);  
	printf("%d %d %d\n", count3, count2, count1);  
	out(j);  
	cout<<" ";  
	out(y);  
	return 0;  
}   
*/  

/*  
1019  
数字黑洞 将四位数字按序做差得到新数，一直循环直到6174   
  
***思路：字符串操作猛如虎***    

//参考答案 玩转字符串  头文件 #include <algorithm>   
>#include<iostream>   
#include<cstring>  
#include<algorithm>  
using namespace std;  
int main()  
{  
	string s,s1,s2;  
	cin>>s;  
	s.insert(0,4-s.length(),'0');  //不足四位填充完整   
	while(true)  //条件达到就break出去，不然执行死循环   
	{  
		string temp=s;  
		sort(s.rbegin(),s.rend());  //将s降序排列 有s   
		s1=s;   
		sort(temp.begin(),temp.end());  
		s2=temp;                    //将temp升序排列  
		if(s1==s2)  
		{  
			cout<<s1<<" - "<<s2<<" = 0000"<<endl;  
			break;   //第一处跳出循环   
		}   
		else  
		{  
			int ts1 = stoi(s1);  //字符串转成数字  
			int ts2=stoi(s2);  
			int dec=ts1-ts2;  
			cout<<s1<<" - "<<s2<<" = ";  
			printf("%04d",dec);  //对于输出结果的控制，用printf操作  
			s[0] = (char)(dec/1000 + '0');  
            dec%=1000;  
            s[1] = (char)(dec/100 + '0');  
            dec%=100;  
            s[2] = (char)(dec/10 + '0');  
            dec%=10;  
            s[3] = (char)(dec + '0');  
		//	s=to_string(ts1-ts2);   
		//	s.insert(0, 4-s.length(), '0');  
		 }   
		 if(s=="6174")break;  //第二处跳出循环  
		 else cout<<endl;   
	}  	
	return 0;  
}  
//第二次仍然不完美  
//递归函数思想做数字黑洞  
>#include<iostream>  
#include<algorithm>  
#include<cstring>  
bool cmp(char a, char b)  
{  
	return a>b;  
}  
using namespace std;   
int main()  
{  
    char a[4];  
	for(int i = 0; i<4; i++){  
		cin>>a[i];  
	}  
	while(true){  
		if(a[0]==a[1] && a[0]==a[2] && a[0]==a[3]){  
		printf("%c%c%c%c - %c%c%c%c = 0000",a[0],a[0],a[0],a[0],a[0],a[0],a[0],a[0]);  
		break;  
	    }  
	    else{  
	char b[4];  
	char c[4];  
	for(int i = 0; i<4; i++){  
		b[i] = a[i];  
	}  
	sort(a,a+strlen(a),cmp);    
	sort(b,b+strlen(a));  
	int temp;  
	temp = ((a[3]-'0') + (a[2]-'0')*10 + (a[1]-'0')*100 + (a[0]-'0')*1000) - ((b[3]-'0') + (b[2]-'0')*10 + (b[1]-'0')*100 + (b[0]-'0')*1000);  
	for(int i = 0; i<4; i++){  
		printf("%c",a[i]);  
	}  
	printf(" - ");  
	for(int i = 0; i<4; i++){  
		printf("%c",b[i]);  
	}  
	printf(" = %04d\n",temp);  
	if(temp == 6174) break;  
	else{  
		a[0] =(char) (temp/1000 + '0');  
	    a[1] =(char) (temp/100%10 + '0');  
	    a[2] =(char) (temp/10%10 +'0');  
     	a[3] =(char) (temp%10 + '0');  
	}  
	}  
	}  
	return 0;  
}   
*/  
 
/*  
1020  
月饼 给出不同月饼的库存量，总售价，市场总需求，算最大收益  

***思路:用a[n][2]分别放入数据，并计算  用三个数组也可以***   

>#include<iostream>  
using namespace std;  
int main()  
{  
	int n;  
	cin>>n;  
	float a1[n];  
	float a2[n];  
	float b[n];  
	int zongliang;  
	cin>>zongliang;  
	for(int i=0;i<n;i++)  //读入库存量   
	{  
		cin>>a1[i];  
	}  
	for(int j=0;j<n;j++)  //读入总售价   
	{  
		cin>>a2[j];  
	}  
	for(int q=0;q<n;q++)  //读入收益单价   
	{  
		b[q]=a2[q]/a1[q];  
	}  
	//按收益单价进行排序  
	for(int i=0;i<n-1;i++)  
	{  
		for(int j=i+1;j<n;j++)  
		{  
			if(b[i]<b[j])  
			{  
				float temp1,temp2,temp3;  
				temp1=b[i];  
				b[i]=b[j];  
				b[j]=temp1;  
				temp2=a1[i];  
				a1[i]=a1[j];  
				a1[j]=temp2;  
				temp3=a2[i];  
				a2[i]=a2[j];  
				a2[j]=temp3;  
			}  
		}  
	}   
	float lirun=0.0;  
	for(int i=0;i<n;i++)  
	{  
		if(zongliang>a1[i])  
		{  
			lirun+=a2[i];  
			zongliang=zongliang-a1[i];  
			continue;  
		}  
		else   //if((zongliang>0)&&(zongliang<a[i][0]))  
		{  
			lirun=lirun+zongliang*b[i];  
			break;  
		}  
	}  
	printf("%.2f",lirun);  
	return 0;  
}  
//第二次代码  
//月饼题 用结构体vector做  考虑市场需求太大，无法全部满足 测试点3   
>#include<iostream>  
#include<algorithm>  
#include<cstring>  
#include<vector>  
using namespace std;   
struct node  
{  
	double cucun;  
	double zongjia;  
	double danjia;  	
};   
bool cmp(node a, node b)  
{  
	return a.danjia>b.danjia;  
}  
int main()  
{  
	int N;  
	double count;  
	cin>>N>>count;  
	double mmax = 0.0;  
	double mmmax = 0.0;  
	vector<node> v(N);  
	for(int i = 0; i<N; i++){  
		cin>>v[i].cucun;  
		mmax +=  v[i].cucun;   
	}  
	for(int i = 0; i<N; i++){  
		cin>>v[i].zongjia;  
		mmmax += v[i].zongjia;  
	}  
	for(int i = 0; i<N; i++){  
		v[i].danjia = v[i].zongjia/v[i].cucun;  
	}  
    sort(v.begin(),v.end(),cmp);  
    int k = 0;   
    double sum = 0.0;  
    if(count>=mmax){   
    	printf("%.2lf",mmmax);  
	}  
	else{   
    	if(count>v[k].cucun){  
    		sum += v[k].zongjia;  
    		count -= v[k].cucun;  
    		k++;  
		}  
		else if(count<=v[k].cucun){  
			sum += count*v[k].zongjia/v[k].cucun;  
		//	count = 0;  
		    break;   
		}  
	}  
	printf("%.2lf",sum);  
	}  
	return 0;  
}      
*/  

/*
1021
各个数字出现次数统计

思路：字符串读入，编写函数形参为字符串和数字，返回值输出此数字在字符串中出现次数
      主程序中判断对应值不为空的输出对应个数
#include<iostream>
using namespace std;

int GeShu(string s,int n)
{
	int count=0;
	for(int i=0;i<s.size();i++)
	{
		if(n==(s[i]-'0'))
		{
			count++;
		}
	}
	return count;
}

int main()
{
	string s;
	cin>>s;
	for(int i=0;i<10;i++)
	{
		if(GeShu(s,i)!=0)
		cout<<i<<":"<<GeShu(s,i)<<endl;
	}
	return 0;	
} 
*/

/*
1022
将数转换成D进制 

思路：辗转相除法  逆序输出余数
#include <iostream>
using namespace std;
int main(){
    int m,n,d;
    cin>>m>>n>>d;
    int sum=m+n;
    int num=0;
    int a[31];
    do{
    a[num++]=sum%d;
    sum /= d;
    }while(sum!=0);

    for(int i=num-1;i>=0;i--)
    {
        cout<<a[i];
    }
    return 0;
} 
*/

/*
1023
组个最小数 给定每个数字的个数，输出第一位不为零的最小数字组合 

思路：先从数组第二位（代表数字1的个数）开始找到首位，后面遍历是否此位有数字需要输出
      定义int[]数组输出数字，注意已输出首位需要扣除一位 
#include<iostream>
using namespace std;
int main()
{
	int num[10]={0},A[10]={0,1,2,3,4,5,6,7,8,9};
	int j=0,i;
	for(i=0;i<10;i++){
		cin>>num[i];
	}
//输出不为零的最小第一位 
	for(j=1;j<10;j++){
		if(num[j]!=0){
			cout<<j;
			break;
		}
	}
//输出其他各位，注意已输出的要减去
	for(int m=0;m<10;m++){
		if(num[m]!=0)   //代表有数字需要输出
		{
			if(m==j){
				num[m]--;
				while(num[m]){
					cout<<A[m];
					num[m]--;
				}
			}else{
			while(num[m]){
				cout<<A[m];
				num[m]--;
				}
			}
		}
	}
	return 0;
}
*/

/*
1024
科学计数法

%[0-9]  a   了解一下
#include<iostream>
using namespace std;
int main()
{
	char figure;
	char a[10001]={0};
	int zhishu;
	//神操作  %[0-9]
	scanf("%c%c.%[0-9]E%d",&figure,&a[0],a+1,&zhishu);
	if(figure=='-')cout<<"-";
	if(zhishu>=0){
		for(int i=0;i<=zhishu||a[i]!=0;i++){
			if(i==zhishu+1)cout<<".";
			//cout<<a[i];
			printf("%c",a[i]==0?'0':a[i]);
		}
	}
	else{
		cout<<"0.";
		for(int i=1;i<(-zhishu);i++){
			cout<<"0";	
		
	}	printf("%s",a);
	}
	return 0;
} 
*/

/*
1025
反转链表

思路：用三个vector<node>去存储初始化，排序，归并后，再输出，注意初始化为稀疏存储，
找到开头后后两个向量都是push_back,此时就可以用下标开始操作了 

#include<iostream> 
#include<vector>
using namespace std;
struct node
{
	int add;
	int data;
	int next;
};
int main()
{
	vector<node>in(100001);
	vector<node>sorted;
	vector<node>out;
	
	int first,N,team;
	cin>>first>>N>>team;
	node temp;
	//第一步 存入第一个向量 
	for(int i=0;i<N;i++)
	{
		cin>>temp.add>>temp.data>>temp.next;
		in[temp.add]=temp;
	}
	//第二步 排序存入第二个向量 
	if(first==-1)cout<<"-1"<<endl;
	else
	{
		int nextadd=first;//！！！！
		while(nextadd!=-1){          //出错了，没有限制结束条件啊！！！ 
			sorted.push_back(in[nextadd]);
		    nextadd=in[nextadd].next; 
		} 
		
	}
	//第三步  存入第三个向量 
	int size=sorted.size();
	int tern=team-1;
	while(tern<size)
    {
    	for(int j=tern;j>tern-team;j--)
    	{
    		out.push_back(sorted[j]);//第二步中用的push_back，所以这里直接使用下标 
		}
		tern+=team; 
	}
	//存在最后几个尾数还未push_back
	for(int j=tern-team+1;j<size;j++)
	{
		out.push_back(sorted[j]);
	} 
	//第四步，除最后一个之外，其余的循环输出，最后一个单独输出
	for(int i=0;i<size-1;i++)
	{
		out[i].next=out[i+1].add;
		printf("%05d %d %05d\n",out[i].add,out[i].data,out[i].next);
	} 
	printf("%05d %d -1\n",out[size-1].add,out[size-1].data);
	return 0;
}
*/ 

/*
1026
程序运行时间 
渣渣题 
#include<iostream>
using namespace std;
int main()
{
	long c1,c2;
	cin>>c1>>c2; 
	long cha=c2-c1;
	if(cha%100>=50)cha=cha/100+1;
	else cha=cha/100;
	printf("%02d:%02d:%02d",cha/3600,cha%3600/60,cha%60);
	
	return 0;
}
*/

/*
1027 格式错误，部分正确 
打印沙漏图案 给定数字与符号 尽可能用掉数字去打印出沙漏图案，输出图案与未用完的个数

思路：用函数返回排数

#include<iostream>
using namespace std;
int paishu(int n)
{
	int count=0;
	int i=3;
	while(n>0)
	{
		count++;
		if(count==1)n=n-1;
		else{
			n=n-2*i;
			i+=2;
		}
	}
	count=count-1;
	return count;
} 
int yu(int n,int hang)
{
	int y;
	while(hang>1)
	{
		n=n-hang*2;
		hang=hang-2;
	}
	y=n-1;
	return y;
}
int main()
{
	int n;
	char c;
	cin>>n;
	cin>>c;
	int hang=paishu(n);
	int zongshu=2*hang-1;
	int kong=0;
	while(hang>=1)
	{
	    
		for(int i=0;i<kong/2;i++)
		cout<<" ";
		for(int j=0;j<zongshu-kong;j++)
		cout<<c;
		for(int i=0;i<kong/2;i++)
		cout<<" ";
		cout<<endl;
		hang=hang-1;
		kong=kong+2;
	}
	
	for(hang=3;hang<=zongshu;hang+=2)
	{
		int kong=zongshu-hang;
		for(int i=0;i<kong/2;i++)
		cout<<" ";
		for(int j=0;j<hang;j++)
		cout<<c;
		for(int i=0;i<kong/2;i++)
		cout<<" ";
		cout<<endl;
	}
	cout<<yu(n,zongshu);
	return 0;
}
//参考答案
#include <iostream>
using namespace std;
int main() {
    int N;
    char c;
    cin >> N >> c;
    int row = 0;
    for (int i = 0; i < N; i++) {
        if ((2 * i * (i + 2) + 1) > N) {
            row = i - 1;
            break;
        }
    }
    for (int i = row; i >= 1; i--) {
        for (int k = row - i; k >= 1; k--)
            cout << " ";
        for (int j = i * 2 + 1; j >= 1; j--)
            cout << c;
        cout << endl;
    }
    for (int i = 0; i < row; i++)
        cout << " ";
    cout << c << endl;
    for (int i = 1; i <= row; i++) {
        for (int k = row - i; k >= 1; k--)
            cout << " ";
        for (int j = i * 2 + 1; j >= 1; j--)
            cout << c;
        cout << endl;
    }
    cout << (N - (2 * row * (row + 2) + 1));
    return 0;
}
 
*/ 

/*
1028
人口普查 输入一系列人名出生日期，判断合理性并输出最年长与最年轻的人名

思路：两个日期的比较方法： 
正常读入，然后用一个long long型变量 = year*10000+month*100+day
#include<iostream>
#include<cstring>
using namespace std; 
int main()
{
	char name[6],Max[6],Min[6];
	char shengri[11];
	int year;
	int mouth;
	int day;
	int count;
	long long max=20140907;
	long long min=18140905;
	cin>>count;
    int shu=0;
	for(int i=0;i<count;i++)
	{
		cin>>name>>shengri;
		year=(shengri[0]-'0')*1000+(shengri[1]-'0')*100+(shengri[2]-'0')*10+(shengri[3]-'0');
		mouth=(shengri[5]-'0')*10+(shengri[6]-'0');
		day=(shengri[8]-'0')*10+(shengri[9]-'0');
		long long birth = year*10000+mouth*100+day;
		if(birth<18140906||birth>20140906)continue;
		else{
			shu++;
			if(birth<max)
			{
				strcpy(Max,name);
				max=birth;
			}
			if(birth>min)
			{
				strcpy(Min,name);
				min=birth;
			}
		}
	}
	if(shu)
	{
		cout<<shu<<" "<<Max<<" "<<Min;
	}
	else cout<<"0";
	return 0;
}
*/ 

/*
1029
旧键盘
给两个字符串，找出缺的字符，对应键输出大写形式

思路：三个字符数组，两个读取，然后遍历，设置标志位，若缺少的第一次出现，放入新数组 
#include<iostream>
#include<cstring>
using namespace std;
int main()
{
	char a[81],b[81],c[81]={0};
	cin>>a;
	cin>>b;
	int j=0,n=0;
	for(int i=0;i<strlen(a);i++)
	{
		if(a[i]==b[j])j++;   //新的遍历方式，第一次见 
	    else
	    {
	    	int flag=0;
	    	if(islower(a[i]))  //如果a[i]是小写  头文件cctype cstring 貌似都可以 
	    	a[i]=toupper(a[i]); //转换为大写
			for(int k=0;k<n;k++) 
			{
				if(a[i]==c[k])
				flag=1;
			}
			if(flag==0)
			c[n++]=a[i];
		}
	}
	for(int i=0;i<n;i++)
	{
		cout<<c[i];
	}
	
	return 0;
} 
*/

/*
1030   部分正确 
完美数列 给出数列项数和参数 找到整个数列中小于等于参数*最小项的项数

思路：首先我们同样保持第一个for循环遍历最小值，在第二个for循环中我们将j置为前一个元素作为最小数时候的长度，
这样就减少了小于上一次的不必要的for循环，j依然小于 N，用一个if判断是否符合条件，
用另一个if判断此次是否大于上次的长度，比如说我们把样例中的数据已经排好序：1 2 3 4 5 6 7 8 9 20 ，
此时我们将array[0]作为最小数，依次向后遍历，最大数j-最小数i+1即为数列的长度，最终找到8为最大的数，
此时数列长度count为8，在将a[1]作为最小数的时候，我们直接将j置为1+8为9，直接比较a[1]和a[9]作为最小最大值得时候是否满足，
不满足则a[1]最为最小数的时候并不能使数列变得更长，则继续再看a[2],这样等到有大于8的时候再更新，就可以了。
#include<iostream>
#include<algorithm> //sort（）函数 
using namespace std; 
int main()
{
	int n;
	
	double a[100010];
	double p;
	cin>>n>>p;
	int i=0,j=0;
	int count=0;
	for(int i=0;i<n;i++)
	{
		cin>>a[i];
	}
	sort(a,a+n);
	for(i=0;i<n;i++)
	{
		for(j=i+count;j<n;j++)
		{
			if(a[j]>a[i]*p)break;
			if(j-1+1>count)
			count=j-i+1; 
		}
	}
	cout<<j-i+1;
	return 0;
}

//标准答案
#include<iostream>
#include<algorithm>
using namespace std;
int main(){
    int N,count=0;
    double p,array[100010];;
    scanf("%d%lf",&N,&p);
    for(int i=0;i<N;i++)
        scanf("%lf",&array[i]);
    sort(array,array+N);
    for(int i=0;i<N;i++)            //遍历，将a[i]作为最小数
        for(int j=i+count;j<N;j++){ //j置为要满足可以更新数列长度的值，减少循环次数
            if(array[j]>array[i]*p) //如果不满足条件了，则将下一个元素最为最小值
            break;
            if(j-i+1>count)         //如果此次的长度大于上一次，更新数列长度
                count=j-i+1;
        }
    printf("%d",count);
    return 0;
}  
*/

/*
1031
查验身份证 前十七位给权值，算乘积和的取模11，对应效验码是否正确

思路：字符串读入，字符串转数组，再数组乘权重数组，再求和，算模，与效验码数组比较
 
#include<iostream>
using namespace std;
int main()
{
	int quanzhong[17]={7,9,10,5,8,4,2,1,6,3,7,9,10,5,8,4,2}; 
	char jiaoyanma[11]={'1','0','X','9','8','7','6','5','4','3','2'};
	int n;
	cin>>n;
//	char *p[n];
	int k=0;
	char num[18];
	for(int j=0;j<n;j++)
	{	
	for(int i=0;i<18;i++)
	{
		cin>>num[i];
	}
	int number[18];
	for(int i=0;i<17;i++)  //取前17位放入数值数组 
	{
		if('0'<=num[i]&&num[i]<='9')
		number[i]=num[i]-'0';
		else
		number[i]=10;
	}
	//加权求和并取模11
	int sum=0;
	for(int i=0;i<17;i++)
	{
		sum+=number[i]*quanzhong[i];
	}
	int mo;
	mo=sum%11;
	if(jiaoyanma[mo]!=num[17])
	{
		for(int i=0;i<18;i++)
		{
			cout<<num[i];
		}
		cout<<endl;
		k=1;
	}
	}
	
	if(k==0)
	{
		cout<<"All passed";
	}
	return 0;
}
*/

/*
1032
输入序号与成绩，涉及到序号重复，最后输出成绩最高的序号和成绩

思路：用数组对应装数，找最大 
#include<iostream>
using namespace std;
int main()
{
	int chengj[100001]={0};
	int name,score,max=0,max_name;
	int n;
	cin>>n;
	for(int i=0;i<n;i++)
	{
		cin>>name>>score;
		chengj[name]+=score;
		if(chengj[name]>max)
		{
			max=chengj[name];
			max_name=name;
		}
	 } 
	 cout<<max_name<<" "<<max<<endl;
	return 0;
}
*/ 
/*
1033
旧键盘打字  设定某些键坏掉，给定字符串，输出实际能输出的字符串

思路：大神思路，将ASCii码的127个位置，先读坏键置为1，再去读好键，为零即可输出，注意大小写同时都置一，注意上档键导致大写全为一
#include <stdio.h>
#include <cctype>

int main()
{
    int list[128] = {0}, bad_key = 0, input;

    while ((bad_key = getchar()) != '\n')   //输入坏键都是大写 
	{
        list[bad_key] = 1;
        list[tolower(bad_key)] = 1; // 将所有坏键赋值为1，如果是字母，相应的大小写字符都要赋值为1
    }
    if (list['+'] == 1) // 检查上档键
        for (int i = 65; i < 91; i++)
            list[i] = 1;
    while ((input = getchar()) != '\n')
        if (list[input] == 0) // 查验字符表，并进行输出
            printf("%c", input);

    return 0;
}
*/

/*
1034  
有理数四则运算  太复杂了，各种讨论 

#include<iostream>
#include<cmath>
#define fun fc
using namespace std;
//辗转相除求最大公约数，真的牛皮！！！ 
long long a, b, c, d;
long long huajian(long long a,long long b)
{
    return b==0?a:huajian(b,a%b);
}

void fc(long long m,long long n)
{
	int flag1=0,flag2=0,flag=0;
	//分母为零 
	if(n==0)
	{
		cout<<"Inf";
		return;  //子程序结束，回到子函数 
	}
	//分子为零 
	if(m==0)
	{
		cout<<"0";
		return;
	}
	//讨论负数  
	if (m < 0) flag1 = 1;
    if (n < 0) flag2 = 1;
    m = abs(m), n = abs(n);
    if (flag1 == 1 && flag2 == 1) flag = 0;
    else if (flag1 == 1 || flag2 == 1) flag = 1;
    //若分子分母值相同 
    if (m == n) {
        if (flag == 1) printf("(-1)");
        else printf("1");
        return;
    } 
	long long x=m%n,y=m/n;  //x表示正整数倍或者零 y表示是否为假分数，整数倍数为y 
    if(x==0)
	{
		if(flag==0)cout<<y;
		else cout<<"-"<<y;
		return;
	} 
	else
	{
		long long t1=m-y*n,t2 =n, t=huajian(t1,t2);
		t1=t1/t,t2=t2/t;
	if (flag == 1) {
            printf("(-");
            if (y != 0) printf("%lld %lld/%lld)", y, t1, t2);
            else printf("%d/%d)", t1, t2);
        } else {
            if (y != 0) printf("%lld %lld/%lld", y, t1, t2);
            else printf("%lld/%lld", t1, t2);
        }	
	}
} 
void print() {
    fun(a, b); printf(" + "); fun(c, d); printf(" = "); fun(a * d + b * c, b * d); printf("\n");
    fun(a, b); printf(" - "); fun(c, d); printf(" = "); fun(a * d - b * c, b * d); printf("\n");
    fun(a, b); printf(" * "); fun(c, d); printf(" = "); fun(a * c, b * d); printf("\n");
    fun(a, b); printf(" / "); fun(c, d); printf(" = "); fun(a * d, b * c); printf("\n");
}
int main() {
    scanf("%lld/%lld %lld/%lld", &a, &b, &c, &d);
    print();
    return 0;
} 

//标准答案
#include <iostream>
#include <cmath>
using namespace std;
long long a, b, c, d;
long long gcd(long long t1, long long t2) {
    return t2 == 0 ? t1 : gcd(t2, t1 % t2);
}
void func(long long m, long long n) {
    int flag1 = 0, flag2 = 0, flag = 0;
    if (n == 0) {
        printf("Inf");
        return ;
    }
    if (m == 0) {
        printf("0");
        return ;
    }
    if (m < 0) flag1 = 1;
    if (n < 0) flag2 = 1;
    m = abs(m), n = abs(n);
    if (flag1 == 1 && flag2 == 1) flag = 0;
    else if (flag1 == 1 || flag2 == 1) flag = 1;
    if (m == n) {
        if (flag == 1) printf("(-1)");
        else printf("1");
        return;
    }       
    long long x = m % n, y = m / n;
    if (x == 0) {
        if (flag == 0) printf("%d", y);
        else printf("(-%d)", y);
        return ;
    } else {
        long long t1 = m - y * n, t2 = n, t = gcd(t1, t2);
        t1 = t1 / t, t2 = t2 / t;
        if (flag == 1) {
            printf("(-");
            if (y != 0) printf("%lld %lld/%lld)", y, t1, t2);
            else printf("%d/%d)", t1, t2);
        } else {
            if (y != 0) printf("%lld %lld/%lld", y, t1, t2);
            else printf("%lld/%lld", t1, t2);
        }
    }
}
void print() {
    func(a, b); printf(" + "); func(c, d); printf(" = "); func(a * d + b * c, b * d); printf("\n");
    func(a, b); printf(" - "); func(c, d); printf(" = "); func(a * d - b * c, b * d); printf("\n");
    func(a, b); printf(" * "); func(c, d); printf(" = "); func(a * c, b * d); printf("\n");
    func(a, b); printf(" / "); func(c, d); printf(" = "); func(a * d, b * c); printf("\n");
}
int main() {
    scanf("%lld/%lld %lld/%lld", &a, &b, &c, &d);
    print();
    return 0;
} 
*/

/*
1035
插入与归并

思路：抓住插入排序前面有序，后面未动，去判断 归并排序要重头来，直到到了那一步，再+1

#include<iostream>
#include<algorithm>
using namespace std;
int main()
{
	int n;
	int a[101];
	int b[101];
	cin>>n;
	for(int i=0;i<n;i++) 
	    cin>>a[i];
	for(int i=0;i<n;i++)
	    cin>>b[i];
	int j,k;
	for(j=0;a[j]<=a[j+1]&&j<n-1;j++);//此时的j为乱序的第一位
	for(k=++j;a[k]==b[k]&&j<n;k++); 
	if(k==n)
	{
		cout<<"Insertion Sort"<<endl;
		sort(a,a+j+1);  //注意下标   都放入a数组最后一起输出 
	}
	else 
	{
		cout<<"Merge Sort"<<endl;
		int k=1;
		int flag=1;//标志位判断是否到了给出的那一步，再下一步即可
		while(flag)
		{
			flag=0;
			for(int i=0;i<n;i++)
			{
				if(a[i]!=b[i])  //排序还未到 
				flag=1;
				//归并排序 
				k*=2;
				for(int i=0;i<n/k;i++)   //i为组数，会越来越少，每次组内排序 
				sort(a+i*k,a+(i+1)*k);
				for(int j=k*(n/k);j<n;j++)//排序最后几个
				sort(a+k*(n/k),a+n); 
			}
		} 
	}
	cout<<a[0];
	for(int i=1;i<n;i++)
	{
		cout<<" "<<a[i];
	}
	return 0;
}
//参考答案
#include<iostream>
#include<algorithm>
using namespace std;
int main()
{
        int N;
        int A1[101], A2[101];  // 原始序列A1  中间序列A2
        int i, j;
        cin>>N;
        for( i=0; i<N; i++ )    cin>>A1[i];
        for( i=0; i<N; i++ )    cin>>A2[i];
 
        for( i=0; A2[i]<=A2[i+1] && i<N-1; i++ ) ; // i作为有序序列最后一个元素下标退出循环
        for( j=++i; A1[j]==A2[j] && j<N; j++  ) ;    // A1 A2从 第一个无序的元素开始 逐一比对
        if( j==N ){// 前半部分有序而后半部分未改动可以确定是插入排序
            cout<<"Insertion Sort"<<endl;
            sort( A1, A1+i+1 );
        }
        else{
            cout<<"Merge Sort"<<endl;
            int k = 1;
            int flag=1;         //用来标记是否归并到 “中间序列”
            while( flag )
            {
                    flag = 0;
                    for( i=0; i<N; i++ )
                        if( A1[i]!=A2[i] )
                            flag = 1;
                    k*=2;
                    for( i=0; i<N/k; i++ )
                        sort( A1+i*k, A1+(i+1)*k );
                    for( i=k*(N/k); i<N; i++ ) // 对 非偶数序列的“尾巴”进行排序
                        sort( A1+k*(N/k), A1+N );
            }
        }
        cout<<A1[0];
        for( i=1; i<N; i++ )
            cout<<" "<<A1[i];
        cout<<endl;
        return 0;
} 
*/

/*
1036
输出正方形图案

思路：先判断奇偶求出行数，然后分首位排和中间排按规则输出
#include<iostream>
using namespace std;

int main()
{
	int n;
	char c;
	cin>>n>>c;
	//打印第一排 
	for(int i=0;i<n;i++)
	cout<<c;
	cout<<endl;
	//打印第2至（n-1）排 
	int j;
	if(n%2==0)j=n/2-1;
	else j=n/2;
	while(j>1)
	{
	cout<<c;
	for(int i=0;i<n-2;i++)
	{
		cout<<" ";
	}
	cout<<c<<endl;
	j--;	
	}
	for(int i=0;i<n;i++)
	cout<<c;
	return 0;
}  
*/

/*
1037
找零钱   定义不同的进制 输出找的钱或者差的钱 
#include<iostream>
using namespace std;
int main()
{
	int a1,b1,c1;
	int a2,b2,c2;
	int a3,b3,c3;
	scanf("%d.%d.%d %d.%d.%d",&a1,&b1,&c1,&a2,&b2,&c2);
if((c2+b2*29+a2*29*17)>=(c1+b1*29+a1*29*17))
{
	
   if(c2>=c1)
	{
		c3=c2-c1;
	}
	else {
		c3=c2+29-c1;
		b2--;
	}
	if(b2>=b1)
	{
		b3=b2-b1;
	}
	else{
		b3=b2+17-b1;
		a2--;
	}
	
		a3=a2-a1;
		cout<<a3<<"."<<b3<<"."<<c3<<endl;
}
else   //倒差钱 
	{
		if(c1>=c2)c3=c1-c2;
		else{
			c3=c1+29-c2;
			b1--;
		}
		if(b1>=b2)b3=b1-b2;
		else{
			b3=b1+17-b2;
			a1--;
		}
		a3=a1-a2;
		cout<<"-"<<a3<<"."<<b3<<"."<<c3<<endl;
	}

	return 0;
}
*/

/*
1038  运行超时考虑用scanf和printf输入输出 
统计同成绩学生 给出n个成绩 给出k个查询值，对应输出人数

思路：将n个学生的成绩对应放入百分数数组，输出数组值
#include<iostream>
using namespace std;
int main()
{
	double n;
	int chengji[101]={0};
	scanf("%lf",&n); 
	int score;
	for(double i=0;i<n;i++)
	{
		scanf("%d",&score);
		if((score>=0)&&(score<=100))
		chengji[score]++;
	} 
	int k;
	scanf("%d",&k);
	int chaxun;
	scanf("%d",&chaxun);
	printf("%d",chengji[chaxun]);
	for(int i=0;i<k-1;i++)
	{
		scanf("%d",&chaxun);
    	printf(" %d",chengji[chaxun]);
	}	
	return 0;
}
*/

/*
1039 
到底买不买  给一个字符串 第二个字符串是否全部包含于第一个，若是，输出多余字符个数，若不是，输出差的数量

思路：字符数组遍历 相同则用*号覆盖，最后数*号的个数对应输出 
#include<iostream>
#include<cstring>
using namespace std;
int main()
{
	char ss1[1001];
	char ss2[1001];
	cin>>ss1;
	cin>>ss2;
	for(int i=0;i<strlen(ss2);i++)
	{
		for(int j=0;j<strlen(ss1);j++)
		{
			if(ss2[i]==ss1[j])
			{
				//相同就用*号覆盖 
				ss2[i]='*';  
				ss1[j]='*'; 
			}	
		}
	 } 
	 int count1=0,count2=0;
	 for(int i=0;i<strlen(ss1);i++)
	 {
	 	if(ss1[i]!='*')
	 	{
	 		count1++;
		 }
	  } 
	  for(int i=0;i<strlen(ss2);i++)
	 {
	 	if(ss2[i]!='*')
	 	{
	 		count2++;
		 }
	  }
	  if(count2)
	  {
	  	cout<<"No "<<count2;
	   }
	   else cout<<"Yes "<<count1; 
	return 0;
}
*/

/*
1040 有几个PAT

大神脑回体：逆序逆数并累加
  
#include<iostream>
#include<cstring>
using namespace std;
int main()
{
	char c[100010];
	scanf("%s",c);
	int num=0,numt=0,numa=0;
	int size=strlen(c);
	//逆序操作
	while(size--)
	{
		if('T'==c[size]) numt++;
		else if('A'==c[size]) numa+=numt;
		else  
		{
			num+=numa;
			if(num>=1000000007)num%=1000000007;
		}
	} 
	cout<<num;
	return 0;
}
*/

/*
1041
考试座位号 给考生部分信息，输出考生其他所有信息

思路：定义一个考生类，包括初始函数，读取特定值函数，输出函数，用特定值判别直接调用输出函数即可 
#include<iostream>
#include<cstring>
using namespace std;
class Student
{
	char Kaohao[15];
	int Shijihao;
	int Kaoshihao;
public:
	void Init(char *kaohao,int s,int k)
	{
		strcpy(Kaohao,kaohao);
		Shijihao=s;
		Kaoshihao=k;
	}
	int Get_Shijihao()
	{
		return Shijihao;
	}
	void Show()
	{
		cout<<Kaohao<<" "<<Kaoshihao<<endl;
	}
};

int main()
{
	char kaohao[15];
	int s;
	int k;
	int n;
	cin>>n;
	Student student[n];
	for(int i=0;i<n;i++)
	{
		cin>>kaohao>>s>>k;
		student[i].Init(kaohao,s,k);
	}
	int m;
	cin>>m;
	for(int i=0;i<m;i++)
	{
		int j;
		cin>>j;
		for(int i=0;i<n;i++)
	    {
		if(student[i].Get_Shijihao()==j)
		{
			student[i].Show();
		}
		}
	}
	return 0;
}
*/ 

/*
1042
字符统计 统计一串字符串中出现次数做多的字符，并输出

思路：用cin.get(字符数组名，长度)去读取字符串，遇空格不停止 然后放入两个数组去合并，再比较大小输出 
#include<iostream>
#include<cstring>
using namespace std;
int main()
{
	//用字符数组读取字符串 记录个数 
	char c[1001];
	cin.get(c,1000);
	//将字母按大小写分别放入对应数组 
	int a[26]={0},A[26]={0},he[26]={0};
	for(int i=0;i<strlen(c);i++)
	{
		if(c[i]>='a'&&c[i]<='z')
		{
			a[c[i]-'a']++;
		}
		else if(c[i]>='A'&&c[i]<='Z')
		{
			A[c[i]-'A']++;
		}		
	}
	//归为一个数组，存放的内容是出现次数 
	for(int i=0;i<26;i++)
	{
		he[i]=a[i]+A[i];
	}
	//找到出现次数最多 
	int max=0;
	int i,k;
	for(i=0;i<26;i++)
	{
	    if(he[i]>max)
		{
	    	max=he[i];
	    	k=i;
		}
			
	}
	cout<<(char)(97+k)<<" "<<max;  
	return 0;
}
*/ 

/*
1043
输出PATest   按顺序输出字符串，没有的就忽略 
 
#include<iostream>
#include<cstring>
using namespace std;
int main()
{
	string s;
	cin>>s;
	int a1=0;int a2=0;int a3=0;int a4=0;int a5=0;int a6=0;
	for(int i=0;i<s.size();i++)
	{
		if(s[i]=='P')
		a1++;
		else if(s[i]=='A')
		a2++;
		else if(s[i]=='T')
		a3++;
		else if(s[i]=='e')
		a4++;
		else if(s[i]=='s')
		a5++;
		else if(s[i]=='t')
		a6++;		
	} 
	for(int i=0;i<s.size();i++)
	{
		if(a1)
		{
		cout<<'P';
		a1--;
	    }
	    if(a2)
		{
		cout<<'A';
		a2--;
	    }
	    if(a3)
		{
		cout<<'T';
		a3--;
	    }
	    if(a4)
		{
		cout<<'e';
		a4--;
	    }
	    if(a5)
		{
		cout<<'s';
		a5--;
	    }
	    if(a6)
		{
		cout<<'t';
		a6--;
	    }
	}
	return 0;
}
*/

/*
1044
火星数字  13进制的正反翻译  还有点错误 

#include<iostream>
#include<cmath>//long型的绝对值函数labs() 
#include<algorithm>
using namespace std;
int main()
{
	string low[13]={"tret","jan","feb","mar","apr","may","jun","jly","aug","sep","oct","nov","dec"};
	string high[12]={"tam","hel","maa","huh","tou","kes","hei","elo","syy","lok","mer","jou"}; 
	int n;
	cin>>n;
	getchar();
	for(int i=0;i<n;i++)
	{
		string a;
		getline(cin,a);
		if(a[0]>='0'&&a[0]<='9')
		{
			int shu;
			if(a[1]>='0'&&a[0]<='9')shu=(a[0]-'0')*10+(a[1]-'0');
			else shu=a[0]-'0';
			string out="";
			int yu;
			yu=shu/13;
			if(yu) out+=high[yu-1]+" ";
			out+=low[shu%13];
			cout<<out<<endl;
		}
		else
		{
			int count=0;
			string s1="",s2="";
			s1=a.substr(0,3);
			s2=a.substr(4,3);
			if(a[4]>='a'&&a[4]<='z')//大数   s1遍历high  s2遍历low 
			{
				for(int i=0;i<12;i++)
				{
					if(s1==high[i])
					{
						count+=(i+1)*13;
					}
				}
				for(int j=0;j<13;j++)
				{
					if(s2==low[j])
					{
						count+=j;
					}
				}
		
	         }
	         else
	         {
	         	for(int i=0;i<12;i++)
				{
					if(s1==high[i])
					{
						count+=(i+1)*13;
					}
				}
				for(int j=0;j<13;j++)
				{
					if(s1==low[j])
					{
						count+=j;
					}
				}
	         	
			 }
			cout<<count<<endl;
		}
	}
	return 0;
}

//标准答案
#include <iostream>
#include <string>
using namespace std;
void fun1(string t);
void fun2(string t);
string a[13] = {"tret","jan", "feb", "mar", "apr", "may", "jun", "jly", "aug", "sep", "oct", "nov", "dec"};
string b[13] = {"","tam", "hel", "maa", "huh", "tou", "kes", "hei", "elo", "syy", "lok", "mer", "jou"};
int tt,temp,temp1,ans = 0;
int main(){
	string t;
	int n;
	cin>>n;
	getchar();
	for(int i = 0;i<n;i++){
		getline(cin,t);
		if(t[0]>='0'&&t[0]<='9')fun1(t);//数字转火星文 
		else fun2(t);//火星文转数字 
		cout<<endl;
	}
	return 0;
} 
 
void fun1(string t){
	int len = t.length();
	int num = 0;
	for(int i = 0;i<len;i++)//将字符串变成数字 
	{
		num = num*10+(t[i]-'0');
	}
	cout<<b[num/13];
	if((num%13)&&(num/13))cout<<" "<<a[num%13];//如果有进位和低位 
	else if(num%13)cout<<a[num%13];//没有进位 
	else if(num==0)cout<<"tret";//为零的情况 
}
 
void fun2(string t){
	if(t.size()==3)//只有一个火星文 
	{
		for(int i = 0;i<13;i++){//遍历高位 
			if(t==b[i]){
				cout<<i*13;
				break;
			}
		}
		for(int i = 0;i<13;i++){//遍历低位 
			if(t==a[i]){
				cout<<i;
			}
		}
	}
	else if(t.size()==4)cout<<"0";//有四位肯定是零 
	else						//有两个火星文 
	{
		string aa = t.substr(0,3);//高位火星文                      ！！！！！！！substr()函数 
		string bb = t.substr(4,3);//低位火星文 
		for(int i = 0;i<13;i++){
			if(aa==b[i])temp = i*13;
			if(bb==a[i])temp1 = i;
		}
		cout<<temp+temp1;
	}
}
*/ 

/*
1045  部分正确，段错误 
快速排序  判断某个数是否可能是快速排序中的朱元数

思路：如果能想到，主元的位置与排完序后该元素所在位置相同，
那么再满足它是它之前所有元素中最大的一个，就可以断定它是主元

#include<iostream>
#include<algorithm>
using namespace std;
int main()
{
	int n;
	cin>>n; 
	int a[n];
	int b[n];
	int shu[n];
	//读入元素数组中 
	for(int i=0;i<n;i++)
	{
		cin>>a[i];
		b[i]=a[i];  //复制数组 
	}
	sort(b,b+n); //数组b升序排列
	int max=0,count=0;
	for(int i=0;i<n;i++)
	{
		if(a[i]>max)
		max=a[i];
		if((max==a[i])&&(a[i]==b[i]))  //a[i]为前项的最大值，且在位置上 
		{
			shu[count++]=a[i];
		}
	} 
	cout<<count<<endl;
	for(int i=0;i<count;i++)
	{
		if(i==0)
		cout<<shu[i];
		else
		cout<<" "<<shu[i];
	}
	return 0;
}

//标准答案
#include <iostream>
#include <algorithm>
#include <stdio.h>
using namespace std;
 
int main()
{
   int N;
   scanf("%d",&N);
   int num[N],num_s[N];  //一个原始数据，另一个保存已经排好的数据
   for(int i=0;i<N;i++)
   {
       scanf("%d",&num[i]);
       num_s[i]=num[i];
   }
   sort(num_s,num_s+N);
   int count=0,max=0;
   int res[N];
   
   for(int i=0;i<N;i++)
   {
       if(num[i]>max)
            max=num[i];
        if(max==num[i] && num[i]==num_s[i])  //如果当前数是从第一个数到当前数最大的一个，且与排完顺序对应位置的数相同则该数就有可能是主元
            res[count++]=num[i];
   }
    printf("%d\n",count);
    //sort(res,res+count);
    for(int i=0;i<count;i++)
        if(i==0)
             printf("%d",res[i]);
        else
            printf(" %d",res[i]);
    printf("\n");
    return 0;
}
 
*/

/*
1046
划拳  两人喊数与出的数之间的关系

思路：分n次直接出结果累加输出，不用二维数组做 
 
#include<iostream>
using namespace std;
int main()
{
	int n;
	cin>>n;
	int jia=0,yi=0,c=0;
	int a[4];
	for(int i=0;i<n;i++)
	{
		for(int k=0;k<=3;k++)
			cin>>a[k];
		if(a[0]+a[2]==a[1] &&a[0]+a[2]==a[3])
		{
			continue;
		}
		if(a[0]+a[2]==a[1])
		jia++;
		if(a[0]+a[2]==a[3])
		yi++;
	} 

	cout<<yi<<" "<<jia<<endl;
	return 0;
}
*/

/*
1047 渣渣题 

#include<iostream>
using namespace std;
int main()
{
	int n;
	cin>>n;
	int team[1001]={0}; 
	int dui,ren,score;
	for(int i=0;i<n;i++)
	{
		scanf("%d-%d %d",&dui,&ren,&score);
		team[dui]+=score;		
	}
	int max=0;
	for(int i=1;i<1001;i++)
	{
		if(team[i]>team[max])
		max=i;
	}
	cout<<max<<" "<<team[max];
	return 0;
}
*/

/*
1048
数字加密

思路：玩正序逆序 
#include<iostream>
using namespace std;
int main()
{
	string s1,s2;
	cin>>s1;
	cin>>s2;
	int max=(s1.size()>s2.size()?s1.size():s2.size());
	int a[100]={0};
	int b[100]={0};
	char c[100]={0}; 
	int k=0;
	//将字符串折算成数字对应放入数组 并反序  即个位在前 
	for(int i=s1.size()-1;i>=0;i--)
	{
		a[k]=s1[i]-'0';
		k++;
	}
	int u=0;
	for(int j=s2.size()-1;j>=0;j--)
	{
		b[u]=s2[j]-'0';
		u++;
	} 
	for(int i=0;i<max;i++)
	{
		if(i%2==0)   //实际为奇数位   对应奇数位规则 
		{
			int temp;
			temp=(a[i]+b[i])%13;
			if(temp>=0&&temp<=9)
			{
				c[i]=temp+'0';
			}
			else if(temp==10)c[i]='J';
			else if(temp==11)c[i]='Q';
			else if(temp==12)c[i]='K';
		}
		else if(i%2==1)  //实际为偶数位
		{
			int tem=b[i]-a[i];
			if(tem>=0)
			{
				c[i]=tem+'0';
			}
			else 
			{
				tem=tem+10;
				c[i]=tem+'0';
			}
		} 
	} 
	for(int i=max-1;i>=0;i--)
	{
		cout<<c[i];
	}
	return 0;
}
*/ 

/*
1049 
数列的片段和   片段必须保证连续 

思路：找关系，画图，发现 sum+=(long long int)(n-i)*(i+1)*a[i];
 
#include<iostream>
using namespace std;
int main()
{
	int n;
	cin>>n;
	double a[n]; 
	float sum=0.00;
	for(int i=0;i<n;i++)
	{		
		cin>>a[i];
		sum+=(long long int)(n-i)*(i+1)*a[i];		
	} 
	printf("%.2f",sum);
	return 0;
}
*/

/*
1051
复数乘法
渣渣题 
#include<iostream>
#include<math.h>
using namespace std;
int main()
{
	double a,b,c,d;
	cin>>a>>b>>c>>d;
	double A1,B1,A2,B2;
	A1=a*cos(b);B1=a*sin(b);
	A2=c*cos(d);B2=c*sin(d);
	double C,D;
	//C+Di
	C=A1*A2-B1*B2;
	D=A1*B2+A2*B1;
	if(-0.005<C&&C<0)cout<<"0.00";
	else printf("%.2f",C);
	if(D>=0)printf("+%.2fi",D);
	else if(-0.005<D&&D<0)cout<<"+0.00i";
	else printf("%.2fi",D);
	return 0;
}
*/

/*
1052
卖个萌 

#include<iostream>
using namespace std;
int get(char p[][4])  //内部修改二维字符数组，并返回总数量值 
{
	char c;
	int i=0,j=0;
	while((c=getchar())!='\n') 
	{
		if(c=='[')
		{
			while((c=getchar())!=']')
			{
				if(c=='\n')return (i-1);
				p[i][j]=c;
				j++;
			}
			p[i][j]='\0';
			i++;
			j=0;
		}
	}
	return (i-1);
}
int main()
{
	char hand[10][4],eye[10][4],mouth[10][4];
	int hct,ect,mct;
	hct=get(hand);
	ect=get(eye);
	mct=get(mouth);
	int count;
	cin>>count;
	for(int i=0;i<count;i++)
	{
		int a1,a2,a3,a4,a5;
		cin>>a1>>a2>>a3>>a4>>a5;
		//越界判断
		if(a1<0||a2<0||a3<0||a4<0||a5<0) 
		{
			cout<<"Are you kidding me? @\/@"<<endl;
		}
		else if(--a1>hct||--a5>hct||--a2>ect||--a4>ect||--a3>mct)
		{
			cout<<"Are you kidding me? @\/@"<<endl;
		}
		else
		{
			printf("%s(%s%s%s)%s\n",hand[a1],eye[a2],mouth[a3],eye[a4],hand[a5]);
			//cout<<hand[a1]<<"("<<eye[a2]<<mouth[a3]<<eye[a4]<<")"<<hand[a5]<<endl;
		}
	}	
	return 0;
} 
//标准答案
#include <math.h>
#include <stdio.h>
char hand[10][5],eye[10][5],mouse[10][5];
int  get_symbol(char p[][5]) //读取符号
{
	char c,i=0,j=0;
	while( (c=getchar()) !='\n')
	{
		if( c == '[')
		{
			while( (c=getchar()) != ']' )
			{
				if(c == '\n')    
					return (i-1);
				p[i][j] = c;
				j++;
 
			}
			p[i][j] = '\0'; 
			i++;
			j=0;
		}
	}
	return (i-1);
}
 
main()
{
	int hand_count,eye_count,mouse_count;
	int n,i,a1,a2,a3,a4,a5;
 
	 hand_count = get_symbol(hand);
	eye_count = get_symbol(eye);
	mouse_count = get_symbol(mouse);
	scanf("%d",&n);
	for(i=0;i<n;i++)
	{
		scanf("%d%d%d%d%d",&a1,&a2,&a3,&a4,&a5);
		if(--a1 > hand_count || --a5 > hand_count || --a2 > eye_count || --a4 > eye_count || --a3 > mouse_count) //注意下标越界的情况
		{
			puts("Are you kidding me? @\\/@");
		}
		else if
			(a1 < 0 || a2 < 0 || a3 < 0 || a4 < 0 || a5< 0)  //注意下标小于0的情况
		{
			puts("Are you kidding me? @\\/@");
		}
		else
		{
			printf("%s(%s%s%s)%s\n",hand[a1],eye[a2],mouse[a3],eye[a4],hand[a5]);
		}
	}
 
	return 0;
}
*/ 

/*
1053
住房空置率
渣渣题 
#include<iostream>
using namespace std;
int main()
{
	int n;
	cin>>n;
	float e;
	cin>>e;
	int guancha;
	cin>>guancha;
	int maybe=0,surely=0;
	for(int i=0;i<n;i++)
	{
		int day;
		cin>>day;
		int count=0;
		for(int j=0;j<day;j++)
		{
			float temp;
			cin>>temp;
			if(temp<e)
			count++;
		}
		if(count>day/2)
		{
			if(day>guancha)
			surely++;
			else maybe++;
		}
	} 
	printf("%.1lf%%",(double)maybe/n*100);
	printf(" %.1lf%%",(double)surely/n*100);
	return 0;
}
*/

/*
1054
求平均值    怀疑人生 
sscanf() – 从一个字符串中读进与指定格式相符的数据   sscanf(a,"%lf",&temp)  a->temp 
sprintf() – 字符串格式化命令，主要功能是把格式化的数据写入某个字符串中。 sprintf(b,"%.2lf",temp)  temp->b

#include<iostream>
#include<cstdio>//sscanf()与sprintf()头文件 
#include<cstring>
using namespace std;
int main()
{
	int n;
	cin>>n;
	char a[50],b[50];
	double temp,sum=0.0;
	int count=0;
	for(int i=0;i<n;i++){
		scanf("%s",a);
		sscanf(a,"%lf",&temp);
		sprintf(b,"%.2lf",temp);
		int flag=0;
		for(int j=0;j<strlen(a);j++){
			if(a[j]!=b[j]) flag=1;
		}
		if(flag || temp<-1000 || temp>1000){
			printf("ERROR: %s is not a legal number\n",a);
			continue;
		}
		else{
			sum+=temp;
			count++;
		}
	}
	if(count==1){
		printf("The average of 1 number is %.2lf",sum);
	}
	else if(count>1){
		printf("The average of %d number is %.2lf",count,sum/count);
	}
	else{
		 printf("The average of 0 numbers is Undefined");
	}
	return 0;
}
//标准答案
#include <iostream>
#include <cstdio>
#include <string.h>
using namespace std;
int main() {
    int n, cnt = 0;
    char a[50], b[50];
    double temp, sum = 0.0;
    cin >> n;
    for(int i = 0; i < n; i++) {
        scanf("%s", a);
        sscanf(a, "%lf", &temp);
        sprintf(b, "%.2lf",temp);
        int flag = 0;
        for(int j = 0; j < strlen(a); j++) {
            if(a[j] != b[j]) flag = 1;
        }
        if(flag || temp < -1000 || temp > 1000) {
            printf("ERROR: %s is not a legal number\n", a);
            continue;
        } else {
            sum += temp;
            cnt++;
        }
    }
    if(cnt == 1) {
        printf("The average of 1 number is %.2lf", sum);
    } else if(cnt > 1) {
        printf("The average of %d numbers is %.2lf", cnt, sum / cnt);
    } else {
        printf("The average of 0 numbers is Undefined");
    }
    return 0;
} 
*/

/*
1056 渣渣题 

#include<iostream>
#include<math.h>
using namespace std;
int main()
{
	int n;
	cin>>n;
	int sum=0;
	for(int i=0;i<n;i++)
	{
		int k;
		cin>>k;
		for(int i=0;i<2;i++)
		{
			sum+=k*pow(10,i);
		}	
	}
	cout<<(n-1)*sum; 
	return 0;
}
*/

/*
1057    
数零一  给出一个字符串，每一位字母对应0-26，将序号相加，得到整数N，再转换为二进制数，数其中零和一的个数

思路：字符串读入，每一位判断并输出序号相加，和转换为二进制数，放入数组来数
c语言函数gets()可读取一行，包括空格，但是c++中报错
c++中getline()的用法 getline（cin,s) 用于字符串与字符数组都可以 
cin.getline(s,50)用于字符数组s[50] 
#include<iostream>
#include<cstring>
#include<string>
using namespace std;
int main()
{
	string s;
	getline(cin,s);
	int sum=0;
	for(int i=0;i<s.size();i++)
	{
		if(s[i]>='A'&&s[i]<='Z')
		{
		    sum+=s[i]-'A'+1;	
		}
		if(s[i]>='a'&&s[i]<='z')
		{
		    sum+=s[i]-'a'+1;	
		}
	} 
    int shu[100];
    int k=0;
    while(sum)
    {
    	shu[k]=sum%2;
    	k++;
    	sum/=2;
	}
	int count0=0,count1=0;
	for(int i=0;i<k;i++) 
	{
		if(shu[i]==0)count0++;
		if(shu[i]==1)count1++;
	}
	cout<<count0<<" "<<count1;
	return 0;
}
*/

/*
1058
选择题

思路：用结构体存入每道题信息，包括string+=char
      用scanf去读取每道题答题情况。string对比判定是否得分

#include<iostream>
using namespace std;
struct daan
{
	int score;
	int count_zong;
	int count_dui;
	string ans;
};
int main()
{
	int renshu,timushu;
	cin>>renshu>>timushu;
	//存储题目信息 
	daan d[105];
	for(int i=0;i<timushu;i++)
	{
		cin>>d[i].score>>d[i].count_zong>>d[i].count_dui;
		char c;
		for(int j=0;j<d[i].count_dui;j++)
		{
			cin>>c;
			d[i].ans+=c;
		}
		
	}
	int wrong[timushu]={0};
	for(int i=0;i<renshu;i++){
		int sco=0;
//		getchar();//?????????????
        scanf("\n");
		for(int j=0;j<timushu;j++)
		{
			if(j!=0)scanf(" ");
			string str;
			int k;
			char s;
			scanf("(%d",&k);
			for(int q=0;q<k;q++){
				scanf(" %c",&s);
				str+=s;
			}
			scanf(")");
			if(str==d[j].ans)sco+=d[j].score;
			else wrong[j]++;
		}
		printf("%d\n",sco);
	}
	int max=-1;
	for(int i=0;i<timushu;i++)
	{
		if(max<wrong[i])max=wrong[i];
	}
	if(max==0)cout<<"Too simple";
	else{
		cout<<max;
		for(int i=0;i<timushu;i++){
			if(max==wrong[i])cout<<" "<<i+1;
		}
	}
	return 0;
} 
*/

/*
1059 C语言竞赛

思路：用数组装序号，数组下标即为名次，查询数先看是否在数组，再返回下标判断奖品

#include<iostream>
#include<cmath>
using namespace std;
int Is(int n)   //判断素数程序 
{
	int flag=1;
	for(int i=2;i<=sqrt(n);i++)
	{
		if(n%i==0)flag=0;
	}
	return flag;
}
int main()
{
	int n;
	cin>>n;
	int a[n+1];
	a[0]=-1;
	int b[10001]={0};
	for(int i=1;i<=n;i++)
	{
		cin>>a[i];
	}
	int k;
	cin>>k;
	while(k--)
	{
		int shu;
		int paiming=0;
		cin>>shu;
		for(int i=1;i<=n;i++)
	    {
		if(shu==a[i]){
			b[shu]++;
			paiming=i;		
		}
		}
		if(b[shu]>1)
			{
				printf("%04d",shu);
				cout<<": Checked"<<endl;
			}
		else{
				
	    if(paiming==0)
	    {
	    	printf("%04d",shu);
	    	cout<<": Are you kidding?"<<endl;
		}
		else if(paiming==1)
		{
			printf("%04d",shu);
			cout<<": Mystery Award"<<endl;
		}
		else if(Is(paiming))
		{
			printf("%04d",shu);
			cout<<": Minion"<<endl;
		}
		else 
		{
			printf("%04d",shu);
			cout<<": Chocolate"<<endl;
		}
	}
	}
	return 0;
} 
*/

/*
1060   有错误 感觉题目没读懂 
爱丁顿数 
#include<iostream>
#include<algorithm>
using namespace std;
int main()
{
	long n;
	cin>>n;
	int a[1000001];
	for(long i=0;i<n;i++)
	{
		cin>>a[i];
	}
	sort(a,a+n);  //升序 
	int count=0;
	int shu;
	for(long i=n-1;i>=0;i++)
	{
		shu=a[i];
		for(long j=0;j<n;j++)
		{
			if(a[j]>shu)count++;
		}
		if(count>=shu)
		{
			cout<<count;
			break;
		}
		else count=0;
	}
	return 0;
}

//标准答案
#include<iostream>
#include<algorithm>
using namespace std;
 
int main(){
  	int n;
 	cin>>n;
 	int a[n];
 	for(int i=0 ;i<n ;i++){
  		cin>>a[i];
 	}
  	sort(a,a+n);
  	int ans = 0;
  	int i;
  	if(a[0]>n){
    	cout<<n<<endl;
  	}
	else{
		for(i=n-1 ;i>=0 ;i--){
  			if(a[i]<=n-i){
  				ans = n-i-1;
  			break;
	  	}
  	}
  	cout<<ans<<endl;
}
  	return 0;
}
*/ 

/*
1061 渣渣题
 
#include<iostream>
using namespace std;
int main()
{
	int shu;
	int n;	
	cin>>shu>>n;
	int a[n];  //分值
	int b[n];  //答案
	int c[n]; 
	for(int i=0;i<n;i++)
	{
		cin>>a[i];
	} 
	for(int i=0;i<n;i++)
	{
		cin>>b[i];
	} 
	for(int i=0;i<shu;i++)
	{
		int sum=0;
		for(int i=0;i<n;i++)
	{
		cin>>c[i];
		if(c[i]==b[i])
		{
			sum+=a[i];
		}
	}
	cout<<sum<<endl;
	}
	return 0;
}
*/

/*
1062
最简分数  给定分母，给定范围，求出这个范围里满足条件的分数
 
#include<iostream>
using namespace std;
int main()
{
	int a1,a2,b1,b2,fenmu;
	scanf("%d/%d %d/%d %d",&a1,&a2,&b1,&b2,&fenmu);
	int a[fenmu]={0};
	int k=0;
	//暂时未判断1/分母 
	float low=(float)a1/(float)a2;
	float high=(float)b1/(float)b2;
	if(low>high)
	{
		float c;
		c=low;
		low=high;
		high=c;
	}
	float temp=(float)1/(float)fenmu;	
	if(temp>low&&temp<high)
			{
				a[0]=1;
				k++;
			}
	for(int i=2;i<fenmu;i++)
	{
		if(fenmu%i!=0)
		{
			float temp=(float)i/(float)fenmu;
			if(temp>low&&temp<high)
			{
				a[k]=i;
				k++;
			}
		}
	}
	printf("%d/%d",a[0],fenmu);
	for(int j=1;j<k;j++)
	{
		printf(" %d/%d",a[j],fenmu);
	}
	return 0;
}
//标准答案
#include<cstdio>
using namespace std;
double a,b,c,d,e; 
double a1,a2,a3;
 
int Gcd(int  a,int b){
	return a%b==0 ? b : Gcd(b,a%b);
}
 
int main(){
	scanf("%lf/%lf",&a,&b); a1 = a/b;
	scanf("%lf/%lf",&c,&d); a2 = c/d;
	if(a1>a2){
		double temp = a1;
		a1 = a2;
		a2 = temp;
	}
	
	scanf("%lf",&e);
 
	int flag = 1;
	for(double i=1 ;i<e ;i++){
		a3 = i/e;
		if(a1<a3&&a3<a2){
			if(Gcd(i,e)==1){
				if(flag){
				flag = 0;
					printf("%.0f/%.0f",i,e);
				}else{
					printf(" %.0f/%.0f",i,e);
				}
			}
			
		}
	}
	printf("\n"); 
	
	return 0;
}
 */
 
/*
1063
渣渣题 
统计最大数输出 
#include<iostream>
#include<math.h> 
using namespace std;
float mo(float a,float b)
{
	return sqrt(a*a+b*b);
}
int main()
{
	int n;
	cin>>n;
	float shuzu[n];
	for(int i=0;i<n;i++)
	{
		int a,b;
		cin>>a>>b;
		shuzu[i]=mo(a,b);
	}
	for(int i=0;i<n-1;i++)
	{
		for(int j=i+1;j<n;j++)
		if(shuzu[i]<shuzu[j])
		{
			float temp;
			temp=shuzu[i];
			shuzu[i]=shuzu[j];
			shuzu[j]=temp;
		}
	}
	printf("%.2f",shuzu[0]);
	return 0;
}
*/

/*
1064
朋友数 各数位和相同即为朋友数，给出一系列数，按增序输出所有朋友数 

思路：朋友数最大为36，用一个数组来装每个数对应的朋友数 
#include<iostream>
using namespace std;
//函数返回各数位和
int he(int n)
{
	int he=0;
	while(n>0)
	{
		he+=n%10;
		n/=10;		
	}
	return he;
 } 
int main()
{
	int n;
	cin>>n;
	int shu;
	int pengyoushuzu[40]={0};
	for(int i=0;i<n;i++)
	{
		cin>>shu;
		pengyoushuzu[he(shu)]++;
	}
	int count=0;
	for(int i=0;i<40;i++)
	{
		if(pengyoushuzu[i])
		{
			count++;
		}
	}
	cout<<count<<endl;
	int i=0;
	for(;count>1;i++)
	{
		if(pengyoushuzu[i])
		{
			cout<<i<<" ";
			count--;
		}
	}
	int k=1;
	for(;k>0;i++)
	{
		if(pengyoushuzu[i])
		{
			cout<<i;
			k--;
		}
	}
	return 0;
}
*/

/*
1065
单身狗

思路：先数组置为-1，存入cp信息互为彼此 用guest[]数组放入来宾，在list[]上cp处为1，
      再去list[]上遍历，若为1，则配偶来了 不为1的放入set，set自动升序，输出即可  

#include<iostream>
#include<set>
using namespace std;
#define MAX 100001
int main() 
{
	int id[MAX]={-1},guest[10001]={0},list[MAX]={0};
	int cpdui;
	cin>>cpdui;
	int male,female;
	for(int i=0;i<cpdui;i++)
	{
		cin>>male>>female;
		//在id[]中互相存入彼此，证明存在 
		id[male]=female;
		id[female]=male;
	}
	int count;
	cin>>count;
	//存入来宾信息 
	for(int j=0;j<count;j++)
	{
		cin>>guest[j];
		if(id[guest[j]]!=-1)  //此人有配偶 
        //配偶信息对应在list[]中置为1 		
        list[id[guest[j]]]=1;
	}
	set<int>s; 
	//遍历list[]，为1证明都到了 
	for(int j=0;j<count;j++)
	{
	if(!list[guest[j]])
		s.insert(guest[j]);
	}
	cout<<s.size()<<endl; 
	for(set<int>::iterator it =s.begin();it!=s.end();it++)
	{
		if(it!=s.begin())
		cout<<" ";
		printf("%05d",*it);
	 } 
	return 0;
}
*/

/* 
1066
图像过滤
渣渣题 
#include<iostream>  //scanf读取 
using namespace std;
int main()
{
	int m,n,low,high,tidai;
	cin>>m>>n>>low>>high>>tidai;
	for(int i=0;i<m;i++)
	{
		int temp;
		scanf("%d",&temp);
		if((temp>=low)&&(temp<=high))temp=tidai;
		printf("%03d",temp);
		for(int j=1;j<n;j++)
		{
			scanf("%d",&temp);
			if((temp>=low)&&(temp<=high))
			{
			temp=tidai;
			}
			printf(" %03d",temp);
		}
		cout<<endl;
	}
	return 0;
}
*/ 

/*
1067 试密码

注意：再一次getchar()去消尾（清空回车符），用getline(cin,s)去读一整行 

#include<iostream>
#include<cstring>
using namespace std;
int main()
{
	string mima;
	int n;
	cin>>mima>>n;
	getchar();
	int count=0;
	for(int i=0;i<n;i++)
	{
		string s;
        getline(cin,s);
        if(s=="#")break;
		if(s==mima)
		{
			cout<<"Welcome in"<<endl;
			break;
		}
		else 
		{
			cout<<"Wrong password: "<<s<<endl;
			count++;
			if(count==n){
				cout<<"Account locked"<<endl;
				break;
			}
		}
	}
	return 0;
}
*/

/*
1068  并不知道哪里错了 
万绿丛中一点红 
 
//map映射判重最好用

#include<iostream>
#include<cmath>
#include<map>
using namespace std;

map<int,int>zhi;
int m,n,yuzhi;
int a[1001][1001];	
bool panduan(int x,int y)
{
	int zuobiao[8][2]={1,0,-1,0,0,1,0,-1,1,1,1,-1,-1,1,-1,-1};
	for(int i=0;i<8;i++)
	{
		int xx,yy;
		xx=x+zuobiao[i][0];
		yy=y+zuobiao[i][1];
		if(xx>=0 && xx<n && yy<m && yy>=0 && abs(a[xx][yy]-a[x][y])<=yuzhi) return false; 
	}
	return false;
}
int main()
{	
cin>>m>>n>>yuzhi;

	for(int i=0;i<n;i++)
	{
		for(int j=0;j<m;j++)
		{
			cin>>a[i][j];
			zhi[a[i][j]]++;
		}
	}
	int count=0;
	int c1,c2,c3;
	for(int i=0;i<n;i++)
	{
		for(int j=0;j<m;j++)
		{
			if(zhi[a[i][j]]==1&&panduan(i,j))
			{
				count++;
				c1=i;
				c2=j;
				c3=a[i][j];
			}
		}
	}
	if(count==0)cout<<"Not Exist"<<endl;
	else if(count>1)cout<<"Not Unique"<<endl;
	else if(count==1)
	{
		cout<<"("<<c2<<", "<<c1<<"): "<<c3<<endl;
	}
	
	
	return 0;
}
//标准答案
#include<cstdio>
#include<iostream>
#include<cstring>
#include<map>
#include<cmath>
using namespace std;
 
map<int, int> vis;
int s[1001][1001];
int n, m ,tol;
int dir[8][2] = {1,0, -1,0, 0,1, 0,-1, 1,1, 1,-1, -1,1, -1,-1};
//判断是否大于阈值 
bool check(int x, int y)
{
	for(int i=0 ;i<8 ;i++){
		int xx = x + dir[i][0];
		int yy = y + dir[i][1];
		if(xx>=0 && xx<n && yy<m && yy>=0 && abs(s[xx][yy]-s[x][y])<=tol ) return false; 
	}
	return true;
}
 
int main(){
	cin>>m>>n>>tol;
	
	for(int i=0 ;i<n ;i++){
		for(int j=0 ;j<m ;j++){
			cin>>s[i][j];
			vis[s[i][j]] ++;  	
		}
	}
	//cnt记录只出现一次的数字的个数
	//x y记录坐标 
	int cnt = 0;
	int x, y;
	for(int i=0 ;i<n ;i++){
		for(int j=0 ;j<m ;j++){
			if(vis[s[i][j]]==1 && check(i,j)){
				cnt++;
				x = i;
				y = j;
			}	
		}
	}
	
	if(cnt==1){
		printf("(%d, %d): %d\n",y+1, x+1, s[x][y]);
	}
	else if(cnt>1){
		puts("Not Unique");
	}
	else{
		puts("Not Exist");
	}
	
	return 0;
}*/

/*
1069   运行超时 
微博转发抽奖  给出转发数，抽奖间隔和第一位序号 满足条件且第一次就输出，满足条件数为零输出特定语句 

#include<iostream>
#include<cstring> 
using namespace std;
int main()
{
	int zhuan,jiange,shouwei;
	cin>>zhuan>>jiange>>shouwei;
	if(shouwei>zhuan)
	cout<<"Keep going..."<<endl;
	else
	{
		char s[1010][30];  //转发的所有人昵称 
		char ss[1010][30]={0};  //存放已中奖人的昵称，用于判断是否重复 
		int k=0;
		for(int i=1;i<=zhuan;i++)
		{
			cin>>s[i];
		}
		for(int j=shouwei;j<=zhuan;)
		{
			for(int i=0;i<k;i++)
			{
					if(strcmp(s[j],ss[k])==0)
		    	{
		    		j=j+1;
		    		break;
				}
			}
		            cout<<s[j]<<endl;
		    		ss[k]==s[j];
		    		k++;
		    		j=j+jiange-1;	
		}
	}
	return 0; 
} 

//标准答案 大佬神操作，用映射 
#include<iostream>
#include<map>
using namespace std;
int main()
{
	int zhuan,jiange,shouwei;
	cin>>zhuan>>jiange>>shouwei;
	map<string,int>choujiang;
	int flag=0;
	string str;
	for(int i=1;i<=zhuan;i++)
	{
		cin>>str;
		if(choujiang[str]==1)shouwei++;
		if(i==shouwei&&choujiang[str]==0)
		{
			choujiang[str]=1;
			cout<<str<<endl;
			flag=1;
			shouwei+=jiange;
		}
	}
	if(flag==0)cout<<"Keep going..."<<endl;
	return 0;
}
*/ 

/*
1070
结绳

思路：数组读入，并使用排序函数使得数组有序，升序，再依次操作 
#include <iostream>
#include <string>
#include <algorithm>  //sort()函数 
using namespace std;
int main()
{
    int n,a[10004];
    cin>>n;
    for(int i=0;i<n;i++) 
	cin>>a[i];
    sort(a,a+n);  //自动排序函数 
    double ans=a[0];
    for(int i=1;i<n;i++)
    {
        ans+=a[i];
        ans/=2;
    }
    cout<<(int)ans;
    return 0;
}
*/

/*
1071 
小赌怡情 

#include<iostream>
using namespace std;
int main()
{
	int qian,lunshu;
	int isprime=0;
	cin>>qian>>lunshu;
	for(int i=0;i<lunshu;i++)
	{
		int a,b;
		int cai;
		int duzi;
		cin>>a>>cai>>duzi>>b;
		if(duzi>qian)
		{
			cout<<"Not enough tokens.  Total = "<<qian<<"."<<endl;
		}
		else 
		{
		if(((cai==0)&&(a>b))||((cai==1)&&(a<b)))
		{
			qian+=duzi;
			cout<<"Win "<<duzi<<"!  Total = "<<qian<<"."<<endl;
		}
		
		else if(((cai==0)&&(a<b))||((cai==1)&&(a>b)))
		{
			qian-=duzi;
			cout<<"Lose "<<duzi<<".  Total = "<<qian<<"."<<endl;
			if(qian==0)
			{
				isprime=1;
				break;
			}
		}
		}	
	} if(isprime==1)
	{
		cout<<"Game Over.";
	}	
	return 0;
} 
*/

/*
1072   printf(" %04d",shoujiao[q]);   //注意输出格式!!!	
开学寄语 将对应字符串输出，输出人数与字符串数量 

#include<iostream> 
using namespace std;
int main()
{
 	int menber,zhonglei;
 	int renshu=0,wupinshu=0;
 	cin>>menber>>zhonglei;
 	int a[zhonglei]={0};
 	for(int i=0;i<zhonglei;i++)
 	{
 		cin>>a[i];
	}
	for(int j=0;j<menber;j++)
	{
		string s;
		int num;
		cin>>s;
		cin>>num;
		int shoujiao[num]={0};
		int k=0;
		for(int r=0;r<num;r++)
		{
			int temp;
			cin>>temp;
			for(int i=0;i<zhonglei;i++)
			{
				if(temp==a[i])
				{
					wupinshu++;
					shoujiao[k]=temp;
					k++;
				}
			}
		}
		if(k)
		{
		int w=k;	
			renshu++;
			cout<<s<<":";
			for(int q=0;q<w;q++)
			{
				printf(" %04d",shoujiao[q]);   //注意输出格式!!!	
			}
			cout<<endl;
			k=0;
			
		}shoujiao[num]={0};
	}
	cout<<renshu<<" "<<wupinshu; 
 	return 0;
}
*/ 

/*
#include<iostream>
using namespace std;
struct daan
{
	int score;
	int count_all;
	int count_right;
	string ans;
};
int main()
{
	int renshu;
	int timushu;
	daan d[102];
	
	for(int i=0;i<timushu;i++)
	return 0;
	{
		cin>>d[i].score>>d[i].count_all>>d[i].count_right;
		char c;
		for(int j=0;j<d[i].count_right;j++)
		{
			cin>>c;
			d[i].ans+=c;
		}
	}
	int wrong[102]={0};
	for(int i=0;i<renshu;i++)
	{
		scanf("\n");
		for(int j=0;j<timushu;j++)
		{
			int sco=0;
			if(j!=0)scanf(" ");
			int k;
			scanf("(%d",&k);
			string s;
			for(int q=0;q<k;q++)
			{
				char cc;
				scanf(" %c"&cc);
				s+=cc;
			}
			scanf(")");
			if(s==d[j].ans)sco+=d[j].score;
			else wrong[j]++;
		}
	}
} 

//标准答案 
#include<iostream>
using namespace std;
struct Ques {
    int qscore;     // 满分
    int qsnum;      // 选项个数
    int qrnum;      // 正确选项个数
    int qawr[128];  // 所有正确选项   abcd对应的asc11码 
};
typedef struct Ques s_ques;

#define LEN 120
int main (void) {
    int snum;           // 学生人数
    int qnum;           // 题目个数
    s_ques ques[LEN];   // 每道题的信息
    int wrong[LEN][128] = {0}; // 错误信息矩阵
    int wmax = 0;              // 最大错误次数
    char ch;
    int i;
    int j;
    int k;

    scanf("%d %d", &snum, &qnum);
    for (i = 1; i <= qnum; i++) {
        ques[i].qawr['a'] = 0;
        ques[i].qawr['b'] = 0;
        ques[i].qawr['c'] = 0;
        ques[i].qawr['d'] = 0;
        ques[i].qawr['e'] = 0;
        scanf("%d %d %d", &ques[i].qscore, &ques[i].qsnum, &ques[i].qrnum);
        for (j = 1; j <= ques[i].qrnum; j++) {
            scanf(" %c", &ch);
            ques[i].qawr[(int)(ch)] = 1;
        }
    }
    getchar(); // 回车挡掉

    int len;   // 初始是第1道题
    int mys;   // 这个学生选了几个选项
    char slt;  // 选中选项
    for (i = 1; i <= snum; i++) {
        double score = 0.0;  // 每个学生的初始分数
        len = 1;
        // 一个 while 循环是一位学生答题的全部情况
        while ((ch = getchar()) != '\n') {
            if (ch == '(') {
                int cawr[128] = {0}; // 用来记录该题答题情况, 有1, 无0
                scanf("%d", &mys);
                for (j = 1; j <= mys; j++) {
                    scanf(" %c", &slt);
                    cawr[(int)(slt)] = 1;
                }

                // 学生这个选项答了, 而正确答案没有, 相应错误+1
                for (k = 'a'; k <= 'e'; k++) {
                    if (cawr[k] != ques[len].qawr[k]) wrong[len][k]++;
                    if (wrong[len][k] > wmax) wmax = wrong[len][k];
                }

                if (
                    // 所有选项全部匹配, 为完全正确
                    cawr['a'] == ques[len].qawr['a'] &&
                    cawr['b'] == ques[len].qawr['b'] &&
                    cawr['c'] == ques[len].qawr['c'] &&
                    cawr['d'] == ques[len].qawr['d'] &&
                    cawr['e'] == ques[len].qawr['e']
                ) {
                    score += (double)(ques[len].qscore);
                } else if (
                    // 这一项学生答了, 而正确答案没有这一项, 该题答错
                    (cawr['a'] == 1 && ques[len].qawr['a'] == 0) ||
                    (cawr['b'] == 1 && ques[len].qawr['b'] == 0) ||
                    (cawr['c'] == 1 && ques[len].qawr['c'] == 0) ||
                    (cawr['d'] == 1 && ques[len].qawr['d'] == 0) ||
                    (cawr['e'] == 1 && ques[len].qawr['e'] == 0) 
                ) {
                    // 该题答错
                    // do nothing
                } else {
                    // 部分正确
                    score += (double)(ques[len].qscore) / 2.0;
                }
                len++; // 进入到下一题
            }
        }
        printf("%.1f\n", score); // 打印这位同学的得分
    }

    if (wmax == 0) {
        printf("Too simple\n");
    } else {
        for (i = 1; i <= qnum; i++) {
            for (j = 'a'; j <= 'e'; j++) {
                if (wrong[i][j] == wmax) {
                    printf("%d %d-%c\n", wrong[i][j], i, j);
                }
            }
        }
    }

    return 0;
}
*/

/*
1074
宇宙无敌加法器 每一位规定不同的进制 输出两个数相加后的结果

思路：用三个字符数组读入三个数，转化为数字数组 逆序相加，求余求商，商进位，最后再逆序输出 
 
#include <iostream>
using namespace std;
int main()
{
    string radix, num1, num2;
    cin >> radix >> num1 >> num2;
    //将num1和num2的长度转为相同
    if (num1.length() > num2.length()) {
        string temp = "";
        for (int i = 0; i < num1.length() - num2.length(); i++) {
            temp += "0";
        }
        num2 = temp + num2;   //字符串可以直接加 
    }
    if (num2.length() > num1.length()) {
        string temp = "";
        for (int i = 0; i < num2.length() - num1.length(); i++) {
            temp += "0";
        }
        num1 = temp + num1;
    }
    int temp = radix.length() - 1;
    int carry = 0;   //进位位 
    string result = "";
    //对各个位进行运算
    for (int i = num1.length() - 1; i >= 0; i--) {
        int sum = (num1[i] - '0') + (num2[i] - '0') + carry;
        int r = radix[temp] - '0';
        temp--;
        if (r == 0) {   //十进制 
            r = 10;
        }
        carry = sum / r;
        result = (char)(sum % r + '0') + result ;
    }
    //有进位且还有进制
    while (carry != 0 && temp >= 0) {
        int sum = carry;
        int r = radix[temp] - '0';
        temp--;
        if (r == 0) {
            r = 10;
        }
        carry = sum / r;
        result = (char)(sum % r + '0') + result ;
    }
    //有进位但没进制，则只需一次
    if (carry != 0) {
        result = (char)(carry + '0') + result ;
    }
    int tag = 0;
    for (int i = 0; i < result.length(); i++) {
        if (tag == 1) {
            cout << result[i];
            continue;
        }
        if (result[i] != '0') {
            if (tag == 0) {
                tag = 1;
            }
            cout << result[i];
        }
    }
    if (tag == 0) {
        cout << 0;
    }
}
*/

/*
1075
链表元素分类

思路：根据条件放入不同向量再归并输出
#include<iostream>
#include<vector>
using namespace std;
struct node
{
	int add;
	int data;
	int next;
};
int main()
{
	vector<node>in(100001);
	vector<node>sorted;
	vector<node>fu;
	vector<node>qujian;
	vector<node>chaoguo;
	vector<node>out; 
	int first,N,high;
	node temp;
	cin>>first>>N>>high;
	//第一步 完成读入 
	for(int i=0;i<N;i++)
	{
		cin>>temp.add>>temp.data>>temp.next;
		in[temp.add]=temp;   //完成稀疏存储 
	}
	//第二步 完成顺序存储 
	if(first==-1)cout<<"-1"<<endl;
	else
	{
		int nextadd=first;
		while(nextadd!=-1)
		{
			sorted.push_back(in[nextadd]);
			nextadd=in[nextadd].next;
		}
	}
	//第三步 遍历并根据条件压入对应向量
	int size=sorted.size();
	for(int i=0;i<size;i++)
	{
		if(sorted[i].data<0)
		fu.push_back(sorted[i]);
		else if(sorted[i].data<=high)
		qujian.push_back(sorted[i]);
		else if(sorted[i].data>high)
		chaoguo.push_back(sorted[i]);
	} 
	//第四步 三个向量依次放入输出向量
	int size1=fu.size();
	int size2=qujian.size();
	int size3=chaoguo.size();
	for(int j=0;j<size1;j++)
	{
		out.push_back(fu[j]);
	} 
	for(int j=0;j<size2;j++)
	{
		out.push_back(qujian[j]);
	}
	for(int j=0;j<size3;j++)
	{
		out.push_back(chaoguo[j]);
	}
	//第四步 输出 注意地址的转换 分两次输出，最后一次单独输出
	int size4=out.size();
	for(int j=0;j<size4-1;j++)
	{
		out[j].next=out[j+1].add;
		printf("%05d %d %05d\n",out[j].add,out[j].data,out[j].next);
	}
	printf("%05d %d -1",out[size4-1].add,out[size4-1].data); 
	return 0;
}
*/

/*
1076
WIFI密码
枚举  渣渣题 
#include<iostream>
using namespace std;
int main()
{
	int n;
	cin>>n;
	for(int i=0;i<n;i++)
	{
		string s1,s2,s3,s4;
		cin>>s1>>s2>>s3>>s4;
		if(s1=="A-T"||s2=="A-T"||s3=="A-T"||s4=="A-T")
		cout<<"1";
		else if(s1=="B-T"||s2=="B-T"||s3=="B-T"||s4=="B-T")
		cout<<"2";
		else if(s1=="C-T"||s2=="C-T"||s3=="C-T"||s4=="C-T")
		cout<<"3";
		else if(s1=="D-T"||s2=="D-T"||s3=="D-T"||s4=="D-T")
		cout<<"4";
	}
	return 0;
}
*/ 

/*
1077
互评成绩计算 
渣渣题
#include<iostream>
#include<algorithm>
#include<cmath>
using namespace std;
int main()
{
	int zushu;
	int maxscore;
	cin>>zushu>>maxscore;
	int a[zushu];
	
	for(int j=0;j<zushu;j++)
	{
	float sum1=0,sum2=0;
	int k=0,max=0,min=100;
	for(int i=0;i<zushu;i++)
	{
		cin>>a[i];
		if(i==0)
		sum1+=a[i];
		else
		{
			if(a[i]>=0&&a[i]<=maxscore)
			{
				if(a[i]>max)
				max=a[i];
				if(a[i]<min)
				min=a[i];
				sum2+=a[i];
				k++;
			}
		}
	} 
	float u=(sum2-min-max)/(k-2);
	float count;
	count=(sum1+u)/2;
	cout<<round(count)<<endl;
	}
	return 0;
}
*/

/*
1078   有一个运行超时 
字符串压缩与解压  
#include<iostream>
using namespace std;
int main()
{
	char c;
	cin>>c;
	getchar();
	string s;
	getline(cin,s);
	if(c=='D'){
		char d[1001]={0};
		int count=0;
		for(int i=0;i<s.size();)//可能存在两个为数的，调试看有没有三个为数的 
		{
			//前一个为数，后一个为字符，需要展开 
			if((s[i]>='1'&&s[i]<='9')&&(!(s[i+1]>='0'&&s[i+1]<='9')))
			{
				int geshu=s[i]-'0';
				for(int j=0;j<geshu;j++)
				{
					d[count++]=s[i+1];
				} 
				i+=2;
			}
			//前两个为数，后一个为字符，需要展开
			else if((s[i]>='1'&&s[i]<='9')&&(s[i+1]>='0'&&s[i+1]<='9')&&(!(s[i+2]>='1'&&s[i+2]<='9')))
			{
				int geshu=(s[i]-'0')*10+s[i+1]-'0';
				for(int j=0;j<geshu;j++)
				{
					d[count++]=s[i+2];
				} 
				i+=3;
			}
			//字符或空格 原样输出 
			else if((s[i]>='a'&&s[i]<='z')||(s[i]>='A'&&s[i]<='Z')||(s[i]==' ')) {
				d[count++]=s[i];
				i++;
			}
			
		}
		for(int i=0;i<count;i++)cout<<d[i];
	}
	else if(c=='C'){
		int count=1;
		for(int i=0;i<s.size();)
		{
			if(s[i]==s[i+1])
			{
				count++;
				i++;
					
			} 
			else
			{
				if(count==1)
				{
					cout<<s[i];
					i++;
				}
				else 
				{
					cout<<count<<s[i];
					i++;
					count=1;
				}
			}
		} 
	
	}	
	return 0;
} 
//标准答案
#include<iostream>
#include<string>
using namespace std;
int main()
{
    char type;
    scanf("%c",&type);
    string s;
    getchar();
    getline(cin,s);
    if(type=='C'){

    for(int i=0;i<s.size();i++){
            int cnt=0;
       while(s[i]==s[i+1])
        i++,cnt++;
       if(cnt!=0)
            cout<<cnt+1;
        cout<<s[i];
    }
    }else{

    for(int i=0;i<s.size();i++){
             int cnt=0;
    while(s[i]<='9'&&s[i]>='0')
        cnt=cnt*10+s[i++]-'0';
    for(int j=0;j<cnt;j++)
    cout<<s[i];
    if(cnt==0)
        cout<<s[i];
    }
    }
    return 0;
}
*/

/*
1080
MOOC期终成绩

思路：用vector<node>存储初始学生信息和最终输出 用map记录有网上成绩的名字，下标依次加一，
push进向量时用下标操作
#include<iostream>
#include<vector>
#include<map>
#include<algorithm>
using namespace std;
//结构体节点存储学生信息 
struct node
{
	string name;
	int online;
	int mid;
	int final;
	int score;
	
};
//函数实现根据结构体某参数进行排序，与sort一起使用 
bool cmp(node a,node b)
{
	return a.score!=b.score?a.score>b.score : a.name<b.name; 
};
int main()
{
	int m,n,k;
	cin>>m>>n>>k;
	int xiabiao=1;//方便遍历 
	//存储所有信息，存储输出信息
	vector<node>v,out;
	//放入有网上成绩的人的信息
	map<string,int>mp;
	//将网上成绩读入 
	for(int i=0;i<m;i++){
		string s;
		int a;
		cin>>s>>a;
		if(a>=200){
			mp[s]=xiabiao++;
			v.push_back(node{s,a,-1,-1,0});
		}
	} 
	//读入期中成绩 
	for(int j=0;j<n;j++){
		string ss;
		int b;
		cin>>ss>>b;
		if(mp[ss]!=0){
			v[mp[ss]-1].mid=b;
		}
	} 
	//读入期末成绩
	for(int i=0;i<k;i++){
		string sss;
		int c;
		cin>>sss>>c;
		if(mp[sss]!=0){
			v[mp[sss]-1].final=v[mp[sss]-1].score=c;
			//神操作   担心原本的四舍五入被去尾，所以加0.5 
			if(v[mp[sss]-1].mid>v[mp[sss]-1].final){
				v[mp[sss]-1].score=int(v[mp[sss]-1].mid*0.4+v[mp[sss]-1].final*0.6+0.5);
			}
		}
	} 
	//放入out中准备输出
	for(int j=0;j<v.size();j++)
	{
		if(v[j].final>=60) 
		out.push_back(v[j]);
	} 
	sort(out.begin(),out.end(),cmp);
	for(int j=0;j<out.size();j++)
	{
		//是out[j].name.c_str()，不是out[j].name 
		printf("%s %d %d %d %d\n",out[j].name.c_str(),out[j].online,out[j].mid,out[j].final,out[j].score);
	}
	return 0;
}
//标准答案
#include <iostream>
#include <algorithm>
#include <vector>
#include <map>
using namespace std;
struct node {
    string name;
    int gp, gm, gf, g;
};
bool cmp(node a, node b) {
    return a.g != b.g ? a.g > b.g : a.name < b.name;
}
map<string, int> idx;
int main() {
    int p, m, n, score, cnt = 1;
    cin >> p >> m >> n;
    vector<node> v, ans;
    string s;
    for (int i = 0; i < p; i++) {
        cin >> s >> score;
        if (score >= 200) {
            v.push_back(node{s, score, -1, -1, 0});   //push_back后就下标操作了
            idx[s] = cnt++;
        }
    }
    for (int i = 0; i < m; i++) {
        cin >> s >> score;
        if (idx[s] != 0) v[idx[s] - 1].gm = score;
    }
    for (int i = 0; i < n; i++) {
        cin >> s >> score;
        if (idx[s] != 0) {
            int temp = idx[s] - 1;
            v[temp].gf = v[temp].g = score;
            if (v[temp].gm > v[temp].gf) v[temp].g = int(v[temp].gm * 0.4 + v[temp].gf * 0.6 + 0.5);
        }
    }
    for (int i = 0; i < v.size(); i++)
        if (v[i].g >= 60) ans.push_back(v[i]);
    sort(ans.begin(), ans.end(), cmp);
    for (int i = 0; i < ans.size(); i++)
        printf("%s %d %d %d %d\n", ans[i].name.c_str(), ans[i].gp, ans[i].gm, ans[i].gf, ans[i].g);
    return 0;
}
*/

/*
1081
检查密码 判断密码的合理性，不同错误输出对应字符串

思路：置标志位去输出每种结果
 
#include<iostream>
using namespace std;
int main()
{
	int n;
	cin>>n;
	getchar();   //在这里可以理解为读取n值完毕，否则n会为第一个getline被读到 
	for(int i=0;i<n;i++)
	{
		int taiduan=0,buhefazifu=0,zhiyouzimu=0,zhiyoushuzi=0,buhefa=0;
		string s;
		getline(cin,s);  //测试点中含空格，用getline读取 
		if(s.size()<6)
		{
		    taiduan=1;
		}
		else
		{
			int yingwen=0,shuzi=0,xiaoshudian=0;
			for(int j=0;j<s.size();j++)
			{   if(s[j]=='.')xiaoshudian++;    //判断语句中间取等是==  切记！！ 
				else if(s[j]>='0'&&s[j]<='9')shuzi++;
				else if((s[j]>='a'&&s[j]<='z')||(s[j]>='A'&&s[j]<='Z'))yingwen++;	
			}
			if(shuzi+xiaoshudian==s.size())zhiyoushuzi=1;
			else if(yingwen+xiaoshudian==s.size())zhiyouzimu=1;
			else if(shuzi+yingwen+xiaoshudian<s.size())buhefa=1;
		}
		if(taiduan)cout<<"Your password is tai duan le."<<endl;
		else if(zhiyouzimu)cout<<"Your password needs shu zi."<<endl;
		else if(buhefa)cout<<"Your password is tai luan le."<<endl; 
		else if(zhiyoushuzi)cout<<"Your password needs zi mu."<<endl;
		else cout<<"Your password is wan mei."<<endl; 
	} 
	return 0;
} 
*/

/*
1082 
射击比赛  给出id与两点坐标，输出冠军与最差的id，涉及距离判断

思路：用类做，用返回距离函数进行比较再输出 
//输出结果结尾有个“口” 
class Player
{
	char ID[4];
	int X;
	int Y;
public:
	void Init(char *id,int x,int y)
	{
		strcpy(ID,id);
		X=x;
		Y=y;
	}
	int juli()
	{
		return (X*X+Y*Y); 
	}
	void show()
	{
		cout<<ID;
	} 
}; 
int main()
{
	int n;
	cin>>n;
	char id[4];
	int x;
	int y;
	Player player[n];
	for(int i=0;i<n;i++)
	{
		cin>>id>>x>>y;
		player[i].Init(id,x,y);
	}
	for(int i=0;i<n-1;i++)   
	{
		for(int j=i+1;j<n;j++)
		{
			if(player[i].juli()<=player[j].juli())  //距离最大的在前面 
			{
				int temp;
				temp=i;
				i=j;
				j=temp;				
			}
		}
	}
	player[n-1].show();
	cout<<" ";
	player[0].show(); 
	return 0;
}
//借鉴答案 
#include<iostream>
#include<cstring>
using namespace std;
int main()
{
	int n,x,y,max=0,min=10000;
	float juli;
	int id,pmax,pmin;
	cin>>n;
	for(int i=0;i<n;i++)
	{
		cin>>id>>x>>y;
		juli=(x*x+y*y);
		if(juli>max)
		{
			max=juli;
			pmax=id;
		}
		if(juli<min)
		{
			min=juli;
			pmin=id;
		}
	}
	printf("%04d %04d",pmin,pmax);
}
*/

/*
1083
是否存在相等的差

思路：将数读入从一开始的数列，数列值与序号做绝对值函数，对应值数组++ 
绝对值函数 对于整数abs()  对于浮点数 fabs()

#include<iostream>
#include<cmath> 
using namespace std;
//返回绝对值
int ab(int a,int b)
{
	int c;
	c=a-b;
	return abs(c);
} 
int main()
{
	int n;
	cin>>n;
	int a[n+1];
	int b[10000]={0};
	int max=0;
	for(int i=1;i<=n;i++)
	{
		cin>>a[i];
		int temp;
		temp=ab(a[i],i);
		b[temp]++;
		if(temp>max)max=temp;
	} 
	for(int j=max;j>=0;j--)   //取等于 
	{
		if(b[j]>1)
		{
			cout<<j<<" "<<b[j]<<endl;
		}
	}
	return 0;
}
*/

/*
1084
外观数列  对每位数字的描述的迭代
str=str+char;直接在字符串里面加字符，允许 

#include<iostream> 
using namespace std;
int main()
{
	int n;
	string s;
	cin>>s>>n;
	for(int i=1;i<n;i++)
	{
		string str;
		int count=0;
		char c=s[0];
		for(int j=0;j<s.size();j++)
		{
			if(s[j]==c)count++;
			else{
				str+=c;
				str+=count+'0'; //不能写成 str=str+(count+'0'); 
				c=s[j];
				count=1;
			}
		}
		if(count)
		{
			str+=c;
			str+=count+'0';		
		}
		s=str;
	}
	cout<<s<<endl;
	return 0;
}
*/

/*
1085
PAT单位排行 
#include <iostream>
#include <cstdio>
#include <cstring>
#include <algorithm>
#include <map>
#include <algorithm>
using namespace std;
struct pat
{
    char num[7],school[7];
    int af,bf,tf,snum,f;
}s[100001];
int n,c,f;
char num[7],school[7];
map<string,int> q;
bool cmp(pat a,pat b)
{
    if(a.f == b.f)
    {
        if(a.snum == b.snum)
        {
            return strcmp(a.school,b.school) < 0;
        }
        return a.snum < b.snum;
    }
    return a.f > b.f;
}
void low(char *t)
{
    for(int i = 0;t[i];i ++)
    t[i] = tolower(t[i]);
}
int main()
{
    scanf("%d",&n);
    for(int i = 0;i < n;i ++)
    {
        scanf("%s%d%s",num,&f,school);
        low(school);
        if(!q[school])q[school] = ++ c,strcpy(s[c].school,school);
        s[q[school]].snum ++;
        if(num[0] == 'A')s[q[school]].af += f;
        else if(num[0] == 'B')s[q[school]].bf += f;
        else s[q[school]].tf += f;
    }
    for(int i = 1;i <= c;i ++)
    {
        s[i].f = s[i].af + s[i].bf / 1.5 + s[i].tf * 1.5;
    }
    sort(s + 1,s + c + 1,cmp);
    int d = 1;
    printf("%d\n",c);
    for(int i = 1;i <= c;i ++)
    {
        if(s[i].f != s[i - 1].f)d = i;
        printf("%d %s %d %d\n",d,s[i].school,s[i].f,s[i].snum);
    }
}
*/ 

/*
1086
就不告诉你 给定两个数，逆序输出乘积  对于100*100 = 10000 逆序输出的时候只有1 就要找到左起第一个不为零的下标 

#include<iostream>
using namespace std;
int main()
{
	int m,n;
	cin>>m>>n;
	int count;
	count = m*n;
	int a[10] = {0};
	int k = 0;
	 
	while(count>0){
		a[k] = count%10;
		k++;
		count /= 10;
	}
	int t;
	for(int i = 0; i<k; i++){
		if(a[i])
		{
			t = i;
		    break;
		}		
	}
	for(int j = t; j<k; j++){
		cout<<a[j];
	}  	
	return 0;
} 
*/

/*
1087
有多少不同的值 对于一个自然数，范围内的数，用一个取整函数去计算方程式不同值的个数

思路：显然堆最方便，但是我不会用，很尴尬，我可以用vector，因为排序方便 再去遍历计数 

#include<iostream>
#include<vector>
#include<algorithm>
using namespace std;
int main()
{
	vector<int> v;//存放结果
	int N;
	cin>>N;
	for(int i = 1; i<=N; i++){
		int temp,temp1,temp2,temp3;
		temp1 = i/2;
		temp2 = i/3;
		temp3 = i/5;
		temp = temp1+temp2+temp3;
		v.push_back(temp);
	} 
	sort(v.begin(),v.end());
	vector<int> ::iterator it;
	int count = 1;
	for(it = v.begin(); it<v.end()-1; it++){
		if(*it != *(it+1)){
			count++;
		}
	}
	cout<<count;
	return 0;
}
*/

/*
1088
三人行 给定关系，求出三个人的成绩，与自己比较并输出结果 

思路：遍历jia所有的取值，判断最后一个倍数的条件，满足就break  遍历这种方法是最牛皮的 
 
#include<iostream>
#include<cmath>
using namespace std;
const string out1 = "Cong"; 
const string out2 = "Ping";
const string out3 = "Gai";
const string out4 = "No Solution";
void oout(int a, int b)
{
	if(a == b) cout<<out2;
	else if(a > b) cout<<out3;
	else cout<<out1;
}
void ooout(int a, double b)
{
	if(a == b) cout<<out2;
	else if(a > b) cout<<out3;
	else cout<<out1;
}
int main()
{
	int me_score,x,y;
	cin>>me_score>>x>>y;
	int jia,yi;
	double bing;
	for(jia = 99; jia>9; jia--){
		yi = jia/10+jia%10*10;
		bing = double(abs(jia-yi))/x;
		if(bing*y == yi)break;
	}
	if(jia == 9){
		cout<<out4;
	}
	else{
		cout<<jia<<" ";
		oout(me_score,jia);
		cout<<" ";
		oout(me_score,yi);
		cout<<" ";
		ooout(me_score,bing);		
	}
	return 0;
} 
*/

/*
1089
狼人杀-简单版 有两个狼 有两个人说假话 

思路：遍历，冒泡假设狼 再去遍历所有说的话，若两个与事实不符，就输出
      第一个固定数组读入每个人说的话 第二个数组全为零，随机狼对应位置为-1，只是为了比较
 
#include<iostream>
#include<cmath>
using namespace std;
int main()
{
	int n;
	cin>>n;
	int a[n+1];
	for(int i = 1; i<=n; i++){
		cin>>a[i];
	}
	
	for(int i = 1; i<n; i++){
		for(int j = i+1; j<n+1; j++){
			int b[n+1] = {0};
		    int flag = 0;
		    int flag1 = 0;
			b[i] = -1;
			b[j] = -1;
			for(int k = 1; k<n+1; k++){
				if(a[k]<0 && (b[abs(a[k])]==0)){
					flag++;
					if(k == i || k == j) flag1++;
				}
				else if(a[k]>0 && (b[a[k]] == -1)){
					flag++;
					if(k == i || k == j) flag1++;
				}
			}
			if(flag == 2)
			{
				if(flag1 == 1){
					cout<<i<<" "<<j;
				    return 0;
				}
			}
		}
	}
	cout<<"No Solution";
	return 0;
} 
*/

/*
1090
危险品装箱

思路：用vector<set<int>> v用堆去做，添加是insert ，查找是find 

#include<iostream>
#include<vector>
#include<set>
using namespace std; 
bool judge(vector<set<int> > v,int *a,int count)
{
	for(int i = 0; i<count-1; i++){
		for(int j = i+1; j<count; j++){
			if(i != j && v[a[i]].find(a[j])!=v[a[i]].end()){
				return false;
			}
		}
	}
	return true;
}
int main()

{
	vector<set<int> > v(100010);  //一定要初始化！！！！！！！！ 
	int m,n;
	cin>>m>>n;
	for(int i = 0; i<m; i++){
		int a,b;
		cin>>a>>b; 
		v[a].insert(b);
		v[b].insert(a);
	}
	for(int j = 0; j<n; j++){
		int count;
		cin>>count;
		int a[count];
		for(int k = 0; k<count; k++){
			cin>>a[k];
		}
		printf("%s\n",judge(v,a,count)?"Yes":"No");
	}
	return 0;
}
*/
