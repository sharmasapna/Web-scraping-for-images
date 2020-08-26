## Web-scraping-for-images 
Some of the methods which I tried are compiled here:  
   
### 1.BeautifulSoup (Web_scraping_images.ipynb)
 
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


### 2.With google_images_download (Web_scraping_images.ipynb)
The code is made available by geeksfoegeeks at :  
https://www.geeksforgeeks.org/how-to-download-google-images-using-python/![image.png](attachment:image.png)  
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

### 3.using gooliser (Web_scraping_images.ipynb)
With this, I was able to download at least 135 images , which seemed pretty cool! The link to this code is as follows https://github.com/teracow/googliser  
Go to the command window:
And type:
```ruby
bash <(curl -skL git.io/get-googliser)
```
You might be asked to enter the password, wait for some time . Installation will take time.Just print the search url from the google search engine and download images.
```ruby
googliser --phrase "puppies" --title 'Puppies!' --number 25 --upper-size 100000 -G
```

  
### 4.pyimagesearch (Web_scraping_images.ipynb)
Thanks to Adrian Rosebrock for writing this code and making it public.  

The final one was a jackpot. I could download as many as 700 images at a time. The claim is that for 1000 images. The website is:  
https://www.pyimagesearch.com/2017/12/04/how-to-create-a-deep-learning-dataset-using-google-images/

Click the "JUMP RIGHT TO THE DOWNLOADS SECTION"" download button and enter the eamil where you wish to receive the source code.  
Download the zip folder named google-images-deep-learning.zip.It is having a .py file and a .js file.  
Open the search engine and brows the google search page till the last page, and open the javascript console in chrome.  
Paste the .js code in the console and run it.  
A file named urls.txt will be downloaded.  
Create a folder where you wish to download the images.  
Open terminal (It will not work on Jupyter notebook )and go to the path where .py file is present.  
Run the .py file as follows: 
```ruby
pyhon download_images.py –-urls path to the urls.txt –-output path to the folder where you wish to download the images  
```
The javascript code is as follows:  
```ruby
/** js code
 * simulate a right-click event so we can grab the image URL using the
 * context menu alleviating the need to navigate to another page
 *
 * attributed to @jmiserez: http://pyimg.co/9qe7y
 *
 * @param   {object}  element  DOM Element
 *
 * @return  {void}
 */
function simulateRightClick( element ) {
    var event1 = new MouseEvent( 'mousedown', {
        bubbles: true,
        cancelable: false,
        view: window,
        button: 2,
        buttons: 2,
        clientX: element.getBoundingClientRect().x,
        clientY: element.getBoundingClientRect().y
    } );
    element.dispatchEvent( event1 );
    var event2 = new MouseEvent( 'mouseup', {
        bubbles: true,
        cancelable: false,
        view: window,
        button: 2,
        buttons: 0,
        clientX: element.getBoundingClientRect().x,
        clientY: element.getBoundingClientRect().y
    } );
    element.dispatchEvent( event2 );
    var event3 = new MouseEvent( 'contextmenu', {
        bubbles: true,
        cancelable: false,
        view: window,
        button: 2,
        buttons: 0,
        clientX: element.getBoundingClientRect().x,
        clientY: element.getBoundingClientRect().y
    } );
    element.dispatchEvent( event3 );
}

/**
 * grabs a URL Parameter from a query string because Google Images
 * stores the full image URL in a query parameter
 *
 * @param   {string}  queryString  The Query String
 * @param   {string}  key          The key to grab a value for
 *
 * @return  {string}               value
 */
function getURLParam( queryString, key ) {
    var vars = queryString.replace( /^\?/, '' ).split( '&' );
    for ( let i = 0; i < vars.length; i++ ) {
        let pair = vars[ i ].split( '=' );
        if ( pair[0] == key ) {
            return pair[1];
        }
    }
    return false;
}

/**
 * Generate and automatically download a txt file from the URL contents
 *
 * @param   {string}  contents  The contents to download
 *
 * @return  {void}
 */
function createDownload( contents ) {
    var hiddenElement = document.createElement( 'a' );
    hiddenElement.href = 'data:attachment/text,' + encodeURI( contents );
    hiddenElement.target = '_blank';
    hiddenElement.download = 'urls.txt';
    hiddenElement.click();
}

/**
 * grab all URLs va a Promise that resolves once all URLs have been
 * acquired
 *
 * @return  {object}  Promise object
 */
function grabUrls() {
    var urls = [];
    return new Promise( function( resolve, reject ) {
        var count = document.querySelectorAll(
        	'.isv-r a:first-of-type' ).length,
            index = 0;
        Array.prototype.forEach.call( document.querySelectorAll(
        	'.isv-r a:first-of-type' ), function( element ) {
            // using the right click menu Google will generate the
            // full-size URL; won't work in Internet Explorer
            // (http://pyimg.co/byukr)
            simulateRightClick( element.querySelector( ':scope img' ) );
            // Wait for it to appear on the <a> element
            var interval = setInterval( function() {
                if ( element.href.trim() !== '' ) {
                    clearInterval( interval );
                    // extract the full-size version of the image
                    let googleUrl = element.href.replace( /.*(\?)/, '$1' ),
                        fullImageUrl = decodeURIComponent(
                        	getURLParam( googleUrl, 'imgurl' ) );
                    if ( fullImageUrl !== 'false' ) {
                        urls.push( fullImageUrl );
                    }
                    // sometimes the URL returns a "false" string and
                    // we still want to count those so our Promise
                    // resolves
                    index++;
                    if ( index == ( count - 1 ) ) {
                        resolve( urls );
                    }
                }
            }, 10 );
        } );
    } );
}

/**
 * Call the main function to grab the URLs and initiate the download
 */
grabUrls().then( function( urls ) {
    urls = urls.join( '\n' );
    createDownload( urls );
} );
```
The python code is as follows:  
```ruby
# USAGE
# python download_images.py --urls urls.txt --output images/santa

# import the necessary packages
from imutils import paths
import argparse
import requests
import cv2
import os

# construct the argument parse and parse the arguments
ap = argparse.ArgumentParser()
ap.add_argument("-u", "--urls", required=True,
	help="path to file containing image URLs")
ap.add_argument("-o", "--output", required=True,
	help="path to output directory of images")
args = vars(ap.parse_args())

# grab the list of URLs from the input file, then initialize the
# total number of images downloaded thus far
rows = open(args["urls"]).read().strip().split("\n")
total = 0

# loop the URLs
for url in rows:
	try:
		# try to download the image
		r = requests.get(url, timeout=60)

		# save the image to disk
		p = os.path.sep.join([args["output"], "{}.jpg".format(
			str(total).zfill(8))])
		f = open(p, "wb")
		f.write(r.content)
		f.close()

		# update the counter
		print("[INFO] downloaded: {}".format(p))
		total += 1

	# handle if any exceptions are thrown during the download process
	except:
		print("[INFO] error downloading {}...skipping".format(p))

# loop over the image paths we just downloaded
for imagePath in paths.list_images(args["output"]):
	# initialize if the image should be deleted or not
	delete = False

	# try to load the image
	try:
		image = cv2.imread(imagePath)

		# if the image is `None` then we could not properly load it
		# from disk, so delete it
		if image is None:
			print("None")
			delete = True

	# if OpenCV cannot load the image then the image is likely
	# corrupt so we should delete it
	except:
		print("Except")
		delete = True

	# check to see if the image should be deleted
	if delete:
		print("[INFO] deleting {}".format(imagePath))
		os.remove(imagePath)
```
### 5. Images from youtube videos as frames (youtube_video_frame_downloader.ipynb )


