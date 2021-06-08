# 問題

回答として連番の自然数aが、N個与えられます。

回答にはそれぞれ、タグt<sub>i</sub>(自然数)がM<sub>i</sub>個付与されます。

タグtは、必ず一つのみのタググループTi(自然数)に属しています。

ある回答a<sub>i</sub>に付与されているtは、t<sub>a,i</sub>とします。

タググループTiは、優先順位l(自然数)を持ちます。
タググループの優先順位順にタグを選択して回答を絞り込むツリー構造のJSON文字列Treeを生成してください。

JSONの元になるオブジェクトの構造は以下のとおりです。
```
interface Tree{
  //タググループID
  tg?: number,
  //タグIDの配列
  t?: Array<number>,
  // 現時点での回答候補
  a: Array<number>,
  //選択されたタグIDに対しての次のT
  next:{
    [t:number]:Tree
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


N<br>
a<sub>1</sub> a<sub>2</sub> ... a<sub>N</sub><br>
M<sub>0</sub><br>
t<sub>0,0</sub> ...  t<sub>0,M<sub>0</sub>-1 </sub><br>
︙<br>
M<sub>N-1</sub><br>
t<sub>N-1,0</sub> ... t<sub>N-1,M<sub>N-1</sub>-1</sub><br> 
P<br>
Q<sub>0</sub><br>
T<sub>0</sub> I<sub>0</sub> g<sub>0,0</sub> ... g<sub>0,Q<sub>0</sub>-1</sub><br>
︙<br>
Q<sub>P-1</sub><br>
T<sub>P-1</sub> I<sub>P-1</sub> g<sub>P-1,0</sub> ... g<sub>P-1,Q<sub>P-1</sub>-1</sub><br>

##　入力例1
```
4    
0 1 2 3
4
1 2 3 8
5
1 0 3 9
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
  "t": [
    0,
    1,
    2,
    3
  ],
  "a": [
    0,
    1,
    2,
    3
  ],
  "next": {
    "0": {
      "tg": 2,
      "t": [
        9,
        10
      ],
      "a": [
        1,
        3
      ],
      "next": {
        "9": {
          "a": [
            1
          ]
        },
        "10": {
          "a": [
            3
          ]
        }
      }
    },
    "1": {
      "tg": 1,
      "t": [
        4,
        5,
        6
      ],
      "a": [
        0,
        1,
        3
      ],
      "next": {
        "4": {
          "tg": 2,
          "t": [
            8,
            9,
            10
          ],
          "a": [
            0,
            3
          ],
          "next": {
            "8": {
              "a": [
                0
              ]
            },
            "10": {
              "a": [
                3
              ]
            }
          }
        },
        "5": {
          "tg": 2,
          "t": [
            8,
            10
          ],
          "a": [
            0,
            3
          ],
          "next": {
            "8": {
              "a": [
                0
              ]
            },
            "10": {
              "a": [
                3
              ]
            }
          }
        },
        "6": {
          "tg": 2,
          "t": [
            8,
            9,
            10
          ],
          "a": [
            0,
            3
          ],
          "next": {
            "8": {
              "a": [
                0
              ]
            },
            "10": {
              "a": [
                3
              ]
            }
          }
        }
      }
    },
    "2": {
      "tg": 2,
      "t": [
        8,
        10
      ],
      "a": [
        0,
        2
      ],
      "next": {
        "8": {
          "a": [
            0
          ]
        },
        "10": {
          "a": [
            2
          ]
        }
      }
    },
    "3": {
      "a": [
        3
      ]
    }
  }
}
```

例えば最初にt<sub>0</sub>を選択した場合、回答が1,3に絞られます。次の優先順位のT<sub>1</sub>に該当するtを回答1,3より探しますが、回答1,3にはT<sub>1</sub>に属するtがありませんのでT<sub>1</sub>では絞り込みができません。  <br>
ということは、T<sub>1</sub>の項目はどの項目を選んでも回答の選択肢が全く減らないということになります。この場合、意味のないT<sub>i</sub>と判断し、T<sub>1</sub>をスキップします。次にT<sub>2</sub>によって絞り込むことを試みます。回答１，３にはT<sub>2</sub>に属するtがそれぞれ紐付けられていますので、t<sub>9</sub>とt<sub>10</sub>を候補として提示します。　　<br>
その結果、回答が無事に一つに絞り込むことができました。<br>

それでは最初にt<sub>1</sub>を選択した場合を考えてみましょう。その場合、回答が0,1,3に絞られます。回答0,1,3の中で、T<sub>1</sub>に属するtを持っているのは回答1のみです。回答1がt<sub>5</sub>を所持しています。<br>
従って、T<sub>1</sub>の要素の中で、t<sub>5</sub>以外を選択したときにt<sub>5</sub>を所持する回答1を除外することができます。（t<sub>5</sub>を選択したときはtg１に属するtを持っていない回答0,3を除外できません。）<br>
この場合、T<sub>1</sub>は意味のある情報ですので除外、及びスキップしません。

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
2, タグが選択されたら、次の優先順位のグループのタグを提示します。   

この繰り返しで回答を絞っていき、回答が一つに絞られる。またはタググループがなくなった時点で終了です。

- 回答候補がどうしても一つに絞れないときは、回答候補が複数あっても終了です。
- 回答候補の中で、タグを選んでも回答候補を全く絞り込めないときは、そのタググループをスキップします。
- 回答候補の中で、該当のタググループに属するタグを一つも無い回答がある場合、その回答は絞り込むことができません。その場合、該当のタググループに属するタグをすべて出力します。

## 余裕のある方向けの問題

タググループTiは優先順位l(自然数)を持ちますが、タググループTiを効率よく回答を絞込める順に最適化してください。

[入力例1に対する回答]では、T<sub>1</sub>をスキップしても回答を一つに絞り込むことができます。

 回答を絞り込むのに要する木構造の深さと候補のtを最小にするアルゴリズムを考えてください。
