<p>这个项目用于收录高中的英语以及语法的讲解，当然很多时候是为了满足安德个人的需求，欢迎提建议呀。以后应该会扩充一下的。以下为正文部分。因为安德找资源的确有点不太方便。</p>
  <h2>必修部分</h2>
<ul>
  <li><a href="https://dfghj123tyuvi.github.io/Compulsory-English/%E5%BF%85%E4%BF%AE%E4%B8%80">New senior English for China student's book 1</a></li>
  <li>New senior English for China student's book 2</li>
</ul>
<div class="media">
        <div class="media-left text-center" >
          <div class="img" style="width: 100px;border-radius: 50%; overflow: Hidden;"><a>{UserLogo}</a></div>
          <div class="use"><a href="#" title="{AddUser}">网友:{AddUser}</a></div>
        </div>
        <div class="media-body p-t-md">
          <p style="height:auto;">{Content}</p>
          <div class="time m-t-md">{AddTime}</div>
        </div>
      </div>
    </li>
  </div>
  <div class="mood m-xs p-xs hide">
    <div class="m-sm">
      <div class="font14">读完这篇文章后，您心情如何？</div>
      <div class="moods row m-t-sm "></div>
      <div class="mood_template hidden">
        <div class="col-md-{Size} m-t-sm text-center">
          <a href="#;" onClick="Member.addFaceVote({faceId});"><img src="/template/{srcurl}" width="24" height="24"/><br/>{VoteMood}({VoteResult})</a>
        </div>
      </div>
    </div>
  </div>
  <div class="comment m-xs p-xs">
    <input type="hidden" id="commemtListFlag" value="N">
    <div>
      <div class="m-sm">
        <div class="row">
          <ul id="commemtlist">
            
          </ul>
        </div>
        <div class="row">
          <div class="col-md-6">
            <span class="member-span-logined hide"> <span style="color: #555">欢迎 <a class="member-username bold"></a>
              </span> 评论
            </span> <span class="member-span-login hide"> <a class="member-register">注册</a> &nbsp;|&nbsp;<a class="member-login">登录</a>
            </span>
          </div>
          <div class="col-md-6" align="right">
            <a class="comment-listlink" target="_blank">[<span class="comment-persontotal darkred"> 0 </span>]人参与 (<span class="darkred">点击查看</span>)
            </a>
          </div>
        </div>
        <div class="m-t-sm">
          <textarea class="form-control comment-content" rows="5" autocomplete="off" placeholder="文明上网，登录评论"></textarea>
          <div class="m-sm">
            <div style="display: inline">
              <input type="button" onclick="addFltrpComment(this);" value="发表评论"
              class="comment-submit btn btn-primary input-sm" />
            </div>
            <p class="m-l-md" style="display: inline">
              <label> <input type="checkbox" class="comment-anonymous" value="true" /> <span>匿名评论</span>
              </label> <span class="pull-right hidden-xs">所有评论仅代表网友意见</span>
            </p>
          </div>
        </div>
      </div>
    </div>
  </div>
  <script>
    var commemtListFlag = $V("commemtListFlag");
    
    if("Y"==commemtListFlag){
    } else{
      $.ajax({
        url:'http://www.unischool.cn/unischool/FrontCommentList/getcommonlist',
        dataType:"jsonp",
        jsonp:"callback",
        data:{"ContentID":window.variablesForComment.ContentID,"ContentType":window.variablesForComment.ContentType,"CatalogID":window.variablesForComment.CatalogID,"CommentCount":10},
        success:function(data){
          if(data.status ==1){
            //$("#commemtlist").empty();
            data.data.forEach(function(p){
              temp = $("#commemtliTemp").html();
              temp =temp.replace(new RegExp('{AddTime}',"gm"),p.AddTime);
              temp =temp.replace(new RegExp('{AddUser}',"gm"),p.AddUser);
              temp =temp.replace(new RegExp('{Content}',"gm"),p.Content);
              var memberLogo=(p.UserLogo=="")?"images/bigNoPicture.jpg":p.UserLogo;
              temp =temp.replace(new RegExp('{UserLogo}',"gm"),"<img src='/"+memberLogo+"' />");
              $("#commemtlist").append(temp);  
            });
            $(".comment-persontotal").text(data.PersonTotal);
          }
        }
      });
    }
    
    function addFltrpComment(ele){
      var comment = $(ele).closest(".comment");
      if (comment.length == 0) {
        Dialog.alert("评论框未处于含有.comment样式的元素内!");
        return;
      }
      var content = comment.find(".comment-content").val();
      if (variablesForComment.IsNeedLogin && !Member.isLogined) {
        Dialog.alert(Lang.get("Messageboard.LoginFirst"));
        return;
      } else if (!content) {
        Dialog.alert(Lang.get("Messageboard.ContentEmpty"));
        return;
      } else if (content.length > 2000) {// 最大长度2000
        Dialog.alert(Lang.get("Messageboard.ContentMore"));
        return false;
      }
      var parentID = 0;
      var functions = comment.closest(".comment-functions");// 如果是在回复处直接显示评论框则应被含有.comment-function的元素包裹
      if (functions.length != 0) {
        parentID = functions.attr("parentID");
      }
      var dc = {
        ContentID : variablesForComment.ContentID,
        SiteID : siteID,
        CmntContent : content,
        ParentID : parentID,
        CmntCheckbox : comment.find(".comment-anonymous").prop("checked")
      };
      Server.sendRequest('comment/commit', dc, function(response) {
        if (response.Status) {
          MsgPop(response.Message);
          $(".comment-content").val("");
        }else{
          Dialog.alert(response.Message);
        }
      });// 成功提示改为冒泡，失败则提示失败原因
    }
  </script>
