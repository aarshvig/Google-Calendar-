# -*- coding: utf-8 -*-
# this file is released under public domain and you can use without limitations

#########################################################################
## This is a samples controller
## - index is the default action of any application
## - user is required for authentication and authorization
## - download is for downloading files uploaded in the db (does streaming)
## - call exposes all registered services (none by default)
#########################################################################
import os
import datetime
import fileinput
@auth.requires_login()
def index():
   'auth' in globals() 
   c=auth.user.id
   if c==18:
       redirect(URL('admin_op'))
   name=auth.user.first_name
   return locals()

    
def desc():
    return locals()
    
def contact():
    return locals()
   
def calendar():
    'auth' in globals()
    e=db(db.events.Nameofuser==auth.user.id).select()
    t=db(db.tasks.Nameofuser==auth.user.id).select()
    a=db(db.admin_events.id>0).select()
    return locals()
    
def user():
    """
    exposes:
    http://..../[app]/default/user/login
    http://..../[app]/default/user/logout
    http://..../[app]/default/user/register
    http://..../[app]/default/user/profile
    http://..../[app]/default/user/retrieve_password
    http://..../[app]/default/user/change_password
    use @auth.requires_login()
        @auth.requires_membership('group name')
        @auth.requires_permission('read','table name',record_id)
    to decorate functions that need access control
    """
    return dict(form=auth())


def download():
    """
    allows downloading of uploaded files
    http://..../[app]/default/download/[filename]
    """
    return response.download(request, db)


def call():
    """
    exposes services. for example:
    http://..../[app]/default/call/jsonrpc
    decorate with @services.jsonrpc the functions to expose
    supports xml, json, xmlrpc, jsonrpc, amfrpc, rss, csv
    """
    return service()


@auth.requires_signature()
def data():
    """
    http://..../[app]/default/data/tables
    http://..../[app]/default/data/create/[table]
    http://..../[app]/default/data/read/[table]/[id]
    http://..../[app]/default/data/update/[table]/[id]
    http://..../[app]/default/data/delete/[table]/[id]
    http://..../[app]/default/data/select/[table]
    http://..../[app]/default/data/search/[table]
    but URLs must be signed, i.e. linked with
      A('table',_href=URL('data/tables',user_signature=True))
    or with the signed load operator
      LOAD('default','data.load',args='tables',ajax=True,user_signature=True)
    """
    return dict(form=crud())
    
def add_events():
    db.events.Nameofuser.default=auth.user.id
    form=SQLFORM(db.events).process(next=URL('default','index'))
    if form.process().accepted:
        response.flash="Done"
    return locals()
    
def add_tasks():
    db.tasks.Nameofuser.default=auth.user.id
    form=SQLFORM(db.tasks).process(next=URL('default','index'))
    if form.process().accepted:
        response.flash="Done"
    return locals()
    
def editeventdetails():
    form=SQLFORM.factory(
    Field("event",db.events,requires=IS_IN_DB(db(db.events.Nameofuser.like(auth.user.id)),'events.id','events.Title'),required=True,label='Select events'),
    Field("event_id", writable=False, readable=False))
    if form.accepts(request.vars,session):
        form.vars.event_id=form.vars.event
    #if form.process().accepted:
        #redirect(URL('formedit',args=form.vars.event_id))
        redirect(URL(r=request, f='formedit?event=%d' % int(form.vars.event_id)))
    elif form.errors:
        response.flash='Errors in form'
    return locals()

    
def edittaskdetails():
    form=SQLFORM.factory(
    Field("task",db.tasks,requires=IS_IN_DB(db(db.tasks.Nameofuser.like(auth.user.id)),'tasks.id','tasks.Title'),required=True,label='Select tasks'),
    Field("task_id", writable=False, readable=False))
    if form.accepts(request.vars,session):
        form.vars.task_id=form.vars.task
   # if form.process().accepted:
        #redirect(URL('formedit',args=form.vars.event_id))
        redirect(URL(r=request, f='formtask?task=%d' % int(form.vars.task_id)))
    elif form.errors:
        response.flash='Errors in form'
    return locals()
    
def taskdonedetails():
    form=SQLFORM.factory(
    Field("task",db.tasks,requires=IS_IN_DB(db(db.tasks.Nameofuser.like(auth.user.id)),'tasks.id','tasks.Title'),required=True,label='Select tasks'),
    Field("task_id", writable=False, readable=False))
    if form.accepts(request.vars,session):
        form.vars.task_id=form.vars.task
