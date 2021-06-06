# 問題

回答として自然数aが、N個与えられます。

回答にはそれぞれ、タグt(自然数)が付与されます。

タグtは、タググループtg(自然数)に属しています。

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
## 成約

```
N<=100
```

## フォーマット


N  
a<sub>1</sub> a<sub>2</sub> a<sub>3</sub> a<sub>4</sub>  
ta1 ta1 ta1  
ta2 ta2 ta2 ta2 ta2 ta2  
ta3 ta3 ta3 ta3 ta3  
ta4 ta4 ta4 ta4  
tg l t t t t  
tg l t t t t  
tg l t t t t  

##　入力例1
```
4
0 1 2 3//
1 2 3 8 //0
1 5 0 3 9//1
6 4 7 2 10//2
0 1 10//3
0 1 0 1 2 3
1 2 4 5 6
2 3 7 8 9 10
```

##　入力例1に対する回答
```
{
  tg: 0,
  t: [0,1,2,3],
  a: [0,1,2,3],
  next:{
    0:{
     tg:1,
     t:[4,5,6],
     a:[1,3],
     next:{
       4:{
        a:[3],
       },
       5:{
        a:[1],
       },
       6:{
        a:[3],
       }
     }
    },
    1:{
     tg:1,
     t:[4,5,6],
     a:[0,1,3],
     next:{
       4:{
        tg:2,
        t:[8,10],
        a:[0,3]
        next:{
         8:{
          a:[0]
         },
         10:{
          a:[10]
         }
        }
       },
       5:{
        a:[1]
       },
       6:{
        a:[0,3]
       }
     }
    },
    2:{
     tg:1,
     t:[4,5,6],
     a:[0,2],
     next:{
      tg:2,
      t:[7,8,9,10],
      a:[0,2],
      next:{
        8:{
          a:[0],
        },
        10:{
          a:[0],
        },
      }
     }
    },
    3:{
     a:[3]
    }
  }
}
```

## 解説
