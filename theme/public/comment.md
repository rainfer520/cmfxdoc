# 评论组件
显示评论组件：

{:Comments("posts",$object_id)}

<!-- 评论文章表里的某个id为$object_id的文章-->
Comments方法说明：

参数1：评论内容所在的表，不带表前缀的表名称，如cmf_posts应该改为“posts”;

参数2：评论内容的id:

参数3：数组，目前支持tpl参数，如array("tpl"=>"comment_custom"),这样设置就会加载模板目录Comment/coment_custom.html这个模板。



评论模板：

默认评论模板文件：Comment/comment.html

<br>
<h3>评论</h3>
<div class="comment-area">

	<hr>
	<form class="form-horizontal comment-form" action="{:U('comment/comment/post')}" method="post">
	  <div class="control-group">
		  <div class="comment-postbox-wraper">
		  	<textarea class="form-control comment-postbox" placeholder="Write your comment here" style="min-height:90px;"  name="content"></textarea>
		  </div>
	  </div>
	  
	  <div class="control-group">
	      <button type="submit" class="btn pull-right btn-primary js-ajax-submit"><i class="fa fa-comment-o"></i> 发表评论</button>
	  </div>
	  
	  <input type="hidden" name="post_table" value="{:sp_authencode('posts')}"/>
	  <input type="hidden" name="post_id" value="{$post_id}"/>
	  <input type="hidden" name="to_uid" value="0"/>
	  <input type="hidden" name="parentid" value="0"/>
	</form>
	
	<script class="comment-tpl" type="text/html">
		<div class="comment" data-username="{$user.user_nicename}" data-uid="{$user.id}">
		  <a class="pull-left" href="{:U('user/index/index',array('id'=>$user['id']))}">
		    <img class="media-object avatar" src="{:U('user/public/avatar',array('id'=>$user['id']))}" class="headicon"/>
		  </a>
		  <div class="comment-body">
		    <div class="comment-content"><a href="{:U('user/index/index',array('id'=>$user['id']))}">{$user.user_nicename}</a>:<span class="content"></span></div>
		    <div><span class="time">刚刚</span> <a onclick="comment_reply(this);" href="javascript:;"><i class="fa fa-comment"></i></a></div>
		  </div>
		  <div class="clearfix"></div>
		</div>
	</script>
	
	<script class="comment-reply-box-tpl" type="text/html">
		<div class="comment-reply-submit">
                    <div class="comment-reply-box">
                        <input type="text" class="textbox" placeholder="回复">
                    </div>
                    <button class="btn pull-right" onclick="comment_submit(this);">回复</button>
                </div>
	</script>
	
	
	<hr>
	<div class="comments">
	<foreach name="comments" item="vo">
	 	<div class="comment" data-id="{$vo.id}" data-uid="{$vo.uid}" data-username="{$vo.full_name}"  id="comment{$vo.id}">
		  <a class="pull-left" href="{:U('user/index/index',array('id'=>$vo['uid']))}">
		    <img class="media-object avatar" src="{:U('user/public/avatar',array('id'=>$vo['uid']))}" class="headicon"/>
		  </a>
		  <div class="comment-body">
		    <div class="comment-content"><a href="{:U('user/index/index',array('id'=>$vo['uid']))}">{$vo.full_name}</a>:<span>{$vo.content}</span></div>
		    <div><span class="time">{:date('m月d日  H:i',strtotime($vo['createtime']))}</span> <a onclick="comment_reply(this);" href="javascript:;"><i class="fa fa-comment"></i></a></div>
		    
		    <if condition="!empty($vo['children'])">
		    	<foreach name="vo.children" item="voo">
			    	<div class="comment" data-id="{$voo.id}" data-uid="{$voo.uid}" data-username="{$voo.full_name}" id="comment{$voo.id}">
					  <a class="pull-left" href="{:U('user/index/index',array('id'=>$voo['uid']))}">
					    <img class="media-object avatar" src="{:U('user/public/avatar',array('id'=>$voo['uid']))}" class="headicon"/>
					  </a>
					  <div class="comment-body">
					    <div class="comment-content"><a href="{:U('user/index/index',array('id'=>$voo['uid']))}">{$voo.full_name}</a>:<span>回复 <a href="{:U('user/index/index',array('id'=>$voo['to_uid']))}">{$parent_comments[$voo['parentid']]['full_name']}</a> {$voo.content}</span></div>
					    <div><span class="time">{:date('m月d日  H:i',strtotime($voo['createtime']))}</span> <a onclick="comment_reply(this);" href="javascript:;"><i class="fa fa-comment"></i></a></div>
					  </div>
					  <div class="clearfix"></div>
					</div>
		    	</foreach>
		    
		    </if>
		    
		    
		  </div>
		  <div class="clearfix"></div>
		</div>
	</foreach>
	</div>
	
</div>