# if form.process().accepted:
#redirect(URL('formedit',args=form.vars.event_id))
        redirect(URL(r=request, f='donetask?task=%d' % int(form.vars.task_id)))
    elif form.errors:
        response.flash='Errors in form'
    return locals()
        
def donetask():
    p=request.vars.task
    db(db.tasks.id == p).update(Completed = 'True')
    redirect(URL('index'))
    return locals()
    
def formedit():
    c=request.vars.event
    a=db(db.events.id==c).select()[0]
    return locals()
    
def formtask():
    c=request.vars.task
    a=db(db.tasks.id==c).select()[0]
    return locals()
    
def edit_events():
    even=request.vars.e
    tit=request.post_vars.Tit
    des=request.post_vars.Des
    stad=request.post_vars.Stad
    eve=request.post_vars.Eve
    sta=request.post_vars.Sta
    end=request.post_vars.End
    all_=request.post_vars.All
    ven=request.post_vars.Ven
    db(db.events.Title == even).update(Title = tit,Description = des,Startdate=stad,Enddate = eve,Starttime = sta,Endtime=end,All_day=all_,Venue=ven)
    #db(db.events.Title == even).update(Description = des)
    c=db(db.events.Title == even).select()
    redirect(URL('index'))
    return locals()
    
def edit_tasks():
    even=request.vars.e
    tit=request.post_vars.Tit
    des=request.post_vars.Des
    deadd=request.post_vars.DeadD
    deadt=request.post_vars.DeadT
    com=request.post_vars.Com
    #all_=request.post_vars.Com
    ven=request.post_vars.Ven
    db(db.tasks.Title == even).update(Title = tit,Description = des,DeadlineDate = deadd,DeadlineTime = deadt,Completed=com,Venue=ven)
    #db(db.events.Title == even).update(Description = des)
    c=db(db.tasks.Title == even).select()
    redirect(URL('index'))
    return locals()
    
#def edit_tasks():
 #   even=request.vars.e
  #  tit=request.post_vars.Tit
   # des=request.post_vars.Des
    #eve=request.post_vars.Eve
    #sta=request.post_vars.Sta
    #end=request.post_vars.End
    #all_=request.post_vars.All
    #ven=request.post_vars.Ven
    #db(db.tasks.Title == even).update(Title = tit,Description = des,Eventdate = eve,Starttime = sta,Endtime=end,All_day=all_,Venue=ven)
    #db(db.events.Title == even).update(Description = des)
    #c=db(db.tasks.Title == even).select()
    #return locals()
#def edit_events2():
    #form = SQLFORM(db.events, request.vars.event)
    #if form.accepts(request.vars, session):
        #redirect("index")
    #elif form.errors:
        #response.flash = "temp"
    #return dict(form=form)

def delete_eventdetails():
    form=SQLFORM.factory(
    Field("event",db.events,requires=IS_IN_DB(db(db.events.Nameofuser.like(auth.user.id)),'events.id','events.Title'),required=True,label='Select events'),
    Field("event_id", writable=False, readable=False))
    if form.accepts(request.vars,session):
        form.vars.event_id=form.vars.event
    #if form.process().accepted:
        #redirect(URL('formedit',args=form.vars.event_id))
        redirect(URL(r=request, f='delete_event?event=%d' % int(form.vars.event_id)))
    elif form.errors:
        response.flash='Errors in form'
    return locals()

def delete_event():
    c=request.vars.event
    crud.delete(db.events,c,next=URL(r=request,f='index'))
    return locals()
    
def admin_op():
    c=auth.user.first_name
    p=db(db.auth_user.id!=18).select(db.auth_user.first_name,db.auth_user.id)
    return locals()

#def edit_events():
 #  c=request.post_vars
  #  
   
   # p=db(db.events.Title==request.post_vars.event).select().first()
    #    form = SQLFORM(db.events, i.id)
     #   if form.accepts(request.vars,session):
      #      redirect(URL('index'))
       # elif form.errors:
        #    response.flash='Errors in form'
    #return dict(form=form)
   
