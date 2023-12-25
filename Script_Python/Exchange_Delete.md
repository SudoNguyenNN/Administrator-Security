import os, time, subprocess, shutil, logging, sys
from logging.handlers import TimedRotatingFileHandler

############## THIS IS FOR LOGGING ##################    
# Create a customer logger:    
logger = logging.getLogger('LogXoaDuLieu')
logger.setLevel(logging.INFO) # set root level
err_logger = logging.getLogger('STDERR')    
err_logger.setLevel(logging.ERROR)  
# Create handlers:
handler = TimedRotatingFileHandler("E:\\01.Script_Delete\Log_delete\EXCHANGE_LOG", when="h", interval=1, backupCount=0) # duong dan file log 1
#(rotate log after 1 minute)
#handler2 = logging.StreamHandler()

#f_handler = logging.FileHandler('D:\\delete2_log') # duong dan file log 2
#c_handler = logging.StreamHandler()

# Set level for handlers:
handler.setLevel(logging.INFO) # same level as root level (log 1)
#handler2.setLevel(logging.ERROR)
#f_handler.setLevel(logging.WARNING) # only log WARNING or higher messages (log 2)
#c_handler.setLevel(logging.ERROR)

# Create formatters and add it to handlers:
format = logging.Formatter('%(asctime)s - %(name)s - %(levelname)s - %(message)s')
handler.setFormatter(format)
#handler2.setFormatter(format)
#f_handler.setFormatter(format)

# Add handlers to the logger:
logger.addHandler(handler)
err_logger.addHandler(handler)
#logger.addHandler(handler2)
#logger.addHandler(f_handler)

class StreamToLogger(object):
    def __init__(self,logger, level):
        self.logger = logger       
        self.level = level
    def write(self, message):
        self.logger.log(self.level, message)
#sys.stderr = StreamToLogger(err_logger, logging.ERROR) #chu y rat nhieu loi nen nhanh chong full log

#######################################################

path = "E:\\BANK_DATA"
now = time.time()
#current_time = time.strftime("%Y/%m/%d %H:%M:%S")

while True:
    
    for root,folders,filenames in os.walk(path):
        if root == path:
            pass
        else:
            for file in filenames:
                if os.stat(os.path.join(root, file)).st_mtime < now - 1 * 86400: # xoa file sau 1 ngay
                    logger.info(f'File da xoa: {os.path.join(root, file)}')
                    try:
                        subprocess.run(["sdelete", "-s", "-p", "3", "-q", (os.path.join(root, file))])                                                
                    except:
                        print('Error happenned during deleting file')
                        logger.error('Error happenned during deleting file')
                        pass
            if root == path:
                pass
            else:
                for folder in folders:
                    if os.stat(os.path.join(root, folder)).st_mtime < now - 1 * 86400: # xoa thu muc sau 1 ngay
                        logger.info(f'Thu muc da xoa: {os.path.join(root, folder)}')
                        try:                            
                            shutil.rmtree(os.path.join(root, folder)) # lenh xoa thu muc ma khong bi anh huong boi access right               
                        except:                       
                            logger.error('Error happenned during removing folder')
                            print('Error happenned during removing folder')
                            pass
    print('Script xoa file va thu muc qua han hon 1 ngay. Tinh trang dang chay binh thuong')
    time.sleep(3)
    


