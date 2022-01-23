<h1 align="center">
  <br>
  [指定專題作品 (Python)] 使用語音辨識功能進行網站導覽和數學運算
</h1>


## 目錄
* [摘要](#摘要)
* [重點程式碼說明](#重點程式碼說明)
* [系統環境](#系統環境)
* [聯絡資訊](#聯絡資訊)
* [致謝](#致謝)
* [權限](#權限)

&nbsp;

## 摘要
### 1. 本作品內含語音辨識相關技術，可使用語音的方式進行 (1)網站導覽 (2)數學運算。
### 2. 語音辨識的處理過程中，加入子執行緒，每秒顯示 1 個 "." 字元，提示使用者語音尚在辨識處理中。

![speech_recognition01](images/speech_recognition01.gif)

<strong><em>假使想要更加了解此程式的話，請參考本頁面底部之作者的聯絡方式。</em></strong>

&nbsp;

## 重點程式碼說明
### 1. 使用 Google 語音辨識模組，並於麥克風獲取音訊。
```python
def google_recognizer():
  while True: 
    r = sr.Recognizer()
    
    # 建立並啟動新的執行緒
    multi_threading = threading.Thread(target = display_while_waiting).start()
    
    with sr.Microphone() as source:
      audio = r.listen(source)

    try: your_command = r.recognize_google(audio)
    except sr.UnknownValueError: print('目前無法正確辨識, 請再說一次, 謝謝。')
    ⋮
```

&nbsp;
  
### 2. 當 Google 語音辨識成功辨識出對話內容後，執行以下的命令。
```python
print('歡迎使用「語音辨識服務」，本程式提供 (1)網站導覽服務 (2)四則運算服務，請問您想要使用哪一種服務？\n(如果想要「退出程式」，請說 exit 或 退出。)\n')

if any(x in your_command for x in ['網站導覽', '第一']):
  print('即將為您提供「網站導覽服務」。')
  open_website_service()

⋮

elif any(x in your_command for x in ['exit', '退出']):
  print('即將退出程式，謝謝使用。')
  break

else: print('目前此程式僅提供：「網站導覽服務」和 「四則運算服務」，謝謝使用。')
```
  
![speech_recognition02](images/speech_recognition02.gif)

&nbsp;

### 3. 語音進行網站導覽
```python
# 根據語音辨識後的中文字串，導覽對應的網站。
# 如果中文字串為 '退出' 或 'exit'，則退出程式。
# 如果中文字串內容，不在設定範圍內，則顯示：輸入錯誤，目前提供的服務，謝謝使用。
def open_website_service():    
  while True:
    print('歡迎使用「網站導覽服務」，目前僅提供：開啟 谷歌 (google), 微軟 (microsoft), Python, 維基百科 (wiki) 等 4 個網站的首頁。\n請問您想要前往哪個網站？\n(如果想要「退出本服務」，請說 exit 或 退出。)\n')

    google_recognizer()

    if any(x in your_command for x in ['google', '谷歌']):
      print('即將為您開啟 Google 官方網站。')
      webbrowser.open('Google 網站')

    ⋮

    else: print('目前網站導覽服務僅提供：開啟 Google、Microsoft、Python、Wiki 官方網站的服務，謝謝使用。')  
```

![url01](images/url01.gif)

&nbsp;

### 4. 語音進行數學運算
```python
# 先將中文字串翻譯成具有阿拉伯數字和特定運算符號的運算式。
def translate_into_expressions():
  ⋮

  for i in range(len(replacement_words_list)):
    your_command = your_command.replace(replacement_words_list[i][0], replacement_words_list[i][1])      
    # 使用正規表示式處理次方項
    your_command = re.sub(r'的(\d+)次方', r'**\1', your_command)

  return your_command

def arithmetic_service():
  print('歡迎使用「四則運算服務」，目前僅接受「加、減、乘、除、次方、左側小括號、右側小括號」之運算功能。\n請說出您想要計算的公式！\n(如果想要「退出本服務」，請說 exit 或 退出。)\n')

  google_recognizer()

  # 先將中文字串翻譯成具有阿拉伯數字和特定運算符號的運算式。
  translate_into_expressions()

  print(f'翻譯的結果為：{your_command}\n')

  # 檢查翻譯後的運算式，是否使用未允許的字元，是則進行數學運算；否則顯示輸入錯誤。
  available_characters = '1234567890+-*/xX^()'
  legal_expression = True

  for index in range(len(your_command)):
    # 判別：如果使用未允許的字元，則 legal_expression = False
    if your_command[index] not in available_characters:
      legal_expression = False
      break

  if legal_expression:
    ⋮

    print('四則運算式與計算結果為：\n 計算結束，謝謝使用。')
    
  # 如果字串為 退出 或 exit，則退出程式。
  elif any(x in your_command for x in ['exit', '退出']):
    print('即將退出四則運算服務，並返回語音辨識主選單，謝謝使用。')
    break

  else: print('輸入錯誤，目前僅提供：加、減、乘、除、次方、左小括號、右小括號 的四則運算服務，謝謝使用。')
```
  
![arithmetic01](images/arithmetic01.gif)

&nbsp;

### 5. 語音辨識的處理過程中，加入子執行緒，可每秒顯示 1 個 "." 字元，提示使用者語音尚在辨識處理中。 
```python  
# 建立子執行緒所對應的函式，將於語音辨識等待過程中執行。
def display_a_dot():
  ⋮

  time.sleep(1)
  print('.', end = '')
  ⋮

def display_while_waiting():
  ⋮
  
  print('語音辨識中，請稍等。')
  display_a_dot()  

# 並於 Google Speech Recognition 語音辨識過程中，啟動子執行緒。
```

&nbsp;

## 系統環境
### 本程式所在作業系統
* OS：Windows 7 / 10 (Mac OS、Linux 系統亦可相容)

### 相關套件
* Python 核心：3.10
* Speech Recognition：3.8
* PyAudio：0.2 (win_amd64)

&nbsp;

## 聯絡資訊
👤 **Larry Jhuang**
  * Email: larry30500@gmail.com

&nbsp;

## 致謝
*非常感謝指導老師 (Francesco Ke) 提供程式設計的靈感和方向，並充分教導程式設計的注意事項和相關細節。*

*如果您喜歡此專案，記得點擊⭐️支持作者。*

&nbsp;

## 權限
目前設定為 MIT 權限。請參閱 `LICENSE`，了解更多相關 MIT 權限的規定。

&nbsp;

[[ 返回目錄 ]](#目錄)

