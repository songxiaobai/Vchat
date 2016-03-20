序号	功能	接口名称	方法	参数	返回值	描述
1	登陆	ChatUserDAO	login()	String user_name,String user_pwd	 User	通过用户名密码进行登陆
2	用户类	ChatUserDAO	doUpdUser	User	boolean	User为客户端传来的已经修改好的User对象,本方法将修改后的User对象更新到数据库
		ChatUserDAO	findUserById	int user_id	User	通过用户ID找到用户的信息并返回
		ChatUserDAO	checkUserName()	String user_name	boolean	注册时，游客自己输入登陆用户名，进行用户名验证是否可用
		ChatUserDAO	doDel()	User	boolean	删除一个User记录,成功删除返回真
		ChatUserDAO	doIns()	 User	boolean	进行注册，同时进行默认分组（我的好友和黑明）的插入操作，必须使用事务处理
3	好友管理	FriendManagerDAO	delFriend()	 FriendGroup	boolean	传入一个FriendGroup对象,FriendFroup.user_id为该用户指定的好友的id,自己的user_id为通过数据库找到对应分组表数据可以找到自己的id
						
		FriendManagerDAO	doInsFriend()	int user_id，Group	FridendGroup	添加好友（备注），在先执行，检测B的好友是否有A，如果有则直接添加。如果没有，则需要B同意
						
4	群管理	QunManagerDAO	findQunById（）	int qun_id	qun	查找群:通过查找群，返回所查找的群资料
		QunManagerDAO	doDelQun()	int qun_id,qunuser	boolean	解散群:先判断传入的qunuser是否是群主,如果是通过findQunById(qun_id)查找群并删除(设置state)
		QunManagerDAO	doInsQun()	Qun,User	int qun_id,User u	创建群:将Qun同步到数据库里,然后将User设置为群主
		QunManagerDAO	doUpdQunName()	Qun,qunuser	boolean	修改群信息:先判断传入的qunuser是否是群主,如果是通过查找群进行群昵称修改，返回是否修改成功
5	好友分组管理	GroupManagerDAO	findGroupById（）	int group_userid	Group	通过分组ID查找分组，返回一个分组对象
		GroupManagerDAO	doUpdGroupName()	Group	boolean	传入一个已经修改好的分组对象,得到他的ID后,通过id查找分组，修改组名称
		GroupManagerDAO	doInsGroup()	Group	Group	修改组的状态，并改名，从而新增分组
		GroupManagerDAO	delGroup()	Group	boolean	查找到分组，修改分组状态
6	群成员管理	FriendGroupManagerDAO	findMemberByUserId()	user_id	FriendGroup	查找群里的成员，返回该成员
		FriendGroupManagerDAO	delFriendGroup()	FriendGroup,User	boolean	先检查User的id与Friend的id是否一样,一样代表是自主退群,不一样则是T人,将其成员状态修改为2（退群）
		FriendGroupManagerDAO	doInsFriendGrou()	FriendGroup	boolean	将新增成员放进数据库中群成员表中
						
7	聊天	ChatDAO	sendMessage（）	int type,FriendChat/QunChat	boolean	发消息
		ChatDAO	getMessage（）	FriendChat/QunChat	boolean	收消息
		ChatDAO	setMessage()	int type,int friendchat_sendid,int friendchat_getid,String MessageText	FriendChat/QunChat	根据type判断要构造的消息对象类型,然后将对应值set,并返回
						
