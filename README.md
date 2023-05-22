from turtle import color
import cv2
import mediapipe as mp
import pyautogui
from tkinter import Frame
cap=cv2.VideoCapture(0)
hand_derector=mp.solutions.hands.Hands()
drawing_utile=mp.solutions.drawing_utils
screen_width, screen_height=pyautogui.size()
index_y=0
while True:
    _, frame=cap.read()
    frame=cv2.flip(frame,1)
    frame_height,frame_width,_=frame.shape
    rgb_frame=cv2.cvtColor(frame, cv2.COLOR_BGR2RGB)
    output=hand_derector.process(rgb_frame)
    hands=output.multi_hand_landmarks
    if hands:
        for hand in hands:
            drawing_utile.draw_landmarks(frame,hand)
            landmarks=hand.landmark
            for id,landmarks in enumerate(landmarks):
                x=int(landmarks.x*frame_width)
                y=int(landmarks.y*frame_height)
                if id==8:
                    cv2.circle(img=frame, center=(x,y), radius=10, color=(0, 255, 255))
                    index_x=screen_width/frame_width*x
                    index_y=screen_width/frame_height*y
                    pyautogui.moveTo(index_x, index_y)
                if id==12:
                    cv2.circle(img=frame, center=(x,y), radius=10, color=(0, 255, 255))
                    thumb_x=screen_width/frame_width*x
                    thumb_y=screen_width/frame_height*y
                    print('outside',abs(index_y-thumb_y))
                    if abs(index_y-thumb_y)<20:
                        pyautogui.click()
                        pyautogui.sleep(1)
            

                

    cv2.imshow('Virtual Mouse',frame)
    cv2.waitKey(1)
