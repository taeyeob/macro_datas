## 목차
1. 매크로데이터 크롤링을 위한 module 다운로드하기
2. Selenium 실행을 위한 Chrome driver 설치하기
2. OECD 데이터 크롤링하기
3. IMF 데이터 크롤링하기
4. FxStreet, Census, 한국무역협회에서 데이터 크롤링하기
5. Investing.com 에서 crude oil, copper 일일수익률 데이터 크롤링하기

## 매크로데이터 크롤링을 위한 module 다운로드하기
1. 명령 프롬프트를 실행합니다.
2. 아래의 순서대로 프롬프트에 입력하여 module 설치합니다.
- **pip install selenium**
- **pip install bs4**
- **pip install pandasdmx**

## Selenium 실행을 위한 Chrome driver 설치하기
1. 각자 Chrome 브라우저의 버전을 확인합니다(설정에 들어가면 확인해볼 수 있습니다).
2. **https://sites.google.com/a/chromium.org/chromedriver/downloads** 사이트에 접속하여 각자 버전에 맞는 드라이버를 설치하고, 바탕화면에 **MacroDatas** 폴더를 생성하여 이 곳에 저장합니다(이후 모든 Python 파일들 역시 이 폴더에 저장합니다).

## OECD 데이터 크롤링하기
1. 위의 폴더에 **oecd.py** 라는 파일을 VSCode를 통해 생성합니다.
2. 다음 코드를 복사, 붙여넣기 한다.
```Python
import os
import datetime
import pandasdmx as pdsx
import csv

now = datetime.datetime.now()

# M1 크롤링
oecd = pdsx.Request('OECD', proxies = {'timeout':'5000'})
data_response1 = oecd.data(resource_id='MEI_FIN', key='MANM.AUS+CAN+CHL+CZE+DNK+HUN+ISL+ISR+JPN+KOR+LVA+MEX+NZL+NOR+POL+SWE+CHE+TUR+GBR+USA+EA19+OECD+NMEC+BRA+CHN+COL+IND+IDN+RUS+ZAF.M/all', params={'startTime' : '2011-01'})

df1 = data_response1.write(data_response1.data.series, parse_time=False)
df1.to_csv('M1'+'_'+str(now.year)+"_"+str(now.month)+"_"+str(now.day)+".csv")

# M3 크롤링
oecd = pdsx.Request('OECD', proxies = {'timeout':'5000'})
data_response2 = oecd.data(resource_id='MEI_FIN', key='MABM.AUS+CAN+CHL+CZE+DNK+HUN+ISL+ISR+JPN+KOR+MEX+NZL+NOR+POL+SWE+CHE+TUR+GBR+USA+EA19+OECD+NMEC+BRA+CHN+COL+IND+IDN+RUS+ZAF.M/all', params={'startTime' : '2011-01'})

df2 = data_response2.write(data_response2.data.series, parse_time=False)
df2.to_csv('M3'+'_'+str(now.year)+"_"+str(now.month)+"_"+str(now.day)+".csv")

# BCI 크롤링
oecd = pdsx.Request('OECD', proxies = {'timeout':'5000'})
data_response3 = oecd.data(resource_id='MEI_CLI', key='BSCICP03.AUS+AUT+BEL+CHL+CZE+DNK+EST+FIN+FRA+DEU+GRC+HUN+ITA+JPN+KOR+LUX+MEX+NLD+NZL+NOR+POL+PRT+SVK+SVN+ESP+SWE+CHE+TUR+GBR+USA+BRA+CHN+IND+IDN+RUS+ZAF.M/all', params={'startTime' : '2010-01'})

df3 = data_response3.write(data_response3.data.series, parse_time=False)
df3.to_csv('BCI'+'_'+str(now.year)+"_"+str(now.month)+"_"+str(now.day)+".csv")

# CCI 크롤링
oecd = pdsx.Request('OECD', proxies = {'timeout':'5000'})
data_response4 = oecd.data(resource_id='MEI_CLI', key='CSCICP03.AUS+AUT+BEL+CAN+CHL+CZE+DNK+EST+FIN+FRA+DEU+GRC+HUN+IRL+ITA+JPN+KOR+LUX+MEX+NLD+NZL+POL+PRT+SVK+SVN+ESP+SWE+CHE+TUR+GBR+USA+BRA+CHN+IDN.M/all', params={'startTime' : '2011-01'})

df4 = data_response4.write(data_response4.data.series, parse_time=False)
df4.to_csv('CCI'+'_'+str(now.year)+"_"+str(now.month)+"_"+str(now.day)+".csv")

# CLI 크롤링
oecd = pdsx.Request('OECD', proxies = {'timeout':'5000'})
data_response5 = oecd.data(resource_id='MEI_CLI', key='LOLITOAA.AUS+AUT+BEL+BRA+CAN+CHL+CHN+CZE+DNK+EST+FIN+FRA+DEU+GRC+HUN+IND+IDN+IRL+ISR+ITA+JPN+KOR+NLD+POL+PRT+RUS+SVN+ZAF+ESP+SWE+TUR+GBR+USA+ISL.M/all', params={'startTime' : '2011-01'})

df5 = data_response5.write(data_response5.data.series, parse_time=False)
df5.to_csv('CLI'+'_'+str(now.year)+"_"+str(now.month)+"_"+str(now.day)+".csv")

# Euro Future Tendency 크롤링
oecd = pdsx.Request('OECD', proxies = {'timeout':'5000'})
data_response6 = oecd.data(resource_id='MEI_BTS_COS', key='BRBUFT.EA19.BLSA.Q+M/all', params={'startTime' : '2010-01'})

df6 = data_response6.write(data_response6.data.series, parse_time=False)
df6.to_csv('Euro_Future_Tendency'+"_"+str(now.year)+"_"+str(now.month)+"_"+str(now.day)+".csv")

# Euro Total Retail 크롤링
oecd = pdsx.Request('OECD', proxies = {'timeout':'5000'})
data_response7 = oecd.data(resource_id='MEI', key='EA19.SLRTTO01.IXOBSA.M/all', params={'startTime' : '2010-01'})

df7 = data_response7.write(data_response7.data.series, parse_time=False)
df7.to_csv('Euro_Total_Retail'+"_"+str(now.year)+"_"+str(now.month)+"_"+str(now.day)+".csv")
```

