$(document).ready(function(){
    window.replyTo = undefined;

    ApiCommentsGetAll();
    $('.comment-form').submit(comment_submit);

});

function ApiCommentsGetAll() {
    $.ajax({
        url: '/api/comments/get_all/',
        headers: {'X-XSRFToken': token},
        data: {post_id: post_id},
        type: "POST",
        success: function (res, textStatus, jqXHR) {
            var data = JSON.parse(res).comments;
            if(data.length > 0) {
                $('#comment-list').text('');
                $('#comment-list').append("<ul class='top'></ul>")

                // 1. 按照时间排列数据，越晚越靠前
                data.sort(sortDataByDate);
                //console.log(data)
                // 2. 先列出所有评论，无论他们是在第几层
                data.forEach(function(comment){
                    //console.log(comment);
                    let comment_vote_up;
                    if (checkNested(comment,'like','comment_like')) {
                        comment_vote_up = comment.like.comment_like;
                        if (comment_vote_up===0) {
                            comment_vote_up = '';
                        }
                    } else {
                        comment_vote_up = '';
                    }
                    var comment_element = "<li class='comment-item' data-comment-id='" + comment._id.$oid  + "' data-reply-to='" + comment.reply_to  + "'>" +
                            "<div class='comment-meta'>" +
                                "<div class='comment-author user-icon'>" + comment.comment_author_name.charAt(0) + "</div>" +
                                "<div class='comment-body'>" +
                                    "<a class='author-name' href='/u/" + comment.comment_author_id.$oid + "' target='_blank'>" + comment.comment_author_name + "</a>" +
                                    "<div class='comment-time'>" +
                                        "<img src='/static/svgs/clock.svg' />" +
                                        "<div>" + "<a title=" + "'" + getDateTime(comment.comment_date.$date) + "'>"  + timeago(comment.comment_date.$date) + "</a>" + "</div>" +
                                    "</div>" +
                                    "<div class='comment-content'>" +
                                        "<div>" + comment.comment_content + "</div>" +
                                    "</div>" +
                                        "<div class='comment-vote comment-voteup'>" +
                                        '<button class="icon-thumbs-up-alt" title=""></button>' +
                                        "<span class='vote-number'>" + comment_vote_up + "</span>" +
                                        "</div>" +
                                        "<div class='comment-vote comment-votedown'>" +
                                        '<button class="icon-thumbs-down-alt" title=""></button>' +
                                        "</div>" +
                                         "<div class='reply-comment' data-reply-to='" + comment._id.$oid + "' data-author-name='" + comment.comment_author_name + "'>" +
                                            "<span class='reply'>回復</span>" +
                                        "</div>" +
                                "</div>" +
                            "</div>" +

                            "<ul></ul>" +
                        "</li>"
                    $('#comment-list ul.top').append(comment_element);
                });

                data.forEach(function(comment){
                    // 3. 检查reply_to变量，如果不为空，则将这条评论移动至他父评论下。
                    var is_reply = (comment.reply_to !== '' && comment.reply_to != "undefined" & comment.reply_to != null);
                    var reply_to_element = $("#comment-list li[data-comment-id='" + comment.reply_to + "']");
                    var reply_element = $("#comment-list li[data-comment-id='" + comment._id.$oid + "']");
                    if (is_reply) {
                        var reply_info = "<i class='icon-forward' title='in reply to'></i>  <a title=" + reply_to_element.find('> .comment-meta .comment-content').text() + ">" +
                            reply_to_element.find('> .comment-meta a.author-name').text()  + "</a>";
                        //console.log(reply_info)
                        reply_element.find('.comment-meta a.author-name').after(reply_info);
                    }
                    if (is_reply) {
                        var parent_depth = parseInt(reply_to_element.attr('depth'));
                        if (parent_depth) {
                            var depth = parent_depth + 1;
                        } else {
                            reply_to_element.attr('depth',0);
                            var depth = 1;
                        }

                        reply_to_element.children('ul').append($("#comment-list li[data-comment-id='" + comment._id.$oid + "']"));
                        reply_element.attr('depth',depth);
                        if (depth >= 3) {
                            reply_element.parent('ul').addClass('comment-max-depth');
                            //console.log('depth');
                        }
                        // 如果是第三层或者以上，则删除当前评论给回复按钮
                        //var comment_item = $("#comment-list li[data-comment-id='" + comment.reply_to + "']").attr('data-reply-to');
                        //if( comment_item !== '' && comment_item !== "undefined" && comment_item !== null) {
                        //    $("#comment-list li[data-comment-id='" + comment._id.$oid + "'] .reply-comment").remove();
                        //}
                    }

                });
                //4.渲染当前用户点赞
                if (checkNested(JSON.parse(res),'article_user_comment_vote')) {
                    let data_article_user_comment_vote = JSON.parse(res).article_user_comment_vote;
                    if (data_article_user_comment_vote.length > 0) {
                        data_article_user_comment_vote.forEach(function (liked_comment) {
                                if (liked_comment.value === 1) {
                                    comment_element = $('#comment-list').find("[data-comment-id=" + liked_comment.comment_id + "] .comment-voteup button:first");
                                } else if (liked_comment.value === 0) {
                                    comment_element = $('#comment-list').find("[data-comment-id=" + liked_comment.comment_id + "] .comment-votedown button:first");
                                }
                                comment_element.addClass('active');
                            }
                        )
                    }
                }
                //5. 生成DOM完成以后，才能监听“回复评论”按钮
                $('.reply-comment').click(function() {
                    comment_form = $(this).parent('.comment-body').children('.comment-form');
                    if (comment_form.length) {
                        comment_form.remove();
                        $(this).children('span').removeClass('reply-open');
                        return;
                    }
                    var author_name = $(this).attr("data-author-name");
                    var content = $('#comment-form textarea[name=comment]').val();

                    window.replyTo = $(this).attr("data-reply-to");
                    //$('#comment-form textarea[name=comment]').val(addReplyToText(content, author_name));
                    var reply_form = $('#comment-form').clone();
                    reply_form.find('textarea').attr("placeholder",'');
                    reply_form.removeAttr('id');
                    reply_form.attr('data-reply-to',$(this).attr("data-reply-to"));
                    reply_form.css('margin-top','0.5rem');

                    $(this).parent('.comment-body').append(reply_form);
                    $(this).children('span').addClass('reply-open');
                    $(this).parent().children('.comment-form').submit(comment_submit);
                    reply_form.find('a.login').click(function(){
                        toggleLoginLayer();
                        }
                    );


                    /*
                    //页面自动上移到评论表格
                    $('html, body').animate({
                        scrollTop: $("#comment-form-wrapper").offset().top
                    }, 200);
                    */
                });

                //6. 监听评论点赞、踩
                $(".comment-vote").click(function() {
                        if (is_login()) {
                            comment_vote($(this));
                        } else {
                            toggleLoginLayer();
                        }
                    }
                );


            }
        }
    });
}
function comment_vote(e){
    let action;
    let comment_id = e.closest('.comment-item').attr('data-comment-id');
    let comment_vote_button = e.find("button");
    let comment_vote_other_button = e.siblings(".comment-vote").find("button");
    if (comment_vote_other_button.hasClass("active")){
        comment_vote_other_button.removeClass("active");
    }
    if(!comment_vote_button.hasClass("active")){
        comment_vote_button.addClass("active");
        if(comment_vote_button.parent().hasClass("comment-voteup")){
            action = 'comment_like';
        } else {
            action = 'comment_unlike';
        }
    } else {
        if(comment_vote_button.parent().hasClass("comment-voteup")){
            action = "undo_comment_like";
        } else {
            action = "undo_comment_unlike";
        }
        comment_vote_button.removeClass("active");
    }

    $.ajax({
        url: '/api/article',
        headers: {'X-XSRFToken' : token },
        data: {post_id: post_id ,user_id:user_id,comment_id:comment_id,action:action},
        type: "POST",
        success: function (res, textStatus, jqXHR) {
            ApiCommentsGetAll();
        },
    });
}
function toLocalTimestamp(utcTimestamp){
    localTimestamp = utcTimestamp + new Date().getTimezoneOffset()*60*1000;
    return localTimestamp;
}
function getDateTime(utcTimestamp) {
    var localTimestamp = toLocalTimestamp(utcTimestamp);
    var date = new Date(localTimestamp);
    var hour = date.getHours() < 10 ? "0" + date.getHours() : date.getHours();
    var minutes = date.getMinutes() < 10 ? "0" + date.getMinutes() : date.getMinutes();
    return date.toISOString().slice(0, 10) + ' ' + hour + ":" + minutes;
}

