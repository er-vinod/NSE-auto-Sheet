 साथियो नमस्कारआज में आपको एक ऐसी जादुई गूगल शीट पायथन कोड, गूगल क्लाउड एपीआई व गिटहब की सहायता से तैयार करना सीखाने वाला हूं जो आपको वोल्यूम के साथ ब्रेकआउट वाले स्टॉक्स अपने आप सेलेक्ट करके देगी।यह पूरा कोर्स है जो एकदम फ्री है। इस कोर्स के वीडियोज की प्लेलिस्ट आपको मेरे यूटयूब चैनल महेश चन्द्र कौशिक पर मिल जायेगी। इस प्लेलिस्ट का लिंक निम्न हैः- https://www.youtube.com/playlist?list=PL-X8WTMcEbY-UTbq_OiXsFoiLZbEzxE4Y

आज मैं आपको एक ऐसा तरीका बताने जा रहा हूँ जिससे आप खुद की एक ऐसी 'स्मार्ट गूगल शीट' बना सकते हैं, जो हर शाम बाज़ार बंद होने के बाद खुद-ब-खुद NSE से नया डेटा लाएगी, उसमें से सबसे ज़्यादा वॉल्यूम वाले टॉप 250 असली शेयर छांटेगी, और आपके फॉर्मूले के हिसाब से आपको बेहतरीन स्टॉक्स की लिस्ट निकाल कर दे देगी।
चाहे आपको कोडिंग का 'C' भी न आता हो, इस गाइड को स्टेप-बाय-स्टेप फॉलो करें और आपका अपना रोबोट तैयार हो जाएगा!

स्टेप 1: अपनी गूगल शीट तैयार करना
सबसे पहले हमें एक खाली गूगल शीट चाहिए जहाँ सारा डेटा आएगा।

Google Sheets खोलें और एक नई शीट बनाएँ।

इसमें नीचे '+' पर क्लिक करके दो टैब (Sheet) बनाएँ।

पहले टैब का नाम रखें: Top 250 Stocks

दूसरे टैब का नाम रखें: Final List

Top 250 Stocks शीट में पहली लाइन (Row 1) में ये हेडिंग्स डाल दें
A1: NSE Code

B1: Volume

C1: Close Price (कॉलम D से आगे हम 50 डीएमए 100 डीएमए 200 डीएमए सीएआर आदि कुछ फार्मूले लगायेगें जो इस पोस्ट में आगे सीखाये जायेगें अभी आपको अपना पूरा फोकस एनएसई से सबसे ज्यादा वोल्यूम वाले टॉप 250 स्टॉक्स की लिस्ट रोज ऑटोमेटिक निकालने करने पर रखना है।!)
स्टेप 2: गूगल क्लाउड (Google Cloud) से API 'चाबी' लेना
हमारी पायथन स्क्रिप्ट को आपकी शीट में डेटा लिखने के लिए एक सुरक्षित रास्ते की ज़रूरत है। इसके लिए हम एक 'Service Account' (रोबोट) बनाएंगे।

Google Cloud Console API Library पर जाएँ।

सर्च बॉक्स में "Google Sheets API" ढूँढें और उसे Enable कर दें।

फिर से सर्च करें "Google Drive API" और उसे भी Enable कर दें।

अब बाएँ मेनू में "Credentials" पर क्लिक करें। ऊपर "+ Create Credentials" पर जाएँ और "Service Account" चुनें।

इसे नाम दें (जैसे: stock-updater-bot) और Create कर दें।

अब सर्विस अकाउंट की लिस्ट में उस नए अकाउंट के Email ID (जो ...iam.gserviceaccount.com जैसा होगा) को कॉपी कर लें।

उसी ईमेल पर क्लिक करें, Keys टैब में जाएँ -> Add Key -> Create new key पर क्लिक करें। JSON चुनें और डाउनलोड कर लें। यह आपकी तिजोरी की चाबी है!

ज़रूरी काम: अब अपनी गूगल शीट पर वापस आएँ। ऊपर दाईं ओर Share बटन दबाएं और उस कॉपी की हुई Service Account Email ID को 'Editor' का एक्सेस देकर सेव कर दें।

स्टेप 3: GitHub पर अपना ऑटोमेशन सेट अप करना
अब हम अपने कोड को एक ऐसे सर्वर पर रखेंगे जो रोज़ शाम को अपने आप चलेगा। इसके लिए GitHub (जो बिल्कुल फ्री है) का इस्तेमाल करेंगे।

GitHub.com पर जाकर अपना अकाउंट बनाएँ या लॉगिन करें।क्लाउड संग्रहण

दाएँ कोने में '+' दबाएं और New repository चुनें। नाम दें (जैसे: NSE-Auto-Sheet) और इसे Private रखें।

अब अपनी रिपॉजिटरी की Settings में जाएँ। बाएँ मेनू में नीचे Secrets and variables > Actions पर क्लिक करें।

New repository secret बटन दबाएं। Name में लिखें: GCP_CREDENTIALS।

जो JSON फाइल आपने डाउनलोड की थी, उसे नोटपैड (Notepad) में खोलें, उसका सारा टेक्स्ट कॉपी करें और यहाँ Secret बॉक्स में पेस्ट करके सेव कर दें।

स्टेप 4: पायथन स्क्रिप्ट (Python Script) डालना
अब हम वह जादुई कोड डालेंगे जो NSE के नए सिस्टम से डेटा लाएगा।