## IMF 데이터 크롤링하기
1. **imf.py** 라는 파일을 생성합니다.
2. 같은 폴더에 **imfoutput.csv** 라는 파일을 엑셀을 통해 생성합니다.
3. 아래 코드를 입력합니다.
```Python
import os
import datetime
from urllib.request import urlopen
from bs4 import BeautifulSoup
import csv

now = datetime.datetime.now()

print("Starting year")
sy = input()

print ("End year")
ey = input()

html = urlopen("https://www.imf.org/external/pubs/ft/weo/2015/02/weodata/weorept.aspx?pr.x=54&pr.y=11&sy=" + sy + "&ey=" + ey + "&scsm=1&ssd=1&sort=country&ds=.&br=1&c=512%2C668%2C914%2C672%2C612%2C946%2C614%2C137%2C311%2C962%2C213%2C674%2C911%2C676%2C193%2C548%2C122%2C556%2C912%2C678%2C313%2C181%2C419%2C867%2C513%2C682%2C316%2C684%2C913%2C273%2C124%2C868%2C339%2C921%2C638%2C948%2C514%2C943%2C218%2C686%2C963%2C688%2C616%2C518%2C223%2C728%2C516%2C558%2C918%2C138%2C748%2C196%2C618%2C278%2C624%2C692%2C522%2C694%2C622%2C142%2C156%2C449%2C626%2C564%2C628%2C565%2C228%2C283%2C924%2C853%2C233%2C288%2C632%2C293%2C636%2C566%2C634%2C964%2C238%2C182%2C662%2C453%2C960%2C968%2C423%2C922%2C935%2C714%2C128%2C862%2C611%2C135%2C321%2C716%2C243%2C456%2C248%2C722%2C469%2C942%2C253%2C718%2C642%2C724%2C643%2C576%2C939%2C936%2C644%2C961%2C819%2C813%2C172%2C199%2C132%2C733%2C646%2C184%2C648%2C524%2C915%2C361%2C134%2C362%2C652%2C364%2C174%2C732%2C328%2C366%2C258%2C734%2C656%2C144%2C654%2C146%2C336%2C463%2C263%2C528%2C268%2C923%2C532%2C738%2C944%2C578%2C176%2C537%2C534%2C742%2C536%2C866%2C429%2C369%2C433%2C744%2C178%2C186%2C436%2C925%2C136%2C869%2C343%2C746%2C158%2C926%2C439%2C466%2C916%2C112%2C664%2C111%2C826%2C298%2C542%2C927%2C967%2C846%2C443%2C299%2C917%2C582%2C544%2C474%2C941%2C754%2C446%2C698%2C666&s=NGDPDPC&grp=0&a=")
#st=2013&ey=2020 부분을 바꾸면서 검색해보면 될듯
bsObj = BeautifulSoup(html, "html.parser")
table = bsObj.find('table', {'class': "fancy"})

output_rows = []
for table_row in table.findAll('tr'):
    columns = table_row.findAll('td')
    output_row = []
    for column in columns:
        output_row.append(column.text)
    output_rows.append(output_row)

with open('imfoutput.csv', 'w') as csvfile:
    writer = csv.writer(csvfile)
    writer.writerows(output_rows)
```
3. 위 코드는 프롬프트 창에서 데이터의 starting year, end year을 입력하여 해당되는 값을 받을 수 있또록 하는 프로그램입니다.

