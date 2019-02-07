# 目次
1. はじめに
1.1 使用上の注意
2. 実行環境
2.1 解凍した際のファイル構造
2.2 mycat.goの中身
2.3 コマンドのコンパイル方法
2.4　コマンド実行方法
3. コマンドを作成してみて
3.1 工夫した点
3.2 もっと工夫できた点

# 1. はじめに
Go言語を最近学び始めたので、catコマンドもどきをGoで作成して見ようと思う。

このプログラム、mycatは引数に与えられたテキストファイル名のパスを取得し、
そのファイル内の文章を一行ずつ標準出力に出力するコマンドである。
オプション-nを指定することで行頭に通し番号を付与することも可能である。

## 1.1 使用上の注意
実行ファイル内にテキストファイルが存在している前提でプログラムを作成しているため、
mycat.exeが存在するディレクトリ内のテキストファイル名しか対応していない。

# 2. 実行環境

## 2.1 解凍した際のファイル構造
mycat/
 ├mycat.go
 ├test_1.txt
 ├test_2.txt
 └README.txt

## 2.2 mycat.goの中身
"""go

package main

import (
	"bufio"
	"flag"
	"fmt"
	"os"
	"path/filepath"
)

func main() {
	var n =flag.Bool("n",false,"通し番号を付与する")
	flag.Parse()
	var(
		files = flag.Args()
		path,err = os.Executable()
	)
	if err!=nil{
		fmt.Fprintln(os.Stderr,"読み込みに失敗しました",err)
	}
	//実行ファイルのディレクトリ名を取得
	path=filepath.Dir(path)
	//通し番号の用意
	i:=1
	for x:=0;x<len(files);x++{
		sf,err:=os.Open(filepath.Join(path,files[x]))
		if err!=nil{
			fmt.Fprintln(os.Stderr,"読み込みに失敗しました",err)
		}else{
			scanner :=bufio.NewScanner(sf)
			for ;scanner.Scan();i++{
				if *n{
					//オプションがある場合
					fmt.Printf("%v: ",i)
				}
				fmt.Println(scanner.Text())
			}
		}
	}

}
"""

## 2.3 コマンドのコンパイル方法
"""cmd
$go build mycat.go
"""
## 2.4 コマンド実行方法
"""cmd
$mycat [オプション] [ファイル名]...
"""

使用できるオプション
-n : 行頭に通し番号を付与することができる

# 3. コマンドを作成してみて
## 3.1 工夫した点
- エラー処理を実装した点である。実行ファイルのパス、テキストファイルのパスを取得できなかった際はエラー出力を出すようにした。
実在しないファイル名を指定された場合も正しくエラー処理される。
- 引数に指定されるテキストファイルの数はいくつでも指定できる。

## 3.2 もっと工夫できた点
今回はBool型のオプションしか実装しなかったが、
- flag.String()
- flag.Int()
を用いることで、数や文字列を指定するオプションも作成できた。