रिपॉजिटरी के मुख्य पेज (Code टैब) पर Add file > Create new file पर क्लिक करें।

फाइल का नाम दें: update_sheet.py

नीचे दिया गया पूरा कोड कॉपी करके उस बॉक्स में पेस्ट कर दें:

Python

import gspread
from oauth2client.service_account import ServiceAccountCredentials
import pandas as pd
import requests
import zipfile
import io
from datetime import datetime, timedelta
import os
import json

# 1. Credentials Setup
creds_json = os.environ.get('GCP_CREDENTIALS')
creds_dict = json.loads(creds_json)
scope = ["https://spreadsheets.google.com/feeds", "https://www.googleapis.com/auth/drive"]
creds = ServiceAccountCredentials.from_json_keyfile_dict(creds_dict, scope)
client = gspread.authorize(creds)

# अपनी गूगल शीट की ID यहाँ डालें (URL के बीच का हिस्सा)
spreadsheet_id = "यहाँ_अपनी_शीट_की_ID_डालें" 
worksheet = client.open_by_key(spreadsheet_id).worksheet("Top 250 Stocks")

# 2. NSE UDiFF Data Fetcher
def fetch_bhavcopy_for_date(date_obj):
    date_str = date_obj.strftime("%Y%m%d")
    url = f"https://nsearchives.nseindia.com/content/cm/BhavCopy_NSE_CM_0_0_0_{date_str}_F_0000.csv.zip"
    
    headers = {
        'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36',
        'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8'
    }
    
    try:
        response = requests.get(url, headers=headers, timeout=15)
        if response.status_code == 200:
            with zipfile.ZipFile(io.BytesIO(response.content)) as z:
                csv_filename = z.namelist()[0]
                with z.open(csv_filename) as f:
                    df = pd.read_csv(f)
                    
                    sym_col = 'TckrSymb' if 'TckrSymb' in df.columns else 'SYMBOL'
                    close_col = 'ClsPric' if 'ClsPric' in df.columns else 'CLOSE'
                    series_col = 'SctySrs' if 'SctySrs' in df.columns else 'SERIES'
                    
                    vol_col = 'TtlTradgVol'
                    for c in ['TtlTradgVol', 'TtlTrdQty', 'TotTrdQty', 'TOTTRDQTY']:
                        if c in df.columns:
                            vol_col = c
                            break
                    
                    # सिर्फ EQ सीरीज और ETFs (LIQUID/BEES) को बाहर करना
                    if series_col in df.columns:
                        df = df[df[series_col].astype(str).str.strip() == 'EQ']
                    filter_keywords = 'BEES|ETF|GOLD|LIQUID|CASE|SILVER|LIQ'
                    df = df[~df[sym_col].astype(str).str.contains(filter_keywords, case=False, na=False)]
                    
                    df_top = df.sort_values(by=vol_col, ascending=False).head(250)
                    return df_top[[sym_col, vol_col, close_col]].values.tolist()
        return None
    except:
        return None

# 3. Execution Logic
date = datetime.now()
data_to_insert = None
fetched_date_str = ""

for i in range(5): 
    test_date = date - timedelta(days=i)
    if test_date.weekday() >= 5: continue
        
    data_to_insert = fetch_bhavcopy_for_date(test_date)
    if data_to_insert:
        fetched_date_str = test_date.strftime('%d-%b-%Y')
        break

# 4. Update Sheet
if data_to_insert:
    worksheet.batch_clear(['A2:C251'])
    worksheet.update('A2', data_to_insert)
    ist_now = (datetime.utcnow() + timedelta(hours=5, minutes=30)).strftime('%d-%b %H:%M')
    status_msg = f"Data Date: {fetched_date_str} | Last Update: {ist_now} (IST)"
    worksheet.update('K2', [[status_msg]])
    print("SUCCESS: Sheet Updated!")
ध्यान दें: कोड में spreadsheet_id की जगह अपनी शीट के URL से ID निकाल कर ज़रूर डालें। फिर Commit changes पर क्लिक करें।प्रोग्रामिंग

स्टेप 5: टाइमर (Cron) सेट करना
हम इस कोड को रोज़ रात 8:15 बजे चलाएंगे ताकि GitHub के सर्वर पर ट्रैफिक न मिले।

अपने GitHub में Actions टैब पर क्लिक करें।

"set up a workflow yourself" पर क्लिक करें।

नीचे दिया गया कोड पेस्ट करें:

YAML

name: Update NSE Stocks to Google Sheet

on:
  schedule:
    # यह समय 14:45 UTC है, जिसका मतलब भारत में रात 8:15 PM है
    - cron: '45 14 * * 1-5'
  workflow_dispatch:

jobs:
  update-sheet:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Repo
        uses: actions/checkout@v4

      - name: Setup Python
        uses: actions/setup-python@v5
        with:
          python-version: '3.10'

      - name: Install Libraries
        run: pip install gspread oauth2client pandas requests

      - name: Run Python Script
        env:
          GCP_CREDENTIALS: ${{ secrets.GCP_CREDENTIALS }}
        run: python update_sheet.py
इसे भी Commit changes कर दें। बस, आपका ऑटोमेशन तैयार है!स्क्रिप्टिंग भाषाएं

