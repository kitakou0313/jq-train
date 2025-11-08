# jqコマンドのメモ

## フィルタ
```
// 特定のキー取得
cat obj.json 
| jq '.count'
3

// 配列要素を個々に出力
cat array.json
 | jq '.[]'
{
  "status": "OK",
  "count": 3,
  "users": [
    {
      "name": "Yamada",
      "age": 26
    },
    {
      "name": "Tanaka",
      "age": 32
    },
    {
      "name": "Suzuki",
      "age": 45
    }
  ]
}
{
  "status": "NG",
  "count": 1,
  "users": [
    {
      "name": "Take",
      "age": 40
    },
    {
      "name": "Yoshi",
      "age": 21
    }
  ]
}

// 要素指定
cat array.json
 | jq '.[1]'
{
  "status": "NG",
  "count": 1,
  "users": [
    {
      "name": "Take",
      "age": 40
    },
    {
      "name": "Yoshi",
      "age": 21
    }
  ]
}

// 再抽出
// pipeで繋げて配列の各要素ごとに抽出
cat array.json
 | jq '.[] | .status' 
"OK"
"NG"

// 配列で囲うことでさらに配列に
cat array.json
 | jq '[.[] | .status]'
[
  "OK",
  "NG"
]
// objectにすることも可能
cat array.json
 | jq '{ name: .[] | .count}'
{
  "name": 3
}
{
  "name": 1
}

// 値をキーにすることも可能 ()でくくる必要がある点に注意
cat obj.json | jq '.users[] | {(.name):.age}'
{
  "Yamada": 26
}
{
  "Tanaka": 32
}
{
  "Suzuki": 45
}

// select 演算子を渡して抽出
echo '[1,2,3,4,5]' | jq '.[] | select(. > 3)'
4
5
// valueはstrの場合""で囲う 定義の時と同じ
cat array.json | jq '.[] | select(.status == "NG")'
{
  "status": "NG",
  "count": 1,
  "users": [
    {
      "name": "Take",
      "age": 40
    },
    {
      "name": "Yoshi",
      "age": 21
    }
  ]
}
```

## メモ
- .スタートはキーを、始まらないのは文字列を表していそう
- |でJSONが渡されたら、そのobj | arrayひとつに対して後続のフィルターが実行されるイメージ
    - []は分解して個々のobjにするイメージ