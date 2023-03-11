# BuildingAI

Name: AI-Powered Pet Feeder

Description: An AI-powered pet feeder that can recognize pets and dispense food when they come in front of it. It will use computer vision and machine learning algorithms to detect and identify the type of pet, then dispense the appropriate food according to its dietary needs.



Code:

import cv2
import numpy as np
import os
from os import listdir
from os.path import isfile, join

# Create the array of class labels
labels = np.array(['dog', 'cat'])

# Create an array of filenames for the images
image_files = [f for f in listdir('./images') if isfile(join('./images', f))]

# Load the images
images = []
for img in image_files:
    images.append(cv2.imread('./images/' + img))

# Convert the images to grayscale
gray_images = []
for img in images:
    gray_images.append(cv2.cvtColor(img, cv2.COLOR_BGR2GRAY))

# Resize the images
resized_images = []
for img in gray_images:
    resized_images.append(cv2.resize(img, (128, 128)))

# Create the feature vector 
features = []
for img in resized_images:
    features.append(img.flatten())

# Train a machine learning model
from sklearn.svm import SVC
clf = SVC()
clf.fit(features, labels)

# Create a function to recognize pets
def recognize_pet(img):
    # Convert the image to grayscale
    gray_img = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
    # Resize the image
    resized_img = cv2.resize(gray_img, (128, 128))
    # Create the feature vector
    feature = resized_img.flatten()
    # Use the model to predict the class label
    prediction = clf.predict([feature])
    # Return the prediction
    return prediction[0]

# Create a function to dispense food
def dispense_food(pet):
    if pet == 'dog':
        print('Dispensing dog food!')
    elif pet == 'cat':
        print('Dispensing cat food!')

# Main loop
while True:
    # Capture the image
    frame = cv2.imread('image.jpg')
    # Recognize the pet
    pet = recognize_pet(frame)
    # Dispense the food
    dispense_food(pet)