ज़्यादा जानें
पायथन स्क्रिप्ट ट्यूटोरियल
Cloud
स्टॉक और बॉन्ड
यहां तक इस कोर्स की कक्षा 1 थी इससे आगे अगली कक्षा में सीखाया जायेगा इस कक्षा का फ्री वीडियो यूटयूब पर निम्न लिंक पर देख सकते हैं:- https://youtu.be/MnIJ6kx6ww4




🚀 ऑटोमैटिक स्टॉक स्कैनर कैसे बनाएं - कक्षा 2 (Moving Averages & Advanced CAR Formula)
नमस्कार दोस्तों! 'ऑटोमैटिक स्टॉक एनालाइज़र शीट' बनाने की इस मास्टरक्लास (कक्षा 2) में आपका स्वागत है।वॉल्यूम विश्लेषण

कक्षा 1 के वीडियो और ब्लॉग पोस्ट में हमने सीखा था कि कैसे गिटहब (GitHub) की मदद से रोज़ाना शाम को NSE से टॉप 250 शेयरों का रॉ डेटा (Raw Data) अपनी गूगल शीट में अपने आप कैसे लाया जाए।

आज की इस कक्षा 2 में हम उस रॉ डेटा में मेरे (महेश कौशिक) कुछ एडवांस फॉर्मूले लगाएंगे और इसे एक 'स्मार्ट स्कैनर' में बदलेंगे। हम अपनी 'Top 250 Stocks' शीट में 50, 100 और 200 दिन का मूविंग एवरेज निकालेंगे, और सबसे खास—मेरा बनाया हुआ एडवांस CAR (Cumulative Average Rule) रेटिंग फॉर्मूला लगाएंगे जो आपको बताएगा कि स्टॉक खरीदने लायक है या नहीं।

ज़्यादा जानें
स्प्रेडशीट सॉफ़्टवेयर
वितरित और क्लाउड कम्प्यूटिंग
आर्थिक बाज़ार के समाचार
तो चलिए, अपनी गूगल शीट खोलिए और मेरे साथ ये 5 आसान स्टेप्स फॉलो कीजिए:

स्टेप 1: लाइव प्राइस (CMP) लाना
हमारी शीट के कॉलम A, B और C में पायथन ने डेटा डाल दिया है। अब हम सेल D2 (CMP) में गूगल फाइनेंस का फॉर्मूला लगाएंगे ताकि दिन में हमें लाइव भाव दिखे।

सेल D2 में यह फॉर्मूला पेस्ट करें:

=GOOGLEFINANCE("NSE:"&A2,"price")
स्टेप 2: मूविंग एवरेज (50, 100 और 200 DMA)
अब हम वो इंडिकेटर्स लगाएंगे जो स्टॉक का ट्रेंड (Trend) बताते हैं।

सेल E2 (50 DMA) में पेस्ट करें:

=IFERROR(AVERAGE(QUERY(SORT(GOOGLEFINANCE("NSE:"&A2,"price",TODAY()-75,TODAY()),1,0),"select Col2 limit 50")), "")
सेल F2 (100 DMA) में पेस्ट करें:

=IFERROR(AVERAGE(QUERY(SORT(GOOGLEFINANCE("NSE:"&A2,"price",TODAY()-150,TODAY()),1,0),"select Col2 limit 100")), "")
सेल G2 (200 DMA) में पेस्ट करें:

=IFERROR(AVERAGE(QUERY(SORT(GOOGLEFINANCE("NSE:"&A2,"price",TODAY()-300,TODAY()),1,0),"select Col2 limit 200")), "")
स्टेप 3: बुल रन (Bull Run) और 200 DMA से दूरी
सेल H2 (Output) में हम एक शर्त लगाएंगे जो बताएगी कि स्टॉक बुल रन में है या नहीं। सेल H2 में पेस्ट करें:

=IF(AND(D2>E2, D2>F2, D2>G2, I2>=0.01, I2<=10), "In Bull Run",
IF(AND(D2<E2, D2<F2, D2<G2, I2>=-10, I2<=-0.01), "In Bear Run", "Unconfirmed"))
ज़्यादा जानें
cloud
सुपर ब्रेकआउट स्टॉक
दलाली और डे ट्रेडिंग
सेल I2 (Difference from 200 DMA) में हम 200 DMA से प्रतिशत दूरी निकालेंगे। सेल I2 में पेस्ट करें:

=((D2-G2)*100)/G2

स्टेप 4: मेरा एडवांस CAR (Cumulative Average Rule) फॉर्मूला
यह इस शीट का सबसे ताकतवर हिस्सा है। यह फॉर्मूला पिछले 52 हफ्तों के हाई (High) का पता लगाता है, वहाँ से अब तक का डेटा निकालकर उसका औसत निकालता है, और यह चेक करता है कि क्या यह स्टॉक लगातार ऊपर की ओर जा रहा है।

(नोट: यह फॉर्मूला लंबा है, आप इस पूरे बॉक्स को एक साथ कॉपी करके सेल J2 में पेस्ट कर सकते हैं, गूगल शीट इसे आसानी से समझ लेगा)

सेल J2 (CAR Rating) में यह पूरा फॉर्मूला कॉपी करके पेस्ट करें:

