# 前言

selenium在windows机器上运行，每次会启动界面，运行很不稳定。于是想到用chrome来了的headless无界面模式，确实方便了不少。
为了提高自动化运行的效率和稳定性，于是把selenium自动化环境部署到linux服务器上，这样更方便。
环境:
centos 7.6
python 3.6
chrome 77.0.3865.90
chromedriver 77.0.3865.40
selenium 3.14

# 安装最新版chrome

## 方法一：yum在线安装

> yum install google-chrome-stable_current_x86_64.rpm

或者指定地址

> yum install https://dl.google.com/linux/direct/google-chrome-stable_current_x86_64.rpm

安装完成之后，检查下版本号

```
> google-chrome -version

Google Chrome 77.0.3865.90 
```

## 方法二：下载到本地后安装

先下载google-chrome最新版77.0.3865.90

> wget https://dl.google.com/linux/direct/google-chrome-stable_current_x86_64.rpm

下载完成后，使用yum安装本地包

> yum localinstall google-chrome-stable_current_x86_64.rpm

# chromedriver驱动

下载chromedriver驱动，历史版本https://sites.google.com/a/chromium.org/chromedriver/downloads找到对应的驱动版本

可以使用wget下载zip包

> wget https://chromedriver.storage.googleapis.com/91.0.4472.19/chromedriver_linux64.zip

解压zip包,如果提示没有zip，那就`yum -y install zip`先安装下

> unzip chromedriver_linux64.zip # 解压zip

解压后把chromedriver移动到/usr/bin/目录下

> mv chromedriver /usr/bin/

查看chromedriver版本号

```
> chromedriver --version

ChromeDriver 77.0.3865.40 (f484704e052e0b556f8030b65b953dce96503217-refs/branch-heads/3865@{#442})
```

# 安装selenium

安装最新版selenium 3.141.0

> pip3 install selenium

# 运行selenium代码

新建一个test_demo.py文件，运行测试代码

```python
from selenium import webdriver
from selenium.webdriver.chrome.options import Options
chrome_options = Options()
chrome_options.add_argument('--headless')  # 无界面
chrome_options.add_argument('--no-sandbox')  # 解决DevToolsActivePort文件不存在报错问题
chrome_options.add_argument('--disable-gpu')   # 禁用GPU硬件加速。如果软件渲染器没有就位，则GPU进程将不会启动。
chrome_options.add_argument('--disable-dev-shm-usage')
chrome_options.add_argument('--window-size=1920,1080')  # 设置当前窗口的宽度和高度
driver = webdriver.Chrome('chromedriver',chrome_options=chrome_options)
#driver = webdriver.Chrome()
driver.get("https://www.cnblogs.com/yoyoketang/")
 
print(driver.page_source)
driver.quit()
```

运行代码

> python3 test_demo.py