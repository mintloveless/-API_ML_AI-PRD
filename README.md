# API_ML_AI PRD

## 微信小程序
---
Target relese|2019
---|:--:
Ecip|课堂笔记
Document onwer|罗雅琴
Designer|罗雅琴
Develope|罗雅琴
QA|罗雅琴

## 价值主张设计

### 加值宣言
---
本产品应用阿里云智能语音交互和通用类图片文字识别，方便用户记录、回顾课堂笔记。
- 应用录音文件识别、实时语音转写，方便用户录音转文字记录回顾课堂内容。
- 应用通用类图片文字识别，方便用户转换课堂的板书为文字。

### 核心价值
---
本产品主要面向学生群体，调用API，解决因各种可能错过课堂内容，方便用户记录和回顾课堂笔记。

### 核心价值与用户痛点
---
- 一名学生没有听清楚老师讲课内容
- 一位用户在老师讲到重点部分的时候，开了会小差，错过了重要内容
- 一名学生，记笔记的速度太慢，总是记不全笔记
- 一位用户想拍下老师的板书整理、回顾笔记，一字一字的再记录太麻烦
- 一位老师，按照教学计划应该将下一部分，但因为学生还没记完笔记，只能停留在这一部分

### 人工智能概率性与用户痛点
---
- 录音太模糊，导致识别的准确率低
- 图片模糊、字迹潦草，导致识别的准确率低

### 需求列表与人工智能API加值
---
标题|用户案例|重要程度
--|:--:|--:
录音文字识别|用户没记全上课内容笔记|很重要
录音文字识别|用户没听清老师讲课内容|很重要
实时语音转写|用户记笔记速度太慢|重要
通用类图片文字识别|想识别老师的板书图片|次重要

## 原型

### 原型1.交互及界面设计
- 小程序笔记中的录音识别功能和上传图片、音频功能，运用阿里云人工智能的智能语音交互API及通用类文字图片识别API。