=IFERROR(IF(A2="","ENTER STOCK",

  LET(

    raw_high, GOOGLEFINANCE("NSE:" & A2, "high", TODAY()-365, TODAY()),

    high_date, IFERROR(TO_DATE(QUERY(raw_high, "SELECT Col1 WHERE Col2 IS NOT NULL ORDER BY Col2 DESC LIMIT 1 LABEL Col1 ''", 1)), TODAY()-30),

    raw_data, IFERROR(GOOGLEFINANCE("NSE:" & A2, "close", high_date, TODAY()), GOOGLEFINANCE("NSE:" & A2, "close", TODAY()-10, TODAY())),

    prices, IFERROR(CHOOSEROWS(INDEX(raw_data, 0, 2), SEQUENCE(ROWS(raw_data)-1, 1, 2, 1)), {0}),

    count_rows, ROWS(prices),

    cum_avg, SCAN(0, SEQUENCE(count_rows), LAMBDA(a,n, AVERAGE(CHOOSEROWS(prices, SEQUENCE(n))))),

    last_10, IF(count_rows < 10, {0;0;0;0;0;0;0;0;0;0}, CHOOSEROWS(cum_avg, SEQUENCE(10, 1, count_rows - 9, 1))),

    check, SUMPRODUCT(--(CHOOSEROWS(last_10, SEQUENCE(9, 1, 2, 1)) > CHOOSEROWS(last_10, SEQUENCE(9, 1, 1, 1)))),

    IF(count_rows < 10, "Short History", IF(check = 9, "Buy/Average Out", "Avoid/Hold"))

  )


), "TICKER NOT FOUND")


👉 ज़रूरी काम: अब सेल D2 से लेकर J2 तक के सभी फॉर्मूलों को एक साथ सेलेक्ट करें और नीचे Row 250 तक ड्रैग (Drag) कर दें (या डबल-क्लिक कर दें)। आपका सारा डेटा कैलकुलेट हो जाएगा!

स्टेप 5: मास्टर फॉर्मूला (Final List तैयार करना)
निश्चित रूप से, यहाँ आपके 'फाईनल लिस्ट' (Final List) वाले स्टेप का पूरा विवरण है, जिसमें मास्टर फॉर्मूला को दो लाइनों में इस तरह व्यवस्थित किया गया है कि यह ब्लॉग पर आसानी से फिट हो जाए और आपके फॉलोवर्स इसे सीधे कॉपी-पेस्ट कर सकें:

स्टेप 5: फाईनल लिस्ट (Final List) तैयार करना
यह इस पूरी प्रोसेस का सबसे महत्वपूर्ण हिस्सा है। यहाँ हम एक मास्टर फॉर्मूले की मदद से अपनी पसंद के बेहतरीन स्टॉक्स को अलग करेंगे।

शीट का सेटअप:

अपनी 'Final List' वाली शीट खोलें।

पहली रो (Row 1) में सेल A1 से F1 तक को सेलेक्ट करें और हेडिंग दें: Stocks In Bull Run

दूसरी रो (Row 2) में कॉलम के नाम इस प्रकार लिखें:

A2: NSE Code

B2: Volume

C2: Previous Close

D2: CMP

E2: Difference from 200 DMAवॉल्यूम विश्लेषण

F2: CAR

मास्टर फॉर्मूला पेस्ट करना: अब हमें सिर्फ एक फॉर्मूला सेल A3 में डालना है। यह फॉर्मूला 'Top 250 Stocks' वाली शीट से डेटा उठाएगा, उसे फिल्टर करेगा और वॉल्यूम के हिसाब से सही क्रम में लगा देगा।

(नोट: इस फॉर्मूले को दो लाइनों में लिखा गया है ताकि ब्लॉग पर यह बाहर न निकले। आप इस पूरे बॉक्स को एक साथ कॉपी करके सेल A3 में पेस्ट कर दें, गूगल शीट इसे बिल्कुल सही तरह से स्वीकार कर लेगा)

सेल A3 में यह फॉर्मूला पेस्ट करें:

Excel

=IFERROR(SORT(FILTER({'Top 250 Stocks'!A:D, 'Top 250 Stocks'!I:J}, 'Top 250 Stocks'!H:H="In Bull Run", 
'Top 250 Stocks'!J:J="Buy/Average Out"), 2, FALSE), "कोई स्टॉक नहीं मिला")

यह फॉर्मूला अपने आप उन सभी स्टॉक्स की लिस्ट तैयार कर देगा जो बुल रन में हैं और जिनकी CAR रेटिंग "Buy/Average Out" है। आपको बार-बार कुछ भी करने की ज़रूरत नहीं है, जैसे ही मुख्य शीट अपडेट होगी, यह लिस्ट भी अपने आप अपडेट हो जाएगी।


🌟 बोनस अपडेट: वॉल्यूम (Volume) की जगह टर्नओवर (Turnover) से टॉप 250 शेयर कैसे चुनें?

मेरे कुछ फॉलोवर्स ने मांग की है कि वे 'टोटल वॉल्यूम' की जगह 'टोटल टर्नओवर' (कुल ट्रेडेड वैल्यू) के आधार पर टॉप 250 स्टॉक्स सेलेक्ट करना चाहते हैं। यह एक बहुत ही शानदार और प्रोफेशनल सोच है!

