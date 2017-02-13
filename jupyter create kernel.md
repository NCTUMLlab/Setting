####Virtualenv python path 

首先先進入 virtualenv，進入名叫 tensorflow 的虛擬環境

`
source active tensorflow
`

進入 virtualenv 後, 查看 virtualenv 下的 python bin 檔執行位置

`which python`

顯示出執行位置, 以我為範例為:

`/home/user/tensorflow/bin/python`

###jupyter create new kernel 

先查詢 jupyter config 位置

`jupyter --path`

會顯示出 config 設定情形與位置, 以我為範例：
```
config:
    /home/user/.jupyter
    /usr/etc/jupyter
    /usr/local/etc/jupyter
    /etc/jupyter
data:
    /home/user/.local/share/jupyter
    /usr/local/share/jupyter
    /usr/share/jupyter
runtime:
    /run/user/1000/jupyter

```

**data** 顯示的路徑第一個為設定在個人目錄下 (需要用 root 方式進入)， 
找到有 **kerenls** 資料夾， 也可以設定在 `/usr/local/share/jupyter` 下，
若沒有 **kernels**，就自行建立一個

新增一個資料夾， 資料夾名稱同 virtualenv 方便管理，


`mkdir tensorflow`

在 新創的資料夾下, 新增一個 `kernel.json`, 內容設定

```
{
 "argv": [ "/Yours/virtualenv/bin/python", "-m", "ipykernel",

 "-f", "{connection_file}"],

 "display_name": "tensorflow",

 "language": "python"
}

```
* `argv` 內的第一個參數為 virtualenv 的 python bin 檔執行位置

* `displayname` 為 jupyter 內顯示 kernel 名稱

### 快速方法

一樣先進入 virtualenv

`source activate myenv`

接著輸入

`python -m ipykernel install --user --name myenv --display-name "Python (myenv)"`



