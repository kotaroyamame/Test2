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
  tg: number,
  //タグIDの配列
  t: Array<number>,
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
tg t t t t l  
tg t t t t l  
tg t t t t l  

##　入力例
```
4
0 1 2 3
1 2 3
1 5 0 3
6 4 7 2
0 1
0 0 1 2 3
1 4 5 6
2 7 8 9 10
```

