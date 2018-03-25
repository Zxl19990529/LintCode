```C++
private：
int *a, l;        //定义私有成员变量，指针a，和长度l；
public:
    CHugeInt(){    //构造函数（缺省函数）
        a = new int[MAX];
        l = 1;
        memset(a, 0, sizeof(int) * MAX);//利用memset函数对数组初始化；
    }
    CHugeInt(const CHugeInt& x){  //复制构造函数
        a = new int[MAX];
        memset(a, 0, sizeof(int) * MAX);
        l = x.l;
        for(int i = 1; i <= l; i++)
            a[i] = x.a[i];
    }
    ~CHugeInt(){    //析构函数，释放直至指向的空间；
        if(a)delete [] a;
    }
    CHugeInt(int n){ //构造函数，参数为整形；
        a = new int[MAX];
        memset(a, 0, sizeof(int) * MAX);
        if(!n)l = 1;  // 若数字为0，则长度为1，只有个位；
        else l = 0;
        while(n){  //将数字n的各个位数分解储存在字符数组中；
            a[++l] = n % 10;
            n /= 10;
        }
    }
    CHugeInt(char *c){    //构造函数，参数为字符串。
        a = new int[MAX];
        memset(a, 0, sizeof(int) * MAX);
        l = strlen(c);    //统计字符串的长度；
        for(int i = l - 1; i >= 0; i--)
            a[l - i] = c[i] - '0';  // 字符转换为整形数字，储存在数组中；      
    }
    int& operator[](int x){   //对[]重载，可以用对象名+[]直接访问里面的数组，简化写法；
        return a[x];
    }
    CHugeInt& operator =(const CHugeInt& x){//对‘=’重载；
        a = new int[MAX];
        memset(a,0,sizeof(*a));
        l = x.l;
        for(int i = 1; i <= l; i++)
            a[i] = x.a[i];
        return *this;
    }
    friend CHugeInt operator +(CHugeInt a, CHugeInt b){//对两个对象之间的‘+’进行重载；
        CHugeInt c; //生成对象c，调用缺省参数的构造函数；
        if(a.l>=b.l)
        {
        	c.l=a.l;
		}
		else c.l=b.l;  //选出两个数组中最长的长度；
        for(int i = 1; i <= c.l; i++)// 对c中的数组进行赋值；
            c[i] = a[i] + b[i];
        for(int i = 1; i <= c.l; i++)
            if(c[i] >= 10)     //进位运算，满十进一；
            {
                if(i == c.l)c.l++;
                c[i + 1] += c[i] / 10;
                c[i] %= 10;
            }
        return c;
    }
    friend ostream& operator << (ostream& os, CHugeInt x){//流输出运算符重载；
        for(int i = x.l; i >= 1; i--)
            os << x.a[i];
        return os;
    }
    friend CHugeInt operator +(CHugeInt a, int b){    
        return a + CHugeInt(b);
    }
    friend CHugeInt operator +(int a, CHugeInt b){
        return CHugeInt(a) + b;
    }
    CHugeInt operator +=(int x){//依次对+=，++，重载；
        *this = *this + CHugeInt(x);
        return *this;
    }
    CHugeInt operator ++(){
        *this = *this + CHugeInt(1);
        return *this;
    }
    CHugeInt operator ++(int){
        CHugeInt tmp = *this;
        *this = CHugeInt(1) + *this;
        return tmp;
    }
```
