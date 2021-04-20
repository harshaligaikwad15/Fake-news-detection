# Fake-news-detection
from tkinter import messagebox
from tkinter import Button
from tkinter import *
import tkinter.font as tkFont
from tkinter import filedialog
import cv2
import time
import numpy as np
t_screen_time = 1000
diff=0


path=''
window =Tk(className="MGM COE")
window.geometry("600x400")
window['bg']='#0DE5CE'
p2=StringVar()
def select():
    path=filedialog.askopenfilename(filetypes=[("Video File",'.mp4')])
    E1.delete(0,END)
    E1.insert(0,path)



 # choose codec according to format needed
fourcc = cv2.VideoWriter_fourcc(*'mp4v') 
video=cv2.VideoWriter('video.avi', fourcc, 1,(256,320))


# Function to extract frames 
def framecaps(path,path2):
    # Path to video file
    print(path==path2)
    diff=0
    
    vidObj = cv2.VideoCapture(path)
    vidObj2 = cv2.VideoCapture(path2)
    
    # Used as counter variable 
    count = 0
    success = 1
    success, image = vidObj.read()
    success, image2 = vidObj2.read()
    
    while success:
        if(path==path2):
            #print('yes')
            
            image = cv2.resize(image, (1280, 720))
            image2 = cv2.resize(image2, (1280, 720))
            difference = cv2.subtract(image, image)
        else:
           
            image = cv2.resize(image, (1280, 720))
            image2 = cv2.resize(image2, (1280, 720))
            difference = cv2.subtract(image, image2)
            
            
        
        
        
        ret,bw_img = cv2.threshold(difference,127,255,cv2.THRESH_BINARY)
        n_white_pix = np.sum(bw_img == 255)
        diff=diff+n_white_pix
        #print(diff)
        
        #print("hi")
        #cv2.imwrite("frame%d.jpg" % count, image)
        
        count += 1
        success, image = vidObj.read()
    print(diff)
    if(diff>10000):
        print('Fake News')
        L2=Label(window,text="Fake News",bg='#0FE7F5',font=fontStyle)
        L2.place(x=250,y=230)

    else:
        print('Original News')
        L2=Label(window,text="Original News",bg='#0FE7F5',font=fontStyle)
        L2.place(x=250,y=230)

        
    return diff



def preprocess(count):
    for i in range(0,count):
        #print("frame%d.jpg" %i)
        im=cv2.imread("frame%d.jpg" %i)
        im2=cv2.cvtColor(im, cv2.COLOR_BGR2GRAY )
        gray_img_eqhist=cv2.equalizeHist(im2)
        cv2.imshow("test",im)
        cv2.imshow("test3",im2)
        cv2.imshow("test2",gray_img_eqhist)
        
        cv2.waitKey(t_screen_time)  
        #cv2.destroyAllWindows()
        video.write(im)
    cv2.destroyAllWindows()
    video.release()
        

def start2():
    
    
    p1="C:/Users/arti/Desktop/Demo3/dataset/original.mp4"
    p3=p2.get()
    #print(p3)
    #print(p1)
    #print(p1==p3)
    c=framecaps(p1,p3)



photo = PhotoImage(file = "tk2.png")
####photo2 = PhotoImage(file = "aaa.png")
####
####B=Button(window,text=" SELECT VEDIO ", image = photo2)
####B.place(x=120,y=6)
canvas = Canvas(window,bg='#0DE5CE', width=600, height=400)
canvas.pack()
fontStyle = tkFont.Font(family="Adelaide", size=28)
L1=Label(window,text="Fake News Verfication",bg='#0DE5CE',font=fontStyle)
L1.place(x=15,y=5)

B=Button(window,text=" Select Video ",bg='#4AF7E7',command = select)
B.place(x=140,y=86)





E1=Entry(window,bd=0,font = ('calibre',18,'normal'),background= "#14B6A5",textvariable=p2)
E1.place(x=230,y=86)
B=Button(window,text="            \t \t\t Verify Video\t\t\t    ",bg='#4AF7E7',command=start2)
B.place(x=140,y=126)
canvas.create_rectangle(140, 156, 500, 346,
            outline="black", fill="#0FE7F5")
fontStyle = tkFont.Font(family="Lucida Grande", size=20)
L1=Label(window,text="Result",bg='#0DE5CE',font=fontStyle)
L1.place(x=250,y=350)



##
##L1=Label(window,text="result",bg='#0DE5CE')
##L1.place(x=300,y=110)
##L2=Label(window,text=" ",bg='#14B6A5',height=6,width=30)
##L2.place(x=300,y=150)
##
