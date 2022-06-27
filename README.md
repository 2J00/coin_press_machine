# coin_press_machine

This code uses Linear Regression to predict the visibility of the seal. 
It also outputs an image with the closest visibility to the predicted visibility.

---
This is a function for creating an image path.
In 'path' variable, add the path that contains the image file.
```
def make_path(file_name):
    path = ''
    join = '{}/{}.png'.format(path,file_name)
    return join
```

This function outputs images with the most similar predicted visibility and predicted visibility when the limit load, adjustment speed, deceleration distance, and actual load are input.
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