import datetime
def sendmail():
    da=request.now.date()
    p=db((db.events.Startdate==da) & (db.auth_user.id==db.events.Nameofuser)).select(db.auth_user.email,db.events.ALL)
    for i in p:
        mess='This is to remind you about '+i['events']['Title']+' at '+str(i['events']['Starttime'])+' in '+i['events']['Venue'] 
        mail.send(i['auth_user']['email'], subject='Event Reminder', message=mess)
    t=db((db.tasks.DeadlineDate==da) & (db.auth_user.id==db.tasks.Nameofuser)).select(db.auth_user.email,db.tasks.ALL)
    for i in t:
        mess='This is to remind you about '+i['tasks']['Title']+' at '+str(i['tasks']['DeadlineTime'])+' in '+i['tasks']['Venue'] 
        mail.send(i['auth_user']['email'], subject='Task Reminder', message=mess)
    a=db((db.admin_events.Startdate==da)).select(db.admin_events.ALL)
    us=db(db.auth_user.id!=18).select(db.auth_user.email)
    for i in a:
        mess='Admin would like to remind you about the '+i['Title']
        for j in us:
            mail.send(j.email, subject='Admin event Reminder', message=mess)
    redirect(URL('admin_op'))
    return dict()
    
def add_adminevents():
    form=SQLFORM(db.admin_events).process(next=URL('default','admin_op'))
    return locals()
    
def editadmineventdetails():
    form=SQLFORM.factory(
    Field("admin_event",db.admin_events,requires=IS_IN_DB(db,'admin_events.id','admin_events.Title'),required=True,label='Select tasks'),
    Field("adminevent_id", writable=False, readable=False))
    if form.accepts(request.vars,session):
        form.vars.adminevent_id=form.vars.admin_event
   # if form.process().accepted:
        #redirect(URL('formedit',args=form.vars.event_id))
        redirect(URL(r=request, f='formedit_admin?admin_event=%d' % int(form.vars.adminevent_id)))
    elif form.errors:
        response.flash='Errors in form'
    return locals()
    
def formedit_admin():
    c=request.vars.admin_event
    a=db(db.admin_events.id==c).select()[0]
    return locals()
    
def edit_adminevents():
    even=request.vars.e
    tit=request.post_vars.Tit
    des=request.post_vars.Des
    stad=request.post_vars.Stad
    eve=request.post_vars.Eve
    sta=request.post_vars.Sta
    end=request.post_vars.End
    all_=request.post_vars.All
    ven=request.post_vars.Ven
    db(db.admin_events.Title == even).update(Title = tit,Description = des,Startdate=stad,Enddate = eve,Starttime = sta,Endtime=end,All_day=all_,Venue=ven)
    #db(db.events.Title == even).update(Description = des)
    c=db(db.admin_events.Title == even).select()
    redirect(URL('admin_op'))
    return locals()
     
def admin_calendar():
    c=request.post_vars.username
    q=db(db.auth_user.id==int(c)).select(db.auth_user.first_name)[0]
    e=db(db.events.Nameofuser==int(c)).select()
    t=db(db.tasks.Nameofuser==int(c)).select()
    a=db(db.admin_events.id>0).select()
    return locals()
    
def deleteadmineventdetails():
    form=SQLFORM.factory(
    Field("admin_event",db.admin_events,requires=IS_IN_DB(db,'admin_events.id','admin_events.Title'),required=True,label='Select events'),
    Field("adminevent_id", writable=False, readable=False))
    if form.accepts(request.vars,session):
        form.vars.adminevent_id=form.vars.admin_event
    #if form.process().accepted:
        #redirect(URL('formedit',args=form.vars.event_id))
        redirect(URL(r=request, f='delete_adminevent?event=%d' % int(form.vars.adminevent_id)))
    elif form.errors:
        response.flash='Errors in form'
    return locals()


def delete_adminevent():
    c=request.vars.event
    crud.delete(db.admin_events,c,next=URL(r=request,f='admin_op'))
    return locals()


def delete_taskdetails():
    form=SQLFORM.factory(
    Field("task",db.tasks,requires=IS_IN_DB(db(db.tasks.Nameofuser.like(auth.user.id)),'tasks.id','tasks.Title'),required=True,label='Select tasks'),
    Field("task_id", writable=False, readable=False))
    if form.accepts(request.vars,session):
        form.vars.task_id=form.vars.task
   # if form.process().accepted:
        #redirect(URL('formedit',args=form.vars.event_id))
        redirect(URL(r=request, f='delete_task?task=%d' % int(form.vars.task_id)))
    elif form.errors:
        response.flash='Errors in form'
    return locals()

def delete_task():
    c=request.vars.task
    crud.delete(db.tasks,c,next=URL(r=request,f='index'))
    return locals()

def taskstat():
    'auth' in globals()
    t=db(db.tasks.Nameofuser==auth.user.id).select()
    length=len(t)
    coun=0
    for i in t:
        if i['Completed']==True:
            coun=coun+1
    if length==0:
        per=0
    else:
        per=float(coun)/float(length)*100
    print length
    print coun
    print coun/length
    return locals()
    
auth.settings.logout_next=URL('user/login')
