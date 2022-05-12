## 1、char *转换string

```c++
char * chatr;
char arstr[];
string str1=chatr;
string str2=arstr;
string s(chatr);
```

## 2、string转换char*

```c++
string str="HI CPP";
const char * mystr=str.c_str();

//////////////////////////////////////////////////////////////////


char *ret=new char[s.length()+1];
strcpy(ret,s.c_str());
```