## FxStreet, Census, 한국무역협회에서 데이터 크롤링하기
1. **rest.py** 파일을 한번 더 생성합니다.
2. **censusoutput.csv, JapOutput.csv, joblessoutput.csv, Koreaoutput.csv** 파일을 생성합니다.
3. 아래의 코드를 한번 더 복붙하여 저장합니다.

```Python
from selenium import webdriver
from selenium.webdriver.support.ui import WebDriverWait
import time
import csv
import pandas as pd

# 이 곳은 각자 컴퓨터의 디렉토리명을 입력해야 합니다. Chrome Driver 이 들어있는 폴더(MacroDatas)의 경로를 path에 입력해 주시면 됩니다. (Username) 이 부분만 각자 컴퓨터 사용자명으로 바꾸면 될 것입니다.
path = 'C:\Users\(Username)\Desktop\MacroDatas\chromedriver'
driver = webdriver.Chrome(path)

#Census - Trade in goods(New Order)
driver.implicitly_wait(3)
driver.get("https://www.census.gov/foreign-trade/balance/c0004.html")

table = driver.find_element_by_xpath('//*[@id="middle-column"]/div/div[1]/table')
rows = table.find_elements_by_tag_name('tr')

output_rows = []
for row in rows:
    columns = row.find_elements_by_tag_name('td')
    output_row = []
    for column in columns:
        output_row.append(column.text)
    output_rows.append(output_row)

with open('censusoutput.csv', 'w') as csvfile:
    writer = csv.writer(csvfile)
    writer.writerows(output_rows)

#FxStreet - Japan IP
driver.implicitly_wait(3)
driver.get("https://www.fxstreet.com/economic-calendar/event/34003141-e6cf-4c81-939c-5e39e0170b33")

table = driver.find_element_by_xpath('//*[@id="Content_C022_Col01"]/div[2]/div/div/table[2]')
rows = table.find_elements_by_tag_name('tr')

output_rows = []
for row in rows:
    columns = row.find_elements_by_tag_name('td')
    output_row = []
    for column in columns:
        output_row.append(column.text)
    output_rows.append(output_row)

with open('JapOutput.csv', 'w') as csvfile:
    writer = csv.writer(csvfile)
    writer.writerows(output_rows)

# FxStreet - US Jobless
driver.implicitly_wait(3)
driver.get("https://www.fxstreet.com/economic-calendar/event/9c689bbf-af2a-4f65-81a8-c5f5e2b78d70")

table = driver.find_element_by_xpath('//*[@id="Content_C022_Col01"]/div[2]/div/div/table[2]')
rows = table.find_elements_by_tag_name('tr')

output_rows = []
for row in rows:
    columns = row.find_elements_by_tag_name('td')
    output_row = []
    for column in columns:
        output_row.append(column.text)
    output_rows.append(output_row)

with open('joblessoutput.csv', 'w') as csvfile:
    writer = csv.writer(csvfile)
    writer.writerows(output_rows)

#한국무역협회 - Korea Export
driver.implicitly_wait(3)
driver.get("http://stat.kita.net/stat/kts/sum/SumImpExpTotalList.screen")

table = driver.find_element_by_xpath('//*[@id="mySheet1"]/tbody/tr[3]/td/div/div[1]/table/tbody')
rows = table.find_elements_by_tag_name('tr')

output_rows = []
for row in rows:
    columns = row.find_elements_by_tag_name('td')
    output_row = []
    for column in columns:
        output_row.append(column.text)
    output_rows.append(output_row)

with open('Koreaoutput.csv', 'w') as csvfile:
    writer = csv.writer(csvfile)
    writer.writerows(output_rows)
```