वॉल्यूम के बजाय टर्नओवर से शेयर चुनने के फायदे: अक्सर 1-2 रुपये वाले पेनी स्टॉक्स (Penny Stocks) का वॉल्यूम (क्वांटिटी) बहुत ज्यादा होता है, जिससे वे फालतू में हमारी लिस्ट में ऊपर आ जाते हैं। लेकिन 'टर्नओवर' हमें यह बताता है कि आज किस शेयर में सबसे ज़्यादा रुपया (पैसा) लगा है। टर्नओवर के आधार पर फिल्टर करने से कचरा शेयर अपने आप बाहर हो जाएंगे और आपको सिर्फ हाई-क्वालिटी और मजबूत स्टॉक्स की लिस्ट मिलेगी, जहाँ बड़े निवेशक (Smart Money) पैसा लगा रहे हैं।

यदि आप भी अपनी शीट को इस नए और ज़्यादा सटीक तरीके से अपग्रेड करना चाहते हैं, तो कोडिंग में उलझने की बिल्कुल ज़रूरत नहीं है। बस इन आसान स्टेप्स को फॉलो करें:


स्टेप 1: अपने GitHub अकाउंट में जाएँ और अपनी update_sheet.py फाइल खोलें। स्टेप 2: दाईं ओर ऊपर पेंसिल (✏️ Edit) के बटन पर क्लिक करें। स्टेप 3: वहां पहले से लिखे हुए पूरे कोड को सेलेक्ट करके डिलीट (Delete) कर दें (पेज को बिल्कुल खाली कर दें)। स्टेप 4: अब नीचे दिए गए इस नए कोड को कॉपी करें और वहां पेस्ट कर दें:

import gspread

from oauth2client.service_account import ServiceAccountCredentials

import pandas as pd

import requests

import zipfile

import io

from datetime import datetime, timedelta

import os

import json



# 1. Credentials Setup

creds_json = os.environ.get('GCP_CREDENTIALS')

if not creds_json:

    print("CRITICAL: GCP_CREDENTIALS secret missing!")

    exit(1)



creds_dict = json.loads(creds_json)

scope = ["https://spreadsheets.google.com/feeds", "https://www.googleapis.com/auth/drive"]

creds = ServiceAccountCredentials.from_json_keyfile_dict(creds_dict, scope)

client = gspread.authorize(creds)



# ⚠️ अपनी गूगल शीट की ID यहाँ डालें

spreadsheet_id = "यहाँ_अपनी_शीट_की_ID_डालें" 

worksheet = client.open_by_key(spreadsheet_id).worksheet("Top 250 Stocks")



# 2. NSE Data Fetcher (TURNOVER LOGIC)वॉल्यूम विश्लेषण

def fetch_bhavcopy_for_date(date_obj):

    date_str = date_obj.strftime("%Y%m%d")

    url = f"https://nsearchives.nseindia.com/content/cm/BhavCopy_NSE_CM_0_0_0_{date_str}_F_0000.csv.zip"

    

    headers = {

        'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36',

        'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8'

    }

    

    try:

        response = requests.get(url, headers=headers, timeout=20)

        

        if response.status_code == 200:

            with zipfile.ZipFile(io.BytesIO(response.content)) as z:

                csv_filename = z.namelist()[0]

                with z.open(csv_filename) as f:

                    df = pd.read_csv(f)

                    

                    df.columns = [c.strip() for c in df.columns]

                    

                    sym_col = next((c for c in ['TckrSymb', 'SYMBOL'] if c in df.columns), None)

                    close_col = next((c for c in ['ClsPric', 'CLOSE'] if c in df.columns), None)

                    series_col = next((c for c in ['SctySrs', 'SERIES'] if c in df.columns), None)

                    

                    # टर्नओवर के लिए कॉलम (TtlTrfVal)

                    turnover_col = next((c for c in ['TtlTrfVal', 'TtlTrdVal', 'TURNOVER_LACS', 'TURNOVER'] if c in df.columns), None)

                    

                    if not all([sym_col, close_col, turnover_col]):

                        return None

                    

                    if series_col:

                        df = df[df[series_col].astype(str).str.strip() == 'EQ']

                    

                    filter_keywords = 'BEES|ETF|GOLD|LIQUID'

                    df = df[~df[sym_col].astype(str).str.contains(filter_keywords, case=False, na=False)]

                    

                    df[turnover_col] = pd.to_numeric(df[turnover_col], errors='coerce')

                    df = df.dropna(subset=[turnover_col])

                    

                    # टर्नओवर के आधार पर टॉप 250

                    df_top = df.sort_values(by=turnover_col, ascending=False).head(250)

                    return df_top[[sym_col, turnover_col, close_col]].values.tolist()

        return None

    except Exception as e:

        return None



# 3. Execution Logic (7-Day Lookback)

date = datetime.now()

data_to_insert = None

fetched_date_str = ""



for i in range(7): 

    test_date = date - timedelta(days=i)

    if test_date.weekday() >= 5: continue

        

    data_to_insert = fetch_bhavcopy_for_date(test_date)

    

    if data_to_insert:

        fetched_date_str = test_date.strftime('%d-%b-%Y')

        break



# 4. Update Sheet

if data_to_insert:

    try:

        worksheet.batch_clear(['A2:C251'])

        worksheet.update('A2', data_to_insert)

        ist_now = (datetime.utcnow() + timedelta(hours=5, minutes=30)).strftime('%d-%b %H:%M')

        status_msg = f"Data Date: {fetched_date_str} | Last Update: {ist_now} (IST)"

        worksheet.update('K2', [[status_msg]])

        print(f"SUCCESS: Turnover Sheet Updated successfully for {fetched_date_str}!")

    except Exception as e:

        print(f"Google Sheet Error: {str(e)}")वेब Apps और ऑनलाइन टूल

