## iPython notbook server
先查詢 jupyter 設定檔的位置

`jupyter --path`

會顯示如下

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

config 顯示有四個路徑可以做設定，以下作法都是在個人目錄下做設定，

可以另外將以下產生的設定擺置其他地方 (ex: `/usr/local/etc/jupyter`)


`jupyter notebook --generate-config`

產生 jupyter https 設定檔，個人目錄下會出現 `.jupyter` 資料夾

接著進入 ipython 環境內產生密碼, 複製產生出的密碼

```
In [1]: from notebook.auth import passwd
In [2]: passwd()
Enter password:
Verify password:
Out[2]: 'sha1:67c9....'
```

產生 `.pem`  和 hash `.key` 的文件
```
openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mykey.key -out mycert.pem
```
產生出來會有`mykey.key`跟`mycert.pem` 這兩個檔案在目錄下

設定 jupyter notebook config 位於：`~/.jupyter/jupyter_notebook_config.py`，將下面東西貼入設定檔內
```
# Set options for certfile, ip, password, and toggle off browser auto-opening
c.NotebookApp.certfile = '/absolute/path/to/your/certificate/mycert.pem'
c.NotebookApp.keyfile = '/absolute/path/to/your/certificate/mykey.key'
# Set ip to '*' to bind on all interfaces (ips) for the public server
c.NotebookApp.ip = '0.0.0.0'
c.NotebookApp.password = 'sha1:bcd259ccf...<your hashed password here>'
c.NotebookApp.open_browser = False

# It is a good idea to set a known, fixed port for server access
c.NotebookApp.port = 9999
```

可以在 jupyter notebook 測試
```
%matplotlib inline 
import matplotlib
import numpy as np
import matplotlib.pyplot as plt


x1 = np.linspace(0.0, 5.0)
x2 = np.linspace(0.0, 2.0)

y1 = np.cos(2 * np.pi * x1) * np.exp(-x1)
y2 = np.cos(2 * np.pi * x2)

plt.subplot(2, 1, 1)
plt.plot(x1, y1, 'ko-')
plt.title('A tale of 2 subplots')
plt.ylabel('Damped oscillation')

plt.subplot(2, 1, 2)
plt.plot(x2, y2, 'r.-')
plt.xlabel('time (s)')
plt.ylabel('Undamped')
```