## Investing.com 에서 crude oil, copper 일일수익률 데이터 크롤링하기
1. **investing.py** 파일을 생성합니다.
2. **copperoutput.csv, crudeoutput.csv** 파일을 생성합니다.
3. 아래 코드를 복붙하고, path의 사용자명을 수정합니다.
```Python
from selenium import webdriver
from selenium.webdriver.support.ui import WebDriverWait, Select
import time
import csv
import pandas as pd

# 이 곳은 각자 컴퓨터의 디렉토리명을 입력해야 합니다. Chrome Driver 이 들어있는 폴더(MacroDatas)의 경로를 path에 입력해 주시면 됩니다. (Username) 이 부분만 각자 컴퓨터 사용자명으로 바꾸면 될 것입니다.
path = 'C:\Users\(Username)\Desktop\MacroDatas\chromedriver'
driver = webdriver.Chrome(path)
# Copper
driver.implicitly_wait(3)
driver.get("https://www.investing.com/commodities/copper-historical-data")

select = driver.find_element_by_id('data_interval')
for option in select.find_elements_by_tag_name('option'):
    if option.text == 'Monthly':
        option.click()
        break

table = driver.find_element_by_xpath('//*[@id="curr_table"]')
rows = table.find_elements_by_tag_name('tr')

output_rows = []
for row in rows:
    columns = row.find_elements_by_tag_name('td')
    output_row = []
    for column in columns:
        output_row.append(column.text)
    output_rows.append(output_row)

with open('copperoutput.csv', 'w') as csvfile:
    writer = csv.writer(csvfile)
    writer.writerows(output_rows)

# Crude Oil
driver.implicitly_wait(3)
driver.get("https://www.investing.com/commodities/crude-oil-historical-data")

select = driver.find_element_by_id('data_interval')
for option in select.find_elements_by_tag_name('option'):
    if option.text == 'Monthly':
        option.click()
        break

table = driver.find_element_by_xpath('//*[@id="curr_table"]')
rows = table.find_elements_by_tag_name('tr')

output_rows = []
for row in rows:
    columns = row.find_elements_by_tag_name('td')
    output_row = []
    for column in columns:
        output_row.append(column.text)
    output_rows.append(output_row)

with open('crudeoutput.csv', 'w') as csvfile:
    writer = csv.writer(csvfile)
    writer.writerows(output_rows)
```

##### 해당 사용설명서대로 진행되지 않을 경우, 주저하지 마시고 연락 주시길 바랍니다. 엄청난 완성물은 아니나 지적 자산이니 외부에 유출하진 말아주세요. 오늘도 수고하셨습니다.