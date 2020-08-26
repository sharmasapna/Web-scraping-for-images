## Web-scraping-for-images 
Web_scraping_images.ipynb notebook contains the following methods:   
Some successful methods have been complied in a paper.    
Web scraping methods using.   
### 1.BeautifulSoup 
 
```ruby
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
```
As can be seen these are urls of 20 images. When these images are opened, we see a small picture in the centre of the screen. As I wanted to download as many images as shown in the google search engine, I tried other methods.


### 2.With google_images_download 
```ruby
# importing google_images_download module 
from google_images_download import google_images_download  
  
# creating object 
response = google_images_download.googleimagesdownload()  
  
search_queries = ['flowers','birds'] 
  
  
def downloadimages(query,n): 
    # keywords is the search query 
    # format is the image file format 
    # limit is the number of images to be downloaded 
    # print urs is to print the image file url 
    # size is the image size which can 
    # be specified manually ("large, medium, icon") 
    # aspect ratio denotes the height width ratio 
    # of images to download. ("tall, square, wide, panoramic") 
    arguments = {"keywords": query, 
                 "format": "jpg", 
                 "limit":n, 
                 "print_urls":True, 
                 "size": "medium", 
                 "aspect_ratio":"panoramic"} 
    try: 
        response.download(arguments) 
      
    # Handling File NotFound Error     
    except FileNotFoundError:  
        arguments = {"keywords": query, 
                     "format": "jpg", 
                     "limit":n, 
                     "print_urls":True,  
                     "size": "medium"} 
                       
        # Providing arguments for the searched query 
        try: 
            # Downloading the photos based 
            # on the given arguments 
            response.download(arguments)  
        except: 
            pass
  
# Driver Code 
for query in search_queries: 
    downloadimages(query,100)  
    print()
```
A folder named download gets created in the same folder where the script is running, and subfolders are created inside it for different search query. But it can download mximum 100 images only.

### 3.using gooliser 
With this, I was able to download at least 135 images , which seemed pretty cool! The link to this code is as follows https://github.com/teracow/googliser  
Go to the command window:
And type:
```ruby
bash <(curl -skL git.io/get-googliser)
```
You might be asked to enter the password, wait for some time . Installation will take time.Just print the search url from the google search engine and download images.
'''ruby
googliser --phrase "puppies" --title 'Puppies!' --number 25 --upper-size 100000 -G

```

3.googledownload   
4.pyimagesearch have been included  
Some of their advantages and disadvantages are also mentioned  
youtube_video_frame_downloader.ipynb contains a method to download images from youtube videos  
Also youtube videos can be used to get images, by downloading the youtube video and capturing its frames as images.
