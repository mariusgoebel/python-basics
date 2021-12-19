# python-basics

#Run this prior 
from random import randint as rnd

memReg = 'members.txt'
exReg = 'inactive.txt'
fee =('yes','no')

def genFiles(current,old):
    with open(current,'w+') as writefile: 
        writefile.write('Membership No  Date Joined  Active  \n')
        data = "{:^13}  {:<11}  {:<6}\n"

        for rowno in range(20):
            date = str(rnd(2015,2020))+ '-' + str(rnd(1,12))+'-'+str(rnd(1,25))
            writefile.write(data.format(rnd(10000,99999),date,fee[rnd(0,1)]))


    with open(old,'w+') as writefile: 
        writefile.write('Membership No  Date Joined  Active  \n')
        data = "{:^13}  {:<11}  {:<6}\n"
        for rowno in range(3):
            date = str(rnd(2015,2020))+ '-' + str(rnd(1,12))+'-'+str(rnd(1,25))
            writefile.write(data.format(rnd(10000,99999),date,fee[1]))


    genFiles(memReg,exReg)

    def cleanFiles(currentMem,exMem):

    '''
    currentMem: File containing list of current members
    exMem: File containing list of old members
    
    Removes all rows from currentMem containing 'no' and appends them to exMem
    '''
    
    with open(currentMem,'r+') as writeFile: 
        with open(exMem,'a+') as appendFile:
            #get the data
            writeFile.seek(0)
            members = writeFile.readlines()
            #remove header
            header = members[0]
            members.pop(0)
                
            inactive = [member for member in members if ('no' in member)]
            '''
            The above is the same as 

            for member in members:
            if 'no' in member:
                inactive.append(member)
            '''
            #go to the beginning of the write file
            writeFile.seek(0) 
            writeFile.write(header)
            for member in members:
                if (member in inactive):
                    appendFile.write(member)
                else:
                    writeFile.write(member)      
            writeFile.truncate()
                
    
    pass 


    # Code to help you see the files
    # Leave as is
    memReg = 'members.txt'
    exReg = 'inactive.txt'
    cleanFiles(memReg,exReg)


    headers = "Membership No  Date Joined  Active  \n"
    with open(memReg,'r') as readFile:
        print("Active Members: \n\n")
        print(readFile.read())
    
    with open(exReg,'r') as readFile:
        print("Inactive Members: \n\n")
        print(readFile.read())
    
    
########### Crypto Chart ###########

    cg = CoinGeckoAPI()

    bitcoin_data = cg.get_coin_market_chart_by_id(id='bitcoin', vs_currency='usd', days=30)
    
    type(bitcoin_data )
    
    bitcoin_price_data = bitcoin_data['prices']

    bitcoin_price_data[0:5]
    
    data = pd.DataFrame(bitcoin_price_data, columns=['TimeStamp', 'Price'])
    
    data['date'] = data['TimeStamp'].apply(lambda d: datetime.date.fromtimestamp(d/1000.0))

    candlestick_data = data.groupby(data.date, as_index=False).agg({"Price": ['min', 'max', 'first', 'last']})
    
    fig = go.Figure(data=[go.Candlestick(x=candlestick_data['date'],
                open=candlestick_data['Price']['first'], 
                high=candlestick_data['Price']['max'],
                low=candlestick_data['Price']['min'], 
                close=candlestick_data['Price']['last'])
                ])

    fig.update_layout(xaxis_rangeslider_visible=False)

    fig.show()
    
    
    ########### IBM Watson Speech to Text ###########

!pip install ibm_watson wget

from ibm_watson import SpeechToTextV1 
import json
from ibm_cloud_sdk_core.authenticators import IAMAuthenticator

url_s2t = "service URL"

iam_apikey_s2t = "service API key"

authenticator = IAMAuthenticator(iam_apikey_s2t)
s2t = SpeechToTextV1(authenticator=authenticator)
s2t.set_service_url(url_s2t)
s2t

!wget -O PolynomialRegressionandPipelines.mp3  https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-PY0101EN-SkillsNetwork/labs/Module%205/data/PolynomialRegressionandPipelines.mp3


filename='PolynomialRegressionandPipelines.mp3'

with open(filename, mode="rb")  as wav:
    response = s2t.recognize(audio=wav, content_type='audio/mp3')
    
response.result

from pandas import json_normalize

json_normalize(response.result['results'],"alternatives")

recognized_text=response.result['results'][0]["alternatives"][0]["transcript"]
type(recognized_text)

############ IBM Watson Language Translator ###########

from ibm_watson import LanguageTranslatorV3

url_lt='Service URL'

apikey_lt='Service API Key'

version_lt='2018-05-01'

authenticator = IAMAuthenticator(apikey_lt)
language_translator = LanguageTranslatorV3(version=version_lt,authenticator=authenticator)
language_translator.set_service_url(url_lt)
language_translator

from pandas import json_normalize

json_normalize(language_translator.list_identifiable_languages().get_result(), "languages")

translation_response = language_translator.translate(\
    text=recognized_text, model_id='en-es')
translation_response

translation=translation_response.get_result()
translation

spanish_translation =translation['translations'][0]['translation']
spanish_translation 

translation_new = language_translator.translate(text=spanish_translation ,model_id='es-en').get_result()

translation_eng=translation_new['translations'][0]['translation']
translation_eng

# Write your code below and press Shift+Enter to execute
translation_response = language_translator.translate(\
    text=recognized_text, model_id='en-fr').get_result()
translation


