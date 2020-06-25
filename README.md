# assignment3
import threading
import requests,os,string,sys
from time import sleep
import multiprocessing 
import hashlib


urls =['http://wiki.netseclab.mu.edu.tr/images/thumb/f/f7/MSKU-BlockchainResearchGroup.jpeg/300px-MSKU-BlockchainResearchGroup.jpeg',
'https://upload.wikimedia.org/wikipedia/tr/9/98/Mu%C4%9Fla_S%C4%B1tk%C4%B1_Ko%C3%A7man_%C3%9Cniversitesi_logo.png',
'https://upload.wikimedia.org/wikipedia/commons/thumb/c/c3/Hawai%27i.jpg/1024px-Hawai%27i.jpg​',
'http://wiki.netseclab.mu.edu.tr/images/thumb/f/f7/MSKU-BlockchainResearchGroup.jpeg/300px-MSKU-BlockchainResearchGroup.jpeg​',
'https://upload.wikimedia.org/wikipedia/commons/thumb/c/c3/Hawai%27i.jpg/1024px-Hawai%27i.jpg​']

n=len(urls)
fileList=[]
child_process= os.fork()
if (child_process==0):
    print("Child PID is : ", os.getpid())

else:
    pid=os.getpid()
    sleep(n+1)
    print("parent PID",pid)
    
def download(link, filelocation):
    r = requests.get(link, stream=True)
    with open(filelocation, 'wb') as f:
        for chunk in r.iter_content(1024):
            if chunk:
                f.write(chunk)
def createNewThread(link, filelocation):
    download_thread = threading.Thread(target=download, args=(link,filelocation))
    download_thread.start()



           
for i in range(0,n):
     word=urls[i]
     slist=word.split('.')
     k=len(slist)
     end="."+str(slist[k-1])
     file="file" + str(i+1) + str(end)
     fileList.append(file)
     print (file + "  current PID:",os.getpid())
     createNewThread(urls[i], file)
print(fileList)



#print("child proc exit status:",os.wait())

print("**********************************************************************part2")

print("the OS is: ",sys.platform)

m=os.cpu_count()
processes=[]

def printing_p(m):
    print ("process ", m+1)
   

for i in range(m):
     p = multiprocessing.Process(target=printing_p, args=(i,))
     processes.append(p)
     p.start()




with open('/proc/meminfo') as file:
    for line in file:
        if 'MemFree' in line:
            free_mem_in_kb = line.split()[1]
            print("available memory=",free_mem_in_kb)
            break



