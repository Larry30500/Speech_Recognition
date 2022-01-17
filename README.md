<h1 align="center">
  <br>
  [指定專題作品] 語音辨識應用
</h1>


## 目錄
* [摘要](#摘要)
* [重點程式碼說明](#重點說明)
* [系統環境](#系統環境)
* [聯絡資料](#聯絡資料)
* [致謝](#致謝)
* [權限](#權限)


## 摘要
### 本作品主要功能為：使用語音辨識相關技術，設計具有 (1)網站導覽 (2)四則運算 服務的小程式。

![speech_recognition01](images/speech_recognition01.gif)

<strong><em>若您有興趣想更了解此程式，請參考下方的聯絡方式，進一步聯絡作者，謝謝參閱。</em></strong>


## 重點程式碼說明
### 1. 於麥克風獲取音訊，並使用 Google 語音辨識模組
  ```python
  r = sr.Recognizer()
  with sr.Microphone() as source:
    audio = r.listen(source)

  try: your_command = r.recognize_google(audio)
  except sr.UnknownValueError: print("Google Speech Recognition 無法辨識您說的話")
  ⋮
  ```
  
### 2. 當 Google Speech Recognition 辨識出對話內容後，執行以下的命令
* 如果字串為 退出 或 exit，則退出程式
  ```python
  if any(x in your_command for x in ['網站導覽', '第一']):
    ⋮
    open_website_service()
    
  elif any(x in your_command for x in ['四則運算', '第二']):
    ⋮
    arithmetic_service()
  
  elif any(x in your_command for x in ['exit', '退出']):
    ⋮
    break
  
  else: print(目前此程式僅提供：「網站導覽服務」和 「四則運算服務」，謝謝使用。)
  ```
  
  ![speech_recognition02](images/speech_recognition02.gif)

### 3. 網站導覽服務
* 根據語音辨識後的中文字串，導覽對應的網站
* 如果中文字串為 退出 或 exit，則退出程式
* 如果中文字串內容，不在設定範圍內，則顯示：輸入錯誤，目前提供的服務，謝謝使用
  ```python
  def open_website_service():
    ⋮
    while True:
      ⋮
      google_recognizer()
      
      if any(x in your_command for x in ['google', '谷歌']):
        ⋮
        webbrowser.open(Google 網頁)
      ⋮
      else: print(目前網站導覽服務僅提供：開啟 Google、Microsoft、Python、Wiki 官方網頁的服務，謝謝使用。)  
  ```

  ![url01](images/url01.gif)
  
### 4. 數學運算服務
* 先將中文字串翻譯成具有數字和特定運算符號的運算式
  ```python
  def translate_into_expressions():
    ⋮
    for i in range(len(replacement_words_list)):
      ⋮
      # 次方項使用正規表示式處理
      your_command = re.sub(r'的(\d+)次方', r'**\1', your_command)
      ⋮
  ```
    
* 檢查翻譯後的運算式，是否使用未允許的字元，是則進行數學運算；否則顯示輸入錯誤
* 如果字串為 退出 或 exit，則退出程式
  ```python
  def arithmetic_service():
    ⋮
    for index in range(len(your_command)):
      # 判別：如果使用未允許的字元，則 legal_expression = False
      if your_command[index] not in available_characters:
        legal_expression = False
        break
    
    if legal_expression:
      ⋮
      print(四則運算式與計算結果為：\n 計算結束，謝謝使用。)
      
    elif any(x in your_command for x in ['exit', '退出']):
      print(即將退出四則運算服務，並返回語音辨識主選單，謝謝使用。)
      break
           
    else: print(輸入錯誤，目前僅提供：加、減、乘、除、次方、左小括號、右小括號 的四則運算服務，謝謝使用)
  ```
  
  ![arithmetic01](images/arithmetic01.gif)

### 5. 插入多執行續，語音辨識等待過程，出現 ... 字元 ，讓使用者了解程式還在辨識中。
  ```python
  multi_threading = threading.Thread(target = display_while_waiting).start()
  ```
  
* 建立子執行緒所對應的函式，將於語音辨識等待過程中執行
  ```python   
  def display_a_dot():
    ⋮
    time.sleep(1)
    print('.', end = '')
    ⋮  
  def display_while_waiting():
    print(語音辨識中，請稍等)
    ⋮ 
    display_a_dot()  
  ```
  
  


## 系統環境
### 作業系統
* OS：Windows 7 / 10 (Mac OS、Linux 系統亦可相容)

### 相關套件
* Python 核心：3.10
* Speech Recognition：3.8
* PyAudio：0.2 (win_amd64)


## 聯絡資料
👤 **Larry Jhuang**
  * Email: larry30500@gmail.com


## 致謝
*非常感謝指導老師 (Francesco Ke) 提供程式設計的靈感和方向，並細心指導學生編寫程式時，所需注重的細節。*

*如果您喜歡此專案，記得點擊⭐️支持作者。*


## 權限
目前設定為 MIT 權限。請參閱 `LICENSE`，了解更多相關 MIT 權限的規定。

<br><br>[返回目錄](#目錄)
