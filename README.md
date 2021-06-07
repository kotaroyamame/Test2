# 問題

回答として自然数aが、N個与えられます。

回答にはそれぞれ、タグt(自然数)がM個付与されます。

タグtは、必ず一つのみのタググループtg(自然数)に属しています。

ある回答a<sub>i</sub>に付与されているtは、t<sub>ai</sub>とします。

タググループtgは、優先順位l(自然数)を持ちます。
タググループの優先順位順にタグを選択して回答を絞り込むツリー構造のJSON文字列Tを生成してください。

JSONの構造は以下のとおりです。
```
interface T{
  //タググループID
  tg?: number,
  //タグIDの配列
  t?: Array<number>,
  // 現時点での回答候補
  a: Array<number>,
  //選択されたタグIDに対しての次のT
  next:{
    [key:ti]:T
  }
}
```
## 制約


1≤I≤P≤N≤100   
1≤M<sub>i</sub>≤1000  
![texclip20210607195510](https://user-images.githubusercontent.com/13118113/121005013-4e8b3800-c7ca-11eb-9aca-072b18691a82.png)    
1≤T<sub>i</sub>≤500  
1≤t<sub>i,j</sub>,g<sub>k,l</sub><1000  

## フォーマット


N  
a<sub>1</sub> a<sub>2</sub> ... a<sub>N</sub>   
M<sub>0</sub>  
t<sub>0,0</sub> ...  t<sub>0,M<sub>0</sub>-1 </sub>  
.  
.  
.  
M<sub>N-1</sub>   
t<sub>N-1,0</sub> ... t<sub>N-1,M<sub>M-1</sub>-1</sub>   
P  
Q<sub>0</sub>  
T<sub>0</sub> I<sub>0</sub> g<sub>0,0</sub> ... g<sub>0,Q<sub>0</sub>-1</sub>   
.  
.  
.  
Q<sub>P-1</sub>   
T<sub>P-1</sub> I<sub>P-1</sub> g<sub>P-1,0</sub> ... g<sub>P-1,Q<sub>P-1</sub>-1</sub>   

##　入力例1
```
4    
0 1 2 3
4
1 2 3 8
5
1 5 0 3 9
5
6 4 7 2 10
3
0 1 10
4
0 1 0 1 2 3
3
1 2 4 5 6
4
2 3 7 8 9 10
```

##　入力例1に対する回答
```
{
  "tg": 0,
  "t": [0,1,2,3],
  "a": [0,1,2,3],
  "next":{
    0:{
     "tg":2,
     "t":[9,10],
     "a":[1,3],
     "next":{
       9:{
        "a":[1]
       },
       10:{
        "a":[3]
       }
     }
    },
    1:{
     "tg":2,
     "t":[8,9,10],
     "a":[0,1,3],
     "next":{
       8:{
        "a":[0]
       },
       9:{
        "a":[1]
       },
       10:{
        "a":[3]
       }
     }
    },
    2:{
     "tg":2,
     "t":[8,10],
     "a":[0,2],
     "next":{
       8:{
        "a":[0]
       },
       10:{
        "a":[2]
       }
     }
    },
    3:{
     "a":[3]
    }
  }
}
```

##　入力例2
```
3 
0 1 2 
4  
0 1 2 3
4  
0 1 2 3
4 
0 1 2 3
2  
0 1 0 1
2  
1 2 2 3
```

##　入力例2に対する回答
```
{
  "a": [0,1,2]
}

```

入力例2の場合、回答0,1,2ともにtg1に属するtを0,1。tg２に属するtを2,3。が紐付いているため、tによって回答を絞り込むことができません。  
よって提示するtgとtが無いため、`{"a":[0,1,2]}`を出力します


## 解説

1, カテゴリグループの順にタグの選択肢を提示します。   
2, タグが選択されたら、次のグループのタグを提示します。   

この繰り返しで回答を絞っていき、回答が一つに絞られた時点で終了です。

- 回答候補がどうしても一つに絞れないときは、回答候補が複数あっても終了です。
- 回答候補の中で、タグを選んでも回答候補を全く絞り込めないときは、そのタググループをスキップします。
- 回答候補の中で、共通のタググループに属するタグが一つもない回答がある場合もそのタググループをスキップします。
- 回答候補の中で、存在しないタグは候補から外します。