else:


    print("FAILED: पिछले 7 दिनों में से किसी भी दिन की फाइल नहीं मिली।")



स्टेप 5: कोड पेस्ट करने के बाद spreadsheet_id = "यहाँ_अपनी_शीट_की_ID_डालें" वाली लाइन में अपनी गूगल शीट की ID वापस डाल लें और ऊपर से Commit changes कर दें। स्टेप 6 (अंतिम काम): अपनी गूगल शीट खोलें और दोनों शीट्स (Top 250 Stocks और Final List) में कॉलम B की हेडिंग को "Volume" से बदलकर "Turnover" कर दें, ताकि आगे कोई कंफ्यूजन न रहे!

बस हो गया! अब आपका स्कैनर और भी ज़्यादा पावरफुल और सटीक बन चुका है।

जिन लोगों के कोई भी समस्या आ रही है वे गूगल जेमिनी खोलें। उसमे यह कमांड लिखें

हैलो, मैं महेश कौशिक सर के निम्न ब्लॉग पोस्ट को पढ़कर उससे सीखकर पायथन कोड से ऑटोमेटेड स्टोक्स सेलेक्श शीट बना रहा हूंः- https://www.maheshkaushik.com/2026/05/blog-post.html

परन्तु इस शीट में मेरे कुछ ऐरर आ रही है क्या आप इस ब्लॉग पोस्ट के अनुसार शीट बनानें मे मेरी मदद कर सकते हैं।


उसके बाद जेमिनी को अपने एरर कोड अपनी समस्या का स्क्रीनशॉट आदि देते जायें व जेमिनी के बताये अनुसार सही करते जायें। आपकी प्रोब्लम सॉल्व हो जायेगी।

इसे समझने के लिये कक्षा 2 का वीडियो इस लिकं से देखेंः-

https://youtu.be/tyeG16q9214?si=cGkG-QJjooFLcsdu




जिन भी फॉलोवर्स के समस्याएं आ रही थी व जो शीट नहीं बना पा रहे थे उनका सर्वे एक गूगल फार्म में करवाया गया तथा गूगल फार्म में प्राप्त सर्वे के अनुसार फॉलोवर्स की समस्या को कैटेगराईज करके सबका समाधान इस गूगल डॉक्यूमेंट में दे दिया गया है यदि आपके भी कोई समस्या आ रही है तो इस गूगल डाक्यूमेंट को पढकर उसका समाधान तलाश कर सकते हैंः-


https://docs.google.com/document/d/1qr9nZUbBiU4yYVkSy6cck_D4Guk0wjf94PJL72m6QIc/edit?usp=drive_link

जो फॉलोवर एक ही शीट में दो अलग अलग टैब बनाकर एक में वोल्यूम के आधार पर टॉप 250 स्टॉक्स डाउनलोड करना चाहते हैं व दूसरी टैब में टर्नओवर के आधार पर टॉप 250 स्टॉक्स डाउनलोड करना चाहते हैं उनके लिये मैंने आपके लिए एक "टू-इन-वन मास्टर कोड" (2-in-1 Master Code) लिखा है। यह दो अलग-अलग रोबोट नहीं हैं, बल्कि एक ही बेहद एडवांस स्क्रिप्ट है।


यह नया कोड क्या करता है? यह NSE के सर्वर पर जाकर भावकॉपी (Bhavcopy) की फाइल सिर्फ 1 ही बार डाउनलोड करता है। फिर यह अपने दिमाग (Memory) में उसी 1 फाइल से 'वॉल्यूम' वाले टॉप 250 शेयर निकालता है और पहली शीट में डाल देता है। फिर सेकंड भर के अंदर, उसी फाइल से 'टर्नओवर' वाले टॉप 250 शेयर निकालता है और दूसरी शीट में डाल देता है।वॉल्यूम विश्लेषण

स्टेप 1: आपकी गूगल शीट में एक नया टैब (Tab) बनाना
चूँकि आप ,एक ही शीट का इस्तेमाल कर रहे हैं। , तो सबसे पहले आपको इसमें टर्नओवर का डेटा मंगाने के लिए एक खाली जगह बनानी होगी:

अपनी उसी गूगल शीट को खोलें। जो बनायी हुयी है। यदि टर्नओवर वाली बनायी हुयी है तो उसी को खोलें।

नीचे '+' बटन दबाकर एक नया टैब (Sheet) बनाएँ और उसका नाम बिल्कुल यह रखें: Top 250 Turnover (स्पेलिंग और स्पेस बिल्कुल यही रखें क्योंकि कोड इसी नाम को ढूँढेगा)।

अब अपनी पुरानी शीट Top 250 Stocks की पहली लाइन (Row 1 की हेडिंग्स A1 से लेकर K1 तक) कॉपी करें और इस नई Top 250 Turnover शीट की Row 1 में पेस्ट कर दें।

(वैकल्पिक) इस नई शीट के B1 में 'Volume' की जगह 'Turnover (Lacs)' लिख लें ताकि कंफ्यूजन न रहे।