```ruby
import cv2
pip install pytube3
from pytube import YouTube

video = YouTube('https://www.youtube.com/watch?v=GTkU4qj6v7g')
#video.streams.all()
video.streams.filter(file_extension = "mp4").all()

```
Use appropriate itag for the resolution of the video to be downloaded.If you need a high resolution video download, then choose the itag for highest resolution to download in the below step  
```ruby
video.streams.get_by_itag(137).download()
```
I used "137" as it was having the maximum resolution. Copy the the name of the file downloaded(say abc.mp4) 
```ruby
video_path = "abc.mp4"
# Video Capture Using OpenCV
cap = cv2.VideoCapture(video_path)
frame_cnt = int(cap.get(cv2.cv2.CAP_PROP_FRAME_COUNT))
fps = cap.get(cv2.CAP_PROP_FPS)
print('Frames in video: ', frame_cnt)
print(f"Frames per sec: {fps}")
```

For getting the frames of the entire video, use the block below . For getting the frames between a particular time duration, use the block next to the below block. ```ruby
# Use this for accessing the entire video
index = 1

for x in range(frame_cnt):
    ret, frame = cap.read()    
    if not ret:
        break
        
    # Get frame timestamp
    frame_timestamp = cap.get(cv2.CAP_PROP_POS_MSEC)
    
    # fetch frame every sec
    if frame_timestamp >= (index * 1000.0): # change the value from 1000 to anyother value if not needed per second
        index = index + 2   # decides the freq. of frames to be saved
        print(f"++ {index}")
        cv2.imwrite(f"images/cv_{index}.png", frame)   
    if cv2.waitKey(20) & 0xFF == ord('q'):
        break
        
cap.release()
cv2.destroyAllWindows()
```
Use this code for generating imaged from a portion of video
```ruby
# Use this in case frames are to be fetched within a certain time frame
# frame_timestamp will be calculated as fps*time*1000 and set the starting index accordingly
index = 1560

for x in range(frame_cnt):
    ret, frame = cap.read()
    
    if not ret:
        break
        
    # Get frame timestamp
    frame_timestamp = cap.get(cv2.CAP_PROP_POS_MSEC)
    if frame_timestamp >= 1560000.0 and frame_timestamp <= 1800000.0 :
        # fetch frame every sec
        if frame_timestamp >= (index * 1000.0):
            index = index + 4   # decides the freq. of frames to be saved
            print(f"++ {index}")
            
            
            cv2.imwrite(f"images/cv_{index}.png", frame)
    
    
    if cv2.waitKey(20) & 0xFF == ord('q'):
        break
        
cap.release()
cv2.destroyAllWindows()
```

Happy web scraping!