function sortDataByDate(a, b) {
    if (a.comment_date.$date < b.comment_date.$date)
        return -1;
    if (a.comment_date.$date > b.comment_date.$date)
        return 1;
        
    return 0;
}

function timeago(dateTimeStamp){   //dateTimeStamp是一个时间毫秒，注意时间戳是秒的形式，在这个毫秒的基础上除以1000，就是十位数的时间戳。13位数的都是时间毫秒。
    var minute = 1000 * 60;      //把分，时，天，周，半个月，一个月用毫秒表示
    var hour = minute * 60;
    var day = hour * 24;
    var week = day * 7;
    var halfamonth = day * 15;
    var month = day * 30;
    var now = new Date().getTime();   //获取当前时间毫秒
    var diffValue = now - toLocalTimestamp(dateTimeStamp);//时间差
    //console.log(diffValue);
    if(diffValue < 0){
        result = undefined;
    } else {
        var minC = diffValue / minute;  //计算时间差的分，时，天，周，月
        var hourC = diffValue / hour;
        var dayC = diffValue / day;
        var weekC = diffValue / week;
        var monthC = diffValue / month;
        if (monthC >= 1 && monthC <= 3) {
            result = " " + parseInt(monthC) + "月前"
        } else if (weekC >= 1 && weekC <= 3) {
            result = " " + parseInt(weekC) + "周前"
        } else if (dayC >= 1 && dayC <= 6) {
            result = " " + parseInt(dayC) + "天前"
        } else if (hourC >= 1 && hourC <= 23) {
            result = " " + parseInt(hourC) + "小时前"
        } else if (minC >= 1 && minC <= 59) {
            result = " " + parseInt(minC) + "分钟前"
        } else if (diffValue >= 0 && diffValue <= minute) {
            result = "刚刚"
        } else {
            var datetime = new Date();
            datetime.setTime(toLocalTimestamp(dateTimeStamp));
            var Nyear = datetime.getFullYear();
            var Nmonth = datetime.getMonth() + 1 < 10 ? "0" + (datetime.getMonth() + 1) : datetime.getMonth() + 1;
            var Ndate = datetime.getDate() < 10 ? "0" + datetime.getDate() : datetime.getDate();
            var Nhour = datetime.getHours() < 10 ? "0" + datetime.getHours() : datetime.getHours();
            var Nminute = datetime.getMinutes() < 10 ? "0" + datetime.getMinutes() : datetime.getMinutes();
            var Nsecond = datetime.getSeconds() < 10 ? "0" + datetime.getSeconds() : datetime.getSeconds();
            result = Nyear + "-" + Nmonth + "-" + Ndate
        }
    }
    if (result == '' || result == undefined || result == null) {
        return '刚刚';
    } else {
        return result;
    }
}

function comment_submit(e) {
        e.preventDefault();
        var comment = $(this).children('.comment-form textarea[name=comment]').val().replace(/(?:\r\n|\r|\n)/g, '<br>');
        var post_id = $('.comment-form input[name=post_id]').val();
        var reply_to = $(this).attr('data-reply-to') || '';
        //console.log(reply_to);
        if (comment !== '') {
            $.ajax({
                url: '/api/comments/add/',
                headers: {'X-XSRFToken': token},
                data: {
                    post_id:post_id,
                    reply_to: reply_to,
                    comment_content: comment
                },
                type: "POST",
                success: function (res, textStatus, jqXHR) {
                    //$("#success-info").text("留言成功");
                    $('.comment-form textarea[name=comment]').val("");
                    ApiCommentsGetAll();
                },
                error: function (jqXHR, status, err) {
                    $("#success-info").text("留言失敗，請檢查網絡或者聯繫管理員");
                },
            });
        }
    }