स्टेप 2: आपका नया 'टू-इन-वन' (2-in-1) पायथन कोड
अब आपको GitHub पर अपनी update_sheet.py फ़ाइल को खोलकर उसमें पहले से मौजूद पूरे कोड को डिलीट (Delete) करना है और यह नया मास्टर कोड पेस्ट करना है। इसमें जहां लिखा है यहां पर अपनी शीट की आईडी डाले वहां आपकी शीट की आईडी डालना मत भुलें आईडी कैसे निकालते हैं यह वीडियो में बताया है नीचे वीडियो का लिंक भी आ जायेगा। 50 प्रतिशत फॉलोवर जिनके ऐरर आती है वो या तो शीट की आईडी डालना ही भूल जाते है या आईडी डालने में कोई गलती करते हैं या आइडी के पास जो इन्वर्टेड कोमा है उनको डीलीट कर देते हैं। इसलिये आईडी वाली गलती का ध्यान कर लेना।

import gspread
from oauth2client.service_account import ServiceAccountCredentials
import pandas as pd
import requests
import zipfile
import io
from datetime import datetime, timedelta
import os
import json

# 1. Credentials Setup
creds_json = os.environ.get('GCP_CREDENTIALS')
if not creds_json:
    print("ERROR: GCP_CREDENTIALS secret missing!")
    exit(1)

creds_dict = json.loads(creds_json)
scope = ["https://spreadsheets.google.com/feeds", "https://www.googleapis.com/auth/drive"]
creds = ServiceAccountCredentials.from_json_keyfile_dict(creds_dict, scope)
client = gspread.authorize(creds)

# आपकी शीट की ID 
spreadsheet_id = "यहां पर अपनी शीट की आईडी डाले "

# दोनों शीट्स को कनेक्ट करना
try:
    ws_volume = client.open_by_key(spreadsheet_id).worksheet("Top 250 Stocks")
    ws_turnover = client.open_by_key(spreadsheet_id).worksheet("Top 250 Turnover")
except Exception as e:
    print(f"Sheet Connection Error: {e}")
    exit(1)

# 2. New NSE UDiFF Data Fetcher
def fetch_bhavcopy_for_date(date_obj):
    date_str = date_obj.strftime("%Y%m%d")
    url = f"https://nsearchives.nseindia.com/content/cm/BhavCopy_NSE_CM_0_0_0_{date_str}_F_0000.csv.zip"
    headers = {
        'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36'
    }
    print(f"--- तारीख {date_obj.strftime('%d-%m-%Y')} चेक कर रहे हैं ---")
    
    try:
        response = requests.get(url, headers=headers, timeout=15)
        if response.status_code == 200:
            print("फाइल मिल गई! अब इसे खोल रहे हैं...")
            with zipfile.ZipFile(io.BytesIO(response.content)) as z:
                csv_filename = z.namelist()[0]
                with z.open(csv_filename) as f:
                    df = pd.read_csv(f)
                    
                    # नए कॉलम के नाम खोजना
                    sym_col = 'TckrSymb' if 'TckrSymb' in df.columns else 'SYMBOL'
                    close_col = 'ClsPric' if 'ClsPric' in df.columns else 'CLOSE'
                    series_col = 'SctySrs' if 'SctySrs' in df.columns else 'SERIES'
                    
                    # 1. वॉल्यूम कॉलम ढूँढना
                    vol_col = 'TtlTradgVol'
                    for c in ['TtlTradgVol', 'TOTTRDQTY', 'TtlTrdQty', 'TotTrdQty']:
                        if c in df.columns:
                            vol_col = c
                            break
                            
                    # 2. टर्नओवर कॉलम ढूँढना (TtlTrfVal)
                    turnover_col = 'TtlTrfVal'
                    for c in ['TtlTrfVal', 'TOTTRDVAL', 'TtlTrdVal', 'TotTrdVal']:
                        if c in df.columns:
                            turnover_col = c
                            break
                    
                    # सिर्फ EQ सीरीज छांटना
                    if series_col in df.columns:
                        df = df[df[series_col].astype(str).str.strip() == 'EQ']
                    
                    # ETF, GOLD, LIQUID हटाना
                    filter_keywords = 'BEES|ETF|GOLD|LIQUID|CASE|SILVER|LIQ'
                    df = df[~df[sym_col].astype(str).str.contains(filter_keywords, case=False, na=False)]
                    
                    # --- डेटा को दो भागों में बाँटना ---
                    
                    # लिस्ट A: वॉल्यूम के आधार पर टॉप 250
                    df_vol = df.sort_values(by=vol_col, ascending=False).head(250)
                    data_vol = df_vol[[sym_col, vol_col, close_col]].values.tolist()
                    
                    # लिस्ट B: टर्नओवर के आधार पर टॉप 250
                    df_turnover = df.sort_values(by=turnover_col, ascending=False).head(250)
                    data_turnover = df_turnover[[sym_col, turnover_col, close_col]].values.tolist()
                    
                    return data_vol, data_turnover
        else:
            print(f"NSE सर्वर ने {response.status_code} रिस्पॉन्स दिया।")वॉल्यूम विश्लेषण
            return None, None
    except Exception as e:
        print(f"Error: {e}")
        return None, None

# 3. Execution Logic (7 दिन पीछे तक चेक करना)
date = datetime.now()
data_vol_to_insert = None
data_turnover_to_insert = None
fetched_date_str = ""

