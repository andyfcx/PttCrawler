# PttCrawler

## 相依性

* Python 3.x
* libssl-dev

## 安裝

1. git clone

```bash
git clone https://github.com/GundamBox/PttCrawler.git
```

![img1](img/1.PNG)

2. change directory

```bash
cd PttCrawler
```

![img1](img/2.PNG)

3. Check Python and Pip Version

必須是 Python3

```bash
sudo apt-get install python3 python3-pip
```

![img1](img/3.PNG)

4. Install Package

```bash
sudo apt-get install libssl-dev
sudo pip3 install -r requirements.txt 
```

![img1](img/4.PNG)
![img1](img/5.PNG)

5. 複製`config_example.ini`為`config.ini`

```bash
cp config_example.ini config.ini
```

![img1](img/6.PNG)

## 設定

```ini
[Database]
# Database Url: [Type]://[Name]
# 目前只支援SQLite
Type = sqlite
Name = ptt.db

[PttUser]
# term.ptt.cc 每個動作的間隔
Delaytime = 2
# selenium 需要用到的webdriver的資料夾
WebdriverFolder = webdriver
# term.ptt.cc 的 登入帳號密碼
UserId = guest
UserPwd = guest
# Choices = {database, json, both}
Output = both

[PttArticle]
# Delaytime 是每篇文章之間的Delaytime
# NextPageDelaytime 是WebPtt索引頁之間的Delaytime
Delaytime = 2.0
NextPageDelaytime = 10.0
# request timeout
Timeout = 10
# Choices = {database, json, both}
Output = both
```

## 使用

### Crawler

1. PTT 文章爬蟲

    從WebPtt爬取文章

    ```bash
    python -m crawler article (--start-date | --index START_INDEX END_INDEX) [--config-path CONFIG_PATH]
    ```

2. PTT 鄉民上站紀錄爬蟲

    利用term.ptt.cc爬取上站紀錄、登入次數、有效文章

    ```bash
    python -m crawler user (--database | --ip IP) [--config-path CONFIG_PATH]
    ```

3. PTT 查Ip Autonomous System Number

    查Ip的ASN(主要查Country code)

    ```bash
    python -m crawler asn (--database | --id ID) [--config-path CONFIG_PATH]
    ```

### Export

匯出成ods或csv

```bash
python export.py --format {ods, csv} --output-folder OUTPUT_FOLDER [--output-prefix OUTPUT_PREFIX]
```

### Schedule

1. Update

```bash
python schedule.py update {article, asn, user} -c CYCLE_TIME [-s START_DATETIME] [--virtualenv VIRTUALENV_PATH]
```

2. Remove

```bash
python schedule.py remove {article, asn, user}
```

## Todo

- [ ] PttArticleCrawler改用`Scrapy`框架替代
- [ ] PttArticleCrawler的`crawling`方法要分為兩個，一個負責抓取`index`跟`web_id`，一個負責抓取與`web_id`對應的文章

## 檔案結構

```
PttCrawler/
|- utils.py
|- export.py
|- query.py
|- schedule.py
|- config_example.ini
|- models/
|   |- __init__.py
|   |- article.py
|   |- asn.py
|   |- base.py
|   `- user.py
|- crawler/
|   |- __init__.py
|   |- __main__.py
|   |- article.py
|   |- asn.py
|   |- crawler_arg.py
|   |- user.py
|- webdriver/
|   |- windows/
|   |   `- chromedriver.exe
|   |- linux/
|   |   `- chromedriver
|   `- mac/
|       `- chromedriver
|- requirements.txt
|- env_wrapper.sh
|- CHANGELOG.md
|- README.md
`- README_ZH.md
```