#### WeChatWorkTool
Wechatworktool is a Django app,which is used to facilitate the internal application development of enterprise wechat and call enterprise wechat interface faster
#### Quick start
Install `pip install django-wechatwork-tool`
1. Add "WeChatWorkTool" to your INSTALLED_APPS setting like this:
    ```python 
    # setting.py
    INSTALLED_APPS = [
        ...
        'WeChatWorkTool',
        ...
    ]
    ```
2. Run python manage.py migrate to create the AccessToken models.
3. Start the development server and visit http://127.0.0.1:8000/admin/ to create a AccessToken (need WeChatWork's corpid and an app secret of WeChatWork)
4. Call where needed:
    ```python
    ...
    from WeChatWorkTool.views import get_access_token
    ...
    access_token = get_access_token("WeChatWorkAPP")
    ```
5. Application verification and callback example
    ```python
    import json
    from django.http import HttpResponse
    from django.views.decorators.csrf import csrf_exempt
    
    from WeChatWorkTool import tool
    
    
    @csrf_exempt
    def call_back(request):
        """WeChatWork messqge callback Simple Example"""
        if request.method == 'GET':
            # Authentication  url
            data = request.GET.dict()
            return HttpResponse(tool.call_back_verify(data, 'sap'))
    
        if request.method == 'POST':
            url_data = request.GET.dict()
            cb = tool.CorpApp('sap').call_back_data(url_data, request.body)
            return HttpResponse(json.dumps(cb), content_type="application/json")
    ```
   
#### WeChatWorkTool
CorpApp: WeChatWorks custom application
```python
def app_info(self):
    """
    get this app info
    :return: App Info
    """

def get_userid_by_code(self, code):
    """get userid by code，It's just for webpage authorization link

    :param code: in webpage authorization link
    :return:UserId
    """

def get_user(self, userid):
    """get user info

    :param userid: WeChatWork user's id
    :return: a dict that user info
    """

def get_user_tag_list(self):
    """get user tag list

    :return: user tag list
    """

def get_department_user_simplelist(self, department_id, fetch_child=False):
    """get user simplelist by department id

    :param department_id:
    :param fetch_child:
    :return
        For success example:
            {
                "errcode" : 0,
                "errmsg" : "ok",
                "invaliduser" :"",
                "invalidparty" : "",
                "invalidtag": ""
            }

        For error example:
            {
                "errcode" : 0,
                "errmsg" : "ok",
                "invaliduser" : [userid1,userid2],
                "invalidparty" : [partyid1,partyid2],
                "invalidtag": [tagid1,tagid2]
            }
    """

def get_department_user_list(self, department_id, fetch_child=False):
    """get user list by department id

    :param department_id:
    :param fetch_child:
    :return:
    """

def get_department(self, id):
    """get department info list

    :param id: WeChatWork department's id
    :return:
    """

def SendMsg(self, msgobj, touser: list = [], toparty: list = [], totag: list = [],
            safe=0, enable_id_trans=0,
            enable_duplicate_check=0,
            duplicate_check_interval=1800):
    """

    :param touser:
    :param toparty:
    :param totag:
    :param msgobj:
    :param safe:
    :param enable_id_trans:
    :param enable_duplicate_check:
    :param duplicate_check_interval:
    :return:
    """
```
#### Massage object
The message type corresponds to the message type of WeChatWork
1. TextMessageObject
2. MediaMessageObject
3. TextCardMessageObject 
4. NewsMessageObject
5. MarkDownMessageObject
6. TaskCardBtnTemplate
7. TaskCardMessageObject
