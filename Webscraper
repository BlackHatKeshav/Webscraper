from bs4 import BeautifulSoup
import requests
import pandas as pd
import numpy as np

def webscraper():
    BASE_URL = [
    'https://eezee.sg/product/honeywell-open-path-gd-short-range-system-02104-n-nsna-4a54149ad5',
    'https://eezee.sg/product/honeywell-receiver-std-version-short-range-atex-2104b2111-58010fddc0',
]

    df = pd.DataFrame(columns=['Description','Manufacturer','Part Number','Price'])

    for b in BASE_URL:
        html = requests.get(b).text
        soup = BeautifulSoup(html, "html.parser")
    
        body = soup.find('body')
        price = body.find('span', class_='heading')
        title = body.find('span', class_= 'jsx-514542 sub-heading grey-base')     
        table = body.find('table',class_='jsx-9035620f2783890c')
        model = table.find_all('span', class_='eezee-blue')[0]
        brand = table.find_all('span', class_='eezee-blue')[1]
        cleanprice = price.text.replace('S$',' ').lstrip()
        l = [title.text,brand.text,model.text,cleanprice] 
        my_series = pd.Series(data = l, index = [ 'Description','Manufacturer','Part Number','Price'])
        print(my_series)
    
        df = df.append(my_series, ignore_index=True)

    writer = pd.ExcelWriter("Eezee_Prices.xlsx", engine='xlsxwriter')

    df.to_excel(writer,index=False, sheet_name='Sheet1')

    workbook  = writer.book
    worksheet = writer.sheets['Sheet1']

    format = workbook.add_format()
    format.set_align('center')
    format.set_align('vcenter')

    worksheet.set_column(0, 1, 60, format)
    worksheet.set_column(1, 2, 15, format)
    worksheet.set_column(2, 3, 20, format)
    worksheet.set_column(3, 4, 15, format)
    
    writer.save()

def difference():
    df1=pd.read_excel('Eezee_Prices.xlsx')
    df2=pd.read_excel('Eezee_Prices1.xlsx')

    df1.equals(df2)

    comparison_values = df1.values == df2.values
    print(comparison_values)
    for x in comparison_values:
        if x == False:
            print(x)

    rows,cols=np.where(comparison_values==False)

    for item in zip(rows,cols):
        df1.iloc[item[0], item[1]] = '{} --> {}'.format(df1.iloc[item[0], item[1]],df2.iloc[item[0], item[1]])

    df1.to_excel('./Excel_diff.xlsx',index=False,header=True)

    writer = pd.ExcelWriter("Excel_diff.xlsx", engine='xlsxwriter')

    df1.to_excel(writer,index=False, sheet_name='Sheet1')

    workbook  = writer.book
    worksheet = writer.sheets['Sheet1']

    format = workbook.add_format()
    format.set_align('center')
    format.set_align('vcenter')

    worksheet.set_column(0, 1, 60, format)
    worksheet.set_column(1, 2, 15, format)
    worksheet.set_column(2, 3, 20, format)
    worksheet.set_column(3, 4, 15, format)
    
    writer.save()

webscraper()