![音频](https://images.gitee.com/uploads/images/2019/1130/154044_55483dfd_1531930.png "屏幕截图.png")
![图片](https://images.gitee.com/uploads/images/2019/1130/154107_91c72f4e_1531930.png "屏幕截图.png")
![录音](https://images.gitee.com/uploads/images/2019/1130/154125_90101dc7_1531930.png "屏幕截图.png")

### 原型2.信息设计
- 小程序录音识别功能运用实时语音转写API，输出文本，实现录音转文字功能
- 小程序上传图片功能运用通用文字识别API，输入图片，输出文本，实现图片转文字功能
- 小程序上传音频功能运用实时语音转写API，输入音频，输出文本，实现音频转文字功能

![产品架构](https://images.gitee.com/uploads/images/2019/1130/152725_f7f9f435_1531930.png "屏幕截图.png")


### 原型3.原型文档
#### [产品原型&架构展示](http://nfunm063.gitee.io/api_ml_ai_product_prototype)
#### [原型下载](https://gitee.com/NFUNM063/api_ml_ai_product_prototype)

### 原型4.口头操作说明 
很多人在生活中都会遇到记漏笔记的情况，课堂笔记小程序帮助可以帮助用户更方便的记笔记，高效且不遗漏重点，帮助学生和老师节省不必要的时间。本小程序运用阿里云智能语音交互API和通用类文字图片识别API，帮助用户将录音、音频、图片转为文字，用户只需在课堂上用少量时间进行拍照、录音等步骤，即可。

## API 产品使用关键AI或机器学习之API的输出入展示

### API1.使用水平
##### 录音文件识别
- 输入
```
# -*- coding: utf8 -*-
import json
import time
from aliyunsdkcore.acs_exception.exceptions import ClientException
from aliyunsdkcore.acs_exception.exceptions import ServerException
from aliyunsdkcore.client import AcsClient
from aliyunsdkcore.request import CommonRequest
def fileTrans(akId, akSecret, appKey, fileLink) :
    # 地域ID，常量内容，请勿改变
    REGION_ID = "cn-shanghai"
    PRODUCT = "nls-filetrans"
    DOMAIN = "filetrans.cn-shanghai.aliyuncs.com"
    API_VERSION = "2018-08-17"
    POST_REQUEST_ACTION = "SubmitTask"
    GET_REQUEST_ACTION = "GetTaskResult"
    # 请求参数key
    KEY_APP_KEY = "appkey"
    KEY_FILE_LINK = "file_link"
    KEY_VERSION = "version"
    KEY_ENABLE_WORDS = "enable_words"
    # 是否开启智能分轨
    KEY_AUTO_SPLIT = "auto_split"
    # 响应参数key
    KEY_TASK = "Task"
    KEY_TASK_ID = "TaskId"
    KEY_STATUS_TEXT = "StatusText"
    KEY_RESULT = "Result"
    # 状态值
    STATUS_SUCCESS = "SUCCESS"
    STATUS_RUNNING = "RUNNING"
    STATUS_QUEUEING = "QUEUEING"
    # 创建AcsClient实例
    client = AcsClient(akId, akSecret, REGION_ID)
    # 提交录音文件识别请求
    postRequest = CommonRequest()
    postRequest.set_domain(DOMAIN)
    postRequest.set_version(API_VERSION)
    postRequest.set_product(PRODUCT)
    postRequest.set_action_name(POST_REQUEST_ACTION)
    postRequest.set_method('POST')
    # 新接入请使用4.0版本，已接入(默认2.0)如需维持现状，请注释掉该参数设置
    # 设置是否输出词信息，默认为false，开启时需要设置version为4.0
    task = {KEY_APP_KEY : appKey, KEY_FILE_LINK : fileLink, KEY_VERSION : "4.0", KEY_ENABLE_WORDS : False}
    # 开启智能分轨，如果开启智能分轨 task中设置KEY_AUTO_SPLIT : True
    # task = {KEY_APP_KEY : appKey, KEY_FILE_LINK : fileLink, KEY_VERSION : "4.0", KEY_ENABLE_WORDS : False, KEY_AUTO_SPLIT : True}
    task = json.dumps(task)
    print(task)
    postRequest.add_body_params(KEY_TASK, task)
    taskId = ""
    try :
        postResponse = client.do_action_with_exception(postRequest)
        postResponse = json.loads(postResponse)
        print (postResponse)
        statusText = postResponse[KEY_STATUS_TEXT]
        if statusText == STATUS_SUCCESS :
            print ("录音文件识别请求成功响应！")
            taskId = postResponse[KEY_TASK_ID]
        else :
            print ("录音文件识别请求失败！")
            return
    except ServerException as e:
        print (e)
    except ClientException as e:
        print (e)
    # 创建CommonRequest，设置任务ID
    getRequest = CommonRequest()
    getRequest.set_domain(DOMAIN)
    getRequest.set_version(API_VERSION)
    getRequest.set_product(PRODUCT)
    getRequest.set_action_name(GET_REQUEST_ACTION)
    getRequest.set_method('GET')
    getRequest.add_query_param(KEY_TASK_ID, taskId)
    # 提交录音文件识别结果查询请求
    # 以轮询的方式进行识别结果的查询，直到服务端返回的状态描述符为"SUCCESS"、"SUCCESS_WITH_NO_VALID_FRAGMENT"，
    # 或者为错误描述，则结束轮询。
    statusText = ""
    while True :
        try :
            getResponse = client.do_action_with_exception(getRequest)
            getResponse = json.loads(getResponse)
            print (getResponse)
            statusText = getResponse[KEY_STATUS_TEXT]
            if statusText == STATUS_RUNNING or statusText == STATUS_QUEUEING :
                # 继续轮询
                time.sleep(3)
            else :
                # 退出轮询
                break
        except ServerException as e:
            print (e)
        except ClientException as e:
            print (e)
    if statusText == STATUS_SUCCESS :
        print ("录音文件识别成功！")
    else :
        print ("录音文件识别失败！")
    return
accessKeyId = "您的AccessKey Id"
accessKeySecret = "您的AccessKey Secret"
appKey = "您的appkey"
fileLink = "https://aliyun-nls.oss-cn-hangzhou.aliyuncs.com/asr/fileASR/examples/nls-sample-16k.wav"
# 执行录音文件识别
fileTrans(accessKeyId, accessKeySecret, appKey, fileLink)
```
- 输出
![录音识别](https://images.gitee.com/uploads/images/2019/1201/154128_e6f38ac9_1531930.png "屏幕截图.png")

##### 实时语音转写
- 输入

```
# -*- coding: utf-8 -*-
import os
import time
import threading
import ali_speech
from ali_speech.callbacks import SpeechTranscriberCallback
from ali_speech.constant import ASRFormat
from ali_speech.constant import ASRSampleRate
class MyCallback(SpeechTranscriberCallback):
    """
    构造函数的参数没有要求，可根据需要设置添加
    示例中的name参数可作为待识别的音频文件名，用于在多线程中进行区分
    """
    def __init__(self, name='default'):
        self._name = name
    def on_started(self, message):
        print('MyCallback.OnRecognitionStarted: %s' % message)
    def on_result_changed(self, message):
        print('MyCallback.OnRecognitionResultChanged: file: %s, task_id: %s, result: %s' % (
            self._name, message['header']['task_id'], message['payload']['result']))
    def on_sentence_begin(self, message):
        print('MyCallback.on_sentence_begin: file: %s, task_id: %s, sentence_id: %s, time: %s' % (
            self._name, message['header']['task_id'], message['payload']['index'], message['payload']['time']))
    def on_sentence_end(self, message):
        print('MyCallback.on_sentence_end: file: %s, task_id: %s, sentence_id: %s, time: %s, result: %s' % (
            self._name,
            message['header']['task_id'], message['payload']['index'],
            message['payload']['time'], message['payload']['result']))
    def on_completed(self, message):
        print('MyCallback.OnRecognitionCompleted: %s' % message)
    def on_task_failed(self, message):
        print('MyCallback.OnRecognitionTaskFailed-task_id:%s, status_text:%s' % (
            message['header']['task_id'], message['header']['status_text']))
    def on_channel_closed(self):
        print('MyCallback.OnRecognitionChannelClosed')
def process(client, appkey, token):
    audio_name = 'nls-sample-16k.wav'
    callback = MyCallback(audio_name)
    transcriber = client.create_transcriber(callback)
    transcriber.set_appkey(appkey)
    transcriber.set_token(token)
    transcriber.set_format(ASRFormat.PCM)
    transcriber.set_sample_rate(ASRSampleRate.SAMPLE_RATE_16K)
    transcriber.set_enable_intermediate_result(False)
    transcriber.set_enable_punctuation_prediction(True)
    transcriber.set_enable_inverse_text_normalization(True)
    try:
        ret = transcriber.start()
        if ret < 0:
            return ret
        print('sending audio...')
        with open(audio_name, 'rb') as f:
            audio = f.read(3200)
            while audio:
                ret = transcriber.send(audio)
                if ret < 0:
                    break
                time.sleep(0.1)
                audio = f.read(3200)
        transcriber.stop()
    except Exception as e:
        print(e)
    finally:
        transcriber.close()
def process_multithread(client, appkey, token, number):
    thread_list = []
    for i in range(0, number):
        thread = threading.Thread(target=process, args=(client, appkey, token))
        thread_list.append(thread)
        thread.start()
    for thread in thread_list:
        thread.join()
if __name__ == "__main__":
    client = ali_speech.NlsClient()
    # 设置输出日志信息的级别：DEBUG、INFO、WARNING、ERROR
    client.set_log_level('INFO')
    appkey = '您的appkey'
    token = '您的Token'
    process(client, appkey, token)
    # 多线程示例
    # process_multithread(client, appkey, token, 2)
```

- 输出
![实时语音转写](https://images.gitee.com/uploads/images/2019/1201/154359_789d840b_1531930.png "屏幕截图.png")

##### 通类图片文字识别
- 试用案例
![。](https://images.gitee.com/uploads/images/2019/1201/154719_17f56967_1531930.png "屏幕截图.png")

- 输入
```
import urllib.request
import urllib.parse
import json
import time
import base64
with open('1.jpg', 'rb') as f:  # 以二进制读取本地图片
    data = f.read()
    encodestr = str(base64.b64encode(data),'utf-8')

# 请求头
headers = {
'Authorization': 'APPCODE 我的appcode',
'Content-Type': 'application/json; charset=UTF-8'
}
def posturl(url,data={}):
    try:
         params=json.dumps(dict).encode(encoding='UTF8')
         req = urllib.request.Request(url, params, headers)
         r = urllib.request.urlopen(req)
         html =r.read()
         r.close();
         return html.decode("utf8")
    except urllib.error.HTTPError as e:
         print(e.code)
         print(e.read().decode("utf8"))
    time.sleep(1)
if __name__=="__main__":
     url_request="https://ocrapi-advanced.taobao.com/ocrservice/advanced"
     dict = {'img': encodestr}
     html = posturl(url_request, data=dict)
     print(html)
```
- 输出
![图像文字识别](https://images.gitee.com/uploads/images/2019/1201/155639_0b10aa22_1531930.png "屏幕截图.png")

### API2.使用比较分析
##### 录音文字识别
- 阿里云

语音识别精度高、解码速率快、反馈时间短
预付费0.9-1.2元/小时，后付费1-2.5元/小时，按秒向下取整。

- 腾讯云

支持 wav、mp3、silk、speex、amr、m4a 等多种主流的音频格式
预付费0.7-1.2元/小时，后付费0.95-1.75元/小时，不足1秒按1秒计。

- 百度AI（测试阶段）

中文普通话识别准确率达98%、多语言识别、批量音频快速识别、根据语音智能匹配合适的标点符号，。！？

- 华为云

识别率高、可定制化
按需计费1-2.5元/小时

> 整体来看，各平台价格差不多。优势排序，百度AI > 阿里云 > 腾讯云、华为云。但百度AI录音文字识别目前处于测试阶段。所以阿里云的录音文字识别更具有应用价值。

##### 实时语音转写
- 阿里云

语音识别精度高、解码速率快、反馈时间短、独创模型优化
预付费1.1-1.8元/小时，后付费1.6-3.5元/小时，按秒向下取整。

- 腾讯云

支持中文普通话、英语、粤语，支持 wav、mp3、silk、speex、amr、m4a 等多种主流的音频格式
预付费1-1.8元/小时，后付费3.2元/小时，不足1秒按1秒计。

- 华为云

识别率高、可定制化
按需计费1.2-3.2元/小时

- 百度AI（测试阶段）

中文普通话识别准确率达98%、支持多设备终端、模块优化

> 整体来看，各平台价格差不多。优势排序，阿里云 > 百度AI > 腾讯云 > 华为云。但百度AI录音文字识别目前处于测试阶段。所以阿里云的录音文字识别更具有应用价值。

##### 通用图片文字识别
- 阿里云

识别准确率高、识别速度快、技术精深、服务稳定，上传图片大小不超过4M
前500次免费，超过238-228580元不等

- 腾讯云

能力全面、准确率高、鲁棒性强、图片大小不超过3MB
1,000次/月的免费调用额度，超过120-200000元不等

- 华为云

上传照片小于10M
每个用户每月有1000次的免费调用次数，超过200-228500元不等

- 百度AI

功能、价格选择多样、支持多语种识别、准确率高、服务稳定，图片大小不超过2M
免费版每天可免费调用5万次（5个工作日内响应，不保证并发），付费版每天可调用无数次，0.0025-0.005元/次（24小时响应，并发数10）

> 优势排序：阿里云 > 百度AI > 腾讯云 > 华为云。价格排序：百度AI > 腾讯云 > 华为云、阿里云。

### API3.使用后风险报告
