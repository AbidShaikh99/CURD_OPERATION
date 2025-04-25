# CURD_OPERATION
from django.shortcuts import render,redirect

import mysql.connector as mcdb
conn = mcdb.connect(host = "localhost", user = "root", password = "", database = 'stu')
print("Sucessfully connected database")
cur = conn.cursor()
def homepageview(request):
    return render(request,'home.html')

def formpageview(request):
    return render(request,'form.html')

def formpageprocess(request):
    txt1 = request.POST["txt1"]
    txt2 = request.POST["txt2"]
    txt3 = request.POST["txt3"]
    cur.execute("INSERT INTO `tbl_product` (`stu_name`,`stu_gender`,`stu_age`) VALUES ('{}', '{}','{}')".format (txt1,txt2,txt3))
    conn.commit()
    return redirect(formpageview) 

def displaydata(request):
    cur.execute("SELECT * FROM `tbl_product`")
    data = cur.fetchall()
    print(list(data))
    return render(request,'display.html',{'mydata': data})

def deletedata(request,id):
    print("delete is ",id)
    cur.execute("delete from `tbl_product` where `stu_id` = {}".format(id))
    conn.commit()
    return redirect(displaydata)

def editdata(request,id):
    print("Edit ID is ",id)
    cur.execute("select * from `tbl_product` where `stu_id` = {}".format(id))
    data = cur.fetchone()
    print(list(data))
    return render(request,'edit.html',{'mydata':data})

def updatedata(request):
    if request.method =='POST':
        print(request.POST)
        txt1 = request.POST ["txt1"]
        txt2 = request.POST ["txt2"]
        txt3 = request.POST ["txt3"]
        txt4 = request.POST ["txt4"]
        cur.execute("update `tbl_product` set `stu_name` = '{}', `stu_gender` = '{}', `stu_age` = '{}' where `stu_id` = '{}'".format(txt2,txt4,txt3,txt1))
        conn.commit()
        return redirect(displaydata)
    
    else:
        return redirect(displaydata)
    
