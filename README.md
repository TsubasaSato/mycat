# �ڎ�
1. �͂��߂�
1.1 �g�p��̒���
2. ���s��
2.1 �𓀂����ۂ̃t�@�C���\��
2.2 mycat.go�̒��g
2.3 �R�}���h�̃R���p�C�����@
2.4�@�R�}���h���s���@
3. �R�}���h���쐬���Ă݂�
3.1 �H�v�����_
3.2 �����ƍH�v�ł����_

# 1. �͂��߂�
Go������ŋߊw�юn�߂��̂ŁAcat�R�}���h���ǂ���Go�ō쐬���Č��悤�Ǝv���B

���̃v���O�����Amycat�͈����ɗ^����ꂽ�e�L�X�g�t�@�C�����̃p�X���擾���A
���̃t�@�C�����̕��͂���s���W���o�͂ɏo�͂���R�}���h�ł���B
�I�v�V����-n���w�肷�邱�Ƃōs���ɒʂ��ԍ���t�^���邱�Ƃ��\�ł���B

## 1.1 �g�p��̒���
���s�t�@�C�����Ƀe�L�X�g�t�@�C�������݂��Ă���O��Ńv���O�������쐬���Ă��邽�߁A
mycat.exe�����݂���f�B���N�g�����̃e�L�X�g�t�@�C���������Ή����Ă��Ȃ��B

# 2. ���s��

## 2.1 �𓀂����ۂ̃t�@�C���\��
mycat���N���b�N�����(GitHub�ɔ�т܂�)
mycat/
 ��mycat.go
 ��test_1.txt
 ��test_2.txt
 ��README.txt

## 2.2 mycat.go�̒��g
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
	var n =flag.Bool("n",false,"�ʂ��ԍ���t�^����")
	flag.Parse()
	var(
		files = flag.Args()
		path,err = os.Executable()
	)
	if err!=nil{
		fmt.Fprintln(os.Stderr,"�ǂݍ��݂Ɏ��s���܂���",err)
	}

	//���s�t�@�C���̃f�B���N�g�������擾
	path=filepath.Dir(path)
	//�ʂ��ԍ��̗p��
	i:=1

	for x:=0;x<len(files);x++{
		sf,err:=os.Open(filepath.Join(path,files[x]))
		if err!=nil{
			fmt.Fprintln(os.Stderr,"�ǂݍ��݂Ɏ��s���܂���",err)
		}else{
			scanner :=bufio.NewScanner(sf)
			for ;scanner.Scan();i++{
				if *n{
					//�I�v�V����������ꍇ
					fmt.Printf("%v: ",i)
				}
				fmt.Println(scanner.Text())
			}
		}
	}

}
"""

## 2.3 �R�}���h�̃R���p�C�����@
"""cmd
$go build mycat.go
"""
## 2.4 �R�}���h���s���@
"""cmd
$mycat [�I�v�V����] [�t�@�C����]...
"""

�g�p�ł���I�v�V����
-n : �s���ɒʂ��ԍ���t�^���邱�Ƃ��ł���

# 3. �R�}���h���쐬���Ă݂�
## 3.1 �H�v�����_
- �G���[���������������_�ł���B���s�t�@�C���̃p�X�A�e�L�X�g�t�@�C���̃p�X���擾�ł��Ȃ������ۂ̓G���[�o�͂��o���悤�ɂ����B
���݂��Ȃ��t�@�C�������w�肳�ꂽ�ꍇ���������G���[���������B
- �����Ɏw�肳���e�L�X�g�t�@�C���̐��͂����ł��w��ł���B

## 3.2 �����ƍH�v�ł����_
�����Bool�^�̃I�v�V���������������Ȃ��������A
- flag.String()
- flag.Int()
��p���邱�ƂŁA���╶������w�肷��I�v�V�������쐬�ł����B
