# coin_press_machine

선형회귀를 사용하여 코인에 찍인 문장이 얼마나 선명한지 예측하는 모델입니다.
예측된 선명도와 함께 가장 흡사한 선명도의 코인 이미지가 출력됩니다.

---
image path를 만드는 함수입니다.
'path' 변수에 이미지 파일이 포함된 경로를 추가합니다. 
```
def make_path(file_name):
    path = ''
    join = '{}/{}.png'.format(path,file_name)
    return join
```

제한하중, 조정속도, 감속거리, 실제하중을 입력하면 예측 선명도와 함께 예측한 선명도와 가장 유사한 선명도를 가진 코인의 이미지를 출력합니다.
```
def simul_coin_press():
    model1 = joblib.load('press_coin2.pkl')
    
    df = pd.read_excel("coindata.xlsx")
    visi = df['visibility'].values
    
    print('제한하중을 입력하세요:')
    limit_w = float(input())
    print('조정속도를 입력하세요:')
    adj_speed = float(input())
    print('감속거리를 입력하세요:')
    sd_length = float(input())
    print('실제하중을 입력하세요:')
    real_w = float(input())
    
    test_data = pd.DataFrame([[limit_w],[adj_speed],[sd_length],[real_w]]).T
    
    visibility = model1.predict(test_data)
    
    file_name = str(find_nearest(visi,visibility))
    
    path = make_path(file_name)

    print('예상 선명도:',visibility)
    img = Image.open(path)
    image = np.array(img)
    
    plt.imshow(image)
    plt.show()
    ```
