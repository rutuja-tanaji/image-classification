#Importing Libraries

from keras.models import Sequential
from keras.layers import Dense
from keras.layers import Conv2D
from keras.layers import MaxPooling2D
from keras.layers import Flatten

# Initialize the Model

cnn_model=Sequential()
cnn_model.add(Conv2D(32,3,3,input_shape=(64,64,3),activation='relu'))
cnn_model.add(MaxPooling2D(pool_size=(2,2)))
cnn_model.add(Flatten())
cnn_model.add(Dense(init="random_uniform",activation="relu",output_dim=120))
cnn_model.add(Dense(init="random_uniform",activation="softmax",output_dim=16))
cnn_model.compile(optimizer="adam",loss="categorical_crossentropy",metrics=['accuracy'])
from keras.preprocessing.image import ImageDataGenerator
train_datagen = ImageDataGenerator(
        rescale=1./255,
        shear_range=0.2,
        zoom_range=0.2,
        horizontal_flip=True)
test_datagen=ImageDataGenerator( rescale=1./255)
x_train = train_datagen.flow_from_directory(
        'C:/train',
        target_size=(64, 64),
        batch_size=32,
        class_mode='categorical')
x_test = test_datagen.flow_from_directory(
        'C:/test',
        target_size=(64, 64),
        batch_size=32,
        class_mode='categorical')
x_train.class_indices
cnn_model.fit_generator(
        x_train,
        steps_per_epoch=250,  #no of testing images/bAtch size
        epochs=5,
        validation_data=x_test,
        validation_steps=62)  #no of testing images/bAtch size
cnn_model.save("Weed1.h5")
import numpy as np
from keras.preprocessing import image

from tkinter import *
from PIL import ImageTk, Image
from tkinter import filedialog
import os
from keras.models import load_model
classifier = load_model('Weed1.h5')
classifier.compile(optimizer = 'adam', loss = 'categorical_crossentropy', metrics = ['accuracy'])
root = Tk()
root.geometry("550x300+300+150")
root.resizable(width=True, height=True)

def openfn():
    filename = filedialog.askopenfilename(title='open')
    return filename
def open_img():
    x = openfn()
    test_image = image.load_img(x, target_size = (64, 64))
    test_image = image.img_to_array(test_image)
    test_image = np.expand_dims(test_image, axis = 0)
    result = classifier.predict_classes(test_image)
    #print(result)
    
    if(result==0):
        y=0
        print("Class:",result)
        print("The weed type is Black grass")
    elif(result==1):
        y=1
        print("Class:",result)
        print("The weed type is charlock")
    elif(result==2):
        y=2
        print("Class:",result)
        print("The weed type is cleavers")
    elif(result==3):
        y=3
        print("Class:",result)
        print("The weed type is common chickweed")
    elif(result==4):
        y=4
        print("Class:",result)
        print("The weed type is common wheat")
    elif(result==5):
        y=5
        print("Class:",result)
        print("The weed type is Fat Hen")
    elif(result==6):
        y=6
        print("Class:",result)
        print("The weed type is loose silky bent")
    elif(result==7):
        y=7
        print("Class:",result)
        print("The weed type is Maize")
    elif(result==8):
        y=8
        print("Class:",result)
        print("The weed type is scentless mayweed")
    elif(result==9):
        y=9
        print("Class:",result)
        print("The weed type is shepherds purse")
    elif(result==10):
        y=10
        print("Class:",result)
        print("The weed type is small-flowered cranesbill")
    elif(result==11):
        y=11
        print("Class:",result)
        print("The weed type is sugar beet")
    elif(result==12):
        y=12
        print("Class:",result)
        print("The weed type is broadleaf")
    elif(result==13):
        y=13
        print("Class:",result)
        print("The weed type is grass")
    elif(result==14):
        y=14
        print("Class:",result)
        print("The weed type is soil")
    elif(result==15):
        y=15
        print("Class:",result)
        print("The weed type is soybean")
    
    else:
        y=16
        print("Unknown Weed type")
    
    index=['Black-grass','Charlock','Cleavers','CommonChickweed','Commonwheat','FatHen','LooseSilky-bent','Maize','ScentlessMayweed','ShepherdsPurse','Small-floweredCranesbill','Sugarbeet','broadleaf','grass','soil','soyabean']
    label = Label( root, text="Prediction : "+index[y])
    label.pack()
    img = Image.open(x)
    img = img.resize((250, 250), Image.ANTIALIAS)
    img = ImageTk.PhotoImage(img)
    panel = Label(root, image=img)
    panel.image = img
    panel.pack()

btn = Button(root, text='open image', command=open_img).pack()

root.mainloop()