for i in range(7):
    test_date = date - timedelta(days=i)
    if test_date.weekday() >= 5: # Skip Sat/Sun
        continue
        
    data_vol, data_turnover = fetch_bhavcopy_for_date(test_date)
    if data_vol and data_turnover:
        data_vol_to_insert = data_vol
        data_turnover_to_insert = data_turnover
        fetched_date_str = test_date.strftime('%d-%b-%Y')
        break

# 4. Update Both Sheets
if data_vol_to_insert and data_turnover_to_insert:
    try:
        # A. वॉल्यूम वाली पुरानी शीट अपडेट करें
        ws_volume.batch_clear(['A2:C251'])
        ws_volume.update('A2', data_vol_to_insert)
        
        # B. टर्नओवर वाली नई शीट अपडेट करें
        ws_turnover.batch_clear(['A2:C251'])
        ws_turnover.update('A2', data_turnover_to_insert)
        
        # टाइमस्टैम्प अपडेट करें
        ist_now = (datetime.utcnow() + timedelta(hours=5, minutes=30)).strftime('%d-%b %H:%M')
        status_msg = f"Data Date: {fetched_date_str} | Last Update: {ist_now} (IST)"
        
        ws_volume.update('K2', [[status_msg]])
        ws_turnover.update('K2', [[status_msg]])
        
        print(f"SUCCESS: दोनों शीट्स (Volume और Turnover) {fetched_date_str} के डेटा से अपडेट हो गई हैं!")
    except Exception as e:
        print(f"Google Sheet अपडेट करने में एरर: {e}")वेब Apps और ऑनलाइन टूल
        exit(1)
else:
    print("FAILED: पिछले 7 दिनों में से किसी भी दिन की फाइल नहीं मिली।")
    exit(1)

अब आपको क्या करना है?

इस कोड को GitHub में पेस्ट करके Commit changes कर दें।

Actions टैब में जाकर एक बार 'Run Workflow' पर क्लिक कर दें।

आप देखेंगे कि आपकी दोनों शीट्स एक साथ सुपरफास्ट स्पीड से भर गई हैं। Top 250 Stocks में पहले की तरह वॉल्यूम वाले शेयर आएंगे और नई Top 250 Turnover शीट में पैसे (Value) वाले बेहतरीन शेयर आ जाएंगे।

यदि किसी के कोई ऐरर आती है तो पूरे कोर्स में फॉलोवर्स द्वारा बतायी गयी कॉमन ऐरर का समाधान इस गूगल डाक्यूमेंट में दे दिया है इसे पढ़कर समाधान कर लेवें।

https://docs.google.com/document/d/1qr9nZUbBiU4yYVkSy6cck_D4Guk0wjf94PJL72m6QIc/edit?usp=sharing

आगे आपको क्या करना है? (सिर्फ 2 मिनट का काम)
स्टेप 1: अपनी शीट में नीचे '+' दबाकर एक नया टैब बनाएँ और उसका नाम Final List Turnover रख दें। Final List वाली शीट का नाम बदलकर Final List Volume कर देवें। 

स्टेप 2: अपनी पुरानी 'Final List Volume' की ऊपर वाली हेडिंग्स (Row 1 के A1 से F1 तक) को कॉपी करें और इस नई शीट की पहली लाइन में पेस्ट कर दें। व  उपर के हेडिंग में वोल्यूम के स्थान पर टर्नओवर कर देवें।

स्टेप 3: अब इस नई शीट के सेल A2 में यह नया फॉर्मूला पेस्ट कर दें:


=IFERROR(SORT(FILTER({'Top 250 Turnover'!A:D, 'Top 250 Turnover'!I:J}, 'Top 250 Turnover'!H:H="In Bull Run", 
'Top 250 Turnover'!J:J="Buy/Average Out"), 2, FALSE), "कोई स्टॉक नहीं मिला")

अब जो मेरे प्यारे फोलोवर्स ऐसे हैं जो या तो सम्मानीय सीनीयर सीटीजन हैं या ज्यादा टेक्नोलाजी फ्रैंडली नहीं है या ऐसे हैं जो सब कुछ जानते हुये भी इतनी मेहनत शीट बनानें की नहीं करना चाहते जिनको मैं प्यार से महाआलसी व महानालायक फोलोवर्स भी कहता हूं। 
उनके रेफरेंस के लिये वोल्यूम व टर्नओवर वाली शीटों में जो फाईनल स्टॉक रोज स्कैन होकर आते हैं उनको वो इन बटनों को दबाकर देख सकते हैं। 
जब भी शीट ऑटो अपडेट होगी बटनों के नीचे के स्टॉक भी अपडेट हो जायेगें ये बटन मेरी शीट से जुड़े हुये हैं। परन्तु याद रखें यह मेरी स्टॉक रेकमाडेंशन नहीं है यह सिर्फ रेफरेंस व ऐजुकेशनल पपर्ज के लिये हैं इसके आधार पर कोई स्टॉक ना खरीदें पहले इनके टेक्नीकल व फंडामेंटल जांचे फिर अपने विवेक को लगाऐ फिर अपने पंजीकृत निवेश सलाहकार को पुछे फिर ही कोई काम करें इन सब शेयरों में मेरा व मेरे रिश्तेदारों का निवेश हो सकता है। इसलिये सावधान रहें सतर्क रहे।
