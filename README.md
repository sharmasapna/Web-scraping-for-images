## Web-scraping-for-images 
Web_scraping_images.ipynb notebook contains the following methods:   
Some successful methods have been complied in a paper.    
Web scraping methods using.   
1.BeautifulSoup 
'''ruby  
import re
import requests
from bs4 import BeautifulSoup
from urllib.parse import urlparse
import os
f = open("images_flowers.txt", "w")
res=[]
def download_google(url):
    #url = 'https://www.google.com/search?q=flowers&sxsrf=ALeKk00uvzQYZFJo03cukIcMS-pcmmbuRQ:1589501547816&source=lnms&tbm=isch&sa=X&ved=2ahUKEwjEm4LZyrTpAhWjhHIEHewPD1MQ_AUoAXoECBAQAw&biw=1440&bih=740'
    page = requests.get(url).text
    soup = BeautifulSoup(page, 'html.parser')

    for raw_img in soup.find_all('img'):
        link = raw_img.get('src')
        res.append(link)
        if link:
            f.write(link +"\n")


download_google('https://www.google.com/search?q=flowers&sxsrf=ALeKk00uvzQYZFJo03cukIcMS-pcmmbuRQ:1589501547816&source=lnms&tbm=isch&sa=X&ved=2ahUKEwjEm4LZyrTpAhWjhHIEHewPD1MQ_AUoAXoECBAQAw&biw=1440&bih=740')

f.close()
'''
2.gooliser  
3.googledownload   
4.pyimagesearch have been included  
Some of their advantages and disadvantages are also mentioned  
youtube_video_frame_downloader.ipynb contains a method to download images from youtube videos  
Also youtube videos can be used to get images, by downloading the youtube video and capturing its frames as images.
