# CNN ve Resnet152V2 modelleriyle kedi ve köpek sınıflandırması

 ## Klasörlere ayırma  
- Hiyerarşik class yapısını rastgele bir şekilde oluşturmak için *'shutil'* kullanıyoruz. Bu işlem sonunda elimizdeki klasör yapısı:
    - Train
        - 0 (cats)
        - 1 (dogs)
    - Val
        - 0 (cats)
        - 1 (dogs)
    - Test1

## Flow from directory
  Öncelikle ImageDataGenerator ile veri arttırma işlemlerinin kıstaslarını belirlemek için aşağıdaki bloğu kullanıyoruz.<br>
  - train_datagen=ImageDataGenerator(<br>
          rotation_range=5,<br>
          ......<br>
          ......<br>
          .....<br>
      )<br>
  Ardından flow_from_directory ile train, validation ve test için verilerimizi hazır hale getiriyoruz.<br>
  - train_generator=train_datagen.flow_from_directory(<br>
          train_data_dir,<br>
          ........<br>
          .......<br>
          ......<br>
    )
## CNN Modelini inşa etme
<div><br>
input_shape=(img_rows, img_cols, 3)<br><br>

model = Sequential()<br>
model.add(Conv2D(32, (3, 3), activation='relu', input_shape = input_shape))<br>
model.add(BatchNormalization())<br>
model.add(MaxPooling2D(pool_size=(2, 2)))<br>
model.add(Dropout(0.3))<br><br>

model.add(Conv2D(64, (3, 3), activation='relu'))<br>
model.add(BatchNormalization())<br>
model.add(MaxPooling2D(pool_size=(2, 2)))<br>
model.add(Dropout(0.3))<br><br>

model.add(Conv2D(128, (3, 3), activation='relu'))<br>
model.add(BatchNormalization())<br>
model.add(MaxPooling2D(pool_size=(2, 2)))<br>
model.add(Dropout(0.3))<br><br>


model.add(Flatten())<br>
model.add(Dense(512, activation='relu'))<br>
model.add(BatchNormalization())<br>
model.add(Dropout(0.3))<br>
model.add(Dense(2, activation='softmax'))<br>
</div>
- Compile ettiğimiz modeli fit etme aşamasında epoch değerini hızlı çalışması açısından 5 veriyorum.
- Modeli evaluate ettiğimizde test başarısının epoch değerine ve  katmanlara göre nispeten iyi olduğunu görüyoruz.

## Resnet152V2 model ile tranfer learning
- imagenet ağırlıklarını yüklüyoruz ve Resnet152V2 katmanlarını donduruyoruz. Ardından kendi katmanlarımızı da bu modele ekleyerek eğitime hazır hale getiriyoruz.
- model.fit() için gereken train,validation ve test kendi inşa ettiğimiz CNN modeli için hazırladığımız veriler olacak bu kısımda tekrar train,validation ve test verilerini hazırlamamıza gerek yok.
- Compile ettiğimiz modeli fit etme aşamasında epoch değerini model ağır olduğu için hızlı çalışması açısından 5 veriyorum.
- Modeli evaluate ettiğimizde test  başarısının gayet  iyi olduğunu görüyoruz.
- Aşağıda ilk grafikte CNN modelimizin ikinci grafikte Resnet152V2'nin grafiklerini görmekteyiz.
- ![rn1](https://user-images.githubusercontent.com/52465630/209405114-694326e6-a939-4d96-bd3e-a2914ea6e98c.png)

- ![rn2](https://user-images.githubusercontent.com/52465630/209404509-4537a8fa-42c2-4dac-aef7-326110dae49a.png)

 ## Result
 - Kendi oluşturduğum CNN modelinin test başarısı : %72.27
 - Transfer learning  kullanarak eğittiğim Resnet152V2 modelinin test başarısı : %99.12


- Kullanılan dataset için https://www.kaggle.com/competitions/dogs-vs-cats/data
